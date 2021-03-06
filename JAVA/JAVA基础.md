# JAVA基础

## @基本语法

### 一，两大基本数据类型

#### 1，引用数据类型

类、接口类型、数组类型、枚举类型、注解类型，字符串型

```java
 A a =new A();
 //变量a的值为它所引用对象的地址
```



##### 4种引用类型

强引用，软引用，弱引用，虚引用

###### 强引用

只要强引用存在，垃圾回收器将永远不会回收被引用的对象，哪怕内存不足时，JVM也会直接抛出`OutOfMemoryError`，不会去回收

```java
//java中默认声明的就是强
Object obj = new Object(); //只要obj还指向Object对象，Object对象就不会被回收
```



###### 软引用

软引用是用来描述一些非必需但仍有用的对象

**在内存足够的时候，软引用对象不会被回收，只有在内存不足时，系统则会回收软引用对象，如果回收了软引用对象之后仍然没有足够的内存，才会抛出内存溢出异常**



###### 弱引用

弱引用的引用强度比软引用要更弱一些，**无论内存是否足够，只要 JVM 开始进行垃圾回收，那些被弱引用关联的对象都会被回收**

```java
//JDK1.2 之后，用 java.lang.ref.WeakReference 来表示弱引用
package java.lang.ref;

public class WeakReference<T> extends Reference<T> {
    public WeakReference(T var1) {
        super(var1);
    }

    public WeakReference(T var1, ReferenceQueue<? super T> var2) {
        super(var1, var2);
    }
}
```



```java
//所得到的结果全是null，垃圾回收了对象
private static void testWeakReference() {
		for (int i = 0; i < 10; i++) {
			byte[] buff = new byte[1024 * 1024];
			WeakReference<byte[]> sr = new WeakReference<>(buff);
			list.add(sr);
		}
		
		System.gc(); //主动通知垃圾回收
		
		for(int i=0; i < list.size(); i++){
			Object obj = ((WeakReference) list.get(i)).get();
			System.out.println(obj);
		}
	}
```



###### 虚引用

虚引用是最弱的一种引用关系，如果一个对象仅持有虚引用，那么它就和没有任何引用一样，它随时可能会被回收



#### 2，基本数据类型

> 或叫做原生类、内置类型

##### 八种基本数据类型

`整型`，`长整型`，`短整型`，`字节`，`单精度浮点型`，`双精度浮点型`，`字符型`，`boolean型`

###### 1，整型

| 整型    |       |
| ------- | ----- |
| `long`  | 8字节 |
| `short` | 2字节 |
| `byte`  | 1字节 |
| `int`   | 4字节 |

> 32位的long占4字节，64位的占8位



**整数溢出问题**



int 类型在 Java 中是“有符号”的。所谓“有符号”就是有正负。

在计算机中用二进制表示所有的信息，这个符号的区别就看首位。

**首位如果是 0，就是正的，1 就是负的。正与负的区别也因此就在于取反加一**

所谓数值溢出就会出现这个现象

Java 中的 ` int` 总共就 32 位，正数上限的情况首位也只能是 0，其他位都可以是 1（就是 2^31-1 的情况）。但是如果正数过大了，例如 2^31，计算机不得不把首位变成 1，并且很快就忘了这是溢出情况，把它按照正常的方式输出了，于是就成了负的，它没有办法自动处理超过溢出的情况，因为 32 位是固定的，它不能因为溢出而临时扩展到 33 位之类的



```java
2^31 - 1 = 0111 1111 1111 1111 1111 1111 1111 1111 = 2147483647
2^31 = 2^31 - 1 + 1 = 1000 0000 0000 0000 0000 0000 0000 0000 = -2147483648
```



存储数值超过了整形数值2^31-1，导致数据**向上溢出**

整数数值小于了-2^31，导致数值**向下溢出**





###### 2，进制

1，十六进制，前缀为0x或者0X

2，八进制，前缀为0

3，二进制，前缀为0b或者0B



> 注意
>
> java没有任何无符号形式的数据类型
>
> **无符号即为** **纯正数**，可以理解为每个数据类型都有符号



###### 3，浮点型

知识点一：

浮点数值适用于无法接受舍入误差的计算中，浮点数值采用二进制系统表示，二进制无法准确的表示分数1/10

```java
double aa =  12/10;
System.out.println(aa);
//输出结果为1.0
```

知识点二：

| 浮点型                      |       |
| --------------------------- | ----- |
| float（单精度类型）后缀F或f | 4字节 |
| double（双精度类型）        | 8字节 |

知识点三：

java常用的内置常数：double变量初始化为无穷大

```java
Double.POSITIVE_INFINITY = 1.0 / 0.0
//输出：Infinity
```

###### 4，字符型

知识点一：

字符型进行算数运算，运算的是ASCCL码

**可以表示为十六进制**，从`\u0000`到`Uffff`（理解为可以用十六进制表示）



知识点二：

