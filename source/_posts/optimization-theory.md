---
title: 最优化理论和应用
mathjax: true
date: 2019-12-02 15:30:44
updated: {{ date }}
categories: 
- 课程
---

# 向量空间与矩阵

## 向量与矩阵

### 线性相关

下式中，只有当所有系数$\alpha_{i}(i=1,2,\cdots,k)$都等于零的前提下才成立，那么就称向量机${\mathbf{a_{1}},\mathbf{a_{2}},\cdots,\mathbf{a_{k}}}$是线性无关的，否则只要有一个$\mathbf{a_{k}}$不等于零，那么就是线性相关。

\begin{equation}
    \alpha_{1} \mathbf{a_{1}}+\alpha_{2} \mathbf{a_{2}}+\cdots+\alpha_{k} \mathbf{a_{k}}=\mathbf{0}
\end{equation}

对于所有包含$\mathbf{0}$向量的集合都是线性相关。

给定了向量$\mathbf{a}$，如果存在标量$a_{1}, a_{2}, \cdots, a_{k}$，使得

\begin{equation}
    \mathbf{a}=\alpha_{1} \mathbf{a_{1}}+\alpha_{2} \mathbf{a_{2}}+\cdots+\alpha_{k} \mathbf{a_{k}}
\end{equation}

那么就称向量$\mathbf{a}$是${\mathbf{a_{1}},\mathbf{a_{2}},\cdots,\mathbf{a_{k}}}$的线性组合。

### 生成子空间

假定$\mathbf{a_{1}},\mathbf{a_{2}},\cdots,\mathbf{a_{k}}$是$\mathbb{R}^n$中的任意向量，那么它们所有线性组合的集合称为生成子空间，记为:

\begin{equation}
    span\left [ \mathbf{a_{1}},\mathbf{a_{2}},\cdots,\mathbf{a_{k}} \right ]=\left \{ \sum_{i=1}^{k}\alpha_{i}\mathbf{a_{i}}:\alpha_{1},\cdots,\alpha_{k},\alpha_{k}\in\mathbb{R} \right \}
\end{equation}

给定子空间$\mathcal{V}$，如果存在**线性无关**的向量集合$\left\{\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}}\right\}\subset \mathcal{V}$，使得$\mathcal{V}=span\left [\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}} \right]$，那么就称$\left \{ \mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}} \right \}$是子空间$\mathcal{V}$的一组基，并且$\mathcal{V}$中所有基都具有相同数量的向量，该数量称为$\mathcal{V}$的维数，记为$dim\mathcal{V}$。

如果$\left |\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}}\right |$是$\mathcal{V}$的基，那么$\mathcal{V}$中任意的向量$\mathbf{a}$都可以通过下式进行表示，其中$\mathbf{a_{i}} \in \mathbb{R},\ i=1,2,\cdots,k$。

\begin{equation}
    \mathbf{a}=\alpha_{1}\mathbf{a_{1}}+\alpha_{2}\mathbf{a_{2}}+\cdots+\alpha_{k}\mathbf{a_{k}}
\end{equation}

## 矩阵的秩

\begin{equation}
    \mathbf{A} = 
        \begin{bmatrix}
            a_{11} & a_{12} & \cdots & a_{1n}\\ 
            a_{21} & a_{22} & \cdots & a_{2n}\\ 
            \vdots & \cdots & \ddots & \vdots\\ 
            a_{m1} & a_{m2} & \cdots & a_{mn}
        \end{bmatrix}
\end{equation}

$\mathbf{A}$的第$k$列用$\mathbf{a_{k}}$表示

\begin{equation}
    \mathbf{a_{k}} = 
        \begin{bmatrix}
            a_{1k}\\ 
            a_{2k}\\ 
            \vdots\\ 
            a_{mk}
        \end{bmatrix}
\end{equation}

矩阵$\mathbf{A}$中线性无关列的最大数目称为$\mathbf{A}$的**秩**，记为$rank\mathbf{A}$，并且$rank\mathbf{A}$就是$span\left[\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}}\right]$的维数。

给定$m\times n$矩阵$\mathbf{A}$，那么$p$**阶子式**是一个$p\times p$矩阵的行列式，该$p\times p$矩阵由矩阵$\mathbf{A}$减去$m-p$行和$n-p$列得到，并且$p\leq min\left \{ m,n \right \}$。同时，如果$m\times n\left ( m \geq n \right )$矩阵$\mathbf{A}$具有$n$阶子式，那么$rank\mathbf{A}=n$。

假定$\mathbf{A}$是$n \times n$的方阵，如果存在$n \times n$方阵$\mathbf{B}$，使得$\mathbf{A}\mathbf{B}=\mathbf{B}\mathbf{A}=\mathbf{I}_{n}$，其中$\mathbf{I}_{n}$为$n \times n$的单位矩阵，$\mathbf{B}$为$\mathbf{A}$的逆矩阵，记为$\mathbf{B}=\mathbf{A}^{-1}$

