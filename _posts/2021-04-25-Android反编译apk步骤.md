---
layout:     post
title:      Android反编译apk步骤
subtitle:   逆向开发
date:       2021-04-25
author:     Tristan
header-img: img/android_reverse_bg.jpeg
catalog: true
tags:
    - 反编译
    - 逆向
    
---

## APK反编译
#### 工具Apktool
官网下载地址 [apktool下载](https://ibotpeaches.github.io/Apktool/install/)

#### 步骤详解
1. **下载apktool**<br/>
* 鼠标移到wrapper script，右键，保存链接命名为apktool。mac会保存为文本的形式<br/>
* 下载apktool-2，下载完之后，修改名字为apktool.jar<br/>
* 之后把两个文件都移到 /usr/local/bin 文件中，我是通过，finder的前往文件夹，移动文件的<br/>
* 之后终端进入到 /usr/local/bin 中<br/>
* 输入两行代码: chmod +x apktool；chmod +x apktool.jar<br/>
* 之后输入apktool，能输出一堆，说明成功了。<br/>

2. **解压apk文件**<br/>
* 下载apk，之后把后缀名改为.zip，进行解压。

3. **反编译代码**
    > apktool d -f [apk文件] -o [输出文件夹]

    ![image](https://user-images.githubusercontent.com/4709890/115993156-25d92580-a604-11eb-809a-1aa51a645052.png)

## 代码反编译
#### 工具smali和dex2jar
下载地址 [smali下载](https://www.greenxf.com/soft/123267.html)<br/>
下载地址 [dex2jar下载](https://sourceforge.net/projects/dex2jar/?source=typ_redirect)

#### smali转dex
> java -jar smali-2.1.3.jar [smali文件夹] -o classes.dex

#### dex转jar
> chmod +x d2j_invoke.sh<br/>
> sh d2j-dex2jar.sh classes.dex

## 代码阅读
#### 工具AndroidStudio
Android开发必备

#### 步骤详解
1. 将jar包拷贝到libs目录
2. 右键将jar标记为lib
3. 这样就可以查看源码了

![image](https://user-images.githubusercontent.com/4709890/115993957-a3526500-a607-11eb-9196-3673cce1a073.png)


