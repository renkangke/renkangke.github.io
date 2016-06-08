title: 记一个ViewPager-with-Fragments-inside-PopupWindow-的坑
date: 2015-05-06 11:54:32 
tags: [技术,Android]

---


记一个ViewPager-with-Fragments-inside-PopupWindow-的坑。


当时实现的一个场景是：在popupwindow中需要实现一个可以左右滑动很多页的viewpager


<!--more-->


ViewPager with Fragments inside PopupWindow (or DialogFragment) 

会出现一个错误： Error no view found for id for fragment

我跟到最后，发现这是Android系统的一个bug， 最终的解决方案是不要用fragment在popupwindow中
可以换成view在pageadaper中，这样就可以了

这个坑当时我查阅了很久，在github和stackoverflow上也都有人问类似问题，特记录于此