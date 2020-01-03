---
title: 优化理论和应用
mathjax: true
date: 2020-01-01 19:25:21
updated: {{ date }}
categories: [课程]
---

# 介绍

## 优化问题

$$\begin{matrix}
    minimize & f_{0}\left ( \mathbf{x} \right ) & \\ 
    st. & f_{i}\left ( \mathbf{x} \right ) \leq b_{i} & i=1,\cdots ,M\\ 
    variables & \mathbf{x} & 
\end{matrix}$$

$\mathbf{x}=\left(x_{1}, \dots, x_{n}\right)^{\top}$：优化变量

$f_{0}: \mathbb{R}^{n} \rightarrow \mathbb{R}$：目标函数

$f_{i}: \mathbb{R}^{n} \rightarrow \mathbb{R}, i=1, \ldots, M$：约束函数

最优解：在满足约束的情况下，$\mathbf{X}^{\star}$是$f_{0}$中所有向量的最小值。

可以高效可靠解决的问题：

>* 1、线性规划问题
>* 2、最小二乘问题
>* 3、凸优化问题

## 线性规划问题

$$\begin{matrix}
    minimize & \mathbf{c}^{\top} \mathbf{x} & \\ 
    st. & \mathbf{a}_{i}^{\top} \mathbf{x} \leq b_{i} & i=1,\cdots ,M\\ 
    variables & \mathbf{x} & 
\end{matrix}$$

### 整数线性规划问题

$$\begin{matrix}
    minimize & \mathbf{c}^{\top} \mathbf{x} & \\ 
    st. & \mathbf{a}_{i}^{\top} \mathbf{x} \leq b_{i} & i=1,\cdots ,M\\ 
    & \mathbf{x} \in \mathbb{Z}^{n} & \\ 
    variables & \mathbf{x} & 
\end{matrix}$$

## 最小二乘问题

$$\begin{matrix}
    minimize & \left \| \mathbf{A}\mathbf{x}-\mathbf{b} \right \|_{2}^{2} & \\ 
    variables & \mathbf{x} & 
\end{matrix}$$

## 凸优化问题

$$\begin{matrix}
    minimize & f_{0}(\mathbf{x}) & \\ 
    st. & f_{i}(\mathbf{x}) \leq b_{i} & i=1,\cdots ,M\\ 
    variables & \mathbf{x} & 
\end{matrix}$$

>* 目标函数和约束函数都是凸函数。

$f_{i}(\alpha \mathbf{x}+\beta \mathbf{y}) \leq \alpha f_{i}(\mathbf{x})+\beta f_{i}(\mathbf{y})$， 假如$\alpha+\beta=1, \alpha \geq 0, \beta \geq 0$

>* 最小二乘和特殊情况下的线性规划是凸优化问题。

## 优化模型分类

>* 1、按照解决方法：数值优化和分析优化。
>* 2、按照约束条件：无约束优化和有约束优化，其中有约束优化分为集约束优化、等式约束优化、不等式约束优化。
>* 3、按照信息完整性：正态优化和信息不确定性的优化，其中信息不确定性的优化分为随机优化和鲁棒优化。
>* 4、按照问题属性：线性规划和非线性规划，其中非线性规划分为凸优化、二次规划、一般非线性规划。
>* 5、按照目标：单目标优化和多目标优化。

# 基础知识

## 符号说明

1、$\mathbb{X}$是一个集合，$x \in \mathbb{X}$表示$x$是$\mathbb{X}$中的元素。

2、$\{x 1, x 2, x 3, \ldots\}$和$\{x: x \in \mathbb{R}, x>5\}$都可以表示一个集合。

3、$\mathbb{X}$和$\mathbb{Y}$都是集合，如果$\mathbb{X} \subset \mathbb{Y}$，那么表示$\mathbb{X}$是$\mathbb{Y}$的子集。

4、$f: \mathbb{X} \rightarrow \mathbb{Y}$表示$f$是从集合$\mathbb{X}$到$\mathbb{Y}$的函数。

5、$\overset{\triangle}{=}$表示“定义为”。

