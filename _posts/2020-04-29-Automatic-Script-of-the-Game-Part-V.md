---

layout: post
title: c = OperationClass()
       
       for i in range(10):
           
           if c.FindStrFastEx(0,0,int(c.width),int(c.height*0.5),"力破奇案有担当","ffffff-111111",0.9):
               c.LeftClick(c.x,c.y,20,10)
               print("found")
               break
           time.sleep(0.5)
       
       
       time.sleep(10)
subtitle: Machine Learning Wiki
tags: [Project, Image Processing]

---

### 6.代码部分

第一次接触游戏脚本，所以参考资料是必不可少的，我主要借鉴的是B站上的一个视频，墨迹归墨迹，讲的还是很详细的，适合我这样的人。

说实话之前python写的也不少了，但是面向对象的思想包括代码写的比较少，这次算是一个不错的实践。

之前说过如何去加载大漠插件，之后所有内容都以大漠为主。那么按照面向对象的思想，我需要把方法封装在类里。还有就是加载的过程我可以封装在构造函数里

#### 6.1构造函数

直接开始写。

```python
class OperationClass():
    def __init__(self):

        self.hwnd = FindWindow("CknNkfBhSEg6cvaeM",None)        
        self.path = os.getcwd()
        regdm = WinDLL(r"%s\DmReg.dll" %self.path)
        regdm.SetDllPathW(r"%s\dm.dll" %self.path)
        self.dm = client.Dispath("dm.dmsoft")
        ret = self.dm.Reg("注册码在这里")
        self.Bind()
```
以上就是具体的构造函数部分，主要做了两件事，一件是将dll文件进行加载，也就是载入脚本。另外一件事是做了绑定操作，在这边使用了自己写的方法Bind,绑定是一个非常重要的过程，它打通了程序和游戏，一旦绑定成功，程序就可以自由的对游戏进行控制。

下面介绍一下除了构造函数之外，我们还定义了什么方法。

#### 6.2 绑定函数Bind

关于绑定方式，这里要说的主要内容是绑定方式，对于不同的游戏，绑定方式是不同的，需要尝试不同的组合，具体的需要在大漠插件的绑定测试工具中进行，一旦成功，我们将自动生成的代码复制进我们的代码就可以完成。
```python
def Bind():
    self.dm.BindWindowEx(self.hwnd,"gdi","dx.mouse.position.lock.api|dx.mouse.position.lock.message|dx.mouse.clip.lock.api|dx.mouse.input.lock.api|dx.mouse.state.api|dx.mouse.api|dx.mouse.cursor","dx.keypad.input.lock.api|dx.keypad.state.api|dx.keypad.api","",2)

     self.dm.SetSimMode(0)
        
     self.dm.EnableRealMouse(2,20,30)
        
     self.dm.EnableRealKeypad(1)
    
     self.dm.SetDict(0,"%s\ziku.txt" %self.path)
    
     self.GetClientSize()
```
在这部分代码中，除了绑定外，还调用了几个函数，比如模拟真实鼠标键盘操作的函数，调用字库等等。

#### 6.3 解绑

有绑定自然有解绑，只是解绑的过程相对简单一些。调用自带的UBindWindow函数就可以。

#### 6.4 点击操作

大漠插件提供鼠标左键右键的操作，单击双击等等，但是在这里我们将点击的操作二次封装，以便更不容易被识别到。主要就是在点击的时候在提供精确坐标的同时加上一定的偏移量。在下面以单击鼠标左键为例来说明。

```python

def LeftClick(self,x1,y1,xr,yr,delay=uniform(0.3,0.8)):
    x = x1 + randint(1, xr)
    y = y1 + randint(1, yr)
    self.dm.MoveTo(x,y)        
    self.dm.LeftClick()        
    time.sleep(delay) 
```

这里一方面加入了坐标的偏移量(randint)，另一方面在每次点击操作后，加入了一个delay，这个delay的取值也是随机量(uniform)。

#### 6.5 获取窗口大小

当前窗口的大小是一个很重要的量，因为一切定位操作都基于这个数据。

```python
def GetClientSize():
    
    ret = self.GetClientSize(self.hwnd)
    self.width = ret[1]
    selfheight = ret[2]
```
#### 6.6 按键操作

 按键比起点击更简单一些，直接调用函数就可以。
 ```python
def KeyPress(self,n):
    self.dm.SetWindowState(self.hwnd,12)
    self.dm.KeyPress(n)
```

#### 6.7 FindStrFastEx识别字符

对画面中的字符串进行识别，这是一个常见的功能。我们将插件中提供的方法进行封装。
```python
def FindStrFastEx(self,x1,y1,x2,y2,string,color_format,sim,n=0):
    ret = self.dm.FindStrFastEx(x1,y1,x2,y2,string,color_format,sim)
    if len(ret)>0:
        ss = ret.split("|")
        num = len(ss)
        s = ss[n].split(",")
        self.x = int(s[1])
        self.y = int(s[2])
            return 1
        else:
            return 0
```

#### 6.8小结

这就是我们所写的操作类里的方法，做了详细的介绍，对于大漠插件本身提供的方法，可以简单的通过手册进行查询。

下面放出的是一个简单的展示。识别文字并进行点击。

```python
c = OperationClass()

for i in range(10):
    
    if c.FindStrFastEx(0,0,int(c.width),int(c.height*0.5),"力破奇案有担当","ffffff-111111",0.9):
        c.LeftClick(c.x,c.y,20,10)
        print("found")
        break
    time.sleep(0.5)


time.sleep(10)
```

效果就不在这里展示了。接下来我们需要更进一步，自动完成一个完整的任务，思路上比较简单，就是摸清楚所有的流程并进行自动化。这部分内容将在接下来的第六部分进行探索。

