---
title: 模式识别原理作业
mathjax: true
date: 2020-04-27 21:15:27
updated: {{ date }}
categories: [课程]
---

# 第二章 贝叶斯决策论

## 课后习题

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

12、设$\omega_{\max}(\mathbf{x})$为类别状态，此时对所有的$i(i=1, \cdots, c)$，有$P\left(\omega_{max} | \mathbf{x}\right) \geqslant P\left(\omega_{i} | \mathbf{x}\right)$。

（1）证明$P\left(\omega_{max} | \mathbf{x}\right) \geq \frac{1}{c}$

证明：

由于$\sum_{i=1}^{c} P\left(\omega_{i} | \mathbf{x}\right)=1$

如果对于任意的$i \neq j$，都有$P\left(\omega_{i} | \mathbf{x}\right)=P\left(\omega_{j} | \mathbf{x}\right)$，那么就可以得到

$$P\left(\omega_{i} | \mathbf{x}\right)=\frac{1}{c}$$

所以如果存在$P\left(\omega_{max} | \mathbf{x}\right) \geqslant P\left(\omega_{i} | \mathbf{x}\right)$，那么

$$P\left(\omega_{max} | \mathbf{x}\right) \geqslant \frac{1}{c}$$

（2）证明对于最小化误差判定规则，平均误差概率为$P(\text {error})=1-\int P\left(\omega_{\max } | \mathbf{x}\right) p(\mathbf{x}) d \mathbf{x}$

证明：

因为最小误差概率为1-最大正确概率，因此按照最小化误差判定规则，平均误差概率为

$$P(\text {error})=1-\int P\left(\omega_{\max } | \mathbf{x}\right) p(\mathbf{x}) d \mathbf{x}$$

（3）利用这两个结论证明$P(\text {error}) \leqslant \frac{c-1}{c}$

证明：

因为

$$P\left(\omega_{max} | \mathbf{x}\right) \geqslant \frac{1}{c}$$

所以

$$-P\left(\omega_{max} | \mathbf{x}\right) \leqslant -\frac{1}{c}$$

又因为

$$P(\text {error})=1-\int P\left(\omega_{\max } | \mathbf{x}\right) p(\mathbf{x}) d \mathbf{x}$$

所以

$$P(\text {error})=1-P\left(\omega_{\max } | \mathbf{x}\right) \int p(\mathbf{x}) d \mathbf{x} \leqslant 1 -\frac{1}{c} \int p(\mathbf{x}) d \mathbf{x}$$

又因为

$$\int p(\mathbf{x}) d \mathbf{x}=1$$

所以

$$P(\text {error})=1-P\left(\omega_{\max } | \mathbf{x}\right) \int p(\mathbf{x}) d \mathbf{x} \leqslant 1 -\frac{1}{c} \int p(\mathbf{x}) d \mathbf{x}=1-\frac{1}{c}=\frac{c-1}{c}$$

所以

$$P(\text {error}) \leqslant \frac{c-1}{c}$$

（4）描述一种情况，在此情况下$P(\text {error}) = \frac{c-1}{c}$

因为当$P\left(\omega_{i} | \mathbf{x}\right)=P\left(\omega_{j} | \mathbf{x}\right)$时

$$P\left(\omega_{i} | \mathbf{x}\right)=\frac{1}{c}$$

所以，只有在所有类别等概率的情况下

$$P(\text {error}) = \frac{c-1}{c}$$

14、考虑有拒绝决策行为的分类问题

（1）利用第13题的结论证明下面的判别函数对于此问题是最优的

$$g_{i}(\mathbf{x})=\left\{\begin{array}{ll}
p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right) & i=1, \cdots, c \\
\frac{\lambda_{s}-\lambda_{f}}{\lambda_{s}} \sum_{j=1}^{c} p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right) & i=c+1
\end{array}\right.$$

证明：

根据第13题的结论可以知道，对于任意的$j$都有$P\left(\omega_{i} | \mathbf{x}\right) \geq P\left(\omega_{j} | \mathbf{x}\right)$，并且$P\left(\omega_{i} | \mathbf{x}\right) \geq 1-\frac{\lambda_{r}}{\lambda_{s}}$

根据贝叶斯公式$p\left(\omega_{i} | \mathbf{x}\right)=\frac{p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right)}{p(\mathbf{x})}$，可以得到对于任意的$j$都有

$$p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right) \geq p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right)$$

$$p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right) \geq \left(1-\frac{\lambda_{r}}{\lambda_{s}}\right) p(\mathbf{x})$$

其中由于$p(\mathbf{x})$是常量，因此不用体现。

由于对于任意的$j$都有$P\left(\omega_{i} | \mathbf{x}\right) \geq P\left(\omega_{j} | \mathbf{x}\right)$，因此就可以到$g_{i}(\mathbf{x}) \geq g_{j}(\mathbf{x})$

所以在$i=1, \cdots, c$的决策函数为

$$g_{i}(\mathbf{x}) = p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right)$$

当$i=c+1$的决策函数为

$$g_{i}(\mathbf{x}) = \left(1-\frac{\lambda_{r}}{\lambda_{s}}\right) p(\mathbf{x})$$

因为$p(\mathbf{x}) = \sum_{j=1}^{c} p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right)$，所以

$$g_{i}(\mathbf{x}) = \left(1-\frac{\lambda_{r}}{\lambda_{s}}\right) \sum_{j=1}^{c} p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right)$$

所以

$$g_{i}(\mathbf{x})=\left\{\begin{array}{ll}
p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right) & i=1, \cdots, c \\
\frac{\lambda_{s}-\lambda_{f}}{\lambda_{s}} \sum_{j=1}^{c} p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right) & i=c+1
\end{array}\right.$$

（2）绘出此判别函数及其判决区域在具有如下特性的两类一维情况下的图形。**（未画图）**

>* $p\left(x | \omega_{1}\right) \sim N(1,1)$
>* $p\left(x | \omega_{2}\right) \sim N(-1,1)$
>* $P\left(\omega_{1}\right)=P\left(\omega_{2}\right)=\frac{1}{2}$
>* $\frac{\lambda_{r}}{\lambda_{s}}=\frac{1}{4}$

根据上述条件可以得到

$g_{1}(x)=p\left(x | \omega_{1}\right) P\left(\omega_{1}\right)=\frac{1}{2} \frac{e^{-(x-1)^{2} / 2}}{\sqrt{2 \pi}}$

$g_{2}(x)=p\left(x | \omega_{2}\right) P\left(\omega_{2}\right)=\frac{1}{2} \frac{e^{-(x+1)^{2} / 2}}{\sqrt{2 \pi}}$

$g_{3}(x) = \frac{3}{8 \sqrt{2 \pi}}\left[e^{\frac{-(x-1)^{2}}{2}}+e^{-(x+1)^{2} / 2}\right]=\frac{3}{4}\left[g_{1}(x)+g_{2}(x)\right]$

（3）定性地描述随$\frac{\lambda_{r}}{\lambda_{s}}$从0增加到1，将会怎样？**（未做）**

（4）在具有如下特定的情况下重复（3）**（未画图）**

>* $p\left(x | \omega_{1}\right) \sim N(1,1)$
>* $p\left(x | \omega_{2}\right) \sim N(0,\frac{1}{4})$
>* $P\left(\omega_{1}\right)=\frac{1}{3}$，$P\left(\omega_{2}\right)=\frac{2}{3}$
>* $\frac{\lambda_{r}}{\lambda_{s}}=\frac{1}{2}$

