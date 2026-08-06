
# Associate Developer Exam Study Guide

MongoDB Certified Associate Developer（開發人員認證）  
對象：​具備至少一種程式語言（如 Node.js、Python、Java、PHP、C#）的開發人員，熟悉 MongoDB 驅動程式。  
考試重點：​CRUD 操作、資料模型設計、索引、聚合框架、MongoDB 架構與設計理念。  
考試時間：​75 分鐘  
費用：​$150 美元  
建議準備：​MongoDB University 的免費課程與練習測驗。   


### Exam Study Guide

[Exam Study Guide](https://learn.mongodb.com/learn/course/mongodb-associate-developer-exam-study-guide/main/associate-dba-exam-study-guide)

| **編號** | **考核內容 (Learning Objectives)** |
| :--- | :--- |
| **Section 1** | **MONGODB 概述與文件模型 (8%)** |
| **1.1** | 辨識 MongoDB BSON 所支援的型別種類。 |
| **1.2** | 給定三份結構（shape）不同的文件，辨識哪些文件可以共存於同一個集合（collection）中。 |
| **Section 2** | **CRUD (51%)** |
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
| **Section 3** | **索引 (17%)** |
| **3.1** | 給定一個正在執行全表掃描（collection scan）的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.2** | 給定一個對陣列欄位進行等值比對且正在執行全表掃描的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.3** | 給定一個無過濾條件、對兩個欄位進行排序且正在執行全表掃描的查詢，辨識哪種索引可以提升該查詢的效能。 |
| **3.4** | 給定一個集合，辨識該集合目前存在多少個索引。 |
| **3.5** | 辨識使用索引的權衡（trade-offs），以及刪除支援查詢的索引後所帶來的影響與後果。 |
| **3.6** | 辨識執行計畫（explain plan）輸出中代表潛在效能問題的資訊，特別是針對給定查詢是否有使用索引。 |
| **Section 4** | **資料建模 (4%)** |
| **3.1** | 給定一個包含三個集合（一個父集合與兩個子集合）以及使用者的情境，辨識哪些是內嵌關係（embedded relationships），哪些應該採用引用連結（linked）。 |
| **4.2** | 辨識被視為反模式（anti-pattern）的資料建模範例。 |
| **Section 5** | **工具與周邊應用 (2%)** |
| **5.1** | 給定一個載入 Atlas 範例資料集（Sample Dataset）的情境，並使用 Data Explorer 查找集合中的第一份文件。 |
| **Section 6** | **驅動程式 (18%)** |
| **6.1** | 定義什麼是 XX 驅動程式（Driver）？ |
| **6.2** | 定義 XX 應用程式如何連接與使用 XX 驅動程式？ |
| **6.3** | 定義 `MongoClient` 用於連接驅動程式與資料庫的 URI 字串包含哪些組成要素。 |
| **6.4** | 辨識在驅動程式層面什麼是連接池（connection pooling），以及它能提供哪些優勢。 |
| **6.5** | 辨識 XX 驅動程式插入單份文件與插入多份文件的正確語法。 |
| **6.6** | 辨識 XX 驅動程式更新單份文件與更新多份文件的正確語法。 |
| **6.7** | 辨識 XX 驅動程式刪除單份文件與刪除多份文件的正確語法。 |
| **6.8** | 辨識 XX 驅動程式查找單份文件與查找多份文件的正確語法。 |
| **6.9** | 辨識 XX 驅動程式建立聚合管道（aggregation pipeline）的正確語法。 |
| **6.10** | 辨識 XX 驅動程式在使用 MongoDB 查詢語言 (MQL) 與使用聚合框架（Aggregation Framework）時的語法差異。 |




### 模擬試題

https://learn.mongodb.com/courses/mongodb-associate-developer-exam-study-guide


https://learn.mongodb.com/learn/learning-path/mongodb-java-developer-path

MongoDB助理開發人員考試

https://learn.mongodb.com/pages/mongodb-associate-developer-exam



ref
https://github.com/yixin0829/mongodb-dev-cert-prep



## Caspar Chang

[MongoDB Certification Study Guide / MongoDB 證照考試指南 重點整理（上）](https://askstw.medium.com/mongodb-study-guide-mongodb-%E5%82%99%E8%80%83-%E8%A6%81%E9%BB%9E-%E4%B8%8A-6cd3259a6bbf)  
[MongoDB Certification Study Guide / MongoDB 證照考試指南 重點整理（下）](https://askstw.medium.com/mongodb-certification-study-guide-1fc53196eea6)  
[MongoDB Certified Overview / MongoDB 認證 流程](https://askstw.medium.com/mongodb-certification-overview-448718bec20e)  




https://www.youtube.com/watch?v=9vraN5NqpaA

https://www.youtube.com/watch?v=iHG6l6LhtZM

https://www.validexamdumps.com/mongodb/c100dba-exam-questions

## 考試預約
https://go.proctoru.com/students/reservations/44603321/edit?bluebird=true





<img width="1917" height="1807" alt="image" src="https://github.com/user-attachments/assets/35208ba3-ccc9-49f1-a0c4-11a794c5f2b9" />


https://learn.mongodb.com/learn/course/mongodb-associate-developer-exam-java/purchase-and-schedule-your-certification/certification?client=customer

<img width="3356" height="1062" alt="image" src="https://github.com/user-attachments/assets/73f5d62e-d92e-48d0-b564-00d059ca6bbd" />


## pass4success
https://www.pass4success.com/mongodb/exam/c100dba

## 
https://www.mongodb.com/zh-cn/docs/manual/reference/glossary/

## 
