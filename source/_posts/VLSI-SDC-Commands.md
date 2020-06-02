---
title: VLSI SDC Commands
date: 2020-06-02 01:29:52
tags: VLSI
---
总结下SDC 方面常用的命令,网上找到一个[UBC的材料](http://www.ece.ubc.ca/~edc/7660.jan2018/lec9.pdf)感觉写的挺好。
此外，[TCL脚本语言](https://www.ibm.com/developerworks/cn/education/linux/l-tcl/l-tcl-blt.html)其实也很重要,应该先总体上过一遍。
    争取每天一个到两个命令详解。一开始会从551ppt上摘录，后面会使用到的更多。
    突然发现[intel](https://www.intel.cn/content/dam/www/programmable/us/en/pdfs/literature/manual/mnl_sdctmq.pdf) 有文档打的挺详细，那就自己打打加深印象。。。
<!--more-->
### Collections

Collections 是时序约束的常用单位,一个 collection 是多个端口的集合。
    
* default collections
```
all_clocks //return a collection of all clocks in the design.
all_inputs //return a collection of all input ports in the design.
all_outputs //return a collection of all output ports in the design.
all_registers //return a collection of all registers in the design.
```

* get_commands
```
get_ports [-nocase] [-nowarn] <filter>
get_pins <filter>
get_net <filter>
get_clock <filter>
get_cells <filter>
```
 * [-nocase] case sensitive
 * [-nowarn] not issue warning strategies
 * [xxxxxxx] you can find more models in Intel's document
 * \<filter> the string are matched using TCL string matching


  
### Timing constrains

* 时钟生成 
```
 create_clock -name "clk0" -period 20 -waveform {0 10} clk1
```
    * -name “clk0”  是产生的时钟源，不是我们能直接使用的，可以理解为一个名字教clk0 的晶震。
    * -period 20 指的是 clk0 信号的时钟周期为 20ns
    * -waveform {0 10} 是，在 0ns 时开始上升，10ns 时开始下降。这是上升和下降时间，不是高低电平持续的时间。
    * clk1 是内部电路的端口名。是我们可以使用的，就是 module 的 clk 入口。

* 设定依赖于其他时钟信号，并由分频等形成的时钟
```
create_generated_clock -source [get_port clk50] -divide_by 2 -name smpl_clk [get_nodes sclk~reg0]
```
    * -source [get_port xxx ] 同上 clk1，是电路连接的时钟端口
    * -name "clk_init" 是时钟源信号的名称
    *  -divide_by ｜-multiply_by <factor> 其中 x 是分频系数，
    *  <source objects> specify the ports or pins the assignment applies



```
set_driving_cell -lib_cell <cell> -pin Z -library <lib> <list_of_inputs>
set_drive <# of ohms>
```

