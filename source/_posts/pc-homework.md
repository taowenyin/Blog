---
title: 模式识别原理作业
mathjax: true
date: 2020-04-27 21:15:27
updated: {{ date }}
categories: [课程]
---

# 2.3节

3、考虑0-1损失函数的极小化极大化准则，即$\lambda_{11}=\lambda_{22}=0$且$\lambda_{12}=\lambda_{21}=1$。

（1）证明在这种情况下判决区域将满足

$$\int_{\mathcal{R}_{2}} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}=\int_{\mathcal{R}_{1}} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x}$$

证：

已知先验概率$P\left(\omega_{1}\right)$和$P\left(\omega_{2}\right)=1-P\left(\omega_{1}\right)$，并根据式子22，并代入$\lambda_{11}=\lambda_{22}=0$和$\lambda_{12}=\lambda_{21}=1$可以得到

$\begin{aligned} 
R=& \int_{\mathcal{R}_{1}}\left[\lambda_{11} P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)+\lambda_{12} P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)\right] d \mathbf{x} +\int_{\mathcal{R}_{2}}\left[\lambda_{21} P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)+\lambda_{22} P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)\right] d \mathbf{x} \\
=& \int_{\mathcal{R}_{1}}\left[P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)\right] d \mathbf{x} +\int_{\mathcal{R}_{2}}\left[P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)\right] d \mathbf{x} \\
=& P\left(\omega_{2}\right)\int_{\mathcal{R}_{1}}p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x} +P\left(\omega_{1}\right)\int_{\mathcal{R}_{2}}p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}
\end{aligned}$

代入$P\left(\omega_{1}\right)$和$P\left(\omega_{2}\right)=1-P\left(\omega_{1}\right)$可以得到

$R\left(P\left(\omega_{1}\right)\right)=P\left(\omega_{1}\right) \int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}+\left(1-P\left(\omega_{1}\right)\right) \int_{\mathcal{R}_{1}} p\left(x | \omega_{2}\right) d \mathbf{x}$

要求极小化极大化，那么就需要对$R\left(P\left(\omega_{1}\right)\right)$求导，即$\frac{d R\left(P\left(\omega_{1}\right)\right)}{d P\left(\omega_{1}\right)}$

就可以得到

$\frac{d R\left(P\left(\omega_{1}\right)\right)}{d P\left(\omega_{1}\right)}=\int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}-\int_{\mathcal{R}_{1}} p\left(x | \omega_{2}\right) d \mathbf{x}=0$

所以就可以得到

$\int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d x=\int_{\mathcal{R}_{1}} p\left(x | \omega_{2}\right) d x$

（2）此解是否总是唯一的？如果不是，请构造一个简单的反例。**（未做）**

6、考虑两个单变量正态分布$p\left(x | \omega_{i}\right) \sim N\left(\mu_{i}, \sigma_{i}^{2}\right)$，且$P\left(\omega_{t}\right)=\frac{1}{2}(i=1,2)$的Neyman-Pearson准则，在0-1损失下，且为了方便设$\mu_{2}>\mu_{1}$。

（1）假设当一样本实际属于$\omega_{1}$，却被认为是$\omega_{2}$时的最大可接受的误差率为$E_{1}$，用以上给定的变量确定单点判决边界。

根据书中22式可以知道

$R\left(P\left(\omega_{1}\right)\right)=P\left(\omega_{1}\right) \int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}+\left(1-P\left(\omega_{1}\right)\right) \int_{\mathcal{R}_{1}} p\left(x | \omega_{2}\right) d \mathbf{x}$

那么$\omega_{1}$被认为是$\omega_{2}$的风险为

$R\left(P\left(\omega_{1}\right)\right)=P\left(\omega_{1}\right) \int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}$

代入$P\left(\omega_{t}\right)=\frac{1}{2}(i=1,2)$，以及$p\left(x | \omega_{i}\right) \sim N\left(\mu_{i}, \sigma_{i}^{2}\right)$可以得到

$R\left(P\left(\omega_{1}\right)\right)=P\left(\omega_{1}\right) \int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}=\frac{1}{2} \int_{\mathcal{R}_{2}} N\left(\mu_{1}, \sigma_{1}^{2}\right)$

假设$x^{*}$是决策边界，以及最大可接受的误差率为$E_{1}$，就可以得到