根据上述条件可以得到

$g_{1}(x)=p\left(x | \omega_{1}\right) P\left(\omega_{1}\right)=\frac{2}{3} \frac{e^{-(x-1)^{2} / 2}}{\sqrt{2 \pi}}$

$g_{2}(x)=p\left(x | \omega_{2}\right) P\left(\omega_{2}\right)=\frac{1}{3} \frac{2 e^{-2 x^{2}}}{\sqrt{2 \pi}}$

$g_{3}(x) = \frac{1}{3 \sqrt{2 \pi}}\left[e^{\frac{-(x-1)^{2}}{2}}+e^{-2x^{2}}\right]=\frac{1}{2}\left[g_{1}(x)+g_{2}(x)\right]$

19、从熵的定义（式（37））开始，推导最大熵分布的一般方程，假定其约束条件的一般形式如下：

$$\int b_{k}(x) p(x) d x=a_{k} . \quad k=1,2, \cdots, q$$

（1）利用拉格朗日待定因子$\lambda_{1} \cdot \lambda_{2}, \cdots , \lambda_{q}$，推导合成的函数式：

$$H_{s}=-\int p(x)\left[\ln p(x)-\sum_{k=0}^{q} \lambda_{k} b_{k}(x)\right] d x-\sum_{k=0}^{q} \lambda_{k} a_{k}$$

解释对所有$x$为什么有$a_{0}=1$及$b_{0}(x)=1$成立。

推导：

按照式（37）得到熵函数

$$H(p(x))=-\int p(x) \ln p(x) d x$$

同时对约束条件进行变化，可以得到

$$\int b_{k}(x) p(x) d x - a_{k} = 0$$

因此，引入拉格朗日因子，得到

$$\begin{aligned}
H_{s} &=-\int p(x) \ln p(x) d x+\sum_{k=1}^{q}\lambda_{k}\left[\int b_{k}(x) p(x) d x-a_{k}\right] \\
 &=-\int p(x) \ln p(x) d x + \sum_{k=1}^{q}\lambda_{k} \int b_{k}(x) p(x) d x - \sum_{k=1}^{q}\lambda_{k}a_{k} \\
 &=-\left[\int p(x) \ln p(x) d x - \sum_{k=1}^{q}\lambda_{k} \int b_{k}(x) p(x) d x\right]- \sum_{k=1}^{q}\lambda_{k}a_{k} \\
 &=-\int p(x)\left[\ln p(x)-\sum_{k=0}^{q} \lambda_{k} b_{k}(x)\right] d x-\sum_{k=0}^{q} \lambda_{k} a_{k}
\end{aligned}$$

**未解释**

（2）根据$H_{s}$对$p(x)$求导，令被积函数为0，由此证明最大熵分布遵循

$$p(x)=\exp \left[\sum_{k=0}^{q} \lambda_{k} b_{k}(x)-1\right]$$

其中$q+1$个参数由上面的约束式确定。

证明：

首先令$H_{s}$对$p(x)$求导，令被积函数为0，就可以得到

$$\frac{\partial H_{s}}{\partial p(x)}=-\int\left[\ln p(x)-\sum_{k=0}^{q} \lambda_{k} b_{k}(x)+1\right] d x=0$$

变换后可以得到

$$\ln p(x)=\sum_{k=0}^{q} \lambda_{k} b_{k}(x)-1$$

最后得到

$$p(x)=\exp \left[\sum_{k=0}^{q} \lambda_{k} b_{k}(x)-1\right]$$

23、考虑三维正态分布$p(\mathbf{x} | \omega) \sim N(\boldsymbol{\mu}, \mathbf{\Sigma})$，其中

$$\boldsymbol{\mu} = \begin{pmatrix} 1\\ 2\\ 2 \end{pmatrix} \text{及} \mathbf{\Sigma} = \begin{pmatrix} 1 & 0 & 0\\ 0 & 5 & 2\\ 0 & 2 & 5 \end{pmatrix}$$

（1）求点$\mathbf{x}_{0}=(0.5,0,1)^{t}$处的概率密度

由3维多元正态密度函数得到

$$p\left(\mathbf{x}_{o} | \omega\right)=\frac{1}{(2 \pi)^{\frac{3}{2}}|\mathbf{\Sigma}|^{\frac{1}{2}}} \exp \left[-\frac{1}{2}\left(\mathbf{x}_{o}-\boldsymbol{\mu}\right)^{t} \mathbf{\Sigma}^{-1}\left(\mathbf{x}_{o}-\boldsymbol{\mu}\right)\right]$$

根据$\mathbf{\Sigma}$值可以得到

$|\mathbf{\Sigma}|=\left|\begin{array}{ccc}1 & 0 & 0 \\ 0 & 5 & 2 \\ 0 & 2 & 5\end{array}\right|=1\left|\begin{array}{cc}5 & 2 \\ 2 & 5\end{array}\right|=21$

$\mathbf{\Sigma}^{-1}=\left(\begin{array}{ccc}1 & 0 & 0 \\ 0 & 5 & 2 \\ 0 & 2 & 5\end{array}\right)^{-1}=\left(\begin{array}{ccc}1 & 0 & 0 \\ 0 & \left(\begin{array}{cc}5 & 2 \\ 2 & 5\end{array}\right)^{-1}\end{array}\right)=\left(\begin{array}{ccc}1 & 0 & 0 \\ 0 & \frac{5}{21} & \frac{-2}{21} \\ 0 & \frac{-2}{21} & \frac{5}{21}\end{array}\right)$

代入$\mathbf{x}_{0}=(0.5,0,1)^{t}$就可以得到

$\begin{aligned}
\left(\mathbf{x}_{o}-\boldsymbol{\mu}\right)^{t} \mathbf{\Sigma}^{-1}\left(\mathbf{x}_{o}-\boldsymbol{\mu}\right) &=\left[\left(\begin{array}{c}5 \\0 \\1 \end{array}\right)-\left(\begin{array}{c} 1 \\ 2 \\ 2 \end{array}\right)\right]^{t}\left(\begin{array}{ccc} 1 & 0 & 0 \\ 0 & 5 / 21 & -2 / 21 \\ 0 & -2 / 21 & 5 / 21 \end{array}\right)^{-1}\left[\left(\begin{array}{c} 0.5 \\ 0 \\ 1 \end{array}\right)-\left(\begin{array}{c} 1 \\ 2 \\ 2 \end{array}\right)\right] \\
 &=\left[\begin{array}{c} -0.5 \\ -8 / 21 \\ -1 / 21 \end{array}\right]^{t}\left[\begin{array}{c} -0.5 \\ -2 \\ -1 \end{array}\right] \\
 &=0.25+\frac{16}{21}+\frac{1}{21} \\
 &=1.06
\end{aligned}$

因此就可以得到

$p\left(\mathbf{x}_{o} | \omega\right)=\frac{1}{(2 \pi)^{\frac{3}{2}}(21)^{\frac{1}{2}}} \exp \left[-\frac{1}{2}(1.06)\right]=8.16 \times 10^{-3}$

