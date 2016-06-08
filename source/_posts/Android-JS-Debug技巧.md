
title: Android JS Debug技巧

date: 2015-08-27 14:34:22

tags: [Android, JS, Debug]

---

本文主要是讲解Android开发中在WebView中与JS交互时Debug的技巧。
## 调试步骤

1. Android版本必需是4.4及以上
2. 打开Android设备调试状态
3. 在使用WebView的代码中加入以下代码,从而开启调试状态。  可参看如下  
4. Chrome 30及以上版本才支持Android设备调试
5. 在Chrome地址栏，输入“about:inspect”或通过“菜单”->“工具”->“检查设备”打开设备检查页面,DevTools工具会自动检测已连接设备运行的可调试页面列表，点击对应页面的“inspect”链接打开调试页面。

<!--more-->

6. 在页面中，可以做如下：

	* 在Elements下查看DOM结构
	* 选中DOM元素后，在设备上会高亮显示，右侧Styles下修改CSS属性可即时生效：
	* 在Sources下断点调试JavaScript
	* 在Console中可运行调试（例如在Android中注入一个java对象别名为："app"）,里面有方法test(). 此时可调用app.test()来直接调试。非常方便


开启调试状态

```
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT){
     WebView.setWebContentsDebuggingEnabled(true);
   }
```



## 比较坑的地方

1. 点击"inspect"时，如果遇到启动了一个白屏界面，说明要翻墙才能使用。一般情况下，只在第一次使用"inspect"时需要翻墙，以后会缓存在本地。
2. 一定要打开调试状态且版本要>=4.4 还要检查chrome版本

## 其它

* 前端也可以用HBuilder来配合调试应用
* 应用启动后则可通过Chrome的DevTools工具连接进行调试。
 