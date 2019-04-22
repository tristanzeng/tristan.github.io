---
layout:     post
title:      用Flutter实现一个MD复杂页面
subtitle:   Flutter从入门到放弃
date:       2019-04-22
author:     Tristan
header-img: img/post-bg-flutter.png
catalog: true
tags:
    - Flutter
    - 首页
    
---

## 背景
Flutter作为全新跨平台应用框架，在页面渲染和MD开发上优势明显，可谓是业界一枝独秀。正好最近有这样的一个机会学习Flutter开发，我们便尝试用它开发一个MD风格的复杂页面，来比较跟原生应用开发的优势。也是想通过对新框架的学习探索，找到适合自己应用的框架。

## 开发历程
- 搭建了开发环境，新建flutter module并学习dart语法
- 调研用Flutter实现CoordinatorLayout的方案
- 实现了首页主框架的demo搭建，目前同样遇到了滑动冲突的问题，在调研解决方案
- 解决了滑动冲突的问题，并集成了下拉刷新能力
- 完成了各区块和feed流的静态UI内容，目前剩余feed流加载更多和负二楼动效
- 实现首页feed流的加载更多功能

## 页面展示
<div>
    <img src="../../../../img/post-flutter-1.jpeg" width="33%" style="display: inline-block; border: 2px solid #000000;"/><img src="../../../../img/post-flutter-2.jpeg" width="33%" style="display: inline-block; border: 2px solid #000000; margin-left: 0.5%; margin-right: 0.5%;"/><img src="../../../../img/post-flutter-3.jpeg" width="33%" style="display: inline-block; border: 2px solid #000000;">
</div>
