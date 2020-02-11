---
title: 【2016】XGBoost：A Scalable Tree Boosting System
date: 2019-11-12 21:18:48
mathjax: true
updated: {{ date }}
tags: [集成学习, 机器学习]
categories: [论文]
---

# 摘要

Tree Boosting是一个高效且被广泛使用的机器学习方法。在本文中，作者描述了一个名叫XGBoost的可扩展的端到端的Tree Boosting系统，该系统能够被数据科学家广泛的应用在许多机器学习的挑战中，从而获取最新的结果。作者为稀疏数据和加权分位数略图提出了一个全新的稀疏感知算法来近似树的学习。更重要的是，作者提供了有关缓存访问模式、数据压缩和分片的方法，以构建一个可伸缩的Tree Boosting系统。通过结合这些方法，XGBoost可以使用比现有系统少得多的资源来可扩展到有数十亿级实例的数据集。

# 介绍

在许多领域中，机器学习和数据驱动方法变得越来越重要。智能垃圾邮件分类器通过从大量垃圾邮件数据和用户反馈中学习来保护我们的电子邮件；广告系统学习将正确的广告与正确的上下文匹配；欺诈检测系统保护银行免受恶意攻击者的攻击；异常事件检测系统帮助实验物理学家找到导致新的物理事件。有两个重要的因素使得这些应用非常成功：使用了有效的统计模型来获取复杂数据之间的依赖关系，以及从大型数据集中根据感兴趣的部分生成模型，从而得到可伸缩的学习系统。

在机器学习方法的实际应用中，Gradient Tree Boosting是一种被广泛应用的技术。Tree Boosting方法已经证明在许多标签分类任务中能提供更好的结果。如使用Tree Boosting进行排序的算法LambdaMART在排序问题上获得了更好的结果。除了作为一个独立的预测工具外，Tree Boosting还被整合到现实世界中，如用于广告点击率预测。最后，Tree Boosting实际上是集成方法的一种，并被用于Netflix大奖等挑战赛中。

在本文中，作者描述了XGBoost方法，该方法是一个可伸缩的Tree Boosting学习系统。本系统是一个开源系统。该系统的影响已经被广泛认识到在一些机器学习和数据挖掘的挑战。以机器学习竞赛网站Kaggle举办的挑战赛为例。在2015年Kaggle博客上发布的29种获奖挑战解决方案中，有17种解决方案使用了XGBoost。在这些解决方案中，有8个单独使用XGBoost训练模型，而其他大多数解决方案则将XGBoost与神经网络集成在一起。为了进行比较，在11个解决方案中使用了第二种最受欢迎的方法，即深度神经网络。在2015年的KDDCup中也见证了该系统的成功，前十名中的每个获胜团队都使用XGBoost。此外，获胜的团队报告说，集成方法的性能仅比配置良好的XGBoost好一小部分。

这些结果表明，作者的系统在大多数问题上能获得更好的结果。这些成功解决方案中的问题包括：商店销售预测、高能物理事件分类、Web文本分类、客户行为预测、运动检测、广告点击率预测、恶意软件分类、产品分类、危险风险预测、大规模在线课程辍学率预测。虽然领域相关的数据分析和特征工程在这些解决方案中扮演着重要的角色，但是XGBoost是学习者的一致选择，这表明了我们的系统和Tree Boosting的影响性和重要性。

XGBoost成功背后最重要的因素是它在所有场景中的可伸缩性。该系统在单台机器上的运行速度是现有流行解决方案的十倍以上，并且在分布式或内存受限的设置中可扩展到数十亿个示例。XGBoost的可伸缩性归因于几个重要的系统和算法优化。这些创新包括：一个处理稀疏数据的全新的树学习算法；理论上合理的加权分位数略图程序能够处理近似树学习中的实例权重。并行和分布式计算使学习更快，从而使模型探索更快。更重要的是，XGBoost利用了核心计算，使数据科学家能够在桌面上处理数亿个示例。最后，将这些技术结合起来，使端到端系统能够以最少的集群资源扩展到更大的数据，这更令人兴奋。本文的主要贡献如下：

>* 作者设计了一个具有高扩展性的端到端的Tree Boosting学习系统。
>* 作者提出了一个理论上合理的加权分位数略图，用于处理近似树学习中的实例权重。
>* 作者引入了一个全新的用于并行树学习的稀疏感知算法。
>* 我们提出了一种有效的缓存感知块结构，用于树外学习。

