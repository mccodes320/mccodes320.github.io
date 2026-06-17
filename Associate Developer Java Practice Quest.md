https://learn.mongodb.com/learn/course/associate-developer-java-practice-questions/prep-questions/practice-questions


## MongoDB Certification Practice

**1. Which numeric type is a valid MongoDB BSON type? (Choose 1)**

a. Float  
b. Number  
c. BIGINT  
**d. 32-bit integer**
**Answer:** D

**Detail:**
在 MongoDB 的 BSON 規範中，常見的數值類型包含：
* **32-bit integer** (int32)
* **64-bit integer** (int64 / Long)
* **Double** (64-bit IEEE 754 floating point)
* **Decimal128** (高精度小數)

---

> **💡 快速複習：**
> JSON 只有一種 `Number` 類型，但 BSON 為了優化儲存與運算效能，將數值細分為明確的位元長度（如 32-bit、64-bit）。這也是為什麼在 MongoDB 中，我們有時需要特別宣告資料型態的原因。






**2. Given the following documents in a collection:**
```json
{ "_id": 1, "n": [1, 2, 5], "p": 0.75, "c": "Green" },
{ "_id": 2, "n": "Orange", "p": "Blue", "c": 42, "q": 14 },
{ "_id": 3, "n": [1, 3, 7], "p": 0.85, "c": "Orange" }
```
Which two documents can successfully be added in the same collection?

a. { _id: 1, n: [1,2,5], p: 0.75, c: 'Green' }  
b. { _id: 5, n: [1,2,5], p: 0.75, c: 'Green' }  
c. { _id: 2, n: [1,2,5], p: 0.75, c: 'Green' }  
d. { _id: 6, n: [1,3,7], p: 0.85, c: 'Orange }

Answer: B, D

Detail:
這題主要考察 MongoDB 的兩個核心特性：

_id 的唯一性 (Primary Key Uniqueness)：
在同一個 Collection 中，_id 欄位的值必須是唯一的。

選項 (a) 的 _id: 1 與現有文件衝突。

選項 (c) 的 _id: 2 與現有文件衝突。
這兩者都會觸發 Duplicate Key Error 而導致寫入失敗。

靈活的 Schema (Dynamic Schema)：
只要 _id 不重複，MongoDB 允許存入任何結構的文件。選項 (b) 與 (d) 的 _id 分別為 5 與 6，目前集合中並不存在，因此可以順利新增。






**3. Given the following documents in a collection:**
```
{_id: 1, txt: "just some text"},
{_id: 2, txt: "just some text"}
```
Which two documents can successfully be added in the same collection?(Choose 2)  
  
a. {_id: 0, txt: "just some text"}
b. {_id: 1, txt: "just some text"}
c. {_id: [4], txt: "just some text"}
d. {_id: 3, txt: "just some text"}

Answer: A, D








4. Given the following document:
  
The name is a.log, the owner of the file is applicationA, the size of the file is 1KB, and the file was deleted.  
  
What command will properly add this document to the files collection using mongosh?(Choose 1)  
  
a. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: true })  
b. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1KB, deleted: True })  
c. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: true }) 
d. db.files.insertOne({ file: "a.log", owner: "applicationA", size: 1024, deleted: True })  

Answer: C











5. Given the following sample documents in products collection:

```
{ "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] },
{ "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] },
{ "name" : "GTablet", "price" : 899, "color" : [ "blue" ], "storage" : [ 16, 64, 128 ] },
{ "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] },
{ "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }
```

Given the following query:
```
db.products.find({$and : [{"price" : {$lte : 800}}, {$or : [{"color" : "purple"}, {"storage" : 1024}]}]})
```
What is the correct output of the query?

(Choose 1)


