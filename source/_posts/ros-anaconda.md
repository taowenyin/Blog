---
title: Anaconda环境下的ROS配置
mathjax: true
date: 2020-02-06 11:22:15
updated: {{ date }}
tags: [ROS, 环境配置]
categories: [ROS]
---

1、安装Anaconda

2、创建Python2.7的环境catkin_ws（可以自行修改成需要的名字）

3、执行环境切换命令`source activate catkin_ws`

4、安装相关依赖

```python
pip install -U rospkg
conda install numpy
conda install pyqt
conda install pydot
conda install pyqtgraph
```

5、在PyCharm中添加ros-python

（1）File->Settings->Project Interpreter->右边小齿轮->Show All->选中正在使用的python

（2）点击右边最下面的图标，打开Interpreter Path

（3）添加`/opt/ros/indigo/lib/python2.7/dist-packages`

6、编写代码，所有的Python顶部必须添加`#!/usr/bin/env python`