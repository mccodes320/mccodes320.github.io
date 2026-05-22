
MongoDB Data Modeling Intro


* Lession 1: Introduction to Data Modeling
* Lession 2: Types of Data Relationships


ref: https://learn.mongodb.com/learn/course/introduction-to-mongodb-data-modeling/lesson-1-introduction-to-data-modeling/learn?page=1













* Lession 1: Introduction to Data Modeling

先提問
What dose my applicayion do?
Whay data will I store ?
How will users access this data?
Whaat data will be most valuable to me?
再用問題描述需要處裡的任務與用戶想法
Your tasks as well as those of users
what your data looks like
The relationships among the data
The tooling you plan to hava
The access patterns that might emerge

建立好的data mode好處:
Make it easier to mange data
Make queries more efficient
Use less memory and CPU
Reduce costs

原則: 一起訪問的數據, 儲存再一起
* Data that is accessed together should be stored together.*

---



* Lesson 2: Types of Data Relationships


## One-to-one

A relationship where a data entity in one set is connected to exactly one data entity in another set

ex. 電影與導演

<img width="1301" height="544" alt="image" src="https://github.com/user-attachments/assets/d0ecfcc6-2183-4b00-8270-b4de91701f37" />


## One-to-many

A relationship where a data entity in one set is connected to any number of data entities in another set

ex. 電影與演員

<img width="1759" height="766" alt="image" src="https://github.com/user-attachments/assets/bbc0bcc4-fb87-4ebf-8a23-e905a9f4c4e3" />

可以用一個查詢得到所有所需要的數據

## Many-to-many

A relationship where any number of data entities in one set are connected to any number of data entities in another set


建模有主要兩種方式
## Embedding
We take related data and insert it into our document

## Referencing
We refer to documents in another collection in our document


<img width="1263" height="421" alt="image" src="https://github.com/user-attachments/assets/f0535cc5-6179-4da7-a812-e691beb1b064" />

<img width="910" height="469" alt="image" src="https://github.com/user-attachments/assets/c62e3198-e51a-4bd8-b6ca-6b5db105fa62" />





