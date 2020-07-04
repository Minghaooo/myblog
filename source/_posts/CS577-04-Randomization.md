---
title: CS577-04-Randomization
date: 2020-06-23 20:21:23
tags: CS577
mathjax: true
---
概率论没学好，猝。
<!--more-->

### Contention Resolution: 
n processes P1, P2, . . . , Pn, each competing for access to a single shared database. 
The database has the property that it can be accessed by at mostone process in a single round;


 A[i, t]denote the event that Pi attempts to access the database in round t. 
 Each process attempts an access in each round with probability p, so the probability of this event, for any i and t, is Pr{A[i, t]} = p.

 a process succeeds in accessing the database is &\delta&
 $$\delta[i, t]= A [i, t] \cap\left(\bigcap_{j \neq i} \overline{ A [j, t]}\right)$$

Claim 1：
Let S[i, t] = event that process i succeeds in accessing the database at time t. Then $1/(e *n) /leq Pr[S(i, t)] /leq 1/(2n)$

Claim2：
 The probability that process i fails to access the database in e*n rounds is at most 1/e After e*n(c ln n) rounds,the probability is at most n-c.

 Claim 3:
 The probability that all processes succeed within 2e * nln(n) rounds is at least 1 - 1/n;

 废话讲了那么多，实际上就是一句话: 让process 随机化，然后我们要做到的是选取参数，使成功概率最大化。经过计算我们发现 p = 1/n 的时候，成功概率是最大的。 