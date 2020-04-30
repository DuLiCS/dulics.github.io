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

在这里假设上面的公式是我们要求的规律,参数值也是我们最终需要得到的结果.当然对于大多数问题来说,我们并不知道内在的规律,如果知道了就不需要机器学习了,而且很多实际问题符合的都不是精确规律,因此对于已有数据集来说,我们假设他符合线性规律,假设这里有三组数据,这里的$$hat{y}$$和精确值对应,为估计值

$$
\begin{aligned}
\hat{y}^{(1)} &= x_1^{(1)} w_1 + x_2^{(1)} w_2 + b,\\
\hat{y}^{(2)} &= x_1^{(2)} w_1 + x_2^{(2)} w_2 + b,\\
\hat{y}^{(3)} &= x_1^{(3)} w_1 + x_2^{(3)} w_2 + b.
\end{aligned}
$$

我们可以把这一组公式转化为矩阵的形式:

$$
\boldsymbol{\hat{y}} =
\begin{bmatrix}
    \hat{y}^{(1)} \\
    \hat{y}^{(2)} \\
    \hat{y}^{(3)}
\end{bmatrix},\quad
\boldsymbol{X} =
\begin{bmatrix}
    x_1^{(1)} & x_2^{(1)} \\
    x_1^{(2)} & x_2^{(2)} \\
    x_1^{(3)} & x_2^{(3)}
\end{bmatrix},\quad
\boldsymbol{w} =
\begin{bmatrix}
    w_1 \\
    w_2
\end{bmatrix}.
$$

在这里估计值和实际值的误差就是$y-\hat{y}$,这里定义的损失函数,损失函数就是估计值和实际值的差平方除2.公式如下:

$$\ell^{(i)}(w_1, w_2, b) = \frac{1}{2} \left(\hat{y}^{(i)} - y^{(i)}\right)^2,$$

一般来说,我们用训练数据的所有误差的平均值来衡量模型预测的质量,这个值被定义为:

$$\ell(w_1, w_2, b) =\frac{1}{n} \sum_{i=1}^n \ell^{(i)}(w_1, w_2, b) =\frac{1}{n} \sum_{i=1}^n \frac{1}{2}\left(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}\right)^2.$$

在模型训练中，我们希望找出一组模型参数，记为$w_1^*, w_2^*, b^*$，来使训练样本平均损失最小：

$$w_1^*, w_2^*, b^* = \operatorname*{argmin}_{w_1, w_2, b}\  \ell(w_1, w_2, b).$$

以上就是线性回归的基本思想.


#### 2.优化算法

优化是非常重要的部分,也就是在一定次数的迭代后对参数进行优化.先选取一组模型参数的初始值，如随机选取；接下来对参数进行多次迭代，使每次迭代都可能降低损失函数的值。在每次迭代中，先随机均匀采样一个由固定数目训练数据样本所组成的小批量（mini-batch)$\mathcal{B}$ ，然后求小批量中数据样本的平均损失有关模型参数的导数（梯度），最后用此结果与预先设定的一个正数的乘积作为模型参数在本次迭代的减小量。
$$
\begin{aligned}
w_1 &\leftarrow w_1 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_1} = w_1 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}x_1^{(i)} \left(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}\right),\\
w_2 &\leftarrow w_2 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial w_2} = w_2 -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}x_2^{(i)} \left(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}\right),\\
b &\leftarrow b -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \frac{ \partial \ell^{(i)}(w_1, w_2, b)  }{\partial b} = b -   \frac{\eta}{|\mathcal{B}|} \sum_{i \in \mathcal{B}}\left(x_1^{(i)} w_1 + x_2^{(i)} w_2 + b - y^{(i)}\right).
\end{aligned}
$$













































