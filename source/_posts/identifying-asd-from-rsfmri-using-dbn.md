---
title: 【2020】Identifying Autism Spectrum Disorder From Resting-State fMRI Using Deep Belief Network
mathjax: true
date: 2020-08-06 22:28:44
updated: {{ date }}
tags: [MRI, 深度学习]
categories: [ASD, 图巻积网络]
---

# 介绍

作者提出了一种使用深度信念网络（DBN）的图分类模型。

1、首先，利用基于图的特征选择方法（GBFS）从外部和内部两个方面来选择ASD的显著功能关联度（FCs）。

2、然后，实现了一种基于有限路径的深度优先搜索（RP-DFS）算法，进一步挖掘图上隐含的拓扑信息。

3、最后，提出一种具有自动超参数调节功能的三层DBN来识别ASD患者。