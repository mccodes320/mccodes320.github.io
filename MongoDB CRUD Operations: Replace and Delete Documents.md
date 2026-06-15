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





