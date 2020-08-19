---
title: ECE551-Digital-System-Design
date: 2020-08-10 23:11:19
tags: VLSI
---
复习下551 课上的重点。
<!--more-->
### 四值逻辑：
<img src="001.png" width="70%">
来看图，与或非其实是属于正常操作，比较需要关注的重点是:

* x 代表不知道是0 还是1， 因此在与计算中，按照 乘法计算 的方法。
* Z 代表高阻，0和z 或得到x， 1和z 与得到x
* 只有z和z直接相连才得到z，其他都是不定态。

### Parameters & Define
A  `define is a global macro;
‘parameter‘ is local to a module.
但是可以在例化时候被重新定义

```
module adder(a,b,sum)
        parameter width = 8;
        input [width-1:0] a;
        input [width-1:0] b;
        output [width-1:0] sum;

        assign sum = a + b;
 endmodule

   ...
    adder add1(a,b,sum1);
    defparam add1.width = 16;
    ...
    adder #(16) add2(a,b,sum2);
    ...
```

localparam cannot be changed from outside

### assert
  assert 在使用中比较有意思的是concurrent 的用法，工作起来就像一个monitor， 不断监视条件是否满足，并且发出警告
  用法为
  ```
assert property(@(posedge clk) req| -> ##[1:2] ack);
  ```
implication operator  req |-> result,  req first, then result need to occur.
其中， ## 主要用于在一个范围之内的延迟，比如 ##[0：3] 就是在0-3 个cycle 内实现 result 

### Stratified Event Queue
<img src="002.png" width="70%">