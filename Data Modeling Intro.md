
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



[題目]    
Which of the following are common types of relationships that are found in every database? (Select all that apply.)    
    
[選項]    
A. One-to-one relationship    
B. One-to-many relationship    
C. Many-to-many relationship    
D. One-to-zillions relationship    
    
[正確答案]：    
ABC    
    
[核心考點]：    
    
[詳細解析]：    
本題考查資料庫資料建模（Data Modeling）中最基本的三種實體關係（Entity Relationships）。無論是在關聯式資料庫（RDBMS）還是非關聯式資料庫（NoSQL）中，資料之間的關聯本質上都可以歸納為三種核心類型：一對一、一對多與多對多。    




[題目]    
What is the type of relationship shown in the following document?    
    
```JSON
{
    "_id": ObjectId("00000001"),
    "name": "Marnie Dupree",
    "grade": "Freshman",
    "studentId": 123456,
    "email": "mdupree@college.edu"
}
```    
[選項]    
A. One-to-one relationship    
B. One-to-many relationship    
C. Many-to-many relationship    

[正確答案]：    
A    

[核心考點]：

[詳細解析]：
本題考查 MongoDB 內嵌文件結構所表達的一對一（One-to-One）關係。在該 student 文件中，除了標準的 name 外，還包含 studentId（學生證號）與 email（電子郵件）。因為在現實業務邏輯中，一位學生通常只會擁有一個唯一的學生證號與一個主要學校信箱，這三個實體呈現一對一的緊密綁定關係。

A. 正確原因：此文件將學生基本資料、學生證號和電子郵件合而為一，這種將一對一關係的屬性直接「扁平化內嵌（Flattened Embedding）」在單一文件中的設計，是 MongoDB 處理一對一關係的最標準作法。

B. 錯誤原因：一對多（One-to-many）關係在 MongoDB 中通常會表現為「物件陣列（Array of Objects）」或「值陣列」，用以儲存多筆子資料。然而該文件中所有欄位皆為單一數值，並無陣列結構。

C. 錯誤原因：多對多（Many-to-many）關係在文件模型中，通常需要雙向的 ID 陣列引用或是獨立的關聯陣列來表達，此處單純的單一文件結構完全無法對應多對多場景。




---

* Lession 3: Modeling Data Relationships

[題目]
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

# Lesson 6: Scaling a Data Model


* 資料模型擴充的四個終極優化指標（Optimum Efficiency）:    
    1. Query result times ：縮短資料檢索與回應延遲。    
    2. 記憶體使用（Memory Usage：減少快取（WiredTiger Cache）與記憶體緩衝區的壓力。    
    3. CPU Usage：降低複雜聚合運算或無索引掃描帶來的運算負擔。    
    4. Storage：優化磁碟佔用率，提高壓縮效率    

* Unbounded documents are documents that grow infinitely
  無邊界文件（Unbounded documents）是指會無限增長的文件。
  會引發的四大效能災難：
  1. It will take up more space in memory ：    
     陣列過大時，讀取整份文件會佔用極大的記憶體與快取空間，降低緩存效率。    
  3. May impact write performance ：    
     在 MongoDB 中，當陣列增長導致文件體積超過原本分配的磁碟空間時，資料庫必須在磁碟上重新移動整份文件並重建索引，引發嚴重的寫入延遲。    
  4. Difficult to perform pagination of comments 分頁查詢困難：    
     極難對內嵌陣列中的資料（如特定幾條留言）進行高效的效能分頁（Pagination）。    
  5. 觸犯 16MB 鐵律：MongoDB 單一文件限制最大為 16MB。陣列無限增長最終會導致寫入失敗，引發系統中斷。    




[題目]
What are the effects of creating unbounded documents when embedding data? (Select all that apply.)

[選項]
A. Unbounded documents impact write performance.
B. Unbounded documents improve pagination performance.
C. Unbounded documents cause storage problems.

[正確答案]：
AC

[核心考點]：

[詳細解析]：
本題考查 MongoDB 中「無限制增長文件（Unbounded Documents）」反模式的負面影響。在內嵌（Embedding）一對多關係時，若子文件數量無上限（如無限的日誌或評論），文件體積會持續膨脹。這不僅會因為觸及 16MB 限制而引發儲存錯誤，還會因為每次更新都需將整個大文件重新寫入磁碟，嚴重拖慢寫入效能。

A. 正確原因：隨著文件體積因內嵌資料而無限制增長，每次有新資料加入時，資料庫都必須在磁碟中重新尋找空間並重寫整份大文件，這會帶來極大的 I/O 開銷並嚴重打擊寫入效能。

