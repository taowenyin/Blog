---
title: ROS键盘检测
mathjax: true
date: 2020-02-06 11:36:19
updated: {{ date }}
tags: [ROS, 调试]
categories: [ROS]
---

> * 1、进入官网下载keyboard_reader（[官网](https://github.com/UTNuclearRoboticsPublic/keyboard_reader)）。

> * 2、复制keyboard_reader文件夹到ROS工作空间的src目录中。

> * 3、修改keyboard_reader.cpp文件中的第118行内容。

> * 4、修改keyboard_reader.cpp文件中的第51行内容，以选择正确的键盘。

> * 5、在ROS工作空间中执行catkin_make指令进行编译。

> * 6、keyboard_reader项目的做法主要是创建一个“keyboard”话题，并发送自定义的Key.msg消息

> * 7、创建订阅。

> * 8、修改package.xml，添加keyboard_reader依赖。

```xml
<build_depend>keyboard_reader</build_depend>
<build_export_depend>keyboard_reader</build_export_depend>
<exec_depend>keyboard_reader</exec_depend>
```

> * 9、修改CMakeLists.txt，添加keyboard_reader依赖。

```makefile
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  keyboard_reader # 添加的内容
)

# 添加MSG依赖
add_dependencies(<package_name> <depend_package_name>_generate_messages_cpp)
add_dependencies(ros_keyboard keyboard_reader_generate_messages_cpp)
```