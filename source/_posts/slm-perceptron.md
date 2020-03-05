---
title: S2-感知机
mathjax: true
date: 2020-03-05 09:50:44
updated: {{ date }}
top: true
tags:
categories: [统计学习方法]
---

# 感知机模型

感知机模型（perceptron）是**二分类的线性分类模型**，其输入为实例的特征向量，输出为实例的类别，取+1或-1。感知机学习旨在求出将训练数据进行线性划分的超平面，**利用梯度下降对误分类的损失函数进行极小化求解**，从而得到感知机模型。

$$f(x)=\operatorname{sign}(w \cdot x+b)$$

其中$x \in \mathcal{X} \subseteq \mathbf{R}^{n}$表示输入的特征空间，$y \in \mathcal{Y}=\{+1,-1\}$表示输出的分类。$w$和$b$为感知机模型的参数，$w \in \mathbf{R}^{n}$为权值向量，**与$x$具有相同的维度**， 并且$w$也是超平面$w \cdot x+b=0$的法向量。$b \in \mathbf{R}$为偏置（bias），**是一个标量**。$w \cdot x$是$w$和$x$的内积。$\operatorname{sign}$是一个符号函数。在超平面法向量方向上为+1，法向量的反方向为-1。

$$\operatorname{sign}(x)=\left\{\begin{array}{ll}
+1, & x \geqslant 0 \\
-1, & x<0
\end{array}\right.$$

# 感知机学习策略

## 数据集的线性可分性

给定一个数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

存在一个超平面$S$能够将数据集的正实例和负实例点完全划分到超平面的两侧。

$$w \cdot x+b=0$$

对于所有$y_{i}=+1$的实例都有$w \cdot x_{i}+b>0$，而对于所有$y_{i}=-1$的实例都有$w \cdot x_{i}+b<0$，那么数据集$T$为线性可分数据集，否则就是线性不可分。

## 感知机学习策略

感知机的损失函数是**误分类点**到超平面$S$的总距离。点$x_{0}$到超平面的距离可以表示为

$$\frac{1}{\|w\|}\left|w \cdot x_{0}+b\right|$$

其中$\left|w \cdot x_{0}+b\right|$表示对超平面得出的值取绝对值，$\|w\|$表示$w$的$L_{2}$范数。对于$n$维$w$，$\|w\|$可以表示为$\left \| w\right \|=\sqrt{w_{1}^{2} + \cdots +w_{n}^{2}}$。

对于误分类数据来说，超平面$S = w \cdot x_{i}+b$得出的值与标签$y_{i}$的符号正好相反，因此

$$-y_{i}\left(w \cdot x_{i}+b\right)>0$$

同时也可以得到误分类点到超平面$S$的距离为

$$-\frac{1}{\|w\|} y_{i}\left(w \cdot x_{i}+b\right)$$

由于$y_{i}$的是为$\{+1,-1\}$，所以并不影响距离计算。

因此就可以得到所有误分类点到超平面$S$的总距离为

$$-\frac{1}{\|w\|} \sum_{x_{i} \in M} y_{i}\left(w \cdot x_{i}+b\right)$$

由于$\|w\|$是常数，所以对总距离没有影响，因此可以到误分类点的损失函数为

$$L(w, b)=-\sum_{x_{i} \in M} y_{i}\left(w \cdot x_{i}+b\right)$$

当没有误分类点时，$L(w, b) = 0$，否则$L(w, b) > 0$。

# 感知机学习算法

## 感知机学习算法的原始形式

求解参数$w$和$b$，使得损失函数最小

$$\min _{w, b} L(w, b)=-\sum_{x_{i} \in M} y_{i}\left(w \cdot x_{i}+b\right)$$

其中$M$为误分类点的集合。

采用随机梯度下降（SGD）来实现。取任意超平面参数$w_{0}$和$b_{0}$，然后用SGD不断极小化损失函数。在极小化过程中，由于采用SGD，**因此不是对所有的误分类集合中的点进行梯度下降，而时随机选取一个误分类点进行梯度下降。** 所以误分类点的梯度可以表示为：

$$\begin{aligned}
&\nabla_{w} L(w, b)=-\sum_{x_{i} \in M} y_{i} x_{i}\\
&\nabla_{b} L(w, b)=-\sum_{x_{i} \in M} y_{i}
\end{aligned}$$

所以$w$和$b$的更新方程为：

$$\begin{aligned}
w &:=w+\eta y_{i} x_{i} \\
b &:=b+\eta y_{i}
\end{aligned}$$

其中$\eta(0<\eta \leqslant 1)$为学习率。

## 感知机学习算法的对偶形式

假设$w$和$b$的初值为0，那么最后学习得到的$w$和$b$可以表示为

$$w=\sum_{i=1}^{N} n_{i} \eta y_{i} x_{i}=\sum_{i=1}^{N} \alpha_{i} y_{i} x_{i}$$

$$b=\sum_{i=1}^{N} n_{i} \eta y_{i}=\sum_{i=1}^{N} \alpha_{i} y_{i}$$

其中$\alpha_{i}=n_{i} \eta$，$\alpha_{i} \geqslant 0$，$i=1,2, \cdots, N$。

根据上面的形式，就可以得到感知机模型的对偶形式

$$f(x)=\operatorname{sign}\left(\sum_{j=1}^{N} \alpha_{j} y_{j} x_{j} \cdot x+b\right)$$

其中$\alpha=\left(\alpha_{1}, \alpha_{2}, \cdots, \alpha_{N}\right)^{\mathrm{T}}$。所以损失函数可以表达为

$$\min _{w, b} L(w, b)=-\sum_{x_{i} \in M} y_{i}\left(\sum_{j=1}^{N} \alpha_{j} y_{j} x_{j} \cdot x+b\right)$$

$\alpha_{i}$和$b$的更新方程为：

$$\begin{aligned}
\alpha_{i} &:=\alpha_{i}+\eta \\
b &:=b+\eta y_{i}
\end{aligned}$$

**这里需要注意的是，每次更新时，只更新对应误分类点$\alpha_{i}$的值，而其他点不用更新（对偶算法的优势，减少计算量，原始算法是更新向量，而这里只更新误分类点）。**

对偶形式的优势：

> 1. 在更新$\alpha_{i}$时，只需要更新误分类点，而不是原始算法中更新整个向量。
> 2. 在对偶形式的损失函数中，$x_{j} \cdot x$是一个内积，该内积可以提前算法，即Gram矩阵。

$$Gram=A^{T} A=\left[\begin{array}{c}
a_{1}^{T} \\
a_{2}^{T} \\
\vdots \\
a_{n}^{T}
\end{array}\right]\left[\begin{array}{llll}
a_{1} & a_{2} & \ldots & a_{n}
\end{array}\right]=\left[\begin{array}{cccc}
a_{1}^{T} a_{1} & a_{1}^{T} a_{2} & \cdots & a_{1}^{T} a_{n} \\
a_{2}^{T} a_{1} & a_{2}^{T} a_{2} & \cdots & a_{2}^{T} a_{n} \\
a_{n}^{T} a_{1} & a_{n}^{T} a_{2} & \cdots & a_{n}^{T} a_{n}
\end{array}\right]$$

# 感知机的收敛性

设训练数据集$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$是线性可分的，其中$x_{i} \in \mathcal{X}=\mathbf{R}^{n}$，$y_{i} \in \mathcal{Y}=\{-1,+1\}$，$i=1,2, \cdots, N$，则

（1）存在满足条件$\left\|\hat{w}_{\mathrm{opt}}\right\|=1$的超平面$\hat{w}_{\mathrm{opt}} \cdot \hat{x}=w_{\mathrm{opt}} \cdot x+b_{\mathrm{opt}}=0$将训练数据集完全正确分开；且存在$\gamma>0$，对所有$i=1,2, \cdots, N$

$$y_{i}\left(\hat{w}_{\mathrm{opt}} \cdot \hat{x}_{i}\right)=y_{i}\left(w_{\mathrm{opt}} \cdot x_{i}+b_{\mathrm{opt}}\right) \geqslant \gamma$$

（2）令$R=\underset{1 \leqslant i \leqslant N}{max}\left\|\hat{x}_{i}\right\|$，则该值计算法在训练数据集上的误分类次数$k$满足不等式

$$k \leqslant\left(\frac{R}{\gamma}\right)^{2}$$

由定义知

$$\hat{w}_{\mathrm{opt}}=\left ( w_{opt}^{T}, b_{opt}\right )^{T}$$

$$\hat{x}=\left ( x^{T}, 1\right )^{T}$$

（1）证明

$\because y_{i}\left(\hat{w}_{\mathrm{opt}} \cdot \hat{x}_{i}\right)=y_{i}\left(w_{\mathrm{opt}} \cdot x_{i}+b_{\mathrm{opt}}\right)>0$

$\therefore \text{存在} \gamma=\underset{i}{min}\left\{y_{i}\left(w_{\mathrm{opt}} \cdot x_{i}+b_{\mathrm{opt}}\right)\right\}$

使得

$$y_{i}\left(\hat{w}_{\mathrm{opt}} \cdot \hat{x}_{i}\right)=y_{i}\left(w_{\mathrm{opt}} \cdot x_{i}+b_{\mathrm{opt}}\right) \geqslant \gamma$$

（2）证明

> 第一步：证明$\left ( \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \geqslant k \eta \gamma\right )$

有感知机的更新公式得到$\hat{w}_{k}=\hat{w}_{k-1}+\eta y_{i} \hat{x}_{i}$

$\therefore \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}}=\left ( \hat{w}_{k-1}+\eta y_{i} \hat{x}_{i}\right ) \cdot \hat{w}_{\mathrm{opt}}=\hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}}+\eta y_{i} \hat{x}_{i} \cdot \hat{w}_{\mathrm{opt}}$

