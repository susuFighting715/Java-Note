# JVM

## 字节码

字节码（Byte-code）是一种包含执行程序，由一序列 op 代码/数据对组成的二进制文件，是一种中间码

平时说的java字节码，指的是用java语言编译成的字节码

> 由于JVM跨语言的平台特性，JVM支持很多语言，所以现在通常讲字节码都是jvm字节码，不同的编译器编译出的字节码文件不同



## 虚拟机

所谓虚拟机（Virtual Machine），就是一台虚拟的计算机。它是一款软件，用来执行一系列虚拟计算机指令

大体上，虚拟机可以分为**系统虚拟机**和**程序虚拟机**



## Java虚拟机

Java虚拟机是一台执行Java字节码的虚拟计算机

JVM平台的各种语言可以共享Java虚拟机带来的跨平台性、优秀的垃圾回器，以及可靠的即时编译器

Java技术的核心就是Java虚拟机（JVM，Java Virtual Machine），因为所有的Java程序都运行在Java虚拟机内部

Java虚拟机就是二进制字节码的运行环境，负责装载字节码到其内部，解释/编译为对应平台上的机器指令执行

特点：

- 一次编译，到处运行
- 自动内存管理
- 自动垃圾回收功能



## JVM所在位置

JVM是运行在操作系统之上的，它与硬件没有直接的交互

> 用户使用高级语言编译产生字节码文件，传入到JVM中解析，再产生运行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200713182357407.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTc1OTc5MQ==,size_16,color_FFFFFF,t_70)



## JVM架构模型

Java编译器输入的指令流基本上是一种基于栈的指令集架构，另外一种指令集架构则是基于寄存器的指令集架构。具体来说：这两种架构之间的区别：

基于栈式架构的特点

- 设计和实现更简单，适用于资源受限的系统；
- 避开了寄存器的分配难题：使用零地址指令方式分配。
- 指令流中的指令大部分是零地址指令，其执行过程依赖于操作栈。指令集更小，编译器容易实现，但是指令多。
- 不需要硬件支持，可移植性更好，更好实现跨平台



基于寄存器架构的特点

- 典型的应用是x86的二进制指令集：比如传统的PC以及Android的Davlik虚拟机。
- 指令集架构则完全依赖硬件，可移植性差
- 性能优秀和执行更高效
- 花费更少的指令去完成一项操作。
- 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令、二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主方水洋



总结：

- 跨平台性
- 指令集小
- 指令多
- 执行性能比寄存器差



## JVM生命周期

jvm的启动

Java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的



jvm的执行

- 程序开始执行时他才运行，程序结束时他就停止。
- 执行一个所谓的Java程序的时候，真真正正在执行的是一个叫做Java虚拟机的进程。



jvm的退出

有如下的几种情况：

- 程序正常执行结束
- 程序在执行过程中遇到了异常或错误而异常终止
- 由于操作系统用现错误而导致Java虚拟机进程终止
- 某线程调用Runtime类或system类的exit方法，或Runtime类的halt方法，并且Java安全管理器也允许这次exit或halt操作。
- 除此之外，JNI（Java Native Interface）规范描述了用JNI Invocation API来加载或卸载 Java虚拟机时，Java虚拟机的退出情况。



## JVM家族

### Sun Classic VM

1，早在1996年Java1.0版本的时候，Sun公司发布了一款名为sun classic VM的Java虚拟机，它同时也是世界上第一款商用Java虚拟机，在JDK1.4时完全被淘汰

2，这款虚拟机内部只提供解释器，因此效率比较低

3，如果使用JIT编译器，就需要进行外挂。但是一旦使用了JIT编译器，JIT就会接管虚拟机的执行系统。解释器就不再工作。解释器和编译器不能配合工作

4，现在hotspot内置了此虚拟机



### Exact VM：

Exact Memory Management：准确式内存管理

具备现代高性能虚拟机的维形

1，热点探测（寻找出热点代码进行缓存）

2，编译器与解释器混合工作模式



### HotSpot VM

1，JDK1.3时，HotSpot VM成为默认虚拟机

