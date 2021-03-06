---
title: 【2020】Convolutional Kernels with an Element-wise Weighting Mechanism for Identifying Abnormal Brain Connectivity Patterns
mathjax: true
date: 2020-08-12 20:08:50
updated: {{ date }}
tags: [MRI, 深度学习]
categories: [ASD]
---

# 摘要

基于深度学习的人脑网络分类近年来受到越来越多的关注。然而，目前的方法在探索脑网络的拓扑结构信息方面仍然是有限的。本文提出了一种新的基于元素加权机制的卷积核来提取脑网络的层次拓扑特征，其中每个权值都被赋予一个具有独特神经科学意义的元素。此外，本文还提出了一种新的基于CKEW的分类框架，通过跟踪特征分析方法，有效地诊断出脑部疾病并挖掘出最重要的原始特征。在ASD和ADHD数据集的fMRI数据集的实验结果表明，与几种最先进的脑疾病分类方法相比，我们的方法能更准确地区分受试群体，而异常的连接模式和大脑区域更可能成为与大脑疾病相关的生物标志物。

# 介绍

作者提出了基于元素加权机制的卷积核（CKEW，convolutional kernels with an element-wise weighting mechanism）。不同于传统的卷积核，在CKEW中的每个边都有独立的参数，而非共享参数。

1、根据功能连接体的特定拓扑局部性，我们设计了一种基于元素加权机制的卷积核（CKEW），该机制赋予每个功能连接体特定的神经科学意义，在此基础上，我们提出了一种新的基于CKEW的分类框架来提取大脑网络的层次拓扑特征。

2、基于所提出的框架，我们提出了一种基于脑网络边和节点的特征跟踪分析方法，通过累积从上到下的高层特征权重，从而发现脑网络的关键原始特征与其分类结果之间的显著相关性，并进行识别与脑疾病相关的生物标志。

3、在一系列模拟数据集和三个真实数据集上进行了系统的实验，以验证所提出的CKEW和新框架。实验结果验证了该框架在所有测试用例上的有效性。而且我们的方法能够显著地识别重要的功能连接和大脑区域。

# 方法

## 核心思想

一个脑网络通常可以表示为一个图$\mathbf{G}=(\mathbf{V}, \mathbf{E})$，其中$\mathbf{V}$表示一组脑区的节点，$\mathbf{E}$是一个邻接矩阵，其中每个元素（边）$E_{i j} \in \mathbf{E}$表示每两个脑区$V_{i}$和$V_{j}$的连接强度。虽然用邻接矩阵表示的脑网络是网格状的数据，但脑网络数据和图像数据有两个本质的区别。

我们提出了一种基于元素加权机制的卷积核（CKEW）来提取脑网络的层次拓扑特征，并将其应用于脑疾病的分类。CKEW为大脑连接组中的每个元素（边）赋予一个唯一的权重，而不是像BrainNetCNN中那样连接到同一节点的所有边的权重都是共享的。目的是更真实有效地提取脑网络的拓扑结构信息。

## 在E2E层中的CKEW

考虑到脑网络中每条边的拓扑局部性，E2E层的CKEW仍然采用BrainNetCNN中提到的十字形结构。然而，一个主要的区别是每个CKEW都使用一个权重池来存储大脑网络中所有边的独立权重。为了保持每个边权重的唯一性，加权池的大小与相应的邻接矩阵的大小相同。一般来说，为了提取高层特征$E_{i j}$，CKEW使用权重池中的第$i$行权重和第$j$列权重作为当前卷积的十字核，对输入特征执行相应的卷积运算，并获得滤波后的邻接矩阵的输出，如下所示

$$E_{r c}^{l+1, o}=f\left(\sum_{i=1}^{M^{l}} \sum_{k=1}^{|\mathbf{V}|}\left(W_{r k}^{l, i, o} E_{r k}^{l, i}+W_{k c}^{l, i, o} E_{k c}^{l, i}\right)+b^{l, o}\right) \quad r, c=1, \cdots,|\mathbf{V}|$$

