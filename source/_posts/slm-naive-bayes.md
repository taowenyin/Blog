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

$$P\left(Y=c_{k} | X=x\right) =\frac{P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}{\underset{k}{\sum} P\left(X=x | Y=c_{k}\right) P\left(Y=c_{k}\right)}$$

代入条件概率分布得到

$$P\left(Y=c_{k} | X=x\right) =\frac{P\left(Y=c_{k}\right) \prod_{j} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}{\underset{k}{\sum} P\left(Y=c_{k}\right) \prod_{j} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}$$

于是，朴素贝叶斯分类器可表示为

$$y=f\left ( x\right )=arg\ \underset{c_{k}}{max}\frac{P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}{\underset{k}{\sum} P\left(Y=c_{k}\right) \underset{j}{\prod} P\left(X^{(j)}=x^{(j)} | Y=c_{k}\right)}$$

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