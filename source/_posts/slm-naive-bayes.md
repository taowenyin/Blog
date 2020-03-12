---
title: S4-朴素贝叶斯
mathjax: true
date: 2020-03-11 10:37:34
updated: {{ date }}
tags:
categories: [统计学习方法]
---

# 朴素贝叶斯的学习与分类

## 基本方法

设输入空间$\mathcal{X} \subseteq \mathbf{R}^{n}$是$n$维向量的集合，输出空间为类标记集合$\mathcal{Y}=\left\{c_{1}, c_{2}, \cdots, c_{K}\right\}$。输入为特征向量$x \in \mathcal{X}$，输出维类标记$y \in \mathcal{Y}$。$X$是定义在输入空间$\mathcal{X}$上的随机向量，$Y$是定义在输出空间$\mathcal{Y}$上的随机变量。$P(X, Y)$是$X$和$Y$的联合概率分布。训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

由$P(X, Y)$独立同分布产生。

朴素贝叶斯法通过训练数据集学习联合概率分布$P(X, Y)=P\left(X=x | Y=c_{k}\right)P\left(Y=c_{k}\right)$。

1. 先验概率分布，$P\left(Y=c_{k}\right), \quad k=1,2, \cdots, K$
2. 条件概率分布，$k=1,2, \cdots, K$

$$P\left(X=x | Y=c_{k}\right)=P\left(X^{(1)}=x^{(1)}, \cdots, X^{(n)}=x^{(n)} | Y=c_{k}\right)=\prod_{j=1}^{n} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)$$

3. 得到了联合概率分布$P(X, Y)$

朴素贝叶斯实际上学习到了生成数据的机制，**所以属于生成模型。** 条件独立假设等于是说用于分类的特征在类确定的条件下是条件独立的，这一假设使朴素贝叶斯法变得简单，但有时牺牲了一定的分类准确率。

朴素贝叶斯法分类时，对给定的输入$x$，通过学习到的模型计算后验概率分布$P\left(Y=c_{k} | X=x\right)$，将后验概率最大的类作为$x$的类输出。后验概率计算根据贝叶斯定理得到：

$$P\left(Y=c_{k} | X=x\right) =\frac{P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}{P\left(X=x\right)}=\frac{P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}{\underset{k}{\sum} P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}$$

代入条件概率分布得到

$$P\left(Y=c_{k} | X=x\right) =\frac{P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}{\underset{k}{\sum} P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}$$

于是，朴素贝叶斯分类器可表示为

$$y=f\left ( x\right )=\arg \underset{c_{k}}{max}\frac{P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}{\underset{k}{\sum} P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}$$

由于上式分母对所有$c_{k}$都相同，所以

$$y=\arg \max _{c_{k}} P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)$$

## 后验概率最大化的含义

假设采用0-1损失函数

$$L(Y, f(X))=\left\{\begin{array}{ll}
1, & Y \neq f(X) \\
0, & Y=f(X)
\end{array}\right.$$

式中$f(X)$是分类决策函数，那么期望风险函数为

$$R_{\exp }(f)=E[L(Y, f(X))]$$

由于期望是根据联合分布$P(X, Y)$得到。因此，条件期望为

$$R_{\mathrm{exp}}(f)=E_{X} \sum_{k=1}^{K}\left[L\left(c_{k}, f(X)\right)\right] P\left(c_{k} | X\right)$$

为了使期望风险最小化，只需要对$X=x$逐个极小化，由此得到：

$$\begin{aligned}
f(x) &=\arg \min _{y \in \mathcal{Y}} \sum_{k=1}^{K} L\left(c_{k}, y\right) P\left(c_{k} | X=x\right) \\
&=\arg \min _{y \in \mathcal{Y}} \sum_{k=1}^{K} P\left(y \neq c_{k} | X=x\right) \\
&=\arg \min _{y \in \mathcal{Y}}\left(1-P\left(y=c_{k} | X=x\right)\right) \\
&=\arg \max _{y \in \mathcal{Y}} P\left(y=c_{k} | X=x\right)
\end{aligned}$$

因此，根据风险最小化准则就得到了后验概率最大化准则：

$$f(x)=\arg \max _{c_{k}} P\left(c_{k} | X=x\right)$$

# 朴素贝叶斯法的参数估计

## 学习和分类算法

输入训练数据$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$，其中$x_{i}=\left(x_{i}^{(1)}, x_{i}^{(2)}, \cdots , x_{i}^{(n)}\right)^{\mathrm{T}}$，$x_{i}^{(j)}$是第$i$个样本的第$j$个特征，$x_{i}^{(j)} \in\left\{a_{j 1}, a_{j 2}, \cdots, a_{j S_{j}}\right\}$，$a_{j l}$是第$j$个特征可能取的第$l$个值，$j=1,2, \cdots, n$，$l=1,2, \cdots, S_{j}$，$y_{i} \in\left\{c_{1}, c_{2}, \cdots, c_{K}\right\}$

输出：实例$x$的分类

1. 计算先验概率及条件概率

$$P\left(Y=c_{k}\right)=\frac{\sum_{i=1}^{N} I\left(y_{i}=c_{k}\right)}{N}, \quad k=1,2, \cdots, K$$

$$P\left(X^{(j)}=a_{j l} | Y=c_{k}\right)=\frac{\sum_{i=1}^{N} I\left(x_{i}^{(j)}=a_{j l}, y_{i}=c_{k}\right)}{\sum_{i=1}^{N} I\left(y_{i}=c_{k}\right)}$$

$$j=1,2, \cdots, n ; \quad l=1,2, \cdots, S_{j} ; \quad k=1,2, \cdots, K$$

2. 对于给定的实例$x=\left(x^{(1)}, x^{(2)}, \cdots, x^{(n)}\right)^{\mathrm{T}}$，计算

$$P\left(Y=c_{k}\right) \prod_{j=1}^{n} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right), \quad k=1,2, \cdots, K$$

3. 确定实例$x$的类

$$y=\arg \max _{c_{k}} P\left(Y=c_{k}\right) \prod_{j=1}^{n} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)$$

## 贝叶斯估计

由于上节采用极大似然估计，因此会出现0的情况，即某个类别没有数据，从而造成分类偏差。为了解决这个问题，就把上节的算法采用条件贝叶斯估计，即$P_{\lambda}\left(X^{(j)}=a_{j l} | Y=c_{k}\right)$变为

$$P_{\lambda}\left(X^{(j)}=a_{j l} | Y=c_{k}\right)=\frac{\sum_{i=1}^{N} I\left(x_{i}^{(j)}=a_{j l}, y_{i}=c_{k}\right)+\lambda}{\sum_{i=1}^{N} I\left(y_{i}=c_{k}\right)+S_{j} \lambda}$$

引入$\lambda \geqslant 0$，当$\lambda=0$时就是极大似然估计。**一般$\lambda=1$，即拉普拉斯平滑。其中$S_{j}$表示特征的数量。** 因此$P_{\lambda}$永远不会为0。

$$P_{\lambda}\left(X^{(j)}=a_{j l} | Y=c_{k}\right)>0$$

$$\sum_{l=1}^{S_{j}} P\left(X^{(j)}=a_{j l} | Y=c_{k}\right)=1$$

同样，先验概率的贝叶斯估计变为

$$P_{\lambda}\left(Y=c_{k}\right)=\frac{\sum_{i=1}^{N} I\left(y_{i}=c_{k}\right)+\lambda}{N+K \lambda}$$

**其中$K$表示$Y$类别的数量。**

# 作业