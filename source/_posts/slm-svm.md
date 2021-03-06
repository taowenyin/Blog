---
title: S7-支持向量机
mathjax: true
date: 2020-04-02 19:09:54
updated: {{ date }}
tags:
categories: [统计学习方法]
---

支持向量机学习方法包含构建由简至繁的模型，分别是线性可分支持向量机、线性支持向量机，以及非线性支持向量机。简单模型是复杂模型的基础，也是复杂模型的特殊情况。

1. 当训练数据线性可分时，通过硬间隔最大化，学习一个线性的分类器，即线性可分支持向量机，又称为硬间隔支持向量机。
2. 当训练数据近似线性可分时，通过软间隔最大化，学习一个线性的分类器，即线性支持向量机，又称为软间隔支持向量机。
3. 当训练数据线性不可分时，通过使用核技巧和软间隔最大化，学习一个非线性支持向量机。

核函数（kernel function）表示将输入从输入空间映射到特征空间得到的特征向量得到的特征向量之间的内积。**通过使用核函数可以学习非线性支持向量机，等价于隐式第在高维的特征空间中学习线性支持向量机。核方法是比支持向量机更为一般的机器学习方法。**

# 线性可分支持向量机与硬间隔最大化

## 线性可分支持向量机

假设给定一个特征空间上的训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

其中，$x_{i} \in \mathcal{X}=\mathbf{R}^{n}$，$y_{i} \in \mathcal{Y}=\{+1,-1\}$，$i=1,2, \cdots, N$。$x_{i}$为第$i$个特征向量，$y_{i}$为$x_{i}$的类标记。当$y_{i}=+1$时，$x_{i}$为正例，当$y_{i}=-1$时，$x_{i}$为负例。

**学习的目标是在特征空间找到一个分离超平面，能将实例分到不同的类。** 分离超平面对应于方程$w \cdot x+b=0$，其中$w$是法向量，$b$是截距，可用$(w, b)$表示。分离超平面将特征空间分为两部分，法向量指向的一则为正类，另一侧为负类。

## 函数间隔和几何间隔

### 函数间隔

一个点距离超平面的远近可以表示为分类预测的置信度。在超平面$w \cdot x+b=0$确定的情况下，$|w \cdot x+b|$表示点$x$距离超平面的远近，而$w \cdot x+b=0$的符号与$y$的符号是否一致表示分类是否正确，**可以用$y(w \cdot x+b)$来表示分类的正确性和置信度，即该函数就是函数间隔。**

对于给定训练数据集$T$和超平面$(w, b)$，那么超平面关于样本点$\left(x_{i}, y_{i}\right)$的函数间隔为

$$\hat{\gamma}_{i}=y_{i}\left(w \cdot x_{i}+b\right)$$

因此超平面关于训练数据$T$的函数间隔为超平面关于$T$中所有样本$\left(x_{i}, y_{i}\right)$的函数间隔的最小值

$$\hat{\gamma}=\min _{i=1, \cdots, N} \hat{\gamma}_{i}$$

### 几何间隔

由于函数间隔会因为$w$和$b$的等比例缩放发生改变，**因此就需要对法向量$w$进行约束，如$\|w\|=1$，从而使得间隔固定，该间隔就是几何间隔。**

对于给定训练数据集$T$和超平面$(w, b)$，那么超平面关于样本点$\left(x_{i}, y_{i}\right)$的几何间隔为

$$\gamma_{i}=y_{i}\left(\frac{w}{\|w\|} \cdot x_{i}+\frac{b}{\|w\|}\right)$$

因此超平面关于训练数据$T$的几何间隔为超平面关于$T$中所有样本$\left(x_{i}, y_{i}\right)$的几何间隔的最小值

$$\gamma=\min _{i=1, \cdots, N} \gamma_{i}$$

因此，可以得到函数间隔$\hat{\gamma}$和几何间隔$\gamma$关系为：

$$\gamma=\frac{\hat{\gamma}}{\|w\|}$$

如果$\|w\|=1$，那么函数间隔和几何间隔相同，如果$w$和$b$的等比例缩放，那么函数间隔也会按比例改变，但是几何间隔不变。

## 间隔最大化

### 最大间隔分离超平面

最大间隔分离超平面可以表示为下面的约束问题：

$$\begin{aligned} 
\underset{w,b}{max} & \quad \gamma \\
\text { s.t. } & \quad y_{i}\left(\frac{w}{\|w\|} \cdot x_{i}+\frac{b}{\|w\|}\right) \geqslant \gamma, i=1,2, \cdots, N
\end{aligned}$$