其中$l$，$r$，$c$，$i$和$o$分别表示层、行、列、输入特征图的索引（$M^{l}$表示CNN所有特征图中的第$l$层）和输出特征图，$E_{r c}^{l+1, o}$表示第$o$个输出特征图中的第$l+1$层的第$r$行、第$c$列的元素，$f(\cdot)$是一个激活函数，$b^{l, o}$是第$o$个输出特征图的第$l+1$层的偏置项。$W_{r k}^{l, i, o}$和$W_{k c}^{l, i, o}$是进出节点$V_{k}$的每条边的唯一权重。

为了解释卷积过程，我们以一个五节点的大脑网络为例。图1显示了使用CKEW的E2E卷积过程的一个实例。图1（a）给出了与大脑网络相对应的完整图，其中红边（$E_{1 2}$）是相关的，黄边或节点（$V_{1}$和$V_{2}$）表示$E_{1 2}$的局部拓扑结构。图1（b）显示了E2E层的CKEW，它由一个十字形的内核组成，行向量$\mathbf{R}_{i}$和列向量$\mathbf{C}_{j}$都是从权重池中选择得到。图1（c）显示了$E_{1 2}$的特征提取过程，左矩阵和右矩阵分别表示输入边的特征映射和输出边的特征映射，中间的图表示选择$\mathbf{R}_{1}=\left\{W_{11}, W_{12}, \cdots, W_{15}\right\}$和$\mathbf{C}_{2}=\left\{W_{12}, W_{22}, \cdots, W_{52}\right\}$作为当前十字核的CKEW。通过将图1（c）中的行向量$\mathbf{R}_{i}$从上到下、列向量$\mathbf{C}_{j}$从左到右分别逐级滑动并进行相应的卷积运算，完成了输入特征映射的所有特征提取过程，在输出特征图（过滤脑网络）中获取所有高级特征，作为下一层的新输入特征图。CKEW的特点是利用一个加权池为每个不同的边提供一个唯一的权重，从而提高每个边特征的识别能力。

{% asset_img ckew-for-e2e.jpg E2E的CKEW示意图 %}

图1：$E_{1 2}$的CKEW示意图

## 在E2N层中的CKEW

根据边权的拓扑局部性和唯一性，在E2N层中的CKEW使用大小为$|\mathbf{V}| \times|\mathbf{V}|$的$M^{l}$特征映射作为数据数据，并执行相应的卷积操作，从而得到大小为$|\mathbf{V}|$的输出向量，如下所示

$$V_{r}^{l+1, o}=f\left(\sum_{i=1}^{M^{l}} \sum_{k=1}^{|\mathbf{V}|}\left(W_{r k}^{l, i, o} E_{r k}^{l, i}+W_{k r}^{l, i, o} E_{k r}^{l, i}\right)+b^{l, o}\right) \quad r=1, \cdots,|\mathbf{V}|$$

类似的，$W_{r k}^{l, i, o}$和$W_{k c}^{l, i, o}$表示进出节点$V_{k}$的每条边的唯一权重。

图2显示了使用CKEW的E2N卷积过程的实例。图2（a）给出了与大脑网络相对应的完整图，其中红色节点（$V_{2}$）是相关节点，黄色边表示$V_{2}$的局部拓扑结构，图2（b）显示的CKEW类似于图1（b）中的E2N层。图2（c）是$V_{2}$的特征提取过程，选择$\mathbf{R}_{2}=\left\{W_{21}, W_{22}, \cdots, W_{25}\right\}$和$\mathbf{C}_{2}=\left\{W_{12}, W_{22}, \cdots, W_{52}\right\}$作为当前的十字形核。通过同时沿图2（c）中左矩阵的主对角线滑动行向量$\mathbf{R}_{i}$和列向量$\mathbf{C}_{j}$并进行相应的卷积运算，跨输入特征映射进行特征提取，在过滤后的向量中获得所有高级特征，将其视为新的输入下一层的特征地图。在E2N层中，CKEW通过对每一条边的唯一权值来扮演不同的边角色，具有各自的神经科学意义。

