---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第七章  卷积神经网络
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



# 第七章 卷积神经网络

终于来到了激动人心的卷积神经网络。卷积神经网络的重要性不用多说，所以这一章着重来介绍。

### 7.1 整体结构

首先看一下CNN的网络结构。神经网络和之前的神经网络一样，可以像乐高积木一样通过组装层来构建，不过出现了新的卷积层（Convolution层）和池化层（Pooling层）。

之前介绍的神经网络中，相邻的所有神经元只见痘有链接，称为**全连接**。结构类似下图所示。

![CNN1.png](/img/CNN1.png)

对比一下，CNN的结构是什么样的。下图就是一个CNN的例子。

![CNN2.png](/img/CNN2.png)

经过对比可以看出，多了卷积层和池化层。其连接顺序是“Convolution - ReLU - （Pooling）”（Pooling层有时也会被省略）。

### 7.2 卷积层

CNN 中出现了一些特有的术语，比如填充、步幅等。此外，各层中传 递的数据是有形状的数据(比如，3 维数据)，这与之前的全连接网络不同， 因此刚开始学习 CNN 时可能会感到难以理解。本节我们将花点时间，认真 学习一下 CNN 中使用的卷积层的结构。


#### 7.2.1 全连接层存在的问题


之前介绍的全连接的神经网络中使用了全连接层(Affine 层)。在全连接 层中，相邻层的神经元全部连接在一起，输出的数量可以任意决定。

全连接层存在什么问题呢?那就是数据的形状被“忽视”了。比如，输入数据是图像时，图像通常是高、长、通道方向上的3维形状。但是，向全连接层输入时，需要将3维数据拉平为1维数据。实际上，前面提到的使用了MNIST数据集的例子中，输入图像就是1通道、高28像素、长28像素的(1, 28, 28)形状，但却被排成1列，以784个数据的形式输入到最开始的 Affine 层。

其实就是说，作为图像来说，三维图像本身就蕴含一些信息，全连接层把图像拉长，使得一些信息被去掉。

卷积层可以保持形状不变，输入是图像时，卷积层会以三维数据的形式接收输入数据，并用三维形式输出到下一层。因此，**在 CNN 中，可以(有可能)正确理解图像等具有形状的数据。**

CNN 中，有时将卷积层的输入输出数据称为**特征图**(feature map)。其中，卷积层的输入数据称为**输入特征图**(input feature map)，输出 数据称为输出特征图(output feature map)。本书中将“输入输出数据”和“特
征图”作为含义相同的词使用。

#### 7.2.2 卷积运算

卷积层进行的处理就是卷积运算。卷积运算相当于图像处理中的“滤波 器运算”。

![CNN3.png](/img/CNN3.png)

上面吧滤波器在输入数据上移动，对应位置相乘最后整个相加。

![CNN4.png](/img/CNN4.png)

下面是加上某个固定值的图。

![CNN5.png](/img/CNN5.png)


#### 7.2.3 填充


在进行卷积层的处理之前，有时要向输入数据的周围填入固定的数据(比 如 0 等)，这称为填充(padding)，是卷积运算中经常会用到的处理。下面是填充0的情况。

![CNN6.png](/img/CNN6.png)


使用填充主要是为了调整输出的大小。比如，对大小为 (4, 4) 的输入 数据应用 (3, 3) 的滤波器时，输出大小变为 (2, 2)，相当于输出大小 比输入大小缩小了 2 个元素。这在反复进行多次卷积运算的深度网 络中会成为问题。为什么呢?因为如果每次进行卷积运算都会缩小 空间，那么在某个时刻输出大小就有可能变为 1，导致无法再应用 卷积运算。为了避免出现这样的情况，就要使用填充。在刚才的例 子中，将填充的幅度设为 1，那么相对于输入大小 (4, 4)，输出大小 也保持为原来的 (4, 4)。因此，卷积运算就可以在保持空间大小不变 的情况下将数据传给下一层。

#### 7.2.4 步幅

应用滤波器的位置间隔称为步幅(stride)。之前的例子中步幅都是1，如
果将步幅设为2，则如图所示，应用滤波器的窗口的间隔变为2个元素。

![CNN7.png](/img/CNN7.png)

直接总结式子。这里，假设输入大小为 $(H, W)$，滤波器大小为 $(FH, FW)$，输出大小为 $(OH, OW)$，填充为 $P$，步幅为 $S$。此时，输出大小可用如下式子进行计算。

$$
OH = \frac{H + 2 P - FH}{S} + 1
$$


$$
OW = \frac{W + 2 P - FW}{S} + 1
$$

#### 7.2.5 3维数据的卷积运算