B. 錯誤原因：無限制增長的文件非但不能提升分頁效能，反而會破壞分頁表現。因為每次查詢都要載入包含無數子元素的超大文件到記憶體中，這會極大地消耗記憶體與網絡傳輸頻寬。

C. 正確原因：MongoDB 的單一 BSON 文件有 16MB 的實體容量上限。如果任由數據無限制地嵌入，文件最終必然會超過此限制，進而導致寫入失敗與嚴重的儲存管理問題。








[題目]
What is the recommended way to avoid the unbounded document sizes that may result from embedding?

[選項]
A. Break data into multiple collections and use references.
B. Break data into multiple databases.
C. Separate documents to store on different servers.

[正確答案]：
A

[核心考點]：

[詳細解析]：
本題考查如何有效避免 MongoDB 中因內嵌（Embedding）所導致的無限制增長文件（Unbounded Documents）反模式。當一對多的關係中，子文件的數量會無上限地增加時，官方與業界最推薦的標準解決方案即是將內嵌陣列「打破並獨立」出來，改以分開的集合（Collections）儲存，並透過 ObjectId 進行引用（References）關聯。

A. 正確原因：這是解決文件無限膨脹的最標準且推薦的做法。透過將子資料轉移至獨立集合並以引用關聯，能將無限長度的一對多轉化為獨立文件的水平擴展，完美杜絕觸及 16MB 上限與寫入效能下降的問題。

B. 錯誤原因：將資料分散到多個不同的「資料庫（Databases）」並無法解決單一文件體積過大的實體限制與設計缺陷，反而會大幅增加多庫管理的架構複雜度。

C. 錯誤原因：將文件存放在不同的服務器上屬於分片（Sharding）或分布式部署的範疇，雖然它能解決叢集的整體儲存量容量問題，但依然無法改變「單一主文件因過度內嵌而超過 16MB」的單點極限限制。




[題目]
What is MongoDB's principle for how you should design your data model?

[選項]
A. Data that is accessed together should be stored together.
B. Data that is collected in the same day should be stored together.
C. Data that is not in a one-to-one relationship should be stored together.

[正確答案]：
A

[核心考點]：

[詳細解析]：
本題考查 MongoDB 資料建模的最核心指導原則（Data Modeling Philosophy）。在非關聯式文件資料庫中，建模的精髓在於「圍繞應用程式的存取模式（Access Patterns）」來設計，而不是像傳統 RDBMS 一樣進行嚴格的正規化拆表。因此，最著名黃金法則即是「經常一起被存取的資料，就應該儲存在一起（Data that is accessed together should be stored together）」。

A. 正確原因：完美闡述了 MongoDB 的核心設計哲學。透過內嵌（Embedding）將經常需要同時讀寫的關聯資料放在同一個文件中，可以最大化減少關聯查詢（Join）的開銷，從而提供極高的讀寫效能。

B. 錯誤原因：資料庫的設計是基於業務邏輯和查詢特徵，而非單純根據「收集日期」來決定資料的實體嵌套結構。依日期歸類屬於資料封存或分區策略，非建模的核心哲學。

C. 錯誤原因：關係的類型（一對一、一對多、多對多）只是評估的維度之一，並非只要「不是一對一關係」就盲目全部儲存在一起。如果一對多關係中的子文件是無限制增長的，儲存在一起反而會引發反模式。




# Lesson 7: Using Atlas Tools for Schema Help



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
    

* 無subset Pattern    
    ```json
    {
      _id ："user_001" ， 
      姓名：‘愛麗絲’ 
      頭像：“https://cdn.example.com/alice.jpg ” 
      貼文：[ /* 3,000 個完整的貼文物件 */ ]   
    }
    ```

# Modeling Patterns

## **1. subset Pattern**

子集模式是 MongoDB 針對單一文件中數組無限增長問題的標準解決方案。當一對多關係（例如產品到評論）可能導致數組無限增長時，嵌入所有項目可能會超出 16MB 的 BSON 文檔大小限制，並且由於 MongoDB 會將整個文檔加載到內存中，從而降低效能。子集模式透過僅保留一個經過篩選的子集（通常是最新或最相關的項目）來解決這個問題，該子集直接嵌入到父文檔中，以便快速、頻繁地存取。完整的資料集儲存在單獨的集合中，並在需要時進行查詢。 MongoDB 的官方資料建模模式文件中明確提到了這種模式。桶模式是處理順序或時間序列資料的有效替代方案，但對於評論等用例來說，它會增加複雜性。計算模式和擴展引用模式分別解決了不同的問題（重複聚合和跨集合資料重複），並且無法阻止無限增長。

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

