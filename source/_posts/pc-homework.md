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