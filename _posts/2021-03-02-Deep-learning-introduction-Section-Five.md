---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第五章 误差反向传播法
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



# 第四章 误差反向传播法

上一章介绍了神经网络的学习，并通过数值微分计算了神经网络的权重参数的梯度，数值微分虽然简单，但是计算起来比较浪费时间。本章使用一个能够高效计算权重参数的梯度的方法--误差反向传播法。
正确理解反向传播法，有两种方法，一种基于**计算图**，一种基于数学式。后者是比较常见的方法，本章通过计算图来理解。

### 5.1 计算图

#### 5.1.1 用计算图求解

我们尝试用计算图解决问题，目的是为了让大家熟悉计算图。

**问题1**：太郎在超市买了 2 个 100 日元一个的苹果，消费税是 10%，请计算支付金额。

计算图通过节点和箭头表示计算过程，节点用圆圈表示，圆圈内是计算的内容。将计算的中间结果写在箭头上方，表示各个节点的计算结果从左向右传递，对于上述问题，求解过程如图所示。
![计算图1](/img/comput_graph_1.png)

开始时，苹果的100日元流到 $\times2$ 节点，变成200日元，然后传递到下一个节点，流向 $\times1.1$ 节点，变成220日元。因此答案是220日元。
上图中把 $\times2$ $\times1.1$ 整体扩起来，不过只用圆圈表示 $\times$ 也是可以的，此时，可以将 2 和 1.1 分别作为变量“苹果的个数”和“消费税”表在外面。
![计算图2](/img/comput_graph_2.png)