（2）构造白化变换$\mathbf{A}_{w}$（式（44）），计算分别表示本征向量和本征值的矩阵$\Phi$和$\mathbf{A}$；接下来，将此分布转化为以原点为中心协方差矩阵为单位阵的分布，即$p(\mathbf{x} | \omega) \sim N(\mathbf{0}, \mathbf{I})$。**（未做）**

（3）将整个同样的转化过程应用与点$\mathbf{x}_{0}$以产生一变化点$\mathbf{x}_{w}$。**（未做）**

（4）通过详细计算，证明原分布中从$\mathbf{x}_{0}$到均值$\boldsymbol{\mu}$的Mahalanobis距离与变换后的分布中从$\mathbf{x}_{w}$到0的Mahalanobis距离相等。**（未做）**

（5）概率密度在一个一般的线性变换下是否保持不变？换句话说，对于某线性变化$\mathbf{T}$，是否有$p\left(\mathbf{x}_{0} | N(\boldsymbol{\mu}, \mathbf{\Sigma})\right)=p\left(\mathbf{T}^{t} \mathbf{x}_{0} | N\left(\mathbf{T}^{t} \boldsymbol{\mu}, \mathbf{T}^{t} \mathbf{\Sigma} \mathbf{T}\right)\right)$？解释原因。**（未做）**

（6）证明当把一个一般的白化电话$\mathbf{A}_{\omega}=\mathbf{\Phi} \mathbf{\Lambda}^{\frac{1}{2}}$应用于一个高斯分布时可保证最终分布的协方差与单位阵$\mathbf{I}$成比例，检查变换后的分布是否仍然具有归一化特征。**（未做）**

31、对于两类一维问题，设$p\left(x | \omega_{i}\right) \sim N\left(\mu_{i}, \sigma^{2}\right)$，且$P\left(\omega_{1}\right)=P\left(\omega_{2}\right)=\frac{1}{2}$。

（1）证明最小误差概率为 **（未做完）**

$$P_{e}=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-u^{2}}{2}} d u$$

其中$a=\frac{\left|\mu_{2}-\mu_{1}\right|}{(2 \sigma)}$

证明：

根据式71可以知道最小误差为

$\begin{aligned} 
P(correct) &=\sum_{i=1}^{c} \int_{\mathcal{R}_{i}} p\left(\mathbf{x} | \omega_{i}\right) P\left(\omega_{i}\right) d \mathbf{x} \\
 &= \frac{1}{2}\int_{\mathcal{R}_{1}} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x} + \frac{1}{2}\int_{\mathcal{R}_{2}} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x} \\
 &= \frac{1}{2}\int_{\mathcal{R}_{1}} \frac{1}{\sqrt{2 \pi} \sigma} \exp \left(-\frac{(x-\mu_{1})^{2}}{2 \sigma^{2}}\right) d \mathbf{x} + \frac{1}{2}\int_{\mathcal{R}_{2}} \frac{1}{\sqrt{2 \pi} \sigma} \exp \left(-\frac{(x-\mu_{2})^{2}}{2 \sigma^{2}}\right) d \mathbf{x}
\end{aligned}$

（2）利用不等式 **（未做）**

$$P_{e}=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-t^{2}}{2}} d t \leqslant \frac{1}{\sqrt{2 \pi} a} e^{\frac{-a^{2}}{2}}$$

证明当$\frac{\left|\mu_{2}-\mu_{1}\right|}{\sigma}$趋于无穷时，$P_{e}$趋向于零。

43、设向量$\mathbf{x}=\left(x_{1}, \cdots, x_{d}\right)^{t}$的分量为二值的（0或1），且设$P\left(\omega_{j}\right)$的类别状态$\omega_{j}$的先验概率，其中$j=1, \cdots, c$。现定义

$$p_{i j}=\operatorname{Pr}\left[x_{i}=1 | \omega_{j}\right] \quad \begin{aligned} i &=1, \cdots, d \\ j &=1, \cdots, c \end{aligned}$$

且对于$\omega_{j}$中所有$\mathbf{x}$，其分量$x_{i}$是统计独立的。

（1）解释$p_{i j}$的含义

$p_{i j}$表示类别为$\omega_{j}$，且$x_{i}$为1情况下的概率。

（2）证明最小误差概率通过下面的判定规则获得：对于所有的$j$和$k$，如果$g_{k}(\mathbf{x}) \geqslant g_{j}(\mathbf{x})$，则判为$\omega_{k}$，其中

$$g_{j}(\mathbf{x})=\sum_{i=1}^{d} x_{i} \ln \frac{p_{i j}}{1-p_{i j}}+\sum_{i=1}^{d} \ln \left(1-p_{i j}\right)+\ln P\left(\omega_{j}\right)$$

证明：

根据贝叶斯公式

$$P\left(\omega_{j} | \mathbf{x}\right)=\frac{p\left(\mathbf{x} | \omega_{j}\right) P\left(\omega_{j}\right)}{p(\mathbf{x})}$$

变形可以得到判别函数

$$g_{j}(\mathbf{x})=\ln p\left(\mathbf{x} | \omega_{j}\right)+\ln P\left(\omega_{j}\right)$$

根据书中式（86）可以得到

$$p\left(\mathbf{x} | \omega_{j}\right)=\prod_{i=1}^{d} p_{i j}^{x_{i}}\left(1-p_{i j}\right)^{1-x_{i}}$$

把上式代入判别函数就可以得到

$$\begin{aligned}
g_{j}(\mathbf{x}) &=\sum_{i=1}^{d}\left[x_{i} \ln p_{i j}+\left(1-x_{i}\right) \ln \left(1-p_{i j}\right)\right]+\ln P\left(\omega_{j}\right) \\
&=\sum_{i=1}^{d} x_{i} \ln \frac{p_{i j}}{1-p_{i j}}+\sum_{i=1}^{d} \ln \left(1-p_{i j}\right)+\ln P\left(\omega_{j}\right)
\end{aligned}$$

48、现有二维的三类别模式，具有下列分布：

>* $p\left(\mathbf{x} | \omega_{1}\right) \sim N(\mathbf{0}, \mathbf{I})$
>* $p\left(\mathbf{x} | \omega_{2}\right) \sim N\left(\left(\begin{array}{l}1 \\ 1\end{array}\right), \mathbf{I}\right)$
>* $p\left(\mathbf{x} | \omega_{3}\right) \sim \frac{1}{2} N\left(\left(\begin{array}{c}0.5 \\ 0.5\end{array}\right), \mathbf{I}\right)+\frac{1}{2} N\left(\left(\begin{array}{c}-0.5 \\ 0.5\end{array}\right), \mathbf{I}\right)$

且$P\left(\omega_{i}\right)=\frac{1}{3}$，$i=1,2,3$。

（1）通过显式的计算后验概率，以最小误差概率对点$\mathbf{x}=\begin{pmatrix} 0.3\\ 0.3 \end{pmatrix}$进行分类。

根据书上式（38）的多维多元正态秘书函数可以得到，当$\mathbf{x}=\begin{pmatrix} 0.3\\ 0.3 \end{pmatrix}$时，$p\left(\mathbf{x} | \omega_{1}\right) P\left(\omega_{1}\right)=0.04849$，$p\left(\mathbf{x} | \omega_{2}\right) P\left(\omega_{2}\right)=0.03250$，$p\left(\mathbf{x} | \omega_{3}\right) P\left(\omega_{3}\right)=0.04437$，所以$\mathbf{x}=\begin{pmatrix} 0.3\\ 0.3 \end{pmatrix}$属于第1类。

