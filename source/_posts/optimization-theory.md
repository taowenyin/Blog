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

下式中，只有当所有系数$\alpha_{i}(i=1,2,\cdots,k)$都等于零的前提下才成立，那么就称向量机${\mathbf{a_{1}},\mathbf{a_{2}},\cdots,\mathbf{a_{k}}}$是线性无关的，否则就是线性相关。

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

给定子空间$\mathcal{V}$，如果存在线性无关的向量集合$\left\{\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}}\right\}\subset \mathcal{V}$，使得$\mathcal{V}=span\left [\mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}} \right]$，那么就称$\left \{ \mathbf{a_{1}}, \mathbf{a_{2}}, \cdots, \mathbf{a_{k}} \right \}$是子空间$\mathcal{V}$的一组基，并且$\mathcal{V}$中所有基都具有相同数量的向量，该数量称为$\mathcal{V}$的维数，记为$dim\mathcal{V}$。