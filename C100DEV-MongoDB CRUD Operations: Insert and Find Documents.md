C100DEV-MongoDB CRUD Operations: Insert and Find Documents
---


* [Lesson 1 : Inserting Documents in a MongoDB Collection](#lesson-1--inserting-documents-in-a-mongodb-collection)
* [Lesson 2 : Finding Documents in a MongoDB Collection](#lesson-2--finding-documents-in-a-mongodb-collection)
* [Lesson 3 : Finding Documents by Using Comparison Operators](#lesson-3--finding-documents-by-using-comparison-operators)
* [Lesson 4 : Querying on Array Elements in MongoDB](#lesson-4--querying-on-array-elements-in-mongodb)
* [Lesson 5 : Updating Documents in a MongoDB Collection](#lesson-5--updating-documents-in-a-mongodb-collection)
* [Lesson 6 : Deleting Documents from a MongoDB Collection](#lesson-6--deleting-documents-from-a-mongodb-collection)

[Lesson 2: Updating MongoDB Documents by Using updateOne()](#lesson-1-replacing-a-document-in-mongodb)   
Lesson 2: Updating MongoDB Documents by Using updateOne()  
Lesson 3: Updating MongoDB Documents by Using findAndModify()  
Lesson 4: Updating MongoDB Documents by Using updateMany()  
Lesson 5: Deleting Documents in MongoDB  


https://learn.mongodb.com/learn/course/mongodb-crud-operations-insert-and-find-documents/lesson-1-inserting-documents-in-a-mongodb-collection/learn   

https://learn.mongodb.com/learn/course/mongodb-crud-operations-replace-and-delete-documents/lesson-5-deleting-documents-in-mongodb/learn?page=1   

# 考試註記

1. 任何find的物件順序皆由系統控制, sort()、skip() 與 limit() 確保分頁與排序邏輯的正確性 [測試1-12]   
   ```sql
   db.orders.find({ status: 'active' }).skip(10).limit(5).sort({ createdAt: -1 })
   ```   
2. 








# Lesson 1 : Inserting Documents in a MongoDB Collection

In this unit, you will be introduced to CRUD operations in MongoDB by inserting and finding documents. Inserting and finding documents will help you discover the ease and usability of MongoDB. You'll also build your own queries that use comparison and logical operators. Using operators will make your queries more precise and, in turn, make your application easier to develop. Finally, you'll learn how to query elements in an array. Arrays are a crucial data type that you will encounter frequently, so it's important that you have a solid understanding of how to work with them.  

* iesertOne(): 插入單個文檔  
* insertMany(): 插入多個文檔
  * 不存在, 就會自動創建

```sql
db.<collection>.iesertOne() 如果<collection>
db.<collection>.iesertMany([<doc1>,<doc2>])
```

## Insert a Single Document  
  
Use insertOne() to insert a document into a collection. Within the parentheses of insertOne(), include an object that contains the document data. Here's an example:

```sql
db.grades.insertOne({
  student_id: 654321,
  products: [
    {
      type: "exam",
      score: 90,
    },
    {
      type: "homework",
      score: 59,
    }
  ],
  class_id: 550,
})
```

## Insert Multiple Documents

Use insertMany() to insert multiple documents at once. Within insertMany(), include the documents within an array. Each document should be separated by a comma. Here's an example:

```sql
db.grades.insertMany([
  {
    student_id: 546789,
    products: [
      {
        type: "quiz",
        score: 50,
      },
      {
        type: "homework",
        score: 70,
      }
    ],
    class_id: 551,
  },
  {
    student_id: 777777,
    products: [
      {
        type: "exam",
        score: 83,
      },
      {
        type: "quiz",
        score: 59,
      }
    ],
    class_id: 550,
  }
])
```

[題目]   
  
What methods are available in MongoDB for inserting a single document? (Select one.)  
  
[選項]  
A. .insertOne()   
B. .inserting()   
C. .InsertDocument()   
D. .insertMany()  
  
[正確答案]：  
A  



[題目]   
  
What methods are available in MongoDB for inserting multiple documents? (Select one.) 
  
[選項]  
A. .InsertDocument()  
B. .inserting()  
C. .insertOne()  
D. .insertMany()  
  
[正確答案]：  
D


## ordered
   
使用 insertMany 時, 帶入參數 ordered, is skipped due to a duplicate key error, and the operation reports a partial success.   

```sql   
db.orders.insertMany([
  { _id: 1, item: "pen" },
  { _id: 2, item: "notebook" },
  { _id: 2, item: "ruler" },    // duplicate — will fail
  { _id: 3, item: "eraser" }
], { ordered: false })
```





---

# Lesson 2 : Finding Documents in a MongoDB Collection

```sql
db.<collection>.find()

{ field : <value> }

{ field : { $eq : <value> } }
```

```sql
db.<collection>.find({
   <field> : { $in: 
     [<value>, <value>,...]
})
```

* Finding Documents in a MongoDB Collection  
Review the following code, which demonstrates how to query documents in MongoDB.

* Find a Document with Equality  
When given equality with an _id field, the find() command will return the specified document that matches the _id. Here's an example:

```
db.zips.find({ _id: ObjectId("5c8eccc1caa187d17ca6ed16") })
```

* Find a Document by Using the $in Operator  
Use the $in operator to select documents where the value of a field equals any value in the specified array. Here's an example:

```sql
db.zips.find({ city: { $in: ["PHOENIX", "CHICAGO"] } })
```

### Quiz 


What methods are available in MongoDB for finding documents? (Select one.)

a.
.find()
b.
.query()
c.
.finding()
d.
.search()

==> A

A: Correct! The find() method is a valid method that's included in the MongoDB Shell to find documents.

BCD:  Incorrect. This is not a valid method included in the MongoDB Shell. What method can you use to find documents?

Quiz 


You are searching for data on a small area in downtown Chicago with the following zip codes:
```
“60601” 
“60602”  
“60603”  
“60604” 
“60605” 
“60606” 
```
Which of the following query documents should you use to ensure that only the documents with the specified zip codes are returned? (Select one.)

a.
{ zip: { $nin : [ "60601", "60602", "60603", "60604", "60605", "60606"] } }

b.
{ zip: { $in : [ "60601", "60602", "60603", "60604", "60605", "60606"] } }

c.
{ zip: { $eq : "60601", "60602", "60603", "60604", "60605", "60606" } }

d.
{ zip: "60601", "60602", "60603", "60604", "60605", "60606" }



==> B

A: Incorrect. The $nin operator returns documents that do not contain the values specified in the array. Including this query would not return the specified Chicago zip codes.
  
B: Correct! The $in operator returns documents that contain the values specified in the array. This query will return the specified Chicago zip codes.

C: Incorrect. This syntax is inaccurate because the $eq operator matches documents that contain only one specified value.

D: Incorrect. This syntax is inaccurate because the implicit equality operator matches documents that contain only one specified value.


筆記:

$eq 的右邊： 只能是一個具體的值（字串、數字、或一個精確的陣列/物件），絕對不能出現逗號分隔的多個純量。

$in 的右邊： 必須是一個中括號 [] 陣列，裡面放所有你允許匹配的複數條件。

$or 的右邊： 必須是一個中括號 [] 陣列，且陣列裡面包的必須是數個獨立且完整的 { 欄位: 條件 } 物件。




---


# Lesson 3: Finding Documents by Using Comparison Operators

$gt (greater than) - 大于
$lt (less than) - 小于
$lte (less than or equal to) - 小于或等于
$gte (greater than or equal to) - 大于或等于

<field> : { <operator> : <value> } 

Finding Documents by Using Comparison Operators
Review the following comparison operators: $gt, $lt, $lte, and $gte.

* $gt  
Use the $gt operator to match documents with a field greater than the given value. For example:  

db.sales.find({ "items.price": { $gt: 50}})  

* $lt  
Use the $lt operator to match documents with a field less than the given value. For example:  
  
db.sales.find({ "items.price": { $lt: 50}})  

* $lte  
Use the $lte operator to match documents with a field less than or equal to the given value. For example:  
  
db.sales.find({ "customer.age": { $lte: 65}})  

* $gte  
Use the $gte operator to match documents with a field greater than or equal to the given value. For example:  
  
db.sales.find({ "customer.age": { $gte: 65}})  
 

QA

Your company is conducting research on the customer experience and is focused on identifying unsatisfied customers. You need to find all customers with a satisfaction rating of 1 or 2.

Which of the following query documents would return all customers with a satisfaction rating of 1 or 2? (Select one.)

a.
{ "customer.satisfaction" : { $gt : 1}}
b.
{ customer.satisfaction : { $lte : 2}}
c.
{ "customer.satisfaction" : { $lt : 2}}
d.
{ "customer.satisfaction" : { $lte : 2}}


==> D


Your company wants to offer a special discount for customers who are 65 or older. Your task is to find the records for these customers. Which of the following queries would return documents for all customers 65 or older? (Select all that apply.)

a.
{ customer.age : { $gte : 65 }}
b.
{ "customer.age" : { $gt : 64 }}
c.
{ "customer.age" : { $gte : 65 }}
d.
{ "customer.age" : { $lte : 65 }}


=>  BC

---

# Lesson 4 : Querying on Array Elements in MongoDB

* Querying on Array Elements in MongoDB *

Review the following code, which demonstrates how to query array elements in MongoDB.

* Find Documents with an Array That Contains a Specified Value *
 
In the following example, "InvestmentFund" is not enclosed in square brackets, so MongoDB returns all documents within the products array that contain the specified value.

db.accounts.find({ products: "InvestmentFund"})

* Find a Document by Using the $elemMatch Operator *
  
Use the $elemMatch operator to find all documents that contain the specified subdocument. For example:
  
```sql
db.sales.find({
  items: {
    $elemMatch: { name: "laptop", price: { $gt: 800 }, quantity: { $gte: 1 } },
  },
})
```

QA

Which of the following operators can be used to find a subdocument that matches specific criteria in an array?

a.
&element
b.
$elemMatch
c.
$subMatch
d.
$docMatch


==> B

ACD: Incorrect. This is not a valid operator included in the MongoDB Shell. What operator can you use to find a subdocument that matches specific criteria in an array?



What will the following query return? (Select one.)

db.books.find({ genre: "Historical" })
a.
All documents where the genre field is equal to either the scalar value of “Historical” or an array that contains “Historical”.
b.
All documents that contain the string “Historical” across any field.
c.
All documents where the genre field does not contain the value “Historical”.

  
==> A

A:Correct! This query will return all documents that contain “Historical” as a scalar value and as an array element within the genre field.


B:Incorrect. This query will not return all documents that contain the string “Historical” in any field. It will return all documents that contain “Historical” as a scalar value and as an array element within the genre field.


C: Incorrect. This query will not filter out all documents that contain the string “Historical” in any field. It will return all documents that contain “Historical” as a scalar value and as an array element within the genre field.

---

# Lesson 5 : Finding Documents by Using Logical Operators



* Finding Documents by Using Logical Operators* 
Review the following logical operators: implicit $and, $or, and $and.

*  Find a Document by Using Implicit $and**  
Use implicit $and to select documents that match multiple expressions. For example:

db.routes.find({ "airline.name": "Southwest Airlines", stops: { $gte: 1 } })
   
*  Find a Document by Using the $or Operator** 
Use the $or operator to select documents that match at least one of the included expressions. For example:

```sql
db.routes.find({
  $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }],
})
```
** Find a Document by Using the $and Operator** 
Use the $and operator to use multiple $or expressions in your query.

```sql
db.routes.find({
  $and: [
    { $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] },
    { $or: [{ "airline.name": "American Airlines" }, { airplane: 320 }] },
  ]
})
```

$and

```sql
db.<collection>.find({
  $and: [
    {<expression>},
    {<expression>},
    ...
  ]
})
```

$or

```sql
db.<collection>.find({
  $or: [
    {<expression 1>},
    {<expression 2>},
    ...
  ]
})
```

db.routes.find({ $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] })

db.routes.find({
  $and: [{ "airline": "Southwest Airlines" }, { "stops": { $gte: 1 } }],
})

4. 複合邏輯查詢範例（$and 巢狀包包含 $or）

```sql
db.routes.find({
  $and: [
    { $or: [
      { dst_airport: "SEA" },
      { src_airport: "SEA" }
    ]},
    { $or: [
      { airline: "American Airlines" },
      { airplane: 320 }
    ]},
  ]
})
```

### Quti
You want to know which mobile food trucks in your neighborhood, Astoria, are the best spots to eat. Using the inspections collection, you’re making a map of all mobile food trucks nearby that have passed inspection. 

What should you include in your query document to ensure that you find all the mobile food vendors in Astoria that passed inspection? (Select one.)

a.
{ "sector": "Mobile Food Vendor - 881" } ,{ "address.city": "ASTORIA"} , {"result": "Pass"}

b.
$and: {“Mobile Food Vendor - 881”, “ASTORIA”, “Pass}

c.
{ "sector": "Mobile Food Vendor - 881" , "address.city": "ASTORIA" , "result": "Pass"}

d.
&and: {“Mobile Food Vendor - 881”, “ASTORIA”, “Pass}


==> C

A:Incorrect. This answer option contains three separate query documents and is invalid. In order to use implicit AND specify the values within one query document.


B:Incorrect. This syntax is invalid. When using the $and operator, you must include each expression within an array.


C:Correct! This query document uses implicit AND, so it will return all mobile food vendors in Astoria that have passed inspection.


D: Incorrect. This syntax is invalid. When using the $and operator, you must include each expression within an array.



What will the following query return? (Select one.)
```sql
db.routes.find({
  $and: [
    { $or: [{ dst_airport: "IST" }, { src_airport: "IST" }] },
    { $or: [{ stops: 0 }, { airline.name: "Turkish Airlines"}] },
  ]
})
```
a.
All flights departing from or landing at the Istanbul airport (IST) that are nonstop or operated by Turkish Airlines.
b.
All flights that are either departing from the Istanbul airport (IST) or Turkish Airlines flights that are nonstop.
c.
All flights that are either landing at the Istanbul airport (IST) or operated by Turkish Airlines.
d.
All Turkish Airlines flights that are nonstop.


=> A

A: Correct! This query will return all flights that either depart from or land at the Istanbul airport that are nonstop or operated by Turkish Airlines.


B:Incorrect. To return all flights that are either departing from the Istanbul airport or nonstop Turkish Airlines flights, you would write the following query:

```sql
db.routes.find({
  $or: [{ src_airport: "IST" }, { airline.name: "Turkish Airlines", stops: 0 }],
})
```
C:Incorrect. To return only flights that are landing at the Istanbul airport or those operated by Turkish Airlines, you would write the following query:
```sql
db.routes.find({
  $or: [{ dst_airport: "IST" }, { airline.name: "Turkish Airlines" }],
})
```
D:Incorrect. To return all Turkish Airlines flights that are nonstop, you would write the following query:

```sql
db.routes.find({ "airline.name": "Turkish Airlines", stops: 0 })

```



* [Macro] $project to element











# Conclusion: MongoDB CRUD Operations - Insert and Find Documents

In this unit, you learned how to insert and find documents in a MongoDB collection. You built queries by using the following comparison and logical operators, as well as array querying techniques.

---

## Key Concepts Covered

### 1. Comparison Operators
* **`$gt`** : Greater than (大於)
* **`$lt`** : Less than (小於)
* **`$lte`** : Less than or equal to (小於或等於)
* **`$gte`** : Greater than or equal to (大於或等於)

### 2. Logical Operators
* **`$and`** : Joins query clauses with a logical `AND` (邏輯與)
* **`$or`** : Joins query clauses with a logical `OR` (邏輯或)

### 3. Array Querying
* Querying elements inside an array.
* Utilizing the **`$elemMatch`** operator to match documents containing an array field with at least one element that matches all specified query criteria.

---

## Resources
Use the following resources to learn more about inserting and finding documents in MongoDB:

### Lesson 01: Inserting Documents in a MongoDB Collection
* [MongoDB Docs: insertOne()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertOne/)
* [MongoDB Docs: insertMany()](https://www.mongodb.com/docs/manual/reference/method/db.collection.insertMany/)

### Lesson 02: Finding Documents in a MongoDB Collection
* [MongoDB Docs: find()](https://www.mongodb.com/docs/manual/reference/method/db.collection.find/)
* [MongoDB Docs: $in](https://www.mongodb.com/docs/manual/reference/operator/query/in/)

### Lesson 03: Finding Documents by Using Comparison Operators
* [MongoDB Docs: Comparison Operators](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/)

### Lesson 04: Querying on Array Elements in MongoDB
* [MongoDB Docs: $elemMatch](https://www.mongodb.com/docs/manual/reference/operator/query/elemMatch/)
* [MongoDB Docs: Querying Arrays](https://www.mongodb.com/docs/manual/tutorial/query-arrays/)

### Lesson 05: Finding Documents by Using Logical Operators
* [MongoDB Docs: Logical Operators](https://www.mongodb.com/docs/manual/reference/operator/query-logical/)

---

## Associate Certification Course Progress

Progress Update: By completing this unit, you've finished 30% of the CRUD content necessary for the Associate Developer Certification exam.

### Next Steps
If you're interested in continuing your certification preparation, your next step is to review the following units:

* Unit 01: Getting Started with MongoDB Atlas
* Unit 02: The MongoDB Document Model
* Unit 03: Connecting to a MongoDB Database
* Unit 05: MongoDB CRUD: Replace and Delete
* Unit 06: MongoDB CRUD: Reading Query Results
* Unit 07: MongoDB Aggregation
* Unit 08: MongoDB Indexes
* Unit 09: MongoDB Atlas Search
* Unit 10: MongoDB Data Modeling
* Unit 11: MongoDB Transactions


# Associate Developer Java Practice Quest

6. An `inventory` collection consists of 200 documents.

What method should be used to get all documents from a cursor using mongosh?

(Choose 1)

Correct Answer

a.
db.inventory.findOne()
b.
db.inventory.find().toArray();
c.
db.inventory.find();
d.
db.inventory.findMany().toArray()









# 小題目




C100DEV 模擬考題


* Lesson 1 : Inserting Documents in a MongoDB Collection
* Lesson 2 : Finding Documents in a MongoDB Collection
* Lesson 3 : Finding Documents by Using Comparison Operators



題目 1：[單選題]
某電商平台正在開發會員註冊模組。當新使用者完成註冊時，後端應用程式需要將一個包含用戶基本資訊的文檔（Document）寫入 users 集合（Collection）中。假設目前資料庫中尚未建立 users 集合。
開發人員寫了以下程式碼：

JavaScript


db.users.insertOne({
  username: "alex99",
  email: "alex@example.com",
  status: "active"
})
請問關於這段程式碼的執行結果，下列敘述何者正確？

A. 系統會拋出錯誤，因為 users 集合不存在，必須先執行 db.createCollection("users")。
B. 程式會成功執行，MongoDB 會自動創建 users 集合，並將該文檔成功插入。
C. 程式會成功執行，但因為 users 集合不存在，該文檔會被寫入系統預設的 default 集合。
D. 系統會拋出錯誤，因為在 insertOne() 中傳入的文檔必須包裹在 JavaScript 陣列 [] 當中。

正確答案： B
核心考點： insertOne() 行為與集合自動創建機制
詳細解析：

選項 B 正確的原因： 根據 MongoDB 官方特性，當執行 insertOne() 或 insertMany() 時，如果指定的集合（Collection）尚不存在，MongoDB 會自動隱式創建該集合，並順利寫入資料，不需要手動預先建立。

其他選項錯誤的原因：

選項 A 錯誤：MongoDB 是動態 Schema 且具備自動創建集合的特性，不需要強制先呼叫 createCollection。

選項 C 錯誤：MongoDB 不會將資料重導向到任何名為 default 的集合。

選項 D 錯誤：insertOne() 的參數要求是單一個 JSON/BSON 物件 {}，只有 insertMany() 才需要使用陣列 [] 包裹。

題目 2：[複選題 (選 2 個)]
物流系統需要初始化一整批新進的倉庫貨架資料，預計同時寫入 3 個貨架的文檔至 warehouses 集合。開發人員希望使用 insertMany() 來提升寫入效率。
請問下列哪兩個選項的語法是完全正確且符合 MongoDB 規範的？（Select 2 answers）

A.

JavaScript


db.warehouses.insertMany(
  { shelf_id: "A1", capacity: 500 },
  { shelf_id: "A2", capacity: 450 },
  { shelf_id: "A3", capacity: 600 }
)
B.

JavaScript


db.warehouses.insertMany([
  { shelf_id: "A1", capacity: 500 },
  { shelf_id: "A2", capacity: 450 },
  { shelf_id: "A3", capacity: 600 }
])
C.

JavaScript


db.warehouses.insertMany([
  { shelf_id: "B1", capacity: 300 }
])
D.

JavaScript


db.warehouses.insertMany(
  [ shelf_id: "C1", capacity: 100 ],
  [ shelf_id: "C2", capacity: 200 ]
)
正確答案： B, C
核心考點： insertMany() 參數格式與陣列規範
詳細解析：

選項 B、C 正確的原因： insertMany() 的核心語法規範是：傳入的參數必須是一個 JavaScript 陣列 []，陣列內部的每個元素則是由花括號 {} 定義的文檔。無論陣列中包含多個文檔（選項 B）還是只有一個文檔（選項 C），只要最外層是用陣列 [] 包裹，即符合語法規則。

其他選項錯誤的原因：

選項 A 錯誤：這是一個極高頻的 C100DEV 考點陷阱。開發人員直接將多個物件作為獨立參數傳入，漏掉了最外層的陣列方括號 []，這會導致驅動程式或 Shell 報錯。

選項 D 錯誤：文檔本身被錯誤地用方括號 [] 包裹（如 [ shelf_id: "C1" ]），這不符合 JSON/BSON 的鍵值對物件表達式（應使用 {}），且最外層結構也混亂。

題目 3：[單選題]
你正在維護一個氣象觀測系統的資料庫，weather_stations 集合中儲存了全美各觀測站的資訊。主管要求你找出位於 紐約（"NEW YORK"）、洛杉磯（"LOS ANGELES"） 或 邁阿密（"MIAMI"） 這三個城市之一的所有觀測站文檔。
為了確保查詢文檔（Query Document）的準確性，下列哪一個查詢語法能夠正確執行並返回預期的結果？

A.

JavaScript


db.weather_stations.find({ city: "NEW YORK", "LOS ANGELES", "MIAMI" })
B.

JavaScript


db.weather_stations.find({ city: { $eq: ["NEW YORK", "LOS ANGELES", "MIAMI"] } })
C.

JavaScript


db.weather_stations.find({ city: { $in: ["NEW YORK", "LOS ANGELES", "MIAMI"] } })
D.

JavaScript


db.weather_stations.find({ city: { $nin: ["NEW YORK", "LOS ANGELES", "MIAMI"] } })
正確答案： C
核心考點： 使用 $in 運算子進行多值比對
詳細解析：

選項 C 正確的原因： 當我們需要篩選出某個欄位的值符合給定清單中的「任意一個」時，必須使用 $in 運算子，且清單必須以陣列 [...] 形式提供。

其他選項錯誤的原因：

選項 A 錯誤：這是不合法的 JSON 語法。隱式等值（Implicit Equality）在同一個欄位上只能指定單一值（例如 { city: "NEW YORK" }），不能直接並列多個字串。

選項 B 錯誤：$eq 後面若接陣列，代表精確比對整個陣列。此查詢會去尋找 city 欄位本身就是一個包含這三個元素之陣列的文檔（例如 city: ["NEW YORK", "LOS ANGELES", "MIAMI"]），而不是尋找單一城市符合其中之一的文檔。

選項 D 錯誤：$nin 的邏輯正好相反（Not In），它會返回不在這三個城市的所有其他觀測站，不符合題目需求。

題目 4：[單選題]
在一個學生評分系統（Grades System）中，每個學生的文檔結構如下所示（內含一個 products 陣列，儲存各科目的題型與分數）：

JSON


{
  "_id": ObjectId("60c72b2f9b1d8b2bad000001"),
  "student_id": 999123,
  "class_id": 550,
  "products": [
    { "type": "exam", "score": 90 },
    { "type": "homework", "score": 85 }
  ]
}
現在，你需要針對 _id 為 ObjectId("60c72b2f9b1d8b2bad000001") 的特定學生進行單一文檔的精確查詢。在 MongoDB Shell 中，下列哪一個指令是官方最推薦且能準確返回該文檔的？

A. db.grades.query({ _id: ObjectId("60c72b2f9b1d8b2bad000001") })
B. db.grades.find({ _id: "60c72b2f9b1d8b2bad000001" })
C. db.grades.search({ _id: ObjectId("60c72b2f9b1d8b2bad000001") })
D. db.grades.find({ _id: ObjectId("60c72b2f9b1d8b2bad000001") })

正確答案： D
核心考點： find() 方法使用與 BSON 類型（ObjectId）精確比對
詳細解析：

選項 D 正確的原因： MongoDB 使用 find() 方法來查詢文檔。同時，由於資料庫中的 _id 是自動或手動生成的 ObjectId 類型（屬於 BSON 類型），在查詢時必須用 ObjectId("...") 包裹字串，才能達成類型的完全匹配。

其他選項錯誤的原因：

選項 A、C 錯誤：MongoDB Shell 中並沒有 .query() 或 .search() 這兩種標準的集合查詢方法，這是混淆傳統關係型資料庫或其他搜尋引擎的干擾選項。

選項 B 錯誤：雖然使用了 find() 語法，但它將 _id 作為普通「字串（String）」進行比對。在 MongoDB 中，"60c72b2...01" (String) 與 ObjectId("60c72b2...01") (ObjectId) 是完全不同的資料類型，此查詢將無法正確匹配並返回該文檔（結果為空）。

題目 5：[複選題 (選 2 個)]
你正在設計一個圖書管理系統的查詢功能，集合名為 books。每本圖書文檔都有一個 tags 欄位，該欄位是一個儲存標籤字串的陣列（例如：tags: ["sci-fi", "novel", "bestseller"]）。
現有使用者想要尋找帶有標籤 "sci-fi" 或者 "fantasy" 的圖書。
請問下列哪兩個查詢語法可以正確實現這個業務需求？（Select 2 answers）

A. db.books.find({ tags: { $in: ["sci-fi", "fantasy"] } })
B. db.books.find({ tags: ["sci-fi", "fantasy"] })
C. db.books.find({ $or: [ { tags: "sci-fi" }, { tags: "fantasy" } ] })
D. db.books.find({ tags: { $eq: "sci-fi", "fantasy" } })

正確答案： A, C
核心考點： 陣列欄位的多值查詢（陣列的多欄位匹配與邏輯運算）
詳細解析：

選項 A 正確的原因： 當針對一個「陣列欄位」使用 $in 運算子時，MongoDB 會檢查該陣列中是否包含清單中的任意一個元素。若書本的 tags 陣列裡有 "sci-fi" 或有 "fantasy"，就會被成功篩選出來。

選項 C 正確的原因： 這是使用邏輯運算子 $or 的等價表達方式。{ tags: "sci-fi" } 會匹配陣列中包含 "sci-fi" 的文檔，配合 $or 陣列，只要滿足其中一個條件即可，完全符合題目需求。

其他選項錯誤的原因：

選項 B 錯誤：這是一個非常經典的 C100DEV 陣列陷阱。當你直接使用隱式等值對陣列進行查詢（{ tags: ["sci-fi", "fantasy"] }），MongoDB 會執行精確陣列匹配。這意味著它只會找出 tags 陣列「剛好只有這兩個元素，且順序必須一模一樣」的文檔。如果某本書的標籤是 ["sci-fi", "novel", "fantasy"]，此語法將無法查到它。

選項 D 錯誤：語法錯誤。$eq 運算子後面不能直接帶有多個用逗號分隔的字串，這是不合法的 JSON 結構。







C100DEV 模擬考題：比較運算子篇
題目 1：[單選題]
某電商系統的用戶權限審查模組需要篩選出 user 集合中，所有符合「法定成年年齡（大於或等於 18 歲）」的活躍用戶。
已知 user 集合現有的資料結構如下：

JSON


{ "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"), "name": "alex001", "mail": "alex001.example.com", "status": "active", "age": "34" }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34e"), "name": "alex002", "mail": "alex002@example.com", "status": "active", "age": "4" }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"), "name": "alex003", "mail": "alex003@example.com", "status": "active", "age": "18" }
若開發人員執行以下查詢，請問該查詢實際返回的文檔數量是多少？

JavaScript


db.user.find({ age: { $gte: 18 } })
A. 0 個文檔
B. 1 個文檔（僅 alex003）
C. 2 個文檔（alex001 與 alex003）
D. 3 個文檔

正確答案： A
核心考點： 比較運算子的資料類型匹配（Data Type Matching）
詳細解析：

選項 A 正確的原因： 這是 C100DEV 考試中最高頻的 BSON 類型陷阱！在提供的 user 集合中，age 欄位的值是用雙引號包裹的「字串（String）」（例如 "34"、"18"）。然而，查詢語句中使用的條件 { $gte: 18 } 是一個「數字（Number/Integer）」。MongoDB 在執行比較運算子（如 $gt, $gte, $lt, $lte）時，預設不會進行隱式的類型轉換。因為字串與數字屬於不同的 BSON 類型，故無法進行大小比較，此查詢將找不到任何匹配的文檔，返回數量為 0。

其他選項錯誤的原因： * 選項 B、C、D 均假設 MongoDB 會自動將字串 "34" 或 "18" 轉換為數字進行比較，這不符合 MongoDB 的運作原理。如果要正確查出，資料庫內的值必須改為數值型態（如 age: 34），或者查詢條件必須針對字串排序規則設計。

題目 2：[複選題 (選 2 個)]
假設經過資料清洗後，user 集合中的 age 欄位已全數成功轉換為「數值（Number）」型態，資料如下：

JSON


{ "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"), "name": "alex001", "mail": "alex001.example.com", "status": "active", "age": 34 }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34e"), "name": "alex002", "mail": "alex002@example.com", "status": "active", "age": 4 }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"), "name": "alex003", "mail": "alex003@example.com", "status": "active", "age": 18 }
現在系統要發放「未成年專屬優惠券」，需要篩選出所有 年齡小於或等於 17 歲 的用戶。請問下列哪兩個查詢文檔可以精確達成這個業務需求？（Select 2 answers）

A. { age: { $lt: 18 } }
B. { age: { $lte: 17 } }
C. { age: { $gt: 0, $lt: 17 } }
D. { age: { $gte: 4, $lte: 18 } }

正確答案： A, B
核心考點： 比較運算子的邊界邏輯運用
詳細解析：

選項 A、B 正確的原因： 在整數年齡的業務場景中，「小於或等於 17 歲（$lte: 17）」與「小於 18 歲（$lt: 18）」在邏輯上是完全等價的。對照目前的資料，這兩個查詢都能夠精確且正確地篩選出 age: 4 的用戶（alex002）。

其他選項錯誤的原因：

選項 C 錯誤：$lt: 17 代表小於 17 歲，這會漏掉剛好 17 歲的邊界用戶（雖然目前資料沒有 17 歲，但此邏輯不符合題目要求的「小於或等於 17 歲」）。

選項 D 錯誤：$lte: 18 會包含 18 歲的用戶，這會將已經成年的 alex003（age: 18）錯誤地納入發放名單中。

題目 3：[單選題]
某資深開發人員正在重構內部的日誌與用戶活動分析系統。已知 user 集合中包含了一個內嵌文檔（Embedded Document）結構，用來記錄用戶的詳細登入統計資訊。某一筆文檔格式如下：

JSON


{
  "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"),
  "name": "alex001",
  "stats": {
    "login_count": 45,
    "failed_attempts": 2
  }
}
若要找出登入次數（login_count）大於 40 次的所有用戶，下列哪一個 MongoDB 查詢語法是完全正確且能成功執行的？

A. db.user.find({ stats.login_count: { $gt: 40 } })
B. db.user.find({ "stats.login_count": { $gt: 40 } })
C. db.user.find({ "stats.login_count": { $gte: 40 } })
D. db.user.find({ stats: { login_count: { $gt: 40 } } })

正確答案： B
核心考點： 內嵌文檔與 Dot Notation（點號表示法）的字串引號規範
詳細解析：

選項 B 正確的原因： 當我們需要查詢內嵌文檔（Embedded Document）或陣列中的欄位時，必須使用點號表示法（Dot Notation），例如 stats.login_count。根據 MongoDB 官方規範，凡是使用點號（.）連接的欄位路徑，在查詢文檔中必須強制用雙引號或單引號包裹成字串。配合大於運算子 $gt: 40，此語法完全正確。

其他選項錯誤的原因：

選項 A 錯誤：這是 C100DEV 極其常見的語法錯誤陷阱。stats.login_count 中間含有點號，若不用引號包裹，JavaScript/JSON 解析器會直接拋出語法錯誤（SyntaxError）。

選項 C 錯誤：雖然加了引號，但使用的是 $gte（大於或等於），這會把登入次數剛好等於 40 的用戶也查出來，不符合題目要求的「大於 40 次」。

選項 D 錯誤：這會變成精確匹配 stats 物件，意指 stats 欄位的值必須「剛好只包含一個叫 login_count 且值為 { $gt: 40 } 的物件」，這不符合 MongoDB 的運算子巢狀語法，會導致查詢不到結果或語法錯誤。

題目 4：[單選題]
在一個高併發的電商後台，營運團隊希望透過特定條件篩選 user 集合中的用戶來做問卷調查。查詢需求為：「篩選出年齡大於 20 歲，且小於 40 歲的活跃用戶」。
假設 age 欄位皆為數值型態。下列哪一個查詢文檔（Query Document）能夠最直觀、正確地同時套用這兩個比較條件？

A. { age: { $gt: 20 }, age: { $lt: 40 } }
B. { age: { $gt: 20 }, $lt: 40 }
C. { age: { $gt: 20, $lt: 40 } }
D. { $gt: 20, $lt: 40, age: true }

正確答案： C
核心考點： 單一欄位組合多個比較運算子的正確 JSON 宣告
詳細解析：

選項 C 正確的原因： 當我們要對「同一個欄位」（age）同時施加多個比較運算子條件（大於 20 且小於 40）時，正確的做法是將這些運算子合併寫在同一個內層物件中，用逗號隔開，即 { age: { $gt: 20, $lt: 40 } }。MongoDB 會將其視為邏輯與（AND）的關係。

其他選項錯誤的原因：

選項 A 錯誤：這觸犯了 JSON 鍵值重複（Duplicate Keys）的陷阱。在同一個 JSON 物件層級中出現了兩個相同的鍵（age），在多數環境解析時，後面的 { $lt: 40 } 會直接覆蓋掉前面的 { $gt: 20 }，導致最後只有小於 40 歲的條件生效。

選項 B 錯誤：運算子 $lt 被抽到了外層，這是不合法的 MQL 結構，MongoDB 會無法辨識並拋出錯誤。

選項 D 錯誤：結構完全混亂，不符合 <field> : { <operator> : <value> } 的官方標準語法。

題目 5：[複選題 (選 3 個)]
公司的資料分析師需要從 user 集合中，排除特定的邊界資料。任務目標是：「找出所有年齡大於或等於 18 歲的用戶」。
假設 age 欄位皆為數值型態，且現有資料庫資料如下：

JSON


{ "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"), "name": "alex001", "age": 34 }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34e"), "name": "alex002", "age": 4 }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"), "name": "alex003", "age": 18 }
請問下列哪三個查詢指令執行後，都能夠成功返回 alex001 (34歲) 與 alex003 (18歲) 這兩筆文檔？（Select 3 answers）

A. db.user.find({ age: { $gte: 18 } })
B. db.user.find({ age: { $gt: 17 } })
C. db.user.find({ age: { $lte: 18 } })
D. db.user.find({ age: { $not: { $lt: 18 } } })

正確答案： A, B, D
核心考點： 比較運算子的邏輯逆向思考與等價語法
詳細解析：

選項 A 正確的原因： 這是最直覺的標準解法，使用 $gte: 18 直接匹配大於或等於 18 歲的資料，完美符合題目要求。

選項 B 正確的原因： 由於資料庫中的年齡皆為整數，在邏輯上，「大於或等於 18（$gte: 18）」與「大於 17（$gt: 17）」所涵蓋的整數集合是完全一致的。對照現有資料，它會正確返回 18 歲與 34 歲的文檔。

選項 D 正確的原因： 這是一個進階的邏輯取反組合。{ $lt: 18 } 代表「小於 18 歲（意即 17 歲以下）」，加上 $not 取反之後，意思就變成了「不是小於 18 歲」，亦即「大於或等於 18 歲」。因此也能成功返回 18 歲與 34 歲的文檔。

其他選項錯誤的原因：

選項 C 錯誤：$lte: 18 代表「小於或等於 18 歲」，這會把 4 歲的 alex002 查出來，並把 34 歲的 alex001 排除在外，與題目需求完全相反。



* Lesson 4 : Querying on Array Elements in MongoDB




這是一套為您量身打造的 MongoDB C100DEV 官方認證高品質模擬考題。本套試題嚴格依據您提供的官方筆記（陣列與內嵌文檔查詢、$elemMatch 核心概念）以及 user 集合的基礎資料進行設計，融合了真實開發情境與高仿真陷阱，幫助您精準掌握考試核心！

題目 1：[單選題]
某遊戲社群平台正在開發用戶推薦模組，需要篩選出 user 集合中對 "gaming"（遊戲）感興趣的活躍用戶。
已知目前的 user 集合資料如下：

JSON


{
  "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"),
  "name": "alex001",
  "status": { "login_count": 45, "failed_attempts": 2 },
  "age": "34"
}
{
  "_id": ObjectId("6a2f5eb7bc8ac27e8830c34e"),
  "name": "alex002",
  "status": { "login_count": 25, "failed_attempts": 3, "interests": "gaming" },
  "age": "4"
}
{
  "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"),
  "name": "alex003",
  "status": { "login_count": 30, "failed_attempts": 1, "interests": ["gaming", "music"] },
  "age": "18"
}
開發人員在 MongoDB Shell 中執行了以下查詢語法：