（2）假设对于某特定的测试点，第一个特征值丢失了，即对$\mathbf{x}=\begin{pmatrix} *\\ 0.3 \end{pmatrix}$进行分类。 **（未做）**

（3）假设对于某特定的测试点，第一个特征值丢失了，即对$\mathbf{x}=\begin{pmatrix} 0.3\\ * \end{pmatrix}$进行分类。 **（未做）**

（4）对点$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$重复以上各步。**（未做完）**

当$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$时，$P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)=0.04344$，$P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)=0.03556$，$P\left(\omega_{3}\right) p\left(\mathbf{x} | \omega_{3}\right)=0.04589$，所以$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$属于第3类

## 思考题

对于有噪声的模型中，其判别函数为

$\begin{aligned} 
P\left(\omega_{i} | \mathbf{x}_{g}, \mathbf{x}_{b}\right) &= \frac{\int p\left(\omega_{i}, \mathbf{x}_{g}, \mathbf{x}_{b}, \mathbf{x}_{t}\right) d \mathbf{x}_{l}}{p\left(\mathbf{x}_{g}, \mathbf{x}_{b}\right)}\\
 &=\frac{\int P\left(\omega_{i} | \mathbf{x}_{g}, \mathbf{x}_{t}\right) p\left(\mathbf{x}_{g}, \mathbf{x}_{t}\right) p\left(\mathbf{x}_{b} | \mathbf{x}_{t}\right) d \mathbf{x}_{t}}{\int p\left(\mathbf{x}_{g}, \mathbf{x}_{\mathbf{t}}\right) p\left(\mathbf{x}_{b} | \mathbf{x}_{t}\right) d \mathbf{x}_{t}} \\ 
 &=\frac{\int g_{i}(\mathbf{x}) p(\mathbf{x}) p\left(\mathbf{x}_{b} | \mathbf{x}_{t}\right) d \mathbf{x}_{t}}{\int p(\mathbf{x}) p\left(\mathbf{x}_{b} | \mathbf{x}_{t}\right) d \mathbf{x}_{t}} 
\end{aligned}$

1、当整个图像都存在在噪声时，$\mathbf{x}_{g}$不存在，只有$\mathbf{x}_{b}$和$\mathbf{x}_{t}$，那么模型是什么样？

2、已知类别$\omega_{i}$，求$\mathbf{x}_{t}$

3、即有确实的数据，又有带噪的数据，什么场景有这么情况？怎么建立起一个概率分布？

# 第三章 贝叶斯决策论

## 课后习题

1、令$x$为服从指数概率密度函数的分布：

$$p(x | \theta)=\left\{
    \begin{array}{ll}
    \theta e^{-\theta x} & x \geqslant 0 \\
    0 & \text { 其他 }
    \end{array}\right.$$

假设$n$个样本点$x_{1}, \cdots, x_{n}$都独立地服从分布$p(x | \theta)$，证明关于$\theta$的最大似然估计结果为

$$\hat{\theta}=\frac{1}{\frac{1}{n} \sum_{k=1}^{n} x_{k}}$$

证明：

要求$p(x | \theta)$的关于$\theta$的最大似然估计，即求下式最大时的$\theta$

$$p(\mathcal{D} | \boldsymbol{\theta})=\prod_{k=1}^{n} p\left(\mathbf{x}_{k} | \theta\right)$$

因此对上式两边求对数，就可以得到

$$\begin{aligned} 
l(\theta) &= \sum_{k=1}^{n} \ln p\left(\mathbf{x}_{k} | \theta\right)\\
 &=\sum_{k=1}^{n}\left[\ln \theta-\theta \mathbf{x}_{k}\right] \\ 
 &=n \ln \theta-\theta \sum_{k=1}^{n} x_{k}
\end{aligned}$$

根据书上式子（6），可以知道要求$\theta$，那么就需要对$l(\theta)$求导，即$\nabla_{\theta} l(\theta)=0$，同时令其等于0。

$$\begin{aligned} 
\nabla_{\theta} l(\theta) &= \frac{\partial}{\partial \theta}\left[n \ln \theta-\theta \sum_{k=1}^{n} x_{k}\right]\\
 &=\frac{n}{\theta}-\sum_{k=1}^{n} x_{k} \\ 
 &=0
\end{aligned}$$

就可以得到

$$\hat{\theta}=\frac{1}{\frac{1}{n} \sum_{k=1}^{n} x_{k}}$$

3、最大似然估计也可以用于估计先验概率。假设样本是连续独立地从自然状态$\omega_{i}$中抽取的每一个自然状态的概率为$P\left(\omega_{i}\right)$。如果第$k$个样本的自然状态为$\omega_{i}$，那么就记$z_{i k}=1$，否则$z_{i k}=0$。

（1）证明

$$P\left(z_{i 1}, \ldots, z_{i n} | P\left(\omega_{i}\right)\right)=\prod_{k=1}^{n} P\left(\omega_{i}\right)^{z_{i k}}\left(1-P\left(\omega_{i}\right)\right)^{1-z_{i k}}$$

证明：

由于第$k$个样本的自然状态为$\omega_{i}$时，$z_{i k}=1$。因此就可以得到

$$P\left[z_{i k}=1 | P\left(\omega_{i}\right)\right]=P\left(\omega_{i}\right)$$

同理就可以得到

$$P\left[z_{i k}=0 | P\left(\omega_{i}\right)\right]=1-P\left(\omega_{i}\right)$$

因此，把上面两个式子进行合并得到

$$P\left[z_{i k} | P\left(\omega_{i}\right)\right]=\left[P\left(\omega_{i}\right)\right]^{z_{i k}}\left[1-P\left(\omega_{i}\right)\right]^{1-z_{i k}}$$

因此就可以得到

$$\begin{aligned} 
P\left(z_{i 1}, \ldots, z_{i n} | P\left(\omega_{i}\right)\right) &= \prod_{k=1}^{n} P\left(z_{i k} | P\left(\omega_{i}\right)\right)\\
 &=\prod_{k=1}^{n}\left[P\left(\omega_{i}\right)\right]^{z_{i k}}\left[1-P\left(\omega_{i}\right)\right]^{1-z_{i k}}
\end{aligned}$$

（2）证明对$P\left(\omega_{i}\right)$的最大似然估计为

$$\hat{P}\left(\omega_{i}\right)=\frac{1}{n} \sum_{k=1}^{n} z_{i k}$$

并且简单解释这个结果。

要求$P\left(\omega_{i}\right)$的最大似然估计，那么就需要对$P\left(z_{i 1}, \ldots, z_{i n} | P\left(\omega_{i}\right)\right)$两边求对数得到

$$\begin{aligned} 
l\left(P\left(\omega_{i}\right)\right) &= \ln P\left(z_{i 1}, \cdots, z_{i n} | P\left(\omega_{i}\right)\right)\\
 &=\ln \left[\prod_{k=1}^{n}\left[P\left(\omega_{i}\right)\right]^{z_{i k}}\left[1-P\left(\omega_{i}\right)\right]^{1-z_{i k}}\right] \\
 &=\sum_{k=1}^{n}\left[z_{i k} \ln P\left(\omega_{i}\right)+\left(1-z_{i k}\right) \ln \left(1-P\left(\omega_{i}\right)\right)\right]
\end{aligned}$$

然后令$l\left(P\left(\omega_{i}\right)\right)$对$P\left(\omega_{i}\right)$求导得到

$$\begin{aligned} 
\nabla_{P\left(\omega_{i}\right)} l\left(P\left(\omega_{i}\right)\right) &= \frac{1}{P\left(\omega_{i}\right)} \sum_{k=1}^{n} z_{i k}-\frac{1}{1-P\left(\omega_{i}\right)} \sum_{k=1}^{n}\left(1-z_{i k}\right)\\
 &=0
\end{aligned}$$

然后进行计算

$$\frac{1}{P\left(\omega_{i}\right)} \sum_{k=1}^{n} z_{i k}-\frac{1}{1-P\left(\omega_{i}\right)} \sum_{k=1}^{n}\left(1-z_{i k}\right)=0$$

通过变换就可以得到

$$\hat{P}\left(\omega_{i}\right)=\frac{1}{n} \sum_{k=1}^{n} z_{i k}$$

6、对多元高斯分布，推导用最大似然估计方法估计均值和方差时的公式（18）（19）。并且明确地给出可能需要的假设条件。

证明：

假设$d$维数据服从高斯分布，因此就可以得到密度函数为

$$p(\mathbf{x} | \boldsymbol{\mu}, \mathbf{\Sigma})=\frac{1}{(2 \pi)^{\frac{d}{2}}|\mathbf{\Sigma}|^{\frac{1}{2}}} \exp \left[-\frac{1}{2}(\mathbf{x}-\boldsymbol{\mu})^{t} \mathbf{\Sigma}^{-1}(\mathbf{x}-\boldsymbol{\mu})\right]$$

所以就可以得到$n$个$x$的密度函数为

$$p\left(\mathbf{x}_{1}, \mathbf{x}_{2}, \ldots, \mathbf{x}_{n} | \boldsymbol{\mu}, \mathbf{\Sigma}\right)=\frac{1}{(2 \pi)^{\frac{n}{2}}|\mathbf{\Sigma}|^{\frac{n}{2}}} \exp \left[-\frac{1}{2} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)^{t} \mathbf{\Sigma}^{-1}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)\right]$$

