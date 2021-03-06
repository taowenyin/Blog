---
title: Self-weighted adaptive structure learning for ASD diagnosis via multi-template multi-center representation
mathjax: true
date: 2020-06-29 21:35:20
updated: {{ date }}
tags: [MultiCenter, 深度学习]
categories: [ASD]
---

# 摘要

孤独症谱系障碍（ASD）是一种神经发育疾病，可引起严重的社交、交流、互动和行为障碍。到目前为止，许多基于图像的机器学习技术已经被提出来解决自闭症的诊断问题。然而，这些技术大多局限于来自一个成像中心的单个模板或数据集。本文提出了一种新的多模板、多中心的集成分类方法，用于自动诊断ASD疾病。具体来说，基于不同的预定义模板和我们提出的基于皮尔逊相关系数的稀疏低秩表示，为每个受试者构建不同的多功能连接（FC）脑网络。从这些FC网络中提取特征后，利用我们的自加权自适应结构学习（SASL）模型选择信息特征来学习最优的相似矩阵。对于每个模板，SASL方法自动分配一个从结构信息中学习的最优权重，而不需要额外的权重和参数。最后，使用基于多模板、多中心表达的集成策略得到最终诊断结果。为了证明我们提出的方法的有效性，我们在公开的ABIDE数据集上进行了大量的实验。实验结果表明，该方法提高了ASD的诊断性能，优于现有的诊断方法。

# 简介

ASD是一组快速发展、高度异质性的神经发育综合征，病因基本未知，主要表现为社会功能受损、沟通障碍、行为和兴趣异常。根据CDC的调查，大约每59名儿童中就有1名被诊断为自闭症。这提示了ASD是一个相当严重的健康问题，急需一种有效的及时诊断方法。由于ASD被发现与脑功能连接中断有关，近年来静息态功能磁共振成像（rs-fMRI）已成为揭示ASD发病机制的常用方法。具体来说，功能连通性（FC）是以不同脑区血氧水平依赖性（BOLD）信号（时序）的相关性为代表的，可以为抑郁症、阿尔茨海默病（AD）和ASD等脑疾病的诊断提供更稳定、更灵敏的生物标志物。

近年来，基于rs-fMRI数据的FC网络构建方法层出不穷。皮尔逊相关系数（PC）是估计不同脑区间FC的最简单方法。然而，PC只模拟简单的线性关系，没有涉及脑疾病的生物学机制，可以提高诊断结果。在构造FC网络时，Lee等人提出了一种名为稀疏表示（SR）的方法来引入稀疏先验。由于脑网络不仅稀疏，而且是模块化的。因此Qiao等人提出了稀疏低秩表示（SLR），它在SR的基础上加入了秩约束对模块化结构进行编码。然而，基于PC的诊断方案在ASD诊断中的性能明显优于SR方案。因此，为了有效地拟合数据和编码先验信息，我们提出了一种基于PC的稀疏低秩表示（PSLR）方法来构造具有生理意义的FC网络。

虽然有许多基于FC网络特征表示的ASD诊断方法被提出，但这些方法都只采用一个预先定义的模板。单一模板的特征表示无法全面揭示不同人群（ASD患者和正常对照组）之间的群体差异。相比之下，基于多模板的方法可以基于不同的模板提取对象的多个特征集，从而可以提供不同而又互补的信息来识别不同的疾病状态。因此，这将导致更具前景的识别性能。例如，刘等人提出了利用来自多个图谱（即模板）的特征表示来识别正常对照组中的AD患者和稳定的认知损害（s-MCI）患者中的进行性轻度认知损害（p-MCI）患者。经过视图集中的多图谱特征选择过程，得到了较好的诊断结果。Jin等人建议将公开的AAL图谱中的90个大脑感兴趣区域（ROI）分为203个ROI和403个子ROI。基于这些ROI，每个被试者可以得到三个不同尺度的FC网络，从而得到三组不同的特征表示。通过将每个ROI的基于多模板的特征表示串联起来，可以得到了更有前景的ASD识别结果。

结合多个模板的特征表示可以有效地提高诊断性能。