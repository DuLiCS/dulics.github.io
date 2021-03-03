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

**问题2**：太郎在超市买了两个苹果三个橘子，其中，苹果每个100日元。橘子每个150日元，消费税是10%，请计算支付金额。按照上面的思路，求解过程如图所示。
![计算图3](/img/comput_graph_3.png)

综上，用计算图解题的流程如下：

1.*构建计算图*

2.*在计算图上从左向右计算*

第二部的“从左向右计算”是一种正方向上的传播过程，称为**正向传播**，与之相对应的是**反向传播**。

#### 5.1.2 局部计算

计算图可以通过传递“局部计算”获得最终结果。下面是一个局部计算的例子。

![计算图4](/img/comput_graph_4.png)

这个图也很好理解，结论是，无论全局计算有多么复杂，只要按照步骤，都可以最终计算出结果。


#### 5.1.3 为何用计算图解题

优点一：局部计算，无论全局多么复杂的计算，都可以通过局部计算使各个节点致力于简单的计算。
优点二：通过反向传播计算导数。

我们重新思考问题1，假设我们想知道苹果价格的波动会在多大程度上影响最终支付的金额，也就是求“支付金额关于苹果价格的导数”，设苹果价格为 $x$,支付金额为 $L$，则相当于求 $\frac{\partial L}{\partial x}$。

现在使用计算图的反向传播求这个导数。

![计算图5](/img/comput_graph_5.png)

如上图所示，反向传播使用与正方向相反的箭头（粗线）表示。反向传播传递“局部导数”，将导数值写在下方，这里坠重支付金额关于苹果价格的导数是2.2，也就是说苹果价格每上升1元，最终支付金额会上涨2.2元。

### 5.2 链式法则

#### 5.2.1 计算图的反向传播

假设存在 $y = f(x)$ 的计算，这个计算的反向传播如图所示。

![计算图6](/img/comput_graph_6.png)

反向传播的计算顺序是，将信号E乘以节点的局部导数 $\frac{\partial y}{\partial x}$，然后将结果传递给下一个节点，这里所说的局部导数是指正向传播中 $y = f(x)$ 的导数，也就是y关于x的导数 $\frac{\partial y}{\partial x}$。比如，假设 $y = f(x) = x^2$ ，
则局部导数为 $\frac{\partial y}{\partial x}= 2x$ 。把这个局部导数乘以上游传过来的值( 本 例 中 为 E )， 然后传递给前面的节点。


#### 5.2.2  链式法则

介绍链式法则之前，先要从**复合函数**说起，复合函数是多个函数构成的函数，比如 $z = (x+y)^2$ 是由以下两个式子构成的。
$$
z = t^2 \\
t = x + y
$$
链式法则是关于复合函数的导数的性质，定义如下。

*如果某个函数由复合函数表示，则该复合函数的导数可以用构成复合函数的各个函数的导数的乘积表示*

这就是链式法则的原理，用上面的表达式为例，$\frac{\partial z}{\partial x}$(z关于x的导数)可以用 $\frac{\partial z}{\partial t}$ (z关于t的导数)和 $\frac{\partial t}{\partial x}$(t关于x的导数)的乘积表示。数学表示如下：

$$
\frac{\partial z}{\partial x} = \frac{\partial z}{\partial t}\frac{\partial t}{\partial x}
$$

式子中的 ${\partial t}$ 可以相互抵消。

现在我们用链式法则，试着求上面表达式的导数，接下来要求式子中的局部导数（偏导数）

$$
\frac{\partial z}{\partial t} = 2t \\
\frac{\partial t}{\partial x} = 1
$$

于是可以得到：

$$
\frac{\partial z}{\partial x} = \frac{\partial z}{\partial t}\frac{\partial t}{\partial x}=2t \times 1 = 2(x+y)
$$

#### 5.2.3 链式法则和计算图

现在我们尝试将链式法则的计算用计算图表达出来。

![计算图7](/img/comput_graph_7.png)

如图所示，计算图的反向传播从右往左传播信号，反向传播的计算顺序是，先将节点的输入信号乘以节点的偏导数，然后再传递给下一个节点。比如，反向传播时，“**2”节点的输入是 $\frac{\partial z}{\partial z}$，将其乘以局部导数 $\frac{\partial z}{\partial z}$ 因为正向传播的输入是t，输出是z，所以这个节点的局部导数是 $\frac{\partial z}{\partial t}$ 然后传递给下一个节点。
在图中最左边是反向传播的结果，他的计算基于上面的规则，因此有 $\frac{\partial z}{\partial z}\frac{\partial z}{\partial t}\frac{\partial t}{\partial x} = \frac{\partial z}{\partial t}\frac{\partial t}{\partial x}=\frac{\partial z}{\partial x}$,对应z关于x的导数。代入得到 $\frac{\partial z}{\partial x}$ 的结果是 $2(x+y)$

![计算图8](/img/comput_graph_8.png)

### 5.3 反向传播

#### 5.3.1 加法节点的反向传播

首先看一下加法节点的反向传播这，这里以 $z = x + y$ 为对象，观察反向传播。$z = x + y$ 的导数可以由下式计算出来。

