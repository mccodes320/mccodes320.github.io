C100DEV-MongoDB CRUD Operations: Insert and Find Documents
---

| **章節** | **CRUD (51%)** |
| :--- | :--- |
| **2.1** | 給定一個需要將結構化文件插入資料庫的情境，辨識正確與錯誤格式的插入命令（insert commands）。 |
| **2.2** | 給定一個提供完整更新文件（未使用更新運算子）的更新情境，辨識其輸出結果與資料庫狀態的變化。 |
| **2.3** | 給定一個使用 `$set` 的更新情境，辨識其輸出結果與資料庫狀態的變化。 |
| **2.4** | 給定一個更新文件的清單情境以及當資料不存在時應插入的位置資訊，辨識應使用的 upsert 命令。 |
| **2.5** | 給定一個需要更新多份文件的清單情境，辨識正確的更新運算式（update expression）。 |
| **2.6** | 給定一個在執行 `findAndModify` 時同時有另一個操作並行運作的情境，辨識其輸出結果與資料庫狀態的變化。 |
| **2.7** | 給定一個需要從資料庫刪除文件的清單情境，辨識應使用的刪除運算式（delete expression）。 |
| **2.8** | 給定一個需要透過簡單等值條件（例如 `{x: 3}`）查找單一文件的清單情境，辨識應使用的運算式。 |
| **2.9** | 辨識符合「對陣列欄位進行等值條件查詢」的文件。 |
| **2.10** | 辨識符合包含「比較運算子（relational operators）」運算式的文件。 |
| **2.11** | 辨識符合包含 `$in` 運算式的文件。 |
| **2.12** | 辨識符合 `$elemMatch` 運算式的文件。 |
| **2.13** | 辨識符合包含多個「邏輯運算子（logical operators）」運算式的文件。 |
| **2.14** | 給定包含排序（sort）與數量限制（limit）的查詢，辨識正確的輸出結果。 |
| **2.15** | 在一組運算式中辨識出不正確的欄位投影（projection）。 |
| **2.16** | 辨識如何從游標（cursor）中取得所有結果。 |
| **2.17** | 辨識用於計算符合查詢條件之文件數量的運算式。 |
| **2.18** | 給定一個索引建立情境，辨識用於定義搜尋索引（search index）的正確命令。 |
| **2.19** | 給定一個應用情境，辨識正確的搜尋查詢（search query）。 |
| **2.20** | 給定使用 `$match` 與 `$group` 的聚合運算式（aggregation expression），辨識正確的輸出結果。 |
| **2.21** | 給定使用 `$lookup` 的聚合運算式（aggregation expression），辨識正確的輸出結果。 |
| **2.22** | 給定使用 `$out` 的聚合運算式（aggregation expression），辨識正確的輸出結果。 |
   
   
   
   
   
