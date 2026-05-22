# Lesson 1: Overview and the Document Model


https://learn.mongodb.com/learn/course/mongodb-and-the-document-model-2025-12-31/lesson-1-overview-of-the-document-model/learn?client=customer


## 本章重點
1. MongoDB 架構階層定義 
2. Document Structure
3. JSON vs. BSON
4. Polymorphic Data
5. Limits
6. 考點補充延伸 (Certification Objectives)

---

## 一、 MongoDB 架構階層定義 

### 1. 樹狀階層關係
* Cluster (集群)
    * └── Node (節點)
        * └── Database (資料庫)
            * └── Collection (集合)
                * └── Document (文件) ※ MongoDB 的最小數據單位

### 2. 各階層詳細定義

| 階層名稱 | 中文翻譯 | 關聯式資料庫對應 (RDBMS) | 詳細說明 |
| :--- | :--- | :--- | :--- |
| **Cluster** | 集群 | 伺服器群組 / 分散式架構 | **最上層架構。** 由多個 Node（節點）組成的伺服器群組。主要用於實現資料的高可用性（Replica Set）或水平擴充（Sharding）。 |
| **Node** | 節點 | 單一資料庫伺服器 | **硬體與實例層。** 指的是集群中的單一伺服器或單一 MongoDB 執行實例（Instance），負責實際的資料存取與運算。 |
| **Database** | 資料庫 | Database (資料庫) | **邏輯儲存層。** 一個節點內可以包含多個獨立的資料庫。每個資料庫擁有自己獨立的儲存檔案與權限控制。 |
| **Collection** | 集合 | Table (資料表) | **資料分組層。** 一個資料庫內包含多個集合。集合用來存放結構相似的 Document，且在 MongoDB 中不需要預先定義綱要（Schema-less）。 |
| **Document** | 文件 | Row / Record (資料列/記錄) | **基礎資料儲存單位。** 位於金字塔最底層。MongoDB 使用 BSON（二進位 JSON）格式來儲存具體的資料欄位與內容，是實際儲存資料的地方。 |

---

## 二、 Document Structure (文件結構)

Document 由 Field-Value pairs (欄位-值對) 組成。

### Field 命名規範：
* 型態為 String。
* 必須具備唯一性 (Unique)。
* 必須具備描述性 (Descriptive)。例如：描述姓名應使用 `name`，而不該簡寫為 `na`。

### Document 範例：
```json
{
  _id: ObjectId('573a1395f29313caabce2360'),
  plot: "Investigating a gold magnate's smuggling, James Bond uncovers a plot to contaminate the Fort Knox gold reserve.",
  genres: [ 'Action', 'Adventure', 'Thriller' ],
  runtime: 110,
  rated: 'APPROVED',
  cast: [ 'Sean Connery', 'Honor Blackman', 'Gert Frèbe', 'Shirley Eaton' ],
  title: 'Goldfinger',
  languages: [ 'English', 'Chinese', 'Spanish' ],
  released: ISODate('1965-01-09T00:00:00.000Z'),
  directors: [ 'Guy Hamilton' ],
  writers: [ 'Richard Maibaum (screenplay)', 'Paul Dehn (screenplay)' ],
  lastupdated: '2015-09-06 00:04:52.777000000',
  year: 1964,
  imdb: { rating: 7.8, votes: 128247, id: 58150 },
  countries: [ 'UK' ],
  type: 'movie'
}
```

## 三、 JSON vs. BSON

JSON
基於文字格式
```json
{ "hello" : "world" }
```
BSON (Binary JSON)
用於 MongoDB 內部儲存與網路傳輸的二進位格式。

提供更高的解析效能與更多延伸資料型態（如 ObjectId, Date）。
```json
\x16\x00\x00\x00
\x02hello\x00
```

### BSON 與 JSON 支援型態對比表


### 一、 JSON 型態表列與規範