其中约束条件表示超平面$(w, b)$关于每个训练样本点的几何间隔至少为$\gamma$。如果改写为函数间隔，即

$$\begin{aligned} 
\underset{w,b}{max} & \quad \frac{\hat{\gamma}}{\|w\|} \\
\text { s.t. } & \quad y_{i}\left(w \cdot x_{i}+b\right) \geqslant \hat{\gamma}, i=1,2, \cdots, N
\end{aligned}$$

当$\hat{\gamma}=1$，则优化问题就变为

$$\begin{aligned} 
\underset{w,b}{max} & \quad \frac{1}{\|w\|} \\
\text { s.t. } & \quad y_{i}\left(w \cdot x_{i}+b\right) - 1 \geqslant 0, i=1,2, \cdots, N
\end{aligned}$$

由因为最大化$\frac{1}{\|w\|}$就等价于最小化$\frac{1}{2}\|w\|^{2}$，因此，最大化优化问题就变成了最小化优化问题。

$$\begin{aligned} 
\underset{w,b}{min} & \quad \frac{1}{2}\|w\|^{2} \\
\text { s.t. } & \quad y_{i}\left(w \cdot x_{i}+b\right) - 1 \geqslant 0, i=1,2, \cdots, N
\end{aligned}$$

上面的优化问题就是一个凸二次规划问题。求出约束最优化问题的解$w^{*}$和$b^{*}$就可以得到最大间隔超平面$w^{*} \cdot x+b^{*}=0$和分类决策函数$f(x)=\operatorname{sign}\left(w^{*} \cdot x+b^{*}\right)$。

**定理：若训练数据集$T$线性可分，则可将训练数据集中的样本点完全正确分开的最大间隔分离超平面存在且唯一。**

### 支持向量和间隔边界

**训练数据集的样本点中与分离超平面距离最近的样本点的实例称为支持向量。** 支持向量的约束条件为

$$y_{i}\left(w \cdot x_{i}+b\right)-1=0$$

当$y_{i}=+1$的正实例点，则支持向量的超平面为

$$H_{1}: w \cdot x+b=1$$

当$y_{i}=-1$的正实例点，则支持向量的超平面为

$$H_{2}: w \cdot x+b=-1$$

$H_{1}$和$H_{2}$平行，并没有实例点落在他们之间。$H_{1}$和$H_{2}$之间称为间隔。由于间隔至于法向量$w$相关，因此当$\hat{\gamma}=1$时，间隔为$\frac{2}{\|w\|}$。

## 学习的对偶算法

**采用对偶算法的优点：对偶问题往往更容易求解，同时对偶算法自然引入核函数，可以推广到非线性分类问题。**

构建拉格朗日函数。对不等式约束引入拉格朗日乘子$\alpha_{i} \geqslant 0$，$i=1,2, \cdots, N$，则根据约束

$$\begin{aligned} 
\underset{w,b}{min} & \quad \frac{1}{2}\|w\|^{2} \\
\text { s.t. } & \quad 1 - y_{i}\left(w \cdot x_{i}+b\right) \leqslant 0, i=1,2, \cdots, N
\end{aligned}$$

可以得到

$$\begin{aligned}
L(w, b, \alpha) &=\frac{1}{2}\|w\|^{2} + \sum_{i=1}^{N} \left [ \alpha_{i} - \alpha_{i} y_{i}\left(w \cdot x_{i}+b\right) \right ] \\
 &=\frac{1}{2}\|w\|^{2} - \sum_{i=1}^{N} \left [ \alpha_{i} y_{i}\left(w \cdot x_{i}+b\right) - \alpha_{i} \right ] \\
 &=\frac{1}{2}\|w\|^{2}-\sum_{i=1}^{N} \alpha_{i} y_{i}\left(w \cdot x_{i}+b\right)+\sum_{i=1}^{N} \alpha_{i}
\end{aligned}$$

其中，$\alpha=\left(\alpha_{1}, \alpha_{2}, \cdots, \alpha_{N}\right)^{\mathrm{T}}$为拉格朗日乘子向量。根据拉格朗日对偶性，原始问题的对偶问题就是极大极小问题：

$$\max _{\alpha} \min _{w, b} L(w, b, \alpha)$$

因此是对$L(w, b, \alpha)$求$w$、$b$的极小值，再对$\alpha$求极大值。

> 第一步：求$\underset{w, b}{min}L(w, b, \alpha)$

拉格朗日函数$L(w, b, \alpha)$分别对$w$和$b$求偏导，并令其等于0。

