---
title: 搭建 Hadoop 平台
date: 2020-03-11 09:18:59
category: Database
tags: Hadoop

---
如何一步步的搭建 Hadoop 平台, 并解决一些搭建过程中遇到的问题. Hadoop 版本为: 2.10
<!--more-->

### 下载hadoop
我这里是下载的 hadoop-2.10,从北京理工大学开源软件镜像，官网下载你懂得....
地址: [http://mirror.bit.edu.cn/apache/hadoop/](http://mirror.bit.edu.cn/apache/hadoop/)

下载完成后解压， `tar -xzvf hadoop-2.10.0.tar.gz`，这里我将hadoop直接放在加目录下。
<table>
	<td>伪分布配置</td>
</table>

### 配置hadoop
**配置文件路径在** `hadoop目录/etc/hadoop`  **中**
*hadooop-env.sh* 中25行左右，配置 `JAVA_HOME`
```bash
# The java implementation to use.
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk
```

*core-site.xml*
```xml
<configuration>
        <!-- 指定HDFS的nameservice -->
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://zachary-pc:9000</value>
        </property>
		<!-- 缓冲区大小 -->
        <property>
                <name>io.file.buffer.size</name>
                <value>131072</value>
        </property>
        <!-- 指定临时文件目录 -->
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/home/zachary/hadoop-2.10.0/tmp</value>
        </property>
</configuration>
```
*hdfs-site.xml*
```xml
<configuration>
<!-- configuration for NameNode:-->
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>/home/zachary/hadoop-2.10.0/name</value>
        </property>
        <!-- 块大小 -->
        <property>
                <name>dfs.blocksize</name>
                <value>268435456</value>
        </property>
        <property>
                <name>dfs.namenode.handler.count</name>
                <value>100</value>
        </property>
<!-- configuration for DateNode:-->
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>/home/zachary/hadoop-2.10.0/data</value>
        </property>
        <!-- 指定HDFS副本的数量 -->
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
</configuration>
```
*mapred-site.xml*
```xml
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
</configuration>
```
yarn-site.xml
```xml
<configuration>
<!-- Site specific YARN configuration properties -->
        <!-- configuration for ResourceManger: -->
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>zahcary-pc</value>
        </property>
        <!-- configuration of NodeManager: reducer获取数据方式 -->
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
      <!-- 配置外网只需要替换外网ip为真实ip，否则默认为 localhost:8088 -->
	  <!-- <property>
	          <name>yarn.resourcemanager.webapp.address</name>
	          <value>外网ip:8088</value>
	  </property> -->
</configuration>
```

格式化namenode，在 `hadoop目录/bin`，命令：`hdfs namenode -format` 。
启动命令在 `hadoop目录/sbin` 下，
启动hadoop：`start-all.sh`
关闭hadoop：`stop-all.sh`
>启动HDFS  start-dfs.sh
>启动YARN  start-yarn.sh
>启动NameNode  hadoop-daemon.sh start namenode
>启动DataNode  hadoop-daemon.sh start datanode
>启动ResourceManager  yarn-daemon.sh start resourcemanager
>启动NodeManager  yarn-daemon.sh start nodemanager

 - [ ] 遇到问题 1
```
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.apache.ibatis.reflection.Reflector (file:/E:/my_java_jar/maven_repository/org/mybatis/mybatis/3.4.5/mybatis-3.4.5.jar) to method java.lang.Class.checkPackageAccess(java.lang.SecurityManager,java.lang.ClassLoader,boolean)
WARNING: Please consider reporting this to the maintainers of org.apache.ibatis.reflection.Reflector
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
```
`警告非法反射访问错误`，我这里将`jdk-13`换成`jdk-8`解决了问题
### 验证启动
```
~/hadoop-2.10.0 ❯❯❯ jps                                             
5153 Jps
4564 NodeManager
4280 SecondaryNameNode
3993 NameNode
4090 DataNode
4462 ResourceManager
```
命令行输入`jps`，有以下6个进程就是成功启动了
通过浏览器输入：`http://ip:50070/` ，登入HDFS管理界面
`http://ip:8088/`，节点管理
### 配置ssh免密登录
命令行输入：`ssh-keygen -t rsa` 生成公钥密钥对，一直按回车直到生成结束。
执行结束后两个文件 `id_rsa` 和 `id_rsa.pub` 其中前者为私钥，后者为公钥，文件位置在当前用户家目录的 `.ssh` 隐藏文件夹下。
然后命令行输入：`cat id_rsa.pub >> authorized_keys`
 - [ ] 遇到问题 2
```
ssh: connect to host zachary-pc port 22: Connection refused
```
我这里是因为没有开启`sshd服务`，启动命令 `service sshd restart` 或者 `systemctl start sshd.service`
### 全局启动hadoop
在上面中，启动hadoop要在hadoop所在目录。
我们可以同过配置环境变量，使hadoop可以全局启动。
`/etc/profile` 中添加：
```bash
export HADOOP_HOME=/home/zachary/hadoop-2.10.0  # hadoop所在目录
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```
重新编译 `profile` ，命令 ：`source /etc/profile`
 - [ ] 遇到问题 3
```
Permission denied: user=dr.who, access=READ_EXECUTE, inode="/tmp":root:super
```
缺失权限; 解决办法：执行 `hdfs dfs -chmod -R 755 /tmp`
<table>
	<td>分布配置</td>
</table>

### 主机名配置
在 `/etc/hosts` 配置主机名，每一台服务器一样，类似：
```bash
192.168.10.1    master
192.168.10.2   slave_1
192.168.10.3   slave_2
```
### ssh配置
将上面配置的公钥复制到从属服务器，命令：
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub master
ssh-copy-id -i ~/.ssh/id_rsa.pub slave_1
ssh-copy-id -i ~/.ssh/id_rsa.pub slave_2
```
### hadoop配置
修改 *core-site.xml* :
```xml
  <property>
      <name>fs.defaultFS</name>
      <value>hdfs://master:9000</value>
  </property>
```
### 验证启动
启动命令同上，只需在主服务器启动即可。
`jps`命令查看，从属服务器有一下进程即启动成功：
```
~$ jps    
4545 NodeManager
4371 DataNode
4678 Jps
```

