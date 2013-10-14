title: "【android studio】project默认jdk设置错误"
id: 409
date: 2013-07-04 01:46:58
tags: 
categories: 
- android
---

好吧，请叫我“天天偷懒好久不更新博客渣渣君”，好嘛，好不容易更新还出个错误解决是不是很令人失望啊。。。其实不是的！！！我已经准备了好多好多，好多好多的经验要分享，偷懒没写而已嘛。。。

今天重装了win7（没SSD了好忧伤），编译project的时候总是报错，研究了一下，发现是project default setting的设置错误。

报错情况：

Cannot run program "C:\Program Files\Java\jdk1.7.0_04\bin\java" (in directory "C:\Program Files (x86)\Android\android-studio\system\compile-server"): CreateProcess error=2, ϵͳÕҲ»µ½ָ¶

因为我更想了jdk到1.7.0_25，但是android studio是默认导入的，so必须在启动的时候设置project default setting。
<!-- more -->

【步骤1】打开欢迎画面，选择configure   

[![1](http://wheam.me/wp-content/uploads/2013/07/1.jpg)](http://wheam.me/wp-content/uploads/2013/07/1.jpg)

【步骤2】进入Project Defaults

[![2](http://wheam.me/wp-content/uploads/2013/07/2.jpg)](http://wheam.me/wp-content/uploads/2013/07/2.jpg)

【步骤3】选择project structure，选择sdks

[![3](http://wheam.me/wp-content/uploads/2013/07/3.jpg)](http://wheam.me/wp-content/uploads/2013/07/3.jpg)

【步骤4】选择你的jdk版本，然后选择你的jdk home path，只要选择对了，再次编译项目就OK了

[![4](http://wheam.me/wp-content/uploads/2013/07/4.jpg)](http://wheam.me/wp-content/uploads/2013/07/4.jpg)

&nbsp;

&nbsp;

&nbsp;