$$\nabla_{w} L(w, b, \alpha)=w-\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}=0$$

$$\nabla_{b} L(w, b, \alpha)=-\sum_{i=1}^{N} \alpha_{i} y_{i}=0$$

得

$$w=\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}$$

$$\sum_{i=1}^{N} \alpha_{i} y_{i}=0$$

因此代入拉格朗日函数得到

$$\begin{aligned}
L(w, b, \alpha) &=\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)-\sum_{i=1}^{N} \alpha_{i} y_{i}\left(\left(\sum_{j=1}^{N} \alpha_{j} y_{j} x_{j}\right) \cdot x_{i}+b\right)+\sum_{i=1}^{N} \alpha_{i} \\
&=-\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i}
\end{aligned}$$

所以

$$\min _{w, b} L(w, b, \alpha)=-\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i}$$

> 第二步：求$\underset{w, b}{min}L(w, b, \alpha)$对$\alpha$的极大值，即是对偶问题

$$\begin{aligned} 
\underset{\alpha}{max} & \quad -\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad \alpha_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

对上式由求极大值转化为求极小值，可以得到

$$\begin{aligned} 
\underset{\alpha}{min} & \quad \frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)-\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad \alpha_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

此时就把原始问题转化为了求极小值的对偶问题，得到$\alpha^{*}=\left(\alpha_{1}^{*}, \alpha_{2}^{*}, \cdots, \alpha_{l}^{*}\right)^{\mathrm{T}}$的最优解。从而解出$w^{*}$和$b^{*}$：

$$w^{*}=\sum_{i=1}^{N} \alpha_{i}^{*} y_{i} x_{i}$$

$$b^{*}=y_{j}-\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x_{i} \cdot x_{j}\right)$$

因此分离超平面可以写为

$$\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x \cdot x_{i}\right)+b^{*}=0$$

因此分类决策函数就可以写成

$$f(x)=\operatorname{sign}\left(\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x \cdot x_{i}\right)+b^{*}\right)$$

该式称为线性可分支持向量机的对偶形式。

# 线性支持向量机与软间隔最大化

## 线性支持向量机

线性支持向量机主要用来解决线性不可分问题，即在训练数据集中有一些特异点，将这些特异点去除后，剩下的样本点组成的数据集线性可分。为了解决这个问题，对每个样本点$\left(x_{i}, y_{i}\right)$引入一个松弛变量$\xi_{i} \geqslant 0$，使得函数间隔加上松弛变量大于等于1。此时，约束条件变为

$$y_{i}\left(w \cdot x_{i}+b\right) \geqslant 1-\xi_{i}$$

由于引入了松弛变量，因此目标函数就需要支付一个代价$\xi_{i}$，因此原先的目标函数$\frac{1}{2}\|w\|^{2}$就变为

$$\frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i}$$

其中，$C>0$称为惩罚参数，$C$值大时对误分类的惩罚增大，$C$值小时对误分类的惩罚减小。该目标函数包含两层含义：使$\frac{1}{2}\|w\|^{2}$尽量小，即间隔尽可能大。同时误分类点的个数尽可能小，$C$是调和二者的系数。

相对于硬间隔最大化，该目标函数为软间隔最大化。同时，线性支持向量机的学习问题就变成下面的凸二次规划问题（原始问题）：

$$\begin{aligned} 
\underset{w,b,\xi}{min} & \quad \frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i} \\
\text { s.t. } & \quad y_{i}\left(w \cdot x_{i}+b\right) \geqslant 1-\xi_{i}, i=1,2, \cdots, N \\
& \quad \xi_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

## 学习的对偶算法

构建拉格朗日函数。对不等式约束引入拉格朗日乘子$\alpha_{i} \geqslant 0$，$\mu_{i} \geqslant 0$，$i=1,2, \cdots, N$，则根据约束

$$\begin{aligned} 
\underset{w,b,\xi}{min} & \quad \frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i} \\
\text { s.t. } & \quad 1-\xi_{i} - y_{i}\left(w \cdot x_{i}+b\right) \leqslant 0, i=1,2, \cdots, N \\
& \quad -\xi_{i} \leqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

可以得到

$$\begin{aligned}
L(w, b, \xi, \alpha, \mu) &=\frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i} + \sum_{i=1}^{N} \left [ \alpha_{i}-\alpha_{i}\xi_{i} - \alpha_{i}y_{i}\left(w \cdot x_{i}+b\right) \right ]-\sum_{i=1}^{N} \mu_{i} \xi_{i} \\
 &=\frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i} - \sum_{i=1}^{N} \alpha_{i} \left [ y_{i}\left(w \cdot x_{i}+b\right) - 1 +\xi_{i} \right ]-\sum_{i=1}^{N} \mu_{i} \xi_{i}
