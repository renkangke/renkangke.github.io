
 title: Android Studio Gradle编译时『No resource found that matches the given name』解决方法（windows系统的坑）
 
 date: 2015-07-14 22:25:22
 
 tags: [Android Studio,gradle,windows] 

---
* 最近帮团队同事配置gradle时，发现一个非常奇怪的问题：
> * 同样的gradle配置的项目，只是修改了一个项目的名称，竟然会出现以下奇怪问题：


## 现象
1. 一个编译完全OK，另外一个编译出现各种问题
2. mac上两个都能正常编译，windows上其中一个编译通不过



<!--more-->


主要Error如下：

```
No resource found that matches the given name (at drawable with value @drawable/xxxxxxxxxxxxxxxxxxxxxxxx)
```
我们做了各种排除，最后发现问题的特点：
两个项目，一个可编译，一个编译失败，区别在于，文件路径不一样长，文件名不一样长，最后由这个想到了是不是这个原因了，mac没有类似限制……
不查不知道啊，一查吓一跳：

 
## 原因


> `windows限制路径字符长度最大值：  260……`[查看这是为何](http://stackoverflow.com/questions/1880321/why-does-the-260-character-path-length-limit-exist-in-windows)
因为gradle编译时引用 build目录下拉取的aar路径层次太深，超过260，编译时不通过,这就是为什么很同用windows的同学，会在配置完全OK的情况下编译失败.

 
## 解决方案：
1. 整个项目挪到根目录，项目文件名改短，暂时解决这个问题
2. 换用Linux系统 或者Mac电脑


## 结束语

windows真是坑啊，这个问题和Android的65536现象极为相似啊！！！




