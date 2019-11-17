---
title: Build Hadoop && Spark Cluster
date: 2018-07-20 22:39:24
tags:
---
单机配置
* 创建用户和用户组
groupadd hadoop//新建用户组
useradd hadoop -g hadoop//把用户加入用户组
passwd hadoop//为用户hadoop设置密码然后会提示输入密码和确认密码
* 修改hostname和hosts
vim /etc/hostname
vim /etc/hosts
* 修改用户权限
vim /etc/sudoers

* 配置无密码登录
cd ~/.ssh/     //如果目录下没有这个文件夹，需要执行以下ssh localhost生成这个文件夹
ssh-keygen -t rsa //生成key,过程中只管敲回车
cat ./id_rsa.pub >> ./authorized_keys //授权, 有时候非root用户授权会失败，就是直接执行ssh localhost还是会提示输入密码，那么多半是文件权限的问题：chmod 600 .ssh/authorized_keys 修改权限即可
* 安装Java JDK
下载Java JDK: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
设置环境变量: vim /etc/profile,在文件最后加入:
#Java Env
export JAVA_HOME=/usr/local/java/jdk1.8.0_162
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
使配置生效: source /etc/profile
测试java环境:
java
java -version
javac
* 安装Hadoop
下载Hadoop: http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-2.6.5/hadoop-2.6.5.tar.gz
解压Hadoop到/usr/local/目录下：tar -zxf ~/hadoop-2.6.5.tar.gz -C /usr/local
mv hadoop-2.6.5/ hadoop
chown -R hadoop ./hadoop/  //设置权限
./hadoop/bin/hadoop version //查看hadoop安装包是否完整
配置环境变量，vim /etc/profile,在文件的最后加入：
export HADOOP_HOME=/usr/local/hadoop
export PATH=$HADOOP_HOME/bin:/$HADOOP_HOME/sbin:$PATH
* 配置Hadoop
    * 总共配置6个文件: slaves、core-site.xml、hdfs-site.xml、mapred-site.xml、yarn-site.xml
    * slaves: 将作为 DataNode也就是Slave机器的主机名写入该文件，每行一个，默认为 localhost，所以在伪分布式配置时，节点即作为 NameNode 也作为 DataNode。分布式配置可以保留 localhost，也可以删掉，让 Master 节点仅作为 NameNode 使用。 本项目让 Master 节点仅作为 NameNode 使用，因此将文件中原来的 localhost 删除，只添加一行内容：162.105.67.62
    * hadoop-env.sh：添加Java JDK路径
# The java implementation to use.
export JAVA_HOME=/usr/local/java/jdk1.8.0_162
* core-site.xml:
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://HadoopMaster:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
</configuration>
* hdfs-site.xml:dfs.replication 一般设为 3，但我们只有一个 Slave 节点，所以 dfs.replication 的值还是设为 1

<configuration>
        <property>
                <name>dfs.namenode.secondary.http-address</name>
                <value>HadoopMaster:50090</value>
        </property>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>file:/usr/local/hadoop/tmp/dfs/name</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>file:/usr/local/hadoop/tmp/dfs/data</value>
        </property>
</configuration>
* mapred-site.xml:需要将mapred-site.xml.template文件重命名
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.address</name>
                <value>HadoopMaster:10020</value>
        </property>
        <property>
                <name>mapreduce.jobhistory.webapp.address</name>
                <value>HadoopMaster:19888</value>
        </property>
</configuration>
*   yarn-site.xml:
<configuration>

<!-- Site specific YARN configuration properties -->
    <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>HadoopMaster</value>
        </property>
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>
