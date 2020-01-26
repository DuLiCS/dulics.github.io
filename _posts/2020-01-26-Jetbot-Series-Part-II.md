---

layout: post
title: Jetbot Series Part II
subtitle: basic motion
tags: [Jetbot, Jetson Nano]

---

Today I gonna talk about the basic motion of the Jetbot.There are four offical samples of the car.As the car, the first one is basic motion.

Following the last post, we open the web browser in the compute.There are four jupyter notebooks in the list.To get started programming JetBot, we'll need to import the Robot class. This class allows us to easily control the robot's motors! This is contained in the jetbot package.

```python
from jetbot import Robot
``` 
Now that we've imported the Robot class we can initialize the class instance as follows

```python
robot = Robot()
```