尽管已有一些有关并行树增强的工作，但是尚未探索诸如核外计算，缓存感知和稀疏感知学习等方向。更重要的是，结合了所有这些方面的端到端系统为真实的案例提供了一种新颖的解决方案。这使数据科学家和研究人员能够构建功能强大的树木增强算法变体。除了这些主要贡献外，作者还在提出正则化学习目标方面做了额外的改进。

# Tree Boosting简介

本章作者将回顾Gradient Tree Boosting算法。推导结果与现有文献中Gradient Boosting推导方法相同。具体而言，二阶方法源自Friedman等人。作者在正则化目标上做了一些微改进，发现在实践中有帮助。

## 正则化学习目标

给定一个数据集，该数据集有$n$个实例和$m$个特征$\mathcal{D}=\left\{\left(\mathbf{x}_{i}, y_{i}\right)\right\}\left(|\mathcal{D}|=n, \mathbf{x}_{i} \in \mathbb{R}^{m}, y_{i} \in \mathbb{R}\right)$，一个集成树模型（如图1所示）$K$个加法函数来预测输出。

\begin{equation}
\hat{y}_{i}=\phi\left(\mathbf{x}_{i}\right)=\sum_{k=1}^{K} f_{k}\left(\mathbf{x}_{i}\right), \quad f_{k} \in \mathcal{F}
\end{equation}

其中，$\mathcal{F}=\left\{f(\mathbf{x})=w_{q(\mathbf{x})}\right\}\left(q: \mathbb{R}^{m} \rightarrow T, w \in \mathbb{R}^{T}\right)$是一个回归树空间（CART）。这里$q$表示实例映射到对应树的叶索引。$T$是每个棵树的叶节点数量。每个$f_{k}$对应一个独立的树结构$q$和权重为$w$的叶节点。与决策树不同，每个回归树在每个叶节点上都包含连续分数，作者使用$w_{i}$来表示第$i$个叶节点的分数。给定一个实例，作者使用树（通过$q$给定）中决策规则把一个实例分配到对应的叶节点上，并且通过对应的叶节点（通过$w$给定）求和来计算最终的预测值。要在学习模型中使用这些算法集，作者就最小化了以下正则化目标。

\begin{equation}
\begin{matrix}
\mathcal{L}(\phi)=\sum_{i} l\left(\hat{y}_{i}, y_{i}\right)+\sum_{k} \Omega\left(f_{k}\right)\\ 
\text { where } \Omega(f)=\gamma T+\frac{1}{2} \lambda\|w\|^{2}
\end{matrix}
\end{equation}

这里的$l$是一个可微的突损失函数，用于度量预测的$\hat{y}_{i}$和目标$y_{i}$的区别。第二个表达式中的$\Omega$是模型复杂度的惩罚项（例如回归树函数）。附加的正则项有助于平滑最终学习到的权重，以避免过度拟合。直观地说，正则化目标是为了选择一个简单预测函数的模型。在正则贪婪森林（RGF）模型中就使用了类似的正则化技术。我们的目标和相应的学习算法比RGF简单且易于并行化。当正则化参数为零时，目标回归到传统的梯度树增强。

{% asset_img tree-ensemble-model.png 集成树模型 %}

图1：集成树模型。实例的最终预测是所有所有树预测结果的和

## Gradient Tree Boosting

公式2的集成树模型包含使用函数作为参数，并且在欧式空间中不能使用传统的优化方法进行优化。相反，该模型是以加法的方式训练的。从形式上来说，让$\hat{y}_{i}^{(t)}$是第$i$个实例在$t$次迭代中预测，并且需要加上$f_{t}$从而最小化下面的目标函数。

$$\mathcal{L}^{(t)}=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t-1)}+f_{t}\left(\mathbf{x}_{i}\right)\right)+\Omega\left(f_{t}\right)$$

因此加上$f_{t}$修改公式（2）模型。通常二阶近似可用于快速优化目标。

$$\mathcal{L}^{(t)} \simeq \sum_{i=1}^{n}\left[l\left(y_{i}, \hat{y}^{(t-1)}\right)+g_{i} f_{t}\left(\mathbf{x}_{i}\right)+\frac{1}{2} h_{i} f_{t}^{2}\left(\mathbf{x}_{i}\right)\right]+\Omega\left(f_{t}\right)$$

