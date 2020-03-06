---
title: S3-$k$近邻法
mathjax: true
date: 2020-03-06 15:50:16
updated: {{ date }}
tags:
categories: [统计学习方法]
---

# $k$近邻算法

输入：训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

其中，$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$为实例的特征向量，$y_{i} \in \mathcal{Y}=\left\{c_{1}, c_{2}, \cdots, c_{K}\right\}$为实例的类别，$i=1,2, \cdots, N$；实例特征向量$x$；

输出：实例$x$所属的类$y$。

（1）根据给定的距离度量，**在训练集$T$中找出与$x$最邻近的$k$个点**，涵盖这$k$个点的$x$的邻域记作$N_{k}(x)$。

（2）在$N_{k}(x)$中根据分类决策规则决定$x$的类别$y$；

$$y=\arg \max _{c_{j}} \sum_{x_{i} \in N_{k}(x)} I\left(y_{i}=c_{j}\right), \quad i=1,2, \cdots, N ; j=1,2, \cdots, K$$

其中，$I$为指示函数，即当$y_{i}=c_{j}$时，$I=1$，否则$I=0$。

当$k=1$时，称为最近邻算法。

**当有新实例点$x$时，最近邻将训练数据集中与$x$最邻近点的类最为$x$的类。**

# $k$近邻模型

$k$近邻模型有三个基本要素：**距离度量、$k$值选择和分类决策。**

## 距离度量

设特征空间$\mathcal{X}$是$n$维实数向量空间$\mathbf{R}^{n}$，$x_{i}, x_{j} \in \mathcal{X}$，$x_{i}=\left(x_{i}^{(1)}, x_{i}^{(2)}, \cdots, x_{i}^{(n)}\right)^{\mathrm{T}}$，$x_{j}=\left(x_{j}^{(1)}, x_{j}^{(2)}, \cdots, x_{j}^{(n)}\right)^{\mathrm{T}}$，$x_{i}$、$x_{j}$的$L_{p}$距离定义为

$$L_{p}\left(x_{i}, x_{j}\right)=\left(\sum_{l=1}^{n}\left|x_{i}^{(l)}-x_{j}^{(l)}\right|^{p}\right)^{\frac{1}{p}}$$

当$p=1$时，该距离为曼哈顿距离。

当$p=2$时，该距离为欧时距离。

当$p=\infty$，该距离为各个坐标距离的最大值

$$L_{\infty}\left(x_{i}, x_{j}\right)=\max _{l}\left|x_{i}^{(l)}-x_{j}^{(l)}\right|$$

## $k$值选择

当$k$值较小时，预测结果对近邻实例点非常敏感，容易受噪声影响，同时也意味着模型变得复杂，容易过拟合。

当$k$值较大时，与输入实例较远的不相关的实例也会对预测起作用，使预测发生错误，同时模型也变得简单。

**因此$k$值的选择一般取较小的值，并通过交叉验证来选取最优$k$值。**

## 分类决策规则

$k$近邻法中通常采用**多数表决**，即输入实例的$k$个邻近的训练实例中的多数类决定输入实例的类。

对给定的实例$x \in \mathcal{X}$，其最近邻的$k$个训练实例点构成集合$N_{k}(x)$。如果涵盖$N_{k}(x)$的区域的类别是$c_{j}$，那么误分类率是

$$\frac{1}{k} \sum_{x_{i} \in N_{k}(x)} I\left(y_{i} \neq c_{j}\right)=1-\frac{1}{k} \sum_{x_{i} \in N_{k}(x)} I\left(y_{i}=c_{j}\right)$$

要使误分类率最小即经验风险最小，就要使$\underset{x_{i} \in N_{k}(x)}{\sum} I\left(y_{i}=c_{j}\right)$最大，所以多数据表决规则等价于经验风险最小化。

# $k$近邻法的实现：$kd$树

## 构造$kd$树

输入：$k$维空间数据集$T=\left\{x_{1}, x_{2}, \cdots, x_{N}\right\}$，其中$x_{i}=\left(x_{i}^{(1)}, x_{i}^{(2)}, \cdots, x_{i}^{(k)}\right)^{\mathrm{T}}$，$i=1,2, \cdots, N$

算法步骤：

