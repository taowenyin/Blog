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

$\therefore (A \setminus B) \in \Gamma,(B \setminus A) \in \Gamma,(A \setminus B) \cup (B \setminus A) \in \Gamma$

$\therefore \left ( A \cap B^{\mathrm{C}} \right ) \cup \left ( B \cap A^{\mathrm{C}} \right )=\left ( A \cup B \right ) \cap \left ( A \cap B \right )^{\mathrm{C}} \in \Gamma$

>* 证明结合律

$\forall A,B,C \in \Gamma$

$\because \left ( A+B \right )+C=(A \setminus B) \cup (B \setminus A)+C=\left ( A \cup B \cup C \right ) \cap \left ( A \cap B \cap C \right )^{\mathrm{C}}$

$\because A+\left ( B+C \right )=A+(B \setminus C) \cup (C \setminus B)=\left ( A \cup B \cup C \right ) \cap \left ( A \cap B \cap C \right )^{\mathrm{C}}$

$\therefore \left ( A+B \right )+C=A+\left ( B+C \right )$

>* 证明有单位元

令$\varnothing=e$，且$\varnothing \in \Gamma$

$\because A+\varnothing=(A \setminus \varnothing) \cup (\varnothing \setminus A)=A$

又$\because \varnothing+A=(\varnothing \setminus A) \cup (A \setminus \varnothing)=A$

$\therefore A+\varnothing=\varnothing+A$

$\therefore \varnothing$为单位元

>* 证明有逆元

令逆元为$B$，$\forall A \in \mathbb{X}$，证明$A \circ B=B \circ A=e=\varnothing$

则$A \circ B=A+B=(A \setminus B) \cup (B \setminus A)=\varnothing$

$\therefore B=A^{\mathrm{C}}$

同理可证$B \circ A=\varnothing$

$\therefore A \circ B=B \circ A=\varnothing$

同理可证$A \ast B=A \ast A^{\mathrm{C}}=A \cap A^{\mathrm{C}}=\varnothing$

（2）$\left ( R,\ast \right )$乘法运算是半群

$\because \forall A,B \in \Gamma$

$\therefore A \ast B=A \cap B \in \Gamma$

$\because \forall A,B,C \in \Gamma$

$\therefore \left ( A \ast B \right ) \ast C=\left ( A \cap B \right ) \cap C=A \cap B \cap C$

$\therefore A \ast \left ( B \ast C \right )=A \cap \left ( B \cap C \right ) =A \cap B \cap C$

$\therefore A \ast \left ( B \ast C \right )=A \ast \left ( B \ast C \right )$

$\therefore$乘法运算是半群

（3）所有元素都是幂等元

$\because \forall A \in \Gamma$

$\therefore A \ast A=A \cap A=A$

$\therefore \Gamma$中所有元素都是幂等元

综上所述，$R$为布尔环。

## 第三题

设$V$是$n$维线性空间，$W$是$V$的一个线性真子空间。

>* 1、写出商空间$V/W$的定义（只要写出它的元素形式，加法、数乘的定义）。
>* 2、若$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}$是$W$的一组基，把它延拓为$V$的一组基$\alpha_{1}, \alpha_{2}, \cdots, \alpha_{m}, \alpha_{m+1}, \cdots, \alpha_{n}(n>m)$，试写出商空间$V/W$一组基，并验证它的线性无关性。

1、商空间$V/W$的定义

$V/W=\left \{ \alpha +W \mid \alpha \in V \right \}$

令$\alpha +W=\hat{\alpha}$，对$\forall \alpha ,\beta \in V$

有$\hat{\alpha}+\hat{\beta}=\left ( \alpha+W \right )+\left ( \beta+W \right )=\left ( \alpha+\beta \right )+W$

$k\hat{\alpha}=k\left ( \alpha +W \right )=k\alpha +W$

2、获取商空间$V/W$一组基

对$\forall \beta +W \in V/W$，有$\beta=x_{1} \alpha_{1}+x_{2} \alpha_{2}+\cdots+x_{n} \alpha_{n}$，则