| 型態名稱 | JSON 空間與格式規範 | BSON 對應型態 |
| :--- | :--- | :--- |
| **String** | UTF-8 編碼字串，帶雙引號 | String |
| **Number** | 雙精度浮點數（JSON 唯一的數字型態） | Double (預設), Int32, Int64, Decimal128 |
| **Object** | 經由 `{}` 包裹的無序 Key-Value 對 | Object (Embedded Document) |
| **Array** | 經由 `[]` 包裹的有序值列表 | Array |
| **true** | 字面值 `true`（1 byte 文字） | Boolean (0x01) |
| **false** | 字面值 `false`（1 byte 文字） | Boolean (0x00) |
| **null** | 字面值 `null` | Null |

---

### 二、 BSON 型態表列與規範

| 型態名稱 | BSON 空間與格式規範 | JSON 是否原生支援？ |
| :--- | :--- | :---: |
| **String** | UTF-8 編碼字串，底層帶長度前綴與結束符 |  |
| **Double** | 64-bit (8 bytes) IEEE 754 浮點數 |  |
| **Boolean** | 1 byte (0x00 或 0x01) |  |
| **Null** | 1 byte 固定標記，代表空值 |  |
| **Array** | 內嵌的文件結構，Key 為字串化的索引 (如 "0", "1") |  |
| **Object** | 內嵌的標準 BSON 文件，最大深度限制為 100 層 |  |
| **Integer** | 分為 32-bit (4 bytes) 與 64-bit (8 bytes) 強型別整數 | ⚠️ (僅能對應 Number) |
| **ObjectId** | 12-byte 固定長度二進位碼 | ❌ |
| **Date** | 64-bit (8 bytes) 整數，儲存 Unix 紀元起算之毫秒數 | ❌ |
| **Timestamp** | 64-bit (8 bytes)，前 4 bytes 為秒數，後 4 bytes 為計數器 | ❌ |
| **Decimal128** | 128-bit (16 bytes) 高精度浮點數 | ❌ |
| **Binary data** | 自訂長度的二進位位元組陣列，帶有 1 byte 的子型態標記 | ❌ |
| **Regular Expression** | 包含樣式（Pattern）與修飾符（Options）的連續字串結構 | ❌ |
| **JavaScript** | 程式碼字串，可選擇性內嵌 Scope 執行環境文件 | ❌ |



## 四、 Polymorphic data(多態數據)

MongoDB 具備 Schema-less 特性，可在同一個 Collection 中處理具備不同 Shapes 與 Types 的多態數據。

Document 的 Key-Value pairs 支援多種資料類型：
* Text (文字)
* Geospatial data (地理空間數據)
* Time-Series (時間序列)
* Graph data (圖形數據)


## 五、 Limits (限制)

為確保資料擷取效率並優化記憶體與頻寬的管理，MongoDB 在維護靈活性的同時設有以下硬性限制：

* **Maximum Document Size:** 單一文檔最大限制為 **16 MB**。
    * **目的：** 優化記憶體使用與網路頻寬（bandwidth），防止大物件占用過多資源導致效能瓶頸。若有大檔案需求應使用 GridFS。
* **Maximum Nesting Levels:** 巢狀結構最大深度限制為 **100 層**。


---

## 六、 考點補充延伸 (Certification Objectives)

### 1.1 Identify the set of value types MongoDB BSON supports
相較於 JSON 僅支援 String, Number, Boolean, Null, Array, Object，BSON 額外擴充了許多強型別以利索引與運算。

**核心 BSON 型態清單（考題常考）：**
* **ObjectId**: 12-byte 的唯一識別碼（包含時間戳記、機器碼、進程 ID 與計數器）。
* **Date**: 64-bit 整數，儲存自 Unix 紀元以來的毫秒數（帶時區資訊，通常以 UTC 儲存）。
* **Timestamp**: 內部特殊型態（通常用於 Oplog 複製集同步），與 Date 不同。
* **String**: UTF-8 編碼的字串。
* **Integer**: 分為 32-bit (Int32) 與 64-bit (Int64/Long)。
* **Double**: 64-bit IEEE 754 浮點數（MongoDB 預設的數字型態）。
* **Decimal128**: 128-bit 高精度浮點數（適用於金融、貨幣計算）。
* **Binary data**: 用於儲存二進位檔案片段或 UUID。
* **Boolean**: `true` 或 `false`。
* **Null**: 空值。
* **Regular Expression**: 直接儲存正規表達式。
* **JavaScript**: 包含或不包含 Scope 的 JS 程式碼。