a. { "name" : "XPhone", "price" : 799, "color" : [ "white", "black" ], "storage" : [ 64, 128, 256 ] }  
b. { "name" : "XPad", "price" : 899, "color" : [ "white", "black", "purple" ], "storage" : [ 128, 256, 512 ] }  
c. { "name" : "GPhone", "price" : 599, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 512 ] }  
d. { "name" : "GPad", "price" : 699, "color" : [ "white", "orange", "gold", "gray" ], "storage" : [ 128, 256, 1024 ] }  

Answer: D
















6. An `inventory` collection consists of 200 documents.

What method should be used to get all documents from a cursor using mongosh?

(Choose 1)

a. db.inventory.findOne()  
b. db.inventory.find().toArray();  
c. db.inventory.find();  
d. db.inventory.findMany().toArray()  


Answer: B












7. A collection has documents like the following:

 { _id: 1, name: 'Oatmeal Fruit Cake with Gummy Bears ', price: 11)},  
 { _id: 2, name: 'Cheesecake Trifle with Chocolate Sprinkles ', price: 14)},  
 { _id: 3, name: 'Pistachio Brownie with Walnuts ', price: 5},  
 { _id: 4, name: 'Strawberry Ice Cream Cake with Butterscotch Syrup ', price: 3)}  
  
How should the 'autocomplete' index be defined to look for matches at the beginning of a word on the name field?(Choose 1)  

a. { "mappings": { "dynamic": false, "fields": { "name": [ { "type": "autocomplete", "tokenization": "regexCaptureGroup"} ] } }}  
b. { "mappings": { "dynamic": false, "fields": { "name": [ { "type": "autocomplete", "tokenization": "edgeGram"} ] } }}  
c. { "mappings": { "dynamic": false, "fields": { "name": [ { "type": "autocomplete", "tokenization": "nGram"} ] } }}  
d. { "mappings": { "dynamic": false, "fields": { "name": [ { "type": "autocomplete", "tokenization": "matchNGram"}} ] } }}  







Answer: B


```
說明

題目要求： 如何定義一個 autocomplete 索引，使其能夠在 name 欄位上尋找「單字開頭」（beginning of a word）的匹配項？

選項 b. edgeGram (正確答案)：
這是專門為自動完成設計的記號化（Tokenization）策略。edgeGram 會從單字的開頭邊緣開始切割字元。例如單字 "Cake" 會被切成 "C", "Ca", "Cak", "Cake"。這完全符合題目要求的「尋找單字開頭」的匹配。

選項 c. nGram：
nGram 會在單字的任意位置切割字元。以 "Cake" 為例，除了開頭外，它還會產生 "a", "ak", "ke" 等記號。雖然它也能找到開頭，但因為它包含了非開頭的匹配，會導致搜尋結果不夠精確，且索引量較大，通常用於搜尋單字中間的片段。

選項 a. regexCaptureGroup：
這是使用正規表示式抓取特定群組，不適用於一般通用的自動完成場景。

選項 d. matchNGram：
雖然 Atlas Search 確實有 autocomplete 類型，但在其 tokenization 設定中，標準的參數名稱是 edgeGram 或 nGram。

核心概念：Edge Gram vs. N-Gram為了讓你更直觀地理解為什麼自動完成（Autocomplete）通常首選 edgeGram，我們可以看下方的對比：策略針對單字 "Apple" 的切分結果適用場景edgeGramA, Ap, App, Appl, Apple自動完成 (Autocomplete)：當使用者輸入 "Ap" 時，能精準鎖定開頭為 Ap 的單字。nGramA, p, p, l, e, Ap, pp, pl, le, App...模糊搜尋 / 子字串搜尋：當使用者記得單字中間有 "pl" 時也能搜尋到。
```









8. Given the following sample documents:

{_id:1, name: "Quesedillas Inc.", active: true },
{_id:2, name: "Pasta Inc.", active: true },
{_id:3, name: "Tacos Inc.", active: false },
{_id:4, name: "Cubanos Inc.", active: false },
{_id:5, name: "Chicken Parm Inc.", active: false }
A company wants to create a mobile app for users to find restaurants by name. The developer wants to show the user restaurants that match their search. An Atlas Search index has already been created to support this query.