java采用Unicode编码方式，无论是中文还是英文字母，都占2字节



###### 5，字符串,

1，字符串在java中存储在字符串常量区中

2，==判断的是对象引用是否是同一个引用，判断字符串相等要用equals方法



**String类**

一，String对象是不可变的，每一个修改String值得方法，都是创建一个新的String对象

二，String做参数，传递的是String的引用

三，String具有只读性，任何引用都不能改变它的值

四，“+”操作符，对字符串进行连接，实际上每一次的连接都是创建一个新对象。再和下一个连接，很消耗内存，程序性能不好，所以就有了`StringBulider`类



常用方法

```java
replaceAll("被替换的字符",""):被替换的字符使用的是正则表达式
split(String string) ：分割字符串
substring(int n):分割出索引之后的全部字符串部分
subString(int start,int end):分割部分字符串
```



**Pattern类和Marcher对象**

在JDK 1.4中，Java增加了对正则表达式的支持

java与正则相关的工具主要在`java.util.regex`包中；此包中主要有两个类：Pattern、Matcher

​														

**`StringBuffer类`**

当对字符串进行修改的时候，需要使用 `StringBuffer` 和 `StringBuilder` 类

和 `String` 类不同的是，`StringBuffer` 和 `StringBuilder` 类的对象能够被多次的修改，并且不产生新的未使用对象。

`StringBuilder` 类在 Java 5 中被提出，它和 `StringBuffer` 之间的最大不同在于 `StringBuilder` 的方法不是线程安全的（不能同步访问）。

由于 `StringBuilder` 相较于 `StringBuffer` 有速度优势，所以多数情况下建议使用 `StringBuilder` 类。

然而在应用程序要求线程安全的情况下，则必须使用 `StringBuffer` 类。





**`StringBulider类`**

一，不论进行几次修改，都只是创建一个`StringBulider`对象

二，`StringBulider`就是基于String不可变的特点出来的

三，显示的创建`StringBulider`还可以预先指定大小，避免措辞重新分配缓存

四，`StringBulider`常用方法



**三个类的比较**

**一，性能**

```
StringBuilder > StringBuffer > String
```

**二，何时使用**

```
String适用于少量的字符串操作的情况

StringBuilder适用于单线程下在字符缓冲区进行大量操作的情况 

StringBuffer适用多线程下在字符缓冲区进行大量操作的情况
```

**`toSting`方法**

一，将`StringBuffer`对象和`StringBulider`对象转换为String对象



**正则表达式**

​																																																																																																																																																																																																																																																																																						

###### 6，`boolean`



7，数据类型的初始值

![img](E:\有道云笔记\新建文件夹\qqA18D73086F0D435363C26D86B8CAD5E6\85c92a00f6234b30add5c47cc6e5cd56\z2xc7zsr.bmp)

附加：String的初始值是null，局部变量没有默认值





### 二，转移字符



|        |      |        |
| ------ | ---- | ------ |
| \b     | 退格 | \u0008 |
| \t     | 制表 | \u0009 |
| \n     | 换行 | \u000a |
| \r     | 回车 | \u000d |
| 双引号 |      | \u0022 |
| 单引号 |      | \u0027 |
| 反斜杠 |      | \u005c |



### 三，变量

变量命名规则：必须为合法的标识符,变量名必须是一个以字母开头，并且由字母或数字组成的序列

> 标识符由任意的字母，下划线，美元符号，数字组成，并且第一个不能为数字，也不能用关键字
>
> 不建议使用美元符



#### 实例变量

1，定义在类中，方法体外的变量

2，也称为对象变量，即没有加static的变量，在创建对象的时候就实例化了



成员变量（归属于实例变量）

1，可以被整个类访问，以及方法，构造方法，特定类的语句块

2，成员变量有默认值，即在分配了内存空间后所有成员变量会初始化，没有赋值的会给成员变量对应类型的值，数据类型不同则默认值不同



#### 局部变量

1，在**方法、构造方法或者语句块中定义的变量**被称为局部变量

2，**方法结束后，变量就会自动销毁**

3， 用的时候是直接入栈的，如果没有赋值，这个变量就没有初始值，也就无法操作，**所以局部变量要初始化**



#### 类变量

1，独立于方法之外的变量，**用 static 修饰**，类变量也叫**静态变量**

2，**静态方法只能访问静态成员变量**

3，**静态成员可以被该类所有方法访问**



#### final变量

1，用final修饰的变量

2，如果是**基本数据类型的变量**，则其数值**一旦在初始化之后便不能更改**；

3，如果是**引用类型的变量**，则在对其**初始化之后便不能再让其指向另一个对象**



final方法

1，声明 final 方法的主要目的是**防止该方法的内容被修改**，类中的 final 方法可**以被子类继承，但是不能被子类修改**



额外知识点

