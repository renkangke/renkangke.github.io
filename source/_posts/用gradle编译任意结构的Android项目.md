
 title: 用gradle编译任意结构的Android项目 
 
 date: 2015-07-02 00:30:58 
 
 tags: [gradle，Android] 

---
* 前记：所欲记者甚多，后续增之。

## 需求

* 继续用`Eclipse`项目的结构，但是使用`gradle`编译，或者说任意的项目结构进行编译。



<!--more-->


## 解决方案

1. Android studio的项目结构
	1. Android Studio 整个项目是一个project
	2. Project中有很多的module
	3. module的类型可以有很多种，比如com.android.application是可以运行的应用程序、com.android.library是Android项目的library.
2. Gradle的编译原则
	1. 每个project目录下放置两个重要的配置文件
		1. settings.gradle. 此文件是配置project中有的module定论的。有多个个module，路径是什么，叫什么名字
		2. build.gradle 此文件是配置gradle全局的一些设置，比如maven库的路径、
	2. 每个module目录下都有一个build.gradle
		1. 此文件会标记比如当前是什么类型（参见1.3）
		2. 签名混淆打多渠道包以及
		3. 包依赖
		4. 其它的gradle task
		
<!--more-->
3. 配置过程
	1. 在project中创建settings.gradle、build.gradle两个文件
		1. project中的settings.gradle中添加如下类型,对应为当前目录路径，其中用`:`分隔。下面代码中module0表示当前根目录中有一个module0,module1表示在根目录下aaa文件夹下bbb文件夹下的module1,根目录下ddd文件夹下的module2。
	
	```
	include 'module0'
	include 'aaa:bbb:module1'
	include 'ddd:module2'

	```
			
		2. build.gradle中填入如下
		
	
	```
	
	buildscript {
    repositories {
        mavenLocal()

        maven {
            url "http://your own maven url"
        }
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.2.3'
    }
}
allprojects {
    repositories {
        mavenLocal()

        maven {
            url "http://your own maven url"
        }
        jcenter()
    }
}

	
	```
	2. 在module中添加build.gradle文件，这里可以配置以下内容设置文件夹：
	
	```
	sourceSets {
        main {
            java.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
            jni.srcDirs = ['jni']
            jniLibs.srcDirs = ['libs']
            manifest.srcFile 'AndroidManifest.xml'
        }
    }
    ```
	
	
	
4. 其它
	1. AndroidManifest.xml的合并
	2. 资源文件的合并
	3. lint的检查
	4. mutilDex
	5. uploadTask
	6. 依赖文件的添加
5. 未完待续


## Q&A

```
finished with non-zero exit value 3 

```
```
 
compile fileTree(dir: 'libs', include: ['*.jar'])换成provided fileTree(dir: 'libs', include: ['*.jar'])
  ```
 








