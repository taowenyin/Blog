---
title: 模式识别原理作业
mathjax: true
date: 2020-04-27 21:15:27
updated: {{ date }}
categories: [课程]
---

# 2.3节

3、考虑0-1损失函数的及消化极大化准则，即$\lambda_{11}=\lambda_{22}=0$且$\lambda_{12}=\lambda_{21}=1$。

（1）证明在这种情况下判决区域将满足

$$\int_{\mathcal{R}_{2}} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}=\int_{\mathcal{R}_{1}} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x}$$

证：

已知先验概率$P\left(\omega_{1}\right)$和$P\left(\omega_{2}\right)=1-P\left(\omega_{1}\right)$，并根据式子22，并代入$\lambda_{11}=\lambda_{22}=0$和$\lambda_{12}=\lambda_{21}=1$可以得到

$\begin{aligned}
R=& \int_{R_{1}}\left[\lambda_{11} P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)+\lambda_{12} P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)\right] d \mathbf{x}+\int_{\mathcal{R}_{2}}\left[\lambda_{21} P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)+\lambda_{22} P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)\right] d \mathbf{x}
\end{aligned}$

（2）此解是否总是唯一的？如果不是，请构造一个简单的反例。