---

layout: post
title: 深度学习入门  基于Python的理论实现
subtitle: 第二章 感知机
tags: [Deep learning, Reading]

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


# 第二章 感知机

这一个系列是深度学习入门-基于Python的理论与实现的学习记录。看之前的文章，就知道开的新坑实在是太多了。刚好现在是寒假，有比较完整的时间，所以想着用这一段时间做些事情。

暂时是这么安排的，对于学习的过程，一方面写文章，一方面做视频，不清楚做这些的时间成本怎么样，先试试看。
<div align = center;">

![书封面](/img/Deep learning from Scratch.jpg)    

</div>

买过各种各样的机器学习的书了，这次打算从底层好好的过一遍。不管是原理还是实现都尽可能贴近原理。

这里是本书的第二章。由于第一章是Python的入门知识，在这里就不需要再赘述了。Dirac

### 1.1 感知机的概念

感知机(Perceptron),是由美国学者Frank Rosenblatt在1975年提出。


