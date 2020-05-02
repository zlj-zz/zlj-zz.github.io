---
title: Redis 常用命令总结
date: 2020-01-12 18:42:01
category: Database
tags: redis

---
Here are some common commands for redis, similar to a simple manual.
<!--more-->

Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## linux

安装 `sudo apt-get install redis`

启动 `redis-server /etc/redis.conf` or `systemctl start redis.service`

连接 `redis-cli` or `reids-cli -h host -p port -a password`

关闭 `kill -9 PID` or `redis-cli shutdown`

配置文件 `/etc/redis.conf`

>`daemonize` 是否为后台运行，`requirepas` 设置密码, `logfile` 设置log日志位置

---

## windows

redis目录下`redis.windows.conf`

387行添加`requirepass admin123`

455行添加`maxheap 1024000000`

启动命令:`redis-server.exe redis.windows.conf`

连接命令:
```bash
redis-cli.exe	# 连接
auth &apos;admin123&apos;	# 输入密码
```

## key通用操作

`DEL key` : key 存在时删除 key（删除任何类型）

`DUMP key` : 序列化 key，返回序列化的值

`EXISTS key` : 判断 key 是否存在 ，返回值（1：存在，0：不存在）

`EXPIRE key seconds` : 设置 key 过期时间（单位：s）

`PEXPIRE key milliseconds` : 设置 key 过期时间（单位：ms）

`TTL key` : 返回 key 剩余存活时间（单位：s ）

`PTTL key` : 返回 key 剩余存活时间（单位：ms ）

`PERSIST key` : 移除 key 过期时间，持久保持

`KEYS pattern` : 查找符合 pattern 的 key，通配符（`*`：所有，`？`：任一字符）

`RANDOM key` : 当前数据库，随机返回一个 key

`RENAME key newname` : 修改 key 名

`MOVE key db` : 移动 key 到指定数据库

`TYPE key` :  返回 key 所存储值的类型

>推荐：key的命名要有规范 ，如：`users:1:name`

---

## String

String 是 redis 最基本的类型，一个 key 对应一个 value，最大存储 512M，String是`二进制安全`的，可以存储任何数据类型。

>二进制安全是指，在传输数据时，保证二进制信息的安全，不被篡改、破译等，如果被攻击，也能及时检测到。

**设置**

`SET key value ` : 设置键值对，key 存在时覆盖

`SETNX key value ` : 设置键值对，仅当 key 不存在时

`SETEX key seconds value ` : 设置键、过期时间、值

`MSET key value [key value ...]` : 同时设置多个键值对

**获取**

`GET key` : 获取 key 的值

`MGET key [key ...]` : 获取多个 key 的值

`GETRANGE key start end` : 获取 key 值字符串的字串（截取范围有 star t到 end，**start 和 end 都包含在内**）

`GETSET key value` : 设置 key 的值，并返回 key 的旧值，key 不存在返回 null

`STRLEN key` : 返回 key 值的长度

**运算**

value 是数字类型，value 加一减一操作：`INCR key` and `DECR key`

value 加减整数操作：`INCRBY key increment` and `DECRBY key increment`

`APPEND key value` : 在 key 值末尾追加，key 不存在时赋值

---

## Hash存储对象

hash是一个string类型的field和value的映射表，特被适合用于`存储对象`，每个hash可以存储 $2^{32}$-1 键值对。

>hash 是最接近关系型数据库结构的数据类型，可以将一条数据记录或一个对象转换成一个 hashmap 存储在 redis 中。


**设置**

`HSET key field value` : 设置单值（field 和 value 为键值对）

`HMSET key field value [field value ...]` : 同时设置多个值

`HSETNX key field value` : 当 field 不存在才设置值

**获取**

`HGET key field` : 获取指定属性的值

`HMGET key field [field ...]` : 同时获取多个属性的值

`HGETALL key` : 获取所有属性和值

`HKEYS key` : 获取所有属性

`HVALS key` : 获取所有值

`HLEN key` : 获取包含属性个数

`HSTRLEN key fidld` : 获取指定属性的长度

