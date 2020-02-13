---
title: 【2013】A Review on Multi-Label Learning Algorithms
mathjax: true
date: 2019-11-24 07:26:13
updated: {{ date }}
tags: [机器学习, 多标签分类]
categories: [论文, 多标签学习]
---

# 介绍

在传统的单标签监督学习中，$x$表示一个实体空间，$y$表示一个标签空间，而传统的监督学习就是通过训练集

\begin{align}
    \left \{ \left ( x_{i} | y_{i} \right ) | 1 \leq i \leq m \right \}
\end{align}

练一个方法使得$f:\mathcal{X} \rightarrow \mathcal{Y}$，并且$x_{i} \in \mathcal{X}$表示一个特征属性的对象，而$y_{i} \in \mathcal{Y}$这事该特征对应的语义标签。

与传统的单标签的监督学习相比，多标签学习中每一个对象都是单独的一个实体，并且这个对象具有一组标签集，而不是一个标签。因此，多标签学习的任务就是构造能够预测实体所具有的标签集的方法。早期的多标签学习主要应用于文本分类，然而在过去十年间，多标签学习已经被广泛的应用于图像、生物信息等领域。

这篇文章对多标签学习进行了回顾，主要分为三个部分：

1、多标签学习的原理（包括学习框架、关键挑战、阈值校准）和评价指标（包括基于实例的评价、基于标签的评价、理论结果）

2、对八种最具代表性的算法进行总结和讨论

3、简要总结了几种相关的学习设定

# 范式

## 形式定义

变量及意义

