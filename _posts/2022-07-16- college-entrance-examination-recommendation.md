---


layout: post
title: College Entrance Examination Recommendation
subtitle: Part I
tags: [Project]

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




## 1.背景介绍

刚好到了志愿填报季，报志愿也是考生和家长都比较关心的问题。现在一切都从工程的角度看问题，做出来的项目必须是有实际意义的东西，不能只做成半成品或者仅仅验证算法，那样的话就没有多少意义。
高考志愿填报是一个大部分人都比较熟悉的东西，只不过现在比以往会复杂一些，多了很多各种各样的计划，提前批那样的情况。现在也有很多机构和网站都在做类似的东西，换句话说，他们掌握的信息和算法不见得有个人掌握的多，因此，这是一个没有太多知识壁垒的项目。也是为后续做类似的应用做基础和准备。

## 2.大体思路

这个项目对于基于数据的项目来说，是比较典型的，因此第一步是数据的获取，简而言之，就是要使用到爬虫技术。因此这篇文章作为Part I，主要就是阐述如何设计，获取数据，最后成为数据库可以随时存取使用。

## 3.爬虫基础

爬虫的选取涉及两个问题，使用什么工具和语言，我更倾向于使用Java，尽管Python可能要更熟悉一些，但是我想为了之后更大的项目做准备，仅仅使用Python可能会在之后遇到瓶颈。之前我只用过selenium做过一些自动化测试的事情，但是selenium的问题在于效率较低，好在这个项目需要访问的网页较少，其实用什么都可以。所以最终的技术路线选择是Java下的某一个框架，能实现就好。