> 堆区：只存放类对象（类中的成员变量），线程共享；
>
> 方法区：又叫静态存储区，存放class文件和静态数据，线程共享;
>
> 栈区：存放方法局部变量，基本类型变量区、执行环境上下文、操作指令区，线程不共享;       



### 四，常量

1，使用关键字final指示

2，常量通常用大写

3，不能修饰抽象类，因为抽象类一般都是需要被继承的，final修饰后就不能继承了



常量分为编译期常量和非编译期常量

​	编译期常量：在程序编译阶段【不需要加载类的字节码】，就可以确定常量的值

​	非编译期常量：在程序运行阶段【需要加载类的字节码】，可以确定常量的值

> ```java
> static final int c = 0;//编译期常量，不需要类加载
> 
> static final Integer d = new Integer(2);//非编译期常量，需要类加载，报错！！
> ```



### 五，**数值间的转换次序**

 **规则**：不是基本数据类型可以任意转换，**占位少的可以转换为占位多的(自动)**，占位多的需要通过强制类型转换，**导致数据丢失**



**即**：**`double`>`float`>`long`>`int`>`short`>`char`**



**信息丢失问题**

当两个不同类型的数进行基本运算符操作时，精度小的数值类型首先会向精度大的数值类型进行转换，超出的高位部分将被丢弃



**转换规则**

1.类型范围小的转换为大的

2.明确类型的表达式，会根据表达式的大小转换

3.都是数字的，转换为范围大的





### 六，流程控制

```
if……else
while（）
do……while（）
switch（）
```

注意：

1，switch语句后的控制表达式只能是`short`、`char`、`int`、`byte`整数类型和枚举类型，不能是`float`，`double`和`boolean`类型。

2，**String类型是java7开始支持**



### 八，循环

```
for循环，for……each循环，while循环
```

**执行顺序**:

1. 初始化语句, 仅在循环开始前执行一次;
2. 布亇表达式, 用于决定是否继续执行正文过程, 表达式中异常则结束循环;
3. 正文过程, 如果过程中存在break, return或者异常, 循环结束(不会执行更新语句), 如果遇到continue, 则会执行更新语句后进入下一轮循环;
4. 更新语句, 注意更新语句不做逻辑真假判断, 到这里一轮循环结束;
5. 布亇表达式, 进入新一轮循环;



### 九，数组

声明方式

```
int data[];
int[] data;
```

数组的初始化

```
int[] a = new int[5];
int a[] = new int[5];
int a[][] = new int[7][];
int a = new int[]{1,2,3,4}
```



### 十，运算符

**算术运算符**

| /         | 取整数                                              |
| --------- | --------------------------------------------------- |
| %         | 取余数                                              |
| +         |                                                     |
| -         |                                                     |
| *         |                                                     |
| 自增/自减 | a++	:	运行完了后再加<br>++a ： 加完了后再运行 |

**注意**：数据类型的转化

1.int类型+String类型：转化为String类型



**关系运算符**

==，!=，>，<，>=，<=



**位运算符**

位运算符的运算是在二进制的基础上进行的

A=60	转化为二进制：0011 1100

B=13	转化为二进制：0000 1101

| ＆   | 如果相对应位都是1，则结果为1，否则为0        | A＆B：12,即0000 1100      |
| ---- | -------------------------------------------- | ------------------------- |
| \|   | 如果相对应位都是 0，则结果为 0，否则为 1     | A \| B：61,即 0011 1101   |
| ^    | 如果相对应位值相同，则结果为0，否则为1       | A ^ B：49,即 0011 0001    |
| 〜   | 对二进制的每一位数进行翻转                   | 〜A：-61,即1100 0011      |
| <<   | 按位左移运算符，向左移指定位数               | A << 2：240，即 1111 0000 |
| >>   | 按位右移运算符。向右移指定位数               | A >> 2：15即 1111         |
| >>>  | 按位右移补零操作符，移动得到的空位以零填充。 | A>>>2：15即0000 1111      |



 **注意**：当 `&` 和 `|` 用于布尔值，得到的也是布尔值，在得到结果前，会计算两个操作数的值



**逻辑运算符**

| &&    |      | A && B为假    |
| ----- | ---- | ------------- |
| \| \| |      | A \| \| B为真 |
| ！    |      | A && B为真    |

**短路逻辑运算符**

当使用与逻辑运算符时，在两个操作数都为true时，结果才为true，但是当得到第一个操作为false时，其结果就必定是false，这时候就不会再判断第二个操作了



**赋值运算符**

| =       |                    | C = A + B将把A + B得到的值赋给C          |
| ------- | ------------------ | ---------------------------------------- |
| + =     |                    | C + = A等价于C = C + A                   |
| - =     |                    | C - = A等价于C = C - A                   |
| * =     |                    | C * = A等价于C = C * A                   |
| / =     |                    | C / = A，C 与 A 同类型时等价于 C = C / A |
| （％）= |                    | C％= A等价于C = C％A                     |
| << =    | 左移位赋值运算符   | C << = 2等价于C = C << 2                 |
| >> =    | 右移位赋值运算符   | C >> = 2等价于C = C >> 2                 |
| ＆=     | 按位与赋值运算符   | C＆= 2等价于C = C＆2                     |
| ^ =     | 按位异或赋值操作符 | C ^ = 2等价于C = C ^ 2                   |
| \| =    | 按位或赋值操作符   | C \| = 2等价于C = C \| 2                 |



