---
layout: post
title: "Activity生命周期（二）"
date: 2016-08-01 22:10:35 +0800
comments: true
categories: [Andorid,Activity,生命周期]
---
####前言
这篇文章主要介绍下，异常情况Activity的生命周期，在这里说明这里的异常情况不是我们程序崩溃，而是指当系统配置发生变化或者内存不足时的情况。

####一、系统内存不足
先介绍一下Activity的优先级，如下:

* 前台Activity，可以与用户进行交互，优先级最高
* 可见但不能进行交互的Activity，例如弹框中的Activity
* 后台Activity，不可见，优先级最低

当系统的内存比较紧张时，会按照这个优先级来回收销毁Activity，在销毁Activity时，会调用onSaveInstance方法，保存当前Activity的状态

####二、系统配置发生变化
当应用启动时，系统会根据系统的配置加载资源，当系统配置发生变化时，默认情况下，Activity会被销毁并重新创建，当然我们也可以阻止其销毁。当系统配置发生变化后，Activity会执行方法onPause、onStop、onDestory方法；

####三、阻止系统配置变化引起的销毁重建
系统的配置有很多内容，当系统某项配置发生变化时我们不想Activity被销毁重建，我们可以在AndroidManifest.xml文件中设置Activity的configChanges属性，这样当此项配置发生变化时，就不会销毁重启了，而是调用Activity的onConfigurationChanged方法，举个🌰：

```
<activity android:name=".DemoActivity" android:configChanges="orientation|screenSize|keyboardHidden"/>
```
只要你不想某项配置的变化导致Activity的销毁重启，你就可以配置进去，多个可以使用"|"来增加

####三、数据的保存和恢复

如果我们想在Activity异常销毁前存储一些数据怎么搞呢；别担心，在异常销毁时，会调用Activity的onSaveInstanceState方法，在此方法中我们可以保存当前Activity的状态等，当在启动调用onCreate方法时，注意onCreate方法有个Bundle参数，这个参数带有在onSaveInstanceState方法中保存的数据（具体是不是一个对象就不知道了，O(∩_∩)O哈哈~），这是我们就可以恢复数据了，除此之外系统还会调用onRestoreInstanceState方法


***注意：***
当不是异常销毁重启时，并不会调用这个方法；在onCreate中使用参数恢复数据时，注意判断参数非空的情况，不是异常重启的情况，参数是为空的。

***调用时机：***
onSaveInstanceState是在onStop之前，具体是在onPause之前还是在onPause之后，这个不确定；
onRestoreInstanceState方法是在onStart方法之后；

***系统恢复数据：***
系统默认情况下，会帮我们保存Activity的状态，重启时恢复数据；在View中有和activity一样的方法onSaveInstanceState和onRestoreInstanceSate，具体保存那些数据可以看下其中的源码