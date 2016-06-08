title: Android应用开发中的夜间模式实现（一）
date: 2014-09-11 00:01:42
tags: [Android,theme,技术]

---
### 前言
在应用开发中会经常遇到要求实现`夜间模式`或者`主题切换`
具体例子如下，我会先讲解第一种方法。
<!--more-->
### 夜间模式

1. 知乎
2. 网易新闻
3. 沪江开心词场
4. Pocket

### 主题切换

1. 腾讯QQ
2. 新浪微博

我今天主要是详述第一种的实现方式：

1. 首先，应用的Application要继承自定义的Theme
	```
	<application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
       	android:label="@string/app_name"
        android:theme="@style/AppTheme">
    </application>
    ```
    
2. 其实AppTheme要实现日间和夜间两种Theme
		
		```
		<style name="AppTheme"/>
		
    	<style name="AppTheme.Light">
        <item name="root_background">@color/white</item>
    	</style>

    	<style name="AppTheme.Dark">
        <item name="root_background">@color/black</item>
   		</style>
		
		```


3. 在自定义属性attr.xml中添加如下：

		```
		<declare-styleable name="Theme">
        	<attr name="root_background" format="reference|color" />
        </declare-styleable>
        ```

4. 在layout中引用自定义属性

	```
		<?xml version="1.0" encoding="utf-8"?>
		<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    	android:layout_width="fill_parent"
    	android:layout_height="fill_parent"
    	android:background="?attr/root_background">
    	</RelativeLayout>
    	```
    	
5. 代码中设置切换：所有Activity都继承BaseThemeActivity,将是否夜间模式的bool值保存在SharedPreferences中切换 SharedPreferences 中 is_night_mode 的值 ，然后调用 restartActivity()重启当前Activity方法即可切换Theme.

```
		@Override
    	protected void onCreate(Bundle savedInstanceState) {
        	setTheme();
        	super.onCreate(savedInstanceState);
   		 }   
   		private void setTheme() {
       		 mCurrentThemeResourceID = getThemeResourceID();
       		 setTheme(mCurrentThemeResourceID);
    		}

  			  private int getThemeResourceID() {
      	  	mIsNightModule = PreferencesUtils.getBoolean(this, getResources().getString(R.string.is_night_mode));
       		 return mIsNightModule ? R.style.AppTheme_Dark : R.style.AppTheme_Light;
   		 }

   		 public static void restartActivity(final Activity activity) {
       		if (activity == null) return;
        	final int enter_anim = android.R.anim.fade_in;
        	final int exit_anim = android.R.anim.fade_out;
        	activity.overridePendingTransition(enter_anim, exit_anim);
       		activity.finish();
       		activity.overridePendingTransition(enter_anim, exit_anim);
        	activity.startActivity(activity.getIntent());
   		 }

   			private final boolean isThemeChanged() {
      	  		return getThemeResourceID() != mCurrentThemeResourceID;
    		}

    		@Override
   			protected void onResume() {
        	super.onResume();
        	if (isThemeChanged()) {
     	       restartActivity(this);
      	 		 }
   			 }
   		``` 
   		 
 #### 总结
 
 1. 其中要注意的是setTheme()方法一定要在super.onCreate(savedInstanceState);之前调用即可
 2. 可用此方式实现多种theme的切换
 
 以后会发博文讲解如何实现主题下载，主题切换等功能。
 
 如果想和我讨论，请在下面评论即可。
 
 ![avatar icon](/img/HeadPicture.jpg)