title:  Study of JNI 
 
date:  2014-10-29 00:12:03

tags: [JNI,Android]

---
Form [Android SDK](http://developer.android.com),It says about `JNI` like this: 

> 
JNI is the Java Native Interface. It defines a way for managed code (written in the Java programming language) to interact with native code (written in C/C++). It's vendor-neutral, has support for loading code from dynamic shared libraries, and while cumbersome at times is reasonably efficient.

<!-- more -->

I am not too familiar with it, so I will read through the Java Native Interface Specification to get a sense for how JNI works and what features are available. 

#### JavaVM and JNIEnv

JNI defines two key data structures, "JavaVM" and "JNIEnv". Both of these are essentially pointers to pointers to function tables. (In the C++ version, they're classes with a pointer to a function table and a member function for each JNI function that indirects through the table.) The JavaVM provides the "invocation interface" functions, which allow you to create and destroy a JavaVM. In theory you can have multiple JavaVMs per process, but Android only allows one.

The JNIEnv provides most of the JNI functions. Your native functions all receive a JNIEnv as the first argument.

The JNIEnv is used for thread-local storage. For this reason, you cannot share a JNIEnv between threads. If a piece of code has no other way to get its JNIEnv, you should share the JavaVM, and use GetEnv to discover the thread's JNIEnv. (Assuming it has one; see AttachCurrentThread below.)

The C declarations of JNIEnv and JavaVM are different from the C++ declarations. The "jni.h" include file provides different typedefs depending on whether it's included into C or C++. For this reason it's a bad idea to include JNIEnv arguments in header files included by both languages. (Put another way: if your header file requires #ifdef __cplusplus, you may have to do some extra work if anything in that header refers to JNIEnv.)

