---
title: 高级ASIC综合 一
date: 2020-06-03 12:58:49
tags: VLSI
---
《高级ASIC芯片综合》读书笔记  第一部分 基本介绍
<!--more-->
### 工具介绍：
Synopsys 产品主要常用的是
* Library Compiler -- 用于 library 的检查以及格式转换等
* Design Compiler 是 Design Vision 的后段。DC 是命令行版本，DV 是GUI。
* Physical Compiler 是 DC 工具的一个超集，包含DC 以及根据时序面积约束优化
* PrimeTime 是全芯片，门级静态时序分析工具
* DFT Compiler 是测试插入工具，用于插入DFT特性，可以从DC 调用
* Formality 是形式验证工具
  
```
analyze/elaborate  
read
```
以上两个命令都可以将RTL读取到DC中，但是有个区别就是
* read 只能读取，不能保存分析结果不能传递参数等
* Analyze /elaborate可以存储中间变量，而且使用 generic 语句等需要参数传递时必须使用此命令。

### 工艺库介绍
 Synopsys 工艺库可以分成 *逻辑库* 和 *物理库*。
* 逻辑库包含于综合过程有关的信息并且通过DC用于设计的综合和优化。
  * 包含引脚到引脚的时序，面积，引脚类型等。
  * 由 .lib 通过 Library Compiler 编程生成.db
* 物理库包含单元的物理特征与 Physical Compiler 相关的其他必要信息。
  * 包含单元的物理尺寸，层信息以及单元方位等相关信息。
  * 每个逻辑单元对应一个物理单元
  * 由LC 生产 .pdb 二进制文件
  
### 划分和编码风格：
