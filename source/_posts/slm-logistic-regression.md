---
title: S6-逻辑斯谛回归与最大熵模型
mathjax: true
date: 2020-03-25 09:25:09
updated: {{ date }}
tags:
categories: [统计学习方法]
---

# 逻辑斯谛回归模型

## 逻辑斯谛分布

设$X$是连续随机变量，$X$服从逻辑斯谛分布是指$X$具有如下的分布函数和密度函数：

$$F(x)=P(X \leqslant x)=\frac{1}{1+\mathrm{e}^{-(x-\mu) / \gamma}}$$

$$f(x)=F^{\prime}(x)=\frac{\mathrm{e}^{-(x-\mu) / \gamma}}{\gamma\left(1+\mathrm{e}^{-(x-\mu) / \gamma}\right)^{2}}$$

其中$\mu$为位置参数，$\gamma>0$为形状参数。

{% asset_img logistic-func.png 逻辑斯谛密度和分布函数 %}

图1：逻辑斯谛密度和分布函数

分布函数是$S$形曲线（sigmoid curve），并且曲线是以$\left(\mu, \frac{1}{2}\right)$作为中心点，满足

$$F(-x+\mu)-\frac{1}{2}=-F(x+\mu)+\frac{1}{2}$$

$S$曲线在中心点增长较快，两端较慢，并且形状参数$\gamma$的值越小，曲线在中心附近增长越快，即曲线越陡。**当$\mu=0$和$\gamma=1$时该函数就为神经网络中的Sigmoid激活函数。**

## 二项逻辑斯谛回归模型

二项逻辑斯谛回归模型是一种分类模型，由条件概率分布$P(Y | X)$表示，其中随机变量$X$取值为实数，随机变量$Y$取值为1或0，因此二项逻辑斯谛回归模型的条件概率分布：

$$P(Y=1 | x)=\frac{\exp (w \cdot x+b)}{1+\exp (w \cdot x+b)}$$

$$P(Y=0 | x)=\frac{1}{1+\exp (w \cdot x+b)}$$

其中$x \in \mathbf{R}^{n}$是输入，$Y \in\{0,1\}$是输出，$w \in \mathbf{R}^{n}$和$b \in \mathbf{R}$是参数，$w$称为权值向量，$b$称为偏置，$w \cdot x$为$w$和$x$的内积。

对于给定的输入实例$x$，逻辑斯谛回归比较$P(Y=1 | x)$和$P(Y=0 | x)$两个条件概率值大小，实例$x$则分到概率大的那一类。如果将$w$和$x$分别扩展为$w=\left(w^{(1)}, w^{(2)}, \cdots, w^{(n)}, b\right)^{\mathrm{T}}$和$x=\left(x^{(1)}, x^{(2)}, \cdots, x^{(n)}, 1\right)^{\mathrm{T}}$，那么二项逻辑斯谛回归模型就变为

$$P(Y=1 | x)=\frac{\exp (w \cdot x)}{1+\exp (w \cdot x)}$$

$$P(Y=0 | x)=\frac{1}{1+\exp (w \cdot x)}$$

**事件的几率（odds）是该事件发生的概率与不发生概率的比值。** 如果事件的发生的概率是$p$，那么该事件的几率为$\frac{p}{1-p}$，两边取对数，得到logit函数是

$$\operatorname{logit}(p)=\log \frac{p}{1-p}$$

而在逻辑斯谛回归模型中，由$P(Y=1 | x)$和$P(Y=0 | x)$两个模型可以得到

$$\log \frac{P(Y=1 | x)}{1-P(Y=1 | x)}=w \cdot x$$

从上式可以直到，在逻辑斯谛回归模型中，输出$Y=1$的对数几率是输入$x$的线性函数或者说输出$Y=1$的对数几率是由输入$x$的线性函数表示的。

同时，由

$$P(Y=1 | x)=\frac{\exp (w \cdot x)}{1+\exp (w \cdot x)}$$

可以知道，**当线性函数$w \cdot x$的值越接近正无穷，那么概率值就越接近1；而当线性函数$w \cdot x$的值越接近负无穷，那么概率值就越接近0（与Sigmoid激活函数的性质相同）。**

## 模型参数估计

给定训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots, \left(x_{N}, y_{N}\right)\right\}$，其中$x_{i} \in \mathbf{R}^{n}$，$y_{i} \in\{0,1\}$，通过极大似然法估计模型参数，从而得到逻辑斯谛回归模型

设

$$P(Y=1 | x)=\pi(x), P(Y=0 | x)=1-\pi(x)$$

则似然函数为