6、假设定义n阶向量$\mathbf{a}=\left[\begin{array}{c}{a_{1}} \\ {a_{2}} \\ {\vdots} \\ {a_{n}}\end{array}\right]$，那么$\mathbf{a}^{\top}=\left[a_{1}, a_{2}, \ldots, a_{n}\right]$。

## 线性相关

向量集$\left \{ \mathbf{a_{1}},\mathbf{a_{2}},\cdots ,\mathbf{a_{k}} \right \}$是线性相关，当且仅当集合中的一个向量可以表示为其他向量的线性组合。

必要条件：$\alpha_{1}\mathbf{a_{1}}+\alpha_{2}\mathbf{a_{2}}+\cdots +\alpha_{k}\mathbf{a_{k}}=\mathbf{0}$，并且其中至少有一个$\alpha_{i} \neq 0$，否则就是不相关。

充分条件：$\left ( -1 \right )\mathbf{a_{1}}+\alpha_{2}\mathbf{a_{2}}+\cdots +\alpha_{k}\mathbf{a_{k}}=\mathbf{0}$

## 二次型函数

$$f\left ( \mathbf{x} \right )=\mathbf{x}^{\top}\mathbf{Q}\mathbf{x}$$

对于任意非零向量$\mathbf{x}$，都有$\mathbf{x}^{\top}\mathbf{Q}\mathbf{x} > 0$，并且如果$\mathbf{Q}=\mathbf{Q}^{T}$，那么$\mathbf{Q}$是正定的。如果$\mathbf{x}^{\top}\mathbf{Q}\mathbf{x} \geq 0$，那么$\mathbf{Q}$是半正定的。相反，还是负定和半负定。

### 二次规划

$$\begin{matrix}
    minimize & \mathbf{x}^{\top}\mathbf{Q}\mathbf{x} + \mathbf{c}^{\top}\mathbf{x} \\ 
    st. & \mathbf{Ax}\leq \mathbf{b}\\ 
\end{matrix}$$

### 线性规划最优控制

$$\begin{matrix}
    minimize & J\left(u, x_{0}, t_{0}, t_{f}\right)=\int_{t_{0}}^{t_{f}}\left[x^{T}(t) Q(t) x(t)+u^{T}(t) R(t) u(t)\right] d t+x\left(t_{f}\right)^{T} S x\left(t_{f}\right) & \\ 
    st. & \dot{x}=A(t) x+B(t) u(t)\\ 
\end{matrix}$$

## 矩阵

### 矩阵范数

$$\left \| \mathbf{A} \right \|_{F}=\left ( \sum_{i=1}^{m}\sum_{j=1}^{n}\left ( a_{ij} \right )^{2} \right )^{\frac{1}{2}}$$

### 线性方程

假设有矩阵$$\mathbf{A}=\left[\mathbf{a}_{1}, \mathbf{a}_{2}, \ldots, \mathbf{a}_{n}\right]$，那么就可以用$\mathbf{A x}=\mathbf{b}$表示一个线性方程。

### 内积

假设$\mathbf{x}, \mathbf{y} \in \mathbb{R}^{n}$，那么内积就可以表示为$\left \langle \mathbf{x}, \mathbf{y} \right \rangle=\sum_{i=1}^{n} x_{i} y_{i}=\mathbf{x}^{T} \mathbf{y}$

### 特征值和特征矩阵

令$\mathbf{A}$是$n \times n$的矩阵，$\lambda$是一个标量，$\mathbf{v}$是一个非0矩阵，如果$\mathbf{Av}=\lambda \mathbf{v}$，那么$\lambda$就是特征值，\mathbf{v}就是特征矩阵。

## 几何概念

### 线段

线段的定义：$\mathbf{z}=\alpha \mathbf{x}+(1-\alpha) \mathbf{y}$，并且$\alpha \in \left [ 0,1 \right ]$。

### 超平面

假设$u_{1}, u_{2}, \ldots, u_{n}, v \in \mathbb{R}$，其中至少有一个$u_{i}$是非零，并且所有点的集合$\mathbf{x}=\left[x_{1}, x_{2}, \ldots, x_{n}\right]^{T}$组成的线性方程