\begin{equation}
    \begin{matrix}
        \beta+W=\left(x_{1} \alpha_{1}+x_{2} \alpha_{2}+\cdots+x_{n} \alpha_{n}\right)+W \\
        =\left(x_{1} \alpha_{1}+W\right)+\cdots+\left(x_{m} \alpha_{m}+W\right)+\left(x_{m+1} \alpha_{m+1}+W\right)+\cdots+\left(x_{n} \alpha_{n}+W\right) \\
        =W+\cdots+W+x_{m+1}\left(\alpha_{m+1}+W\right)+\cdots+x_{n}\left(\alpha_{n}+W\right) \\
        =x_{m+1}\left(\alpha_{m+1}+W\right)+\cdots+x_{n}\left(\alpha_{n}+W\right) \\
        =x_{m+1}\hat{\alpha_{m+1}}+\cdots+x_{n}\hat{\alpha_{n}}
    \end{matrix}
\end{equation}

因此$V/W$中任意向量都可以用$\hat{\alpha_{m+1}},\cdots ,\hat{\alpha_{n}}$线性表示。

3、验证这组基的线性无关性$k_{m+1}\hat{\alpha_{m+1}}+\cdots+k_{n}\hat{\alpha_{n}}=\hat{0}$

设$k_{m+1}\left(\alpha_{m+1}+W\right)+\cdots+k_{n}\left(\alpha_{n}+W\right)=W$

则$\left ( k_{m+1}\alpha_{m+1}+\cdots+k_{n}\alpha_{n} \right )+W=W$

$\therefore k_{m+1}\alpha_{m+1}+\cdots+k_{n}\alpha_{n} \in W$

$\therefore k_{m+1}\alpha_{m+1}+\cdots+k_{n}\alpha_{n} = -k_{1}\alpha_{1}-\cdots-k_{m}\alpha_{m}$

$\therefore k_{1}\alpha_{1}+\cdots+k_{m}\alpha_{m}+k_{m+1}\alpha_{m+1}+\cdots+k_{n}\alpha_{n}=0$

因为$\alpha_{1}, \cdots, \alpha_{n}$是一组线性无关的基

所以$k_{1}=k_{2}=\cdots=k_{n}=0$

因此$\hat{\alpha_{m+1}},\cdots,\hat{\alpha_{n}}$是$V/W$的一组线性无关基。

## 第四题

设$\left ( X, \rho \right )$是距离空间，令

\begin{equation}
    d(x, y)=\frac{\rho(x, y)}{1+\rho(x, y)}, \forall x, y \in X
\end{equation}

>* 1、求证：$\left ( X, d \right )$也是距离空间。
>* 2、若$X$中的点列$\left\{x_{n}\right\}_{n=1}^{\infty}$按距离$\rho$收敛到$x$，求证$\left\{x_{n}\right\}_{n=1}^{\infty}$也按距离$d$收敛到$x$
>* 3、若$A$是$\left ( X, d \right )$中的闭集，求证：$A$也是$\left ( X, \rho \right )$中的闭集。

1、证明$\left ( X, d \right )$也是距离空间：非负性、对称性、三角不等式

（1）非负性

$\because \rho$是距离空间

$\therefore \rho(x, y) \geq 0$

$\therefore d(x, y)=\frac{\rho(x, y)}{1+\rho(x, y)} \geq 0$

（2）对称性

$\because \rho$是距离空间

$\therefore \rho(x, y) = \rho(y, x)$

$\therefore d(x, y)=\frac{\rho(x, y)}{1+\rho(x, y)} = \frac{\rho(y, x)}{1+\rho(y, x)} = d(y, x)$

（3）三角不等式

$\because \rho$是距离空间

$\therefore \rho(x, z) \leq \rho(x, y) + \rho(y, z)$

$\therefore d(x, z)=\frac{\rho(x, z)}{1+\rho(x, z)} \leq \frac{\rho(x, y)+\rho(y, z)}{1+\rho(x, y)+\rho(y, z)}$

$\therefore \frac{\rho(x, y)+\rho(y, z)}{1+\rho(x, y)+\rho(y, z)}=\frac{\rho(x, y)}{1+\rho(x, y)+\rho(y, z)}+\frac{\rho(y, z)}{1+\rho(x, y)+\rho(y, z)}$