JavaScript


db.user.find({ "status.interests": "gaming" })
請問關於此查詢的執行結果，下列敘述何者正確？

A. 僅會返回 alex002 的文檔，因為 alex003 的 interests 是一個陣列，無法與單一字串進行等值匹配。
B. 僅會返回 alex003 的文檔，因為該隱式語法是 MongoDB 專門用來檢索陣列元素的，無法匹配標量字串。
C. 系統會拋出錯誤，因為 status.interests 包含點號（Dot Notation），但在外層欄位名稱上漏掉了雙引號。
D. 會同時成功返回 alex002 與 alex003 的文檔。

正確答案： D
核心考點： 巢狀結構下的標量（Scalar）與陣列欄位隱式匹配
詳細解析：

選項 D 正確的原因：根據 MongoDB 官方文件與考試大綱規範，當使用 { field: "value" } 的隱式等值語法查詢時，MongoDB 具備多樣性匹配能力。如果目標欄位是單一純量值（Scalar Value），它會直接比對是否相等（匹配 alex002 的 "gaming"）；如果該欄位是一個陣列（Array），MongoDB 會自動遍歷該陣列，檢查內部是否「包含」該指定值（匹配 alex003 陣列中的 "gaming"）。配合正確的點號表示法字串包裹，此查詢會正確且同時返回這兩筆文檔。

