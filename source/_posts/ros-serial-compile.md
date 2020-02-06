---
title: ROS串口标准库serial的编译与使用
mathjax: true
date: 2020-02-06 11:33:01
updated: {{ date }}
tags: [ROS, 串口通信]
categories: [ROS]
---

[参考链接](http://wjwwood.io/serial/)

> * 1、下载serial库

```bash
git clone https://github.com/wjwwood/serial.git
```

> * 2、编译serial库

```bash
sudo apt-get install doxygen
cd serial
make
make test
make docs
make install
make uninstall（卸载）
```

> * 3、复制到ros库

```bash
cp -R /tmp/usr/local/include/ /opt/ros/kinetic/
cp -R /tmp/usr/local/lib/ /opt/ros/kinetic/
cp -R /tmp/usr/local/share/ /opt/ros/kinetic/
```

> * 4、创建serial依赖的功能包

```bash
catkin_create_pkg <package_name> serial std_msgs roscpp rospy
```