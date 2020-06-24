---
title: CS577-01 Divide and Conquer
date: 2020-06-01 13:16:04
tags: CS577
---

 A typical divide-and-conquer algorithm has three steps:
*  divide a problem into smaller subproblems
*  solve each problem independently
*  combine all problems to get the right solution
<!--more-->
### Mergesort
Divide array into two halves
Recursively sort each half
Merge two halves to make sorted whole

### Counting Inversions
这个算法的主要难点在于merge. divide 不难，一刀两半就成。

在combine 的时候，也跟mergesort 类似，因为右边的一定是应该比较大的，所以当右边的比左边的大几时就+几。

在 recursive call 的时候，顺手给他sort一下会比较好。 所以分两个部分:

sort and count + merge and count