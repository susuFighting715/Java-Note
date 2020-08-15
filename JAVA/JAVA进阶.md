# JAVA进阶

> 笔记来源于黑马视频，播客，书籍，以及个人理解

## I/O：输入/输出流

**什么是IO** 

Java中I/O操作：主要是指使用 java.io 包下的内容，进行输入、输出操作

IO流：就是数据传输的通道

流：一组有顺序的，有起点和终点的字节集合，是对数据传输的总称或抽象

输入：也叫做读取数据

输出：也叫做作写入数据 



**分类**

根据数据的流向分为：输入流和输出流

​	输入流 ：把数据从 其他设备 上读取到 内存 中的流。 

​	输出流 ：把数据从 内存 中写出到 其他设备 上的流。

格局数据的类型分为：字节流和字符流

​	字节流 ：以字节为单位，读写数据的流。 

​	字符流 ：以字符为单位，读写数据的流。



**字节流和字符流的区别（知识点）**

1，字节流没有缓冲区，直接输出，字符流有缓冲区。

> 也就是说字节流不需要调用close方法就已经把内容输出了，但是字符流会在调用close或者flush方法时，内容才会输出

 2，读写单位不同

> 字节流以字节（8bit）为单位，字符流以字符为单位，根据码表的不同而不同

3，处理对象不同

> 字节流能处理所有类型的数据，而字符流只能处理字符类型的数据



### 一，File类

Java文件类以抽象的方式代表文件名和目录路径名。该类主要用于文件和目录的创建、文件的查找和文件的删除等

> 其中有很多API方法就不做讨论

```java
File file=new File("test.txt");
//不会在当前目录下创建文件
//但是使用其他流初始化时，没有文件就会在根目录下创建这个文件
```



输出

#### **FileOutputStream** 

FileOutputStream 类是文件输出流，用于将数据写出到文件

> 写入数据的原理：
>
> (内存-->硬盘)
>
> java程序-->JVM(java虚拟机)-->OS(操作系统)-->OS调用写数据的方法-->把数据写入到文件中

构造方法 ：

```java
public FileOutputStream(File file) ：创建文件输出流以写入由指定的File对象表示的文件

public FileOutputStream(String name) ： 创建文件输出流以指定的名称写入文件 
```

**注意：**

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有这个文件，会创建该文件。如果有这个文件，会清空这个文件的数据，需要标明追加数据



**笔记**：（需要验证正确与否）

写入一个字节，在实际的文件中，显示的却是ASCLL码

```java
FileOutputStream fos = new FileOutputStream(file); 
fos.write(33);//写入文件的是感叹号（！）
```

验证是正确的，写入的是对应ASCLL码的字符





#### **FileWriter** 

java.io.FileWriter 类是写出字符到文件的便利类

> 构造时使用系统默认的字符编码和默认字节缓冲区

构造方法 

```java
FileWriter(File file) ： 创建一个新的FileWriter，给定要读取的File对象 	

FileWriter(String fileName) ： 创建一个新的 FileWriter，给定要读取的文件的名称 
```





输入

#### **FileInputStream** 

java.io.FileInputStream 类是文件输入流，从文件中读取字节

构造方法 

```java
public FileInputStream(File file)
```

 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的File对象ﬁle命名

```
 FileInputStream(String name) 
```

通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名name命名 

**注意：**

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有该文件,会抛出 FileNotFoundException ，不同于输出流

当文件读取到结尾时返回 -1,循环结束



#### **FileReader**

java.io.FileReader 类是读取字符文件的便利类,是一个抽象类

> 构造时使用系统默认的字符编码和默认字节缓冲区

**字符编码**：字节与字符的对应规则

> Windows系统的中文编码默认是GBK编码表。
>
> idea中UTF-8编码表

**字节缓冲区**：一个字节数组，用来临时存储字节数据。

**构造方法** :

```java
FileReader(File file) ： 创建一个新的 FileReader ，给定要读取的File对象  

FileReader(String fileName) ： 创建一个新的 FileReader 给定要读取的文件的名称 
```



#### **数据追加**

在测试的时候，每次程序运行，创建输出流对象，都会清空目标文件中的数据。

