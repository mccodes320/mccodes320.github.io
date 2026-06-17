Lesson 1: Replacing a Document in MongoDB
Lesson 2: Updating MongoDB Documents by Using updateOne()
Lesson 3: Updating MongoDB Documents by Using findAndModify()
Lesson 4: Updating MongoDB Documents by Using updateMany()
Lesson 5: Deleting Documents in MongoDB

https://learn.mongodb.com/learn/course/mongodb-crud-operations-replace-and-delete-documents/lesson-5-deleting-documents-in-mongodb/learn?page=1


# Lesson 1: Replacing a Document in MongoDB

**replaceOne()**

```sql
db.collection.replaceOne(filter, replcacement, options)
```

To replace documents in MongoDB, we use the replaceOne() method. The replaceOne() method takes the following parameters:

* filter: A query that matches the document to replace.
* replacement: The new document to replace the old one with.
* options: An object that specifies options for the update.

In the previous video, we use the **_id** field to filter the document. In our replacement document, we provide the entire document that should be inserted in its place. Here's the example code from the video:

```sql
db.books.replaceOne(
  {
    _id: ObjectId("6282afeb441a74a98dbbec4e"),
  },
  {
    title: "Data Science Fundamentals for Python and MongoDB",
    isbn: "1484235967",
    publishedDate: new Date("2018-5-10"),
    thumbnailUrl:
      "https://m.media-amazon.com/images/I/71opmUBc2wL._AC_UY218_.jpg",
    authors: ["David Paper"],
    categories: ["Data Science"],
  }
)


{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```



### Quit

Which of the following statements regarding the `replaceOne()` method for the MongoDB Shell (`mongosh`) are true? (Select all that apply.)

a.
This method is used to replace a single document that matches the filter document.

b.
This method accepts a filter document, a replacement document, and an optional options document.

c.
This method can replace multiple documents in a collection.

d.
This method returns a document containing an acknowledgement of the operation, a matched count, modified count, and an upserted ID (if applicable).

==> ABD

A: Correct! The `replaceOne()` method is used to replace a single document that matches the filter document.

B:Correct! The `replaceOne()` method accepts a filter document, a replacement document, and an optional options document.

C: Incorrect. The `replaceOne()` method is used to replace a single document that matches the filter document. Which of the other statements describe `replaceOne()`?

D:  Correct! The `replaceOne()` method returns a document containing an acknowledgement of the operation, a matched count, modified count, and an upserted ID (if applicable).

### Quit

You want to replace the following document from the birds collection with a new document that contains additional information on recent sightings, the scientific name of each species, and wingspan. What field should you use in the filter document to ensure that this specific document is replaced? (Select one.)
```sql
{ _id: ObjectId("6286809e2f3fa87b7d86dccd") },
  {
    common_name: "Morning Dove",
    habitat: ["urban areas", "farms", "grassland"],
    diet: ["seeds"]
  }
```
a.

{ _id: ObjectId("6286809e2f3fa87b7d86dccd") }

b.
{ diet: ["seeds"] }

c.
{ habitat: ["urban areas"] }

d.
{ scientific_name: "Zenaida macroura" }

==> A



A: Correct! Including the _id field as the filter document ensures that you’ll replace this specific document by using replaceOne().

B: Incorrect. { diet: ["seeds"] } is not a unique field, so you cannot ensure that you will replace this specific document. If you use the diet field as the filter, MongoDB will replace the first document that contains { diet: ["seeds"] }.

C: Incorrect. { habitat: ["urban areas"] } is not a unique field, so you cannot ensure that you will replace this specific document. If you use the diet field as the filter, MongoDB will replace the first document that contains { habitat: ["urban areas"] }.

D: Incorrect. The document that you want to replace does not contain { scientific_name: "Zenaida macroura" }. Using { scientific_name: "Zenaida macroura" } as the filter will replace another document.





# Lesson 2: Updating MongoDB Documents by Using updateOne()

updateOne

```sql
db.collection.updateOne(
  <filter>,
  <update>,
  {options}
)
```

$set operator:
Adds new fields and values to a document  
Replaces the value of a field with a specified value  

$pust
Appends a value to an array  
If absent, $push adds the array field with the value as its element  
Add elements to an array

upsert
Insert a document with provided information if matching documents don't exit.
The update operations will be carried out



## Updating MongoDB Documents by Using updateOne()  
The updateOne() method accepts a filter document, an update document, and an optional options object. MongoDB provides update operators and options to help you update documents. In this section, we'll cover three of them: $set, upsert, and $push.

**$set** 

The $set operator replaces the value of a field with the specified value, as shown in the following code:
```sql
db.podcasts.updateOne(
  {
    _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8"),
  },

  {
    $set: {
      subscribers: 98562,
    },
  }
)
```

**upsert**

The upsert option creates a new document if no documents match the filtered criteria. Here's an example:

```sql
db.podcasts.updateOne(
  { title: "The Developer Hub" },
  { $set: { topics: ["databases", "MongoDB"] } },
  { upsert: true }
)
```
**$push**

The $push operator adds a new value to the hosts array field. Here's an example:

```sql
db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
```


### Quit

You want to add an element to the **items** array field in the sales collection. To do this, what should you include in the update document? (Select one.)


A.

 { $set: { items:[{ “name”: "tablet", “price”: 200}] } }
B.

 { $update: { items:[{ “name”: "tablet", “price”: 200}] } }
C.

 { $push: { items:[{ “name”: "tablet", “price”: 200}] } }
D.

 { $upsert: { items:[{ “name”: "tablet", “price”: 200}] } }
 
a.
Option A

b.
Option B

c.
Option C

d.
Option D
 
==>  C




Air France has recently passed inspection. In the following document, you need to update the **results** field from Fail to Pass. To do this, what should you include in your update document? (Select one.)
```sql
{
  _id: ObjectId("56d61033a378eccde8a837f9"),
  id: '31041-2015-ENFO',
  certificate_number: 3045325,
  business_name: 'AIR FRANCE',
  date: 'Jun  9 2015',
  result: 'Fail',
  sector: 'Travel Agency - 440',
  address: {
    city: 'JAMAICA',
    zip: 11430,
    street: 'JFK INTL AIRPORT BLVD',
    number: 1
  }
}
```
A.
 { $set: {result: ‘Pass’} }
 
B.
 { $upsert: {result: ‘Pass’} }
 
C.
 { $insert: {result: ‘Pass’} }
 
D.
 { $push: {result: ‘Pass’} }
 
a.
Option A

b.
Option B

c.
Option C

d.
Option D


==> A







# Lesson 3: Updating MongoDB Documents by Using findAndModify()

Updating MongoDB Documents by Using findAndModify()

The findAndModify() method is used to find and replace a single document in MongoDB. It accepts a filter document, a replacement document, and an optional options object. The following code shows an example:

**findAndModify()**

Retures the document that has just been updated

Set new option to ture, 確保返回的資料是新版本

updateOne + findOne

**updateOne**

increment the downloads field

**findOne()**

Return the document using _id

**Problem with approach**

Two round trips to the server, findAndModify is only one

Another user could modify the document before findOne(), returning a different version of the document.

```sql
db.podcasts.findAndModify({
  query: { _id: ObjectId("6261a92dfee1ff300dc80bf1") },
  update: { $inc: { subscribers: 1 } },
  new: true,
})
```


```
{
  _id: ObjectId('6a2f5eb7bc8ac27e8830c34f'),
  name: 'alex003',
  mail: 'alex003@example.com',
  status: {
    login_count: 30,
    failed_attempts: 1,
    interests: [
      'gaming',
      'music'
    ]
  },
  age: '18',
  count: 84
}

db.user.findAndModify(
  {
    query:{name:"alex003"},
    update:{$inc:{count:1}},
    new:true
  })

{
  _id: ObjectId('6a2f5eb7bc8ac27e8830c34f'),
  name: 'alex003',
  mail: 'alex003@example.com',
  status: {
    login_count: 30,
    failed_attempts: 1,
    interests: [
      'gaming',
      'music'
    ]
  },
  age: '18',
  count: 85
}
```

### Quit 

Using the zips collection, you write the following query. This query updates the population, which is stored in the pop field, in one zip code in Santa Fe, New Mexico. What will be returned? (Select one.)
```sql
db.zips.findAndModify({
  query: { _id: ObjectId("5c8eccc1caa187d17ca72ee7") },
  update: { $set: { pop: 40000 } },
  new: true,
})
```

