title: "使用MediaRecoder和MediaPlayer实现简单录音播放软件"
id: 317
date: 2012-12-30 23:08:51
tags: 
categories: 
- android
---

<div>

如标题，讲解看注释，项目已经上传github，https://github.com/wheam/MediaRecorderTry

简单的录音播放软件，实现了一下功能：

<div>录制声音，播放声音，存储声音到SD卡，文件存储异常处理，部分防止错误操作的处理</div>
<!-- more -->
<div> </div>
<div>【主界面，中间的文字用于提示】</div>
<div>[![MediaRecoderTry1](http://wheam.me/wp-content/uploads/2012/12/MediaRecoderTry1.jpg)](http://wheam.me/?attachment_id=318)</div>
<div> </div>
<div>【恶意卖萌】</div>
<div>[![eyimaimeng](http://wheam.me/wp-content/uploads/2012/12/eyimaimeng.jpg)](http://wheam.me/?attachment_id=319)</div>
</div>
<div> </div>
<div> </div>
<div>参考了andorid文档和一部分博客，链接如下：</div>
<div>官方文档</div>
<div>[https://developer.android.com/reference/android/media/MediaRecorder.html](https://developer.android.com/reference/android/media/MediaRecorder.html)</div>
<div>[https://developer.android.com/reference/android/media/MediaPlayer.html](https://developer.android.com/reference/android/media/MediaPlayer.html)</div>
<div> </div>
<div>一个录音示例，只参照了使用部分</div>
<div>[http://blog.csdn.net/acu/article/details/7296998](http://blog.csdn.net/acu/article/details/7296998)</div>
<div> </div>
<div>判断文件存在</div>
<div>[http://www.eoeandroid.com/thread-67172-1-1.html](http://www.eoeandroid.com/thread-67172-1-1.html)</div>
<div> </div>
<div>判断文件夹是否存在</div>
<div>[http://blog.sina.com.cn/s/blog_6ffbcfdb0100qn3a.html](http://blog.sina.com.cn/s/blog_6ffbcfdb0100qn3a.html)</div>
<div> </div>
<div> </div>
<div> </div>
<div>首先是manifest中添加这两个:</div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #242424;"><span style="color: #99968b; font-style: italic;">1 </span><span style="color: #8ac6f2;">&lt;uses-permission</span> <span style="color: #cae682;">android:name=</span><span style="color: #95e454; font-style: italic;">"android.permission.WRITE_EXTERNAL_STORAGE"</span> <span style="color: #8ac6f2;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">2 </span><span style="color: #8ac6f2;">&lt;uses-permission</span> <span style="color: #cae682;">android:name=</span><span style="color: #95e454; font-style: italic;">"android.permission.RECORD_AUDIO"</span> <span style="color: #8ac6f2;">/&gt;</span></div>
<div> </div>
<div> </div>
<div> </div>
<div>接着还是XML布局：</div>
<div> </div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #000000;"><span style="color: #99968b; font-style: italic;">01 </span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">RelativeLayout</span> <span style="color: #f6f3e8;">xmlns:</span><span style="color: #f6f3e8;">android</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"http://schemas.android.com/apk/res/android"</span>
 <span style="color: #99968b; font-style: italic;">02 </span>    <span style="color: #f6f3e8;">xmlns:</span><span style="color: #f6f3e8;">tools</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"http://schemas.android.com/tools"</span>
 <span style="color: #99968b; font-style: italic;">03 </span>    <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_width</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span>
 <span style="color: #99968b; font-style: italic;">04 </span>    <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_height</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span> <span style="color: #f6f3e8;">&gt;</span>
 <span style="color: #f810b0;">05 </span>   <span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">EditText</span>
 <span style="color: #99968b; font-style: italic;">06 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">hint</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"输入文件名"</span>
 <span style="color: #99968b; font-style: italic;">07 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_width</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"fill_parent"</span>
 <span style="color: #99968b; font-style: italic;">08 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_height</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">09 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">id</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"@+id/edit_name"</span>
 <span style="color: #f810b0;">10 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_alignParentTop</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"true"</span>
 <span style="color: #99968b; font-style: italic;">11 </span>       <span style="color: #f6f3e8;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">12 </span>   <span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">Button</span>
 <span style="color: #99968b; font-style: italic;">13 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_width</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"fill_parent"</span>
 <span style="color: #99968b; font-style: italic;">14 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_height</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #f810b0;">15 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">id</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"@+id/bn_re"</span>
 <span style="color: #99968b; font-style: italic;">16 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_below</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"@id/edit_name"</span>
 <span style="color: #99968b; font-style: italic;">17 </span>       <span style="color: #f6f3e8;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">18 </span>   <span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">TextView</span>
 <span style="color: #99968b; font-style: italic;">19 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_width</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"fill_parent"</span>
 <span style="color: #f810b0;">20 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_height</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">21 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">id</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"@+id/info"</span>
 <span style="color: #99968b; font-style: italic;">22 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">textColor</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"#33b5e5"</span>
 <span style="color: #99968b; font-style: italic;">23 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_centerInParent</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"true"</span>
 <span style="color: #99968b; font-style: italic;">24 </span>       <span style="color: #f6f3e8;">/&gt;</span>
 <span style="color: #f810b0;">25 </span>   <span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">Button</span>
 <span style="color: #99968b; font-style: italic;">26 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_width</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"fill_parent"</span>
 <span style="color: #99968b; font-style: italic;">27 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_height</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">28 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">id</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"@+id/bn_play"</span>
 <span style="color: #99968b; font-style: italic;">29 </span>       <span style="color: #f6f3e8;">android:</span><span style="color: #f6f3e8;">layout_alignParentBottom</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"true"</span>
 <span style="color: #f810b0;">30 </span>       <span style="color: #f6f3e8;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">31 </span><span style="color: #f6f3e8;">&lt;/</span><span style="color: #f6f3e8;">RelativeLayout</span><span style="color: #f6f3e8;">&gt;</span></div>
<div> </div>
<div> </div>
<div> </div>
<div>具体代码实现：</div>
<div> </div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #000000;"><span style="color: #99968b; font-style: italic;">001 </span><span style="color: #f6f3e8;">package</span> <span style="color: #f6f3e8;">sssta</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">mediarecordertry</span>;
 <span style="color: #99968b; font-style: italic;">002 </span>
 <span style="color: #99968b; font-style: italic;">003 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">java.io.File</span>;
 <span style="color: #99968b; font-style: italic;">004 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">java.io.IOException</span>;
 <span style="color: #f810b0;">005 </span>
 <span style="color: #99968b; font-style: italic;">006 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.media.MediaPlayer</span>;
 <span style="color: #99968b; font-style: italic;">007 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.media.MediaRecorder</span>;
 <span style="color: #99968b; font-style: italic;">008 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.os.Bundle</span>;
 <span style="color: #99968b; font-style: italic;">009 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.os.Environment</span>;
 <span style="color: #f810b0;">010 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.app.Activity</span>;
 <span style="color: #99968b; font-style: italic;">011 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.Menu</span>;
 <span style="color: #99968b; font-style: italic;">012 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.View</span>;
 <span style="color: #99968b; font-style: italic;">013 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.View.OnClickListener</span>;
 <span style="color: #99968b; font-style: italic;">014 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.Button</span>;
 <span style="color: #f810b0;">015 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.EditText</span>;
 <span style="color: #99968b; font-style: italic;">016 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.TextView</span>;
 <span style="color: #99968b; font-style: italic;">017 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.Toast</span>;
 <span style="color: #99968b; font-style: italic;">018 </span>
 <span style="color: #99968b; font-style: italic;">019 </span><span style="color: #8ac6f2;">public</span> <span style="color: #8ac6f2;">class</span> <span style="color: #f6f3e8;">MainActivity</span> <span style="color: #8ac6f2;">extends</span> <span style="color: #f6f3e8;">Activity</span> <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">020 </span>
 <span style="color: #99968b; font-style: italic;">021 </span>    <span style="color: #99968b; font-style: italic;">//定义了一些必要的控件和参数</span>
 <span style="color: #99968b; font-style: italic;">022 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">Button</span> <span style="color: #f6f3e8;">bn_re</span>;
 <span style="color: #99968b; font-style: italic;">023 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">Button</span> <span style="color: #f6f3e8;">bn_play</span>;
 <span style="color: #99968b; font-style: italic;">024 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">EditText</span> <span style="color: #f6f3e8;">edit_name</span>;
 <span style="color: #f810b0;">025 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">TextView</span> <span style="color: #f6f3e8;">info</span>;
 <span style="color: #99968b; font-style: italic;">026 </span>
 <span style="color: #99968b; font-style: italic;">027 </span>    <span style="color: #99968b; font-style: italic;">//定义了几个String，path为存储路径，name为文件名，需要输入</span>
 <span style="color: #99968b; font-style: italic;">028 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #8ac6f2;">final</span> <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path_ex</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"/mnt/sdcard/MediaRecorderTry"</span>;
 <span style="color: #99968b; font-style: italic;">029 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #8ac6f2;">final</span> <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path_bh</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">".3gp"</span>;
 <span style="color: #f810b0;">030 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path</span>;
 <span style="color: #99968b; font-style: italic;">031 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">name</span>;
 <span style="color: #99968b; font-style: italic;">032 </span>
 <span style="color: #99968b; font-style: italic;">033 </span>    <span style="color: #99968b; font-style: italic;">//定义录制和播放最主要的两个实例，MediaPlayer,MediaRecoder</span>
 <span style="color: #99968b; font-style: italic;">034 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">MediaPlayer</span> <span style="color: #f6f3e8;">mediaPlayer</span>;
 <span style="color: #f810b0;">035 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #f6f3e8;">MediaRecorder</span> <span style="color: #f6f3e8;">mediaRecorder</span>;
 <span style="color: #99968b; font-style: italic;">036 </span>
 <span style="color: #99968b; font-style: italic;">037 </span>    <span style="color: #f6f3e8;">@Override</span>
 <span style="color: #99968b; font-style: italic;">038 </span>    <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onCreate</span>(<span style="color: #f6f3e8;">Bundle</span> <span style="color: #f6f3e8;">savedInstanceState</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">039 </span>        <span style="color: #8ac6f2;">super</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">onCreate</span>(<span style="color: #f6f3e8;">savedInstanceState</span>);
 <span style="color: #f810b0;">040 </span>        <span style="color: #f6f3e8;">setContentView</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">activity_main</span>);
 <span style="color: #99968b; font-style: italic;">041 </span>
 <span style="color: #99968b; font-style: italic;">042 </span>        <span style="color: #f6f3e8;">createSDCardDir</span>();
 <span style="color: #99968b; font-style: italic;">043 </span>
 <span style="color: #99968b; font-style: italic;">044 </span>        <span style="color: #99968b; font-style: italic;">//必要的初始化</span>
 <span style="color: #f810b0;">045 </span>        <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">Button</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">bn_play</span>);
 <span style="color: #99968b; font-style: italic;">046 </span>        <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">Button</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">bn_re</span>);
 <span style="color: #99968b; font-style: italic;">047 </span>        <span style="color: #f6f3e8;">edit_name</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">EditText</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">edit_name</span>);
 <span style="color: #99968b; font-style: italic;">048 </span>        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">TextView</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">info</span>);
 <span style="color: #99968b; font-style: italic;">049 </span>
 <span style="color: #f810b0;">050 </span>        <span style="color: #99968b; font-style: italic;">//预先设置三个控件的text，也作为控制的状态变量</span>
 <span style="color: #99968b; font-style: italic;">051 </span>        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"猛敲上面录制声音"</span>);
 <span style="color: #99968b; font-style: italic;">052 </span>        <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"开始录制"</span>);
 <span style="color: #99968b; font-style: italic;">053 </span>        <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"播放声音"</span>);
 <span style="color: #99968b; font-style: italic;">054 </span>
 <span style="color: #f810b0;">055 </span>        <span style="color: #99968b; font-style: italic;">//设置录制按键的点击事件</span>
 <span style="color: #99968b; font-style: italic;">056 </span>        <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOnClickListener</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">OnClickListener</span>() <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">057 </span>            <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(<span style="color: #f6f3e8;">View</span> <span style="color: #f6f3e8;">v</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">058 </span>                <span style="color: #99968b; font-style: italic;">// TODO Auto-generated method stub</span>
 <span style="color: #99968b; font-style: italic;">059 </span>
 <span style="color: #f810b0;">060 </span>                <span style="color: #99968b; font-style: italic;">//在bn_re显示“开始录制”时触发录制事件</span>
 <span style="color: #99968b; font-style: italic;">061 </span>                <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">equals</span>(<span style="color: #95e454; font-style: italic;">"开始录制"</span>))
 <span style="color: #99968b; font-style: italic;">062 </span>                <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">063 </span>                    <span style="color: #99968b; font-style: italic;">//防止EditText为空导致录入空文件名</span>
 <span style="color: #99968b; font-style: italic;">064 </span>                    <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">edit_name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">()!=</span><span style="color: #8ac6f2;">null</span><span style="color: #f6f3e8;">&amp;&amp;</span><span style="color: #f6f3e8;">edit_name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">length</span><span style="color: #f6f3e8;">()!=</span><span style="color: #e5786d;">0</span>)
 <span style="color: #f810b0;">065 </span>                    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">066 </span>                        <span style="color: #99968b; font-style: italic;">//定义个临时变量来存储EditText中的name</span>
 <span style="color: #99968b; font-style: italic;">067 </span>                        <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">name_temp</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">edit_name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span>();
 <span style="color: #99968b; font-style: italic;">068 </span>                        <span style="color: #99968b; font-style: italic;">//设置TextView显示现在的状态</span>
 <span style="color: #99968b; font-style: italic;">069 </span>                        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"录制中。。。文件名为:"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">name_temp</span>);
 <span style="color: #f810b0;">070 </span>                        <span style="color: #99968b; font-style: italic;">//将临时name赋值给name</span>
 <span style="color: #99968b; font-style: italic;">071 </span>                        <span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">name_temp</span>;
 <span style="color: #99968b; font-style: italic;">072 </span>                        <span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">path_ex</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">path_bh</span>;
 <span style="color: #99968b; font-style: italic;">073 </span>                        <span style="color: #99968b; font-style: italic;">//如果已经存在这个文件了，则提示直接播放</span>
 <span style="color: #99968b; font-style: italic;">074 </span>                        <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">fileIsExists</span>(<span style="color: #f6f3e8;">path</span>))
 <span style="color: #f810b0;">075 </span>                        <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">076 </span>                            <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"此文件已经存在，现在可以直接播放，但是就是不能录音"</span>);
 <span style="color: #99968b; font-style: italic;">077 </span>                            <span style="color: #f6f3e8;">edit_name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">""</span>);
 <span style="color: #99968b; font-style: italic;">078 </span>                        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">079 </span>                        <span style="color: #8ac6f2;">else</span>
 <span style="color: #f810b0;">080 </span>                        <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">081 </span>                            <span style="color: #99968b; font-style: italic;">//看此方法定义，开始录制声音</span>
 <span style="color: #99968b; font-style: italic;">082 </span>                            <span style="color: #f6f3e8;">Record</span>();
 <span style="color: #99968b; font-style: italic;">083 </span>                            <span style="color: #99968b; font-style: italic;">//bn_re改变状态</span>
 <span style="color: #99968b; font-style: italic;">084 </span>                            <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"停止录制"</span>);
 <span style="color: #f810b0;">085 </span>                            <span style="color: #99968b; font-style: italic;">//设置播放声音按键不可点击，防止出错</span>
 <span style="color: #99968b; font-style: italic;">086 </span>                            <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setClickable</span>(<span style="color: #8ac6f2;">false</span>);
 <span style="color: #99968b; font-style: italic;">087 </span>                        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">088 </span>
 <span style="color: #99968b; font-style: italic;">089 </span>                    <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">090 </span>                    <span style="color: #8ac6f2;">else</span>
 <span style="color: #99968b; font-style: italic;">091 </span>                    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">092 </span>                        <span style="color: #99968b; font-style: italic;">//如果EditText是空的，显示提醒--！</span>
 <span style="color: #99968b; font-style: italic;">093 </span>                        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"你是不是傻啊？你是傻吧！没有文件名啊！"</span>);
 <span style="color: #99968b; font-style: italic;">094 </span>                    <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">095 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">096 </span>                <span style="color: #99968b; font-style: italic;">//在bn_re显示“停止录制”时触发停止录制事件</span>
 <span style="color: #99968b; font-style: italic;">097 </span>                <span style="color: #8ac6f2;">else</span> <span style="color: #cae682;">if</span>(<span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">equals</span>(<span style="color: #95e454; font-style: italic;">"停止录制"</span>))
 <span style="color: #99968b; font-style: italic;">098 </span>                <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">099 </span>                    <span style="color: #99968b; font-style: italic;">//看此方法定义，停止录制声音</span>
 <span style="color: #f810b0;">100 </span>                    <span style="color: #f6f3e8;">StopRecord</span>();
 <span style="color: #99968b; font-style: italic;">101 </span>                    <span style="color: #99968b; font-style: italic;">//TextView显示现在的状态</span>
 <span style="color: #99968b; font-style: italic;">102 </span>                    <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"录制完毕，文件名："</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">name</span>);
 <span style="color: #99968b; font-style: italic;">103 </span>                    <span style="color: #99968b; font-style: italic;">//bn_re改变状态</span>
 <span style="color: #99968b; font-style: italic;">104 </span>                    <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"开始录制"</span>);
 <span style="color: #f810b0;">105 </span>                    <span style="color: #99968b; font-style: italic;">//设置播放按键可以点击</span>
 <span style="color: #99968b; font-style: italic;">106 </span>                    <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setClickable</span>(<span style="color: #8ac6f2;">true</span>);
 <span style="color: #99968b; font-style: italic;">107 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">108 </span>            <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">109 </span>        <span style="color: #f6f3e8;">});</span>
 <span style="color: #f810b0;">110 </span>        <span style="color: #99968b; font-style: italic;">//设置播放按键的点击事件</span>
 <span style="color: #99968b; font-style: italic;">111 </span>        <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOnClickListener</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">OnClickListener</span>() <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">112 </span>            <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(<span style="color: #f6f3e8;">View</span> <span style="color: #f6f3e8;">v</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">113 </span>                <span style="color: #99968b; font-style: italic;">// TODO Auto-generated method stub</span>
 <span style="color: #99968b; font-style: italic;">114 </span>                <span style="color: #99968b; font-style: italic;">//如果路径不为空，也就是已经有录制的声音了，就直接播放</span>
 <span style="color: #f810b0;">115 </span>                <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">!=</span><span style="color: #8ac6f2;">null</span><span style="color: #f6f3e8;">&amp;&amp;</span><span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">length</span><span style="color: #f6f3e8;">()!=</span><span style="color: #e5786d;">0</span>)
 <span style="color: #99968b; font-style: italic;">116 </span>                <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">117 </span>                    <span style="color: #99968b; font-style: italic;">//如果bn_play显示为播放声音</span>
 <span style="color: #99968b; font-style: italic;">118 </span>                    <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">equals</span>(<span style="color: #95e454; font-style: italic;">"播放声音"</span>))
 <span style="color: #99968b; font-style: italic;">119 </span>                    <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">120 </span>                        <span style="color: #99968b; font-style: italic;">//播放声音，查看此方法定义</span>
 <span style="color: #99968b; font-style: italic;">121 </span>                        <span style="color: #f6f3e8;">Play</span>();
 <span style="color: #99968b; font-style: italic;">122 </span>                        <span style="color: #99968b; font-style: italic;">//一样的提示和更改按钮状态</span>
 <span style="color: #99968b; font-style: italic;">123 </span>                        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"别理我，播放声音ing"</span>);
 <span style="color: #99968b; font-style: italic;">124 </span>                        <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"停止播放"</span>);
 <span style="color: #f810b0;">125 </span>                        <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setClickable</span>(<span style="color: #8ac6f2;">false</span>);
 <span style="color: #99968b; font-style: italic;">126 </span>                    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">127 </span>                    <span style="color: #8ac6f2;">else</span> <span style="color: #cae682;">if</span>(<span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">equals</span>(<span style="color: #95e454; font-style: italic;">"停止播放"</span>))
 <span style="color: #99968b; font-style: italic;">128 </span>                    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">129 </span>                        <span style="color: #99968b; font-style: italic;">//停止播放声音，查看此方法定义</span>
 <span style="color: #f810b0;">130 </span>                        <span style="color: #f6f3e8;">StopPlay</span>();
 <span style="color: #99968b; font-style: italic;">131 </span>                        <span style="color: #99968b; font-style: italic;">//一样的提示和更改按钮状态</span>
 <span style="color: #99968b; font-style: italic;">132 </span>                        <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"现在已经有声音，播放请敲下面，重新录制敲上面"</span>);
 <span style="color: #99968b; font-style: italic;">133 </span>                        <span style="color: #f6f3e8;">bn_play</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"播放声音"</span>);
 <span style="color: #99968b; font-style: italic;">134 </span>                        <span style="color: #f6f3e8;">bn_re</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setClickable</span>(<span style="color: #8ac6f2;">true</span>);
 <span style="color: #f810b0;">135 </span>                    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">136 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">137 </span>                <span style="color: #99968b; font-style: italic;">//如果路径为空，也就是没有录制的声音了，则提示</span>
 <span style="color: #99968b; font-style: italic;">138 </span>                <span style="color: #8ac6f2;">else</span>
 <span style="color: #99968b; font-style: italic;">139 </span>                <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">140 </span>                    <span style="color: #99968b; font-style: italic;">//设置提醒</span>
 <span style="color: #99968b; font-style: italic;">141 </span>                    <span style="color: #f6f3e8;">info</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setText</span>(<span style="color: #95e454; font-style: italic;">"你是不是傻啊？你是傻吧！还没有录制声音啊！"</span>);
 <span style="color: #99968b; font-style: italic;">142 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">143 </span>            <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">144 </span>        <span style="color: #f6f3e8;">});</span>
 <span style="color: #f810b0;">145 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">146 </span>
 <span style="color: #99968b; font-style: italic;">147 </span>    <span style="color: #99968b; font-style: italic;">//录制声音的方法</span>
 <span style="color: #99968b; font-style: italic;">148 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">Record</span>()
 <span style="color: #99968b; font-style: italic;">149 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">150 </span>        <span style="color: #99968b; font-style: italic;">//建立实例</span>
 <span style="color: #99968b; font-style: italic;">151 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">MediaRecorder</span>();
 <span style="color: #99968b; font-style: italic;">152 </span>        <span style="color: #99968b; font-style: italic;">//设置声音获取的方式，此处为麦克风</span>
 <span style="color: #99968b; font-style: italic;">153 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setAudioSource</span>(<span style="color: #f6f3e8;">MediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">AudioSource</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">MIC</span>);
 <span style="color: #99968b; font-style: italic;">154 </span>        <span style="color: #99968b; font-style: italic;">//设置声音的封装格式</span>
 <span style="color: #f810b0;">155 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOutputFormat</span>(<span style="color: #f6f3e8;">MediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">OutputFormat</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">THREE_GPP</span>);
 <span style="color: #99968b; font-style: italic;">156 </span>        <span style="color: #99968b; font-style: italic;">//设置声音的编码格式</span>
 <span style="color: #99968b; font-style: italic;">157 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setAudioEncoder</span>(<span style="color: #f6f3e8;">MediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">AudioEncoder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">AMR_NB</span>);
 <span style="color: #99968b; font-style: italic;">158 </span>        <span style="color: #99968b; font-style: italic;">//为path赋值</span>
 <span style="color: #99968b; font-style: italic;">159 </span>
 <span style="color: #f810b0;">160 </span>        <span style="color: #99968b; font-style: italic;">//设置声音的保存路径</span>
 <span style="color: #99968b; font-style: italic;">161 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOutputFile</span>(<span style="color: #f6f3e8;">path</span>);
 <span style="color: #99968b; font-style: italic;">162 </span>        <span style="color: #99968b; font-style: italic;">//try-catch语句是为了捕捉异常</span>
 <span style="color: #99968b; font-style: italic;">163 </span>        <span style="color: #8ac6f2;">try</span> <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">164 </span>            <span style="color: #99968b; font-style: italic;">//准备和播放，拟写者两句，系统会自动提示捕捉异常</span>
 <span style="color: #f810b0;">165 </span>            <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">prepare</span>();
 <span style="color: #99968b; font-style: italic;">166 </span>            <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">start</span>();
 <span style="color: #99968b; font-style: italic;">167 </span>            <span style="color: #99968b; font-style: italic;">//下面是两个异常，自动添加</span>
 <span style="color: #99968b; font-style: italic;">168 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">IllegalStateException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">169 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #f810b0;">170 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #99968b; font-style: italic;">171 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">IOException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">172 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #99968b; font-style: italic;">173 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #99968b; font-style: italic;">174 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">175 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">176 </span>    <span style="color: #99968b; font-style: italic;">//停止录制方法</span>
 <span style="color: #99968b; font-style: italic;">177 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">StopRecord</span>()
 <span style="color: #99968b; font-style: italic;">178 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">179 </span>        <span style="color: #99968b; font-style: italic;">//首先是要stop，其次是要reset，此时MediaRecoder回复到初始化状态</span>
 <span style="color: #f810b0;">180 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">stop</span>();
 <span style="color: #99968b; font-style: italic;">181 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">reset</span>();
 <span style="color: #99968b; font-style: italic;">182 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">183 </span>
 <span style="color: #99968b; font-style: italic;">184 </span>    <span style="color: #8ac6f2;">protected</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">OnStop</span>()
 <span style="color: #f810b0;">185 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">186 </span>        <span style="color: #99968b; font-style: italic;">//在程序退出时释放MediaRecoder和MediaPlayer两个资源</span>
 <span style="color: #99968b; font-style: italic;">187 </span>        <span style="color: #f6f3e8;">mediaRecorder</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">release</span>();
 <span style="color: #99968b; font-style: italic;">188 </span>        <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">release</span>();
 <span style="color: #99968b; font-style: italic;">189 </span>        <span style="color: #8ac6f2;">super</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">onStop</span>();
 <span style="color: #f810b0;">190 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">191 </span>    <span style="color: #99968b; font-style: italic;">//播放声音方法</span>
 <span style="color: #99968b; font-style: italic;">192 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">Play</span>()
 <span style="color: #99968b; font-style: italic;">193 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">194 </span>        <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">MediaPlayer</span>();
 <span style="color: #f810b0;">195 </span>        <span style="color: #99968b; font-style: italic;">//try-catch语句是为了捕捉异常</span>
 <span style="color: #99968b; font-style: italic;">196 </span>        <span style="color: #8ac6f2;">try</span> <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">197 </span>            <span style="color: #99968b; font-style: italic;">//首先设置播放路径，之后准备并且开始播放</span>
 <span style="color: #99968b; font-style: italic;">198 </span>            <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setDataSource</span>(<span style="color: #f6f3e8;">path</span>);
 <span style="color: #99968b; font-style: italic;">199 </span>            <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">prepare</span>();
 <span style="color: #f810b0;">200 </span>            <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">start</span>();
 <span style="color: #99968b; font-style: italic;">201 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">IllegalArgumentException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">202 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #99968b; font-style: italic;">203 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #99968b; font-style: italic;">204 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">SecurityException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">205 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #99968b; font-style: italic;">206 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #99968b; font-style: italic;">207 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">IllegalStateException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">208 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #99968b; font-style: italic;">209 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #f810b0;">210 </span>        <span style="color: #f6f3e8;">}</span> <span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">IOException</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">211 </span>            <span style="color: #99968b; font-style: italic;">// TODO Auto-generated catch block</span>
 <span style="color: #99968b; font-style: italic;">212 </span>            <span style="color: #f6f3e8;">e</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">printStackTrace</span>();
 <span style="color: #99968b; font-style: italic;">213 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">214 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">215 </span>    <span style="color: #99968b; font-style: italic;">//停止播放声音方法</span>
 <span style="color: #99968b; font-style: italic;">216 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">StopPlay</span>()
 <span style="color: #99968b; font-style: italic;">217 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">218 </span>        <span style="color: #99968b; font-style: italic;">//首先是要stop，其次是要reset，此时MediaRecoder回复到初始化状态</span>
 <span style="color: #99968b; font-style: italic;">219 </span>        <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">stop</span>();
 <span style="color: #f810b0;">220 </span>        <span style="color: #f6f3e8;">mediaPlayer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">reset</span>();
 <span style="color: #99968b; font-style: italic;">221 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">222 </span>    <span style="color: #99968b; font-style: italic;">//判断是否存在此文件</span>
 <span style="color: #99968b; font-style: italic;">223 </span>    <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">boolean</span> <span style="color: #cae682;">fileIsExists</span>(<span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">224 </span>        <span style="color: #8ac6f2;">try</span><span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">225 </span>                   <span style="color: #f6f3e8;">File</span> <span style="color: #f6f3e8;">f</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">File</span>(<span style="color: #f6f3e8;">path</span>);
 <span style="color: #99968b; font-style: italic;">226 </span>                <span style="color: #8ac6f2;">if</span><span style="color: #f6f3e8;">(!</span><span style="color: #f6f3e8;">f</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">exists</span>())
 <span style="color: #99968b; font-style: italic;">227 </span>                <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">228 </span>                    <span style="color: #8ac6f2;">return</span> <span style="color: #8ac6f2;">false</span>;
 <span style="color: #99968b; font-style: italic;">229 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">230 </span>
 <span style="color: #99968b; font-style: italic;">231 </span>        <span style="color: #f6f3e8;">}</span><span style="color: #8ac6f2;">catch</span> (<span style="color: #f6f3e8;">Exception</span> <span style="color: #f6f3e8;">e</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">232 </span>                <span style="color: #99968b; font-style: italic;">// TODO: handle exception</span>
 <span style="color: #99968b; font-style: italic;">233 </span>                <span style="color: #8ac6f2;">return</span> <span style="color: #8ac6f2;">false</span>;
 <span style="color: #99968b; font-style: italic;">234 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">235 </span>        <span style="color: #8ac6f2;">return</span> <span style="color: #8ac6f2;">true</span>;
 <span style="color: #99968b; font-style: italic;">236 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">237 </span>    <span style="color: #99968b; font-style: italic;">//如果程序文件夹不存在，创建一个新的文件夹</span>
 <span style="color: #99968b; font-style: italic;">238 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">createSDCardDir</span><span style="color: #f6f3e8;">(){</span>
 <span style="color: #99968b; font-style: italic;">239 </span>        <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">Environment</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">MEDIA_MOUNTED</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">equals</span>(<span style="color: #f6f3e8;">Environment</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getExternalStorageState</span><span style="color: #f6f3e8;">())){</span>
 <span style="color: #f810b0;">240 </span>               <span style="color: #99968b; font-style: italic;">// 创建一个文件夹对象，赋值为外部存储器的目录</span>
 <span style="color: #99968b; font-style: italic;">241 </span>                <span style="color: #f6f3e8;">File</span> <span style="color: #f6f3e8;">sdcardDir</span> <span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">Environment</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getExternalStorageDirectory</span>();
 <span style="color: #99968b; font-style: italic;">242 </span>              <span style="color: #99968b; font-style: italic;">//得到一个路径，内容是sdcard的文件夹路径和名字</span>
 <span style="color: #99968b; font-style: italic;">243 </span>                <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">sdcardDir</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getPath</span><span style="color: #f6f3e8;">()+</span><span style="color: #95e454; font-style: italic;">"/MediaRecorderTry"</span>;
 <span style="color: #99968b; font-style: italic;">244 </span>                <span style="color: #f6f3e8;">File</span> <span style="color: #f6f3e8;">path1</span> <span style="color: #f6f3e8;">=</span> <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">File</span>(<span style="color: #f6f3e8;">path</span>);
 <span style="color: #f810b0;">245 </span>               <span style="color: #8ac6f2;">if</span> <span style="color: #f6f3e8;">(!</span><span style="color: #f6f3e8;">path1</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">exists</span>()) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">246 </span>                <span style="color: #99968b; font-style: italic;">//若不存在，创建目录，可以在应用启动的时候创建</span>
 <span style="color: #99968b; font-style: italic;">247 </span>                <span style="color: #f6f3e8;">path1</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">mkdirs</span>();
 <span style="color: #99968b; font-style: italic;">248 </span>              <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">249 </span>               <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">250 </span>        <span style="color: #8ac6f2;">else</span><span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">251 </span>            <span style="color: #8ac6f2;">return</span>;
 <span style="color: #99968b; font-style: italic;">252 </span>       <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">253 </span>       <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">254 </span>
 <span style="color: #f810b0;">255 </span><span style="color: #f6f3e8;">}</span></div>
<div> </div>
<div> </div>