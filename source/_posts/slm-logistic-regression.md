---
title: S6-逻辑斯谛回归与最大熵模型
mathjax: true
date: 2020-03-25 09:25:09
updated: {{ date }}
tags:
categories: [统计学习方法]
---

# 综述

逻辑斯谛回归模型与最大熵模型都是对数线性模型。

所谓对数线性模型就是对模型两边取对数，就得到一个关于$w$和$x$的线性模型，即$\ln P(Y=c_{i}|X=x) = w_{i} \cdot x$。在逻辑斯谛回归模型中，对数模型是关于$x$的线性函数，而在最大熵模型中，对数模型是关于$x$函数的线性函数。

逻辑斯谛回归模型与最大熵模型的区别在于，逻辑斯谛回归模型是判别模型，而最大熵模型是生成模型。因此在最大熵模型中不仅$Y$具有随机性，$X$也是一个随机变量，并对应这一个经验分布$\tilde{P}\left ( X\right )$。

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

从上式可以知道，在逻辑斯谛回归模型中，输出$Y=1$的对数几率是输入$x$的线性函数或者说输出$Y=1$的对数几率是由输入$x$的线性函数表示的。

**同时，由于$P(Y=1 | x)$的取值范围是$\left [ 0， 1\right ]$，但是在线性函数$w \cdot x$中不会规定$w$只能取某个范围内的值，因此$w \cdot x$的取值为$\left ( -\infty , +\infty \right )$，所以把$P(Y=1 | x)$取值区间和$w \cdot x$的取值区间进行对应变换的函数方法就叫做$\operatorname{logit}$函数（逻辑斯谛函数），因此就得到上式函数，并反解出下面的模型表达。**

$$P(Y=1 | x)=\frac{\exp (w \cdot x)}{1+\exp (w \cdot x)}$$

可以知道，**当线性函数$w \cdot x$的值越接近正无穷，那么概率值就越接近1；而当线性函数$w \cdot x$的值越接近负无穷，那么概率值就越接近0（与Sigmoid激活函数的性质相同）。**

## 模型参数估计

给定训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots, \left(x_{N}, y_{N}\right)\right\}$，其中$x_{i} \in \mathbf{R}^{n}$，$y_{i} \in\{0,1\}$，通过极大似然法估计模型参数，从而得到逻辑斯谛回归模型

由逻辑斯谛函数可以知道

$$P(Y=1 | x)=\pi(x), P(Y=0 | x)=1-\pi(x)$$

接下来就要求出$w$，在概率模型中，参数的求解大多用极大似然估计，即求样本的联合概率密度

$$P_{w}\left ( y \mid x\right ) = \left[\pi\left(x\right)\right]^{y}\left[1-\pi\left(x\right)\right]^{1-y}$$

对于$N$个样本来说，就可以得到

$$\max L(w) = \prod_{i=1}^{N}\left[\pi\left(x_{i}\right)\right]^{y_{i}}\left[1-\pi\left(x_{i}\right)\right]^{1-y_{i}}$$

则两边取对数可以得到最大似然函数

$$\begin{aligned}
\max \ln L(w) &=\sum_{i=1}^{N}\left[y_{i} \log \pi\left(x_{i}\right)+\left(1-y_{i}\right) \log \left(1-\pi\left(x_{i}\right)\right)\right] \\
&=\sum_{i=1}^{N}\left[y_{i} \log \frac{\pi\left(x_{i}\right)}{1-\pi\left(x_{i}\right)}+\log \left(1-\pi\left(x_{i}\right)\right)\right] \\
&=\sum_{i=1}^{N}\left[y_{i}\left(w \cdot x_{i}\right)-\log \left(1+\exp \left(w \cdot x_{i}\right)\right)\right]
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

$$w=\arg \max L_{\tilde{P}}\left(P_{w}\right)=\log \prod_{x, y} P(y | x)^{\tilde{P}(x, y)}$$

所以$P^{*}=P_{w^{*}}=P_{w^{*}}(y | x)$就是学习得到的最优模型。

## 极大似然估计

**对偶函数的极大化等价于最大熵模型的极大似然估计，** 证明如下：

已知训练数据的经验概率分布$\tilde{P}(X, Y)$，条件概率分布$P(Y | X)$的对数似然函数可以表示为

$$L_{\tilde{P}}\left(P_{w}\right)=\log \prod_{x, y} P(y | x)^{\tilde{P}(x, y)}=\sum_{x, y} \tilde{P}(x, y) \log P(y | x)$$

