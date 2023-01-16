

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


**非受檢例外 Unchecked Exception**

**屬於 RuntiomeException 衍生出來的類別實例, 代表API設計者實作某方法實, 某些條件成立時會引發錯誤, 
而且認為API客戶端應該再呼叫方法前做好檢查, 以避免引發錯誤.**

之所以命名為執行時期例外, 是因為編譯器不會強迫一定得在語法上加以處理,
**亦稱為非受檢例外 (Unchecked Exception)**

例如使用陣列時, 若存取超出索引就會拋出 ArrayIndexOutOfBoundsException, 但是編譯器並不會強迫你在語法上加以處理,
因為ArrayIndexOutOfBoundsException屬於 RuntimeException.

若在Average範例中, InputMismatchException也是RuntimeException.


8.1.3 要抓還是要拋 ?

以此為例, 讀取文字檔案, 以字串回傳
```java
public class FileUtil {
   public static String readFile(String name) {
      StringBuilder text = new StringBuilder();
      try {
         Scanner console = new Scanner(new FileInputStream(name));
         while (console.hasNext()) {
            text.append(console.nextLine()).append('\n');
         }
      } catch (FileNotFoundException e) {
         e.printStackTrace();
      }
      return text.toString();
   }
}
```
因為建構 FileInputStream 會拋出 FileNotFoundException , 所以在這裡主控台中就會顯示錯誤
但是code並不一定會用在文字模式中, 也可能是web網頁, 像這種情況使用者是不會看到訊息的.
所以, 以這個案例來說不論是用catch中寫死例外處理, 或是輸出錯誤訊息, 都不符合需求.

所以方法設計流程中發生例外, 而你的設計並沒有充足的資訊知道該如何處理,
那麼就可以把例外拋出, 讓呼叫方法的客戶端自己處理


```java
   public static String readFile(String name) throws FileNotFoundException {
      StringBuilder text = new StringBuilder();
      Scanner console = new Scanner(new FileInputStream(name));
      while (console.hasNext()) {
         text.append(console.nextLine()).append('\n');
      }
      return text.toString();
   }
```

使用 throws 宣告此方法會拋出例外類型或父類別, 編譯器才會讓你通過


想先處理部分事項再拋出:

```java
   public static String readFile(String name) throws FileNotFoundException {
      StringBuilder text = new StringBuilder();
      try {
         Scanner console = new Scanner(new FileInputStream(name));
         while (console.hasNext()) {
            text.append(console.nextLine()).append('\n');
         }
      } catch (FileNotFoundException e) {
         e.printStackTrace();
         throw e;
      }
      return text.toString();
   }
```
catch 區塊進行玩部分錯誤處理之後, 可以使用 throw 再將例外拋出.
事實上, 也可以在任何流程中拍出例外,  一定要在catch 區塊中,
因為如果在流程中拋出例外, 就直接跳離原有流程,
可以拋出受檢或非受檢例外

如果拋出的是受檢例外, 表示你認為客戶端有能力且應可以處理例外, 此時必須在方法上使用throwvms宣告,
如果拋出的例外是非受檢例外, 表示你認為客戶端呼叫方法的時機出錯了, 拋出例外要求客戶端修正bug再來呼叫方法, 就不需要throws宣告

如果使用繼承時, 父類別某個方法宣告 throws 某些例外, 子類別重新定義該方法時可以:
* 不宣告 throws 任何例外
* 可 throws 父類別該方法中宣告的某些例外
* 可 throws 物類別該方中宣告的子例外
不可以
* throws 父類別方法中宣告的其他例外
* throws 父類別方中宣告例外之父類別

自訂受檢例外
public class CustomizedException extends Exception {} 
自訂非受檢例外
public class CustomizedException extends RuntimeException {} 


```java

catch(SomeException ex){
  throw new CustomizedException("Error exception);
}
```

 8.1.6 Assert

有時候, 需求或設計時就可以確認, 程式執行的某個時間點或某個情況下, 一定是楚瑜或不屬於某個狀態,
若不是, 則是嚴重的錯誤, 開發過程中發現這種嚴重的錯誤, 必須立即停止程式確認需求與設計.

而這是一種斷言 Assertion, 例如某個時間點程式某個變數值一定是多少.
斷言的結果一定是成立或是不成立, 逾期結果與實際程式狀態相同時, 斷言成立, 否則就是斷言不成立.

兩種語法:
assert boolean_expression
assert boolean_expression: detail_expression

boolean_expression為true就不做事, 如我是false就會發生AssertiobError.
detail_expression如果是false就將此顯示於文字描述結果中.

若要在程式執行時, 啟動斷言檢查, 可以在java指令中指定 -enableassertions或是-ea 引數.

什麼時候使用斷言呢? 一般會建議
1. 斷言客戶端呼叫方法前, 已經準備好某些前置條件(通常在private方法中)
2. 斷言客戶端呼叫方法後, 具有方法承諾的結果.
3. 斷言物件某個時間點下的狀態
4. 使用斷言取代註解
5. 斷言程式流程中絕對不會執行到的程式碼部分

public void charge (ine money) throws InsufficientException {
  assert money>=0 : "扣負數?不是叫我儲值" ;
  
  checkBalance(monoey);
  this.valance -= money;
  
  asssert this,balence >- 0:"this.balance不能負數";  
}



8.2.1 使用 finally
最後一定會被執行關閉資源的動作, try-catch 就可以搭配 finally,
無倫try區塊中有無發生例外, 若有撰寫finally就一定會被執行.


如果程式中有先return的部分, 而且也有寫Finall區塊,
那finally區快就會先執行, 在座returnㄡ
```java
public class FinallyDemo {
   public static void main(String[] args) {
         System.out.println(test(true));
   }
   
   static int test(boolean flag) {
      try {
         if(flag) {
            return 1;
         }
      }finally {
         System.out.println("finally");
      }
      return 0;
   }
}
```
finally
1


8.2.2 自動嘗試關閉資源

經常使用 try finally 嘗試關閉資源, 會發現程式撰寫的流程是類似的.
所以在java7之後, **新增了嘗試關閉資源(Try-with-resource)語法**

想要嘗試自動關閉資源的物件, 是撰寫try 之後的跨號中,
如無須Catch處裡任何例外, 可以不用撰寫 也不用寫Finally自行嘗試關閉資源



```java
public class FileUtil2 {
   public static void main(String[] args) {
   }
   public static String readFile(String name) throws FileNotFoundException {
      StringBuilder text = new StringBuilder();
      try (Scanner console = new Scanner(new FileInputStream(name))){
         while(console.hasNext()) {
            text.append(console.nextLine()).append("\n");
         }
      }
      return text.toString();
   }
}

```