$\therefore \frac{\rho(x, y)}{1+\rho(x, y)+\rho(y, z)}+\frac{\rho(y, z)}{1+\rho(x, y)+\rho(y, z)} \leq \frac{\rho(x, y)}{1+\rho(x, y)}+\frac{\rho(y, z)}{1+\rho(y, z)}=d(x,y)+d(y,z)$

$\therefore d(x, z) \leq d(x, y)+d(y, z)$

因此$\left ( X, d \right )$是距离空间

2、证明$d$收敛到$x$：随着$x_{n}$的增加，距离接近于0

$\because X$中的点列$\left\{x_{n}\right\}_{n=1}^{\infty}$按距离$\rho$收敛到$x$

$\therefore \forall \varepsilon > 0,\exists N$时，当$n>N$是$\rho \left ( x_{n},x \right ) < \varepsilon$

$\therefore \forall \varepsilon > 0,\exists N$时，当$n>N$是$d \left ( x_{n},x \right ) < \frac{\varepsilon}{1+\varepsilon}$

$\therefore \frac{\varepsilon}{1+\varepsilon}$任意小，并且$\frac{\varepsilon}{1+\varepsilon}>0$

$\therefore \left\{x_{n}\right\}_{n=1}^{\infty}$也按距离$d$收敛到$x$

3、$A$也是$\left ( X, \rho \right )$中的闭集

$\because A$是$\left ( X, d \right )$中的闭集的充要条件是$A$中任意一个收敛点列必收敛于$A$中的一点。

$\therefore X$中点列$\left\{x_{n}\right\}_{n=1}^{\infty}$按距离$d$收敛到$X$中的一个点$x_{0}$。

有第2问结果知道点列$\left\{x_{n}\right\}_{n=1}^{\infty}$在距离$\rho$也收敛于$x_{0}$，并且$x_{0} \in X$。

$\therefore A$是$\left ( X, \rho \right )$中的闭集。

## 第五题

设$V$是数域$P$上的赋范线性空间，证明：

>* 1、$V$中的收敛点列$\left\{x_{n}\right\}$的极限是唯一的。
>* 2、$V$中收敛序列$\left\{x_{n}\right\}$必有界，即存在正数$M$，使得$\left\|x_{n}\right\| \leq M(n=1,2, \cdots)$;
>* 3、如果$V$中序列$\left\{x_{n}\right\}$收敛到$x \in V$，则$\left\{x_{n}\right\}$的任意一个子序列{$\left\{x_{n_{k}}\right\}$}也收敛到$x$；
>* 4、如果$V$中序列$\left\{x_{n}\right\}$，$\left\{y_{n}\right\}$分别收敛到$x,y \in V$，则对任意$a,b \in P$，有

\begin{equation}
    \underset{n \rightarrow \infty}{lim}\left(a x_{n}+b y_{n}\right)=ax+by
\end{equation}

1、证明$\left\{x_{n}\right\}$极限的唯一性

设$x,\tilde{x}$都是$x_{n}$的极限，则得到$0\leq \|x+\tilde{x} \| \leq \|x+x_{n}\|+\|x_{n}+\tilde{x}\|$

$\therefore$当$n \rightarrow \infty$时，$\|x+x_{n} \| \rightarrow 0,\|x_{n}+\tilde{x}\| \rightarrow 0$

$\therefore \|x+\tilde{x} \|=0$

$\therefore x=\tilde{x}$

2、证明$\left\{x_{n}\right\}$必有界

由$x_{n} \rightarrow x \left ( n \rightarrow \infty \right )$知，存在正数$K$，使得当$k > K$时，$d\left ( x_{k},x \right )< 1$

\begin{equation}
    r=max\left \{ d\left ( x_{1},x \right ),\cdots ,d\left ( x_{K-1},x \right ),1 \right \}
\end{equation}

则$\left\{x_{n}\right\}\subseteq U\left ( x,r \right )$

3、证明任意一个子序列{$\left\{x_{n_{k}}\right\}$}也收敛到$x$

$\because x_{n} \rightarrow x$

$\therefore \exists N$，使得$n > N$时有$\left \| x_{n}-x \right \| < \varepsilon$

$\because x_{n_{k}}$是$x_{n}$的子列

$\therefore \exists k$，使得$k>K$时，有$n_{k}>N$

