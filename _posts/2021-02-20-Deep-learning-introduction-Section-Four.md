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

神经网络的特征就是可以从数据中学习。所谓从数据中学习，指的就是可以由税局自动决定权重参数的值。

#### 4.1.1 数据驱动

所谓数据驱动，强调了数据对机器学习的重要性。几乎所有的事情都和数据有关。以下列出三种解决问题的方法。

* 人面对问题时，研究问题，想出算法，最终解决问题。
* 从图中提取特征，再学习模式，最终解决问题。
* 直接学习数据，得到答案。

深度学习有时也被称为端到端机器学习（end-to-end machine learning）。


#### 4.1.2 训练数据和测试数据

在机器学习中一般将数据集分为测试集和训练集。


### 4.2 损失函数 