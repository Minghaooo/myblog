---
title: CS577-05-Greedy-Algorithm
date: 2020-07-02 10:50:34
tags: CS577
mathjax: true
---
<img src="001.png" width="50%">

An algorithm is greedy if it builds up a solution in small steps, choosing a decision at each step myopically and builds up a solution in small steps.

<!--more-->
### Interval Scheduling

问题叙述和之前动态规划里面的类似
goal: accept as large a set of non-overlapping intervals as possible.

basic ideal: 
* use a simple rule to select a first request $i_{1}$, and reject all requests that not compatible with $i_{1}$
* select next request $i_{2}$ and repeat the last process
* continue until run out all the requests

这里的选择方法是：
* choose a request that has the smallest finishing time

proof: 因为题目的要求是取得最大的数量，因此用反证法证明：
    assumption: optimal set O 有更多的 requests。

    因为根据贪婪算法，我们的选择方式是 使每个request 的结束时间最短，所以在取k 个任务时，一定有 A 比 O 结束的早。因此要有更多的数量， 那么O 里面一定有 k+1 个任务是可选的。 但是贪婪算法为啥不用这个呢，不合理，，，所以假设错误。

### Schedule Minimize lateness
    
suppose that we need to satisfy some requests, but some can be delayed for some time,,,
the request i has the deadline $d_{i}$ and it requires a contiguous time interval of length $t_{i}$.

the lateness is $f(i) - d_{i}$ if the finish time is later than deadline.  or it should be 0;

一言以蔽之，实际上这个题目就是，对于每个任务，他有开始时间和结束时间，此外还有一个 deadline, 在这个ddl结束前完成的话，lateness 就是0. 在ddl 结束后完成的，lateness 就是超出的时间。

算法也很简单，对所有的任务按照 deadline 的时间重新排序，然后按照这个顺序来进行选择。