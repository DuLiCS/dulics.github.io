---


layout: post
title: TCM Recongition
subtitle: Part I
tags: [Project, Deep Learning]

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



## TCM Recongition Part I


TCM(Tradition Chinese Medicine) Recongition System 是本年度的创新创业大赛项目，旨在完成以下任务，中药识别，中药质量鉴定，中药真伪鉴定，中药产地鉴定。使用 Deep Learning Technology 来完成任务。大概画了一个导图。

![TCM1](/img/TCM1.png)


### 1.Qt
因为药材还在路上,所以先做准备工作，主要就是数据采集，这次打算做一个可视化工具做这个事情，按键拍照，选择类别，图像命名。
####1.1 设计
以前只用tkinter做过界面，有点简陋，这次就用qt来做。具体Qt的基础知识在这里就不说了，只说重要的。具体的设计如下，大概画了一下，主要就是显示照片，两个按钮，一个填类别的地方。

![TCM2](/img/TCM2.png)

想了想做UI没有价值，直接做成web app显然更好。

#### 1.2 实现

这个项目只要实现就ok,今晚想办法解决。