a.
The updated document, which contains a population of 40000

b.
The original document, prior to the update, which contains a population of 34054

c.
All documents with a population of 40000

d.
A new document that contains only an _id field and a population field


==> A 

a.
Correct! When the new option is set to true, findAndModify() returns the updated document. This query will return the updated document with a population of 40000.

b.
Incorrect. When the new option is set to true, findAndModify() returns the updated document. This query will return the updated document with a population of 40000.

c.
Incorrect. findAndModify() will update and return a single document, not multiple documents.

d.
Incorrect. findAndModify() will insert a new document only if the upsert option is set to true. This query does not include the upsert option.


### Quit

$#$#  

What would happen if you ran the following query on the zips collection? Note that there is currently no document for the city of Taos. (Select one.)
```sql
db.zips.findAndModify({
  query: { zip: 87571 },
  update: { $set: { city: "TAOS", state: "NM", pop: 40000 } },
  upsert: true,
  new: true,
})
```
a.
A new document would be inserted because the new option is set to true.

b.
A new document would be inserted because the upsert option is set to true.

c.
You would receive an error, because you cannot insert a new document when using the findAndModify() method.

==> B


a.
Incorrect. When the new option is set to true, the updated version of a document is returned, regardless of whether that document is new or existing.

b.
Correct! When the upsert option is set to true, a new document will be inserted if one does not already exist. For existing documents, the upsert option will cause the document to be updated.

c.
Incorrect. If you use findAndModify() to insert a new document without including the upsert option, you will receive an error or a null response,and the document will not be inserted. In this example, the document is inserted because the upsert option is set to true.



# Lesson 4: Updating MongoDB Documents by Using updateMany()

To update multiple documents, use the updateMany() method. This method accepts a filter document, an update document, and an optional options object. The following code shows an example:

**updateMany**

Not an all-or-nothing operation

Will not roll back updates 更新失敗會停止, 要重新執行update剩下的文黨

Updates will be visible as soon as they're performed 

Not appropriate for some use cases 不適用於金融交易等, 避免效能議題

Update nultioke documents with updateMany()

Accepts a filter dicumenbt, an update document, and an options object

Reutrns an output message showing the number if matched and midufied documents.

Filter document

Update document

Options object

```
db.collection.updateMany(
  <filter>,
  <update>,
  {options}
)

db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { $set: { status: "LEGACY" } }
)
```


### quit

Three computer science classes, with the class_ids of 377, 259, and 350, have earned 100 extra credit points by competing in a hackathon. You need to update the database so that all students who are in these classes receive extra credit points. Note that you will use the grades collection, which is in the sample_training database.

Which of the following queries will accomplish this goal? (Select one).


A.
```sql
db.grades.insertMany(
  {
    class_id: {
$in: [ 377, 259, 350 ]
    },
   },
  {
    $push: {
      scores: [ 
{ type : 'extra credit', score: 100 }
]
    }
  }
)
```

B.
```sql
db.grades.updateMany(
  {
    class_id: {
$in: [ 377, 259, 350 ]
    },
   },
  {
    $push: {
      scores: [ 
{ type : 'extra credit', score: 100 }
]
    }
  }
)
```

C.
```sql
db.grades.updateOne(
  {
    class_id: {
$in: [ 377, 259, 350 ]
    },
   },
  {
    $push: {
      scores: [ 
{ type : 'extra credit', score: 100 }
]
    }
  }
)
```


D.
```sql
db.grades.findAndModify(
  {
    class_id: {
$in: [ 377, 259, 350]
    },
   },
  {
    $push: {
      scores: [ 
{ type : 'extra credit', score: 100 }
]
    }
  }
)
```


a.
Option A
b.
Option B
c.
Option C
d.
Option D

===> B


### Quit

Select the area within the code snippet that corresponds to each question prompt.


<img width="1500" height="845" alt="image" src="https://github.com/user-attachments/assets/cbd8fea8-5316-42c7-988e-92a3b3520707" />






# Lesson 5: Deleting Documents in MongoDB

**Deleting Documents in MongoDB** 

To delete documents, use the deleteOne() or deleteMany() methods. Both methods accept a filter document and an options object.

**Delete One Document**

The following code shows an example of the deleteOne() method:

```sql
db.podcasts.deleteOne({ _id: ObjectId("6282c9862acb966e76bbf20a") })
```

**Delete Many Documents**

The following code shows an example of the deleteMany() method:

```sql
db.podcasts.deleteMany({category: “crime”})

{
  acknowledged: true,
  deletedCount: 1
}
```

## Quit


United Airlines is the only airline that has a route from the Denver Airport (DEN) to the Northwest Arkansas Airport (XNA). It has decided to cancel this route due to low ridership.

Which of the following queries will delete the route? (Select one.)

Note that these documents are contained in the routes collection in the sample_training database.

A.
db.routes.deleteOne({ "airline.name": "United Airlines"})

B.
db.routes.delete({ "airline.name": "United Airlines"})

C.
db.routes.delete({ src_airport: "DEN", dst_airport: "XNA"})

D.
db.routes.deleteOne({ src_airport: "DEN", dst_airport: "XNA"})


a.
Option A  
b.
Option B  
c.
Option C  
d.
Option D  


==> D


a. 
Incorrect. This query would delete the first document that has the airline name of United Airlines. Instead, you need to delete the document that departs from Denver, where the src_airport field is set to “DEN” and lands in the Northwest Arkansas airport, where the dst_airport field is set to “XNA”.

 
b.  
Incorrect. db.collection.delete() is not a valid collection method in MongoDB.

c.
Incorrect. Although the airports are correct, db.collection.delete() is not a valid collection method in MongoDB.

d.
Correct! The db.collection.deleteOne() method is used to delete a single document. The filter document contains the src_airport field with a value of “DEN” to specify a flight departing from Denver, and a dst_airport field with a value of “XNA” to specify a flight landing in the Northwest Arkansas Airport.




Air Berlin has filed for bankruptcy and ceased operations. You need to update the routes collection to delete all documents that contain an airline name of Air Berlin. Which of the following queries should you use? (Select one.)

A.
db.routes.deleteOne({ "airline.name": "Air Berlin"})

B.
db.routes.delete("Air Berlin")

C.
db.routes.deleteMany({ "airline.name": "Air Berlin"})

D.
db.routes.deleteMany("Air Berlin")

a.
Option A  
b.
Option B  
c.
Option C  
d.
Option D  


==> C






### Conclusion



MongoDB CRUD Operations: Replace and Delete Documents
In this unit, you learned how to modify query results with MongoDB. Specifically, you:

Replaced a single document by using db.collection.replaceOne().
Updated a field value by using the $set update operator in db.collection.updateOne().
Added a value to an array by using the $push update operator in db.collection.updateOne().
Added a new field value to a document by using the upsert option in db.collection.updateOne().
Found and modified a document by using db.collection.findAndModify().
Updated multiple documents by using db.collection.updateMany().
Deleted a document by using db.collection.deleteOne().

Resources
Use the following resources to learn more about modifying query results in MongoDB:

Lesson 01: Replacing a Document in MongoDB
MongoDB Docs: replaceOne()
Lesson 02: Updating MongoDB Documents by Using updateOne()
MongoDB Docs: Update Operators
MongoDB Docs: $set
MongoDB Docs: $push
MongoDB Docs: upsert
Lesson 03: Updating MongoDB Documents by Using findAndModify()
MongoDB Docs: findAndModify()
Lesson 04: Updating MongoDB Documents by Using findAndModify()
MongoDB Docs: updateMany()
Lesson 05: Deleting Documents in MongoDB
MongoDB Docs: deleteOne()
MongoDB Docs: deleteMany()
Previous



### 小測驗



題目 1：[單選題]
某電子書城系統正在維護一個名為 books 的集合。開發人員發現某一本書的 BSON 結構過於混亂，決定使用 replaceOne() 將該文檔完全替換為一個乾淨、重構後的全新文檔。
已知目標文檔的 _id 為 ObjectId("6282afeb441a74a98dbbec4e")。
請問下列哪一個 replaceOne() 的操作語法是完全正確且符合 MongoDB 官方規範的？

A.

JavaScript