What query satisfies these requirements?(Choose 1)

a. db.restaurants.aggregate([{ "$search": { "text": { "path": "name", "synonym": "cuban"} } }])
b. db.restaurants.aggregate([{ "$search": { "text": { "path": "name", "query": "cuban"} } }])
c. db.restaurants.aggregate([{ "$search": { "text": { "field": "name","query": "cuban"} } }]) 
d. db.restaurants.aggregate([{ "$search": { "text": { "field": "name", "synonym": "cuban"} } }])







Answer: B
















9. Given the data set and query:
```json

{ "_id" : ObjectId("512bc95fe835e68f199c8686"), "player" : "p1", "score" : 89 }  
{ "_id" : ObjectId("512bc962e835e68f199c8687"), "player" : "p2", "score" : 85 }  
{ "_id" : ObjectId("55f5a192d4bede9ac365b257"), "player" : "p2", "score" : 65 }  
{ "_id" : ObjectId("55f5a192d4bede9ac365b258"), "player" : "p3", "score" : 65 }  
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b259"), "player" : "p3", "score" : 75 }  
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25a"), "player" : "p5", "score" : 70 }  
{ "_id" : ObjectId("55f5a1d3d4bede9ac365b25b"), "player" : "p6", "score" : 100 }  
```

```json
db.scores.aggregate(
[{ $group: {
      _id: '$player',
      score: {
           $avg: '$score'
      }
 },
 { $match: {
     score: { 
            $gt: 70
      }
 }
])
```
What is the output?​

(Choose 1)

a. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 85 } { "player" : "p3", "score" : 75 } { "player" : "p6", "score" : 100 }
b. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p6", "score" : 100 }   
c. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p5", "score" : 70 } { "player" : "p6", "score" : 100 } 
d. { "player" : "p1", "score" : 89 } { "player" : "p2", "score" : 75 } { "player" : "p3", "score" : 70 } { "player" : "p6", "score" : 100 }






Answer: B






10. A collection coll in database mdb has the following documents :

{_id: 1, type: "A", value: 60}
{_id: 2, type: "B", value: 80}
{_id: 3, type: "C", value: 10}
After executing the following aggregation pipeline:

db.getSiblingDB("mdb").coll.aggregate([
    { $out: {db:'test', collection:'results'}} ])
What are two expected results?

(Choose 2)

a. Collection `results` is created in database `test`.
b. There is a syntax error command. Collection `results` is not created.
c. No documents in collection `coll` are written to collection `results`. 
d. All documents in collection `coll` are written to collection `results`.








Answer:  A,D




```
選項分析：

(a) 在 test 資料庫建立了 results 集合：錯誤。因為操作失敗，集合不會被建立。

(b) 指令存在語法錯誤，集合未建立 (Correct)：正確。這反映了跨庫操作在特定限制下被視為無效指令。

(c) coll 中的文件沒有被寫入 results (Correct)：正確。因為指令執行失敗，自然沒有資料遷移。

(d) 所有文件都寫入 results：錯誤。這是操作成功才會有的結果。

// 格式：db.getSiblingDB("資料庫名稱").集合名稱.countDocuments()
db.getSiblingDB("test").users.aggregate([{
  $out: {
    db:'testInput',
    coll:'results'
  }
}])

```










11. Given the following documents:

{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}
If the following command is executed:

db.coll.replaceOne({}, {a: "ten", b: "five"})
What is the result?(Choose 1)

a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}  
b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}  
c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}  
d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"} 








Answer:  B





12. Given the collection called coll, with only the following documents,

{  _id:1,  a:1,  b:1 },
{  _id:2,  a:2 }

The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()?(Choose 1)

a. [{_id:1,  b:2}, {_id:2,  b:2}]
b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}] 
c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}]




Answer:  D






13. Given the following documents:

{_id:1, a: "one", b: "four"}
{_id:2, a: "two", b: "four"}
{_id:3, a: "three", b: "four", c: "three"}
If the following command is executed:

