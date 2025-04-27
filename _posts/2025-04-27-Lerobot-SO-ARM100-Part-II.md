---


layout: post
title: Lerobot SO-ARM100 Part II
subtitle: 软件调试
tags: [Project, Robot]

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




# Lerobot 软件调试

硬件部分参见[文章](https://dulics.github.io/2025-04-23-Lerobot-SO-ARM100-Part-I/)。这一篇主要是软件部分，现在还在等东西陆续到位，五一前应该没有问题，现在刚好提前把要做的事情过一下，提高效率。

## 1. Install LeRobot

1. 安装Miniconda（anconda也可以，总而言之，能执行conda命令就可以）。
2. 重启shell。
3. 创建并激活conda环境

   ```
   conda create -y -n lerobot python=3.10
   conda activate lerobot
   ```

4. 克隆LeRobot
   ```
   git clone https://github.com/huggingface/lerobot.git ~/lerobot
   ```

5. 安装ffmpeg
   ```
   conda install ffmpeg -c conda-forge
   ```

6. 安装飞特电机的依赖

```
cd ~/lerobot && pip install -e ".[feetech]"
```

## 2.配置电机