---

### 三、 核心說明與認證考點

#### 1. 通用與數字型態轉換
* **Number 的分流**：JSON 規範中沒有純整數，所有數字都是浮點數。MongoDB 為了索引效能與精確計數，在底層 BSON 將其嚴格區分為 32-bit Integer、64-bit Integer 與 Double。
* **Object 限制**：BSON 的內嵌文件（巢狀結構）最大深度限制為 **100 層**。

#### 2. BSON 專屬強型態（JSON 不支援）
* **ObjectId**：12-byte 的唯一識別碼。底層由「4-byte 時間戳記 + 5-byte 隨機值 + 3-byte 計數器」組成。在 JSON 中通常只能被迫表達為 24 字元的字串。
* **Date vs Timestamp**：
    * `Date` 是以 UTC 時間儲存的毫秒級 64-bit 整數，為日常開發使用。
    * `Timestamp` 是**內部特殊型態**，專門用於 Replica Set 的 Oplog（操作日誌）同步，兩者不可混用。
* **Decimal128**：**金融計算必考**。128-bit 高精度，專門解決 Double 浮點數運算時產生的微小誤差，適用於交易、貨幣與電商價格計算法。
* **Binary data**：用於儲存圖片、UUID 或微型二進位檔案。若用 JSON 傳輸必須轉為 Base64（會導致資料體積膨脹約 33%）；BSON 則能以原生二進位大小高效儲存。

---

### 各型態核心說明與認證考點

#### 1. 通用與數字型態考點 (JSON 支援或部分支援)
* **Integer (Int32 / Int64)**：JSON 規範中沒有純整數，所有數字都是浮點數。MongoDB 為了索引效能與精確計數，在 BSON 嚴格區分 32-bit 與 64-bit 整數。
* **Object (Embedded Document)**：巢狀文件深度限制為 **100 層**，考題常考限制邊界。

#### 2. BSON 專屬強型態考點 (JSON 不支援)
* **ObjectId**：12-byte 的唯一識別碼。底層結構包含：4-byte 時間戳記（Seconds）+ 5-byte 隨機值（Random value）+ 3-byte 計數器（Counter）。
* **Date**：以 UTC 時間儲存，底層是毫秒級的 64-bit 整數。
* **Timestamp**：**內部特殊型態**。一般開發不應使用，它專門用於 Replica Set 的 Oplog（操作日誌）同步，與 Date 完全不同。
* **Decimal128**：**金融計算必考**。128-bit 高精度，專門解決 Double 浮點數運算時產生的微小誤差，適用於錢包、金融交易與電商價格。
* **Binary data**：用於儲存圖片、UUID 或微型二進位檔案。若用 JSON 傳輸通常得轉 Base64，這會導致資料體積膨脹約 33%；BSON 則能以原生大小高效儲存。


---


### Question 1
**What is considered a best practice when naming fields in MongoDB documents? (Select one.)**

* (a) Using short, abbreviated names to conserve space on disk.
* (b) Re-using field names within a document to optimize indexes.
* (c) Using descriptive, unique names.
* (d) Using generic names.

**Answer:** C

**Explanation:**
* **A:** Short, abbreviated names can make data less clear and more difficult to manage. When it comes to naming document fields, space considerations are generally secondary to readability and maintainability.  
*(中文說明：使用簡短或縮寫的名稱會降低資料的可讀性，增加管理難度。在命名欄位時，資料的可讀性與可維護性通常遠比節省磁碟空間更重要。)*
* **B:** Re-using field names within a document does not optimize indexes and is not allowed because field names must be unique within a document.  
*(中文說明：在同一個文件中重複使用相同的欄位名稱並不能優化索引，而且這是不允許的，因為單一文件內的欄位名稱必須具備唯一性。)*
* **C:** Descriptive, unique field names in MongoDB documents help make the data more understandable and maintainable.  
*(中文說明：在 MongoDB 文件中使用具備描述性且唯一的欄位名稱，能讓資料更容易被理解與維護。)*
* **D:** Generic names are often unclear because they usually do not convey specific information about the data stored in the field.  
*(中文說明：通用或含糊的名稱通常不夠清晰，因為它們無法傳達該欄位所儲存資料的具體含意。)*