\end{aligned}$$

根据拉格朗日对偶性，原始问题的对偶问题就是极大极小问题：

$$\max _{\alpha} \min _{w, b} L(w, b, \xi, \alpha, \mu)$$

因此是对$L(w, b, \xi, \alpha, \mu)$求$w$、$b$、$\xi$的极小值。

> 第一步：求$\underset{w, b}{min}L(w, b, \xi, \alpha, \mu)$

拉格朗日函数$L(w, b, \xi, \alpha, \mu)$分别对$w$、$b$、$\xi$求偏导，并令其等于0。

$$\nabla_{w} L(w, b, \xi, \alpha, \mu)=w-\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}=0$$

$$\nabla_{b} L(w, b, \xi, \alpha, \mu)=-\sum_{i=1}^{N} \alpha_{i} y_{i}=0$$

$$\nabla_{\xi_{i}} L(w, b, \xi, \alpha, \mu)=C-\alpha_{i}-\mu_{i}=0$$

得

$$w=\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}$$

$$\sum_{i=1}^{N} \alpha_{i} y_{i}=0$$

$$C-\alpha_{i}-\mu_{i}=0$$

因此代入拉格朗日函数得到

$$\min _{w, b, \xi} L(w, b, \xi, \alpha, \mu)=-\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i}$$

> 第二步：求$\underset{w, b, \xi}{min} L(w, b, \xi, \alpha, \mu)$对$\alpha$的极大值，即是对偶问题

$$\begin{aligned} 
\underset{\alpha}{max} & \quad -\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad C-\alpha_{i}-\mu_{i}=0 \\
 & \quad \alpha_{i} \geqslant 0, \quad i=1,2, \cdots, N \\
 & \quad \mu_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

将约束进行变换，消去$\mu_{i}$，从而只留下$\alpha_{i}$，因此对偶问题就写为

$$\begin{aligned} 
\underset{\alpha}{max} & \quad -\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)+\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad 0 \leqslant \alpha_{i} \leqslant C
\end{aligned}$$

此时就把原始问题转化为了求极小值的对偶问题，得到$\alpha^{*}=\left(\alpha_{1}^{*}, \alpha_{2}^{*}, \cdots, \alpha_{l}^{*}\right)^{\mathrm{T}}$的最优解。从而解出$w^{*}$和$b^{*}$：

$$w^{*}=\sum_{i=1}^{N} \alpha_{i}^{*} y_{i} x_{i}$$

$$b^{*}=y_{j}-\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x_{i} \cdot x_{j}\right)$$

因此分离超平面可以写为

$$\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x \cdot x_{i}\right)+b^{*}=0$$

因此分类决策函数就可以写成

$$f(x)=\operatorname{sign}\left(\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x \cdot x_{i}\right)+b^{*}\right)$$

该式称为线性可分支持向量机的对偶形式。

## 支持向量（没懂）

| $\alpha_{i}^{*}$ | $\xi_{i}$ | $x_{i}$ |
| :---: | :---: | :---: |
| $\alpha_{i}^{*}<C$ | $\xi_{i}=0$ | $x_{i}$落在间隔边界上 |
| $\alpha_{i}^{*}=C$ | $0<\xi_{i}<1$ | 分类正确，$x_{i}$落在间隔边界与分离超平面之间 |
| $\alpha_{i}^{*}=C$ | $\xi_{i}=1$ | $x_{i}$落在分离超平面上 |
| $\alpha_{i}^{*}=C$ | $\xi_{i}>1$ | $x_{i}$落在分离超平面误分类一侧 |

# 非线性支持向量机与核函数

给定训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$，其中$x_{i}$属于输入空间，$x_{i} \in \mathcal{X}=\mathbf{R}^{n}$，对应的标记有两类$y_{i} \in \mathcal{Y}=\{-1,+1\}$，$i=1,2, \cdots, N$。如果能用$\mathbf{R}^{n}$中的一个超曲面将正负例正确分开，则称该问题为非线性可分问题。

**要解决非线性可分问题，就是将非线性问题转化为线性问题，通过解变换后的线性问题方法来解决原来的非线性问题。**

{% asset_img nonlinear.png 非线性分类问题与核技巧示例 %}

图1：非线性分类问题与核技巧示例

