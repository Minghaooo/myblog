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

(2)Every sample must be sampled.
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

### Synchronizing fast signals into slow clock domains

可能在接受器在采样前改变了两次，发送了两个信号。 或者就是离采样信号的时钟边沿太小了。有两个方向可以解决这个问题：
* 开环设计，但是保证信号是被采样到的。
* 闭环设计，发送一个接收到到的请求信号。

#### Requirement for reliable signal passing between clock domains
从slow 到fast 的时候，如果fast 的clk 在slow 的1.5x 以上，那么这种情况下CDC 就被认为问题不大， 因为会被采样一到两次以上（我也觉得没啥问题）。所以一般情况下用2级ff 就可以搞定了。

CDC 信号应该要比接受域信号宽1到1.5倍。（must be stable for three destination clock edges
    
### passing a fast CDC pulse (!)
如果CDC信号在接受域的波长 只有一个周期， 那么可能无法被采样到。但是当CDC 信号比较长，但是又不够长的时候（比如刚好两个CLK， 那么有可能这个pulse 刚好在两个rising clk edge 的边边上，那么可能再第一个edge 造成setup time 不够，第二个clk hold time 不够， 然后就无法成功建立波形。

#### Open loop 
Advantage：
* the fastest way to pass signals 

Disadvantage:
* three edges design requirement 是凑上去的，如果涉及有改动的话也不一定能注意到。 因此可以加个assert 来保证。（这是缺点吗。，感觉可能是）

#### closed loop
接收到以后发送一个确认信号。
* AD： very safe.
* Dis: have delay.


### passing multiple signals between clock signals.

在多个信号传输的时候 两拍ff 就没啥用了。原因是多个bit 跨时域传输时候，每个ff 会有clk skew, 然后就会有采样时间不一样的抖动问题。（发送信号也可能有clk 抖动。）

可以用以下一个方案解决：
#### consolidate 多个CDC 信号变成一个信号。
感觉是删除的意思，
##### example 1 （两个相同的信号）： 
有一个load 信号，还有一个 enable 信号。如果要求同时进入的话，可能会因为clk skew造成一个信号无法读入，但是因为有3 clk 的保证，所以还是能采样到的，但是可能相差一个节拍。然后就造成了错位。但是实际上并不需要两个信号来完成这个操作。
##### example 2 （两个相差1clk 的信号）：
如果用两个信号控制pipeline的两级寄存器，那么 aen1 和aen2 有可能以为clk skew 而无法被准时准确识别到，而造成该信号延后一个clk（和 example 中一样）然后造成pipeline 的错误。正确的方法是应该只传输一个信号，而在接受域里用寄存器生成第二级流水线的控制信号（delay 一个周期）。
##### example 3 (编码的两个信号)
如果信号是被编码的，那么由于clk skew造成的错误可能导致解码错误。这时候应该用 Multi-cycle Path (MCP) 和 fifo 来解决问题。
* closed loop -MCP with feedback
* closed loop -MCP with acknowledged feedback

或者
* Asynchronous fifo implementation 
* 2-deep fifo implementation

#### MCP formulation 

实际上SPI 用的就是这个原理。 在发送数据时候发送一个跟数据匹配好的时钟信号。先发送数据以后，过几个时钟周期，这个时钟信号才发送（实际上是个采样使能信号，当信号为高电平时，接受域采样。这是setup 和 hold time 都是满足时序要求的。

##### MCP formulation using a synchronized enable pulse
这是最常见的方法。还是添加一个enable 信号。
 
##### MCP closed loop with feedback
pass the enable signal back to the sending clock domain.
这个信号被发送域接收到以后，发送域就可以改变发送的数据。 


### Counters (fifo)

#### grey code

在fifo 中， grey code 是经常被使用的， 但是grey code 不需要被采样，而只是 full 和 empty 的信号比较重要。是否有一种编码方式可以被用来跨过CDC信号呢？没有。
grey code 每次仅翻转一次，

#### binary counter
把 binary counter 进行跨时钟域传送跟传送多个信号是一样的情况， 因为同时会有多个信号翻转。可能会错误的触发full 或者empty 的信号

#### Implementation (坑)


### Fifo （2 kinds）


