---
title: ROS节点调试
mathjax: true
date: 2020-02-06 11:35:15
updated: {{ date }}
tags: [ROS, 调试]
categories: [ROS]
---

> * 1、将下面两行加入到CMakeLists.txt中

```bash
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
set (CMAKE_VERBOSE_MAKEFILE ON)
```

> * 2、修改launch.json中的program

```bash
${workspaceFolder}/devel/lib/<package_name>/<node_name>
```