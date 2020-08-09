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