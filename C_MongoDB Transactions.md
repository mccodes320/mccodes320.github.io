
Lesson 1: Introduction to ACID Transactions  
Lesson 2: ACID Transactions in MongoDB  






Lesson 1: Introduction to ACID Transactions
---
https://learn.mongodb.com/learn/course/mongodb-transactions/lesson-1-introduction-to-acid-transactions/learn

ACID Transcation

An ACID transcation is a group of database operations that must happen together or not at all, ensuring database safety and consittency

使用場景在將記錄傳喚到另一則記錄中, 如金融交易與商品賣賣

Atomicity 原子性, 全部的操作只有同時成功或同時失敗

Guarantees that every transaction is "all or nothing" when committing data to a database. For example, we don't want money to be taken from an account but not successfully added to another.

Consistency 一致性

Guarantees that the data written to the database is consistent with database constraints. For example, if an account balance cannot be less than 0, a transaction would fail before violating this constraint.

Isolation 隔離

Guarantees that each transaction that is run concurrently leaves the database in the same state as if the transactions were run sequentially. In other words, multiple transactions can happen at the same time without affecting the outcome of the other transactions.

Durability 耐久性

Durability: Guarantees that data is never lost. Data is saved to non-volatile memory, so any modifications made to data by a successful transaction will persist, even in the event of a power or hardware failure.


QA


Which of the following is the best definition of an ACID transaction? (Select one.)

a.
A group of database operations that cannot fail.
b.
Database operations that involve transferring money between two parties.
c.
A group of database operations that must happen all together or not at all.
d.
Database operations that are durable and will persist even if the server is down.


==> C


a:A database operation involved in an ACID transaction can fail. But if an operation does fail, the rest will fail as well.

B:ACID transactions can be used to transfer funds or goods between parties, but they are not limited to this use case.

C: An ACID transaction involves a group of database operations that must happen together successfully or not at all.

D:  Durability is only one of the ACID properties that transactions have.





Which of the following scenarios require the use of an ACID transaction? (Select all that apply.)

a.
Updating a bank database to reflect the transfer of money from Customer A's bank account into Customer B's bank account.
b.
Updating inventory and shopping cart records when a customer adds an item to their online shopping cart in an ecommerce app.
c.
Updating records in a user database to bulk add user profile pictures.
d.
Adding multiple documents that contain information about movies to a database.


==> AB




A:In the case of a bank account transfer, the database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that all operations happen successfully and securely, or that they don't happen at all.


B:In most scenarios involving the transfer of value, inventory, or ownership of goods, database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that both the inventory and shopping cart records are updated together or not at all.


C:In this scenario, adding user profile pictures to a database probably does not need to be an ACID compliant transaction. ACID transactions are needed when database operations MUST happen atomically, such as most scenarios involving the transfer of value, inventory, or ownership of goods.


D: In this scenario, adding new documents that contain information about movies probably does not need to be an ACID-compliant transaction. ACID transactions are needed when database operations MUST happen atomically, such as most scenarios involving the transfer of value, inventory, or ownership of goods.




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
