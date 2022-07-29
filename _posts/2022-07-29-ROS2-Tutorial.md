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


7.29 开始学习


前面的部分就不写在这里了，包括ROS2环境配置等等。这个部分没有什么太多的难度，只要梯子可以正常用基本都可以一键配置。因此从通信机制的第一部分来写。主要学习的是古月居的ROS2入门21讲图文教程。

环境：Ubuntu 20.04.2 ARM64
ROS2版本：Foxy

源码获取方式

```
$ mkdir -p ~/dev_ws/src
$ cd ~/dev_ws/src
$ git clone https://gitee.com/guyuehome/ros2_21_tutorials.git
```
```
$ sudo apt install python3-colcon-ros
$ cd ~/dev_ws/
$ colcon build

```
$ source install/local_setup.sh # 仅在当前终端生效
$ echo " source ~/dev_ws/install/local_setup.sh" >> ~/.bashrc # 所有终端均生效



## 1.节点

ROS2是服务于机器人的操作系统。机器人是各种功能的综合体，每一项功能就像机器人的一个工作细胞，众多细胞通过一些机制连接到一起，成为了一个机器人整体。

在ROS中，我们给这些 “细胞”取了一个名字，那就是**节点**。

### 1.1 通信模型

![ROS2](/img/ROS21.png)

上面的图描述了一个机器人系统，一个机器人在物理上往往不是一个整体，上面的ComputerA负责视频采集和点击控制。ComputerB可以监控，控制机器人。这些不同的功能虽然在不同的计算机中，但是整体组成了完整的机器人系统。这些不同的工作细胞就是节点。

- 节点在机器人系统中的职责就是**执行某些具体的任务**，从计算机操作系统的角度来看，也叫做进程；
- 每个节点都是一个可以**独立运行的可执行文件**，比如执行某一个python程序，或者执行C++编译生成的结果，都算是运行了一个节点；
- 既然每个节点都是独立的执行文件，那自然就可以想到，得到这个执行文件的**编程语言可以是不同的**，比如C++、Python，乃至Java、Ruby等更多语言。
- 这些节点是功能各不相同的细胞，根据系统设计的不同，可能位于计算机A，也可能位于计算机B，还有可能运行在云端，这叫做**分布式**，也就是可以分布在不同的硬件载体上；
- 每一个节点都需要有**唯一的命名**，当我们想要去找到某一个节点的时候，或者想要查询某一个节点的状态时，可以通过节点的名称来做查询。

### 1.2 面向过程的Hello World

```
ros2 run learning_node node_helloworld
```
以上是ros2运行一个节点的代码。实现的是将Hello world打印到终端上。

![ROS2](/img/ROS22.png)

源码：

```python
import rclpy                                   # ROS2 Python接口库
from rclpy.node import Node                    # ROS2 节点类
import time

def main(args=None):                           # ROS2节点主入口main函数
    rclpy.init(args=args)                      # ROS2 Python接口初始化
    node = Node("node_helloworld")             # 创建ROS2节点对象并进行初始化

    while rclpy.ok():                          # ROS2系统是否正常运行
        node.get_logger().info("Hello World")  # ROS2日志输出
        time.sleep(0.5)                        # 休眠控制循环时间

    node.destroy_node()                        # 销毁节点对象
    rclpy.shutdown()                           # 关闭ROS2 Python接口
```

完成代码编写后还需要设置编译选项，让系统知道Python程序的入口在哪里。

```python
    entry_points={
        'console_scripts': [
         'node_helloworld       = learning_node.node_helloworld:main',
        ],
```




### 1.3 案例二 面向对象的Hello world

```
ros2 run learning_node node_helloworld_class
```
![ROS2](/img/ROS23.png)

源码
```python
import rclpy                                     # ROS2 Python接口库
from rclpy.node import Node                      # ROS2 节点类
import time

"""
创建一个HelloWorld节点, 初始化时输出“hello world”日志
"""
class HelloWorldNode(Node):
    def __init__(self, name):
        super().__init__(name)                     # ROS2节点父类初始化
        while rclpy.ok():                          # ROS2系统是否正常运行
            self.get_logger().info("Hello World")  # ROS2日志输出
            time.sleep(0.5)                        # 休眠控制循环时间

def main(args=None):                               # ROS2节点主入口main函数
    rclpy.init(args=args)                          # ROS2 Python接口初始化
    node = HelloWorldNode("node_helloworld_class") # 创建ROS2节点对象并进行初始化
    rclpy.spin(node)                               # 循环等待ROS2退出
    node.destroy_node()                            # 销毁节点对象
    rclpy.shutdown()                               # 关闭ROS2 Python接口


```python
    entry_points={
        'console_scripts': [
         'node_helloworld       = learning_node.node_helloworld:main',
         'node_helloworld_class = learning_node.node_helloworld_class:main',
        ],
```

### 1.4 创建节点的流程

- 编程接口初始化
- 创建节点并初始化
- 实现节点功能
- 销毁节点并关闭接口

### 案例三 物体识别节点

现在做一点更有实际意义的内容。下面的图片是一张苹果的照片。编写一个节点来对苹果进行识别。

![ROS2](/img/ROS23.png)

这里需要使用opencv，按照下面的命令进行安装。

```
sudo apt install python3-opencv
```
例程的运行方式。

```
ros2 run learning_node node_object
```

源码：

```python
import rclpy                            # ROS2 Python接口库
from rclpy.node import Node             # ROS2 节点类

import cv2                              # OpenCV图像处理库
import numpy as np                      # Python数值计算库

lower_red = np.array([0, 90, 128])     # 红色的HSV阈值下限
upper_red = np.array([180, 255, 255])  # 红色的HSV阈值上限

def object_detect(image):
    hsv_img = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)      # 图像从BGR颜色模型转换为HSV模型
    mask_red = cv2.inRange(hsv_img, lower_red, upper_red) # 图像二值化

    contours, hierarchy = cv2.findContours(mask_red, cv2.RETR_LIST, cv2.CHAIN_APPROX_NONE) # 图像中轮廓检测

    for cnt in contours:                                  # 去除一些轮廓面积太小的噪声
        if cnt.shape[0] < 150:
            continue

        (x, y, w, h) = cv2.boundingRect(cnt)              # 得到苹果所在轮廓的左上角xy像素坐标及轮廓范围的宽和高
        cv2.drawContours(image, [cnt], -1, (0, 255, 0), 2)# 将苹果的轮廓勾勒出来
        cv2.circle(image, (int(x+w/2), int(y+h/2)), 5, (0, 255, 0), -1)# 将苹果的图像中心点画出来

    cv2.imshow("object", image)                           # 使用OpenCV显示处理后的图像效果
    cv2.waitKey(0)
    cv2.destroyAllWindows()

def main(args=None):                                      # ROS2节点主入口main函数
    rclpy.init(args=args)                                 # ROS2 Python接口初始化
    node = Node("node_object")                            # 创建ROS2节点对象并进行初始化
    node.get_logger().info("ROS2节点示例：检测图片中的苹果")

    image = cv2.imread('/home/hcx/dev_ws/src/ros2_21_tutorials/learning_node/learning_node/apple.jpg')  # 读取图像
    object_detect(image)                                   # 苹果检测
    rclpy.spin(node)                                       # 循环等待ROS2退出
    node.destroy_node()                                    # 销毁节点对象
    rclpy.shutdown()                                       # 关闭ROS2 Python接口

```