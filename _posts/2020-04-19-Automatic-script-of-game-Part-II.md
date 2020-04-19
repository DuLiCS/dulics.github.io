---

layout: post
title: Automatic Script of the Game Part II
subtitle: Part II
tags: [Project,Image Processing]

---

### 2. 文字识别

借着昨天的写.昨天的进度是顺利的将文字进行了分割,接下来的部分将是识别.

先说一点就是每一天的任务完成后,没有特殊情况,文章的内容便不再进行改动,以此达到记录的目的.

今天在观察时发现了一点疏漏,在这里说明一下.主要内容就是文字的内容在一定程度上是共享的.我们拿两个字来举例说明.


![字符1](/img/Char_1.png)

![字符2](/img/Char_2.png)

观察可以得到,“学”字和“以”字的右边和左边重合了.但是在写这句话的时候,突然意识到这一点似乎并不重要,只要按照一定的标准统一就可以,无需刻意.也就是说,我认定这是什么就是什么,和我定义的相同即可.

#### 2.1 识别方法

那么接下来就进入正题,开始文字识别的方法部分.这一部分可以有多种思路,首先能想到的就是用Tensorflow撺一个多层感知器,用最简单的神经网络就足以做到精准识别.这是思路一.但是这么规整统一的样本真的需要这么复杂的识别方式么?因此这里就有了思路二,简单实用的方法.余弦相似度应该就可以很好的完成任务.或者直接计算两个向量的欧氏距离即可.第二种方法中的两个变种的精确性将在之后有更进一步的数据后进行验证,但考虑到余弦相似性已经很快,而且比较直接计算距离在理论上更加复杂一点点(当然只是一点点).下面还是简单介绍一下余弦相似的理论[[1]](https://zh.wikipedia.org/wiki/%E4%BD%99%E5%BC%A6%E7%9B%B8%E4%BC%BC%E6%80%A7).

余弦相似性通过测量两个向量的夹角的余弦值来度量它们之间的相似性。0度角的余弦值是1，而其他任何角度的余弦值都不大于1；并且其最小值是-1。从而两个向量之间的角度的余弦值确定两个向量是否大致指向相同的方向。两个向量有相同的指向时，余弦相似度的值为1；两个向量夹角为90°时，余弦相似度的值为0；两个向量指向完全相反的方向时，余弦相似度的值为-1。这结果是与向量的长度无关的，仅仅与向量的指向方向相关。余弦相似度通常用于正空间，因此给出的值为0到1之间。

#### 2.1.1 余弦相似性的定义

两个向量间的余弦值可以通过使用欧几里得点积公式求出：

![欧几里得点积](/img/similarity_cos_1.svg)

给定两个属性向量,A和B,其余弦相似性θ由点积和向量长度给出,如下所示：

![余弦相似性定义](/img/similarity_cos_2.svg)

给出的相似性范围从-1到1:-1意味着两个向量指向的方向正好截然相反,1表示它们的指向是完全相同的,0通常表示它们之间是独立的,而在这之间的值则表示中间的相似性或相异性。


到这里识别的部分基本说完了,现在没有办法再进行下去是因为现在系统还没有完全跑起来,很多实际的问题还没有遇到,只有在跑程序的过程中才可以发现问题,进行测试和优化.
因此我们接下来转向第三个问题,也就是控制,因为这个自动执行的程序的交互式靠点击完成的,所以交互也是一个很重要的环节.

### 3. 交互

#### 3.1 pyautogui

]查阅了相关资料后,由于这些库都没有使用过,就先用pyautogui来试试手.
安装部分很简单,在anaconda环境管理里没有找到,直接用pip安装就可以了

```
pip install pyautogui
```

#### 3.2 获取屏幕分辨率

依赖的库有点多,安装起来稍微花了点时间.使用起来也不算太难,下面简单介绍一下.既然要控制鼠标,那么我们需要知道屏幕的大小,才能定位.pyautogui提供了这样的库来的到当前屏幕的分辨率.

```python
pyautogui.size()
```

但是问题来,返回的结果我看不懂了,

```
Size(width=1280, height=800)
```
我的电脑屏幕明明是2560x1600的分辨率,外接屏幕也是1600x900,那么为什么会返回这么一个结果呢.(这时的我还不知道为什么).那么接下来要做两件事,一个是在我的surface book2检验返回的结果是否正确,另一个就是查查[官方文档](https://pyautogui.readthedocs.io/en/latest/).

Locations on your screen are referred to by X and Y Cartesian coordinates. The X coordinate starts at 0 on the left side and increases going right. Unlike in mathematics, the Y coordinate starts at 0 on the top and increases going down.

```
0,0       X increases -->
+---------------------------+
|                           | Y increases
|                           |     |
|   1920 x 1080 screen      |     |
|                           |     V
|                           |
|                           |
+---------------------------+ 1919, 1079
```

The pixel at the top-left corner is at coordinates 0, 0. If your screen’s resolution is 1920 x 1080, the pixel in the lower right corner will be 1919, 1079 (since the coordinates begin at 0, not 1).

The screen resolution size is returned by the size() function as a tuple of two integers. The current X and Y coordinates of the mouse cursor are returned by the position() function.

单看这个文档看不出什么端倪,我把外接显示器拔掉也是一样的结果,那么现在只能尝试在Windows上使用了.

经过测试,在windows上是没有问题的,显示分辨率3240x2160.也不知道是为什么,在OSX上就是有问题,暂时先不管这个问题,因为最后还是要在windows环境下跑的.

#### 3.3 要用到的方法

接下来介绍一下主要使用的几个函数.

**Mouse Movement**

```python
>>> pyautogui.moveTo(100, 200)   # moves mouse to X of 100, Y of 200.
>>> pyautogui.moveTo(None, 500)  # moves mouse to X of 100, Y of 500.
>>> pyautogui.moveTo(600, None)  # moves mouse to X of 600, Y of 500.
```

用法很简单,输入X和Y的坐标即可.有一点要说的是None这个参数的意思是传入鼠标当前位置的X或Y值.正常来说,如果单单是用MoveTo的话,鼠标会立刻移动到指定坐标,如果我们希望这个过程是gradually的,那么就需要另外一个参数,duration,这个单位默认是秒,例如

```python
>>> pyautogui.moveTo(100, 200, 2)   # moves mouse to X of 100, Y of 200 over 2 seconds
```

底下有个注意事项,为了方便起见,我就不翻译了,直接粘过来,非常简单.

(If the duration is less than ```pyautogui.MINIMUM_DURATION``` the movement will be instant. By default, ```pyautogui.MINIMUM_DURATION``` is 0.1.)




