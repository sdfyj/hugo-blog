---
title: "二叉树期权定价模型"
date: 2019-07-02
categories:
  - 金融与投资
  - 统计学方法
tags:
  - 期权
slug: binomial_tree_option_pricing
---

二叉树期权定价模型也称为二项期权定价模型

模型假设股价波动只有向上和向下两个方向，且假设在整个考察期内，股价每次向上(或向下)波动的概率和幅度不变。模型将考察的存续期分为若干阶段，根据股价的历史波动率模拟出正股在整个存续期内所有可能的发展路径，并对每一路径上的每一节点计算权证行权收益和用贴现法计算出的权证价格。对于美式权证，由于可以提前行权，每一节点上权证的理论价格应为权证行权收益和贴现计算出的权证价格两者较大者。

在利用风险中性方法对衍生产品定价时，首先计算在风险中性世界里各种不同结果发生的概率，然后由此计算衍生产品的收益期望值。衍生产品的价格等于这个期望值在无风险利率下的现值。

风险中性世界的两个特点可以简化对衍生产品的定价：

1、股票（或任何投资）的收益率期望等于无风险利率

2、用于对期权（或其他证券）的收益期望值贴现的利率等于无风险利率

由二叉树模型可以对布莱克-斯科尔斯-默顿期权定价公式进行推导，当二叉树的步数趋向于无穷，二项分布渐进于正态分布，公式趋于收敛到布莱克-斯科尔斯-默顿期权定价公式。