---
title: CS400-01-JAVA-IO
date: 2020-06-22 21:07:53
tags: JAVA
---
开个新的JAVA 主题，半路出家土和尚，编程相关的课只学过C语言，还是单片机版的， 没学指针啥的。可能比部分文科生或者非 CS 非 ECE 理工科宝宝们学的还要少一点，可以说基本上所有编程都自学的。 

作业要求是这样的，所以我也只写那么多。

 * uses System.out to display text prompts and results to the user console window
 * uses instance of java.util.Scanner connected to the keyboard to accept and use input
 * uses PrintWriter instance to write data and or results to a file.
 * uses instance of a Scanner to read input data from a file.

<!--more-->

### 读取
 Scanner 明明是读写用的却不放在IO里面. 估计是 当成一个工具用了。

System.in 好像据说是java 的标准输入流，就是不太好用，，估计类似于C语言的 fgetc ？ 
然后Scanner 类似于包装好了以后给你直接使用，其读取范围是从空格到空格之间的内容。而读取的类型可以查看文档，查了下是利用正则表达式完成的。 
```
import java.util.Scanner;  // Import the Scanner class

class Main {
  public static void main(String[] args) {

    Scanner myObj = new Scanner(System.in);  // Create a Scanner object
    //Scanner myObj = new Scanner(f);//从文件接收数据

    System.out.println("Enter username");

    String userName = myObj.nextLine();  // Read user input
    System.out.println("Username is: " + userName);  // Output user input
  }
}
```
Scanner 是一个小工具吧，给啥读啥。 给system.in 就从stream 读取，给file 的 hander 就从文件读取。还是比较好理解的。

### PrintWriter
感觉java 是个比较神奇的东西，，，当然也是我比较菜。

java.io,PrintWriter可以用来创建一个文件并向本文文件写入数据
```
import java.io.File;
import java.io.PrintWriter;

public class WriteData {

     public static void main(String[] args){
         File file=new File("num.txt");//文件对象

         PrintWriter fp=new PrintWriter(file);//由PrintWriter 创建fp，并且给file 的 
         \\hander（跟指针差不多吧） 然后用这个hander 进行操作。
         fp.println("hello");
         fp.close();
     }
}

```