之前的卷积运算的例子都是以有高、长方向的2维形状为对象的。但是，图像是3维数据，除了高、长方向之外，还需要处理通道方向。这里，我们按照与之前相同的顺序，看一下对加上了通道方向的3维数据进行卷积运算的例子。

![CNN8.png](/img/CNN8.png)


![CNN9.png](/img/CNN9.png)

通道数只能设定为和输入数据的通道数相同的值。


#### 7.2.6 结合方块思考

将数据和滤波器结合长方体的方块来考虑，3 维数据的卷积运算会很 容易理解。

![CNN10.png](/img/CNN10.png)

在这个例子中，数据输出是 1 张特征图。所谓 1 张特征图，换句话说， 就是通道数为 1 的特征图。那么，如果要在通道方向上也拥有多个卷积运算的输出，该怎么做呢?为此，就需要用到多个滤波器(权重)。用图表示的话，如图所示。

![CNN11.png](/img/CNN11.png)

卷积运算中(和全连接层一样)存在偏置。在图 7-11 的例子中，如果进一步追加偏置的加法运算处理，则结果如下面的图所示。

![CNN12.png](/img/CNN12.png)

#### 7.2.7 批处理

![CNN13.png](/img/CNN13.png)

图示的批处理版的数据流中，在各个数据的开头添加了批用的维度。 像这样，数据作为 4 维的形状在各层间传递。这里需要注意的是，网络间传 递的是 4 维数据，对这 N 个数据进行了卷积运算。也就是说，批处理将 N 次 的处理汇总成了 1 次进行。

### 7.3 池化层

池化是缩小高、长方向上的空间的运算。比如，如图所示，进行将
2 × 2 的区域集约成 1 个元素的处理，缩小空间大小。

![CNN14.png](/img/CNN14.png)


上图的例子是按步幅 2 进行 2 × 2 的 Max 池化时的处理顺序。“Max 池化”是获取最大值的运算。

除了 Max 池化之外，还有 Average 池化等。相对于 Max 池化是从 目标区域中取出最大值，Average 池化则是计算目标区域的平均值。 在图像识别领域，主要使用 Max 池化。


### 池化层的特征

**没有要学习的参数**

池化层和卷积层不同，没有要学习的参数。池化只是从目标区域中取最 大值(或者平均值)，所以不存在要学习的参数。

**通道数不发生变化**

经过池化运算，输入数据和输出数据的通道数不会发生变化。如图 7-15 所示，计算是按通道独立进行的。


![CNN15.png](/img/CNN15.png)

**对微小的位置变化具有鲁棒性(健壮)**

输入数据发生微小偏差时，池化仍会返回相同的结果。因此，池化对 输入数据的微小偏差具有鲁棒性。

![CNN16.png](/img/CNN16.png)

### 7.4 卷积层和池化层的实现

#### 7.4.1 4维数组

```python
import numpy as np
x = np.random.rand(10,1,28,28)
x.shape
```

如果要访问第1个数据，只要写x[0]就可以。

如果要访问第 1 个数据的第 1 个通道的空间数据，可以写成下面这样。

```python
>>> x[0,0]
```

像这样，CNN 中处理的是 4 维数据，因此卷积运算的实现看上去会很复 杂，但是通过使用下面要介绍的 im2col 这个技巧，问题就会变得很简单。


#### 7.4.2 基于im2col的展开


如果老老实实地实现卷积运算，估计要重复好几层的for语句。这样的实现有点麻烦，而且，NumPy中存在使用for语句后处理变慢的缺点(NumPy中，访问元素时最好不要用for 语句)。这里，我们不使用for语句，而是使用im2col这个便利的函数进行简单的实现。


im2col 是一个函数，将输入数据展开以适合滤波器(权重)。如图所示，对 3 维的输入数据应用 im2col 后，数据转换为 2 维矩阵(正确地讲，是把包含 批数量的 4 维数据转换成了 2 维数据)。

![CNN17.png](/img/CNN17.png)

im2col会把输入数据展开以适合滤波器(权重)。具体地说，如图所示，对于输入数据，将应用滤波器的区域(3维方块)横向展开为1列。im2col会在所有应用滤波器的地方进行这个展开处理。

![CNN18.png](/img/CNN18.png)

使用 im2col 展开输入数据后，之后就只需将卷积层的滤波器(权重)纵 向展开为1列，并计算2个矩阵的乘积即可(参照图)。这和全连接层的 Affine 层进行的处理基本相同。

![CNN19.png](/img/CNN19.png)


#### 7.4.3 卷积层的实现

本书提供了 im2col 函数，并将这个 im2col 函数作为黑盒(不关心内部实现) 使用。