其他選項錯誤的原因：

選項 A 錯誤：忽略了 MongoDB 對陣列包含關係的隱式查詢優化，陣列欄位不需特殊解構即可比對單一元素。

選項 B 錯誤：忽略了隱式等值查詢同樣適用於普通標量字串的比對。

選項 C 錯誤：題目中的查詢欄位 "status.interests" 已經正確使用了雙引號包裹，符合點號表示法（Dot Notation）的語法規範，不會報錯。

題目 2：[複選題 (選 2 個)]
公司的電商後台系統有一套產品訂單集合 sales，其中每個文檔都包含一個名為 items 的陣列欄位，用來儲存單次交易購買的多個商品明細。某一筆文檔結構如下：

JSON


{
  "_id": ObjectId("6a2f5eb7bc8ac27e8830c35a"),
  "order_no": "TX9991",
  "items": [
    { "name": "keyboard", "price": 120, "quantity": 2 },
    { "item_type": "accessory", "name": "laptop", "price": 1200, "quantity": 1 }
  ]
}
營運團隊希望找出「在同一個商品品項中，商品名稱為 laptop、單價大於 800 元、且購買數量大於或等於 1」的訂單。
請問下列哪兩個查詢文檔（Query Documents）可以完全正確且安全地實現這個業務邏輯？（Select 2 answers）

