---


layout: post
title: ROS2 Journey Part II
subtitle: Understanding Nodes
tags: [ROS]

---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>


# ROS2 Journey II



Nodes是ROS2的核心概念之一，值得单独拿出来写一节。



### 1. Nodes in ROS 2

ROS中的每一个node都应该负责一个单独的模块化的任务，比如在ROS控制的小车里，一个node控制电机，一个控制激光测距仪。每一个node可以通过topics，services，actions or parameters接收和发送数据。下面的图说的很清楚。

![](/img/Nodes-TopicandService.gif)

在ROS 2中，一个C++或Python可执行文件可以包含一个或多个nodes。

### 2. Tasks

`ros2 run <package_name> <executable_name>`

以上是ros2 从package中运行可执行文件的基本方式。对于我们这个例子，应该是如下形式：

`ros2 run turtlesim turtlesim_node`

这将打开一个窗口。

![](/img/2023-03-03_23-20-08.png)

在这里package名为turtlesim，可执行文件名为turtlesim_node。同时用`ro2 node list`查看正在运行的node name。

![](/img/2023-03-03_23-22-36.png)

这时候再打开一个terminal，运行teleop node。

`ros2 run turtlesim turtle_teleop_key`

这时候依然是在turtlesim package中运行另外一个可执行文件，turtle_teleop_key。这时用`ros2 node list`命令可以看到此时有两个活跃的nodes。
