
MongoDB Data Modeling Intro


* Lesson 1: Introduction to Data Modeling
* Lesson 2: Types of Data Relationships
* Lesson 3: Modeling Data Relationships
* Lesson 4: Embedding Data in Documents
* Lesson 5: Referencing Data in Documents
* Lesson 6: Scaling a Data Model
* Lesson 7: Using Atlas Tools for Schema Help
* Introduction to MongoDB Data Modeling
# The Subset Pattern


<img width="488" height="333" alt="image" src="https://github.com/user-attachments/assets/86b35937-f47f-4346-88ea-c4fa44f1d9c4" />


ref: https://learn.mongodb.com/learn/course/introduction-to-mongodb-data-modeling/lesson-1-introduction-to-data-modeling/learn?page=1













* Lession 1: Introduction to Data Modeling

先提問
What dose my applicayion do?
Whay data will I store ?
How will users access this data?
Whaat data will be most valuable to me?
再用問題描述需要處裡的任務與用戶想法
Your tasks as well as those of users
what your data looks like
The relationships among the data
The tooling you plan to hava
The access patterns that might emerge

建立好的data mode好處:
Make it easier to mange data
Make queries more efficient
Use less memory and CPU
Reduce costs

原則: 一起訪問的數據, 儲存再一起
* Data that is accessed together should be stored together.*

---



* Lesson 2: Types of Data Relationships


## One-to-one

A relationship where a data entity in one set is connected to exactly one data entity in another set

ex. 電影與導演

<img width="1301" height="544" alt="image" src="https://github.com/user-attachments/assets/d0ecfcc6-2183-4b00-8270-b4de91701f37" />


## One-to-many

A relationship where a data entity in one set is connected to any number of data entities in another set

ex. 電影與演員

<img width="1759" height="766" alt="image" src="https://github.com/user-attachments/assets/bbc0bcc4-fb87-4ebf-8a23-e905a9f4c4e3" />

可以用一個查詢得到所有所需要的數據

## Many-to-many

A relationship where any number of data entities in one set are connected to any number of data entities in another set


建模有主要兩種方式
## Embedding
We take related data and insert it into our document

## Referencing
We refer to documents in another collection in our document


<img width="1263" height="421" alt="image" src="https://github.com/user-attachments/assets/f0535cc5-6179-4da7-a812-e691beb1b064" />

<img width="910" height="469" alt="image" src="https://github.com/user-attachments/assets/c62e3198-e51a-4bd8-b6ca-6b5db105fa62" />



QA


Which of the following are common types of relationships that are found in every database? (Select all that apply.)

a.
One-to-one relationship
b.
One-to-many relationship
c.
Many-to-many relationship
d.
One-to-zillions relationship


=> abc


What is the type of relationship shown in the following document?
```json
{
    "_id": ObjectId("00000001"),
    "name": "Marnie Dupree",
    "grade": "Freshman",
    "studentId": 123456,
    "email": "mdupree@college.edu"
}
```
a.
One-to-one relationship
b.
One-to-many relationship
c.
Many-to-many relationship


=> a





---

* Lession 3: Modeling Data Relationships

QA

A legacy bank database has been ported to MongoDB, resulting in a set of collections that were mapped to their original tables.

You're tasked with redesigning the accounts collection of the banking database to make the information clearer. The bank would like you to keep the customers' contact information and account information separate.

The following is a sample document in the accounts collection:
```json
{
  "account_id": "MDB653115886",
  "account_holder": "Herminia Mckinney",
  "account_type": "savings",
  "balance": 6617.34,
  "street_num": 123,
  "street": "Main St",
  "city": "Tulsa",
  "state": "OK",
  "zip": 74008,
  "country": "USA",
  "home_phone": 1234567890,
  "cell_phone": 1111111111,
  "transfers": [
    ...
  ],    
}
```
Which of the following actions can be made to improve this schema? Consider the following requirements:

* Preserve the one-to-one relationship among all the fields.
* Keep the contact and account information separate.
* Store data together that is accessed together.

Hint
Remember that you can embed subdocuments and create separate collections.

a.
Create two collections: one for accounts and one for customer_info.
b.
Embed the phone numbers as a subdocument.
c.
Create two collections that have no overlapping fields.
d.
Keep the current schema as is.




=> aB

A:Creating two collections, one for accounts and one for customer_info, aligns with the customer's requirements. It also ensures that data that is accessed together is stored together.


B: Embedding the phone numbers as a subdocument can improve the schema, and it ensures that data that is accessed together is stored together.


C:Creating two collections that have no overlapping fields would not keep related information together, as there would not be references to link the two collections. This would not follow the principle of storing data together that is accessed together.


D:Keeping the current schema as is would not be the best option here given the requirements. The current schema does not keep contact information and account information separate, and it does not follow the principle of storing data together that is accessed together.


---