$$u_{1} x_{1}+u_{2} x_{2}+\ldots+u_{n} x_{n}=v$$

称为超平面。

当$n=2$时，超平面为一条直线。

当$n=3$时，超平面为一个平面。

即超平面为源空间维度的$n-1$。

当$u_{1} x_{1}+u_{2} x_{2}+\ldots+u_{n} x_{n} \geq v$，表示正半空间。$H_{+}=\left\{\mathbf{x} \in \mathbb{R}^{n}: \mathbf{u}^{T} \mathbf{x} \geq v\right\}$。

当$u_{1} x_{1}+u_{2} x_{2}+\ldots+u_{n} x_{n} \leq v$，表示正半空间。$H_{-}=\left\{\mathbf{x} \in \mathbb{R}^{n}: \mathbf{u}^{T} \mathbf{x} \leq v\right\}$。

### 凸集

假设$\Theta \in \mathbb{R}^{n}$是一个集合，那么如果集合中的任意两个向量$\mathbf{u}$和$\mathbf{v}$组成的线段中的点也在集合中，那么该集合为凸集，即$\alpha \mathbf{u}+(1-\alpha) \mathbf{v} \in \Theta$，且$\alpha \in[0,1]$。

凸集的例子：空集、由一个点组成的集合、一条直线和线段、子空间、超平面、半空间、$\mathbb{R}^{n}$。

### 凸包

$\textbf{conv} \mathbb{C}=\left \{ \theta_{1} \mathbf{x}_{1}+\cdots+\theta_{k} \mathbf{x}_{k} \mid \mathbf{x}_{i} \in \mathbb{C}, \theta_{i} \geq 0, i=1, \ldots, k,\theta_{1}+\cdots+\theta_{k}=1 \right \}$

### 领域

点$\mathbf{x} \in \mathbb{R}^{n}$的邻域可以定义为$\left\{\mathbf{y} \in \mathbb{R}^{n}:\|\mathbf{y}-\mathbf{x}\|<\varepsilon\right\}$，其中$\varepsilon$为正值。

## 微积分

1、给定$f: \mathbb{R}^{n} \rightarrow \mathbb{R}$，那么$f$的梯度是一个函数$\nabla f: \mathbb{R}^{n} \rightarrow \mathbb{R}^{n}$，记作$\nabla f(\mathbf{x})=\left[\begin{array}{c}{\frac{\partial f}{\partial x_{1}}(\mathbf{x})} \\ {\vdots} \\ {\frac{\partial f}{\partial x_{n}}(\mathbf{x})}\end{array}\right]$，其中$\nabla f(\mathbf{x})$是一个在$\mathbb{R}^{n}$中的向量，记作$\nabla f(\mathbf{x})=D f(\mathbf{x})^{\top}$。

2、给定$f: \mathbb{R}^{n} \rightarrow \mathbb{R}^{m}$，$f=\left[f_{1}, \ldots, f_{m}\right]^{\top}$，那么$f$的微分方程$D f: \mathbb{R}^{n} \rightarrow \mathbb{R}^{m \times n}$为$D f(x)=\left[\begin{array}{ccc}{\frac{\partial f_{1}}{\partial x_{1}}(\mathbf{x})} & {\dots} & {\frac{\partial f_{1}}{\partial x_{n}}(\mathbf{x})} \\ {\vdots} & {} & {\vdots} \\ {\frac{\partial f_{m}}{\partial x_{1}}(\mathbf{x})} & {\cdots} & {\frac{\partial f_{m}}{\partial x_{n}}(\mathbf{x})}\end{array}\right]$。