其中$g_{i}=\partial_{\hat{y}^{(t-1)}} l\left(y_{i}, \hat{y}^{(t-1)}\right)$和$h_{i}=\partial_{\hat{y}}^{2}(t-1) l\left(y_{i}, \hat{y}^{(t-1)}\right)$是损失函数中的一阶和二阶统计。在$t$步，作者移除了常数项从而简化了目标函数。

\begin{equation}
\tilde{\mathcal{L}}^{(t)}=\sum_{i=1}^{n}\left[g_{i} f_{t}\left(\mathbf{x}_{i}\right)+\frac{1}{2} h_{i} f_{t}^{2}\left(\mathbf{x}_{i}\right)\right]+\Omega\left(f_{t}\right)
\end{equation}

定义$I_{j}=\left\{i | q\left(\mathbf{x}_{i}\right)=j\right\}$为叶节点$j$上的实例集。作者通过展开$\Omega$重写公式三得到。

\begin{equation}
\begin{aligned}
\tilde{\mathcal{L}}^{(t)} &=\sum_{i=1}^{n}\left[g_{i} f_{t}\left(\mathbf{x}_{i}\right)+\frac{1}{2} h_{i} f_{t}^{2}\left(\mathbf{x}_{i}\right)\right]+\gamma T+\frac{1}{2} \lambda \sum_{j=1}^{T} w_{j}^{2} \\
&=\sum_{j=1}^{T}\left[\left(\sum_{i \in I_{j}} g_{i}\right) w_{j}+\frac{1}{2}\left(\sum_{i \in I_{j}} h_{i}+\lambda\right) w_{j}^{2}\right]+\gamma T
\end{aligned}
\end{equation}

对于固定结构$q(\mathbf{x})$，作者通过下式计算得到在叶节点$j$上的最有权重$w_{j}^{*}$：

\begin{equation}
w_{j}^{*}=-\frac{\sum_{i \in I_{j}} g_{i}}{\sum_{i \in I_{j}} h_{i}+\lambda}
\end{equation}

并且通过下式计算对应的最优解

\begin{equation}
\tilde{\mathcal{L}}^{(t)}(q)=-\frac{1}{2} \sum_{j=1}^{T} \frac{\left(\sum_{i \in I_{j}} g_{i}\right)^{2}}{\sum_{i \in I_{j}} h_{i}+\lambda}+\gamma T
\end{equation}

公式6是评价评分函数，可用来衡量树结构$q$的质量。该分数类似于评估决策树的不纯度情况，不同的是它是通过更广泛的目标函数得出。图2展示了这个分数是如何计算的。

{% asset_img structure-score-calculation.png 树结构不纯度分数计算 %}

图2：树结构分数。我们只需要对每一片叶子的梯度和二阶梯度进行统计，然后应用评分公式得到质量分数。

通常不可能枚举所有可能的树结构$q$。取而代之的是一个贪心算法，它从一个叶子开始，迭代地向树中添加分支。假设$I_{L}$和$I_{R}$是节点左右两边的数据集。设$I=I_{L} \cup I_{R}$，那么分割后的损失减少量由下是给出：

\begin{equation}
\mathcal{L}_{s p l i t}=\frac{1}{2}\left[\frac{\left(\sum_{i \in I_{L}} g_{i}\right)^{2}}{\sum_{i \in I_{L}} h_{i}+\lambda}+\frac{\left(\sum_{i \in I_{R}} g_{i}\right)^{2}}{\sum_{i \in I_{R}} h_{i}+\lambda}-\frac{\left(\sum_{i \in I} g_{i}\right)^{2}}{\sum_{i \in I} h_{i}+\lambda}\right]-\gamma
\end{equation}

该公式通常在实践中用于评估拆分后的候选对象。

## 收缩率和列（特征）采样

除了2.1节的正则化目标函数外，还有两个额外的技术来进一步防止过拟合。第一个技术是Friedman的收缩率。在每一步Tree Boosting生长后，收缩率将新增加的权重按因子$\eta$进行缩放。类似于随机优化中的学习率，收缩减少了每棵树的影响，并为以后的树留出了改进模型的空间。第二个技术是列（特征）采样。这个技术在随即森林中使用，它在用于梯度增强的商业软件TreeNet中实现，但在现有的开源软件包中未实现。根据用户反馈，与传统的行采样（也支持）相比，使用列采样可以防止过度拟合。列采样的使用也加快了稍后描述的并行算法的计算。