| 符号 | 数学意义 |
| - | :---- |
| $\mathcal{X}$ | 表示$d$维的实体空间$\mathbb{R}^{d}$（或$\mathbb{Z}^{d}$） |
| $\mathcal{Y}$ | 表示带有$q$个可能标签的标签空间$\left \{ y_{1}, y_{2}, \cdots , y_{q} \right \}$ |
| $x$ | 表示具有$d$维的特征向量实体$x$，$\left ( x_{1}, x_{2}, \cdots , x_{d} \right )^{T} \left ( x \in \mathcal{X} \right )$ |
| $Y$ | 表示与某个实体$x$的相关标签集$\left ( Y \in \mathcal{Y} \right )$ |
| $\bar{Y}$ | 表示在$\mathcal{Y}$中$Y$的补集 |
| $\mathcal{D}$ | 表示多标签训练集$\left \{ \left ( x_{i}, Y_{i} \right ) \mid 1 \leq i \leq m \right \}$ |
| $\mathcal{S}$ | 表示多标签测试集$\left \{ \left ( x_{i}, Y_{i} \right ) \mid 1 \leq i \leq p \right \}$ |
| $f\left ( \cdot , \cdot \right )$ | 表示一个实值函数$f: \mathcal{X} \times \mathcal{Y} \rightarrow \mathbb{R}$, $f\left ( x, y \right )$返回$x$每个可能标签$y$的置信度。简单说，当与$x$相关的标签${y}' \in Y$时，那么置信度函数$f$返回最大值，当与$x$不相关的标签${y}'' \notin Y$时，那么置信度函数$f$返回最小值，即$f\left ( x,{y}' \right ) > f\left ( x, {y}'' \right )$ |
| $t\left ( \cdot  \right )$ | 表示阈值函数$t: \mathcal{X} \rightarrow \mathbb{R}$, 即该函数能够把$\mathcal{X}$映射到$\mathbb{R}$上，并且对于分类器$h\left ( \cdot  \right )$来说只有当实值函数$f\left ( \cdot , \cdot \right )$的值大于阈值函数$t\left ( \cdot  \right )$时，分类器$h\left ( \cdot  \right )$才能返回可能的标签集，数学表达为： $h\left ( x \right ) = \left \{ y \mid f \left ( x, y \right ) > t \left ( x \right ) , y \in \mathcal{Y} \right \}$ |
| $h\left ( \cdot  \right )$ | 表示一个多标签分类器函数$h: \mathcal{X} \rightarrow 2^{\mathcal{Y}}$, $h\left ( x \right )$返回$x$可能的标签集，该函数由置信度函数$f\left ( \cdot , \cdot \right )$和阈值函数$t\left ( \cdot  \right )$组合得到$h\left ( x \right ) = \left \{ y \mid f \left ( x, y \right ) > t \left ( x \right ) , y \in \mathcal{Y} \right \}$ |
| $rank_{f}\left ( \cdot , \cdot  \right )$ | 表示通过$f\left ( x , \cdot \right )$函数获得每个在相关标签集$\mathcal{Y}$中的标签$y$的置信度，并对这些置信度进行降序排名 |
| $\left | \cdot  \right |$ | 表示以集合$\mathcal{A}$为例，$\left | \mathcal{A}  \right |$返回集合$\mathcal{A}$的基数 |
| $[\![ \cdot ]\!]$ | 表示当$\pi$成立时$[\![ \pi ]\!]$返回数字1，否则返回0 |
| $\phi \left ( \cdot , \cdot \right )$ | 表示当$y \in Y$时，$\phi \left ( Y , y \right )$返回+1，否则返回-1 |
| $\mathcal{D}_{j}$ | 表示从多标签训练集$\mathcal{D}$中的第$j$类标签$y_{j}$得到的二元训练集$\left \{ \left ( x_{i}, \phi \left ( Y_{i}, y_{i} \right ) \right ) \mid 1 \leq i \leq m \right \}$，即当遍历所有的$x_{i}$所对应的相关标签集$Y_{i}$，当$y_{j} \in Y_{i}$时，那么上式$\left ( x_{i}, \phi \left ( Y_{i}, y_{i} \right ) \right )$就为1，否则为0 |
| $\psi \left ( \cdot ,\cdot ,\cdot  \right )$ | 表示假如$y_{j} \in Y$，并且$y_{k} \notin Y$，那么$\psi \left ( Y, y_{j}, y_{k} \right )$返回+1，而假如$y_{j} \notin Y$，并且$y_{k} \in Y$，那么$\psi \left ( Y, y_{j}, y_{k} \right )$返回-1 |
| $\mathcal{D}_{jk}$ | 表示从多标签训练集$\mathcal{D}$中的标签对$\left(y_{j}, y_{k}\right)$得到的二元训练集$\left \{ \left ( x_{i}, \psi \left ( Y_{i}, y_{j}, y_{k} \right ) \right ) \mid \phi \left ( Y_{i},y_{j} \neq \phi \left ( Y_{i}, y_{k} \right ), 1 \leq i \leq m \right ) \right \}$，即当遍历所有的$x_{i}$所对应的相关标签集$Y_{i}$时，$y_{j}$和$y_{k}$有且只有一个属于$Y_{i}$时，那么上式$\left ( x_{i}, \psi \left ( Y_{i}, y_{j}, y_{k} \right ) \right )$就为1，否则为-1 |
| $\sigma_{\mathcal{Y}}\left ( \cdot  \right )$ | 表示一个把$\mathcal{Y}$的幂集映射到正整数的映射函数$\sigma_{\mathcal{Y}}:2^{\mathcal{Y}} \rightarrow \mathbb{N}$（$\sigma_{\mathcal{Y}}^{-1}$表示逆函数） |
| $\mathcal{D}_{\mathcal{Y}}^{\dagger}$ | 表示从训练集$\mathcal{D}$获得多类单标签训练集$\left \{ \left ( x_{i}, \sigma_{\mathcal{Y}}\left ( Y_{i} \right ) \right ) \mid 1 \leq i \leq m \right \}$ |
| $\mathcal{B}$ | 表示二元学习算法【复杂度：$\mathcal{F}_{\mathcal{B}}\left ( m,d \right )$为训练集的复杂度，$\mathcal{F}_{\mathcal{B}}^{'}\left ( d \right )$为每个实例的测试集复杂度】 |
| $\mathcal{M}$ | 表示多类学习算法【复杂度：$\mathcal{F}_{\mathcal{M}}\left ( m,d,q \right )$为训练集的复杂度，$\mathcal{F}_{\mathcal{M}}^{'}\left ( d,q \right )$为每个实例的测试集复杂度】 |

### 学习框架

有多种有效的多标签指标可以用于描述多标签数据集的特征。其中最直接的方法就是首先测量多标签数据集的方法是平均标签基数：$LCard\left ( \mathcal{D} \right ) = \frac{1}{m} \sum_{i = 1}^{m} \left | Y_{i} \right |$，即遍历所有$x_{i}$所对应的相关标签集$Y_{i}$的基数，并且求和在求平均，从而得到标签集的平均标签基数。然后通过标签基数归一化得到标签密度$LDen\left ( \mathcal{D} \right ) = \frac{1}{\left | \mathcal{Y} \right |} \cdot LCard \left ( \mathcal{D} \right )$。另一个广泛用于测量多标签数据集多样性的方法是：$LDiv \left ( \mathcal{D} \right ) = \left | \left \{ Y \mid \exists x : \left ( x, Y \right ) \in \mathcal{D} \right \} \right |$，即在某个多标签数据集中出现不同标签集的数量。同样，标签的多样性可以通过实例的数量来进行归一化，以表示不同标签集的比例：$PLDiv \left ( \mathcal{D} \right ) = \frac{1}{\left | \mathcal{D} \right |} \cdot LDiv\left ( \mathcal{D} \right )$。

### 主要挑战

多标签学习的主要挑战是随着标签种类的增加，标签集的增加呈指数级增长。假如有20种标签（$q = 20$），那么可能的标签集就有$2^{20}$个。

为了应对标签集的指数增长，就需要在学习过程中增加标签与标签之间的关系或者依赖。因此，利用标签之间的关系就是多标签学习成功的关键。目前增加标签和标签之间的关联按照所使用方法的顺序，主要包含三种：

1、一阶策略：一阶策略把多标签学习转化为多个不相关的二元分类问题，即对每个标签进行分类。该方法的优点就是简单，当时缺点是由于忽略了标签之间的关系，因此效果次之。

2、二阶策略：二阶策略考虑了标签对内的关系，即某个标签和其他所有标签之间关联度的排序。因此，使用二阶策略具有良好的泛化性。但在实际应用中标签直接的关系往往超过两两之间的二阶关系。

3、高阶策略：高阶策略考虑了一个标签和所有标签之间的关系或者多个随机标签子集的关系。因此，使用高阶策略与采用一阶和二阶策略相比具有更好的泛化性，但计算要求更高，可扩展性更低。

### 阈值标定

在多标签学习中通常采用置信度函数$f\left ( \cdot , \cdot \right )$的返回值作为学习的模型。因此为了获得实例$x$的正确标签集，即$h\left ( x  \right )$，那么每个标签的置信度函数$f\left ( x , y \right )$都需要通过阈值函数$t\left ( x  \right )$进行校准。通常阈值标定有两种方法，分别是使用固定的阈值函数$t\left ( \cdot  \right )$和通过训练集中获取动态阈值函数$t\left ( \cdot  \right )$。

1、固定阈值函数：采用这种方法最直接的就是使用0作为标定常量。另一个做法是使用0.5作为标定常量。此外，对于在测试集中未知的实例$x$，设置标定常量可以最小化训练集和测试集在某个多标签指标上的差异，特别是标签基数。

2、动态阈值函数：采用这种方法通常使用stacking处理（即将训练集拆分为N个部分，当某个模型随机训练拆分后的某个数据集，并把训练后的结果给到下一个新的训练集，以此类推）来获取动态阈值函数。一个常用的获取动态阈值函数$t\left ( \cdot  \right )$的方法是使用线性模型，即当$f^{*}\left ( x \right ) = \left ( f\left ( x, y_{1} \right ), \cdots , f\left ( x, y_{q} \right ) \right )^{T} \in \mathbb{R}^{q}$时（保存了实例$x$对于$q$个标签的置信度），阈值函数模型为$t\left ( x \right ) = \left \langle \omega ^{*}, f^{*}\left ( x \right ) \right \rangle + b^{*}$，就是说只要能计算出$q$维向量的权重$w^{*}$和偏差$b^{*}$，就可以解决基于训练集$\mathcal{D}$的线性最小二乘问题$\underset{\left \{ \omega^{*}, b^{*} \right \}}{min}\sum_{i=1}^{m} \left ( \left \langle w^{*}, f^{*}\left ( x_{i} \right ) \right \rangle + b^{*} - s\left ( x_{i} \right ) \right )^{2}$。其中，$s\left ( x_{i} \right ) = argmin_{a \in \mathbb{R}}\left ( \left | \left \{ y_{j} \mid y_{j} \in Y_{i}, f\left ( x_{i}, y_{i} \right ) \leq a \right \} \right | + \left | \left \{ y_{k} \mid y_{k} \in \bar{Y_{i}}, f\left ( x_{i}, y_{k} \right ) \geq a \right \} \right | \right )$表示stacking模型处理输出，该模型把每个训练集中可能与$x_{i}$相关的标签集$\mathcal{Y}$分为相关和不相关两个部分，从而达到最小的错误分类。
\end{enumerate}

> 最小二乘问题：简单理解就是让总误差的平方最小$\varepsilon$的$y$就是真值。通过对下式求导就可以得到最小二乘的一个特例算术平均数，即算术平均数可以让误差最小。其中的“二乘”就是指平方。

\begin{center}
    $\left | y - y_{i} \right | \rightarrow \left ( y - y_{i} \right )^{2}$ \\
    $\varepsilon = \sum \left ( y - y_{i} \right )^{2}$
\end{center}

> 把最小二乘进行推广，就可以到如线性方程$f\left ( x \right ) = ax + b$，又可以得到总误差$\varepsilon = \sum \left ( f\left ( x_{i} \right ) - y_{i} \right )^{2} = \sum \left ( ax_{i} + b - y_{i} \right )^{2}$。

{% asset_img least-squares.png 最小二乘 %}

{% asset_img linear-least-squares.png 线性最小二乘 %}

## 评价指标

### 评价分类

传统监督学习的泛化性评价指标通常采用精度、F值（F-Measure）、ROC曲线的下面积（AUC）等来表示。由于多标签学习中一个实例会和多个标签相关联，而不是像单标签学习中一个实例和一个标签相关联。因此与传统的但标签学习相比，多标签学习的评价指标要复杂的多，大致可以分为两大类，分别是基于实例的评价和基于标签的评价。

{% asset_img roc.png ROC和AUC %}

>* 1、准确率（Accuracy）：预测准确率的高低，即$\frac{TP+TN}{TP+TN+FP+FN}$。
>* 2、查准率（Precision，P指标，【宁愿漏掉，不可错杀】）：预测为正例中实际为正例的占比，即$\frac{TP}{TP+FP}$。
>* 3、查全率（Recall，R指标，【宁愿错杀，不可漏掉】）：，正例预测正确占所有正例预测的比例，即$\frac{TP}{TP+FN}$。
>* 4、F值：查准率和查全率在非负权重$\beta$下的加权调和平均值，即$\frac{\left ( 1 + \beta^{2}  \right ) \times P \times R}{\beta ^{2}\left ( P+R \right )}$，$\beta$一般取0.3。当$\beta$取1时，那么F值就为P指标和R指标的调和平均，即F1值。
>* 5、ROC曲线：该曲线是由X轴的假正率（FPR，$\frac{FP}{FP+TN}$）和Y轴的真正率（TPR，$\frac{TP}{TP+FN}$）组成一个$1 \times 1$正方形，如图1所示。
>* 6、AUC面积：即ROC曲线的下半部分面积，当$AUC < 0.5$表示预测没有意义，当$0.5 < AUC < 0.7$表示预测价值较低，当$0.5 < AUC < 0.9$表示具有一定的预测价值，当$AUC > 0.9$表示预测准确度较高。

| 项目 | 预测正类（Positive） |  预测负类（Negative） |  实际总计 |
| :--: | :--: | :--: | :--: |
| 实际正类（True） | 将正类预测为正类（TP） | 将正类预测为负类（FN） | 实际为正类（$TP + FN = P$） |
| 实际负类（False） | 将负类预测为正类（FP） | 将负类预测为负类（TN） | 实际为负类（$FP + TN = N$） |
| 预测总计 | 预测为正类（$TP + FP = P^{'}$） | 预测为负类（$TN + FN = N^{'}$） | $P+N$ |

1、基于实例的评价：通过获取每个实例的性能评价，然后取平均。

2、基于标签的评价：通过对每个标签的性能进行评价从而获取整个学习系统的性能，并且返回所有类别标签的宏平均或微平均。

>* 1、宏平均：即在多标签（多类）分类中把每一类指标做算术平均，然后在根据平均后的结果计算F值。
>* 2、微平均：即在多标签（多类）分类中的查准率$\frac{\sum TP}{\sum TP + \sum FP}$。

{% asset_img evaluation-metrics.png 主要的多标签评价指标 %}

需要注意的是，除了从分类角度考虑多标签分类系统$h\left ( \cdot  \right )$的泛化性外，也可以通过置信度$f\left ( \cdot , \cdot  \right )$的排序来考虑多标签分类系统的泛化性

### 基于实例的评价指标

1、子集精度（Subset Accuracy）：即分类正确的比例，如预测标签集$h\left ( x_{i} \right )$与基准标签集$Y_{i}$相同时，则$[\![h\left ( x_{i} \right )=Y_{i}]\!]$返回1，否则返回0，其中$p$为实例的个数。但当标签集空间$q$很大时，由于判定的条件是全等，因此过于严格，造成评价解决并不合理。
    
\begin{align}
    subsetacc(h)=\frac{1}{p}\sum_{i=1}^{p}[\![h\left ( x_{i} \right )=Y_{i}]\!]
\end{align}
    
2、汉明损失（Hamming Loss）：即计算每个实例预测得到的标签集与基准标签集的差值的数量，包含没有被预测到的或者被误分类的标签数量，其中$h\left ( x_{i} \right )$表示实际预测得到的标签集，$\Delta Y_{i}$表示预测标签集和标准标签集的差值。当属于测试集$\mathcal{S}$的实例只与一个标签相关，则$hloss\left ( h \right )$是传统分类误差率的$2 / q$倍。
    
\begin{align}
    hloss\left ( h \right )=\frac{1}{p}\sum_{i=1}^{p}\left | h\left ( x_{i} \right )\Delta Y_{i} \right |
\end{align}


3、准确率（Accuracy）、查准率（Precision，P指标）、查全率（Recall，R指标）、F值。
    
\begin{align}
    \begin{matrix}
        Accuracy_{exam}\left ( h \right )=\frac{1}{p}\sum_{i=1}^{p}\frac{\left | Y_{i}\bigcap h\left ( x_{i} \right ) \right |}{\left | Y_{i}\bigcup h\left ( x_{i} \right ) \right |} & Precision_{exam}\left ( h \right )=\frac{1}{p}\sum_{i=1}^{p}\frac{\left | Y_{i}\bigcap h\left ( x_{i} \right ) \right |}{\left | h\left ( x_{i} \right ) \right |} \\ 
        Recall_{exam}\left ( h \right )=\frac{1}{p}\sum_{i=1}^{p}\frac{\left | Y_{i}\bigcap h\left ( x_{i} \right ) \right |}{\left | Y_{i} \right |} & F_{\beta }^{exam}\left ( h \right )=\frac{\left ( 1+\beta ^{2} \right )\cdot Precision_{exam}\left ( h \right )\cdot Recall_{exam}\left ( h \right )}{\beta ^{2}\cdot Precision_{exam}\left ( h \right )+Recall_{exam}\left ( h \right )}
    \end{matrix}
\end{align}


通过置信度函数$f\left ( \cdot ,\cdot  \right )$可以有四个基于实例的排序指标对多标签实例进行度量。

1、1-错误率（One-error）：即输出结果中排序第一的标签一定不属于当前实例。
    
\begin{align}
    one-error\left ( f \right ) = \frac{1}{p}\sum_{i=1}^{p} [\![\left [ argmax_{y\in \mathcal{Y}}f\left ( x_{i}, y \right ) \right ] \notin Y_{i}]\!]
\end{align}


2、覆盖率（Coverage）：对获取的每个与实例$x_{i}$相关标签$y$的置信度按照降序进行排名后，获得最后一个被正确预测的标签排名与第一个正确预测的标签排名的差值（或者说是距离），即距离越小越好，说明正确预测的标签即排名越高。
    
\begin{align}
    coverage\left ( f \right )=\frac{1}{p}\sum_{i=1}^{p}max_{y\in Y_{i}}rank_{f}\left ( x_{i},y \right )-1
\end{align}

{% asset_img coverage.png 覆盖率的例子 %}

3、损失排序（Ranking Loss）：把与$x_{i}$相关的标签集${y}'$的置信度与不相关的标签集${y}''$的置信度进行两两排序比较，获得不相关标签集出现在相关标签集中的概率，该损失值越小，预测结果越好。
    
\begin{align}
    rloss\left ( f \right )=\frac{1}{p}\sum_{i=1}^{p}\left | \left \{ \left ( {y}',{y}'' \right ) \mid f\left ( x_{i},{y}' \right ) \leq f\left ( x_{i},{y}'' \right ),\left ( {y}',{y}'' \right ) \in Y_{i}\times \bar{Y_{i}} \right \} \right |
\end{align}

4、平均精度（Average Precision）：分母是标签$x_{i}$的相关标签$y$的排名，分子表示属于$x_{i}$相关标签集$Y_{i}$且排名小于等于相关标签$y$的个数。该值越大，预测效果越好。
    
\begin{align}
    avgprec\left ( f \right )=\frac{1}{p}\sum_{i=1}^{p}\frac{1}{\left | Y_{i} \right |}\underset{y \in Y_{i}}{\sum} \frac{\left | \left \{ {y}'\mid rank_{f}\left ( x,{y}' \right ) \leq rank_{f}\left ( x_{i},y \right ),{y}'\in Y_{i} \right \} \right |}{rank_{f}\left ( x_{i},y \right )}
\end{align}

对于1-错误率（One-error）、覆盖率（Coverage）、损失排序（Ranking Loss）而言都是值越小说明学习系统的性能越好，当1-错误率（One-error）和损失排序（Ranking Loss）为0时，覆盖率（Coverage）为$\frac{1}{p}\sum_{i=1}^{p}\left | Y_{i} \right |-1$时，系统性能最好。而对于其他评价系统来说，值越大说明系统性能越好，1为最佳性能。


### 基于标签的评价指标

对于基于多标签分类器$h\left ( \cdot  \right )$的第$j$类标签$y_{j}$有四个基本特征量，这四个基本特征量都是在二元分类上某个标签基本特征量，分别是

\begin{align}
    \begin{matrix}
        TP_{j}=\left | \left \{ x_{i} \mid y_{i} \in Y_{i} \wedge y_{j} \in h\left ( x_{i} \right ), 1 \leq i \leq p \right \} \right | & FP_{j}=\left | \left \{ x_{i} \mid y_{i} \notin Y_{i} \wedge y_{j} \in h\left ( x_{i} \right ), 1 \leq i \leq p \right \} \right | \\ 
        TN_{j}=\left | \left \{ x_{i} \mid y_{i} \notin Y_{i} \wedge y_{j} \notin h\left ( x_{i} \right ), 1 \leq i \leq p \right \} \right | & FN_{j}=\left | \left \{ x_{i} \mid y_{i} \in Y_{i} \wedge y_{j} \notin h\left ( x_{i} \right ), 1 \leq i \leq p \right \} \right |
    \end{matrix}
\end{align}

其中$TP_{j}$表示正类预测为正类，$FP_{j}$表示负类预测为正类，$TN_{j}$表示正类预测为负类，$FN_{j}$表示负类预测为负类。并且基于上面四个基本特征量可以得到大部分的二元分类指标。假设$B \left(TP_{j}, FP_{j}, TN_{j}, FN_{j}\right)$表示一个二元分类的评价指标（$B \in \left [ 准确率、查准率、查全率、F^{\beta} \right ]^{4}$），那么基于标签的分类指标就可以从宏平均或微平均得到。

1、宏平均（Macro-averaging）

\begin{align}
    B_{macro}\left ( h \right )=\frac{1}{q}\sum_{j=1}^{q}B\left ( TP_{j},FP_{j},TN_{j},FN_{j} \right )
\end{align}

2、微平均（Micro-averaging）

\begin{align}
    B_{micro}\left ( h \right )=B\left ( \sum_{j=1}^{q}TP_{j},\sum_{j=1}^{q}FP_{j},\sum_{j=1}^{q}TN_{j},\sum_{j=1}^{q}FN_{j} \right )
\end{align}

同时，还可以得到$Accuracy_{macro}\left ( h \right )=Accuracy_{micro}\left ( h \right )$，以及$Accuracy_{micro}\left ( h \right )+hloss\left ( h \right )=1$。

当置信度函数$f\left(\cdot , \cdot \right)$可用时，可以获得基于标签的指标排序，如宏平均AUC，并且下式也遵守AUC和Wilcoxon-Mann-Whitney统计量之间的关系。

\begin{align}
    AUC_{macro}=\frac{1}{q}\sum_{j=1}^{q}AUC_{j}=\frac{1}{q}\sum_{j=1}^{q}\frac{\left | \left \{ \left ( {x}',{x}'' \right ) \mid f\left ( {x}',y_{j} \geq f\left ( {x}'',y_{j} \right ),\left ( {x}',{x}'' \right ) \in \mathcal{Z}_{j} \times \bar{\mathcal{Z}_{j}} \right ) \right \} \right |}{\left | \mathcal{Z}_{j} \right |\left | \bar{\mathcal{Z}_{j}} \right |}
\end{align}

其中，$\mathcal{Z}_{j} = \left \{ x_{i} \mid y_{j} \in Y_{i}, 1 \leq i \leq p \right \}\left ( \bar{\mathcal{Z}_{j}} = \left \{ x_{i} \mid y_{j} \notin Y_{i}, 1 \leq i \leq p \right \} \right )$

此外，还可以获得微平均AUC。

\begin{align}
    AUC_{micro}=\frac{\left | \left \{ \left ( {x}',{x}'',{y}',{y}'' \right ) \mid f\left ( {x}',{y}' \right ) \geq f\left ( {x}'',{y}'' \right ),\left ( {x}',{y}' \right )\in \mathcal{S}^{+},\left ( {x}'',{y}'' \right ) \in \mathcal{S}^{-} \right \} \right |}{\left | \mathcal{S}^{+} \right |\left | \mathcal{S}^{-} \right |}
\end{align}

不论是微平均AUC还是宏平均AUC，都是值越大，系统的性能越好，最优值为1。

### 理论结果

目前的多标签学习算法都在一个方面进行了优化，并且有研究表明如果只是单纯的提高子集的精度（Subset Accuracy），并且使用汉明损失（Hamming Loss）的话，系统的性能反而更差，因此需要多标签学习算法的性能应该在更为广泛的度量范围内进行测试，而不是仅在优化的算法上进行测试。

由于多标签指标通常是非凸的和不连续的，因此在实践中，大多数学习算法都会使用一些替代方法来优化或者替代多标签指标。最近，研究了多标签学习的一致性，即一个分类器的损失函数是否是否会随着训练集大小的增加而收敛到贝叶斯损失，并且还提出了一种在$\mathcal{X} \times 2^{\mathcal{Y}}$固定分布上，基于代理损失函数的多标签学习一致性的充分必要条件，即具有最优代理损失函数的分类器集必须落入能够产生最优原始多标签损失函数的分类器集中。

> 代理损失函数：即当目标函数非凸、不连续时，数学性质不好，优化起来比较复杂，这时候需要使用其他的性能较好的函数进行替换

> 贝叶斯损失（Bayes loss）：

通过关注排序损失（ranking loss）可以发现标签对所定义的非双向凸代替损失与排序损失（ranking loss）一致，并且一些近期的多标签学习方法与传统确定的多标签学习方法得到的结果不一致。对于这个负面结果，在最小化排序损失（ranking loss）中得到了与多标签学习具有一致性的补充结果。即通过简化双向排序问题，使得在单个标签上定义具有简单变量的凸代理函数，从而使排序损失（ranking loss）的遗憾界和收敛速度具有一致性。

> 遗憾界（regret bounds）：

# 学习算法

## 算法分类

本论文选择八个主要的算法，这八个算法首先具有普适性，即每个算法都有其独特的特点；其次具有原始的影响，是大多数后续算法的基础；最后是具有较高的影响力，这些算法都在多标签学习领域中被大量引用。多标签学习领域中可以把算法大致分为两大类，分别是：

1、问题转换的方法：即核心是把数据拟合到某一个算法上。这类方法是把多标签学习的问题转化为已有的学习方案上。其中代表算法首先是一阶的二元关联方法，其次是把多标签学习转化为标签排序的二阶标签排序校准方法，最后是把多标签学习转化为二元分类问题的高阶分类链方法和把多标签学习转化为多类分类任务的高阶随机k-labelsets方法。

2、算法适配的方法：即核心是把现有的某一个算法拟合到数据上。该类方法是使用目前流行的学习技术来适配多标签学习的问题，从而处理多标签数据。其中代表算法首先是通过适配惰性学习技术的一阶ML-KNN方法和适配了决策树的一阶ML-DT方法，其次是适配了核技术的二阶Rank-SVM方法和适配了信息论技术的二阶CML方法。

{% asset_img multi-label-learning-algorithms.png 多标签学习算法 %}

## 问题转换的方法

### 二元关联

该算法的基本思想是把多标签学习问题转化为$q$个不相关的二元分类问题，即每个标签的二元分类问题都可能是实例$x_{i}$上的标签。例如对于第$j$个标签$y_{j}$，那么本算法首先考虑标签$y_{j}$是否属于实例$x_{i}$的相关标签集$Y_{i}$来构建相应的二元训练集$\mathcal{D}_{j}$。

\begin{align}
    \mathcal{D}_{j}=\left \{ \left ( x_{i},\phi \left ( Y_{i}, y_{j} \right ) \right ) \mid 1 \leq i \leq m \right \} \nonumber \\
    where \ \phi \left ( Y_{i}, y_{i} \right )=
    \left\{\begin{matrix}
    +1, & y_{j} \in Y_{i} \\ 
    -1, & otherwise
    \end{matrix}\right.
\end{align}

然后通过二元学习算法$\mathcal{B}$构建二元分类器$g_{j} : \mathcal{X} \rightarrow \mathbb{R}$，即$g_{j} \leftarrow \mathcal{B}\left ( \mathcal{D}_{j} \right )$。因此，对于任意的多标签训练实例$\left( x_{i}, Y_{i} \right)$，实例$x_{i}$都将参与$q$个二元分类器的学习过程。对于相关标签$y_{j} \in Y_{i}$来说，$x_{i}$是作为分类器$g_{j}\left ( \cdot  \right )$的一个正例；此外，对于不相关标签$y_{k} \in \bar{Y_{i}}$来说，$x_{i}$是作为分类器$g_{j}\left ( \cdot  \right )$的一个负例。这种训练策略被称为交叉训练。

对于未知实例$x$来说，二元关联预测是通过查询每个独立二元分类器，并结合其他相关标签来构建实例$x$的相关标签集$Y$。

\begin{align}
    Y = \left \{ y_{j} \mid g_{j}\left ( x \right ) > 0, 1 \leq j \leq q \right \}
\end{align}

注意，当所有的二元分类器都输出负例时，那么预测得到的标签集$Y$将为空。因此，为了避免产生预测标签集$Y$为空，就需要引入T-Criterion规则。该准则通过包含具有最大输出的标签实例或者具有最少负例的实例来对上式进行补充。

\begin{align}
    Y = \left \{ y_{j} \mid g_{j}\left ( x \right ) > 0, 1 \leq j \leq q \right \} \bigcup \left \{ y_{j^{*}} = j^{*} = argmax_{1 \leq j \leq q} g_{j}\left ( x \right ) \right \}
\end{align}

评论：二元关联是许多最先进的多标签学习技术的基础模块。此外，由于二元关联忽略了标签之间的潜在管理，并且当标签数量$q$很大但标签密度（$LDen\left ( \mathcal{D} \right )$）较低时，二元分类器就会在每个标签上出现类失衡的问题。

{% asset_img binary-relevance.png 二元关联算法 %}

### 分类器链

该算法的基本思想是把多标签学习问题转化为一个二元分类问题链，即在分类器链中的子二元分类器建立在上一个子二元分类器预测的基础上。

假如有$q$个可能的标签类$\left\{ y_{1}, y_{2}, y_{3}, \cdots , y_{q}\right\}$，并且有一个全排列函数$\tau : \left ( 1, \cdots ,q \right ) \rightarrow \left ( 1, \cdots ,q \right )$可以对所有标签进行排序，即$y_{\tau \left ( 1 \right )} \succ y_{\tau \left ( 2 \right )} \succ \cdots \succ y_{\tau \left ( q \right )}$，其中$\tau \left ( j \right )\left(1 \leq j \leq q\right)$返回排列好的标签号。对于在一个已排列列表的第$j$个标签$y_{\tau \left ( j \right )} \left(1 \leq j \leq q\right)$，通过添加与$y_{\tau \left ( j \right )}$之前所有标签相关的实例来构建一个二元训练集。

\begin{align}
    \mathcal{D}_{\tau \left ( j \right )} = \left \{ \left ( \left [ x_{i}, pre_{\tau \left ( j \right )}^{i} \right ], \phi \left ( Y_{i}, y_{\tau \left ( j \right )} \right ) \right ) \mid 1 \leq i \leq m \right \} \nonumber \\
    where \ pre_{\tau \left ( j \right )}^{i} = \left ( \phi \left ( Y_{i}, y_{\tau \left ( 1 \right )} \right ), \cdots , \phi \left ( Y_{i}, y_{\tau \left ( j-1 \right )} \right ) \right )^{T}
\end{align}

其中，$\left [ x_{i}, pre_{\tau \left ( j \right )}^{i} \right ]$由$x_{i}$和$pre_{\tau \left ( j \right )}^{i}$两个向量组成。其中$pre_{\tau \left ( j \right )}^{i}$表示已排序标签$y_{\tau \left ( j \right )}$之前的所有标签与实例$x_{i}$的二元相关性赋值（$pre_{\tau \left ( 1 \right )}^{i} = \varnothing$，即$pre_{\tau \left ( 1 \right )}^{i}$为空集）。

然后，通过一些二元学习算法$\mathcal{B}$构建标签$y_{\tau \left ( j \right )}$的二元分类器$g_{\tau \left ( j \right )} : \mathcal{X} \times \left \{ -1, +1 \right \}^{j-1} \rightarrow \mathbb{R}$，并且来决定$y_{\tau \left ( j \right )}$是否是$x_{i}$的相关标签，即$g_{\tau \left ( j \right )} \leftarrow \mathcal{B}\left ( \mathcal{D}_{\tau \left ( j \right )} \right )$。

对于一个未知的实例$x$，通过遍历整个分类器链来预测$x$的相关标签集$Y$。假设$\lambda_{\tau \left ( j \right )}^{x} \in \left \{ -1, +1 \right \}$是标签$y_{\tau \left ( j \right )}$在实例$x_{i}$上的预测结果，那么器递归推导如下：

\begin{align}
    \begin{matrix}
        \lambda_{\tau \left ( 1 \right )}^{x} = sign\left [ g_{\tau \left ( 1 \right )}\left ( x \right ) \right ] \\ 
        \lambda_{\tau \left ( j \right )}^{x} = sign\left [ g_{\tau \left ( 1 \right )}\left ( \left [ x, \lambda_{\tau \left ( 1 \right )}^{x}, \cdots , \lambda_{\tau \left ( j-1 \right )}^{x} \right ] \right ) \right ]\left ( 2 \leq j \leq q \right )
    \end{matrix}
\end{align}

其中，$sign\left [ \cdot  \right ]$是一个带符号的函数。因此，实例$x$相应的预测标签集为：

\begin{align}
    Y = \left \{ y_{\tau \left ( j \right )} \mid \lambda_{\tau \left ( j \right )}^{x} = +1, 1 \leq j \leq q \right \}
\end{align}

对于采用分类器链来解决多标签学习的问题来说，全排列函数$\tau$的顺序对结果有很大影响。由于排序的影响，因此可以在标签集上通过$n$个随机排列$\tau$来构建一个分类器链的集合，即$\tau ^{\left ( 1 \right )}, \tau ^{\left ( 2 \right )}, \cdots , \tau ^{\left ( n \right )}$。对于每一个排列$\tau ^{\left ( r \right )}\left ( 1 \leq r \leq n \right )$来说，并不是直接通过原始的训练集$\mathcal{D}$产生一个分类器链，而是通过对训练集$\mathcal{D}$进行采样（$\left | \mathcal{D}^{\left( r \right)} \right | = 0.67 \cdot \left | \mathcal{D} \right |$）或者替换（$\left | \mathcal{D}^{\left( r \right)} \right | = \left | \mathcal{D} \right |$）的方式来得到一个修改后的训练集$\mathcal{D}^{\left( r \right)}$。

评论：分类器链以随机方式考虑标签之间的相关性。与二进关联相比，分类器链具有利用标签之间相关性的优点，但是同时由于分类器链自身的特性，因此就无法实现并行处理。在训练阶段，分类器链从参考标记拓展了实例空间的特性，即$pre_{\tau \left ( j \right )}^{i}$。另一种替代扩展特征值为二元值的可行方案是，当算法$\mathcal{B}$（如朴素贝叶斯）产生的模型能够返回后验概率时，那么扩展特征值就会输出概率，而不是二元值。

{% asset_img classifier-chains.png 分类器链 %}

### 校准标签排序

该算法的基本思想是把多标签学习问题转化为标签排序问题，即利用成熟的两两标签比较技术实现标签集中标签的排序。

假如有$q$个可能的标签类$\left\{ y_{1}, y_{2}, y_{3}, \cdots , y_{q}\right\}$，那么按照两两成对比较就会有总共$\frac{q\left ( q-1 \right )}{2}$个二元分类器，其中每一对标签为$\left ( y_{j}, y_{k} \right )\left ( 1\leq j\leq k\leq q \right )$。要对标签$\left ( y_{j}, y_{k} \right )$进行两两比较，首先通过考虑每个训练实例$x$与标签对$y_{j}$和$y_{k}$之间的相对相关性才能构建一个基于成对标签的二元训练集。

\begin{align}
    \mathcal{D}_{jk}=\left \{ \left ( x_{i},\varphi \left ( Y_{i},y_{j},y_{k} \right ) \right ) \mid \phi \left ( Y_{i},y_{j} \right ) \neq \phi \left ( Y_{i},y_{k} \right ),1\leq i\leq m \right \} \nonumber \\
    where \ \varphi \left ( Y_{i}, y_{j}, y_{k} \right ) = \left\{\begin{matrix}
    +1, & if \ \phi \left ( Y_{i}, y_{j} \right ) = +1 \ and \ \phi \left ( Y_{i}, y_{k} \right ) = -1 \\ 
    -1, & if \ \phi \left ( Y_{i}, y_{j} \right ) = -1 \ and \ \phi \left ( Y_{i}, y_{k} \right ) = +1
    \end{matrix}\right.
\end{align}

也就是说只有当实例$x_{i}$与标签$y_{j}$和标签$y_{k}$具有上面的显著关系时才会被包含在训练集$\mathcal{D}_{jk}$中。然后通过一些二元分类算法$\mathcal{B}$构建一个标签$\left ( y_{j}, y_{k} \right )$的二元分类器$g_{jk} : \mathcal{X} \rightarrow \mathbb{R}$，即$g_{jk} \leftarrow \mathcal{B}\left ( \mathcal{D}_{jk} \right )$。因此对于任意的多标签训练实例$\left ( x_{i}, Y_{i} \right )$来说，实例$x_{i}$都将参与到$\left | Y_{i} \right |\left | \bar{Y_{i}} \right |$二元分类器的学习过程中。对于任意$x \in \mathcal{X}$的实例，当$g_{jk}\left(x\right) > 0$时，那么学习系统会投票给$y_{j}$，否则就投票给$y_{k}$。

对于一个未知的实例$x$，校准标签排序算法首先把实例$x$反馈到$\frac{q\left ( q-1 \right )}{2}$个已经训练好的二元分类器上，以此来获得所有可能标签的投票结果。

\begin{align}
    \zeta \left ( x,y_{j} \right )=\sum_{k=1}^{j-1}[\![ g_{kj}\left ( x \right ) \leq 0 ]\!] + \sum_{k=j+1}^{q}[\![ g_{kj}\left ( x \right ) > 0 ]\!] \ \left ( 1 \leq j \leq q \right )
\end{align}

根据上面的定义，可以得到$\sum_{j=1}^{q}\zeta \left ( x, y_{i} \right ) = \frac{q\left ( q-1 \right )}{2}$，并且$\mathcal{Y}$中的所有标签都可以根据各自的投票进行重新排序（即关系被任意打破）。

因此，还需要使用阈值函数来将排好序的标签列表进行重新分类，即分为相关标签和不相关标签。而对于使用标签对方式的校准标签排序算法而言，需要在每个多标签训练实例$\left ( x_{i},Y_{i} \right )$中引入了一个虚拟标签$y_{V}$。因此虚拟标签$y_{V}$就是区分$x_{i}$的相关标签和不相关标签的一个分割点。换句话说，比$y_{V}$排名高的就是相关标签$y_{j} \in Y_{i}$，而比$y_{V}$排名低的就是不相关标签$y_{k} \in \bar{Y_{i}}$。

除了原始的$\frac{q\left ( q-1 \right )}{2}$个二元分类器外，对于每个新标签对$\left ( y_{j},y_{V} \right )\left ( 1 \leq j \leq q \right )$来言，又会有$q$个辅助二元分类器。这个二元训练集将会以以下方式进行构建：

\begin{align}
    \mathcal{D}_{jV}=\left \{ \left ( x_{i}, \varphi \left ( Y_{i}, y_{j}, y_{V} \right ) \right ) \mid 1 \leq i \leq m\right \} \nonumber \\
    where \ \varphi \left ( Y_{i}, y_{j}, y_{V} \right ) = \left\{\begin{matrix}
        +1, & if \ y_{j} \in Y_{i} \\ 
        -1, & otherwise
        \end{matrix}\right.
\end{align}

基于新添加二元训练集，通过一个二元分类算法$\mathcal{B}$构建了一个虚拟标签的二元分类器$g_{jV} : \mathcal{X} \rightarrow \mathbb{R}$，即$g_{jV} \leftarrow \mathcal{B}\left ( \mathcal{D}_{jV} \right )$。然后通过引入新的分类器来更新投票函数$\zeta \left ( x,y_{j} \right )$，具体如下：

\begin{align}
    \zeta^{*} \left ( x,y_{j} \right ) = \zeta \left ( x,y_{j} \right ) + [\![ g_{jV}\left ( x \right ) > 0 ]\!] \left ( 1 \leq j \leq q \right )
\end{align}

此外，虚拟标签的总投票数可以计算为：

\begin{align}
    \zeta^{*} \left ( x,y_{j} \right ) = \sum_{j=1}^{q}[\![ g_{jV}\left ( x \right ) \leq 0 ]\!]
\end{align}

因此，对于未知实例$x$的预测标签集可以表示为：

\begin{align}
    Y = \left \{ y_{j} \mid \zeta^{*} \left ( x,y_{j} \right ) > \zeta^{*} \left ( x,y_{V} \right ) , 1 \leq j \leq q \right \}
\end{align}

比较二元关联算法和校准标签排序算法中的训练集创建，可以看出两者的训练集产生相同。因此，校准标签排序算法可以被认为是标签之间两两比较算法的扩展版，即是为了便于学习而把$q$个二元关联分类器扩展得到$\frac{q\left ( q-1 \right )}{2}$个二元分类器。

评论：校准标签排序算法是通过两两标签构建分类器的二阶方法。相较于以往一对多算法构建的二元分类器相比，校准标签排序以一对一方式构建二元分类器（除了虚拟标签），具有减轻因标签类不平衡造成的问题的优点。此外，通过校准标签排序算法生成的分类器的数量从原先的随着标签$q$数量成线性增长变为了随着标签$q$数量成二次增长。因此，校准标签排序的优化和改进主要集中在通过精确裁剪或近似裁剪从而减少测试阶段的分类器数量，例如通过底层二元学习算法$\mathcal{B}$的特性（感知机的对偶形式等）可以在训练阶段更为高效的获取二次数量的感知机。

> 感知机的对偶形式：

{% asset_img calibrated-label-ranking.png 校准标签排序 %}

### Random k-Labelsets

该算法的基本思想是把多标签学习问题转换为多类分类问题的集合，即集合中的每个学习器组件都会针对标签空间$\mathcal{Y}$中的某个随机子集使用LP（Label Powerset）技术实现一个多类分类器。LP技术是把多标签学习问题转化为多类（单标签）分类问题最直接的方法。假设$\sigma_{\mathcal{Y}}:2^{\mathcal{Y}} \rightarrow \mathbb{N}$是$\mathcal{Y}$的幂集映射到自然数的映射函数，而$\sigma_{\mathcal{Y}}^{-1}$是逆函数。在训练阶段，LP首先把原始的多标签训练集$\mathcal{D}$中的每个不同标签作为一个新类，从而把在$\mathcal{D}$转化为多类训练集。

\begin{align}
    \mathcal{D}_{\mathcal{Y}}^{\dagger}=\left \{ \left ( x_{i}, \sigma_{\mathcal{Y}}\left ( Y_{i} \right ) \right ) \mid 1\leq i \leq m \right \}
\end{align}

因此，$\mathcal{D}_{\mathcal{Y}}^{\dagger}$的新的多类标签集合为：

\begin{align}
    \Gamma \left ( \mathcal{D}_{\mathcal{Y}}^{\dagger} \right )=\left \{ \sigma_{\mathcal{Y}} \left ( Y_{i} \right ) \mid 1 \leq i \leq m \right \}
\end{align}

显然，$\left | \Gamma \left ( \mathcal{D}_{\mathcal{Y}}^{\dagger} \right ) \right | \leq min\left ( m, 2^{\left | \mathcal{Y} \right |} \right )$，即新的多类标签集一定小于等于训练集中实例的数量或者标签集全排列后的数量中最小的那个。然后，通过一些多类学习算法$\mathcal{M}$构建一个多类分类器$g_{\mathcal{Y}}^{\dagger }:\mathcal{X}\rightarrow \Gamma \left ( \mathcal{D}_{\dagger }^{\mathcal{Y}} \right )$，即$g_{\mathcal{Y}}^{\dagger } \leftarrow \mathcal{M} \left ( \mathcal{D}_{\dagger }^{\mathcal{Y}} \right )$。因此，对于任意多标签训练实例$\left ( x_{i}, Y_{i} \right )$，实例$x_{i}$将会被重新分配一个已经映射好的新的单标签$\sigma_{\mathcal{Y}}\left ( Y_{i} \right )$，并参与到后续的多类分类器中。

对于未知的实例$x$，LP要预测其相关标签集$Y$，那么首先就要查询多类分类器的预测，然后把结果通过逆函数得到相关标签集$Y$的幂集。

\begin{align}
    Y=\sigma_{\mathcal{Y}}^{-1}\left ( g_{\mathcal{Y}}^{\dagger} \left ( x \right ) \right )
\end{align}

不幸的是，LP在可行性方面有两个主要的缺点，分别是：

1、不完备性：根据上式可以知道，LP预测的标签集只能是训练集中的标签集，即训练集外的标签集就不能进行泛化$\left \{ Y_{i} \mid 1 \leq i \leq m\right \}$。
    
2、效率较低：当$\mathcal{Y}$非常大时，就会在$\Gamma \left ( \mathcal{D}_{\mathcal{Y}}^{\dagger} \right )$有非常多的新的标签类，导致在训练分类器$g_{\mathcal{Y}}^{\dagger } \left(\cdot \right)$是非常的复杂，并且一些新产生的标签类其对应的实例有非常少。

因此，为了保持LP的简单性，以及克服LP的两个主要缺点，Random k-Labelsets选择在多标签数据的学习中把集成学习与LP相结合。这个方法的核心是仅在随机k-Labelsets（在$\mathcal{Y}$标签集中大小为$k$的子集）中使用LP，从而保证计算的效率，然后把多个LP分类器进行组合，从而保证了预测的完备性。

假设$\mathcal{Y}^{k}$表示在标签集$\mathcal{Y}$中所有可能的k-Labelsets，例如第$l$个k-Labelsets表示为$\mathcal{Y}^{k}\left(l\right)$，即$\mathcal{Y}^{k}\left(l\right) \subseteq \mathcal{Y},\left | \mathcal{Y}^{k}\left(l\right) \right | = k, 1 \leq l \leq \binom{q}{k}$，类似于$\mathcal{D}_{\mathcal{Y}}^{\dagger}$，一个多类训练集能够通过把原始标签空间$\mathcal{Y}$缩小为$\mathcal{Y}^{k}\left(l\right)$进行构建。

\begin{align}
    \mathcal{D}_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger}=\left \{ \left ( x_{i}, \sigma_{\mathcal{Y}^{k}\left ( l \right )}\left ( Y_{i} \cap \mathcal{Y}^{k}\left ( l \right ) \right ) \right ) \mid 1 \leq i \leq m \right \}
\end{align}

因此，新的多类标签集可以通过$\mathcal{D}_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger}$表示为：

\begin{align}
    \Gamma \left ( \mathcal{D}_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger} \right )=\left \{ \sigma_{\mathcal{Y}^{k}\left ( l \right )}\left ( Y_{i} \cap \mathcal{Y}^{k}\left ( l \right ) \right ) \mid 1 \leq i \leq m \right \}
\end{align}

然后，通过多类学习算法$\mathcal{M}$构建一个多类分类器$g_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger}:\mathcal{X} \rightarrow \Gamma \left ( \mathcal{D}_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger} \right )$，即$g_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger}: \mathcal{M} \left ( \mathcal{D}_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger} \right )$。

为了创建了由$n$个分类组件的集合，Random k-Labelsets会在$n$个子标签集$\mathcal{Y}^{k}\left ( l_{r} \right )\left ( 1 \leq r \leq n \right )$上使用LP，使得每个子标签集都会产生一个多类分类器$g_{\mathcal{Y}^{k}\left ( l \right )}^{\dagger}\left ( \cdot  \right )$。对于未知的实例$x$，每类标签都需要计算以下两个数量。

\begin{align}
    \begin{matrix}
        \tau \left ( x, y_{j} \right ) = \sum_{r=1}^{n}[\![ y_{j} \in \mathcal{Y}^{k}\left ( l_{r} \right ) ]\!] \ \left ( 1 \leq j \leq q \right ) \\ 
        \mu \left ( x, y_{j} \right ) = \sum_{r=1}^{n} [\![ y_{j} \in \sigma_{\mathcal{Y}^{k}\left ( l_{r} \right )}^{-1} \left ( g_{\mathcal{Y}^{k}\left ( l_{r} \right )}^{\dagger } \left ( x \right )\right ) ) ]\!] \ \left ( 1 \leq j \leq q \right )
    \end{matrix}
\end{align}

其中，$\tau \left ( x, y_{j} \right )$表示在集合中标签$y_{j}$可以获取的最大预期投票数，而$\mu \left ( x, y_{j} \right )$则表示在集合中$y_{j}$获取的实际投票数。因此，对于标签集的预测就可以等价为：

\begin{align}
    Y=\left \{ y_{j} \mid \frac{\mu \left ( x, y_{j} \right )}{\tau \left ( x, y_{j} \right )} > 0.5, 1 \leq j \leq q \right \}
\end{align}

换句话说，当实际投标数超过了最大投票数的一半时，那么就认为$y_{j}$标签与实例$x$相关。对于有$n$个k-labelsets来说，其每个标签的最大平均投票数为$\frac{nk}{q}$。在Random k-Labelsets算法的经验，一般$k = 3，n = 2q$。

评论：Random k-Labelsets算法是一个高阶方法，即标签相关性的维度，由子标签集的大小决定。此外，在使用Random k-Labelsets算法时，可以通过预设数量阈值从而把在标签数量低于阈值的训练集$\mathcal{D}$裁剪掉。虽然Random k-Labelsets算法通过将集成学习作为其固有的一部分来修正LP的缺点，但集成学习也可以作为一种元级策略，通过包含同质或异质的多标签学习器来促进多标签学习。

{% asset_img random-k-Labelsets.png Random k-Labelsets %}

## 算法适配的方法

### 多标签的K最邻近分类算法（ML-KNN）

该算法的基本思想是通过适配KNN技术来处理多标签数据，即利用最大后验概率（MAP）准则对邻近标签内的信息进行推理，从而实现对多标签数据的预测。

对于未知实例$x$，假设$\mathcal{N}\left(x\right)$表示实例$x$在训练集$\mathcal{D}$中的最邻近数据集。通常，实例之间的相似性使用欧氏距离进行度量。对于第$j$类标签，在ML-KNN选择计算下面的统计信息：

\begin{align}
    C_{j}=\sum_{\left ( x^{*},Y^{*} \right ) \in \mathcal{N}\left ( x \right )}[\![ y_{j} \in Y^{*} ]\!]
\end{align}

其中$C_{j}$表示与标签$y_{j}$相关的实例$x$的领域数量。

假设$H_{j}$是实例$x$有标签$y_{j}$的事件，并且$\mathbb{P}\left ( H_{j} \mid C_{j} \right )$表示实例$x$有标签$y_{j}$，且正好有$C_{j}$个领域的情况下的后验概率。而$\mathbb{P}\left (\neg H_{j} \mid C_{j} \right )$则表示实例$x$没有标签$y_{j}$，且正好有$C_{j}$个领域的情况下的后验概率。因此，按照MAP准则，标签集的预测通过判断$\mathbb{P}\left ( H_{j} \mid C_{j} \right )$是否大于$\mathbb{P}\left (\neg H_{j} \mid C_{j} \right )$或者不大于。

\begin{align}
    Y = \left \{ y_{j} \mid \frac{\mathbb{P}\left (H_{j} \mid C_{j} \right )}{\mathbb{P}\left (\neg H_{j} \mid C_{j} \right )} > 1, 1 \leq j \leq q\right \}
\end{align}

>* 1、KNN的说明：即计算实例$x$和所有训练实例的距离，然后进行排序，取前k个实例，这k个实例的多数属于某个类，实例$x$就归属于哪个类。虽然KNN简单，但由于要和所有的训练实例计算距离，因此当训练集特别大的时候，这种方式非常耗时，可以采用KD树（K-Dimensional Tree）的方式来减少输入实例和训练实例的计算次数从而优化性能。
>* 2、KD树（K-Dimensional Tree）：以树型方式对$N$维空间的实例进行存储，以便对其进行快速检索。KD树的创建相当于用不断垂直于坐标轴的超平面将$N$维空间进行划分，构成一系列的$N$维超矩阵区域。
>* 3、KD树的生成：首先从$m$个样本的$N$维特征中分别计算$N$个特征取值的方差，用方差最大的第$k$维特征$n_{k}$来作为根节点，然后选择特征$n_{k}$的中间值$n_{kv}$作为样本的划分点，对于所有第$k$维特征的实例来说，其值小于$n_{kv}$时，则该实例划入左分支，对取值大于等于$n_{kv}$的样本，则划入右分支。然后再对左子树和右子树，采用相同的方法在分支中找到方差最大的特征作为当前分支的根节点，最后递归的生成KD树。
>* 4、KD树的搜索：首先找到包含预测目标点的子节点，以该节点为中心，以目标前到该节点的距离为半径，得到一个超球体，从KNN的原理可以知道最近邻的点一定在该超球体内部。其次，返回该节点的父节点，检查另一个子节点所包含的超矩形是否与球体相交，如果有相交，那么就需要在该节点查询是否有更加近的近邻，如果有就更新最近邻和超球体，如果没有相交就继续返回父节点，在另一个子节点中查找最近邻。最后直到回到了根节点，算法结束，此时保存的最近领就是最终的最近邻。

{% asset_img kd-create.png KD创建 %}

{% asset_img kd-structure.png Random KD结构 %}

{% asset_img kd-search.png Random KD搜索 %}

> 欧式距离的说明：即常见的两点之间的距离，对于二维坐标系其距离为：$E\left ( d \right ) = \sqrt{\left ( x_{1} - x_{2} \right )^{2} + \left ( y_{1} - y_{2} \right )^{2}}$。如果扩展到N维数据，那么两点之间的距离就是各维度数据分别求差后平方，然后再后求和，最后再开平方，即$E\left ( d \right ) = \sqrt{\left ( x_{1} - x_{2} \right )^{2} + \left ( y_{1} - y_{2} \right )^{2} + \left ( z_{1} - z_{2} \right )^{2} + \cdots }$。

根据贝叶斯定理，就可以得到：

\begin{align}
    \frac{\mathbb{P}\left (H_{j} \mid C_{j} \right )}{\mathbb{P}\left (\neg H_{j} \mid C_{j} \right )} = \frac{\mathbb{P}\left ( H_{j} \right ) \cdot \mathbb{P}\left ( C_{j} \mid H_{j} \right )}{\mathbb{P}\left ( \neg H_{j} \right ) \cdot \mathbb{P}\left ( C_{j} \mid \neg H_{j} \right )}
\end{align}

这里的$\mathbb{P}\left ( H_{j} \right )$（$\mathbb{P}\left ( \neg H_{j} \right )$）表示实例$x$与标签$y_{j}$具有相关性的事件$H_{j}$（或$x$与标签$y_{j}$不相关）的先验概率。此外，$\mathbb{P}\left (C_{j} \mid H_{j} \right )$（$\mathbb{P}\left (C_{j} \mid \neg H_{j} \right )$）表示当事件$H_{j}$成立（不成立）的情况下，实例$x$在标签$y_{j}$上有$C_{j}$个邻近的概率。通过上式就可以实现先验概率的估计和执行预测。

为了完成上面的任务，ML-KNN采用频率计数的策略。首先，通过计算与每个标签关联的训练样本的数量来估计先验概率。

\begin{align}
    \mathbb{P}\left ( H_{j} \right )=\frac{s + \sum_{i=1}^{m}[\![ y_{j} \in Y_{i} ]\!]}{s \times 2 + m}; \mathbb{P}\left ( \neg H_{j} \right ) = 1 -  \mathbb{P}\left ( H_{j} \right )\ \left ( 1 \leq j \leq q \right )
\end{align}

其中，$s$是用于控制平均先验概率对估计影响的平滑参数。在拉普拉斯平滑中，该值一般取值为1。

> 拉普拉斯平滑的说明：通过增加平滑参数$s$解决0概率问题。

其次，就是对似然性的估计。对于第$j$个标签$y_{j}$，ML-KNN维护了两个频率数组$\mathcal{K}_{j}$和$\tilde{\mathcal{K}}_{j}$，每个数组都包含了$k+1$个元素：

> 最大似然的说明：似然和概率的区别在于，概率是知道事件发生的概率推算结果，而似然是根据结果推算事件发生的概率，即概率是参数推断结果的过程，而似然是结果推断参数分布的过程。延生到机器学习，就可以知道机器学习也是通过结果进行学习，然后求最大似然，最后得到未知变量的预测结果

\begin{align}
    \begin{matrix}
        \mathcal{K}_{j}\left [ r \right ] = \sum_{i=1}^{m}[\![ y_{i} \in Y_{i} ]\!] \cdot [\![ \delta_{j}\left ( x_{i} \right ) = r ]\!] \ \left ( 0 \leq r \leq k \right ) \\ 
        \tilde{\mathcal{K}_{j}}\left [ r \right ] = \sum_{i=1}^{m}[\![ y_{i} \notin Y_{i} ]\!] \cdot [\![ \delta_{j}\left ( x_{i} \right ) = r ]\!] \ \left ( 0 \leq r \leq k \right ) \\ 
        where \ \delta_{j}\left ( x_{i} \right ) = \sum_{\left ( x^{*}, Y^{*} \right ) \in \mathcal{N}\left ( x_{i} \right )}[\![ y_{j} \in Y^{*} ]\!]
    \end{matrix}
\end{align}

其中，$\delta_{j}\left ( x_{i} \right )$记录了与标签$y_{j}$相关的实例$x_{i}$邻域的数量。因此，$\mathcal{K}_{j}\left [ r \right ]$保存了与标签$y_{j}$相关，并且有$r$个邻域的训练实例的数量。同样，$\tilde{\mathcal{K}_{j}}\left [ r \right ]$保存了与标签$y_{j}$不相关，但有$r$个邻域的训练实例的数量。随后，似然性就可以通过$\mathcal{K}_{j}\left [ r \right ]$和$\tilde{\mathcal{K}_{j}}\left [ r \right ]$这两个数组进行估算。

\begin{align}
    \begin{matrix}
        \mathbb{P}\left ( C_{j} \mid H_{j} \right ) = \frac{s + \mathcal{K}_{j}\left [ C_{j} \right ]}{s \times \left ( k+1 \right ) + \sum_{r=0}^{k}\mathcal{K}_{j}\left [ r \right ] } \ \left ( 1 \leq j \leq q,0 \leq C_{j} \leq k \right )\\ 
        \mathbb{P}\left ( C_{j} \mid \neg H_{j} \right ) = \frac{s + \tilde{\mathcal{K}_{j}}\left [ C_{j} \right ]}{s \times \left ( k+1 \right ) + \sum_{r=0}^{k}\tilde{\mathcal{K}_{j}}\left [ r \right ] } \ \left ( 1 \leq j \leq q,0 \leq C_{j} \leq k \right )
    \end{matrix}
\end{align}

因此，有了先验概率和似然性就可以得到对标签集进行预测。

评价：ML-KNN算法是一个通过单独推理每个标签相关性的一阶算法。ML-KNN继承了惰性学习和贝叶斯推理的优点，具体如下：

1、决策边界可以自适应地调整，用于应对识别每个未知实例的不同领域。
    
2、由于估算了每类标签的先验概率，因此大大减轻了类别失衡问题。

其实还有其他使用惰性学习来处理多标签数据，如集合了KNN和聚合排名，通过识别一个特定标签来把KNN扩展到整个训练集。由于ML-KNN忽略了标签之间的相关性，因此已经提出了一些方法把标签之间的关系加入到ML-KNN中。

{% asset_img ml-knn.png ML-kNN %}

### 多标签决策树（ML-DT）}

该算法的基本思想是通过适配决策树技术来处理多标签数据，即利用多标签熵的信息增益准则来递归构建决策树。

>* 1、决策树的说明：决策树是一种基本的分类与回归方法。决策树的产生通常分为两个部分，分别是构造和剪枝。
>* 2、决策树的构造：即就是对更节点、分支和叶片的选择，并且节点和节点之间存在父子关系。其中要特别注意的是叶片是最底部的节点，是决策的结果。
>* 3、决策树的剪枝：解决过拟合的问题。其中过拟合表示模型训练的太好，使得模型过于死板，分类条件过于严格。欠拟合表示模型训练的效果不理想。剪枝的方法分为两种，分别是预剪枝和后剪枝。
>* 4、预剪枝：在决策树构造时就进行剪枝。即在构造的过程中对节点进行评估，如果对某个分支节点在验证集中不能带来准确性的提升，那么对这个分支节点的划分就没有意义，这时就会把当前分支节点作为叶节点。
>* 5、后剪枝：在生成决策树之后再进行剪枝。通常会从决策树的叶节点开始，逐层向上对每个节点进行评估。如果该节点存在与否对节点子树在分类准确性上差别影响不大，或者剪掉该节点子树，能在验证集中带来准确性的提升，那么就可以把该节点子树进行剪枝。使用这个节点子树的叶片节点来替代，类标记为这个节点子树中最频繁的那个类。
>* 6、在决策树中根节点的选择由两个指标组成：纯度和信息熵。
>* 7、纯度：让目标变量的分歧最小。
>* 8、信息熵：表示信息的不确定性$Entropy\left ( t \right ) = -\sum_{i=0}^{c-1}p\left ( i \mid t \right )\log_{2}p\left ( i \mid t \right )$。其中$p\left ( i \mid t \right )$表示节点$t$为分类$i$的概率。当不确定性越大时，它所包含的信息量也就越大，信息熵也就越高。并且当信息熵越大，纯度越低，而如果集合中的所有样本均匀混合时，信息熵最大，纯度最低。

> 构建决策树时通常会采用纯度来构建，而常用的“不纯度”指标有三种，也对应了三种算法，分别是信息增益（ID3 算法）、信息增益率（C4.5算法）以及基尼指数（Cart 算法）。

>* 1、信息增益（ID3 算法）：即将信息熵最大的作为每个分支的父节点的属性，该方法可以提高分支的纯度，并降低信息熵。计算方法是父节点的信息熵减去所有子节点信息熵和。在具体计算时计算每个子节点的归一化信息熵，即按照每个子节点在父节点中出现的概率。

\begin{align}
    Gain\left (D, a \right )=Entropy\left ( D \right ) - \sum_{i=1}^{k}\frac{\left | D_{i} \right |}{\left | D \right |}Entropy\left ( D_{i} \right )
\end{align}

> 其中$D$表示父节点，$D_{i}$表示子节点，$Gain\left ( D, a \right )$中的$a$表示选择的属性。ID3算法的缺陷是该算法倾向于选择取值种类比较多的属性，即有些属性可能对分类任务没有太大作用，但是他们仍然可能会被选为最优属性。例如极端情况是如果把“编号”用做一个可选属性，那么ID3就会把编号作为最有属性，但是实际上该属性是无关属性。

>* 2、信息增益率（C4.5算法）：为了避免ID3算法的问题，C4.5算法使用信息增益率作为属性原则的依据，即当属性有很多值的时候，相当于被划分成了许多份，虽然信息增益变大了，但是对于 C4.5 来说，属性熵也会变大，所以整体的信息增益率并不大。
>* 3、悲观剪枝（PEP）：为了解决ID3容易造成过拟合的问题，在构建完成决策树后采用悲观剪枝的方法来提升决策树的泛化能力。该方法是后剪枝技术中的一种，通过递归估算每个内部节点的分类错误率，比较剪枝前后这个节点的分类错误率来决定是否对其进行剪枝，并且这种剪枝方法不需要单独的测试数据集。
>* 4、离散化处理连续属性：C4.5还可以对连续属性进行离散化处理。其方法是选择具有最高信息增益所对应的阈值。

{% asset_img decision-tree.png 决策树 %}

假设有$n$个实例的多标签数据集$\mathcal{T} = \left \{ \left ( x_{i}, Y_{i} \right ) \mid 1 \leq i \leq n \right \}$，通过沿着分割值$\vartheta$的第$l$个特征对数据集$\mathcal{T}$进行划分，可以得到信息增益为：

\begin{align}
    \begin{matrix}
        IG\left ( \mathcal{T} , l, \vartheta \right ) = MLEnt\left ( \mathcal{T}  \right ) - \sum \frac{\left | \mathcal{T}  \right |}{\left | \mathcal{T} ^{\rho } \right |}\cdot MLEnt\left ( \mathcal{T} ^{\rho } \right )\left ( \rho \in \left \{ -,+ \right \} \right )\\ 
        where\ \mathcal{T} ^{-}=\left \{ \left ( x_{i},Y_{i} \right ) \mid x_{il} \leq \vartheta , 1 \leq i \leq n \right \}, \ \mathcal{T} ^{+}=\left \{ \left ( x_{i},Y_{i} \right ) \mid x_{il} > \vartheta , 1 \leq i \leq n \right \}
    \end{matrix}
\end{align}

其中，$\mathcal{T} ^{-}\left(\mathcal{T} ^{+}\right)$表示在第$l$个特征上值小于（大于）$\vartheta$的所有实例。

从根节点（即$\mathcal{T} = \mathcal{D}$）开始，ML-DT通过识别特征以及根据上式获取的最大信息增益来确认相应的分割点，然后生成关于$\mathcal{T} ^{-}$和$\mathcal{T} ^{+}$的两个子节点。通过将$\mathcal{T} ^{-}$或$\mathcal{T} ^{+}$作为新的根节点，递归地调用上述过程，直到满足一些停止准则$\mathcal{C}$才不进行上述过程（即子节点的大小小于预设的阈值）。

要实例化ML-DT，那么就需要有计算多标签熵（即$MLEnt\left ( \cdot \right )$）的方法。一个简单的方法是把每个相关子标签集$Y \in \mathcal{Y}$作为一个新类，然后使用传统的单标签熵来计算多标签熵：

\begin{align}
    \begin{matrix}
        \widehat{MLEnt\left ( \mathcal{T} \right )}=-\sum \mathbb{P}\left ( Y \right )\cdot \log_{2}\left ( \mathbb{P}\left ( Y \right ) \right )\left ( Y \subseteq \mathcal{Y} \right )\\ 
        where \ \mathbb{P}\left ( Y \right )=\frac{\sum_{i=1}^{n}\left \| Y_{i} = Y \right \|}{n}
    \end{matrix}
\end{align}

然而，由于新类数量的增长与$\left | \mathcal{Y} \right |$的空间大小相关，并且呈指数增长，但其中许多新类甚至不会出现在$\mathcal{T}$中，因此这些新类就具有可以被忽略的估计概率（即$\mathbb{P}\left ( Y \right ) = 0$）。为了避免这个问题，ML-DT假设标签之间是相互独立的，并且使用可分解的方式计算多标签的熵：

\begin{align}
    \begin{matrix}
        MLEnt\left ( \mathcal{T} \right ) = \sum_{j=1}^{q} - p_{j}\log_{2}p_{j}-\left ( 1-p_{j} \right )\log_{2}\left ( 1 - p_{j} \right )\\ 
        where \ p_{j} = \frac{\sum_{i=1}^{n}\left \| y_{i} \in Y_{i} \right \|}{n}
    \end{matrix}
\end{align}

其中，$p_{j}$表示在$\mathcal{T}$中与标签$y_{j}$相关的实例数量的占比。需要注意的是$MLEnt\left ( \mathcal{T} \right )$可以被认为是在标签相互独立假设下的$\widehat{MLEnt\left ( \mathcal{T} \right )}$简化版，并且$MLEnt\left ( \mathcal{T} \right ) > \widehat{MLEnt\left ( \mathcal{T} \right )}$。

对于未知的实例$x$，它验证对应路径遍历学习到的决策树，直到到达与多个训练实例$\mathcal{T} \subseteq \mathcal{D}$相关的叶节点。相应的预测标签集为：

\begin{align}
    Y=\left \{ y_{j} \mid p_{j} > 0.5, 1 \leq j \leq q \right \}
\end{align}

换句话说，假如进入某个叶节点的训练实例都与标签$y_{j}$相关，就认为该叶节点与标签$y_{j}$相关，那么在该叶节点中的所有测试实例就都与标签$y_{j}$相关。

评论：ML-DT是在计算多标签熵时，假设标签之间相互独立情况下的一阶方法。ML-DT的一个显著特点是它能够高效的从多标签数据中获得决策树模型。未来，ML-DT可以从剪枝策略或集成学习技术方面进行改进。

{% asset_img ml-dt.png 多标签决策树 %}

### Rank Support Vector Machine（Rank-SVM）

该算法的基本思想是使用最大边缘策略来处理多标签数据，即其中一组线性分类器被优化为最小化经验排序损失，并且使用核技巧处理非线性问题。

> SVM的说明：

假设学习系统由$q$条线性分类器$\mathcal{W}=\left \{ \left ( \omega_{j}, b_{j} \right ) \mid 1 \leq j \leq q \right \}$组成，其中$\omega_{j} \in \mathbb{R}^{d}$和$b_{j} \in \mathbb{R}$是第$j$类标签$y_{j}$的权重向量和偏差。因此，Rank-SVM通过考虑实例在相关和不相关标签上的排序能力来定义学习系统在实例$x_{i}$的相关标签$Y_{i}$（即$\left ( x_{i},Y_{i} \right )$）上的边界。

\begin{align}
    \underset{\left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}}{min}\frac{\left \langle \omega_{j}-\omega_{k},x_{i} \right \rangle + b_{j} - b_{k}}{\left \| \omega_{j}-\omega_{k} \right \|}
\end{align}

其中，$\left \langle u, \upsilon \right \rangle$返回内积$u^{\top}\upsilon$。从几何的角度说，每一个相关与不相关的标签对$\left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}$，它们的判别边界都对应于一个超平面$\left \langle \omega_{j}-\omega_{k},x \right \rangle + b_{j} - b_{k} = 0$。因此，上式考虑$x_{i}$到每个相关、不相关标签对超平面距离的$L_{2}$，并且返回在实例$x_{i}$的相关标签$Y_{i}$上的最小边界。因此，在整个训练集$\mathcal{D}$上的学习系统边界就为：

\begin{align}
    \underset{\left ( x_{i}, Y_{i} \right ) \in \mathcal{D}}{min} \ \underset{\left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}}{min}\frac{\left \langle \omega_{j}-\omega_{k},x_{i} \right \rangle + b_{j} - b_{k}}{\left \| \omega_{j}-\omega_{k} \right \|}
\end{align}

> 超平面的说明：一种忽略空间维数的线性函数。

当学习系统能够为训练集中实例的每个相关和不相关的标签进行正确排序时，上式就会返回正边界。在这种理想状态下，就可以对线性分类器进行重新缩放：

1、$\forall 1 \leq i \leq m \ and \ \left ( y_{i},y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}, \ \left \langle \omega_{j} - \omega_{k}, x_{i} \right \rangle + b_{j} - b_{k} > 1$
    
2、$\exists i^{*} \in \left \{ 1, \cdots , m \right \} \ and \ \left ( y_{j^{*}},y_{k^{*}} \right ) \in Y_{i^{*}} \times \bar{Y_{i^{*}}}, \ \left \langle \omega_{j^{*}} - \omega_{k^{*}}, x_{i^{*}} \right \rangle + b_{j^{*}} - b_{k^{*}} > 1$

因此，在整个训练集$\mathcal{D}$上的学习系统的最大边界就可以表示为：

\begin{align}
    \begin{matrix}
        \underset{\mathcal{W}}{max} \ \underset{\left ( x_{i}, Y_{i} \right ) \in \mathcal{D}}{min} \ \underset{\left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}}{min} \ \frac{1}{\left \| \omega_{j}-\omega_{k} \right \|^{2}}\\ 
        subject \ to : \left \langle \omega_{j} - \omega_{k}, x_{i} \right \rangle + b_{j} - b_{k} \geq 1 \ \left ( 1 \leq i \leq m, \left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}} \right )
    \end{matrix}