## **2. Computed Pattern**

計算模式可以減少重複計算，但無法解決數組無限增長的問題——它儲存的是聚合結果，而不是單一評論。這將導致無法檢索單一評論內容。

## **3. Bucket Pattern**

多個時間序列資料點分組到一個文件（「桶」）中，而不是將每個測量值儲存為單獨的文件。這顯著減少了文件數量，提高了壓縮率，並實現了高效的範圍查詢。預先計算的總和欄位（最小值、最大值、總和、計數）嵌入到每個桶中，以避免讀取時進行代價高昂的陣列解包

## **4. Extended Reference Pattern**

## **5. Outlier Pattern**

異常值模式是 MongoDB 的一種模式設計策略，用於處理集合中大多數文件的資料結構和大小相似，但少數文件（即異常值）包含的資料量卻顯著超出預期的情況。異常值模式並非圍繞這些異常值設計整個模式，而是保持典型文檔的緊湊性，並將多餘的資料重定向到單獨的溢出文檔中，並使用一個標誌字段來指示這種拆分。    
    
異常值模式會將異常文檔的資料有意拆分到一個主文檔和一個或多個溢出文檔中，而正常文檔則保持獨立。布林標誌（例如「has_extra_followers」）會向應用層發出訊號，表示需要取得並合併額外的文件才能產生完整的結果。如果沒有此檢查，應用程式會默默地將嵌入的陣列視為完整資料集，這對於大多數文件來說是準確的，但對於異常值來說是錯誤的。這種默默的不完整性尤其危險，因為它不會產生任何錯誤——查詢成功，但結果是錯誤的。此模式的全部價值取決於應用程式是否能根據該標誌正確地分支其邏輯：對於正常文檔，直接使用嵌入的資料；對於異常值，執行額外的查找和合併操作。這種設計權衡取捨需要略微增加應用程式程式碼的複雜性，以換取文件大小可控，並避免大多數用例中出現 BSON 文件大小限制問題。    





[題目]
In MongoDB one-to-many relationship modeling, a blogging platform stores authors and their blog posts. Each author can have hundreds of posts, and the application frequently needs to display an author's profile page showing their 5 most recent posts along with full author details on every post page. A developer is deciding between three approaches: 
(A) embed all posts inside the author document, 
(B) store posts in a separate collection with an author reference, and also store a 'recentPosts' array of the 5 latest post summaries inside the author document, or 
(C) store posts in a separate collection with only an author reference. 

Which approach best balances read performance and data consistency for this use case? (difficulty: hard)

[選項]
A. Store posts in a separate collection with only an author reference, and rely solely on $lookup aggregation to join data at query time
B. Store posts in a separate collection and embed the full author document inside every post document, so each post is fully self-contained
C. Embed all posts inside the author document, because embedding always provides the best read performance by avoiding joins
D. Store posts in a separate collection with an author reference, and store a 'recentPosts' summary array inside the author document (Extended Reference / Subset pattern)

[正確答案]：
D

[核心考點]：

[詳細解析]：
本題考查 MongoDB 進階設計模式中的「子集模式（Subset Pattern）」與「擴充引用模式（Extended Reference Pattern）」。當面對一對多（且多方數量龐大）的場景，若前端有極為頻繁且固定的查詢特徵（例如永遠只看最新 5 篇、或者看文章時必看作者簡介），將全量資料分開儲存以防文件膨脹，同時在主文件中複製一份「高頻存取的小子集」或「靜態擴充資訊」，是平衡讀取效能與維護成本的最優解。

A. 錯誤原因：雖然這種做法能確保強一致性，但在高併發、讀多寫少的部落格場景中，每次讀取首頁都必須實時進行 $lookup（記憶體 Join）運算，會帶來不必要的系統開銷與效能瓶頸。

B. 錯誤原因：在每篇章中都內嵌「完整」的作者文件會造成嚴重的資料冗餘與寫入放大（Write Amplification）。一旦作者修改了個人簡介或頭像，系統必須同步更新成百上千篇的文章文件，極易引發資料不一致。

C. 錯誤原因：當作者擁有數百篇以上文章時，直接內嵌所有文章會導致文件體積無限制增長（Unbounded），極易觸及 16MB 限制；且每次只想看作者簡介時，都會被迫從磁碟載入所有文章，非常浪費 I/O。

D. 正確原因：此為官方推薦的子集/擴充引用模式。將完整文章獨立存放以防膨脹，並在作者文件中僅冗餘最新 5 篇的「摘要陣列」供首頁直接一秒讀取。雖在寫入新文章時需要同步維護該陣列，但完美換取了極高的讀取效能，平衡度最高。











