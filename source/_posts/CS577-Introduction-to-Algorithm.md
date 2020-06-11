---
title: CS577 Introduction to Algorithm
date: 2020-05-29 23:46:49
tags: CS577
mathjax: true
---
本系列整理自麦屯最菜 Master 对 CS577 的理解
* 不会贴课件，仅作为课堂笔记。
* 真正的从0开始！ （只学过点C语言，看过点数据结构，上课用到过C语言和 python，，，因为整天都在敲 verilog 啊！
* 课本是 UIUC 教授 Jeff Erickson 的 开源书本 [Algorithm](http://jeffe.cs.illinois.edu/teaching/algorithms/)
<!--more-->
## <center>算法的时间复杂度<center>

课上老师默认大家都对这写基本概念比较了解，oh然而我并不。
其实参考大佬们的 blog 就好了，推荐 [算法的时间复杂度和空间复杂度-总结](https://blog.csdn.net/zolalad/article/details/11848739)。大佬们 blog 写的很清楚，我自己整理一遍只是为了更好的理解。
[还有这个](https://blog.gocalf.com/algorithm-complexity-and-master-theorem)

### 算法复杂度记法
复杂度相关的记号有 O, $\Omega, \theta $, o, 和 Master 定理 

$$
f(n)=O(g(n)): \exists c>0, n_{0} \in \mathbb{N}, \forall n \geq n_{0}, f(n) \leq c g(n) ; \text { f 的阶不高于 } g \text { 的阶。 }
$$
$$
f(n)=\Omega(g(n)): \exists c>0, n_{0} \in \mathbb{N}, \forall n \geq n_{0}, f(n) \geq c g(n) ; \text { f 的阶不低于 } \mathrm{g} \text { 的阶。 }
$$
$$
f(n)=\theta(g(n)): \quad \Longleftrightarrow f(n)=O(g(n)) 且 f(n)=\Omega(g(n)); \text { f的阶等于 } \mathrm{g} \text { 的阶。 }
$$
$$
f(n)=o(g(n)): \forall \varepsilon>0, \exists n_{0} \in \mathbb{N}, \forall n \geq n_{0}, f(n) / g(n)<\varepsilon ; \text { f 的阶低于 } g \text { 的阶。 }
$$
O 给出了 fn 在渐进意义上的上界，$\Omega$ 给出了下界， $\theta$ 表示同阶。

### Constant $E.g. O(1)$

* 算法中语句执行次数为一个常数，则时间复杂度为O(1)

### Logarithm $E.g. O(log n)$

* 典型的就是二分查找法

### Polynomial time $E.g. O(n^{3}), O(n^{2}log n)$

* 1. Linear Time $O(n)$
  *  其实最简单的例子是for循环一遍， 多一个循环复杂度就 * n
  
    ```
    MergeArray(a[n], b[n]):
    i = 1, j = 1;
    while both are nonempty
        if(a[i]<=b[j])
            append a[i] to output list and increment i;
        else 
            append b[j] to output list and increment j;
    append remainder of nonempty list to output list 
    ```

* 2. Quadratic $O(n^{2})$
  * 当循环为N个for循环时，就可以得到 n*n 的复杂度
```
Min = (x[1]-x[2])^2 + (y[1]-y[2])
    for i form 1 to n
        for j from i+1 to n
            d = (x[i]-x[j])^2 + (y[i]-y[j])^2;
            if d < min
                min <- d;
```

### Exponential $ E.g. O(2^{n}), O(10^{n}) $
 这个emmm 先让我看会