2，不管是现在仍在广泛使用的JDK6，还是使用比例较多的JDK8中，默认的虚拟机都是HotSpot

3，HotSpot指的就是它的热点代码探测技术。

- 通过计数器找到最具编译价值代码，触发即时编译或栈上替换
- 通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡



### JRockit

1，专注于服务器端应用，JRockit内部不包含解析器实现，全部代码都靠即时编译器编译后执行

2，JRockit JVM是世界上最快的JVM



### IBM的J9

1，全称：IBM Technology for Java Virtual Machine，简称IT4J，内部代号：J9

2，市场定位与HotSpot接近，服务器端、桌面应用、嵌入式等多用途VM广泛用于IBM的各种Java产品

3，目前，有影响力的三大商用虚拟机之一，也号称是世界上最快的Java虚拟机



### KVM和CDC / CLDC Hotspot

- 智能控制器、传感器
- 老人手机、经济欠发达地区的功能手机



### Azul VM

1，Azul VM是Azu1Systems公司在HotSpot基础上进行大量改进，运行于Azul Systems公司的专有硬件Vega系统上的ava虚拟机



### Liquid VM

1，高性能Java虚拟机中的战斗机



### Apache Marmony

### Micorsoft JVM

1，微软为了在IE3浏览器中支持Java Applets，开发了Microsoft JVM

### Taobao JVM

1，基于openJDK开发了自己的定制版本AlibabaJDK，简称AJDK。是整个阿里Java体系的基石。

2，基于openJDK Hotspot VM发布的国内第一个优化、深度定制且开源的高性能服务器版Java虚拟机。

3，taobao vm应用在阿里产品上性能高，硬件严重依赖inte1的cpu，损失了兼容性，但提高了性能

4，目前已经在淘宝、天猫上线，把oracle官方JvM版本全部替换了



### Dalvik VM

1，谷歌开发的，应用于Android系统，并在Android2.2中提供了JIT，发展迅猛。

2，Dalvik y只能称作虚拟机，而不能称作“Java虚拟机”，它没有遵循 Java虚拟机规范



### Graal VM



# 类加载子系统

负责加载Class文件

流程

> 在.class文件->JVM->最终成为元数据模板，此过程就要一个运输工具（类装载器Class Loader），扮演一个快递员的角色。

![img](https://img-blog.csdnimg.cn/20200713183403217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTc1OTc5MQ==,size_16,color_FFFFFF,t_70)

字节码文件通过ClasserLoader加载，产生一个Class文件，然后再通过链接（验证，解析），最后就是初始化，调用类



![img](https://img-blog.csdnimg.cn/20200713183333649.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTc1OTc5MQ==,size_16,color_FFFFFF,t_70)

## 加载阶段

通过一个类的全限定名获取定义此类的二进制字节流

将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构

在内存中生成一个代表这个类的java.lang.Class对象，作为方法区这个类的各种数据的访问入口



## 链接阶段

验证：确保Class文件的字节流中包含信息符合当前虚拟机的要求

主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证



准备：为类变量分配内存并且设置该类变量的默认初始值，零值

> final修饰的static在编译的时候就已经分配了
>
> 
>
> 实例变量不会分配，会随着对象一起分配到java堆中



解析：将常量池捏符号引用转换为直接引用

> 在加载类的时候实际上会去加载很多其他的类，不可能每个加载的类都存放在一个堆中，所以实际上是去使用引用



## 初始化阶段

1，初始化阶段就是执行类构造器方法<clinit>()方法的过程，其不同于类的构造器

2，虚拟机保证一个类的<clinit>()方法在多线程下同步枷锁，也就是说，调用重复创建这个类时只是调用缓存中存在的类对象

```java
public class ClassInitTest {
    private static int num = 1;
    static {
        num = 2;
        number = 20;
        System.out.println(num);
        System.out.println(number);  //报错，非法的前向引用
    }

    private static int number = 10;

    public static void main(String[] args) {
        
        System.out.println(ClassInitTest.num); // 2
        System.out.println(ClassInitTest.number); // 10
    }
}
```

