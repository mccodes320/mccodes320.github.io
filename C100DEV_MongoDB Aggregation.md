Lesson 1: Introduction to MongoDB Aggregation  
Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline  
Lesson 3: Using $sort and $limit Stages in a MongoDB Aggregation Pipeline  
Lesson 4: Using $project, $count, and $set Stages in a MongoDB Aggregation Pipeline  
Lesson 5: Using the $out Stage in a MongoDB Aggregation Pipeline  

# 考試註記
1. $match、$group、$sort 三者的順序 ESR, 即便pipleline順序不同系統還是會依照ESR執行




# Introduction to MongoDB Aggregation

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

### 題目 (Question)
---
Which of the following tasks cannot be completed with an aggregation pipeline? (Select one.)

a. Filtering for relevant pieces of data  
b. Finding data from outside sources  
c. Grouping documents  
d. Calculating total values from a field across many documents

**正確答案：B**



### 選項解析 (Explanation)

* **a. Filtering for relevant pieces of data**
  * You can filter for relevant pieces of data by using aggregation, but can you change the documents in the database?
* **b. Finding data from outside sources**
  * **(Correct)** You cannot use aggregation to find data from outside sources.
* **c. Grouping documents**
  * You can group documents together by using aggregation, but can you change those documents in the database?
* **d. Calculating total values from a field across many documents**
  * You can calculate totals from a group of documents by using aggregation, but can you change those documents in the database?




### 題目 (Question)
---
Which command performs an aggregation operation by using an aggregation pipeline? (Select one.)

a. group()
b. filter()
c. aggregation()
d. aggregate()

**正確答案：D**



### 選項解析 (Explanation)

* **a. group()**
  * group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.
* **b. filter()**
  * group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.
* **c. aggregation()**
  * group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.
* **d. aggregate()**
  * **(Correct)** aggregate() performs an aggregation operation by using an aggregation pipeline. aggregate() takes an array of aggregation stages to form the pipeline. An aggregation function is written as db.collection.aggregate().

# Highlight Zone Challenge: Introduction to MongoDB Aggregation

```SQL
db.collection.aggregate ([
  {
    $match : {
        { size : "small" }
    },
    ...
])
```

