
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

### 7.2.2 匿名內部類別 Anonymous inner class 

假設要開發多人連線程式, 對每個連線客戶端都會建立client物件封裝相關訊息:

```java
package chapter7_2_2;

public class Client {
   public final String ip;
   public final String name;

   public Client(String ip, String name) {
      this.ip = ip;
      this.name = name;
   }
}
```

程式中建立的Clinet物件, 都會加入 ClientQueue 集中管理, 
若程式中其他部分希望在 ClientQueue 的Client加入或移除時可以收到通知,
以便做一些處理, 那麼可以將client加入或移除的資訊包裝為ClientEvent.
```java
package chapter7_2_2;

public class ClientEvent {
   private Client client;

   public ClientEvent(Client client) {
      this.client = client;
   }
   
   public String getName() {
      return client.name;
   }
   
   public String getIp() {
      return client.ip;
   }
}
```
定義ClientListener介面, 如果有物件對Client加入ClinetQueue有興趣, 則可以實作介面:
```java
package chapter7_2_2;

public interface ClientListener {
   void clientAdded(ClientEvent event);
   void clientRemoved(ClientEvent event);
}
```

如果有物件對Clinet加入ClientQueue有興趣, 可以實作 ClientListener() 註冊.
當每個 Client 透過 Queue 的add() 收集時, 會用ArrayList 收集 Client, 
接著使用 ClientEvent 封裝Client相關資訊, 接著使用 for 迴圈將註冊的 ClientListener 逐一取出,
並呼叫 ClientAdded() 方法進行通知. 

```java
package chapter7_2_2;

import java.util.ArrayList;

public class ClientQueue {
   // 收集連線的client
   private ArrayList<Client> clients = new ArrayList<Client>();
   // 收集對ClientQueue有興趣的ClientListener
   private ArrayList<ClientListener> listeners = new ArrayList<ClientListener>();
   // 註冊lientListener
   public void addClientListener(ClientListener listener) {
      listeners.add(listener);
   }
   
   public void add(Client client) {
      // 新增Client
      clients.add(client);
      // 通知資訊包裝為ClientEvent
      ClientEvent event = new ClientEvent(client);
      for (int i = 0; i < listeners.size(); i++) {
         ClientListener listener = (ClientListener) listeners.get(i);
         // 逐一通知ClientListener通知
         listener.clientAdded(event);
      }
   }

   public void remove(Client client) {
      clients.remove(client);
      ClientEvent event = new ClientEvent(client);
      for (int i = 0; i < listeners.size(); i++) {
         ClientListener listener = (ClientListener) listeners.get(i);
         listener.clientRemoved(event);
      }
   }
}

```


測試可用以下程式碼, 
其中使用匿名內部類別, 直接建立實作ClientListener的物件:

```java
package chapter7_2_2;

public class MultiChat {
   public static void main(String[] args) {
      Client c1 = new Client("127.0.0.1", "T1");
      Client c2 = new Client("127.168.0.1", "T2");
      
      ClientQueue queue = new ClientQueue();
      queue.addClientListener(new ClientListener() {
         @Override
         public void clientAdded(ClientEvent event) {
            System.out.printf("%s 從 %s 連線\n", event.getName(),event.getIp());
         }

         @Override
         public void clientRemoved(ClientEvent event) {
            System.out.printf("%s 從 %s 離線\n", event.getName(),event.getIp());
         }
      });
      
      queue.add(c1);
      queue.add(c2);
      
      queue.remove(c2);
      queue.remove(c1);
   }
}

```