**条件运算符（?:）**

条件运算符也被称为三元运算符

```java
variable x = (expression) ? value if true : value if false
```

**注意！**

当三元操作遇到可以转换为数字的类型，会自动做类型提升

```java
Object a = true ? Interge(1) : Double(2);
```

实际上会自动转换为

```java
Object a = true ? Double(1) : Double(2);
```

也就是输出 

```java
a = 1.0
```



**`instanceof` 运算符**

该运算符用于操作对象实例，检查该对象是否是一个特定类型（类类型或接口类型）

```java
String name = "James"; 
boolean result = name instanceof String;  // 由于 name 是 String 类型，所以返回真
```



**额外注意**：**Java运算符优先级**

- ! > ~ > 二元运算 >一元运算 
- \* > / >%
- <<大于>>大于>>>
- `<` 大于 `<=` 大于` >` 大于 `>=` 大于`instanceof`



### 十一，递归

递归：指在当前方法内调用自己 

递归的分类: 递归分为两种，直接递归和间接递归



 直接递归称为方法自身调用自己

 间接递归可以A方法调用B方法，B方法调用C方法，C方法调用A方法



 **注意事项**： 递归一定要有条件限定，保证递归能够停止下来，否则会发生栈内存溢出



 在递归中虽然有限定条件，但是递归次数不能太多。否则也会发生栈内存溢出。

 构造方法,禁止递归

```
Exception in thread "main" java.lang.StackOverflowError
```



### 十二，权限修饰符

| 修饰符    | 类中 | 同一个包中 | 子类中 | 任何地方 |
| --------- | ---- | ---------- | ------ | -------- |
| public    | YES  | YES        | YES    | YES      |
| protected | YES  | YES        | YES    |          |
| default   | YES  | YES        |        |          |
| private   | YES  |            |        |          |



注意：

1，**声明时没有写访问修饰符默认是default**，**default不能修饰变量**



## @字符编码和字符集

**字符编码**（Character Encoding）: 就是一套自然语言的字符与二进制数之间的对应规则。

计算机中储存的信息都是用二进制数表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果



按照某种规则，将字符存储到计算机中，称为**编码**

反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码**。



**编码表**：生活中文字和计算机中二进制的对应规则

**字符集**：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符号、数字等。

> 常见字符集有ASCII字符集、GBK字符集、Unicode字符集
>
> Windows系统中创建的文本文件时，由于Windows系统的默认是GBK编码，就会出现乱码



String类的构造方法提供了把字节数组转换成字符的方法

`public String(byte[] bytes)`

`public String(byte[] bytes, String charsetName)`

同时提供了`public byte[] getBytes()`和`public byte[] getBytes(String charsetName)`方法来把字符串转换为字节数组，有参方法用来指定字符集。

```java
// 编码和解码要统一
byte[] bytes = "我真的是个好人".getBytes("utf-8");
String s = new String(bytes, "utf-8"
```



## @对象和类

### 一，面向对象程序设计（OOP）

- 多态
- 继承
- 封装
- 抽象
- 类
- 对象
- 实例
- 方法
- 重载

**面向对象程序的特性：封装性，多态性，继承性，（抽象性)**

**面向对象的五大基本原则：单一职责原则，开放封闭原则，里氏替换原则，接口隔离原则，依赖倒置原则**



**面向对象的语言的优点**：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

**缺点**：性能比面向过程低 



**面向过程的语言优点**：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源

**缺点**：没有面向对象易维护、易复用、易扩展



JAVA语言的特点（在《java程序设计习题精编》）

1.简单性：

2.面向对象：

3.分布式：java程序能通过url打开或者访问网络上的对象，就像访问本地文件

4.健壮：强类型，异常处理，内存自动回收

5.安全性：java有一个安全防范机制

6.体系结构中立：Java编译器通过生成与特定的计算机体系结构无关的字节码指令来实现这一特性

7.可移植：java的基本数据类型与硬件平台无关

8.解释型：Java解释器可以在任何一只了解释器的机器上执行Java字节码

9.高性能：

10.多线程：

11.动态:



### 二，类

#### 概述

1，类是构造对象的模板或蓝图，它描述一类对象的行为和状态。

2，创建对象的过程——是类的实例化

3，所有的类都是“Object类”的子类



#### 类的分类

1，基本类和不可变类

2，基本类分为普通类，抽象类，接口

- 普通类：使用 class 定义且不含有抽象方法的类
- 抽象类：使用 abstract class 定义的类，它可以含有或不含有抽象方法
- 接口：使用 interface 定义的类



