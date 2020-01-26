---

layout: post
title: Jetbot Series Part II
subtitle: basic motion
tags: [Jetbot, Jetson Nano]

---

Today I gonna talk about the basic motion of the Jetbot.There are four offical samples of the car.As the car, the first one is basic motion.

Following the last post, we open the web browser in the compute.There are four jupyter notebooks in the list.

### Importing the Robot class

To get started programming JetBot, we'll need to import the Robot class. This class allows us to easily control the robot's motors! This is contained in the jetbot package.

```python
from jetbot import Robot
``` 
Now that we've imported the ``Robot`` class we can initialize the class instance as follows


```python
robot = Robot()
```

### Commanding the robot

Now that we've created our ``Robot`` instance we named "robot", we can use this instance to control the robot. To make the robot spin counterclockwise at 30% of it's max speed we can call the following

```python
robot.left(speed=0.3)
```
Cool, you should see the robot spin counterclockwise!

> If your robot didn't turn left, that means one of the motors is wired backwards!  Try powering down your
> robot and swapping the terminals that the ``red`` and ``black`` cables of the incorrect motor.


Now, to stop the robot you can call the ``stop`` method.

```python
robot.stop()
```
Maybe we only want to run the robot for a set period of time.  For that, we can use the Python ``time`` package. 

```python
import time
```
This package defines the ``sleep`` function, which causes the code execution to block for the specified number of seconds
before running the next command.  Try the following to make the robot turn left only for half a second.

```python
robot.left(0.3)
time.sleep(0.5)
robot.stop()
```
Great.  You should see the robot turn left for a bit and then stop.

> Wondering what happened to the ``speed=`` inside the ``left`` method?  Python allows 
> us to set function parameters by either their name, or the order that they are defined
> (without specifying the name).

The ``BasicJetbot`` class also has the methods ``right``, ``forward``, and ``backwards``.  Try creating your own cell to make
the robot move forward at 50% speed for one second.

Create a new cell by highlighting an existing cell and pressing ``b`` or the ``+`` icon above.  Once you've done that, type in the code that you think will make the robot move forward at 50% speed for one second.

### Controlling motors individually

Above we saw how we can control the robot using commands like ``left``, ``right``, etc.  But what if we want to set each motor speed 
individually?  Well, there are two ways you can do this

The first way is to call the ``set_motors`` method.  For example, to turn along a left arch for a second we could set the left motor to 30% and the right motor to 60% like follows.

```python
robot.set_motors(0.3, 0.6)
time.sleep(1.0)
robot.stop()
```

Great!  You should see the robot move along a left arch.  But actually, there's another way that we could accomplish the same thing.

The ``Robot`` class has two attributes named ``left_motor`` and ``right_motor`` that represent each motor individually.
These attributes are ``Motor`` class instances, each which contains a ``value`` attribute.  This ``value`` attribute
is a [traitlet](https://github.com/ipython/traitlets) which generates ``events`` when assigned a new value.  In the motor
class, we attach a function that updates the motor commands whenever the value changes.

So, to accomplish the exact same thing we did above, we could execute the following.

```python
robot.left_motor.value = 0.3
robot.right_motor.value = 0.6
time.sleep(1.0)
robot.left_motor.value = 0.0
robot.right_motor.value = 0.0
```