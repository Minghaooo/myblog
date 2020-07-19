---
title: VLSI CDC Clock Domain Crossing
date: 2020-07-18 19:51:19
tags: VLSI
mathjax: true
---
CDC 就是跨时域的信号处理，非常重要。

<!--more-->

### 亚稳态 Metastability

数字信号一般在 0 和 1 之间，但是有时候总会抽风所以处于不知道是什么的状态，导致输出结果无法预测。
亚稳态无法彻底避免，但是它带来的不好的影响可以被消除（ can not be avoided, but the detrimental effects can be neutralized.) 

亚稳态的出现归根到底是因为建立时间和保持时间不能满足要求。
 * 建立时间 （setup time）： 时钟沿到来之前数据需要保持稳定的时间。
 * 保持时间 （hold time）： 时钟沿到达之后数据需要保持的时间。


### 同步化问题 Synchronizers

    这个 synchronizer 是将从别的时钟域过来的信号转换为同步信号的一个东西。

    Do I need to sample every value of a signal passed from one domain to another ? (这是啥问题...?)

(1) it is permitted to miss samples that are passed between clock domains.
 * 在异步fifo 中 synchronized grey code counter do not need to capture every legal value from the opposite clock domain. 
 * 但是 empty 和 full 信号必须被准确识别。

（2）Every sample must be sampled.
*  一个CDC 信号必须被 recognized or recognized, and acknowledged,在这个信号改变被允许之前。
####　two flip-flop synchronizer
第一个触发器将异步信号采样，并且等待一个节拍采样并送入同步时钟区， 并且等待一个时钟周期，这个信号叫 stage 1 信号。 然后 stage 1 被相同的时钟信号的触发器采样进入stage 2 ，这个stage 2 信号是稳定并且有效的同步信号。实际上stage 2 信号不稳定也是有可能的，但是从概率论上来讲已经是足够使用了。
这个概率是 MTBF mean time before failure 是指， 经过两级ff 以后任然不稳定的概率。 MTBF 越长越好。这个公式是：
 $$
    MTBF = \frac{1}{f_{clk} * f_{data} * X}
 $$

 频率越高，数据变化速度越大，失败率越高。

#### Three flip-flop synchronizer

在某些高速计算中这个概率有点大，所以有时候会用到三级触发器。

#### Synchronizing signals from the sending clock domain
不仅仅是在接收域的时钟需要flipflop 需要稳定，实际上发送域也需要。从组合逻辑出来的信号，一都会不是很稳定（combinational settling）, 然后会造成更多的亚稳态（相当于发送数据的变化频率变高了）





