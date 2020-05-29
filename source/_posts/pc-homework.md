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

（2）此解是否总是唯一的？如果不是，请构造一个简单的反例。

解：

解不是总是唯一的。当$P\left(\omega_{1}\right)=P\left(\omega_{2}\right)=0.5$，并且

$$p\left(x | \omega_{1}\right)=\left\{\begin{array}{ll}
1 & -0.5 \leq x \leq 0.5 \\
0 & \text { otherwise }
\end{array}\right.$$

$$p\left(x | \omega_{2}\right)=\left\{\begin{array}{ll}
1 & 0 \leq x \leq 1 \\
0 & \text { otherwise }
\end{array}\right.$$

可以得到$\mathcal{R}_{1}$的决策区为$\mathcal{R}_{1}=[-0.5,0.25]$，可以得到$\mathcal{R}_{2}$的决策区为$\mathcal{R}_{2}=[0,0.5]$，因此解不是唯一的。

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

（2）绘出此判别函数及其判决区域在具有如下特性的两类一维情况下的图形。

>* $p\left(x | \omega_{1}\right) \sim N(1,1)$
>* $p\left(x | \omega_{2}\right) \sim N(-1,1)$
>* $P\left(\omega_{1}\right)=P\left(\omega_{2}\right)=\frac{1}{2}$
>* $\frac{\lambda_{r}}{\lambda_{s}}=\frac{1}{4}$

根据上述条件可以得到

$g_{1}(x)=p\left(x | \omega_{1}\right) P\left(\omega_{1}\right)=\frac{1}{2} \frac{e^{-(x-1)^{2} / 2}}{\sqrt{2 \pi}}$

$g_{2}(x)=p\left(x | \omega_{2}\right) P\left(\omega_{2}\right)=\frac{1}{2} \frac{e^{-(x+1)^{2} / 2}}{\sqrt{2 \pi}}$

$g_{3}(x) = \frac{3}{8 \sqrt{2 \pi}}\left[e^{\frac{-(x-1)^{2}}{2}}+e^{-(x+1)^{2} / 2}\right]=\frac{3}{4}\left[g_{1}(x)+g_{2}(x)\right]$

（3）定性地描述随$\frac{\lambda_{r}}{\lambda_{s}}$从0增加到1，将会怎样？

（4）在具有如下特定的情况下重复（3）

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

因为$\int p(x) d x=1$，所以$a_{0}=b_{0}=1$对所有$x$成立。

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

（2）构造白化变换$\mathbf{A}_{w}$（式（44）），计算分别表示本征向量和本征值的矩阵$\Phi$和$\mathbf{A}$；接下来，将此分布转化为以原点为中心协方差矩阵为单位阵的分布，即$p(\mathbf{x} | \omega) \sim N(\mathbf{0}, \mathbf{I})$。**（不会）**

（3）将整个同样的转化过程应用与点$\mathbf{x}_{0}$以产生一变化点$\mathbf{x}_{w}$。**（不会）**

（4）通过详细计算，证明原分布中从$\mathbf{x}_{0}$到均值$\boldsymbol{\mu}$的Mahalanobis距离与变换后的分布中从$\mathbf{x}_{w}$到0的Mahalanobis距离相等。**（不会）**

（5）概率密度在一个一般的线性变换下是否保持不变？换句话说，对于某线性变化$\mathbf{T}$，是否有$p\left(\mathbf{x}_{0} | N(\boldsymbol{\mu}, \mathbf{\Sigma})\right)=p\left(\mathbf{T}^{t} \mathbf{x}_{0} | N\left(\mathbf{T}^{t} \boldsymbol{\mu}, \mathbf{T}^{t} \mathbf{\Sigma} \mathbf{T}\right)\right)$？解释原因。**（不会）**

（6）证明当把一个一般的白化电话$\mathbf{A}_{\omega}=\mathbf{\Phi} \mathbf{\Lambda}^{\frac{1}{2}}$应用于一个高斯分布时可保证最终分布的协方差与单位阵$\mathbf{I}$成比例，检查变换后的分布是否仍然具有归一化特征。**（不会）**

31、对于两类一维问题，设$p\left(x | \omega_{i}\right) \sim N\left(\mu_{i}, \sigma^{2}\right)$，且$P\left(\omega_{1}\right)=P\left(\omega_{2}\right)=\frac{1}{2}$。

（1）证明最小误差概率为

$$P_{e}=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-u^{2}}{2}} d u$$

其中$a=\frac{\left|\mu_{2}-\mu_{1}\right|}{(2 \sigma)}$

证明：

假设$\left|x-\mu_{1}\right|<\left|x-\mu_{2}\right|$为$\omega_{1}$，否则就是$\omega_{2}$，假设$\mu_{1}<\mu_{2}$，那么就可以得到

$$\begin{aligned}
P(\text {error}) &=P\left(\left|x-\mu_{1}\right|>\left|x-\mu_{2}\right| | \omega_{1}\right) P\left(\omega_{1}\right)+P\left(\left|x-\mu_{2}\right|>\left|x-\mu_{1}\right| | \omega_{2}\right) P\left(\omega_{2}\right) \\
&=P_{e}=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-u^{2}}{2}} d u
\end{aligned}$$

其中$a=\frac{\left|\mu_{2}-\mu_{1}\right|}{(2 \sigma)}$

（2）利用不等式

$$P_{e}=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-t^{2}}{2}} d t \leqslant \frac{1}{\sqrt{2 \pi} a} e^{\frac{-a^{2}}{2}}$$

证明当$\frac{\left|\mu_{2}-\mu_{1}\right|}{\sigma}$趋于无穷时，$P_{e}$趋向于零。

证明：

因为

$$\lim _{a \rightarrow \infty} \frac{1}{\sqrt{2 \pi} a} e^{\frac{-a^{2}}{2}}=0$$

并且

$$\begin{aligned}
P_{e} &=\frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} e^{\frac{-u^{2}}{2}} d u \\
& \leq \frac{1}{\sqrt{2 \pi}} \int_{a}^{\infty} \frac{u}{a} e^{\frac{-u^{2}}{2}} d u \\
&=\frac{1}{\sqrt{2 \pi} a} e^{\frac{-a^{2}}{2}}
\end{aligned}$$

因此，当$a=\frac{\left|\mu_{2}-\mu_{1}\right|}{\sigma}$趋于无穷时，$P_{e}$趋向于零。

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

（2）假设对于某特定的测试点，第一个特征值丢失了，即对$\mathbf{x}=\begin{pmatrix} *\\ 0.3 \end{pmatrix}$进行分类。

根据

$$P\left(\omega_{i}\right) p\left(\left(\begin{array}{c}
* \\
0.3
\end{array}\right) | \omega_{i}\right)=P\left(\omega_{i}\right) \int_{-\infty}^{\infty} p\left(\left(\begin{array}{c}
x \\
0.3
\end{array}\right) | \omega_{i}\right) d x$$

可以得到$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{1}\right)=0.12713$，$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{2}\right)=0.10409$，$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{3}\right)=0.13035$，所以$\mathbf{x}=\begin{pmatrix} *\\ 0.3 \end{pmatrix}$为第3类。