A.

JSON


{
  "items": {
    "$elemMatch": { "name": "laptop", "price": { "$gt": 800 }, "quantity": { "$gte": 1 } }
  }
}
B.

JSON


{
  "items.name": "laptop",
  "items.price": { "$gt": 800 },
  "items.quantity": { "$gte": 1 }
}
C.

JSON


{
  "items": {
    "$subMatch": { "name": "laptop", "price": { "$gt": 800 }, "quantity": { "$gte": 1 } }
  }
}
D.

JSON


{
  "$and": [
    { "items": { "$elemMatch": { "name": "laptop", "price": { "$gt": 800 } } } },
    { "items": { "$elemMatch": { "name": "laptop", "quantity": { "$gte": 1 } } } }
  ]
}
正確答案： A, D
核心考點： $elemMatch 運算子確保陣列內嵌文檔的多條件多欄位匹配
詳細解析：

選項 A 正確的原因：根據 MongoDB 官方標準，當你需要對陣列（Array）中的「同一個內嵌文檔（Subdocument）」同時滿足多個篩選條件時，必須使用 $elemMatch 運算子，將所有條件包裹在同一個作用域中，這能精確定位到符合規格的單一商品。

選項 D 正確的原因：雖然這個寫法較為冗長，但它利用 $and 邏輯運算子將兩個 $elemMatch 條件組合起來。第一個條件確保陣列中有某個商品的 name 是 "laptop" 且 price > 800；第二個條件確保陣列中有某個商品的 name 是 "laptop" 且 quantity >= 1。由於兩者都限定了 name: "laptop"，在外層 $and 的共同約束下，實際上是指向同一個商品品項，因此在邏輯上也是安全且正確的。

