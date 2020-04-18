---
title: S8-提升方法
mathjax: true
date: 2020-04-16 09:41:02
updated: {{ date }}
tags:
categories: [统计学习方法]
---

提升（boosting）方法通过改变训练样本的权重，学习多个分类器，并将这些分类器进行线性组合，从而提高分类的性能，其中典型的方法就是AdaBoost方法。

# 提升方法AdaBoost算法

## 提升方法的基本思路

基本思想：对于复杂任务来说，将多个专家的判断进行适当的综合所得出的判断，要比其中任何一个专家独立的判断好。在概率近似正确（probably approximately correct, PAC）学习框架下，一个强可学习的充分必要条件是弱可学习。因此，**“提升方法”的目标是把弱可学习提升为强可学习，即从弱学习器出发，反复学习，得到一系列弱分类器（基本分类器），然后组合这些弱分类器，从而构成一个强分类器。**

> 1、每一轮如何改变训练数据的权值或概率分布？

AdaBoost的做法是提高被前一轮弱分类器错分类样本的权值，并降低被正确分类样本的权值，从而使得错分类的数据能在接下来弱分类器中获得更大关注。

> 2、如何将弱分类器组合成一个强分类器？

AdaBoost的做法是加权多数表决，即加大误分类小的弱分类器的权值，减少误分类大的弱分类器的权值。

## AdaBoost算法

假设给定一个二分类的训练数据集

$$T=\left\{\left(x_{1}, y_{1}\right),\left(x_{2}, y_{2}\right), \cdots,\left(x_{N}, y_{N}\right)\right\}$$

实例$x_{i} \in \mathcal{X} \subseteq \mathbf{R}^{n}$，标记$y_{i} \in \mathcal{Y}=\{-1,+1\}$

1、初始化训练数据的权值分布

$$D_{1}=\left(w_{11}, \cdots, w_{1 i}, \cdots, w_{1 N}\right), \quad w_{1 i}=\frac{1}{N}, \quad i=1,2, \cdots, N$$

2、对$m=1,2, \cdots, M$

（1）使用具有权值分布$D_{m}$的训练数据学习，得到基本分类器（弱分类器）

$$G_{m}(x): \mathcal{X} \rightarrow \{-1,+1\}$$

（2）计算$G_{m}(x)$在训练数据集上的分类误差率

$$e_{m}=\sum_{i=1}^{N} P\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)=\sum_{i=1}^{N} w_{m i} I\left(G_{m}\left(x_{i}\right) \neq y_{i}\right)$$

（3）计算$G_{m}(x)$的系数

$$\alpha_{m}=\frac{1}{2} \log \frac{1-e_{m}}{e_{m}}$$

此处的对数为自然对数。

（4）更新训练数据集的权值分布

$$D_{m+1}=\left(w_{m+1,1}, \cdots, w_{m+1, i}, \cdots, w_{m+1, N}\right)$$

$$w_{m+1, i}=\frac{w_{m i}}{Z_{m}} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right), \quad i=1,2, \cdots, N$$

其中$Z_{m}$表示规范化因子

$$Z_{m}=\sum_{i=1}^{N} w_{m i} \exp \left(-\alpha_{m} y_{i} G_{m}\left(x_{i}\right)\right)$$

它使$D_{m+1}$成为一个概率分布

3、构建基本分类器的线性组合

$$f(x)=\sum_{m=1}^{M} \alpha_{m} G_{m}(x)$$

得到最终分类器

$$\begin{aligned}
G(x) &=\operatorname{sign}(f(x)) \\
&=\operatorname{sign}\left(\sum_{m=1}^{M} \alpha_{m} G_{m}(x)\right)
\end{aligned}$$

# AdaBoost算法的训练误差

> 1、AdaBoost算法最终分类器的训练误差界为

$$\frac{1}{N} \sum_{i=1}^{N} I\left(G\left(x_{i}\right) \neq y_{i}\right) \leqslant \frac{1}{N} \sum_{i} \exp \left(-y_{i} f\left(x_{i}\right)\right)=\prod_{m} Z_{m}$$

> 2、二分类问题AdaBoost的训练误差界

$$\begin{aligned}
\prod_{m=1}^{M} Z_{m} &=\prod_{m=1}^{M}[2 \sqrt{e_{m}\left(1-e_{m}\right)}] \\
&=\prod_{m=1}^{M} \sqrt{\left(1-4 \gamma_{m}^{2}\right)} \\
& \leqslant \exp \left(-2 \sum_{m=1}^{M} \gamma_{m}^{2}\right)
\end{aligned}$$

其中$\gamma_{m}=\frac{1}{2}-e_{m}$

> 3、如果存在$\gamma>0$，对所有$m$有$\gamma_{m} \geqslant \gamma$，则

$$\frac{1}{N} \sum_{i=1}^{N} I\left(G\left(x_{i}\right) \neq y_{i}\right) \leqslant \exp \left(-2 M \gamma^{2}\right)$$

# 工程实践

```python
sklearn.ensemble.AdaBoostClassifier(
    base_estimator=None, n_estimators=50, learning_rate=1., 
    algorithm='SAMME.R', random_state=None
)
```

1. base_estimator（类型：object，默认值：DecisionTreeClassifier(max_depth=1)）：基本分类器。
2. n_estimators（类型：int，默认值：50）：分类器的最大数量
3. learning_rate（类型：float，默认值：1.）：取值范围为$\left ( 0,1 \right ]$。
4. algorithm（类型：str，默认值：'SAMME.R'）：算法收敛的速度
5. random_state（类型：int，默认值：None）：随机数种子。