（3）假设对于某特定的测试点，第一个特征值丢失了，即对$\mathbf{x}=\begin{pmatrix} 0.3\\ * \end{pmatrix}$进行分类。

根据

$$P\left(\omega_{i}\right) \tilde{p}\left(\left(\begin{array}{c}
0.3 \\
*
\end{array}\right) | \omega_{i}\right)=P\left(\omega_{i}\right) \int_{-\infty}^{\infty} p\left(\left(\begin{array}{l}
0.3 \\
y
\end{array}\right) | \omega_{i}\right) d y$$

可以得到$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{1}\right)=0.12713$，$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{2}\right)=0.10409$，$P\left(\omega_{1}\right) p\left((*, 0.3)^{t} | \omega_{3}\right)=0.11346$，所以$\mathbf{x}=\begin{pmatrix} 0.3\\ * \end{pmatrix}$为第1类。

（4）对点$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$重复以上各步。

当$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$时，$P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)=0.04344$，$P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)=0.03556$，$P\left(\omega_{3}\right) p\left(\mathbf{x} | \omega_{3}\right)=0.04589$，所以$\mathbf{x}=\begin{pmatrix} 0.2\\ 0.6 \end{pmatrix}$属于第3类

当$\mathbf{x}=\begin{pmatrix} *\\ 0.6 \end{pmatrix}$时，$P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)=0.11108$，$P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)=0.12276$，$P\left(\omega_{3}\right) p\left(\mathbf{x} | \omega_{3}\right)=0.13232$，所以$\mathbf{x}=\begin{pmatrix} *\\ 0.6 \end{pmatrix}$属于第3类

当$\mathbf{x}=\begin{pmatrix} 0.2\\ * \end{pmatrix}$时，$P\left(\omega_{1}\right) p\left(\mathbf{x} | \omega_{1}\right)=0.11108$，$P\left(\omega_{2}\right) p\left(\mathbf{x} | \omega_{2}\right)=0.12276$，$P\left(\omega_{3}\right) p\left(\mathbf{x} | \omega_{3}\right)=0.10247$，所以$\mathbf{x}=\begin{pmatrix} 0.2\\ * \end{pmatrix}$属于第2类

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

$$\begin{aligned} 
l(\boldsymbol{\mu}, \mathbf{\Sigma}) &= -\frac{n}{2} \ln (2 \pi)-\frac{n}{2} \ln |\mathbf{\Sigma}|-\frac{1}{2} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)^{t} \mathbf{\Sigma}^{-1}\left(\mathbf{x}_{k}-\boldsymbol{\mu}\right)\\
 &=-\frac{n}{2} \ln (2 \pi)-\frac{n}{2} \ln |\mathbf{\Sigma}|-\frac{1}{2}\left[\sum_{k=1}^{n} \mathbf{x}_{k}^{t} \mathbf{x}_{k}-2 \boldsymbol{\mu}^{t} \mathbf{\Sigma}^{-1} \mathbf{x}_{k}+n \boldsymbol{\mu}^{t} \mathbf{\Sigma}^{-1} \boldsymbol{\mu}\right]
\end{aligned}$$

然后分别对上式中的$\boldsymbol{\mu}$和$\mathbf{\Sigma}$进行求导，并令其等于0，就可以解出

$$\hat{\boldsymbol{\mu}}=\frac{1}{n} \sum_{k=1}^{n} \mathbf{x}_{k}$$

$$\hat{\mathbf{\Sigma}}=\frac{1}{n} \sum_{k=1}^{n}\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)\left(\mathbf{x}_{k}-\hat{\boldsymbol{\mu}}\right)^{t}$$

13、令$p(\mathbf{x} | \mathbf{\Sigma}) \sim \mathrm{N}(\boldsymbol{\mu}, \mathbf{\Sigma})$，其中$\boldsymbol{\mu}$已知，而$\mathbf{\Sigma}$未知。证明对$\mathbf{\Sigma}$的最大似然估计为 **（不会）**

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

$$\begin{aligned} 
\mathbf{C}_{n+1} &= \frac{1}{n} \sum_{k=1}^{n+1}\left(\mathbf{x}_{k}-\mathbf{m}_{n+1}\right)\left(\mathbf{x}_{k}-\mathbf{m}_{n+1}\right)^{t} \\
 &=\frac{1}{n}\left[\sum_{k=1}^{n}\left(\mathbf{x}_{k}-\mathbf{m}_{n+1}\right)\left(\mathbf{x}_{k}-\mathbf{m}_{n+1}\right)^{t}+\left(\mathbf{x}_{n+1}-\mathbf{m}_{n+1}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n+1}\right)^{t}\right] \\
 &=\frac{1}{n}\left[(n-1) \mathbf{C}_{n}+\frac{n}{(n+1)^{2}}\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)^{t}\right]+\frac{1}{n}\left(\left(\frac{n}{n+1}\right)^{2}\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)^{t}\right) \\
 &=\frac{n-1}{n} \mathbf{C}_{n}+\left(\frac{1}{(n+1)^{2}}+\frac{n}{(n+1)^{2}}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)\left(\mathbf{x}_{n+1}-b f m_{n}\right)^{t} \\
 &=\frac{n-1}{n} \mathbf{C}_{n}+\frac{1}{n+1}\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)^{t}
\end{aligned}$$

（3）用这些递归公式计算样本均值和样本方差的计算复杂度分别是多少？

因为

$$\hat{\mu}_{n+1}=\hat{\mu}_{n}+\frac{1}{n+1}\left(\mathbf{x}_{n+1}-\hat{\mu}_{n}\right)$$

所以均值复杂度为$O(d)$

因为

$$\mathbf{C}_{n+1}=\frac{n-1}{n} \mathbf{C}_{n}+\frac{1}{(n+1)}\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)\left(\mathbf{x}_{n+1}-\mathbf{m}_{n}\right)^{t}$$

所以均值复杂度为$O\left(d n^{2}\right)$

（4）在什么情况下，你会偏向于采用非递归公式；而在什么情况下，你会偏向于采用递归公式。

递归方法可以实现在线分类。非递归方法由于没有混入新样本的误差，所以更精确。

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

（3）在（1）和（2）之间，哪个与公式（96）的关系更密切，请解释为什么。

（1）与公式（96）关系更加密切。因为当$\tilde{m}_{i}=\mu_{i}$，并且$\tilde{s}_{i}^{2}=\sigma_{i}^{2}$，同时统计没有发生改变时，$J(\mathbf{w})$和$J_{1}(\mathbf{w})$有精确的应对关系。

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

（3）如果$y=\mathbf{w}^{t} \mathbf{x}$，证明在约束条件$J_{2}=1$下，使得$J_{1}$最大化的$\mathbf{w}$为 **（不会）**

$$\mathbf{w}=\lambda\left(\frac{1}{n_{1}} \mathbf{S}_{1}+\frac{1}{n_{2}} \mathbf{S}_{2}\right)^{-1}\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)$$

其中

