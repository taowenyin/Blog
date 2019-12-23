---
title: 【2019】DeepGBM：A Deep Learning Framework Distilled by GBDT for Online Prediction Tasks
mathjax: true
date: 2019-11-18 08:48:53
updated: {{ date }}
tags: [集成学习, 机器学习, 深度学习]
categories: [论文]
---

# 0.摘要

在现实应用中，在线预测是最基本的任务。典型的在线预测任务具备两个特征，分别是表格式的输入空间和在线数据生成。具体说，就是表格式输入空间表示既有稀疏分类特征也有稠密的数值特征，而在线数据生成则是具有连续生成具有动态分布数据的任务。因此，利用表格输入空间进行有效学习和快速适应在线生成的数据是获取在线预测模型的两个重要挑战。虽然采用GBDT和NN已经在实践中被广泛使用，当时它们都有其自身的缺点。特别地，GBDT很难适应动态生成的在线数据，并且在面对稀疏的分类特征时往往是无效的；另一方面，在面对稠密的数值特征时，NN很难获取令人满意的性能。在这篇文章中，作者提出了一个新的学习框架——DeepGBM，该框架集成了NN和GBDT的优点并提出了两个新的NN组件：

1、CatNN：专注于处理稀疏的分类特征。

2、GBDT2NN：通过GBDT对稠密数值特征提取。

在这两个组件的支持下，DeepGBM可以同时利用分类和数值特性，同时保持高效在线更新的能力。

{% asset_img deepgbm.png DeepGBM框架 %}

# 1.介绍

在线预测在许多实际工业应用中是非常重要的一类任务，如广告搜索中的点击预测、Web搜索中的内容排序、推荐系统中的内容优化、交通规划中的行程时间估计等。

一个典型的在线预测任务中通常具有两个基本特征，即表格式的输入空间和在线数据生成。其中，表格式的输入空间意味着在线预测任务的输入特征可以包括分类和表格数值特征。例如，在广告点击预测任务的特征空间中通常包含广告类别等分类特征空间，以及查询与广告文本相似性等数值特征空间，在线数据生成意味着这些任务的实际数据是以在线方式生成，并且数据分布是实时动态的。例如，在新闻推荐系统中可以实时生成大量的数据，并且在不断涌现的新闻中可以在不同的时间内产生动态的特征分布。

因此，要为在线预测任务寻求有效的基学习器模型，就必须解决两个主要挑战：

1、在表格式输入空间中如何学习有效的模型？

2、如何适配在线数据生成的模型？

目前，两类机器学习模型被广泛应用于在线预测任务，分别时GBDT和NN。不幸的是，这两个方法都不能同时很好的应对上面的两个挑战。换言之，在解决在线预测任务时，GBDT或NN都会产生各自的优缺点。

{% asset_img different-models.png 不同模型的比较 %}

## 1.1GBDT优缺点

1、GBDT的主要优势在于它能够有效地处理密集的数值特征。因为，GBDT可以在每次迭代中选取信息增益最大的特征来构建树，因此它可以自动地选择和组合有用的数值特征，以更好地适应训练目标。这就是为什么GBDT在点击预测、Web搜索排名和其他公认的预测任务中展示了它的有效性。当时，GBDT在在线预测任务中有两个主要的弱点。首先，由于GBDT中**学习到的树是不可微的，所以在联机模式下更新GBDT模型是很困难的。**从0开始的不断训练使得GBDT在学习在线预测任务时效率很低，这一弱点阻碍了GBDT对超大规模数据的学习，**因为将大量数据加载到内存中进行学习通常是不切实际的。**

