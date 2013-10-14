title: "ArrayAdapter实现简单ListView"
id: 268
date: 2012-12-15 00:40:17
tags: 
categories: 
- android
---

&nbsp;

<div>
	<span style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">为了教某绿ListView，我自己写了个示例教程，参看了部分google检索到的代码，但是比较乱，最后还是直接参照文档写的。</span>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		其实ListView一直没有真正实际运用到程序中过（大部分写的都比较奇葩，非信息呈现类），看了很多软件，尤其面包旅行这种，感觉ListView实现大图片应用也是十分好看的，嗯呢。
	</div>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		一样的，直接看代码和注释
	</div>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		&nbsp;
	</div>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		首先还是布局文件：
	</div>
	<!-- more -->
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		&nbsp;
	</div>
	<div class="source" style="font-family: Consolas; color: rgb(246, 243, 232); background-color: rgb(0, 0, 0);">
		<span style="font-size:14px;"><span style="color: rgb(153, 150, 139); font-style: italic;">01&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;LinearLayout</span> <span style="color: rgb(202, 230, 130);">xmlns:android=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;http://schemas.android.com/apk/res/android&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">02&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">xmlns:tools=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;http://schemas.android.com/tools&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">03&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;match_parent&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">04&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;match_parent&quot;</span>

		<span style="color: rgb(248, 16, 176);">05&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:orientation=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;vertical&quot;</span> <span style="color: rgb(138, 198, 242);">&gt;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">06&nbsp;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">07&nbsp;</span> &nbsp;

		<span style="color: rgb(153, 150, 139); font-style: italic;">08&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;EditText</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">09&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

		<span style="color: rgb(248, 16, 176);">10&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">11&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/edit&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">12&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:hint=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;随便填写个&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">13&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">/&gt;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">14&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;Button</span>

		<span style="color: rgb(248, 16, 176);">15&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/bn&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">16&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">17&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">18&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:text=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;点击添加&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">19&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">/&gt;</span>

		<span style="color: rgb(248, 16, 176);">20&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">&lt;ListView</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">21&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_width=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;fill_parent&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">22&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:layout_height=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;wrap_content&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">23&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">android:id=</span><span style="color: rgb(149, 228, 84); font-style: italic;">&quot;@+id/list&quot;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">24&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">/&gt;</span>

		<span style="color: rgb(248, 16, 176);">25&nbsp;</span>

		<span style="color: rgb(153, 150, 139); font-style: italic;">26&nbsp;</span><span style="color: rgb(138, 198, 242);">&lt;/LinearLayout&gt;</span></span>
	</div>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		&nbsp;
	</div>
	<div style="color: rgb(0, 0, 0); font-family: 'Microsoft YaHei UI'; text-align: -webkit-auto; font-size: medium;">
		代码实现：
	</div>
