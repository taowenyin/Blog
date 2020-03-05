---
title: S1-统计学习及监督学习概论
mathjax: true
date: 2020-02-29 09:47:22
updated: {{ date }}
tags:
categories: [统计学习方法]
---

# 统计学习

统计学习组成由监督学习（supervised learning）、无监督学习（unsupervised learning）、强化学习（reinforcement learning）等组成。

1. 假设空间（hypothesis space）：从给定的、有限的、用于学习的训练数据（training data）集合出发，假设数据是独立同分布产生的；并且假设学习的模型属于某个函数的集合。
2. 评价准则（evaluation criterion）：从假设空间选取一个最有模型，使它对已知的训练数据及位置的测试数据（test data）在给定的评价准则下有最优的预测，最优模型的选取由算法实现。

统计学习方法包括：模型的假设空间（模型）、模型选择的准则（策略）和模型学习算法（算法）。

统计学习方法的步骤：

>* Step1：得到一个有限的训练数据集合；
>* Step2：确定包含所有可能的模型的假设空间，即学习模型的集合；
>* Step3：确定模型选择的准则，即学习的策略；
>* Step4：实现求解最优模型的算法，即学习的算法；
>* Step5：通过学习方法选择最优模型；
>* Step6：利用学习的最优模型对新数据进行预测或分析。

# 统计学习的分类

## 基本分类

### 监督学习

从标注数据中学习预测模型的机器学习问题。**监督学习的本质是学习输入到输出的映射的统计规律。** 模型可以实现分类和回归。

监督学习的模型可以是概率模型或非概率模型，即条件概率分布$P\left ( Y \mid X\right )$或决策函数$Y=f\left ( X\right )$。

### 无监督学习

从无标注数据中学习预测模型的机器学习问题。**无监督学习的本质是学习数据中的统计规律或潜在结构。** 模型可以实现聚类、降维或概率估计。

无监督学习的模型可以表示为$z = g\left ( x\right )$，条件概率分布为$P\left ( z \mid x\right )$或$P\left ( x \mid z\right )$。

### 强化学习

智能系统在与环境的连续互动中学习最优行为策略的机器学习问题。**强化学习的本质是学习最优的序贯决策。** 强化学习的马尔科夫决策过程是状态、奖励、动作序列上的随机过程，有五元组组成$\left \langle S, A, P, r, \gamma \right \rangle$。

>* $S$：有限状态（state）的集合；
>* $A$：有限动作（action）的集合；
>* $P$：状态转移概率（transition probability）函数$P\left(s^{\prime} | s, a\right)=P\left(s_{t+1} = s^{\prime} | s_{t}=s, a_{t}=a\right)$；
>* $r$：奖励函数（reward function）$r(s, a)=E\left(r_{t+1} | s_{t}=s, a_{t}=a\right)$；
>* $\gamma$：衰减系数（discount factor）$\gamma \in[0,1]$；

强化学习的目标是在所有可能的策略中选出价值函数最大的策略。

强化学习分为有模型方法（马尔科夫决策过程）和无模型方法（基于策略、基于价值）。

### 半监督学习

利用标注数据和未标注数据学习预测模型的机器学习问题。旨在利用未标注数据中的信息，辅助标注数据进行监督学习，以较低的成本达到很好的学习效果。

### 主动学习

机器不断主动给出实例进行标注，然后利用标注数据学习预测模型的机器学习问题。旨在找出对学习最优帮助的实例进行标注，以最小的标注代价，达到较好的学习效果。

## 按模型分类

1、概率和非概率模型

条件概率分布$P\left ( y \mid x\right )$和函数$y=f\left ( x\right )$可以进行相互转化。条件概率分布最大化后得到函数，函数归一化后得到条件概率分布。

无论模型如何复杂，均可以用最基本的加法规则和乘法跪在进行概率推理。

$$\begin{matrix}
\text{加法规则:} & P\left ( x\right ) = \underset{y}{\sum}P\left ( x,y\right )\\ 
\text{乘法规则:} & P\left ( x,y\right ) = P\left ( x\right )P\left ( y\mid x\right )
\end{matrix}$$

其中$x$和$y$是随机变量

2、线性模型与非线性模型

如果模型$y=f\left ( x\right )$是线性函数，那么模型是线性模型，否则就是非线性模型。

3、参数模型和非参数模型

参数模型：模型的参数维度固定。

非参数模型：模型的参数维度不固定，甚至无穷大。

## 按技巧分类