```java
public FileOutputStream(File file, boolean append) //文件输出流以写入由指定的 File对象表示的文件。 
public FileOutputStream(String name, boolean append) //创建文件输出流以指定的名称写入文件。
```

 这两个构造方法，参数中都需要传入一个boolean类型的值， true 表示追加数据， false 表示清空原有数据

 这样创建的输出流对象，就可以指定是否追加续写了





### 二，字节流

**一切皆为字节**

一切文件数据(文本、图片、视频等)在存储时，都是以二进制数字的形式保存，都一个一个的字节，那么传输时一 样如此。所以，字节流可以传输任意文件数据。在操作流的时候，无论使用什么样的流对象，底层传输的始终为二进制数据



#### **字节输出流（OutputStream ）**

java.io.OutputStream 抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地

它定义了字节输出流的基本共性功能方法

```java
public void close() 关闭此输出流并释放与此流相关联的任何系统资源 		
public void flush() 刷新此输出流并强制任何缓冲的输出字节被写出
public void write(byte[] b) 将 b.length字节从指定的字节数组写入此输出流
public void write(byte[] b, int off, int len) ：
//从指定的字节数组写入 len字节，从偏移量oﬀ开始输出到此输出流
public abstract void write(int b) ：将指定的字节输出流
```

**写入字节数据** 

1. 写出字节： write(int b) 方法，每次可以写出一个字节数据

2. 写出字节数组： write(byte[] b) ，每次可以写出数组中的数据

3. 写出指定长度字节数组： write(byte[] b, int off, int len) ,每次写出从oﬀ索引开始,len个字节



#### **字节输入流（InputStream）**

InputStream 抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中

它定义了字节输入 流的基本共性功能方法

```java
public void close() ：关闭此输入流并释放与此流相关联的任何系统资源。   
public abstract int read() ： 从输入流读取数据的下一个字节。
public int read(byte[] b) ： 从输入流中读取一些字节数，并将它们存储到字节数组b中
```

> 读取数据的原理(硬盘-->内存)
>
> ​        java程序-->JVM-->OS-->OS读取数据的方法-->读取文件



**笔记：**

读取多个字节时，需要一个循环的过程，因为不清楚文件中有多少字节，数组在不确定的情况下不知道能不能装的下文件中的字节，read（）方法读取到一个字节就会返回，一般需要一个循环条件，作为结束的标志

```java
//模板
byte[] bytes = new byte[2024];
int length = 0;
InputStream is = new InputStream(file);
while ((length = is.read(bytes))!=-1){
	System.out.println(new String(bytes, 0, length));
}
is.close();
```

**注意：**read方法在读取了一个字节后，会将指针移动到下一个字节后

​    并且在流的末端返回-1作为结束标志





### 三，字符流

当使用字节流读取文本文件时，可能会有一个小问题。就是遇到中文字符时，可能不会显示完整的字符，那是因为 一个中文字符可能占用多个字节存储。所以Java提供一些字符流类，以字符为单位读写数据，专门用于处理文本文件。

UTF-8：一个中文占用三字节

GBK:     一个中文占用2字节



#### **字符输入流（Reader）** 

java.io.Reader 抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中

它定义了字符输入流的基本共性功能方法

```java
public void close() ：关闭此流并释放与此流相关联的任何系统资源
public int read() ： 从输入流读取一个字符
public int read(char[] a) ： 从输入流中读取一些字符，并将它们存储到字符数组a中
```



#### **字符输出流（Writer）** 

java.io.Writer 抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地

它定义了字节 输出流的基本共性功能方法

```java
void write(int c) 写入单个字符。 
void write(char[] cbuf) 写入字符数组。 
abstract void write(char[]a, int off, int len)
 写入字符数组的某一部分,a数组的开始索引,len 写的字符个数
void write(String str)
写入字符串
void write(String str, int off, int len) 
写入字符串的某一部分,oﬀ字符串的开始索引,len写的字符个数
void flush() 刷新该流的缓冲
void close() 关闭此流，但要先刷新它
```



### 四，打印流

概述

平时我们在控制台打印输出，是调用`print`方法和`println`方法完成的，这两个方法都来自于`java.io.PrintStream`类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。

> 包含   字节打印流：PrintStream    字符打印流：PrintWriter。

#### **PrintStream**

构造方法

```java
public PrintStream(String fileName) ： 使用指定的文件名创建一个新的打印流，即为改变打印流向，将输出的地方转移到fileName的地方
    
//例子
PrintStream ps = new PrintStream("文件地");
System.setOut(ps);//把输出语句的目的地改变为打印流的目的地    
```