db.books.replaceOne(
  { _id: ObjectId("6282afeb441a74a98dbbec4e") },
  { $set: { title: "MongoDB Architecture", isbn: "1484235967", authors: ["David Paper"] } }
)
B.

JavaScript


db.books.replaceOne(
  { _id: ObjectId("6282afeb441a74a98dbbec4e") },
  { title: "MongoDB Architecture", isbn: "1484235967", authors: ["David Paper"] }
)
C.

JavaScript


db.books.replaceOne(
  { title: "MongoDB Architecture", isbn: "1484235967", authors: ["David Paper"] },
  { _id: ObjectId("6282afeb441a74a98dbbec4e") }
)
D.

JavaScript


db.books.replaceOne(
  [ { _id: ObjectId("6282afeb441a74a98dbbec4e") } ],
  [ { title: "MongoDB Architecture", isbn: "1484235967", authors: ["David Paper"] } ]
)
正確答案： B
核心考點： replaceOne() 的參數順序與替換文檔（Replacement Document）的結構限制
詳細解析：

選項 B 正確的原因： replaceOne(filter, replacement, options) 的第二個參數是 replacement（替換文檔）。替換文檔必須是一個由花括號 {} 包裹的全新完整文檔，用來直接覆蓋舊文檔。選項 B 的參數順序正確（先過濾，後替換），且替換文檔結構完全合法。

其他選項錯誤的原因：

選項 A 錯誤：這是 C100DEV 關於 replaceOne 最高頻的經典陷阱！ replaceOne() 的替換文檔中絕對不能包含任何原子更新操作符（如 $set、$inc 等）。如果包含更新操作符，MongoDB 會直接拋出錯誤（replaceOne, literal replacement object expected）。更新操作符只能用於 updateOne() 或 updateMany()。

選項 C 錯誤：參數順序完全顛倒。replaceOne() 必須第一個傳入過濾條件（filter），第二個傳入新文檔（replacement）。

選項 D 錯誤：replaceOne() 處理的是單一文檔，參數必須是 JSON 物件 {}，不能用 JavaScript 陣列 [] 包裹。





題目 2：[複選題 (選 3 個)]
在一個鳥類生態觀測系統中，後端工程師成功執行了一次 db.birds.replaceOne(...) 操作，並收到了來自 MongoDB Shell (mongosh) 的返回值對象（Result Object）。
請問在操作成功執行且確實修改了資料的情況下，該返回的結果對象中保證會包含下列哪些欄位？（Select 3 answers）

A. acknowledged
B. matchedCount
C. modifiedCount
D. deletedCount

正確答案： A, B, C
核心考點： replaceOne() 操作傳回值對象的結構
詳細解析：

選項 A、B、C 正確的原因： 根據 MongoDB 官方文件與範例，replaceOne() 執行後會返回一個固定的結果文檔，用以向客戶端回報執行狀態。該物件固定包含：acknowledged（操作是否被許可/確認）、matchedCount（符合過濾條件的文檔數量）、modifiedCount（實際被修改/替換的文檔數量），以及 upsertedCount / upsertedId（若使用了 upsert 參數）。

其他選項錯誤的原因：

選項 D 錯誤：deletedCount 是執行刪除操作（如 deleteOne() 或 deleteMany()）時才會返回的狀態欄位，文檔替換操作（Replace）中不會出現此欄位。



題目 3：[單選題]
某飯店預訂系統的 rooms 集合中有多個房間文檔。因為全面翻新，某一間特定的豪華套房需要更新其所有的硬體配置與說明。目前該房間文檔在資料庫中的狀態如下：

JSON


{
  "_id": ObjectId("6286809e2f3fa87b7d86dccd"),
  "room_type": "Deluxe Suite",
  "status": "available",
  "amenities": ["TV", "WiFi", "Minibar"]
}
為了確保在執行 replaceOne() 時，「百分之百只會精確替換掉這一個特定的房間文檔」，而不會誤傷到其他同樣是豪華套房（Deluxe Suite）或同樣處於可用狀態（available）的房間，開發人員應該在第一個參數 filter 文檔中指定哪一個欄位？

A. { room_type: "Deluxe Suite" }
B. { amenities: ["TV", "WiFi", "Minibar"] }
C. { _id: ObjectId("6286809e2f3fa87b7d86dccd") }
D. { status: "available" }

正確答案： C
核心考點： 利用唯一的 _id 欄位確保替換操作的精確性與安全性
詳細解析：

選項 C 正確的原因： 在 MongoDB 中，_id 欄位是集合內所有文檔的主鍵（Primary Key），具有全域唯一性（Unique Constraint）。在 replaceOne() 的 filter 中使用 _id 進行比對，能保證有且僅有該筆特定的文檔會被匹配並替換，這是官方強烈推薦的最佳實踐。

其他選項錯誤的原因：

選項 A、D 錯誤：room_type 與 status 都不是唯一鍵欄位。如果使用這兩個欄位作為過濾條件，MongoDB 只會找到資料庫中「第一筆」符合條件的文檔進行替換，極有可能誤將其他房間的資料完全覆蓋。

選項 B 錯誤：amenities 是一個普通陣列，多個房間都可能擁有相同的設施列表，不具備唯一性，且陣列精確匹配容易因為元素順序不同而失效。


題目 4：[單選題]
假設一個高併發的物聯網（IoT）設備日誌集合 devices 中，存在多筆擁有相同 device_name 但時間戳記不同的文檔。
此時，一個初階開發人員寫了以下程式碼並在生產環境中執行：

JavaScript


db.devices.replaceOne(
  { device_name: "Sensor-B10" },
  { device_name: "Sensor-B10", status: "deactivated", firmware: "v2.1" }
)
已知資料庫中目前有 5 筆 device_name 為 "Sensor-B10" 的文檔。請問該指令執行後，資料庫會發生什麼事？

A. 5 筆符合條件的文檔都會被全新的文檔完全替換。
B. 系統會拋出錯誤，因為 replaceOne() 發現匹配到多筆文檔時會拒絕執行。
C. 只有系統隨機排序或內部索引掃描到的第一筆符合條件的文檔會被替換，其餘 4 筆文檔保持不變。
D. 什麼都不會發生，因為 replaceOne() 強制要求過濾條件必須包含 _id 欄位。

正確答案： C
核心考點： replaceOne() 在面對多重匹配（Multiple Matches）時的行為限制
詳細解析：

選項 C 正確的原因： 正如其名，replaceOne() 的設計初衷與底層行為就是「只替換單一一個文檔（Single Document）」。如果給予的 filter 條件匹配到了集合中的多筆文檔，MongoDB 不會報錯，而是會根據內部的儲存引擎順序或索引掃描順序，選取找到的「第一筆」文檔進行完全替換，其他符合條件的文檔則會被完全忽略。

其他選項錯誤的原因：

選項 A 錯誤：這是 updateMany()（或概念上的 replaceMany，但 MongoDB 官方並無提供 replaceMany）才會做的事，replaceOne 絕不會影響多筆文檔。

選項 B、D 錯誤：replaceOne() 並不強制要求過濾條件必須是 _id，它允許非唯一鍵過濾，匹配到多筆時也不會拋出異常，只會遵循「只處理第一筆」的原則。


題目 5：[單選題]
某電商系統的客戶支援團隊，需要處理一筆退貨用戶的資料修正。原有的用戶文檔結構如下：

JSON


{
  "_id": ObjectId("6282afeb441a74a98dbbec4e"),
  "username": "customer_gold",
  "tier": "Gold",
  "joined_date": ISODate("2023-01-15T00:00:00Z")
}
管理員執行了以下替換指令：

JavaScript


db.users.replaceOne(
  { _id: ObjectId("6282afeb441a74a98dbbec4e") },
  { tier: "Platinum", note: "Upgraded due to high loyalty" }
)
請問操作成功完成後，該文檔在資料庫中的最終完整模樣會變成什麼？

A.

JSON


{
  "_id": ObjectId("6282afeb441a74a98dbbec4e"),
  "username": "customer_gold",
  "tier": "Platinum",
  "joined_date": ISODate("2023-01-15T00:00:00Z"),
  "note": "Upgraded due to high loyalty"
}
B.

JSON


{
  "_id": ObjectId("6282afeb441a74a98dbbec4e"),
  "tier": "Platinum",
  "note": "Upgraded due to high loyalty"
}
C.