1、贝叶斯学习

假设随机变量$D$表示数据，随机变量$\theta$表示模型参数。根据贝叶斯定理，可以用下面公式计算后验概率$P(\theta | D)$：

$$P(\theta | D)=\frac{P(\theta) P(D | \theta)}{P(D)}$$

其中$P(\theta)$是先验概率，$P(\theta | D)$是似然概率。

2、核方法

使用核函数表示和学习非线性模型的一种机器学习方法。

# 统计学习方法三要素

经验风险最小化求得的最优模型就是

$$\min _{f \in \mathcal{F}} \frac{1}{N} \sum_{i=1}^{N} L\left(y_{i}, f\left(x_{i}\right)\right)$$

其中$\mathcal{F}$是假设空间。

当样本足够大时，经验风险最小化能保证很好的学习效果。但是当样本容量很小时，经验风险的效果就未必很好，会产生“过拟合”现象。

结构化风险最小化（正则化）是为了防止过拟合而提出的。结构风险在经验风险的基础上加上了模型复杂度的正则化向或惩罚项。

$$R_{\mathrm{srm}}(f)=\frac{1}{N} \sum_{i=1}^{N} L\left(y_{i}, f\left(x_{i}\right)\right)+\lambda J(f)$$

其中$J(f)$表示模型复杂度。$\lambda \geqslant 0$是系数，用以权衡经验风险和模型复杂度。

# 正则化与交叉验证

在回归问题中，损失函数是平方项，那么正则化项可以是参数向量的$L_{2}$范数。

$$L(w)=\frac{1}{N} \sum_{i=1}^{N}\left(f\left(x_{i} ; w\right)-y_{i}\right)^{2}+\frac{\lambda}{2}\|w\|^{2}$$

其中$\|w\|$表示参数向量$w$的$L_{2}$范数，$\frac{1}{2}$是为了在求导时约掉。

参数向量$w$也可以是$L_{1}$范数。

$$L(w)=\frac{1}{N} \sum_{i=1}^{N}\left(f\left(x_{i} ; w\right)-y_{i}\right)^{2}+\lambda\|w\|_{1}$$

# 作业

推导下述正态分布均值的极大似然估计和贝叶斯估计

数据$x_{1}, \dots, x_{n}$来自正态分布$\mathrm{N}\left(\mu, \sigma^{2}\right)$，其中$\sigma^{2}$已知。

（1）根据样本$x_{1}, \dots, x_{n}$写出$\mu$的极大似然估计。

已经正态分布的概率密度函数$f(x)=\frac{1}{\sqrt{2\pi } \sigma } \exp \left(-\frac{(x-\mu)^{2}}{2 \sigma^{2}}\right)$

> 第一步：写出似然函数

$\begin{aligned} 
\because L\left(\mu \right) & = f\left(x_{1} \mid \mu, \sigma^{2}\right) \ast \cdots \ast f\left(x_{n} \mid \mu, \sigma^{2}\right) \\
 & = \prod_{i=1}^{n} f\left(x_{i} \mid \mu, \sigma^{2}\right) \\
 & = \prod_{i=1}^{n} \frac{1}{\sqrt{2 \pi}\sigma} \exp \left(-\frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\ 
 & = \left(\frac{1}{\sqrt{2 \pi} \sigma}\right)^{n} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\
 & = \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right)
\end{aligned}$

> 第二步：对似然函数两边取对数

$\begin{aligned} 
\therefore \ln L\left (\mu \right ) &= \ln\left [ \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right)\right ]\\
 & = \ln \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} + \ln \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\
 & = -\frac{n}{2} \ln 2 \pi \sigma^{2} - \sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}} \\
 & = -\frac{n}{2}\left(\ln {2 \pi}+\ln \sigma^{2}\right) - \frac{1}{2 \sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2} \\
 & = -\frac{n}{2} \ln {2 \pi} -\frac{n}{2}\ln {\sigma^{2}} - \frac{1}{2 \sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2}
\end{aligned}$

> 第三步：要求最大似然$\mu$，就是对$\mu$进行求导，并且使导数等于0。

$\frac{\partial \ln L\left (\mu \right )}{\partial \mu}=-\frac{1}{\sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)=0$

> 第四步：求解似然方程

$\therefore \sum_{i=1}^{n}\left(x_{i}-\mu\right)=0$

$\therefore \hat{\mu}=\frac{1}{n} \sum_{i=1}^{n} x_{i}=\bar{x}$