Lesson 4: Embedding Data in Documents

Embedding:
Avoids application joins

Provides better performance for read operations

Allows developers to update related data in a single write operation


WARNING:
Embedding data into a single document can create large documents.
Large documents have to be read into memory in full, which can result in a slow application performance for your end user.
Continuously adding data without limit creates unbounded documents


Embedded documents:
Store related data in a single document

Simplify queries and improves overall query performance

Ideal for one-to-many and many-to-many relationships among data

Has pitfalls like large documents and unbounded documents



QA

Which of the following statements is/are true about embedding data in documents? (Select all that apply.)

a.
Embedding data in documents simplifies queries.
b.
Embedding data in documents improves the overall performance of queries.
c.
Embedding data in documents makes your document smaller over time.
d.
Embedding data in documents never results in an unbounded document.

=> AB

A:Embedding data simplifies queries because it avoids application joins. It fulfills the principle that data that is accessed together should be stored together.


B:Embedding data provides better performance for read operations. Embedded documents enable you to store all kinds of related information in a single document.


C:Over time, embedding data in a single document can make your document increasingly larger as you add data. This can lead to excessive memory and add latency for reads because large documents must be read into memory in full.


D:When embedding data, you might structure your document in a way that data is added continuously, without limit. This creates an unbounded document, which might exceed the maximum BSON document size of 16 MB.



Which of the following relationship types often use embedding? (Select all that apply.)

a.
One-to-one relationship
b.
One-to-many relationship
c.
Many-to-many relationship

=> AB

A:With MongoDB, embedding a one-to-one relationship means putting the two pieces of information in the same document. You could also opt to use a subdocument to group related information, such as the components of an address.


B:Embedding is often used when there are one-to-many relationships in the data that's being stored. MongoDB recommends embedding documents to simplify queries and improve overall query performance.


C: Embedding is often used when there are many-to-many relationships in the data that's being stored. MongoDB recommends embedding documents to simplify queries and improve overall query performance. It is important to note that embedding this type of relationship may introduce data duplication.

---

* Lesson 5: Referencing Data in Documents

當想要將兩個資料儲存在不同的collection, 但也確保collection相關時使用


References:
Save the _id field of one document in another document as a link between the two
Simple and sufficient for most use cases

將一個文件的 _id 欄位儲存在另一個文件中，作為兩者之間的連結
簡單且足以應付大多數的使用場景




Using references is called linking or data normalization
使用引用（References）也被稱為連結（Linking）或資料正規化（Data Normalization）




Referencing:
No duplication of data
Smaller documents
Querying from multiple documents costs extra resources and impacts read performance


沒有重複的資料（資料不重疊）
文件容量較小
從多個文件中進行查詢會消耗額外的資源，並影響讀取效能

Here's a quick summary of the pro's and con's of embedding vs. referencing in MongoDB:


<img width="1505" height="911" alt="image" src="https://github.com/user-attachments/assets/79da1e2a-1994-414c-83f0-3adaf03c5910" />


<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/347ac4c4-8899-49ee-b0cf-22c38009e2b1" />


QA

Which of the following statements is/are true about referencing data in documents? (Select all that apply.)

a.
Referencing data in documents avoids duplication of data.
b.
Referencing data in documents may result in smaller documents.
c.
Referencing data in documents links documents by using the same field.
d.
Referencing data in documents improves read performance.



=> ABC


A:Referencing allows you to store data in two different collections and ensure that the collections are related. This avoids duplication of data.


B:
C:References save the _id field of one document in another document as a link between the two.


D:




Imagine the following are a sample of documents from a users collection:

```json

{
    "id": "AL001",
    "name": "Ella Richardson"
},
{
    "id": "AL002",
    "name": "Jackie Thomas"
},
{
    "id": "AL003",
    "name": "Justin McDonald"
},
```



Consider the following document from a posts collection, which contains data about a blog post and its comments. Which field is used as a reference? (Select one.)
```json
{
    "author": "Aileen Long",
    "title": "Learn MongoDB in 30 Mins",
    "published_date": ISODate("2020-05-18T14:10:30Z"),
    "tags": ["mongodb", "introductory", "database", "nosql"],
    "comments": [
        {
            "comment_id": "LM001",
            "user_id": "AL001",
            "comment_date": ISODate("2020-05-19T14:22:00Z"),
            "comment": "Great read!"
        },
        {
            "comment_id": "LM002",
            "user_id": "AL002",
            "comment_date": ISODate("2020-06-01T08:00:00Z"),
            "comment": "So easy to understand - thanks!"
        }
    ]
}
```

a.
comment_id
b.
comments
c.
date
d.
user_id


=> D


----

* Lesson 6: Scaling a Data Model



Optimum efficiency of:
1. Query result times

2. Memory usage

3. CPU usage

4. Storage