1. Select the aggregation method.  => db.collection.aggregate
2. Select the expression. => { size : "small" }
3. Select the aggregation stage. = > $match : {


---


## Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline
https://learn.mongodb.com/learn/course/mongodb-aggregation/lesson-2-using-match-and-group-stages-in-a-mongodb-aggregation-pipeline/learn?client=customer


QA  
You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:
```SQL
_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
   y: 31.458009
   x: 86.925771 
pop: 8556
state: "AL"
```
What will the output of this aggregation pipeline be? (Select one.)

```SQL
db.zips.aggregate([
{
   $match: { "state": "CA" }
},
{
   $group: { "_id": "$zip" }
}
])
```

a.
One document for each city located in California (CA)
b.
One document for each zip code located in California (CA)
c.
The total number of documents that contain a zip code in the state of California (CA)
d.
All documents that contain a zip where the state is California (CA)


==> B

a: In this example, the $match stage finds all documents where the state is California (CA). Next, the $group stage groups documents according to a group key. What is the group key in this example?


b:First, the $match stage finds all documents where the state is California (CA). Next, the $group stage groups documents according to a group key. The output of this stage is one document for each unique value of this group key. In this example, the output is one document for each zip code because the group key is the zip code.


c:There is another stage that shows the total number of documents in the aggregation pipeline.


d:The output of the $group stage is one document for each unique value of the group key. To return all documents that contain a zip code where the state is California (CA), you should use a find() query.








Instructions: Select an answer choice and then click "See Results" to submit.

You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:
```SQL
_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
   y: 31.458009
   x: 86.925771 
pop: 8556
state: "AL"
```
What will the output of this aggregation pipeline be? (Select one.)
```SQL
db.zips.aggregate([
{
   $match: { "state": "TX" }
},
{
   $group: { "_id": "$city" }
}
])
```
a.
One document for each city located in Texas (TX)
b.
One document containing all cities located in Texas (TX)
c.
One document for each state in United States except Texas (TX)
d.
All documents that contain a city where the state is Texas (TX)


==> A


a:First, the $match stage finds all documents where the state is Texas (TX). Next, the $group stage groups documents according to a group key. The output of this stage is one document for each unique value of this group key. In this example, the output is one document for each city because the group key is the city.


b:The output of the $group stage is one document for each unique value of the group key, not one document containing all unique values of the group key.


c:This is not how the $match and $group stages work in a MongoDB aggregation pipeline. In this pipeline, the $match stage finds all documents where the state is Texas (TX). Next, the $group stage groups documents according to a group key. The output of this stage is one document for each unique value of this group key. In this example, the output is one document for each city because the group key is the city.


d:The output of the $group stage is one document for each unique value of the group key. To return all documents that contain a city where the state is Texas (TX), you should use a find() query.




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




QA

You have a collection that contains zip codes in the United States. Here’s a sample document from this collection:

_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
   y: 31.458009
   x: 86.925771 
pop: 8556
state: "AL"
What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate([
{
   $group: { "_id": "$pop" }
},
{
   $sort: { _id: -1 }
}
])
a.
One document for the population of each zip code, sorted in random order
b.
10 documents for the population of each zip code
c.
One document for the population of each zip code, sorted in descending order
d.
One document for the population of each zip code, sorted in ascending order



== > C


Select an answer choice and then click "See Results" to submit. 

You have a collection that contains all the zip codes in the United States. Here’s a sample document from this collection:

_id: ObjectId('5c8eccc1caa187d17ca6eea2')
city: "EVERGREEN"
zip: "36401"
loc: Object
   y: 31.458009
   x: 86.925771 
pop: 8556
state: "AL"
What will the output of this aggregation pipeline be? (Select one.)

db.zips.aggregate([
{
   $group: { "_id": "$pop" }
},
{
   $sort: { _id: -1 }
},
{
   $limit: { 10 }
}
])
a.
All documents, each containing the population of a zip code as the _id, sorted by population in ascending order
b.
All documents, each containing the population of a zip code as the _id, sorted by population in descending order
c.
10 documents, each containing the population of a zip code as the _id, sorted by population in descending order
d.
10 documents, each containing the population of a zip code as the _id, sorted by population in ascending order

==> C



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




QA


What is the main difference between $set and $project? (Select one.)

a.
$set changes the values of fields. $project can show and hide fields, but it can't set values of fields.
b.
$set can only create new fields, and $project can only modify existing fields.
c.
$project, $set, and $addFields are all interchangable.
d.
$set is used to create or change values of new or existing fields. $project can be used to create or change the value of fields, but it can also be used to specify which fields to show in the documents in the aggregation pipeline.


=> D


a: $set and $project can both create new fields as well as set and change values.


b:$set and $project can both create new fields as well as set and change values.


c:$set and $project can both create new fields as well as set and change values.


d:$set and $project can both create and assign values to fields, but only $project can be used to reshape the data.





Select an answer choice and then click "See Results" to submit. 

What does the $count stage return? (Select one.)

a.
A single document with one field that contains the value set to the number of documents at this stage in the aggregation pipeline.
b.
The number of groups in an aggregation pipeline.
c.
A set number of documents from the aggregation pipeline.
d.
A single document that contains the number of fields that have been modified in an aggregation pipeline.


==> A




a:The $count stage returns a document with a field named in the parameters with the value set to the number of documents at this stage in the aggregation pipeline.


b:The $count stage can be used in an aggregation pipeline that does not include a $group stage.


c:You may be thinking of the $limit stage. The $count stage performs a different task.


d:The $count stage does not track fields that have been modified on its own.





## Lesson 5: Using the $out Stage in a MongoDB Aggregation Pipeline

$out

Writes the doucuments that are returned by an aggregation pipeline into a collection

Must be the last stage

Creates a new collection if it does not already exist

If collection exists, $out replaces the existing collection with new data

Store the aggregation output into a collection


```SQl
$out: {
    db: "<db>",
    coll: "<newcollection>"
}
```
<img width="781" height="432" alt="image" src="https://github.com/user-attachments/assets/e26d5849-3688-4ce1-a541-3e8ddce948bf" />

<img width="747" height="245" alt="image" src="https://github.com/user-attachments/assets/c6dedd28-7d8b-48f1-a343-5f5efa3a85a5" />

<img width="812" height="267" alt="image" src="https://github.com/user-attachments/assets/157ee01d-88d9-4adf-91e0-6fadffa6c9db" />


QA

What does the $out stage do? (Select one). 

a.
Removes documents from the aggregation pipeline.
b.
Returns all the documents currently in the aggregation pipeline as one large document.
c.
Creates a new collection that contains the documents in this stage of the aggregation pipeline.
d.
Removes the current user who is running the aggregation pipeline from the database.


==> C


Select an answer choice and then click "See Results" to submit.

What happens if you set the $out stage to output to a collection that already exists? (Select one.)

a.
The existing collection is erased and replaced with the outputted documents.
b.
A second collection with "_1" appended to the name is created.
c.
A new database with the specified name of the collection is created.
d.
An error is returned, and the existing collection is not modified.


==> A



a:Users must be careful not to overwrite any collections unintentionally when specifying the name of the collection to output the documents to.


b:The $out stage will not change the name of the collection to write to. Consider why it's important to be careful when specifying the name of the collection.


c:The $out stage writes to a new collection. It may be in a different database than that of the base collection, but it won't create a new database. Consider why it's important be careful when specifying the name of the collection.


d:This will not be flagged as an error. Consider why it's important be careful when specifying the name of the collection.













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

# Associate Developer Java Practice Quest


7. A collection has documents like the following:

 { _id: 1, name: 'Oatmeal Fruit Cake with Gummy Bears ', price: 11)},
 { _id: 2, name: 'Cheesecake Trifle with Chocolate Sprinkles ', price: 14)},
 { _id: 3, name: 'Pistachio Brownie with Walnuts ', price: 5},
 { _id: 4, name: 'Strawberry Ice Cream Cake with Butterscotch Syrup ', price: 3)}
How should the 'autocomplete' index be defined to look for matches at the beginning of a word on the name field?

(Choose 1)

Incorrect - 0 out of 1 correct answer chosen

a.
{  "mappings": { "dynamic": false, "fields": { "name": [   {  "type": "autocomplete", "tokenization": "regexCaptureGroup"} ]     } }}

b.
{  "mappings": { "dynamic": false, "fields": { "name": [   {  "type": "autocomplete", "tokenization": "edgeGram"} ]    } }}

c.
{  "mappings": { "dynamic": false, "fields": { "name": [   {  "type": "autocomplete", "tokenization": "nGram"} ]    } }}

d.
{  "mappings": { "dynamic": false, "fields": { "name": [   {  "type": "autocomplete", "tokenization": "matchNGram"}} ]     } }}


## $search 進行自動補全查詢

```
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

對應altrs

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