3、如果$f$是二阶可微，那么$\mathbf{F}=D^{2} f=\left[\begin{array}{cccc}{\frac{\partial^{2} f}{\partial x_{1}^{2}}} & {\frac{\partial^{2} f}{\partial x_{2} \partial x_{1}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{n} \partial x_{1}}} \\ {\frac{\partial^{2} f}{\partial x_{1} \partial x_{2}}} & {\frac{\partial^{2} f}{\partial x_{2}^{2}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{n} \partial x_{1}}} \\ {\vdots} & {\vdots} & {\ddots} & {\vdots} \\ {\frac{\partial^{2} f}{\partial x_{1} \partial x_{n}}} & {\frac{\partial^{2} f}{\partial x_{2} \partial x_{n}}} & {\cdots} & {\frac{\partial^{2} f}{\partial x_{n}^{2}}}\end{array}\right]$，该矩阵称为$f$的黑塞矩阵（Hessian）。

## 水平集和梯度

$\nabla f\left(\mathbf{x}_{0}\right)$是$f$在$\mathbf{x}_{0}$点处最大增长率方向。

# 无约束优化问题

## 局部极小点x

$\mathbf{x}^{\ast}$是局部极小点的条件是$\nabla f\left(\mathbf{x}^{\ast}\right)=0$。

## 可行方向

如果存在$\alpha_{0}>0$，并且满足$\alpha \in\left[0, \alpha_{0}\right]$，使得$\mathbf{x}+\alpha \mathbf{d} \in \Omega$，那么$\mathbf{d} \in \mathbb{R}^{n}, \mathbf{d} \neq 0$就是$\mathbf{x} \in \Omega$中的可行方向。

### 方向导数

当可行方向$\mathbf{d}$是单位向量时，那么函数$f$在$\mathbf{x}$处沿方向$\mathbf{d}$的增长率可以用内积表示，即$\frac{\partial f}{\partial \mathbf{d}}(\mathbf{x})=\mathbf{d}^{\top} \nabla f(\mathbf{x})=\nabla f(\mathbf{x})^{\top} \mathbf{d}=\left \langle \nabla f(\mathbf{x}), \mathbf{d} \right \rangle$。

### FONC（一阶必要条件）

如果$\mathbf{x}^{\ast}$是函数$f$在$\Omega$上的局部极小点，那么对于$\mathbf{x}^{\ast}$处的任意可行方向$\mathbf{d}$都有$\mathbf{d}^{T} \nabla f\left(\mathbf{x}^{\ast}\right) \geq 0$。

### SONC（二阶必要条件）

如果$f$在约束集$\Omega$上二阶连续可导，并且$\mathbf{x}^{\ast}$是函数$f$在$\Omega$上的局部极小点，$\mathbf{d}$是$\mathbf{x}^{\ast}$处的可行方向，并且$\mathbf{d}^{T} \nabla f\left(\mathbf{x}^{\ast}\right) = 0$，则$\mathbf{d}^{T} F\left(\mathbf{x}^{\ast}\right) \mathbf{d} \geq 0$，其中$F$为函数$f$的黑塞矩阵。

### SOSC（二阶充分条件）

如果$f$在约束集$\Omega$上二阶连续可导，如果同时满足$\nabla f\left(\mathbf{x}^{\ast}\right)=0$和$F\left(\mathbf{x}^{\ast}\right)>0$，那么$\mathbf{X}^{\ast}$为严格局部极小点。

# 一维搜索方法

## 介绍

该方法主要使用迭代搜索算法或线搜法，基本流程是：

>* 1、赋一个初值$x^{\left ( 0 \right )}$。
>* 2、生成迭代序列$x^{\left ( 1 \right )},x^{\left ( 2 \right )},\cdots$。
>* 3、每次迭代，下一个$x^{\left ( k+1 \right )}$依赖于上一个$x^{\left ( k \right )}$。

目标函数$f$，一阶导数${f}'$，二阶导数${f}''$。

常用的一维搜索算法：

>* 1、黄金分割法。（只使用目标函数$f$）
>* 2、斐波那契数列方法。（只使用目标函数$f$）
>* 3、二分法。（只使用目标函数的一阶导数${f}'$）
>* 4、割线法。（只使用目标函数的一阶导数${f}'$）
>* 5、牛顿法。（只使用目标函数的一阶导数${f}'$和二阶导数${f}''$）

## 泰勒公式