根据最大似然的求解方法，即对上式两边求对数，就可以得到

$$l(\boldsymbol{\mu}, \mathbf{\Sigma})=-\frac{n}{2} \ln (2 \pi)-\frac{n}{2} \ln |\mathbf{\Sigma}|-\frac{1}{2} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)^{t} \mathbf{\Sigma}^{-1}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)=0$$

**（上式化简不会）**

然后分别对上式中的$\boldsymbol{\mu}$和$\mathbf{\Sigma}$进行求导，并令其等于0，就可以解出

$$\hat{\boldsymbol{\mu}}=\frac{1}{n} \sum_{k=1}^{n} \mathbf{x}_{k}$$

$$\hat{\mathbf{\Sigma}}=\frac{1}{n} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)^{t}$$

13、令$p(\mathbf{x} | \mathbf{\Sigma}) \sim \mathrm{N}(\boldsymbol{\mu}, \mathbf{\Sigma})$，其中$\boldsymbol{\mu}$已知，而$\mathbf{\Sigma}$未知。证明对$\mathbf{\Sigma}$的最大似然估计为 **（未做）**

$$\hat{\mathbf{\Sigma}}=\frac{1}{n} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)^{t}$$

（1）证明$\mathbf{a}^{t} \mathbf{A} \mathbf{a}=\operatorname{tr}\left[\mathbf{A} \mathbf{a} \mathbf{a}^{t}\right]$，其中矩阵的迹$\operatorname{tr}(\mathbf{A})$是矩阵$\mathbf{A}$对角元素之和。

（2）证明似然函数可以写作

$$p\left(\mathbf{x}_{1}, \cdots, \mathbf{x}_{n} | \mathbf{\Sigma}\right)=\frac{1}{(2 \pi)^{\frac{n d}{2}}}\left|\mathbf{\Sigma}^{-1}\right|^{\frac{n}{2}} \exp \left[-\frac{1}{2} \operatorname{tr}\left[\mathbf{\Sigma}^{-1} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)^{t}\right]\right]$$

（3）如果$\mathbf{A}=\mathbf{\Sigma}^{-1} \hat{\mathbf{\Sigma}}$，并且$\mathbf{A}$的本征值为$\lambda_{1}, \lambda_{2}, \cdots, \lambda_{n}$。证明我们在上面的结果能够导出

$$p\left(\mathbf{x}_{1}, \cdots, \mathbf{x}_{n} | \mathbf{\Sigma}\right)=\frac{1}{(2 \pi)^{\frac{n d}{2}}|\hat{\mathbf{\Sigma}}|^{\frac{n}{2}}}\left(\lambda_{1} \cdots \lambda_{d}\right)^{\frac{n}{2}} \exp \left[-\frac{n}{2}\left(\lambda_{1}+\cdots+\lambda_{d}\right)\right]$$

（4）证明，当$\lambda_{1}=\lambda_{2}=\dots=\lambda_{d}=1$时，似然函数达到最大。并且解释这个结果。

35、对于样本$\mathbf{x}_{1}, \cdots, \mathbf{x}_{n}$（每个样本是$d$维的），定义样本均值和样本协方差如下：

$$\hat{\mu}_{n}=\frac{1}{n} \sum_{k=1}^{n} x_{k}$$

$$\mathbf{C}_{n}=\frac{1}{n-1} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}_{n}\right)\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}_{n}\right)^{t}$$

这些被称为非递归公式。

（1）用这些公式计算样本均值和样本协方差的计算复杂度分别是多少？

均值的计算复杂度为$O(d n)$，方差的计算复杂度为$O\left(d n^{2}\right)$

（2）证明，用递归方法求解样本均值和样本协方差的公式为

$$\hat{\boldsymbol{\mu}}_{n+1}=\hat{\boldsymbol{\mu}}_{n}+\frac{1}{n+1}\left(\mathbf{x}_{n+1}-\hat{\boldsymbol{\mu}}_{n}\right)$$

**未证明**

$$\mathbf{C}_{n+1}=\frac{n-1}{n} \mathbf{C}_{n}+\frac{1}{n+1}\left(\mathbf{x}_{n+1}-\hat{\boldsymbol{\mu}}_{n}\right)\left(\mathbf{x}_{n+1}-\hat{\boldsymbol{\mu}}_{n}\right)^{t}$$ 

证明：

因为$\hat{\mu}_{n}=\frac{1}{n} \sum_{k=1}^{n} x_{k}$，所以

$$\begin{aligned} 
\hat{\mu}_{n+1} &= \frac{1}{n+1} \sum_{k=1}^{n+1} x_{k}\\
 &=\frac{1}{n+1}\left[\sum_{k=1}^{n} \mathbf{x}_{k}+\mathbf{x}_{n+1}\right]
\end{aligned}$$

因为

$$\hat{\mu}_{n}=\frac{1}{n} \sum_{k=1}^{n} x_{k}$$

所以

$$\sum_{k=1}^{n} x_{k}=n \hat{\mu}_{n}$$

所以

