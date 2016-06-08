title: The Solution of Android Studio Generate JavaDoc Error  
 
date: 2015-05-13 20:35:00   
 
tags: [Android Studio, Android, 技术] 

---

Maybe sometimes you need user Android Studio to generate Java Documents, you Can use Android Studio —> Tools —> Generate JavaDoc to do it, but basically you will get error.

<!-- more -->

Maybe sometimes you need user Android Studio to generate Java Documents, you Can use Android Studio —> Tools —> Generate JavaDoc to do it, but basically you will get error.

There has two cases:

One: nullPointError

You can add command in Other Command Line like this:

      -bootclasspath /Users/'Your user name'/android-sdks/platforms/android-22/android.jar 

The Other One:

Garbled：

You can add command behind the above command like this:

    -encoding utf-8 -charset utf-8