\end{align}

假设有足够的训练样本，因此对于每个标签对$\left ( y_{j}, y_{k} \right )\left ( j \neq k \right )$，都存在$\left ( x, Y \right ) \in \mathcal{D}$满足$\left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}$。因此，上式就等价于$max_{\mathcal{W}} \ min_{1 \leq j \leq k \leq q}\frac{1}{\left \| \omega_{j}-\omega_{k} \right \|^{2}}$，并且可以把问题优化后写为：

\begin{align}
    \begin{matrix}
        \underset{\mathcal{W}}{min} \ \underset{1 \leq j \leq k \leq q}{max} \ \left \| \omega_{j} - \omega_{k} \right \|^{2}\\ 
        subject \ to : \left \langle \omega_{j} - \omega_{k}, x_{i} \right \rangle + b_{j} - b_{k} \geq 1 \ \left ( 1 \leq i \leq m, \left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}} \right )
    \end{matrix}
\end{align}

为了克服$max$操作带来的困难，Rank-SVM选择$sum$操作才逼近$max$来简化上式：

\begin{align}
    \begin{matrix}
        \underset{\mathcal{W}}{min} \sum_{i=1}^{q} \left \| \omega_{j} \right \|^{2}\\ 
        subject \ to : \left \langle \omega_{j} - \omega_{k}, x_{i} \right \rangle + b_{j} - b_{k} \geq 1 \ \left ( 1 \leq i \leq m, \left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}} \right )
    \end{matrix}