$$
\frac{\partial z}{\partial x} = 1 \\
\frac{\partial z}{\partial y} = 1
$$

上面两个式子都等于1，用计算图表示如下所示。

![计算图9](/img/comput_graph_9.png)

![计算图10](/img/comput_graph_10.png)
下面来看一个具体的例子。假设有“$10 + 5 =15$”这样的计算，加法节点的反向传播只是将信号输出到下一个节点。


#### 5.3.2 乘法节点的反向传播

用同样的方式看乘法节点。假设 $z =xy$ 于是有

$$
\frac{\partial z}{\partial x} = y \\
\frac{\partial z}{\partial y} = x
$$

下面看一个具体的例子， $10 \times 5 =50$。

![计算图9](/img/comput_graph_11.png)

![计算图10](/img/comput_graph_12.png)

#### 5.3.3 苹果的例子


接着来思考一下苹果的例子。这里要解的问题是苹果的价格、苹果的个数、消费税这3个变量各自如何影响最终支付的金额。这个问题相当于求“支付金额关于苹果的价格的导数”“支付金额关于苹果的个数的导数”“支付金额关于消费税的导数”。
![计算图10](/img/comput_graph_13.png)

### 5.4 简单层的实现

本节用python实现前面的苹果例子，乘法节点称为“乘法层”，加法节点称为“加法层”。

#### 5.4.1 乘法层的实现

```python
class MulLayer:
    def __init__(self):
        self.x = None
        self.y = None

    def forward(self, x, y):
        self.x = x
        self.y = y
        out = x * y

        return out

    def backward(self, dout):
        dx = dout * self.y
        dy = dout * self.x

        return dx,dy
```
init() 中会初始化实例变量 x 和 y，它们用于保存正向传播时的输入值。 forward() 接收 x 和 y 两个参数，将它们相乘后输出。backward() 将从上游传 来的导数(dout)乘以正向传播的翻转值，然后传给下游。

![计算图10](/img/comput_graph_13.png)

参照上图可以作如下实现。


```python

apple = 100
apple_num = 2
tax = 1.1

#layer

mul_apple_layer = MulLayer()
mul_tax_layer = MulLayer()

#forward

apple_price = mul_apple_layer.forward(apple,apple_num)
price = mul_tax_layer.forward(apple_price,tax)

print(price)



#backward

dprice = 1
dapple_price,dtax = mul_tax_layer.backward(dprice)
dapple,dapple_num = mul_apple_layer.backward(dapple_price)

print(dapple,dapple_num,dtax)
```

#### 5.4.2 加法层的实现

```python

class AddLayer:
    def __init__(self):
        pass

    def forward(self,x,y):

        out = x + y

        return out

    def backward(self,dout):

        dx = dout * 1
        dy = dout * 1

        return dx,dy
```


![计算图14](/img/comput_graph_14.png)


使用之前的方法进行实现。

```python
apple = 100
orange = 150
apple_num = 2
orange_num = 3
tax = 1.1

#layer

mul_apple_layer = MulLayer()
mul_orange_layer = MulLayer()
add_apple_orange_layer = AddLayer()
mul_tax_layer = MulLayer()

#forward

apple_price = mul_apple_layer.forward(apple,apple_num)
orange_price = mul_orange_layer.forward(orange,orange_num)
all_price = add_apple_orange_layer.forward(apple_price,orange_price)
price = mul_tax_layer.forward(all_price,tax)

#backward
dprice = 1
dall_price,dtax   = mul_tax_layer.backward(dprice)
dapple_price, dorange_price = add_apple_orange_layer.backward(dall_price)
dorange,dorange_num = mul_orange_layer.backward(dorange_price)
dapple,dapple_num = mul_apple_layer.backward(dapple_price)

print(price)
print(dapple_num,dapple,dorange,dorange_num,dtax)
```

### 5.5 激活函数层的实现

现在把计算图的思想应用到神经网络。

#### 5.5.1 ReLU层

激活函数ReLU数学表达式如下:

$$
f(x) =
\begin{cases}
x & x \gt 0 \\
0 & x \leqslant 0
\end{cases}
$$

可以求出y关于x的导数:

$$
\frac{\partial y}{\partial x} =
\begin{cases}
1 & x \gt 0 \\
0 & x \leqslant 0
\end{cases}
$$

 根据上面的表达式可以得知，正向传播时如果输入x大于零，则反向传播会将上游的值原封不动的传递，当正向传播的值x小于0时，会将信号停留在此处。

 下面是是实现：

 ```python
 class Relu:
    def __init__(self):
        self.mask = None

    def forward(self, x):
        self.mask = (x <= 0)
        out = x.copy()
        out[self.mask] = 0 #True的位置变成0 也就是小于0的变成0

        return out

    def backward(self,dout):
        dout[self.mask] = 0
        dx = dout

        return dx

x = np.array([[1,-0.5],[-2.0,3.0]])
print(x)
mask = (x<=0)
out = x.copy()
out[mask] = 0
print(out)
 ```


 #### 5.5.2 Sigmoid 层

 接下来是sigmoid函数的实现

 $$
 y = \frac{1}{1+exp(-x)}
 $$