Unbounded documents are documents that grow infinitely.
無邊界文件（Unbounded documents）是指會無限增長的文件。


Problems as the array grows larger:
It will take up more space in memory  

May impact write performance  

Difficult to perform pagination of comments  

Maximum document size of 16 MB will lead to storage problems




QA


What are the effects of creating unbounded documents when embedding data? (Select all that apply.)

a.
Unbounded documents impact write performance.
b.
Unbounded documents improve pagination performance.
c.
Unbounded documents cause storage problems.

==> AC

A:Embedding data will make the document larger and impact write performance. As more data is added to each document, the entire document is rewritten into MongoDB data storage.


B:
C:Unbounded documents caused by embedding will eventually run into storage problems by exceeding the maximum document size of 16 MB.


D:









What is the recommended way to avoid the unbounded document sizes that may result from embedding?

a.
Break data into multiple collections and use references.
b.
Break data into multiple databases.
c.
Separate documents to store on different servers.

==>  A


A:To prevent unbounded document sizes that may result from embedding, you can break up your data into multiple collecitons and use references to keep frequently accessed data together.





What is MongoDB's principle for how you should design your data model?

a.
Data that is accessed together should be stored together.
b.
Data that is collected in the same day should be stored together.
c.
Data that is not in a one-to-one relationship should be stored together.

==> A

A:Data that is accessed together should be stored together. How you model your data depends entirely on your particular application's data access patterns. You want to structure your data to match the ways that your application queries and updates it.



---


Lesson 7: Using Atlas Tools for Schema Help



Which tab in Data Explorer shows ways to improve your schemas?

a.
Indexes
b.
Schema Anti-Patterns
c.
Find




==> B

B:The Schema Anti-Patterns tab highlights any issues in the collection and provides details to resolve them. You can improve your schema by resolving the anti-patterns that are shown.



What is the minimum Atlas Cluster tier that you must have to use the Performance Advisor tool?

a.
M0
b.
M10
c.
M30


==> A

a:The Performance Advisor tool is not available in the free M0 cluster tier.




---

* Introduction to MongoDB Data Modeling



https://learn.mongodb.com/learn/course/introduction-to-mongodb-data-modeling/conclusion/learn?client=customer&page=2

Introduction to MongoDB Data Modeling
In this unit, you learned how to:

Explain the purpose of data modeling.
Identify the types of data relationships (one to one, one to many, many to many).
Model data relationships.
Identify the differences between embedded and referenced data models.
Scale a data model.
Use Atlas Tools for schema help.

Resources
Use the following resources to learn more about the basics of data modeling:

Lesson 01: Introduction to Data Modeling
Data Modeling Introduction
Separating Data That is Accessed Together
Lesson 02: Types of Data Relationships
Data Model Design
Model Relationships Between Documents
Embedding MongoDB
MongoDB Schema Design Best Practices
Lesson 03: Modeling Data Relationships
Data Model Design
Model Relationships Between Documents
Lesson 04: Embedding Data in Documents
Embedding MongoDB
Model One-to-One Relationships with Embedded Documents
Model One-to-Many Relationships with Embedded Documents
Lesson 05: Referencing Data in Documents
Normalized Data Models
Model One-to-Many Relationships with Document References
Lesson 06: Scaling a Data Model
Operational Factors and Data Models
Performance Best Practices: MongoDB Data Modeling and Memory Sizing
Lesson 07: Using Atlas Tools for Schema Help
A Summary of Schema Design Anti-Patterns and How to Spot Them




# The Subset Pattern
    
一種資料建模技術，用於處理文件很大但查詢只需要其中一小部分資料的情況。它不是每次都把整個大文檔加載到內存中，而是將其拆分成一個熱文檔（包含頻繁訪問的字段）和一個冷文檔（包含很少訪問的完整歷史數據）。    
    
關鍵在於：MongoDB 會將整個文件載入到工作集（記憶體）中。如果一個文件有 500KB，但你只需要其中的 2KB，那麼你就浪費了記憶體——這會降低大規模應用時的效能。    
    

無subset Pattern    
```json
{
  _id ："user_001" ， 
  姓名：‘愛麗絲’ 
  頭像：“https://cdn.example.com/alice.jpg ” 
  貼文：[ /* 3,000 個完整的貼文物件 */ ]   
}
```


```json
// 主要（熱門）文件 — 使用者集合
{
  _id ："user_001" ， 
  姓名：‘愛麗絲’ 
  頭像：“https://cdn.example.com/alice.jpg ” 
  recent_posts : [ /* 僅顯示最新的 5 篇文章 */ ]   
}
 
// 擴充（冷）文件 — users_extended 集合
{
  user_id : "user_001" , 
  all_posts : [ /* 所有 3,000 篇文章 */ ],   
  追蹤者：[ /* 完整列表 */ ]，   
  以下：[ /* 完整列表 */ ]   
}
```





