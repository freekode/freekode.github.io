---
layout: post
title: Never use robot.setAutoDelay()
---

I have one project, where I use SpringWorker to easily start and kill threads.

Within this threads I am using a [Robot](https://docs.oracle.com/javase/7/docs/api/java/awt/Robot.html) class, to simulate mouse and keyboard action. For proper using there must be a little delay between actions (press and release button). I started use simple [robot.setAutoDelay(int)](https://docs.oracle.com/javase/7/docs/api/java/awt/Robot.html#setAutoDelay%28int%29).

Also with robot I am using `Thread.sleep()`, for killing the threads you must not ignore `InterruptedException`, if you do, each time when you will try to `interrput()` you simply go to ignore block and you thread will continue.

What is doing `robot.setAutoDelay(int)` it is ignoring the `InterruptedException`. You can go to sources and see, that after invoking any func in robot, it calling `robot.delay()` which is simply ignore our exception.

So try to place your own `Thread.sleep()` in each place where it should be instead of .`robot.setAutoDelay()`
