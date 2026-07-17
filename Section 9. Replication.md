[題目]

In MongoDB, Read Concern Levels determine the consistency and isolation guarantees for read operations. A developer is building a financial application that requires reading data only after it has been acknowledged by a majority of the replica set members and committed to the journal, ensuring the data will not be rolled back even in the event of a primary failover. Which read concern level should the developer use? (difficulty: medium)

[選項]

A. available
B. local
C. linearizable
D. majority

[正確答案]：
D

[核心考點]：

[詳細解析]：
本題考查 MongoDB 的 Read Concern 級別。`majority` 級別保證讀取的數據已寫入副本集的多數成員，並在預設下持久化至日誌，確保數據在 Primary 發生故障轉移時不會被回滾，適合金融級高可靠性場景。
<img width="1034" height="507" alt="image" src="https://github.com/user-attachments/assets/326bb1f7-695e-4574-bfe0-b5e5c11d8d1d" />

A. 錯誤原因：`available` 級別提供最低延遲的讀取，僅返回當前節點上的最新數據，完全不保證該數據是否已寫入多數成員，極易在發生容錯移轉時遇到數據回滾。

B. 錯誤原因：`local` 為預設級別（與 `available` 類似），僅保證讀取當前節點的本地數據，無任何多數節點確認或日誌持久化保證，同樣存在嚴重的數據回滾風險。

C. 錯誤原因：`linearizable`（線性一致性）主要解決避免讀取到發生網路分區時的「前任主節點」過期數據（Stale Data）。雖然它也能防回滾，但它會引入極高昂的網路協調與往返延遲開銷，不屬於本題以防回滾與多數寫入為核心訴求的最適當解法。

D. 正確原因：`majority` 級別是 MongoDB 官方推薦用於避免髒讀（Dirty Read）與回滾（Rollback）的標準讀取確認配置。它保證數據已被多數節點確認並寫入 Journal，即使 Primary 當機發生故障轉移，該數據也絕對安全。




[題目]

In the context of MongoDB backup and restore utilities (mongodump and mongorestore), you have a replica set with a primary and two secondaries. You run mongodump with the --oplog flag against the replica set. Which of the following statements correctly describes what the --oplog flag does and what file it produces? (difficulty: hard)

[選項]

A. The --oplog flag causes mongodump to also capture the oplog entries that occurred during the dump, saving them in a file named oplog.bson at the top level of the dump output, enabling a point-in-time consistent snapshot.
B. The --oplog flag is only valid when connecting directly to a secondary node, and it saves oplog entries in a file named oplog.json in each individual database subdirectory of the dump output.
C. The --oplog flag causes mongodump to dump only the contents of the local.oplog.rs collection, saving it as a regular collection dump inside the 'local' database directory, and does not affect consistency of other collections.
D. The --oplog flag instructs mongodump to pause all write operations on the primary during the dump to guarantee consistency, and no additional file is created since consistency is achieved through locking.

[正確答案]：
A

[核心考點]：

[詳細解析]：
本題考查 MongoDB 備份工具 mongodump 的 --oplog 參數機制。該參數會在備份期間持續記錄所有寫入操作，並在輸出目錄頂層生成 oplog.bson，以便還原時重放，實現時間點一致性備份。

A. 正確原因：精確描述了 --oplog 的行為。它捕獲備份期間的 oplog，在 dump 頂層生成 oplog.bson，配合 mongorestore --oplogReplay 可達到時間點一致性。

B. 錯誤原因：此參數在 Primary 或 Secondary 節點皆可使用，且生成的是位於 dump 頂層的 oplog.bson 二進位檔，而非各資料庫子目錄下的 .json 文件。

C. 錯誤原因：--oplog 會一併備份目標資料庫與備份期間的寫入日誌，而非「僅」備份 local.oplog.rs 集合本身。

D. 錯誤原因：mongodump 無法也不會主動暫停 Primary 節點的寫入（此為 fsyncLock 的功能），其備份時的一致性是透過記錄與重放 oplog 達成。




[題目]  
In MongoDB Replica Sets, a replica set member is configured with priority 0 and votes 1. Considering how replica sets handle elections and data replication, which of the following statements accurately describes the behavior of this member? (difficulty: hard)
在 MongoDB 副本集（Replica Sets）中，某個副本集成員被配置為 priority 0 且 votes 1。考慮到副本集如何處理選舉和數據複製，下列哪一個敘述準確地描述了該成員的行為？（難度：困難）

[選項]  
A. The member cannot become primary and cannot cast a vote during elections, making it functionally equivalent to a hidden member with votes 0.
B. The member can be elected as primary and will receive write operations, but it counts toward the election quorum.
C. The member cannot become primary and cannot participate in elections, but it still replicates data from the primary.
D. The member cannot become primary but can cast a vote during elections, and it continues to replicate data from the primary.
	
[正確答案]：
D	
	
[核心考點]： 	
	
[詳細解析]：
本題考查副本集成員配置。當成員的 `priority` 設為 0 時，它不能被選為 Primary；而 `votes` 設為 1 代表它保有投票權並參與法定人數（Quorum）計算。此類別成員仍會持續與 Primary 同步數據。

A. 錯誤原因：該成員的 `votes` 設為 1，因此在選舉中「可以」投票，這與 `votes: 0` 的成員完全不同。

B. 錯誤原因：`priority` 為 0 的成員代表其優先級為最低，在任何情況下都「無法」被選舉為 Primary。

C. 錯誤原因：該成員雖然無法成為 Primary，但因為 `votes: 1`，它依然「可以」參與選舉並投出關鍵的一票。

D. 正確原因：`priority: 0` 限制了它無法被選為 Primary，而 `votes: 1` 賦予它選舉投票權，且作為 Secondary 它依然會正常複製數據。







[題目]  
In a MongoDB replica set, which of the following accurately describes what happens when a primary node becomes unreachable and the election process completes successfully? (difficulty: hard)
在 MongoDB 副本集中，下列哪一個敘述準確地描述了當主節點（primary）變得無法連線且選舉程序成功完成時所發生的情況？（難度：困難）

[選項]  
A. The secondary node that has been running the longest since its last restart is automatically promoted to primary, regardless of its replication lag.
B. A secondary node with the highest priority and an oplog that is at least as up-to-date as the majority of voting members wins the election and becomes the new primary.
C. Any secondary node can immediately accept write operations during the election window to ensure high availability, then synchronize changes back to the newly elected primary.
D. All write operations are buffered by the drivers and automatically replayed to the new primary once it is elected, with no writes being rejected during the election window.
	
[正確答案]：
B	
	
[核心考點]： 	
	
[詳細解析]：
本題考查 MongoDB 副本集選舉機制。當主節點斷線，副本集會發起選舉。只有具備最高優先級（Priority）且 Oplog 數據最為最新（至少與參與投票的多數成員一致）的從節點，才能成功當選為新主節點。

A. 錯誤原因：晉升新主節點與節點的運行時間（Uptime）完全無關。副本集會嚴格考量複製延遲，數據落後的節點無法當選。

B. 正確原因：MongoDB 選舉中，投票成員只會投票給 Priority 最高且其 Oplog 數據與自己一樣新（或更新）的候選節點，因此獲勝者必須滿足此條件。

C. 錯誤原因：從節點（Secondary）在任何時候都不能接受寫入。在選舉期間，整個副本集沒有 Primary，因此會處於唯讀狀態，無法執行寫入。

D. 錯誤原因：驅動程式雖支援可重試寫入（Retryable Writes），但它有超時與類型限制。並非所有寫入在選舉期間都能被無限期緩衝且不被拒絕。






