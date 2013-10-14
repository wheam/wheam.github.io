title: "使用dialog和ArrayAdapter实现简单记事软件"
id: 297
date: 2012-12-29 20:35:27
tags: 
categories: 
- android
---

<div>
<div>好吧，本来不应该是简单记事软件，但是这个就是类似记事软件，因为上篇的缘故，我还不能说是listview，真忧伤。</div>
<div>代码已经上传github，https://github.com/wheam/HelloVoice</div>
</div>
<div>因为功能难以界定，所以我会截图说明。</div>
<!-- more -->
<div>【主界面就是单纯的listview】</div>
<div>[![listviewmain](http://wheam.me/wp-content/uploads/2012/12/listviewmain1.jpg)](http://wheam.me/?attachment_id=310)</div>
<div>![](file:///C:/Users/wheam/AppData/Local/Temp/enhtmlclip/QQ截图20121229174334.jpg)</div>
<div>【点击添加按钮能够添加新的listview项】</div>
<div>[![listviewadd](http://wheam.me/wp-content/uploads/2012/12/listviewadd1.jpg)](http://wheam.me/?attachment_id=312)</div>
<div> </div>
<div>![](file:///C:/Users/wheam/AppData/Local/Temp/enhtmlclip/QQ截图20121229174323.jpg)</div>
<div>【长按删除单个item】</div>
<div>[![listviewde](http://wheam.me/wp-content/uploads/2012/12/listviewde1.jpg)](http://wheam.me/?attachment_id=311)</div>
<div> </div>
<div> </div>
<div>
<div>
<div>
<div>首先是activity布局文件：</div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #000000;"><span style="color: #99968b; font-style: italic;">01 </span><span style="color: #8ac6f2;">&lt;LinearLayout</span> <span style="color: #cae682;">xmlns:android=</span><span style="color: #95e454; font-style: italic;">"http://schemas.android.com/apk/res/android"</span>
 <span style="color: #99968b; font-style: italic;">02 </span>    <span style="color: #cae682;">xmlns:tools=</span><span style="color: #95e454; font-style: italic;">"http://schemas.android.com/tools"</span>
 <span style="color: #99968b; font-style: italic;">03 </span>    <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span>
 <span style="color: #99968b; font-style: italic;">04 </span>    <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span>
 <span style="color: #f810b0;">05 </span>    <span style="color: #cae682;">android:orientation=</span><span style="color: #95e454; font-style: italic;">"vertical"</span> <span style="color: #8ac6f2;">&gt;</span>
 <span style="color: #99968b; font-style: italic;">06 </span>
 <span style="color: #99968b; font-style: italic;">07 </span>    <span style="color: #8ac6f2;">&lt;LinearLayout</span>
 <span style="color: #99968b; font-style: italic;">08 </span>        <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">09 </span>        <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #f810b0;">10 </span>        <span style="color: #cae682;">android:orientation=</span><span style="color: #95e454; font-style: italic;">"horizontal"</span>
 <span style="color: #99968b; font-style: italic;">11 </span>        <span style="color: #cae682;">android:layout_gravity=</span><span style="color: #95e454; font-style: italic;">"center"</span>
 <span style="color: #99968b; font-style: italic;">12 </span>        <span style="color: #8ac6f2;">&gt;</span>
 <span style="color: #99968b; font-style: italic;">13 </span>           <span style="color: #8ac6f2;">&lt;TextView</span>
 <span style="color: #99968b; font-style: italic;">14 </span>            <span style="color: #cae682;">android:text=</span><span style="color: #95e454; font-style: italic;">"呵呵"</span>
 <span style="color: #f810b0;">15 </span>            <span style="color: #cae682;">android:textSize=</span><span style="color: #95e454; font-style: italic;">"25dip"</span>
 <span style="color: #99968b; font-style: italic;">16 </span>            <span style="color: #cae682;">android:textColor=</span><span style="color: #95e454; font-style: italic;">"#33b5e5"</span>
 <span style="color: #99968b; font-style: italic;">17 </span>            <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">18 </span>            <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">19 </span>            <span style="color: #cae682;">android:gravity=</span><span style="color: #95e454; font-style: italic;">"center"</span>
 <span style="color: #f810b0;">20 </span>            <span style="color: #8ac6f2;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">21 </span>        <span style="color: #8ac6f2;">&lt;Button</span>
 <span style="color: #99968b; font-style: italic;">22 </span>            <span style="color: #cae682;">android:text=</span><span style="color: #95e454; font-style: italic;">"添加"</span>
 <span style="color: #99968b; font-style: italic;">23 </span>            <span style="color: #cae682;">android:textColor=</span><span style="color: #95e454; font-style: italic;">"#33b5e5"</span>
 <span style="color: #99968b; font-style: italic;">24 </span>            <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #f810b0;">25 </span>            <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">26 </span>            <span style="color: #cae682;">android:id=</span><span style="color: #95e454; font-style: italic;">"@+id/bn_add"</span>
 <span style="color: #99968b; font-style: italic;">27 </span>
 <span style="color: #99968b; font-style: italic;">28 </span>            <span style="color: #8ac6f2;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">29 </span>    <span style="color: #8ac6f2;">&lt;/LinearLayout&gt;</span>
 <span style="color: #f810b0;">30 </span>
 <span style="color: #99968b; font-style: italic;">31 </span>    <span style="color: #8ac6f2;">&lt;ListView</span>
 <span style="color: #99968b; font-style: italic;">32 </span>        <span style="color: #cae682;">android:id=</span><span style="color: #95e454; font-style: italic;">"@+id/list"</span>
 <span style="color: #99968b; font-style: italic;">33 </span>        <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"fill_parent"</span>
 <span style="color: #99968b; font-style: italic;">34 </span>        <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #f810b0;">35 </span>     <span style="color: #8ac6f2;">&gt;</span>
 <span style="color: #99968b; font-style: italic;">36 </span>    <span style="color: #8ac6f2;">&lt;/ListView&gt;</span>
 <span style="color: #99968b; font-style: italic;">37 </span>
 <span style="color: #99968b; font-style: italic;">38 </span><span style="color: #8ac6f2;">&lt;/LinearLayout&gt;</span></div>
<div> </div>
<div> </div>
<div> </div>
<div>然后在res/layout下新建xml布局文件，命名为dialog.xml：</div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #000000;"><span style="color: #99968b; font-style: italic;">01 </span><span style="color: #e5786d;">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span>
 <span style="color: #99968b; font-style: italic;">02 </span><span style="color: #8ac6f2;">&lt;LinearLayout</span> <span style="color: #cae682;">xmlns:android=</span><span style="color: #95e454; font-style: italic;">"http://schemas.android.com/apk/res/android"</span>
 <span style="color: #99968b; font-style: italic;">03 </span>    <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span>
 <span style="color: #99968b; font-style: italic;">04 </span>    <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"match_parent"</span>
 <span style="color: #f810b0;">05 </span>    <span style="color: #cae682;">android:orientation=</span><span style="color: #95e454; font-style: italic;">"vertical"</span>
 <span style="color: #99968b; font-style: italic;">06 </span>    <span style="color: #cae682;">android:id=</span><span style="color: #95e454; font-style: italic;">"@+id/dialog"</span> <span style="color: #8ac6f2;">&gt;</span>
 <span style="color: #99968b; font-style: italic;">07 </span>   <span style="color: #8ac6f2;">&lt;EditText</span>
 <span style="color: #99968b; font-style: italic;">08 </span>       <span style="color: #cae682;">android:hint=</span><span style="color: #95e454; font-style: italic;">"输入声音描述"</span>
 <span style="color: #99968b; font-style: italic;">09 </span>       <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #f810b0;">10 </span>       <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">11 </span>       <span style="color: #cae682;">android:id=</span><span style="color: #95e454; font-style: italic;">"@+id/dialog_name"</span>
 <span style="color: #99968b; font-style: italic;">12 </span>       <span style="color: #cae682;">android:layout_gravity=</span><span style="color: #95e454; font-style: italic;">"center"</span>
 <span style="color: #99968b; font-style: italic;">13 </span>       <span style="color: #8ac6f2;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">14 </span>   <span style="color: #8ac6f2;">&lt;EditText</span>
 <span style="color: #f810b0;">15 </span>       <span style="color: #cae682;">android:hint=</span><span style="color: #95e454; font-style: italic;">"输入声音时间"</span>
 <span style="color: #99968b; font-style: italic;">16 </span>       <span style="color: #cae682;">android:layout_height=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">17 </span>       <span style="color: #cae682;">android:layout_width=</span><span style="color: #95e454; font-style: italic;">"wrap_content"</span>
 <span style="color: #99968b; font-style: italic;">18 </span>       <span style="color: #cae682;">android:id=</span><span style="color: #95e454; font-style: italic;">"@+id/dialog_time"</span>
 <span style="color: #99968b; font-style: italic;">19 </span>       <span style="color: #cae682;">android:inputType=</span><span style="color: #95e454; font-style: italic;">"number"</span>
 <span style="color: #f810b0;">20 </span>       <span style="color: #cae682;">android:layout_gravity=</span><span style="color: #95e454; font-style: italic;">"center"</span>
 <span style="color: #99968b; font-style: italic;">21 </span>       <span style="color: #8ac6f2;">/&gt;</span>
 <span style="color: #99968b; font-style: italic;">22 </span><span style="color: #8ac6f2;">&lt;/LinearLayout&gt;</span></div>
<div> </div>
<div> </div>
<div> </div>
<div>接着拟写Voice类：</div>
<div> </div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #000000;"><span style="color: #99968b; font-style: italic;">01 </span><span style="color: #f6f3e8;">package</span> <span style="color: #f6f3e8;">sssta</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">hellovoice</span>;
 <span style="color: #99968b; font-style: italic;">02 </span>
 <span style="color: #99968b; font-style: italic;">03 </span><span style="color: #8ac6f2;">public</span> <span style="color: #8ac6f2;">class</span> <span style="color: #f6f3e8;">Voice</span> <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">04 </span>
 <span style="color: #f810b0;">05 </span>    <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">name</span>;<span style="color: #99968b; font-style: italic;">//声音的描述</span>
 <span style="color: #99968b; font-style: italic;">06 </span>    <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">time</span>;<span style="color: #99968b; font-style: italic;">//声音长度</span>
 <span style="color: #99968b; font-style: italic;">07 </span>    <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">path</span><span style="color: #f6f3e8;">=</span><span style="color: #95e454; font-style: italic;">"/mnt/sdcard/seemore/"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">+</span><span style="color: #95e454; font-style: italic;">".mp3"</span>;<span style="color: #99968b; font-style: italic;">//直接使用name作为声音路径</span>
 <span style="color: #99968b; font-style: italic;">08 </span>
 <span style="color: #99968b; font-style: italic;">09 </span>    <span style="color: #99968b; font-style: italic;">//构造方法，传入一个string，一个int值，分别赋值给name和time</span>
 <span style="color: #f810b0;">10 </span>    <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">Voice</span>(<span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">a</span><span style="color: #f6f3e8;">,</span><span style="color: #cae682;">int</span> b)
 <span style="color: #99968b; font-style: italic;">11 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">12 </span>        <span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">a</span>;
 <span style="color: #99968b; font-style: italic;">13 </span>        <span style="color: #f6f3e8;">time</span><span style="color: #f6f3e8;">=</span>b;
 <span style="color: #99968b; font-style: italic;">14 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">15 </span><span style="color: #f6f3e8;">}</span></div>
<div> </div>
<div> </div>
<div> </div>
<div>再接着拟写activity：</div>
</div>
</div>
<div> </div>
</div>
<div> </div>
<div>![](file:///C:/Users/wheam/AppData/Local/Temp/enhtmlclip/QQ截图20121229174344.jpg)</div>
<div class="source" style="font-family: Consolas; color: #f6f3e8; background-color: #242424;"><span style="color: #99968b; font-style: italic;">001 </span><span style="color: #f6f3e8;">package</span> <span style="color: #f6f3e8;">sssta</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">hellovoice</span>;
 <span style="color: #99968b; font-style: italic;">002 </span>
 <span style="color: #99968b; font-style: italic;">003 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">java.util.ArrayList</span>;
 <span style="color: #99968b; font-style: italic;">004 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">java.util.List</span>;
 <span style="color: #f810b0;">005 </span>
 <span style="color: #99968b; font-style: italic;">006 </span>
 <span style="color: #99968b; font-style: italic;">007 </span>
 <span style="color: #99968b; font-style: italic;">008 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.os.Bundle</span>;
 <span style="color: #99968b; font-style: italic;">009 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.app.Activity</span>;
 <span style="color: #f810b0;">010 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.app.AlertDialog</span>;
 <span style="color: #99968b; font-style: italic;">011 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.app.Dialog</span>;
 <span style="color: #99968b; font-style: italic;">012 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.content.DialogInterface</span>;
 <span style="color: #99968b; font-style: italic;">013 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.content.SharedPreferences</span>;
 <span style="color: #99968b; font-style: italic;">014 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.LayoutInflater</span>;
 <span style="color: #f810b0;">015 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.Menu</span>;
 <span style="color: #99968b; font-style: italic;">016 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.View</span>;
 <span style="color: #99968b; font-style: italic;">017 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.View.OnClickListener</span>;
 <span style="color: #99968b; font-style: italic;">018 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.view.ViewGroup</span>;
 <span style="color: #99968b; font-style: italic;">019 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.AdapterView</span>;
 <span style="color: #f810b0;">020 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.AdapterView.OnItemLongClickListener</span>;
 <span style="color: #99968b; font-style: italic;">021 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.ArrayAdapter</span>;
 <span style="color: #99968b; font-style: italic;">022 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.Button</span>;
 <span style="color: #99968b; font-style: italic;">023 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.EditText</span>;
 <span style="color: #99968b; font-style: italic;">024 </span><span style="color: #f6f3e8;">import</span> <span style="color: #f6f3e8;">android.widget.ListView</span>;
 <span style="color: #f810b0;">025 </span>
 <span style="color: #99968b; font-style: italic;">026 </span><span style="color: #8ac6f2;">public</span> <span style="color: #8ac6f2;">class</span> <span style="color: #f6f3e8;">MainActivity</span> <span style="color: #8ac6f2;">extends</span> <span style="color: #f6f3e8;">Activity</span> <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">027 </span>
 <span style="color: #99968b; font-style: italic;">028 </span>    <span style="color: #f6f3e8;">Button</span> <span style="color: #f6f3e8;">bn_add</span>;
 <span style="color: #99968b; font-style: italic;">029 </span>    <span style="color: #f6f3e8;">EditText</span> <span style="color: #f6f3e8;">dialog_name</span>;
 <span style="color: #f810b0;">030 </span>    <span style="color: #f6f3e8;">EditText</span> <span style="color: #f6f3e8;">dialog_time</span>;
 <span style="color: #99968b; font-style: italic;">031 </span>
 <span style="color: #99968b; font-style: italic;">032 </span>    <span style="color: #99968b; font-style: italic;">//注意这三个东西的长度保持一致</span>
 <span style="color: #99968b; font-style: italic;">033 </span>    <span style="color: #f6f3e8;">List</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">Voice</span><span style="color: #f6f3e8;">&gt;</span> <span style="color: #f6f3e8;">listVoice</span>;
 <span style="color: #99968b; font-style: italic;">034 </span>    <span style="color: #f6f3e8;">List</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">String</span><span style="color: #f6f3e8;">&gt;</span> <span style="color: #f6f3e8;">listString</span>;
 <span style="color: #f810b0;">035 </span>    <span style="color: #f6f3e8;">ListView</span> <span style="color: #f6f3e8;">listView</span>;
 <span style="color: #99968b; font-style: italic;">036 </span>
 <span style="color: #99968b; font-style: italic;">037 </span>    <span style="color: #99968b; font-style: italic;">//listview的适配器</span>
 <span style="color: #99968b; font-style: italic;">038 </span>    <span style="color: #f6f3e8;">ArrayAdapter</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">String</span><span style="color: #f6f3e8;">&gt;</span> <span style="color: #f6f3e8;">adapter</span>;
 <span style="color: #99968b; font-style: italic;">039 </span>
 <span style="color: #f810b0;">040 </span>    <span style="color: #f6f3e8;">SharedPreferences</span> <span style="color: #f6f3e8;">sharedPreferences</span>;<span style="color: #99968b; font-style: italic;">//定义sharedprefence</span>
 <span style="color: #99968b; font-style: italic;">041 </span>    <span style="color: #f6f3e8;">SharedPreferences</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">Editor</span> <span style="color: #f6f3e8;">editor</span>;<span style="color: #99968b; font-style: italic;">//定义editor，用于提交数据到sharedprefence，具体参看文档</span>
 <span style="color: #99968b; font-style: italic;">042 </span>
 <span style="color: #99968b; font-style: italic;">043 </span>    <span style="color: #f6f3e8;">@Override</span>
 <span style="color: #99968b; font-style: italic;">044 </span>    <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onCreate</span>(<span style="color: #f6f3e8;">Bundle</span> <span style="color: #f6f3e8;">savedInstanceState</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">045 </span>        <span style="color: #8ac6f2;">super</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">onCreate</span>(<span style="color: #f6f3e8;">savedInstanceState</span>);
 <span style="color: #99968b; font-style: italic;">046 </span>        <span style="color: #f6f3e8;">setContentView</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">activity_main</span>);
 <span style="color: #99968b; font-style: italic;">047 </span>        <span style="color: #99968b; font-style: italic;">//添加按钮</span>
 <span style="color: #99968b; font-style: italic;">048 </span>        <span style="color: #f6f3e8;">bn_add</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">Button</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">bn_add</span>);
 <span style="color: #99968b; font-style: italic;">049 </span>        <span style="color: #99968b; font-style: italic;">//获取listview</span>
 <span style="color: #f810b0;">050 </span>        <span style="color: #f6f3e8;">listView</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">ListView</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">list</span>);
 <span style="color: #99968b; font-style: italic;">051 </span>
 <span style="color: #99968b; font-style: italic;">052 </span>        <span style="color: #99968b; font-style: italic;">//使用getSharedPreferences获取存储的prefences，一样的使用key-value方式存储，使用key查找，之后是一个mode，MODE_PRIVATE表示此prefences只能此程序使用</span>
 <span style="color: #99968b; font-style: italic;">053 </span>        <span style="color: #f6f3e8;">sharedPreferences</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">getSharedPreferences</span>(<span style="color: #95e454; font-style: italic;">"HelloVoice"</span><span style="color: #f6f3e8;">,</span><span style="color: #f6f3e8;">MODE_PRIVATE</span>);
 <span style="color: #99968b; font-style: italic;">054 </span>
 <span style="color: #f810b0;">055 </span>        <span style="color: #99968b; font-style: italic;">//实例listVoice和listString</span>
 <span style="color: #99968b; font-style: italic;">056 </span>        <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">ArrayList</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">Voice</span><span style="color: #f6f3e8;">&gt;();</span>
 <span style="color: #99968b; font-style: italic;">057 </span>        <span style="color: #f6f3e8;">listString</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">ArrayList</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">String</span><span style="color: #f6f3e8;">&gt;();</span>
 <span style="color: #99968b; font-style: italic;">058 </span>
 <span style="color: #99968b; font-style: italic;">059 </span>        <span style="color: #99968b; font-style: italic;">//首先获取上次退出时listView的长度</span>
 <span style="color: #f810b0;">060 </span>        <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">size</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">sharedPreferences</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getInt</span>(<span style="color: #95e454; font-style: italic;">"size"</span><span style="color: #f6f3e8;">,</span> <span style="color: #e5786d;">0</span>);
 <span style="color: #99968b; font-style: italic;">061 </span>
 <span style="color: #99968b; font-style: italic;">062 </span>        <span style="color: #99968b; font-style: italic;">//循环上次退出时listview长度次</span>
 <span style="color: #99968b; font-style: italic;">063 </span>        <span style="color: #8ac6f2;">for</span>(<span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">=</span><span style="color: #e5786d;">0</span>;<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">size</span>;<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">++)</span>
 <span style="color: #99968b; font-style: italic;">064 </span>        <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">065 </span>            <span style="color: #99968b; font-style: italic;">//获取上次存储的listVoice中每一项的name和time</span>
 <span style="color: #99968b; font-style: italic;">066 </span>            <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">temp_name</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">sharedPreferences</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getString</span>(<span style="color: #95e454; font-style: italic;">"name"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">,</span> <span style="color: #8ac6f2;">null</span>);
 <span style="color: #99968b; font-style: italic;">067 </span>            <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">temp_time</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">sharedPreferences</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getInt</span>(<span style="color: #95e454; font-style: italic;">"time"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">,</span> <span style="color: #e5786d;">0</span>);
 <span style="color: #99968b; font-style: italic;">068 </span>            <span style="color: #99968b; font-style: italic;">//使用构造方法直接传入两个参数，生成新的Voice</span>
 <span style="color: #99968b; font-style: italic;">069 </span>            <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">add</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">Voice</span>(<span style="color: #f6f3e8;">temp_name</span><span style="color: #f6f3e8;">,</span><span style="color: #f6f3e8;">temp_time</span>));
 <span style="color: #f810b0;">070 </span>            <span style="color: #99968b; font-style: italic;">//listView显示声音描述和声音时长</span>
 <span style="color: #99968b; font-style: italic;">071 </span>            <span style="color: #f6f3e8;">listString</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">add</span>(<span style="color: #f6f3e8;">temp_name</span><span style="color: #f6f3e8;">+</span><span style="color: #95e454; font-style: italic;">"  "</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">temp_time</span><span style="color: #f6f3e8;">+</span><span style="color: #95e454; font-style: italic;">"s"</span>);
 <span style="color: #99968b; font-style: italic;">072 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">073 </span>
 <span style="color: #99968b; font-style: italic;">074 </span>        <span style="color: #99968b; font-style: italic;">//new ArrayAdapter&lt;String&gt;(context, textViewResourceId, objects)中的三个参数</span>
 <span style="color: #f810b0;">075 </span>        <span style="color: #99968b; font-style: italic;">//context 官方文档解释是The current context. 一直想不通，看到大家用this就跟着用了，求大神解释</span>
 <span style="color: #99968b; font-style: italic;">076 </span>        <span style="color: #99968b; font-style: italic;">//textViewResourceId是指ListView中每一个Item的布局，我代码里使用的是默认的单行文字布局，没有自定义，可以自定义</span>
 <span style="color: #99968b; font-style: italic;">077 </span>        <span style="color: #99968b; font-style: italic;">//最后的objects就是你的数据，一个String数组或者其他数据容器，大部分示例代码中会使用一个自定义getData（）方法来返回一个设定好的ArrayList&lt;String&gt;，我这里直接使用的自定义好的</span>
 <span style="color: #99968b; font-style: italic;">078 </span>        <span style="color: #f6f3e8;">adapter</span><span style="color: #f6f3e8;">=</span><span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">ArrayAdapter</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">String</span><span style="color: #f6f3e8;">&gt;(</span><span style="color: #8ac6f2;">this</span><span style="color: #f6f3e8;">,</span><span style="color: #f6f3e8;">android</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">simple_list_item_1</span><span style="color: #f6f3e8;">,</span><span style="color: #f6f3e8;">listString</span>);
 <span style="color: #99968b; font-style: italic;">079 </span>
 <span style="color: #f810b0;">080 </span>        <span style="color: #99968b; font-style: italic;">//设置适配器</span>
 <span style="color: #99968b; font-style: italic;">081 </span>        <span style="color: #f6f3e8;">listView</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setAdapter</span>(<span style="color: #f6f3e8;">adapter</span>);
 <span style="color: #99968b; font-style: italic;">082 </span>
 <span style="color: #99968b; font-style: italic;">083 </span>        <span style="color: #99968b; font-style: italic;">//绑定添加按钮事件</span>
 <span style="color: #99968b; font-style: italic;">084 </span>        <span style="color: #f6f3e8;">bn_add</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOnClickListener</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">OnClickListener</span>() <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">085 </span>
 <span style="color: #99968b; font-style: italic;">086 </span>            <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(<span style="color: #f6f3e8;">View</span> <span style="color: #f6f3e8;">v</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">087 </span>                <span style="color: #99968b; font-style: italic;">// TODO Auto-generated method stub</span>
 <span style="color: #99968b; font-style: italic;">088 </span>                <span style="color: #99968b; font-style: italic;">//看此方法定义</span>
 <span style="color: #99968b; font-style: italic;">089 </span>                <span style="color: #f6f3e8;">AddDialog</span>();
 <span style="color: #f810b0;">090 </span>            <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">091 </span>        <span style="color: #f6f3e8;">});</span>
 <span style="color: #99968b; font-style: italic;">092 </span>
 <span style="color: #99968b; font-style: italic;">093 </span>        <span style="color: #99968b; font-style: italic;">//设定长按listiew每一个item的事件</span>
 <span style="color: #99968b; font-style: italic;">094 </span>        <span style="color: #f6f3e8;">listView</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setOnItemLongClickListener</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">OnItemLongClickListener</span>() <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">095 </span>
 <span style="color: #99968b; font-style: italic;">096 </span>            <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">boolean</span> <span style="color: #cae682;">onItemLongClick</span>(<span style="color: #f6f3e8;">AdapterView</span><span style="color: #f6f3e8;">&lt;?&gt;</span> <span style="color: #f6f3e8;">arg0</span><span style="color: #f6f3e8;">,</span> <span style="color: #f6f3e8;">View</span> <span style="color: #f6f3e8;">arg1</span><span style="color: #f6f3e8;">,</span>
 <span style="color: #99968b; font-style: italic;">097 </span>                    <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">arg2</span><span style="color: #f6f3e8;">,</span> <span style="color: #cae682;">long</span> <span style="color: #f6f3e8;">arg3</span>) <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">098 </span>                <span style="color: #99968b; font-style: italic;">// TODO Auto-generated method stub</span>
 <span style="color: #99968b; font-style: italic;">099 </span>                <span style="color: #99968b; font-style: italic;">//看此方法定义，传入arg2参数即告知被长按的item的序号，也是对应在listString，listVoice中的序号</span>
 <span style="color: #f810b0;">100 </span>                <span style="color: #f6f3e8;">LongDialog</span>(<span style="color: #f6f3e8;">arg2</span>);
 <span style="color: #99968b; font-style: italic;">101 </span>                <span style="color: #8ac6f2;">return</span> <span style="color: #8ac6f2;">false</span>;
 <span style="color: #99968b; font-style: italic;">102 </span>            <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">103 </span>        <span style="color: #f6f3e8;">});</span>
 <span style="color: #99968b; font-style: italic;">104 </span>
 <span style="color: #f810b0;">105 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">106 </span>
 <span style="color: #99968b; font-style: italic;">107 </span>    <span style="color: #8ac6f2;">protected</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onStop</span>()
 <span style="color: #99968b; font-style: italic;">108 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">109 </span>        <span style="color: #99968b; font-style: italic;">//在程序退出时保存，看此方法定义</span>
 <span style="color: #f810b0;">110 </span>        <span style="color: #f6f3e8;">CommitTo</span>();
 <span style="color: #99968b; font-style: italic;">111 </span>        <span style="color: #8ac6f2;">super</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">onStop</span>();
 <span style="color: #99968b; font-style: italic;">112 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">113 </span>
 <span style="color: #99968b; font-style: italic;">114 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">CommitTo</span>()
 <span style="color: #f810b0;">115 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">116 </span>        <span style="color: #99968b; font-style: italic;">//获取editor</span>
 <span style="color: #99968b; font-style: italic;">117 </span>        <span style="color: #f6f3e8;">editor</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">sharedPreferences</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">edit</span>();
 <span style="color: #99968b; font-style: italic;">118 </span>        <span style="color: #99968b; font-style: italic;">//先将listVoice即listview，也是listString的长度存入</span>
 <span style="color: #99968b; font-style: italic;">119 </span>        <span style="color: #f6f3e8;">editor</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">putInt</span>(<span style="color: #95e454; font-style: italic;">"size"</span><span style="color: #f6f3e8;">,</span> <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">size</span>());
 <span style="color: #f810b0;">120 </span>
 <span style="color: #99968b; font-style: italic;">121 </span>        <span style="color: #99968b; font-style: italic;">//循环长度次数</span>
 <span style="color: #99968b; font-style: italic;">122 </span>        <span style="color: #8ac6f2;">for</span>(<span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">=</span><span style="color: #e5786d;">0</span>;<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">&lt;</span><span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">size</span>();<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">++)</span>
 <span style="color: #99968b; font-style: italic;">123 </span>        <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">124 </span>            <span style="color: #99968b; font-style: italic;">//直接分别存入listVoice每一项的name和time</span>
 <span style="color: #f810b0;">125 </span>            <span style="color: #f6f3e8;">editor</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">putString</span>(<span style="color: #95e454; font-style: italic;">"name"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">,</span> <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">get</span>(<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">).</span><span style="color: #cae682;">name</span>);
 <span style="color: #99968b; font-style: italic;">126 </span>            <span style="color: #f6f3e8;">editor</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">putInt</span>(<span style="color: #95e454; font-style: italic;">"time"</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">,</span> <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">get</span>(<span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">).</span><span style="color: #cae682;">time</span>);
 <span style="color: #99968b; font-style: italic;">127 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">128 </span>        <span style="color: #99968b; font-style: italic;">//提交，不使用此方法不会存储</span>
 <span style="color: #99968b; font-style: italic;">129 </span>        <span style="color: #f6f3e8;">editor</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">commit</span>();
 <span style="color: #f810b0;">130 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">131 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">VoiceAdd</span>()
 <span style="color: #99968b; font-style: italic;">132 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">133 </span>         <span style="color: #99968b; font-style: italic;">//获取dialog中两个edittext的内容</span>
 <span style="color: #99968b; font-style: italic;">134 </span>         <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">dialog_name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span>();
 <span style="color: #f810b0;">135 </span>         <span style="color: #f6f3e8;">String</span> <span style="color: #f6f3e8;">timeString</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">dialog_time</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">getText</span><span style="color: #f6f3e8;">().</span><span style="color: #cae682;">toString</span>();
 <span style="color: #99968b; font-style: italic;">136 </span>
 <span style="color: #99968b; font-style: italic;">137 </span>         <span style="color: #99968b; font-style: italic;">//判断语句是防止edittext是空的时候，添加了空白的东西</span>
 <span style="color: #99968b; font-style: italic;">138 </span>        <span style="color: #8ac6f2;">if</span>(<span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">!=</span><span style="color: #8ac6f2;">null</span><span style="color: #f6f3e8;">&amp;&amp;</span><span style="color: #f6f3e8;">timeString</span><span style="color: #f6f3e8;">!=</span><span style="color: #8ac6f2;">null</span><span style="color: #f6f3e8;">&amp;&amp;</span><span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">length</span><span style="color: #f6f3e8;">()!=</span><span style="color: #e5786d;">0</span><span style="color: #f6f3e8;">&amp;&amp;</span><span style="color: #f6f3e8;">timeString</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">length</span><span style="color: #f6f3e8;">()!=</span><span style="color: #e5786d;">0</span>)
 <span style="color: #99968b; font-style: italic;">139 </span>        <span style="color: #f6f3e8;">{</span>
 <span style="color: #f810b0;">140 </span>            <span style="color: #99968b; font-style: italic;">//转换用户输入的string变成int值</span>
 <span style="color: #99968b; font-style: italic;">141 </span>            <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">time</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">Integer</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">parseInt</span>(<span style="color: #f6f3e8;">timeString</span>);
 <span style="color: #99968b; font-style: italic;">142 </span>            <span style="color: #99968b; font-style: italic;">//再次使用构造器生成Voice，并且添加到listVoice</span>
 <span style="color: #99968b; font-style: italic;">143 </span>            <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">add</span>(<span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">Voice</span>(<span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">,</span><span style="color: #f6f3e8;">time</span>));
 <span style="color: #99968b; font-style: italic;">144 </span>            <span style="color: #99968b; font-style: italic;">//一样的将声音描述和长度添加到listString</span>
 <span style="color: #f810b0;">145 </span>            <span style="color: #f6f3e8;">listString</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">add</span>(<span style="color: #f6f3e8;">name</span><span style="color: #f6f3e8;">+</span><span style="color: #95e454; font-style: italic;">"  "</span><span style="color: #f6f3e8;">+</span><span style="color: #f6f3e8;">time</span><span style="color: #f6f3e8;">+</span><span style="color: #95e454; font-style: italic;">"s"</span>);
 <span style="color: #99968b; font-style: italic;">146 </span>            <span style="color: #99968b; font-style: italic;">//更新listview</span>
 <span style="color: #99968b; font-style: italic;">147 </span>            <span style="color: #f6f3e8;">adapter</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">notifyDataSetChanged</span>();
 <span style="color: #99968b; font-style: italic;">148 </span>        <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">149 </span>
 <span style="color: #f810b0;">150 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">151 </span>
 <span style="color: #99968b; font-style: italic;">152 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">AddDialog</span>()
 <span style="color: #99968b; font-style: italic;">153 </span>    <span style="color: #f6f3e8;">{</span>
 <span style="color: #99968b; font-style: italic;">154 </span>        <span style="color: #99968b; font-style: italic;">//layoutinflater用来查找资源中的xml资源并且示例化</span>
 <span style="color: #f810b0;">155 </span>        <span style="color: #f6f3e8;">LayoutInflater</span> <span style="color: #f6f3e8;">inflater</span> <span style="color: #f6f3e8;">=</span> <span style="color: #f6f3e8;">getLayoutInflater</span>();
 <span style="color: #99968b; font-style: italic;">156 </span>        <span style="color: #99968b; font-style: italic;">//获取自定义dialog的layout</span>
 <span style="color: #99968b; font-style: italic;">157 </span>        <span style="color: #f6f3e8;">View</span> <span style="color: #f6f3e8;">layout</span><span style="color: #f6f3e8;">=</span><span style="color: #f6f3e8;">inflater</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">inflate</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">dialog</span><span style="color: #f6f3e8;">,</span> (<span style="color: #f6f3e8;">ViewGroup</span>)<span style="color: #f6f3e8;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">dialog</span>));
 <span style="color: #99968b; font-style: italic;">158 </span>        <span style="color: #99968b; font-style: italic;">//注意此处获取dialog的布局xml中的控件的时候，必须使用layout.，因为findviewbyid是查找父view中的id，而此edittext的父view是dialog不是activity</span>
 <span style="color: #99968b; font-style: italic;">159 </span>        <span style="color: #f6f3e8;">dialog_name</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">EditText</span>)<span style="color: #f6f3e8;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">dialog_name</span>);
 <span style="color: #f810b0;">160 </span>        <span style="color: #f6f3e8;">dialog_time</span><span style="color: #f6f3e8;">=(</span><span style="color: #f6f3e8;">EditText</span>)<span style="color: #f6f3e8;">layout</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">findViewById</span>(<span style="color: #f6f3e8;">R</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">id</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">dialog_time</span>);
 <span style="color: #99968b; font-style: italic;">161 </span>
 <span style="color: #99968b; font-style: italic;">162 </span>        <span style="color: #99968b; font-style: italic;">//新建一个对话框</span>
 <span style="color: #99968b; font-style: italic;">163 </span>        <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">AlertDialog</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">Builder</span>(<span style="color: #8ac6f2;">this</span>)
 <span style="color: #99968b; font-style: italic;">164 </span>        <span style="color: #99968b; font-style: italic;">//设置dialog标题</span>
 <span style="color: #f810b0;">165 </span>        <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setTitle</span>(<span style="color: #95e454; font-style: italic;">"添加新的声音"</span>)
 <span style="color: #99968b; font-style: italic;">166 </span>        <span style="color: #99968b; font-style: italic;">//设置dialog的自定义样式</span>
 <span style="color: #99968b; font-style: italic;">167 </span>        <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setView</span>(<span style="color: #f6f3e8;">layout</span>)
 <span style="color: #99968b; font-style: italic;">168 </span>        <span style="color: #99968b; font-style: italic;">//设置确定按键</span>
 <span style="color: #99968b; font-style: italic;">169 </span>        <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setPositiveButton</span>(<span style="color: #95e454; font-style: italic;">"确定"</span><span style="color: #f6f3e8;">,</span>  <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">DialogInterface</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">OnClickListener</span><span style="color: #f6f3e8;">(){</span>
 <span style="color: #f810b0;">170 </span>                <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(
 <span style="color: #99968b; font-style: italic;">171 </span>                    <span style="color: #f6f3e8;">DialogInterface</span> <span style="color: #f6f3e8;">dialoginterface</span><span style="color: #f6f3e8;">,</span> <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">172 </span>                    <span style="color: #99968b; font-style: italic;">//这里写确定键按下的处理  </span>
 <span style="color: #99968b; font-style: italic;">173 </span>                    <span style="color: #f6f3e8;">VoiceAdd</span>();
 <span style="color: #99968b; font-style: italic;">174 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">175 </span>            <span style="color: #f6f3e8;">})</span>
 <span style="color: #99968b; font-style: italic;">176 </span>        <span style="color: #99968b; font-style: italic;">//设置取消按键，这里不需要填入任何内容</span>
 <span style="color: #99968b; font-style: italic;">177 </span>        <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setNegativeButton</span>(<span style="color: #95e454; font-style: italic;">"取消"</span><span style="color: #f6f3e8;">,</span>
 <span style="color: #99968b; font-style: italic;">178 </span>                <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">DialogInterface</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">OnClickListener</span><span style="color: #f6f3e8;">(){</span>
 <span style="color: #99968b; font-style: italic;">179 </span>                <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(
 <span style="color: #f810b0;">180 </span>                    <span style="color: #f6f3e8;">DialogInterface</span> <span style="color: #f6f3e8;">dialoginterface</span><span style="color: #f6f3e8;">,</span> <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">181 </span>                    <span style="color: #99968b; font-style: italic;">//这里写取消键按下的处理                          </span>
 <span style="color: #99968b; font-style: italic;">182 </span>
 <span style="color: #99968b; font-style: italic;">183 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">184 </span>            <span style="color: #f6f3e8;">})</span>
 <span style="color: #f810b0;">185 </span>        <span style="color: #99968b; font-style: italic;">//这个方法必须要，显示dialog</span>
 <span style="color: #99968b; font-style: italic;">186 </span>        <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">show</span>();
 <span style="color: #99968b; font-style: italic;">187 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">188 </span>    <span style="color: #8ac6f2;">private</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">LongDialog</span>( <span style="color: #8ac6f2;">final</span> <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">arg2</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">189 </span>
 <span style="color: #f810b0;">190 </span>        <span style="color: #99968b; font-style: italic;">//新建一个对话框</span>
 <span style="color: #99968b; font-style: italic;">191 </span>        <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">AlertDialog</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">Builder</span>(<span style="color: #f6f3e8;">MainActivity</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">this</span>)
 <span style="color: #99968b; font-style: italic;">192 </span>            <span style="color: #99968b; font-style: italic;">//设置dialog标题</span>
 <span style="color: #99968b; font-style: italic;">193 </span>            <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setTitle</span>(<span style="color: #95e454; font-style: italic;">"温馨提示"</span>)
 <span style="color: #99968b; font-style: italic;">194 </span>            <span style="color: #99968b; font-style: italic;">//设置提醒消息</span>
 <span style="color: #f810b0;">195 </span>            <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setMessage</span>(<span style="color: #95e454; font-style: italic;">"是否删除这个声音"</span>)
 <span style="color: #99968b; font-style: italic;">196 </span>            <span style="color: #99968b; font-style: italic;">//设置确定按键</span>
 <span style="color: #99968b; font-style: italic;">197 </span>            <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setPositiveButton</span>(<span style="color: #95e454; font-style: italic;">"确定"</span><span style="color: #f6f3e8;">,</span>
 <span style="color: #99968b; font-style: italic;">198 </span>                <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">DialogInterface</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">OnClickListener</span><span style="color: #f6f3e8;">(){</span>
 <span style="color: #99968b; font-style: italic;">199 </span>                <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(
 <span style="color: #f810b0;">200 </span>                    <span style="color: #f6f3e8;">DialogInterface</span> <span style="color: #f6f3e8;">dialoginterface</span><span style="color: #f6f3e8;">,</span> <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">201 </span>                    <span style="color: #99968b; font-style: italic;">//这里写确定键按下的处理  </span>
 <span style="color: #99968b; font-style: italic;">202 </span>                    <span style="color: #f6f3e8;">listVoice</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">remove</span>(<span style="color: #f6f3e8;">arg2</span>);
 <span style="color: #99968b; font-style: italic;">203 </span>                    <span style="color: #f6f3e8;">listString</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">remove</span>(<span style="color: #f6f3e8;">arg2</span>);
 <span style="color: #99968b; font-style: italic;">204 </span>                    <span style="color: #99968b; font-style: italic;">//CommitTo();</span>
 <span style="color: #f810b0;">205 </span>                    <span style="color: #f6f3e8;">adapter</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">notifyDataSetChanged</span>();
 <span style="color: #99968b; font-style: italic;">206 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">207 </span>            <span style="color: #f6f3e8;">})</span>
 <span style="color: #99968b; font-style: italic;">208 </span>            <span style="color: #99968b; font-style: italic;">//设置取消按键，这里不需要填入任何内容</span>
 <span style="color: #99968b; font-style: italic;">209 </span>            <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">setNegativeButton</span>(<span style="color: #95e454; font-style: italic;">"取消"</span><span style="color: #f6f3e8;">,</span>
 <span style="color: #f810b0;">210 </span>                <span style="color: #8ac6f2;">new</span> <span style="color: #f6f3e8;">DialogInterface</span><span style="color: #f6f3e8;">.</span><span style="color: #cae682;">OnClickListener</span><span style="color: #f6f3e8;">(){</span>
 <span style="color: #99968b; font-style: italic;">211 </span>                <span style="color: #8ac6f2;">public</span> <span style="color: #cae682;">void</span> <span style="color: #cae682;">onClick</span>(
 <span style="color: #99968b; font-style: italic;">212 </span>                    <span style="color: #f6f3e8;">DialogInterface</span> <span style="color: #f6f3e8;">dialoginterface</span><span style="color: #f6f3e8;">,</span> <span style="color: #cae682;">int</span> <span style="color: #f6f3e8;">i</span><span style="color: #f6f3e8;">){</span>
 <span style="color: #99968b; font-style: italic;">213 </span>                    <span style="color: #99968b; font-style: italic;">//这里写取消键按下的处理                          </span>
 <span style="color: #99968b; font-style: italic;">214 </span>
 <span style="color: #f810b0;">215 </span>                <span style="color: #f6f3e8;">}</span>
 <span style="color: #99968b; font-style: italic;">216 </span>            <span style="color: #f6f3e8;">})</span>
 <span style="color: #99968b; font-style: italic;">217 </span>            <span style="color: #99968b; font-style: italic;">//这个方法必须要，显示dialog</span>
 <span style="color: #99968b; font-style: italic;">218 </span>            <span style="color: #f6f3e8;">.</span><span style="color: #cae682;">show</span>();
 <span style="color: #99968b; font-style: italic;">219 </span>    <span style="color: #f6f3e8;">}</span>
 <span style="color: #f810b0;">220 </span>
 <span style="color: #99968b; font-style: italic;">221 </span>
 <span style="color: #99968b; font-style: italic;">222 </span><span style="color: #f6f3e8;">}</span></div>