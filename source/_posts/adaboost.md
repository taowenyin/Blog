---
title: AdaBoost理解和推导
date: 2019-11-11 19:13:30
mathjax: true
updated: {{ date }}
tags: 
- 集成学习
- 机器学习
categories: 
- 机器学习
---

# 1.AdaBoost介绍

AdaBoost（Adaptive Boosting，自适应增强集成算法），即自动通过**前一个基模型对样本预测的误差来调整当前模型中的权重**，然后基于新的权重来训练得到新的模型，以此反复直到训练的基模型个数达到设定的数量，最后把**所有基模型的训练结果**通过组合策略进行集成，从而得到最终模型。基模型的种类很多，可以是决策树，也可以是神经网络等。这个过程对结果的预测主要有两部分的提高：

1、训练过程的提高：通过**增加**之前基模型中的分类或回归错误的样本权重，并**降低**了正确分类和回归的样本权重，从而使得**错误分类的样本在下一个基模型中能够被更大的关注。**

2、模型结果的提高：在最后的组成策略中采用**增加**分类或回归误差较小的模型权重，**降低**分类或回归误差较大的模型权重，从而**提高正确模型的贡献率。**

AdaBoost要解决的问题主要有四个，分别是：

1、如何计算模型的预测的误差率？

2、如何确定基模型的权重

3、如何更新样本的权重

4、如何将多个基模型组合在一起

假设训练集$D=\left \{ \left ( x^{\left ( 1 \right )}, y^{\left ( 1 \right )} \right ),\left ( x^{\left ( 2 \right )}, y^{\left ( 2 \right )} \right ), \cdots ,\left ( x^{\left ( m \right )}, y^{\left ( m \right )} \right ) \right \}$，其中样本的个数为$m$，$y^{\left ( i \right )} \in \left \{ -1, +1 \right \}, i \in \left \{ 1, 2, \cdots , m \right \}$，并且基模型的个数为$T$，第$t$个基模型中每个训练样本的权重为$D_{t}=\left ( \omega_{t,1}, \omega_{t,2}, \cdots , \omega_{t,m} \right )$。

# 2.分类问题中AdaBoost的算法流程

1、初始化数据集在**第一个**基模型中的样本权重：$D_{1}=\left ( \omega_{1,1}, \omega_{1,2}, \cdots , \omega_{1,m}  \right )$，其中$\omega_{1,i} = \frac{1}{m}, i = 1, 2, \cdots , m$。

2、令$t = 1, 2, \cdots, T$，循环执行下面的语句：

（1）使用带有权重的训练集$D_{t}$训练一个基模型$h_{t}$。

（2）计算基模型$h_{t}$的误差率$\epsilon_{t} = \sum_{i=1}^{m}\omega_{t,i}I\left ( x^{\left ( i \right )} \neq y^{\left ( i \right )} \right )$，其中$I \in \left \{ 0, 1 \right \}$为指示函数。

（3）计算基模型$h_{t}$在最终集成模型中的权重$\alpha_{t}=\frac{1}{2}\ln \frac{1-\epsilon_{t}}{\epsilon_{t}}$。

（4）更新下一个基模型中样本的权重$D_{t+1}=\left ( \omega_{t+1,1}, \omega_{t+1,2}, \cdots , \omega_{t+1,m}  \right )$，其中

\begin{align}
    \omega_{t+1,i} = \frac{\omega_{t,i}}{Z_{t}}exp\left ( -\alpha_{t}y^{\left ( i \right )}h_{t}\left ( x^{\left ( i \right )} \right ) \right ), i = 1, 2, \cdots, m
\end{align}

其中，$Z_{t} = \sum_{i=1}^{m}\omega_{t,i}exp\left ( -\alpha_{t}y^{\left ( i \right )}h_{t}\left ( x^{\left ( i \right )} \right ) \right )$是一个归一化因子。

3、组合各基模型，从而生成最终模型$H\left ( x \right ) = \sum_{t=1}^{T}\alpha_{t}h_{t}\left ( x \right )$。

结合上面的流程就可以回答AdaBoost要解决的四个主要问题：

**1、如何计算模型的预测的误差率：**模型的预测误差$\epsilon_{t}$通过计算带权重的样本错误分类比例来获得样本的预测误差。

**2、如何确定基模型的权重：**基模型模型的权重$\alpha_{t}$随着模型的预测误差$\epsilon_{t}$的减小而增大，也就是说基模型的预测误差越小，最后对于最终集成模型的权重越大。由于一般模型的准确率都大于0.5，因此$1 - \epsilon_{t} \geq 0.5$，所以$\frac{1 - \epsilon_{t}}{\epsilon_{t}} \geq 1$，所以$\alpha_{t}=\frac{1}{2}\ln \frac{1-\epsilon_{t}}{\epsilon_{t}} \geq 0$。

