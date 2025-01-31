---


layout: post
title: 《The Nature of Computation》
subtitle: 2.1 Problems and Solutions
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


# Problems and Solutions


这一节是第二章的前言，主要探讨 欧拉路径和汉密尔顿路径的本质区别，并引出计算复杂度的概念。尽管两者都涉及图的遍历，但寻找汉密尔顿路径似乎比欧拉路径难得多。作者提出疑问：为什么一个问题容易，而另一个问题却像在大海捞针？

为了理解这些问题的本质，我们不能只关注具体案例（如柯尼斯堡七桥问题或十二面体图）。我们需要思考 在任意规模的城市或图中，这些问题的难度如何增长，并用 计算复杂度 来衡量它们的难度。

计算复杂度关注 解决问题所需的计算资源（如时间和内存），以及这些资源如何随着问题规模的增长而变化。但复杂度理论的目标不是为了编写更高效的程序，正如物理学的目标不是造更好的宇宙飞船，而是理解问题的底层结构，并探索其本质，例如：问题是否可以拆解为更小的部分，各个部分能否独立求解。


2.1.1

这里讲什么是问题，像柯尼斯堡七桥问题，我们给出了答案就是解决了，但是我们可以把问题更一般的问出来。对于象棋有限大小的棋盘，走法也是有限的，但是当我们定义任意大小任意规则的象棋，有限的步骤就不存在了。定义一个问题时，需要精确的知道输入是什么，问了什么问题。

2.2.2

我们定义了问题，如何定义解？一系列计算运行后，可以得到期望的结果，我们通常不用特定的语言来写而是写成伪代码。

第一个例子是计算整数$a$和$b$的最大公约数。$gcd(a,b)=1$则互质。欧几里得在公元前300年的解法是。

$gcd(a,b)=gcd(a, b\; mod \; a)$

这可以看作是一个递归问题，连续的调用$gcd(a,b)$即可。伪代码如下：


```
Euclid(a,b)
begin
	if b = 0 then return a;
	return Euclid(b, a mod b);
end
```
一个例子：

```
Euclid(120, 33)
= Euclid(33, 21)
= Euclid(21, 12)
= Euclid(12, 9)
= Euclid(9, 3)
= Euclid(3, 0)
=3
```
这个算法怎么样？快还是慢呢？

2.2.3

我们想知道一个算法性能如何，需要考虑最坏的情况，例如我们说一个算法在一定时间内总是成功，那么他在任何时候都成立，即使是最坏的情况。

问题的复杂度是由最坏的情况来表示的，一个问题有困难的情况那么问题就是困难的，只有所有情况都容易，问题才是容易的。