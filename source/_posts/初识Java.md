---
title: 初识Java
tags: 后端 JavaSE
categories: 后端
thumbnail: http://q2owh35dm.bkt.clouddn.com/3.jpg
date: 2019-12-13 21:30:20
---


## 1. 简介
Java是跨平台语言，运行在虚拟机JVM上，JVM又运行在相应的系统上。
开发我们需要先安装JDK（java delelopment kit），即Java开发工具包，其中包括了JVM，也包括了JRE（java runtime environment）Java运行环境。

JavaSE: 标准版，定义了Java的基本语法。
JavaME：移动版，现已基本不用，用在GPS导航、机顶盒、塞班系统等。
JavaEE: 企业版，如用JavaWeb开发企业级B/S架构应用。

<!-- more --> 

企业级应用常见两种架构：
  B/S架构：broswer server。例如淘宝网
  C/S架构：client server。例如微信、QQ

## 2. 编写Java程序
1）编写：编写Java源代码，即编写".java"的文件。（程序员）
2）编译：将Java源代码编译成计算机能够识别的字节码文件（16进制），文件所在文件夹的命令行中运行`javac 文件名.java`编译该文件,成功后生成'文件名.class'的字节码文件 。（JVM编译器）
3）运行：运行Java程序，命令行中运行`java 文件名`即可，注意不需要带文件名后缀class。（JVM）



```java
public class MyFirstDemo {
  public static void main (String[] args) {
    System.out.print("Hello World!");
  }
}
```

常见错误：
1.每次修改代码都要重新编译。
2.类名与文件名不一致。
3.关键字写错。
4.关键字的顺序写错。

## 3. 开发工具
### eclipse
新建一个Java project（配置name和JRE），之后会生成src（编写自己的class）和JRE System Library（包含很多基础类）。
点击run as java application即可运行，在工作空间文件夹中生成bin（存'.java')和src文件夹(存'.class')。

### IDEA
更好用，代码提示更加智能。



