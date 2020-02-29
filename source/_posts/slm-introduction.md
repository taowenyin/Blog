---
title: S1-统计学习及监督学习概论
mathjax: true
date: 2020-02-29 09:47:22
updated: {{ date }}
top: true
tags:
categories: [统计学习方法]
---

推导下述正态分布均值的极大似然估计和贝叶斯估计

数据$x_{1}, \dots, x_{n}$来自正态分布$\mathrm{N}\left(\mu, \sigma^{2}\right)$，其中$\sigma^{2}$已知。

（1）根据样本$x_{1}, \dots, x_{n}$写出$\mu$的极大似然估计。

已经正态分布的概率密度函数$f(x)=\frac{1}{\sqrt{2\pi } \sigma } \exp \left(-\frac{(x-\mu)^{2}}{2 \sigma^{2}}\right)$

$\begin{aligned} 
\because L\left(\mu, \sigma^{2}\right) & = p\left(\mathbf{x_{1}}=x_{1} \mid \mu, \sigma^{2}\right) \ast \cdots \ast p\left(\mathbf{x_{n}}=x_{n} \mid \mu, \sigma^{2}\right) \\
 & = \prod_{i=1}^{n} p\left(\mathbf{x_{i}}=x_{i} \mid \mu, \sigma^{2}\right) \\
 & = \prod_{i=1}^{n} \frac{1}{\sqrt{2 \pi}\sigma} \exp \left(-\frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\ 
 & = \left(\frac{1}{\sqrt{2 \pi} \sigma}\right)^{n} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\
 & = \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right)
\end{aligned}$

两边取对数

$\begin{aligned} 
\therefore \ln L\left (\mu, \sigma^{2} \right ) &= \ln\left [ \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right)\right ]\\
 & = \ln \left(2 \pi \sigma^{2}\right)^{-\frac{n}{2}} + \ln \exp \left(-\sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}}\right) \\
 & = -\frac{n}{2} \ln 2 \pi \sigma^{2} - \sum_{i=1}^{n} \frac{\left(x_{i}-\mu\right)^{2}}{2 \sigma^{2}} \\
 & = -\frac{n}{2}\left(\ln {2 \pi}+\ln \sigma^{2}\right) - \frac{1}{2 \sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2} \\
 & = -\frac{n}{2} \ln {2 \pi} -\frac{n}{2}\ln {\sigma^{2}} - \frac{1}{2 \sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2}
\end{aligned}$

要求最大似然$\mu$，就是对$\ln L\left (\mu, \sigma^{2} \right )$进行求导，并且使导数等于0。

$\frac{\partial \ln L\left (\mu, \sigma^{2} \right )}{\partial \mu}=-\frac{1}{\sigma^{2}} \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2}=0$

$\therefore \sum_{i=1}^{n}\left(x_{i}-\mu\right)^{2}=0$

$\therefore \mu=\frac{1}{n} \sum_{i=1}^{n} x_{i}=\bar{x}$

所以$\mu$极大似然估计为$\hat{\mu}=\bar{x}$