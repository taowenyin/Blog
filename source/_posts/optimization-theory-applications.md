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