{% asset_img ckew-for-e2n.jpg E2N的CKEW示意图 %}

图2：$V_{2}$的CKEW示意图

## 基于CKEW的脑分类网络

基于所提出的CKEWs，我们提出了一个基本的大脑网络分类框架，如图3所示。该框架主要包括5层：三层卷积层（E2E、E2N和N2G）、一个全连接层和一个Softmax分类器层。更具体地说，$L_{0}$是一个输入层，它可能由一个或多个连接体（邻接矩阵）组成，用于组合大脑网络的各种信息，而本文中只有一个连接体。通过E2E层的CKEWs，我们可以将$L_{0}$中的邻接矩阵作为输入数据，在$L_{1}$中获得边级特征映射作为过滤后的邻接矩阵。基于E2N层的CKEW，我们可以继续从$L_{1}$中提取高级特征，并在$L_{2}$中获得节点级特征映射。然后，我们使用N2G卷积核来连续提取更多的高层特征，并在$L_{3}$中得到图级特征映射。此外，使用一个全连接层（$L_{4}$）从先前的特征映射中进一步提取更高级的特征。最后，利用softmax分类器（$L_{5}$）对脑疾病进行诊断。接下来，我们将对N2G卷积核、全连通层、Softmax分类器以及使用CKEWs对基本框架进行训练等方面进行简要介绍。

{% asset_img classify-brain-networks.jpg 脑网络分类器 %}

图3：基于CKEW的脑网络分类器框架

如BrainNetCNN文所述，节点到图（N2G）的卷积核被定义为一个简单的列向量。它以可能表示过滤节点的$M^{l}$特征映射为输入，对节点进行加权组合，输出单个标量$G^{l+1, o}$，如下所示：

$$G^{l+1, o}=f\left(\sum_{i=1}^{M^{l}} \sum_{r=1}^{|\mathbf{V}|} C_{r}^{l, i, o} N_{r}^{l, i}+b^{l, o}\right)$$

全连接层主要对前一层的输出进行线性组合和非线性变换，以提取更多的高层特征。全连接层的输出定义为，

$$A^{l+1, o}=f\left(\sum_{i=1}^{M^{l}} W_{i o}^{l} G^{l, i}+b_{o}^{l}\right)$$

其中，$W_{i o}^{l} \in \mathbb{R}^{M^{l} \times M^{l+1}}$是权重，$b_{o}^{l} \in \mathbb{R}^{M^{l+1}}$是第$l$层的偏置项；$M^{l}$表示第$l$层全连接层的神经元的数量。在不失一般性的情况下，可以通过使用几个完全连接的层来提取更深层次的高级功能来扩展基本框架。

Softmax分类是我们框架的最后一层。$p\left(y^{(n)}=j \mid \mathbf{A}^{l^{(n)}} ; \boldsymbol{\theta}\right)$是特征图$\mathbf{A}^{l^{(n)}}$把第$l$层第$n$个样本分类到类别$j$的概率，其中$y^{(n)}$是第$n$个样本的标签，$\boldsymbol{\theta}=\left\{\mathbf{W}^{l}, \mathbf{b}^{l}\right\}$，$\mathbf{W}^{l} \in \mathbb{R}^{M^{l} \times J}$是权重，$\mathbf{b}^{l} \in \mathbb{R}^{J}$是偏置项。表达式可以表示为

$$p\left(y^{(n)}=j \mid \mathbf{A}^{(n)} ; \boldsymbol{\theta}\right)=\frac{\exp \left(\sum_{i=1}^{M^{l}} W_{i j}^{l} A_{i}^{l^{(n)}}+b_{j}^{l}\right)}{\sum_{k=1}^{J} \exp \left(\sum_{i=1}^{M^{l}} W_{i k}^{l} A_{i}^{l^{(n)}}+b_{k}^{l}\right)}$$

其中$J$表示类别的种类。

为了训练我们的框架，我们采用如下损失函数：