$$\lambda=\left[\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)^{t}\left(\frac{1}{n_{1}} \mathbf{S}_{1}+\frac{1}{n_{2}} \mathbf{S}_{2}\right)\left(\mathbf{m}_{1}-\mathbf{m}_{2}\right)\right]^{\frac{1}{2}}$$

$$\mathbf{m}_{i}=\frac{1}{n_{i}} \sum_{\mathbf{x} \in \mathcal{D}_{i}} \mathbf{x}$$

和

$$\mathbf{S}_{i}=\sum_{\mathbf{x} \in \mathcal{D}_{i}} n_{i}\left(\mathbf{m}_{i}-\mathbf{m}\right)\left(\mathbf{m}_{i}-\mathbf{m}\right)^{t}$$

40、如果$\mathbf{S}_{B}$和$\mathbf{S}_{W}$为两个对称$d \times d$的实数矩阵，那么我们知道存在着$n$个本征值$\lambda_{1}, \cdots, \lambda_{n}$，满足$\left|\mathbf{S}_{B}-\lambda \mathbf{S}_{W}\right|=0$，和对应的$n$个本征向量$\mathbf{e}_{1}, \cdots, \mathbf{e}_{n}$，满足$\mathbf{S}_{\mathbf{B}} \mathbf{e}_{i}=\lambda_{i} \mathbf{S}_{W} \mathbf{e}_{i}$。而且，当$\mathbf{S}_{W}$正定时，这些本征向量就能够被归一化，因此$\mathbf{e}_{i}^{t} \mathbf{S}_{W} \mathbf{e}_{j}=\delta_{i j}$和$\mathbf{e}_{i}^{t} \mathbf{S}_{B} \mathbf{e}_{j}=\lambda_{i} \delta_{i j}$。令$\tilde{\mathbf{S}}_{W}=\mathbf{W}^{t} \mathbf{S}_{W} \mathbf{W}$和$\tilde{\mathbf{S}}_{B}=\mathbf{W}^{t} \mathbf{S}_{B} \mathbf{W}$，其中$\mathbf{W}$为一个$d \times n$的矩阵，其各列向量对应于前面所述的$n$个本征向量。**（不会）**

（1）证明$\tilde{\mathbf{S}}_{W}$是一个$n \times n$的单位矩阵$I$，而$\tilde{\mathbf{S}}_{B}$是一个对角矩阵，其中各个对角线上的元素正好是前面所述的$n$个本征值。（这表明多重判别函数分析中的判别函数都是互不相关的）

（2）求$J=\frac{|\tilde{\mathbf{S}}_{B}|}{|\tilde{\mathbf{S}}_{W}|}$的值。

（3）令$\mathbf{y}=\mathbf{W}^{t} \mathbf{x}$。然后令$\mathbf{y}^{\prime}=\mathbf{Q D y}$，其中$\mathbf{D}$为$n \times n$的非奇异对角矩阵，表示对坐标轴的尺度变换，$\mathbf{Q}$为正交矩阵，表示对坐标轴的旋转。证明$J$对这种变换具有不变性。

41、考虑两个正态分布，它们的协方差矩阵相同，但都是任意的。证明，对于一个合适的阈值，Fisher线性分类函数可以用负的对数似然比来得到。**（不会）**

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

（2）

根据式子（24）得到

$$\begin{aligned} 
\operatorname{Var}\left[p_{n}(x)\right] &= \operatorname{Var}\left[\frac{1}{n h_{n}} \sum_{i=1}^{n} \varphi\left(\frac{x-x_{i}}{h_{n}}\right)\right] \\
 &=\frac{1}{n^{2} h_{n}^{2}} \sum_{i=1}^{n} \operatorname{Var}\left[\varphi\left(\frac{x-x_{i}}{h_{n}}\right)\right] \\
 &=\frac{1}{n h_{n}^{2}} \operatorname{Var}\left[\varphi\left(\frac{x-v}{h_{n}}\right)\right] \\ 
 &=\frac{1}{n h_{n}^{2}}\left\{\mathcal{E}\left[\varphi^{2}\left(\frac{x-v}{h_{n}}\right)\right]-\left(\mathcal{E}\left[\varphi\left(\frac{x-v}{h_{n}}\right)\right]\right)^{2}\right\}
\end{aligned}$$

所以$\operatorname{Var}\left[p_{n}(x)\right] \approx \frac{1}{2 n h_{n} \sqrt{\pi}} p(x)$

（3）

