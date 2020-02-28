---
title: 各类数据集说明
mathjax: true
date: 2020-02-18 19:46:01
updated: {{ date }}
top: true
tags:
categories: [数据集]
---

# [UCI](http://archive.ics.uci.edu/ml/datasets.php)

## [SMS Spam Collection Data Set（垃圾短信数据集）](http://archive.ics.uci.edu/ml/datasets/SMS+Spam+Collection)

| 特点 | 正例 | 反例 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 文本数据 | 4825 | 747 | 5574 | 分类、聚类 |

## [Internet Advertisements Data Set（网络广告）](http://archive.ics.uci.edu/ml/datasets/Internet+Advertisements)

| 特点 | 正例 | 反例 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 图片数据 | 2821 | 458 | 3279 | 分类 |

## [Iris Data Set（鸢尾花数据集）](http://archive.ics.uci.edu/ml/datasets/Iris)

| 特点 | 类别数 | 总数 | 特征数 | 推荐领域 |
| :---: | :---: | :---: | :---: | :---: |
| 文本数据 | 3 | 150 | 4 | 分类 |

# [kaggle](https://www.kaggle.com/)

## [Sentiment Analysis on Movie Reviews（影评中的情感分析）](https://www.kaggle.com/c/sentiment-analysis-on-movie-reviews/overview)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 文本数据 | 5 | 156060 | 分类、聚类 |

# [LIBSVM Dataset](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/)

## [scene-classification（多标签场景分类）](https://www.csie.ntu.edu.tw/~cjlin/libsvmtools/datasets/multilabel.html#scene-classification)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 图像数据 | 6 | 2407 | 多标签分类 |

| 编号 | 类别 | 训练图片 | 测试图片 | 合计 |
| :---: | :---: | :---: | :---: | :---: |
| 0 | 海滩（Beach） | 194 | 175 | 369 |
| 1 | 日落（Sunset） | 165 | 199 | 364 |
| 2 | 落叶（Fall foliage） | 184 | 176 | 360 |
| 3 | 田（Field） | 161 | 166 | 327 |
| 0,3 | 海滩+田（Beach+Field） | 0 | 1 | 1 |
| 2,3 | 落叶+田（Fall foliage+Field） | 8 | 16 | 23 |
| 4 | 山（Mountain） | 223 | 182 | 405 |
| 0,4 | 海滩+山（Beach+Mountain） | 21 | 17 | 38 |
| 2,4 | 落叶+山（Fall foliage+Mountain） | 5 | 8 | 13 |
| 3,4 | 田+山（Field+Mountain） | 26 | 49 | 75 |
| 2,3,4 | 田+落叶+山（Field+Fall foliage+Mountain） | 1 | 0 | 1 |
| 5 | 城市（Urban） | 210 | 195 | 405 |
| 0,5 | 海滩+城市（Beach+Urban） | 12 | 7 | 19 |
| 3,5 | 田+城市（Field+Urban） | 1 | 5 | 6 |
| 4,5 | 山+城市（Mountain+Urban） | 1 | 5 | 6 |
|  | 合计 | 1211 | 1196 | 2407 |


# 其他

## [20 Newsgroups（新闻组文档）](http://qwone.com/~jason/20Newsgroups/)

| 特点 | 类别数 | 总数 | 推荐领域 |
| :---: | :---: | :---: | :---: |
| 文本数据 | 20 | 20000 | 分类 |