# 分割查找算法

## Basic Exact Greedy Algorithm（基本精确的贪心算法）

树学习中的一个关键问题是如何找到公式7所表示的最佳分割点。为了找到最佳分割点，分割算法会在所有特征上枚举所有可能的分割点。作者称之为精确贪心算法。大多数现有的单机Tree Boosting实现，如scikit-learn、GBM，以及单机版的XGBoost都支持精确贪心算法。精确贪心算法如算法1所示。计算上要求枚举连续特征的所有可能分割。为了实现这一点，算法必须首先根据特征值对数据进行排序，然后访问排序后的数据，从而逐渐增加公式7中树结构得分的梯度统计。

{% asset_img exact-greedy-algorithm.png 分割点寻找的精准贪心算法 %}

## 近似算法

精准贪心算法非常强大，因为它枚举了所有可能的分裂点。然而，当数据不能完全装入内存时，就不可能有效地这样做。同样的问题也出现在分布式设置中。为了支持这两种设置中的有效梯度树提升，需要一种近似算法。

在算法2中，作者总结了一个近似的框架，类似于在过去的文献中提出的想法。综上所述，该算法首先根据特征分布的比例提出候选分割点（具体标准将在3.3节给出）。该算法将连续特征映射到由这些候选点分割的组成的内存中，对统计信息进行聚合，并根据聚合的统计信息在多个方案中找到最优解。

{% asset_img approximate-algorithm-split-finding.png 分割点寻找的近似算法 %}

根据需求的不同，有两种算法改进，一种是全局变量在树构造的初始阶段提出所有候选拆分，并在所有级别使用相同的拆分查找建议，另一种是局部变量在每次拆分后重新提出。全局方法比局部方法需要更少的选择步骤。但是，通常全局方案需要更多的候选点，因为每次拆分后候选点都没有细化。而局部方案在分割后细化候选对象，并且可能更适合更深的树。在Higgs boson数据集上不同算法的比较如图3所示。作者发现，局部方案确实需要更少的候选人。如果有足够的候选人，全局方案可以和本地提案一样准确。

{% asset_img higgs-dataset.png Higgs-10M数据集上测试AUC收敛性的比较 %}

图3：Higgs-10M数据集上测试AUC收敛性的比较。EPS参数对应于近似数略图的精度。这大致相当于提案中的1/eps桶。作者发现，局部方案所需的存储空间更少，因为它细化了分离的候选人。

大多数现有的分布式树学习近似算法也遵循这一框架。值得注意的是，也可以直接构造近似直方图的梯度统计。也可以使用其他的二进制策略而不是分位数。分位数策略受益于可分配和可重新计算，作者将在下一小节详细介绍。从图3中，作者还发现，分位数策略可以是近似的精度与精准贪心算法相同的精度。

作者系统有效地支持精确贪心的单机设置，以及具有局部和全局方案的近似算法。用户可以根据自己的需要自由选择方法。

# 参考资料

[超级女王XGBoost到底“绝”在哪里？](https://zhuanlan.zhihu.com/p/65805298)

[XGBoost超详细推导，终于有人讲明白了！](https://mp.weixin.qq.com/s?__biz=Mzg2MjI5Mzk0MA==&mid=2247484256&idx=1&sn=9417deb81a2ebffb5e81603425badab7&chksm=ce0b59bbf97cd0ad03e79a87e8f0b8e9479768203046357305a4a5483f2de316580ccff6b7cf&mpshare=1&scene=1&srcid=1109RvBiOxncgoKzk6aNypZR&sharer_sharetime=1573614979764&sharer_shareid=98178f12e3ad48ff6ea4030d6be95e39&pass_ticket=ya68GxkXVWyQO%2Fydmx5oO9LpFScrK%2BdaKW%2BrLyBrVg5f9LKKBiLd1F6CgRa%2BfPeg#rd)

[【干货合集】通俗理解kaggle比赛大杀器xgboost](https://zhuanlan.zhihu.com/p/41417638)

[【机器学习】决策树（下）——XGBoost、LightGBM（非常详细）](https://zhuanlan.zhihu.com/p/87885678)

[XGBoost论文阅读及其原理](https://zhuanlan.zhihu.com/p/36794802)

[机器学习竞赛大杀器XGBoost--原理篇](https://zhuanlan.zhihu.com/p/31654000)