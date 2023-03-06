---


layout: post
title: ROS2 Journey Part IV
subtitle: Understanding Services
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


# ROS2 Journey IV



## 1. Background

Services是另一种节点间通信的方式。Service基于call-and-response 模型，对应着的是topic的publisher- subscriber模型。对比来说，topic可以让节点持续的更新数据，services知识在被client请求时才传数据。

![](/img/Service-SingleServiceClient.gif)

![](/img/Service-MultipleServiceClient.gif)
