### MongoDB Indexes

https://learn.mongodb.com/learn/course/mongodb-indexes

Lesson 1: Using MongoDB Indexes in Collections
Lesson 2: Creating a Single Field Index in MongoDB
Lesson 3: Creating a Multikey Index in
Lesson 4: Working with Compound Indexes in MongoDB
Lesson 5: Deleting MongoDB Indexes



**Lesson 1: Using MongoDB Indexes in Collections**  
--
1. **What indexes are**  
   * Special data structures, 易於偏歷和高校搜索的緒格式儲存一小部分的集合數據
   * Store small portion of the data
   * Ordered and easy to search efficiently
   * Point to document indetity, 指向文檔標示, 允許快速查詢和更新資料
![image](https://github.com/user-attachments/assets/64ec2f93-27f6-446d-9d79-944784de7fed)

2. **How indexes can improve performance**
   * Speed up queries
   * Reduce disk I/O
   * Reduce resources required
   * Support equaity matches and range-based operations and return sorted results. 支援查詢, 回傳排序結果.
   indexes會依據建立時所提供的所以欄位和排序, 將資料儲存在以建立的資料表中  

3. **Costs of using indexes**

   **ˇ3.1 Without indexes**  
   * MongoDB reads all documents(collection scan)
   * Sorts results in memory. 如果查詢要已有排序的方式輸出, 也會需要額外在記憶體中做排序.
  
   **3.2 With indexes**  
   * MongoDB only fetches the documents indentified by the index based on the query. 可以依照索引更快的回傳結果
  
   There is one default index per collection, which includes only the _id field  
   預設來說, 每個collection只建立一個索引  
   Every query should use an index  
   每個查詢都應該使用索引  

   注意: 索引具有寫入效能的成本, 在插入新的文件或更新時, 也需要針對索引去更動. 此外, 如果collection有太多索引, 反而會造成寫入效能降低.  
   
4. Most common index type:
   * Single field
   * Compound
   * Multikey indexes operate on an array field



## Q&A

**1. Which of the following statements about indexes are correct? (Select all the that apply.)**

a. Indexes are data structures that improve performance, support efficient equality matches and range-based query operations, and can return sorted results.
  
b. Indexes are automatically created based on usage patterns.  
       
c. Indexes are used to make querying faster for users. One of the easiest ways to improve the performance of a slow query is create indexes on the data that is used most often.
    
d. When using an index, MongoDB reads every document in a collection to check if it matches the query that's being run.  


```
Answer:
A,C

A. 說明：索引是提升效能的資料結構。它支持高效的等值匹配（Equality matches）與範圍查詢（Range-based queries），並能直接回傳排序後的結果。透過索引，MongoDB 僅需處理必要的數據，而不必掃描整個集合。

B. 說明：[錯誤]。MongoDB 不會根據使用習慣自動建立索引。開發者必須手動建立索引來優化慢查詢。不過，MongoDB Atlas 提供建議工具，可提示用戶哪些索引應該建立或移除。
  
C. 說明：[正確]。索引能讓查詢更快，因為 MongoDB 只需掃描索引即可找到所需數據。在頻繁使用的欄位上建立索引，是優化慢查詢最簡單且有效的方法之一。
  
D. 說明：[錯誤]。這描述的是「全表掃描（Collection Scan）」。當有索引可用時，MongoDB 不需要讀取集合中的每個文件，而是掃描索引以精確定位目標數據。
```

**2. Which of the following statements about indexes are true? (Select one.)**
   
A. Indexes improve query performance and have no impact on write performance.

b. Indexes improve query performance at the cost of write performance.

c. Indexes have no impact on query performance but improve write performance.

d. Indexes have a negative impact on query performance but improve write performance.



```
Answer:
B

[a] Indexes improve query performance and have no impact on write performance.

翻譯：索引能提升查詢效能，且對寫入效能沒有影響。

說明：[錯誤]。雖然索引能提升查詢速度，但它們是有代價的。每當執行寫入操作時，相關的索引都必須同步更新，這會消耗額外的時間。

[b] Indexes improve query performance at the cost of write performance.

翻譯：索引以犧牲寫入效能為代價，換取查詢效能的提升。

說明：[正確]。這是數據庫設計中經典的權衡（Trade-off）。對於大多數應用場景，這種交換是值得的。索引應該建立在頻繁被查詢的欄位，或是那些雖然查詢頻率低但運算成本極高的查詢上。

[c] Indexes have no impact on query performance but improve write performance.

翻譯：索引對查詢效能沒有影響，但能提升寫入效能。

說明：[錯誤]。事實正好相反。索引的主要目的是加速查詢流程，而寫入效能反而會因為需要更新索引結構而下降。

[d] Indexes have a negative impact on query performance but improve write performance.

翻譯：索引對查詢效能有負面影響，但能提升寫入效能。

說明：[錯誤]。索引對查詢有積極（正面）的影響，對寫入則會造成額外的負擔。


```


**Single field index**

Single field index: A single field index is an index on a single field of a document. MongoDB creates a single field index on the _id field by default, but additional indexes may be needed for other fields as well. A single field index can also be a multikey index if it operates on an array field.


**Compound index**

Compound index: MongoDB supports compound indexes, where a single index structure holds references to multiple fields within a collection's documents. A compound index is created by specifying the fields that the index should reference, followed by the order in which the fields should be sorted. The order of the fields in the index is important because it determines the order in which the documents are returned when querying the collection. A compound index can also be a multikey index if one of the fields is an array.

**Multikey index**

Multikey index: A multikey index is an index on an array field. Each element in the array gets an index key, which supports efficient querying against array fields. Both single field and compound indexes can have an array field, so there are both multikey single field indexes and multikey compound indexes.

| 索引類型                                  | 說明                                              | 特性說明                                                |
| ------------------------------------- | ----------------------------------------------- | --------------------------------------------------- |
| **Single Field Index**<br>（單一欄位索引） | 對單一欄位建立索引。預設 MongoDB 對 `_id` 欄位會建立此類型索引。        | 可提升該欄位查詢效率。<br>若該欄位為陣列，則此索引會變成 **Multikey Index**。  |
| **Compound Index**<br>（複合索引）       | 對**多個欄位**建立一組索引結構，可提升多欄位查詢效率。<br>建立時會指定欄位與排序方向。 | 查詢時需注意欄位的順序與排序，遵循「前綴原則」。<br>若其中有陣列欄位，也會變成 Multikey。 |
| **Multikey Index**<br>（多鍵索引）       | 專門用於索引陣列欄位，陣列中的每個值都會被索引。                        | 可應用於單一欄位或複合索引中，讓陣列查詢效率提升。<br>**不能同時索引多個陣列欄位**。      |

MongoDB 在 _id 欄位會自動建立單一欄位索引。

Multikey 索引是自動推斷的，不需特別指定，只要索引欄位是陣列就會自動變為 multikey。

複合索引查詢時建議遵守前綴欄位排序，如：{a: 1, b: 1} 能支援查詢 {a} 或 {a, b}，但不能只查 {b}。












**Lesson 2: Creating a Single Field Index in MongoDB**
-- 

Create a Single Field Index by using createIndex()

Enforce uniqueness

Identify indexes by using getIndexes()

Determine the indexes being userd in a query by using explain()


**Single Field Indexes**

Single Field Indexes 單字段索引  
Index on single field 單字段上的索引  
Support queries and sort on a single field 支持單字段上的查詢和排序   

```
db.coll.createdIndex({fieldname:1})
```

可以用createIndex()創建單個字串索引, 並將順序列列為1

在 Compound Index 多個字串排序時, 並須指定順序

**Creating a Single Field Index**
  
Review the code below, which demonstrates how to create a single field index in a collection.  


**Create a Single Field Index**   

Use **createIndex()** to create a new index in a collection. Within the parentheses of **createIndex()**, include an object that contains the field and sort order.  
```
db.customers.createIndex({
  birthdate: 1
})
```

**Create a Unique Single Field Index**

Add **{unique:true}** as a second, optional, parameter in **createIndex()** to force uniqueness in the index field values. Once the unique index is created, any inserts or updates including duplicated values in the collection for the index field/s will fail.

```
db.customers.createIndex({
  email: 1
},
{
  unique:true
})
```
MongoDB only creates the unique index if there is no duplication in the field values for the index field/s.


**View the Indexes used in a Collection**
Use **getIndexes()** to see all the indexes created in a collection.
```
db.customers.getIndexes()
```

**Check if an index is being used on a query**

查詢是否使用索引

Use **explain()** in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).

