


Lesson 1: Introduction to MongoDB Aggregation
Lesson 2: Using $match and $group Stages in a MongoDB Aggregation Pipeline

## Introduction to MongoDB Aggregation

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

db.users.aggregate([
  {
    $addFields: {
      defaultUsername: {
        $concat: [ "$first_name", "$last_name" ]
      }
    }
  }
])


QA

Which of the following tasks cannot be completed with an aggregation pipeline? (Select one.)

a.
Filtering for relevant pieces of data
b.
Finding data from outside sources
c.
Grouping documents
d.
Calculating total values from a field across many documents


=>B

a:You can filter for relevant pieces of data by using aggregation, but can you change the documents in the database?


b:You cannot use aggregation to find data from outside sources.


c:You can group documents together by using aggregation, but can you change those documents in the database?


d:You can calculate totals from a group of documents by using aggregation, but can you change those documents in the database?




Select an answer choice and then click "See Results" to submit.

Which command performs an aggregation operation by using an aggregation pipeline? (Select one.)

a.
group()
b.
filter()
c.
aggregation()
d.
aggregate()


=>d


a:group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.


b:group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.


c:group is a stage that can be used in an aggregation pipeline. It does not perform an aggregation operation.


d:aggregate() performs an aggregation operation by using an aggregation pipeline. aggregate() takes an array of aggregation stages to form the pipeline. An aggregation function is written as db.collection.aggregate().

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

a:
b:
c:
d:

