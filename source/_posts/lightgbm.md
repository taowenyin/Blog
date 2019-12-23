---
title: 【2017】LightGBM：A Highly Efficient Gradient Boosting Decision Tree
date: 2019-11-12 21:18:48
mathjax: true
updated: {{ date }}
tags: [集成学习, 机器学习]
categories: [论文]
---

# 1.介绍

以往的GBDT中，当数据特征纬度较高、数据量较大时，其算法的效率和可扩展性都不是很理想，主要原因是需要针对每一个特征的信息增益来确认其分割特征点。为了解决这个问题，作者提出两个新方法，分别是GOSS（Gradient-based One-Side Sampling，基于梯度的单边采样）和EFB（Exclusive Feature Bundling，互斥特征捆绑）。

1、GOSS：**核心思想是把很大一部分小梯度的排除在计算信息增益之外，而主要使用具有大梯度的数据来计算信息增益。**原因是具有大梯度的特征在计算信息增益时扮演了更为主要的角色，因此GOSS可以在小数据量的情况下获取较为精确的信息增益估计。此外，在采样率的情况下，且信息增益的取值范围较大时，这种方法相较于均匀随机采样可以得到得到更精确的增益估计。

2、EFB：**核心思想是把相互排斥的特征（即很少同时取相同值）捆绑在一起，从而减少特征的数量。**要找到精确的互斥特征绑定是一个NP难问题，但是通过贪心算法进行实现很好的近似，从而实现在特征数量最少的同时，也不影响分割点确定的准确性，该问题可以转化为`图着色问题`。

# 2.准备工作

## 2.1GBDT和复杂度分析

GBDT的主要性能瓶颈在于学习决策树，而学习决策树最耗时的是寻找最佳分割点。目前最受欢迎的寻找最优分割点的算法有两个，分别是pre-sorted算法（预排序算法）和histogram-based算法（基于直方图的算法）。

1、pre-sorted算法（预排序算法）：该算法在预排序的特征值上枚举了所有可能的分割点。该算法能够找到最优分割点，但是在训练速度和内存损耗上效率不高。

2、histogram-based算法（基于直方图的算法）：不同于pre-sorted算法，histogram-based算法不需要在分类后的特征值上寻找分割点，而是将连续的特征值存储到离散的存储箱（bins）中，并在训练过程中利用这些存储箱（bins）构造每个叶节点的特征直方图。**由于histogram-based算法在内存损耗和训练速度上更为高效，因此作者采用这种方法来寻找最佳分割点。**该算法的构建直方图的复杂度是$O\left ( \#data \times \#feature \right )$，最佳分割点的复杂度是$O\left ( \#bin \times \#feature \right )$，其中由于bin的数量通常比数据data要小很多，所以分割点寻找的复杂度主要在直方图的构建，那么如果能够降低数据或者特征，那么就可以大大提高GBDT的训练速度。

## 2.2相关工作

GBDT的实现已经有许多，如XGBoost、pGBRT、Scikit-learn，以及GBM。其中使用R语言实现的Scikit-learn和GBM中，寻找最优分割点就采用的pre-sorted算法，而在pGBRT中则是histogram-based算法，XGBoost同时支持两种算法。由于XGBoost是其他算法中最为出色的，因此作者采用XGBoost作为验证的基准。

为了降低训练数据的大小，通常的做法是对数据进行下采样，把权重小于固定阈值的数据进行过滤。例如，在SGB中每次迭代中使用随机子集来训练弱学习器，或在训练过程中采用动态采样率。然而，除了SGB外，这些工作都是基于AdaBoost，不能够直接用于GBDT，因为GBDT在初始化时数据没有相应的权重。虽然SGB能够支持GBDT，但是它会是GBDT损失经度，所以也不是一个理想的选择。

类似的，为了降低训练集中特征的数量，通常的做法是过滤掉相对较弱的特征，如利用PCA（主成分分析）或者PP（投影追踪）来完成。但是，这些方法主要依赖于特征所包含的冗余信息，而在实践中这些做法并不总是正确，因为所谓特征就是具有独特的性质，而如果直接删除，那么都会在一定程度上影响训练的效果。

在实际应用中，大规模数据往往非常稀疏。采用pre-sorted算法的GBDT通过忽略数据值为0的特征来降低训练成本，但是采用histogram-based算法的GBDT就不能针对稀疏问题进行有效的解决，这是因为histogram-based算法不管数据的特征值是0还是非0都需要检索特征箱中的值，而基于histogram-based算法的GBDT正是利用这一稀疏特性。

所以针对以往研究的局限性，提出了两种新方法，分别是基于梯度的单边采样（GOSS）和互斥特征捆绑（EFB），算法如图1。

{% asset_img goss-efb.png GOSS和EFB的算法 %}

<center>图1：GOSS和EFB的算法</center>

# 3.GOSS（Gradient-based One-Side Sampling，基于梯度的单边采样）

使用GOSS在进行采样时对小梯度的数据只进行部分采样，因此为了补偿减少数据造成的分布影响，在计算信息增益时，GOSS对小梯度数据引入了一个常数乘法器，具体方法如下：

1、GOSS首先把数据按照它们的梯度绝对值进行排序。

2、选择前面$a \times 100\%$的数据。

3、在剩余的数据中随机选取$b \times 100\%$的数据。

4、在计算信息增益时将具有小梯度的数据放大$\frac{1-a}{b}$倍。

通过GOSS，就可以把模型的更多注意力放在梯度较大、训练不足的数据上，并且不会改变原始数据的分布。

# 3.1原理分析

GBDT使用决策树来得到一个函数，该函数把输入空间$\mathcal{X}^{\mathcal{S}}$转化为一个梯度空间$\mathcal{G}$。假设有一个服从`独立同分布`的具有$n$个实例的训练集$\left \{ x_{1}, x_{2}, \cdots , x_{n} \right \}$，其中$x_{i}$是一个在$\mathcal{X}^{\mathcal{S}}$空间中具有$s$维的向量。在每次梯度增强的迭代中，模型损失函数的负梯度就可以表示为$\left \{ g_{1}, g_{2}, \cdots , g_{n} \right \}$。决策树通过最大信息增益来把模型分为不同的节点。在GBDT中，信息增益的计算常常通过分裂后的方差来获得。

**定义：**假设$O$是训练集在决策树中的固定节点。此节点上点$d$处的分割特征$j$的方差增益可以定义为：

\begin{align}
    \begin{matrix}
        V_{j \mid O}\left ( d \right )=\frac{1}{n_{O}}\left ( \frac{\left ( \sum_{\left \{ x_{i} \in O\ :\ x_{xj} \leq d \right \}}g_{i} \right )^{2}}{n_{l \mid O}^{j}\left ( d \right )} + \frac{\left ( \sum_{\left \{ x_{i} \in O\ :\ x_{xj} > d \right \}}g_{i} \right )^{2}}{n_{r \mid O}^{j}\left ( d \right )} \right )\\ 
        where\ n_{O}=\sum I\left [ x_{i} \in O \right ]\\ 
        \ n_{l \mid O}^{j}\left ( d \right )=\sum I\left [ x_{i} \in O\ :\ x_{ij} \leq d \right ]\ and\ n_{r \mid O}^{j}\left ( d \right )=\sum I\left [ x_{i} \in O\ :\ x_{ij} > d \right ]
    \end{matrix}
\end{align}

上式中的$\frac{\left ( \sum_{\left \{ x_{i} \in O\ :\ x_{xj} \leq d \right \}}g_{i} \right )^{2}}{n_{l \mid O}^{j}\left ( d \right )}$表示左支的平均信息增益，$\frac{\left ( \sum_{\left \{ x_{i} \in O\ :\ x_{xj} > d \right \}}g_{i} \right )^{2}}{n_{r \mid O}^{j}\left ( d \right )}$表示右支的平均信息增益，最后求得以节点$d$和特征点$j$为分割点的平均方差增益。

对于特征$j$来说，决策树选择$d_{j}^{\ast}=argmax_{d}V_{j}\left ( d \right )$节点，并且计算最大增益$V_{j}\left ( d_{j}^{\ast} \right )$。然后，数据以$d_{j\ast}$节点处的$j^{\ast}$个特征进行分割为左右两个子节点。

在GOSS方法中：

1、把训练集中的所有数据按照梯度的绝对值进行降序排列。

2、保留最大梯度的前$a\%$，并得到一个子集$A$。

3、对于剩余数据集$A^{c},\ \left ( 1-a \right ) \times 100\%$中的实例都是具有较小梯度，作者随机从该数据集中采样一个子数据集$B$，该子数据集的大小为$b \times \left | A^{c} \right |$。

4、最后作者在$A \cup B$的集合中求方差增益$\tilde{V_{j}}\left ( d \right )$来对数据进行分割。

\begin{align}
    \begin{matrix}
        \tilde{V_{j}}\left ( d \right ) = \frac{1}{n}\left ( \frac{\left ( \sum_{x_{i} \in A_{l}}g_{i} + \frac{1-a}{b}\sum_{x_{i} \in B_{l}} g_{i} \right )^{2}}{n_{l}^{j}\left ( d \right )} + \frac{\left ( \sum_{x_{i} \in A_{r}}g_{i} + \frac{1-a}{b}\sum_{x_{i} \in B_{r}} g_{i} \right )^{2}}{n_{r}^{j}\left ( d \right )} \right )\\ 
        where \ A_{l} = \left \{ x_{i} \in A\ :\ x_{ij} \leq d \right \} \\ 
        A_{r} = \left \{ x_{i} \in A\ :\ x_{ij} > d \right \} \\ 
        B_{l} = \left \{ x_{i} \in B\ :\ x_{ij} \leq d \right \} \\ 
        B_{r} = \left \{ x_{i} \in B\ :\ x_{ij} > d \right \}
    \end{matrix}
\end{align}

由于对数据进行了部分采样，因此系数$\frac{1-a}{b}$的作用是把$B$的梯度规则化到$A^{c}$大小。因此GOSS中，作者使用一个较小的数据集来计算得到$\tilde{V_{j}}\left ( d \right )$，而不是使用所有的实例来精确的获取精确的分割点$V_{j \mid O}\left ( d \right )$，从而大幅减少计算的成本。同时，结果证明了GOSS不会损失太多的训练精度，并且结果优于随机采样。

**定理：**假设GOSS中的近似误差为$\varepsilon \left ( d \right ) = \left | \tilde{V_{j}}\left ( d \right ) - V_{j \mid O}\left ( d \right ) \right |$，并且左梯度$\bar{g}_{l}^{j}\left ( d \right )=\frac{\sum_{x_{i} \in \left ( A \cup A^{c} \right )_{l}}\left | g_{i} \right |}{n_{l}^{j}\left ( d \right )}$，右梯度$\bar{g}_{r}^{j}\left ( d \right )=\frac{\sum_{x_{i} \in \left ( A \cup A^{c} \right )_{r}}\left | g_{i} \right |}{n_{r}^{j}\left ( d \right )}$。当概率至少为$1-\delta$时，就可以得到

\begin{align}
    \begin{matrix}
        \varepsilon \left ( d \right ) \leq C_{a,b}^{2}\ln\frac{1}{\delta} \cdot max\left \{ \frac{1}{n_{l}^{j}\left ( d \right )}, \frac{1}{n_{r}^{j}\left ( d \right )} \right \}+2DC_{a,b}\sqrt{\frac{\ln\frac{1}{\delta}}{n}}\\ 
        where\ C_{a,b}=\frac{1-a}{\sqrt{b}}max_{x_{i} \in A^{c}}\left | g_{i} \right |\ and \ D=max\left ( \bar{g}_{l}^{j}\left ( d \right ),\bar{g}_{r}^{j}\left ( d \right ) \right )
    \end{matrix}
\end{align}

根据这个定理，可以得到以下推论：

1、GOSS的逼近比为$\mathcal{O}\left ( \frac{1}{n_{l}^{j}\left ( d \right )} + \frac{1}{n_{r}^{j}\left ( d \right )} + \frac{1}{\sqrt{n}}\right )$。假如决策树分裂的不太平衡时（例如，$n_{l}^{j}\left ( d \right ) \geq \mathcal{O}\left ( \sqrt{n} \right )$并且$n_{r}^{j}\left ( d \right ) \geq \mathcal{O}\left ( \sqrt{n} \right )$），近似误差主要有上式中的第二项为主，并且当$n \rightarrow \infty $时$\mathcal{O}\left ( \sqrt{n} \right )$趋近于0。这就意味着当数据量很大时，近似值非常准确。

2、在GOSS中，随机采样的一个特殊情况就是$a = 0$。在大多情况下，GOSS可以在$C_{0, \beta} > C_{a , \beta - a}$的性能优于随机采样，该条件等价于当$\alpha_{a} = \frac{max_{x_{i} \in A \cup A^{c}}\left | g_{i} \right |}{max_{x_{i} \in A^{c}}\left | g_{i} \right |}$时，$\frac{\alpha_{a}}{\sqrt{\beta}} > \frac{1-a}{\sqrt{\beta - a}}$。

作者认为在GOSS中泛化误差为$\varepsilon_{gen}^{GOSS}\left ( d \right )=\left | \tilde{V}_{j}\left ( d \right )-V_{\ast}\left ( d \right ) \right |$，即通过GOSS对训练集进行抽样后计算得到的方差增益与真实分布计算得到的真实方差增益之间的差距。作者推导得到$\varepsilon_{gen}^{GOSS}\left ( d \right ) \leq \left | \tilde{V}_{j}\left ( d \right )-V_{j}\left ( d \right ) \right | + \left | V_{j}\left ( d \right )-V_{\ast}\left ( d \right ) \right | \triangleq \varepsilon_{GOSS}\left ( d \right ) + \varepsilon_{gen}\left ( d \right )$，因此如果GOSS近似是正确的，那么GOSS的泛化误差将接近于使用全部数据计算得到的泛化误差。此外，抽样会增加基础学习者的多样性，这可能有助于提高泛化性能

# 4.EFB（Exclusive Feature Bundling，互斥特征捆绑）

{% asset_img gb-mef.png 贪婪绑定和合并互斥特征的算法 %}

<center>图2：贪婪绑定和合并互斥特征的算法</center>

高位数据通常非常稀疏，并且特征空间的稀疏性就给了作者设计一个接近无损降低特征数量的可能。具体来说，在稀疏特征空间中，许多特征是互斥的，即它们从不同时取非零值，因此就可以安全的把这些互斥特征绑定到一个特征上。通过一个精心设计的特征搜索算法，就可以从特征集中构建与单个特征相同的特征直方图。使用这种方法，直方图构建的复杂度就由$O\left ( \#data \times \#feature \right )$变为$O\left ( \#data \times \#bundle \right )$，并且$\#bundle \ll \#feature$。从而在不影响精度的前提下，显著提升GBDT的训练速度。**在该算法中主要有两个问题需要解决，分别是如何确定哪些特性需要被绑定在一起和如何构造绑定的特征包。**

**定理：**将特征划分为最小数量的互斥包是NP难问题。

**证明：**该问题可以归结为图着色问题。由于图着色问题是NP-难问题，因此可以推导出将特征划分为最小数量的互斥包也是NP难问题。

给定任意图着色问题的实例$G=\left ( V,E \right )$，通过实例$G$中的每一个特征得到一个关联矩阵$\left | V \right |$，该矩阵中每一行都是一个特征。很容易看出，在该问题中，一个互斥特征集对应于一组具有相同颜色的顶点，反之亦然。

1、对于**如何确定哪些特性需要被绑定在一起的问题（算法3）**而言：在上面的定理中已经表明寻找最有特征集是一个NP-难问题，这也就表明要在有限时间内找到解是不可能的。为了找到一个较好的近似算法，作者首先将特征绑定定义为图着色问题，如果两个特征不是互斥，那么就以特征为顶点，并为两个特征之间添加一条边。然后使用贪心算法，从而实现图着色到特征邦定集的良好结果（具有恒定的近似比）。此外，还有有相当多的特性，并不是完全互斥，但也很少有同时为非零的情况。如果该算法允许少量的冲突，那么就可以得到更少的特征集，从而进一步提高计算效率。通过简单的计算，随机地改变一小部分特征值最多影响训练的精度为$\mathcal{O}\left ( \left [ \left ( 1-\gamma \right )n \right ]^\frac{-2}{3} \right )$，其中$\gamma$是每个个特征集中的最大冲突率（即不是完全互斥的比例）。因此，如果选择一个相对较小的$\gamma$，就将能够在精度和效率之间达到一个很好的平衡。

基于上面的结论，作者为互斥特征集设计了算法3，具体流程如下：

（1）构建一个带加权边的图，其权值对应于特征之间的总冲突。

（2）按照特征在图中的度进行降序排列。

（3）检查有序列表中的每个特征，并小于一个较小的冲突值时就（由$\gamma$控制）把其分配给现有的特征集，反之则创建一个新的特征集。

算法3的时间复杂度为$O\left ( \#feature^{2} \right )$，并且在训练前只运行一次。当特征的数量不是很大时，这种复杂性是可以接受的，但如果有数百万个特征，仍然可能受到影响。为了进一步提高效率，作者提出在不构建图的情况下更为有效的排序策略：

**按非零值计数排序，这与按度排序相似，因为非零值越多，冲突的概率就越高。**

2、对于**如何构造绑定的特征包的问题（算法4）**而言：这个问题的关键是如何保证在新的特征集中可以识别出原始特征。由于在直方图算法中存储了离散的特征集，而不是连续的特征值，因此我们可以通过**在不同的特征集中放置互斥特征来构造特征集，并且通过向原始特征值添加偏移来完成**。

（1）假设在一个特征集中有两个特征，其中原始特征A的取值为[0, 10)，原始特征B的取值为[0, 20)。

（2）在特征B中添加一个偏移量10，使得特征B变为[10, 30)。

（3）这样就可以安全的把特征A和B进行合并，从而形成一个新的特征集来代替原始的特征A和B，取值范围为[0, 30]。

EFB算法可以将大量的互斥特征捆绑到较少的特征集上，从而有效地避免了对零特征值的不必要计算。此外，也可以为每个特征使用一个表来记录具有非零值的数据，从而忽略零特征值来直方图的算法。通过在表中寻找数据可以把一个特征的直方图构建的计算成本从$O\left ( \#data \right )$变为$O\left ( \#non-zero-data \right )$。但是，这种方法需要额外的内存和计算成本来维护整个树生长过程中的每个特征表，并且这个优化并不与EFB相冲突，当特征集稀疏时仍然可以使用EFB。

# 参考资料

[LightGBM原理分析](https://zhuanlan.zhihu.com/p/38516467)

[开源 | LightGBM：三天内收获GitHub 1000  星](https://www.msra.cn/zh-cn/news/features/lightgbm-20170105)

[如何看待微软新开源的LightGBM?](https://www.zhihu.com/question/51644470)

[从结构到性能，一文概述XGBoost、Light GBM和CatBoost的同与不同](https://zhuanlan.zhihu.com/p/34698733)

[Lightgbm基本原理介绍](https://blog.csdn.net/qq_24519677/article/details/82811215)

[图着色问题](https://baike.baidu.com/item/%E5%9B%BE%E7%9D%80%E8%89%B2%E9%97%AE%E9%A2%98)