im2col 这一便捷函数具有以下接口。
im2col (input_data, filter_h, filter_w, stride=1, pad=0)
* input_data― 由(数据量，通道，高，长)的4维数组构成的输入数据
* filter_h― 滤波器的高
* filter_w― 滤波器的长
* stride― 步幅
* pad― 填充

```python
import sys, os
import numpy as np
sys.path.append(os.pardir)
from common.util import im2col

x1 = np.random.rand(1, 3, 7, 7)
col1 = im2col(x1, 5, 5, stride=1, pad=0)
print(col1.shape) #(9, 75)

x2 = np.random.rand(10, 3, 7, 7)
col2 = im2col(x2, 5, 5, stride=1, pad=0)
print(col2.shape) #(90, 75)
```

第一个是批大小为 1、通道为 3 的 7 × 7 的数据，第二个的批大小为 10，数据形状和第一个相同。分别对其应用 im2col 函数，在这两种情形下，第 2 维的元素个数均为 75。这是滤波器(通道为 3、大小为 5 × 5)的元素个数的总和。批大小为 1 时，im2col的结果是 (9, 75),9 是因为在 7 × 7 的矩阵中移动 5 × 5 的矩阵有9种不同的情况。而第 2 个例子中批大小为 10，所以保存了 10 倍的数据，即 (90, 75)。

接下来实现卷积层。

```python
class Convolution:
    def __init__(self, W, b, stride = 1, pad = 0):
        self.W = W
        self.b = b
        self.stride = stride
        self.pad = pad

    def forward(self, x):
        FN, C, FH, FW = self.W.shape
        N, C, H, W = x.shape
        out_h = int(1 + (H + 2*self.pad - FH) / self.stride)
        out_w = int(1 + (W + 2*self.pad - FW) / self.stride)

        col = im2col(x, FH, FW, self.stride, self.pad)
        col_W = self.W.reshape(FN, -1).T
        out = np.dot(col, col_W) + self.b

        out = out.reshape(N, out_h, out_w, -1).transpose(0, 3, 1, 2)

        return
```
#### 7.4.4 池化层的实现

池化层的实现和卷积层相同，都使用 im2col 展开输入数据。不过，池化 的情况下，在通道方向上是独立的，这一点和卷积层不同。

![CNN20.png](/img/CNN20.png)


像这样展开之后，只需对展开的矩阵求各行的最大值，并转换为合适的 形状即可。

![CNN21.png](/img/CNN21.png)

```python
class Pooling:
    def __init__(self, pool_h, pool_w, stride = 1, pad = 0):
        self.pool_h = pool_h
        self.pool_w = pool_w
        self.stride = stride
        self.pad = pad

    def forward(self, x):
        N, C, H, W = x.shape
        out_h = int(1 + (H - self.pool_h) / self.stride)
        out_w = int(1 + (W - self.pool_w) / self.stride)

        col = im2col(x, self.pool_h, self.pool_w, self.stride, self.pad)
        col = col.reshape(-1, self.pool_h*self.pool_w)

        out = np.max(col, axis=1)
        out = out.reshape(N, out_h, out_w, C).transpose(0, 3, 1, 2)

        return out
```

池化层的实现按下面 3 个阶段进行。

1. 展开输入数据。
2. 求各行的最大值。
3. 转换为合适的输出大小。


### 7.5 CNN的实现


我们已经实现了卷积层和池化层，现在来组合这些层，搭建进行手写数
字识别的 CNN。

![CNN22.png](/img/CNN22.png)

上图网络的构成是“Convolution - ReLU - Pooling -Affine - ReLU - Affine - Softmax”，我们将它实现为名为SimpleConvNet的类。


参数
* input_dim― 输入数据的维度:(通道，高，长)
* conv_param― 卷积层的超参数(字典)。字典的关键字如下:
    filter_num― 滤波器的数量 filter_size― 滤波器的大小 stride― 步幅
    pad― 填充
* hidden_size― 隐藏层(全连接)的神经元数量
* output_size― 输出层(全连接)的神经元数量
* weitght_int_std― 初始化时权重的标准差

卷积层的超参数通过名为 conv_param 的字典传入。我们设想它会像 {'filter_num':30,'filter_size':5, 'pad':0, 'stride':1} 这样，保存必要 的超参数值。

SimpleConvNet 的初始化的实现稍长，我们分成 3 部分来说明，首先是初 始化的最开始部分。


```python
class SimpleConvNet:
    def __init__(self, input_dim=(1, 28, 28), conv_param={'filter_num':30, 'filter_size':5, 'pad':0, 'stride':1}, hidden_size = 100, output_size = 10, weight_init_std = 0.01):
        filter_num = conv_param['filter_num']
        filter_size = conv_param['filter_size']
        filter_pad = conv_param['pad']
        filter_stried
```
