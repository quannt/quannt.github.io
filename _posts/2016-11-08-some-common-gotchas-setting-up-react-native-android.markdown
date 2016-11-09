---
layout: post
title: "Some common gotchas when setting up React Native development environment for Android"
categories: [programming, react]
fullview: false
---

The [Getting Started](https://facebook.github.io/react-native/docs/getting-started.html) guide for React Native is simple and easy to follow. However, I met 2 problems that after some googling I found out that it also happens to a lot of 
people. So here goes my attempt to save the humanity. Please note that I use a Windows machine.


### 1)  Unable to upload some APKs
The error looks something like this 
{% highlight %}
* What went wrong:
Execution failed for task ':app:installDebug'.
> com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: Unable to upload some APKs
 {% endhighlight %}

Apparently the gradle version used by React Native 1.3.1 does not work pretty well, you have to downgrade it to a lower version. To fix this,
browse to `{Your project path}\android\` and open `build.gradle`.

Replace 

{% highlight json %}
classpath 'com.android.tools.build:gradle:1.3.1'
 {% endhighlight %}

with 
{% highlight json %}
classpath 'com.android.tools.build:gradle:1.2.3'
 {% endhighlight %}
 
### 2) Could not connect to development server
![alt text](http://i.imgur.com/P4COBEN.png "Error screenshot")

Well the problem is rather obvious here, your mobile device cannot connect to the React native packager on port 8081. 
If you look carefully at the screenshot, React Native actually tells you how to fix that.
* If your device is running on Android 5+, running `adb reverse tcp:8081 tcp:8081` fixes the problem. 
Before doing this, please make sure you have added `Android\sdk\platform-tools` to your local Path environment. The absolute path is `C:\Users\Quan\AppData\Local\Android\sdk\platform-tools` on my Windows machine.
* If your device is running on Android lowser than 5, run `curl "http://localhost:8081/index.android.bundle?platform=android" -o "android/app/src/main/assets/index.android.bundle"`

