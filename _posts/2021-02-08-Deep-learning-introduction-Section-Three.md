---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第三章 神经网络
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


# 第三章 神经网络


紧接着第二章的内容，我们开始介绍神经网络。上一章感知机的参数都是我们认为验证给定的，对于神经网络来说，一个重要的性质就是<font color=red>可以自动地从数据中学习到合适的权重参数。</font>

### 3.1 从感知机到神经网络

#### 3.1.1 神经网络的例子

下图就是一个神经网络的例子，把最左边的一列称为<font color=red>输入层</font>，中间一列称为<font color=red>中间层</font>，中间层有时又称为<font color=red>隐藏层</font>，最右边一列称为<font color=red>输出层</font>。

![神经网络结构](/img/NLP.png)

按之前的习惯，我们把输入层称为第0层，隐藏层称为第1层，输出层称为第二层，因此在这里我们称之为两层神经网络。注意，也有些地方将其称为三层神经网络。但怎么叫都无伤大雅。上图