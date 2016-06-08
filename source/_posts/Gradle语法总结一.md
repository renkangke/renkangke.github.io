title: Gradle语法总结一

date: 2015-09-20 22:06:10

tags: [Android, Gradle]

---

## 序

> 当前的构建工作中，`Gradle`是非常火的一个。
> `Google`更是将其与`Android Studio`一起打包，强推。

## build

>依据不同的工序，处理你的代码，将之处理成你想要的各种东西


## 年代史

`Ant` --> `Maven` --> `Gradle`

<!-- more -->

## 根基

> `Maven`编译规则是用`XML`来编写
> `Gradle`基础是`Groovy`语言、这个必须掌握
> `Groovy`基于`Java`并且**拓展**
> `Groovy`是一门动态语言

## Groovy倒底是什么？

**Groovy是在 java平台上的、 具有像Python， Ruby 和 Smalltalk 语言特性的灵活动态语言， Groovy保证了这些特性像 Java语法一样被 Java开发者使用** 

## 待续（这个题目内容还是蛮多的）

## Basic Setting

* `build.gradle`配置

```
buildscript {
 repositories {
     jcenter() 
 }
 dependencies {
     classpath 'com.android.tools.build:gradle:1.2.3'
 } 
}

```

> 其中 `jcenter()`是默认中央`maven`库.
> 下面`dependencies`中的`classpath`是编译时`gradle`的版本


* Android Application

```
apply plugin: 'com.android.application'
```

* Android Library

```
apply plugin: 'com.android.library'
```

* Maven

```
apply plugin: 'maven'
```

> 以上其中 `apply plugin`是应用的插件，第一个是`Android`应用，第二个是`Android`库，第三个是当使用了`maven`的语法，需要引入它

* 编译SDK相关配置

```
android {
 compileSdkVersion 22
 buildToolsVersion "22.0.1"
}
```

* Project Structure

```
app
├── build.gradle
├── settings.gradle
└── app
    ├── build.gradle
    ├── build
    ├── libs
    └── src
        └── main
            ├── java
            │   └── com.package.myapp
            └── res
                ├── drawable
                ├── layout
                └── etc.

```

* `Gradle Wrapper`结构

```
app/
 ├── gradlew 
 ├── gradlew.bat
 └── gradle/wrapper/
     ├── gradle-wrapper.jar
     └── gradle-wrapper.properties
```




