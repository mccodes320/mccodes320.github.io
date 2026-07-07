
Lesson 1: Introduction to ACID Transactions  
Lesson 2: ACID Transactions in MongoDB  



# 考試註記
1. Read Concern
  1.  local 讀取當前節點上的最新數據，不論該數據是否已被複製到其他節點。
  2.  available 行為與 local 類似，但針對分片叢集（Sharded Cluster）優化，直接返回節點當前最快拿到的數據。
  3.  majority 僅讀取已被多數（Majority）節點確認且寫入日誌（Journal）的持久化數據。
  4.  linearizable 僅讀取已被多數節點確認的數據，且 Primary 在回應前必須即時向多數 Secondary 發送確認，確保自己此時此刻仍是唯一的 Primary。
  * 安全性排序：linearizable > majority > local = available
  * 效能（速度）排序：available $\ge$ local > majority > linearizable
  * 金鑰考點：題目中只要出現 「財務應用 (financial)」、「絕對不會被回滾 (will not be rolled back)」、「多數成員確認 (majority of the replica set members)」，在沒有特殊強調防範雙主過時讀取的前提下，標準答案必然是 majority。



Lesson 1: Introduction to ACID Transactions
---
https://learn.mongodb.com/learn/course/mongodb-transactions/lesson-1-introduction-to-acid-transactions/learn

ACID Transcation

An ACID transcation is a group of database operations that must happen together or not at all, ensuring database safety and consittency

使用場景在將記錄傳喚到另一則記錄中, 如金融交易與商品賣賣

* Atomicity 原子性, 全部的操作只有同時成功或同時失敗

  Guarantees that every transaction is "all or nothing" when committing data to a database. For example, we don't want money to be taken from an account but not successfully added to another.

* Consistency 一致性

  Guarantees that the data written to the database is consistent with database constraints. For example, if an account balance cannot be less than 0, a transaction would fail before violating this constraint.

* Isolation 隔離

  Guarantees that each transaction that is run concurrently leaves the database in the same state as if the transactions were run sequentially. In other words, multiple transactions can happen at the same time without affecting the outcome of the other transactions.

* Durability 耐久性

  Durability: Guarantees that data is never lost. Data is saved to non-volatile memory, so any modifications made to data by a successful transaction will persist, even in the event of a power or hardware failure.


[題目]
Which of the following is the best definition of an ACID transaction? (Select one.)

[選項]
A. A group of database operations that cannot fail.
B. Database operations that involve transferring money between two parties.
C. A group of database operations that must happen all together or not at all.
D. Database operations that are durable and will persist even if the server is down.

[正確答案]：
C

[核心考點]：

[詳細解析]：
本題考查資料庫中 ACID 事務（Transaction）的核心定義。ACID 是原子性（Atomicity）、一致性（Consistency）、隔離性（Isolation）與持久性（Durability）的縮寫。其中，事務最核心、最基礎的定義即為「原子性（Atomicity）」，意即一組操作是一個不可分割的整體，要麼全部成功，要麼全部失敗回滾（All or nothing）。

A. 錯誤原因：事務內部的操作「可以」失敗。ACID 的機制是確保一旦其中某個操作失敗時，整個事務都會跟著失敗並回滾，而非保證操作絕對不會出錯。

B. 錯誤原因：雖然轉帳（金流交易）是 ACID 事務最經典的應用場景，但事務的定義與適用範圍絕不侷限於轉帳，任何需要確保資料完整性的複合操作皆適用。

C. 正確原因：精確描述了 ACID 事務最關鍵的「原子性（All or nothing）」本質，這也是定義一組操作是否構成事務的最核心標準。

D. 錯誤原因：此敘述僅單純描述了「持久性（Durability）」這一項特性，並不足以代表整個完整 ACID 事務的全面性定義。



[題目]
Which of the following scenarios require the use of an ACID transaction? (Select all that apply.)

[選項]
A. Updating a bank database to reflect the transfer of money from Customer A's bank account into Customer B's bank account.
B. Updating inventory and shopping cart records when a customer adds an item to their online shopping cart in an ecommerce app.
C. Updating records in a user database to bulk add user profile pictures.
D. Adding multiple documents that contain information about movies to a database.  

[正確答案]：
AB

[核心考點]：

[詳細解析]：
本題考查在實際業務場景中評估是否需要引入 ACID 事務（Transaction）。事務的核心價值在於確保「跨多個操作或多個文件的修改必須具備原子性（Atomicity）」，即要麼全部成功，要麼全部失敗。這通常發生在涉及價值轉移、庫存扣減、所有權變更等不允許出現中間狀態（如錢扣了對方沒收到、庫存扣了購物車沒加成功）的商務邏輯中。

A. 正確原因：銀行轉帳是經典的事務場景。A 帳戶扣款與 B 帳戶存款兩個操作必須同時成功或同時失敗，絕不允許扣款成功但存款失敗的中間狀態存在。

B. 正確原因：電商購物涉及庫存扣減與購物車新增。這兩個操作必須保持原子性，否則會導致庫存數據與購物車實際數量不一致，進而引發超賣或帳實不符，因此需要事務支持。

