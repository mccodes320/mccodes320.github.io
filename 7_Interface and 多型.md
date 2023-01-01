
### 7.2 介面語法細節

在 Java 中, 可以使用 interface 來定義抽象的行為死與外觀, **如果是介面中的方法, 可宣告為 public abstract.**

```java
public interface Swimmer {
   public abstract void swim();
}
```
介面中的方法沒有實作時, 一定是公開且抽象的,
所以撰寫時可以省略 **public abstract**,
編譯器會自動幫你補上.

```java
public interface Swimmer {
   void swim();  // 預設就是 public abstract
}
```

** CASE 1 **

```java
interface Action {
   void execute();
}

class Some implements Action {
   void execute() {  
      System.out.println("do execute");
   }
}
```
執行結果會如何呢?

Some 物件內的 execute() 編譯就會錯誤Cannot reduce the visibility of the inherited method from Action
錯誤原因是Action中定義的execute()其實預設為public abstract,
而Somoe類別實作時, 卻沒有賦予 public,
因此就是預設為套件權限, 等同於將Action中的public 的方法縮小為套件權限, 所以就編譯失敗了!
所以要修改成va
```java
class Some implements Action {
   public void execute() {
      System.out.println("do execute");
   }
}
```

**CASE 2**
```java
interface Action {
   private abstract void execute();
}
```
直接拋錯
Illegal combination of modifiers for the private interface method execute; additionally only one of static and strictfp is permitted

**CASR 3**
```java
interface Action {
   abstract void execute();
}
```
這樣也可以編譯成功

### inerface 可以定義常數
常見的介面中定義**列舉常數**, 增加程式可讀性
但是在interface中, 也只能定義 public static final的列舉常數
```java
package chapter7;

public interface Action {
   public static final int STOP = 0;
   public static final int RIGHT = 1;
   public static final int LEFT = 2;
   public static final int UP = 3;
   public static final int DOWN = 4;
}
```
```java
package chapter7;

public class Game {
   public static void main(String[] args) {
      play(Action.LEFT);
   }

   public static void play(int action) {
      switch (action) {
      case Action.STOP:
         System.out.println("STOP GAME");
         break;
      case Action.RIGHT:
         System.out.println("RIGHT GAME");
         break;
      case Action.LEFT:
         System.out.println("LEFT GAME");
         break;
      case Action.UP:
         System.out.println("UP GAME");
         break;
      case Action.DOWN:
         System.out.println("DOWN GAME");
         break;
      default:
         System.out.println("no supper");
      }
   }
}
```
output: LEFT GAME

撰寫上也可以寫成,
但一定要賦予指定值

在類別中定列舉常數也是可以的, 
但是一定要明確寫出public static final.

```java
public interface Action {
   int STOP = 0;
   int RIGHT = 1;
   int LEFT = 2;
   int UP = 3;
   int DOWN = 4;
}
```