假设$\mathcal{X} \subset \mathbf{R}^{2}$，$x=\left(x^{(1)}, x^{(2)}\right)^{\mathrm{T}} \in \mathcal{X}$，新空间$\mathcal{Z} \subset \mathbf{R}^{2}$，$z=\left(z^{(1)}, z^{(2)}\right)^{\mathrm{T}} \in \mathcal{Z}$，定义从原空间到新空间的变换（映射）：

$$z=\phi(x)=\left(\left(x^{(1)}\right)^{2},\left(x^{(2)}\right)^{2}\right)^{\mathrm{T}}$$

经过变换$z=\phi(x)$，把原空间$\mathcal{X} \subset \mathbf{R}^{2}$变换为新空间$\mathcal{Z} \subset \mathbf{R}^{2}$，原空间中的点就变换为新空间中的点，原空间中的椭圆

$$w_{1}\left(x^{(1)}\right)^{2}+w_{2}\left(x^{(2)}\right)^{2}+b=0$$

变换为新空间中的直线

$$w_{1} z^{(1)}+w_{2} z^{(2)}+b=0$$

求解非线性分类问题分两步（核技巧）：

1. 使用一个变换将原空间数据映射到新空间。
2. 在新空间里用线性分类学习方法从训练数据中学习分类模型。

由于线性支持向量机的对偶问题中，无论目标函数还是决策函数都只涉及输入实例与实例之间的内积$x_{i} \cdot x_{j}$，因此可以使用核函数$K\left(x_{i}, x_{j}\right)=\phi\left(x_{i}\right) \cdot \phi\left(x_{j}\right)$来代替，此时的对偶目标函数为

$$W(\alpha)=\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j} K\left(x_{i}, x_{j}\right)-\sum_{i=1}^{N} \alpha_{i}$$

分类决策函数中的内积也可以使用核函数代替

$$\begin{aligned}
f(x) &=\operatorname{sign}\left(\sum_{i=1}^{N_{s}} a_{i}^{*} y_{i} \phi\left(x_{i}\right) \cdot \phi(x)+b^{*}\right) \\
&=\operatorname{sign}\left(\sum_{i=1}^{N_{s}} a_{i}^{*} y_{i} K\left(x_{i}, x\right)+b^{*}\right)
\end{aligned}$$

在实际应用中，直接计算$K(x, z)$比较容易，而通过$\phi(x)$和$\phi(z)$计算$K(x, z)$比较难。

$$K(x, z)=\phi(x) \cdot \phi(z)$$

由于学习过程是隐式的在特征空间进行，并不需要显示的定义特征空间和映射函数，所以该方法称为核技巧。

## 正定核

$K(x, z)$为正定核函数的充要条件是$x_{i} \in \mathcal{X}$，$i=1,2, \cdots, m$，$K(x, z)$对应的Gram矩阵

$$K=\left[K\left(x_{i}, x_{j}\right)\right]_{m \times m}$$

是半正定矩阵。

等价于设$\mathcal{X} \subset \mathbf{R}^{n}$，$K(x, z)$是定义在$\mathcal{X} \times \mathcal{X}$上的对称函数，如果对任意$x_{i} \in \mathcal{X}$，$i=1,2, \cdots, m$，$K(x, z)$对应的Gram矩阵

$$K=\left[K\left(x_{i}, x_{j}\right)\right]_{m \times m}$$

是半正定矩阵，那么称$K(x, z)$是正定核。

## 常用核函数

1. 多项式核函数（polynomial kernel function）

$$K(x, z)=(x \cdot z+1)^{p}$$

2. 高斯核函数（Gaussian kernel function）

$$K(x, z)=\exp \left(-\frac{\|x-z\|^{2}}{2 \sigma^{2}}\right)$$

3. 字符串核函数（string kernel function）**没懂**

$$\begin{aligned}
k_{n}(s, t) &=\sum_{u \in \Sigma^{n}}\left[\phi_{n}(s)\right]_{u}\left[\phi_{n}(t)\right]_{u} \\
&=\sum_{u \in \Sigma^{n}} \sum_{(i, j): s(i)=t(j)=u} \lambda^{l(i)} \lambda^{l(j)}
\end{aligned}$$

# SMO序列最小最优化算法（**没懂**）

SMO（序列最小最优化）算法要解如下凸二次规划的对偶问题

$$\begin{aligned} 
\underset{\alpha}{max} & \quad -\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}K(x, z)+\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad 0 \leqslant \alpha_{i} \leqslant C, \quad i=1,2, \cdots, N
\end{aligned}$$

# 工程实践

## 函数原型