---

### Question 2
**What is a key characteristic of MongoDB's document model that allows for handling polymorphic data, or data of different shapes and types? (Select one.)**

* (a) Maximum document size of 16 MB
* (b) Similarity to JSON objects
* (c) Flexible schema
* (d) Single data type storage

**Answer:** C

**Explanation:**
* **A:** MongoDB documents do have a maximum document size of 16 MB, but this limit does not have an impact on its ability to handle polymorphic data.  
*(中文說明：MongoDB 文件的確有 16 MB 的最大容量限制，但這個限制與能否處理多態數據（Polymorphic data）無關。)*
* **B:** MongoDB documents are similar to JSON objects, which allows for efficient data storage and traversal. However, this trait is not directly related to its ability to handle polymorphic data.  
*(中文說明：MongoDB 文件類似於 JSON 物件，這有利於高效儲存與走訪資料。然而，這項特點並非處理多態數據的直接原因。)*
* **C:** A key characteristic of MongoDB's document model that allows for handling polymorphic data is its flexible schema, which allows documents within a collection to have different structures.  
*(中文說明：MongoDB 文件模型能處理多態數據的核心特性在於其彈性綱要（Flexible schema），這允許同一個 Collection 中的文件擁有完全不同的資料結構。)*
* **D:** MongoDB allows for storing diverse data types within a single document.  
*(中文說明：MongoDB 允許在單一文件中儲存多種不同的資料型別（但這屬於型別支援，而非解釋動態結構多態性的主因）。)*

---

### Question 3
**Which of the following value types are supported by MongoDB BSON? (Select one.)** *以下哪一組是 MongoDB BSON 支援的資料型別？*

* (a) String, Integer, Boolean, Date
* (b) String, Integer, Boolean, Date, Binary, ObjectId, Array
* (c) String, Integer, XML, Date
* (d) String, Integer, Boolean, Set

**Answer:** B

**Explanation:**
* BSON supports a wide variety of types, including String, Number (Integer/Double), Boolean, Date, Binary, ObjectId, Array, and Embedded Documents. XML and Set are not native BSON types.  
*(中文說明：BSON 支援多種型別，包含 String、Number、Boolean、Date、Binary、ObjectId、Array 與 Embedded Document，XML 與 Set 並非 BSON 原生支援的型別。)*

---

### Question 4
**Which BSON type is automatically generated by MongoDB as the default value for the _id field if not provided? (Select one.)** *若未指定 _id，MongoDB 預設會自動產生哪一種 BSON 型別？*

* (a) String
* (b) UUID
* (c) ObjectId
* (d) Integer

**Answer:** C

**Explanation:**
* If the `_id` field is not explicitly provided in a document upon insertion, MongoDB automatically generates a unique 12-byte ObjectId as its default value.  
*(中文說明：如果在寫入文件時未明確指定 `_id` 欄位，MongoDB 預設會自動產生一個唯一的 12-byte ObjectId 作為其值。)*



### Question 5
**You have the following three documents. Which of them can coexist in the same MongoDB collection? (Select one.)** *下列三個文件，哪些可以同時存在於同一個 MongoDB collection 中？*

```json
// Document A
{ "_id": 1, "name": "Alice", "age": 30 }

// Document B
{ "_id": 2, "name": "Bob", "email": "bob@test.com" }

// Document C
{ "_id": 3, "name": "Charlie", "age": "thirty" }
```

* (a) nly Document A and B
* (b) Only Document A and C
* (c) Only Document B and C
* (d) Document A, B, and C
  
**Answer:** D
  