泰勒公式是许多数值方法和优化模型的基础。

假设$f: \mathbb{R} \rightarrow \mathbb{R}$在区间$[a, b]$上是$m$次连续可微函数。假设$h=[b-a]$，那么$f$的第$i$阶导数$f^{(i)}$为

$$f(b)=f(a)+\frac{h}{1 !} f^{(1)}(a)+\frac{h^{2}}{2 !} f^{(2)}(a)+\ldots+\frac{h^{m-1}}{(m-1) !} f^{(m-1)}(a)+R_{m}$$

其中$R_{m}$为余项

$$R_{m}=\frac{h^{m}(1-\theta)^{m-1}}{(m-1) !} f^{(m)}(a+\theta h)=\frac{h^{m}}{m !}\left(a+\theta^{\prime} h\right)$$

其中$\theta, \theta^{\prime} \in(0,1)$。

## 牛顿法（牛顿切线法）

牛顿法主要有两方面的应用

1、求方程根：使用一阶导数求实值函数的根。

2、优化：使用一阶和二阶导数求一个实值函数优化器的逼近。

> 1、目标：

求$x^{\ast}$，使得$g\left(x^{\ast}\right)=0$

> 2、核心思想：

基于泰勒公式展开，构建新的函数来逼近原函数。

$$f(x)=g\left(x_{0}\right)+\left(x-x_{0}\right) g^{\prime}\left(x_{0}\right)$$

由于$f(x)$是$g\left(x\right)$的近似。因此，要求的$f(x)$极小点，那么就要满足一阶必要条件，即$f(x)=0$，那么上式就可以得到

$$x=x_{0}-\frac{g\left(x_{0}\right)}{g^{\prime}\left(x_{0}\right)}$$

从而得到整个迭代过程为

$$x_{k+1}=x_{k}-\frac{g\left(x_{k}\right)}{g^{\prime}\left(x_{k}\right)}$$

> 3、几何角度

几何上来说，就是找到$x^{\left ( k \right )}$出的切线与$x$轴的交点作为$x^{\left ( k+1 \right )}$，然后再以$x^{\left ( k+1 \right )}$点的切线找到$x^{\left ( k+2 \right )}$，一次类推最终逼近极值点。

> 4、注意事项

如果$\frac{g\left(x_{k}\right)}{g^{\prime}\left(x_{k}\right)}$的比值不是足够小时，牛顿法可能就会失效，因为会错过极小值，所以初始点的选择很重要。

## 牛顿优化法

构建一个在$x^{(k)}$点的原函数、一阶函数和二界函数，并且构建一个逼近于原函数$f(x)$的近似函数

$$q(x)=f\left(x^{(k)}\right)+f^{\prime}\left(x^{(k)}\right)\left(x-x^{(k)}\right)+\frac{1}{2} f^{\prime \prime}\left(x^{(k)}\right)\left(x-x^{(k)}\right)^{2}$$

其中$q(x)$与$f(x)$的一阶和二阶导数相比配

$q\left(x^{(k)}\right)=f\left(x^{(k)}\right)$

$q^{\prime}\left(x^{(k)}\right)=f^{\prime}\left(x^{(k)}\right)$

$q^{\prime \prime}\left(x^{(k)}\right)=f^{\prime \prime}\left(x^{(k)}\right)$

原先去$f$的最小值，现在去近似函数$q$的最小值。

按照一阶必要条件（FONC）可以得到

$$q^{\prime}(x)=f^{\prime}\left(x^{(k)}\right)+f^{\prime \prime}\left(x^{(k)}\right)\left(x-x^{(k)}\right)=0$$

令$x=x^{(k+1)}$，那么就可以得到

$$x^{(k+1)}=x^{(k)}-\frac{f^{\prime}\left(x^{(k)}\right)}{f^{\prime \prime}\left(x^{(k)}\right)}$$

注意，对于区间内任何$x$都有$f^{\prime \prime}(x)>0$，那么牛顿法能正常工作，但如果对于一些$x$造成$f^{\prime \prime}(x)<0$，那么牛顿法可能就会收敛到极大点，而不是极小点。

