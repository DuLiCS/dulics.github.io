---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第六章  与学习相关的技巧
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



# 第六章 与学习相关的技巧

本章像是一个补充，主题涉及寻找最优权重参数的最优化方法、权重参数的初始值、超参数的设定方法等。此外，为了应对过拟合，本章还将介绍权值衰减、Dropout等正则化方法，并进行实现。最后将对近年来众多研究中使用的Batch Normalization方法进行简单的介绍。

这部分推荐大家可以去看一下李宏毅老师的课，专门讲optimizer的。[传送门](https://www.bilibili.com/video/BV1Bz411B7kR?t=966)

### 6.1 参数的更新

神经网络的学习的目的是找到使损失函数的值尽可能小的参数。这是寻
找最优参数的问题，解决这个问题的过程称为**最优化**（optimization）。

在前几章中，为了找到最优参数，我们将参数的梯度（导数）作为了线索。使用参数的梯度，沿梯度方向更新参数，并重复这个步骤多次，从而逐渐靠近最优参数，这个过程称为随机梯度下降法（stochastic gradient descent），简称SGD。SGD是一个简单的方法，不过比起胡乱地搜索参数空间，也算是“聪明”的方法。这一节将给出SGD的缺点，并介绍其他优化方法。


#### 6.1.1 探险家的故事

先打一个比方，寻找最优参数时，我们所处的状况和这位探险家一样，是一个漆黑的世界。我们必须在没有地图、不能睁眼的情况下，在广袤、复杂的地形中寻找“至深之地”。大家可以想象这是一个多么难的问题。在这么困难的状况下，地面的坡度显得尤为重要。探险家虽然看不到周围的情况，但是能够知道当前所在位置的坡度（通过脚底感受地面的倾斜状况）。于是，朝着当前所在位置的坡度最大的方向前进，就是SGD的策略。勇敢的探险家心里可能想着只要重复这一策略，总有一天可以到达“至深之地”。

#### 6.1.2 SGD

先复习SGD：

$$
W \to W - \eta \frac{\partial L}{\partial W}
$$

我们写一个名为SGD的Python类。

```python
class SGD:
  def __init__(self, lr = 0.01):
    self.lr = lr

    def update(self, params, grads):
      for key in params.keys():
        params[key] -= self.lr * grads[key]

```


在调用的时候，是这个样子的:

```python
network = TwoLayerNet()
optimizer = SGD

for i in range(10000):
  ···
  x_batch, t_batch = get_mini_batch(...)
  grads = network.gradient(x_batch, t_batch)
  params = network.params
  optimizer.update(params, grads)
```

#### 6.1.3 SGD的缺点

虽然SGD简单，并且容易实现，但是在解决某些问题时可能没有效率。这里，在指出SGD的缺点之际，我们来思考一下求下面这个函数的最小值的问题。

$$
f(x, y) = \frac{1}{20}x^2 + y^2
$$

用几张图来说明一下局限性。

第一张图，是上面函数的图像和等高线图

![SGD3](/img/SGD3.PNG)


接下来是梯度方向图

![SGD2](/img/SGD2.PNG)


最后是从 $(x, y) = (-7.0,2.0)$ 开始的路径。

![SGD1](/img/SGD1.PNG)


这个路径很显然非常低效。下面将介绍取代的方法。Momentum、AdaGrad、Adam.

#### 6.1.4 Momentum

Momentum 的数学表达如下：

$$
\upsilon \leftarrow \alpha \upsilon - \eta\frac{\partial L}{\partial W} \\
W \leftarrow W + \upsilon
$$

![SGD4](/img/SGD4.PNG)

![SGD5](/img/SGD5.PNG)



更新路径就像小球在碗中滚动一样。和SGD相比，我们发现“之”字形的“程度”减轻了。这是因为虽然x轴方向上受到的力非常小，但是一直在同一方向上受力，所以朝同一个方向会有一定的加速。反过来，虽然y轴方向上受到的力很大，但是因为交互地受到正方向和反方向的力，它们会互相抵消，所以y轴方向上的速度不稳定。因此，和SGD时的情形相比，可以更快地朝x轴方向靠近，减弱“之”字形的变动程度这个应该是这么理解的，动量的存在会让当前的计算受到上一步累加的动量的影响。


最终就是这个样子：

![SGD6](/img/SGD6.PNG)


#### 6.1.5 AdaGrad

在神经网络的学习中，学习率（数学式中记为 $\eta$ ）的值很重要。学习率过小，会导致学习花费过多时间；反过来，学习率过大，则会导致学习发散而不能正确进行。

在关于学习率的有效技巧中，有一种被称为**学习率衰减**（learning rate decay）的方法，即随着学习的进行，使学习率逐渐减小。实际上，一开始“多”学，然后逐渐“少”学的方法，在神经网络的学习中经常被使用。

AdaGrad会为参数的每个元素适当地调整学习率，与此同时进行学习（AdaGrad的Ada来自英文单词Adaptive，即“适当的”的意思）。下面，让我们用数学式表示AdaGrad的更新方法。

$$
h \leftarrow h + \frac{\partial L}{\partial W} \bigodot \frac{\partial L}{\partial W} \\

W \leftarrow W - \eta\frac{1}{\sqrt{h}}\frac{\partial L}{\partial W}
$$

![SGD6](/img/SGD7.PNG)

就是当某一步走得太远的的时候，会调整步子小一点，大概就这个意思。

### 6.1.6 Adam
