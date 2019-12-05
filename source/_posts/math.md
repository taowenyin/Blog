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

>* 1、在$Z$上定义运算：$a \circ b = a+b+4$，求证$\mathbb{Z}$按$\circ$运算作为一个群。
>* 2、如果在$Z$上定义运算$a \odot b = a^{k}$，说明$\mathbb{Z}$在$\odot$运算上不是一个群。

（注：题目中的“$+$”以及“$a^{k}$”都是指通常意义下数的加法和乘方）

1、证明群：运算封闭、结合律、有单位元、有逆元。

>* 证明运算封闭

$\because \forall a, b \in z$

$\therefore a \circ b=a+b+4 \in z$

>* 证明结合律

$\forall a, b, c \in z$

$\because (a \cdot b) \cdot c=(a+b+4) \cdot c=a+b+4+c+4=a+b+c+8$

$a \cdot (b \cdot c)=a \cdot (b+c+4)=a+b+c+4+4=a+b+c+8$

$\therefore (a \cdot b) \cdot c=a \cdot (b \cdot c)$

>* 证明有单位元

令单位元为$e$，$\forall a \in \mathbb{Z}$，证明$a \circ e=e \circ a=a$

则$a \circ e=a+e+4=a$，所以$e=-4$

$\because e \circ a=-4+a+4=a$

$\therefore a \circ e=e \circ a=a$

>* 证明有逆元

令逆元为$b$，$\forall a \in \mathbb{Z}$，证明$a \circ b=b \circ a=e$

则$a \circ b=a+b+4=e=-4$，所以$b=-a-8$

$\because b \circ a=b+a+4=-a-8+a+4=-4$

$\because a \circ b$在$\mathbb{Z}$上构成群。

2、证明群：运算封闭、结合律、有单位元、有逆元只要有一个不符合就不是群。

令$a=2,b=-2$，那么$a \odot b=a^{b}=2^{-2}=\frac{1}{4} \notin \mathbb{Z}$

所以$a \odot b = a^{k}$在$\mathbb{Z}$不是一个群

## 第二题

设$R$是一个环，若$\forall a \in R$，都有$a^{2} = a$，则称R为布尔环

>* 1、求证：若$R$是布尔环，则$\forall a \in R$，$a+a=0$。
>* 2、设$X$是一个集合，$\Gamma$表示由$X$的全部子集作为元素得到的集合，定义$\Gamma$中的两种运算，$A+B=(A \setminus B) \cup (B \setminus A)$和$A \ast B = A \cap B$，求证$\Gamma$在这两个运算下构成一个环，并且是布尔环。（注：“$\setminus$”表示两个集合做差集）

1、$\forall a,b \in R$，则有$a+b \in R$

$\because R$是一个布尔环

$\therefore (a+b)^{2}=a+b$

又$\because (a+b)^{2}=a^{2}+ab+ba+b^{2}$，并且$R$是布尔环

$\therefore a^{2}=a,b^{2}=b$

$\therefore (a+b)^{2}=a^{2}+ab+ba+b^{2}=a+ab+ba+b=a+b$

$\therefore ab+ba=0$

$\therefore ab=-ba$

令$b=a$，则$a^{2}=-a^{2}$

$\therefore 2a^{2}=0$

$\therefore 2a=0$

$\therefore a+a=0$

2、证明布尔环：加法是交换群、乘法是半群、所有元素都是幂等元。

（1）$\left ( R,+ \right )$加法运算是交换群

>* 证明运算封闭

$\because \forall A,B \in \Gamma$

$\therefore (A \setminus B) \in \Gamma,(B \setminus A) \in \Gamma$

>* 证明结合律

>* 证明有单位元

>* 证明有逆元

## 第三题

设$V$是$n$维线性空间，$W$是$V$的一个线性真子空间。

>* 1、写出商空间$V/W$的定义（只要写出它的元素形式，加法、数乘的定义）。
>* 2、若$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}$是$W$的一组基，把它延拓为$V$的一组基$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}, \alpha_{m+1}, \cdots, \alpha_{n}(n>m)$，试写出商空间$V/W$一组基，并验证它的线性无关性。

## 第四题

设$\left ( X, \rho \right )$是距离空间，令

\begin{equation}
    d(x, y)=\frac{\rho(x, y)}{1+\rho(x, y)}, \forall x, y \in X
\end{equation}

>* 1、求证：$\left ( X, d \right )$也是距离空间。
>* 2、若$X$中的点列$\left\{x_{n}\right\}_{n=1}^{\infty}$按距离$\rho$收敛到$x$，求证$\left\{x_{n}\right\}_{n=1}^{\infty}$也按距离$d$收敛到$x$
>* 3、若$A$是$\left ( X, d \right )$中的闭集，求证：$A$也是$\left ( X, \rho \right )$中的闭集。

## 第五题

设$V$是数域$P$上的赋范线性空间，证明：

>* 1、$V$中的收敛点列$\left\{x_{n}\right\}$的极限是唯一的。
>* 2、$V$中收敛序列$\left\{x_{n}\right\}$必有界，即存在正数$M$，使得$\left\|x_{u}\right\| \leq M(n=1,2, \cdots)$;
>* 3、如果$V$中序列$\left\{x_{n}\right\}$收敛到$x \in V$，则$\left\{x_{n}\right\}$的任意一个子序列{$\left\{x_{n_{k}}\right\}$}也收敛到$x$；
>* 4、如果$V$中序列$\left\{x_{n}\right\}$，$\left\{y_{n}\right\}$分别收敛到$x,y \in V$，则对任意$a,b \in P$，有

\begin{equation}
    \lim_{n \rightarrow \infty}\left(a x_{n}+b y_{n}\right)=ax+by
\end{equation}