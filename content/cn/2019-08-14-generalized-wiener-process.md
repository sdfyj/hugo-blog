---
title: "广义维纳过程"
date: 2019-08-14
categories:
  - 金融与投资
  - 统计学方法
tags:
  - 统计
  - 期权
slug: generalized_wiener_process
---


在随机过程中，变量在每单位时间内变化的期望值叫作变量的漂移率（drift rate），方差叫作变量的方差率（variance rate）。而基本维纳过程的漂移率为0，方差率为1。漂移率为0意味着将来任意时刻变量z的期望值等于其当前值，方差率等于1意味着在长度为T的任何时间区间内，z变化的方差等于T。广义维纳过程（generalized wiener process）x可以通过dz定义

`$$dx=adt+bdz$$`

其中a和b是常数。

adt项说明变量x的单位时间漂移率为a。如果没有bdz项，以上方程变为dx=adt，即dx/dt=a。对t进行积分得出

`$$x=x_0+at$$`

`$x_0$`为x在0时刻的初始值。在一段时间T内，变量x的增量为aT。bdz项可被看成附加在变量x路径上的噪声或扰动，它们的幅度为维纳过程的b倍。维纳过程的标准差为1，因此b倍维纳过程在单位时间内的方差率为`$b^2$`。在短时间`$\Delta t$`内，有

`$$\Delta x=a\Delta t+b\varepsilon\sqrt{\Delta t}$$`

其中`$\varepsilon$`服从标准正态分布。因此`$\Delta x$`服从正态分布，均值`$a\Delta t$`，标准差`$b\sqrt{\Delta t}$`，方差`$b^2\Delta t$`。

在任意时间T内，变量x的变化服从正态分布，均值`$aT$`，标准差`$b\sqrt{T}$`，方差`$b^2T$`。