所以$\mu$极大似然估计为$\hat{\mu}=\bar{x}$

（2）假设$\mu$的先验分布是正态分布$\mathrm{N}\left(0, \tau^{2}\right)$，其中$\sigma^{2}$已知，根据样本$x_{1}, \dots, x_{n}$写出$\mu$的贝叶斯估计。

由于$\mu$的先验分布服从正态分布$\mathrm{N}\left(0, \tau^{2}\right)$，因此$\mu$的先验概率密度为$f(x)=\frac{1}{\sqrt{2\pi } \tau } \exp \left(-\frac{\mu^{2}}{2 \tau^{2}}\right)$

> 第一步：写出贝叶斯公式

$\begin{aligned} 
\because L\left(\mu \mid x_{1}, \dots, x_{n} \right) &= \frac{f\left ( \mu \right ) \ast f\left ( x_{1} \mid \mu \right ) \ast \dots \ast f\left ( x_{n} \mid \mu \right )}{\int f\left ( \mu, x_{1}, \dots, x_{n}\right )d\mu} \\
 &=k \ast \frac{1}{\sqrt{2\pi } \tau } \exp \left(-\frac{\mu^{2}}{2 \tau^{2}}\right) \ast \frac{1}{\sqrt{2\pi } \sigma } \exp \left(-\frac{(x-\mu)^{2}}{2 \sigma^{2}}\right) \\
 &=\frac{k}{\tau}\left( 2 \pi \right)^{-\frac{n+1}{2}} \sigma^{-n}\exp \left(-\frac{\mu^{2}}{2 \tau^{2}}\right)\exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\
\end{aligned}$

> 第二步：对贝叶斯函数两边取对数

$\begin{aligned} 
\therefore \ln L\left (\mu \mid x_{1}, \dots, x_{n} \right ) &= \ln\left [ \frac{k}{\tau}\left( 2 \pi \right)^{-\frac{n+1}{2}} \sigma^{-n} \exp \left(-\frac{\mu^{2}}{2 \tau^{2}}\right)\exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right)\right ]\\
 & = \ln \left(\frac{k}{\tau}\left( 2 \pi \right)^{-\frac{n+1}{2}} \sigma^{-n}\right) - \frac{\mu^{2}}{2 \tau^{2}} -\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}} \\
 & = \ln \left(\frac{k}{\tau}\left( 2 \pi \right)^{-\frac{n+1}{2}} \sigma^{-n}\right) - \frac{\mu^{2}}{2 \tau^{2}} - \frac{1}{2 \sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2} \\
\end{aligned}$

> 第三步：要求概率最大的$\mu$，就是对$\mu$进行求导，并且使导数等于0。

$\frac{\partial \ln L\left (\mu \mid x_{1}, \dots, x_{n}\right )}{\partial \mu}=-\frac{\mu}{\tau^{2}} + \frac{1}{\sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)=0$

> 第四步：求解贝叶斯方程

$\therefore \sum_{i=1}^{n}\left(x_{i}-\mu\right) = \frac{\mu \cdot \sigma^{2}}{\tau^{2}}$

$\therefore \sum_{i=1}^{n}x_{i} - n \cdot \mu = \frac{\mu \cdot \sigma^{2}}{\tau^{2}}$

$\therefore \sum_{i=1}^{n}x_{i} = n \cdot \mu + \frac{\mu \cdot \sigma^{2}}{\tau^{2}} = \mu \left (n + \frac{\sigma^{2}}{\tau^{2}} \right )$

$\therefore \hat{\mu}=\frac{\sum_{i=1}^{n}x_{i}}{n + \frac{\sigma^{2}}{\tau^{2}}}$

---

> 思考

1、极大似然估计是假设未知参数$\theta$是一个固定值，因此目标是找到这个$\theta$，使得数据集$maxP\left (D \mid \theta \right )$发生的概率最大。

2、贝叶斯估计则是假设未知参数$\theta$服从一定的概率分布，因此目标是是在数据集$D$发生的情况下，哪个$\theta$的概率最大$maxP\left (\theta \mid D \right )$。

3、当数据集的数量较大时，极大似然估计与贝叶斯估计相似，$\mu$的估计都为$\hat{\mu}=\bar{x}$较为可信。

4、当数据集的数量较小时，$\mu$极大似然估计$\hat{\mu}=\bar{x}$就与真实的$\hat{\mu}$不同，即不可信。因此，此时贝叶斯估计更加合适。