> 对上式的推导

最大似然函数的一般形式是样本集$X$中各个样本的联合概率

$$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)=\prod_{i=1}^{n} p\left(x_{i} ; \theta\right)$$

假设样本集的大小为$n$个，$X$的取值有$k$个，分别是$v_{1}, v_{2}, \dots, v_{k}$，$C\left(X=v_{i}\right)$表示在观测值中样本$v_{i}$出现的频率，所以$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)$可以表示为

$$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)=\prod_{i=1}^{k} p\left(v_{i} ; \theta\right)^{C\left(X=v_{i}\right)}$$

对等式两边同时开$n$次方，得到

$$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)^{\frac{1}{n}}=\prod_{i=1}^{k} p\left(v_{i} ; \theta\right)^{\frac{\mathcal{C}\left(X=v_{i}\right)}{n}}$$

因为经验概率$\tilde{p}(x)=\frac{C\left(X=v_{i}\right)}{n}$，所以上式可以改写为

$$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)^{\frac{1}{n}}=\prod_{i=1}^{k} p\left(v_{i} ; \theta\right)^{\tilde{p}(x)}$$

由于求$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)$最大值和求$L\left(x_{1}, x_{2}, \ldots, x_{n} ; \theta\right)^{\frac{1}{n}}$最大值相同，因此上式可以表示为

$$L\left(x ; \theta\right)=\prod_{i=1}^{k} p\left(v_{i} ; \theta\right)^{\tilde{p}(x)}$$

因此，对$L_{\tilde{P}}\left(P_{w}\right)$两边取对数，就可以得到

$$L_{\tilde{P}}\left(P_{w}\right)=\log \prod_{x, y} P(y | x)^{\tilde{P}(x, y)}=\sum_{x, y} \tilde{P}(x, y) \log P(y | x)$$

把最大熵模型

$$P_{w}(y | x)=\frac{1}{Z_{w}(x)} \exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right)$$

代入上式$L_{\tilde{P}}\left(P_{w}\right)$，可以得到

$$\begin{aligned}
L_{\tilde{P}}\left(P_{w}\right) &=\sum_{x, y} \tilde{P}(x, y) \log P(y | x) \\
&=\sum_{x, y} \tilde{P}(x, y) \log \left( \frac{1}{Z_{w}(x)} \exp \left(\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right) \right) \\
&=\sum_{x, y} \tilde{P}(x, y) \sum_{i=1}^{n} w_{i} f_{i}(x, y)-\sum_{x, y} \tilde{P}(x, y) \log Z_{w}(x) \\
&=\sum_{x, y} \tilde{P}(x, y) \sum_{i=1}^{n} w_{i} f_{i}(x, y)-\sum_{x} \tilde{P}(x) \log Z_{w}(x)
\end{aligned}$$

**由对偶函数可以得到（没懂）**

因为$\sum_{y} P(y | x)=1$，所以

$$\begin{aligned}
\Psi(w)=& \sum_{x, y} \tilde{P}(x) P(y | x) \log P(y | x)+w_{0}\left(1-\sum_{y} P(y | x)\right)+ \sum_{i=1}^{n} w_{i}\left(\sum_{x, y} \tilde{P}(x, y) f_{i}(x, y)-\sum_{x, y} \tilde{P}(x) P(y | x) f_{i}(x, y)\right)\\
=& \sum_{x, y} \tilde{P}(x) P_{w}(y | x) \log P_{w}(y | x)+ \sum_{i=1}^{n} w_{i}\left(\sum_{x, y} \tilde{P}(x, y) f_{i}(x, y)-\sum_{x, y} \tilde{P}(x) P_{w}(y | x) f_{i}(x, y)\right)\\
=& \sum_{x, y} \tilde{P}(x, y) \sum_{i=1}^{n} w_{i} f_{i}(x, y)+\sum_{x, y} \tilde{P}(x) P_{w}(y | x)\left(\log P_{w}(y | x)-\sum_{i=1}^{n} w_{i} f_{i}(x, y)\right) \\
=& \sum_{x, y} \tilde{P}(x, y) \sum_{i=1}^{n} w_{i} f_{i}(x, y)-\sum_{x, y} \tilde{P}(x) P_{w}(y | x) \log Z_{w}(x) \\
=& \sum_{x, y} \tilde{P}(x, y) \sum_{i=1}^{n} w_{i} f_{i}(x, y)-\sum_{x} \tilde{P}(x) \log Z_{w}(x)
\end{aligned}$$