$$\prod_{i=1}^{N}\left[\pi\left(x_{i}\right)\right]^{y_{i}}\left[1-\pi\left(x_{i}\right)\right]^{1-y_{i}}$$

则两边取对数可以得到似然函数

$$\begin{aligned}
L(w) &=\sum_{i=1}^{N}\left[y_{i} \log \pi\left(x_{i}\right)+\left(1-y_{i}\right) \log \left(1-\pi\left(x_{i}\right)\right)\right] \\
&=\sum_{i=1}^{N}\left[y_{i} \log \frac{\pi\left(x_{i}\right)}{1-\pi\left(x_{i}\right)}+\log \left(1-\pi\left(x_{i}\right)\right)\right] \\
&=\sum_{i=1}^{N}\left[y_{i}\left(w \cdot x_{i}\right)-\log \left(1+\exp \left(w \cdot x_{i}\right)\right]\right.
\end{aligned}$$

对$L(w)$求极大值，得到$w$的估计值

**此时，问题就变成以对数似然函数为目标函数的最优化问题，通常采用梯度下降法或拟牛顿法求解。** 得到$w$的极大似然估计值$\hat{w}$，此时的逻辑斯谛回归模型为

$$P(Y=1 | x)=\frac{\exp (\hat{w} \cdot x)}{1+\exp (\hat{w} \cdot x)}$$

$$P(Y=0 | x)=\frac{1}{1+\exp (\hat{w} \cdot x)}$$

## 多项逻辑斯谛回归

把二项逻辑斯谛回归模型推广到多项逻辑斯谛回归模型。假设离散型随机变量$Y$的取值集合是$\{1,2, \cdots, K\}$，那么多项逻辑斯谛回归模型是

$$\begin{aligned}
P(Y=k | x) &=\frac{\exp \left(w_{k} \cdot x\right)}{1+\sum_{k=1}^{K-1} \exp \left(w_{k} \cdot x\right)}, \quad k=1,2, \cdots, K-1 \\
P(Y=K | x) &=\frac{1}{1+\sum_{k=1}^{K-1} \exp \left(w_{k} \cdot x\right)}
\end{aligned}$$

其中$x \in \mathbf{R}^{n+1}$，$w_{k} \in \mathbf{R}^{n+1}$

同样，参数估计可以推广到多项逻辑斯谛回归。

# 最大熵模型

最大熵原理认为，熵最大的模型是最好的模型。假设离散随机变量$X$的概率分布是$P(X)$，则其熵是

$$H(P)=-\sum_{x} P(x) \log P(x)$$

熵满足不等式：

$$0 \leqslant H(P) \leqslant \log |X|$$

式中，$|X|$是$X$的取值个数，当且仅当$X$的分布是均匀分布时右边的等号成立，也就是说，当$X$服从均匀分布时，熵最大。

给定训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

可以确定联合分布$P(X, Y)$的经验分布和边缘分布$P(X)$的经验分布，分别是$\tilde{P}(X, Y)$和$\tilde{P}(X)$，即

$$\tilde{P}(X=x, Y=y)=\frac{\nu(X=x, Y=y)}{N}$$

$$\tilde{P}(X=x)=\frac{\nu(X=x)}{N}$$

其中，$\nu(X=x, Y=y)$表示训练数据中样本$(x, y)$出现的频数，$\nu(X=x)$表示训练数据中输入$x$出现的频数，$N$表示训练样本容量。

用特征函数$f(x, y)$表述输入$x$与输出$y$之间的关系，可以定义为

$$f(x, y)=\left\{\begin{array}{l}1, x \text{与} y \text{满足某一事实} \\ 0, \text{否则}\end{array}\right.$$

则特征函数$f(x, y)$关于经验分布$\tilde{P}(X, Y)$的期望值$E_{\tilde{P}}(f)$可以表示为

$$E_{\tilde{P}}(f)=\sum_{x, y} \tilde{P}(x, y) f(x, y)$$

特征函数$f(x, y)$关于模型$P(Y | X)$与经验分布$\tilde{P}(X)$的期望值$E_{P}(f)$可以表示为

$$E_{P}(f)=\sum_{x, y} \tilde{P}(x) P(y | x) f(x, y)$$

如果模型能够从训练数据中获取信息，那么就可以假设$E_{P}(f)$和$E_{\tilde{P}}(f)$相等

$$E_{P}(f)=E_{\tilde{P}}(f)$$

或

$$\sum_{x, y} \tilde{P}(x) P(y | x) f(x, y)=\sum_{x, y} \tilde{P}(x, y) f(x, y)$$

上式作为模型学习的约束条件。假如由$n$个特征函数$f_{i}(x, y)$，$i=1,2, \cdots, n$，就有$n$个约束条件。

假设满足所有约束条件的模型集合为

$$\mathcal{C} \equiv\left\{P \in \mathcal{P} | E_{P}\left(f_{i}\right)=E_{\tilde{P}}\left(f_{i}\right), \quad i=1,2, \cdots, n\right\}$$

定义在条件概率分布$P(Y | X)$上的条件熵为

$$H(P)=-\sum_{x, y} \tilde{P}(x) P(y | x) \log P(y | x)$$

则模型集合$\mathcal{C}$中的条件熵$H(P)$最大的模型称为最大熵模型。

## 最大熵模型的学习

给定训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$以及特征函数$f_{i}(x, y)$，$i=1,2, \cdots, n$，则最大熵模型的学习等价于约束最优化问题：

$$\begin{array}{ll}
\underset{P \in \mathbf{C}}{max} & H(P)=-\sum_{x, y} \tilde{P}(x) P(y | x) \log P(y | x) \\
\text { s.t. } & E_{P}\left(f_{i}\right)=E_{\tilde{P}}\left(f_{i}\right), \quad i=1,2, \cdots, n \\
& \sum_{y} P(y | x)=1
\end{array}$$

按照解决最优化问题的习惯，把最优化问题转化为最小化问题

$$\begin{array}{ll}
\underset{P \in \mathbf{C}}{min}&-H(P)=\sum_{x, y} \tilde{P}(x) P(y | x) \log P(y | x)\\
\text { s.t. } & E_{P}\left(f_{i}\right)-E_{\tilde{P}}\left(f_{i}\right)=0, \quad i=1,2, \cdots, n\\
&\sum_{y} P(y | x)=1
\end{array}$$

这里将最优化问题转化为无约束的对偶问题进行求解，从而解决原始问题。

> 第一步：引入拉格朗日乘子$w_{0}, w_{1}, w_{2}, \cdots, w_{n}$，定义拉格朗日函数$L(P, w)$

$$\begin{aligned}
L(P, w) \equiv &-H(P)+w_{0}\left(1-\sum_{y} P(y | x)\right)+\sum_{i=1}^{n} w_{i}\left(E_{\tilde{P}}\left(f_{i}\right)-E_{P}\left(f_{i}\right)\right) \\
=& \sum_{x, y} \tilde{P}(x) P(y | x) \log P(y | x)+w_{0}\left(1-\sum_{y} P(y | x)\right)+ \sum_{i=1}^{n} w_{i}\left(\sum_{x, y} \tilde{P}(x, y) f_{i}(x, y)-\sum_{x, y} \tilde{P}(x) P(y | x) f_{i}(x, y)\right)
\end{aligned}$$

此时的原始问题为

$$\min _{P \in \mathbf{C}} \max _{w} L(P, w)$$

那么对偶问题就是

$$\max _{\boldsymbol{w}} \min _{\boldsymbol{P} \in \mathbf{C}} L(P, w)$$

即先对$P$求最小值，然后在对$w$求最大值。

> 第二步：先求对偶问题内部的极小化问题$\underset{P \in \mathbf{C}}{min} L(P, w)$，即求$L(P, w)$对$P(y | x)$的偏导。

$$\begin{aligned}
\frac{\partial L(P, w)}{\partial P(y | x)} &=\sum_{x, y} \tilde{P}(x)(\log P(y | x)+1)-\sum_{y} w_{0}-\sum_{x, y}\left(\tilde{P}(x) \sum_{i=1}^{n} w_{i} f_{i}(x, y)\right) \\
&=\sum_{x, y} \tilde{P}(x)\left(\log P(y | x)+1-w_{0}-\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right)
\end{aligned}$$

令偏导等于0，从而得到

$$P(y | x)=\exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)+w_{0}-1\right)=\frac{\exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right)}{\exp \left(1-w_{0}\right)}$$

由于$\sum_{y} P(y | x)=1$，所以

$$P_{w}(y | x)=\frac{1}{Z_{w}(x)} \exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right)$$

其中

$$Z_{w}(x)=\sum_{y} \exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right)$$

$Z_{w}(x)$称为规范化因子，$f_{i}(x, y)$是特征函数，$w_{i}$特征权值。并且模型$P_{w}=P_{w}(y | x)$就是最大熵模型，$w$是最大熵模型中的参数向量。

> 第三步：求外部的极大化问题，得到$w^{*}$。

所以$P^{*}=P_{w^{*}}=P_{w^{*}}(y | x)$就是学习得到的最优模型。