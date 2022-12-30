



|KeyWord|類別內部|相同套件類別|不同套件類別
|:-----|----:|:----:|:----:|
|public|可存取|可存取|可存取|
|protected|可存取|可存取|子類別可存取|
|N/A|可存取|可存取|不可存取|
|private|可存取|不可存取|不可存取|


6.2.3 再看建構式

#### CASE 1
```java
class Some{
   Some(){
      System.out.println("call Some");
   }
}

class Other extends Some {
   Other(){
      System.out.println("call Other");
   }
}
```

```java
new Some();
```
Output: call Some

```java
new Other();
```
Output: 
call Some
call Other

#### CASE 2

```java
class Some{
   Some(){
      System.out.println("call Some");
   }
   Some(int i){
     System.out.println("call Some i: " + i);
   }
}

class Other extends Some {
   Other(){
      System.out.println("call Other");
   }
}
```

```java
new Some();
```
Output: call Some

```java
new Other();
```
Output: 
call Some
call Other

#### CASE 3

```java
class Some{
   Some(){
      System.out.println("call Some");
   }
   Some(int i){
     System.out.println("call Some i: " + i);
   }
}

class Other extends Some {
   Other(){
      super(10);
      System.out.println("call Other");
   }
}
```
```java
new Some();
```
Output: call Some

```java
new Other();
```
Output: 
call Some i: 10
call Other

#### CASE 4

```java
class Some{
   Some(int i){
      System.out.println("call Some i: " + i);
   }   
}
```
```java
class Other extends Some {
   Other(){   <<< 
      System.out.println("call Other");
   }
}
```
Compile error : Implicit super constructor Some() is undefined. Must explicitly invoke another constructor

```java
class Other extends Some {
   Other(){
      System.out.println("call Other");
	  super(10);  <<<
   }
}
```
Compile error : Constructor call must be the first statement in a constructor

** this() and super 只能擇一呼叫, 而且一定要再建構式第一行執行**








### 6.2.6 Garbage Collection, GC
建構物件會佔據記憶體, 如果程式執行流程中已無再使用某個物件, 該物件就只是徒消耗記憶體的垃圾.

對於不再有用的物件, JVM有GC機制, 收集到的垃圾物件所佔據的記憶體空間 會被GC釋放.
哪些會被JVM認定為垃圾物件呢? 執行流程中, 無法透過變數參考的物件就是.
執行流程? 就是執行緒 Thread
不同的需求也會有不同的垃圾收集演算法, 細節交給JVM即可.

CASE 1

假設有個類別, 程式進入點開始, 如下撰寫:
```java
class Some(){
   Some next;
}

Some s1 = new Some();
Some s2 = new Some();
s1 = s2;
```
程式到第2行時, 主執行緒可以透過參考名稱所參考到物件為
S1~[]
S2~[]
* 兩個物件都有牌子