> 第一步：以$x^{(1)}$的中位数作为切分点 **（注意：取最靠近真正中位数的点）**，把超矩形区域切分为**深度为1**的两个子区域，左区域小于切分点，右区域大于切分点。**此时的根节点的深度为0。** 同时，**把该切分点保存为根结点。**
> 
> 第二步（重复）：**在每个子区域**，对深度为$j$的节点，选择$x^{(l)}$作为切分的坐标轴（$l=j(\bmod k)+1$），把区域分为深度为$j+1$的两个部分。左区域小于切分点$x^{(l)}$，右区域大于切分点$x^{(l)}$。
>
> 第三步：直到两个子区域没有实例时，则停止，从而形成$kd$树。

## 搜索$kd$树

输入：已构造的$kd$树，目标点$x$

算法步骤：

> 第一步：从根结点出发，递归向下方位$kd$树，根据节点把目标点$x$进行左移或右移，直到也节点。
>
> 第二步：以此叶节点为“当前最近实例点”。
>
> 第三步：递归向上回退。
>
> 第四步：如果回退的节点比“当前最近实例点”距离近，那么就把该回退的节点作为“当前最近实例点”。
>
> 第五步：如果检查“当前最近实例点”的父节点的另一子节点对应的区域是否有更近的点。方法是，以目标点与“当前最近实例点”的距离为半径形成一个超球体，看是否有相交。如果有相交，那么说明可能有“当前最近实例点”，那么就移动到父节点的另一个区域进行最近邻搜索。如果没有相交，那么就继续向上回退。
>
> 第六步：当到根结点时，搜索结束，所有一个“当前最近实例点”就是目标点$x$的最近邻点。

$kd$树搜索的平均计算复杂度是$O(\log N)$。

# 作业

1、思考$k$近邻算法的模型复杂度体现在哪里？什么情况下会造成过拟合？

$k$近邻算法的模型复杂度体现在$k$值的大小。当$k$过小时就会容易造成过拟合。

2、给定一个二维空间的数据集$T=\{(5,4),(9,6),(4,7),(2,3),(8,1),(7,2)\}$，$Y=\{1, 1, 1, 0, 0, 0\}$，找到数据点$S=(5, 3)$的最近邻（$k=1$），并对$S$点进行分类预测。

（1）用“线性扫描”算法自编程实现

```python
import numpy as np


class KNNModel:
    """
    input_x：实例数据集
    input_y：标签结果
    k：近邻数
    S：数据点
    """
    def __init__(self, input_x, input_y, k, s):
        self._input_x = input_x
        self._input_y = input_y
        self._s = s

    def train(self):
        # 以map形式保存距离
        d = {}
        for i in range(len(self._input_x)):
            # 计算欧氏距离
            l = np.sqrt(np.sum(self._input_x[i] - self._s)**2)
            # 保存距离
            d[l] = i

        # 升序排序后的结果
        d_sorted = sorted(d.keys())
        # 得到最近的那个索引
        index = d[d_sorted[0]]

        # 打印分类
        print(self._input_y[index])


if __name__ == '__main__':
    train_x = np.array([
        [5, 4],
        [9, 6],
        [4, 7],
        [2, 3],
        [8, 1],
        [7, 2],
    ])
    train_y = np.array([1, 1, 1, -1, -1, -1])
    s = np.array([(5, 3)])

    knn = KNNModel(train_x, train_y, 1, s)
    knn.train()
```

（2）试调用sklearn.neighbors的KNeighborsClassifier模块，对$S$点进行分类预测，并对比近邻数$k$取值不同，对分类预测结果的影响。

```python
import numpy as np

from sklearn.neighbors import KNeighborsClassifier

if __name__ == '__main__':
    train_x = np.array([
        [5, 4],
        [9, 6],
        [4, 7],
        [2, 3],
        [8, 1],
        [7, 2],
    ])
    train_y = np.array([1, 1, 1, -1, -1, -1])
    s = np.array([(5, 3)])

    knn = KNeighborsClassifier(n_neighbors=1, n_jobs=-1)
    knn.fit(train_y, train_y)
    y = knn.predict(s)

    print('Predict Y = %s' % y)

```

（3）思考“线性扫描”算法和“kd树”算法的时间复杂度。

$kd$树搜索的平均计算复杂度是$O(\log N)$，而线性扫描的计算复杂度是$O(N)$