* The **IXSCAN** stage indicates the query is using an index and what index is being selected.
* The **COLLSCAN** stage indicates a collection scan is perform, not using any indexes.
* The **FETCH** stage indicates documents are being read from the collection.
* The **SORT** stage indicates documents are being sorted in memory.

| 階段名稱       | 說明                                      |
| ---------- | --------------------------------------- |
| `IXSCAN`   | **Index Scan**：代表正在使用索引進行查詢。這是你期望看到的結果。 |
| `COLLSCAN` | **Collection Scan**：全表掃描。代表沒有使用索引，效能差。  |
| `FETCH`    | 從集合中讀取實際文件內容，通常是搭配 IXSCAN 使用。           |
| `SORT`     | 表示在記憶體中進行排序，若沒有用索引排序會耗費更多資源。            |


```
db.customers.explain().find({
  birthdate: {
    $gt:ISODate("1995-08-01")
    }
  })
```
```
db.customers.explain().find({
  birthdate: {
    $gt:ISODate("1995-08-01")
    }
  }).sort({
    email:1
    })
```

**Q&A**

**What is a single field index? (Select one.)**

**a. An index that supports efficient querying against one field**
Correct. 

A single field index is an index that supports efficient querying against a single field. By default, all collections have a single field index on the _id field, but users can define additional indexes that support important queries. A single field index is also a multikey index if the value of the field is an array.

