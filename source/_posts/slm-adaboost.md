---
title: S8-提升方法
mathjax: true
date: 2020-04-16 09:41:02
updated: {{ date }}
tags:
categories: [统计学习方法]
---

提升（boosting）方法通过改变训练样本的权重，学习多个分类器，并将这些分类器进行线性组合，从而提高分类的性能，其中典型的方法就是AdaBoost方法。

# 提升方法AdaBoost算法

## 提升方法的基本思路

基本思想：对于复杂任务来说，将多个专家的判断进行适当的综合所得出的判断，要比其中任何一个专家独立的判断好。在概率近似正确（probably approximately correct, PAC）学习框架下，一个强可学习的充分必要条件是弱可学习。因此，**“提升方法”的目标是把弱可学习提升为强可学习，即从弱学习器出发，反复学习，得到一系列弱分类器（基本分类器），然后组合这些弱分类器，从而构成一个强分类器。**

> 1、每一轮如何改变训练数据的权值或概率分布？

AdaBoost的做法是提高被前一轮弱分类器错分类样本的权值，并降低被正确分类样本的权值，从而使得错分类的数据能在接下来弱分类器中获得更大关注。

> 2、如何将弱分类器组合成一个强分类器？

AdaBoost的做法是加权多数表决，即加大误分类小的弱分类器的权值，减少误分类大的弱分类器的权值。

## AdaBoost算法

假设给定一个二分类的训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

实例$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，标记$y_{i} \in \mathcal{Y}=\{-1,+1\}$

1、初始化训练数据的权值分布

$$D_{1}=\left(w_{11}, \cdots, w_{1 i}, \cdots, w_{1 N}\right), \quad w_{1 i}=\frac{1}{N}, \quad i=1,2, \cdots, N$$

2、对$m=1,2, \cdots, M$

（1）使用具有权值分布$D_{m}$的训练数据学习，得到基本分类器（弱分类器）

$$G_{m}(x): \mathcal{X} \rightarrow \{-1,+1\}$$

（2）计算$G_{m}(x)$在训练数据集上的分类误差率

$$e_{m}=\sum_{i=1}^{N} P\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)=\sum_{i=1}^{N} w_{m i} I\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)$$

（3）计算$G_{m}(x)$的系数

$$\alpha_{m}=\frac{1}{2} \log \frac{1-e_{m}}{e_{m}}$$

此处的对数为自然对数。

（4）更新训练数据集的权值分布

$$D_{m+1}=\left(w_{m+1,1}, \cdots, w_{m+1, i}, \cdots, w_{m+1, N}\right)$$

$$w_{m+1, i}=\frac{w_{m i}}{Z_{m}} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right), \quad i=1,2, \cdots, N$$

其中$Z_{m}$表示规范化因子

$$Z_{m}=\sum_{i=1}^{N} w_{m i} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right)$$

它使$D_{m+1}$成为一个概率分布

3、构建基本分类器的线性组合

$$f(x)=\sum_{m=1}^{M} \alpha_{m} G_{m}(x)$$

得到最终分类器

$$\begin{aligned}
G(x) &=\operatorname{sign}(f(x)) \\
&=\operatorname{sign}\left(\sum_{m=1}^{M} \alpha_{m} G_{m}(x)\right)
\end{aligned}$$

# AdaBoost算法的训练误差

> 1、AdaBoost算法最终分类器的训练误差界为

$$\frac{1}{N} \sum_{i=1}^{N} I\left(G\left(x_{i}\right) \neq y_{i}\right) \leqslant \frac{1}{N} \sum_{i} \exp \left(-y_{i} f\left(x_{i}\right)\right)=\prod_{m} Z_{m}$$

> 2、二分类问题AdaBoost的训练误差界

