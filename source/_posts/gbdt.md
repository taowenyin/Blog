---
title: GBDT理解和推导
date: 2019-11-12 21:22:53
mathjax: true
updated: {{ date }}
tags: [集成学习, 机器学习]
categories: [机器学习]
---

# 1.GBDT介绍

GBDT（Gradient Boosting Descision Tree，梯度上升决策树），与AdaBoost不同的是，GBDT限定了基模型只能是CART回归树（不是分类树），其次是采用梯度提升的思想来不断更新模型。

> 例：假设一个人的年龄为30岁，现在要使用模型来预测。第一次拟合目标为30，假设预测值为20，两者相差10；第二次拟合目标不再是30，而是对应的前面的残差10，假设这次预测值为8，这次残差为2；此时如果继续预测，那下一次的拟合目标就是2。以此类推，每次拟合的目标都是上一次的残差值。

GBDT其实就是参考这一思想。假设前一个迭代的集成模型为$H_{t-1}\left ( x \right )$，对应的损失函数为$L\left ( y, H_{t-1}\left ( x \right ) \right )$，那么在本轮的迭代中就需要找到一个拟合目标，并训练一个模型$h_{t}\left ( x \right )$，使得本轮的集成模型为$H_{t}\left ( x \right ) = H_{t-1}\left ( x \right ) + h_{t}\left ( x \right )$，并且对应的损失函数$L\left ( y, H_{t}\left ( x \right ) \right )$最小。在实际应用中，如果拟合的目标只是残差的话，是有一定的局限性，因为只有损失函数为平方损失函数时，那么拟合目标为残差才能降低损失函数，但是是其他损失函数，那么拟合目标就不是仅仅是残差。

如果损失函数为均方差损失函数$L\left ( y, H_{t}\left ( x \right ) \right ) = \left ( \frac{1}{2} \right ) \ast \left ( y_{i} - H_{t}\left ( x \right ) \right )^{2}$，那么对$H_{t}\left ( x \right )$进行求导，就可以得到负梯度：

\begin{align}
    -\left [ \frac{\partial L\left ( y, H_{t}\left ( x \right ) \right )}{\partial H_{t}\left ( x \right )} \right ] = \left ( y - H_{t}\left ( x \right ) \right )
\end{align}

此时，拟合的目标恰好为残差，所以只有平方损失函数的拟合目标为残差。为了把GBDT扩展到更复杂的损失函数，**因此提出了使用上一轮的损失函数的负梯度来作为本轮的拟合目标。**

# 2.为什么使用负梯度

为了证明拟合目标的损失函数的负梯度就能够降低损失函数，需要在证明的过程中使用泰勒一阶展开公式，因此泰勒一阶展开的形式为：

\begin{align}
    f\left ( x \right ) \approx f\left ( x_{0} \right ) + {f}'\left ( x_{0} \right )\left ( x - x_{0} \right )
\end{align}

而在$t$轮的集成模型可以表示为：

\begin{align}
    H_{t}\left ( x \right ) = H_{t-1}\left ( x \right ) + h_{t}\left ( x \right )
\end{align}

因此，第$t$轮的损失函数$L\left ( y, H_{t}\left ( x \right ) \right )$在$H_{t-1}\left ( x \right )$处进行一阶泰勒展开为：

\begin{align}
    \begin{matrix}
        L\left ( y, H_{t}\left ( x \right ) \right ) \approx L\left ( y, H_{t-1}\left ( x \right ) \right ) + \frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )}\left ( H_{t}\left ( x \right ) - H_{t-1}\left ( x \right ) \right )\\ 
        = L\left ( y, H_{t-1}\left ( x \right ) \right ) + \frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )}h_{t}\left ( x \right )
    \end{matrix}
\end{align}

如果想要$L\left ( y, H_{t}\left ( x \right ) \right ) \leq L\left ( y, H_{t-1}\left ( x \right ) \right )$，那么上式中的$\frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )}h_{t}\left ( x \right ) \leq 0$，那么就可以使：