JSON


{
  "tier": "Platinum",
  "note": "Upgraded due to high loyalty"
}
D. 系統會拋出錯誤，因為新文檔漏掉了原本必填的 username 和 joined_date 欄位。

正確答案： B
核心考點： replaceOne() 造成的「完全覆蓋（Full Overwrite）」效果與 _id 的不變性
詳細解析：

選項 B 正確的原因： 這是文檔替換（Replace）與局部更新（Update）最本質的區別。replaceOne() 會把舊文檔中除了 _id 以外的所有舊欄位（如原有的 username、joined_date）全部抹除，並用你在第二個參數傳入的新物件做徹底的覆蓋。而 _id 欄位作為文檔的唯一標識符，在替換過程中會被自動保留，不會遺失。因此最終文檔會留下原本的 _id，其餘欄位完全變成新傳入的模樣。

其他選項錯誤的原因：

選項 A 錯誤：這是誤把 replaceOne() 的行為當成了 updateOne() 配合 $set 的局部更新行為。

選項 C 錯誤：雖然新文檔會覆蓋舊文檔，但 _id 欄位是不會被移除或改變的，它會自動延續。

選項 D 錯誤：MongoDB 集合預設是動態 Schema（無 Schema 限制），不存在「漏掉原本欄位會報錯」的限制。


C100DEV 模擬考題：文檔更新（updateOne）篇
題目 1：[單選題]
某跨國航空公司因安全複查合格，需要更新 inspections 集合中特定商戶的審查結果。該商戶目前的文檔結構如下：

JSON


{
  "_id": ObjectId("56d61033a378eccde8a837f9"),
  "id": "31041-2015-ENFO",
  "business_name": "AIR FRANCE",
  "result": "Fail",
  "sector": "Travel Agency - 440"
}
開發人員的任務是僅將 result 欄位的值從 "Fail" 改為 "Pass"，並保持其他所有原有欄位（如 business_name、sector 等）完好無損。請問下列哪一個更新文檔（Update Document）能夠正確達成這個目的？

A. { result: "Pass" }
B. { $set: { result: "Pass" } }
C. { $push: { result: "Pass" } }

D. { $insert: { result: "Pass" } }

正確答案： B
核心考點： $set 操作符的欄位局部替換特性
詳細解析：

選項 B 正確的原因： 根據 MongoDB 官方筆記，$set 操作符專門用於局部修改文檔。如果該欄位已存在，它會用指定的值取代該欄位原本的值；如果不存在則會新增。使用 { $set: { result: "Pass" } } 可以精確修改 result，而不會影響文檔中的其他數據。

其他選項錯誤的原因：

選項 A 錯誤：在 updateOne() 中，如果更新參數不包含任何以 $ 開頭的操作符，MongoDB 會拋出錯誤或將其誤認為全體替換操作。局部更新必須顯式聲明操作符。

選項 C 錯誤：$push 是專門用於陣列（Array）欄位的操作符。由於目前的 result 是一個字串（Scalar Value），對其執行 $push 會導致型態衝突錯誤。

選項 D 錯誤：$insert 不是 MongoDB 官方支援的合法更新操作符，屬於高仿真干擾項。



題目 1：[單選題]
某跨國航空公司因安全複查合格，需要更新 inspections 集合中特定商戶的審查結果。該商戶目前的文檔結構如下：

JSON


{
  "_id": ObjectId("56d61033a378eccde8a837f9"),
  "id": "31041-2015-ENFO",
  "business_name": "AIR FRANCE",
  "result": "Fail",
  "sector": "Travel Agency - 440"
}
開發人員的任務是僅將 result 欄位的值從 "Fail" 改為 "Pass"，並保持其他所有原有欄位（如 business_name、sector 等）完好無損。請問下列哪一個更新文檔（Update Document）能夠正確達成這個目的？

A. { result: "Pass" }
B. { $set: { result: "Pass" } }
C. { $push: { result: "Pass" } }

D. { $insert: { result: "Pass" } }

正確答案： B
核心考點： $set 操作符的欄位局部替換特性
詳細解析：

選項 B 正確的原因： 根據 MongoDB 官方筆記，$set 操作符專門用於局部修改文檔。如果該欄位已存在，它會用指定的值取代該欄位原本的值；如果不存在則會新增。使用 { $set: { result: "Pass" } } 可以精確修改 result，而不會影響文檔中的其他數據。

其他選項錯誤的原因：

選項 A 錯誤：在 updateOne() 中，如果更新參數不包含任何以 $ 開頭的操作符，MongoDB 會拋出錯誤或將其誤認為全體替換操作。局部更新必須顯式聲明操作符。

選項 C 錯誤：$push 是專門用於陣列（Array）欄位的操作符。由於目前的 result 是一個字串（Scalar Value），對其執行 $push 會導致型態衝突錯誤。

選項 D 錯誤：$insert 不是 MongoDB 官方支援的合法更新操作符，屬於高仿真干擾項。



題目 2：[單選題]
某電商系統的訂單集合 sales 允許用戶在完成交易後，將新購買的商品附加到 items 陣列中。
已知某個用戶目前的文檔結構中，items 欄位原本是一個包含一個物件的陣列。現在，系統需要向該陣列中追加一個全新的商品物件：{ "name": "tablet", "price": 200 }。
請問在 updateOne() 的第二個參數中，應該如何撰寫更新文檔？

A. { $push: { items: { "name": "tablet", "price": 200 } } }
B. { $set: { items: { "name": "tablet", "price": 200 } } }
C. { $update: { items: [ { "name": "tablet", "price": 200 } ] } }
D. { $upsert: { items: { "name": "tablet", "price": 200 } } }

正確答案： A
核心考點： $push 操作符在陣列元素的追加應用
詳細解析：

選項 A 正確的原因： 官方筆記明確指出，$push 操作符的功能是「Appends a value to an array」（將一個值追加到陣列中）。當我們想在不破壞舊有陣列元素的前提下加入新成員，應該使用 $push 指向該陣列欄位。

其他選項錯誤的原因：

選項 B 錯誤：如果使用 $set，它會把原本的 items 整個陣列抹除，並將其替換為單一個物件 { "name": "tablet", "price": 200 }，導致原本的歷史購買商品全部遺失。

選項 C、D 錯誤：$update 和 $upsert 皆非 MongoDB 的合法更新操作符。upsert 是一個布林值選項（Option），必須放在第三個參數中，不能當作第二個參數的操作符。


題目 3：[單選題]
播客管理系統（Podcasts System）正在準備錄製一個全新的節目，名為 "The Developer Hub"。由於這是一個全新的節目，目前資料庫的 podcasts 集合中完全沒有任何符合 { title: "The Developer Hub" } 的文檔。
開發人員執行了以下指令：

JavaScript


db.podcasts.updateOne(
  { title: "The Developer Hub" },
  { $set: { topics: ["databases", "MongoDB"] } },
  { upsert: true }
)
請問該指令執行成功後，資料庫的實際變化是什麼？

A. 什麼都不會發生，且 matchedCount 為 0，因為資料庫中找不到符合過濾條件的文檔。
B. 系統會拋出錯誤，因為 updateOne 預設不允許在查無資料時自動建立文檔。
C. 系統會自動建立一筆全新文檔，該文檔同時包含 title: "The Developer Hub" 以及 topics: ["databases", "MongoDB"] 兩個欄位。
D. 系統會自動建立一筆全新文檔，但該文檔只會包含 topics 欄位，過濾條件中的 title 欄位會被忽略。

正確答案： C
核心考點： upsert: true 選項觸犯時的文檔自動構建機制
詳細解析：

選項 C 正確的原因： 根據 MongoDB 官方對於 upsert（Update + Insert）的定義：「Insert a document with provided information if matching documents don't exist.」當過濾條件（filter）找不到任何匹配文檔，且指定了 { upsert: true } 時，MongoDB 會創建一個全新文檔。最重要的是，這個新文檔會自動融合過濾條件中的查詢欄位（title）以及更新操作符中的修改欄位（topics）。

其他選項錯誤的原因：

選項 A、B 錯誤：忽略了 upsert: true 參數的作用，該參數就是為了解決「查無資料則新增」的業務情境。

選項 D 錯誤：在 upsert 生成新文檔時，過濾條件中的等值查詢欄位會被自動寫入新文檔中，不會被忽略。


