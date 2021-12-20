---
layout:     post
title:      一种Application初始化前获得APP启动参数的方法
subtitle:   隐私弹窗实现原理
date:       2021-12-06
author:     Tristan
header-img: img/post-bg-multimedia.jpeg
catalog: true
tags:
    - 启动参数
    - 隐私弹窗

---

## 背景
APP在启动时通常可以带一些参数，Android系统会在Application初始化后，紧接着根据这些参数创建相应的页面Activity，同时会把这些参数传递给页面，然后完成程序到页面的启动。从系统提供的能力看，启动参数只有在启动页创建后通过页面Intent才可以获取，Application的初始化是在启动页创建之前完成的，它的初始化方法中系统并没有给其提供启动参数。

## 问题
一些在Application初始化依赖启动参数的逻辑无法在初始化执行，很多都移到了启动页Activity的初始化方法中。比如：一般App会在LaunchActivity或HomeActivity中解析外部调起的协议数据再做分发。

## 目标
本文主要讲一种Hook方案，重在解决Android在Application初始化时无法直接获得启动参数的问题。同时，也是要引出另一个主题，就是在启动页启动之前，乃至Application初始化（SDK初始化）之前，能不能根据启动参数做一些逻辑和页面处理（隐私弹窗）。

## 实现思路
1. 通过Hook Instrumentation技术实现对Application的初始化和Activity页面创建这两个方法的重写。
2. Hook Application的初始化方法，将初始化逻辑暂时屏蔽。
3. Hook Activity页面的创建方法，首先获取到启动参数。
4. 接着将获取到的启动参数传给Application，重新完成初始化逻辑。
5. 完成初始化后，记得将Hook取消，防止其它非启动页被Hook后再次初始化Application。
6. 继续往下执行Activity页面的创建方法，走完后续的启动流程。
<img  referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"  src="https://wos.58cdn.com.cn/IjGfEdCbIlr/ishare/03c668e3-19ee-4f6c-a37f-e9c2795fc57e图片 1.png" width="50%"></img>

## 具体实现
1. 通过反射拿到 ActivityThread 的 sCurrentActivityThread 变量的值，然后再通过该实例变量拿到mInstrumentation 变量。再通过反射 将 mInstrumentation 变量设置为我们自定义的 CustomInstrumentation 对象。这样就实现了Hook Instrumentation。
<img  referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"  src="https://wos.58cdn.com.cn/IjGfEdCbIlr/ishare/c6bda895-b5b2-4650-9b4c-407b044cae72图片 2.png" width="80%"></img>

2. 自定义Instrumentation。通过重写callApplicationOnCreate方法，将父类的callApplicationOnCreate方法暂时不要调用，以此屏蔽Application的初始化方法。
<img  referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"  src="https://wos.58cdn.com.cn/IjGfEdCbIlr/ishare/33cf6b39-8280-43cf-966c-1aebd3b23579图片 3.png" width="80%"></img>

3. 同理，重写newActivity方法，在newActivity方法中可以直接获取到intent对象，它存放了启动参数。

4. 取到启动参数后，交给自定义的Application，然后Application在初始化时就可以获得启动参数了。紧接着，通过重新调用父类的callApplicationOnCreate方法完成Application的初始化任务。
<img  referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"   referrerPolicy="no-referrer"  src="https://wos.58cdn.com.cn/IjGfEdCbIlr/ishare/e5532a5e-fa39-48b2-9a5c-2ff80ded2c14图片 4.png" width="80%"></img>

5. 可以看到，Application在初始化时已经可以获得启动参数Bundle对象了。获得启动参数后，不应该再重写newActivity方法了，这里需要将Hook取消。Hooker对象在自定义的Application中，通过调用它的resetInstrumentation方法可以把之前替换的Instrumentation还原回去。

6. 最后通过调用父类的newActivity方法执行完创建启动页的流程，往下的启动流程则继续交由系统来完成。

基于Hook的思想，加之以上方法步骤的实现，便可以在Application初始化初期获得启动参数了。

## 特殊方法获得启动参数

![image.png](https://wos.58cdn.com.cn/IjGfEdCbIlr/ishare/ba3da035-f19c-426a-b60d-a216549521a1image.png)

## 总结
Hook技术不会很大程度地去侵害原有的代码逻辑，可以看见，通过在自定义CustomApplication的attachBaseContext方法中一次挂入新的Instrumentation，便可在Application中获得Bundle启动参数对象。同理，还可以重写Instrumentation的newActivity方法实现更多的技术需求，比如：启动页的隐私授权弹窗逻辑等。
