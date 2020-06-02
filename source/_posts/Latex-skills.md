---
title: Latex skills
date: 2020-06-01 01:11:46
tags: Latex
---
记录下Xinyuan遇到的问题和最后找到的解决方案! （:一个问题一个女朋友我记着呢
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