$$\begin{aligned} 
\hat{\mu}_{n+1} &= \frac{1}{n+1} \sum_{k=1}^{n+1} x_{k} \\
 &=\frac{1}{n+1}\left[\sum_{k=1}^{n} \mathbf{x}_{k}+\mathbf{x}_{n+1}\right] \\
 &=\frac{1}{n+1}\left[n \hat{\mu}_{n}+\mathbf{x}_{n+1}\right] \\
 &=\frac{n}{n+1} \hat{\mu}_{n} + \frac{1}{n+1} \mathbf{x}_{n+1} \\
 &=\frac{n+1-1}{n+1} \hat{\mu}_{n} + \frac{1}{n+1} \mathbf{x}_{n+1} \\
 &=\hat{\mu}_{n} - \frac{1}{n+1}\hat{\mu}_{n} + \frac{1}{n+1} \mathbf{x}_{n+1} \\
 &=\hat{\mu}_{n}+\frac{1}{n+1}\left(\mathbf{x}_{n+1}-\hat{\mu}_{n}\right)
\end{aligned}$$


（3）用这些递归公式计算样本均值和样本方差的计算复杂度分别是多少？**未完全**

均值的复杂度为$O(d)$。

（4）在什么情况下，你会偏向于采用非递归公式；而在什么情况下，你会偏向于采用递归公式。**未完成**

38、令$p_{x}\left(\mathbf{x} | \omega_{i}\right)$，$i=1,2$为任意的概率密度函数，均值为$\mu_{i}$，协方差矩阵为$\mathbf{\Sigma}_{i}$，其中并不要求$p_{x}\left(\mathbf{x} | \omega_{i}\right)$必须为正态概率密度。令$y=\mathbf{w}^{t} \mathbf{x}$表示投影，并且设投影后的结果的概率密度函数为$p\left(y | \omega_{i}\right)$，其均值为$\mu_{i}$,方差为$\sigma_{1}^{2}$。

（1）证明准则函数

$$J_{1}(\mathbf{w})=\frac{\left(\mu_{1}-\mu_{2}\right)^{2}}{\sigma_{1}^{2}+\sigma_{2}^{2}}$$

当

$$\mathbf{w}=\left(\Sigma_{1}+\Sigma_{2}\right)^{-1}\left(\mu_{1}-\mu_{2}\right)$$

时取得最大值。

证明：

已知$y=\mathbf{w}^{t} \mathbf{x}$，并且均值为$\mu_{i}$，就可以得到

$$\mu_{i}=\frac{1}{n_{i}} \sum y=\frac{1}{n_{i}} \sum \mathbf{w}^{t} \mathbf{x}=\mathbf{w}^{t} \boldsymbol{\mu}_{i}$$

同时根据式子（99）还可以得到方差$\sigma_{1}^{2}$为

$$\sigma_{i}^{2}=\sum\left(y-\mu_{i}\right)^{2}=\mathbf{w}^{t}\left[\sum\left(\mathbf{x}-\boldsymbol{\mu}_{i}\right)\left(\mathbf{x}-\boldsymbol{\mu}_{i}\right)^{t}\right] \mathbf{w}$$

根据式子（97）可以得到类内部散布矩阵$\mathbf{S}_{i}$

$$\mathbf{S}_{i}=\sum\left(\mathbf{x}-\boldsymbol{\mu}_{i}\right)\left(\mathbf{x}-\boldsymbol{\mu}_{i}\right)^{t}$$

因为$i=1,2$，所以总类内散步矩阵$\mathbf{S}_{w}$

$$\mathbf{S}_{W}=\mathbf{S}_{1}+\mathbf{S}_{2}$$

总类间散布矩阵$\mathbf{S}_{B}$为

$$\mathbf{S}_{B}=\left(\boldsymbol{\mu}_{1}-\boldsymbol{\mu}_{2}\right)\left(\boldsymbol{\mu}_{1}-\boldsymbol{\mu}_{2}\right)^{t}$$

根据式子（103）和准则函数可以得到

$$J_{1}(\mathbf{w})=\frac{\mathbf{w}^{t} \mathbf{S}_{B} \mathbf{w}}{\mathbf{w}^{\prime} \mathbf{S}_{W} \mathbf{w}}=\frac{\left(\mu_{1}-\mu_{2}\right)^{2}}{\sigma_{1}^{2}+\sigma_{2}^{2}}$$

可以知道

$$\mathbf{w}^{t} \mathbf{S}_{B} \mathbf{w}=\left(\mu_{1}-\mu_{2}\right)^{2}$$

$$\mathbf{w}^{\prime} \mathbf{S}_{W} \mathbf{w}=\sigma_{1}^{2}+\sigma_{2}^{2}$$

从而求得了

$$\mathbf{w}=\left(\mathbf{S}_{1}+\mathbf{S}_{2}\right)^{-1}\left(\mu_{1}-\mu_{2}\right)$$

（2）如果$P\left(\omega_{i}\right)$为$\omega_{i}$的先验概率，证明

$$J_{2}(\mathbf{w})=\frac{\left(\boldsymbol{\mu}_{1}-\boldsymbol{\mu}_{2}\right)^{2}}{P\left(\omega_{1}\right) \sigma_{1}^{2}+P\left(\omega_{2}\right) \sigma_{2}^{2}}$$

当

$$\mathbf{w}=\left[P\left(\omega_{1}\right) \mathbf{\Sigma}_{1}+P\left(\omega_{2}\right) \mathbf{\Sigma}_{2}\right]^{-1}\left(\mu_{1}-\mu_{2}\right)$$

时取得最大值。

证明：

根据式子（103）和第1题，可以知道

$$\mathbf{S}_{W}=P\left(\omega_{1}\right) \mathbf{\Sigma}_{1}+P\left(\omega_{2}\right) \mathbf{\Sigma}_{2}$$

因此，根据式子（106）就可以得到

$$\mathbf{w}=\left[P\left(\omega_{1}\right) \mathbf{\Sigma}_{1}+P\left(\omega_{2}\right) \mathbf{\Sigma}_{2}\right]^{-1}\left(\mu_{1}-\mu_{2}\right)$$

（3）在（1）和（2）之间，哪个于公式（96）的关系更密切，请解释为什么。**（未完成）**

39、表达式

$$J_{1}=\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{i}-y_{j}\right)^{2}$$

是总体组内离散度的度量。

（1）证明这个离散度公式等价于

$$J_{1}=\left(m_{1}-m_{2}\right)^{2}+\frac{1}{n_{1}} s_{1}^{2}+\frac{1}{n_{2}} s_{2}^{2}$$

证明

$$\begin{aligned} 
J_{1} &= \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left[\left(y_{i}-m_{1}\right)-\left(y_{j}-m_{2}\right)+\left(m_{1}-m_{2}\right)\right]^{2} \\
 &=\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left[\left(y_{i}-m_{1}\right)^{2}+\left(y_{j}-m_{2}\right)^{2}+\left(m_{1}-m_{2}\right)^{2} +2\left(y_{i}-m_{1}\right)\left(y_{j}-m_{2}\right)+2\left(y_{i}-m_{1}\right)\left(m_{1}-m_{2}\right)+2\left(y_{j}-m_{2}\right)\left(m_{1}-m_{2}\right)\right] \\
 &=\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{i}-m_{1}\right)^{2}+\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{j}-m_{2}\right)^{2}+\left(m_{1}-m_{2}\right)^{2} + \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{i}-m_{1}\right)\left(y_{j}-m_{2}\right)+\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{i}-m_{1}\right)\left(m_{1}-m_{2}\right) + \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{j}-m_{2}\right)\left(m_{1}-m_{2}\right)
