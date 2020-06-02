---
title: VLSI SDC Commands
date: 2020-06-02 01:29:52
tags: VLSI
---
总结下SDC 方面常用的命令,网上找到一个[UBC的材料](http://www.ece.ubc.ca/~edc/7660.jan2018/lec9.pdf)感觉写的挺好。
此外，[TCL脚本语言](https://www.ibm.com/developerworks/cn/education/linux/l-tcl/l-tcl-blt.html)其实也很重要,应该先总体上过一遍。
    争取每天一个到两个命令详解。一开始会从551ppt上摘录，后面会使用到的更多。
<!--more-->
### Collections
    Collections 

### Timing constrains
* 时钟生成
```
 create_clock -name "clk0" -period 20 -waveform {0 10} clk1
```
    * -name “clk0”  是产生的时钟源，不是我们能直接使用的，可以理解为一个名字教clk0 的晶震。
    * -period 20 指的是 clk0 信号的时钟周期为 20ns
    * -waveform {0 10} 是，在 0ns 时开始上升，10ns 时开始下降。这是上升和下降时间，不是高低电平持续的时间。
    * clk1 是内部电路的端口名。是我们可以使用的，就是 module 的 clk 入口。

