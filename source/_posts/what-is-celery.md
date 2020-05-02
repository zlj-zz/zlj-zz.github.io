---
title: Celery 介绍与入门
date: 2020-01-23 22:12:32
tags: celery

---
Do you know what `celery` is? Let's take a look and learn how to use it.
<!--more-->


# Celery简介和原理

**Celery(芹菜)是一个异步任务队列/基于分布式消息传递的作业队列。它侧重于实时操作**，但对调度支持也很好。Celery用于生产系统每天处理数以百万计的任务。Celery是用Python编写的，但该协议可以在任何语言实现。

简单来说就是你可以交给celery一些工作，它可以帮你同时的完成，而你并不需要等着。说白了就是一打工仔，但是celery一个顶很多个。

**Celery是怎么顶多个打工仔的呢？**
其实是因为`celery workers` ，它是Celery的一个工作队列，里面有多少`worker(任务执行单元)`就可以同时干多少件任务。

这时我们又必须说到Celery中的另一个概念了：`broker`。
`broker`是一个消息中间件，可以理解成一个邮箱。如果没有`broker`，当你丢给Celery任务时，如果恰好所有的`worker`都在工作，那么你就必须等待有一个`worker`完成任务才可以。`broker`的引入正好解决可这个问题，你只需要把任务告诉broker，它就会帮你传递给worker。

但这样又出现了新的问题，你无法得知`broker`什么时候传递任务，也无法得知`worker`什么时候完成任务。为此，Celery实现了一个`backend`用于任务完成的结果。
###	原理图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9ickZyUUZ3QUIwWXJldkhBcHBBSG1pYjZkWFFuTXRqQnlFTmJCNE01Y2liRzZXaHUyRGlhZUJUeEJaZTRmWkRyckJ0SGhZY2FDNFh4aWJUeFA1OTV0aWFkRUlRLzY0MA?x-oss-process=image/format,png)
Celery本身不提供消息服务，但是可以方便的使用第三发提供，如：
`RabbitMQ`   ` Redis`     `Amazon SQS`

Worker是Celery提供的任务执行的单元，worker并发的运行在分布式的系统节点中

Celery支持以不同的方式存储任务的结果，如：

`AMQP, redis，memcached, mongodb，SQLAlchemy, Django ORM，Apache Cassandra, IronCache ....`

# Celery的安装
### 安装
您可以通过 python 的 pip 安装或通过源代码进行安装 Celery，使用pip进行安装：
```bash
$ pip install -U Celery

 -U 不存在下载celery，存在更新celery
```

### 捆绑
Celery 自定义了一组用于安装 Celery 和特定功能的依赖。

您可以在中括号加入您需要依赖，并可以通过逗号分割需要安装的多个依赖包。

```bash
$ pip install "celery[librabbitmq]"
$ pip install "celery[librabbitmq,redis,auth,msgpack]"
```

# Celery的简单使用
Celery 上手比较简单，不需要配置文件就可以直接运行，大多数情况下，使用默认的配置就可以满足。

**这是一个简单的 Demo**：

```python
from celery import Celery

# broker指定中间人，我选择的是redis
# backend指定存储任务结果的数据库，我没有使用
app = Celery('hello', broker='redis://127.0.0.1:3679/1')
@app.task
def hello():
   return 'hello world'
```
**然后在shell中启动celery**

```bash
celery -A hello worker --loglevel=info
# -A 是Application的首字母
# hello是应用的所在
# --loglevel=info 指定log等级
```
**使用**
```python
from hello import hello
hello.delay()
```