#### 类的三种关系

依赖（uses-a）：一个类的方法操纵另一个类的方法，我们就说一个类依赖于另一个类

（尽可能的减少这种依赖关系的类——即减少类之间的耦合度）

聚合（has-a）：聚合关系意味着一个类的对象包含另一个类的对象

  聚合也可以称为“关联”，这种关系的标准符号不易区分

继承（is-a）：



**注意**：

  1.并不是所有的类都具有面向对象特征



### 三，对象

封装——将数据和行为组合在一个包中，并对对象的使用者隐藏了数据的实现方式

对象中的数据称为——实例域

操纵数据的过程称为——方法

#### 对象的三个主要特征

- 对象的行为（是可调用的方法定义的）
- 对象的状态（保留着描述当前特征的信息，状态的改变必须通过调用方法实现）
- 对象标识

#### 对象的使用

概念一：构造器，使用构造器构造新的实例，是一种特殊的方法，用来构造并初始化对象

创建对象需要以下三步：

- **声明**：声明一个对象，包括对象名称和对象类型。
- **实例化**：使用关键字new来创建一个对象。
- **初始化**：使用new创建对象时，会调用构造方法初始化对象



### 四，方法

 方法是用于操作对象以及存取他们的实例域

#### 解刨方法

方法的第一个参数为隐式参数，是出现在方法名前的类对象

方法的第二个参数是在括号中的数值，称为显式参数

在每一个方法中，关键字this表示隐式参数





#### 构造方法

- 每个类都有构造方法。
- 如果没有显式地为类定义构造方法（没有定义任何构造函数），Java编译器（JVM）将会为该类提供一个默认构造方法。

#### 私有方法

不会被外部的其他类操作调用



#### 静态方法

静态方法是一种不能面向对象实施操作的方法，没有隐式参数，即没有this参数，没有实例域（因为不能操作对象），但是可以访问自身类中的静态域，也可以称为类方法



一般通过类名调用静态方法，可以使用对象调用静态方法（ 不推荐），但需要是该类的对象，通常在工具类中使用类调用静态方法



  **使用静态方法的情况**：

​    一个方法不需要访问对象的状态，所需要的参数都是显式参数提供

​    一个方法只需要访问类的静态域



#### 方法参数

java总是采用按值调用

```java
public static void reset(int value) {
        value = 0;
    }

    public static void main(String[] args) {
        int value = 100;
        System.out.println(value);//100

        reset(value);
        System.out.println(value);//100
    }
```

####  封装

  **概念**

  封装（Encapsulation）是指一种将抽象性函式接口的实现细节部份包装、隐藏起来的方法

  封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问

  要访问该类的代码和数据，必须通过严格的接口控制

  封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段

  适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性

  

**实现Java封装的步骤：**

1. 修改属性的可见性来限制对属性的访问（一般限制为private）

```java
private String name; 
private int age;
```

2. 对每个值属性提供对外的公共方法访问，也就是创建一对赋取值方法，用于对私有属性的访问

```java
public int getAge(){   
    return age;   
}  
public String getName(){   
    return name;   
} //以下是对外的公共访问，访问类中的私有属性   
public void setAge(int age){    
    this.age = age;    
}   
public void setName(String name){    
    this.name = name;    
}
```

采用 **this** 关键字是为了解决实例变量（private String name）和局部变量（setName(String name)中的name变量）之间发生的同名的冲突



**条件**：

1.一个私有的数据域

2.一个公共的域访问器方法（get方法）

3.一个公有的域更改器方法（set方法）



## @继承和多态

### 一，继承

 继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为



> 子类又被称为派生类
>
> 父类又被称为超类



#### 关键字

 继承可以使用 **extends** 和 **implements** 这两个关键字来实现继承



**注意：**

继承关系之中，如果要实例化子类对象，会默认先调用父类构造，为父类之中的属性初始化，之后再调用子类构造，为子类之中的属性初始化

即：默认情况下，子类会找到父类之中的无参构造方法。

现在默认调用的是无参构造，而如果这个时候父类没有无参构造，则子类必须通过super()调用指定参数的构造方法



#### 继承类型

![img](E:\有道云笔记\新建文件夹\qqA18D73086F0D435363C26D86B8CAD5E6\b81695219f214116b773a0ee8c5831d7\dfm4byfk.bmp)



#### 继承特性

- 子类拥有父类非 private 的属性、方法
- 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展
- 子类可以用自己的方式实现父类的方法
- 提高了类之间的耦合性（继承的缺点，耦合度高就会造成代码之间的联系越紧密，代码独立性越差）



#### super 与 this 关键字

```java
class A{
    public void a(){ b() }
    public void b(){     }
}

class B extend A {
    public void a(){
        super.a();
    }
    public void b(){}
}

```



额外注意：

1，只要被子类重写的方法，不被super调用都是调用子类方法

