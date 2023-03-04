---


layout: post
title: ROS2 Journey Part III
subtitle: Understanding Topics
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


# ROS2 Journey III



## 1. Background

Topics 扮演的角色主要是 nodes 间交换信息用。

![](/img/Topic-SinglePublisherandSingleSubscriber.gif)

同样的，这个传递不是 1 to 1 的，可以是 1 to 1， 1 to many, many to 1 或 many to many。

![](/img/Topic-MultiplePublisherandMultipleSubscriber.gif)

Topics 是数据在节点或系统不同部分传递的主要方式。

### 2. Tasks

首先打开 `/turtlesim_node`和`/turtle_teleop_key`。

接下来使用rqt_graph来可视化。直接输入`rqt_graph`

![](/img/2023-03-04_11-12-27.png)

这张图上，`/turtlesim` node 和 `tele_turtle`





