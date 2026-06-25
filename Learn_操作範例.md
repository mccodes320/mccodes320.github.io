
[CRUD_Insert_find](#crud_insert_find)  
  [1.1 insert](#11-insert)  
  [1.2 insertOne](#12-insertone-不同類型)  
  [1.3 inserMany](#13-insermany)  
  [1.4 例外處理](#1.4例外處理)  


[END](#end)  













# CRUD_Insert_find

## 1.1 insert

已經棄用了, 未來請使用 insertOne, insertMany, or bulkWrite.

```sql
db.test.insert({case1:1})
DeprecationWarning: Collection.insert() is deprecated. Use insertOne, insertMany, or bulkWrite.

{
  acknowledged: true,
  insertedIds: { '0': ObjectId('6910d26a60eef558cb6c4bd3') }
}
```

## 1.2 insertOne 不同類型

```sql
db.test.insertOne(
       {
         "name": "Alice",
         "age": 30,
         "isStudent": false,
         "courses": ["Math", "Science"],
         "address": {
           "street": "123 Main St",
           "city": "Anytown"
         }
       }
       )
{
  acknowledged: true,
  insertedId: ObjectId('6924634119543dba926c4bd0')
}
```


## 1.3 inserMany

輸入是Array要使用 []

```sql
db.stores.insertMany([
  { "_id": 1, "name": "芝加哥咖啡廳", "zip": "60601" },   // 符合
  { "_id": 2, "name": "風城書店", "zip": "60603" },     // 符合
  { "_id": 3, "name": "千禧公園紀念品店", "zip": "60605" }, // 符合
  { "_id": 4, "name": "郊區大賣場", "zip": "60123" },    // 不符合
  { "_id": 5, "name": "洛杉磯分店", "zip": "90001" }     // 不符合
])


{
  acknowledged: true,
  insertedIds: {
    '0': 1,
    '1': 2,
    '2': 3,
    '3': 4,
    '4': 5
  }
```


## 1.4 例外處理

如果沒用[]框起

```
MongoInvalidArgumentError: Argument "docs" must be an array of documents
```




## CRUD_find

```sql
db.stores.insertMany([
  { "_id": 1, "name": "芝加哥咖啡廳", "zip": "60601" },   // 符合
  { "_id": 2, "name": "風城書店", "zip": "60603" },     // 符合
  { "_id": 3, "name": "千禧公園紀念品店", "zip": "60605" }, // 符合
  { "_id": 4, "name": "郊區大賣場", "zip": "60123" },    // 不符合
  { "_id": 5, "name": "洛杉磯分店", "zip": "90001" }     // 不符合
])
```


### find

```sql
db.stores.find()

{ "_id": 1, "name": "芝加哥咖啡廳", "zip": "60601" },
{ "_id": 2, "name": "風城書店", "zip": "60603" },
{ "_id": 3, "name": "千禧公園紀念品店", "zip": "60605" },
{ "_id": 4, "name": "郊區大賣場", "zip": "60123" },
{ "_id": 5, "name": "洛杉磯分店", "zip": "90001" }

```

### find_$in

注意 in 內是 []

```sql
db.stores.find({zip: { $in:["60601","60123"]}})

{ "_id": 1, "name": "芝加哥咖啡廳", "zip": "60601" }
{ "_id": 4, "name": "郊區大賣場", "zip": "60123" }

```


### find_$eq


```sql

db.stores.find({
  zip: {
    $eq: "90001"
  }
})

{ "_id": 5, "name": "洛杉磯分店", "zip": "90001" }

```

### find_$or


```sql
db.stores.find({
  $or : [
    {zip: "90001"},
    {zip: "60603"}
  ]
})


{ "_id": 2, "name": "風城書店", "zip": "60603" }
{ "_id": 5, "name": "洛杉磯分店", "zip": "90001" }

```


$eq 的右邊： 只能是一個具體的值（字串、數字、或一個精確的陣列/物件），絕對不能出現逗號分隔的多個純量。

$in 的右邊： 必須是一個中括號 [] 陣列，裡面放所有你允許匹配的複數條件。

$or 的右邊： 必須是一個中括號 [] 陣列，且陣列裡面包的必須是數個獨立且完整的 { 欄位: 條件 } 物件。



### find_$lt_$lte


```sql
db.sales.insertMany([
  { "_id": 1, "customer": { "name": "張三", "satisfaction": 1 } },
  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } },
  { "_id": 3, "customer": { "name": "王五", "satisfaction": 3 } },
  { "_id": 4, "customer": { "name": "趙六", "satisfaction": 5 } } 
])

```

```sql
db.sales.find({
  "customer.satisfaction":5
})

 { "_id": 4, "customer": { "name": "趙六", "satisfaction": 5 } } 
```



```sql
db.sales.find({
  "customer.satisfaction": {
    $lte:3
  }
})


  { "_id": 1, "customer": { "name": "張三", "satisfaction": 1 } }
  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } }
  { "_id": 3, "customer": { "name": "王五", "satisfaction": 3 } }
```

```sql

db.sales.find({
  "customer.satisfaction": {
    $lt:3
  }
})

  { "_id": 1, "customer": { "name": "張三", "satisfaction": 1 } }
  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } }

```



### find_$gte_$gt


```sql

db.sales.find({
  "customer.satisfaction": {
    $gt:1
  }
})


  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } }
  { "_id": 3, "customer": { "name": "王五", "satisfaction": 3 } }
  { "_id": 4, "customer": { "name": "趙六", "satisfaction": 5 } }


db.sales.find({
  "customer.satisfaction": {
    $gte:1
  }
})

  { "_id": 1, "customer": { "name": "張三", "satisfaction": 1 } }
  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } }
  { "_id": 3, "customer": { "name": "王五", "satisfaction": 3 } }
  { "_id": 4, "customer": { "name": "趙六", "satisfaction": 5 } }
```



### find_$gt_$lt


```sql


db.sales.find({
  "customer.satisfaction": {
    $gte:1, $lt:3
  }
})

  { "_id": 1, "customer": { "name": "張三", "satisfaction": 1 } }
  { "_id": 2, "customer": { "name": "李四", "satisfaction": 2 } }
```

### 錯誤示範: 不會有錯誤訊息
```sql
 db.sales.find({
  "customer.satisfaction": {
    $gte:3, $lt:1
  }
})
```

## Querying on Array Elements

```sql
db.books.insertMany([
  { "_id": 1, "title": "圖書 A", "genre": "Historical" },          // 狀況一：純字串（符合）
  { "_id": 2, "title": "圖書 B", "genre": ["Historical", "Sci-Fi"] },// 狀況二：陣列包含該元素（符合）
  { "_id": 3, "title": "圖書 C", "genre": ["Fantasy", "Romance"] }  // 狀況三：陣列不包含（不符合）
])
```


```sql
db.books.find({genre:"Historical"})

  { "_id": 1, "title": "圖書 A", "genre": "Historical" }
  { "_id": 2, "title": "圖書 B", "genre": ["Historical", "Sci-Fi"] }

```

### 完全查詢

```sql
db.books.find({  
    genre : ['Fantasy', 'Romance']  
})


{ "_id": 3, "title": "圖書 C", "genre": ["Fantasy", "Romance"] }

```

### 指定型態 $type
```sql
db.books.find({
    genre : {$eq: 'Historical', $type: "string"}
 })


  { "_id": 1, "title": "圖書 A", "genre": "Historical" }
  { "_id": 2, "title": "圖書 B", "genre": ["Historical", "Sci-Fi"] }



db.books.find({
    genre : {$eq: 'Historical', $type: "array"}
 })

  { "_id": 2, "title": "圖書 B", "genre": ["Historical", "Sci-Fi"] }

```

### $elemMatch


```sql

db.sales.insertMany([
  {
    "_id": 101,
    "order_no": "A01",
    "items": [
      { "name": "laptop", "price": 1000, "quantity": 2 }, // 滿足所有條件（符合）
      { "name": "mouse", "price": 20, "quantity": 5 }
    ]
  },
  {
    "_id": 102,
    "order_no": "A02",
    "items": [
      { "name": "laptop", "price": 500, "quantity": 1 }, // 價格太便宜（不符合）
      { "name": "phone", "price": 900, "quantity": 1 }   // 不是 laptop（不符合）
    ]
  }
])

```


```sql

db.sales.find({
  "items": {
    $elemMatch: {
      "name": "laptop"
    }
  }
})


  {
    "_id": 101,
    "order_no": "A01",
    "items": [
      { "name": "laptop", "price": 1000, "quantity": 2 }, // 滿足所有條件（符合）
      { "name": "mouse", "price": 20, "quantity": 5 }
    ]
  }
  {
    "_id": 102,
    "order_no": "A02",
    "items": [
      { "name": "laptop", "price": 500, "quantity": 1 }, // 價格太便宜（不符合）
      { "name": "phone", "price": 900, "quantity": 1 }   // 不是 laptop（不符合）
    ]
  }




db.sales.find({
  "items": {
    $elemMatch: {
      "name": "mouse"
    }
  }
})


  {
    "_id": 101,
    "order_no": "A01",
    "items": [
      { "name": "laptop", "price": 1000, "quantity": 2 }, // 滿足所有條件（符合）
      { "name": "mouse", "price": 20, "quantity": 5 }
    ]
  }



db.sales.find({
  "items": {
    $elemMatch: {
      "name": "mouse",
      "price": 900
    }
  }
})

- - - -




db.sales.find({
  "items": {
    $elemMatch: {
      "name": "mouse",
      "price": 20
    }
  }
})

  {
    "_id": 101,
    "order_no": "A01",
    "items": [
      { "name": "laptop", "price": 1000, "quantity": 2 }, // 滿足所有條件（符合）
      { "name": "mouse", "price": 20, "quantity": 5 }
    ]
  }

```


## 轉為陣列輸出

1. 

```sql

 db.test.find()
[
  { _id: ObjectId('6910d49e60eef558cb6c4bd6'),
 case1: 111 },
  { _id: ObjectId('691210d9edec7070aa6c4bd0'),
 case1: 1, value: 'abc' },
  { _id: ObjectId('69121101edec7070aa6c4bd1'),
 case1: 1111 },
  { _id: ObjectId('6912122d93360a6b996c4bd0'),
 case1: 123 },
  { _id: ObjectId('6912122d93360a6b996c4bd1'),
 case1: 234 }
]
```


2. toArray

```sql 

[
  { _id: ObjectId('6a3a465cd509d783cba36819'), case1: 111 },
  { _id: ObjectId('6a3a465cd509d783cba3681a'), case1: 1 },
  { _id: ObjectId('6a3a465cd509d783cba3681b'), case1: 1111 },
  { _id: ObjectId('6a3a465cd509d783cba3681c'), case1: 123 }
]
```


2. 將結果轉為陣列輸出第一筆資料中的特定欄位

```sql
db.books.find().toArray()[0]

{ _id: ObjectId('6a3a465cd509d783cba36819'), case1: 111 }

db.books.find().toArray()[2]

{ _id: ObjectId('6a3a465cd509d783cba3681b'), case1: 1111 }

db.books.find().toArray()[0]["case1"]

111
```



4. 顯示指定欄位

```

[
  { _id: ObjectId('6a3a465cd509d783cba36819'), case1: 111, count: 100 },
  { _id: ObjectId('6a3a465cd509d783cba3681a'), case1: 1, count: 55 },
  { _id: ObjectId('6a3a465cd509d783cba3681b'), case1: 1111, count: 55 },
  { _id: ObjectId('6a3a465cd509d783cba3681c'), case1: 123, count: 55 }
]

db.books.find({},{_id:0,case1:1}).toArray()

[ { case1: 111 }, { case1: 1 }, { case1: 1111 }, { case1: 123 } ]

db.books.find({},{_id:0,case1:1}).toArray()[2]

{ case1: 1111 }
```

### 查詢子類別

```js
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({'address.street':{$regex:"^123 Main St"}})
[
  {
    _id: ObjectId('6924634119543dba926c4bd0'),
    name: 'Alice',
    age: 30,
    isStudent: false,
    courses: [ 'Math', 'Science' ],
    address: { street: '123 Main St', city: 'Anytown' }
  }
]
```


5. 多重條件查詢

```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({name:'mark1',age:22})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd0'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd1'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW003'
  }
]
```

多重條件查詢 OR

OR後方需接上陣列    

```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({'$or':[{age:22},{work:'TW001'}]})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd0'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd1'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW003'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd2'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW001'
  }
]
```

多重條件查詢 AND

```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({'$and':[{age:22},{work:'TW001'}]})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd0'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW001'
  }
]
```

條件判斷式 $eq

```
db.test.find({name:{'$eq':'mark2'}})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd2'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd3'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW002'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd4'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW004'
  }
]
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({name:'mark2'})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd2'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd3'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW002'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd4'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW004'
  }
]
```
條件判斷式 $in

```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.find({ name:{'$in':['mark1','mark2']}})
[
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd0'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd1'),
    name: 'mark1',
    id: 'A123456789',
    age: 22,
    work: 'TW003'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd2'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW001'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd3'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW002'
  },
  {
    _id: ObjectId('6914c0d0e00d9ff0b66c4bd4'),
    name: 'mark2',
    id: 'A223456783',
    age: 13,
    work: 'TW004'
  }
]
```






















### find_$and_$or



```sql

db.inspections.insertMany([
  { 
    "_id": 1, 
    "name": "紐約烤肉餐車", 
    "sector": "Mobile Food Vendor - 881", 
    "address": { "city": "ASTORIA" }, 
    "result": "Pass" 
  }, // 完全符合 (回傳)
  { 
    "_id": 2, 
    "name": "美味漢堡店", 
    "sector": "Restaurant", 
    "address": { "city": "ASTORIA" }, 
    "result": "Pass" 
  }, // 行業別不符 (排除)
  { 
    "_id": 3, 
    "name": "皇后區炸雞餐車", 
    "sector": "Mobile Food Vendor - 881", 
    "address": { "city": "QUEENS" }, 
    "result": "Pass" 
  }  // 地區不符 (排除)
])


```





```sql

db.inspections.find({
  name: '皇后區炸雞餐車',
  sector: 'Mobile Food Vendor - 881',
})

等同於

db.inspections.find({
  $and: [
    { name: '皇后區炸雞餐車'},
    { sector: 'Mobile Food Vendor - 881'}
    ]
})


  { 
    "_id": 3, 
    "name": "皇后區炸雞餐車", 
    "sector": "Mobile Food Vendor - 881", 
    "address": { "city": "QUEENS" }, 
    "result": "Pass" 
  } 

```



```sql

db.routes.insertMany([
  {
    "_id": 201,
    "flight_no": "TK001",
    "src_airport": "JFK",
    "dst_airport": "IST", // 滿足群組一 (降落地是 IST)
    "stops": 0,           // 滿足群組二 (直飛)
    "airline": { "name": "Turkish Airlines" }
  }, // 完全符合 (回傳)
  {
    "_id": 202,
    "flight_no": "TK002",
    "src_airport": "IST", // 滿足群組一 (出發地是 IST)
    "stops": 1,
    "airline": { "name": "Turkish Airlines" } // 滿足群組二 (土航營運)
  }, // 完全符合 (回傳)
  {
    "_id": 203,
    "flight_no": "AA123",
    "src_airport": "LAX",
    "dst_airport": "ORD", // 不滿足群組一 (與 IST 無關)
    "stops": 0,           // 滿足群組二 (直飛)
    "airline": { "name": "American Airlines" }
  } // 不符合 (排除)
])

{
  acknowledged: true,
  insertedIds: {
    '0': 201,
    '1': 202,
    '2': 203
  }
}
```

```sql

db.routes.find({
  $and: [
    { $or: [{ dst_airport: "IST" }, { src_airport: "IST" }] },
    { $or: [{ stops: 0 }, { "airline.name": "Turkish Airlines" }] }
  ]
})

  {
    "_id": 201,
    "flight_no": "TK001",
    "src_airport": "JFK",
    "dst_airport": "IST", // 滿足群組一 (降落地是 IST)
    "stops": 0,           // 滿足群組二 (直飛)
    "airline": { "name": "Turkish Airlines" }
  }

  {
    "_id": 202,
    "flight_no": "TK002",
    "src_airport": "IST", // 滿足群組一 (出發地是 IST)
    "stops": 1,
    "airline": { "name": "Turkish Airlines" } // 滿足群組二 (土航營運)
  }

```






## CRUD_update


```sql
db.books.insertMany(
[
  {case1: 111 },
  {case1: 1 },
  {case1: 1111 },
  {case1: 123 },
  {case1: 234 }
])

{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('6a3a4584d509d783cba36814'),
    '1': ObjectId('6a3a4584d509d783cba36815'),
    '2': ObjectId('6a3a4584d509d783cba36816'),
    '3': ObjectId('6a3a4584d509d783cba36817'),
    '4': ObjectId('6a3a4584d509d783cba36818')
  }
}


```

### 1. update

欄位不存在則不會新增

```sql
 db.test.update({case1:1},{'$set': {case1:1,value:'abc'}})
DeprecationWarning: Collection.update() is deprecated. Use updateOne, updateMany, or bulkWrite.

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 0
}

 db.test.find()
[
  { _id: ObjectId('6910d49e60eef558cb6c4bd6'), case1: 111 },
  { _id: ObjectId('691210d9edec7070aa6c4bd0'), case1: 1 },
  { _id: ObjectId('69121101edec7070aa6c4bd1'), case1: 1111 },
  { _id: ObjectId('6912122d93360a6b996c4bd0'), case1: 123 },
  { _id: ObjectId('6912122d93360a6b996c4bd1'), case1: 234 }
]
```

### 2. updateOne

```sql
db.books.updateOne(
  { case1: 234},
  { $set: { case:234, count:1}}
)

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}


 { _id: ObjectId('6910d49e60eef558cb6c4bd6'), case1: 111 }
 { _id: ObjectId('6a3a465cd509d783cba3681d'),  case1: 234, case: 234, count: 1 }
 { _id: ObjectId('69121101edec7070aa6c4bd1'), case1: 1111 }
 { _id: ObjectId('6912122d93360a6b996c4bd0'), case1: 123 }
 { _id: ObjectId('6912122d93360a6b996c4bd1'), case1: 234 }


db.books.updateOne(
  {case1: 234},
  { $set: {case:234, count:5}}
)

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

{ _id: ObjectId('6910d49e60eef558cb6c4bd6'), case1: 111 }
{ _id: ObjectId('6a3a465cd509d783cba3681d'),   case1: 234,   case: 234,   count: 5 }
 { _id: ObjectId('69121101edec7070aa6c4bd1'), case1: 1111 }
 { _id: ObjectId('6912122d93360a6b996c4bd0'), case1: 123 }
 { _id: ObjectId('6912122d93360a6b996c4bd1'), case1: 234 }
```

### 3. upsert 

The upsert option creates a new document if no documents match the filtered criteria. Here's an example:

如果資料表不存在, 預設upsert為true


```sql
{
  _id: ObjectId('6a3a465cd509d783cba3681d'),
  case1: 234,
  case: 234,
  count: 5
}


db.books.updateOne(
  { case1: 234},
  { $set: {case:234, count1:6}},
  { upsert: true }
)

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

{
  _id: ObjectId('6a3a465cd509d783cba3681d'),
  case1: 234,
  case: 234,
  count: 5,
  count1: 6
}

```


### 4. collection: 預設不存在
```sql
db.books3.updateOne(
  { case1: 234},
  { $set: {case:234, count3:6}},
  { upsert: true }
)

{
  acknowledged: true,
  insertedId: ObjectId('6a3a49089ff2c2dab11fadc0'),
  matchedCount: 0,
  modifiedCount: 0,
  upsertedCount: 1
}

db.books3.find()

{
  _id: ObjectId('6a3a49089ff2c2dab11fadc0'),
  case1: 234,
  case: 234,
  count3: 6
}
```










3-2-8 計算查詢筆數

3-2-9 Distinct

```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.distinct('_id')
[
  ObjectId('6910d49e60eef558cb6c4bd6'),
  ObjectId('691210d9edec7070aa6c4bd0'),
  ObjectId('69121101edec7070aa6c4bd1'),
  ObjectId('6912122d93360a6b996c4bd0'),
  ObjectId('6912122d93360a6b996c4bd1'),
  ObjectId('6914c0d0e00d9ff0b66c4bd0'),
  ObjectId('6914c0d0e00d9ff0b66c4bd1'),
  ObjectId('6914c0d0e00d9ff0b66c4bd2'),
  ObjectId('6914c0d0e00d9ff0b66c4bd3'),
  ObjectId('6914c0d0e00d9ff0b66c4bd4'),
  ObjectId('691610d664376aa3136c4bd0'),
  ObjectId('691610d664376aa3136c4bd1')
]
```
Distinct 增加查詢條件
```
Atlas atlas-f9gk44-shard-0 [primary] test> db.test.distinct('id', { 'id': { '$regex': 'A2' } } )
[ 'A223456783' ]
```



### Aggregation 

## 依照page_id計算數量

前置

```js
db.pageview.insertMany(
[
{user_id:"user1", page_id:"A0001","time":"2015-08-13 00:27:59"},
{user_id:"user1", page_id:"A0001","time":"2015-08-12 00:27:59"},
{user_id:"user2", page_id:"A0001","time":"2015-08-11 00:27:59"},
{user_id:"user2", page_id:"A0002","time":"2015-08-12 00:27:59"},
{user_id:"user1", page_id:"A0002","time":"2015-09-11 00:27:59"}
]0



db.event.insertMany(
[
{user_id:"user1", event_id:"A0001","time":"2015-08-13 00:27:59"},
{user_id:"user1", event_id:"A0001","time":"2015-08-12 00:27:59"},
{user_id:"user2", event_id:"A0001","time":"2015-08-11 00:27:59"},
{user_id:"user2", event_id:"A0002","time":"2015-08-12 00:27:59"},
{user_id:"user1", event_id:"A0002","time":"2015-09-11 00:27:59"}
]
)

```

```js
group

{
  _id: "$page_id",
  count: {
    $sum: 1
  }
}

```

```js
Atlas atlas-f9gk44-shard-0 [primary] test> db.pageview.aggregate([ { $match: { page_id: "A0001" } }] )
[
  {
    _id: ObjectId('692c6df06b08816be26c4bd0'),
    user_id: 'user1',
    page_id: 'A0001',
    length: '15',
    time: '2015-08-13 00:27:59'
  },
  {
    _id: ObjectId('692c6df06b08816be26c4bd1'),
    user_id: 'user1',
    page_id: 'A0001',
    length: '25',
    time: '2015-08-12 00:27:59'
  },
  {
    _id: ObjectId('692c6df06b08816be26c4bd2'),
    user_id: 'user2',
    page_id: 'A0001',
    length: '35',
    time: '2015-08-11 00:27:59'
  }
]

--等同於
Atlas atlas-f9gk44-shard-0 [primary] test> db.pageview.find({page_id:"A0001"})
[
  {
    _id: ObjectId('692c6df06b08816be26c4bd0'),
    user_id: 'user1',
    page_id: 'A0001',
    length: '15',
    time: '2015-08-13 00:27:59'
  },
  {
    _id: ObjectId('692c6df06b08816be26c4bd1'),
    user_id: 'user1',
    page_id: 'A0001',
    length: '25',
    time: '2015-08-12 00:27:59'
  },
  {
    _id: ObjectId('692c6df06b08816be26c4bd2'),
    user_id: 'user2',
    page_id: 'A0001',
    length: '35',
    time: '2015-08-11 00:27:59'
  }
]

```


## 練習Update Array

```sql
db.orders.insertMany([
  {
    "order_no": "ORD-2026-001",
    "items": [
      { "prod_id": "A105", "qty": 2, "tags": ["electronics", "discounts"] },
    ]  }
]);


{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('6a3257ac1a36d673204607b8')
  }
}


db.orders.find()

{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 2,
      tags: [
        'electronics',
        'discounts'
      ]
    }
  ]
}



db.orders.updateOne({"items.prod_id":"A105"},{$set:{"items.$.qty":3}})

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}


db.orders.find()
{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [
        'electronics',
        'discounts'
      ]
    }
  ]
}


db.orders.updateOne({"items.prod_id":"A105"},{$push:{"home":"u"}})

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

// 使用 $push 搭配 "items.$"，代表更新剛剛「第一個匹配到的那件商品」的 tags


{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [
        'electronics',
        'discounts'
      ]
    }
  ],
  home: [
    'u'
  ]
}
```


## 移除

```sql


{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [
        'electronics',
        'discounts'
      ]
    }
  ],
  home: [
    'u'
  ]
}

db.orders.updateOne({	order_no: 'ORD-2026-001'},{$unset:{"home":""}})

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

db.orders.find()


{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [
        'electronics',
        'discounts'
      ]
    }
  ]
}


```

## 移除 qty: 3,
```
{

  _id: ObjectId('6a3257ac1a36d673204607b8'),

  order_no: 'ORD-2026-001',

  items: [

    {

      prod_id: 'A105',

      qty: 3,

      tags: [ 'electronics', 'discounts' ],

      "tees": "GOOD"  

    }

  ],

  home: [ 'u' ]

}

db.orders.updateOne(
  // 1. 查詢條件：先精確定位出是哪一筆訂單，以及 items 陣列中哪一個物件包含 prod_id: "A105"
  { "items.prod_id": "A105" }, 
  
  // 2. 更新動作：使用 $unset 移除該物件內部的 qty 欄位
  { $unset: { "items.$.qty": "" } }
);
```


## 移除electronics

```sql
{

  _id: ObjectId('6a3257ac1a36d673204607b8'),

  order_no: 'ORD-2026-001',

  items: [

    {

      prod_id: 'A105',

      qty: 3,

      tags: [ 'electronics', 'discounts' ],

      "tees": "GOOD"  

    }

  ],

  home: [ 'u' ]

}


db.orders.updateOne(
  // 1. 查詢條件：定位到 items 陣列中 prod_id 為 A105 的商品
  { "items.prod_id": "A105" }, 
  
  // 2. 更新動作：使用 $pull 深入該商品的 items.$.tags 陣列中，拔除 "electronics"
  { $pull: { "items.$.tags": "electronics" } }
);

```

### 🛠️ MongoDB 巢狀結構「刪除/移除」大絕招對比表

| 你的操作目標 | 核心操作符 | 語法關鍵範例 |
| :--- | :---: | :--- |
| **移除整個頂層欄位**<br>(如：移除整個 `home` 欄位) | **`$unset`** | `{ $unset: { "home": "" } }` |
| **移除巢狀物件內某欄位**<br>(如：移除商品內的 `qty` 欄位) | **`$unset` + `$`** | `{ $unset: { "items.$.qty": "" } }` |
| **移除純值陣列中的某個元素**<br>(如：移除 `tags` 裡的 `"electronics"`) | **`$pull` + `$`** | `{ $pull: { "items.$.tags": "electronics" } }` |
| **從物件陣列中剃除整個物件**<br>(如：直接把 A105 整件商品從訂單退掉) | **`$pull`** | `{ $pull: { "items": { "prod_id": "A105" } } }` |

```sql

db.orders.updateOne(
  { "items.prod_id": "A105" },
  { $set: { "items.$.tees": "GOOD" } } // 使用 $set 搭配位置操作符 $
);

{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [ 'electronics', 'discounts' ],
      "tees": "GOOD"  // 👈 欄位被「塞入」現有的物件內
    }
  ],
  home: [ 'u' ]
}

```
```sql
db.orders.updateOne(
  { "items.prod_id": "A105" },
  { $push: { "items": { "tees": "GOOD" } } } // 直接對整個陣列進行 $push
);


{
  _id: ObjectId('6a3257ac1a36d673204607b8'),
  order_no: 'ORD-2026-001',
  items: [
    {
      prod_id: 'A105',
      qty: 3,
      tags: [ 'electronics', 'discounts' ]
    },
    {
      "tees": "GOOD"  // 👈 變成陣列中的「第二個獨立元素」
    }
  ],
  home: [ 'u' ]
}
```



## Aggregate
---

```sql
db.products.insertMany([
  { "_id": 101, "name": "Pro Laptop", "category": "Electronics", "inventory": { "on_hand": 15, "warehouse": "A" }, "tags": ["premium", "work"], "price": 1200, "specs": { "ram": 16, "storage": 512 } },
  { "_id": 102, "name": "Gaming Mouse", "category": "Electronics", "inventory": { "on_hand": 0, "warehouse": "A" }, "tags": ["gaming", "sale"], "price": 80, "specs": { "ram": 0, "storage": 0 } },
  { "_id": 103, "name": "Java Guide Book", "category": "Books", "inventory": { "on_hand": 40, "warehouse": "B" }, "tags": ["education", "java"], "price": 45, "specs": { "pages": 500 } },
  { "_id": 104, "name": "Spring Boot In Action", "category": "Books", "inventory": { "on_hand": 12, "warehouse": "B" }, "tags": ["education", "java", "premium"], "price": 60, "specs": { "pages": 420 } },
  { "_id": 105, "name": "Wireless Headphones", "category": "Electronics", "inventory": { "on_hand": 8, "warehouse": "A" }, "tags": ["premium", "audio"], "price": 150, "specs": { "battery_life": 30 } }
]);
```

### 1. $match 與 $group 的商務指標計算

題目：請篩選出目前「仍有庫存（on_hand > 0）」的商品，並依據「分類（category）」分組，計算各分類的「平均價格（avgPrice）」與「商品總庫存量（totalStock）」。

```sql
db.products.aggregate([
  // 階段 1: 篩選庫存大於 0 的文件 (巢狀欄位使用點號路徑且必須加引號)
  {
    $match: { "inventory.on_hand": { $gt: 0 } }
  },
  // 階段 2: 依分類進行統計
  {
    $group: {
      _id: "$category",                             // 陷阱防範：這裡的 category 必須加 $ 符號
      avgPrice: { $avg: "$price" },                // 引用 price 欄位計算平均
      totalStock: { $sum: "$inventory.on_hand" }   // 引用巢狀欄位進行加總
    }
  }
]);


[
  { _id: 'Books', avgPrice: 52.5, totalStock: 52 },
  { _id: 'Electronics', avgPrice: 675, totalStock: 23 }
]

```

點號路徑提取（Dot Notation）：如何在 $match 中過濾巢狀物件屬性（如 inventory.on_hand）。

欄位引用符號 $：在 $group 的 _id 或者是聚合表達式（如 $sum, $avg）中，指向現有欄位時前方必須加上 $。這是考題中最常見的干擾項陷阱（例如故意給成 _id: "category"）。


### $project、$set 與資料重構

請重構輸出資料。要求只保留商品名稱（name）與價格（price），隱藏 _id，並追加一個新欄位「has_tag_java」，當商品標籤（tags）內包含 "java" 時為 true，否則為 false。

```sql

db.products.aggregate([
  // 階段 1: 先使用 $set 建立動態判斷欄位，保留原本資料結構
  {
    $set: {
      has_tag_java: { $in: ["java", "$tags"] } // 使用 $in 評估運算子檢查陣列中是否含有指定值
    }
  },
  // 階段 2: 使用 $project 進行最終的欄位外觀裁剪
  {
    $project: {
      _id: 0,            // 顯式隱藏預設會出現的 _id
      name: 1,           // 保留 name
      price: 1,          // 保留 price
      has_tag_java: 1    // 保留前一階段剛建好的新欄位
    }
  }
]);


Output:

[
  { name: 'Pro Laptop', price: 1200, has_tag_java: false },
  { name: 'Gaming Mouse', price: 80, has_tag_java: false },
  { name: 'Java Guide Book', price: 45, has_tag_java: true },
  { name: 'Spring Boot In Action', price: 60, has_tag_java: true },
  { name: 'Wireless Headphones', price: 150, has_tag_java: false }
]

```

出題委員考點拆解
$set 與 $project 的本質差異：$set（別名 $addFields）是在保留舊文件的基礎上追加/覆蓋欄位；而 $project 是重新塑形，未指定的頂層欄位（除了 _id）預設都會被隱藏。

布林值投影：在 $project 中，1 代表顯示，0 代表隱藏，這兩個數字絕對不能跟表達式（如條件判斷）混用。


### $sort、$limit 與分頁/排行效能最佳實踐
---

題目：請找出整個倉庫中，價格最昂貴（降冪排序）的前兩名商品，並且只要顯示其名稱與價格即可。


```sql
db.products.aggregate([
  // 階段 1: 依據價格從高到低進行降冪排序
  {
    $sort: { "price": -1 }
  },
  // 階段 2: 攔截前兩名（與 $sort 連用可觸發記憶體優化機制）
  {
    $limit: 2
  },
  // 階段 3: 清理不必要的欄位以便報表閱讀
  {
    $project: {
      _id: 0,
      name: 1,
      price: 1
    }
  }
]);

Output:

[
  { name: 'Pro Laptop', price: 1200 },
  { name: 'Wireless Headphones', price: 150 }
]
```

出題委員考點拆解
管線優化（Pipeline Optimization）：如果 $sort 與 $limit 緊鄰在一起，MongoDB 會啟動 Top-N 最佳化排序演算法，這時只會在記憶體中維護指定數量（N）的文件，極大釋放記憶體壓力。

排序規則：-1 代表降冪（從大到小）。



### $set 搭配邏輯條件（$cond 三元運算子）

題目：不影響原有的欄位結構，請為每件商品追加一個「pricing_level（價格分級）」欄位。規則是：如果價格（price）大於或等於 100 元，顯示 "Premium"；否則顯示 "Budget"。


```sql

db.products.insertMany([
  { "_id": 201, "name": "4K Monitor", "category": "Electronics", "price": 400, "on_sale": true, "reviews": [5, 4, 5, 3] },
  { "_id": 202, "name": "Mechanical Keyboard", "category": "Electronics", "price": 120, "on_sale": false, "reviews": [4, 4] },
  { "_id": 203, "name": "Python Cookbook", "category": "Books", "price": 50, "on_sale": true, "reviews": [5, 5, 4] },
  { "_id": 204, "name": "Desk Mat", "category": "Accessories", "price": 25, "on_sale": false, "reviews": [] },
  { "_id": 205, "name": "Ergonomic Chair", "category": "Furniture", "price": 350, "on_sale": true, "reviews": [4, 3, 2, 4] }
]);


```

```sql
db.products.aggregate([
  {
    $set: {
      "pricing_level": {
        $cond: {
          if: { $gte: ["$price", 100] }, // 條件：price >= 100
          then: "Premium",               // 成立
          else: "Budget"                 // 不成立
        }
      }
    }
  }
]);

output:

[
  { "_id": 201, "name": "4K Monitor", "category": "Electronics", "price": 400, "on_sale": true, "reviews": [5, 4, 5, 3], "pricing_level": "Premium" },
  { "_id": 204, "name": "Desk Mat", "category": "Accessories", "price": 25, "on_sale": false, "reviews": [], "pricing_level": "Budget" }
]

```

$cond 語法結構：它的格式是 { $cond: { if: <條件>, then: <成立的值>, else: <不成立的值> } }，或是陣列寫法 [if, then, else]。考試極愛考語法填空。

與現有欄位共存：使用 $set 可以在「完全不破壞原始資料（如 price, reviews）」的前提下，塞入一個動態計算出來的標籤。


### $project 進行數學計算與「無中生有」的欄位重構

題目：請產出一份乾淨的「評價報告」。要求隱藏 _id、把 name 改名為 product_name，並且建立兩個新欄位：「total_reviews（評價總筆數）」與「average_score（平均分數，若無評價則為 null）」。


```sql
db.products.aggregate([
  {
    $project: {
      "_id": 0,                                 // 隱藏 _id
      "product_name": "$name",                  // 改名：新欄位名稱對應舊欄位的值
      "total_reviews": { $size: "$reviews" },   // 計算陣列長度
      "average_score": { $avg: "$reviews" }    // 計算陣列平均值（若陣列為空，自動回傳 null）
    }
  }
]);

[
  { "product_name": "4K Monitor", "total_reviews": 4, "average_score": 4.25 },
  { "product_name": "Mechanical Keyboard", "total_reviews": 2, "average_score": 4 },
  { "product_name": "Python Cookbook", "total_reviews": 3, "average_score": 4.666666666666667 },
  { "product_name": "Desk Mat", "total_reviews": 0, "average_score": null },
  { "product_name": "Ergonomic Chair", "total_reviews": 4, "average_score": 3.25 }
]
```

出題委員考點拆解
陣列運算子 $size 與 $avg：如何即時對 Document 內部的陣列欄位（reviews）進行長度與平均值計算。

欄位強迫改名與隱藏：利用 $project 將本來的 name 改名為 product_name，並徹底隱藏不相關的 category 與 on_sale。

### $set 與 $project 終極混合（模擬最難電商折價題）


題目：

第一步（使用 $set）：計算「final_price（最終售價）」。如果商品是特價中（on_sale 為 true），最終售價為原價打八折（price * 0.8）；如果不是特價中，則維持原價。

第二步（使用 $project）：只輸出商品名稱、原價、最終售價，以及一個全新的「saved_amount（省下的金額）」。

```sql
db.products.aggregate([
  // 階段 1：先用 $set 算出售價，並暫存成一個新欄位 final_price
  {
    $set: {
      "final_price": {
        $cond: {
          if: { $eq: ["$on_sale", true] },
          then: { $multiply: ["$price", 0.8] }, // 特價打八折
          else: "$price"                        // 原價
        }
      }
    }
  },
  // 階段 2：有了 final_price 後，用 $project 計算省下的錢，並裁切外觀
  {
    $project: {
      "_id": 0,
      "name": 1,
      "original_price": "$price",
      "final_price": 1,
      "saved_amount": { $subtract: ["$price", "$final_price"] } // 原價減售價
    }
  }
]);


[
  { "name": "4K Monitor", "final_price": 320, "original_price": 400, "saved_amount": 80 },
  { "name": "Mechanical Keyboard", "final_price": 120, "original_price": 120, "saved_amount": 0 },
  { "name": "Python Cookbook", "final_price": 40, "original_price": 50, "saved_amount": 10 },
  { "name": "Desk Mat", "final_price": 25, "original_price": 25, "saved_amount": 0 },
  { "name": "Ergonomic Chair", "final_price": 280, "original_price": 350, "saved_amount": 70 }
]
```


### $set 搭配 $in 的組合技
---

題目：
管理階層希望重構輸出資料。要求最終結果只保留 name 欄位（隱藏 _id），並追加一個布林值新欄位 has_java。當 tags 陣列中包含 "java" 時，has_java 為 true；否則為 false。

```sql
db.items.insertMany([
  { "_id": 1, "name": "Book A", "tags": ["java", "spring"] },
  { "_id": 2, "name": "Book B", "tags": ["python"] },
  { "_id": 3, "name": "Book C", "tags": [] }
]);
`
```


```sql
db.items.aggregate([
  {
    $set: { "has_java": { $in: ["java", "$tags"] } }
  },
  {
    $project: { "_id": 0, "name": 1, "has_java": 1 }
  }
])


Output:
[
  { "name": "Book A", "has_java": true },
  { "name": "Book B", "has_java": false },
  { "name": "Book C", "has_java": false }
]

```





### $sortByCount

```sql
db.companies.insertMany([{
  _id: ObjectId("52cdef7c4bab8bd675297da4"),
  name: 'Powerset',
  category_code: 'search',
  founded_year: 2006
},
{
  _id: ObjectId("52cdef7c4bab8bd675297da5"),
  name: 'Technorati',
  category_code: 'advertising',
  founded_year: 2002
},
{
  _id: ObjectId("52cdef7c4bab8bd675297da7"),
  name: 'AddThis',
  category_code: 'advertising',
  founded_year: 2004
},
{
  _id: ObjectId("52cdef7c4bab8bd675297da8"),
  name: 'OpenX',
  category_code: 'advertising',
  founded_year: 2008
},
{
  _id: ObjectId("52cdef7c4bab8bd675297daa"),
  name: 'Sparter',
  category_code: 'games_video',
  founded_year: 2007
},
{
  _id: ObjectId("52cdef7c4bab8bd675297dac"),
  name: 'Veoh',
  category_code: 'games_video',
  founded_year: 2004
},
{
  _id: ObjectId("52cdef7c4bab8bd675297dae"),
  name: 'Thoof',
  category_code: 'web',
  founded_year: 2006
}])

{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('52cdef7c4bab8bd675297da4'),
    '1': ObjectId('52cdef7c4bab8bd675297da5'),
    '2': ObjectId('52cdef7c4bab8bd675297da7'),
    '3': ObjectId('52cdef7c4bab8bd675297da8'),
    '4': ObjectId('52cdef7c4bab8bd675297daa'),
    '5': ObjectId('52cdef7c4bab8bd675297dac'),
    '6': ObjectId('52cdef7c4bab8bd675297dae')
  }
}

## sortByCount 
db.companies.aggregate( [ { "$sortByCount": "$category_code" } ] )

{
  _id: 'advertising',
  count: 3
}

{
  _id: 'games_video',
  count: 2
}

{
  _id: 'search',
  count: 1
}

{
  _id: 'search',
  count: 1
}

```











markdown## 第一章：認識 Markdown

這是一段介紹。

##第二章：進階技巧

### 語法小技巧

# END