**3、如何更新样本的权重：**由于$y^{\left ( i \right )} \in \left \{ -1, +1 \right \}, h_{t}\left ( x^{\left ( i \right )} \right ) \in \left \{ -1, +1 \right \}$，因此$\omega_{t+1,i} = \frac{\omega_{t,i}}{Z_{t}}exp\left ( -\alpha_{t}y^{\left ( i \right )}h_{t}\left ( x^{\left ( i \right )} \right ) \right )$可以等价为：

\begin{align}
    \omega_{t+1,i} = \left\{\begin{matrix}
    \frac{\omega_{t,i}exp\left ( \alpha_{t} \right )}{Z_{t}} & h_{t}\left ( x^{\left ( i \right )} \right ) \neq y^{i}\\ 
    \frac{\omega_{t,i}exp\left ( -\alpha_{t} \right )}{Z_{t}} & h_{t}\left ( x^{\left ( i \right )} \right ) = y^{i}
    \end{matrix}\right.
\end{align}

相比于正确分类的样本，错误分类的样本权重被放大了$exp\left ( 2 \alpha t \right ) = \frac{1 - \epsilon_{t}}{\epsilon_{t}}$倍，因此也是$\alpha_{t}=\frac{1}{2}\ln \frac{1-\epsilon_{t}}{\epsilon_{t}} \geq 0$中$\frac{1}{2}$的由来。

**4、如何将多个基模型组合在一起：**将各个模型的权重$\alpha_{t}$与对应的基模型的预测结果$h_{t}\left ( x \right )$相乘后求和。

# 3.AdaBoost的损失函数

在AdaBoost中，第$t$轮计算中，通过带有权重的数据集$D_{t}$得到一个基模型$h_{t}\left ( x \right )$和对应的权重$\alpha_{t}$，这时得到的集成模型为$H_{t}\left ( x \right )$，其对应的损失函数为：

\begin{align}
    L\left ( y, H_{t}\left ( x \right ) \right ) = \sum_{i=1}^{m}exp\left ( -y^{\left ( i \right )}H_{t}\left ( x^{\left ( i \right )} \right ) \right )
\end{align}

其中$m$为样本总数，$t$为当前训练的基模型的个数和第$t$个基模型。

通过AdaBoost最终模型的表达式$H\left ( x \right ) = \sum_{t=1}^{T}\alpha_{t}h_{t}\left ( x \right )$可以得到：

\begin{align}
    H_{t}\left ( x \right ) = H_{t-1}\left ( x \right ) + \alpha_{t}h_{t}\left ( x \right )
\end{align}

可以得到损失函数为

\begin{align}
    L\left ( y, H_{t}\left ( x \right ) \right ) = \sum_{i=1}^{m}exp\left ( -y^{\left ( i \right )}\left ( H_{t-1}\left ( x^{\left ( i \right )} \right ) + \alpha_{t}h_{t}\left ( x^{\left ( i \right )} \right ) \right ) \right)
\end{align}

有了损失函数，最小化损失函数即可得到$\alpha_{t}$和$h_{t}{\left ( x \right )}$，即

\begin{align}
    \left ( \alpha_{t}, h_{t}\left ( x \right ) \right ) = arg \, \underset{\alpha ,h\left ( x \right )}{min} \sum_{i=1}^{m}exp\left ( -y^{\left ( i \right )}\left ( H_{t-1}\left ( x^{\left ( i \right )} \right ) + \alpha_{t}h_{t}\left ( x^{\left ( i \right )} \right ) \right ) \right)
\end{align}

# 参考资料

[AdaBoost：一个经典的自适应增强集成算法](https://mp.weixin.qq.com/s?__biz=MzU1MjYzNjQwOQ==&mid=2247485628&idx=1&sn=d11d3dbfb414614b980d72227549273c&chksm=fbfe522acc89db3cd36f7e875b389e1ce344bf1effe31877c1b0e7dd3111b46ec8ebe4c0eb9a&mpshare=1&scene=1&srcid=1111G2VhASsJxwHgzxgzAh4U&sharer_sharetime=1573470134351&sharer_shareid=98178f12e3ad48ff6ea4030d6be95e39&pass_ticket=MkqdUCw3eQi%2F6alllcv%2FqGjCfFLhCgOjS2xNHsKL6Fi6sWeIqC%2BZz2YVHbV2%2Fo9q#rd)