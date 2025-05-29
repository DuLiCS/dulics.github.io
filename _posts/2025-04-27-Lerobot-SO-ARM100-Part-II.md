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

### 2.1 配置电机

1. 找到每条机械臂的 USB 端口
   
   驱动板连接电源、type c连接驱动板和计算机（我这里是mac）。运行下面的程序,出现Remove the USB cable from your MotorsBus and press Enter when done.后拔下usb按回车：

   ```
   python lerobot/scripts/find_motors_bus_port.py
   ```
   我的输出如下：

   ```
   python lerobot/scripts/find_motors_bus_port.py
   Finding all available ports for the MotorsBus.
   Ports before disconnecting: ['/dev/tty', '/dev/ttyp0', '/dev/ttyp1', '/dev/ttyp2', '/dev/ttyp3', '/dev/ttyp4', '/dev/ttyp5', '/dev/ttyp6', '/dev/ttyp7', '/dev/ttyp8', '/dev/ttyp9', '/dev/ttypa', '/dev/ttypb', '/dev/ttypc', '/dev/ttypd', '/dev/ttype', '/dev/ttypf', '/dev/ttyq0', '/dev/ttyq1', '/dev/ttyq2', '/dev/ttyq3', '/dev/ttyq4', '/dev/ttyq5', '/dev/ttyq6', '/dev/ttyq7', '/dev/ttyq8', '/dev/ttyq9', '/dev/ttyqa', '/dev/ttyqb', '/dev/ttyqc', '/dev/ttyqd', '/dev/ttyqe', '/dev/ttyqf', '/dev/ttyr0', '/dev/ttyr1', '/dev/ttyr2', '/dev/ttyr3', '/dev/ttyr4', '/dev/ttyr5', '/dev/ttyr6', '/dev/ttyr7', '/dev/ttyr8', '/dev/ttyr9', '/dev/ttyra', '/dev/ttyrb', '/dev/ttyrc', '/dev/ttyrd', '/dev/ttyre', '/dev/ttyrf', '/dev/ttys0', '/dev/ttys1', '/dev/ttys2', '/dev/ttys3', '/dev/ttys4', '/dev/ttys5', '/dev/ttys6', '/dev/ttys7', '/dev/ttys8', '/dev/ttys9', '/dev/ttysa', '/dev/ttysb', '/dev/ttysc', '/dev/ttysd', '/dev/ttyse', '/dev/ttysf', '/dev/ttyt0', '/dev/ttyt1', '/dev/ttyt2', '/dev/ttyt3', '/dev/ttyt4', '/dev/ttyt5', '/dev/ttyt6', '/dev/ttyt7', '/dev/ttyt8', '/dev/ttyt9', '/dev/ttyta', '/dev/ttytb', '/dev/ttytc', '/dev/ttytd', '/dev/ttyte', '/dev/ttytf', '/dev/ttyu0', '/dev/ttyu1', '/dev/ttyu2', '/dev/ttyu3', '/dev/ttyu4', '/dev/ttyu5', '/dev/ttyu6', '/dev/ttyu7', '/dev/ttyu8', '/dev/ttyu9', '/dev/ttyua', '/dev/ttyub', '/dev/ttyuc', '/dev/ttyud', '/dev/ttyue', '/dev/ttyuf', '/dev/ttyv0', '/dev/ttyv1', '/dev/ttyv2', '/dev/ttyv3', '/dev/ttyv4', '/dev/ttyv5', '/dev/ttyv6', '/dev/ttyv7', '/dev/ttyv8', '/dev/ttyv9', '/dev/ttyva', '/dev/ttyvb', '/dev/ttyvc', '/dev/ttyvd', '/dev/ttyve', '/dev/ttyvf', '/dev/ttyw0', '/dev/ttyw1', '/dev/ttyw2', '/dev/ttyw3', '/dev/ttyw4', '/dev/ttyw5', '/dev/ttyw6', '/dev/ttyw7', '/dev/ttyw8', '/dev/ttyw9', '/dev/ttywa', '/dev/ttywb', '/dev/ttywc', '/dev/ttywd', '/dev/ttywe', '/dev/ttywf', '/dev/tty.OTR', '/dev/tty.vector', '/dev/tty.EMBERTON', '/dev/tty.Pyra', '/dev/tty.Bluetooth-Incoming-Port', '/dev/ttys001', '/dev/ttys000', '/dev/ttys002', '/dev/tty.wchusbserial5A460840351', '/dev/tty.usbmodem5A460840351', '/dev/ttys003']
   Remove the USB cable from your MotorsBus and press Enter when done.

   Traceback (most recent call last):
   File "/Users/duli/CS/Project/lerobot-ARM/lerobot/lerobot/scripts/find_motors_bus_port.py", line 55, in <module>
      find_port()
      ~~~~~~~~~^^
   File "/Users/duli/CS/Project/lerobot-ARM/lerobot/lerobot/scripts/find_motors_bus_port.py", line 50, in find_port
      raise OSError(f"Could not detect the port. More than one port was found ({ports_diff}).")
   OSError: Could not detect the port. More than one port was found (['/dev/tty.usbmodem5A460840351', '/dev/tty.wchusbserial5A460840351']).
   ```

   这里是因为找到了两个port，原因和mac的端口机制有关。参照官方示例选择了usbmodem5A460840351。

