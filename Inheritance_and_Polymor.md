



| Key Word | 類別內部 | 相同套件類別 | 不同套件類別
| :-----| ----: | :----: | :----:|
| public | 可存取 | 可存取 | 可存取|
| protected | 可存取 | 可存取 | 子類別可存取 |
| N/A | 可存取 | 可存取  |  不可存取 |
| private |可存取 | 不可存取 | 不可存取 |


6.2.3 再看建構式
```
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

```
new Some();
```
call Some

```
new Other();
```
call Some
call Other
