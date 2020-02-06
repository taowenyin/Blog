---
title: rosserial-stm32的编译与安装
mathjax: true
date: 2020-02-06 11:27:33
updated: {{ date }}
tags: [ROS, 串口通信]
categories: [ROS]
---

[参考链接1](https://blog.csdn.net/m0_38089090/article/details/79870815)

[参考链接2](https://blog.csdn.net/qq_37416258/article/details/84844051)

> * 1、安装ROS串口库，指令如下。

```bash
sudo apt-get install ros-kinetic-rosserial
```

> * 2、进入官网下载rosserial_stm32（[官网](http://wiki.ros.org/rosserial)）。

> * 3、把下载后的文件解压并重命名为rosserial_stm32，进入ROS的工作空间。

> * 4、复制rosserial_stm32文件夹到ROS工作空间的src目录中。

> * 5、在ROS工作空间中执行catkin_make指令进行编译。

> * 【可能需要的步骤】执行安装指令，并载入环境安装目录中的环境变量，指令如下。

```bash
catkin_make install
source ROS工作空间/install/setup.bash
```

> * 6、进入STM32CubeMx创建的项目目录，并执行如下指令。

```bash
rosrun rosserial_stm32 make_libraries.py .
```

> * 7、修改Inc目录下的STM32Hardware.h文件，并把头文件中的“stm32f3xx_halxxx.h”修改为“stm32f1xx_halxxx.h”。

> * 8、修改main.c->main.cpp。

> * 9、修改CMakeLists.txt中的PROJECT(HelloSTM32ROS C CXX ASM)为PROJECT(HelloSTM32ROS CXX C ASM)。

> * 10、修改Inc目录下的STM32Hardware.h文件中的定时器和串口对象。

```c
extern TIM_HandleTypeDef htim2;
extern UART_HandleTypeDef huart1;
```

> * 11、添加roscore文件夹和mainpp.cpp、mainpp.h，并完成相应内容。（注：文件名和文件夹名可以自定义）

> * 12、使用该库需要启动rosserial作为数据传递的中介。

```bash
rosrun rosserial_python serial_node.py
```