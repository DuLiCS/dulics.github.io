---

layout: post
title: Automatic Script of the Game Part III
subtitle: Part III
tags: [Project,Image Processing]

---

### 4. 初步整合

#### 4.1 截图函数封装

图像是一切的根本,所以第一步先将截图程序封装测试.这部分就不贴上来了.

#### 4.2 模拟点击任务,自动寻路

第一步上来就遇到大问题了,什么问题呢,就是在游戏内不能控制鼠标,这就遇到了大问题了.这个问题解决不了那一切都是白搭,猜想这是游戏的反脚本机制,试了各种python库都无效,但是在别的应用都没有问题,所以赶紧去查资料解决问题.解决问题大概就是调用脚本,那么现在就赶紧尝试.有两个脚本可用,一个是大漠还有一个是天使插件.

中间遇到了很多问题,主要就是注册不成功,提示没有注册类或者不是有效的Win32程序.那么解决的办法也很简单,就是装一个32位的python,当然这里说的很简单,实际上试了很多次.那么这两个插件的使用方式如下:

#### 大漠

```python
from ctypes import WinDLL
import os

path = os.getcwd()

DmReg = WinDLL(r"%s\DmReg.dll" %path)

DmReg.SetDllPathW(r"%s\DmReg.dll" %path,0)

from win32com import client

dm = client.Dispatch("dm.dmsoft")
```

#### 乐玩

```python
from comtypes.client import CreateObject
import os

path = os.getcwd()

os.system(r"regsvr32 %s\lw.dll" %path)
lw = CreateObject("lw.lwsoft3")
```