$$\begin{aligned}
p(x)-\bar{p}_{n}(x) &=\frac{1}{\sqrt{2 \pi} \sigma} \exp \left[-\frac{1}{2} \frac{(x-\mu)^{2}}{\sigma^{2}}\right]-\frac{1}{\sqrt{2 \pi} \sqrt{h_{n}^{2}+\sigma^{2}}} \exp \left[-\frac{1}{2} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right] \\
&=\frac{1}{\sqrt{2 \pi} \sigma} \exp \left[-\frac{1}{2} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right]\left\{1-\frac{\sigma}{\sqrt{h_{n}^{2}+\sigma^{2}}} \exp \left[-\frac{1}{2} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}+\frac{1}{2} \frac{(x-\mu)^{2}}{\sigma^{2}}\right]\right\} \\
&=p(x)\left\{1-\frac{1}{\sqrt{1+\left(h_{n} / \sigma\right)^{2}}} \exp \left[-\frac{(x-\mu)^{2}}{2}\left\{\frac{1}{h_{n}^{2}+\sigma^{2}}-\frac{1}{\sigma^{2}}\right\}\right]\right\} \\
&=p(x)\left\{1-\frac{1}{\sqrt{1+\left(h_{n} / \sigma\right)^{2}}} \exp \left[\frac{1}{2} \frac{h_{n}^{2}(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right]\right\}
\end{aligned}$$

把$h_{n}$扩展到二阶得到

$$\frac{1}{\sqrt{1+\left(\frac{h_{n}}{\sigma}\right)^{2}}} \simeq 1-\frac{1}{2}\left(h_{n} / \sigma\right)^{2}$$

$$\exp \left[\frac{h_{n}^{2}}{2 \sigma^{2}} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right] \simeq 1+\frac{h_{n}^{2}}{2 \sigma^{2}} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}$$

忽略高于$h_{n}^{2}$的项得到

$$\begin{aligned}
p(x)-\bar{p}_{n}(x) & \simeq p(x)\left\{1-\left(1-\frac{1}{2}\left(\frac{h_{n}}{\sigma}\right)^{2}\right)\left(1+\frac{h_{n}^{2}}{2 \sigma^{2}} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right)\right\} \\
& \simeq p(x)\left\{1-1+\frac{1}{2} \frac{h_{n}^{2}}{\sigma^{2}}-\frac{h_{n}^{2}}{2 \sigma^{2}} \frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right\} \\
& \simeq \frac{1}{2}\left(\frac{h_{n}}{\sigma}\right)^{2}\left[1-\frac{(x-\mu)^{2}}{h_{n}^{2}+\sigma^{2}}\right] p(x) \\
& \simeq \frac{1}{2}\left(\frac{h_{n}}{\sigma}\right)^{2}\left[1-\left(\frac{x-\mu}{\sigma}\right)^{2}\right] p(x)
\end{aligned}$$

7、证明最近邻规则中的Voronoi网格必须是凸的。也就是说，对于同一个体积中的两个点$\mathbf{x}_{1}$，$\mathbf{x}_{2}$，位于连接着两个点的线段上的所有点也必定位于这个体积内部。

证明：

要证明Voronoi细胞是凸的，就是证明在Voronoi网格中的任意两点，连接它们的直线也都在网格中。假设$\mathbf{x}^{*}$是Voronoi网格中的一个样本点，$\mathbf{y}$为其他样本点。如果构建一个超平面，该超平面靠近$\mathbf{x}^{*}$而非$\mathbf{y}$，那么可以认为$\mathbf{x}_{1}$，$\mathbf{x}_{2}$之间的连线上的点更加接近$\mathbf{x}^{*}$。因此就可以得到连线上的点为

$$\mathbf{x}(\lambda)=(1-\lambda) \mathbf{x}_{1}+\lambda \mathbf{x}_{2}$$

其中$0 \leq \lambda \leq 1$

因为超平面定义的半空间是凸的，因此所有的$\mathbf{x}(\lambda)$都比$\mathbf{y}$更加接近$\mathbf{x}^{*}$。对于Voronoi网格中每个$\mathbf{x}_{1}$，$\mathbf{x}_{2}$有满足这样的条件。此外，对于其他的采样点$\mathbf{y}_{i}$，结果也都成立。因此，$\mathbf{x}(\lambda)$比任何其他标记点更接近$\mathbf{x}^{*}$。

根据凸的定义，就可以得到了Voronoi网格是凸的。

9、考虑下面的二维空间的3-类别问题 **（不会）**

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

## 课后习题

1、讨论线性判别函数对以下二维的单模和多模问题的应用

（1）绘制两个多模分布，要求有一个线性判别函数能给出一个很好的，或者（有可能的话）是最优的分类器。

证明：当两个分布在分类面的两边，并且对称，那么这两个分布的概率密度也沿这条垂直线具有相同的值，此时的分类面为贝叶斯决策边界，可以给出最小的分类误差。

（2）绘制两个单模分布，要求对最好的线性判别函数都只能给出很差的分类效果。

证明：当两个单模分布明显重叠时，虽然有最好的线性判别函数，但是分类效果还是很差。

（3）考虑两个圆周对称高斯分布$p\left(\mathbf{x} | \omega_{i}\right) \sim N\left(\boldsymbol{\mu}_{i}, a_{i} \mathbf{I}\right)$且$P\left(\omega_{i}\right)$，$i=1,2$。其中$\mathbf{I}$是单位矩阵且其他参数可取任意值。不作任何计算，请说明这个两类问题的最优判决边界是否是直线。如果不是的话，请给出一个最优判别函数不是一条直线的例子。

证明：假设有两个不同方差的高斯分布$\sigma_{1}^{2} \neq \sigma_{2}^{2}$，并且$\sigma_{2}>\sigma_{1}$。但是由于$p\left(\mathbf{x} | \omega_{i}\right)$符合正态分布，因此均值的未知固定，所以不能使用直线作为判别面，而是使用圆形作为判别面。

4、考虑判别中用的超平面。

（1）证明在从超平面$g(\mathbf{x})=\mathbf{w}^{t} \mathbf{x}+w_{0}=0$到点$\mathbf{x}_{a}$的距离为$\frac{g\left(\mathbf{x}_{a}\right)}{\| \mathbf{w} \|}$，且对应的点是约束条件$g(\mathbf{x})=0$下的满足使$\left\|\mathbf{x}-\mathbf{x}_{a}\right\|^{2}$最小的$\mathbf{x}_{a}$。

证明：

为了最小化$\left\|\mathbf{x}-\mathbf{x}_{a}\right\|^{2}$，并且约束条件为$g(\mathbf{x})=0$，因此根据拉格朗日变换可以得到

$$f(\mathbf{x}, \lambda)=\left\|\mathbf{x}-\mathbf{x}_{a}\right\|^{2}+2 \lambda[g(\mathbf{x})]$$

对上式进行展开就可以得到

$$\begin{aligned}
f(\mathbf{x}, \lambda) &=\left\|\mathbf{x}-\mathbf{x}_{a}\right\|^{2}+2 \lambda\left[\mathbf{w}^{t} \mathbf{x}+w_{0}\right] \\
&=\left(\mathbf{x}-\mathbf{x}_{a}\right)^{t}\left(\mathbf{x}-\mathbf{x}_{a}\right)+2 \lambda\left(\mathbf{w}^{t} \mathbf{x}+w_{0}\right) \\
&=\mathbf{x}^{t} \mathbf{x}-2 \mathbf{x}^{t} \mathbf{x}_{a}+\mathbf{x}_{a}^{t} \mathbf{x}_{a}+2 \lambda\left(\mathbf{x}^{t} \mathbf{w}+w_{0}\right)
\end{aligned}$$

分别对$\mathbf{x}$和$\lambda$求导就可以得到

$$\frac{\partial f(\mathbf{x}, \lambda)}{\partial \mathbf{x}}=\mathbf{x}-\mathbf{x}_{a}+\lambda \mathbf{w}=0$$

$$\frac{\partial f(\mathbf{x}, \lambda)}{\partial \lambda}=\mathbf{w}^{t} \mathbf{x}+w_{0}=0$$

就可以得到

$$\mathbf{x}=\mathbf{x}_{a}-\lambda \mathbf{w}$$

代入$\frac{\partial f(\mathbf{x}, \lambda)}{\partial \lambda}$就可以得到

$$\begin{aligned}
\mathbf{w}^{t} \mathbf{x}+w_{0} &=\mathbf{w}^{t}\left(\mathbf{x}_{a}-\lambda \mathbf{w}\right)+w_{0} \\
&=\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}-\lambda \mathbf{w}^{t} \mathbf{w} \\
&=0
\end{aligned}$$

根据上式就可以得到

$$\lambda=\frac{\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}}{\mathbf{w}^{t} \mathbf{w}}$$

因此就可以得到

$$\mathbf{x}=\mathbf{x}_{a}-\lambda \mathbf{w}=\mathbf{x}_{a}-\left[\frac{\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}}{\mathbf{w}^{t} \mathbf{w}}\right] \mathbf{w}$$

把上式代入$\left\|\mathbf{x}-\mathbf{x}_{a}\right\|$就可以得到

$$\begin{aligned}
\left\|\mathbf{x}-\mathbf{x}_{a}\right\| &=\left\|\mathbf{x}_{a}-\left[\frac{\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}}{\mathbf{w}^{t} \mathbf{w}}\right] \mathbf{w}-\mathbf{x}_{a}\right\| \\
&=\left\|\left(\frac{\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}}{\mathbf{w}^{t} \mathbf{w}}\right) \mathbf{w}\right\| \\
&=\frac{\left|g\left(\mathbf{x}_{a}\right)\right|\|\mathbf{w}\|}{\|\mathbf{w}\|^{2}} \\
&=\frac{\left|g\left(\mathbf{x}_{a}\right)\right|}{\|\mathbf{w}\|}
\end{aligned}$$

得证