\end{align}

为了解决上式中的约束在实际情况下不能得到满足，因此在上式中引入了松弛变量：

\begin{align}
    \begin{matrix}
        \underset{\left \{ \mathcal{W}, \Xi \right \}}{min}\sum_{j=1}^{q}\left \| \mathcal{W}_{j} \right \|^{2} + C\sum_{i=1}^{m}\frac{1}{\left | Y_{i} \right |\left | \bar{Y_{i}} \right |}\underset{\left ( y_{j},y_{k} \right ) \in Y_{i} \times \bar{Y_{i}}}{\sum }\xi_{ijk}\\ 
        subject \ to : \left \langle \omega_{j} - \omega_{k}, x_{i} \right \rangle + b_{j} - b_{k} \geq 1 - \xi_{ijk},\ \xi_{ijk} \geq 0 \left ( 1 \leq i \leq m, \left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}} \right )
    \end{matrix}
\end{align}

其中，$\Xi = \left \{ \xi_{ijk} \mid 1 \leq i \leq m, \left ( y_{j}, y_{k} \right ) \in Y_{i} \times \bar{Y_{i}} \right \}$是一个松弛变量集。上式中的目标由权衡参数$C$平衡的两部分组成。具体而言，第一部分对应于学习系统的边界，而第二部分对应于以铰链形式实现的学习系统替代了以排序损失实现的学习系统。注意，替代排序损失也可以用其他方式来实现，例如神经网络的全局误差函数的指数形式。

