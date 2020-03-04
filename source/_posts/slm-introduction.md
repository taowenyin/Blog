---
title: S1-统计学习及监督学习概论
mathjax: true
date: 2020-02-29 09:47:22
updated: {{ date }}
top: true
tags:
categories: [统计学习方法]
---

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

3、当数据集的数量较大时，$\mu$极大似然估计为$\hat{\mu}=\bar{x}$较为可信。

4、当数据集的数量较小时，$\mu$极大似然估计$\hat{\mu}=\bar{x}$就与真实的$\hat{\mu}$不同，即不可信。因此，此时贝叶斯估计更加合适。
