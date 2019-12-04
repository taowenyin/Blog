---
title: 高等工程应用数学
mathjax: true
date: 2019-12-04 22:44:11
updated: {{ date }}
categories: 
- 课程
---

# 考试题目

## 第一题

设$Z$表示整数集

>* （1）在$Z$上定义运算：$a \circ b = a+b+4$，求证$\mathbb{Z}$按$\circ$运算作为一个群。
>* （2）如果在$Z$上定义运算$a \odot b = a^k$，说明$\mathbb{Z}$运算不是一个群。

（注：题目中的“+”以及“a^k”都是指通常意义下数的加法和乘方）

## 第二题

设$R$是一个环，若$\forall a \in R$，都有$a^{2} = a$，则称R为布尔环

>* （1）求证：若$R$是布尔环，则$\forall a \in R$，$a+a=0$。
>* （2）设$X$是一个集合，$\Gamma$表示由$X$的全部子集作为元素得到的集合，定义$\Gamma$中的两种运算，$A+B=(A \setminus B) \cup (B \setminus A)$和$A \ast B = A \cap B$，求证$\Gamma$在这两个运算下构成一个环，并且是布尔环。（注：“\”表示两个集合做差集）

## 第三题

设$V$是$n$维线性空间，$W$是$V$的一个线性真子空间。

>* （1）写出商空间$V/W$的定义（只要写出它的元素形式，加法、数乘的定义）。
>* （2）若$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}$是$W$的一组基，把它延拓为$V$的一组基$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}, \alpha_{m+1}, \cdots, \alpha_{n}(n>m)$，试写出商空间$V/W$一组基，并验证它的线性无关性。

## 第四题

设$\left ( X, \rho \right )$是距离空间，令

\begin{equation}
    d(x, y)=\frac{\rho(x, y)}{1+\rho(x, y)}, \forall x, y \in X
\end{equation}

>* （1）求证：$\left ( X, d \right )$也是距离空间。
>* （2）若$X$中的点列$\left\{x_{n}\right\}_{n=1}^{\infty}$按距离$\rho$收敛到$x$，求证$\left\{x_{n}\right\}_{n=1}^{\infty}$也按距离$d$收敛到$x$
>* （3）若$A$是$\left ( X, d \right )$中的闭集，求证：$A$也是$\left ( X, \rho \right )$中的闭集。

## 第五题

设$V$是数域$P$上的赋范线性空间，证明：

>* （1）$V$中的收敛点列$\left\{x_{n}\right\}$的极限是唯一的。
>* （2）$V$中收敛序列$\left\{x_{n}\right\}$必有界，即存在正数$M$，使得$\left\|x_{u}\right\| \leq M(n=1,2, \cdots)$;
>* （3）如果$V$中序列$\left\{x_{n}\right\}$收敛到$x \in V$，则$\left\{x_{n}\right\}$的任意一个子序列{$\left\{x_{n_{k}}\right\}$}也收敛到$x$；
>* （4）如果$V$中序列$\left\{x_{n}\right\}$，$\left\{y_{n}\right\}$分别收敛到$x,y \in V$，则对任意$a,b \in P$，有

\begin{equation}
    \lim_{n \rightarrow \infty}\left(a x_{n}+b y_{n}\right)=ax+by
\end{equation}