## 线性方程

假设方程写成矩阵形式

\begin{equation}
    \mathbf{A}\mathbf{x}=\mathbf{b}
\end{equation}

其中$\mathbf{A}$为系数矩阵，$\left [ \mathbf{A},\mathbf{b} \right ]$为增广矩阵，那么方程$\mathbf{A}\mathbf{x}=\mathbf{b}$要有解，那么$rank\mathbf{A}=rank\left [ \mathbf{A},\mathbf{b} \right ]$。

如果$\mathbf{A} \in \mathbb{R}^{m \times n}$，且$rank\mathbf{A}=m$，那么只要$n-m$个未知数就可以求解$\mathbf{A}\mathbf{x}=\mathbf{b}$。

## 内积和范数

对于$\mathbf{x},\mathbf{y} \in \mathbb{R}^{n}$，其欧式内积为：

\begin{equation}
    \left \langle \mathbf{x},\mathbf{y} \right \rangle=\sum_{i=1}^{n}x_{i}y_{i}=\mathbf{x}^{\top}\mathbf{y}
\end{equation}

如果向量$\mathbf{x}$和$\mathbf{y}$，使得$\left \langle \mathbf{x},\mathbf{y} \right \rangle=0$，那么$\mathbf{x}$和$\mathbf{y}$**正交**。

向量$\mathbf{x}$的欧式范数为$\left \| \mathbf{x} \right \|=\sqrt{\left \langle \mathbf{x},\mathbf{x} \right \rangle}=\sqrt{\mathbf{x}^{\top}\mathbf{x}}$。

**柯西-施瓦茨不等式：**$\left | \left \langle \mathbf{x},\mathbf{y} \right \rangle \right |=\left \| \mathbf{x} \right \|\left \| \mathbf{y} \right \|$

$\mathbb{R}^{n}$**中的毕达哥拉斯定理：**如果$\mathbf{x}$和$\mathbf{y}$正交，则$\left \langle \mathbf{x},\mathbf{y} \right \rangle=0$，就可以得到$\left \| \mathbf{x}+\mathbf{y} \right \|^{2}=\left \| \mathbf{x} \right \|^{2} + 2\left \langle \mathbf{x},\mathbf{y} \right \rangle + \left \| \mathbf{y} \right \|^{2}=\left \| \mathbf{x} \right \|^{2}+\left \| \mathbf{y} \right \|^{2}$。

# 变换

## 线性变换

**相似矩阵：**给定两个$n \times n$的矩阵$\mathbf{A}$和$\mathbf{B}$，如果存在一个非奇异矩阵（可逆）$\mathbf{T}$，使得$\mathbf{A}=\mathbf{T}^{-1}\mathbf{B}\mathbf{T}$，那么称阵$\mathbf{A}$和$\mathbf{B}$相似，在不同的基下，相似矩阵对应的线性变换相同。

## 特征值与特征向量

特征值：令$\mathbf{A}$为$n \times n$的方阵，如果存在$\lambda$和非零向量$\mathbf{v}$满足等式$\mathbf{A}\mathbf{v}=\lambda\mathbf{v}$，那么称$\lambda$为特征值，$\mathbf{v}$为特征向量。$\lambda$为$\mathbf{A}$的充要条件为矩阵$\lambda\mathbf{I}-\mathbf{A}$是**奇异矩阵，**即多项式$\textbf{det}\left [ \lambda\mathbf{I}-\mathbf{A} \right ]=0$，其中$\mathbf{I}$是$n \times n$的单位矩阵，多项式为特征多项式，下面的方程为特征方程。

\begin{equation}
    \textbf{det}[\lambda \mathbf{I}-\mathbf{A}]=\lambda^{n}+a_{n-1} \lambda^{n-1}+\cdots+a_{1} \lambda+a_{0}=0
\end{equation}

1、假定方程$\textbf{det}\left [ \lambda\mathbf{I}-\mathbf{A} \right ]=0$存在$n$个相异的根$\lambda_{1},\lambda_{2},\cdots,\lambda_{n}$，那么就存在$n$个线性无关的向量$\mathbf{\mathcal{v}_{1}},\mathbf{\mathcal{v}_{2}},\cdots,\mathbf{\mathcal{v}_{n}}$。当矩阵$\mathbf{A}=\mathbf{A}^{\top}$，则称$\mathbf{A}$为对称矩阵。

2、对于$n \times n$实数对称矩阵，其$n$个特征向量是相互正交的。

## 正投影

## 矩阵范数

矩阵$\mathbf{A}$的范数记为$\left \| \mathbf{A} \right \|$，是满足以下条件的任意函数$\left \| \cdot \right \|$

1、如果$\mathbf{A} \neq \mathbf{0}$，那么就有$\left \| \mathbf{A} \right \| > 0$，$\left \| \mathbf{0} \right \| = 0$。