**Explanation:**
* MongoDB 採用 schema-less 設計，同一個 collection 中的文件可以有不同欄位結構與資料型別。

### Question 6
*Which statement best describes MongoDB’s document model? (Select one.)
下列哪一項最能正確描述 MongoDB 的文件模型？
* A. All documents in a collection must have identical fields
* B. Documents are stored in tables with fixed schemas
* C. Documents are stored as flexible JSON-like structures
* D. Documents must follow a predefined relational schema

正確答案：C

解釋：MongoDB 使用彈性的文件模型，以類 JSON 的 BSON 格式儲存資料，不強制要求固定結構。




# Lesson 2: Data Types in MongoDB




### 一、 JSON 型態表列與規範

| 型態名稱 | JSON 空間與格式規範 | BSON 對應型態 |
| :--- | :--- | :--- |
| **String** | UTF-8 編碼字串，帶雙引號 | String |
| **Number** | 雙精度浮點數（JSON 唯一的數字型態） | Double (預設), Int32, Int64, Decimal128 |
| **Object** | 經由 `{}` 包裹的無序 Key-Value 對 | Object (Embedded Document) |
| **Array** | 經由 `[]` 包裹的有序值列表 | Array |
| **true** | 字面值 `true`（1 byte 文字） | Boolean (0x01) |
| **false** | 字面值 `false`（1 byte 文字） | Boolean (0x00) |
| **null** | 字面值 `null` | Null |

String 

Characters that represent text   

Single or double quotes(引號)  


Number

* Integers => 32/64 bit  

* Floating-point values  
 

Objecy

curly braces

Key-value pairs

Embed document

物件用大括號表示,封裝相key-value pairs
透過將物件崁入父文件中

Dates

* Represented as a timestamp
* Efficient for querying and sorting
* Epoch (millisceonds since January 1, 1970)

Compact

* Efficient for storage
* Easy to covert
* Fast comaprisons and computations

ObjectId
* _id
* Unique Identifier
* Must be unique
* MongoDB will automatically generate unique Object for _id

* 是一個24自元的十六進制
* 由Timestamp+Random Value+Counter 組成, 以確保其唯一性






### Question 1
**Which of the following statements about the _id field in MongoDB is true? (Select all that apply.)

* (A) It provides a unique identifier for each document.
* (B) It must be defined explicitly by the user when inserting a document.
* (C) If omitted during document insertion, MongoDB generates an ObjectId automatically.
* (D) It is required for every document in MongoDB.

**Answer:** ACD

**Explanation:**
* **A:** The _id field in MongoDB is a unique field that’s used to identify each document in a collection. It ensures that each document has a distinct identifier and allows for efficient querying and data retrieval.
* **B:** 
The _id field does not have to be explicitly defined by the user. If it’s not provided during document insertion, MongoDB will automatically generate a unique ObjectId for the _id field.
* **C:** The _id field in MongoDB is necessary for each document and must be unique. If it’s not provided during document insertion, MongoDB will automatically create an ObjectId for the _id field.
* **D:** Every document in MongoDB must have an _id field. If it’s not provided during document insertion, MongoDB will automatically create an ObjectId for the _id field.

---

### Question 2
**Which of the following data types is used for high-precision calculations and financial data in BSON? (Select one.)**

* (a) Boolean
* (b) Decimal128
* (c) Int32
* (d) Null

**Answer:** B

**Explanation:**

A: In BSON, boolean values are used to represent binary state data with values of true or false. They cannot be used for high-precision calculations or financial data.
B: In BSON, the decimal128 data type is used for high-precision calculations and financial data.
C: In BSON, the int32 data type is a 32-bit integer. Because integers are whole numbers only, they are unable to process fractional or decimal values that are required for financial data or high-precision calculations.
D: In BSON, null represents the absence of a value and cannot be used for calculations of any kind.



# Lesson 3: Managing Databases, Collections, and Documents in Atlas


<img width="2513" height="1558" alt="image" src="https://github.com/user-attachments/assets/ff2872dd-3f0f-4cae-9acf-30241e615cab" />