\begin{align}
    h_{t}\left ( x \right ) = -\frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )}
\end{align}

此时

\begin{align}
    L\left ( y, H_{t}\left ( x \right ) \right ) = L\left ( y, H_{t-1}\left ( x \right ) \right ) - \left ( \frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )} \right )^2
\end{align}

从上面的过程可以知道$h_{t}\left ( x \right ) = -\frac{\partial L\left ( y, H_{t-1}\left ( x \right ) \right )}{\partial H_{t-1}\left ( x \right )}$，因此如果第$t$轮的基模型$h_{t}\left ( x \right )$拟合的目标是$t-1$轮损失函数的负梯度，那么第$t$轮的损失函数最小。如果损失函数为平方损失函数是，负梯度也就变为了残差。**GBDT的求解过程可以认为是在函数空间的梯度下降，也就是将带求解的模型函数作为梯度下降中要求解的参数。**

# 3.GBDT算法流程

假设训练集$D=\left \{ \left ( x^{\left ( 1 \right )}, y^{\left ( 1 \right )} \right ),\left ( x^{\left ( 2 \right )}, y^{\left ( 2 \right )} \right ), \cdots ,\left ( x^{\left ( m \right )}, y^{\left ( m \right )} \right ) \right \}$，其中样本的个数为$m$，$y^{\left ( i \right )} \in \left \{ -1, +1 \right \}, i \in \left \{ 1, 2, \cdots , m \right \}$，并且基模型的个数为$T$。

1、初始化模型$H_{0}\left ( x \right ) = 0$。

2、令$t = 1, 2, \cdots , T$，循环执行下面的语句：

（1）对于每个样本$i = 1, 2, \cdots , m$，计算$t-1$轮损失函数的负梯度$\frac{\partial L\left ( y^{\left ( i \right )}, H_{t-1}\left ( x^{\left ( i \right )} \right ) \right )}{\partial H_{t-1}\left ( x^{\left ( i \right )} \right )}$，并更新$t$轮样本的标签$y^{\left ( i \right )}$。

（2）根据更新后的$\left ( x^{\left ( i \right )}, y^{\left ( i \right )} \right ), i = 1, 2, \cdots , m$训练得到第$t$轮的基模型（CART回归树）$h_{t}\left ( x \right )$。

（3）生成第$t$轮集成模型$H_{t}\left ( x \right ) = H_{t-1}\left ( x \right ) + h_{t}\left ( x \right )$。

# 4.GBDT正则化

GBDT正则化常见的有三种方式：

1、类似于AdaBoost中的学习率$v$，则集成模型变为

\begin{align}
    H_{t}\left ( x \right ) = H_{t-1}\left ( x \right ) + vh_{t}\left ( x \right )
\end{align}

$v$的取值范围是$0 < v \leq 1$，对于同样的训练集学习效果，$v$越小就表示需要更多的迭代次数，通常用学习率和迭代最大次数一起来决定算法的拟合效果。

2、采用控制子采样（SubSample）的比例，也就是说只是用一部分样本训练，但是如果采样比例过小的话，在降低方差的同时也会提高偏差，因此不能过低。

3、控制每个决策树（基模型）的复杂度，也就是对决策树进行一些剪枝操作。

# 参考资料

[XGBoost都已经烂大街了，你还不知道GBDT是咋回事？](https://mp.weixin.qq.com/s?__biz=MzU1MjYzNjQwOQ==&mid=2247485694&idx=1&sn=9a07363ba6e3993d6bc8d03cff491bb6&chksm=fbfe5268cc89db7e994c698fad2cc51ff4820d175d3e93def7ce21a820327391d5c1863077e3&mpshare=1&scene=1&srcid=1111PiSfwyQV742U1d1y0N3i&sharer_sharetime=1573563502132&sharer_shareid=98178f12e3ad48ff6ea4030d6be95e39&pass_ticket=RJ0j6JdTxwW3VpGnjiKrEDd90fErtIsefimQSsFiOiGzIaL7WonIFJuRM%2BUUNQ1P#rd)