2、对于任意$c \in \mathbb{R}$，就有$\left \| c\mathbf{A} \right \|=\left | c \right |\left \| \mathbf{A} \right \|$。

3、$\left \| \mathbf{A}+\mathbf{B} \right \| \leq \left \| \mathbf{A} \right \|+\left \| \mathbf{B} \right \|$

4、$\left \| \mathbf{A} \mathbf{B} \right \| \leq \left \| \mathbf{A} \right \|\left \| \mathbf{B} \right \|$

令

\begin{equation}
    \left \| \mathbf{x} \right \|=\sqrt{\left(\sum_{k=1}^{n}\left|x_{k}\right|^{2}\right)}=\sqrt{\langle\mathbf{x}, \mathbf{x}\rangle}
\end{equation}

则由该向量函数导出矩阵范数为

\begin{equation}
    \left \| \mathbf{A} \right \|=\sqrt{\lambda_{1}}
\end{equation}

其中，$\lambda_{1}$是矩阵$\mathbf{A}^{\top}\mathbf{A}$的最大特征矩阵。

**瑞利不等式：**如果$n \times n$矩阵$\mathbf{P}$是一个实数对称正定矩阵，则有

\begin{equation}
    \lambda_{\min }(\mathbf{P})\|\mathbf{x}\|^{2} \leq \mathbf{x}^{\top} \mathbf{P} \mathbf{x} \leq \lambda_{\max }(\mathbf{P})\|\mathbf{x}\|^{2}
\end{equation}

其中，$\lambda_{\min }(\mathbf{P})$表示$\mathbf{P}$的最小特征值，$\lambda_{\max }(\mathbf{P})$表示$\mathbf{P}$的最大特征值。

# 有关几何概念

## 线段

$\mathbf{x}$和$\mathbf{y}$是空间$\mathbb{R}^{n}$中的两个点，$\mathbf{z}$是两点之间连线上的点，那么就可以得到

\begin{equation}
    \mathbf{z}-\mathbf{y}=\alpha \left ( \mathbf{x}-\mathbf{y} \right )
\end{equation}

其中$\alpha$是区间$\left [ 0,1 \right ]$之间的实数，因此上式可写为

\begin{equation}
    \mathbf{z}=\alpha \mathbf{x}+\left ( 1-\alpha  \right )\mathbf{y}
\end{equation}

并且表示为

\begin{equation}
    \left \{ \alpha \mathbf{x}+\left ( 1-\alpha  \right )\mathbf{y}:\alpha \in \left [ 0,1 \right ] \right \}
\end{equation}

## 超平面和线性簇

令$u_{1}, u_{2}, \dots, u_{n}, v \in \mathbb{R}$，其中至少存在一个$u_{i}$不为零，由所有满足线性方程

\begin{equation}
    u_{1} x_{1}+u_{2} x_{2}+\cdots+u_{n} x_{n}=v
\end{equation}

的点$\mathbf{x}=\left[x_{1}, x_{2}, \cdots, x_{n}\right]^{\top}$组成的集合成为空间$\mathbb{R}^{n}$的超平面，可写为

\begin{equation}
    \left\{\mathbf{x} \in \mathbb{R}^{n}: \mathbf{u}^{\top} \mathbf{x}=v\right\}
\end{equation}

当$n$为2时，即二维空间时，超平面为一条直线方程$u_{1} x_{1}+u_{2} x_{2}=v$，当$n$为3时，即三维空间时，超平面为一个面。

## 凸集

已知两点$\mathbf{u},\mathbf{v} \in \mathbb{R}^{n}$之间的线段可表示为集合

\begin{equation}
    \left \{ \mathbf{w}=\mathbb{R}^{n}: \mathbf{w}=\alpha \mathbf{u}+(1-\alpha) \mathbf{v}, \alpha \in[0,1] \right \}
\end{equation}

如果点$\mathbf{w}=\alpha \mathbf{u}+(1-\alpha) \mathbf{v}\left ( \alpha \in \left [ 0,1 \right ] \right )$称为点$\mathbf{u}$和点$\mathbf{v}$的凸组合。如果$\mathbf{u},\mathbf{v} \in \Theta$，并且$\mathbf{u}$和$\mathbf{v}$之间的线段都在$\Theta$内，那么$\Theta \in \mathbb{R}^{n}$为凸集。

凸集的性质：

1、如果$\Theta$是一个凸集，且$\beta$是一个实数，那么$\beta \Theta=\{\mathbf{x}: \mathbf{x}=\beta \mathbf{v}, \mathbf{v} \in \Theta\}$也是凸集。

2、如果$\Theta_{1}$和$\Theta_{2}$都是凸集，那么集合$\Theta_{1}+\Theta_{2}=\left\{\mathbf{x}: \mathbf{x}=\mathbf{v}_{1}+\mathbf{v}_{2}, \mathbf{v}_{1} \in \Theta_{1}, \mathbf{v}_{2} \in \Theta_{2}\right\}$也是凸集。

3、任意多个凸集的交集都是凸集。