其他選項錯誤的原因：

選項 B 錯誤：這是 C100DEV 認證中最經典且高頻的陣列邏輯陷阱！若不使用 $elemMatch 而直接使用點號（Dot Notation）將多個條件拆開，MongoDB 會獨立判斷陣列中的元素。只要陣列中「有任意一個商品名稱是 laptop」，且「有任意一個商品價格大於 800（例如鍵盤價格為 900）」，且「有任意一個商品數量大於等於 1」，該文檔就會被錯誤返回。這無法保證所有條件都同時限制在「同一個商品物件」上。

選項 C 錯誤：$subMatch 非 MongoDB 官方支援的合法運算子，是混淆關係型資料庫子查詢觀念的虛構運算子。

題目 3：[單選題]
線上讀書會系統的 books 集合儲存了各書籍的資訊。每本圖書文檔都有一個 genre（流派）欄位，該欄位可能是一個單一的字串（標量），也可能是一個儲存多個標籤字串的陣列（例如：genre: ["Historical", "Fiction"]）。
如果系統管理員執行以下命令：

JavaScript


db.books.find({ genre: "Historical" })
請問關於此命令返回的結果，下列敘述何者完全符合 MongoDB 的官方運作機制？

A. 僅會返回 genre 欄位型態為單一字串且值精確等於 "Historical" 的圖書文檔。
B. 會返回 genre 欄位值等於標量 "Historical"，或者 genre 欄位為陣列且內部「包含」"Historical" 元素的所有圖書文檔。
C. 會返回資料庫中任何欄位（包含內嵌文檔或全文字段）含有 "Historical" 字串的所有圖書文檔。
D. 系統會拋出型態不匹配的錯誤，因為不能使用相同的語法同時查詢標量與陣列欄位。