## 割线法

如果函数二阶不可导，那么二阶导数就可以近似为

$$f^{\prime \prime}\left(x^{(k)}\right) \approx \frac{f^{\prime}\left(x^{(k)}\right)-f^{\prime}\left(x^{(k-1)}\right)}{x^{(k)}-x^{(k-1)}}$$

因此，就可以得到割线法公式

$$x^{(k+1)}=x^{(k)}-\frac{x^{(k)}-x^{(k-1)}}{f^{\prime}\left(x^{(k)}\right)-f^{\prime}\left(x^{(k-1)}\right)} f^{\prime}\left(x^{(k)}\right)$$

该方法需要有两个初始值，分别是$x^{(k)}$和$x^{(k-1)}$。

## 多维优化问题中的一维搜索

一维搜索方法在多维优化问题中扮演非常重要的角色。特别是对于多维优化问题的迭代求解算法，通常每次迭代都会包括一维搜索过程。

令目标函数$f: \mathbb{R}^{n} \rightarrow \mathbb{R}$，寻找$f$极小值的迭代算法为

$$\mathbf{x}^{(k+1)}=\mathbf{x}^{(k)}+\alpha_{k} \mathbf{d}^{(k)}$$

其中$\mathbf{x}^{(0)}$为给定的初始点，$\alpha_{k} \geq 0$为步长，其确定方式为使下面函数最小

$$\phi_{k}(\alpha)=f\left(\mathbf{x}^{(k)}+\alpha \mathbf{d}^{(k)}\right)$$

其中$\mathbf{d}$为搜索方向。通过一维搜索确定$\alpha_{k}$后，可以使得$f\left(\mathbf{x}^{(k+1)}\right) < f\left(\mathbf{x}^{(k)}\right)$。

# 梯度方法

常用的梯度方法

> 梯度下降

>* 1、固定步长的梯度下降
>* 2、最速梯度下降
>* 3、随机梯度下降

> 共轭梯度

## 介绍

因为$\mathbf{d}=\nabla f(\mathbf{x})$是增长率最大的方向，因此$\mathbf{d}=-\nabla f(\mathbf{x})$就是下降最快的方向。

梯度下降算法的核心实现：

$$\mathbf{x}^{(k+1)}=\mathbf{x}^{(k)}-\alpha_{k} \nabla f\left(\mathbf{x}^{(k)}\right)$$

其中$\alpha_{k}$是步长，$\nabla f\left(\mathbf{x}^{(k)}\right)$是方向。

1、$\alpha_{k}$选择的影响：

> 当$\alpha_{k}$太小，那么迭代的次数就会变多。

> 当$\alpha_{k}$太大，那么就会在最优点之间来回震荡。

2、$\alpha_{k}$的选择有许多方法：

> 可以在每次迭代中使$\alpha_{k}$固定，也可以让$\alpha_{k}$随迭代变化而变化。

> 动态计算$\alpha_{k}$

$$\alpha_{k}=\arg \min _{\alpha \geq 0} f\left(\mathbf{x}^{(\mathbf{k})}-\alpha \nabla f\left(\mathbf{x}^{(k)}\right)\right)$$

## 最速下降法

> 该方法时一个梯度法。

> 选择步长以实现目标函数在每个单独步骤中的最大减少量。

$$\alpha_{k}=\arg \min _{\alpha \geq 0} f\left(\mathbf{x}^{(\mathbf{k})}-\alpha \nabla f\left(\mathbf{x}^{(\mathbf{k})}\right)\right)$$

最速下降法的基本过程：x

>* 1、每一步以$\mathbf{x}^{(\mathbf{k})}$开始。
>* 2、通过线搜法沿着方向$-\nabla f\left(\mathbf{x}^{(\mathbf{k})}\right)$获取最小的$\mathbf{x}^{(\mathbf{k}+\mathbf{1})}$。