db.coll.replaceOne({}, {a: "ten", b: "five"})
What is the result?(Choose 1)

a. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five"}
b. {_id:1, a: "ten", b: "five"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}  
c. {_id:1, a: "ten", b: "five"} {_id:2, a: "ten", b: "five"} {_id:3, a: "ten", b: "five", c: "three"}
d. {_id:1, a: "one", b: "four"} {_id:2, a: "two", b: "four"} {_id:3, a: "three", b: "four", c: "three"}





Answer:  B









14. Given the collection called coll, with only the following documents,

{  _id:1,  a:1,  b:1 },
{  _id:2,  a:2 }
The update operation db.coll.updateMany({},{$set:{b:2}}) successfully completes.

What is the output of db.coll.find()? (Choose 1)


a. [{_id:1,  b:2}, {_id:2,  b:2}]
b. [{_id:1,  a:1,  b:2}, {_id:2,  a:2}] 
c. [{_id:1,  a:1,  b:1}, {_id:2,  a:2,  b:2}]
d. [{_id:1,  a:1,  b:2}, {_id:2,  a:2,  b:2}]



Answer:  D


15. Given the following document from the cakeFlavors collection. All documents in this collection have the same schema.

{
"_id" : 1,
"flavor" : "chocolate",
"number" : 15
}
What operation on the cakeFlavors collection will update the value of the number field to 100 for a document with a "strawberry" flavor value and insert a new document if it does not exist?(Choose 1)

a. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
b. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { $upsert: true })
c. db.cakeFlavors.insertOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true })
d. db.cakeFlavors.updateOne({ flavor: "strawberry"} , { $set: { number: 100 } }, { upsert: true }) 


Answer:  D


16. Given the following example document from the movie collection:

{
   _id: ObjectId("62872ccd590c3c06d78af00d"),
   genres: [ "Drama", "Romance", "War" ],
   title: "A.B.",
   year: 1921,
   tomatoes: { rating: 3.9, votes: 507, id: "76" },
   countries: [ "USA" ],
   classic : false
}
All documents in this collection have the same schema.

What command updates the value of the classic field to true for all documents with a year value less than 2000?(Choose 1)

a. db.movie.updateOne({ year: { $lt: 2000 } }, { $set: { classic: true } }, { multi: true })
b. db.movie.updateMany({ year: { $lt: 2000 } }, { $set: { classic: true } })  
c. db.movie.updateMulti({ year: { $lt: 2000 } }, { $set: { classic: true } })
d. db.movie.updateBulk({ year: { $lt: 2000 } }, { $set: { classic: true } })


Answer:  B









17. Given the following sample documents in the loans collection:

{ _id: 122, book: "ABC", name: "L.A.", date: ISODate("2022-05-20") },
{ _id: 343, book: "EFF", name: "T.B.", date: ISODate("2022-05-22") },
{ _id: 454, book: "CFH", name: "M.C.", date: ISODate("2022-05-12") }
What command deletes the document where the value of book is "EFF" and user is "T.B.", and returns the deleted document?(Choose 1)

a. db.loans.deleteOne( { book: "EFF", name: "T.B." } )
b. db.loans.findOneAndDelete( { book: "EFF", name: "T.B." } )  
c. db.loans.remove( { book: "EFF", name: "T.B." } , {returnDocument: true} )
d. db.loans.deleteOne( { book: "EFF", name: "T.B." }, {returnDocument: true} )


Answer:  B




```
db.test.find().toArray()

  { _id: ObjectId('69c54f90fb71ca84655214f2'), a: 1, b: 2 },
  { _id: ObjectId('69c54f90fb71ca84655214f3'), a: 2, b: 2 },
  { _id: ObjectId('69c55475ce43f2c803755265'), a: 3, c: 3 },
  { _id: ObjectId('69c5549dce43f2c803755266'), a: 4, c: 4 },
  { _id: ObjectId('69c55592ce43f2c80375526f'), a: 5, c: 4 }
]

db.test.findOneAndDelete({a:1,b:2})
{
  _id: ObjectId('69c54f90fb71ca84655214f2'),
  a: 1,
  b: 2
}
db.test.deleteOne({a:2})
{
  acknowledged: true,
  deletedCount: 1
}


```