2，**super关键字必须在构造方法的第一行 ，子类构造函数第一行默认就是super()**

```java
public class A {
    public A(){
        System.out.println("A");
    }
    public static void main(String[] args) {
        new A().a();
    }
}
----------------------------------------------------------
public class B  extends  A{
    public B(){
        System.out.println("B");
    }
    public static void main(String[] args) {
        new B().a();
    }
}
//输出的是A B
//如果父类中包含有参构造器，却没有无参构造器，则在子类构造器中一定要使用“super(参数)”指定调用父类的有参构造器，不然就会报错
```

3，final 关键字声明类可以把类定义为不能继承的，即最终类；或者用于修饰方法，该方法不能被子类重写



#### 构造方法

1.构造方法可以进行重载，但是参数列表必须不相同，不可以返回值和访问级别进行区分

2构造方法没有返回值

3.构造方法一定要与定义为public的类同名

4.构造方法不能被对象调用，只会创建对象，使用new关键字



#### 重载与重写



重载



表示同一个类中可以有

- 名称相同
- 方法的参数列表各不相同（即参数个数和类型不同）																					



注意

1 ，在使用重载的时候只能通过不同的参数样式

2 ，不能通过访问权限，返回类型，抛出的异常进行重载

3 ，方法的异常类型不会对重载造成影响

4 ，对于继承来说，如果父类的访问权限是private，那么就不能在子类中进行重载，定义的话，也是相当于在子类中增加了一个新的方法

5，方法的返回值与重载无关





重写																																																																																					

- 参数相同
- 把父类中定义的那个完全相同的方法给覆盖了

 

注意

1，如果父类的方法的类型的是**private类型**，那么，子类则**不存在覆盖的限制**，则相当于子类增加了一个全新的方法。

2，覆盖的方法的标志必须要和被覆盖的**方法的标志完全匹配**，才能达到覆盖的效果。

3，覆盖的方法的返回值必须和被覆盖方法的**返回值一致**。

4，覆盖的方法所抛出的异常必须和被覆盖方法的**所抛出的异常一致**，或者是其异常的子类。

5，子类的访问权限只能比父类大，不能比父类小。



#### 子类与父类的代码块的执行顺序

父类的静态->子类的静态->父类的非静态->父类的构造->子类的非静态->子类的构造



**类的加载顺序**

(1) 父类静态代码块(包括静态初始化块，静态属性，但不包括静态方法)

(2) 子类静态代码块(包括静态初始化块，静态属性，但不包括静态方法 )

(3) 父类非静态代码块( 包括非静态初始化块，非静态属性 )

(4) 父类构造函数

(5) 子类非静态代码块 ( 包括非静态初始化块，非静态属性 )

(6) 子类构造函数



### 二，多态

多态是同一个行为具有多个不同表现形式或形态的能力。

多态的特征是表现出多种形态，具有多种实现方式。或者多态是具有表现多种形态的能力的特征。或者同一个实现接口，使用不同的实例而执行不同的操作。

  

作用:

  1，可以增强程序的可扩展性及可维护性，使代码更加简洁。

  2，不但能减少编码的工作量，也能大大提高程序的可维护性及可扩展性



#### 三个必要条件

继承，重写，父类引用指向子类对象

当使用多态方式调用方法时，**首先检查父类中是否有该方法**，如果没有，则编译错误；如果有，再去调用子类的同名方法



#### 实现方式

方式一：重写

方式二：接口

方式三：抽象类和抽象方法



#### 抽象类（abstract）

在面向对象的概念中，所有的对象都是通过类来描绘的，但是反过来，**并不是所有的类都是用来描绘对象的**，如果**一个类中没有包含足够的信息来描绘一个具体的对象**，这样的类就是**抽象类**。



特点：

1，不能实例化对象

2，抽象类必须被继承



#### 抽象方法

**抽象方法只包含一个方法名，而没有方法体**

抽象方法没有定义，**方法名后面直接跟一个分号**，而不是花括号。



注意

- **如果一个类包含抽象方法，那么该类必须是抽象类**
- **任何子类必须重写父类的抽象方法**，**或者声明自身为抽象类**



## @序列化

### 一，定义

Java序列化就是指把Java对象转换为字节序列的过程

Java反序列化就是指把字节序列恢复为Java对象的过程



> 用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息
>
> 字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息
>
> 反之，该字节序列还可以从文件中读取回来，重构对象，对它进行反序列化，对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象



一个对象要想序列化，必须满足两个**条件**:

一：该类必须实现`java.io.Serializable ` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。

二：该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。



### 二，作用

序列化

1，在传递和保存对象时.保证对象的完整性和可传递性

2，对象转换为有序字节流,以便在网络上传输或者保存在本地文件中

反序列化

1，根据字节流中保存的对象状态及描述信息，通过反序列化重建对象



**总结：**序列化机制的核心作用就是对象状态的保存与重建



**例子：**

**1，**json/xml的数据传递