在最速下降法中，假如$\left\{\mathbf{x}^{(\mathbf{k})}\right\}_{k=0}^{\infty}$是每次迭代得到的值，那么对于每个$k$，都有$\mathbf{x}^{(\mathbf{k}+\mathbf{1})}-\mathbf{x}^{(\mathbf{k})}$与$\mathbf{x}^{(\mathbf{k}+2)}-\mathbf{x}^{(\mathbf{k}+1)}$正交。

当$\mathbf{x}^{(\mathbf{k}+\mathbf{1})}=\mathbf{x}^{(\mathbf{k})}$，那么这种情况就是算法停止的条件。

> 梯度收敛的总结

1、如果目标函数$f$是凸的，则通过满足Wolfe条件的线搜索来选择步长，则相应的梯度法是全局收敛的。

2、如果目标函数是二次型$f(\mathbf{x})=\frac{1}{2} \mathbf{x}^{T} Q \mathbf{x}-\mathbf{b}^{T} \mathbf{x}$，并且$Q$是正定的，那么最速梯度法是全局收敛。

3、如果目标函数是二次型$f(\mathbf{x})=\frac{1}{2} \mathbf{x}^{T} Q \mathbf{x}-\mathbf{b}^{T} \mathbf{x}$，并且$Q$是正定的，那么固定步长梯度法也是全局收敛，并且$0<\alpha<\frac{2}{\lambda_{\max }(Q)}$。

给定一个序列$\left\{\mathbf{x}^{(k)}\right\}$，该序列收敛到$\mathbf{x}^{\ast}$。当$\lim _{k \rightarrow \infty}\left\|\mathbf{x}^{(k)}-\mathbf{x}^{*}\right\|=0$时，那么收敛的阶数是$p$，并且$p \in \mathbb{R}$。

当$p=1$（一阶收敛），并且$\lim _{k \rightarrow \infty} \frac{\left\|\mathbf{x}^{(k+1)}-\mathbf{x}^{*}\right\|}{\left\|\mathbf{x}^{(k)}-\mathbf{x}^{*}\right\|^{p}}=1$，那么收敛是次线性的。

当$p=1$（一阶收敛），并且$\lim _{k \rightarrow \infty} \frac{\left\|\mathbf{x}^{(k+1)}-\mathbf{x}^{*}\right\|}{\left\|\mathbf{x}^{(k)}-\mathbf{x}^{*}\right\|^{p}}<1$，那么收敛是线性的。

当$p>1$，那么收敛是超线性的。

当$p=2$（二阶收敛），那么收敛是二次型的。

当$p=3$（三阶收敛），那么收敛是立方的。

> 例子

假设$x^{\left ( k \right )}=\frac{1}{k}$，并且$x^{\left ( k \right )} \rightarrow 0$，那么$\frac{\left|x^{(k+1)}\right|}{\left|x^{(k)}\right|^{p}}=\frac{\frac{1}{k+1}}{\frac{1}{k^{p}}}=\frac{k^{p}}{k+1}$

当$p > 1$时，上式趋近于$\infty$。

当$p < 1$时，上式收敛到0。

当$p = 1$时，上式收敛到1。

因此，收敛的阶数为1。

# 牛顿法

## 基本思想

1、给定一个初值点。

2、构造目标函数的二次逼近，该目标函数与该点上的第一和第二导数值相匹配。

3、最小化近似二次函数代替原来的目标函数。

4、使用近似函数的极小值作为起点，迭代地重复该过程。

## 牛顿法（没有步阶大小，或者步阶为1）

1、给定$f: \mathbb{R}^{n} \rightarrow \mathbb{R}$，并且当前迭代为$\mathbf{x}^{(k)}$。

2、通过二次型近似函数$f$求$\mathbf{x}^{(k+1)}$。

$$q(\mathbf{x})=f\left(\mathbf{x}^{(k)}\right)+\left(\mathbf{x}-\mathbf{x}^{(k)}\right)^{\top} \mathbf{g}^{(k)}+\frac{1}{2}\left(\mathbf{x}-\mathbf{x}^{(k)}\right)^{\top} \mathbf{F}\left(\mathbf{x}^{(k)}\right)\left(\mathbf{x}-\mathbf{x}^{(k)}\right)$$