題目 4：[複選題 (選 2 個)]
假設某播客節目文檔中原本完全沒有 hosts（主持人）這個欄位（即該欄位為 absent 狀態）。
現在，系統管理員希望為該文檔新增主持人資訊，並執行了以下指令：

JavaScript


db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
請問關於此操作的執行結果與 MongoDB 潛在行為，下列哪兩個敘述是正確的？（Select 2 answers）

A. 系統會成功執行，因為當欄位不存在時，$push 會隱式自動建立一個名為 hosts 的陣列，並將 "Nic Raboy" 作為第一個元素。
B. 系統會拋出錯誤，因為 $push 強制要求目標欄位必須預先存在且必須是陣列型態。
C. 執行成功後返回的結果對象中，modifiedCount 的值將會是 1。
D. 執行成功後返回的結果對象中，upsertedCount 的值將會是 1。

正確答案： A, C
核心考點： $push 面對缺失欄位時的隱式行為與傳回值對象狀態辨識
詳細解析：

選項 A 正確的原因： 官方筆記特別提及：「If absent, $push adds the array field with the value as its element」。這代表即使文檔原本沒有該欄位，MongoDB 也不會報錯，而是會非常智能地幫你初始化該陣列。

選項 C 正確的原因： 由於這是一次對已存在文檔的局部內容修改，因此它屬於標準的修改操作，返回結果的 modifiedCount（修改計數）必定為 1。

其他選項錯誤的原因：

選項 B 錯誤：不符合 MongoDB 動態 Schema 的設計哲學與 $push 的官方規範。

選項 D 錯誤：upsertedCount 只有在「整個文檔都不存在，且因為設定了 upsert: true 而建立全新文檔」時才會大於 0。本題是透過 _id 匹配現有文檔，只是該文檔缺少某個欄位，所以不屬於 upsert。


題目 5：[單選題]
某日誌分析系統的 logs 集合中包含大量文檔。初階工程師想要對某個已知的錯誤日誌文檔進行欄位維護。該文檔的結構如下：

JSON


{
  "_id": ObjectId("6282afeb441a74a98dbbec4e"),
  "error_code": 404,
  "tags": ["network", "timeout"]
}
該工程師寫出了以下查詢與更新指令：

JavaScript


db.logs.updateOne(
  { _id: ObjectId("6282afeb441a74a98dbbec4e") },
  { $set: { error_code: 500, message: "Internal Server Error" } }
)
請問當該更新操作成功執行後，該文檔在資料庫中的 tags 欄位 會發生什麼變化？

A. tags 欄位會被自動移除，因為更新文檔中沒有提到它。
B. tags 欄位會保持不變，依然是 ["network", "timeout"]。
C. tags 欄位的值會被自動清空，變成一個空陣列 []。
D. 系統會拋出語法錯誤，因為 $set 不能同時修改一個舊欄位並新增一個新欄位。

正確答案： B
核心考點： $set 的精確局部修改特性（與 Replace 的本質區別）
詳細解析：

選項 B 正確的原因： 這是 C100DEV 考試中用於區分 replaceOne()（完全覆蓋）與 updateOne() 搭配 $set（局部更新）的黃金考點。使用 $set 時，只有在 $set 物件中被明確指定的欄位才會被修改或新增。其餘未被提及的欄位（如本題的 tags）在更新過程中會得到完全的保留，不受任何影響。

其他選項錯誤的原因：

選項 A、C 錯誤：這是混淆了 replaceOne() 的完全覆蓋行為（Replace 會抹除沒提到的欄位）。

選項 D 錯誤：$set 語法完全支援在同一個程式區塊內同時對多個欄位進行修改與新增，用逗號隔開即可，這是標準合法的操作。


C100DEV 模擬考題：findAndModify 篇
題目 1：[單選題]
在一個高併發的播客（Podcast）訂閱系統中，每當有新用戶點擊訂閱，系統都需要將該節目的訂閱人數（subscribers）加 1，並且必須立刻獲取更新後的精確訂閱人數以展示在前端。
開發人員在 podcasts 集合中執行了以下指令：

JavaScript


db.podcasts.findAndModify({
  query: { _id: ObjectId("6261a92dfee1ff300dc80bf1") },
  update: { $inc: { subscribers: 1 } },
  new: true
})
已知該文檔在執行前的 subscribers 值為 99。請問該指令執行成功後，會向應用程式返回什麼內容？

A. 返回一個包含 { acknowledged: true, modifiedCount: 1 } 的操作結果對象。
B. 返回更新之前的舊文檔，其內部的 subscribers 值為 99。
C. 返回更新之後的全新文檔，其內部的 subscribers 值為 100。
D. 返回一個全新建立的文檔，該文檔只包含 _id 和 subscribers: 1 兩個欄位。

正確答案： C
核心考點： findAndModify() 中 new: true 選項的傳回值行為
詳細解析：

選項 C 正確的原因： 根據 MongoDB 官方文件的規範，findAndModify() 預設會返回修改前的舊文檔。但是，當明確將 new 選項設置為 true 時（new: true），它會確保操作在資料庫底層修改完成後，返回剛被更新過的新版本文檔。因此，傳回的文檔中 subscribers 的值會是自增後的 100。

其他選項錯誤的原因：

選項 A 錯誤：這是 updateOne() 方法的返回值結構。findAndModify() 的特點是直接返回文檔本身（Document），而不是操作回報對象。

選項 B 錯誤：如果將 new 設為 false（或不設定該參數），才會返回更新前的舊文檔。

選項 D 錯誤：findAndModify() 只有在設定了 upsert: true 且查無資料時才會插入新文檔，本題是透過 _id 精確匹配既有文檔並局部更新，故不會遺失其他欄位。


題目 2：[單選題]
某遊戲公司使用 user 集合來記錄玩家資訊。某一筆玩家文檔在資料庫中的初始狀態如下：

JSON


{
  "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"),
  "name": "alex003",
  "count": 84
}
後端開發人員在沒有設定 new 參數的情況下，執行了以下指令：

JavaScript


db.user.findAndModify({
  query: { name: "alex003" },
  update: { $inc: { count: 1 } }
})
當這道指令執行完畢且資料庫已成功將 count 欄位修改為 85 後，應用程式中接收到的變數（返回值）其內部的 count 數值會是多少？

A. 84
B. 85
C. 1
D. undefined

正確答案： A
核心考點： findAndModify() 預設返回值行為（預設為 new: false）
詳細解析：

選項 A 正確的原因： 這是官方 C100DEV 認證中非常經典的細節陷阱。在 findAndModify() 中，new 選項的預設值是 false。這意味著如果開發人員沒有顯式聲明 new: true，MongoDB 預設會返回「修改前（Prior to the update）」的原始文檔。雖然此時資料庫內的值確實已經成功自增變成了 85，但返回給應用程式的文檔中，count 依然是舊的 84。

其他選項錯誤的原因：

選項 B 錯誤：只有在主動加上 new: true 的情況下，返回值才會是更新後的 85。

選項 C、D 錯誤：不符合 MongoDB 傳回整個原始文檔的設計行為。



題目 3：[複選題 (選 2 個)]
架構師在評審代碼時，發現有新進工程師為了實現「更新並獲取文檔」，寫了如下的兩行程式碼：

JavaScript


// 步驟 1：先執行更新
db.user.updateOne({ name: "alex003" }, { $inc: { count: 1 } });
// 步驟 2：再執行查詢獲取文檔
let updatedDoc = db.user.findOne({ name: "alex003" });
架構師指出，在高併發（High Concurrency）的業務場景下，這種「先 updateOne 再 findOne」的作法存在嚴重的架構缺陷。
請問改用 findAndModify() 能夠解決上述代碼的哪些潛在問題？（Select 2 answers）

A. 解決網路往返延遲（Network Round Trips）問題：原寫法需要與資料庫伺服器進行兩次網路交互，而 findAndModify() 只需要一次。
B. 解決競態條件（Race Condition）與資料不一致問題：避免在步驟 1 與步驟 2 的空隙中，有其他併發用戶修改了該文檔，導致 findOne 拿到非預期的版本。
C. 解決全表掃描（COLLSCAN）問題：findAndModify() 在底層會自動強制使用索引，而 findOne() 預設不會使用索引。
D. 解決存儲引擎鎖定（Locking）問題：findAndModify() 可以完全避免資料庫產生任何讀寫鎖定。