2、GBDT无法在稀疏分类特征上进行学习。特别是将分类特征转换成稀疏、高维的one-hot编码后，稀疏特征的信息增益将变得非常小，`因为在稀疏特征情况下的具有不平衡增益的分割与不分割几乎相同。`因此，GBDT就不能利用稀疏特征来有效的构建决策树。尽管有一些分类编码方式可以直接将分类值转换为稠密的数值，但在对不同分类进行编码转换时，由于很多编码值可能非常相似，当时系统并不知道，所以就会造成与原始数据之间的误差。通过列举可能的二元分类，分类特征也可以直接用于决策树的学习，然而这种方法在分类特征稀疏的情况下，由于每类数据太少，因此统计信息会有误差，往往就会造成训练数据的过拟合。简而言之，虽然GBDT对稠密的数值特征具有较好的学习性，但还是有两个明显缺点，即难以适应在线数据生成和对于稀疏分类特征学习的无效性，这导致GBDT在许多在线预测任务中失败，特别是那些需要在线调整模型和包含许多稀疏分类特征的模型。

## 1.2NN优缺点

神经网络的优势在于通过采用**批处理模式的反向传播算法和公认的对稀疏分类特征具有较好学习性的嵌套结构**对在线任务中的大规模数据学习的有效性。最近的一些研究表明，在如点击预测和推荐系统等在线预测任务中使用神经网络得到了成功的应用。然而，神经网络的主要挑战在于它在学习稠密的数值表格特征方面较弱。全连接神经网络（FCNN）虽然可以直接用于稠密的数值特征，但由于其全连接的模型结构导致了非常复杂的优化超平面方面很容易陷入局部最优，因此常常性能不理想。因此，在许多具有稠密数值表特征的任务中，神经网络性能不比GBDT好。综上所述，尽管神经网络能够有效地处理稀疏的分类特征，并且适应在线数据生成，但是对于稠密的数值表格特征学习仍然很难得到有效的模型。

本文提出的DeepGBM框架主要包含了两个组件：

1、CatNN：接收分类特征的NN结构。

2、GBDT2NN：接收数值特征的NN结构。

为了充分利用GBDT在学习数值特征方面的优势，GBDT2NN把GBDT学习到的内容转化为神经网络的建模过程。具体来说，为了提高知识提取的有效性，GBDT2NN不仅传递了预先训练的GBDT知识，而且还融合了所得到的树结构中隐含的特征重要性和数据划分知识。这样，在达到与GBDT相当的性能的同时，采用神经网络结构的GBDT2NN在面对在线数据生成时，可以很容易地通过不断涌现的数据进行模型更新。

DeepGBM由两个基于神经网络的组件CatNN和GBDT2NN组成，在保持高效在线学习的重要能力的同时，可以在类别特征和数值特征上具有较强的学习能力。本文的三个主要贡献：

1、结合GBDT和NN的优点提出了DeepGBM方法，在保留有效在线更新模型能力的同时，利用分类和数值两种特征，对各种具有表格数据的预测任务进行处理。

2、通过考虑GBDT模型学习树中输入选择、结构和输出，提出了一种将GBDT模型中学到的知识提取为神经网络模型的方法。

3、实验表明DeepGBM是一个成熟的模型，已经能够被用于各类预测任务中，并且具有较好的性能。通过枚举可能的二分类，分类特征也可以直接用于树学习，但是这种方法在分类特征稀疏的情况下会造成训练数据过度拟合。

# 2.相关工作

## 2.1在在线预测任务中应用GBDT

1、在线更新树模型：XGBoost、LightGBM等对在线更新树模型提供了简单的方法，它们保持树结构不变，并通过新数据更新叶输出，但是这种方法的性能离预期的太远。此外，还有人提出了当有新数据时，就重新查找树的分割点，但是由于忽略的历史数据，因此性能也不稳定。

2、树中的分类特征：为了方便决策树能更好的处理它们，通过一些编码方法将稀疏的分类值转换为稠密的数值。CatBoost使用类似数字编码来对分类特征编码，但是它会引起信息损失。

## 2.2在在线预测任务中应用NN

由于FCNN中超平面的优化非常复杂，容易陷入局部最优解，即使采用了归一化和正则化技术，但是对于具有稠密数值特征的数据来说FCNN的性能并不优于GBDT。另一种是把稠密的数值特征进行离散化，形成分类的形式，从而可以更好地处理特征分类的相关工作。但是由于输出仍将需要连接到完全连接层，因此离散化实际上无法提高处理数值特征的效率，并且离散化会增加模型的复杂度，并且由于模型参数的增加而导致过拟合。