> 上面一段都没动的说明：

需要注意的是，上式是一个具有凸目标和线性约束的标准二次规划（QP）问题，可以用现成的QP求解器进行求解。此外，为了使Rank-SVM具有非线性分类能力，一种常用的方法是通过核技巧求解上式的对偶形式。此外，Rank-SVM还可以使用stacking来设置阈值函数：

\begin{align}
    \begin{matrix}
        t\left ( x  \right ) = \left \langle \omega^{*}, f^{*}\left ( x \right ) \right \rangle + b^{*}\\ 
        where \ \ f^{*}\left ( x \right ) = \left ( f\left ( x, y_{1} \right ), \cdots , f\left ( x, y_{q} \right ) \right )^{T} \ and \ f\left ( x, y_{j} \right ) = \left \langle \omega_{j}, x \right \rangle + b_{j}
    \end{matrix}
\end{align}

然后对于未知实例$x$，预测的标签集为：

\begin{align}
    Y = \left \{ y_{j} \mid \left \langle \omega_{j}, x \right \rangle + b_{j} > \left \langle \omega^{*}, f^{*}\left ( x \right ) \right \rangle + b^{*}, 1 \leq j \leq q \right \}
\end{align}  

评论：Rank-SVM是一个在相关和不相关标签对上定义了超平面边界的二阶方法。该算法得益于核技术在处理非线性分类上的优势，并且基于此可以进一步的实现相关变体。首先，上式中的经验排序损失可以用其他损失结构来代替，如汉明损失。它可以被归类为结构化输出分类的一般形式。其次，阈值策略可以用stacking以外的技术来实现。第三，为了避免核技术在选择的问题，因此在多标签数据中可以使用多核技术来进行学习。