# 作业

1、自编程实现课本例题8.1

```python
import numpy as np
import time


# AdaBoost算法类
class AdaBoost:
    def __init__(self, x, y, tol=0.05, max_iter=10):
        self.x = x
        self.y = y
        self.tol = tol
        self.max_iter = max_iter
        # 初始化样本权重：（1）步骤
        self.w = np.full(x.shape[0], 1 / x.shape[0])
        # 弱分类器的权重
        self.alpha = []
        # 弱分类器
        self.G = []
        # 分类器阈值的上下限
        self.v_min = min(x) - 0.5
        self.v_max = max(x) + 0.5

        self._select_v()

    # 计算阈值，返回阈值，符号，和误分类值
    def _select_v(self):
        # 误分类的最小值
        e_min = np.inf
        # 小于阈值的样本分类
        sign = None
        # 最佳的阈值
        v_best = None

        # 遍历所有可能的阈值
        for v in np.arange(self.v_min, self.v_max + 0.5, 1):
            # 计算小于和大于阈值，把错分类的数据和样本权重相乘：（3）（b）步骤
            e_1 = abs((self.y[self.x < v] - 1) * self.w[self.x < v]) # 由于采用了+1、-1方式，因此最终值为原值的2倍，并且有正负号，需要取绝对值
            e_2 = abs((self.y[self.x > v] + 1) * self.w[self.x > v])
            e = (e_1.sum() + e_2.sum()) / 2 # 计算误分类值
            # 若误差小于0.5，那么说明x < v时，y = 1的假设是正确的，否则就是错误的
            if e < 0.5:
                flag = 1
            else:
                e = 1 - e
                flag = -1

            # 寻找最小的误分类，并保存误分类值、阈值和分类
            if e < e_min:
                e_min = e
                sign = flag
                v_best = v

        return v_best, sign, e_min

    def fit(self):
        G = 0
        for i in range(self.max_iter):
            # 获得最佳参数
            v_best, sign, e_min = self._select_v()
            # 计算基础模型的alpha系数：（2）（c）步骤
            alpha = 1 / 2 * np.log((1 - e_min) / e_min)
            # 保存模型alpha系数
            self.alpha.append(alpha)
            # 保存模型弱分类器参数
            self.G.append((v_best, sign))
            # 计算基本分类器的结果：（3）步骤
            _G = self.base_estimator(self.x, i)
            # 把基本分类器的结果与对应的模型权重相乘
            G = G + alpha * _G
            # 计算总模型的预测结果
            y_predict = np.sign(G)
            # 计算误差率
            error_rate = np.sum(np.abs(y_predict - self.y)) / 2 / self.y.shape[0]
            # 如果误差率小于设定值，则结束
            if error_rate < self.tol:
                print('迭代次数:', i + 1)
                break
            else:
                # 更新权重
                self._update_w()


    # 选择第i分类器进行预测
    def base_estimator(self, x, i):
        # 获取基本模型的数据
        v_best, sign = self.G[i]
        _G = x.copy()
        # 对输入数据进行预测，并返回预测值
        _G[_G < v_best] = sign
        _G[_G > v_best] = -sign
        return _G

    # 更新权重
    def _update_w(self):
        # 获取最后一个模型的信息
        v_best, sign = self.G[-1]
        # 获取最后一个模型的alpha
        alpha = self.alpha[-1]
        # 重建分类器结果
        G = np.zeros(self.y.size, dtype=int)
        G[self.x < v_best] = sign
        G[self.x > v_best] = -sign

        # 计算更新的权重：（2）（d）步骤
        _w = self.w * np.exp(-1 * alpha * self.y * G)
        self.w = _w / _w.sum()

    # 对数据进行预测
    def predict(self, x):
        G = 0
        for i in range(len(self.alpha)):
            _G = self.base_estimator(x, i)
            alpha = self.alpha[i]
            G = G + alpha * _G

        y_predict = np.sign(G)
        return y_predict.astype(int)

    # 计算分数
    def score(self, x, y):
        y_predict = self.predict(x)
        error_rate = np.sum(np.abs(y_predict - self.y)) / 2 / self.y.shape[0]
        return 1 - error_rate

# AdaBoost
if __name__ == '__main__':
    # 计算开始时间
    star = time.time()

    data = np.array([
        [0, 1], [1, 1], [2, 1], [3, -1], [4, -1], [5, -1], [6, 1], [7, 1], [8, 1], [9, -1]
    ])

    train_x = data[:, 0]
    train_y = data[:, 1]

    clf = AdaBoost(train_x, train_y)
    clf.fit()
    y_predict = clf.predict(train_x)
    score = clf.score(train_x, train_y)

    # 计算结束事时间
    end = time.time()
    print('用时：{:.3f}s'.format(end - star))
```

2、调用sklearn.ensemble.AdaBoostClassifier对例题8.1进行实现

```python
import numpy as np
import time

from sklearn.ensemble import AdaBoostClassifier

# AdaBoost
if __name__ == '__main__':
    # 计算开始时间
    star = time.time()

    data = np.array([
        [0, 1], [1, 1], [2, 1], [3, -1], [4, -1], [5, -1], [6, 1], [7, 1], [8, 1], [9, -1]
    ])

    train_x = data[:, 0].reshape(10, 1)
    train_y = data[:, 1]

    clf = AdaBoostClassifier()
    clf.fit(train_x, train_y)
    y_predict = clf.predict(train_x)
    score = clf.score(train_x, train_y)

    # 计算结束事时间
    end = time.time()
    print('用时：{:.3f}s'.format(end - star))
```