其中还有很多printf和print方法，也就是说

```java
System.out.printf()//可以打印任何数据
```



### PrintWriter

专门打印字符

构造

```java
public PrintWriter(String fileName)
```



### 五，缓冲流

**概述**

缓冲流,也叫高效流，是对4个基本的`FileXxx` 流的增强，所以也是4个流，按照数据类型分类：

字节缓冲输入流：BufferedInputStream

字节缓冲输出流：BufferedOutputStream

字符缓冲输出流：BufferedReader

字符缓冲输入流：BufferedWriter

**基本原理**

是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO请求次数，从而提高读写的效率。



**构造方法**

**字节缓冲流**

```java
public BufferedInputStream(InputStream in)：创建一个 新的缓冲输入流 
public BufferedOutputStream(OutputStream out)： 创建一个新的缓冲输出流
```

**字符缓冲流**

```java
public BufferedReader(Reader in)` ：创建一个 新的缓冲输入流
public BufferedWriter(Writer out)`： 创建一个新的缓冲输出流
```

特有的方法;

```java
BufferedReader：public String readLine(): 读一行文字
BufferedWriter：public void newLine(): 写一行行分隔符,由系统属性定义符号
```





### 六，转换流

#### **InputStreamReader**

`java.io.InputStreamReader`，是Reader的子类，是从字节流到字符流的桥梁。

它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集

> 转换流是字节与字符间的桥梁



何时使用转换流？

1. 当字节和字符之间有转换动作时
2. 操作的数据需要编码或解码时



构造方法

```java
InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。 
InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。
```

#### **OutputStreamWriter**

`java.io.OutputStreamWriter` ，是Writer的子类，是从字符流到字节流的桥梁。

使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

构造方法

```java
OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。 
OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。
```



### 七，递归

递归打印多级目录 

分析：多级目录的打印，就是当目录的嵌套。遍历之前，无从知道到底有多少级目录，所以我们还是要使用递归实现。

 文件搜索 

```
搜索 D:\aaa 目录中的 .java 文件。

String中的endWith方法，返回值为boolean

file.getName().endsWith(".java")
```



```java
		File file = new File(pathname);//创建File
        File[] files = file.listFiles();//把当前File对象下所有的文件夹或目录
        for (File file1: files){
            if (file1.isFile()){//递归的终止条件是：当前File为文件
                System.out.println(file1.getAbsolutePath());
            }
            else{//当前File为目录，则，打印当前File的子文件的递归目录
                //String s = file1.getPath();
                //printAllFiles(s);
                //printAllFiles(file1.getPath());
                System.out.println(file1.getAbsolutePath());
                printAllFiles(file1.getAbsolutePath());
            }

        }
```



### 八，序列化和反序列化

代码操作

```java
public class Book implements Serializable {
    private String name;
    private int price;
    public Book(String name , int price){
        this.name = name;
        this.price= price;
    }

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }
}

public static void main(String[] args) {
        File file = new File("test.txt");
        try {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file));
            oos.writeObject(new Book("sb",22));
            oos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```



反序列化

```java
ObjectInputStream ois=new ObjectInputStream(new FileInputStream(new File("e:"+File.separator+"demoA.txt")));
 
Object obj=ois.readObject()
```



### 九，关闭和刷新

因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据 的。如果我们既想写出数据，又想继续使用流，就需要 flush 方法了。

flush ：刷新缓冲区，流对象可以继续使用。 

close :先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了



### 十，Scanner



### 十，新特性

JDK9

```java
FileInputStream fis = new FileInputStream("c:\\1.jpg");
FileOutputStream fos = new FileOutputStream("d:\\1.jpg");
        try(fis;fos){
            int len = 0;
            while((len = fis.read())!=-1){
                //使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
                fos.write(len);
            }
        }catch (IOException e){
            System.out.println(e);
        }
```

JDK7

```java
try(
 FileInputStream fis = new FileInputStream("c:\\1.jpg");
 FileOutputStream fos = new FileOutputStream("d:\\1.jpg");)
 {
            int len = 0;
            while((len = fis.read())!=-1){
                //使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
                fos.write(len);
            }
        }catch (IOException e){
            System.out.println(e);
        }

```

