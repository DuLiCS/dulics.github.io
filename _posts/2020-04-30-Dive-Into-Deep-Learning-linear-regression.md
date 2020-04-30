---

layout: post
title: Dive into Deep Learning Linear Regression
subtitle: 
tags: [Deep Learning, AI]

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

### 前言

世界读书日的时候买了很多书,其中只有一本专业书,就是这本Dive into deep learning.和之前买的Deep Learning那本书不同,这本书注重实践,通过写某一个算法的Scratch版本来学习算法.从零开始手撸算法有助于理解.从简单的开始,第一个就是线性回归.这部分的内容打算按照自己的理解来写,毕竟照搬没有意义而且这本书的相关资源非常丰富,书的PDF版本包括所有代码(jupyter版本)以及视频.

#### 1.从零开始的深度学习--线性回归

对于线性回归,相信所有的人都不陌生,这是非常常见且简单的算法,但是以我个人来说,听人讲和自己推导是完全不同的,会有很多细节都没有注意到.之后的内容将按照自己的理解写第一部分,对照着教程进行总结,以此希望能到达良好的效果,毕竟不管是Deep Learning或者Machine Learning都已经开始学了很多次了,大都半途而废,这次希望能坚持下去吧.

线性的意思以我理解来源于直线,也就是恒定斜率的概念.对于一条直线来说,它的数学公式大概是下面这样:

$$y = ax + b$$

这应该是线性关系最简单的形式,对于实际问题来说,影响结果的因素不只有一种,于是,这样的关系可以表示为(以两个变量为例):

$$y = w_1x_1+w_2x_2+b$$