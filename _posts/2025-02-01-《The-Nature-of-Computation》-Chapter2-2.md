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

其中C^{'} = logC。对于木星和其伽利略卫星，也满足这样的关系。对于不同的系统，常数$C$取决于