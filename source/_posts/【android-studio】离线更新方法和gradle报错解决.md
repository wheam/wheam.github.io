title: "【android studio】离线更新方法和gradle报错解决"
id: 423
date: 2013-07-08 23:55:13
tags: 
categories: 
- android
---

andorid studio确实是个坑啊，不建议新童鞋在preview版本的时候用于开发，除非你跟楼主一样纯粹为了好看 本文主要解决一些坑的，看了很久很久很久很久的stackoverflow加上各种搜索，现在整理一下解决办法。

【离线更新】

ps.由于android studio不提供0.1.9 - 0.2.0的升级包，so，你可以重新装了，0.2.0之后的更新下述应该还是可以使用的。

<!-- more -->

参考文章：[http://my.oschina.net/aquar/blog/141334](http://my.oschina.net/aquar/blog/141334)

其实这就是个坑啊，android studio 使用help - update会报网络连接错误，具体情况请看这篇帖子，上面有google工程师出来解决，不过反正我是字太多没怎么看

[https://code.google.com/p/android/issues/detail?id=56068](https://code.google.com/p/android/issues/detail?id=56068)

1、首先打开此链接[http://tools.android.com/recent](http://tools.android.com/recent)，或者[https://dl.google.com/android/studio/patches/updates.xml](https://dl.google.com/android/studio/patches/updates.xml) ，找到最新的android studio版本，并且记住版本号，如0.1.9（Build number： 130.72944）

2、然后打开你的android studio    - help - About，查看目前的build number

3、修改此链接

http://dl.google.com/android/studio/patches/AI-(此处填入你现在的版本的build number)-（此处填入你需要升级到的）-patch-win（这里对应windows）.jar

打开此链接即可下载更新包

4、进入你的android studio安装根目录，输入以下命令更新，注意此时关闭你的android studio

java -classpath  AI-(此处填入你现在的版本的build number)-（此处填入你需要升级到的）-patch-win（这里对应windows）.jar  com.intellij.updater.Runner install .

最后的点不能省略，代表安装到当前目录

5、打开android studio  -help - About 查看目前版本号，更新则代表升级成功

&nbsp;

【gradle报错】

参考链接：

[http://www.cnblogs.com/timeng/archive/2013/05/17/3084185.html](http://www.cnblogs.com/timeng/archive/2013/05/17/3084185.html)

[http://stackoverflow.com/questions/16585869/android-studio-error-installing-gradle/16600297#16600297](http://stackoverflow.com/questions/16585869/android-studio-error-installing-gradle/16600297#16600297)

[http://stackoverflow.com/questions/16727505/android-studio-project-structure-not-coming-properly/16943005#16943005](http://stackoverflow.com/questions/16727505/android-studio-project-structure-not-coming-properly/16943005#16943005)

[http://stackoverflow.com/questions/16586674/failed-to-import-new-gradle-project-in-android-studio/16620500#16620500](http://stackoverflow.com/questions/16586674/failed-to-import-new-gradle-project-in-android-studio/16620500#16620500)

这个问题主要表现形式为安装报错或者是安装成功之后新建的project编译不能进行，idea提示报错（导入原来的eclipse项目似乎不会遇到此问题），原因是gradle环境未安装，idea好像因为被墙不能自动下载gradle

1、先通过此链接或者其他方式下载gradle [http://download.csdn.net/detail/zjilvufe/5395393](http://download.csdn.net/detail/zjilvufe/5395393)

2、解压到任意的位置，并且将其bin的路径加入到系统环境Path中

3、完成之后在cmd中输入gradle -v验证是否成功