```python
sklearn.svm.SVC(
            C=1.0, kernel='rbf', degree=3, gamma='scale',
            coef0=0.0, shrinking=True, probability=False,
            tol=1e-3, cache_size=200, class_weight=None,
            verbose=False, max_iter=-1, decision_function_shape='ovr',
            break_ties=False, random_state=None)
```

1. C（类型：flot，默认值：1.0，`超参：是`）：正则化参数，即惩罚项的系数。
2. kernel（类型：str，默认值：rbf，`超参：是`）：核函数的选择。scikit-learn中支持4种核函数，分别是：
    - linear：线性核函数$K\left(x, x^{\prime}\right)=\left\langle x, x^{\prime}\right\rangle$
    - poly：多项式核函数$K\left(x, x^{\prime}\right)=\left(\gamma\left\langle x, x^{\prime}\right\rangle+ r\right)^{d}$
    - rbf：径向核函数/高斯核函数$K\left(x, x^{\prime}\right)=\exp \left(-\gamma\left\|x-x^{\prime}\right\|^{2}\right)$
    - sigmoid：sigmoid核函数$K\left(x, x^{\prime}\right)=\tanh \left(\gamma\left\langle x, x^{\prime}\right\rangle+ r\right)$
    - precomputed：核矩阵，使用自定义转换矩阵
3. degree（类型：int，默认值：3，`超参：是`）：设置多项式的阶数。**只有kernel函数为poly才有效，即公式中的$d$。**
4. gamma（类型：int或str，默认值：scale，`超参：是`）：核函数的系数。**只有kernel函数为poly、rbf、sigmod才有效，即公式中的$\gamma$。**
    - 当gamma为scale时，$\text {gamma}=\frac{1}{\text {n_features} \times \text {X.var()}}$
    - 当gamma为auto时，$\text {gamma}=\frac{1}{\text {n_features}}$
5. coef0（类型：float，默认值：0.0，`超参：是`）：核函数的独立项。**只有kernel函数为poly、sigmod才有效，即公式中的$r$。**
6. shrinking（类型：boolean，默认值：True）：是否使用启发式的最优算法。
7. probability（类型：boolean，默认值：False）：是否使用概率估计。
8. tol（类型：float，默认值：1e-3）：停止训练的误差值大小。
9. cache_size（类型：float，默认值：200）：训练时所需要的内存大小，单位MB。
10. class_weight（类型：dict或str，默认值：None）：给每个类分别设置不同的惩罚参数C，即$\text {class_weight}\left [ i\right ] \times C$。如果参数是“balanced”，那么类别权重自动调整为与输入数据中的类频率成反比的权重$\frac{\text{n_samples}}{\text{n_classes} \times \text{np.bincount}\left ( y\right )}$。
11. verbose（类型：boolean，默认值：False）：是否启用详细输出。
12. max_iter（类型：int，默认值：-1）：最大迭代次数，如果为-1，那么不受限制。
13. decision_function_shape（类型：str，默认值：ovr）：决策函数类型。
    - ovo：返回形状为$\left (\text{n_samples}, \frac{\text{n_classes} \times \left ( \text{n_classes} - 1\right )}{2} \right )$的决策函数，该决策函数一般用于多类别策略。
    - ovr：返回形状为$\left (\text{n_samples}, \text{n_classes} \right )$的决策函数。
14. break_ties（类型：boolean，默认值：False）：当该值为True时，decision_function_shape为ovr，且类别数量大于2时，预测结果将根据decision_function_shape的置信度来来打破平局，否则就返回所有类别中的第一个。要注意的是打破平局相比简单的预测所需代价更高。
15. random_state（类型：int，默认值：None）：随机数种子，如果不设置则默认为`np.random`。

## 程序流程

1. 将原始数据转化为SVM算法软件或包所能识别的数据格式；
2. 将数据标准化；（防止样本中不同特征数值大小相差较大影响分类器性能）
3. 不知使用什么核函数，考虑使用RBF；
4. 利用交叉验证网格搜索寻找最优参数（C, γ）；（交叉验证防止过拟合，网格搜索在指定范围内寻找最优参数）
5. 使用最优参数来训练模型；
6. 测试。

## 核函数选择

1. 样本数量远小于特征数量：这种情况，利用情况利用linear核效果会高于RBF核。
2. 样本数量和特征数量一样大：linear核合适，且速度也更快。
3. 样本数量远大于特征数量：非线性核RBF等合适。

## GridSearchCV小技巧

网格大小如果设置范围大且步长密集的话难免耗时，但是不这样的话又可能找到的参数不是很好，针对这解决方法是，先在大范围，大步长的粗糙网格内寻找参数。在找到的参数左右在设置精细步长找寻最优参数比如：