（2）证明$\mathbf{x}_{a}$到超平面的投影为

$$\mathbf{x}_{p}=\mathbf{x}_{a}-\frac{g\left(\mathbf{x}_{a}\right)}{\|\mathbf{w}\|^{2}} \mathbf{w}$$

证明：

根据上题可以知道

$$\mathbf{x}=\mathbf{x}_{a}-\lambda \mathbf{w}$$

$$\lambda=\frac{\mathbf{w}^{t} \mathbf{x}_{a}+w_{0}}{\mathbf{w}^{t} \mathbf{w}}=\frac{g\left(\mathbf{x}_{a}\right)}{\|\mathbf{w}\|^{2}}$$

所以

$$\mathbf{x}=\mathbf{x}_{a}-\frac{g\left(\mathbf{x}_{a}\right)}{\|\mathbf{w}\|^{2}} \mathbf{w}$$

得证

12、考虑二次判别函数（式（4））**（不会）**

$$g(\mathbf{x})=w_{0}+\sum_{i=1}^{d} w_{i} x_{i}+\sum_{i=1}^{d} \sum_{j=1}^{d} w_{i j} x_{i} x_{j}$$

并定义对称的非奇异矩阵$\mathbf{W}=\left[w_{ij}\right]$。说明判决边界的基本特性可用尺度矩阵$\overline{\mathbf{W}}=\frac{\mathbf{W}}{\mathbf{w}^{\prime} \mathbf{W}^{-1} \mathbf{w}-4 w_{0}}$描述如下：

（1）如果$\overline{\mathbf{W}}$正比于单位矩阵$\mathbf{I}$，那么判决边界为超平面。

（2）如果$\overline{\mathbf{W}}$是正定的，判决边界是超椭圆体。

（3）如果$\overline{\mathbf{W}}$的本正值有正有负，判决边界是超双曲面。

（4）设$\mathbf{w}=\begin{bmatrix} 5\\ 2\\ -2 \end{bmatrix}$和$\mathbf{W}=\begin{bmatrix} 1 & 2 & 0\\ 2 & 5 & 1\\ 0 & 1 & -3 \end{bmatrix}$，它的解有什么特性？

（5）设$\mathbf{w}=\begin{bmatrix} 2\\ -1\\ 3 \end{bmatrix}$和$\mathbf{W}=\begin{bmatrix} 1 & 2 & 3\\ 2 & 0 & 4\\ 3 & 4 & -5 \end{bmatrix}$，它的解有什么特性？

14、考虑平方误差和准则函数（式（43））

$$J_{s}(\mathbf{a})=\sum_{i=1}^{n}\left(\mathbf{a}^{t} \mathbf{y}_{i}-b_{i}\right)^{2}$$

令$b_{i}=b$，取如下6个训练点

$$\begin{array}{ccc}
\omega_{1}:(1,5)^{t} & (2,9)^{t} & (-5,-3)^{t} \\
\omega_{2}:(2,-3)^{t} & (-1,-4)^{t} & (0,2)^{t}
\end{array}$$

解：

根据式子（10）可以知道，把样本进行扩展得到

$$\mathbf{Y}=\left(\begin{array}{rrr}
1 & 1 & 5 \\
1 & 2 & 9 \\
1 & -5 & -3 \\
-1 & -2 & 3 \\
-1 & 1 & 4 \\
-1 & 0 & -2
\end{array}\right) \mathbf{a}=\left(\begin{array}{l}
a_{1} \\
a_{2} \\
a_{3}
\end{array}\right) \mathbf{b}=\left(\begin{array}{l}
b \\
b \\
b \\
b \\
b \\
b
\end{array}\right)$$

根据平方和误差准则得到

$$\mathbf{J}_{s}(\mathbf{a})=\frac{1}{2} \sum_{i=1}^{n}\left(\mathbf{a}^{t} \mathbf{y}_{i}-b\right)^{2}=\frac{(\mathbf{Y} \mathbf{a}-\mathbf{b})^{t}(\mathbf{Y} \mathbf{a}-\mathbf{b})}{2}$$

（1）计算它的赫森矩阵。

对$\mathbf{J}_{s}(\mathbf{a})$进行求导可以得到

$$\boldsymbol{\nabla} J_{s}(\mathbf{a})=\boldsymbol{Y}^{t}(\mathbf{Y} \mathbf{a}-\mathbf{b})=\left(\begin{array}{ccccccc}
6 a_{1} & - & a_{2} & + & 6 a_{3} & + & 0 \\
-a_{1} & + & 35 a_{2} & + & 36 a_{3} & + & 3 b \\
6 a_{1} & + & 36 a_{2} & + & 144 a_{3} & - & 16 b
\end{array}\right)$$

因此$\mathbf{H}$矩阵为

$$\mathbf{H}=\mathbf{Y}^{t} \mathbf{Y}=\left(\begin{array}{ccc}
6 & -1 & 6 \\
-1 & 35 & 36 \\
6 & 36 & 144
\end{array}\right)$$

（2）假定二次准则函数，计算最优学习率。

根据式子（14）可以知道学习率$\eta$为

$$\eta=\frac{\left[\boldsymbol{\nabla} J_{s}(\mathbf{a})\right]^{t} \boldsymbol{\nabla} J_{s}(\mathbf{a})}{\left[\boldsymbol{\nabla} J_{s}(\mathbf{a})\right]^{t} \mathbf{H} \nabla J_{s}(\mathbf{a})}$$

22、推广5.8.3节的结论，证明使得准则函数

$$J_{s}^{\prime}(\mathbf{a})=\sum_{\mathbf{y} \in \mathcal{Y}_{1}}\left(\mathbf{a}^{\prime} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}+\sum_{\mathbf{y} \in \mathcal{Y}_{2}}\left(\mathbf{a}^{\prime} \mathbf{y}-\left(\lambda_{12}-\lambda_{22}\right)\right)^{2}$$

极小化的向量$\mathbf{a}$同时使得$J_{s}^{\prime}(\mathbf{a})$渐近地以最小均方差逼近贝叶斯判断函数

$$\left(\lambda_{21}-\lambda_{11}\right) P\left(\omega_{1} | \mathbf{x}\right)-\left(\lambda_{12}-\lambda_{22}\right) P\left(\omega_{2} | \mathbf{x}\right)$$

证明：

由于判别函数为

$$g_{0}(\mathbf{x})=\left(\lambda_{21}-\lambda_{11}\right) P\left(\omega_{1} | \mathbf{x}\right)-\left(\lambda_{12}-\lambda_{22}\right) P\left(\omega_{2} | \mathbf{x}\right)$$

以及准则函数为

$$J_{s}^{\prime}(\mathbf{a})=\sum_{\mathbf{y} \in \mathcal{Y}_{1}}\left(\mathbf{a}^{\prime} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}+\sum_{\mathbf{y} \in \mathcal{Y}_{2}}\left(\mathbf{a}^{\prime} \mathbf{y}-\left(\lambda_{12}-\lambda_{22}\right)\right)^{2}$$

根据式子（57）得到逼近误差为

$$\varepsilon^{2}=\int\left[\mathbf{a}^{t} \mathbf{y}-g_{o}(\mathbf{x})\right]^{2} p(\mathbf{x}) d \mathbf{x}$$

