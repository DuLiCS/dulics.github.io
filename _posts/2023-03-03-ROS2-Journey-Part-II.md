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

### 2. 