**2，**Serializable接口

Serializable是Java提供的序列化接口，是一个空接口，**为对象提供标准的序列化与反序列化操作**



**实现方式**

①若Student类仅仅实现了**Serializable接口**，则可以按照以下方式进行序列化和反序列化。

> `ObjectOutputStream`采用默认的序列化方式，对Student对象的非transient的实例变量进行序列化。     
>
> `ObjcetInputStream`采用默认的反序列化方式，对Student对象的非transient的实例变量进行反序列化。

 

②若Student类仅仅实现了Serializable接口，并且还定义了`readObject(ObjectInputStream in)`和`writeObject(ObjectOutputSteam out)`，则采用以下方式进行序列化与反序列化。

> `ObjectOutputStream`调用Student对象的`writeObject(ObjectOutputStream out)`的方法进行序列化。
>
> `ObjectInputStream`会调用Student对象的`readObject(ObjectInputStream in)`的方法进行反序列化。

 

③若Student类实现了`Externalnalizable`接口，且Student类必须实现`readExternal(ObjectInput in)`和`writeExternal(ObjectOutput out)`方法，则按照以下方式进行序列化与反序列化

> `ObjectOutputStream`调用Student对象的`writeExternal(ObjectOutput out))`的方法进行序列化。 
>
> `ObjectInputStream`会调用Student对象的`readExternal(ObjectInput in)`的方法进行反序列化。

## @内部类

即定义在一个类的内部

```JAVA
public class A {
     class B{}
}
```

1，可以定义非静态属性和方法，不可以定义static修饰的属性和方法，可以定义static final修饰的编译期变量，除非用static修饰这个内部类

> 为什么不可以定义static修饰的属性和方法？
>
> ​	首先内部类是外部类的一个成员，只有当外部类初始化的时候，内部类才能初始化，静态变量属于类级别，在类加载的时候就初始化
>
> ​	但是可以使用 static final 修饰的常量，即编译期常量



### 一，成员内部类

1，可以无条件的访问外部类的所有成员属性和成员方法（包括private和静态成员）

方式：

1-直接写属性名，其实本质还是外部类.this.属性

```java
private int aa = 1;
    class Inner{
        public void get(){
            System.out.println(aa);
        }
    }
//等同于
		public void get(){
            System.out.println(外部类.this.aa);
        }
//这种写法适用于当需要访问和外部类同名的成员时
```

反编译后的源码

```java
public class Outter
{
    private int a;
    
    public Outter() {
        this.a = 3;
    }
    
    class Inner
    {
        public void get() {
            System.out.println(Outter.this.a);
        }
    }
}
```



2，外部类访问内部类时需要创建成员内部类对象做引用

```
外部类.内部类 in = new 外部类().new 内部类();
```

3，内部类具有访问private，protected的权限



### 二，局部内部类

定义：定义在方法中的内部类



1、内部类不能被public、private、static修饰；

2、在外部类中不能创建内部类的实例；

3、内部类访问包含他的方法中的变量必须有final修饰；

4、外部类不能访问局部内部类，只能在方法体中访问局部内部类，且访问必须在内部类定义之后。



> 为什么必须有final修饰呢？



 首先需要知道的一点是:内部类和外部类是处于同一个级别的,内部类不会因为定义在方法中就会随着方法的执行完毕就被销毁.

这里就会产生问题：当外部类的方法结束时，局部变量就会被销毁了，但是内部类对象可能还存在(只有没有人再引用它时，才会死亡)。这里就出现了一个矛盾：内部类对象访问了一个不存在的变量。为了解决这个问题，就将局部变量复制了一份作为内部类的成员变量，这样当局部变量死亡后，内部类仍可以访问它，实际访问的是局部变量的”copy”。这样就好像延长了局部变量的生命周期

### 三，匿名内部类

1，唯一没有构造器的类

2，一般使用匿名内部类编写监听事件的代码

3，不能有访问修饰符和static修饰符

```java
public class O {
    class A{}
    public void a(A a){
        new Inner().get();
    } 
    public void b(){
        O o = new O();
        o.a(new A());
    }
}

```



### 四，静态内部类

```java
 static class ss{
        public void get(){
            System.out.println(aa);
        }
    }
```

1，不需要依赖于外部类

2，不能使用外部类非static的成员和方法	



## @包装类

Java中的基本数据类型没有方法和属性，而包装类就是为了让这些拥有方法和属性

### 一，分类

每个基本数据类型都对应着一个包装类

### 二，转换

装箱：基本数据类型转换为包装类

```java
int i = 1;
//自动装箱
Intager ii = i;
//手动装箱
Intager ii = new Intager(i);
```



拆箱：包装类转换为基本数据类型

```java
Intager i = 1;
//自动拆箱
int ii = i;
//手动拆箱
int ii=i.intValue();
```



### 三，常用的方法

