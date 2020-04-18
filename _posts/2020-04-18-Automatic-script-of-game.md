---

layout: post
title: Automatic Script of the Game
subtitle: Part I
tags: [Project,Image Processing]

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

这是游戏中的一个常见画面,我们首先关注右边的任务追踪部分,因为这个部分会显示当前任务状态.因为这部分位置固定,所以直接在目标位置截取就可以.


#### 1.2 图像基本处理部分
接下来就是常规操作,我们首先要将图片转化为灰度图,并且二值化.

```python

img = cv2.imread('img_Winapi.bmp')

gray_img = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)

cv2.imwrite('gray_img.jpg', gray_img)

ret, thresh = cv2.threshold(gray_img,10,255,cv2.THRESH_BINARY_INV)
```

二值化后,我们直接截取目标区域.对于后续,通过观察,任务追踪部分的上下幅度是一定的,而任务名称有大有小,此部分代码将在后续进行修改,思路也很简单,取一个相对较宽的范围,做纵向投影进行分割就好.主要原因是任务叙述部分主要由四部分组成.1.图标2.正传3.任务名4.任务状态,四部分之间是有显著的空间的,二值化后,此部分像素纵向投影为0.二值化并分割后的结果如下.

![](/img/task_area.jpg)

#### 1.3 文字分割

接下来需要将文字进行分割.我门将图片进行水平投影并绘图得到如下结果.

![](/img/task_hist.png)

我们能观察到两处明显的分割点,恰好将这行文字分割为三部分.文字部分我使用过开运算进行处理,但是效果非常不理想.

![](/img/open_process.png)

因此我想,既然文字规格是相近的,所以我直接将其按像素进行分割就可以.首先要找到断点位置.我们直接对图片的纵向投影按照正方向和反方向进行搜寻,找到两个方向的拐点.


```python

task_segment_img = thresh[350:420,1120:1290]

cv2.imwrite('task_area.jpg',task_segment_img)

horizon_img = np.sum(task_segment_img,axis=1)

vertical_img = np.sum(task_segment_img,axis=0)

len(vertical_img)

task_detial_image = task_segment_img[33:,:]

guide_image = task_segment_img[:33,:]

task_hist = np.sum(task_detial_image,axis = 0)

zero_index = np.where(task_hist == 0)


turning_point = []

for i in range(len(zero_index[0])-1):
    if (zero_index[0][i+1] - zero_index[0][i])  !=1:
        turning_point.append(i)
        turning_point.append(i+1)
        

seg_list = []

for i in range(len(turning_point)//2):
    seg_list.append(task_detial_image[:,zero_index[0][turning_point[2*i]]+1:zero_index[0][turning_point[2*i+1]]])

```

以下就得到了三个部分的分割图片.

![](/img/Part_1.png)

![](/img/Part_2.png)

![](/img/Part_3.png)


中间部分每个字符水平宽度为14个字符.分割结果如下.

![](/img/Char_1.png)

![](/img/Char_2.png)

![](/img/Char_3.png)

![](/img/Char_4.png)































