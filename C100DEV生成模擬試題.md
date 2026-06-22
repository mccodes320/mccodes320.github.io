### 題目

情境敘述：
您正在使用 Node.js 驅動程序（MongoDB Node.js Driver）開發一個高併發的 Web API 服務。在程式碼審查（Code Review）時，您發現了以下路由處理常式：

```sql
// 路由處理常式（Route Handler）
app.get('/api/users/:id', async (req, res) => {
    const client = new MongoClient(process.env.MONGODB_URI);
    try {
        await client.connect();
        const db = client.db('crm');
        const user = await db.collection('users').findOne({ _id: req.params.id });
        res.json(user);
    } catch (err) {
        res.status(500).send(err.message);
    } finally {
        await client.close();
    }
});
```

這段程式碼在生產環境高負載下導致了嚴重的效能瓶頸，並引發了資料庫端「Too many open connections」的錯誤。

請選出兩個能夠解決此效能瓶頸，且符合官方驅動程式最佳實踐的修改建議：

選項：

A. 驅動程式預設沒有啟用連線池（Connection Pooling）。應在 URI 連線字串中加上 ?maxPoolSize=100，但維持在每個 request 中創建並關閉 MongoClient。

B. 應將 MongoClient 實例改為全域宣告（或模組層級單例），在應用程式啟動時呼叫 client.connect() 一次，並在各個路由中重複使用同一個 client 實例，且不在每次請求結束時呼叫 client.close()。

C. 呼叫 client.connect() 會耗費極大資源，應將其替換為非同步的 db.reconnect() 來重複利用現有 TCP 管道。

D. 官方驅動程式的 MongoClient 內部已預設管理了一個連線池（預設最大 100 個連線）。保持全域單一實例可以讓多個併發請求共享並複用這些連線，消除了頻繁建立/銷毀 TCP 與 TLS 連線的巨大開銷。







正確答案：B, D

核心考點：Driver Best Practices & Connection Pooling, 考試大綱: Section 6.4

詳細解析：

選項 B 與 D 正確的原因：

為什麼原本的代碼很糟糕？ 在每個 HTTP 請求進來時都執行 new MongoClient() 並呼叫 connect()，接著在請求結束時 close()。這意味著每一次 API 呼叫都要重新進行 DNS 解析、TCP 三向交握、TLS 安全協定握手、以及 MongoDB 的節點探索與監控（SDAM 協定）。這會產生極高的延遲，並在瞬時併發高時瞬間耗盡資料庫的可用連線與系統的 ephemeral ports。
最佳實踐： MongoDB 官方驅動程式的設計初衷是**「單例模式（Singleton）」。MongoClient 物件內部自帶並自動維護**一個高效的連線池（預設 maxPoolSize 通常為 100 [1]）。
因此，應在應用程式啟動時初始化並連接一次 client（如選項 B），之後所有的 API 請求都共享這個全域實例（如選項 D）。驅動程式會自動在連線池中調配閒置連線給查詢使用，大幅提升並發能力與系統回應速度。
其他選項錯誤的原因：

選項 A 錯誤： 即使加上了 maxPoolSize，只要您在每個 request 結束時呼叫了 client.close()，整個連線池都會被徹底銷毀，下一筆請求進來時依然需要重新建立所有連線，完全無法解決瓶頸。
選項 C 錯誤： Node.js 驅動程式中並不存在 db.reconnect() 這樣的 API 方法。解決連線問題的唯一標準做法就是正確管理 MongoClient 的生命週期。







### 題目 21：[單選題]
情境敘述： 您正在為一個 SaaS 平台開發「用戶基本資料」模組，對應的 users 集合中，每個用戶都有一個非必填的選填欄位 tax_id（統一編號）。

大多數的個人用戶其文件中完全不包含 tax_id 欄位（即欄位缺失，Missing）。
僅有少數企業用戶擁有 tax_id，且平台要求只要有填寫 tax_id 的用戶，其號碼就必須是唯一的（不允許重複）。
目前集合中已存在多筆「完全沒有 tax_id 欄位」的個人用戶文件。

為了在資料庫層級強制實施「若存在則必須唯一」的約束，且允許無數個個人用戶文件共存，您應該建立下列哪一種索引？

選項：

A.

db.users.createIndex({ "tax_id": 1 }, { unique: true })
B.

db.users.createIndex({ "tax_id": 1 }, { unique: true, sparse: true })
C.

db.users.createIndex({ "tax_id": 1 }, { sparse: true })
D. MongoDB 不支援對不存在的欄位實施唯一性約束。您必須在應用程式端利用虛擬隨機值（例如 UUID）填充所有缺失的 tax_id，再建立標準唯一索引。

正確答案：B

核心考點：Sparse Unique Indexes / Null Values and Unique Constraints, 考試大綱: Section 3.5

詳細解析：

選項 B 正確的原因： 在 MongoDB 中，標準唯一索引（Normal Unique Index） 會將「缺失的欄位」視為擁有 null 值6。如果您在沒有 sparse: true 的情況下建立唯一索引（選項 A），資料庫允許插入第一筆缺失 tax_id 的文件6。但是，當您嘗試插入第二筆同樣缺失 tax_id 的文件時，MongoDB 會因為在索引中發現了重複的 null 鍵值而拋出 E11000 duplicate key error 寫入失敗6。 搭配 sparse: true（稀疏屬性） 後，稀疏索引會直接忽略（不索引） 那些完全缺失 tax_id 欄位的文件15。因為這些文件不在索引樹中，所以它們不會觸發「唯一性（Unique）」的衝突檢查6。同時，對於那些確實擁有 tax_id 欄位的文件，它們會被寫入索引並嚴格執行唯一性比對6。這完美滿足了「若存在則必須唯一」的要求。

其他選項錯誤的原因：

選項 A 錯誤： 這是標準唯一索引。如上所述，當存在多個缺失 tax_id 的用戶時，會因為重複的 null 鍵值而導致後續用戶寫入失敗6。
選項 C 錯誤： 雖然使用了 sparse: true 避免了缺失欄位的索引，但該指令缺少 { unique: true } 選項，因此無法在資料庫層級保證已填寫 tax_id 的企業用戶資料不會重複。
選項 D 錯誤： 這是關係型資料庫（SQL）的妥協思維。MongoDB 透過「稀疏唯一索引」原生且完美地支持了這種場景，無需在程式端塞入無意義的虛擬資料來污染資料庫。