再根据式子（58）可以得到

$$\begin{aligned}
J_{s}^{\prime}(\mathbf{a}) &=\sum_{\mathbf{y} \in \mathcal{Y}_{1}}\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}+\sum_{\mathbf{y} \in \mathcal{Y}_{2}}\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{12}-\lambda_{22}\right)\right)^{2} \\
&=n\left[\frac{n_{1}}{n} \frac{1}{n} \sum_{\mathbf{y} \in \mathcal{Y}_{1}}\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}+\frac{n_{2}}{n} \frac{1}{n} \sum_{\mathbf{y} \in \mathcal{Y}_{2}}\left(\mathbf{a}^{t} \mathbf{y}+\left(\lambda_{12}-\lambda_{22}\right)\right)^{2}\right]
\end{aligned}$$

根据大数定理和式子（59），当$n \rightarrow \infty$时，概率为1

$$\begin{aligned}
\lim _{n \rightarrow \infty} \frac{1}{n} J_{s}^{\prime}(\mathbf{a}) &=J^{\prime}(\mathbf{a}) \\
&=P\left(\omega_{1}\right) \mathcal{E}_{1}\left[\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}\right]+P\left(\omega_{2}\right) \mathcal{E}_{2}\left[\left(\mathbf{a}^{t} \mathbf{y}+\left(\lambda_{12}-\lambda_{22}\right)\right)^{2}\right]
\end{aligned}$$

其中

$$\mathcal{E}_{1}\left[\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}\right]=\int\left(\mathbf{a}^{t} \mathbf{y}-\left(\lambda_{21}-\lambda_{11}\right)\right)^{2} p\left(\mathbf{x} | \omega_{1}\right) d \mathbf{x}$$

$$\mathcal{E}_{2}\left[\left(\mathbf{a}^{t} \mathbf{y}+\left(\lambda_{21}-\lambda_{11}\right)\right)^{2}\right]=\int\left(\mathbf{a}^{t} \mathbf{y}+\left(\lambda_{12}-\lambda_{22}\right)\right)^{2} p\left(\mathbf{x} | \omega_{2}\right) d \mathbf{x}$$

代入准则方程，就可以得到

$$J^{\prime}(\mathbf{a})=\int\left(\mathbf{a}^{t} \mathbf{y}\right)^{2} p(\mathbf{x}) d \mathbf{x}+2 \int \mathbf{a}^{t} \mathbf{y} g_{0}(\mathbf{x}) p(\mathbf{x}) d \mathbf{x}+\left(\lambda_{21}-\lambda_{11}\right)^{2} P\left(\omega_{1}\right)+\left(\lambda_{12}-\lambda_{22}\right)^{2} P\left(\omega_{2}\right)$$

改写判别函数为

$$\left(\lambda_{21}-\lambda_{11}\right) p\left(\mathbf{x}, \omega_{1}\right)+\left(\lambda_{12}-\lambda_{22}\right) p\left(\mathbf{x}, \omega_{2}\right)=\left(\lambda_{21}-\lambda_{11}\right) p\left(\mathbf{x} | \omega_{1}\right)+\left(\lambda_{12}-\lambda_{22}\right) p\left(\mathbf{x} | \omega_{2}\right)=g_{0}(\mathbf{x}) p(\mathbf{x})$$

就有

$$J^{\prime}(\mathbf{a})=\int\left[\mathbf{a}^{t} \mathbf{y}-g_{o}(\mathbf{x})\right]^{2} p(\mathbf{x}) d \mathbf{x}+\int\left[\left(\lambda_{21}-\lambda_{11}\right)^{2} P\left(\omega_{1}\right)+\left(\lambda_{12}-\lambda_{22}\right)^{2} P\left(\omega_{2}\right)-g_{o}^{2}(\mathbf{x})\right] p(\mathbf{x}) d \mathbf{x}$$

因为上式中的第二项与$\mathbf{a}$无关，所以只要$\mathbf{a}$最小化，那么整个式子就最小化。

32、考虑支持向量机和分属两类的训练样本：**（不会）**

$$\begin{array}{ccc}
\omega_{1}:(1,1)^{t} & (2,2)^{t} & (2,0)^{t} \\
\omega_{2}:(0,0)^{t} & (1,0)^{t} & (0,1)^{t}
\end{array}$$

（1）在图中作出这6个训练点，构造具有最优超平面和最优间隔的权向量。

{% asset_img svm.png 超平面 %}

（2）哪些是支持向量？

支持向量为$(1,1)^{t}$、$(2,0)^{t}$、$(1,0)^{t}$、$(0,1)^{t}$

（3）通过寻找拉格朗日待定乘数$\alpha_{i}$来构造在对偶空间的解，并将它与（1）中的结果比较。

# 第八章 非度量方法

## 课后习题

5、考虑利用熵步纯度训练一颗二叉树，可以参考（1）和（5）。

（1）经过单次是/否的查询判断，所引起的步纯度下降总是比1比特小。

（2）对例1中的两棵树，验证以下每次分支都引起不纯度下降，但是下降差总是小于1比特。虽然这样，解释一下为什么某个结点的不纯度却可能比后续节点还小。

（3）将（1）的结果推广到任意分支率的情况。

解

根据式子（1）知道不纯度表示为

$$i(N)=-\sum P\left(\omega_{i}\right) \log _{2}\left(P\left(\omega_{i}\right)\right)=H\left(\omega_{i}\right)$$

（1）假设由一个二元特征$F \in\{R, L\}$，这两个节点的不纯度为

$$\begin{aligned}
P(L) i(L)+P(R) i(R) &=-P(L) \sum_{i} P\left(\omega_{i} | L\right) \log _{2}\left(P\left(\omega_{i} | L\right)\right)-P(R) \sum_{i} P\left(\omega_{i} | R\right) \log _{2}\left(P\left(\omega_{i} | R\right)\right) \\
&=-\sum_{i} P\left(\omega_{i}, L\right) \log _{2}\left(\frac{P\left(\omega_{i}, L\right)}{P(L)}\right)-\sum_{i} P\left(\omega_{i}, R\right) \log _{2}\left(\frac{P\left(\omega_{i}, R\right)}{P(R)}\right) \\
&=-\sum_{i, F} P\left(\omega_{i}, F\right) \log _{2}\left(P\left(\omega_{i}, F\right)\right)+P(L) \log _{2}(P(L))+P(R) \log _{2}(P(R)) \\
&=H(\omega, F)-H(F)
\end{aligned}$$

因此分裂后的纯度为

$$\Delta i(N)=H(\omega)-H(\omega, F)+H(F)$$

因为$H(\omega) \leq H(\omega, F) \leq H(\omega)+H(F)$，所以

$$0 \leq \Delta i(N) \leq H(F) \leq 1 \text { bit }$$