$$\begin{aligned}
\prod_{m=1}^{M} Z_{m} &=\prod_{m=1}^{M}[2 \sqrt{e_{m}\left(1-e_{m}\right)}] \\
&=\prod_{m=1}^{M} \sqrt{\left(1-4 \gamma_{m}^{2}\right)} \\
& \leqslant \exp \left(-2 \sum_{m=1}^{M} \gamma_{m}^{2}\right)
\end{aligned}$$

其中$\gamma_{m}=\frac{1}{2}-e_{m}$

> 3、如果存在$\gamma>0$，对所有$m$有$\gamma_{m} \geqslant \gamma$，则

$$\frac{1}{N} \sum_{i=1}^{N} I\left(G\left(x_{i}\right) \neq y_{i}\right) \leqslant \exp \left(-2 M \gamma^{2}\right)$$

# AdaBoost算法的解释

AdaBoost算法的另一个解释认为，AdaBoost算法的**模型是加法模型，损失函数为指数函数，学习算法为前向分步算法**的二分类学习方法。

## 前向分步算法

考虑加法模型

$$f(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$$

其中$b\left(x ; \gamma_{m}\right)$为基函数，$\gamma_{m}$为基函数的参数，$\beta_{m}$为基函数的系数，因此$f(x)=\sum_{m=1}^{M} \alpha_{m} G_{m}(x)$就是一个加法模型。

在给定训练数据及损失函数$L(y, f(x))$的条件下，学习加法模型$f(x)$的经验风险极小化即损失函数极小化问题

$$\min _{\beta_{m}, \gamma_{m}} \sum_{i=1}^{N} L\left(y_{i}, \sum_{m=1}^{M} \beta_{m} b\left(x_{i} ; \gamma_{m}\right)\right)$$

要求上式的最小化，**其方法是最小化其中的每一步，从而逼近优化目标函数：**

$$\min _{\beta, \gamma} \sum_{i=1}^{N} L\left(y_{i}, \beta b\left(x_{i} ; \gamma\right)\right)$$

> 前向分步算法过程

输入训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$；损失函数$L(y, f(x))$；基函数据集$\{b(x ; \gamma)\}$；

1、初始化$f_{0}(x)=0$

2、对$m=1,2, \cdots, M$

（1）极小化损失函数

$$\left(\beta_{m}, \gamma_{m}\right)=\arg \min _{\beta, \gamma} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+\beta b\left(x_{i} ; \gamma\right)\right)$$

得到参数$\beta_{m}$，$\gamma_{m}$。

（2）更新

$$f_{m}(x)=f_{m-1}(x)+\beta_{m} b\left(x ; \gamma_{m}\right)$$

（3）得到加法模型

$$f(x)=f_{M}(x)=\sum_{m=1}^{M} \beta_{m} b\left(x ; \gamma_{m}\right)$$

以此，前向分步算法将求解出从$m=1$到$M$所有参数$\beta_{m}$和$\gamma_{m}$的优化问题逐次求解各个$\beta_{m}$、$\gamma_{m}$的优化问题。

## 前向分步算法与AdaBoost

AdaBoost算法是前向分步加法算法的特例，模型是由基本分类器组成的加法模型，损失函数是指数模型函数。

$$L(y, f(x))=\exp [-y f(x)]$$

# 提升树

以决策树为基函数的提升方法。对于分类问题决策树是二叉分类树，对于回归问题决策树是二叉回归树，其加法模型为：

$$f_{M}(x)=\sum_{m=1}^{M} T\left(x ; \Theta_{m}\right)$$

其中，$T\left(x ; \Theta_{m}\right)$表示决策树，$\Theta_{m}$表示决策树的参数，$M$为树的个数。

第$m$步的模型是

$$f_{m}(x)=f_{m-1}(x)+T\left(x ; \Theta_{m}\right)$$

其中$f_{m-1}(x)$表示当前模型，通过经验风险极小化确定一棵决策树的参数$\Theta_{m}$

$$\hat{\Theta}_{m}=\arg \min _{\Theta_{m}} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+T\left(x_{i} ; \Theta_{m}\right)\right)$$

