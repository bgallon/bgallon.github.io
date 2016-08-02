---
layout: post
title: "Activity生命周期（一）"
date: 2016-07-31 14:45:43
comments: true
categories: [Andorid,Activity,生命周期]
---
####前言
经过两天的努力，搭建了Octopress,虽然有点老，但是感觉还不错，比起csdn这个超级方便，起码没有讨厌的广告，在我眼前晃来晃去，写起文章来，有种极客范；以前很少写文章，但是很多东西总是会慢慢忘记，就再此做个记录吧。开始今天的话题，Activity作为Android四大组件之一，是使用最为频繁的一种组件。下面主页解析一下Activity生命周期中的一些问题

####一、生命周期方法
![Activity生命周期图](http://pic001.cnblogs.com/img/tea9/201008/2010080516521645.png)<br>
***onCreate：***Activity生命周期中第一个方法，我们可以进行一些初始化工作，比如加载布局和数据等<br>
***onStart：***Activity已经可见了，但是还没有出现在前台，无法进行交互<br>
***onResume:***Activity出现在前台，用户可以与其进行交互<br>
***onPause:***Activity正在停止,在这个方法里主页不能进行耗时的处理，否则可能会影响新Activity的启动<br>
***onStop:***Activity即将停止<br>
***onRestart:***表示Activity正在重新启动，当当前Activity从不可见重新变为可见时调用。一般启动新的Activity又回来，会调动，或者按home键到桌面，又回到此Activity时调用。<br>
***onDestroy:***Activity正在销毁，一般点击返回键时触发<br>

####二、Activity常见情形
* 第一次启动，onCreate->onStart->onResume
* 当启动新Activity或者点击Home回到桌面时，会继续调用onPause->onStop,但是当启动新的Activity是透明主题时，不会调用onStop方法
*  当用户回到Activity时，会继续调用onRestart->onStart->onResume
*  当用户点击返回键时，onPause->onStop->onDestory

####三、特别说明
* onPause和onResume,onStop和onStart是配对的，看起来onPause和onStop，onStart和onResume方法的含义是差不多的，其实这几个方法的描述重点不用，onPause和onResume是描述是否可以进行交互；而onStart和onStop来描述是否可见。
* 启动新的Activity时，onPause先进行调用，才会调用新的Activity的生命周期