---
title: "泛化误差界限（结构风险）"
date: 2019-07-29
categories:
  - 统计学方法
tags:
  - 统计
  - 机器学习
slug: generalization_error 
---
1.泛化误差界

机器学习的能力和它的表现，有一个衡量的标准那就是统计学习中的泛化误差界。所谓泛化误差，就是指机器学习在除训练集之外的测试集上的预测误差。传统的机器学习追求在训练集上的预测误差最小化（经验风险），然后放到实际中去预测测试集的文本，却一败涂地。这就是泛化性能太差，而泛化误差界是指一个界限值。

假设我们有l个观察值，每个观察值由两个元素组成 一个属于Rn(n维空间）的向量：Xi  ∈  Rn (i = 1 , 2 , 3 , 4...l)  已经和这个向量相对应的映射值 Yi 在二分类文本中 Xi就表示第i个文本的特征向量，前面介绍过，那么Yi = {+1,-1} 由两个类别组成。

接着我们假设有一个未知的分布 P(X,Y)  是X 到 Y 的映射，给定Xi 都有一个固定的Yi 与之对应（可以当做实际测试数据集）与此同时我们使用机器学习得到了一个函数 f (x, α) 表示输入 x 会得出x的映射 y，这里的 α 是机器学习的某一个参数，比如具有固定的结果的神经网络，具有表示重量和偏见的  α 。 那么该机器学习算法的风险（误差）公式 ：

`$$R(\alpha) = \int \frac{1}{2}|y-f(x,\alpha)|dP(x,y)$$`

R (α) 术语叫做期望风险（expected risk），我们也可以称作为真实风险，因为它是衡量机器学习和实际测试数据集之间的误差。

而机器学习和训练数据集之间的误差也有个名字叫做经验风险 （empirical risk）用 `$R_{emp}(α)$`  表示：

`$$R_{emp}(\alpha) = \frac{1}{2l}\sum_{i=1}^l|y_i-f(x_i,\alpha)|$$`

这里给定参数 α 和训练集  {xi,yi}  得到的 `$R_{emp}(α)$` 是一个确定的数字，取一个 η  （ 0<= η<=1）用 1- η 机器学习出现误差的概率，R (α) 有一个上界可以表示为（Vapnik, 1995 ）：

`$$R(\alpha) \leq R_{emp}(\alpha)+\phi(\frac{h}{l})$$`

Φ(h/l) 叫做置信风险 （VC confidence）， h 叫做VC维度 （Vapnik Chervonenkis (VC) dimension） l 就是样本数目  Φ(h/l) 的公式如下：

`$$\phi(\frac{h}{l})=\sqrt{\frac{h(log(2l/h)+1)-log(\eta/4)}{l}}$$`

以上公式可以看到，要使真实风险最小，统计学习就要使得R (α) 的上界最小，也就是让`$\phi(h/l)-R_{emp}(α)$`变的最小，也就是结构风险最小。可以说 `$\phi(h/l)-R_{emp}(α)$` 叫做**结构风险**，它是一个实际的边界值，是R (α) 的上限，就是统计学中的**泛化误差界限**。那么怎么使得结构风险最小化呢？下面就要提到VC维度理论。

2.VC维度

上面说到置信风险Φ(h/l) 中的h叫做VC 维度，那么VC维度是什么呢？举个例子：

假设我们有 l 个点，每个点我们都有个标记 Yi = {+1,-1}，把这 l 个点 分别进行标记+1或者-1 ，那么有2l 种方法。对于函数集 { f(x,α) } （f(x,α) 就是前面提到的机器学习得到的分类函数） 中的函数，2l中的每个方法都能从函数集中找到一个函数去成功的标记，那么就说这个函数集 { f(x,α) } 的VC维度为 l 。也就是函数集能够进行标记的数据点的最大个数（用行话叫做分散）。

这是在二维坐标平面上，3个点，将他们进行比较为2个类别，会有2的3次方，也就是8种方法，可以看到每种标记方法我们都可以找到一条直线把这3个点分散（就是根据类别分开）。但是4个点的时候，你却发现有几种情况你无法找到一条直线将他们按照类别分开在直线的两侧。这也就说明了在二维空间里，一个直线的集合的VC维度是3。


推广到n维空间Rn 中 超平面（Oriented Hyperplanes）的VC维度是n+1。证明的方法是根据一个定理：

在n维空间 Rn 中有一个m个点的集合，选择任意一个点作为原点，如果剩下的m-1个点是线性可分的，那么这m个点就可以被一个超平面集合分散
.
推广一下，在n维空间 Rn 中我们总是可以选取n+1个点，选取其中一个点作为原点，剩下的n个点在n维空间是一定线性可分的，因此n+1个点在n维空间中是可以被超平面集合分散的。

VC维度越高，表示函数集的复杂程度越高。一般的函数集的越复杂，VC维度越高，能够对训练集的每一个文本都可以精确分类，也就是把经验风险降到最低，但是前面说过，面对训练集之外的样本却一塌糊涂。就是因为VC维度太高了，为什么呢？因为忽视了置信风险。

可以看到VC维度 h 越高，置信风险也越高，对于任意的l, 置信风险都是h的递增函数。

对于一般情况VC维度越高，机器学习的置信风险越高，泛化能力就会越差。然而也有相反的情况：

反例：

如果VC维度到达无限，比如K邻近算法，k=1 只有一个类，那么所有的样本都会被正确分类，他的VC维度是无限的，经验风险是0。他的泛化能力反而是无限强的。

如果VC维度到达无限，那么上面的说的泛化误差界限就没有实际的用处了，所以VC维度高到无限对性能的影响也不一定是差的。

 
3.SRM 结构风险最小化

在第1节中已经提到了让  Φ(h/l) +  Remp(α) 最小化 ，就是结构风险最小化，也即SRM（ Structural Risk Minimization ）

具体的思想是：

通过将函数集划分为多个子集。 对于每个子集按照VC维度排列，在每个子集中寻找最小经验风险,然后在子集之间折衷考虑经验风险和置信风险之和最小,得到的泛化误差界最小。

而SVM 则是将结构风险最小化较好实现的算法

[文章来源](https://www.cnblogs.com/dacc123/p/9015829.html)