{% asset_img rank-svm.png Rank-SVM %}

### Collective Multi-Label Classifier（CML）

该算法的基本思想是适应最大熵原理处理多标签数据，其中标签之间的相关性被编码为所得到的结果，其分布必须满足的约束。

对于任意的对标签实例$\left ( x, Y \right )$，让$\left(x, y\right)$使用二元标签向量$y = \left ( y_{1}, y_{2}, \cdots , y_{q} \right )^{T} \in \left \{ -1, +1 \right \}^{q}$来表示相应的随机变量，其中第$j$个分量表示$Y$是否包含第$j$个标签，即$\left(y_{j} = +1\right)$表示包含，$\left(y_{j} = -1\right)$表示不包含。从统计上讲，多标签学习任务可以等价于学习联合概率分布$p\left(x, y\right)$。

假设给定他们的分布$p\left ( \cdot ,\cdot  \right )$，那么$\mathcal{H}_{p}\left(x, y\right)$就表示实例与标签$\left(x, y\right)$的熵。最大熵的原理是假定当前知识状态的最佳分布模型是一个服从与给定事实集合$\mathcal{K}$的最大$\mathcal{H}_{p}\left(x, y\right)$。

\begin{align}
    \begin{matrix}
        \underset{p}{max} \ \mathcal{H}_{p}\left ( x, y \right )\\ 
        subject to: \ \mathbb{E}_{p}\left [ f_{k}\left ( x, y \right ) \right ] = F_{k} \ \left ( k \in \mathcal{K} \right )
    \end{matrix}