$R\left(P\left(\omega_{1}\right)\right)=P\left(\omega_{1}\right) \int_{\mathcal{R}_{2}} p\left(x | \omega_{1}\right) d \mathbf{x}=\frac{1}{2} \int_{\mathcal{R}_{2}} N\left(\mu_{1}, \sigma_{1}^{2}\right)=\frac{1}{2} \int_{x^{*}}^{\infty} N\left(\mu_{1}, \sigma_{1}^{2}\right) d \mathbf{x} \leq E_{1}$

（2）对于此边界，将$\omega_{2}$错分为$\omega_{1}$的误差率是多少？

$R\left(P\left(\omega_{2}\right)\right)=P\left(\omega_{2}\right) \int_{\mathcal{R}_{1}} p\left(x | \omega_{2}\right) d \mathbf{x}=\frac{1}{2} \int_{\mathcal{R}_{1}} N\left(\mu_{2}, \sigma_{2}^{2}\right)=\frac{1}{2} \int_{-\infty}^{x^{*}} N\left(\mu_{2}, \sigma_{2}^{2}\right) d \mathbf{x} \leq E_{2}$

（3）在0-1损失率下的总误差率是多少？

因为总误差$E=E_{1}+E_{2}$

所以

$\begin{aligned}
E &=E_{1}+E_{2} \\ 
 &=\frac{1}{2} \int_{x^{*}}^{\infty} N\left(\mu_{1}, \sigma_{1}^{2}\right) d \mathbf{x}+\frac{1}{2} \int_{-\infty}^{x^{*}} N\left(\mu_{2}, \sigma_{2}^{2}\right) d \mathbf{x} 
\end{aligned}$

（4）将你的结论用于特殊情况：$p\left(x | \omega_{1}\right) \sim N(-1,1)$及$p\left(x | \omega_{2}\right) \sim N(1,1)$，且$E_{1}=0.05$。

因为$E_{1}=0.05$

所以$\frac{1}{2} \int_{x^{*}}^{\infty} N\left(\mu_{1}, \sigma_{1}^{2}\right) d \mathbf{x}=0.05$

把正态分布转化为标准正态分布，可以得到

$\begin{aligned}
\frac{1}{2} \int_{x^{*}}^{\infty} N\left(\mu_{1}, \sigma_{1}^{2}\right) d \mathbf{x} &=\frac{1}{2} P(x > x^{*}) \\ 
 &=\frac{1}{2} [1 - P(x \leq x^{*})] \\ 
 &=\frac{1}{2} [1 - \Phi \left ( \frac{x^{*}-\mu_{1}}{\sigma_{1}}\right )] \\ 
 &=0.05
\end{aligned}$

代入$\mu_{1}=-1$和$\sigma_{1}=1$，就可以得到

$\frac{1}{2} [1 - \Phi \left ( \frac{x^{*}-\mu_{1}}{\sigma_{1}}\right )]=\frac{1}{2} [1 - \Phi \left ( x^{*}+1\right )]=0.05$

所以$\Phi \left ( x^{*}+1\right )=0.9$，就可以通过查表求得$x^{*}+1=1.28$，所以$x^{*}=0.28$，代入$\frac{1}{2} \int_{-\infty}^{x^{*}} N\left(\mu_{2}, \sigma_{2}^{2}\right) d \mathbf{x}$，就可以求得$E_{2}$，再根据$E=E_{1}+E_{2}$，就可以求得$E$。

$$\int_{\mathcal{R}_{2}} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}=\int_{\mathcal{R}_{1}} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x}$$

所以$E=0.4321$

（5）将你的结论与贝叶斯误差率（即没有Neyman-Pearson条件）作比较。

因为在0-1损失下

$$\int_{\mathcal{R}_{2}} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}=\int_{\mathcal{R}_{1}} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x}$$

同时，由于在本题中的贝叶斯误差$x^{*}=0$，所以

$\begin{aligned}
E_{B} &=2 \int_{0}^{\infty} \frac{1}{2} N\left(\mu_{1}, \sigma_{1}^{2}\right) d \mathbf{x} \\ 
 &=2 \int_{0}^{\infty} \frac{1}{2} N\left(1, 1\right) d \mathbf{x} \\
 &=\int_{0}^{\infty} N\left(1, 1\right) d \mathbf{x}
\end{aligned}$