**其他**

`HDEL key field [field ...]` : 删除属性

`HEXISTS key field` : 判断属性是否存在

`HINCRBY key field increment` : value 加整数

`HINCRBYFLOAT key field increment` : value 加浮点数 

---

## list

list的元素类型为String，按照插入顺序排列，在头或尾添加元素。

**设置**

`LPUSH key value [value ...]` 在头部插入

`RPUSH key value [value ...]` 在尾部插入

`LINSERT key BEFORE|AFTER pivot value` 在指定元素 前/后 插入

`LSET key index value` 设置指定索引的元素值

**获取**

`LRANGE key start stop` 返回指定范围中的元素

`LLEN key` 获取包含属性个数

`LINDEX key index` 返回索引对应值

`LPOP key` 返回并删除key对应list的首元素

`RPOP key` 返回并删除key对应list的尾元素

`LTRIM key start stop` 裁剪

`BLPOP key [key ...] timeout` 返回并删除首元素，不存在则等待到超时时间

`BRPOP key [key ...] timeout` 返回并删除尾元素，不存在则等待到超时时间

>0 代表第一个元素，-1 代表最后一个元素

**高级**

`RPOPLPUSH source destination` 移除source尾元素，添加到destination头部，并返回该值

>RPOPLPUSH l1 l1  循环队列

---

## set

set为无序集合，元素类型为String，元素有唯一性

>底层使用了intset和hashtable来那个中数据结构存储，`insert` 理解为数组，`hashtable`就是普通哈希表， `insert`内部是有序的，查找时通过二分查找

**设置**

`SADD key member [member ...]` 添加元素

**获取**

`SMEMBERS key` 获取所有元素

` SCARD key` 获取元素个数

`SISMEMBER key member` 判断元素是否是集合成员

`SRANDMEMBER key [count]` 随机返回count个数成员

**删除**

`SREM key member [member ...]` 删除成员

`SPOP key [count]`  移除并返回 count 个成员

**其他**

`SMOVE source destination member` 将指定 member 从 source 移动到 destination 集合

`SINTER key [key ...]` 返回多个集合交集

`SINTERSTORE destination key [key ...]` 返回多个集合交集，并存储到 destination

`SDIFF key [key ...]` 返回多个集合差集

`SDIFFSTORE destination key [key ...]` 返回多个集合差集，并存储到 destination

`SUNION key [key ...]` 返回多个集合并集

`SUNIONSTROE destination key [key ...]` 返回多个集合并集，并存储到 destination

## zset

zset为有序集合，元素类型为String，元素有唯一性；每个元素有double类型的权重(`score`)，通过权重排序，权重可相同

**设置**

`ZADD key [NX|XX] [CH] [INCR] score member [score member ...]` 添加元素

- XX: 仅仅更新存在的成员，不添加新成员。
- NX: 不更新存在的成员。只添加新成员。
- CH: 修改返回值为发生变化的成员总数，原始是返回新添加成员的总数 (CH 是 changed 的意思)。更改的元素是新添加的成员，已经存在的成员更新分数。 所以在命令中指定的成员有相同的分数将不被计算在内。
- INCR: 当ZADD指定这个选项时，成员的操作就等同ZINCRBY命令，对成员的分数进行递增操作。
>注：在通常情况下，ZADD返回值只计算新添加成员的数量。

**获取**

`ZCARD key` 获取元素个数

`ZSCORE key menber` 返回指定 menber 的 score

`ZRANGE key start stop` 返回指定 index 范围元素（分数低到高）

`ZREVRANGE key start stop` 返回指定 index 范围元素（分数高到低）

`ZCOUNT key min max` 返回指定 score 范围元素个数

`ZRANK key member` 返回指定成员的索引

**删除**

`ZREM key member [member ...]` 移除指定成员

`ZREMRANGEBYSCORE key min max` 移除制定分数间的成员

`ZREMRANGEBYRANK key start stop` 移除指定索引间的成员

---

[redis命令网站](http://redis.cn/commands.html#generic)