1. 一开始寻找范围是$C=2^{-5},2^{-3}, \cdots ,2^{15}$和$\gamma = 2^{-15},2^{-13}, \cdots ,2^{3}$，由此找到的最优参数是$\left ( 2^{3}, 2^{-5}\right )$；
2. 然后设置更小一点的步长，参数范围变为$C=2^{1},2^{1.25}, \cdots ,2^{5}$和$\gamma = 2^{−7},2^{−6.75}, \cdots ,2^{-3}$

这样既可以避免一开始就使用大范围，小步长而导致分类器进行过于多的计算而导致计算时间的增加。

## 常用函数

1. 样本到分离超平面的距离

```python
sklearn.svm.SVC.decision_function(X)
```

2. 根据给定的训练数据拟合SVM模型

```python
sklearn.svm.SVC.fit(X, y[, sample_weight=None])
```

3. 获取此估算器的参数并以字典形式显示

```python
sklearn.svm.SVC.get_params([deep=True])
```

4. 根据测试数据集进行预测

```python
sklearn.svm.SVC.predict(X)
```

5. 返回给定测试数据和标签的平均精确度

```python
sklearn.svm.SVC.score(X, y)
```

6. 返回样本的对数概率以及普通概率，注意该函数只有在SVM构建时的probability参数为True时才有效。

```python
sklearn.svm.SVC.predict_log_proba(X_test)
sklearn.svm.SVC.predict_proba(X_test)
```

# 作业

1、完成习题7.2：已知正例点$x_{1}=(1,2)^{\top}$，$x_{2}=(2,3)^{\top}$，$x_{3}=(3,3)^{\top}$，负例点$x_{4}=(2,1)^{\top}$，$x_{5}=(3,2)^{\top}$，试求最大间隔分离超平面和分类决策函数，并在图上画出分离超平面、间隔边界及支持向量

解：根据下式

$$\begin{aligned} 
\underset{\alpha}{min} & \quad \frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)-\sum_{i=1}^{N} \alpha_{i} \\
\text { s.t. } & \quad \sum_{i=1}^{N} \alpha_{i} y_{i}=0 \\
 & \quad \alpha_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

可以得到

$$\begin{aligned} 
\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)-\sum_{i=1}^{N} \alpha_{i} & = \frac{1}{2}\left(5\alpha_{1}^{2}+13\alpha_{2}^{2}+18\alpha_{3}^{2}+5\alpha_{4}^{2}+13\alpha_{5}^{2}+16\alpha_{1}\alpha_{2}+18\alpha_{1}\alpha_{3}-8\alpha_{1}\alpha_{4}-14\alpha_{1}\alpha_{5}+30\alpha_{2}\alpha_{3}-14\alpha_{2}\alpha_{4}-24\alpha_{2}\alpha_{5}-18\alpha_{3}\alpha_{4}-30\alpha_{3}\alpha_{5}-16\alpha_{4}\alpha_{5}\right)-\alpha_{1}-\alpha_{2}-\alpha_{3}-\alpha_{4}-\alpha_{5} \\
\sum_{i=1}^{N} \alpha_{i} y_{i} & = \alpha_{1}+\alpha_{2}+\alpha_{3}-\alpha_{4}-\alpha_{5}=0 \\
 & \quad \alpha_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

把$\alpha_{5} = \alpha_{1}+\alpha_{2}+\alpha_{3}-\alpha_{4}$代入上式消去$\alpha_{5}$，再分别对上式$\alpha_{1}$、$\alpha_{2}$、$\alpha_{3}$、$\alpha_{4}$求偏导，并令其为0