<img width="1739" height="1187" alt="image" src="https://github.com/user-attachments/assets/45d338b4-ff27-46d3-bef8-a41887aaba16" />

<img width="1707" height="1082" alt="image" src="https://github.com/user-attachments/assets/b36adbe0-bd8c-441f-b226-4c962a4363bd" />

LAB主要就是如何建立一個collection, 跟新增一筆資料, 再到CLI做FindOne


QA


In Atlas, how do you perform a simple query to find specific data? (Select one.)

a.
Select the Browse Collections option.
b.
View your data in table format.
c.
It isn't possible to perform a simple query in Atlas.
d.
Enter field-value pairs in the filter bar on the Collections tab.

Answer: D

**Explanation:**
A: The Browse Collections option allows you to navigate your collections, but to perform a simple query in Atlas, you need to enter field-value pairs directly in the filter bar on the Collections tab.

B: Viewing data in table format in Atlas helps to visualize your documents, but to perform a simple query, you must enter field-value pairs in the filter bar on the Collections tab.


C:It is possible to perform simple queries directly in Atlas. To find specific data, you must enter field-value pairs in the filter bar on the Collections tab.


D: To perform a simple query in Atlas to find specific data, you must enter field-value pairs directly in the filter bar on the Collections tab.



You are working with the memorabilia collection in the Atlas UI and need to add a new document for an autographed photo from the movie The Alamo, which is signed by three people. What best describes the steps you should take? (Select one.)

a.
Click Create Database followed by Insert Document, enter the field names and corresponding data types, click the Add to Existing Collection button.
b.
Click Insert Document, choose a view, enter the field names and corresponding data types, click Insert.
c.
In the document view filter bar, insert the "new" keyword followed by a JSON document with the field-value pairings, then click Apply.

Answer: A

**Explanation:**

A:The Insert Document button is found on the Document view once a collection is selected and cannot be accessed from the Create Database modal. The Create Database button is used to create a new database and an initial collection within that database.


B:To add a new document to the memorabilia collection from the Atlas UI, click the Insert Document button to open the Insert Document modal, where you can add the document field-value pairings. Once all field-value pairings are added, click the Insert button.

C:The filter bar on the document view in the Atlas UI can only be used to query for documents. To insert documents into a collection, you must first click the Insert Document button, add the appropriate field-value pairings, and then click the Insert button.



Lesson 4: Data Relationships

Entities
Attributes
Relationship
1. one to one
2. ont to many
3. many to many

* 1. Entities
  unique and independent(ex. a persion, a product, an organization or location, for example.)
  Represented by documents
  Grouped in collection
* 2. Attributes
     Characteristics that describe an entity
     Field-value pairs
     

QA

In our movies collection, which is an example of a one-to-many relationship? (Select one.)

a.
Movies and the theaters that are showing them.
b.
A movie and the studio that released it.
c.
A movie and its cast members.
d.
Theaters and the movies they are showing.



Answer: C

**Explanation:**

A:A movie typically is shown in many theaters at the same time, and a theater typically shows many movies at the same time. This represents a many-To-many relationship, not a one-to-many relationship.
B:A movie is released by a single studio, not multiple studios. This represents a one-to-one relationship.
C:A movie typically has many cast members. This represents a one-to-many relationship.
D: A theater typically shows many films at once, and a movie typically plays in many theaters, which represents a many-to-many relationship.



In the context of a movie database application, which of the following is an example of an entity? (Select one.)

a.
The rating of a movie.
b.
A document representing a movie tracked in the database application.
c.
The genre of a movie.
d.
The release date of a movie.

Answer: B

**Explanation:**


A:The rating of a movie is an attribute, not an entity. The movie is the entity that is being described by the rating attribute.


B:A movie is a distinct object in the database application. We would want to have documents per movie that we keep together in a collection.


C:The genre of a movie is an attribute, not an entity. The movie is the entity that is being described by the genre attribute.


D: The release date is an attribute of that movie. The movie is the entity that is being described by the release date attribute.


Lesson 5: Embedding and Referencing


Embedding

