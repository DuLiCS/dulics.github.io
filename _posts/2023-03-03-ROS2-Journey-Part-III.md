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

