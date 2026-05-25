* Lesson 1 : Inserting Documents in a MongoDB Collection
* Lesson 2 : Finding Documents in a MongoDB Collection
* Lesson 3: Finding Documents by Using Comparison Operators
* Lesson 4: Querying on Array Elements in MongoDB
* Lesson 5: Finding Documents by Using Logical Operators


https://learn.mongodb.com/learn/course/mongodb-crud-operations-insert-and-find-documents/lesson-1-inserting-documents-in-a-mongodb-collection/learn

In this unit, you will be introduced to CRUD operations in MongoDB by inserting and finding documents. Inserting and finding documents will help you discover the ease and usability of MongoDB. You'll also build your own queries that use comparison and logical operators. Using operators will make your queries more precise and, in turn, make your application easier to develop. Finally, you'll learn how to query elements in an array. Arrays are a crucial data type that you will encounter frequently, so it's important that you have a solid understanding of how to work with them.  

iesertOne(): 插入單個文檔. 
insertMany(): 插入多個文檔

```json
db.<collection>.iesertOne() 如果<collection>不存在, 就會自動創建
db.<collection>.iesertMany([<doc1>,<doc2>])
```

QA

Insert a Single Document  
  
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

Insert Multiple Documents

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

Q&A

What methods are available in MongoDB for inserting a single document? (Select one.)

a. .insertOne() 

b. .inserting() 

c. .InsertDocument()

d. .insertMany() 


==> A 

Select an answer choice and then click "See Results" to submit.

* What methods are available in MongoDB for inserting multiple documents? (Select one.) *



---

 Lesson2 : Finding Documents in a MongoDB Collection

db.<collection>.find()

{ field : <value> }

{ field : { $eq : <value> } }


db.<collection>.find({
   <field> : { $in: 
     [<value>, <value>,...]
})


** Finding Documents in a MongoDB Collection

Review the following code, which demonstrates how to query documents in MongoDB.

** Find a Document with Equality

When given equality with an _id field, the find() command will return the specified document that matches the _id. Here's an example:

```
db.zips.find({ _id: ObjectId("5c8eccc1caa187d17ca6ed16") })
```

** Find a Document by Using the $in Operator

Use the $in operator to select documents where the value of a field equals any value in the specified array. Here's an example:

```sql
db.zips.find({ city: { $in: ["PHOENIX", "CHICAGO"] } })
```

Quiz 


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

==> B

A: Incorrect. The $nin operator returns documents that do not contain the values specified in the array. Including this query would not return the specified Chicago zip codes.
  
B: Correct! The $in operator returns documents that contain the values specified in the array. This query will return the specified Chicago zip codes.

C: Incorrect. This syntax is inaccurate because the $eq operator matches documents that contain only one specified value.

D: Incorrect. This syntax is inaccurate because the implicit equality operator matches documents that contain only one specified value.






---



** Lesson 3: Finding Documents by Using Comparison Operators

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

** Lesson 4: Querying on Array Elements in MongoDB **

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

* Lesson 5: Finding Documents by Using Logical Operators *



** Finding Documents by Using Logical Operators** 
Review the following logical operators: implicit $and, $or, and $and.

**  Find a Document by Using Implicit $and**  
Use implicit $and to select documents that match multiple expressions. For example:

db.routes.find({ "airline.name": "Southwest Airlines", stops: { $gte: 1 } })
**  Find a Document by Using the $or Operator** 
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

db.<collection>.find({
  $and: [
    {<expression>},
    {<expression>},
    ...
  ]
})

$or

db.<collection>.find({
  $or: [
    {<expression 1>},
    {<expression 2>},
    ...
  ]
})


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

QA

*-*-*
You want to know which mobile food trucks in your neighborhood, Astoria, are the best spots to eat. Using the inspections collection, you’re making a map of all mobile food trucks nearby that have passed inspection. What should you include in your query document to ensure that you find all the mobile food vendors in Astoria that passed inspection? (Select one.)

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




