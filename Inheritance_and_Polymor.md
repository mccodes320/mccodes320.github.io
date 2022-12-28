



|KeyWord|類別內部|相同套件類別|不同套件類別
|:-----|----:|:----:|:----:|
|public|可存取|可存取|可存取|
|protected|可存取|可存取|子類別可存取|
|N/A|可存取|可存取|不可存取|
|private|可存取|不可存取|不可存取|


6.2.3 再看建構式

CASE 1
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

CASE 2

```java
class Some{
   Some(){
      System.out.println("call Some");
   }
   **Some(int i){**
     **System.out.println("call Some i: " + i);**
   **}**
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

CASE 3

```java
class Some{
   Some(){
      System.out.println("call Some");
   }
   **Some(int i){**
     **System.out.println("call Some i: " + i);**
   **}**
}

class Other extends Some {
   Other(){
      **super(10);**
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


