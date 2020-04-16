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

（2）计算$G_{m}(x)$在训练数据集三的分类误差率

$$e_{m}=\sum_{i=1}^{N} P\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)=\sum_{i=1}^{N} w_{m i} I\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)$$

（3）计算$G_{m}(x)$的系数

$$\alpha_{m}=\frac{1}{2} \log \frac{1-e_{m}}{e_{m}}$$

此处的对数为自然对数。

（4）更新训练数据集的权值分布

$$D_{m+1}=\left(w_{m+1,1}, \cdots, w_{m+1, i}, \cdots, w_{m+1, N}\right)$$

$$w_{m+1, i}=\frac{w_{m i}}{Z_{m}} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right), \quad i=1,2, \cdots, N$$