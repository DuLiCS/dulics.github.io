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

神经网络用来寻找最优权重参数的指标被称作<font color=red>损失函数</font>。这个损失函数可以使用任意函数，但一半用均方误差和交叉熵误差等。

#### 4.2.1 均方误差

用作损失函数的函数很多，其中最有名的是**均方误差**。(mean squared error MSE)，表达形式如下：

$$
E = \frac{1}{2}\sum_{k}(y_k-t_k)^2
$$

这里的$y_k$代表神经网络的输出，$t_k$代表监督数据，$k$代表维数。下来我们进行实现：

```python
def mean_squared_error(y,t):
    return (np.sum((y-t)**2))*0.5
```
观察数学意义可以知道，MSE的大小直接反映了预测结果和真实值之间的差异。


#### 4.2.2 交叉熵误差

$$
E= -\sum_kt_klog\ y_k
$$

公式里的$log$代表以$e$为底的自然对数($log_e$)。$y_k$是神经网络的输出，$t_k$是监督值，也就是标准值。$t_k$使用one-hot表示。

```python
def cross_entropy_error(y,t):
    delta = 1e-7
    return - np.sum(t * np.log(y + delta))
```
因为np.log(0)会计算成负无穷，所以这里delta的作用是为了应对这个情况。

#### 4.2.3 mini-batch学习

上一章我们讲了批处理，对应实际中使用的批处理，损失函数的写法也要做相应改变。之前介绍的损失函数考虑的都是单个数据的损失函数，如果要求所有的训练数据的损失函数的总和，以交叉熵为例，可以写成如下的形式：

$$
E = -\frac{1}{N}\sum_n\sum_kt_{nk}log\ y_{nk}
$$

这里假设有$N$个数据，$t_nk$代表第$n$个数据的第$k$个元素的值。另外，MNIST有60000个数据，假设以全部数据为对象求损失函数的和，计算过程需要花费较长的时间。实际中的数据集比这个量更大，因此，我们从全部数据中选出一批数据，然后对每个mini-batch进行学习。这种方式叫做mini-batch学习。

之前我们已经有了读取数据集的代码，接下来我们要从中随机选取10个数据。随机选取可以使用np.random.choice()。

```python
import sys,os
import numpy as np

sys.path.append(os.pardir)
from dataset.mnist import load_mnist

(x_train, t_train),(x_test, t_test) = load_mnist(flatten=True, normalize=True)


train_size = x_train.shape[0]
batch_size = 10
batch_mask = np.random.choice(train_size,batch_size)

x_batch = x_train[batch_mask]
t_batch = t_train[batch_mask]
```

#### 4.2.4 mini-batch 版交叉熵误差的实现

```python
def cross_entropy_error(y,t):
    if y.ndim == 1:
        t = t.reshape(1, t.size)
        y = y.reshape(1, y.size)
    
    delta = 1e-7
    batch_size = y.shape[0]
    return -np.sum(t * np.log(y + delta))/batch_size
```

对于非one-hot标签，可以用下面的形式：

```python

```