**证明得到对偶函数的极大化等价于最大熵模型的极大似然估计。**

# 工程实践

```python
sklearn.linear_model.LogisticRegression(
    penalty='l2', dual=False, tol=0.0001, C=1.0, 
    fit_intercept=True, intercept_scaling=1, class_weight=None, 
    random_state=None, solver='lbfgs', max_iter=100, multi_class='auto', 
    verbose=0, warm_start=False, n_jobs=None, l1_ratio=None)
```

1. penalty（类型：str，默认值：l2）：正则化项。scikit-learn中支持4种正则化项，分别是：
   - l1：L1正则化项。
   - l2：“newton-cg“、“sag“、“lbfgs“求解器只支持L2正则化项。
   - elasticnet：该正则化项值支持”saga“求解器。
   - none：不是用正则化项，但“liblinear”求解器不支持该属性值。
2. dual（类型：bool，默认值：False）：使用对偶或者原始形式。对偶形式只在“liblinear”求解器的L2正则化项时才有效。当样本数>特征数时，属性值建议为False。
3. tol（类型：float，默认值：1e-4）：迭代终止的阈值。
4. C（类型：float，默认值：1.0）：正则化系数的倒数。
5. fit_intercept（类型：bool，默认值：True）：是否有截距。
6. intercept_scaling（类型：float，默认值：1）：当求解器是“liblinear”，并且fit_intercept为True时，该属性才有效。此时$x$就变成[x, self.intercept_scaling]，即合成特征。此时截距就变成intercept_scaling * synthetic_feature_weight。
7. class_weight（类型：dict、 列表dict或"balanced"，默认值：None）：类别权重。
8. random_state（类型：int，默认值：None）：随机数种子。
9. solver（类型：str，默认值：lbfgs）：求解器。
   - newton-cg：可以处理多类问题，并支持多项损失函数。支持L2正则化项或没有正则化项。
   - lbfgs：支持L2正则化项或没有正则化项。
   - liblinear：小数据集，该求解器更好。可以处理多类问题，但只支持OVR模式。支持L1正则化项，但不支持没有正则化项。
   - sag（随机平均梯度下降求解器，Stochastic Average Gradient descent solver）：大数据集，该求解器更好。可以处理多类问题，并支持多项损失函数。支持L2正则化项或没有正则化项。
   - saga：大数据集，该求解器更好。可以处理多类问题，并支持多项损失函数。支持L2、L1、elasticnet正则化项或没有正则化项。
10. max_iter（类型：int，默认值：100）：最大迭代次数。
11. multi_class（类型：str，默认值：auto）：多分类的方式。
    - auto：当是二分类或求解器为liblinear时，采用OVR。其他情况采用multinomial。
    - ovr：
    - multinomial：
12. verbose（类型：int，默认值：0）：对于liblinear和lbfgs解算器，该值可以是任何正数。
13. warm_start（类型：bool，默认值：False）：当为False时，每次调用该函数会擦出前一次得到的参数，如果是True，那么会利用前一次的值进行初始化，但对于liblinear求解器无效。
14. n_jobs（类型：int，默认值：None）：当multi_class为ovr时采用并行计算。当如果是liblinear求解器，那么该参数无效。
15. l1_ratio（类型：float，默认值：None）：取值范围是$\left [ 0,1 \right ]$之间。只有到正则化项为“elasticnet”时，该参数才有效。该参数为0时，等价于L2正则化。该值为1时，等价于L1正则化。

# 作业

0、要求逻辑斯缔回归模型中的$w$，可以采用梯度下降法来实现，因此就需要对$L(w)$进行求导，从而得到$w$的更新值，当更新值小于某个值时，则$w$更新停止。

已知

$$L(w) = \sum_{i=1}^{N}\left[y_{i}\left(w \cdot x_{i}\right)-\log \left(1+\exp \left(w \cdot x_{i}\right)\right)\right]$$

对$w$求导就可以得到

$$\begin{aligned} \frac{\partial L(w)}{\partial w} &=\sum_{i=1}^{N}y_{i}  \left( x_{i}-\frac{\exp \left(w \cdot x_{i}\right) x_{i}}{1+\exp \left(w \cdot x_{i}\right)}\right) \\ &=\sum_{i=1}^{N}y_{i}  \left(y_{i} x_{i}-\frac{x_{i}}{1+\exp \left(-w \cdot x_{i}\right)}\right) \\ &=\sum_{i=1}^{N} x_{i}\left(y_{i}-\frac{1}{1+ \exp\left(-w \cdot x_{i}\right)}\right)\end{aligned}$$

