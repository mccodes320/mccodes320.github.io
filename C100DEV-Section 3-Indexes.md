# C100DEV

| **編號** | **考核內容 (Learning Objectives)** |
| :--- | :--- |
| **Section 3** | **索引 (17%)** |
| **3.1** | 給定一個正在執行全表掃描（collection scan）的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.2** | 給定一個對陣列欄位進行等值比對且正在執行全表掃描的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.3** | 給定一個無過濾條件、對兩個欄位進行排序且正在執行全表掃描的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.4** | 給定一個集合，辨識該集合目前存在多少個索引。 |
| **3.5** | 辨識使用索引的權衡（trade-offs），以及刪除支援查詢的索引後所帶來的影響與後果。 |
| **3.6** | 辨識執行計畫（explain plan）輸出中代表潛在效能問題的資訊，特別是針對給定查詢是否有使用索引。 |



### MongoDB Indexes

https://learn.mongodb.com/learn/course/mongodb-indexes

* [Lesson 1: Using MongoDB Indexes in Collections](#lesson-1-using-mongodb-indexes-in-collections)
* [Lesson 2: A Single Field Index](#lesson-2-creating-a-single-field-index-in-mongodb)
* [Lesson 3: A Multikey Index](#lesson-3-creating-a-multikey-index-in-mongodb)
* [Lesson 4: A Compound Index](#lesson-4-video-working-with-compound-indexes-in-mongodb)
* [Lesson 5: Deleting MongoDB Indexes](#lesson-5-deleting-mongodb-indexes)
* Lesson 6: getIndexes(), explain
* [ESR](#esr)
* a Single Text Index
* Hashed index
* MongoDB Atlas Search
* TTL
     

# 考試註記
1. create { status: 1, orderDate: -1, customerId: 1 }   
   查詢時 db.orders.find({ status: 'shipped', orderDate: { $gte: ISODate('2024-01-01') }, customerId: 'C123' })
   會因為範圍性查詢導致index失效   



# Lesson 1: Using MongoDB Indexes in Collections

## 1. 什麼是索引 (What Indexes Are)

索引是一種特殊的有序資料結構，主要作用與特性如下：

* 儲存少量資料：僅儲存集合中特定欄位的一小部分資料，並以有序表單的形式整理。
* 精準指向文檔：索引項目會指向文檔的實體位置（RecordID），提供快速查詢與更新。
* 降低資源消耗：加快查詢速度、減少磁碟 I/O，大幅提升整體資料庫效能。
* 避免記憶體排序：當查詢的排序順序與索引一致時，MongoDB 可直接依索引順序回傳結果，無需在記憶體中進行 SORT。
* 支援多種查詢：支援等值比對（Equality Matches）與範圍查詢（Range-based Operations）。

## 2. 索引的運作與成本比較

MongoDB 查詢時的運作機制與相關成本如下：

* 無索引情況 (Without Indexes)：
  * MongoDB 必須掃描整個集合（Collection Scan）來尋找符合條件的文件。
  * 若查詢包含排序，必須在記憶體中進行額外排序作業。
* 有索引情況 (With Indexes)：
  * MongoDB 僅需讀取索引所標示的特定文件，甚至能直接從索引回傳結果。
  * 每個 Collection 預設都會在 `_id` 欄位上建立一個預設索引。
* 寫入成本與注意事項：
  * 每次執行插入、更新或刪除作業時，資料庫皆須同步更新受影響的索引樹。
  * 當索引數量過多時，會引發「寫入放大 (Write Amplification)」現象，導致寫入效能顯著降低。

注意：索引具有寫入效能的成本，在插入新的文件或更新時，也需要針對索引去更動。
注意：如果 Collection 有太多索引，反而會造成寫入效能降低。

## 3. 常見索引類型 (Index Types)

* 單一欄位索引 (Single Field Index)：針對單一欄位建立索引（例如 `_id` 預設索引）。
* 複合索引 (Compound Index)：由多個欄位組成的索引。
* 多鍵索引 (Multikey Index)：針對陣列欄位建立的索引，MongoDB 會自動推斷並調整為多鍵索引。

## 4. 索引的底層結構與儲存內容

MongoDB 使用 B-Tree（B 樹）資料結構來管理索引，索引內部主要儲存以下兩種資訊：

* 索引鍵 (Index Keys)：建立索引時指定的欄位與對應數值。
* 記錄識別碼 (RecordID / Pointer)：由 WiredTiger 儲存引擎產生的內部 64 位元整數，代表該文件在磁碟或記憶體區塊中的實體位置。

## 5. 索引底層管理與異動成本 (B-Tree & Write Amplification)

MongoDB 採用 B-Tree（B 樹）資料結構來維護與管理索引：

* 異動流程：當執行 `insert`、`update` 或 `delete` 時，資料庫除將原始文件寫入磁碟外，還必須同時更新所有受影響的 B-Tree 索引樹。
* 寫入放大 (Write Amplification)：若集合中建立過多索引，每次資料異動都會觸發大量索引樹更新，引發嚴重的寫入放大現象，導致寫入操作變得極為耗時與耗費資源。

## 6. 索引使用重點與最佳實踐 (Best Practices)

* 自動建立預設索引：MongoDB 會自動在 `_id` 欄位建立單一欄位索引，不需手動建立。
* 自動推斷多鍵索引：當索引欄位為陣列（Array）型態時，MongoDB 會自動將其推斷並轉為 Multikey 索引，無需特別指定。
* 最左前綴原則 (Leftmost Prefix Rule)：
  * 複合索引查詢時必須遵守前綴欄位排序。
  * 例如建立 `{a: 1, b: 1}` 索引，可支援 `{a}` 或 `{a, b}` 的查詢，但無法僅針對 `{b}` 進行索引查詢。




# Lesson 2: Single Field Index

支援在單一欄位進行查詢與排序。

1. 建立單一欄位索引

使用 createIndex() 建立單一欄位索引：
* 升冪排序：1
* 降冪排序：-1

```sql
> db.coll.createIndex({fieldname: 1})
< fieldname_1

```

2. 強制唯一性 (Unique Index)

在 createIndex() 的第二個選填參數加入 {unique: true} 可強制索引欄位值不重複。
建立後，任何包含重複值的插入或更新操作皆會失敗。

```sql
> db.coll.createIndex({fieldname: 1}, {unique: true})
< fieldname_1

```

3. 自訂索引名稱 (Index Name)

可以在第二個選填參數中加入 name 屬性來指定索引名稱：

```sql
> db.coll.createIndex({fieldname: 1}, {unique: true, name: 'haaaa'})
< haaaa
```





# Lesson 3: Creating a Multikey Index in MongoDB

* 針對陣列欄位（Array Field）建立的索引。
* 可以是單一欄位索引或複合索引。
* 只要被索引的欄位中包含陣列，即為 Multikey Index。
* 陣列內部可包含巢狀物件或其他資料型別。
* 在複合索引中，每個索引只能有一個欄位是陣列型態。


資料內容：

```json
{
	"_id": ObjectId("60c72b2f9b1d8b2bad8e4531"),
	"name": "Charles",
	"email": "test@yahoo.com",
	"accounts": [100, 101, 102, 103]
}

```

建立多鍵索引（Multikey Index）：

```javascript
// 建立單一欄位的多鍵索引
db.customers.createIndex({accounts: 1})

// 建立複合欄位的多鍵索引
db.customers.createIndex({email: 1, accounts: 1})
```


# Lesson 4: A Compound Indexes in MongoDB

* 針對多個欄位建立的索引（多欄位索引）。
* 若包含陣列欄位，亦可作為多鍵索引（Multikey Index）。
* 每個複合索引最多只能包含一個陣列欄位。
* 支援符合索引前綴（Prefix）的查詢。

資料內容:

```javascript
  {
    username: "alice99",
    name: "Alice Wang",
    active: true,
    birthdate: ISODate("1998-05-12T00:00:00Z"),
    accounts: [1001, 1002]
  },
  {
    username: "bob_smith",
    name: "Bob Smith",
    active: true,
    birthdate: ISODate("1992-11-20T00:00:00Z"),
    accounts: [2001]
  }
```


建立複合索引範例：

```javascript
db.customers.createIndex({active: 1, birthdate: -1, name: 1})

```

可以使用該索引的查詢：

```javascript
db.customers.find({active: true}).sort({birthdate: -1})
db.customers.find({birthdate: {$lt: ISODate("1995-08-01")}, active: true})

```

無法使用該索引的查詢：

```javascript
db.customers.find({birthdate: {$lt: ISODate("1995-08-01")}})
db.customers.find({}).sort({birthdate: 1})
```










# Lesson 5: Deleting MongoDB Indexes

* 刪除索引可能會影響查詢效能。
* dropIndex() 用於刪除單一索引。
* dropIndexes() 用於刪除多個或除 _id 外的所有索引。
* hideIndex() 用於隱藏索引，不影響寫入效能但查詢會忽略該索引。
* unhideIndex() 用於取消隱藏索引。

1. 建立測試索引範例
```sql
db.orders.createIndex({ "userId": 1 })
db.orders.createIndex({ "userId": 1, "test1": 1 }, { name: "test1" })
db.orders.createIndex({ "totalAmount": 1 })
```

2. 隱藏與取消隱藏索引 (hideIndex & unhideIndex)
   在刪除索引前，可先隱藏索引以評估影響。隱藏或取消隱藏可以透過欄位條件或索引名稱操作：

```sql
// 透過欄位條件隱藏索引
db.orders.hideIndex({ userId: 1 })


{
  hidden_old: false,
  hidden_new: true,
  ok: 1,
  '$clusterTime': {
	clusterTime: Timestamp({ t: 1788937237, i: 2 }),
	signature: {
	  hash: Binary.createFromBase64('0Mt7QOShXuPBg9OoKIcJCElN/qg=', 0),
	  keyId: Long('7623464695818616839')
	}
  },
  operationTime: Timestamp({ t: 1788937237, i: 2 })
}



// 透過欄位條件取消隱藏
db.orders.unhideIndex({ userId: 1 })


{
  hidden_old: true,
  hidden_new: false,
  ok: 1,
  '$clusterTime': {
	clusterTime: Timestamp({ t: 1788937244, i: 2 }),
	signature: {
	  hash: Binary.createFromBase64('uRTY4xihrrAsbpoAKWPaSkXUpmw=', 0),
	  keyId: Long('7623464695818616839')
	}
  },
  operationTime: Timestamp({ t: 1788937244, i: 2 })
}
```

3. 刪除單一索引 (dropIndex)
   使用 dropIndex() 刪除指定索引，參數傳入欄位鍵值物件或索引名稱字串：

```sql
// 透過欄位鍵值刪除
db.orders.dropIndex({ userId: 1 })

{
  nIndexesWas: 5,
  ok: 1,
  '$clusterTime': {
	clusterTime: Timestamp({ t: 1788937328, i: 2 }),
	signature: {
	  hash: Binary.createFromBase64('ttJzuX+bKvfGj+tQSoV7WX5bLTc=', 0),
	  keyId: Long('7623464695818616839')
	}
  },
  operationTime: Timestamp({ t: 1788937328, i: 2 })
}


// 透過索引名稱刪除
db.orders.dropIndex('status_1_createdAt_-1')
```

4. 刪除多個索引 (dropIndexes)
   使用 dropIndexes() 刪除集合中除了 _id 以外的所有索引，或傳入索引名稱陣列刪除指定的複數索引：

```sql
// 刪除除 _id 以外的所有索引
db.orders.dropIndexes()

// 傳入陣列刪除多個指定索引
db.orders.dropIndexes(['index1name', 'index2name', 'index3name'])
```
















# Lesson 6: getIndexes() 檢視集合所有索引	
* 用於列出指定集合（Collection）目前存在的所有索引結構、名稱與特殊屬性（例如 TTL 索引的過期秒數）：
    ```sql
    db.customers.getIndexes()

     [
       { v: 2, key: { _id: 1 }, name: '_id_' },
       { v: 2, key: { lastModified: 1 }, name: 'lastModified_1', expireAfterSeconds: 10 }
     ]
    
    ```

# Lesson 7: explain() 剖析查詢執行計畫
   * Use **explain()** in a collection when running a query to see the Execution plan. 
   * **只有find()可以使用explain(), findOne()不能.**
     ```
     TypeError: db.listingsAnd ... 1")}}).explain is not a function
     ```
   * 三種模式說明與範例
     ```sql
		// Mode 1: queryPlanner（預設值，不實際執行查詢）
		db.customers.explain("queryPlanner").find({ email: "alice@example.com" })
		
		// Mode 2: executionStats（最常用！實際執行查詢並輸出精確統計）
		db.customers.explain("executionStats").find({ email: "alice@example.com" })
		
		// Mode 3: allPlansExecution（輸出優化器評估所有候選計畫時的完整數據）
		db.customers.explain("allPlansExecution").find({ email: "alice@example.com" })
     ```
   * 實戰輸出結果與關鍵指標解讀 (executionStats)
     ```sql
		db.customers.explain("executionStats").find(
		  { email: "timothy78@hotmail.com" },
		  { email: 1, _id: 0 }
		)
     ```
   * 輸出資料
     ```sql
		{
		  explainVersion: '1',
		  queryPlanner: {
		    winningPlan: {
		      stage: 'PROJECTION_COVERED', // 階段 2：達成覆蓋查詢（零硬碟 I/O）
		      inputStage: {
		        stage: 'IXSCAN',          // 階段 1：使用索引掃描
		        keyPattern: { email: 1 },
		        indexName: 'email_1',
		        isMultiKey: false
		      }
		    },
		    rejectedPlans: []              // 優化器淘汰的其他計畫
		  },
		  executionStats: {
		    executionSuccess: true,
		    nReturned: 1,                 // [關鍵] 最終回傳的文件筆數：1 筆
		    executionTimeMillis: 0,       // [關鍵] 總耗時：0 毫秒
		    totalKeysExamined: 1,         // [關鍵] 掃描的索引鍵數：1 個
		    totalDocsExamined: 0,         // [關鍵] 讀取硬碟文件數：0 筆（極致效能！）
		    executionStages: {
		      stage: 'PROJECTION_COVERED',
		      nReturned: 1,
		      executionTimeMillisEstimate: 0
		    }
		  }
		}
     ```
   * 資料結構
        * 由內往外看winningPlan﹐從 inputStage 傳遞給父 stage 
     ```sql
	     winningPlan: {
	       isCached: false,         // 是否使用了快取的執行計畫 (Boolean)
	       stage: 'STAGE_NAME',     // 最外層（最終處理）的階段名稱 (String)
	       // ... 該 stage 的專屬參數 ...
	       inputStage: {            // 傳遞資料給上一層的子階段 (Object, 選填)
	         stage: 'SUB_STAGE_NAME',
	         // ... 核心屬性 ...
	         inputStage: { ... }    // 若有多層，會繼續往下巢狀嵌套
	       }
	     },
	     rejectedPlans: [           // 被評估後淘汰的執行計畫列表 (Array)
	       {
	         stage: 'STAGE_NAME',   // 候選計畫的執行階段（結構與 winningPlan 相同）
	         inputStage: { ... }
	       }
	     ]
     ```
    

* MongoDB 執行階段 (Execution Stages) 完全解析

| 執行階段 Stage | 全稱 Full Name | 定義與運作機制 Mechanism | 效能影響與優化建議 Assessment & Action |
| :--- | :--- | :--- | :--- |
| PROJECTION_COVERED | Covered Query (覆蓋查詢) | 當查詢與回傳所需的欄位全部包含在索引中（且排除 _id），MongoDB 完全不需 FETCH 文件即可直接回傳結果。 | 極致 (Optimal+)：零磁碟 I/O，完全依靠記憶體內的索引提供結果，效能達到理論極限。 |
| IXSCAN | Index Scan (索引掃描) | 查詢成功使用了索引。資料庫僅檢索 B-Tree 索引樹中的特定鍵值，迅速定位符合條件的文件位置。 | 最佳 (Optimal)：代表查詢已受索引優化，無須掃描全集合。 |
| FETCH | Fetch Documents (檢索文件) | 資料庫根據前一階段（如 IXSCAN）獲取的位置指標，從磁碟或快取中讀取完整的文件內容。 | 正常 (Normal)：搭配 IXSCAN 使用屬於正常流程；若需極限優化可嘗試調整 Projection 以達成 PROJECTION_COVERED。 |
| PROJECTION_SIMPLE | Simple Projection (一般投射) | 資料已被取得（透過 COLLSCAN 或 FETCH），最後在記憶體中過濾或剪裁出查詢指定要回傳的欄位。 | 正常 (Normal)：純記憶體欄位裁剪動作，通常開銷極低。 |
| SORT | In-Memory Sort (記憶體排序) | 資料庫在記憶體中對結果進行排序，發生於排序欄位未命中索引或 ESR 索引順序不符時。 | 高風險 (High Risk)：極耗 CPU 與記憶體。若排序資料超過 100 MB 限制查詢會報錯中斷，應建立符合排序條件的複合索引。 |
| COLLSCAN | Collection Scan (全集合掃描) | 查詢未命中任何索引。資料庫必須從頭到尾逐筆讀取集合中的每一份文件來比對條件。 | 極差 (Poor)：在大型資料集中會造成嚴重磁碟 I/O 負擔與高延遲，應立即針對查詢條件建立索引。 |





# Lesson 8: ESR 原則的核心架構

* Equality (E) 相等性：單一欄位的精確匹配（例如 status: "ACTIVE"）。必須優先置於索引第一位，能大幅過濾無效文件，減少檢索時間。
* Sort (S) 排序：決定結果集順序的欄位（例如 .sort({ createdAt: -1 })）。利用索引排序可消除開銷極高的記憶體排序（In-Memory Sort）。
* Range (R) 範圍：包含範圍查詢條件的欄位（例如 $gte, $lt, $in）。必須放在排序欄位之後。


Equality + Sort + Range  

```JavaScript
// 寫法 A (先寫 Range 再寫 Equality)
db.listingsAndReviews.find({
  price: { $gte: 100 },    // Range
  status: "ACTIVE"         // Equality
}).sort({ createdAt: -1 })  // Sort

// 寫法 B (先寫 Equality 再寫 Range)
db.listingsAndReviews.find({
  status: "ACTIVE",        // Equality
  price: { $gte: 100 }     // Range
}).sort({ createdAt: -1 })  // Sort
```
對於 MongoDB 來說，寫法 A 與 寫法 B 完全等價！	
= > 完美: { status: 1, createdAt: -1, price: 1 }	
= > 錯誤: { status: 1, price: 1, createdAt: -1 }  // E -> R -> S (錯誤)	
	
### 當兩個E的時候
* 答案是：在絕大多數情況下沒有差別，MongoDB Optimizer 都會自動處理。
* 碰到相同的情況, 選擇性高（高基數）的放前面
```JavaScript
db.listingsAndReviews.find(
  { status: "ACTIVE", price: 1 },
  { status: 1, price: 1, createdAt: 1, _id: 0 } // 明確只輸出索引內有的欄位並剔除 _id
).sort({ createdAt: -1 })
```
{ status: 1, price: 1, createdAt: -1 } // E -> E -> S


```JavaScript
db.listingsAndReviews.find(
{ status: "ACTIVE", price: 1 },
{ status: 1, price: 1, createdAt: 1, _id: 0 } // 明確只輸出索引內有的欄位並剔除 _id
).sort({ createdAt: 1,status:1}) 
```
= > { price: 1, createdAt: 1, status: 1 } // E(精準匹配) -> S (排序第一順位) -> ES (同時滿足 Equality 與 Sort)
```JavaScript
db.listingsAndReviews.find(
{ status: "ACTIVE", price: 1 },
{ status: 1, price: 1, createdAt: 1, _id: 0 } // 明確只輸出索引內有的欄位並剔除 _id
).sort({ status:1, createdAt: 1}) 
```
= > { price: 1, status: 1, createdAt: 1 } // E(精準匹配) ->  ES (同時滿足 Equality 與 Sort)-> S (排序第一順位)





# Lesson 9: 







db.listingsAndReviews.find({last_scraped:{ $gt: ISODate("1991-04-01")}}).explain()

db.listingsAndReviews.createIndex({last_scraped:1})

```
winningPlan: {
  stage: 'COLLSCAN',
  direction: 'forward'
}

winningPlan: {
  stage: 'FETCH',
  inputStage: {
	stage: 'IXSCAN',
	direction: 'forward',
  }
}
```

db.listingsAndReviews.find({last_scraped:{ $gt: ISODate("1991-04-01")}},{last_scraped:1}).explain()

```
winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'COLLSCAN',
	direction: 'forward'
  }
}

winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'FETCH',
	inputStage: {
	  stage: 'IXSCAN',
	  direction: 'forward'
	}
}
```

db.listingsAndReviews.find({last_scraped:{ $gt: ISODate("1991-04-01")}},{last_scraped:-1}).explain()

```

winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'COLLSCAN',
	direction: 'forward'
  }
}

winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'FETCH',
	inputStage: {
	  stage: 'IXSCAN'
	}
  }
}

[1. inputStage: IXSCAN] ➔ 透過索引快速定位出 > 1991-04-01 的資料指標
          ↓
[2. inputStage: FETCH] ➔ 根據指標回到硬碟/快取讀取完整文件（拿取 _id 欄位）
          ↓
[3. Top Stage: PROJECTION_SIMPLE] ➔ 剪裁資料，只留下 last_scraped 與 _id 回傳給使用者

```



db.listingsAndReviews.find({last_scraped:{ $gt: ISODate("1991-04-01")}},{last_scraped:1,_id:0}).explain()

```
winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'COLLSCAN',
	direction: 'forward'
  }
}

winningPlan: {
  stage: 'PROJECTION_COVERED',
  inputStage: {
	stage: 'IXSCAN',
	direction: 'forward',
  }
}
```


db.listingsAndReviews.find(
  { last_scraped: { $gt: ISODate("1991-04-01") } },
  { last_scraped: 1 , _id: 1}
).sort({ _id: -1 }).explain()

rejectedPlans：用 last_scraped 索引找資料，最後拿去記憶體做 SORT（耗費記憶體）。

winningPlan：直接反向掃描主鍵索引 { _id: 1 }。因為 _id 本身已經排序好了，引擎邊讀取邊過濾 last_scraped，直接免去了記憶體排序（In-Memory Sort）的開銷！

```
winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'FETCH',
	inputStage: {
	  stage: 'IXSCAN'
	}
  }
}


winningPlan: {
  stage: 'PROJECTION_SIMPLE',
  inputStage: {
	stage: 'FETCH',

	inputStage: {
	  stage: 'IXSCAN',
	  keyPattern: {
		_id: 1
	  }
	}
  }
},
rejectedPlans: [
  {
	stage: 'SORT',
	sortPattern: {
	  _id: -1
	},
	inputStage: {
	  stage: 'PROJECTION_SIMPLE',
	  inputStage: {
		stage: 'FETCH',
		inputStage: {
		  stage: 'IXSCAN',
		}
	  }
	}
}	
```



# Lesson 9: 複合索引Compound Index Prefix Rule and Best Practices

複合索引（Compound Index）是由多個欄位按特定順序組合而成的單一索引。理解其運作底層（B-Tree 樹狀結構）與前綴原則（Prefix Rule），是設計高效能查詢的關鍵。


### 1. 前綴原則 (Prefix Rule) 的核心定義

#### 何謂索引前綴（Index Prefixes）？

假設我們建立了一個包含三個欄位的複合索引：

```javascript
db.users.createIndex({ db: 1, collection: 1, status: 1 })
```

所謂的「前綴」，指的是該索引從最左側欄位開始、依序向右組合出來的子集：

* 第一前綴：{ db: 1 }
* 第二前綴：{ db: 1, collection: 1 }
* 完整索引：{ db: 1, collection: 1, status: 1 }（涵蓋全部欄位）

#### B-Tree 的底層排序機制與查詢命中分析

WiredTiger 儲存引擎在排序複合索引時，嚴格遵循「先比較左邊欄位，左欄位相同時，才比較右邊欄位」的順序。因此，複合索引只能支援以該索引前綴開始的查詢條件。

```javascript
// 情況 A：完全匹配（極高效）
// 順序命中全部欄位，直達 B-Tree 目標節點
db.users.find({ db: "test", collection: "orders", status: "active" })

// 情況 B：部分匹配且符合前綴（高效）
// 分別命中第一與第二前綴，無須再為 { db: 1 } 單獨建立獨立索引
db.users.find({ db: "test" })
db.users.find({ db: "test", collection: "orders" })

// 情況 C：不符合前綴（完全不支援）
// 完全跳過了最左邊的 db 欄位。右邊資料在 B-Tree 中是無序分散的，無法進行 IXSCAN，只能退回 COLLSCAN
db.users.find({ collection: "orders", status: "active" })

// 情況 D：前綴中間出現「斷層」
// 雖然包含最左邊欄位 db，但跳過了中間的 collection 欄位
db.users.find({ db: "test", status: "active" })
```

> 中間斷層分析：當查詢跳過中間欄位時，MongoDB 仍可用最左側欄位（db）做索引範圍掃描（IXSCAN）來縮小搜尋範圍，但對於後續欄位（status）只能在索引樹上做後續過濾（Index Post-filtering），效能無法達到連續匹配前綴的最佳狀態。

---

### 2. 複合索引查詢最佳實踐 (Best Practices)

假設針對商品集合建立以下符合 ESR 原則的複合索引：

```javascript
db.products.createIndex({ category: 1, price: -1, stock: 1 })
```

#### 最佳查詢 (Perfect)	
* 等值 + 範圍組合：完全涵蓋索引前綴。		
```javascript
db.products.find({ category: "electronics", price: { $gte: 100 }, stock: { $gt: 10 } })
```


* 前綴過濾 + 排序：命中第一前綴 category 過濾，並順應第二欄位 price 的排序方向（-1），直接從小至大或由大至小讀取索引，免去記憶體內排序（In-Memory Sort: False）。
```javascript
db.products.find({ category: "electronics" }).sort({ price: -1 })
```


* 單前綴過濾：僅帶入最左側 category，索引發揮高品質單鍵過濾效果，執行 IXSCAN 迅速限縮範圍。	
```javascript
db.products.find({ category: "electronics" })
```

#### 次佳查詢 (Mid)	
* 中間斷層過濾：漏掉中間欄位 price，只能利用 category 進行索引邊界掃描。在索引樹中撈出符合 electronics 的條目後，再進行掃描後過濾（Post-scan filtering）。	
```javascript
db.products.find({ category: "electronics", stock: { $gt: 0 } })
```

#### 劣質查詢 (Bad)
* 缺少最左前綴：完全未帶入最左欄位 category，導致索引完全失效，迫使資料庫進行全集合掃描（COLLSCAN）。	
```javascript
db.products.find({ price: { $gte: 100 }, stock: { $gt: 0 } }
```


* 排序欄位跳過前綴或方向不符：排序欄位未包含前綴，或排序欄位順序與方向無法透過索引滿足，導致系統進行高代價的記憶體內排序（SORT 階段）。
```javascript
db.products.find({ category: "electronics" }).sort({ stock: 1 })
```




  

# a Single Text Index
   
* 每個集合只允許一個文字索引——這是絕對的，不能繞過。   
* 文字索引僅適用於字串欄位。索引定義中的非字串欄位將被忽略。例如數值欄位會自動排除
* 文字搜尋預設不區分大小寫，但會根據語言設定而區分變音符號
* 對於包含大量字串欄位的大型集合，文字索引可能會增加寫入延遲和儲存開銷。

```sql
db.products.createIndex({ description: "text" })
```


# $natural 
   
直接執行反向索引   
   
```sql
db.appLogs.find().sort({ $natural: -1 });

```

# TTL


```sql
db.sessions.createIndex (
  { lastAccessed : 1 },  
  { expireAfterSeconds : 3600 }  
）；
```

* Limitations of TTL Indexes

Cannot be compound indexes — A TTL index must be a single-field index only. Adding expireAfterSeconds to a compound index will cause an error.   

No sub-second precision — The TTL monitor runs approximately every 60 seconds. Documents are NOT deleted the instant they expire; there is always a potential delay.   

Only works on non-capped collections — TTL indexes have no effect on capped collections.   

Field must be BSON Date type — If the field is missing or holds a non-date type (e.g., a string or integer), the document will NOT be expired.   

Only runs on replica set primary — The TTL background thread only operates on the primary node.   

No pause on primary stepdown — TTL expiration does not have a built-in pause-and-sync mechanism tied to replica set elections; it resumes normally after a new primary is elected.   

High system load can increase delay — Under heavy workloads, the TTL monitor may take longer than 60 seconds to process deletions.   





#  Hashed index

## 觀念總覽：MongoDB 雜湊索引（Hashed Index）與分片應用

MongoDB 的雜湊索引會透過**確定性雜湊函式（Deterministic Hash Function）**將欄位值轉為雜湊值後儲存。雜湊索引的核心目的在於將高基數（High-Cardinality）資料均勻散佈於分片叢集中，但同時具備「浮點數截斷」與「無法支援範圍查詢」等關鍵限制。


## 建立雜湊索引與分片步驟
當集合（如 `sessions`）擁有數百萬筆資料且 `userId` 屬於高基數欄位時：

```sql
// Step 1: 對資料庫啟用分片功能
sh.enableSharding("myDatabase");

// Step 2: 在目標欄位上建立雜湊索引
db.sessions.createIndex({ userId: "hashed" });

// Step 3: 使用該雜湊索引對集合進行分片
sh.shardCollection("myDatabase.sessions", { userId: "hashed" });

```


```sql
db.sessions.insertMany([
  { userId: 2.1, session: "abc" },
  { userId: 2.5, session: "def" },
  { userId: 2.9, session: "ghi" }
]);
```

1. 計算過程：2.1、2.5、2.9 在雜湊前皆會被截斷為整數 2。
2. 結果：三者算出相同的雜湊值 hash(2)，最終被分發至同一個分片上。若設定唯一索引（Unique Index）更會直接觸發重複鍵錯誤（Duplicate Key Error）。


// ❌ 範圍查詢：無法使用雜湊索引，導致全集合掃描（Collection Scan）
db.sessions.find({ userId: { $gt: "U500" } });

// ✅ 等值查詢：高效命中雜湊索引（Index Scan）
db.sessions.find({ userId: "U1234" });

























**Q&A**

**What is a single field index? (Select one.)**  
  
A. An index that supports efficient querying against one field  
B. An index that supports efficient querying against multiple fields  
C. An index that only supports efficient querying against fields with scalar values  
D. An index that supports efficient querying against fields that are already indexed by another user-defined index  
  
    
答案：A

解釋開始：
A. 說明：A single field index is an index that supports efficient querying against a single field. By default, all collections have a single field index on the _id field, but users can define additional indexes that support important queries. A single field index is also a multikey index if the value of the field is an array.
B. 說明：An index that supports efficient queries against multiple fields is called a compound index.
C. 說明：Single field indexes can also support efficient querying against a single array field.
D. 說明：A single field index doesn't support efficient querying against fields that are already indexed by another user-defined index. When a single field is already indexed—for example, by a compound index—creating an additional single field index can cause over-indexing and performance issues.
解釋結束


**Q&A**
  
You have a collection of customer details. The following is a sample document from the collection:  
  
```sql
{
  "_id": { "$oid": "5ca4bbcea2dd94ee58162a6a" },
  "username": "hillrachel",
  "name": "Katherine David",
  "address": "55711 Janet Plaza Apt. 865\nChristinachester, CT 62716",
  "birthdate": { "$date": { "$numberLong": "582848134000" } },
  "email": "timothy78@hotmail.com",
  "Accounts": [
    { "$numberInt": "462501" },
    { "$numberInt": "228290" },
    { "$numberInt": "968786" },
    { "$numberInt": "515844" },
    { "$numberInt": "377292" }
  ],
  "tier_and_details": {}
}
```
  
You create a single field index on the email field, with the unique constraint set to true:
```
db.customers.createIndex({email:1}, {unique:true})
```
What would happen if you attempt to insert a new document with an email that already exists in the collection? (Select one.)

Correct Answer

A. The new document will be inserted and replace the old document in the collection.  
B. The new document will be inserted and the old document will remain in the collection.  
C. MongoDB will return a duplicate key error, and the document will be inserted.  
D. MongoDB will return a duplicate key error, and the document will not be inserted.  
  
  
答案：D  
  
解釋開始：  
A. 說明：That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. The new document will not be inserted in this example because the email address already exists in another document in the collection.  
B. 說明：That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. The new document will not be inserted in this example. Because the email address already exists in another document in the collection, the unique constraint would prevent the new document from being inserted.  
C. 說明：That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. While a duplicate error key would be returned, the new document would not be inserted in this example because the email address already exists in another document in the collection.  
D. 說明：Unique indexes ensure that indexed fields do not store duplicate values. In this example, MongoDB will return a duplicate key error if you attempt to insert a new document with an email that already exists in the collection, as the unique constraint was set to true.  

解釋結束



**Q&A**

**1. What is a multikey index? (Select one.) **

A. An index on one field only where the field is not an array  
B. An index where one of the indexed fields contains an array  
C. An index on more than one field where none of the fields are arrays  
D. An index on more than one field where multiple fields are arrays  
  
**Ans: A**  

```
A. A multikey index is any index where one of the indexed fields contains an array, including both single field and compound indexes. In a compound index, only one of the fields can be an array.
B. Multikey indexes support efficient queries against array fields by creating an index key for each element in the array. This allows MongoDB to search for the index key of each element in the array rather than scan the entire array, which results in dramatic performance gains in your queries.
C. A multikey index is any index where one of the indexed fields contains an array, including both single field and compound indexes. In a compound index, only one of the fields can be an array.
D. A multikey index is any index where one of the indexed fields contains an array, including both single field and compound indexes. In a compound index, only one of the fields can be an array.

```

**Q&A**

**What is the maximum number of array fields per multikey index? (Select one.)**

a. 1  

b. 3  

c. 5  

d. Unlimited  

**Ans: A**  

```
a. Correct! The maximum number of array fields per multikey index is 1. If an index has multiple fields, only one of them can be an array.

b. Incorrect. The maximum number of array fields per multikey index is not 3. However, there is a limitation on the number of array fields per index.

c. Incorrect. The maximum number of array fields per multikey index is not 5. However, there is a limitation on the number of array fields per index.

d. Incorrect. The maximum number of array fields per multikey index is not unlimited.

```



**Q&A**

What is a compound index? (Select one.)

a.
An index that supports queries that combine the field name and the value into one string
Incorrect. 

An index that combines the field name and the value into a single string does not exist MongoDB. Reconsider the fields that compound indexes support queries against.

b.
An index that supports queries against unknown or arbitrary fields
Incorrect.

An index that support queries against unknown or arbitrary fields is a wildcard index. Wildcard indexes were introduced in MongoDB 4.2. They allow users to query against fields that are not explicitly defined in the collection. Reconsider the fields that compound indexes support queries against.

c.
An index that contains references to multiple fields within a document
Correct!

A compound index is an index that contains references to multiple fields within a document. Compound indexes are created by adding a comma-separated list of fields and their corresponding sort order to the index definition.

d.
An index that supports queries that are run on two collections at the same time
Incorrect. 

An index that supports running queries on two collections simultaneously doesn't currently exist in MongoDB. Reconsider the fields that compound indexes support queries against.

**Q&A**


What is the recommended order of fields in a compound index? (Select one.)

Correct Answer

a.
Sort, Range, Equality
Incorrect. There is a recommended order of indexed fields in a compound index. The order of indexed fields is important because query optimization depends on the order of the fields to determine which indexes to use.

b.
Range, Sort, Equality
Incorrect. There is a recommended order of indexed fields in a compound index. The order of indexed fields is important because query optimization depends on the order of the fields to determine which indexes to use.

c.
Equality, Sort, Range
Correct! The recommended order of indexed fields in a compound index is Equality, Sort, and Range. Optimized queries use the first field in the index, Equality, to determine which documents match the query. The second field in the index, Sort, is used to determine the order of the documents. The third field, Range, is used to determine which documents to include in the result set.

d.
The order of indexed fields is not important.
Incorrect. There is a recommended order of indexed fields in a compound index. The order of indexed fields is important because query optimization depends on the order of the fields to determine which indexes to use when executing a query.


**Q&A**

What are the ramifications of deleting an index that is supporting a query? (Select one.)

Correct Answer

a.
The performance of the query will improve.
Incorrect. 

Deleting an index that is supporting a query won’t improve the performance. However, it might cause MongoDB to have to scan the entire database to find the documents that match the query.

b.
The performance of the query will be negatively affected.
Correct! 

The performance of the query will be negatively affected by the deletion of the only index that is currently supporting that query. Indexes generally improve the performance and time efficiency of queries by reducing the number of times that the database needs to be accessed.

c.
The query will fail.
Incorrect. 

Deleting the only index that is supporting a query won't cause it to fail. However, it might cause MongoDB to have to scan the entire database to find the documents that match the query.

d.
The query will perform as expected.
Incorrect. 

Deleting an index that is supporting a query will have an impact. It might cause MongoDB to have to scan the entire database to find the documents that match the query.



**Q&A**

You have a collection of customer details. The following is a sample document from this collection:
```
{
  "_id": { "$oid": "5ca4bbcea2dd94ee58162a6a" },
  "username": "hillrachel",
  "name": "Katherine David",
  "address": "55711 Janet Plaza Apt. 865\nChristinachester, CT 62716",
  "birthdate": { "$date": { "$numberLong": "582848134000" } },
  "email": "timothy78@hotmail.com",
  "Accounts": [
    { "$numberInt": "462501" },
    { "$numberInt": "228290" },
    { "$numberInt": "968786" },
    { "$numberInt": "515844" },
    { "$numberInt": "377292" }
  ],
  "tier_and_details": {}
}
```
You have an index on the email field. Here’s the command you used to create the index:
```
db.customers.createIndex({email:1})
```
Before deleting it, you want to assess the impact of removing this index on the performance of the query. To do this, which command should you use? (Select one.)

Correct Answer

a.
dropIndex()
Incorrect. 

The dropIndex() command deletes an index. Deleting the only index supporting a query will affect the performance of that query. You should hide the index before deleting it. This way, you'll be able to assess the impact of removing the index on query performance. What command is used to hide indexes?

b.
dropIndexes()
Incorrect.

The dropIndexes() command deletes multiple indexes. Deleting the only index supporting a query will affect the performance of that query. You should hide the index before deleting it. This way, you'll be able to assess the impact of removing the index on query performance. What command is used to hide indexes?

c.
getIndexes()
Incorrect.

The getIndexes() command returns an array that holds a list of documents that identify and describe the existing indexes on the collection, including hidden indexes. What command is used to hide indexes?

d.
hideIndex()
Correct. 

The hideIndex() command hides an index. By hiding an index, you'll be able to assess the impact of removing the index on query performance. MongoDB does not use hidden indexes in queries but continues to update their keys. This allows you to assess if removing the index affects the query performance and unhide the index if needed. Unhiding an index is faster than recreating it. In this example, you would use the command db.customers.hideIndex({email:1}).

## MongoDB Indexes
In this unit, we learned what indexes are and how they improve performance. We reviewed and built different indexes:

* Single-field (one field)
* Compound (2 to 32 fields)
We worked with index properties like unique and understood that Multikey indexes are indexes that include one array field.

We used the following commands in the collection to create or delete indexes:

* createIndex()
* dropIndex()
Finally, we learned how to view the indexes being used in a collection with the getIndexes() command and how to check if the index is being used in a query by executing the explain() command.


### GTP 範例題目
---
題目 1：[單選題]

你正在為一家跨國物流公司設計高併發（High-concurrency）的包裹追蹤系統。目前 shipments 集合每秒有數萬筆的物流狀態更新（大量寫入）。為了支援客服人員點對點的快速查詢與排序，你被要求評估索引策略。根據 MongoDB 索引的核心原理與成本，以下哪一個敘述最符合官方的最佳實踐（Best Practices）？

A. 為了確保客服的每一個查詢都能維持最高效率，我們應該針對可能被查詢的 15 個欄位各自建立 Single Field Index。

B. 當我們對包含排序（Sort）需求的欄位建立索引後，MongoDB 會依據建立時指定的欄位與方向排序儲存，這能有效消除查詢時在記憶體中進行排序（In-memory sort）的資源消耗。

C. 預設的 _id 索引只包含 _id 欄位，如果我們建立了一個包含 _id 的 Compound Index，原生的 _id 索引就會被自動刪除以節省空間。

D. 為了極大化加速寫入（Insert/Update）效能，我們應該在集合中建立多個 Multikey Index 來分散陣列欄位的寫入壓力。

正確答案： B

核心考點： Index Costs and Sorting Mechanics (索引成本與排序機制)

詳細解析：

選項 B 正確的原因：
根據你的筆記核心：「Indexes 會依據建立時所提供的索引欄位和排序，將資料儲存在已建立的資料結構中。」當查詢包含 sort() 且該排序欄位已建立對應索引時，MongoDB 可以直接依序遍歷（Walk the index）並回傳結果，這能完全避免高成本的記憶體內排序（In-memory Sort），進而大幅減少 CPU 與記憶體資源的消耗。

其他選項錯誤的原因：

選項 A 錯誤： 筆記明確指出「索引具有寫入效能的成本，在插入新的文件或更新時，也需要針對索引去更動。如果集合有太多索引，反而會造成寫入效能降低。」在每秒數萬筆寫入的高併發場景下，盲目建立 15 個索引會導致嚴重的寫入瓶頸（Write Amplification）。

選項 C 錯誤： 預設的 _id 索引是強制性且不可刪除的，它是由 MongoDB 自動建立且獨佔的，任何自定義的 Compound Index 都無法自動覆蓋或刪除原生的 _id 索引。

選項 D 錯誤： 觀念完全顛倒。Multikey Index 是針對「陣列（Array）」欄位建立的索引，由於陣列中的每個元素都需要在索引結構中建立一個對應的 entry（點點相連），因此 Multikey Index 的寫入與維護成本比普通索引更高，絕不可能用來「分散寫入壓力」。


















