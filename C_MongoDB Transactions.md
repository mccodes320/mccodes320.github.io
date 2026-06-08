
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

### Multi-Document Transactions
ACID transactions in MongoDB are typically used only by applications where values are exchanged between different parties, such as banking or business applications. If you find yourself in a scenario where a multi-document transaction is required, it's very likely that you will complete a transaction with one of MongoDB's drivers. For now, let's focus on completing and canceling multi-document transactions in the shell to become familiar with the steps.


### Using a Transaction: Video Code
Here is a recap of the code that's used to complete a multi-document transaction:

const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')

//Add database operations like .updateOne() here

session.commitTransaction()

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








