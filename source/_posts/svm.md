---
title: scikit-learn中的SVM（支持向量机）
mathjax: true
date: 2020-02-26 19:09:54
updated: {{ date }}
tags: [机器学习, 决策树]
categories: [基础]
---

# 函数原型

```python
sklearn.svm.SVC(
            C=1.0, kernel='rbf', degree=3, gamma='scale',
            coef0=0.0, shrinking=True, probability=False,
            tol=1e-3, cache_size=200, class_weight=None,
            verbose=False, max_iter=-1, decision_function_shape='ovr',
            break_ties=False, random_state=None)
```

1. C（类型：flot，默认值：1.0，`超参：是`）：正则化参数，即惩罚项的系数。
2. kernel（类型：str，默认值：rbf，`超参：是`）：核函数的选择。scikit-learn中支持4种核函数，分别是：
    - linear：线性核函数$K\left(x, x^{\prime}\right)=\left\langle x, x^{\prime}\right\rangle$
    - poly：多项式核函数$K\left(x, x^{\prime}\right)=\left(\gamma\left\langle x, x^{\prime}\right\rangle+ r\right)^{d}$
    - rbf：径向核函数/高斯核函数$K\left(x, x^{\prime}\right)=\exp \left(-\gamma\left\|x-x^{\prime}\right\|^{2}\right)$
    - sigmoid：sigmoid核函数$K\left(x, x^{\prime}\right)=\tanh \left(\gamma\left\langle x, x^{\prime}\right\rangle+ r\right)$
    - precomputed：核矩阵，使用自定义转换矩阵
3. degree（类型：int，默认值：3，`超参：是`）：设置多项式的阶数。**只有kernel函数为poly才有效，即公式中的$d$。**
4. gamma（类型：int或str，默认值：scale，`超参：是`）：核函数的系数。**只有kernel函数为poly、rbf、sigmod才有效，即公式中的$\gamma$。**
    - 当gamma为scale时，$\text {gamma}=\frac{1}{\text {n_features} \times \text {X.var()}}$
    - 当gamma为auto时，$\text {gamma}=\frac{1}{\text {n_features}}$
5. coef0（类型：float，默认值：0.0，`超参：是`）：核函数的独立项。**只有kernel函数为poly、sigmod才有效，即公式中的$r$。**
6. shrinking（类型：boolean，默认值：True）：是否使用启发式的最优算法。
7. probability（类型：boolean，默认值：False）：是否使用概率估计。
8. tol（类型：float，默认值：1e-3）：停止训练的误差值大小。
9. cache_size（类型：float，默认值：200）：训练时所需要的内存大小，单位MB。
10. class_weight（类型：dict或str，默认值：None）：给每个类分别设置不同的惩罚参数C，即$\text {class_weight}\left [ i\right ] \times C$。如果参数是“balanced”，那么类别权重自动调整为与输入数据中的类频率成反比的权重$\frac{\text{n_samples}}{\text{n_classes} \times \text{np.bincount}\left ( y\right )}$。
11. verbose（类型：boolean，默认值：False）：是否启用详细输出。
12. max_iter（类型：int，默认值：-1）：最大迭代次数，如果为-1，那么不受限制。
13. decision_function_shape（类型：str，默认值：ovr）：决策函数类型。
    - ovo：返回形状为$\left (\text{n_samples}, \frac{\text{n_classes} \times \left ( \text{n_classes} - 1\right )}{2} \right )$的决策函数，该决策函数一般用于多类别策略。
    - ovr：返回形状为$\left (\text{n_samples}, \text{n_classes} \right )$的决策函数。
14. break_ties（类型：boolean，默认值：False）：当该值为True时，decision_function_shape为ovr，且类别数量大于2时，预测结果将根据decision_function_shape的置信度来来打破平局，否则就返回所有类别中的第一个。要注意的是打破平局相比简单的预测所需代价更高。
15. random_state（类型：int，默认值：None）：随机数种子，如果不设置则默认为`np.random`。

# 程序流程

1. 将原始数据转化为SVM算法软件或包所能识别的数据格式；
2. 将数据标准化；（防止样本中不同特征数值大小相差较大影响分类器性能）
3. 不知使用什么核函数，考虑使用RBF；
4. 利用交叉验证网格搜索寻找最优参数（C, γ）；（交叉验证防止过拟合，网格搜索在指定范围内寻找最优参数）
5. 使用最优参数来训练模型；
6. 测试。

# 核函数选择

1. 样本数量远小于特征数量：这种情况，利用情况利用linear核效果会高于RBF核。
2. 样本数量和特征数量一样大：linear核合适，且速度也更快。
3. 样本数量远大于特征数量：非线性核RBF等合适。

# GridSearchCV小技巧

网格大小如果设置范围大且步长密集的话难免耗时，但是不这样的话又可能找到的参数不是很好，针对这解决方法是，先在大范围，大步长的粗糙网格内寻找参数。在找到的参数左右在设置精细步长找寻最优参数比如：

1. 一开始寻找范围是$C=2^{-5},2^{-3}, \cdots ,2^{15}$和$\gamma = 2^{-15},2^{-13}, \cdots ,2^{3}$，由此找到的最优参数是$\left ( 2^{3}, 2^{-5}\right )$；
2. 然后设置更小一点的步长，参数范围变为$C=2^{1},2^{1.25}, \cdots ,2^{5}$和$\gamma = 2^{−7},2^{−6.75}, \cdots ,2^{-3}$

这样既可以避免一开始就使用大范围，小步长而导致分类器进行过于多的计算而导致计算时间的增加。

# 常用函数

1. 样本到分离超平面的距离

```python
sklearn.svm.SVC.decision_function(X)
```

2. 根据给定的训练数据拟合SVM模型

```python
sklearn.svm.SVC.fit(X, y[, sample_weight=None])
```

3. 获取此估算器的参数并以字典形式显示

```python
sklearn.svm.SVC.get_params([deep=True])
```

4. 根据测试数据集进行预测

```python
sklearn.svm.SVC.predict(X)
```

5. 返回给定测试数据和标签的平均精确度

```python
sklearn.svm.SVC.score(X, y)
```

6. 返回样本的对数概率以及普通概率，注意该函数只有在SVM构建时的probability参数为True时才有效。

```python
sklearn.svm.SVC.predict_log_proba(X_test)
sklearn.svm.SVC.predict_proba(X_test)
```