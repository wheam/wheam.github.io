title: "测试android摄像头预览支持分辨率程序"
id: 264
date: 2012-12-13 13:16:51
tags: 
categories: 
- android
---

&nbsp;

<div>
	<span style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">因为SeeMore UI设计对摄像头支持分辨率的要求，所以很需要手机支持的预览分辨率尽量和手机分辨率相差不多，为了测试各手机支持的预览分辨率，我写了这个程序来简单测试。直接看代码和注释吧，这个程序也可以当做个最简单的自定义摄像头程序。</span>
</div>
<div>
	&nbsp;
</div>
<!-- more -->
<div>
	<span style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">首先是布局文件：</span>
</div>
<div class="source" style="font-family: Consolas, 'Lucida Console', 'Courier New'; color: rgb(246, 243, 232); background-color: rgb(36, 36, 36);">
	<span style="color: rgb(153, 150, 139); font-style: italic;">01&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;FrameLayout</span> <span style="color: rgb(202, 230, 130);">xmlns:android=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;http://schemas.android.com/apk/res/android&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">02&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">xmlns:tools=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;http://schemas.android.com/tools&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">03&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;match_parent&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">04&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;match_parent&quot;</span> <span style="color: rgb(138, 198, 242);">&gt;</span>

	<span style="color: rgb(248, 16, 176);">05&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">06&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;SurfaceView</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">07&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/Camera&quot;</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">08&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">09&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(248, 16, 176);">10&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:gravity=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;center&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">11&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:visibility=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;visible&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">12&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">/&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">13&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;LinearLayout</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">14&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(248, 16, 176);">15&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">16&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:orientation=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;vertical&quot;</span> <span style="color: rgb(138, 198, 242);">&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">17&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">18&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;TextView</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">19&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(248, 16, 176);">20&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">21&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:textColor=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;#FFFFFF&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">22&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/out&quot;</span><span style="color: rgb(138, 198, 242);">/&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">23&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;Button</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">24&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/bn&quot;</span>

	<span style="color: rgb(248, 16, 176);">25&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">26&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">27&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:text=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;点击我显示摄像头预览分辨率&quot;</span><span style="color: rgb(138, 198, 242);">/&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">28&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;/LinearLayout&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">29&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;/FrameLayout&gt;</span>
</div>
<div>
	&nbsp;
</div>
<div>
	&nbsp;
</div>
<div>
	&nbsp;
</div>
<div>
	<span style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">接着manifest文件里面必须有这两个声明</span>
</div>
<div class="source" style="font-family: Consolas; color: rgb(246, 243, 232); background-color: rgb(36, 36, 36);">
	<span style="color: rgb(153, 150, 139); font-style: italic;">1&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;uses-permission</span> <span style="color: rgb(202, 230, 130);">android:name=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;android.permission.CAMERA&quot;</span> <span style="color: rgb(138, 198, 242);">/&gt;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">2&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;uses-feature</span> <span style="color: rgb(202, 230, 130);">android:name=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;android.hardware.camera&quot;</span> <span style="color: rgb(138, 198, 242);">/&gt;</span>
</div>
<div>
	&nbsp;
</div>
<div>
	&nbsp;
</div>
<div>
	&nbsp;
</div>
<div>
	&nbsp;
</div>
<div>
	<font color="#000000" face="Microsoft YaHei UI" size="3">下面是java代码：</font>
</div>

     <div class="source" style="font-family: Consolas; color: rgb(246, 243, 232); background-color: rgb(36, 36, 36);"><span style="color: rgb(153, 150, 139); font-style: italic;">001&nbsp;</span><span style="color: rgb(246, 243, 232);">package</span> <span style="color: rgb(246, 243, 232);">com</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">camerapreview</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">002&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">003&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">java.util.Iterator</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">004&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">java.util.List</span>;