正確答案： B
核心考點： 隱式等值查詢對標量與陣列元素的共通相容性
詳細解析：

選項 B 正確的原因：這道題直接引用了官方範例題目的核心概念。在 MongoDB 之中，對一個欄位進行最基礎的鍵值對查詢（例如 { genre: "Historical" }），其匹配邏輯是寬鬆且靈活的：它既能完美匹配欄位本身就是該字串的文檔，也能自動向下探測，匹配那些將該字串作為陣列其中一個元素的文檔。這展現了 MongoDB 作為 NoSQL 資料庫在處理半結構化資料時的易用性。

其他選項錯誤的原因：

選項 A 錯誤：限縮了查詢範圍，忽略了對陣列元素的自動檢索匹配能力。

選項 C 錯誤：該查詢加上了特定的路徑限制（genre），並不會發起跨欄位的全文搜尋。

選項 D 錯誤：MongoDB 是動態 Schema 語言，此語法完全合法，不會拋出任何型態錯誤。

題目 4：[單選題]
在安全審計系統中，你需要對 user 集合進行一項風險排查。已知 user 集合的文檔結構中有一個名為 status 的內嵌物件，用來儲存登入相關指標。某一筆文檔格式如下：

JSON


{
  "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"),
  "name": "alex001",
  "status": {
    "login_count": 45,
    "failed_attempts": 2
  }
}
主管要求你找出所有 「登入失敗次數（failed_attempts）剛好等於 2 次」 的用戶文檔。下列四個由不同實習生寫出的 MongoDB 查詢指令中，哪一個是完全正確且能成功執行並返回預期結果的？

