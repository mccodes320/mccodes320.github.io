* [Lesson 1 : Inserting Documents in a MongoDB Collection](#lesson-1--inserting-documents-in-a-mongodb-collection)
* [Lesson 2 : Finding Documents in a MongoDB Collection](#lesson-2--finding-documents-in-a-mongodb-collection)
* [Lesson 3 : Finding Documents by Using Comparison Operators](#lesson-3--finding-documents-by-using-comparison-operators)
* [Lesson 4 : Querying on Array Elements in MongoDB](#lesson-4--querying-on-array-elements-in-mongodb)
* [Lesson 5 : Updating Documents in a MongoDB Collection](#lesson-5--updating-documents-in-a-mongodb-collection)
* [Lesson 6 : Deleting Documents from a MongoDB Collection](#lesson-6--deleting-documents-from-a-mongodb-collection)


https://learn.mongodb.com/learn/course/mongodb-crud-operations-insert-and-find-documents/lesson-1-inserting-documents-in-a-mongodb-collection/learn

# Lesson 1 : Inserting Documents in a MongoDB Collection

In this unit, you will be introduced to CRUD operations in MongoDB by inserting and finding documents. Inserting and finding documents will help you discover the ease and usability of MongoDB. You'll also build your own queries that use comparison and logical operators. Using operators will make your queries more precise and, in turn, make your application easier to develop. Finally, you'll learn how to query elements in an array. Arrays are a crucial data type that you will encounter frequently, so it's important that you have a solid understanding of how to work with them.  

* iesertOne(): 插入單個文檔  
* insertMany(): 插入多個文檔

```sql
db.<collection>.iesertOne() 如果<collection>不存在, 就會自動創建
db.<collection>.iesertMany([<doc1>,<doc2>])
```

QA

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
    },
    {
      type: "quiz",
      score: 75,
    },
    {
      type: "homework",
      score: 88,
    },
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
      },
      {
        type: "quiz",
        score: 66,
      },
      {
        type: "exam",
        score: 70,
      },
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
      },
      {
        type: "quiz",
        score: 72,
      },
      {
        type: "quiz",
        score: 67,
      },
    ],
    class_id: 550,
  },
  {
    student_id: 223344,
    products: [
      {
        type: "exam",
        score: 45,
      },
      {
        type: "homework",
        score: 39,
      },
      {
        type: "quiz",
        score: 40,
      },
      {
        type: "homework",
        score: 88,
      },
    ],
    class_id: 551,
  },
])
```

### Q&A

What methods are available in MongoDB for inserting a single document? (Select one.)

a. .insertOne() 

b. .inserting() 

c. .InsertDocument()

d. .insertMany() 


==> A 

### Q&A

Select an answer choice and then click "See Results" to submit.

* What methods are available in MongoDB for inserting multiple documents? (Select one.) *

a.
.InsertDocument()  
b.
.inserting()  
c.
.insertOne()  
d.
.insertMany()  


==>  D

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


---

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