```java
Integer.toString()//将整型转换为字符串
Integer.parseInt()//将字符串转换为int类型
valueOf()//方法把字符串转换为包装类然后通过自动拆箱
```





## @事件处理

### 一，概念

**事件**：用户对组件的一个操作，称之为一个事件

**事件源：**发生事件的组件就是事件源

**事件监听器（处理器）**：监听并负责处理事件的方法



### 二，执行顺序

1、给事件源注册监听器

2、事件被触发

3、组件产生一个相应的事件对象，并把此对象传递给与之关联的事件监听器

4、事件处理器执行相关的代码来处理该事件



### 三，使用

匿名监听器

```java
button.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent actionEvent) {

            }
        });
```



监听器

```java
public class mylistener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent actionEvent) {
        System.out.println("我是监听器");
    }
}


button.addActionListener(actionEvent -> new mylistener());
button.addActionListener(new mylistener());
```



### 四，分类

动作事件（`ActionEvent`）

接口：`ActionListener`



焦点事件（`FocousEvent`）

接口：`FocusListener`



键盘事件（`KeyEvent`）

接口：`KeyListener`



鼠标事件

接口：`MouseListener`



窗口事件



选项事件

表格模型事件



## @异常

**异常** ：指的是程序在执行过程中，出现的非正常的情况，最终**会导致JVM的非正常停止**

**注意**：	

1.在Java等面向对象的编程语言中，**异常本身是一个类**，产生异常就是创建异常对象并抛出了一个异常对象

2.java处理异常的方式是中断处理

3.异常指的并不是语法错误,语法错了,编译不通过,不会产生字节码文件,根本不能运行.



异常产生过程

1. 方法处发生异常
2. JVM会产生一个异常对象：包括异常的名称，内容，产生位置
3. JVM将异常发送给方法的调用者（调用者无法处理，又会向上一级发送异常对象，即JVM）
4. JVM接受到异常对象，打印对象信息，并终止程序

![img](E:\有道云笔记\新建文件夹\qqA18D73086F0D435363C26D86B8CAD5E6\a06541cd094646bb8347118bde642426\异常产生过程.png)



### 一，分类

**`Throwable`体系：**

异常的根类是**`Throwable`**，其下有两个子类：**Error**与**Exception**

**Error**:严重错误Error，无法通过处理的错误，只能事先避免。

**Exception**:表示异常，异常产生后程序员可以通过代码的方式纠正，使程序继续运行，是必须要处理的



### 二，异常(Exception)的分类

**编译时期异常**:checked异常	

在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)



**运行时期异常**:runtime异常(非检查异常)

在运行时期,检查异常.在编译时期,运行异常不会编译器检测(不报错)。(如数学异常3/0)



### 三，异常处理

Java异常处理的五个关键字：**try、catch、finally、throw、throws**



**抛出异常throw**

**用在方法内**，用来抛出一个异常对象，将这个异常对象传递到调用者处，**并结束当前方法的执行**

```
使用格式：
throw new 异常类名(参数)
```

**注意**：

如果产生了问题，我们就会throw将问题描述类即异常进行抛出，**也就是将问题返回给该方法的调用者。**



那么对于调用者来说，有两种处理方法：

1，一种是进行捕获处理

2，另一种就是继续将问题声明出去，使用throws声明处理



**声明异常throws**

运用于方法声明之上,用于表示当前方法不处理异常,而是提醒该方法的**调用者**来处理异常(抛出异常

```
声明异常格式：
修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   }	
```



**捕获异常try…catch**

1. 该方法不处理,而是声明抛出,由该方法的调用者来处理(throws)。

2. 在方法中使用try-catch的语句块来处理异常。
3. catch和finally不能同时省略

```
捕获异常语法如下：
try{
     编写可能会出现异常的代码
}catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}
```

**注意**:

* 这种异常处理方式，要求**多个catch中的异常不能相同**，并且若catch中的多个异常之间有子父类异常的关系，那么**子类异常要求在上面的catch处理**，**父类异常在下面的catch处理**

* **运行时异常**被抛出可以不处理。即不捕获也不声明抛出

* **如果finally有return语句,永远返回finally中的结果,避免该情况**

**（如果try语句中有return，返回的是try语句中的返回值，并且不受catch其他语句影响返回值）**

* 如果父类抛出了多个异常,子类重写父类方法时,抛出和父类相同的异常或者是父类异常的子类或者不抛出异常

* **父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常**。此时子类产生该异常，只能捕获处理，不能声明抛出



### 四，自定义异常

- 所有异常都必须是 Throwable 的子类。
- 如果希望写一个检查性异常类，则需要继承 Exception 类。
- 如果你想写一个运行时异常类，那么需要继承 RuntimeException 类。

```
格式：
class MyException extends Exception{
//定义有参构造方法
    public RegistException(String message) {
        super(message);
    } }
//只继承Exception 类来创建的异常类是检查性异常类。


if(exprl){
    throw new MuException()
}
```





## @Swing

> 不做讨论