正確答案： A, B
核心考點： findAndModify() 的原子性（Atomicity）與效能優勢
詳細解析：

選項 A 正確的原因： 官方筆記中明確提到「Two round trips to the server, findAndModify is only one」。合併操作能有效減少網路開銷，這在高併發系統中對降低延遲（Latency）至關重要。

選項 B 正確的原因： 這是 NoSQL 分散式系統的核心觀念。非原子性的兩步操作（先改後查）在併發時，極有可能在「剛改完但還沒查」的瞬間，有另一個線程插隊把資料又改成了其他值，導致 findOne() 拿到的資料被污染。而 findAndModify() 是一個單一的原子性操作（Atomic Operation），在隔離的鎖定範圍內一次性完成修改與返回，保證了資料的一致性。

其他選項錯誤的原因：

選項 C 錯誤：是否使用索引取決於 query 條件中的欄位（如 name）有沒有建立索引，與使用 findOne 還是 findAndModify 無關。

選項 D 錯誤：findAndModify() 依然需要依賴 WiredTiger 存儲引擎的行級鎖（Document-level Locking）來保證原子性，無法也不應該完全避免鎖定。




題目 4：[單選題]
在線上物流系統中，需要處理郵遞區號集合 zips 的人口數據更新。已知過濾條件為 { _id: ObjectId("5c8eccc1caa187d17ca72ee7") }。
開發人員執行了以下指令，試圖更新人口數據（pop）：

JavaScript


db.zips.findAndModify({
  query: { _id: ObjectId("5c8eccc1caa187d17ca72ee7") },
  update: { $set: { pop: 40000 } },
  new: true
})
假設這道指令在執行時，資料庫中完全沒有任何符合該 _id 的文檔。請問該指令的執行結果為何？

A. 系統會自動插入一筆全新文檔，該文檔包含該 _id 以及 pop: 40000。
B. 指令成功執行，但返回結果為 null，且資料庫中不會新增任何文檔。
C. 系統會拋出 DocumentNotFoundException 運行時錯誤。
D. 系統會自動將該更新操作轉為全表廣播，修改第一筆掃描到的文檔。

正確答案： B
核心考點： findAndModify() 查無資料時的預設行為（缺失 upsert 參數）
詳細解析：

選項 B 正確的原因： 這道題直接引用了官方範例題目的核心邏輯。findAndModify() 雖然包含了尋找與修改的功能，但當 query 條件在資料庫中找不到任何匹配的對象時，它預設只會默默地返回 null，而不會主動做任何事。

其他選項錯誤的原因：

選項 A 錯誤：只有在配置物件中明確加入 { upsert: true } 選項時，查無資料才會觸發自動插入新文檔的機制。

選項 C 錯誤：查無匹配資料在 MongoDB 查詢中屬於正常合法的執行結果，不會拋出異常。

選項 D 錯誤：屬於毫無依據的錯誤邏輯，MongoDB 絕不會在查無資料時去隨機修改其他無關的文檔。



題目 5：[單選題]
某資深架構師在審查 MQL 程式碼規範時，指出了一段不符合 MongoDB 官方標準的 findAndModify 語法錯誤。
請從下列四個選項中，選出語法與結構完全合法且正確的 findAndModify() 呼叫方式：

A.

JavaScript


db.user.findAndModify(
  { name: "alex003" },
  { $inc: { count: 1 } }
)
B.

JavaScript


db.user.findAndModify({
  filter: { name: "alex003" },
  replacement: { $inc: { count: 1 } },
  returnNewDocument: true
})
C.

JavaScript


db.user.findAndModify({
  query: { name: "alex003" },
  update: { $inc: { count: 1 } },
  new: true
})
D.

JavaScript


db.user.findAndModify(
  [ { query: { name: "alex003" } } ],
  [ { update: { $inc: { count: 1 } } } ]
)
正確答案： C
核心考點： findAndModify() 的參數封裝物件語法規範
詳細解析：

選項 C 正確的原因： 根據 MongoDB 官方標準筆記與定義，findAndModify() 的設計與普通的 updateOne() 不同，它接受單一個大物件作為參數，而這個大物件內部必須使用特定的官方鍵名（Keys）來進行宣告，包括：query（過濾條件）、update（更新內容或替換文檔）、new（是否返回新文檔）等。選項 C 完全符合此標準規範。

其他選項錯誤的原因：

選項 A 錯誤：這是誤用了 updateOne(filter, update) 的多參數傳遞格式。如果把條件和更新拆成兩個獨立物件傳給 findAndModify()，會導致語法解析錯誤。

選項 B 錯誤：使用了虛構或混淆的鍵名（如 filter、replacement、returnNewDocument 均非這一個特定 Method 的法定鍵名），這在 mongosh 中會無法被正確識別。

選項 D 錯誤：錯誤地將物件包裹在陣列 [] 中，違反了該 API 的參數定義。





C100DEV 模擬考題：多文檔更新（updateMany）篇
題目 1：[單選題]
某線上教育平台的系統中，有三個班級（class_id 分別為 377、259 和 350）的學生在一場黑客松競賽中獲得了 100 分的額外加分。
現在，架構師要求更新 grades 集合，確保這三個班級內的所有學生文檔，都能在其 scores 陣列欄位中成功追加該筆加分紀錄：{ type: "extra credit", score: 100 }。
請問下列哪一個 MongoDB 查詢語法能夠完全且正確地達成這個商務目標？

A.

JavaScript


db.grades.updateOne(
  { class_id: { $in: [377, 259, 350] } },
  { $push: { scores: { type: "extra credit", score: 100 } } }
)
B.

JavaScript


db.grades.updateMany(
  { class_id: { $in: [377, 259, 350] } },
  { $push: { scores: { type: "extra credit", score: 100 } } }
)
C.

JavaScript


db.grades.insertMany(
  { class_id: { $in: [377, 259, 350] } },
  { $push: { scores: { type: "extra credit", score: 100 } } }
)
D.

JavaScript


db.grades.findAndModify(
  { class_id: { $in: [377, 259, 350] } },
  { $push: { scores: { type: "extra credit", score: 100 } } }
)
正確答案： B
核心考點： 批量文檔更新方法 updateMany() 的應用
詳細解析：

選項 B 正確的原因： 根據官方範例題目的核心邏輯，當我們需要更改符合特定條件（本題為三個班級編號之一）的「多個（Multiple）」文檔時，必須使用 updateMany() 方法。配合 $in 運算子匹配多個班級，以及 $push 運算子將物件追加到陣列中，選項 B 是唯一能夠修改集合中所有符合條件學生的正確做法。

其他選項錯誤的原因：

選項 A 錯誤：updateOne() 的底層行為是「不論匹配到多少筆，永遠只會更新找到的第一筆文檔」，因此無法涵蓋所有三個班級的學生。

選項 C 錯誤：insertMany() 是專門用來「插入全新文檔」的方法，其參數結構要求為文檔陣列，不能搭配過濾條件（filter）與更新操作符（如 $push），執行時會拋出異常。

選項 D 錯誤：findAndModify() 是用於查找並修改「單一文檔」的原子操作，無法批次修改多個學生的資料。



題目 2：[複選題 (選 3 個)]
在進行 C100DEV 官方認證考核前，考生必須充分理解 updateMany() 的底層行為特徵。
請問關於 MongoDB Shell 中的 updateMany() 方法，下列哪些敘述是完全符合官方文件描述的？（Select 3 answers）

A. 該方法接受三個核心參數：過濾文檔（Filter document）、更新文檔（Update document）以及一個可選的選項對象（Options object）。
B. 當其中某一筆文檔在更新過程中因硬體故障或網絡中斷而失敗時，MongoDB 會自動將該指令先前已完成更新的文檔全部回滾（Rollback）至初始狀態。
C. 執行成功後，該方法會返回一個輸出訊息，顯示匹配到的文檔數量（matchedCount）以及實際被修改的文檔數量（modifiedCount）。
D. 每當其中一筆文檔完成更新，該修改對其他併發的讀取操作來說是「即時可見（Visible as soon as they're performed）」的，而非等待全體更新完畢後才一次性釋放。

正確答案： A, C, D
核心考點： updateMany() 的參數結構、返回值與資料可見性（Visibility）特性
詳細解析：