\end{aligned}$$

因为

$$s_{1}^{2}=\sum_{y_{i} \in \mathcal{Y}_{1}}\left(y_{i}-m_{1}\right)^{2}$$

$$s_{2}^{2}=\sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{j}-m_{2}\right)^{2}$$

所以

$$\begin{aligned} 
J_{1} &= \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left[\left(y_{i}-m_{1}\right)-\left(y_{j}-m_{2}\right)+\left(m_{1}-m_{2}\right)\right]^{2} \\
 &=\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left[\left(y_{i}-m_{1}\right)^{2}+\left(y_{j}-m_{2}\right)^{2}+\left(m_{1}-m_{2}\right)^{2} +2\left(y_{i}-m_{1}\right)\left(y_{j}-m_{2}\right)+2\left(y_{i}-m_{1}\right)\left(m_{1}-m_{2}\right)+2\left(y_{j}-m_{2}\right)\left(m_{1}-m_{2}\right)\right] \\
 &=\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{i}-m_{1}\right)^{2}+\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}}\left(y_{j}-m_{2}\right)^{2}+\left(m_{1}-m_{2}\right)^{2} + \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{i}-m_{1}\right)\left(y_{j}-m_{2}\right)+\frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{i}-m_{1}\right)\left(m_{1}-m_{2}\right) + \frac{1}{n_{1} n_{2}} \sum_{y_{i} \in \mathcal{Y}_{1}} \sum_{y_{j} \in \mathcal{Y}_{2}} 2\left(y_{j}-m_{2}\right)\left(m_{1}-m_{2}\right) \\ 
 &=\frac{1}{n_{1}} s_{1}^{2}+\frac{1}{n_{2}} s_{2}^{2}+\left(m_{1}-m_{2}\right)^{2}
\end{aligned}$$

（2）证明，整体离散度为

$$J_{2}=\frac{1}{n_{1}} s_{1}^{2}+\frac{1}{n_{2}} s_{2}^{2}$$

证明：

因为$P\left(\omega_{1}\right)=\frac{1}{n_{1}}$，$P\left(\omega_{2}\right)=\frac{1}{n_{2}}$，所以可以得到整体的离散度为

$$J_{2}=\frac{1}{n_{1}} s_{1}^{2}+\frac{1}{n_{2}} s_{2}^{2}$$

（3）如果$y=\mathbf{w}^{t} \mathbf{x}$，证明在约束条件$J_{2}=1$下，使得$J_{1}$最大化的$\mathbf{w}$为 **（未完成）**

$$\mathbf{w}=\lambda\left(\frac{1}{n_{1}} \mathbf{S}_{1}+\frac{1}{n_{2}} \mathbf{S}_{2}\right)^{-1}\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)$$

其中

$$\lambda=\left[\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)^{t}\left(\frac{1}{n_{1}} \mathbf{S}_{1}+\frac{1}{n_{2}} \mathbf{S}_{2}\right)\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)\right]^{\frac{1}{2}}$$

$$\mathbf{m}_{i}=\frac{1}{n_{i}} \sum_{\mathbf{x} \in \mathcal{D}_{i}} \mathbf{x}$$

和

$$\mathbf{S}_{i}=\sum_{\mathbf{x} \in \mathcal{D}_{i}} n_{i}\left(\mathbf{m}_{i}-\mathbf{m}\right)\left(\mathbf{m}_{i}-\mathbf{m}\right)^{t}$$

40、如果$\mathbf{S}_{B}$和$\mathbf{S}_{W}$为两个对称$d \times d$的实数矩阵，那么我们知道存在着$n$个本征值$\lambda_{1}, \cdots, \lambda_{n}$，满足$\left|\mathbf{S}_{B}-\lambda \mathbf{S}_{W}\right|=0$，和对应的$n$个本征向量$\mathbf{e}_{1}, \cdots, \mathbf{e}_{n}$，满足$\mathbf{S}_{\mathbf{B}} \mathbf{e}_{i}=\lambda_{i} \mathbf{S}_{W} \mathbf{e}_{i}$。而且，当$\mathbf{S}_{W}$正定时，这些本征向量就能够被归一化，因此$\mathbf{e}_{i}^{t} \mathbf{S}_{W} \mathbf{e}_{j}=\delta_{i j}$和$\mathbf{e}_{i}^{t} \mathbf{S}_{B} \mathbf{e}_{j}=\lambda_{i} \delta_{i j}$。令$\tilde{\mathbf{S}}_{W}=\mathbf{W}^{t} \mathbf{S}_{W} \mathbf{W}$和$\tilde{\mathbf{S}}_{B}=\mathbf{W}^{t} \mathbf{S}_{B} \mathbf{W}$，其中$\mathbf{W}$为一个$d \times n$的矩阵，其各列向量对应于前面所述的$n$个本征向量。**（未完成）**

（1）证明$\tilde{\mathbf{S}}_{W}$是一个$n \times n$的单位矩阵$I$，而$\tilde{\mathbf{S}}_{B}$是一个对角矩阵，其中各个对角线上的元素正好是前面所述的$n$个本征值。（这表明多重判别函数分析中的判别函数都是互不相关的）

（2）求$J=\frac{|\tilde{\mathbf{S}}_{B}|}{|\tilde{\mathbf{S}}_{W}|}$的值。

（3）令$\mathbf{y}=\mathbf{W}^{t} \mathbf{x}$。然后令$\mathbf{y}^{\prime}=\mathbf{Q D y}$，其中$\mathbf{D}$为$n \times n$的非奇异对角矩阵，表示对坐标轴的尺度变换，$\mathbf{Q}$为正交矩阵，表示对坐标轴的旋转。证明$J$对这种变换具有不变性。

41、考虑两个正态分布，它们的协方差矩阵相同，但都是任意的。证明，对于一个合适的阈值，Fisher线性分类函数可以用负的对数似然比来得到。**（未完成）**

# 第四章 非参数技术

## 课后习题

2、考虑一个正态分布$p(x) \sim N\left(\mu, \sigma^{2}\right)$和Parzen窗函数$\varphi(x) \sim N(0,1)$，证明Parzen窗估计

$$p_{n}(x)=\frac{1}{n h_{n}} \sum_{i=1}^{n} \varphi\left(\frac{x-x_{i}}{h_{n}}\right)$$

有如下的性质：

（1）$\bar{p}_{n}(x) \sim N\left(\mu, \sigma^{2}+h_{n}^{2}\right)$

（2）$\operatorname{Var}\left[p_{n}(x)\right] \approx \frac{1}{2 n h_{n} \sqrt{\pi}} p(x)$

（3）对于$h_{n}$较小时，$p(x)-\bar{p}_{n}(x) \approx \frac{1}{2}\left(\frac{h_{n}}{\sigma}\right)^{2}\left[1-\left(\frac{x-\mu}{\sigma}\right)^{2}\right] p(x)$

注意，如果$h_{n}=\frac{h_{1}}{\sqrt{n}}$，那么这个结果表示由于偏差导致的无差率为$\frac{1}{n}$的速度趋向于零，而噪声的标准差以速度$\sqrt[4]{n}$趋于零。

