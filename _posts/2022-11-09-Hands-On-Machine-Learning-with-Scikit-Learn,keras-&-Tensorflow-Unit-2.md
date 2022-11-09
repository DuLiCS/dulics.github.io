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

RMSE对应欧几里得范数，也叫做$\ell_2$范数，也即是距离。记作：

$$
\|\cdot\|_2
$$

或

$$
\|\cdot\|
$$

MAE对应$\ell_1$范数，也称为曼哈顿范数。理解为街区移动两点间的距离。记作：
$$
\|\cdot\|_1
$$
一般而言，包含$n$个元素的向量$v$的$\ell_k$定义为：
$$
\|\mathbf{v}\|_k=\left(\left|v_0\right|^k+\left|v_1\right|^k+\cdots+\left|v_n\right|^k\right)^{\frac{1}{k}}
$$
性质：

- 范数指标越高，越关注大值忽略小值。

  

  

## 2.3 获取数据



![](\img\housing_info.png)



