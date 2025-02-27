---


layout: post
title: 《The Nature of Computation》
subtitle: 2.2 Time Space and Scaling
tags: [读书]

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


# Time Space and Scaling


时间（运行时间）和空间（内存）是两个主要的算法指标。除此之外还有其他包括函数调用次数，两个人通信所需要的比特数，抛硬币的次数等等。我们可以计算当问题规模变化的时候，计算资源是如何变化的。

2.2.1

首先书中举了开普勒第三定律的例子。开普勒发现九大行星的周期$T$以及太阳的距离$R$存在如下关系。

T = C · R^{2/3}

分别求对数得到如下线性关系

logT = C^{'} + \frac{3}{2}logR

其中C^{'} = logC。对于木星和其伽利略卫星，也满足这样的关系。对于不同的系统，常数$C$取决于中心天体的质量，所以对于系统和系统是不同的，但是对数坐标的斜率或者说指数$2/3$是一样的，这样的规律反映了天体运行的基本性质，常数并不重要，重要的是反映了$T$和$R$的关系。

同样的，每个人的电脑是不一样的，我们不关心在不同电脑上的运行情况，我们关心随着$n$的变化，运行时间有什么变化规律。这里的$n$是什么，对于欧拉路径问题，他是顶点或是边的数量，一般来说，他是我要告诉你的信息量。


开普勒定律的启示：行星轨道周期 T 与轨道半径 R 之间的比例关系中，指数 3/2 才是关键，具体的常数因子不重要。

计算复杂度的核心思想：计算问题中，我们不关注计算机性能的具体差异，而是关注随着问题规模 n 增长，运行时间如何变化。这种增长趋势决定了算法的可扩展性（Scalability），即当输入规模变大时，算法的运行时间是否仍然可控。

问题规模 n 的不同定义：对于不同类型的问题，n 可能代表顶点数、边数、位数、数据大小等，通常对应描述问题所需的信息量。

本小节通过物理学中的比例关系来类比计算复杂度的增长关系，强调了指数增长规律的重要性，而不是具体的常数因子。

2.2.2