選項 A 正確的原因： 標準語法為 db.collection.updateMany(<filter>, <update>, {options})，包含這三個重要區塊。

選項 C 正確的原因： 官方筆記中明確提到，updateMany「Returns an output message showing the number of matched and modified documents」，這能讓客戶端確認實際受影響的文檔總數。

選項 D 正確的原因： 官方核心筆記明確指出：「Updates will be visible as soon as they're performed」。MongoDB 的 updateMany 採取逐筆更新並即時寫入的機制，各文檔的變更會立即對外暴露。

其他選項錯誤的原因：

選項 B 錯誤：這是 C100DEV 關於 updateMany 最經典且必考的觀念陷阱！ 筆記強調，updateMany「Not an all-or-nothing operation; Will not roll back updates」。如果在更新 100 筆文檔的過程中，在第 50 筆時發生失敗，前 49 筆已經成功的更新會維持在資料庫中，絕對不會進行自動回滾，開發人員必須在重新連線後手動或透過程式重新執行更新剩餘的文檔。


題目 3：[單選題]
某電子書城系統儲存了大量的書籍文檔。為了清理過期的舊資料，開發團隊需要執行一項維護任務：將所有出版日期（publishedDate）早於 2019 年 1 月 1 日的書籍，其 status 欄位全部修改為 "LEGACY"。
請問下列哪一個 MQL 指令能夠完全正確且安全地實現這個業務情境？

A.

JavaScript


db.books.updateMany(
  { publishedDate: { $lt: "2019-01-01" } },
  { $set: { status: "LEGACY" } }
)
B.

JavaScript


db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { status: "LEGACY" }
)
C.

JavaScript


db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { $set: { status: "LEGACY" } }
)
D.

JavaScript


db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { $push: { status: "LEGACY" } }
)
正確答案： C
核心考點： BSON Date 類型的正確比較與 $set 更新操作符的組合
詳細解析：

選項 C 正確的原因： 根據官方筆記範例，在進行時間欄位的比較時，必須使用 JavaScript 的 new Date() 構造函數，將字串封裝為正確的 BSON Date 類型物件。同時，在更新文檔中，必須使用 $set 操作符來局部更改（或建立）status 欄位的值。

其他選項錯誤的原因：

選項 A 錯誤：在過濾條件中直接使用字串 "2019-01-01"，這會導致 MongoDB 在底層將 Date 類型與 String 類型進行比對，由於 BSON 類型不匹配，查詢將無法正確匹配到任何書籍。

選項 B 錯誤：在更新文檔中漏掉了 $set 操作符。直接傳入 { status: "LEGACY" } 會引發語法錯誤，因為 updateMany() 強制要求第二個參數必須包含合法的原子更新操作符。

選項 D 錯誤：$push 是專門用於將元素追加到陣列（Array）的。本題的業務需求是將 status 設定為一個標量字串（Scalar String），使用 $push 會與既有的字串型態衝突或產生非預期的陣列結構。


題目 4：[單選題]
在架構評審會議上，一位來自關係型資料庫（如 MySQL）背景的開發人員提出：「我們應該在所有涉及核心帳務、高併發金融轉帳交易（如：同時扣減多個錢包賬戶餘額）的場景中，全面使用 db.collection.updateMany() 來加速多賬戶的批量修改，因為它天然支援分散式全體事務回滾。」
作為資深資料庫架構師，你應該如何依據 MongoDB 官方的最佳實踐與底層設計來回應此觀點？

A. 完全贊同。因為 updateMany() 的底層是全有或全無（All-or-nothing）的原子性操作，非常適合金融事務。
B. 糾正該觀點。因為 updateMany() 逐筆執行的特性並「不適合（Not appropriate）」某些高度嚴格的金融交易場景。若需跨多文檔的金融原子性，應避免單獨使用此方法，以防止更新中途失敗後留下部分修改、部分未修改的混亂狀態。
C. 糾正該觀點。因為 updateMany() 只能用於更新字串欄位，無法處理數值計算（如餘額扣減）。
D. 完全贊同。因為 updateMany() 在執行中途若發生錯誤，會自動聯絡其他分片（Shards）強制撤銷先前所有的可見變更。

正確答案： B
核心考點： updateMany() 的適用場景限制（金融場景的潛在風險防範）
詳細解析：

選項 B 正確的原因： 這是官方筆記中提及的架構師級思考：「Not appropriate for some use cases」。由於 updateMany() 在底層並非一個不可分割的單一事務（Not an all-or-nothing operation），且更新在執行時會即時生效並對外可見。如果在金融轉帳等要求強一致性（Strong Consistency）與 ACID 事務的場景下，更新到一半發生斷電，將導致嚴重的帳目對不齊、資料不一致災害。因此，在不搭配多文檔分散式事務（Multi-document Transactions）的情況下，單獨使用 updateMany() 是不適合此類情境的。

其他選項錯誤的原因：

選項 A、D 錯誤：這兩項完全違背了 updateMany() 的真實機制，把 MongoDB 誤認為具備傳統關聯式資料庫批次操作的自動集體回滾功能。

選項 C 錯誤：updateMany() 搭配 $inc 或 $set 當然可以處理數值計算，其限制在於「事務邊界與原子性」，而非資料型態。





題目 5：[單選題]
某系統的日誌集合 logs 裡有數萬筆文檔。開發人員在 MongoDB Shell 中執行了以下多文檔更新指令：

JavaScript


db.logs.updateMany(
  { level: "ERROR", processed: false },
  { $set: { processed: true, review_date: new Date() } }
)
執行完畢後，Shell 視窗輸出了以下 BSON 結果訊息：

JSON


{
  "acknowledged": true,
  "insertedId": null,
  "matchedCount": 150,
  "modifiedCount": 0,
  "upsertedCount": 0
}
請根據這個傳回的訊息，判斷下列哪一個關於資料庫實際狀態的推論是完全正確的？

A. 資料庫中根本沒有任何符合 { level: "ERROR", processed: false } 條件的日誌文檔。
B. 系統成功找到了 150 筆符合條件的日誌文檔，並且將這 150 筆文檔的 processed 欄位全部修改成了 true。
C. 系統成功找到了 150 筆文檔，但這 150 筆文檔目前的 processed 本來就已經是 true 且 review_date 也完全一致，因此沒有發生任何實際的欄位值改變。
D. 操作引發了靜默失敗（Silent Failure），雖然匹配到了 150 筆，但因為沒有加上 upsert: true 導致修改計數全部歸零。

正確答案： C
核心考點： 深入辨識 updateMany() 返回結果中 matchedCount 與 modifiedCount 的對比含義
詳細解析：

選項 C 正確的原因： 這是 C100DEV 考試中檢驗開發人員是否細心觀察操作回報的經典細節題。結果顯示 "matchedCount": 150，代表過濾條件確實精確匹配到了 150 筆文檔。然而，"modifiedCount": 0 說明實際被修改的文檔數為 0。在 MongoDB 的優化機制中，如果發現準備寫入的新值與該文檔目前儲存的舊值完全一模一樣，MongoDB 為了節省磁碟 I/O，會跳過該筆文檔的實際寫入操作，從而導致「匹配到了，但修改計數為 0」的現象。

其他選項錯誤的原因：

選項 A 錯誤：若沒有符合條件的文檔，matchedCount 應該顯示為 0，而非 150。

選項 B 錯誤：如果全部修改成功且數值發生了變化，modifiedCount 應該顯示為 150。

選項 D 錯誤：這不是失敗，而是正常的等值冪等行為（Idempotent Behavior）；且 upsert 與此處匹配到既有文檔後的修改計數無關。




C100DEV 模擬考題：文檔刪除（Delete Operations）篇
題目 1：[單選題]
聯合航空（United Airlines）是唯一營運從丹佛機場（DEN）飛往西北阿肯色機場（XNA）航線的航空公司。由於搭載率持續低迷，該公司決定永久取消這一條特定的航線。
這批數據儲存在 sample_training 資料庫的 routes 集合中。
請問下列哪一個 MQL 指令最符合官方最佳實踐，能精確且安全地刪除這條航線文檔？

A. db.routes.deleteOne({ "airline.name": "United Airlines" })
B. db.routes.delete({ "airline.name": "United Airlines" })
C. db.routes.delete({ src_airport: "DEN", dst_airport: "XNA" })
D. db.routes.deleteOne({ src_airport: "DEN", dst_airport: "XNA" })