4、证明$\underset{n \rightarrow \infty}{lim} \left(a x_{n}+b y_{n}\right)=ax+by$

$\because \left \|\left(a x_{n}+b y_{n}\right)-\left ( ax+by\right ) \right \|=\left \| a\left ( x_{n}-x\right )+b\left ( y_{n}-y\right )\right \|$

$\therefore \left \| a\left ( x_{n}-x\right )+b\left ( y_{n}-y\right )\right \| \leq \left \| a\left ( x_{n}-x\right )\right \|+\left \| b\left ( y_{n}-y\right )\right \|$

$\because \left \| a\left ( x_{n}-x\right )\right \|+\left \| b\left ( y_{n}-y\right )\right \|=\left | a\right |\left \| x_{n}-x\right \|+\left | b\right |\left \| y_{n}-y\right \|$

$\therefore \left \|\left(a x_{n}+b y_{n}\right)-\left ( ax+by\right ) \right \| \leq \left | a\right |\left \| x_{n}-x\right \|+\left | b\right |\left \| y_{n}-y\right \|$

又$\because x_{n}$收敛到$x$，并且$y_{n}$也收敛到$y$

$\therefore \underset{n \rightarrow \infty}{lim} \left(a x_{n}+b y_{n}\right)=ax+by$

## 第六题

证$U_{n}$是循环群，$U_{n}=span\left ( e^{i\frac{2\pi k}{n}} \right )\left ( i=0,1,\cdots,n-1 \right )$

先证群，再证循环群

设$\varepsilon ^{k}=e^{\frac{i \cdot 2\pi \cdot k}{n}}$

当$k=n$时，$\varepsilon ^{n}=e^{i \cdot 2\pi}=1,\varepsilon ^{0}=e^{0}=1$

$\forall \varepsilon ^{a},\varepsilon ^{a},0\leq a,b\leq n-1$

>* 证明运算封闭

$\because \varepsilon ^{a}\cdot \varepsilon ^{b}=e^{i\frac{2\pi a}{n}}\cdot e^{i\frac{2\pi b}{n}}=e^{i\frac{2\pi \left ( a+b \right )}{n}}$

$\therefore \varepsilon ^{a}\cdot \varepsilon ^{b} \in U_{n}$

>* 证明结合律

$\because \left ( \varepsilon ^{a}\cdot \varepsilon ^{b} \right )\cdot \varepsilon ^{c}=e^{i\frac{2\pi \left ( a+b \right )}{n}}\cdot e^{i\frac{2\pi c}{n}}=e^{i\frac{2\pi \left ( a+b+c \right )}{n}}$

$\because \varepsilon ^{a} \cdot \left ( \varepsilon ^{b}\cdot \varepsilon ^{c} \right )=e^{i\frac{2\pi a}{n}} \cdot e^{i\frac{2\pi \left ( b+c \right )}{n}}=e^{i\frac{2\pi \left ( a+b+c \right )}{n}}$

$\therefore \left ( \varepsilon ^{a}\cdot \varepsilon ^{b} \right )\cdot \varepsilon ^{c}=\varepsilon ^{a} \cdot \left ( \varepsilon ^{b}\cdot \varepsilon ^{c} \right )$

>* 证明有单位元

$\because \varepsilon ^{0}=e ^{0}=1$

并且$\because \varepsilon ^{k}\cdot \varepsilon ^{0}=\varepsilon ^{k}$

$\therefore \varepsilon ^{0}$为单位元

>* 证明有逆元

$\forall \varepsilon ^{k} \in U_{n}$

则$\exists \varepsilon ^{n-k}$使得

$\varepsilon ^{k} \cdot \varepsilon ^{n-k}=\varepsilon ^{n}=1=\varepsilon ^{0}$

$\varepsilon ^{n-k} \cdot \varepsilon ^{k}=\varepsilon ^{0}$

综上所述$U_{n}$是群

$\because \varepsilon ^{k}=e^{\frac{i \cdot 2\pi \cdot k}{n}}$，即生成元是$e^{\frac{i \cdot 2\pi}{n}}$

$\therefore U_{n}=span\left ( e^{i\frac{2\pi k}{n}} \right )$是循环群