\end{align}

一般来说，事实被表达为在实例与标签$\left(x, y\right)$某个函数期望的约束，即约束为$\mathbb{E}_{p}\left [ f_{k}\left ( x, y \right ) \right ] = F_{k}$。这里的$\mathbb{E}_{p}\left(\cdot\right)$表示$p\left( \cdot, \cdot\right)$的期望算子，而$F_{k}$则表示训练集估计的期望值，如$\frac{1}{m} \sum_{\left ( x, y \right ) \in \mathcal{D}}f_{k}\left ( x, y \right )$。

利用标准拉格朗日乘数法，结合$p\left( \cdot, \cdot\right)$上的归一化约束（即$\mathbb{E}_{p}\left[1\right] = 1$），可以求解方程（51）的约束优化问题。因此，最优分布落在了Gibbs分布族上。

\begin{align}
    p\left ( y \mid x \right ) = \frac{1}{Z_{\Lambda}\left ( x \right )}exp\left ( \sum_{k \in \mathcal{K}} \lambda_{k} \cdot f_{k}\left ( x, y \right ) \right )
\end{align}

> Gibbs的说明：

这里的$\Lambda = \left \{ \lambda_{k} \mid k \in \mathcal{K} \right \}$是一组待确定的参数集合，并且$Z_{\Lambda}\left ( x \right )$是归一化因子的分区函数，即：