b.
An index that supports efficient querying against multiple fields
Incorrect. 

An index that supports efficient queries against multiple fields is called a compound index.

c.
An index that only supports efficient querying against fields with scalar values
Incorrect. 

Single field indexes can also support efficient querying against a single array field.

d.
An index that supports efficient querying against fields that are already indexed by another user-defined index
Incorrect. 

A single field index doesn't support efficient querying against fields that are already indexed by another user-defined index. When a single field is already indexed—for example, by a compound index—creating an additional single field index can cause over-indexing and performance issues.


**Q&A**

You have a collection of customer details. The following is a sample document from the collection:

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

You create a single field index on the email field, with the unique constraint set to true:
```
db.customers.createIndex({email:1}, {unique:true})
```
What would happen if you attempt to insert a new document with an email that already exists in the collection? (Select one.)

Correct Answer

a.
The new document will be inserted and replace the old document in the collection.
Incorrect. 

That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. The new document will not be inserted in this example because the email address already exists in another document in the collection.

b.
The new document will be inserted and the old document will remain in the collection.
Incorrect. 

That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. The new document will not be inserted in this example. Because the email address already exists in another document in the collection, the unique constraint would prevent the new document from being inserted.

c.
MongoDB will return a duplicate key error, and the document will be inserted.
Incorrect. 

That is not how the unique constraint operates. Unique indexes ensure that indexed fields do not store duplicate values. While a duplicate error key would be returned, the new document would not be inserted in this example because the email address already exists in another document in the collection.

d.
MongoDB will return a duplicate key error, and the document will not be inserted.
Correct. 

Unique indexes ensure that indexed fields do not store duplicate values. In this example, MongoDB will return a duplicate key error if you attempt to insert a new document with an email that already exists in the collection, as the unique constraint was set to true.