## 2.3组合NN和GBDT

1、Tree-link NN（像决策树的神经网络）：像GoogLeNet在一定程度上具有决策树的特点。但这些模型都主要面向机器视觉，而不是具有数据表输入的在线预测任务。还有如NNRF算法，该算法利用树状神经网络和随机特征选择来提高表格数据学习的效果，但是NNRF只使用随机特征组合，而不利用GBDT等训练数据的统计信息。

2、Convert Trees to NN（把决策树转化为神经网络）：这些工作通常效率很低，因为它们使用冗余且非常稀疏的神经网络来表示简单的决策树。当有许多树时，这种转换方案必须构造一个非常宽的神经网络来表示它们，这很难得到实际应用。

3、Combining NN and GBDT（组合NN和GBDT）：Facebook使用叶指数预测作为Logistic回归的输入分类特征。微软用GBDT拟合神经网络的残差。然而，由于GBDT中的在线更新模型问题没有得到解决，因此这些工作无法有效地使用。Facebook在其中也指出了这个问题，因为他们框架中的GBDT模型需要每天重新训练，才能获得良好的在线性能。

# 3.DeepGBM

CatNN是处理分类特征的NN结构，GBDT2NN是处理稠密数值特征的NN结构。

## 3.1处理稀疏分类特征的CatNN

为了解决在线预测问题，NN已经被广泛的应用于学习分类特征的预测模型，如Wide&Deep、PNN、DeepFM、xDeepFM。由于CATNN的目标是与这些模型的目标相同，因此可以直接把现有的神经网络结构应用在CatNN中。CatNN于前期的研究相似，主要使用嵌套技术（Embedding Technology）把高维的稀疏向量转化为稠密向量。此外，作者还利用前面研究得到的FM组件和Deep组件来学习特征之间的相互作用，并且CatNN并不限制于这两个组件，它可以使用任何的其他类似NN组件。

嵌套技术（Embedding Technology）是通过低维稠密数据来表示高维稀疏向量的一种方法：

\begin{equation}
    E_{V_{i}}\left(x_{i}\right)=\text { embedding_lookup}\left(V_{i}, x_{i}\right)
\end{equation}

其中$x_{i}$是第$i$维特征的值，$V_{i}$存储了第$i$个特征所有嵌套（embeddings）并且能够被用于反向传播，而$E_{V_{i}}\left(x_{i}\right)$将返回$x_{i}$对应的嵌套向量。基于此，作者使用FM组件学习1阶线性函数和2阶特征之间的相互作用：

\begin{equation}
    y_{FM}(x)=w_{0}+\langle w, x\rangle+\sum_{i=1}^{d} \sum_{j=i+1}^{d}\left\langle E_{V_{i}}\left(x_{i}\right), E_{V_{j}}\left(x_{j}\right)\right\rangle x_{i} x_{j}
\end{equation}

其中$d$表示特征的数量，$w_{0}$和$w$是线性部分的参数$\left \langle \cdot , \cdot  \right \rangle$是内积操作。

然后，使用Deep组件来学习高阶特征之间的相互作用。

\begin{equation}
    y_{Deep}(x)=\mathcal{N}\left(\left[E_{V_{1}}\left(x_{1}\right)^{T}, E_{V_{2}}\left(x_{2}\right)^{T}, \ldots, E_{V_{d}}\left(x_{d}\right)^{T}\right]^{T} ; \theta\right)
\end{equation}

其中$\mathcal{N}\left ( x, \theta \right )$表示输入为$x$，参数为$\theta$的多层NN网络模型。通过组合这FM和Deep组件，CatNN网络最终的输出为：

\begin{equation}
    y_{Cat}\left ( x \right )=y_{FM}\left ( x \right ) + y_{Deep}\left ( x \right )
\end{equation}

## 3.2处理稠密数值空间的GBDT2NN