## 回归问题的提升树

已知训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$，$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，$\mathcal{X}$为输入控件，$y_{i} \in \mathcal{Y} \subseteq \mathbf{R}$，$\mathcal{Y}$为输入空间。如果将输入空间$\mathcal{X}$划分为$J$个互不相交的区域$R_{1}, R_{2}, \cdots, R_{J}$，并且每个区域三确定输出产量$c_{j}$，那么此时树可以表示为

$$T(x ; \Theta)=\sum_{j=1}^{J} c_{j} I\left(x \in R_{j}\right)$$

其中，参数$\Theta=\left\{\left(R_{1}, c_{1}\right),\left(R_{2}, c_{2}\right), \cdots,\left(R_{J}, c_{J}\right)\right\}$表示树的区域划分和各区域三的参数。$J$是回归树的复杂度，即叶节点的个数。

回归问题提升树使用的前向分步算法

$$\begin{aligned}
f_{0}(x) &=0 \\
f_{m}(x) &=f_{m-1}(x)+T\left(x ; \Theta_{m}\right), \quad m=1,2, \cdots, M \\
f_{M}(x) &=\sum_{m=1}^{M} T\left(x ; \Theta_{m}\right)
\end{aligned}$$

第$m$步的当前模型$f_{m-1}(x)$，需求解

$$\hat{\Theta}_{m}=\arg \min _{\Theta_{m}} \sum_{i=1}^{N} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+T\left(x_{i} ; \Theta_{m}\right)\right)$$

得到$\hat{\Theta}_{m}$，即第$m$棵树的参数。

损失函数为平方损失

$$L(y, f(x))=(y-f(x))^{2}$$

其损失函数就变为

$$\begin{aligned}
L\left(y, f_{m-1}(x)+T\left(x ; \Theta_{m}\right)\right) &=\left[y-f_{m-1}(x)-T\left(x ; \Theta_{m}\right)\right]^{2} \\
&=\left[r-T\left(x ; \Theta_{m}\right)\right]^{2}
\end{aligned}$$

其中$r=y-f_{m-1}(x)$是当前模型拟合数据的残差（residual）

## 梯度提升

利用最速下降法的近似方法，其关键是利用损失函数的负梯度在当前模型的值

$$-\left[\frac{\partial L\left(y, f\left(x_{i}\right)\right)}{\partial f\left(x_{i}\right)}\right]_{f(x)=f_{m-1}(x)}$$

作为回归问题提升树中的残差近似值。

> 梯度提升算法过程

输入训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$，$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，$y_{i} \in \mathcal{Y} \subseteq \mathbf{R}$；损失函数$L(y, f(x))$

1、初始化

$$f_{0}(x)=\arg \min _{c} \sum_{i=1}^{N} L\left(y_{i}, c\right)$$

2、对$m=1,2, \cdots, M$

（1）对$i=1,2, \cdots, N$，计算

$$r_{m i}=-\left[\frac{\partial L\left(y_{i}, f\left(x_{i}\right)\right)}{\partial f\left(x_{i}\right)}\right]_{f(x)=f_{m-1}(x)}$$

（2）对$r_{mi}$拟合一个回归树，得到第$m$棵树的叶节点区域$R_{m j}, j=1,2, \cdots, J$。

（3）对$j=1,2, \cdots, J$，计算

$$c_{m j}=\arg \min _{c} \sum_{x_{i} \in R_{m j}} L\left(y_{i}, f_{m-1}\left(x_{i}\right)+c\right)$$

（4）更新

$$f_{m}(x)=f_{m-1}(x)+\sum_{j=1}^{J} c_{m j} I\left(x \in R_{m j}\right)$$

3、得到回归树

$$\hat{f}(x)=f_{M}(x)=\sum_{m=1}^{M} \sum_{j=1}^{J} c_{m j} I\left(x \in R_{m j}\right)$$