2. 修改配置。

   根据主从臂的端口号修改配置文件。同一块驱动板改动usb端口并不改变端口号。文件位置在lerobot/common/robot_devices/robots/configs.py 434行的位置。

   ```
   @RobotConfig.register_subclass("so100")
   @dataclass
   class So100RobotConfig(ManipulatorRobotConfig):
      calibration_dir: str = ".cache/calibration/so100"
      # `max_relative_target` limits the magnitude of the relative positional target vector for safety purposes.
      # Set this to a positive scalar to have the same value for all motors, or a list that is the same length as
      # the number of motors in your follower arms.
      max_relative_target: int | None = None

      leader_arms: dict[str, MotorsBusConfig] = field(
         default_factory=lambda: {
               "main": FeetechMotorsBusConfig(
                  port="/dev/tty.usbmodem58760431091",  <-- UPDATE HERE
                  motors={
                     # name: (index, model)
                     "shoulder_pan": [1, "sts3215"],
                     "shoulder_lift": [2, "sts3215"],
                     "elbow_flex": [3, "sts3215"],
                     "wrist_flex": [4, "sts3215"],
                     "wrist_roll": [5, "sts3215"],
                     "gripper": [6, "sts3215"],
                  },
               ),
         }
      )

      follower_arms: dict[str, MotorsBusConfig] = field(
         default_factory=lambda: {
               "main": FeetechMotorsBusConfig(
                  port="/dev/tty.usbmodem585A0076891",  <-- UPDATE HERE
                  motors={
                     # name: (index, model)
                     "shoulder_pan": [1, "sts3215"],
                     "shoulder_lift": [2, "sts3215"],
                     "elbow_flex": [3, "sts3215"],
                     "wrist_flex": [4, "sts3215"],
                     "wrist_roll": [5, "sts3215"],
                     "gripper": [6, "sts3215"],
                  },
               ),
         }
      )
    ```

3. 设置电机ID

   分别插入电机运行代码如下,配置ID1-6，主从电机分别执行。执行后贴标签：

   ```
      python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem58760432961 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 1
   ```
   、、、
   python lerobot/scripts/configure_motor.py \
  --port /dev/tty.usbmodem5A460831501 \
  --brand feetech \
  --model sts3215 \
  --baudrate 1000000 \
  --ID 1
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.42it/s]
   Motor index found at: 1
   Setting its index to desired index 1
   Present Position [2047]
   Offset [0]
   Disconnected from motor bus.
   python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem5A460831501 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 2
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.44it/s]
   Motor index found at: 1
   Setting its index to desired index 2
   Present Position [2047]
   Offset [0]
   Disconnected from motor bus.
   python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem5A460831501 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 3
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.44it/s]
   Motor index found at: 1
   Setting its index to desired index 3
   Present Position [2051]
   Offset [0]
   Disconnected from motor bus.
   python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem5A460831501 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 4
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.44it/s]
   Motor index found at: 1
   Setting its index to desired index 4
   Present Position [2046]
   Offset [0]
   Disconnected from motor bus.
   python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem5A460831501 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 5
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.44it/s]
   Motor index found at: 1
   Setting its index to desired index 5
   Present Position [2050]
   Offset [0]
   Disconnected from motor bus.
   python lerobot/scripts/configure_motor.py \
   --port /dev/tty.usbmodem5A460831501 \
   --brand feetech \
   --model sts3215 \
   --baudrate 1000000 \
   --ID 6
   Connected on port /dev/tty.usbmodem5A460831501
   Scanning all baudrates and motor indices
   100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 9/9 [00:00<00:00, 16.44it/s]
   Motor index found at: 1
   Setting its index to desired index 6
   Present Position [2049]
   Offset [0]
   Disconnected from motor bus.
   ```


### 2.2 拆除主臂电机的齿轮。

   开盖后去除齿轮。** 特别强调：拆除时不能移动轴，否则很麻烦！！！！ **。另外，螺丝非常紧，一定要找好趁手的工具，拆了4个手快废了，上面的两个螺丝非常难拆，有一颗螺丝差点滑丝，用钳子取下来的。今天已经买了电动工具，工欲善其事，必先利其器。


## 3. 校准

安装好所有后，需要对电机进行校准。

```
python lerobot/scripts/control_robot.py \
  --robot.type=so100 \
  --robot.cameras='{}' \
  --control.type=calibrate \
  --control.arms='["main_follower"]'
```

按照提示摆所有的位置。

| 1. Middle position | 2. Zero position                                                                                                                                       | 3. Rotated position                                                                                                                                             | 4. Rest position                                                                                                                                       |
| ------------ |------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [](/img/follower_middle.jpg)| [](/img/follower_zero.jpg) | <img src="../media/so101/follower_rotated.webp?raw=true" alt="SO-101 leader arm rotated position" title="SO-101 leader arm rotated position" style="width:100%;"> | <img src="../media/so101/follower_rest.webp?raw=true" alt="SO-101 leader arm rest position" title="SO-101 leader arm rest position" style="width:100%;"> |