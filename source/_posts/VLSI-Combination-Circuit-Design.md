---
title: VLSI Combination Circuit Design
date: 2020-06-03 00:39:06
tags: VLSI
mathjax: true
---
Combinational circuit are those whose outputs only depends on the present inputs. 
Since the delay of a logic gate depends on its output current I, load C, and output voltage swing $\Delta V$.
$$
t \propto \frac{C}{I} \Delta V
$$

* Static CMOS drawbacks:
  * requires both pMOS and nMOS while just on work at a time.
  * all node voltages must transition between 0 and Vdd.
* Advantages:
  * robustness, as long as there is no errors in logic design or manufacturing.

<!--more-->
### Bubble Pushing

DeMorgan's law can be used to reduce the use of inverts.
![](001.png)
$$
\begin{array}{l}
\overline{A \cdot B}=\overline{A}+\overline{B} \\\\  
\overline{A+B}=\overline{A} \cdot \overline{B}
\end{array}
$$

### Compound Gates
    

### Input ordering Delay Effect
since some logic gates are inherently asymmetric in that some inputs see less capacitance than another.


### Asymmetric Gates



### skewed Gates
define HI-skew gates to favor the rising output transition and LO-skew gates to favor the falling output transition.

### P/N Ratio


### Multiple Threshold Voltage
