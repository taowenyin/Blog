---
title: 优化理论和优化
mathjax: true
date: 2020-01-01 19:25:21
updated: {{ date }}
categories: 
- 课程
---

# 基础知识

## 线性相关

向量集$\left \{ \mathbf{a_{1}},\mathbf{a_{2}},\cdots ,\mathbf{a_{k}} \right \}$是线性相关，当且仅当集合中的一个向量可以表示为其他向量的线性组合。

必要条件：$\alpha_{1}\mathbf{a_{1}}+\alpha_{2}\mathbf{a_{2}}+\cdots +\alpha_{k}\mathbf{a_{k}}=\mathbf{0}$

充分条件：$\left ( -1 \right )\mathbf{a_{1}}+\alpha_{2}\mathbf{a_{2}}+\cdots +\alpha_{k}\mathbf{a_{k}}=\mathbf{0}$

## 二次型函数

$$f\left ( \mathbf{x} \right )=\mathbf{x}^{\top}\mathbf{Q}\mathbf{x}$$

对于任意非零向量$\mathbf{x}$，都有$\mathbf{x}^{\top}\mathbf{Q}\mathbf{x} > 0$，那么$\mathbf{Q}$是正定的。如果$\mathbf{x}^{\top}\mathbf{Q}\mathbf{x} \geq 0$，那么$\mathbf{Q}$是半正定的。相反，还是负定和半负定。

## 矩阵范数

$$\left \| \mathbf{A} \right \|_{F}=\left ( \sum_{i=1}^{m}\sum_{j=1}^{n}\left ( a_{ij} \right )^{2} \right )^{\frac{1}{2}}$$

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