![圖片](https://user-images.githubusercontent.com/118010660/209963909-7e7738ad-5eb9-48ee-9341-95173020ea33.png)

到第3行, 是將s2參考的物件s1參考, 

![圖片](https://user-images.githubusercontent.com/118010660/209964096-e8c8356c-a229-4cd5-b775-15c4818007ac.png)

* 沒有牌子就是垃圾

CASE 2
```java
Some s = new Some();
s.next() = new Some();
s = null;
```
![圖片](https://user-images.githubusercontent.com/118010660/209964662-fdfa6deb-f644-4a37-a2cc-acf7f40da611.png)

* 鏈狀參考

主流程開始, 可以透過s參考到中間物件, 而some.next可以參考至最右邊的物件.
第三行後, 由於主流程無法透過S參考到中間物件, 也無法再透過中間物件的next參考至右邊物件,
所以現在兩個物件都是垃圾.

![圖片](https://user-images.githubusercontent.com/118010660/209964877-529016cb-e070-4d94-b313-cc09fcf67f29.png)


### 6.2.7 再看抽象類別

撰寫程式時, 常碰到看似不合理又非得要完成的需求,
舉例來說, 老闆要你開發猜數字遊戲, 會隨機產生 0~9的數字,
輸入者輸入的數字與隨機產生的數字相比, 如果相同顯示猜中了,
不同就繼續讓使用者輸入數字, 直到猜中為止.

```java
public class Chatper_6_2_7 {
   public static void main(String[] args) {
      Scanner c = new Scanner(System.in);
      int number = (int) (Math.random()*10);
      int guess;
      do {
         System.out.println("Enter the number: ");
         guess = c.nextInt();
      } while(number != guess);
      System.out.println("Done");
      c.close();
   }
}
```

可以在給老闆之後, 老闆又說, 
我有說要是在文字模式下執行嗎? 還沒決定哪個環境, 不能拖到下禮拜.
而且還要再多個團隊與部門接合作開發. 互相等待的情況下可能就又拖了兩三個禮拜.

這種需求就透過Design來解決.
像是取得使用者輸入, 顯示結果的環境未定, 但已知部分是可以先做的.

```java
public abstract class GuessGame {
   public void go() {
      int number = (int) (Math.random()*10);
      int guess;
      do {
         System.out.println("Enter the number: ");
         guess = nextInt();
      } while(number != guess);
      System.out.println("Done: " + number);
   }
   
   public void println(String text) {
      print(text + "\n");
   }
   
   public abstract void print(String text);
   public abstract int nextInt();
}
```
這裡可以看到, 哪個類別定義的不完整, 
因為老闆還沒確定在哪個環境進行猜數字,
所以如何輸出跟取得使用者輸入就不先實作,
但是猜數字部分是可以先實作的, 
雖然是抽象類別, 但是go()還是可以呼叫.

開完會了, 
終於還是決定於文字模式下執行猜數字遊戲,
那就可以另外撰寫類別來實作GuessGame, 實作當中的抽象方法

```java
import java.util.Scanner;

public class ConsleGame extends GuessGame{
   private Scanner sc = new Scanner(System.in);
   
   @Override
   public void print(String text) {
      System.out.println("ConsleGame: " + text);
   }

   @Override
   public int nextInt() {
      return sc.nextInt();
   }
}
```

所以在建構了ConsleGame實例,
執行了go() 並呼叫了 print() , nextInt()等, 都是在執行ConsleGame中定義的流程,
完整的猜數字也就出來了.

```java
public class Guess {
   public static void main(String[] args) {
      ConsleGame gg = new ConsleGame();
      gg.go();
   }
```
Enter the number: 
0
Done: 0

### 重點複習
多形 :
以抽象來講, 就是使用單一介面操作多種形態的物件 !

練習題:

1. 使用6.2.5 設計的ArrayList類別收集物件,顯示所收集物件之字串描述, 必須如下:
```java
ArrayList list = new ArrayList();
// ... 收集物件
for(int i = 0 ; i < list.size() ; i++) {
   out.println(list.get(i));
}
```
請重新定義ArrryList的toString()方法,
讓客戶端想顯示收集物件之字串描述, 如下:

ArrayList list = new ArrayList();
out.println(list);

**Solution**
ArrayList

```java
package chapter6;

import java.util.Arrays;

public class ArrayList {
   private Object[] elems;
   private int next;

   public ArrayList(int capacity) {
      elems = new Object[capacity];
   }
   
   public ArrayList() {
      this(16);
   }
   
   public void add(Object o) {
      if(next == elems.length) {
         elems = Arrays.copyOf(elems, elems.length *2);
      }
      elems [next ++] = o;
   }
   
   public Object get(int index ) {
      return elems[index];
   }
   
   public int size() {
      return next;
   }
   
   @Override
   public String toString() {
      var desc = new StringBuilder();
      for(int i = 0 ; i < next-1 ; i++) {
         desc.append(elems[i]).append(",");
      }
      desc.append(elems[next-1]);
      return desc.toString();
   }
}

```
```java
   public static void main(String[] args) {
      ArrayList list = new ArrayList();
      list.add("A");
      list.add("B");
      list.add("C");
      System.out.println(list.toString());
   }
```