C. 錯誤原因：批量上傳使用者大頭貼通常是彼此獨立的寫入操作。即使其中某幾張圖片上傳失敗，也不需要讓其他已經成功上傳的圖片跟著回滾，使用一般的批量寫入即可，無需事務。

D. 錯誤原因：向資料庫寫入多部電影的資訊，每部電影文件都是獨立的個體，彼此間沒有必須「同生共死」的強烈商務關聯性，使用標準的 insertMany() 即可高效完成，不需要開銷較大的事務。




Lesson 2: ACID Transactions in MongoDB
---

A group of db operations that will be completed as a unit or not at all. 

### Single-document operations are already atomic in MondoDB
僅影響一個Collection基本上在mongoDB本質上就是原子的

類似updateOne，將修改後的結果同時展示給客戶端
<img width="1161" height="795" alt="image" src="https://github.com/user-attachments/assets/83c34e30-3982-47de-907e-f98624581f8c" />

### Mulit-document Operations
相較Single 多個文檔操作本質上就不是原子的，需要額外的步驟

1. Are not inherently atimic
2. Require extra steps to have ACID properties
3. 
<img width="646" height="795" alt="image" src="https://github.com/user-attachments/assets/b580f9dc-1966-448b-a603-0164f4dcb9c8" />

<img width="1199" height="806" alt="image" src="https://github.com/user-attachments/assets/7618a963-2b28-4988-bfc5-b96b39d7cb78" />

### Mulit-document ACID Transcation

1. MongoDB "locks" resources involved in a transcation
2. Incurs performance cost and affects latency
3. Use multi-document transcations as a precise(精確) tool。在絕對確定要使用ACID屬性完成多的文檔操作時才被使用


### Learn

All single-document operations are atomic。  
  
No extra steps are needed to provide ACID propertoes to single-document operations  

<img width="482" height="456" alt="image" src="https://github.com/user-attachments/assets/95ad000e-71c6-43cc-8ded-141b9c215b40" />





QA

Which of the following statements are true about multi-document transactions in MongoDB? (Select all that apply.)

a.
Database operations that affect more than one document, like .updateMany(), are not inherently atomic in MongoDB and must be completed by using a multi-document transaction in order to have ACID properties.
b.
Multi-document transactions should be treated as a precise tool that is used only in certain scenarios.
c.
Using a multi-document transaction with a MongoDB database ensures that the database will be in a consistent state after running a set of operations on multiple documents.
d.
When using multi-document transactions in MongoDB, each individual read and write operation must be wrapped in its own transaction.


==> ABC


A:A multi-document must be used to ensure that database operations that affect more than one document in a MongoDB database have ACID properties.


B:Multi-document transactions can incur a performance cost. It's important to follow best practices and use multi-document transactions only when ACID properties are essential.


C:Using a multi-document transaction guarantees that operations have ACID properties and, therefore, that the database will be in a consistent state once the transaction is complete.


D:All database operations involved in a multi-document transaction should be wrapped in the same transaction so that all the operations succeed or fail together.















Nadia needs to update customer account balances across multiple collections in MongoDB. It's important that the database operations used in this transaction adhere to ACID properties. Should Nadia use a transaction in this scenario? (Select one.)

a.
Nadia does NOT need to use a transaction in this scenario because multi-document operations are inherently atomic in MongoDB.
b.
Nadia does need to use a transaction in this scenario because multi-document operations are NOT inherently atomic in MongoDB.

==> B



A:Nadia should use a multi-document transaction in this scenario. Unlike single-document operations, multi-document operations are not inherently atomic in MongoDB. Because the group of database operations will be performed on multiple documents and require ACID properties, they should use a multi-document transaction.


B:Nadia should use a multi-document transaction in this scenario. Unlike single-document operations, multi-document operations are not inherently atomic in MongoDB. Because the group of database operations will be performed on multiple documents and require ACID properties, they should use a multi-document transaction.


C:
D:


Lesson 3: Using Transactions in MongoDB
---

示範多文件交易與中斷多文件交易，帳戶之間的資金轉移。 

因為是資金交易, 預設60秒內完成  

兩個帳戶舉例如下
<img width="1136" height="757" alt="image" src="https://github.com/user-attachments/assets/34dfe597-981a-4be5-87b0-023ad8425f9f" />

### Session

Used to group database operations that are related to each other and should be run together




Using the mongosh tab, run the following command to create a session variable for the multi-document transaction:
  
```SQL
const session = db.getMongo().startSession()    # 進入mongodb Session
```

This command creates a new session, but it hasn't started a transaction yet. In the next step, you'll start the transaction using this session


Next, start a transaction using the session variable you created:
 
```SQL
session.startTransaction()                      # 開始交易
```

Now use the following command to create an account variable which is a reference to the accounts collection using the session you created:

```SQL
const account = session.getDatabase('bank').getCollection('accounts')                         # 帳戶合集的操作
```
```SQL
account.updateOne({ account_id: "MDB740836066"} , {$inc: {balance: -30}})
```
```SQL

account.updateOne({ account_id: "MDB963134500"} , {$inc: {balance: 30}})

```