</div>
<div class="source" style="font-family: Consolas; color: rgb(246, 243, 232); background-color: rgb(0, 0, 0);">
	<span style="font-size:14px;"><span style="color: rgb(153, 150, 139); font-style: italic;">001&nbsp;</span><span style="color: rgb(246, 243, 232);">package</span> <span style="color: rgb(246, 243, 232);">com</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">listviewtry</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">002&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">003&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">java.util.ArrayList</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">004&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">java.util.List</span>;

	<span style="color: rgb(248, 16, 176);">005&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">006&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">007&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">008&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.os.Bundle</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">009&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.app.Activity</span>;

	<span style="color: rgb(248, 16, 176);">010&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.app.AlertDialog</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">011&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.content.DialogInterface</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">012&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.database.DataSetObserver</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">013&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.Menu</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">014&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.View</span>;

	<span style="color: rgb(248, 16, 176);">015&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.View.OnClickListener</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">016&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.view.ViewGroup</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">017&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.AdapterView</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">018&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.AdapterView.OnItemClickListener</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">019&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.AdapterView.OnItemLongClickListener</span>;

	<span style="color: rgb(248, 16, 176);">020&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.ArrayAdapter</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">021&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.Button</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">022&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.EditText</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">023&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.ListAdapter</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">024&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.ListView</span>;

	<span style="color: rgb(248, 16, 176);">025&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.SimpleAdapter</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">026&nbsp;</span><span style="color: rgb(246, 243, 232);">import</span> <span style="color: rgb(246, 243, 232);">android.widget.Toast</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">027&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">028&nbsp;</span><span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(138, 198, 242);">class</span> <span style="color: rgb(246, 243, 232);">MainActivity</span> <span style="color: rgb(138, 198, 242);">extends</span> <span style="color: rgb(246, 243, 232);">Activity</span> <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">029&nbsp;</span>

	<span style="color: rgb(248, 16, 176);">030&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">ListView</span> <span style="color: rgb(246, 243, 232);">listView</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">031&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">List</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">String</span><span style="color: rgb(246, 243, 232);">&gt;</span> <span style="color: rgb(246, 243, 232);">listString</span>; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//ArrayAdapter只能固定显示String，此List只为存储信息</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">032&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(246, 243, 232);">EditText</span> <span style="color: rgb(246, 243, 232);">edit</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">033&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">ArrayAdapter</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">String</span><span style="color: rgb(246, 243, 232);">&gt;</span> <span style="color: rgb(246, 243, 232);">adapter</span>; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//Adapter是连接ListView数据的关键</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">034&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">@Override</span>

	<span style="color: rgb(248, 16, 176);">035&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onCreate</span>(<span style="color: rgb(246, 243, 232);">Bundle</span> <span style="color: rgb(246, 243, 232);">savedInstanceState</span>) <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">036&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">super</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">onCreate</span>(<span style="color: rgb(246, 243, 232);">savedInstanceState</span>);

	<span style="color: rgb(153, 150, 139); font-style: italic;">037&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">setContentView</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">layout</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">activity_main</span>);

	<span style="color: rgb(153, 150, 139); font-style: italic;">038&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listView</span><span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">ListView</span>)<span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">list</span>);<span style="color: rgb(153, 150, 139); font-style: italic;">//获取布局文件中已经写好的ListView</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">039&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listString</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">ArrayList</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">String</span><span style="color: rgb(246, 243, 232);">&gt;();</span>

	<span style="color: rgb(248, 16, 176);">040&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listString</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">add</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;这是第一个&quot;</span>);<span style="color: rgb(153, 150, 139); font-style: italic;">//这个就是单纯为了保证ListView里面开始不会什么都没有</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">041&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">adapter</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">ArrayAdapter</span><span style="color: rgb(246, 243, 232);">&lt;</span><span style="color: rgb(246, 243, 232);">String</span><span style="color: rgb(246, 243, 232);">&gt;(</span><span style="color: rgb(138, 198, 242);">this</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(246, 243, 232);">android</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">layout</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">simple_list_item_1</span><span style="color: rgb(246, 243, 232);">,</span><span style="color: rgb(246, 243, 232);">listString</span>);

	<span style="color: rgb(153, 150, 139); font-style: italic;">042&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//new ArrayAdapter&lt;String&gt;(context, textViewResourceId, objects)中的三个参数</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">043&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//context 官方文档解释是The current context. 一直想不通，看到大家用this就跟着用了，求大神解释</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">044&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//textViewResourceId是指ListView中每一个Item的布局，我代码里使用的是默认的单行文字布局，没有自定义，可以自定义</span>

	<span style="color: rgb(248, 16, 176);">045&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//最后的objects就是你的数据，一个String数组或者其他数据容器，大部分示例代码中会使用一个自定义getData（）方法来返回一个设定好的ArrayList&lt;String&gt;，我这里直接使用的自定义好的</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">046&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">047&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listView</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setAdapter</span>(<span style="color: rgb(246, 243, 232);">adapter</span>);<span style="color: rgb(153, 150, 139); font-style: italic;">//设置适配器</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">048&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">049&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(248, 16, 176);">050&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">Button</span> bn<span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">Button</span>)<span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span>bn);

	<span style="color: rgb(153, 150, 139); font-style: italic;">051&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">edit</span><span style="color: rgb(246, 243, 232);">=(</span><span style="color: rgb(246, 243, 232);">EditText</span>)<span style="color: rgb(246, 243, 232);">findViewById</span>(<span style="color: rgb(246, 243, 232);">R</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">id</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">edit</span>);

	<span style="color: rgb(153, 150, 139); font-style: italic;">052&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">053&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;bn<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setOnClickListener</span>(<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">OnClickListener</span>() <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">054&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(248, 16, 176);">055&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onClick</span>(<span style="color: rgb(246, 243, 232);">View</span> <span style="color: rgb(246, 243, 232);">v</span>) <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">056&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">057&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">String</span> <span style="color: rgb(246, 243, 232);">tempString</span><span style="color: rgb(246, 243, 232);">=</span><span style="color: rgb(246, 243, 232);">edit</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">getText</span><span style="color: rgb(246, 243, 232);">().</span><span style="color: rgb(202, 230, 130);">toString</span>();

	<span style="color: rgb(153, 150, 139); font-style: italic;">058&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listString</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">add</span>(<span style="color: rgb(246, 243, 232);">tempString</span>);

	<span style="color: rgb(153, 150, 139); font-style: italic;">059&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">adapter</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">notifyDataSetChanged</span>();<span style="color: rgb(153, 150, 139); font-style: italic;">//此方法为更新ListView</span>

	<span style="color: rgb(248, 16, 176);">060&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">061&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">});</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">062&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">063&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listView</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setOnItemClickListener</span>(<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">OnItemClickListener</span>() <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">064&nbsp;</span>

	<span style="color: rgb(248, 16, 176);">065&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onItemClick</span>(<span style="color: rgb(246, 243, 232);">AdapterView</span><span style="color: rgb(246, 243, 232);">&lt;?&gt;</span> <span style="color: rgb(246, 243, 232);">arg0</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(246, 243, 232);">View</span> <span style="color: rgb(246, 243, 232);">arg1</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">arg2</span><span style="color: rgb(246, 243, 232);">,</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">066&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">long</span> <span style="color: rgb(246, 243, 232);">arg3</span>) <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">067&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">068&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">Toast</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">makeText</span>(<span style="color: rgb(246, 243, 232);">getApplicationContext</span><span style="color: rgb(246, 243, 232);">(),</span> <span style="color: rgb(246, 243, 232);">listString</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">get</span>(<span style="color: rgb(246, 243, 232);">arg2</span><span style="color: rgb(246, 243, 232);">),</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">069&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span style="color: rgb(246, 243, 232);">Toast</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">LENGTH_SHORT</span><span style="color: rgb(246, 243, 232);">).</span><span style="color: rgb(202, 230, 130);">show</span>();

	<span style="color: rgb(248, 16, 176);">070&nbsp;</span> &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">071&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">072&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">});</span><span style="color: rgb(153, 150, 139); font-style: italic;">//ListView的OnItemClickListener实现，点击单个Item后触发事件，主要的参数是int arg2为点击的ID，ListView中的第几个Item</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">073&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">074&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listView</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setOnItemLongClickListener</span>(<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">OnItemLongClickListener</span>() <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(248, 16, 176);">075&nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">076&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">boolean</span> <span style="color: rgb(202, 230, 130);">onItemLongClick</span>(<span style="color: rgb(246, 243, 232);">AdapterView</span><span style="color: rgb(246, 243, 232);">&lt;?&gt;</span> <span style="color: rgb(246, 243, 232);">arg0</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(246, 243, 232);">View</span> <span style="color: rgb(246, 243, 232);">arg1</span><span style="color: rgb(246, 243, 232);">,</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">077&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">arg2</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">long</span> <span style="color: rgb(246, 243, 232);">arg3</span>) <span style="color: rgb(246, 243, 232);">{</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">078&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">// TODO Auto-generated method stub</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">079&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(248, 16, 176);">080&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">OpenOutDialog</span>(<span style="color: rgb(246, 243, 232);">arg2</span>);<span style="color: rgb(153, 150, 139); font-style: italic;">//实现对话框，详见下</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">081&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">082&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">return</span> <span style="color: rgb(138, 198, 242);">false</span>;

	<span style="color: rgb(153, 150, 139); font-style: italic;">083&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">084&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">});</span><span style="color: rgb(153, 150, 139); font-style: italic;">//ListView的OnItemLongClickListener实现，同前者</span>

	<span style="color: rgb(248, 16, 176);">085&nbsp;</span> &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">086&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">087&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">088&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">089&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span>

	<span style="color: rgb(248, 16, 176);">090&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">private</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">OpenOutDialog</span>( <span style="color: rgb(138, 198, 242);">final</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">arg2</span><span style="color: rgb(246, 243, 232);">){</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">091&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">AlertDialog</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">Builder</span>(<span style="color: rgb(246, 243, 232);">MainActivity</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">this</span>) &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">092&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setTitle</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;温馨提示&quot;</span>) &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">093&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setMessage</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;是否删除这个条目&quot;</span>) &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">094&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setPositiveButton</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;确定&quot;</span><span style="color: rgb(246, 243, 232);">,</span> &nbsp;

	<span style="color: rgb(248, 16, 176);">095&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">DialogInterface</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">OnClickListener</span><span style="color: rgb(246, 243, 232);">(){</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">096&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onClick</span>( &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">097&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">DialogInterface</span> <span style="color: rgb(246, 243, 232);">dialoginterface</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">){</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">098&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//这里写确定键按下的处理 &nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">099&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">listString</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">remove</span>(<span style="color: rgb(246, 243, 232);">arg2</span>);

	<span style="color: rgb(248, 16, 176);">100&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">adapter</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">notifyDataSetChanged</span>();

	<span style="color: rgb(153, 150, 139); font-style: italic;">101&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">102&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">})</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">103&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">setNegativeButton</span>(<span style="color: rgb(149, 228, 84); font-style: italic;">&quot;取消&quot;</span><span style="color: rgb(246, 243, 232);">,</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">104&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">new</span> <span style="color: rgb(246, 243, 232);">DialogInterface</span><span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">OnClickListener</span><span style="color: rgb(246, 243, 232);">(){</span> &nbsp;

	<span style="color: rgb(248, 16, 176);">105&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(138, 198, 242);">public</span> <span style="color: rgb(202, 230, 130);">void</span> <span style="color: rgb(202, 230, 130);">onClick</span>( &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">106&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">DialogInterface</span> <span style="color: rgb(246, 243, 232);">dialoginterface</span><span style="color: rgb(246, 243, 232);">,</span> <span style="color: rgb(202, 230, 130);">int</span> <span style="color: rgb(246, 243, 232);">i</span><span style="color: rgb(246, 243, 232);">){</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">107&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(153, 150, 139); font-style: italic;">//这里写取消键按下的处理 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</span>

	<span style="color: rgb(153, 150, 139); font-style: italic;">108&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">109&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> &nbsp;

	<span style="color: rgb(248, 16, 176);">110&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">})</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">111&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">.</span><span style="color: rgb(202, 230, 130);">show</span>(); &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">112&nbsp;</span> &nbsp; &nbsp;<span style="color: rgb(246, 243, 232);">}</span> &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">113&nbsp;</span> &nbsp; &nbsp; &nbsp; &nbsp;

	<span style="color: rgb(153, 150, 139); font-style: italic;">114&nbsp;</span><span style="color: rgb(246, 243, 232);">}</span></span>
</div>

	&nbsp;