1 to 1
1 to Many

Embedding preferred for one-to-many

1. Access as single unit
2. Small, fixed-size related data
3. Minimize database operations


Referencing

1. Independent acces
2. Large or growing documents

再有大文複雜的數據時, 使用引用叫好


<img width="1710" height="887" alt="image" src="https://github.com/user-attachments/assets/6e6901fa-5686-438f-9560-b4cb08e17324" />

減少多對多, 以避免複雜性

QA

What is the typical approach for modeling a one-to-one relationship in MongoDB, where one entity is related to exactly one other entity? (Select one.)

a.
Embedding
b.
Referencing
c.
Flattening
d.
Normalizing



Answer: A

**Explanation:**

A:To model a one-to-one relationship in MongoDB, the typical approach is to embed the data. Embedding keeps related data together within the parent document, simplifying queries and allowing all related information to be stored and retrieved in a single document.

B:Referencing is not the preferred approach for modeling a one-to-one relationship, as it involves storing related data in separate documents, which can increase complexity.

C: Flattening refers to reducing nested data structures and is not specifically related to modeling a one-to-one relationship in MongoDB.

D: While normalizing data is the standard practice for relational databases, the core principle of data modeling for MongoDB is "Data that is accessed together should be stored together." Normalization goes against this principle.



You are designing a MongoDB schema for a movie database. There are approximately 40 movie genres represented in the database. A movie will typically belong to a few genres, and each genre could include thousands of movies.

If you want to include the genre information for each movie, while also being able to query for all movies within a genre, how should you model the relationship between movies and genres? 

a.
Embed the genres directly within the movie document.
b.
Reference the genres in the movie document using an array of genre IDs.
c.
Embed the movie documents within each genre document.
d.
Reference the movies in each genre document using an array of movie IDs.

Answer: A

**Explanation:**

A:Embedding genres within the movie document is suitable for this many-to-many relationship. Since each movie will likely not have more than a few genres, the genres can be directly embedded without fear of bloating the documents.


B:Referencing the genres in the movie document using an array of genre IDs is suitable for some many-to-many relationships, but does not provide any benefit in this scenario. You would need to run a second query for just a few results, leading to slower application performance. Embedding would provide the same results in a single query.


C:Embedding movies within each genre document is not suitable for this many-to-many relationship. Potentially thousands of movies belong to a given genre, and an array of thousands of movies can quickly consume too much space, leading to bloated documents.


D: Referencing movies in each genre document using an array of movie IDs does not meet the requirement to include genre information within each movie documents and is not recommended. With potentially thousands of movies per genre, these arrays could grow very large, leading to document bloat and performance issues during updates.










MongoDB and the Document Model
In this unit, you learned how to:

Describe MongoDB’s document model and the structure of documents
Explain the purpose of a flexible schema
List data types supported by MongoDB
Create a database and collection in the Atlas UI
Insert a document in a collection using the Atlas UI
Identify different types of data relationships: one-to-one, one-to-many, and many-to-many
Distinguish between embedding and referencing and when to use them

























### Question 7
*Which query returns all documents where the age field exists, regardless of its value? (Select one.)
下列哪個查詢可回傳所有包含 age 欄位的文件，不論其值為何？
* A. { age: { $exists: true } }
* B. { age: { $exist: true } }
* C. { age: { $ne: null } }
* D. { age: true }

**Answer:** A

解釋：$exists 用來判斷欄位是否存在，$exist 為錯誤語法。

### Question 8
*Which index type supports equality matches and range queries efficiently?
* A. Text index
* B. Hashed index
* C. B-tree index
* D. TTL index
**Answer:** C
解釋：MongoDB 的索引底層結構為 B-tree，非常適合等值與範圍查詢。

### Question 9
*Which statement about indexes is TRUE?
* A. Indexes improve query performance with no trade-offs
* B. Indexes reduce disk I/O
* C. Indexes slow down all queries
* D. Indexes are auto-created by MongoDB
**Answer:** B
解釋：索引能減少實際需要讀取的文件數量，直接降低 disk I/O。



