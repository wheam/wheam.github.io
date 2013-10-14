title: "【开源框架】android-async-http stdudy note"
id: 418
date: 2013-07-08 23:28:00
tags: 
categories: 
- android
---

首先容我先挖个坑，先只讲怎么用，后面有时间会把这个开源框架的研究补上。

android-async-http 的主页：[http://loopj.com/android-async-http/](http://loopj.com/android-async-http/)

android-async-http 的github主页：[https://github.com/loopj/android-async-http](https://github.com/loopj/android-async-http)

本文参考博客之一：[http://blog.csdn.net/tangren03/article/details/8824081](http://blog.csdn.net/tangren03/article/details/8824081)

其实关于所有的使用方法，在此框架的主页上都有了，只不过是英文的，我来点流行的步骤分解法，三步即可使用此框架
<!-- more -->

【步骤1】通过主页或者github下载最新的jar

//这一步实在太简单了，不说了

【步骤2】将jar作为lib加入到你的项目中

//此处详解android studio中导入jar作为lib的方法

1、将你的jar拷贝到项目文件夹的lib下

[![asynchttp1](http://wheam.me/wp-content/uploads/2013/07/asynchttp1.jpg)](http://wheam.me/wp-content/uploads/2013/07/asynchttp1.jpg)

&nbsp;

2、打开file - project structure-libs 确认jar已经导入，否则使用“+”号添加

[![asynchttp2](http://wheam.me/wp-content/uploads/2013/07/asynchttp2.jpg)](http://wheam.me/wp-content/uploads/2013/07/asynchttp2.jpg)

3、打开你的build.gradle，在dependencies加入如下代码

&nbsp;

<div class="source" style="font-family: undefined; color: #dcdccc; background-color: #3f3f3f;"><span style="color: #dcdccc;">dependencies</span> <span style="color: #dcdccc;">{</span>
    <span style="color: #dcdccc;">compile</span> <span style="color: #efef8f;">files</span>(<span style="color: #dcdccc;">'</span><span style="color: #dcdccc;">libs</span><span style="color: #dcdccc;">/</span><span style="color: #dcdccc;">android</span><span style="color: #dcdccc;">-</span><span style="color: #dcdccc;">support</span><span style="color: #dcdccc;">-</span><span style="color: #dcdccc;">v4</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">jar</span><span style="color: #dcdccc;">'</span>)
    <span style="color: #dcdccc;">compile</span> <span style="color: #efef8f;">files</span>(<span style="color: #dcdccc;">'</span><span style="color: #dcdccc;">libs</span><span style="color: #dcdccc;">/</span><span style="color: #dcdccc;">android</span><span style="color: #dcdccc;">-</span><span style="color: #dcdccc;">async</span><span style="color: #dcdccc;">-</span><span style="color: #dcdccc;">http</span><span style="color: #dcdccc;">-</span><span style="color: #c0bed1;">1.4</span><span style="color: #dcdccc;">.</span><span style="color: #8cd0d3;">3</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">jar</span><span style="color: #dcdccc;">'</span>)
    <span style="color: #7f9f7f;">//或者你想偷懒可以只写下面这一句，所有libs文件下的jar都会被编译</span>
    <span style="color: #dcdccc;">compile</span> <span style="color: #efef8f;">fileTree</span>(<span style="color: #dfcfaf; text-decoration: underline;">dir:</span> <span style="color: #cc9393;">"libs"</span><span style="color: #dcdccc;">,</span> <span style="color: #dfcfaf; text-decoration: underline;">include:</span> <span style="color: #cc9393;">"*.jar"</span>)

<span style="color: #dcdccc;">}</span>

</div>

4、在你的项目文件夹下通过命令行clean你的gradlew，如不完成，代码虽然能编译，但是会报NoClassDefFoundError，输入以下命令即可

windows   "你的android studio安装路径\sdk\tools\templates\gradle\wrapper\gradlew.bat" clean  记住路径的双引号是必须的

Mac： 在项目文件夹下使用命令 ./gradlew clean

[![asynchttp3](http://wheam.me/wp-content/uploads/2013/07/asynchttp3.jpg)](http://wheam.me/wp-content/uploads/2013/07/asynchttp3.jpg)

5、重新打开你的android studio

【步骤3】拟写十分简单的代码即可使用

<div class="source" style="font-family: undefined; color: #dcdccc; background-color: #3f3f3f;"><span style="color: #7f9f7f;">//使用AsyncHttpClient类新建其实例</span>
        <span style="color: #dcdccc;">AsyncHttpClient</span> <span style="color: #dcdccc;">asyncHttpClient</span> <span style="color: #dcdccc;">=</span> <span style="color: #e3ceab;">new</span> <span style="color: #dcdccc;">AsyncHttpClient</span>();
        <span style="color: #7f9f7f;">//使用AsyncHttpClient的get方法，其中传入的第一个参数为你需要get的url，这里的url是跑不了的，第二个参数为控制句柄</span>
        <span style="color: #7f9f7f;">//根据实际情况可以选择不一样的对象来对应不同的使用场景，具体类型参照主页说明，这里只演示返回json的</span>
        <span style="color: #dcdccc;">asyncHttpClient</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">get</span>(<span style="color: #cc9393;">"http://wheam.me"</span><span style="color: #dcdccc;">,</span><span style="color: #e3ceab;">new</span> <span style="color: #dcdccc;">JsonHttpResponseHandler</span><span style="color: #dcdccc;">(){</span>
            <span style="color: #dcdccc;">@Override</span>
            <span style="color: #efefaf; font-weight: bold;">public</span> <span style="color: #dfdfbf; font-weight: bold;">void</span> <span style="color: #efef8f;">onSuccess</span>(<span style="color: #dcdccc;">JSONObject</span> <span style="color: #dcdccc;">jsonObject</span>) <span style="color: #dcdccc;">{</span>
                <span style="color: #7f9f7f;">//onSuccess是成功返回结果时候回调的，其中的参数即为返回结果</span>
                <span style="color: #efefaf; font-weight: bold;">super</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">onSuccess</span>(<span style="color: #dcdccc;">jsonObject</span>);
                <span style="color: #e3ceab;">try</span> <span style="color: #dcdccc;">{</span>
                    <span style="color: #dcdccc;">textview</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">setText</span>(<span style="color: #dcdccc;">jsonObject</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">getString</span>(<span style="color: #cc9393;">"title"</span>));
                <span style="color: #dcdccc;">}</span> <span style="color: #e3ceab;">catch</span> (<span style="color: #dcdccc;">JSONException</span> <span style="color: #dcdccc;">e</span>) <span style="color: #dcdccc;">{</span>
                    <span style="color: #dcdccc;">e</span><span style="color: #dcdccc;">.</span><span style="color: #efef8f;">printStackTrace</span>();
                <span style="color: #dcdccc;">}</span>
            <span style="color: #dcdccc;">}</span>
        <span style="color: #dcdccc;">});</span></div>