* [Lesson 1 : Inserting Documents in a MongoDB Collection](#lesson-1--inserting-documents-in-a-mongodb-collection)
* [Lesson 2 : Finding Documents in a MongoDB Collection](#lesson-2--finding-documents-in-a-mongodb-collection)
* [Lesson 3 : Finding Documents by Using Comparison Operators](#lesson-3--finding-documents-by-using-comparison-operators)
* [Lesson 4 : Querying on Array Elements in MongoDB](#lesson-4--querying-on-array-elements-in-mongodb)
* [Lesson 5 : Updating Documents in a MongoDB Collection](#lesson-5--updating-documents-in-a-mongodb-collection)
* [Lesson 6 : Deleting Documents from a MongoDB Collection](#lesson-6--deleting-documents-from-a-mongodb-collection)

* [Lesson 1: Replacing a Document in MongoDB](#lesson-1-replacing-a-document-in-mongodb)   
* [Lesson 2: Updating MongoDB Documents by Using updateOne()](#lesson-2-updating-mongodb-documents-by-using-updateone)

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




# Lesson 1 : Inserting Documents in a MongoDB Collection

* inesertOne() and insertMany()
* collection不存在, 就會自動創建
* insertMany 新增是Arrary, 要用[]

```sql
db.<collection>.iesertOne() 
db.<collection>.iesertMany([<doc1>,<doc2>])
```

## Insert a Single Document  

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




### ordered


| 參數設定 | 寫入資料筆數 | 資料庫最終內容 | 說明 |
| :--- | :--- | :--- | :--- |
| **`ordered: false`** | **3 筆成功**，1 筆失敗 | `_id: 1, 2(notebook), 3` | 第 3 筆失敗後**繼續往下**，第 4 筆 `_id: 3` 成功寫入。 |
| **`ordered: true`**<br>*(預設行為)* | **2 筆成功**，2 筆未執行 | `_id: 1, 2(notebook)` | 第 3 筆失敗後**立刻中斷**，第 4 筆直接被跳過未處理。 |

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

為了配合上述查詢運算子的所有範例指令（統一針對 **`sales`** 集合），以下提供可建立並寫入這些測試資料的 `insertMany()` 指令，以及更新後的比較與邏輯運算子對照表。

---

### 測試資料準備 (MongoDB Shell Insert)

```javascript
db.sales.insertMany([
  {
    _id: 1,
    customer: { age: 70 },
    items: [
      { name: "A", price: 60 },
      { name: "C", price: 30 }
    ]
  },
  {
    _id: 2,
    customer: { age: 30 },
    items: [
      { name: "B", price: 50 }
    ]
  },
  {
    _id: 3,
    customer: { age: 25 },
    items: [
      { name: "D", price: 10 }
    ]
  }
])

```


### 比較運算子 (Comparison Operators)

| 運算子 | 中文含意 | 範例指令 |
| --- | --- | --- |
| **`$eq`** | 等於 | `db.sales.find({ "items.price": { $eq: 50 } })` |
| **`$ne`** | 不等於 | `db.sales.find({ "items.price": { $ne: 50 } })` |
| **`$gt`** | 大於 | `db.sales.find({ "items.price": { $gt: 50 } })` |
| **`$gte`** | 大於或等於 | `db.sales.find({ "customer.age": { $gte: 65 } })` |
| **`$lt`** | 小於 | `db.sales.find({ "items.price": { $lt: 50 } })` |
| **`$lte`** | 小於或等於 | `db.sales.find({ "customer.age": { $lte: 65 } })` |
| **`$in`** | 包含於 | `db.sales.find({ "items.name": { $in: ["A", "B"] } })` |
| **`$nin`** | 不包含於 | `db.sales.find({ "items.name": { $nin: ["A", "B"] } })` |

---

### 邏輯運算子 (Logical Operators)

| 運算子 | 中文含意 | 範例指令 |
| --- | --- | --- |
| **`$or`** | 或 (至少一個成立) | `db.sales.find({ $or: [{ "items.price": { $gt: 50 } }, { "customer.age": { $gte: 65 } }] })` |
| **`$and`** | 且 (全部成立) | `db.sales.find({ $and: [{ "items.price": { $gt: 50 } }, { "customer.age": { $gte: 65 } }] })` |
| **`$not`** | 非 (條件反轉) | `db.sales.find({ "items.price": { $not: { $gt: 50 } } })` |
| **`$nor`** | 皆非 (全部不成立) | `db.sales.find({ $nor: [{ "items.price": { $gt: 50 } }, { "customer.age": { $gte: 65 } }] })` |





# Lesson 3: Finding Documents by Using Comparison Operators

### MongoDB 比較運算子 (Comparison Operators)

| 比較運算子 | 英文全稱 | 中文含意 | 範例指令 |
| :--- | :--- | :--- | :--- |
| **`$gt`** | Greater Than | 大於 | `db.sales.find({ "items.price": { $gt: 50 } })` |
| **`$gte`** | Greater Than or Equal to | 大於或等於 | `db.sales.find({ "customer.age": { $gte: 65 } })` |
| **`$lt`** | Less Than | 小於 | `db.sales.find({ "items.price": { $lt: 50 } })` |
| **`$lte`** | Less Than or Equal to | 小於或等於 | `db.sales.find({ "customer.age": { $lte: 65 } })` |

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

# Create Collection

```sql
db.createCollection('eventLogs', { 
  capped: true, 
  size: 10000000, 
  max: 1000 
})
```

1. capped: true：聲明此集合為「上限集合」。預設為 false（一般集合）。   
2. size: 10000000 (必填)：指定此集合佔用的最大容量上限（以 Byte 為單位）。   
   例如 10000000 Bytes = 10 MB。
3. max: 1000 (選填)：指定此集合能儲存的最大文件數量上限（本例為最多 1000 筆）。
4. 先進先出 (FIFO - First In, First Out)：     
   當集合空間滿了（達到 size 上限）或是筆數滿了（達到 max 上限）時，最舊的文件會自動被刪除，替新插入的文件騰出空間   
5. 自然插入順序 (Natural Order): 資料會嚴格按照寫入的時間順序進行儲存，查詢時預設就會依照插入順序回傳   














# 2.20 

# Introduction to MongoDB Aggregation



Lesson 1: Introduction to MongoDB Aggregation  
Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline  
Lesson 3: Using $sort and $limit Stages in a MongoDB Aggregation Pipeline  
Lesson 4: Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline  
Lesson 5: Using the $out Stage in a MongoDB Aggregation Pipeline  
# $facet

# 考試註記
1. $match、$group、$sort, $project 的順序 ESR, 即便pipleline順序不同系統還是會依照ESR執行

* Aggregation
An analysis and summary of data

* Stage

An aggregation operation performed on the data

* Aggregation Pipeline

A series of stages completed one at a time, in order

Pipeline由多個階段組成
1. Filtered
2. Sorted
3. Grouped
4. Transformed 轉換

* Structure of an Aggregation Pipeline

```sql
db.collection.aggregate([
  { $state_name: { <expression> },
  { $state_name: { <expression> }
])
```

* Stage

$match: Filters for data that matches criteria

$group: Groups documents based on criteria

$sort: Puts the documents in a specified order


使用 $addFields 階段（保留原本所有欄位，並加上新欄位）
這是最常見的用法。當你想從資料庫撈出資料，並在不修改資料庫原始檔案的情況下，動態產生一個 defaultUsername 給前端使用：

```SQL
db.users.aggregate([
  {
    $addFields: {
      defaultUsername: {
        $concat: [ "$first_name", "$last_name" ]
      }
    }
  }
])
```

## Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline
https://learn.mongodb.com/learn/course/mongodb-aggregation/lesson-2-using-match-and-group-stages-in-a-mongodb-aggregation-pipeline/learn?client=customer

Lesson 3: Using $sort and $limit Stages in a MongoDB Aggregation Pipeline

$sort: 
Sorts all input documents and passes them through pipeline in sorted order

1: Ascending order 
-1 : Descending order


$limit

Limits the number of documents that are passed on to the next aggregation stage



Order of stages matters!



## Learn

Using $sort and $limit Stages in a MongoDB Aggregation Pipeline
Review the following sections, which show the code for the $sort and $limit aggregation stages.

$sort
The $sort stage sorts all input documents and returns them to the pipeline in sorted order. We use 1 to represent ascending order, and -1 to represent descending order.

{
    $sort: {
        "field_name": 1
    }
}


$limit
The $limit stage returns only a specified number of records.

{
  $limit: 5
}


$sort and $limit in an Aggregation Pipeline
The following aggregation pipeline sorts the documents in descending order, so the documents with the greatest pop value appear first, and limits the output to only the first five documents after sorting.

db.zips.aggregate([
{
  $sort: {
    pop: -1
  }
},
{
  $limit:  5
}
])

## Lesson 4: Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline

### $project: 
* Determines output shape 決定最後顯示的形狀
* Projection similar to find() operations
* Should be the last stage to format the output

```SQL
$project : {
  <field> : 1,
  <field> : <value>,
...
  <field> : <new value>
}
```

<value> : 
1 to include
0 to exclude 
ne wvalue specified for new fields and existing fields being given a new value


### $set

添加或修改自斷名稱與值
Adds or modifies fields in the  pipeline  
Useful when we want to change existing fields in pipeline or add new ones to be used in upcoming pipeline stages  

```SQL
$set : {
  <field> : <value>,
  <field> : <value>,
...
  <field> : <new value>
}
```

例如: 新增一個字段 pop_2022, 
根據每個郵政編碼中的平均人口增長顯示下一年度的預計人口.
目前美國人口增加率(2021)是.31%.
所以 1.0031 * 原始人口數在四捨五入, 

```SQL
db.zips.aggregate([
  { $set : { 
              pop_2022 : { $round: { $multoply: [1.0031, "$pop"] }}
           }
      }
  ])
```

<img width="616" height="733" alt="image" src="https://github.com/user-attachments/assets/6d699449-a6a9-4bac-8480-61affd308b0c" />



$count

Counts dicument int the pipleline  
Returns the total document count.  



```SQL
$count : <field_name> 
```




## Learn 

Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline
Review the following sections, which show the code for the $project, $set, and $count aggregation stages.

$project
The $project stage specifies the fields of the output documents. 1 means that the field should be included, and 0 means that the field should be supressed. The field can also be assigned a new value.

{
    $project: {
        state:1, 
        zip:1,
        population:"$pop",
        _id:0
    }
}
$set
The $set stage creates new fields or changes the value of existing fields, and then outputs the documents with the new fields.

{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
$count
The $count stage creates a new document, with the number of documents at that stage in the aggregation pipeline assigned to the specified field name.

{
  $count: "total_zips"
}


### Learn

MongoDB Aggregation
In this unit, you learned how to use aggregation in MongoDB and create an aggregation pipeline. You also learned how to use several of the most common aggregation stages, including:



| Stage      | 用途說明              |
| ---------- | ----------------- |
| `$match`   | 篩選條件（類似 find）     |
| `$group`   | 分組並進行聚合（如總數、平均）   |
| `$project` | 選擇/修改欄位內容（可改欄位名稱） |
| `$sort`    | 排序                |
| `$limit`   | 取前 N 筆資料          |
| `$skip`    | 跳過前 N 筆資料         |
| `$unwind`  | 展開陣列欄位，每個元素變成一筆文件 |
| `$lookup`  | 關聯查詢（類似 SQL JOIN） |


Resources
Use the following resources to learn more about inserting and finding documents in MongoDB:

Lesson 01: Introduction to MongoDB Aggregation
MongoDB Docs: Aggregation Operations

MongoDB Docs: Aggregation Pipelines

Lesson 02: Using $match and $group Stages in a MongoDB Aggregation Pipeline
MongoDB Docs: $match

MongoDB Docs: $group

Lesson 03: Using $sort and $limit Stages in a MongoDB Aggregation Pipeline
MongoDB Docs: $sort

MongoDB Docs: $limit

Lesson 04: Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline
MongoDB Docs: $project

MongoDB Docs: $count

MongoDB Docs: $set

Lesson 05: Using $out Stage in a MongoDB Aggregation Pipeline
MongoDB Docs: $out


a:
b:
c:
d:



## $search 進行自動補全查詢

```sql
db.user.createSearchIndex(
  "default", // 索引名稱
  {
    mappings: {
      dynamic: false,
      fields: {
        name: [
          {
            type: "autocomplete",
            tokenization: "edgeGram",
            minGrams: 2,
            maxGrams: 15
          }
        ]
      }
    }
  }
)

註： minGrams: 2 代表使用者至少要輸入 2 個字母（例如 "Ch"）才會觸發自動補全提示，這能大幅提升搜尋效能與精準度。

```

對應MongoDB Atlas

``` json
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "name": [
        {
          "type": "autocomplete",
          "tokenization": "edgeGram",
          "minGrams": 2,
          "maxGrams": 15
        }
      ]
    }
  }
}
```


搜尋 "Ch" （預期命中 Cheesecake、Chocolate）

```sql
db.user.aggregate([
  {
    $search: {
      "index": "default",
      "autocomplete": {
        "query": "Ch",
        "path": "name"
      }
    }
  },
  { $project: { _id: 1, name: 1, price: 1 } }
])
```

1. index: "default"
含義：指定要使用的 Search Index（搜尋索引）名稱。

說明：你在上一階段透過 createSearchIndex 建立的索引名稱是 "default"。如果你建立了多個不同的搜尋索引（例如一個給商品名稱用、一個給文章內文用），你就必須在這裡填入對應的索引名稱，告訴引擎你要用哪一套規則來搜。

2. autocomplete: { ... }
含義：指定要使用的 搜尋運算子（Operator）。

說明：$search 支援很多種搜尋方式（例如：精準字串搜尋 text、模糊搜尋 phrase、數值範圍 near 等）。這裡使用 autocomplete，代表你要啟動「自動補全 / 預測字詞」的搜尋模式。它會配合你索引中設定的 edgeGram，去比對單字的開頭。

3. query: "Ch"
含義：使用者實際輸入的搜尋關鍵字（Search Term）。

說明：這通常是前端搜尋框即時傳進來的字串。在這個範例中，代表使用者輸入了 "Ch"，搜尋引擎會拿這個 "Ch" 去字庫裡比對哪些單字的開頭符合 C-h。

4. path: "name"
含義：指定要在哪一個資料欄位（Field）進行搜尋。

說明：告訴引擎你要在文件的哪裡找 "Ch"。這裡填寫 "name"，表示要在 name（甜點名稱）這個欄位裡尋找。

補充：如果你的搜尋索引同時開放了多個欄位（例如 name 和 description），你也可以用陣列形式傳入多個路徑，例如："path": ["name", "description"]。



  
## $out  將聚合產生的結果直接寫入到指定的集合（Collection）中


10. A collection coll in database mdb has the following documents :

{_id: 1, type: "A", value: 60}
{_id: 2, type: "B", value: 80}
{_id: 3, type: "C", value: 10}
After executing the following aggregation pipeline:

db.getSiblingDB("mdb").coll.aggregate([
    { $out: {db:'test', collection:'results'}} ])
What are two expected results?

a.
Collection `results` is created in database `test`.
b.
There is a syntax error command. Collection `results` is not created.
c.
No documents in collection `coll` are written to collection `results`.
d.
All documents in collection `coll` are written to collection `results`.


==> Bc

```sql
{ $out: { db: "<目標資料庫>", collection: "<目標集合>" } }
```

```sql
db.getSiblingDB("mdb").coll.aggregate([
  { $out: { db: 'test', collection: 'results' } }
])
```
1. db.getSiblingDB("mdb").coll：代表切換到 mdb 資料庫，並對 coll 集合執行聚合。

2. $out: {db:'test', collection:'results'}：這是完全正確的跨資料庫語法。它會指示 MongoDB 在 test 資料庫中建立一個名為 results 的新集合。

3. 資料複製行為：因為 $out 之前沒有任何過濾（如 $match）或修改階段，所以 coll 集合內部的所有文件（All documents）都會被原封不動地輸出並寫入到 test.results 中。




# $facet


MongoDB 聚合框架中的 `$facet` 階段專門用於在一次資料遍歷中實現多維度分析。當文件進入 `$facet` 階段時，每個已定義的子管道都會獨立接收同一組完整的輸入文件－子管道之間不存在共用、過濾或排序。每個子管道都會在其自身的輸入副本上運行一系列聚合階段（例如 `$group`、`$sort`、`$count` 等）。 `$facet` 階段的最終輸出始終是一個單獨的文檔，其中每個鍵都與子管道的名稱相匹配，其值是一個包含該子管道結果的數組。這種設計對於電子商務分面搜尋等場景特別有用，在這些場景中，您可能需要同時計算按類別、價格範圍和可用性狀態劃分的計數，而無需多次掃描集合。需要記住的一個重要限制是，$facet 子管道本身不能包含某些階段，這些階段會以影響記憶體限制的方式改變文件的數量，例如 $facet 嵌套在 $facet 中，或者像 $out 和 $merge 這樣的階段。  

```json

{ 
  $facet : { 
    "按類別" : [ 
      { $group : { _id : "$category" , count : { $sum : 1 } } }       
    ],
    “cityStatus” ：[ 
      { $group : { _id : "$status" , total : { $sum : "$amount" } } },       
      { $sort : { total : - 1 } }    
    ],
    「概述」：[ 
      { $count : "totalDocuments" }  
    ]
  }
}

```

最終產生一個類似這樣的文件：

```json
{
  "按類別" : [ 
    { "_id" : "電子產品" , "count" : 42 },     
    { "_id" : "服裝" , "count" : 17 }     
  ],
  “cityStatus” ：[ 
    { "_id" : "active" , "total" : 9500 },     
    { "_id" : "inactive" , "total" : 2100 }     
  ],
  「概述」：[ 
    { "totalDocuments" : 59 }   
  ]
}
```






