本节首先介绍如何把一棵树提取为NN，然后推广这个想法至在GBDT中提取多棵树。

### 3.2.1提取一棵树

之前的树提取工作大多依赖于已学习到的函数来进行模型知识的转换，从而使得新生成的模型与原模型相似。由于树与神经网络的本质不同，因此除了传统的模型提取方法外，树模型中的更多知识也可以被提取并转化为神经网络。例如树中的特征选择和权重，以及树结构中所隐含的数据划分，都是树中的重要知识。

**1、树特征选择：**与神经网络相比，树模型的一个特点是不会使用所有的输入特征，因为它的学习会根据统计信息进行选择适合训练目标的特征。因此，我们可以根据树选择的特征来传递这些信息，以提高神经网络模型的学习效率，而不是使用所有的输入特征。形式上可以定义$\mathbb{I}^{t}$作为树$t$中已选择特征的指示器，然后只需要使用$x\left [ \mathbb{I}^{t} \right ]$作为NN的输入。

**2、树结构：**决策树的结构其实质上是将数据划分为多个不重叠的区域（叶子），即将数据聚类成不同的类，同一叶中的数据属于同一类。由于神经网络和决策树的本质不同，因此把直接把决策树转化为神金网络并不容易。幸运的是，由于神经网络已被证明能够逼近任何函数，所以我们可以使用神经网络模型来逼近树结构的函数，并实现结构信息的提取。如图2所示，可以使用神经网络来拟合树生成的聚类结果，从而使神经网络逼近决策树的结构函数。形式上将树结构函数$t$表示为$C^{t}\left ( x \right )$，该函数返回样本$x$输出的叶索引，即树生成的聚类结果。然后使用神经网络模型来逼近结构函数$C^{t}\left ( \cdot \right )$，学习过程可以表示为：

\begin{equation}
    \underset{\theta}{min}\frac{1}{n}\sum_{i=1}^{n}{\mathcal{L}}'\left ( \mathcal{N}\left ( x^{i}\left [ \mathbb{I}^t \right ]; \theta \right ),L^{t,i} \right )
\end{equation}

其中$n$表示训练样本的个数，$x^{i}$表示是第$i$个训练样本，$L^{t,i}$是第$x^{i}$样本以one-hot形式表示的叶索引$C^{t}\left ( x^{i} \right )$，$\mathbb{I}^{t}$是当前树$t$中所用到特征的索引，$\theta$是神经网络模型$\mathcal{N}$的参数，并且可以通过方向传播进行更新，${\mathcal{L}}'$是多分类问题的损失函数，如交叉熵。因此，通过学习就可以得到一个网络模型$\mathcal{N}\left ( \cdot ; \theta \right )$。由于神经网络具有很强的表达能力，学习的神经网络模型会完全逼近决策树的结构函数。

{% asset_img nn-approximate-tree.png 神经网络逼近决策树 %}

**3、树输出：**在前面的步骤中学习了从输入到树结构的映射，所以要获取树的输出，只需要知道从树结构到树输出的映射。然而，由于每个叶索引都有对应的叶值，所以该映射并不需要学习。因此，可以将树$t$的叶值表示为$q^{t}$，$q_{i}^{t}$表示第i个叶的叶值。因此就可以通过$p^{t}=L^{t} \times q^{t}$把$L^{t}$映射到树输出。

结合上面的方法，从$t$树获取得到的神经网络的输出可以表示为：

\begin{equation}
    y^{t} \left ( x \right ) = \mathcal{N}\left ( x\left [ \mathbb{I}^{t}; \theta \right ] \right ) \times q^{t}
\end{equation}

### 3.2.2多树的提取

要把单树提取的思想推广到GBDT中，一个直接的方法就是把有多少树就有多少神经网络，并且一一对应（$\#NN=\#tree$）。但是结构提取的目标（$O\left ( \left | L \right | \times \#NN \right )$）具有高维性，因此该方法的效率很低。为了提高效率，提出了叶嵌套提取法（Leaf Embedding Distillation）和树分组法（Tree Grouping）来分别对$\left | L \right |$和$\#NN$进行降维。