\begin{align}
    Z_{\Lambda}\left ( x \right ) = \sum_{x}exp\left ( \sum_{k \in \mathcal{K}} \lambda_{k} \cdot f_{k}\left ( x, y \right ) \right )
\end{align}

通过假设高斯先验（即$\lambda_{k} \sim \mathcal{N}\left ( 0, \varepsilon^{2} \right )$）。通过最大化对数后验概率函数可以找到$\Lambda$中的参数。

\begin{align}
    \begin{matrix}
        l\left ( \Lambda \mid \mathcal{D} \right ) = log\left ( \prod_{\left ( x, y \right ) \in \mathcal{D}} p\left ( y \mid x \right ) \right ) - \sum_{k \in \mathcal{K}}\frac{\lambda_{2}^{l}}{2\varepsilon^{2}}\\ 
        =\sum_{\left ( x, y \right ) \in \mathcal{D}}\left ( \sum_{k \in \mathcal{K}} \lambda_{k} \cdot f_{k}\left ( x, y \right ) - logZ_{\Lambda}\left ( x \right ) \right ) - \sum_{k \in \mathcal{K}}\frac{\lambda_{2}^{k}}{2\varepsilon^{2}}
    \end{matrix}
\end{align}

需要注意的是，等式（54）是$\Lambda$上的一个凸函数，其全局最大值（虽然不是封闭形式）可以由任何现成的无约束优化方法如BFGS找到。一般来说，大多数数值分析都需要$l\left ( \Lambda \mid \mathcal{D} \right )$的梯度。

\begin{align}
    \frac{\partial l\left ( \Lambda \mid \mathcal{D} \right )}{\partial \lambda_{k}}=\sum_{\left ( x,y \right ) \in \mathcal{D}}\left ( f_{k}\left ( x, y \right ) - \sum_{u}f_{k}\left ( x, y \right )p\left ( y \mid x \right ) \right )-\frac{\lambda_{k}}{\varepsilon^{2}} \ \left ( k \in \mathcal{K} \right )
\end{align}

对于CML，约束集通常包含两部分$\mathcal{K} = \mathcal{K}_{1} \cup \mathcal{K}_{2}$。具体来说，$\mathcal{K}_{1} = \left \{ \left ( l, j \right ) \mid 1 \leq l \leq d, 1 \leq j \leq q \right \}$使用$f_{k}\left ( x, y \right ) = x_{l} \cdot \left \| y_{j} = 1 \right \|\left ( k = \left ( l, j \right ) \in \mathcal{K}_{1} \right )$指定了全部$d \cdot q$个约束。此外，$\mathcal{K}_{2} = \left \{ \left ( j_{1}, j_{2}, b_{1}, b_{2} \right ) \mid 1 \leq j_{1} \leq j_{2} \leq q , b_{1}, b_{2} \in \left \{ -1, +1 \right \}\right \}$使用$f_{k}\left ( x, y \right ) = \left \| y_{j_{1}} = b_{1} \right \| \cdot \left \| y_{j_{2}} = b_{2} \right \| \left ( k = \left ( j_{1}, j_{2}, b_{1}, b_{2} \right ) \in \mathcal{K}_{2} \right )$指定了全部$4 \cdot \binom{q}{2}$个约束。实际上，$CML$的$\mathcal{K}$中的约束可以用其他变体方式指定。

对于位置的实例$x$，预测的标签集为：

\begin{align}
    Y = arg \ \underset{y}{max} \left ( y \mid x \right )
\end{align}

需要注意的是，使用$arg \ max$进行精确推论只适用于较小的标签集。否则需要使用剪枝技术来减少$arg \ max$的搜索空间，例如只考虑训练集中出现的标签集。

评论：CML是一个在每个标签对的相关性上增加了$\mathcal{K}_{2}$约束的二阶方法。CML所研究的二阶相关性比Rank-SVM更具一般性，因为Rank-SVM只考虑相关与不相关的标签对。作为条件随机场模型（CRF），CML倾向使用在等式（52）中使用条件概率分布$p\left ( y \mid x \right )$来进行分类。有趣的是，$p\left ( y \mid x \right )$可以使用多种方式进行分解，例如$p\left ( y \mid x \right ) = \prod_{j=1}^{q}p\left ( y_{j} \mid x, y_{1}, \cdots , y_{j-1} \right )$，其中的每一项可以由分类器链中的分类器来进行建模，或者$p\left ( y \mid x \right ) = \prod_{j=1}^{q}p\left ( y_{j} \mid x, pa_{j} \right )$，其中的每一项可以由有向图中的节点$y_{j}$和它的父节点$pa_{j}$进行建模，并且当有向图具有有限拓扑结构的多位贝叶斯网络时，该算法成立。有向图也可用于建模多故障诊断，其中$y_{j}$表示某一个设备部件的良好或故障状态。另一方面，已经有了一些多标签生成模型，其目的是模拟联合概率分布$p\left ( y \mid x \right )$。

{% asset_img cml.png CML %}

> CRF的说明：

# 总结

{% asset_img algorithms-reviewed.png 多标签学习算法综述 %}

图5总结了八种多标签学习算法的特性，包括基本思想、标签相关性、计算复杂性、已经测试域和优化（代理）指标。从图5可以知道，汉明损失和排序损失是最流行的度量指标之一，并且在前文也对其进行了理论分析。此外，值得注意的是，通过Random k-Labelsets优化的子集精度仅是对k-Labelsets而不是整个标签空间。

图5中的测试领域是该算法对应的原始文件中使用效果最好的数据类型。然而，所有这些有代表性的多标签学习算法都是通用的，并且可以应用于各种数据类型。然而，每种学习算法的计算复杂度对其适用于不同规模的数据起着关键的作用。这里，可以从三个主要方面来研究数据可伸缩性，包括训练实例的数量（即$m$）、维度（即$d$）和可能的标签类别数量（即$q$）。此外，当实例的维度远远大于类标签的数量时（即$d \gg q$），使用将标签类别作为实例空间的额外特征的策略将对算法不会有太多改善。

作为研究最多的监督学习框架，图5中的几种算法都采用二元分类器作为多标签数据学习的中间步骤。对二元分类的转换的最初来源于著名的AdaBoost.MH算法，在该算法中每个多标签训练实例$\left ( x_{i}, Y_{i} \right )$被转换成$q$个二元实例$\left \{ \left ( \left [ x_{i}, y_{j} \right ],\phi \left ( Y_{i}, y_{j} \right ) \right ) \mid 1 \leq j \leq q\right \}$。该算法被认为是一种高阶方法，其中$\mathcal{Y}$中的标签被认为是$\mathcal{X}$的附加特征，并且通过共享实例$x$把彼此之间进行关联，只要二元学习算法$\mathcal{B}$能够捕获特征之间的依赖关系。二元分类转换的其他方法可以通过诸如堆叠聚合或纠错输出码（ECOC）等技术来实现。

> AdaboSt.MH的说明：基于AdaBoost.M1算法的一种标签转化方案的多标签算法。

> 堆叠聚合的说明：

> ECOC的说明：能够将多类分类问题转化为多个二分类问题，而且利用纠错输出码本身具有纠错能力的特性，可以提高监督学习算法的预测精度。

此外，适应一阶算法的方法不能简单地认为是二元相关性与特定二元学习者的结合。例如，ML-KNN不仅仅是二元相关性与KNN的结合，而是利用贝叶斯推理与相邻信息进行推理。同时，ML-DT也不仅仅是二元相关性与决策树结合组成为单一决策树，而构建了$q$个决策树，以适应所有类别的标签（基于多标签熵）。

# 相关学习的设置

与多标签学习相关的学习设置有很多值得讨论的问题，如多实例学习、有序分类、多任务学习、数据流分类等。

在多实例学习研究的问题中每个示例是由一袋实例和一个（二元）标签来描述。只有当该袋实例中至少包含了一个正实例，那么该袋就被认为是正例。与输出（标签）空间具有对象歧义（复杂语义）的多标签学习模型相比，多实例学习可以被认为是在输入（实例）空间具有对象的歧义性的模型。目前在多标签数据中多实例学习已经有了一些初步的尝试。

有序分类研究的问题是对所有类别的标签进行自然排序。在多标签学习中，可以对每类标签的相关性进行排序，以便将各个标签（$y_{j} \in \left \{ -1, +1 \right \}$）进行分级归类（$y_{j} \in \left \{ m_{1}, m_{2}, \cdots , m_{k} \right \} \ where \ m_{1} < m_{2} < \cdots < m_{k}$）。因此，分级多标签学习只能提供模糊的序号，而不是标签相关性的明确判断。现有的研究表明，分级多标签学习可以通过将其转化为一组有序分类问题（每类标签都是一个问题），或一组标准的多标签学习问题（每个成员级别是一个问题）来解决。

多任务学习主要研究的是多个任务并行训练的问题，利用相关任务的训练信息作为归纳偏差，以此来提高其他任务的泛化性能。然而，多任务学习与多标签学习之间存在着本质的区别。首先，在多标签学习中，所有的示例共享相同的特征空间，而在多任务学习中，任务可以在相同的特征空间，也可以在不同的特征空间。其次，多标签学习的目的是预测与对象相关联的标签子集，而多任务学习的目的是同时完成多个任务，并不关心哪个任务子集应该与哪个对象相关联（该学习算法中把标签认为是一个任务），因为它通常假定每个对象与所有任务相关。第三，在多标签学习中，处理标签空间较大的问题很常见，但在多任务学习中，考虑大量任务是不合理的。然而，多任务学习的技术可能可以用来帮助多标签学习。

数据流分类主要研究现实世界中的对象在线生成和实时处理的问题。目前，多标签的流媒体数据广泛存在于即时新闻、电子邮件、微博等现实场景中。作为流媒体数据分析的常见挑战，如何有效地对多标签数据流进行分类是处理概念漂移问题的关键因素。现有的数据流分类模型通过在新的一批示例到达时更新分类器来实现概念漂移。一般采用采用衰落假设，即随着时间的推移，过去数据的影响逐渐下降，或者每当检测到概念漂移时保持变化检测器报警。

# 结论

在本文中，回顾了最先进的多标签学习的范式、算法和相关的学习设置。此外，本文并没有尝试回顾所有的学习技术，而是选择八个最具代表性的多标签学习算法进行描述。在下表中总结了一些用于多标签学习的在线资源，包括学术活动（教程、研讨会、专题）、公开可用的软件和数据集。

{% asset_img online-resources.png 多标签学习的在线资源 %}

目前，虽然标签之间的相关性已经被用于各种多标签学习技术，但是在标签相关性的使用上还没有任何关于底层概念或任何原理性机制。最近的研究表明，标签之间的相关性可能是不对称的，即一个标签对另一个标签的影响不一定在相反的方向上是相同的，或者是局部相同，即不同的实例共享不同的标签相关性，很少有全局适用的相关性。然而，充分理解标签相关性，特别是对于具有较大输出空间的场景，仍然是多标签学习的目标。

在第三节重点介绍多标签学习算法的特性。这篇综述的一个对不同多标签学习算法优缺点的深入研究的补充。在文献中，我们可以找到一个最近尝试进行的具有广泛性的实验比较，即12个多标签学习算法与16个评价指标进行了比较。有趣的是，分类和排名指标最佳的算法是基于集成学习技术，即预测决策树的随机森林。然而，在更广泛的范围内或集中在某个类型内模型比较是值得进一步探讨的课题。
