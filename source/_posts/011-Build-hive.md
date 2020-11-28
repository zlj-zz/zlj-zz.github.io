---
title: 部署 hive
date: 2020-03-11 22:01:55
category: Database
tags: Hive
---

如何一步步的搭建 Hive, 选择 MySQL 作为元仓库, 并解决一些搭建过程中遇到的问题. Hive 版本为: 2.3.6

<!--more-->

Hive 是一个数据仓库基础工具在 Hadoop 中用来处理结构化数据。它架构在 Hadoop 之上，总归为大数据，并使得查询和分析方便。并提供简单的 sql 查询功能，可以将 sql 语句转换为 MapReduce 任务进行运行。

MapReduce 程序对于 Java 程序员来说比较容易写，但是对于其他语言使用者来说不太方便。Hive 让对 Hadoop 使用 SQL 查询（实际上 SQL 后台转化为了 MapReduce）成为可能，那些非 Java 程序员也可以更方便地使用。

### 下载 hive

hive 是基于 hadoop，所以要先安装 hadoop， [hadoop 安装部署](https://blog.csdn.net/qq_38410494/article/details/104801399)
下载 hive 前要查看支持的版本
下载地址，北京理工大学开源软件镜：[http://mirror.bit.edu.cn/apache/hive/](http://mirror.bit.edu.cn/apache/hive/)
解压 hive：`tar -xzvf apache-hive-2.3.6-bin.jar`

### hive 配置

1. **配置 hive 环境变量**
   打开 `/etc/profile`
   `bash export HIVE_HOME=/opt/module/hive-2.3.6-bin PATH=$PATH:$HIVE_HOME/bin `
   重新编译 `profile'`， `source /etc/profile`
2. **hive 配置文件**
   hive 的配置文件在 hive 目录下的 `conf` 中
   **a**. `cp hive-env.sh.template hive-env.sh`
   **b**. `cp hive-log4j2.properties.template hive-log4j2.propertie`
   **c**. `cp hive-default.xml.template hive-site.xml`
   修改 `hive-site.xml` ，这里以 2.3.6 为例：
   544 行，连接数据库设置，默认 derby
   `xml <name>javax.jdo.option.ConnectionURL</name> <value>jdbc:derby:;databaseName=metastore_db;create=true</value> 修改 <name>javax.jdo.option.ConnectionURL</name> <value>jdbc:mysql://127.0.0.1:3306/hive?allowPublicKeyPublicKeyRetrieval=true&amp;userSSL=false</value> `
   1019 行，连接驱动设置
   `xml <name>javax.jdo.option.ConnectionDriverName</name> <value>org.apache.derby.jdbc.EmbeddedDriver</value> 修改 <name>javax.jdo.option.ConnectionDriverName</name> <!-- 这里使用mysql8.0, 低版本为 com.mysql.jdbc.Driver--> <value>com.mysql.cj.jdbc.Driver</value> `
   1044 行，设置用户名
   `xml <name>javax.jdo.option.ConnectionUserName</name> <value>root</value> `
   529 行，设置密码
   `xml <name>javax.jdo.option.ConnectionPassword</name> <value>mysql</value> `
   配置缓存目录，共三个，分别在 74、80、3975 行，将 `${system:java.io.tmpdir}` 换成具体目录
   `bash <name>hive.exec.local.scratchdir</name> <value>${system:java.io.tmpdir}/${system:user.name}</value> <name>hive.downloaded.resources.dir</name> <value>${system:java.io.tmpdir}/${hive.session.id}_resources</value> <name>hive.server2.logging.operation.log.location</name> <value>${system:java.io.tmpdir}/${system:user.name}/operation_logs</value> `
3. **添加驱动**
   驱动下载地址：[http://mvnrepository.com/artifact/mysql/mysql-connector-java](http://mvnrepository.com/artifact/mysql/mysql-connector-java)
   下载文件：mysql-connector-java-8.0.17.jar，将驱动添加到：hive 目录下的 `/lib` 中

### 启动

1. 初始化 `schematool -dbType mysql -initSchema`

   ```
   SLF4J: Class path contains multiple SLF4J bindings.
   SLF4J: Found binding in [jar:file:/opt/module/hive-2.3.6-bin/lib/log4j-slf4j-impl-2.6.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: Found binding in [jar:file:/opt/module/hadoop-2.8.5/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
   SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
   SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
   Metastore connection URL:        jdbc:mysql://192.168.228.128:3306/onhive?
   Metastore Connection Driver :    com.mysql.cj.jdbc.Driver
   Metastore connection User:       hive
   Starting metastore schema initialization to 2.3.0
   Initialization script hive-schema-2.3.0.mysql.sql
   Initialization script completed
   schemaTool completed
   ```

   初始化成功

2. 启动 ，命令行输入 `hive`

   ```
   Logging initialized using configuration in file:/home/zachary/hive-2.3.6/conf/hive-log4j2.properties Async: true
   Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
   hive>
   ```

   成功启动

3. 推出 `quit;` or `exit;`

### 解决问题

- [ ] 遇到问题 1
      hive 中使用 `show tables;` ，出现报错：

```
Failed with exception java.io.IOException:java.lang.IllegalArgumentException: java.net.URISyntaxException: Relative path in absolute URI: ${system:user.name%7D
```

解决，修改配置文件：

```xml
<!-- 修改前 -->
<name>hive.exec.local.scratchdir</name>
<value>/home/lch/software/Hive/apache-hive-2.1.1-bin/tmp/${ｓｙstem:user.name}</value>

<!-- 修改后 -->
 <name>hive.exec.local.scratchdir</name>
<value>/home/lch/software/Hive/apache-hive-2.1.1-bin/tmp/${user.name}</value>
```
