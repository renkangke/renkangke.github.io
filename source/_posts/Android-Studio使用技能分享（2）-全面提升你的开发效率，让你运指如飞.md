title:  Android Studio使用技能分享（2）--全面提升你的开发效率，让你运指如飞   

date:  2015-05-21 16:35:31  

tags:  [Android Stuio, 技术]

---

去年我曾经做过一个这方面的分享，现在我再次做关于Android Studio 使用技能分享的第二季，我为什么做这个分享呢，因为我们每天工作的大部分时间就是在代码中傲游，自从Android Studio出来后，它极大地提高了我们的效率，小明多次说过，提升效率就是提升生产力！ 

想不再加班吗？想早点回家吗？想单位时间内价值最大吗？ 
这些还是值得你花点时间去熟练的，正所谓磨刀不误砍柴工。 

正如我的副标题所说：全面提升你的开发效率，让你运指如飞。 

下面会介绍很多的快捷方法，我今天会讲很多技巧，但你不需要立刻全部记住，只是让大家先了解有这些方法，有个初步的印象，然后后面在使用中再一步步熟悉它们。

<!-- more -->


 

### 回顾上一次的部分分享
#### Android Studio相对Eclipse 比较赞的一些特性

 1. 颜色、图片在布局和代码中可以实时预览
 2. string可以实时预览
 3. 多屏预览、截图带有设备框，可随时录制模拟器视频
 4. 可以直接打开文件所在位置 
 5. 跨工程移动、搜索、跳转
 5. 自动保存，无需一直Ctrl + S
 6. 即使文件关闭依然可以回退N个历史
 7. 智能重构、智能预测报错
 8. 每一行文件编辑历史，可追溯到人 10、各种插件
 9. 例如ADB、.gitignore、sql、markdown、
 10. 图片可直接转.9图片,并且自带.9编辑

第一步，我们先讲搜索，而搜索这个功能向来非常重要，无论是一个网站、一个应用，还是一个IDE，对它们来说，搜索都是至关重要的。

### 1、Search & Navigation

#### 各种快速查找  
* Cmd + O                            ---- 查找Class  
       * 连按两次的技巧  
       * Option的功能  
* Cmd + Shift + O                    ---- 查找文件 比如Resource Assets等  
* Cmd + Option + O                  ----  查找方法和变量  

* Partial Matching 局部匹配  
* MainActivity —> Mi  
* Mi:22  
* Cmd + L  
#### 不知道命令快捷键时怎么办？
*  Cmd + Shift + A            ---- 查找命令  
*  Double Shift                 ---- Search everywhere  
#### 最近文件历史，方便快速切换： 
*  Cmd + E                      ----  Recently opened files   
* Cmd + Shift + E         ----   Recently edited files   
*  cmd+alt+left/right       ----  Navigate Back/Forward   
*  cmd+shift+backspace ---- Last Edit Location   
#### 关于方法和引用的快捷键：
*  Cmd + F12                    ---- Class Members

*  Option + F7                  ----查找哪里引用了该方法
*  Cmd + Option + F7     ---- 列出引用的列表

 * Ctrl + H                        ----  显示层级结构
 * Ctrl + Option + H       ----    显示所有调用方法的地方
#### 强大而方便的全局查找：
* Cmd + F                     ----    在当前文件内查找 
* Cmd + Shift + F           ----  在全局内查找（可自定义）
#### 一些其它常用快捷键：
* Cmd + B                    ----    跳转到申明
* Cmd + Option + B       ----  跳转到实现

* Cmd + U 跳转到超类 
* Cmd +Shift + B           ----  类型申明     
* Cmd +Ctrl +  ↑           ----   相关联的文件

* Cmd + P                   ----    方法参数
* Ctrl + J                    ----     方法文档

### 2、Refactor 
#### 一些基础功能：

* Ctrl + T                   ----      重构
* Shift + F6                 ----    重命名
* F5   copy
* Cmd + D                  ----    复制当前行在下方
* Cmd + delete             ---- 安全删除
* Cmd + Shift +   ↑       ---- 上下移动当前行 或者方法
* Cmd + Shift + Enter  ----  补全
#### 我比较喜欢的几个功能：
* F6   move
* Ctrl + Shift + J          ----  合并行
* Shift + Enter            ----   换行
* Option + F1              ----  选择菜单    

#### 非常赞建议必须记住的几个快捷键：

* Cmd + Option + V   ----  Variable
* Cmd + Option + C   ----  Constant
* Cmd + Option + F    ---- Field
* Cmd + Option + P   ----  Parameter
* Cmd + Option + M   ---- Member

* Cmd + Option + T    ---- Surround with
#### 其它的一些不错的功能
* Cmd + N                   New
* Option + Enter          
* Find and replace code duplicates 

### 3、Debug 
* Run / Debug / Step  ……（常用功能我就不啰嗦了）
* Attach Debugger              建议自己设定快捷键

* Toggle Breakpoints           Cmd+F8               
* Conditional Breakpoints  
* Logging Breakpoints   
* Temporary Breakpoints  
* Disable Breakpoints
* Evaluate Expression


### 4、Other
* a、布局预览
xmlns:tools="http://schemas.android.com/tools"
tools:visibility=“invisible"
* b、自动导入：Preferences -> Editor -> Auto Import -> Java
* c、Navigation Menu
* d、 Setting up the Android Studio Proxy
Android Studio supports HTTP proxy settings so you can run Android Studio behind a firewall or secure network.
* e、use github & gist & sh are project on Github
* f、 Live Templates
* g、生成Doc文档
* h、plugin (eg: .gitignore)
* i、analyze （eg: Inspect code Android lint)
* j、bookmark 
* k、Presentation Mode
* Option + F1             ----    选择菜单    

### 附带分享神人： Luis von Ahn
略

PS: PPT附件见评论……