$$\begin{aligned}   
\nabla_{\alpha_{1}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = 4\alpha_{1}+2\alpha_{2}-2\alpha_{4}-2=0 \\
\nabla_{\alpha_{2}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = 2\alpha_{1}+2\alpha_{2}+\alpha_{3}-2=0 \\
\nabla_{\alpha_{3}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = \alpha_{2}+\alpha_{3}+\alpha_{4}-2=0 \\
\nabla_{\alpha_{4}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = -2\alpha_{1}+\alpha_{3}+2\alpha_{4}=0 
\end{aligned}$$

由于上式无解，所以通过观测，由于$x_{2}$不在间隔边界，因此$\alpha_{2}=0$，因此上式的求导就变为

$$\begin{aligned}   
\nabla_{\alpha_{1}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = 4\alpha_{1}+2\alpha_{2}-2\alpha_{4}-2=0 \\
\nabla_{\alpha_{3}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = \alpha_{2}+\alpha_{3}+\alpha_{4}-2=0 \\
\nabla_{\alpha_{4}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = -2\alpha_{1}+\alpha_{3}+2\alpha_{4}=0 
\end{aligned}$$

代入$\alpha_{2}=0$得到

$$\begin{aligned}   
\nabla_{\alpha_{1}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = 2\alpha_{1}-\alpha_{4}-1=0 \\
\nabla_{\alpha_{3}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = \alpha_{3}+\alpha_{4}-2=0 \\
\nabla_{\alpha_{4}} L(\alpha_{1}, \alpha_{2},\alpha_{3},\alpha_{4}) & = -2\alpha_{1}+\alpha_{3}+2\alpha_{4}=0 
\end{aligned}$$

由于上式依然无解，因此分三种情况进行讨论，分别是：

> 情况1：$\alpha_{1}=0,\alpha_{2}=0$

求得$\alpha_{1}=0$、$\alpha_{2}=0$、$\alpha_{3}=2$、$\alpha_{4}=0$、$\alpha_{5}=2$

> 情况2：$\alpha_{3}=0,\alpha_{2}=0$

求得$\alpha_{1}=1$、$\alpha_{2}=0$、$\alpha_{3}=0$、$\alpha_{4}=1$、$\alpha_{5}=0$

> 情况3：$\alpha_{4}=0,\alpha_{2}=0$

求得$\alpha_{1}=0.5$、$\alpha_{2}=0$、$\alpha_{3}=2$、$\alpha_{4}=0$、$\alpha_{5}=2.5$

把所有解分别代入，求最小值

$$L(\alpha)=\frac{1}{2} \sum_{i=1}^{N} \sum_{j=1}^{N} \alpha_{i} \alpha_{j} y_{i} y_{j}\left(x_{i} \cdot x_{j}\right)-\sum_{i=1}^{N} \alpha_{i}$$

求得

$L_{1}(\alpha)=-2$，$L_{2}(\alpha)=-1$，$L_{3}(\alpha)=-2.5$

所以$L_{3}(\alpha)$最小，所以$\alpha$的最优解为$\alpha_{1}=0.5$、$\alpha_{2}=0$、$\alpha_{3}=2$、$\alpha_{4}=0$、$\alpha_{5}=2.5$。

再把所有$\alpha$代入下式，可以求得$w^{*}$和$b^{*}$

$$w^{*}=\sum_{i=1}^{N} \alpha_{i}^{*} y_{i} x_{i}=\left(-1,2\right)$$

$$b^{*}=y_{j}-\sum_{i=1}^{N} \alpha_{i}^{*} y_{i}\left(x_{i} \cdot x_{j}\right)=-2$$

所以分离超平面为

$$-x^{(1)}+2 x^{(2)}-2=0$$

决策函数为

$$f(x)=\operatorname{sign}\left(-x^{(1)}+2 x^{(2)}-2\right)$$

所以，$x_{1}=(1,2)^{\top}$，$x_{3}=(3,3)^{\top}$，$x_{5}=(3,2)^{\top}$是支持向量。

2、完成习题7.3：线性支持向量机还可以定义成以下形式：

$$\begin{aligned} 
\underset{w,b,\xi}{min} & \quad \frac{1}{2}\|w\|^{2}+C \sum_{i=1}^{N} \xi_{i}^{2} \\
\text { s.t. } & \quad y_{i}\left(w \cdot x_{i}+b\right) \geqslant 1-\xi_{i}, i=1,2, \cdots, N \\
& \quad \xi_{i} \geqslant 0, \quad i=1,2, \cdots, N
\end{aligned}$$

试求其对偶形式。

3、试调用sklearn.svm中的SVC模块求解习题7.2，长时改变参数，如C、kernel，比较结果

```python
import numpy as np
import time

from sklearn.svm import SVC

if __name__ == '__main__':
    # 计算开始时间
    star = time.time()

    data = np.array([
        [1, 2, 1], [2, 3, 1], [3, 3, 1],
        [2, 1, -1], [3, 2, -1]
    ])

    train_x = data[:, 0:2]
    train_y = data[:, 2]

    clf = SVC(C=100, kernel='linear')
    clf.fit(train_x, train_y)

    # 获取w
    w = clf.coef_[0]
    # 获取截距
    b = clf.intercept_

    # 打印超平面
    print(clf.support_vectors_)
    print(w, b)

    # 计算结束事时间
    end = time.time()
    print('用时：{:.3f}s'.format(end - star))
```