（2）在每个节点上，子节点处的加权不纯度将小于父节点处的加权不纯度，即使单个子节点的不纯度可能大于其父节点。例如，在例1的上树中的（$x_{2}<0.61$）节点处，不纯度为0.65，而在左子节点处杂质为0.0，在右子节点处杂质为1.0。左枝取$\frac{2}{3}$次，则子代的加权不纯度为$\frac{2}{3} \times 0+\frac{1}{3} \times 1=0.33$。同样，在子树的（$x_{1}<0.61$）节点，右子节点的不纯度（0.92）高于父节点（0.76），但子节点的加权平均值为$\frac{2}{3} \times 0+\frac{1}{3} \times 0.92=0.304$。在每种情况下，杂质的减少量在0到1位之间，视需要而定。

（3）对于B分支，有$0 \leq \Delta i(N) \leq \log _{2}(B)\text { bit }$。

8、考虑一个2-类分类问题，采用如下的训练模式：

| $\omega_{1}$ | $\omega_{2}$ |
| :--: | :--: |
| 0110 | 1011 |
| 1010 | 0000 |
| 0011 | 0100 |
| 1111 | 1110 |

（1）用熵不纯度（式（1））手工生成一颗未剪枝的分类树。

（2）利用简单的逻辑表达式简化规则对上面得到的类别进行简化，以得到最简单的逻辑表达式（使用最少的AND和OR）

解：

此2-类分类问题一共有4个属性，分别是$\left\{a_{1}, a_{2}, a_{3}, a_{4}\right\}$

（1）根据不纯度计算可以得生成树

{% asset_img 2-tree.png 生成的树 %}

（2）$\omega_{1}=a_{3}$ AND (NOT $a_{1}$ OR $a_{1}$ AND (NOT $a_{2}$ AND NOT $a_{4}$) OR ($a_{2}$ AND $a_{4}$))

$\omega_{2}$=NOT $a_{3}$ OR ($a_{3}$ AND $a_{1}$) AND ((NOT $a_{2}$ AND $a_{4}$) OR ($a_{2}$ AND NOT $a_{4}$))

# 第九章 独立算法的机器学习

## 课后习题

23、请证明式（24）中的“留一法”均值估计$\mu(\cdot)$等价于式（22）的样本均值$\hat{\mu}$。

证明：

根据式子（25）

$$\begin{aligned}
\mu_{(\cdot)} &=\frac{1}{n} \sum_{i=1}^{n} \mu_{(i)} \\
&=\frac{1}{n} \sum_{i=1}^{n}\left[\frac{1}{n-1} \sum_{j \neq i} x_{j}\right] \\
&=\frac{1}{n(n-1)} \sum_{i=1}^{n}\left[\sum_{j=1}^{n} x_{j}-x_{i}\right] \\
&=\frac{1}{n(n-1)} \sum_{i=1}^{n}\left[n \hat{\mu}-x_{i}\right] \\
&=\frac{n}{n-1} \hat{\mu}-\frac{1}{n(n-1)} \sum_{i=1}^{n} x_{i} \\
&=\frac{n}{n-1} \hat{\mu}-\frac{1}{n-1} \hat{\mu} \\
&=\hat{\mu}
\end{aligned}$$

40、如果$n^{\prime}$个独立随机选取的测试集中有$k$个错分类的模型，那么如式（38）所示，$k$具有二项式分布 **（不会）**

$$P(k)=\left(\begin{array}{l} n^{\prime} \\ k \end{array}\right) p^{k}(1-p)^{n^{\prime}-k}$$

请证明$p$的最大似然估计$\hat{p}=\frac{k}{n^{\prime}}$，如式（39）所示。

# 第十章 无监督学习和聚类

## 课后习题

3、假设一个一维的混合正态模型由两个正态分量组成，每个分量都以原点为中心：

$$p(x | \boldsymbol{\theta})=P\left(\omega_{1}\right) \frac{1}{\sqrt{2 \pi} \sigma_{1}} e^{\frac{-x^{2}}{\left(2 \sigma_{1}^{2}\right)}}+\left(1-P\left(\omega_{1}\right)\right) \frac{1}{\sqrt{2 \pi} \sigma_{2}} e^{\frac{-x^{2}}{\left(2 \sigma_{2}^{2}\right)}}$$

而参数向量为$\boldsymbol{\theta}=\left(P\left(\omega_{1}\right), \sigma_{1}, \sigma_{2}\right)^{t}$。

（1）证明在这些条件下，这个混合密度是完全不可辨识的。

（2）假设$P\left(\omega_{1}\right)$的值是确定而未知的，混合模型是可辨识的吗？

（3）假设$\sigma_{1}$和$\sigma_{2}$是已知的，但$P\left(\omega_{1}\right)$是未知的。模型是可辨识的吗？也就是说，$P\left(\omega_{1}\right)$可由样本数据求出吗？

证明：

（1）当$\sigma_{1}=\sigma_{2}$，$P\left(\omega_{1}\right)$的取值范围为[0,1]，保持相同的混合密度。因此，混合密度无法确定。

（2）如果$P\left(\omega_{1}\right)$是固定的且已知的，并且不等于0、0.5或1，那么模型是可辨识的。对于$P\left(\omega_{1}\right)$的这三个值，不能恢复第一个分布的参数。如果$P\left(\omega_{1}\right)=1$，不能恢复第二个分布的参数。如果$P\left(\omega_{1}\right)=0.5$，那么两个分布的参数可以互换。

（3）如果$\sigma_{1}=\sigma_{2}$，因为$P\left(\omega_{1}\right)$和$P\left(\omega_{2}\right)$可以互换，所以$P\left(\omega_{1}\right)$不能够被识别。如果$\sigma_{1} \neq \sigma_{2}$，那么$P\left(\omega_{1}\right)$可以确定。

4、令$\mathbf{x}$表示含有$d$个分量的向量，每个分量在0或1间取值，让$P(\mathbf{x} | \theta)$表示$c$个多变量伯努利分布组成的混合分布，

$$P(\mathbf{x} | \boldsymbol{\theta})=\sum_{i=1}^{c} P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right) P\left(\omega_{i}\right)$$

其中

$$P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right)=\prod_{j=1}^{d} \theta_{i j}^{x_{j}}\left(1-\theta_{i j}\right)^{1-x_{j}}$$

（1）请推出下面的偏导数公式

$$\frac{\partial \ln P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right)}{\partial \theta_{i j}}=\frac{\mathbf{x}_{i}-\theta_{i j}}{\theta_{i j}\left(1-\theta_{i j}\right)}$$

（2）用最大似然估计的一般公式证明参数向量$\boldsymbol{\theta}_{i}$的最大似然估计$\hat{\boldsymbol{\theta}_{i}}$，必须满足

$$\hat{\boldsymbol{\theta}}_{i}=\frac{\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right) \mathbf{x}_{k}}{\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right)}$$

（3）解释你在（2）中得到的答案。

解：

（1）首先对$P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right)$取对数

$$\ln P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right)=\sum_{i=1}^{d}\left[x_{i j} \ln \theta_{i j}+\left(1-x_{i j}\right) \ln \left(1-\theta_{i j}\right)\right]$$

然后对$\theta_{i j}$求导得到

