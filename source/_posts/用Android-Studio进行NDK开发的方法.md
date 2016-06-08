 title: 用Android Studio进行NDK开发的方法 
 
 date: 2016-01-22 22:25:25 
 
 tags: [Android, NDK] 

---
> 以前大家都是用Eclipse进行NDK开发，特别麻烦，要使用各种命令，现在Android Studio也支持NDK项目的创建和开发了，并且还对调试进行了支持，使用起来简单方便，但需要使用Gradle针对Android Studio插件的实验版本。本篇文章将会教你学会如何用Android Studio创建一个使用C/C++开发的NDK项目。


<!--more-->


## 第一步：创建项目
> ps:本人讲解全部以Mac为主，还请Windows和Linux童鞋见谅。

1. 直接New Project创建一个项目。
2. “File” > “Settings” > “Build, Execution, Deployment” > “Build Tools” > “Gradle”. Select “Use Default Gradle wrapper (recommended)”, 点击 “OK”. 


## 第二步：设置Gradle

1. Google为使Android Studio 支持 NDK开发，专门研发了针对Andorid Stuido的gradle插件的实验版：'com.android.tools.build:gradle-experimental:0.4.0'。请你在整个工程的根目录下的build.gradle文件中将：
```
classpath 'com.android.tools.build:gradle:xxxx'
```	
	改为：
	
classpath 'com.android.tools.build:gradle-experimental:0.4.0'
		
2. 将你工程根目录下 “Gradle Scripts” > “gradle-wrapper.properties (Gradle Version)” 打开并修改为:

		distributionUrl=https\://services.gradle.org/distributions/gradle-2.4-all.zip
		
	to:

		distributionUrl=https\://services.gradle.org/distributions/gradle-2.8-all.zip
		
3. 你的工程下的主module下的build.gradle应该有一部分是这样的

		apply plugin: 'com.android.application'
		
		android {
		   compileSdkVersion 23
		   buildToolsVersion "23.0.1"
		
		   defaultConfig {
		       applicationId "com.renkangke.ndk"
		       minSdkVersion 22
		       targetSdkVersion 23
		       versionCode 1
		       versionName "1.0"
		   }
		   buildTypes {
		       release {
		           minifyEnabled false
		           proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
		       }
		   }
		}
		
	将上面的一部分修改为下面的代码（请细心注意中间的一些变化，不细心看不出来哈）：	

		apply plugin: 'com.android.model.application'
		
		model {
		   android {
		       compileSdkVersion = 23
		       buildToolsVersion = "23.0.1"
		
		       defaultConfig.with {
		           applicationId = "com.renkangke.ndk"
		           minSdkVersion.apiLevel = 22
		           targetSdkVersion.apiLevel = 23
		           versionCode = 1
		           versionName = "1.0"
		       }
		   }
		   android.buildTypes {
		       release {
		           minifyEnabled = false
		           proguardFiles.add(file('proguard-android.txt'))
		       }
		   }
		}
		
## 第三步：配置NDK开发环境

1. 去Android官网下载NDK包:[点我点我点我去下载](http://developer.android.com/intl/zh-cn/ndk/downloads/index.html)，[不会翻墙的点我](http://itxs.co/s/c349exav)，你懂的。
2. 然后“File” > “Project Structure” > “SDK Location”, “Android NDK Location” and click “...”. 添加好你的NDK地址，这个页面也可以直接下载。 
3. 在第二步第3点中的build.gradle下面再继续添加(renkangke-ndk-name是你的ndk so library的名字，你可以换成自己的)

		android.ndk {
		    moduleName = "renkangke-ndk-name"
		}

## 第四步：添加JNI代码

1. 在你的MainActivity中添加如下代码：

		static {
		       System.loadLibrary("renkangke-ndk-name");
		}
		public native String getStringFromJNI();// 请注意这里是native方法
		
2. 光标选择方法 “getStringFromJNI()”. 通过option + return快捷键提示，自动生成jni方法.
3. 这样会在java文件夹同一目录自动生成jni目录

		#include <jni.h>

		JNIEXPORT jstring JNICALL
		Java_com_renkangke_ndk_MainActivity_getStringFromJNI(JNIEnv *env, jobject instance) {
		
		   // 注意：上面的方法名是 Java+包名+类名+方法名，中间用下划线分割
		   // 两个参数是默认固定参数
		  
		   return (*env)->NewStringUTF(env, returnValue);
		}
4. 修改returnValue为"Hello, I am from jni."
5. 然后将MainActiviy中的getStringFromJNI()的值打印出来，即为从c/c++中返回的值。

#### OK，此时你就可以随心所欲地用C/C++开发你的Android程序了

ps: 当你build后，so文件就生成在build目录下。



