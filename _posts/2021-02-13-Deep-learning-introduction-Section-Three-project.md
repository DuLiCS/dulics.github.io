---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第三章 手写数字识别
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

### 3.6 手写数字识别


上一个post介绍了神经网络的基本内容，这一节搭配项目解决实际问题。这个例子非常简单，是一个机器学习里的Hello world。手写数字识别问题。但是这个例子是不完全的，我们<font color=red>假设学习已经全部完成</font>，我们用学习到的参数，先实现神经网络的“推理处理”。这也叫神经网络的前向传播。

#### 3.6.1 MNIST数据集

这个数据集网上的资料实在太多了，就连他的进阶版本Fashion MNIST也出来很久了，相信能看到现在的人没有太多人不知道这个数据集。

介绍简单带过。MNIST数据集(Mixed National Institute of Standards and Technology database)是美国国家标准与技术研究院收集整理的大型手写数字数据库,包含60,000个示例的训练集以及10,000个示例的测试集。

MNIST的图像是28$\times$28像素的灰度图像（1通道），像素的取值在0到255之间。每个图像都标有对应的阿拉伯数字标签。

这本书提供了数据集和相应的代码。[传送门](https://www.ituring.com.cn/book/1921)

```python
import sys,os

sys.path.append(os.pardir)
from dataset.mnist import load_mnist

(x_train, t_train),(x_test, t_test) = load_mnist(flatten=True, normalize=False)

print(x_train.shape)
print(t_train.shape)
print(x_test.shape)
print(t_test.shape)
``` 