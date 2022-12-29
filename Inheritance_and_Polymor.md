



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

Case 

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
到第3, 是將s2參考的物件s1參考, 
![圖片](https://user-images.githubusercontent.com/118010660/209963909-7e7738ad-5eb9-48ee-9341-95173020ea33.png)








