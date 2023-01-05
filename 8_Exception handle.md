

錯誤, 會被包裝成物件, 這些物件都是可以拋出的(throws),
因此設計錯誤物件都繼承自 **java.lang.Throwable**.

Throwable 定義了取得的錯誤訊息, 堆疊追蹤(Stack Trace) 等方法,
有兩個子類別: **java.lang.Error** 跟 **java.lang.Exception**.

![圖片](https://user-images.githubusercontent.com/118010660/210779529-416baffb-bc02-494c-83b2-ac3a16a11d14.png)

**Error與其子類別實例代表嚴重系統錯誤**, 如硬體問題, JVM錯誤等等, 雖然也可以用try-cache處裡, 但不建議, JAVA本身也沒辦法處理.

程式本身的錯誤, 建議使用Exception或其子類別來表現, 所以通常稱錯誤處理為例外處裡(Exceptopn handling).

單就與法語繼承架構上來說, 如果某個方法宣告會拋Throwable或其子類別來實例,
只要不屬於Error或RuntimeException與其子類別,
就必須明訂try-cache來處理, 或使用throws宣告這個方法會拋出例外, 否則會編譯失敗.

在呼叫System.in.read()時, in 是System的靜態成員, 型態為 java.io.InputStream:
![圖片](https://user-images.githubusercontent.com/118010660/210781639-4f30dd65-41c3-4144-965c-4945c01c4fe9.png)

IOException 是Exception 的直接子類別, 所以編譯器會明確要求使用語法處理.
**Exception 與其子類別, 但非屬於RumtimeExcption或其子類別, 稱為受檢例外(Checked Exception)**

受誰檢查 ? **編譯器檢查**

**Checked Exception 存在的目的, 在API設計者時做某方法時, 受某些條件成立時會引發錯誤,
而且認為呼叫方法的客戶端有能力處理錯誤,要求編輯器提醒客戶端必須明確處理錯誤.**
不然不可通故編輯, API客戶端無權選擇要不要處理.




