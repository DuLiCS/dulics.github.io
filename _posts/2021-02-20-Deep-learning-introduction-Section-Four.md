---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第四章 神经网络的学习
tags: [Machine learning, Reading]

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


# 第四章 神经网络的学习

上一章我们讲前向传播，主要说的数据的传递和计算，没有涉及到参数的训练和选择，本章的主题是神经网络的学习。为了使神经网络可以学习，引入了损失函数这个指标，学习的目的是以该损失函数为基准，找出使它的值最小的权重参数。

### 4.1 从数据中学习

神经网络的特征就是可以从数据中学习。所谓从数据中学习，指的就是