因此，就可以通过$\frac{\partial L(w)}{\partial w}$来更新$w$

$$w \leftarrow w+\eta \frac{\partial L(w)}{\partial w}$$

1、已知训练数据集$D=\{(3, 3, 3), (4, 3, 2), (2, 1, 2), (1, 1, 1), (-1, 0, 1), (2, -2, 1)\}$，$Y=\{1, 1, 1, 0, 0, 0\}$，用python 自编程实现逻辑斯缔回归模型，并对点$(1, 2, -2)$进行分类。

```python
import numpy as np
import pandas as pd
import time

# 逻辑斯谛回归模型
class LogisticRegression:
    """
    input_x：输入训练特征
    input_y：训练结果
    learn_rate：学习率
    max_iter：最大迭代次数
    tol：迭代停止阈值
    """
    def __init__(self, input_x, input_y, learn_rate=0.1, max_iter=10000, tol=1e-2):
        self.input_x = input_x
        self.input_y = input_y.reshape(len(input_y), 1)
        self.learn_rate = learn_rate
        self.max_iter = max_iter
        self.tol = tol
        self.w = 0

        # 对数据进行预处理
        self.input_x = self.preprocessing(input_x)

    # 数据预处理
    def preprocessing(self, x):
        # 获取特征数量
        row = x.shape[0]
        # 获取X的最后一列数据
        one_data = np.ones(row).reshape(row, 1)
        # 把wx+b->w
        x = np.hstack((x, one_data))

        return x

    # 计算导后方程中的sigmod
    def sigmod(self, x):
        return 1 / (1 + np.exp(-x))

    def fit(self):
        # 初始化权重
        self.w = np.zeros(self.input_x.shape[1], dtype=np.float).reshape(1, self.input_x.shape[1])
        # 记录迭代次数
        times = 0

        for i in range(self.max_iter):
            # 计算梯度值
            z = self.sigmod(np.dot(self.input_x, self.w.T))
            grad = self.input_x * (self.input_y - z)
            # 求每个分量的梯度和
            grad = grad.sum(axis=0)
            if (np.abs(grad) <= self.tol).all():
                break
            else:
                self.w += self.learn_rate * grad
                times += 1

        print('迭代次数：{}次'.format(times))
        print('最终权重：{}次'.format(self.w))

    # 预测实例
    def predict(self, x):
        # 计算预测的概率
        p = self.sigmod(np.dot(self.preprocessing(x), self.w.T))
        print('Y = 1的概率为：{:.2%}'.format(p[0, 0]))

        # 计算结果
        if p[0, 0] > 0.5:
            p = 1
        else:
            p = 0

        return p


if __name__ == '__main__':
    # 计算开始时间
    star = time.time()

    data = np.array([
        [3, 3, 3, 1], [4, 3, 2, 1], [2, 1, 2, 1],
        [1, 1, 1, 0], [-1, 0, 1, 0], [2, -2, 1, 0],
    ])

    test_x = np.array([[1, 2, -2]])

    train_x = data[:, 0:3]
    train_y = data[:, 3]

    logisticRegression = LogisticRegression(train_x, train_y)
    # 拟合数据
    logisticRegression.fit()
    # 预测数据
    print(logisticRegression.predict(test_x))

    # 计算结束事时间
    end = time.time()
    print('用时：{:.3f}s'.format(end - star))
```

2、试调用sklearn.linear_model的LogisticRegression模块，对点$(1, 2, -2)$进行分类，尝试改变参数，选择不同算法，如梯度下降法和拟牛顿法。

```python
import numpy as np
import pandas as pd
import time

from sklearn.linear_model import LogisticRegression


if __name__ == '__main__':
    # 计算开始时间
    star = time.time()

    data = np.array([
        [3, 3, 3, 1], [4, 3, 2, 1], [2, 1, 2, 1],
        [1, 1, 1, 0], [-1, 0, 1, 0], [2, -2, 1, 0],
    ])

    test_x = np.array([[1, 2, -2]])

    train_x = data[:, 0:3]
    train_y = data[:, 3]

    logisticRegression = LogisticRegression(max_iter=1000)
    # 拟合数据
    logisticRegression.fit(train_x, train_y)
    # 预测数据
    print('训练集的准确率：{}'.format(logisticRegression.score(train_x, train_y)))
    print('预测结果：{}'.format(logisticRegression.predict(test_x)))

    # 计算结束事时间
    end = time.time()
    print('用时：{:.3f}s'.format(end - star))
```