18. Given the following sample documents in the inventory collection:

{ _id: 6305, name : "A. S.", "assignment" : 5, "status" : "A" },   
{ _id: 6308, name : "B. M.", "assignment" : 3, "status" : "B" },   
{ _id: 6312, name : "E. M.", "assignment" : 5, "status" : "C" },   
{ _id: 6319, name : "R. S.", "assignment" : 2, "status" : "D" },   
{ _id: 6322, name : "A. S.", "assignment" : 2, "status" : "A" },   
{ _id: 6234, name : "R. S.", "assignment" : 1, "status" : "B" },
{ _id: 6235, name : "A. S.", "assignment" : 1, "status" : "C" },
{ _id: 6315, name : "E. M.", "assignment" : 3, "status" : "A" }
What expression will remove all documents with "status" : "C" in the inventory collection?(Choose 1)

a. db.inventory.delete ({"status" : "C"})
b. db.inventory.deleteOne ({"status" : "C"})
c. db.inventory.deleteMany ({"status" : "C"})  
d. db.inventory.findOneAndDelete ({"status" : "C"})




Answer:  C








19. Given the following documents in the ratings collection:

{ _id: 0, hotel: "AAA", rating: 4.5 },
{ _id: 1, hotel: "BBB", rating: 3.0 },
{ _id: 2, hotel: "CCC", rating: 4.2 }
What mongosh command will return a document for hotel "CCC"?(Choose 1)

a. db.ratings.return_one( {hotel: "CCC"} )
b. db.ratings.find_one( {hotel: "CCC"} )
c. db.ratings.returnOne( {hotel: "CCC"} )
d. db.ratings.findOne( {hotel: "CCC"} ) 






Answer:  D



20. The following query generates a collection scan:

db.people.find({employer : "ABC" }).sort ({last_name:1 , job:1})
Which two indexes will most improve the performance of the query?(Choose 2)

a. db.people.createIndex({employer:1, last_name : 1  , job : 1 } )  
b. db.people.createIndex({employer:1, last_name : -1 , job : -1 } )  
c. db.people.createIndex({employer:1, last_name : -1 , job : 1 } )
d. db.people.createIndex({employer:1, last_name : 1  , job : -1 } )







Answer:  AB

21. Given a collection called collection, in which all documents have the following shape:

{
 _id:1,
 objs:[{a:1,b:2},{a:2,b:1}] 
}
And the query on this collection:

db.collection.find({"objs.a":1})  
What index will support this query? (Choose 1)  

a. {"objs.a":1}  
b. {objs:1}  
c. {objs:1,"objs.a"1}  
d. {objs:1,"a"1}  








Answer:  A



查詢優化器（Query Planner）會去尋找鍵值完全對應 "objs.a" 的索引

多鍵索引（Multikey Index）: 對內嵌文件陣列（Array of Embedded Documents）進行索引優化

db.collection.createIndex({"objs.a": 1})




22. Given the following query(Choose 2):
db.coll.find({}).sort({"product": 1, "price": 1})
Which two indexes will improve the performance of this query?

a. {"product": 1, "price": 1}  **correct**
b. {"product": 1, "price": -1}
c. {"product": -1, "price": 1}
d. {"product": -1, "price": -1}  **correct**










Answer:  AD









23. Given the following query(Choose 2):

db.coll.find({}).sort({"product": 1, "price": 1})
Which two indexes improve the performance of this query the most?

