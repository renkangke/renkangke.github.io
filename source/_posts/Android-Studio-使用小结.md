title: Android Studio 使用小结
date: 2014-09-15 00:22:51
tags: [Android,Android Studio,技术]

---

从去年（2013年5月）Google发布Android Studio 0.1.0版本，到如今已经一年多了,已经升级到0.8.6 Beta版 ，从刚开始大家报怨bug多，编译困难，到如今已经基本趋于稳定了，在这个过程中，我一直使用Android Studio,一年多的时间，团队里只有我一个人使用。
<!-- more -->
#### 1. Android Studio 使用Eclipse的项目结构
> 因为在一个团队里，目前十几个Android开发工程师，只有我一人用Android Studio,期间也推荐过很多同事用，但是大家都是下下来，导入项目，然后一堆问题，一时半会适应不了就放弃使用。看来我还是应该早点把在多看中买的一本`《布道之道》`看完，推荐布道能力弱爆了有没有。大家都不用这个开发，那我只好也使用Eclipse的项目结构喽！

1. 使用Open Project 打开Eclipse项目
2. 此时如果运行处没有此项目，即不显示运行按键
	a. 去Edit Configurations里 如果没有Application就新建一个
	b. 如果没有当前moudle就去moudle settings 里添加一个当前的moudle
	c. 再到Edit Configurations 添加好，如果显示Default Activity not found
	d. 那么就到moudle setting 里把source 里的src文件夹标记为Source
3. 继续添加其它依赖项目
4. 添加其它jar包

> `Tip`:如果实在觉得Android Studio 不够成熟、更新还是有些bug,可以考虑intellij idea社区版或者付费版，Android Studio本身就是基于它的，并且它更稳定。



#### 2. Android Studio自己的项目结构
> 目前这个结构用起来还不错，把src和res一起放在main目录下，可以选择ant、maven、gradle来构建、官方默认推荐的是gradle。可以直接使用maven库中的library。使用起来比较简单，网上资料也比较多。就不多说了。

#### 3. Android Studio 快捷键及一些使用技巧
> Android Studio 快捷键有很多种，可以选Eclipse的，也可以先Mac OS 10.5+ ，当然自定义那是必须的。这些快捷键一定要熟练，文件检索功能非常强大！能够极大的提升工作效率，这也是我为什么一直在用它的原因！