3、最小化$q$来迭代下一个$\mathbf{x}^{(k+1)}$。

4、假设$\mathbf{g}^{(k)}=\nabla f\left(\mathbf{x}^{(k)}\right)$。通过一阶必要条件，可以知道$\nabla q\left(\mathbf{x}^{(k)}\right)=0$。

$$\nabla q\left(\mathbf{x}^{(k)}\right)=\mathbf{g}^{(k)}+\mathbf{F}\left(\mathbf{x}^{(k)}\right)\left(\mathbf{x}-\mathbf{x}^{(k)}\right)=0$$

5、牛顿算法

$$\mathbf{x}^{(k+1)}=\mathbf{x}^{(k)}-\mathbf{F}\left(\mathbf{x}^{(k)}\right)^{-1} \mathbf{g}^{(k)}$$

搜索方向

$$\mathbf{d}^{(k)}=-\mathbf{F}\left(\mathbf{x}^{(k)}\right)^{-1} \mathbf{g}^{(k)}=\mathbf{x}^{(k+1)}-\mathbf{x}^{(k)}$$

## Levenberg-Marquardt修正

如果黑塞矩阵$\mathbf{F}\left(\mathbf{x}^{(k)}\right)$不正定，那么搜索方向就会使下降方向$\mathbf{d}^{(k)}=-\mathbf{F}\left(\mathbf{x}^{(k)}\right)^{-1} \mathbf{g}^{(k)}$可能不会是下降方向，因此Levenberg-Marquardt修正解决了这个问题。

$$\mathbf{x}^{(k+1)}=\mathbf{x}^{(k)}-\alpha_{k}\left(\mathbf{F}\left(\mathbf{x}^{(k)}\right)+\mu_{k} \mathbf{I}\right)^{-1} \mathbf{g}^{(k)}$$

其中$\mu_{k} \geq 0$，$F$为对称矩阵。

1、$\mu_{k} \rightarrow 0$：Levenberg-Marquardt修正逐步接近牛顿法。

2、$\mu_{k} \rightarrow \infty$：Levenberg-Marquardt修正表现出步长较小时的梯度方法的特征。

实际应用中，$\mu_{k}$的初始值较小，然后逐步增加，直到出现下降特征，即$f\left ( \mathbf{x}^{k+1} \right ) < f\left ( \mathbf{x}^{k} \right )$为止。

## 牛顿法总结

1、如果起始点与最小值足够近，牛顿法的效果会很好。

2、可以合并一个步长以确保下降

3、对于二次型，一步收敛。

# 共轭方向法

## 介绍

共轭方向法的效率和计算复杂度位于最速下降法和牛顿法之间。

共轭方向法有如下特征：

1、对于$n$维二次型问题，能够在$n$步内得到结果。

2、共轭方向法的典型代表共轭梯度法不需要计算黑塞矩阵。

3、不需要存储$n \times n$矩阵，也不需要求逆。

4、比最速下降法复杂。

通常情况下比最速下降法性能更好，但不如牛顿法。迭代搜索方法效率的关键因素是每次迭代的搜索方向。对于某些函数，最佳搜索方向是Q-共轭方向。

## 共轭向量

1、给定对称举证$\mathbf{Q} \in \mathbb{R}^{n \times n}$。

2、如果两个向量$\mathbf{d}(1)$和$\mathbf{d}(2)$是Q共轭，那么$\mathbf{d}^{(1) \top} \mathbf{Q} \mathbf{d}^{(2)}=0$。

3、假设$\mathbf{Q}$是对称${n \times n}$举证，那么对于方向$\mathbf{d}^{(0)}, \mathbf{d}^{(1)}, \mathbf{d}^{(2)}, \ldots, \mathbf{d}^{(m)}$如果是Q共轭的话，那么对于所有$i \neq j$都有$\mathbf{d}^{(i) \top} \mathbf{Q} \mathbf{d}^{(j)}=0$。

4、如果$\mathbf{Q}=\mathbf{I}$，那么共轭变为正交。

## 共轭方向算法

