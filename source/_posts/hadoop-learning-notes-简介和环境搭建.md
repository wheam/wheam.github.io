title: "Hadoop Learning Notes —— 简介和环境搭建"
id: 337
date: 2013-01-04 00:47:45
tags: 
categories: 
- java
---

进行了三天惨无人道的hadoop学习，应该是得留下点什么，打算出一个系列《Hadoop Learning Notes》，介绍一下本次学习主要使用到的HBase和MapReduce，HDFS虽然不太用得上，但是在搭建环境的时候必须要有，也开一篇简单的文章吧，HBase和MapReduce我会尽量给出示例代码。

<div> </div>
<div>第一篇是简介和环境搭建，简介其实不太想写，百度百科和wiki已经非常好了，不过我还是想把自己的理解整理一下，不一定是完全正确，但是好歹是我学习的结晶，以后再看或许会有些感触。</div>
<div>环境搭建是个很重要的环节，这里给出的示例是自己亲手尝试并且成功的经验，参考了一些大神的文章。</div>
<div> </div>
<div>第二篇是HBase，会简单介绍HBase的原理（--！我自己也才入门，只能讲讲），并且我会给出一个示例代码，演示直接在集群上操作HBase。PS.第一次接触NoSql，表示很新奇！</div>
<div> </div>
<div>第三篇是MapReduce，并行运算的关键，也是我需要深入研究的部分，我可能会不只出一篇，而且这东西确实我包括XDCCL的研究生在内都不怎么熟悉，可能会有些不对的地方，望见谅。</div>
<div> </div>
<div>第四篇是HDFS，我会给出一些示例，但是不会保证这一定是对的，因为我在项目中基本使用不到HDFS，而且分布式文件系统有时候也没那么广的适用范围。</div>
<div> </div>
<div>好吧，开始正题喽。</div>
<!-- more -->
<div> </div>
<div>一、简介：</div>
<div>     接下来的都是凭借个人理解拟写，为了保证各位看官认知的准确性，关于hadoop的准确介绍，请看wiki：http://zh.wikipedia.org/wiki/Hadoop 或者将就下百度百科http://baike.baidu.com/view/908354.htm。</div>
<div>     想要学习了解hadoop的推荐EasyHadoop社区http://www.easyhadoop.org/包括EasyHadoop开源社区3号群145680489。</div>
<div> </div>
<div>     hadoop是一个apache开发维护的分布式基础架构，但是其实hadoop并不能算是一个真正的云平台，主要用于在搜索，分布式存储，分布式计算等方面，优点是可靠且能够使用低廉的计算设备构建集群。</div>
<div>     hadoop的MapReduce，HBase，HDFS是受到google lab的Map/Reduce，BigTable，GFS（Google File System）的启发开发的，前者分别是后者的开源实现（google V5）。</div>
<div> </div>
<div>     集群：hadoop对集群要求很低，甚至可以在一台笔记本上模拟单机或者伪分布式集群，而如果你有多台计算机，完全可以部署一个小型的hadoop集群。hadoop集群基于Linux环境，主要使用java语言开发。</div>
<div> </div>
<div>     HDFS：Hadoop Distributed File System，hadoop的分布式文件系统，由一个NameNode和很多个DataNode组成，由NameNode控制文件系统的名称空间，Client的访问和复制快的映射；DataNode相应来自NameNode的命令，创建，复制，删除文件块，并且由心跳包控制，在断开心跳包的时候，NameNode会复制该DataNode上的文件块，HDFS可靠性的由来。</div>
<div> </div>
<div>     HBase：一个分布式面向列的NoSql，非结构化存储数据库，使用Table存储，适用于对超大规模数据集的实时随机读写，需要采用MapReduce对其存储的海量数据进行处理。HBase提供了十分简单的API用于存取管理。</div>
<div> </div>
<div>     MapReduce：MapReduce是构建在HDFS上的大规模并行处理计算框架，将所有运行于集群上的计算抽象为Map和Reduce两个函数，将处理的数据集分解为多个小数据集，完全并行运算。</div>
<div> </div>
<div>     F&amp;Q</div>
<div>     Q：Hadoop有什么用？</div>
<div>     A：低廉的构建分布式计算架构，搭建集群，解决大数据和并行运算问题。</div>
<div>     Q：Hadoop的优缺点？</div>
<div>     A：优点是可靠，易扩展，低廉，缺点是并不是一个真正的云平台，而且使用范围有限。</div>
<div>     Q：Hadoop和其他云平台如OpenStack，CloudStack等对比？</div>
<div>     A：推荐个文章吧http://wenku.baidu.com/view/887588b9c77da26925c5b0bc.html，其他自己搜索吧，总之，OpenStack是主流。</div>
<div>     Q：你初步接触了Hadoop最大的感受是什么？</div>
<div>     A：这是个简单易懂的平台，而且能够切实的解决SeeMore对大数据和高计算能力的要求，我觉得花上一段时间来学习使用很值得，可惜好的集群没那么容易得到。</div>
<div> </div>
<div> </div>
<div>二、Ubuntu下hadoop伪分布式环境搭建</div>
<div>     首先说，其实这个比想象中的简单，但是包括学习了解和尝试，我还是弄了差不多两天，其中遇到的版本问题让我们蛋疼不已。</div>
<div>     参照了此博客http://blog.sina.com.cn/s/blog_62186b4601011c6a.html</div>
<div> </div>
<div>     1、你需要准备Linux JDK环境，如果已经设置好，请获取JAVA_HOME地址；</div>
<div>     如果没有，请下载JDK 7u10 ，下载地址 http://www.oracle.com/technetwork/java/javase/downloads/index.html </div>
<div> </div>
<div>
<div>     具体配置参考这个吧，讲的十分详细了  http://blog.csdn.net/lorkoy/article/details/7931129</div>
</div>
<div>     </div>
<div>     2、Hadoop安装文件，我使用的版本是hadoop0.20.203.0，下载地址有空再附上吧</div>
<div>     下载好后，解压放在 /home/username/   （username为你的账户名字，比如我的就是wheam）下，文件夹名为了方便，设置为hadoop。</div>
<div>     </div>
<div>     3、修改conf/hadoop-env.sh配置</div>
<div>          在terminal下，输入以下指令</div>
<div>               cd  /home/username/hadoop/conf    // 进入conf文件夹</div>
<div>               sudo gedit hadoop-env.sh    // 会要求输入密码，编辑你的hadoop-env.sh文件，我不会告诉你们我不会用vim</div>
<div>          此时系统会打开gedit编辑器，在文档的最后添加以下几句</div>
<div>               export JAVA_HOME=“步骤1中让你获取的JAVA_HOME地址”，一般默认为/usr/local/你的jdk版本号 </div>
<div>               export HADOOP_HOME=/home/username/hadoop</div>
<div>               export PATH=$PATH:/home/username/hadoop/bin</div>
<div> </div>
<div>          保存退出继续</div>
<div> </div>
<div> </div>
<div><span style="font-family: 微软雅黑;"><span style="color: #ff0000;">PS：</span>在编辑下述的三个xml文档的时候，楼主遇到了个神奇的错误，xml文档开头不能有空行，否则会报错，具体看这个文章</span>[http://hacklin.blogspot.com/2008/10/installing-hadoop-on-ubuntu.html](http://hacklin.blogspot.com/2008/10/installing-hadoop-on-ubuntu.html)<span style="font-family: 微软雅黑;"> （需要翻墙）</span></div>
<div> </div>
<div>     4、配置conf/core-site.xml</div>
<div>          此时如果你是按照步骤来的，那么你还是在/home/username/hadoop/conf 下，如果不是，再次输入命令cd  /home/username/hadoop/conf </div>
<div>          然后输入</div>
<div>          sudo gedit core-site.xml</div>
<div>          接着打开编辑器之后，删除以前的所有内容，输入这段xml代码，注意替换username为你的用户名https://gist.github.com/4449374</div>

<script src="https://gist.github.com/4449374.js"></script>

<div> </div>
<div>     5、配置hdfs-site.xml</div>
<div>          保持在/home/username/hadoop/conf 下，输入</div>
<div>          sudo gedit hdfs-site.xml</div>
<div>          </div>
<div>          打开编辑器后删除所有内容，输入以下xml代码</div>
<div>          https://gist.github.com/4449385</div>
<div>          </div>
<div> </div>
<script src="https://gist.github.com/4449385.js"></script>
<div> </div>
<div>     6、配置mapred-site.xml</div>
<div>          保持在/home/username/hadoop/conf 下，输入</div>
<div>          sudo gedit mapred-site.xml  </div>
<div>          </div>
<div>          打开编辑器后删除所有内容，输入以下xml代码https://gist.github.com/4449392</div>
<div> </div>
<script src="https://gist.github.com/4449392.js"></script></p>
<div><span style="font-family: 微软雅黑;"><span style="color: #ff0000;">PS：重要修改</span> 由于JVM和Hadoop版本不一样的问题，后续使用sudo会报错，所以，</span></div>
<div><span style="font-family: 微软雅黑;">需要进行以下修改</span></div>
<div>         cd /home/username/hadoop/bin </div>
<div>         sudo gedit hadoop</div>
<div>         然后搜索jvm</div>
<div>          在</div>
<div>if [[ $EUID -eq 0 ]]; then</div>

    HADOOP_OPTS="$HADOOP_OPTS -jvm server $HADOOP_DATANODE_OPTS"
   else
     HADOOP_OPTS="$HADOOP_OPTS -server $HADOOP_DATANODE_OPTS"
   fi

<div>里面删除if [[ $EUID -eq 0 ]]; then</p>
<div>    HADOOP_OPTS="$HADOOP_OPTS -jvm server $HADOOP_DATANODE_OPTS"
   else还有最后的fi</div>
<div> </div>
</div>
<div> </div>
<div>     7、格式化NameNode和DataNode</div>
<div>          输入命令</div>
<div>          cd /home/username/hadoop</div>
<div>          sudo bin/hadoop namenode -format</div>
<div>          sudo bin/hadoop datanode -format</div>
<div> </div>
<div>     8、启动所有hadoop进程</div>
<div>          保持在/home/username/hadoop 下输入命令</div>
<div>          sudo bin/start-all.sh </div>
<div>          中间会要求输入密码很多次，等我后面更新不需要密码验证的方法</div>
<div>          同样假如你要关闭hadoop的话，输入命令为：</div>
<div>          sudo bin/stop-all.sh</div>
<div>          </div>
<div>     9、查看hadoop运行情况</div>
<div>          保持在/home/username/hadoop 下输入命令</div>
<div>          jps</div>
<div>          如果正常情况，会现实有NameNode，DataNode，JobTracker，TaskTracker等</div>
<div> </div>
<div>     10、查看集群状态</div>
<div>          保持在/home/username/hadoop 下输入命令</div>
<div>          sudo bin/hadoop dfsadmin -report</div>
<div> </div>
<div>     11、测试WordCount程序</div>
<div>          </p>

&nbsp;

<div>
<div>          A、在hadoop/test下创建txt文件，并且输入多个单词</div>
<div>                cd /home/username/hadoop 并且一直保持在此文件夹内</div>
<div>                sudo  mkdir test       // 创建一个名为test的文件夹</div>
<div>                cd test</div>
<div>                sudo gedit test1.txt</div>
<div>                之后会打开编辑器，在编辑器内输入任意多个单词</div>
<div>               </div>
<div>          B、在HDFS下创建input文件夹</div>
<div>               sudo bin/hadoop fs -mkdir input   // 在HDFS下创建名为input的文件夹</div>
<div>               其他命令</div>
<div>               sudo bin/hadoop fs -ls  // 查看</div>
<div>               sudo bin/hadoop fs -rmr name  // 删除名为name的文件夹</div>
<div>               sudo bin/hadoop fs -rm name  // 删除名为name的文件</div>
<div> </div>
<div>          C、退出hadoop的安全模式，便于之后执行程序</div>
<div>               bin/hadoop dfsadmin -safemode leave</div>
<div> </div>
<div>          D、将test中我们写好的txt文件导入到HDFS中</div>
<div>               bin/hadoop fs -put /home/username/hadoop/test/* input</div>
<div>          </div>
<div>          E、执行hadoop自带的WordCount程序</div>
<div>               bin/hadoop jar hadoop-examples-0.20.203.0.jar wordcount input output</div>
<div> </div>
<div>          F、查看执行结果</div>
<div>               bin/hadoop dfs -cat output/*</div>
</div>
</div>
<div> </div>
<div> </div>
<div> </div>
<div>好的，至此，Hadoop的环境已经搭建好了，可以开始hadoop开发了，但是其实远没有那么简单，敬请期待下一期的HBae介绍及Demo和MapReduce开发详解。</div>
<div> </div>
<div> </div>
<div> </div>
<div> </div>
<div> </div>
<div> </div>
<div> </div>
<div> </div>