<span style="color: rgb(248, 16, 176);">005&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">006&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.hardware.Camera</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">007&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.os.Bundle</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">008&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.app.Activity</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">009&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.content.res.Configuration</span>;
<span style="color: rgb(248, 16, 176);">010&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.Menu</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">011&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.SurfaceHolder</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">012&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.SurfaceView</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">013&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.View</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">014&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.View.OnClickListener</span>;
<span style="color: rgb(248, 16, 176);">015&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.Button</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">016&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.TextView</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">017&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">018&nbsp;</span><span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(138, 198, 242);">class</span> <span style="color: rgb(246, 243, 232);">MainActivity</span> <span style="color: rgb(138, 198, 242);">extends</span> <span style="color: rgb(246, 243, 232);">Activity</span> <span style="color: rgb(138, 198, 242);">implements</span> <span style="color: rgb(246, 243, 232);">SurfaceHolder</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Callback</span><span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">019&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">Camera</span> <span style="color: rgb(246, 243, 232);">camera</span>;<span style="color: rgb(153, 150, 139); font-style: italic;">//此处为andorid.hardware.camera 需要和图像的camera区别开</span>
<span style="color: rgb(248, 16, 176);">020&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">List</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">Camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Size</span><span style="color: rgb(246, 243, 232);">&gt;</span> <span style="color: rgb(246, 243, 232);">previewSizes</span>;<span style="color: rgb(153, 150, 139); font-style: italic;">//用于存储预览分辨率</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">021&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">TextView</span> <span style="color: rgb(246, 243, 232);">out</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">022&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">String</span> <span style="color: rgb(246, 243, 232);">string</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">023&nbsp;</span> &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">024&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">@Override</span>
<span style="color: rgb(248, 16, 176);">025&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onCreate</span>(<span style="color: rgb(246, 243, 232);">Bundle</span> <span style="color: rgb(246, 243, 232);">savedInstanceState</span>) <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">026&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">super</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">onCreate</span>(<span style="color: rgb(246, 243, 232);">savedInstanceState</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">027&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">setContentView</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">layout</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">activity_main</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">028&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">029&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">out</span><span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">TextView</span>) <span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">out</span>);
<span style="color: rgb(248, 16, 176);">030&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">Button</span> bn<span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">Button</span>) <span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span>bn);
<span style="color: rgb(153, 150, 139); font-style: italic;">031&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">SurfaceView</span> <span style="color: rgb(246, 243, 232);">surfaceView</span><span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">SurfaceView</span>) <span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Camera</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">032&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">033&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//此三句主要是使用surfaceholder实现对surfview的控制</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">034&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">SurfaceHolder</span> <span style="color: rgb(246, 243, 232);">holder</span> <span style="color: rgb(246, 243, 232);">=</span> <span style="color: rgb(246, 243, 232);">surfaceView</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">getHolder</span>();
<span style="color: rgb(248, 16, 176);">035&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">holder</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setType</span>(<span style="color: rgb(246, 243, 232);">SurfaceHolder</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">SURFACE_TYPE_PUSH_BUFFERS</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">036&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">holder</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">addCallback</span>(<span style="color: rgb(138, 198, 242);">this</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">037&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">038&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;bn<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setOnClickListener</span>(<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">OnClickListener</span>() <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">039&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(248, 16, 176);">040&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onClick</span>(<span style="color: rgb(246, 243, 232);">View</span> <span style="color: rgb(246, 243, 232);">v</span>) <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">041&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">042&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">string</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;支持的分辨率有： &nbsp; &nbsp;&quot;</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">043&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">for</span>(<span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(229, 120, 109);">0</span>;<span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">previewSizes</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">size</span>();<span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">++)</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">044&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(248, 16, 176);">045&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">Iterator</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">Camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Size</span><span style="color: rgb(246, 243, 232);">&gt;</span> <span style="color: rgb(246, 243, 232);">itor</span> <span style="color: rgb(246, 243, 232);">=</span> <span style="color: rgb(246, 243, 232);">previewSizes</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">iterator</span>();<span style="color: rgb(153, 150, 139); font-style: italic;">//我能说我一听到迭代器就想起李小爽在李寒班上课时问老师的问题么</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">046&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">if</span><span style="color: rgb(246, 243, 232);">(!</span><span style="color: rgb(246, 243, 232);">itor</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">hasNext</span>())<span style="color: rgb(153, 150, 139); font-style: italic;">//如果迭代器里面没有下一个了，就直接break</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">047&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">048&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">break</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">049&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(248, 16, 176);">050&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">if</span>(<span style="color: rgb(246, 243, 232);">previewSizes</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">get</span>(<span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">)==</span><span style="color: rgb(138, 198, 242);">null</span>)<span style="color: rgb(153, 150, 139); font-style: italic;">//如果此处为空，则进行下一个，</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">051&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">continue</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">052&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//直接放进一个string里</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">053&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">string</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(246, 243, 232);">string</span><span style="color: rgb(246, 243, 232);">+</span><span style="color: rgb(246, 243, 232);">previewSizes</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">get</span>(<span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">).</span><span style="color: rgb(202, 230, 130);">height</span><span style="color: rgb(246, 243, 232);">+</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;*&quot;</span><span style="color: rgb(246, 243, 232);">+</span><span style="color: rgb(246, 243, 232);">previewSizes</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">get</span>(<span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">).</span><span style="color: rgb(202, 230, 130);">width</span><span style="color: rgb(246, 243, 232);">+</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot; &nbsp; &nbsp;&quot;</span>;
<span style="color: rgb(153, 150, 139); font-style: italic;">054&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(248, 16, 176);">055&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">056&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">057&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">058&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">out</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setText</span>(<span style="color: rgb(246, 243, 232);">string</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">059&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(248, 16, 176);">060&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">});</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">061&nbsp;</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">062&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">063&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">064&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">surfaceChanged</span>(<span style="color: rgb(246, 243, 232);">SurfaceHolder</span> <span style="color: rgb(246, 243, 232);">holder</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">format</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">width</span><span style="color: rgb(246, 243, 232);">,</span>
<span style="color: rgb(248, 16, 176);">065&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">height</span>) <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">066&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">067&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">068&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">069&nbsp;</span>
<span style="color: rgb(248, 16, 176);">070&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">surfaceCreated</span>(<span style="color: rgb(246, 243, 232);">SurfaceHolder</span> <span style="color: rgb(246, 243, 232);">holder</span>) <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">071&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">072&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(246, 243, 232);">Camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">open</span>();<span style="color: rgb(153, 150, 139); font-style: italic;">//打开摄像头</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">073&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">074&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">try</span><span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(248, 16, 176);">075&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">Camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Parameters</span> <span style="color: rgb(246, 243, 232);">param</span> <span style="color: rgb(246, 243, 232);">=</span> <span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">getParameters</span>(); &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">076&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">077&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//不设置这个，图像和实际会相差90度</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">078&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">if</span>(<span style="color: rgb(138, 198, 242);">this</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">getResources</span><span style="color: rgb(246, 243, 232);">().</span><span style="color: rgb(202, 230, 130);">getConfiguration</span><span style="color: rgb(246, 243, 232);">().</span><span style="color: rgb(202, 230, 130);">orientation</span> <span style="color: rgb(246, 243, 232);">!=</span> <span style="color: rgb(246, 243, 232);">Configuration</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">ORIENTATION_LANDSCAPE</span><span style="color: rgb(246, 243, 232);">){</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">079&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//如果是竖屏 &nbsp;</span>
<span style="color: rgb(248, 16, 176);">080&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">param</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">set</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;orientation&quot;</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(149, 228, 84); font-style: italic;">&quot;portrait&quot;</span>); &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">081&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//在2.2以上可以使用 &nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">082&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setDisplayOrientation</span>(<span style="color: rgb(229, 120, 109);">90</span>); &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">083&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span><span style="color: rgb(138, 198, 242);">else</span><span style="color: rgb(246, 243, 232);">{</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">084&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">param</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">set</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;orientation&quot;</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(149, 228, 84); font-style: italic;">&quot;landscape&quot;</span>); &nbsp;
<span style="color: rgb(248, 16, 176);">085&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//在2.2以上可以使用 &nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">086&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setDisplayOrientation</span>(<span style="color: rgb(229, 120, 109);">0</span>); &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">087&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">088&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">previewSizes</span> <span style="color: rgb(246, 243, 232);">=</span> <span style="color: rgb(246, 243, 232);">param</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">getSupportedPreviewSizes</span>();<span style="color: rgb(153, 150, 139); font-style: italic;">//获取支持的预览分辨率</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">089&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setParameters</span>(<span style="color: rgb(246, 243, 232);">param</span>); &nbsp;
<span style="color: rgb(248, 16, 176);">090&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setPreviewDisplay</span>(<span style="color: rgb(246, 243, 232);">holder</span>); &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">091&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> <span style="color: rgb(138, 198, 242);">catch</span> (<span style="color: rgb(246, 243, 232);">Exception</span> <span style="color: rgb(246, 243, 232);">e</span>) <span style="color: rgb(246, 243, 232);">{</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">092&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// 如果出现异常，则释放Camera对象 &nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">093&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">release</span>(); &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">094&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;
<span style="color: rgb(248, 16, 176);">095&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> &nbsp;
<span style="color: rgb(153, 150, 139); font-style: italic;">096&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">startPreview</span>();
<span style="color: rgb(153, 150, 139); font-style: italic;">097&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">098&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">099&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">surfaceDestroyed</span>(<span style="color: rgb(246, 243, 232);">SurfaceHolder</span> <span style="color: rgb(246, 243, 232);">holder</span>) <span style="color: rgb(246, 243, 232);">{</span>
<span style="color: rgb(248, 16, 176);">100&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">101&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setPreviewCallback</span>(<span style="color: rgb(138, 198, 242);">null</span>);
<span style="color: rgb(153, 150, 139); font-style: italic;">102&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">stopPreview</span>();
<span style="color: rgb(153, 150, 139); font-style: italic;">103&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">release</span>();
<span style="color: rgb(153, 150, 139); font-style: italic;">104&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(246, 243, 232);">camera</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(138, 198, 242);">null</span>; &nbsp; &nbsp;
<span style="color: rgb(248, 16, 176);">105&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">106&nbsp;</span>
<span style="color: rgb(153, 150, 139); font-style: italic;">107&nbsp;</span> &nbsp; 
<span style="color: rgb(153, 150, 139); font-style: italic;">108&nbsp;</span><span style="color: rgb(246, 243, 232);">}</span>
</div>

  