## Lesson 3: Creating a Multikey Index in MongoDB

How MongoDB works with array fields in an index 如何在索引中使用陣列欄位  

使用客戶集合來說明
![image](https://github.com/user-attachments/assets/f314acd7-ec55-4962-8fa9-091af5faf89b)

![image](https://github.com/user-attachments/assets/85773ca2-5a58-415e-9c0d-47ef9dc95578)

**Multikey Indexes**

Index on an array filed

Can be signle field or compound index

![image](https://github.com/user-attachments/assets/6897809a-faa2-4566-9e59-865d6454dc16)
以上範例為創建Multikey Indexes  

如果要做查詢, 希望找到具有特定帳號的客戶, 那就要對帳戶作索引

使用getIndexes() 來查找集合中的索引, 會輸出有三個索引

![image](https://github.com/user-attachments/assets/15d59410-75f4-4eaf-97f0-8bdbe60c83ec)

建立索引後

![image](https://github.com/user-attachments/assets/ac46d545-449d-435c-9dcb-9254fc94991e)

Multikey indexing in MongoDB:
MongoDB 中的多鍵索引：

Any index where one of the indexed fields contains an array
任何被索引欄位中包含陣列的索引

The array can hold nested objects or other field types
陣列可以包含巢狀物件或其他欄位類型

In a compound index, only one field can be an array per index
在複合索引中，每個索引只能有一個欄位是陣列


**Understanding Multikey Indexes**  
Review the code below, which demonstrates how multikey indexes work. If a single field or compound index includes an array field, then the index is a multikey index.


**Create a Single field Multikey Index**  
Use createIndex() to create a new index in a collection. Include an object as parameter that contains the array field and sort order. In this example accounts is an array field.
```
db.customers.createIndex({
  accounts: 1
})
```
**View the Indexes used in a Collection**
Use getIndexes() to see all the indexes created in a collection.

db.customers.getIndexes()

**Check if an index is being used on a query**
Use explain() in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).

* The IXSCAN stage indicates the query is using an index and what index is being selected.
* The COLLSCAN stage indicates a collection scan is perform, not using any indexes.
* The FETCH stage indicates documents are being read from the collection.
* The SORT stage indicates documents are being sorted in memory.
```
db.customers.explain().find({
  accounts: 627788
  })
```

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




## Lesson 4. Video: Working with Compound Indexes in MongoDB


**Compound indexes 複合索引**
What compound indexes are 什麼是複合索引  
How to create a compound index 如何建立複合索引  
How to use a compound index to sort or cover queries 如何使用複合索引來排序或覆蓋查詢  

Index on multiple fields
多欄位索引

Can be a multikey index if it includes an array field
如果包含陣列欄位，可以是多鍵索引

Maximum of one array field per index
每個索引最多只有一個陣列欄位

Support queries that match on the prefix of the index fields
支援對索引欄位前綴進行匹配的查詢

![image](https://github.com/user-attachments/assets/5d1afcdd-34fb-4675-a87c-c425597b06e6)

The order of the fields in a compound index matters
複合索引中欄位的順序很重要

Follow this order: Equality, Sort, Range
遵循此順序：相等、排序、範圍

The sort order of the field values in the index matters
索引中欄位值的排序順序很重要


**Equality**
相等性

Test exact matches on single field
測試單一欄位的精確匹配

Should be placed first in a compound index
應放在複合索引的第一位

Reduces query processing time
減少查詢處理時間

Retrieves fewer documents
檢索更少的文檔


**Sort**
排序

Determines the order of results
決定結果的順序

Index sort eliminates the need for in-memory sorts
索引排序消除了記憶體中排序的需要

Sort order is important if query results are sorted by more than 1 field and they mix sort orders
如果查詢結果按多個欄位排序且它們混合了排序順序，則排序順序很重要


**Working with Compound Indexes**
Review the code below, which demonstrates how to create a compound index in a collection.


**Create a Compound Index**
Use createIndex() to create a new index in a collection. Within the parentheses of createIndex(), include an object that contains two or more fields and their sort order.
```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```

**Order of Fields in a Compound Index**
The order of the fields matters when creating the index and the sort order. It is recommended to list the fields in the following order: Equality, Sort, and Range.

* Equality: field/s that matches on a single field value in a query
* Sort: field/s that orders the results by in a query
* Range: field/s that the query filter in a range of valid values
The following query includes an equality match on the active field, a sort on birthday (descending) and name (ascending), and a range query on birthday too.
```
db.customers.find({
  birthdate: {
    $gte:ISODate("1977-01-01")
    },
    active:true
    }).sort({
      birthdate:-1, 
      name:1
      })
```
Here's an example of an efficient index for this query:
```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```
View the Indexes used in a Collection
Use getIndexes() to see all the indexes created in a collection.
```
db.customers.getIndexes()
```
Check if an index is being used on a query
Use explain() in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.). Some of these are:

The IXSCAN stage indicates the query is using an index and what index is being selected.
The COLLSCAN stage indicates a collection scan is perform, not using any indexes.
The FETCH stage indicates documents are being read from the collection.
The SORT stage indicates documents are being sorted in memory.
```
db.customers.explain().find({
  birthdate: {
    $gte:ISODate("1977-01-01")
    },
  active:true
  }).sort({
    birthdate:-1,
    name:1
    })
```
**Cover a query by the Index**
An Index covers a query when MongoDB does not need to fetch the data from memory since all the required data is already returned by the index.

In most cases, we can use projections to return only the required fields and cover the query. Make sure those fields in the projection are in the index.

By adding the projection **{name:1,birthdate:1,_id:0}** in the previous query, we can limit the returned fields to only name and birthdate. These fields are part of the index and when we run the explain() command, the execution plan shows only two stages:

* IXSCAN - Index scan using the compound index
* PROJECTION_COVERED - All the information needed is returned by the index, no need to fetch from memory

```
db.customers.explain().find({
  birthdate: {
    $gte:ISODate("1977-01-01")
    },
  active:true
  },
  {name:1,
    birthdate:1, 
    _id:0
  }).sort({
    birthdate:-1,
    name:1
    })
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

## Lesson 5: Deleting MongoDB Indexes

Deleting an index can affect the query performance.
刪除索引可能會影響查詢效能。

dropIndex() to delete one index.
使用 dropIndex() 刪除一個索引。

dropIndexes() to delete more than one index.
使用 dropIndexes() 刪除多個索引。

hideIndex() to hide an index.
使用 hideIndex() 隱藏一個索引。

Delete an index in the Atlas UI with the Drop Index option.
在 Atlas UI 中，使用「Drop Index (刪除索引)」選項來刪除索引。


**Deleting an Index**
Review the code below, which demonstrates how to delete indexes in a collection.


**View the Indexes used in a Collection**
Use getIndexes() to see all the indexes created in a collection. There is always a default index in every collection on _id field. This index is used by MongoDB internally and cannot be deleted.
```
db.customers.getIndexes()
```
**Delete an Index**
Use dropIndex() to delete an existing index from a collection. Within the parentheses of dropIndex(), include an object representing the index key or provide the index name as a string.

Delete index by name:
```
db.customers.dropIndex(
  'active_1_birthdate_-1_name_1'
)
```
Delete index by key:
```
db.customers.dropIndex({
  active:1,
  birthdate:-1, 
  name:1
})
```
**Delete Indexes**
Use dropIndexes() to delete all the indexes from a collection, with the exception of the default index on _id.

db.customers.dropIndexes()
The dropIndexes() command also can accept an array of index names as a parameter to delete a specific list of indexes.
```
db.collection.dropIndexes([
  'index1name', 'index2name', 'index3name'
  ])
```

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