A. db.user.find({ status: { failed_attempts: 2 } })
B. db.user.find({ "status.failed_attempts": 2 })
C. db.user.find({ status.failed_attempts: 2 })
D. db.user.find({ "status": { "$elemMatch": { "failed_attempts": 2 } } })

正確答案： B
核心考點： 內嵌文檔物件的精確匹配與點號表示法（Dot Notation）引號規範
詳細解析：

選項 B 正確的原因：在 MongoDB 中，若要深入查詢內嵌物件（Embedded Document）內部的特定欄位，標準且最佳的做法是使用點號表示法（Dot Notation），將路徑拼接為 "status.failed_attempts"。同時，官方規範強調：凡是路徑中帶有點號（.）的欄位，在查詢表達式中必須強制使用引號（單引號或雙引號）包裹成字串，否則無法通過 JavaScript/JSON 的語法解析。

其他選項錯誤的原因：

選項 A 錯誤：這會變成對整個 status 物件進行內嵌文檔精確匹配（Subdocument Exact Match）。這意味著 status 的內容、欄位的順序必須與你給定的 { failed_attempts: 2 }「完全一模一樣」。因為現實中 alex001 的 status 物件還包含了 login_count: 45，且順序不同，所以這個查詢會什麼都查不到（結果為空）。

選項 C 錯誤：這觸犯了嚴重的語法錯誤（SyntaxError）。因為 status.failed_attempts 中間帶有點號，卻沒有用引號包裹，資料庫驅動程式或 Shell 會在解析 JSON 鍵值時直接崩潰。

選項 D 錯誤：$elemMatch 運算子是專門用來查詢陣列（Array）中元素的。此處的 status 是一個單一的內嵌物件（Object）而非陣列，使用 $elemMatch 會導致邏輯錯誤或查詢無效。

題目 5：[複選題 (選 3 個)]
為了優化內部的通知機制，開發團隊希望在 user 集合中篩選出特定行為特徵的用戶。
現有的用戶文檔資料與結構如下（部分用戶的 interests 欄位為陣列型態）：

JSON


{ "_id": ObjectId("6a2f5dcabc8ac27e8830c34d"), "name": "alex001", "status": { "login_count": 45 } }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34e"), "name": "alex002", "status": { "interests": "gaming" } }
{ "_id": ObjectId("6a2f5eb7bc8ac27e8830c34f"), "name": "alex003", "status": { "interests": ["gaming", "music"] } }
業務需求為：「找出其興趣標籤中包含 'gaming' 的所有用戶文檔」。
請問下列哪三個查詢語法在 MongoDB Shell 中執行後，都能夠成功返回 alex002 與 alex003 這兩筆文檔？（Select 3 answers）

A. db.user.find({ "status.interests": "gaming" })
B. db.user.find({ "status.interests": { "$in": ["gaming"] } })
C. db.user.find({ "status.interests": ["gaming"] })
D. db.user.find({ "$or": [ { "status.interests": "gaming" }, { "status.interests": { "$elemMatch": { "$eq": "gaming" } } } ] })

正確答案： A, B, D
核心考點： 陣列多值比對運算子與隱式包含語法的等價轉換
詳細解析：

選項 A 正確的原因：如前述考點所述，利用隱式等值語法，可以同時相容於純量字串 "gaming" 的精確匹配（alex002）以及陣列元素的包含匹配（alex003）。

選項 B 正確的原因：$in 運算子後面接一個陣列條件清單（["gaming"]），其核心行為是「只要目標欄位的值等於清單中的任意一員，或者目標陣列欄位中包含清單中的任意一員即算匹配」。因此它與選項 A 的隱式語法在執行邏輯上完全等價，可以正確返回兩筆文檔。

選項 D 正確的原因：這是一個進階的邏輯組合。第一個 $or 分支 { "status.interests": "gaming" } 透過隱式匹配已經可以把 alex002 和 alex003 都查出來了；第二個分支使用了專門針對純字串陣列元素的 { "$elemMatch": { "$eq": "gaming" } }，這能精確匹配到 alex003 的陣列。由於是 $or（聯集）關係，最終返回的集合依然是正確的 alex002 與 alex003，且語法完全合法。

其他選項錯誤的原因：

選項 C 錯誤：這是一個極具迷惑性的陣列精確匹配陷阱。當直接將一個陣列 ["gaming"] 賦值給查詢條件時，MongoDB 會執行全陣列精確匹配（包含長度、元素值、以及嚴格的順序）。對照資料，alex002 是字串不是陣列（不匹配）；alex003 的陣列是 ["gaming", "music"]（長度與元素不完全相等，不匹配）。因此選項 C 最終會返回空結果。







* Lesson 5 : Finding Documents by Using Logical Operators


C100DEV 模擬考題：邏輯運算子篇
題目 1：[單選題]
某航空公司正在開發航線查詢系統。若要在 routes 集合中尋找目的地機場（dst_airport）為 "SEA" 或者出發地機場（src_airport）為 "SEA" 的所有航班。下列哪一個查詢語法是完全正確且能成功執行的？

A. db.routes.find({ $or: [ { dst_airport: "SEA" }, { src_airport: "SEA" } ] })
B. db.routes.find({ dst_airport: "SEA", $or: { src_airport: "SEA" } })
C. db.routes.find({ $or: { dst_airport: "SEA", src_airport: "SEA" } })
D. db.routes.find({ dst_airport: "SEA" } , { src_airport: "SEA" })

