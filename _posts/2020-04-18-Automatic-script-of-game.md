---

layout: post
title: Automatic Script of the Game
subtitle: Part I
tags: [Project]

---

### 写在前面

这个系列是主旨是记录项目的完成情况.所以接下来要做的就是对项目情况做一个简要介绍.这个项目是一个在游戏中自动完成任务的脚本,用来刷游戏金币以及装备以达到盈利的目的.目标游戏是《倩女幽魂》,这个目标的选取当然不是随意的(arbitrary)的,这是一个合作项目,我负责代码编写.

### 1.分析及方法

这个游戏的任务完成较为简单,在我短暂游玩过后,大致明白了主要流程.首先点击任务标志,寻路过程是自动的(省去了很多麻烦),寻路时会显示离目标点的距离.完成后自动交任务,循环往复.

首先,对任务情况的识别是一个重要的点,以及之后选择消灭的目标,例如消灭某某怪物10只,会显示实时进度.

#### 1.1 游戏截图

游戏是运行在Windows系统上的,所以获得实时截图是第一个小的目标.这个完成起来相对容易,主要参考来自于[Python指定窗口截屏](https://www.cnblogs.com/guxingy/p/12201076.html).

```python

#对后台窗口截图
import win32gui, win32ui, win32con
from ctypes import windll
from PIL import Image
import cv2
import numpy

#获取后台窗口的句柄，注意后台窗口不能最小化
hWnd = win32gui.FindWindow("类","标题") #窗口的类名可以用Visual Studio的SPY++工具获取
#获取句柄窗口的大小信息
left, top, right, bot = win32gui.GetWindowRect(hWnd)
width = right - left
height = bot - top
#返回句柄窗口的设备环境，覆盖整个窗口，包括非客户区，标题栏，菜单，边框
hWndDC = win32gui.GetWindowDC(hWnd)
#创建设备描述表
mfcDC = win32ui.CreateDCFromHandle(hWndDC)
#创建内存设备描述表
saveDC = mfcDC.CreateCompatibleDC()
#创建位图对象准备保存图片
saveBitMap = win32ui.CreateBitmap()
#为bitmap开辟存储空间
saveBitMap.CreateCompatibleBitmap(mfcDC,width,height)
#将截图保存到saveBitMap中
saveDC.SelectObject(saveBitMap)
#保存bitmap到内存设备描述表
saveDC.BitBlt((0,0), (width,height), mfcDC, (0, 0), win32con.SRCCOPY)

#如果要截图到打印设备：
###最后一个int参数：0-保存整个窗口，1-只保存客户区。如果PrintWindow成功函数返回值为1
result = windll.user32.PrintWindow(hWnd,saveDC.GetSafeHdc(),0)
print(result) #PrintWindow成功则输出1

#保存图像
##方法一：windows api保存
###保存bitmap到文件
saveBitMap.SaveBitmapFile(saveDC,"img_Winapi.bmp")
```

这里需要注意的一点是,获取类名和标题的方式是用,Spy++工具,这个软件很好下载,将瞄准器图标移动到游戏窗口上就可以完成,这里就不过多赘述了.

得到的图片大概是这个样子.

![](/img/img_Winapi.jpg)
