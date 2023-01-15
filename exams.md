``` java
1. 下列何者可以編譯成功(複選題)
A. char xy = 'xy';
B. String s2 = "s2";
C. char a = '\';
D. String s = "\"";
E. int i = 1 + 0.4;
```
BD


``` java
2. 請選擇合法敘述句(複選題)
A. String #s1="Hello!";
B. int $money=2000;
C. double _tax=0.16;
D. double ~pi=3.14;
```
BC
``` java
3.程式碼如下:
01 int i=1;
02 long I=1;
03 float f=1.0f;
04 double d=1.0;
05 sum=i+I+f+d;
請問sum應該定義成什麼資料類型? 
A. byte
B. short
C. int
D. long
E. float
F. double
```
F
``` java
4.下列選向哪些可以編譯成功?(選擇3個) 
A. int i=(int)(1+1.1f+1.1);
B. double d=(float)(1+1.1f+1.1);
C. long I=1+1.1f+1.1;
D. float f=(long)(1+1.1f+1.1);
E. int i=(int)1.1f+1.1;
```
AB**D**

5. 結果為何
``` java
7+6-5*4/3%(2+1)
``` 
13

7+6-5*4/3%(2+1)
7+6-20/3%(2+1)
7+6-20/3%(3)
7+6-20/3%(3)
7+6-6%(3)
7+6-0


7. 
``` java
程式碼如下, 最後結果為何
boolean result;
int i = 1;
result = 1 == 2 && ++i >=2
syso(result + " " + i)

A. true 1
B. true 2
C. false 1
D. false 2
```

C

8. 
結果為何
```java
int x = 1, y= 1;
boolean b = ++x > ++y;
System.out.println(b);
```
false

9.
結果為何
```java
int x = 1, y= 1;
boolean b = !(x>y) ^ !(x<y);
System.out.println(b);
```
false

位元運算子：
a = 0011 1100
b = 0000 1101

&：位元AND運算，兩者都有才取，否則取零。
a&b = 0000 1100 
|：位元OR運算，至少兩者其一有則為一，兩者皆無則為零。
a|b = 0011 1101
^：位元XOR運算，只有兩者其一有才為一，兩者皆有及兩者皆無都為零。
a^b = 0011 0001
~：取相反值，一變零，零變一。
~a = 1100 0011