**1、叶嵌套提取法（Leaf Embedding Distillation）：**如图3所示，作者采用嵌套技术来降低结构提取目标$L$的维数，同时对信息进行重新训练。更具体地说，由于叶指数和叶值之间存在双射关系，因此使用叶值来学习嵌套，从形式上说，这个学习的过程可以表示为：

\begin{equation}
    \underset{w,w_{0},\omega^{t}}{min}\frac{1}{n}\sum_{i=1}^{n}{\mathcal{L}}''\left ( w^{T}\mathcal{H}\left ( L^{t,i};\omega^{t} \right ) + w_{0}, p^{t,i} \right )
\end{equation}

其中$H^{t,i} = \mathcal{H}\left ( L^{t,i};\omega^{t} \right )$是一个具有参数$\omega^{t}$的单层全连通网络，它将一个one-hot叶索引$L^{t,i}$转换为稠密嵌套的$H^{t,i}$，$p^{t,i}$是样本$x^{i}$的叶预测值，${\mathcal{L}}''$是与树中相同的损失函数，$w$和$w_{0}$是将嵌套映射为叶值的参数。这种方法，作者使用了嵌套式的稠密数据作为目标来逼近树结构函数，从而不是使用稀疏高维on-hot数据$L$，其学习过程可以表示为：

\begin{equation}
    \min _{\boldsymbol{\theta}} \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}\left(\mathcal{N}\left(\boldsymbol{x}^{i}\left[\mathbb{I}^{t}\right] ; \boldsymbol{\theta}\right), \boldsymbol{H}^{t, i}\right)
\end{equation}

其中$\mathcal{L}$表示拟合`嵌套式稠密（Dense Embedding）`的类似于L2回归损失函数。由于$H^{t, i}$的维数要比$L$小得多，因此嵌套式叶提取在多树提取中更为有效，并且该方法使用更少的神经网络参数。

{% asset_img leaf-embedding-distillation.png 叶嵌套提取法 %}

**2、树分组法（Tree Grouping）：**为了降低神经网络的纬度，作者把树进行了分组，并且使用一个神经网络从一组树中提取神经网络模型，但是分组存在两个问题，即如何对树进行分组和如何从一组树中提取神经网络模型，其步骤如下：

（1）对于分组策略已经有了许多方法，例如平均随机分组、按顺序分组、基于重要性或相似性分组等，在本文中，作者使用平均随机分组。假设有$m$个树，并且把这些数分为$k$组，那么每组中的树就有$s=\left \lceil \frac{m}{k} \right \rceil$，并且第$j$树分组为$\mathbb{T}_{j}$，它包含了来自GBDT中的随机$s$个树。

（2）为了从多棵树中提取模型，作者扩展多棵树的叶嵌套提取法。其形式为，假如某个树分组为$\mathbb{T}$，那么就扩展式子7从多棵树中学习的叶嵌套。

\begin{equation}
    \underset{w,w_{0},\omega^{T}}{min}\frac{1}{n}\sum_{i=1}^{n}{\mathcal{L}}''\left ( w^{T}\mathcal{H}\left ( \parallel_{t \in \mathbb{T}}\left ( L^{t,i} \right );\omega^{\mathbb{T}} \right ) + w_{0}, \underset{t \in \mathbb{T}}{\sum}p^{t,i} \right )
\end{equation}

其中$\parallel_{t \in \mathbb{T}}$是一个链接操作，$G^{\mathbb{T},i} = \mathcal{H}\left ( \parallel_{t \in \mathbb{T}}\left ( L^{t,i} \right );\omega^{\mathbb{T}} \right )$是一个单层的全连接神经网络用于转换多个ont-hot向量，即为了在$\mathbb{T}$中使用嵌套式稠密数据$G^{\mathbb{T},i}$，因此连接多个one-hot叶索引向量。然后将新的嵌套作为神经网络模型的提取目标，其学习过程可以表示为：

