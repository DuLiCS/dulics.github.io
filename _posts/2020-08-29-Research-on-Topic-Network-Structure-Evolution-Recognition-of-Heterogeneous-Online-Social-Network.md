---

layout: post
title: Research on Topic Network Structure Evolution Recognition of Heterogeneous Online Social Network
subtitle:
tags: [NLP,Research]

---


# Research on Topic Network Structure Evolution Recognition of Heterogeneous Online Social Network
## 异源在线社会网络信息扩散结构演化识别研究 


本项目源起于Spence Zhou的硕士毕业论文,需要重新发文章,之前他的代码已经遗失,现在需要重新写,但是他没有时间去做这些事情,因此就交给了我.写这篇post的初衷在于记录整个代码编写的过程.

由于是实现,我们直接从系统设计开始.

### 系统设计

异源在线社会网络话题发现和信息扩散结构演化识别系统包括:数据获取模块、文本预处理模块、话题发现模块、话题扩散结构演化识别模块 4 个模块，系统各模块之间交互关系如图 5-1 所示，数据获取模块将异源在线社会网络中的数据抓取并保存 到系统数据库中，通过文本预处理模块将数据库中的文本信息进行关键词抽取，将处 理后的数据利用文本间基于关键词重合的网络模型构造文本关系网络，对文本关系网 络运用动态社团发现算法进行社团划分从而进行话题发现，将所发现话题的参与用户 信息映射到话题传播网络模型中，再次利用动态社团发现算法对话题传播网络进行动 态社团划分，最对话题扩散结构演化进行识别。本节对系统各个模块的内部流程和详 细设计进行说明。

![系统设计](/img/system_design.png)

