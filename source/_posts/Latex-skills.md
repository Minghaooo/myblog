---
title: Latex skills
date: 2020-06-01 01:11:46
tags: Latex
---
记录下Xinyuan的模板和使用中遇到的问题! 
<!--more-->

1. 对于超级长的Url 不自动换行，可以添加这个包解决问题，（比较新的版本才有,可以手动安装)
```
 \usepackage{xurl}
```

2. 在 xelatex 下使用 Algorithm 
```
\usepackage[algo2e,linesnumbered,ruled,vlined]{algorithm2e} 

\beign{algorithm2e}[H]
\end{algorithm2e}
```
* [ ]内是选择伪代码块的样式，
* 在 \begin{algorithm} 有冲突的情况下（我也不知道为啥不能用),在 package 里加入algo2e 就可以用 \beign{algorithm2e}


3. Algorithm [改中文](https://www.cnblogs.com/tsingke/p/5833221.html)
中文latex模式下，更改Input为输入，更改Output为输出的方法： 在算法内部插入、
```
\SetKwInOut{KIN}{输入}
\SetKwInOut{KOUT}{输出}
```
用我们定义的新的宏名“\KIN” 在算法开头相应位置替换掉自带的\KwInput，用"\KOUT" 替换掉\KwOutput 即可，实现中文的输入和输出.
```
\renewcommand{\algorithmcfname}{算法}
\begin{algorithm}[H]
%\SetAlgoNoLine
\SetKwInOut{KIN}{输入}
\SetKwInOut{KOUT}{输出}
%\BlankLine  %空一行
    \caption{标准DE算法 }
    \label{DE_algo} %
    \KIN{Population: $ M$; Dimension: $ D $; Genetation: $ T $ }
    \KOUT{The best vector (solution)  $ \varDelta $ }
    $ t \leftarrow 1 (initialization) $\;
```
（:一个问题一个女朋友我记着呢