$$\begin{aligned}
\frac{\partial \ln P\left(\mathbf{x} | \omega_{i}, \boldsymbol{\theta}_{i}\right)}{\partial \theta_{i j}} &=\frac{x_{i j}}{\theta_{i j}}-\frac{1-x_{i j}}{1-\theta_{i j}} \\
&=\frac{x_{i j}\left(1-\theta_{i j}\right)-\theta_{i j}\left(1-x_{i j}\right)}{\theta_{i j}\left(1-\theta_{i j}\right)} \\
&=\frac{x_{i j}-x_{i j} \theta_{i j}-\theta_{i j}+\theta_{i j} x_{i j}}{\theta_{i j}\left(1-\theta_{i j}\right)} \\
&=\frac{x_{i j}-\theta_{i j}}{\theta_{i j}\left(1-\theta_{i j}\right)}
\end{aligned}$$

（2）根据式子（7）可以知道

$$\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right) \nabla_{\boldsymbol{\theta}_{i}} \ln P\left(x_{k} | \omega_{i}, \hat{\boldsymbol{\theta}}_{i}\right)=0$$

更具（1）中的结论可以得到

$$\nabla_{\boldsymbol{\theta}_{i}} \ln P\left(x_{k} | \omega_{i}, \hat{\boldsymbol{\theta}}_{i}\right)=\frac{x_{k} \hat{\boldsymbol{\theta}}_{i}}{\hat{\boldsymbol{\theta}}_{i}\left(1-\hat{\boldsymbol{\theta}}_{i}\right)}$$

因此就可以得到

$$\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right) \frac{\mathbf{x}_{k}-\hat{\boldsymbol{\theta}}_{i}}{\hat{\boldsymbol{\theta}}_{i}\left(1-\hat{\boldsymbol{\theta}}_{i}\right)}=0$$

假设$\hat{\boldsymbol{\theta}}_{i} \in(0,1)$，因此就有

$$\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right)\left(\mathbf{x}_{k}-\hat{\boldsymbol{\theta}}_{i}\right)=\mathbf{0}$$

所以就得到

$$\hat{\boldsymbol{\theta}}_{i}=\frac{\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right) \mathbf{x}_{k}}{\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right)}$$

（3）$\boldsymbol{\theta}_{i}$的最大似然估计$\hat{\boldsymbol{\theta}}_{i}$是$\mathbf{x}_{k}$的加权平均，其中权重是混合权重$\hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}_{i}\right)$的后验概率。

6、考虑一个含有$c$个成分的混合概率，参数向量$\theta$和先验概率$P\left(\omega_{i}\right)$都是未知的。令$\hat{P}\left(\omega_{i}\right)$表示$P\left(\omega_{i}\right)$的最大似然估计，$\hat{\boldsymbol{\theta}}_{i}$表示$\boldsymbol{\theta}_{i}$的最大似然估计。证明如果似然函数是可微的而且对于任何$i$，$\hat{P}\left(\omega_{i}\right) \neq 0$，则$\hat{P}\left(\omega_{i}\right)$和$\hat{\boldsymbol{\theta}}_{i}$必须满足式（11）和（12），即有 **（不会）**

$$\hat{P}\left(\omega_{i}\right)=\frac{1}{n} \sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}\right)$$

和

$$\sum_{k=1}^{n} \hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}\right) \nabla_{\boldsymbol{\theta}_{i}} \ln p\left(\mathbf{x}_{k} | \omega_{i}, \hat{\boldsymbol{\theta}}_{i}\right)=0$$

其中

$$\hat{P}\left(\omega_{i} | \mathbf{x}_{k}, \hat{\boldsymbol{\theta}}\right)=\frac{p\left(\mathbf{x}_{k} | \omega_{i}, \hat{\boldsymbol{\theta}}_{i}\right) \hat{P}\left(\omega_{i}\right)}{\sum_{i=1}^{c} p\left(\mathbf{x}_{k} | \omega_{j}, \hat{\boldsymbol{\theta}}_{j}\right) \hat{P}\left(\omega_{j}\right)}$$

32、定义相似性度量为$s\left(\mathbf{x}, \mathbf{x}^{\prime}\right)=\frac{\mathbf{x}^{t} \mathbf{x}^{\prime}}{\left(\|\mathbf{x}\|\left\|\mathbf{x}^{\prime}\right\|\right)}$

（1）如果$\mathbf{x}$的每个分量都是二值的（取-1或1），$x_{i}=1$表示$\mathbf{x}$拥有第$i$项属性，$x_{i}=-1$则表示$\mathbf{x}$不具有这个属性，请解释相似性度量的意义。

（2）证明在这种情况下，欧几里得距离的平方满足

$$\left\|\mathbf{x}-\mathbf{x}^{\prime}\right\|^{2}=2 d\left(1-s\left(\mathbf{x}, \mathbf{x}^{\prime}\right)\right)$$

证明：

根据式子（50）得到相似性度量为

（1）因为$\mathbf{x}$和$\mathbf{x}^{\prime}$是$d$维的向量，并且当$x_{i}=1$表示$\mathbf{x}$拥有第$i$项属性，$x_{i}=-1$则表示$\mathbf{x}$不具有这个属性。因此向量的欧式长度维

$$\|\mathbf{x}\|=\left\|\mathbf{x}^{\prime}\right\|=\sqrt{\sum_{i=1}^{d} x_{i}^{2}}=\sqrt{\sum_{i=1}^{d} 1}=\sqrt{d}$$

所以相似性度量可以写为

$$\begin{aligned}
s\left(\mathbf{x}, \mathbf{x}^{\prime}\right) &=\frac{\mathbf{x}^{t} \mathbf{x}^{\prime}}{\sqrt{d} \sqrt{d}} \\
&=\frac{1}{d} \sum_{i=1}^{d} x_{i} x_{i}^{\prime} \\
&=\frac{1}{d} [\text{一般特征数}-\text{非一般特征数}] \\
&=\frac{1}{d} [\text{一般特征数}-(d-\text{非一般特征数)}] \\
&=\frac{2}{d} (\text{一般特征数}) - 1
\end{aligned}$$

（2）根据（1）结果，使$\|\mathbf{x}\|=\left\|\mathbf{x}^{\prime}\right\|=\sqrt{d}$

$$\begin{aligned}
\left\|\mathbf{x}-\mathbf{x}^{\prime}\right\|^{2} &=\left(\mathbf{x}-\mathbf{x}^{\prime}\right)^{t}\left(\mathbf{x}-\mathbf{x}^{\prime}\right) \\
&=\mathbf{x}^{t} \mathbf{x}+\mathbf{x}^{\prime t} \mathbf{x}^{\prime}-2 \mathbf{x}^{t} \mathbf{x}^{\prime} \\
&=\|\mathbf{x}\|^{2}+\left\|\mathbf{x}^{\prime}\right\|^{2}-2 s\left(\mathbf{x}, \mathbf{x}^{\prime}\right)\|\mathbf{x}\|\left\|\mathbf{x}^{\prime}\right\| \\
&=d+d-2 s\left(\mathbf{x}, \mathbf{x}^{\prime}\right) \sqrt{d} \sqrt{d} \\
&=2 d\left[1-s\left(\mathbf{x}, \mathbf{x}^{\prime}\right)\right]
\end{aligned}$$