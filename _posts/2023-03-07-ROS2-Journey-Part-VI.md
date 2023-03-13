---


layout: post
title: ROS2 Journey Part VI
subtitle: Understanding Actions
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


# ROS2 Journey VI



## 1. Background

Actions是一种通信类型，主要面向长时运行的任务。包含三个方面：goal, feedback, and a result。

Actions基于topics和services构建。功能上近似于services，actions可以被取消。同时还提供稳定的反馈，相对的，services返回一个单一的response。

Actions使用client- server 模型。Action client节点传递goal到action server节点认可goal并返回反馈流和结果。

![](/img/Action-SingleActionClient.gif)

## Tasks