证明：

因为Parzen窗函数$\varphi(x) \sim N(0,1)$，并且$p(x) \sim N\left(\mu, \sigma^{2}\right)$

所以

$$\varphi(x)=\frac{1}{\sqrt{2 \pi}} e^{-\frac{x^{2}}{2}}$$

$$p(x)=\frac{1}{\sqrt{2 \pi} \sigma} \exp \left(-\frac{(x-\mu)^{2}}{2 \sigma^{2}}\right)$$

（1）

根据式子（23）得到

$$\begin{aligned} 
\bar{p}_{n}(x) &= \mathcal{E}\left[p_{n}(x)\right] \\
 &=\frac{1}{n h_{n}} \sum_{i=1}^{n} \mathcal{E}\left[\varphi\left(\frac{x-x_{i}}{h_{n}}\right)\right] \\
 &=\frac{1}{h_{n}} \int_{-\infty}^{\infty} \varphi\left(\frac{x-v}{h_{n}}\right) p(v) d v \\ 
 &=\frac{1}{h_{n}} \int_{-\infty}^{\infty} \frac{1}{\sqrt{2 \pi}} \exp \left[-\frac{1}{2}\left(\frac{x-v}{h_{n}}\right)^{2}\right] \frac{1}{\sqrt{2 \pi} \sigma} \exp \left[-\frac{1}{2}\left(\frac{v-\mu}{\sigma}\right)^{2}\right] d v \\
 &=\frac{1}{\sqrt{2 \pi} \sqrt{h_{n}^{2}+\sigma^{2}}} \exp \left[-\frac{1}{2} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right]
\end{aligned}$$

所以$\bar{p}_{n}(x) \sim N\left(\mu, \sigma^{2}+h_{n}^{2}\right)$

（2）**（未完成）**

根据式子（24）得到

$$\begin{aligned} 
\operatorname{Var}\left[p_{n}(x)\right] &= \operatorname{Var}\left[\frac{1}{n h_{n}} \sum_{i=1}^{n} \varphi\left(\frac{x-x_{i}}{h_{n}}\right)\right] \\
 &=\frac{1}{n^{2} h_{n}^{2}} \sum_{i=1}^{n} \operatorname{Var}\left[\varphi\left(\frac{x-x_{i}}{h_{n}}\right)\right] \\
 &=\frac{1}{n h_{n}^{2}} \operatorname{Var}\left[\varphi\left(\frac{x-v}{h_{n}}\right)\right] \\ 
 &=\frac{1}{n h_{n}^{2}}\left\{\mathcal{E}\left[\varphi^{2}\left(\frac{x-v}{h_{n}}\right)\right]-\left(\mathcal{E}\left[\varphi\left(\frac{x-v}{h_{n}}\right)\right]\right)^{2}\right\}
\end{aligned}$$

所以$\operatorname{Var}\left[p_{n}(x)\right] \approx \frac{1}{2 n h_{n} \sqrt{\pi}} p(x)$

（3）**（未完成）**

7、证明最近邻规则中的Voronoi网格必须是凸的。也就是说，对于同一个体积中的两个点$\mathbf{x}_{1}$，$\mathbf{x}_{2}$，位于连接着两个点的线段上的所有点也必定位于这个体积内部。

证明：

要证明Voronoi细胞是凸的，就是证明在Voronoi网格中的任意两点，连接它们的直线也都在网格中。假设$\mathbf{x}^{*}$是Voronoi网格中的一个样本点，$\mathbf{y}$为其他样本点。如果构建一个超平面，该超平面靠近$\mathbf{x}^{*}$而非$\mathbf{y}$，那么可以认为$\mathbf{x}_{1}$，$\mathbf{x}_{2}$之间的连线上的点更加接近$\mathbf{x}^{*}$。因此就可以得到连线上的点为

$$\mathbf{x}(\lambda)=(1-\lambda) \mathbf{x}_{1}+\lambda \mathbf{x}_{2}$$

其中$0 \leq \lambda \leq 1$

因为超平面定义的半空间是凸的，因此所有的$\mathbf{x}(\lambda)$都比$\mathbf{y}$更加接近$\mathbf{x}^{*}$。对于Voronoi网格中每个$\mathbf{x}_{1}$，$\mathbf{x}_{2}$有满足这样的条件。此外，对于其他的采样点$\mathbf{y}_{i}$，结果也都成立。因此，$\mathbf{x}(\lambda)$比任何其他标记点更接近$\mathbf{x}^{*}$。

根据凸的定义，就可以得到了Voronoi网格是凸的。

9、考虑下面的二维空间的3-类别问题 **（未完成）**

| $\omega_{1}$ | | $\omega_{2}$ | | $\omega_{3}$ | |
| :------: | :------: | :------: | :------: | :------: | :------: |
| $x_{1}$ | $x_{2}$ | $x_{1}$ | $x_{2}$ | $x_{1}$ | $x_{2}$ |
| 10 | 0 | 5 | 10 | 2 | 8 |
| 0 | -10 | 0 | 5 | -5 | 2 |
| 5 | -2 | 5 | 5 | 10 | -4 |

（1）画出用最近邻规则区分分类$\omega_{1}$和$\omega_{2}$的决策边界。计算样本均值$\mathbf{m}_{1}$和$\mathbf{m}_{2}$。在同一张图上，画出如果把样本归类为与之最近的样本均值的阿哥类时的判定边界。

（2）对类别$\omega_{1}$和$\omega_{3}$，重复（1）。

（3）对类别$\omega_{2}$和$\omega_{3}$，重复（1）。

（4）对类别$\omega_{1}$、$\omega_{2}$和$\omega_{3}$，重复（1）。

## 思考题

1、KNN会不会过拟合？

2、无穷多个样本能修正前面的错误估计吗？

# 第五章 线性判别函数

1、讨论线性判别函数对以下二维的单模和多模问题的应用

（1）绘制两个多模分布，要求有一个线性判别函数能给出一个很好的，或者（有可能的话）是最优的分类器。

证明：当两个分布在分类面的两边，并且对称，那么这两个分布的概率密度也沿这条垂直线具有相同的值，此时的分类面为贝叶斯决策边界，可以给出最小的分类误差。

（2）绘制两个单模分布，要求对最好的线性判别函数都只能给出很差的分类效果。

证明：当两个单模分布明显重叠时，虽然有最好的线性判别函数，但是分类效果还是很差。

（3）考虑两个圆周对称高斯分布$p\left(\mathbf{x} | \omega_{i}\right) \sim N\left(\boldsymbol{\mu}_{i}, a_{i} \mathbf{I}\right)$且$P\left(\omega_{i}\right)$，$i=1,2$。其中$\mathbf{I}$是单位矩阵且其他参数可取任意值。不作任何计算，请说明这个两类问题的最优判决边界是否是直线。如果不是的话，请给出一个最优判别函数不是一条直线的例子。

证明：假设有两个不同方差的高斯分布$\sigma_{1}^{2} \neq \sigma_{2}^{2}$，并且$\sigma_{2}>\sigma_{1}$。但是由于$p\left(\mathbf{x} | \omega_{i}\right)$符合正态分布，因此均值的未知固定，所以不能使用直线作为判别面，而是使用圆形作为判别面。

4、

## 课后习题