$\because y_{i}\left(\hat{w}_{\mathrm{opt}} \cdot \hat{x}_{i}\right) \geqslant \gamma$

$\therefore \hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}}+\eta y_{i} \hat{x}_{i} \cdot \hat{w}_{\mathrm{opt}} \geqslant \hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}} + \eta \gamma$

$\therefore \hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}}+\eta y_{i} \hat{x}_{i} \cdot \hat{w}_{\mathrm{opt}} \geqslant \hat{w}_{k-2} \cdot \hat{w}_{\mathrm{opt}} + \eta \gamma + \eta \gamma$

以此类推

$\therefore \hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}}+\eta y_{i} \hat{x}_{i} \cdot \hat{w}_{\mathrm{opt}} \geqslant \hat{w}_{0} \cdot \hat{w}_{\mathrm{opt}} + k \eta \gamma$

假设$\hat{w}_{0}=\left ( 0, \cdots , 0\right )^{T}$

$\therefore \hat{w}_{k-1} \cdot \hat{w}_{\mathrm{opt}}+\eta y_{i} \hat{x}_{i} \cdot \hat{w}_{\mathrm{opt}} \geqslant k \eta \gamma$

$\therefore \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \geqslant k \eta \gamma$

> 第二步$\left ( \left\|\hat{w}_{k}\right\|^{2} \leq k\eta^{2}R^{2}\right )$