正確答案： A
核心考點： $or 運算子的語法結構規範
詳細解析：

選項 A 正確的原因： 根據 MongoDB 官方文件，$or 運算子要求其右側的值必須是一個陣列（Array），而陣列中的每個元素都必須是一個獨立完整的條件物件。選項 A 完全符合 { $or: [ { 條件1 }, { 條件2 } ] } 的標準語法。

其他選項錯誤的原因：

選項 B 錯誤：這是不合法的邏輯結構，$or 不能以這種非對稱的巢狀方式直接掛載在另一個欄位的同級層。

選項 C 錯誤：$or 後面直接接了花括號物件 {}，漏掉了陣列方括號 []，這會導致 MongoDB 拋出語法錯誤。

選項 D 錯誤：在 find() 方法中傳入兩個獨立的 JSON 物件，第一個物件會被視為「查詢條件（Query）」，第二個物件會被視為「投影參數（Projection）」，因此無法達成 OR 的邏輯篩選。



題目 2：[單選題]
外送平台正在重構紐約市 Astoria 地區的餐車篩選模組。需要找出所有符合以下三個條件的餐車：

行業別（sector）為 "Mobile Food Vendor - 881"

縣市地區（address.city）為 "ASTORIA"

檢查結果（result）為 "Pass"

為了保持程式碼簡潔，開發團隊希望使用隱式 AND（Implicit $and）。請問下列哪一個查詢文檔（Query Document）最符合官方的最佳實踐？

A. { "sector": "Mobile Food Vendor - 881" } , { "address.city": "ASTORIA" } , { "result": "Pass" }
B. { "sector": "Mobile Food Vendor - 881", "address.city": "ASTORIA", "result": "Pass" }
C. { $and: { "sector": "Mobile Food Vendor - 881", "address.city": "ASTORIA", "result": "Pass" } }
D. { &and: [ { "sector": "Mobile Food Vendor - 881" }, { "address.city": "ASTORIA" }, { "result": "Pass" } ] }

正確答案： B
核心考點： 隱式 AND 的宣告與語法規範
詳細解析：

選項 B 正確的原因： 在 MongoDB MQL 中，當你在同一個 JSON 物件（花括號 {}）中指定多個不同欄位的鍵值對，並用逗號隔開時，MongoDB 預設就會以隱式 AND 的邏輯來處理它們。這是最簡潔且官方強烈推薦的最佳實踐（Best Practices）。

其他選項錯誤的原因：

選項 A 錯誤：將三個條件拆成獨立的物件並用逗號隔開，會違反 find() 的參數定義（如題目 1 所述），導致語法無效。

選項 C 錯誤：即便要使用顯式 $and，其後方的值也必須是陣列 []，不能直接跟花括號物件。

選項 D 錯誤：高仿真干擾項。MongoDB 的所有運算子前綴都必須是美元符號 $，而不是安培符號 &。



題目 3：[單選題]
在航班調度系統中，資深架構師寫了以下的查詢指令：

JavaScript


db.routes.find({
  $and: [
    { $or: [ { dst_airport: "IST" }, { src_airport: "IST" } ] },
    { $or: [ { stops: 0 }, { "airline.name": "Turkish Airlines" } ] }
  ]
})
請問這道指令返回的文檔在商務邏輯上代表什麼含義？

A. 所有直飛的土耳其航空航班，且必須從伊斯坦堡機場（IST）出發。
B. 所有從伊斯坦堡機場（IST）出發的航班，或是由土耳其航空營運且直飛的航班。
C. 所有從伊斯坦堡機場（IST）出發或抵達，且同時為直飛航班或由土耳其航空營運的航班。
D. 所有抵達伊斯坦堡機場（IST）的直飛航班，或者所有由土耳其航空營運的航班。


正確答案： C
核心考點： 複合邏輯查詢（$and 巢狀包含 $or）的商務邏輯解讀


題目 4：[單選題]
行銷團隊希望在 routes 集合中，尋找「由 Southwest Airlines 營運且轉機次數（stops）大於或等於 1 次」的航線。開發人員想要透過顯式 $and（Explicit $and）來撰寫這個語法。
請問下列哪一個是完全正確的顯式 $and 語法？

A. db.routes.find({ $and: [ "airline.name": "Southwest Airlines", stops: { $gte: 1 } ] })
B. db.routes.find({ $and: [ { "airline.name": "Southwest Airlines" }, { stops: { $gte: 1 } } ] })
C. db.routes.find({ "airline.name": "Southwest Airlines", $and: [ { stops: { $gte: 1 } } ] })
D. db.routes.find({ $and: { "airline.name": "Southwest Airlines", stops: [ { $gte: 1 } ] } })
正確答案： B
核心考點： 顯式 $and 運算子的標準陣列元素宣告
詳細解析：

選項 B 正確的原因： 當使用顯式 $and 時，語法結構必須為 { $and: [ { 條件1 }, { 條件2 } ] }。陣列內部的每一個條件，都必須用獨立的花括號 {} 包裹成完整的 JSON 物件，選項 B 格式完全正確。

其他選項錯誤的原因：

選項 A 錯誤：雖然加了陣列方括號 []，但陣列內部的兩個條件沒有分別用花括號 {} 包裹，這是不合法的 JavaScript/JSON 陣列語法。

選項 C 錯誤：將其中一個條件抽到最外層，而 $and 陣列中只剩一個條件，雖然部分版本可能相容，但這不符合「顯式組合多條件」的正確標準寫法。

選項 D 錯誤：語法完全混亂，$and 後方直接使用了花括號物件，且比較運算子位置錯誤。


題目 5：[單選題]
在進行查詢優化與架構檢視時，出題委員指出：在 MongoDB 的同一個查詢文檔中，若我們需要組合兩個不同的 $or 運算子條件群組（例如：滿足「條件 A 或 B」，並且同時要滿足「條件 C 或 D」）。
根據官方文件的最佳實踐，我們必須使用下列哪一個運算子作為最外層的包裹？

A. 不需要任何外層運算子，直接在同一個物件中並列兩個 $or 鍵名即可
B. $or
C. $and
D. $nor


正確答案： C
核心考點： 避免 JSON 鍵名重複與顯式 $and 的必要使用場景

詳細安全解析：

選項 C 正確的原因： 這是 C100DEV 證照中非常重要的一個觀念題。在標準的 JSON 規範中，同一個物件層級內不允許存在重複的鍵（Duplicate Keys）。如果我們直接寫 { $or: [...], $or: [...] }，後面的 $or 會直接把前面的 $or 覆蓋掉。因此，當一條查詢語句中需要同時套用多個獨立的 $or 區塊時，必須強制顯式使用 $and 運算子作為最外層的包裹，將這兩個 $or 區塊作為陣列元素傳入。

其他選項錯誤的原因：

選項 A 錯誤：如上所述，並列重複的鍵名會導致後者覆蓋前者，遺失部分查詢條件。

選項 B 錯誤：若最外層再使用 $or，會將整體的商務邏輯直接拉平變成「A 或 B 或 C 或 D」的聯集，無法表達兩個群組之間的「並且（交集）」關係。

選項 D 錯誤：$nor 代表「既不...也不...」，會徹底顛覆題目所需的邏輯關係



# C100DEV-MongoDB CRUD Operations: Replace and Delete Documents



  



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