Finally, complete the transaction on the session using the following command:

```sql
session.commitTranscation()
```



### Multi-Document Transactions
ACID transactions in MongoDB are typically used only by applications where values are exchanged between different parties, such as banking or business applications. If you find yourself in a scenario where a multi-document transaction is required, it's very likely that you will complete a transaction with one of MongoDB's drivers. For now, let's focus on completing and canceling multi-document transactions in the shell to become familiar with the steps.


### Using a Transaction: Video Code
Here is a recap of the code that's used to complete a multi-document transaction:

```SQL
const session = db.getMongo().startSession()    # 進入mongodb Session

session.startTransaction()                      # 開始交易

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')                         # 帳戶合集的操作

//Add database operations like .updateOne() here

session.commitTransaction()
```

### Aborting a Transaction
If you find yourself in a scenario that requires you to roll back database operations before a transaction is completed, you can abort the transaction. Doing so will roll back the database to its original state, before the transaction was initiated.


### Aborting a Transaction: Video Code
Here is a recap of the code that's used to cancel a transaction before it completes:
```sql
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')

//Add database operations like .updateOne() here

session.abortTransaction()
```


QA


You are creating a transaction that does the following:

* Inserts a new savings account into the accounts collection for an existing customer

* Funds the new savings account with $200 from their checking account

The transaction looks like this: 

```sql
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('bank').getCollection('accounts')

account.insertOne({
  account_id: "MDB361428849",
  account_holder: "Donna Wood",
  account_type: "savings",
  balance: 200.0,
  transfers_complete: [],
  last_updated: new Date()
})
account.updateOne( { account_id: "MDB919841472" }, {$inc: { balance: -200.00 }})
```


What method should you use to complete this transaction? (Select one.)

a.
endTransaction()
b.
commitTransaction()
c.
endSessions()
d.
completeTransaction()


==> B


A:.endTransaction() is not a valid method in MongoDB.


B:We use .commitTransaction() to commit a transaction.session.commitTransaction() should be placed at the end of the transaction.

C:endSessions() expires specific sessions and will not commit a transaction.

D:completeTransaction() is not a valid method in MongoDB.





Select one or more answer choices and then click "See Results" to submit.

Which of the following commands will output to the shell if they are successful?

a.
.startTransaction()
b.
.commitTransaction()
c.
.abortTransaction()

==> B








## Conclusion


### MongoDB Transactions
In this unit, you learned that ACID transactions ensure that database operations, such as transferring funds from one account to another, happen together or not at all. You also explored how ACID transactions work with the document model in MongoDB. Finally, you learned how to create and use multi-document transactions by using the startTransaction() and commitTransaction() commands, and how to cancel multi-document transactions by using the abortTransaction() command.


### Resources
Use the following resources to learn more about ACID transactions in MongoDB:

What are ACID transactions?
How do ACID transactions work in MongoDB?





[題目]  
In MongoDB CRUD Transactions, you are running a multi-document transaction across a replica set. Inside the transaction, you perform an insertOne() followed by an updateOne() on a different collection. The updateOne() throws a transient transaction error (error code 112 - WriteConflict). Which of the following describes the correct behavior and recommended approach in this situation? (difficulty: hard)  
  
[選項]  
A. MongoDB automatically retries only the failed updateOne() operation within the same transaction, preserving the insertOne() result, and the transaction continues normally after the retry succeeds.  
B. The entire transaction is aborted automatically by MongoDB, and the driver retries only the commit operation using a retry loop, without re-executing the transaction body.  
C. The entire transaction is aborted and the application must catch the transient error and retry the full transaction logic from the beginning, including both the insertOne() and the updateOne().  
D. The transaction is paused until the write conflict is resolved by the other conflicting operation completing, after which MongoDB resumes the transaction automatically from the point of failure.  
  
[正確答案]：  
C  
  
[核心考點]：  
  
[詳細解析]：  
本題考查 MongoDB 多文件交易中暫時性錯誤（TransientTransactionError）的處理機制。當交易主體內的操作（如 updateOne）因寫入衝突而失敗時，整個交易會被自動中止。此時應用程式必須捕獲該錯誤，並從頭重新執行整個交易邏輯（包含之前的 insertOne）。  
  
A. 錯誤原因：MongoDB 不會在交易內部自動重試單一失敗的語句。寫入衝突會破壞交易的 ACID 隔離狀態，因此無法單獨保留 insertOne 的結果。  
  
B. 錯誤原因：只有當交易在最後的「提交階段（Commit）」遇到暫時性網路錯誤時，才只需單獨重試 commit 動作。若在主體執行期間出錯，則必須重試整個交易主體。  

C. 正確原因：完全符合官方最佳實踐。寫入衝突（WriteConflict）屬於典型的暫時性錯誤，驅動程式或應用程式必須捕獲它，並透過重試迴圈從頭完整重新執行交易。  

D. 錯誤原因：MongoDB 不會主動暫停或在衝突解決後自動恢復交易。為了避免資料庫死鎖（Deadlock）並確保並發安全，系統會立即拋出錯誤並中止交易。  