\begin{equation}
    \mathcal{L}^{\mathbb{T}}=\min _{\boldsymbol{\theta}^{\mathbb{T}}} \frac{1}{n} \sum_{i=1}^{n} \mathcal{L}\left(\mathcal{N}\left(x^{i}\left[\mathbb{I}^{\mathbb{T}}\right] ; \boldsymbol{\theta}^{\mathbb{T}}\right), G^{\mathbb{T}, i}\right)
\end{equation}

其中$\mathbb{I}^{\mathbb{T}}$是在树分组$\mathbb{T}$中所使用的特征集。当树分组$\mathbb{T}$中树的数量很大时，$\mathbb{I}^{\mathbb{T}}$可能会包含很多的特征，从而影响特征选择，因此，可以只能根据特征重要性在中使用最重要的特征。综上所述，从树分组$\mathbb{T}$中提取的神经网络最终输出为：

\begin{equation}
    y_{\mathbb{T}}(\boldsymbol{x})=\boldsymbol{w}^{T} \times \mathcal{N}\left(\boldsymbol{x}\left[\mathbb{I}^{\mathbb{T}}\right] ; \theta^{\mathbb{T}}\right)+w_{0}
\end{equation}

并且包含$k$个树分组的GBDT模型输出为：

\begin{equation}
    y_{GBDT2NN}(x)=\sum_{j=1}^{k} y_{\mathbb{T}_{j}}(x)
\end{equation}

综上所述，由于使用了叶嵌套提取法和树分组法，GBDT2NN可以有效地将GBDT中的许多树提取为一个紧凑的神经网络模型。此外，除了树的输出，树的特征选择和结构信息也被有效地提取到神经网络模型中。

## 3.3训练DeepGBM

### 3.3.1离线状态下端到端训练

为了训练DeepGBM，首先需要使用离线数据训练GBDT模型，然后使用式子9得到GBDT中树的嵌套叶。然后就进行端到端的DeepGBM训练。DeepGBM的输出可以表示为：

\begin{equation}
    \hat{y}(\boldsymbol{x})=\sigma^{\prime}\left(w_{1} \times y_{GBDT2NN}(\boldsymbol{x})+w_{2} \times y_{Cat}(\boldsymbol{x})\right)
\end{equation}

其中$w_{1}$和$w_{2}$是GBDT2NN和CatNN结合后需要训练的参数，$\sigma^{\prime}$是转换的输出，类似于二分类的$sigmoid$函数。最后就可以使用下面这个端到端的损失函数训练：

\begin{equation}
    \mathcal{L}_{offline}=\alpha \mathcal{L}^{\prime \prime}(\hat{y}(x), y)+\beta \sum_{j=1}^{k} \mathcal{L}^{\mathbb{T}_{j}}
\end{equation}

其中$y$是样本$x$的训练目标，$\mathcal{L}^{\prime \prime}$是一个类似于分类任务中的交叉熵损失函数，$\mathcal{L}^{\mathbb{T}}$是在式子10中的树组$\mathbb{T}$的嵌套损失函数。$k$是树组的个数，$\alpha$和$\beta$是预先给定的超参数，分别用于控制端到端损失和嵌套损失的强度。

### 3.3.2在线模型更新

由于GBDT模型是离线训练的，因此在在线更新中的嵌套学习中使用GBDT模型会影响在线实时性。因此，在在线模型更新中并不包括更新$\mathcal{L}^{\mathbb{T}}$，同时在线更新模型的损失函数为：

\begin{equation}
    \mathcal{L}_{online}=\mathcal{L}^{\prime \prime}(\hat{y}(\mathbf{x}), y)
\end{equation}

这个只能用于计算端到端的损失。因此，当DeepGBM在线时，只需要通过$\mathcal{L}_{online}$来更新模型的新数据，而不需要把GBDT和训练过程进行再次训练。综上所述，DeepGBM能非常有效地执行在线任务，并且它还可以很好地处理稠密数值特征和稀疏分类特征。