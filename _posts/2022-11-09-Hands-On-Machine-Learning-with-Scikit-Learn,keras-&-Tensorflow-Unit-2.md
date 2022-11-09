---


layout: post
title: Hands On Machine Learning with Scikit Learn,keras & Tensorflow Unit 2
subtitle: Unit 2
tags: [tensorflow, machine learning]

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



# 目录



# 第二章 端到端的机器学习项目

第二章是加州房价问题，很经典的问题了，这里把重要的东西总结出来就可以。

### 2.2.2 选择性能指标

RMSE算是经典的误差表示形式。对于较大的误差，因为平方的缘故，数值较大。

$$
\operatorname{RMSE}(\mathbf{X}, h)=\sqrt{\frac{1}{m} \sum_{i=1}^m\left(h\left(\mathbf{x}^{(i)}\right)-y^{(i)}\right)^2}
$$

个别数值较大或有异常区域是，使用MAE（Mean Absolute Error）

$$
\operatorname{MAE}(\mathbf{X}, h)=\frac{1}{m} \sum_{i=1}^m\left|h\left(\mathbf{x}^{(i)}\right)-y^{(i)}\right|
$$

RMSE对应欧几里得范数  $\ell_2$，也即是距离。记作：

$$
\|\cdot\|_2
$$

或

$$
\|\cdot\|
$$

MAE对应