a. { v: 2, key: { price: 1, product: 1 }, name: 'price_1_product_1' }
b. { v: 2, key: { product: 1, price: 1 }, name: 'product_1_price_1' }  
c. { v: 2, key: { price: -1, product: -1 }, name: 'price_-1_product_-1' }
d. { v: 2, key: { product: -1, price: -1 }, name: 'product_-1_price_-1' }  










Answer:  BD





24. What mongosh command shows how many indexes are associated with an inventory collection?(Choose 1)

a. db.inventory.getIndexes()   
b. db.inventory.showIndexes()
c. db.inventory.displayIndexes()
d. db.inventory.indexes()












Answer:  A






25. A typical products collection is in an e-commerce database.
What schema is the most effective?(Choose 1)

a. Orders for products should be embedded as an array in each product document.
b. Reviews for products should be embedded as an array in each product document.
c. Current and historical prices for product should be embedded in each product document. 
d. Current inventory/availability for product should be embedded in each product document. 










Answer:  D GCP:C









26. A Cooking dataset is in Atlas. There is a Recipes database with a Desserts collection.

How can one document be found that provides a recipe for cookies without chocolate using Atlas Data Explorer?(Choose 1)

a.
1. Select the collection on the left-hand side.
2. Select the "Aggregation" view.
3. Specify the first stage as `$match` query: {dessert_type: "Cookie"} ```
4. Specify the second stage as `$match` query: ``` {ingredients: {$all: ["chocolate"]}}
5. Set `$limit` to 1.

b.
1. Select the collection on the left-hand side.
2. Select the "Aggregation" view.
3.Specify the first stage as `$match` query: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]}} ```
4. Set `$limit` to 1.  **correct**

c.
1. Select the collection on the left-hand side.
2. Specify the "Find" view.
3. Specify the filter query: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"]} ```
4. Specify the project query: ``` {dessert_type: 1}


d.
1. Select the collection on the left-hand side.
2. Specify the "Find" view.
3. Specify the filter query with limit: ``` {dessert_type: "Cookie", ingredients: {$nin: ["chocolate"], $limit: 1}











Answer: B



27. What are two valid method names for the MongoClient class?(Choose 2)

a. close()  
b. open()
c. destroy()
d. getDatabase()  









Answer: AD




28. What are two advantages to using Connection Pooling within the Java Driver?  (Choose 2)

a. Reduce the latency for an application. **correct**
b. Limit the number of connections to the server. **correct**
c. Remove the need for the application to authenticate.
d. Remove the need to open and close connections in the application.










Answer: AB

29. Assuming correct imports, properly instantiated MongoClient called client, and given the following Java code:

MongoCollection collection = client.getDatabase("employees").getCollection("records");
Bson filter = eq("position", "Developer Advocate");
Bson update = set("position", "Developer Evangelist");
What is the correct syntax for updating multiple documents with a single command?
(Choose 1)


a. collection.UpdateMany(update, filter);
b. collection.UpdateMany(filter, update); 
c. collection.UpdateMultiple(update, filter);
d. collection.UpdateMultiple(filter, update);










Answer: B

30. Given the following sample documents:

{_id:1, name: "Quesedillas Inc.", active: true },
{_id:2, name: "Pasta Inc.", active: true },
{_id:3, name: "Tacos Inc.", active: false },
{_id:4, name: "Cubanos Inc.", active: false },
{_id:5, name: "Chicken Parm Inc.", active: false },
A company wants to create a mobile app for users to find restaurants by name. The developer wants to show the user restaurants that match their search. An Atlas Search index has already been created to support this query.

What query satisfies these requirements?(Choose 1)

a. db.restaurants.aggregate([{ "$search": { "text": { "path": "name", "synonym": "cuban"}    } }])
b. db.restaurants.aggregate([{ "$search": { "text": { "path": "name", "query": "cuban"}    } }])  
c. db.restaurants.aggregate([{ "$search": { "text": { "field": "name", "query": "cuban"}    } }])
d. db.restaurants.aggregate([{ "$search": { "text": { "field": "name", "synonym": "cuban"}    } }])












Answer: B
