$$J(\mathbf{W}, \mathbf{b})=-\frac{1}{N}\left[\sum_{n=1}^{N} \sum_{j=1}^{J} 1\left\{y^{(n)}=j\right\} \log p\left(y^{(n)}=j \mid \mathbf{A}^{l^{(n)}} ; \boldsymbol{\theta}\right)\right]+\frac{\lambda}{2}\|\mathbf{W}\|_{2}^{2}$$

其中$\mathbf{W}$，$\mathbf{b}$包括框架中的所有参数（权重和偏差），$N$是样本数量，$\frac{\lambda}{2}\|\mathbf{W}\|_{2}^{2}$是$L_{2}$正则项，其目的是控制权重的大小以防止过度拟合，参数λ用于控制等式上式中两项的相对重要性。

在训练中，我们采用Adam方法最小化损失函数$J(\mathbf{W}, \mathbf{b})$。批大小设置为96，学习率设置为0.0001。$\lambda$的值是从候选集{0.1，0.01，0.05，0.001，0.005，0.0005}中选取的。为了避免过拟合，我们采取了早期停止策略。一旦在验证集上的$J(\mathbf{W}, \mathbf{b})$在$E_{patience}$阶段没有进一步优化，或者迭代过程达到最大迭代次数（$E_{max}$阶段），训练过程将停止。在本文中，$E_{patience}$和$E_{max}$分别设为50和10000。算法1说明了基于CKEW的分类过程的细节。

{% asset_img algorithm-1.jpg 算法1 %}

## 原始特征重要性评价

为了更好地理解该框架的分类结果，我们提出了一种跟踪特征分析方法，该方法可以自上而下地探测根源，从而反导出对结果影响更大的相对重要的原始特征（脑网络中的节点和边）。在深度模型中，一个特征在一个层中的重要性可以描述为一些相关权重的组合。因此，特征分析可以通过两个相邻层之间权重的线性组合来执行。根据文献中的方法，我们从上到下给出了各层特征的重要性度量（称为Importance Metric of Features，IMF）。具体地说，特征分析可以按以下顺序导出：Softmax层、全连接层、N2G层、E2N层和E2E层。

在Softmax层中，第$i$个输入特征的重要性可以通过中提出的方法来计算。对于本文中的二元分类，公式表示为，

$$I M F^{L_{4}, i}=W^{L_{5}, i, 1}-W^{L_{5}, i, 0}$$

基于$I M F^{L_{4}, i}$，在全连接层的第$i$个输入特征的重要性可以按照下面的计算得到：

$$I M F^{L_{3}, i}=\sum_{o=1}^{M^{L_{4}}} W^{L_{4}, i, o} I M F^{L_{4}, o}$$

根据前一层计算出的特征的重要性，可以分别计算第$i$个输入特征在N2G层、E2N层和E2E层中的重要性：

$$I M F_{r}^{L_{2}, i}=\sum_{o=1}^{M^{L_{3}}} W_{r}^{L_{3}, i, o} I M F^{L_{3}, o}$$

$$I M F_{r c}^{L_{1}, i}=\sum_{o=1}^{M^{L_{2}}} W_{r c}^{L_{2}, i, o}\left(I M F_{r}^{L_{2}, o}+I M F_{c}^{L_{2}, o}\right)$$

$$I M F_{r c}^{L_{0}, i}=\sum_{o=1}^{M^{L_{1}}} W_{r c}^{L_{1}, i, o} \sum_{k=1}^{|\mathbf{V}|}\left(I M F_{r k}^{L_{1}, i, o}+I M F_{c k}^{L_{1}, i, o}\right)$$

最后，利用下式分别确定原始脑网络中各边（$E_{rc}$）和每个节点（$N_{r}$）对分类结果的重要作用。

$$I M F\left(E_{r c}\right)=\sum_{i=1}^{M^{L_{0}}} I M F_{r c}^{L_{0}, i}$$

$$I M F\left(N_{r}\right)=\sum_{c=1}^{|\mathbf{V}|} I M F\left(E_{r c}\right)$$