正確答案： D
核心考點： 精確過濾條件與 deleteOne() 的組合應用
詳細解析：

選項 D 正確的原因： 根據業務場景，目標是刪除「特定起訖點（DEN 到 XNA）」的單一航線。使用 deleteOne() 可以確保即便有其他匹配資料也只會刪除一筆。而過濾條件同時指定 src_airport: "DEN" 與 dst_airport: "XNA"，能精確鎖定該條航線，符合官方範例規範。

其他選項錯誤的原因：

選項 A 錯誤：這是一個非常高明的邏輯陷阱。如果只使用 { "airline.name": "United Airlines" } 作為過濾條件，MongoDB 會直接刪除它掃描到的「第一筆」聯合航空的航線文檔（例如可能是紐約飛洛杉磯），進而造成嚴重的數據誤刪。

選項 B、C 錯誤：這是 C100DEV 考試中最常見的語法陷阱！ db.collection.delete() 不是 MongoDB 官方標準 Shell (mongosh) 的合法集合方法。調用此方法會直接拋出 TypeError 運行時異常。


題目 2：[單選題]
柏林航空（Air Berlin）已正式宣告破產並停止所有商業營運。作為資料庫架構師，你需要對 routes 集合進行維護，將系統中所有屬於柏林航空的航線文檔全部予以清除。
請問你應該使用下列哪一個查詢指令？

A. db.routes.deleteOne({ "airline.name": "Air Berlin" })
B. db.routes.delete("Air Berlin")
C. db.routes.deleteMany({ "airline.name": "Air Berlin" })
D. db.routes.deleteMany("Air Berlin")

正確答案： C
核心考點： 批量刪除方法 deleteMany() 的使用場景
詳細解析：

選項 C 正確的原因： 由於航空公司破產，意味著該公司旗下的「所有（All）」航線都必須從系統中移除。因此，必須使用 deleteMany() 方法來匹配並刪除多個文檔。過濾條件傳入 { "airline.name": "Air Berlin" } JSON 物件以正確匹配目標。

其他選項錯誤的原因：

選項 A 錯誤：deleteOne() 預設只會刪除找到的第一筆文檔，執行後資料庫裡依然會殘留大量其他柏林航空的航線數據。

選項 B 錯誤：使用了不存在的方法 delete()，且傳入的參數是純字串，而非標準的 JSON 查詢文檔。

選項 D 錯誤：雖然使用了正確的方法 deleteMany()，但參數直接傳入了純字串 "Air Berlin"。MongoDB 的過濾參數必須是包含鍵值對的 JSON/BSON 物件 {}，否則會發生語法錯誤。


題目 3：[單選題]
某播客（Podcast）平台正在清理違規內容。管理員對 podcasts 集合執行了以下指令，試圖刪除所有分類（category）為 "crime" 的節目：

JavaScript


db.podcasts.deleteMany({ category: "crime" })
執行成功後，MongoDB Shell (mongosh) 傳回了以下的操作結果對象（Result Object）：

JSON


{
  "acknowledged": true,
  "deletedCount": 142
}
請從認證出題委員的角度，指出下列哪一項關於此傳回物件與資料庫狀態的解讀是完全正確的？

A. 系統成功刪除了 1 筆文檔，並且有 142 筆文檔被移入了解析快取。
B. 該操作獲得了伺服器確認（acknowledged 为 true），且資料庫中總共有 142 筆符合條件的文檔被徹底刪除。
C. 該返回值表明操作引發了異常，因為刪除多筆文檔時不應該返回 deletedCount。
D. deletedCount: 142 代表系統找到了 142 筆文檔，但由於缺乏強制參數，實際只刪除了其中的 0 筆。

正確答案： B
核心考點： 深入解析刪除操作傳回值對象的欄位語義
詳細解析：

選項 B 正確的原因： 根據官方筆記範例，deleteMany() 和 deleteOne() 在執行完畢後，都會返回一個固定的狀態文檔。其中 "acknowledged": true 代表操作已被 MongoDB 集群正確接收並寫入確認；"deletedCount" 則精確回報了這次操作中實際被刪除的文檔總數量。在本題中，代表 142 筆違規節目文檔已成功被移除。

其他選項錯誤的原因：

選項 A、D 錯誤：對 deletedCount 欄位的字面含義與底層機制產生了嚴重誤解。

選項 C 錯誤：這是標準且合法的成功傳回物件結構，並非異常。


題目 4：[複選題 (選 2 個)]
電商系統的購物車日誌集合 carts 中含有大量歷史數據。為了優化存儲空間，後端團隊準備在腳本中調用 MongoDB 的刪除 API。
請問關於 deleteOne() 和 deleteMany() 的共通性與規範，下列哪兩個敘述是正確的？（Select 2 answers）

A. 這兩個方法都強制要求傳入的第一個參數必須是過濾文檔（Filter document），用來指定匹配規則。
B. 這兩個方法都支援傳入第二個可選的參數，即選項對象（Options object）。
C. 如果傳入的過濾條件為空物件 {}（例如 db.carts.deleteMany({})），MongoDB 會因為安全防護機制拒絕執行並拋出錯誤。
D. 傳統關係型資料庫（如 SQL）使用 DROP TABLE 來清空數據，而 MongoDB 中唯一能清空集合數據且保留結構的方法是調用不帶參數的 db.carts.deleteOne()。

正確答案： A, B
核心考點： 刪除方法的參數結構與通用底層行為
詳細解析：

選項 A、B 正確的原因： 官方筆記第一段明確指出：「Both methods accept a filter document and an options object.」（這兩個方法都接受一個過濾文檔和一個選項物件）。其標準函數簽章皆為 db.collection.method(filter, options)。

其他選項錯誤的原因：

選項 C 錯誤：這是一個極具迷惑性的實務陷阱。在 MongoDB 中，傳入空物件 {} 作為過濾條件，代表「匹配集合中的所有文檔」。因此，db.carts.deleteMany({}) 將會直接清空整個集合內的所有數據，MongoDB 預設不會對此進行攔截或報錯，開發時需格外小心。

選項 D 錯誤：deleteOne() 永遠只會刪除一筆文檔，無法用來清空整個集合；若要清空集合且保留結構，應該使用 deleteMany({})。


題目 5：[單選題]
某高併發物流追蹤系統中，有一筆由於前端重試機制重複送出的錯誤異常日誌文檔，其 _id 為 ObjectId("6282c9862acb966e76bbf20a")。
開發團隊希望在 logs 集合中精確刪除這一筆特定的重複文檔。
請問下列哪一個查詢文檔最符合官方推薦的最佳實踐，能達到最高效且保證唯一的刪除效果？

A. db.logs.deleteMany({ _id: ObjectId("6282c9862acb966e76bbf20a") })
B. db.logs.deleteOne({ _id: "6282c9862acb966e76bbf20a" })
C. db.logs.deleteOne({ _id: ObjectId("6282c9862acb966e76bbf20a") })
D. db.logs.deleteOne({ log_status: "DUPLICATE" })

正確答案： C
核心考點： 使用唯一主鍵 _id 搭配正確類型的最佳實踐
詳細解析：

選項 C 正確的原因： 當目標是刪除「某一筆特定文檔」時，官方最佳實踐是使用 deleteOne() 搭配集合中具備唯一性約束（Unique Constraint）的 _id 欄位。同時，必須使用 ObjectId() 構造函數將字串封裝為正確的 BSON 類型。

其他選項錯誤的原因：

選項 A 錯誤：雖然 _id 具備唯一性，但既然商務目標明確已知為單一文檔，使用 deleteMany() 會引發潛在的底層架構檢視質疑，不符合針對單一文檔操作的語義。

選項 B 錯誤：這是高仿真類型錯誤陷阱！ 這裡將 _id 的值寫成了純字串 "6282c9862acb966e76bbf20a"。在 MongoDB 中，ObjectId 類型與 String 類型是完全不同的。如果直接傳入字串，查詢將無法匹配到任何文檔，導致刪除失敗（deletedCount 會為 0）。

選項 D 錯誤：log_status 不是唯一鍵欄位。使用它作為過濾條件，會導致系統誤刪第一筆掃描到的重複日誌，而無法保證處理的就是目標文檔。