$\because y_{i}=\{-1,+1\}$

$\therefore \left \| y_{i}\right \|^{2}=1$

$\because \left\|\hat{w}_{k}\right\|^{2}=\left \| \hat{w}_{k-1}+\eta y_{i} \hat{x}_{i}\right \|^{2}=\left\|\hat{w}_{k-1}\right\|^{2}+2 \eta y_{i} \hat{w}_{k-1} \cdot \hat{x}_{i}+\eta^{2}\left\|\hat{x}_{i}\right\|^{2}$

又$\because y_{i}$是误分类点

$\therefore y_{i} < 0$

$\therefore \eta y_{i} \hat{w}_{k-1} \cdot \hat{x}_{i} < 0$

$\therefore \left\|\hat{w}_{k-1}\right\|^{2}+2 \eta y_{i} \hat{w}_{k-1} \cdot \hat{x}_{i}+\eta^{2}\left\|\hat{x}_{i}\right\|^{2} \leq \left\|\hat{w}_{k-1}\right\|^{2}+\eta^{2}\left\|\hat{x}_{i}\right\|^{2}$

又$\because R=\underset{1 \leqslant i \leqslant N}{max}\left\|\hat{x}_{i}\right\|$

$\therefore \left\|\hat{w}_{k-1}\right\|^{2}+\eta^{2}\left\|\hat{x}_{i}\right\|^{2}=\left\|\hat{w}_{k-1}\right\|^{2}+\eta^{2}R^{2}$

$\therefore \left\|\hat{w}_{k}\right\|^{2} \leq \left\|\hat{w}_{k-1}\right\|^{2}+\eta^{2}R^{2}$

以此类推

$\therefore \left\|\hat{w}_{k}\right\|^{2} \leq \left\|\hat{w}_{0}\right\|^{2}+k\eta^{2}R^{2}$

$\because \hat{w}_{0}=\left ( 0, \cdots , 0\right )^{T}$

$\therefore \left\|\hat{w}_{k}\right\|^{2} \leq k\eta^{2}R^{2}$

根据柯西不等式$\hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \leqslant\left\|\hat{w}_{k}\right\|\left\|\hat{w}_{\mathrm{opt}}\right\|$

$\because \left\|\hat{w}_{\mathrm{opt}}\right\|=1$

$\therefore \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \leqslant\left\|\hat{w}_{k}\right\|$

$\therefore \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \leqslant\left\|\hat{w}_{k}\right\| \leqslant \sqrt{k\eta^{2}R^{2}}$

$\therefore k \eta \gamma \leqslant \hat{w}_{k} \cdot \hat{w}_{\mathrm{opt}} \leqslant\left\|\hat{w}_{k}\right\| \leqslant \sqrt{k\eta^{2}R^{2}}$

$\therefore k \eta \gamma \leqslant \sqrt{k\eta^{2}R^{2}}$

$\therefore k \leqslant\left(\frac{R}{\gamma}\right)^{2}$