

# 什么是NoSQL

NoSQL = Not Only SQL （不仅仅是SQL）
泛指非关系型数据库，随着web2.0互联网的诞生！传统的关系型数据库很难对付web2.0时代！尤其
是超大规模的高并发的社区！ 暴露出来很多难以克服的问题，NoSQL在当今大数据环境下发展的十分迅
速，Redis是发展最快的，而且是我们当下必须要掌握的一个技术！
很多的数据类型用户的个人信息，社交网络，地理位置。这些数据类型的存储不需要一个固定的格式！
不需要多余的操作就可以横向扩展的 ！ Map<String,Object> 使用键值对来控制

## NoSQL 特点

解耦！
1、方便扩展（数据之间没有关系，很好扩展！）
2、大数据量高性能（Redis 一秒写8万次，读取11万，NoSQL的缓存记录级，是一种细粒度的缓存，性
能会比较高！）
3、数据类型是多样型的！（不需要事先设计数据库！随取随用！如果是数据量十分大的表，很多人就无
法设计了！）
4、传统 RDBMS 和 NoSQL
了解：3V+3高
大数据时代的3V：主要是描述问题的

1. 海量Volume
2. 多样Variety
3. 实时Velocity

大数据时代的3高：主要是对程序的要求

1. 高并发
2. 高可扩
3. 高性能

## NoSQL的四大分类

**键值(** [*Key-Value*](http://baike.baidu.com/view/6728383.htm) **)存储[数据库](http://baike.baidu.com/view/1088.htm)**

这一类数据库主要会使用到一个 [哈希表](http://baike.baidu.com/view/329976.htm)，这个表中有一个特定的键和一个指针指向特定的数据。Key/value模型对于IT系统来说的优势在于简单、易部署。但是如果 [DBA](http://baike.baidu.com/subview/67156/5112091.htm)只对部分值进行查询或更新的时候，Key/value就显得效率低下了。 [3] 举例如：Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB.

**列存储数据库。**

这部分数据库通常是用来应对分布式存储的海量数据。键仍然存在，但是它们的特点是指向了多个列。这些列是由列家族来安排的。如：Cassandra, HBase, Riak.

**文档型数据库**

文档型数据库的灵感是来自于Lotus Notes办公软件的，而且它同第一种键值存储相类似。该类型的数据模型是版本化的文档，半结构化的文档以特定的格式存储，比如JSON。文档型数据库可 以看作是键值数据库的升级版，允许之间嵌套键值。而且文档型数据库比键值数据库的查询效率更高。如：CouchDB, MongoDb. 国内也有文档型数据库SequoiaDB，目前已经开源。

**图形(Graph)数据库**

图形结构的数据库同其他行列以及刚性结构的SQL数据库不同，它是使用灵活的图形模型，并且能够扩展到多个服务器上。NoSQL数据库没有标准的查询语言(SQL)，因此进行数据库查询需要制定数据模型。许多NoSQL数据库都有REST式的数据接口或者查询API。 [2] 如：Neo4J, InfoGrid, Infinite Graph.

因此，我们总结NoSQL数据库在以下的这几种情况下比较适用：1、数据模型比较简单；2、需要灵活性更强的IT系统；3、对数据库性能要求较高；4、不需要高度的数据一致性；5、对于给定key，比较容易映射复杂值的环境。

NoSQL数据库的四大分类表格分析

|         **分类**         |                  **Examples举例**                  |                         典型应用场景                         |                      数据模型                      |                             优点                             |                             缺点                             |
| :----------------------: | :------------------------------------------------: | :----------------------------------------------------------: | :------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| **键值（key-value）[3]** | Tokyo Cabinet/Tyrant, Redis, Voldemort, Oracle BDB | 内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等等。[3] | Key 指向 Value 的键值对，通常用hash table来实现[3] |                          查找速度快                          |      数据无结构化，通常只被当作字符串或者二进制数据[3]       |
|   **列存储数据库[3]**    |               Cassandra, HBase, Riak               |                       分布式的文件系统                       |         以列簇式存储，将同一列数据存在一起         |         查找速度快，可扩展性强，更容易进行分布式扩展         |                         功能相对局限                         |
|   **文档型数据库[3]**    |                  CouchDB, MongoDb                  | Web应用（与Key-Value类似，Value是结构化的，不同的是数据库能够了解Value的内容） |      Key-Value对应的键值对，Value为结构化数据      | 数据结构要求不严格，表结构可变，不需要像关系型数据库一样需要预先定义表结构 |            查询性能不高，而且缺乏统一的查询语法。            |
| **图形(Graph)数据库[3]** |          Neo4J, InfoGrid, Infinite Graph           |           社交网络，推荐系统等。专注于构建关系图谱           |                       图结构                       |     利用图结构相关算法。比如最短路径寻址，N度关系查找等      | 很多时候需要对整个图做计算才能得出需要的信息，而且这种结构不太好做分布式的集群方案。[3] |

[原文连接](https://blog.csdn.net/chenleixing/article/details/43192639)

# Redis入门

## 概述

**Redis是什么？**

**百度百科**

Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C语言)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/数据库/103728)，并提供多种语言的API。

**官网介绍**

[Redis官网](https://redis.io/)

[Redis中文网](http://www.redis.cn/)

Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

## Linux安装Redis

1. 下载安装包`redis-5.0.8.tar.gz`

2. `tar -zxvf redis-5.0.8.tar.gz`解压到`/home/redis`目录下

~~~bash
[root@VM_0_2_centos redis]# cd redis-5.0.8/
[root@VM_0_2_centos redis-5.0.8]# ls
00-RELEASENOTES  CONTRIBUTING  deps     Makefile   README.md   runtest          runtest-moduleapi  sentinel.conf  tests
BUGS             COPYING       INSTALL  MANIFESTO  redis.conf  runtest-cluster  runtest-sentinel   src            utils
~~~

3.  安装基本环境

~~~bash
yum install gcc-c++
make
make install
~~~

4. redis的默认安装路径`/usr/local/bin`
5. 将redis配置文件redis.conf。复制到` /usr/local/bin`目录下
6. redis默认不是后台启动的，修改配置文件！

~~~bash
[root@VM_0_2_centos bin]# vim redis.conf
# 输入 /daemonize 查找到当前行 点击i键修改
daemonize yes #改为yes
# 点击ESC键 输入:wq退出即可
~~~

7. 启动Redis服务`redis-server redis.conf`

~~~bash
[root@VM_0_2_centos bin]# ps -ef | grep redis
root     12818 29140  0 14:31 pts/0    00:00:00 grep --color=auto redis
root     16001     1  0 May06 ?        04:44:02 redis-server 127.0.0.1:6379
~~~

7. 使用Redis客户端进行测试

~~~bash
[root@VM_0_2_centos bin]# redis-cli -p 6379
127.0.0.1:6379> shutdown # 关闭服务

~~~

## Redis自带的压测工具

**redis-benchmark**是官方自带的性能测试工具，我们可以设置相关参数进行性能测试。

**命令参数**

------

| 选项 | 描述                                       | 默认值    |
| ---- | ------------------------------------------ | --------- |
| -h   | 指定服务器主机名                           | 127.0.0.1 |
| -p   | 指定服务器端口                             | 6379      |
| -s   | 指定服务器 socket                          |           |
| -c   | 指定并发连接数                             | 50        |
| -n   | 指定请求数                                 | 10000     |
| -d   | 以字节的形式指定 SET/GET 值的数据大小      | 3         |
| -k   | 1=keep alive 0=reconnect                   | 1         |
| -r   | SET/GET/INCR 使用随机 key, SADD 使用随机值 |           |
| -P   | 通过管道传输 请求                          | 1         |
| -q   | 强制退出 redis。仅显示 query/sec 值        |           |
| –csv | 以 CSV 格式输出                            |           |
| -l   | 生成循环，永久执行测试                     |           |
| -t   | 仅运行以逗号分隔的测试命令列表。           |           |
| -I   | Idle 模式。仅打开 N 个 idle 连接并等待。   |           |

服务器（1核2G）

~~~bash
[root@VM_0_2_centos bin]# redis-benchmark -h localhost -p 6379 -c 100 -n 100000 # 测试：100个并发连接 100000请求
====== PING_INLINE ======
  100000 requests completed in 3.80 seconds # 对我们的10万进行写入测试，用了3.80秒
  100 parallel clients # 100个并发客户端
  3 bytes payload # 每次写三个字节
  keep alive: 1 # 只有一台服务器来处理这些请求

3.90% <= 1 milliseconds
71.28% <= 2 milliseconds 
100.00% <= 45 milliseconds 
26301.95 requests per second # 平均每秒处理26301.95次请求

~~~



## 基础知识

~~~java
127.0.0.1:6379> flushdb # 清除当前数据库的所有数据
OK
127.0.0.1:6379> keys * # 查询当前数据库的所有key
(empty list or set)
127.0.0.1:6379> set f f # 新增一个数据
OK
127.0.0.1:6379> keys *
1) "f"
127.0.0.1:6379> 
127.0.0.1:6379> dbsize # 查看当前数据库的大小
(integer) 1
127.0.0.1:6379> select 2 # 切换数据库，Redis默认有16个数据库
OK
127.0.0.1:6379[2]> set key value
OK
127.0.0.1:6379[2]> dbsize
(integer) 1
127.0.0.1:6379[2]> get key # 获取对应key的值
"value"
127.0.0.1:6379[2]> get f
(nil)
127.0.0.1:6379[2]> keys *
1) "key"
127.0.0.1:6379[2]> flushall # 清除所有数据库中的值
OK
127.0.0.1:6379[2]> keys *
(empty list or set)
127.0.0.1:6379[2]> select 1
OK
127.0.0.1:6379[1]> keys *
(empty list or set)
~~~



## 五大数据类型

**[字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets)**



### 1、 字符串

~~~bash
127.0.0.1:6379> keys * # 获取所有key
(empty list or set)
127.0.0.1:6379> set name feige # 设置一个键name值为feige
OK
127.0.0.1:6379> get name # 获取对应key的值
"feige"
127.0.0.1:6379> setnx name feige # 不存在则设置
(integer) 0
127.0.0.1:6379> setex fk 10 feige # 设置键fk值为feige并设置过期时间为10s
OK
127.0.0.1:6379> ttl fk # 查看key还有多久过期
(integer) 5
127.0.0.1:6379> ttl fk
(integer) -2
127.0.0.1:6379> expire name 30 # 给key设置过期时间，单位秒
(integer) 1
127.0.0.1:6379> ttl name
~~~

### 2、散列

~~~bash
xiaofei:0>hmset user username feige age 21 # 类似Java中的HashMap，以键值对的方式存储{"user":{"username":"feige","age":"21"}}
"OK"
xiaofei:0>hget user username # 通过键获取值
"feige"
xiaofei:0>hgetall user # 获取所有键值
 1)  "username"
 2)  "feige"
 3)  "age"
 4)  "21"
xiaofei:0>hset user nickname xiaofei
"1"
xiaofei:0>hgetall user
 1)  "username"
 2)  "feige"
 3)  "age"
 4)  "21"
 5)  "nickname"
 6)  "xiaofei"
xiaofei:0>hincrby user age 1 # 根据键对应的值执行加操作
"22"
xiaofei:0>hlen user #获取键对应字段的个数
"3"
xiaofei:0>hexists user username # 判断是否存在该字段
"1"
xiaofei:0>hexists user name
"0"
xiaofei:0>hkeys user # 只获取key
 1)  "username"
 2)  "age"
 3)  "nickname"
xiaofei:0>hvals user # 只获取value
 1)  "feige"
 2)  "22"
 3)  "xiaofei"
~~~



### 3、列表

~~~bash
xiaofei:0>rpush list A # RPUSH命令可向list的右边（尾部）添加一个新元素
"1"
xiaofei:0>rpush list B
"2"
xiaofei:0>lpush list C #  LPUSH 命令可向list的左边（头部）添加一个新元素
"3"
xiaofei:0>lrange list 0 -1 # LRANGE 命令可从list中取出一定范围的元素，0表示开始元素，-1表示最后一个元素，-2表示倒数第二个元素，其他以此类推
 1)  "C"
 2)  "A"
 3)  "B"
xiaofei:0>rpush list D E F # 向list尾部添加多个元素
"6"
xiaofei:0>lrange list 0 -1
 1)  "C"
 2)  "A"
 3)  "B"
 4)  "D"
 5)  "E"
 6)  "F"
xiaofei:0>rpop list # 删除list右边的元素并同时返回删除的值
"F"
xiaofei:0>lpop list # 删除list左边的元素并同时返回删除的值
"C"
xiaofei:0>lrange list 0 -1
 1)  "A"
 2)  "B"
 3)  "D"
 4)  "E"
xiaofei:0>ltrim list 0 2 # 把list从左边截取指定长度
"OK"
xiaofei:0>lrange list 0 -1
 1)  "A"
 2)  "B"
 3)  "D"
~~~

### 4、集合

~~~bash
xiaofei:0>sadd set 1 3 4 # 往集合里添加元素
"3"
xiaofei:0>smembers set # 获取集合元素
 1)  "1"
 2)  "3"
 3)  "4"
xiaofei:0>sismember set 3 # 查看集合中是否存在该元素
"1"
xiaofei:0>sismember set 30
"0"
xiaofei:0>sadd set1 3 4
"2"
xiaofei:0>smembers set1
 1)  "3"
 2)  "4"
xiaofei:0>sdiff set set1 # 求两个集合的差集
 1)  "1"
xiaofei:0>sunion set set1 # 求两个集合的并集
 1)  "1"
 2)  "3"
 3)  "4"
xiaofei:0>sinter set set1 # 求两个集合的交集
 1)  "3"
 2)  "4"
xiaofei:0>srandmember set # 随机获取一个元素
"3"
xiaofei:0>smembers set
 1)  "3"
 2)  "4"
xiaofei:0>smembers set1
 1)  "3"
 2)  "4"
xiaofei:0> scard set  # 获取set集合中的内容元素个数！
(integer) 2
xiaofei:0>spop set # 随机删除集合中的元素，并返回
"3"
xiaofei:0>spop set
"4"
xiaofei:0>spop set
null
~~~



### 5、有序集合

~~~bash
xiaofei:0>zadd name 15 jinli # 添加元素，第一个参数分数，通过第一个参数排序
"1"
xiaofei:0>zadd name 14 hufei
"1"
xiaofei:0>zadd name 16 liuyuanzhi
"1"
xiaofei:0>zrange name 0 -1 # 获取指定范围的元素
 1)  "hufei"
 2)  "jinli"
 3)  "liuyuanzhi"
 xiaofei:0>zrevrange name 0 -1 # 反转有序集合并返回
 1)  "liuyuanzhi"
 2)  "jinli"
 3)  "hufei"
 xiaofei:0>zrange name 0 -1 withscores # 返回分数
 1)  "hufei"
 2)  "14"
 3)  "jinli"
 4)  "15"
 5)  "liuyuanzhi"
 6)  "16"
~~~

## 事务

~~~bash
xiaofei:0>multi # 开启事务
"OK"
xiaofei:0>set k1 v1 #命令入队列
"QUEUED"
xiaofei:0>set k2 v2
"QUEUED"
xiaofei:0>get k1
"QUEUED"
xiaofei:0>exec # 执行事务
 1)  "OK"
 2)  "OK"
 3)  "v1"
 
 
xiaofei:0>multi
"OK"
xiaofei:0>set k3 v3
"QUEUED"
xiaofei:0>set k4 v4
"QUEUED"
xiaofei:0>discard # 放弃事务
"OK"
xiaofei:0>get k3 # 队列中的命令都不会执行
null
xiaofei:0>

# 编译错误，所有命令都不会执行
xiaofei:0>multi 
"OK"
xiaofei:0>set key1 val1
"QUEUED"
xiaofei:0>getset key1
"ERR wrong number of arguments for 'getset' command"
xiaofei:0>get key1
"QUEUED"
xiaofei:0>exec
"EXECABORT Transaction discarded because of previous errors."
xiaofei:0>get key1
null

# 运行时错误，命令会继续执行
xiaofei:0>multi
"OK"
xiaofei:0>set key1 va
"QUEUED"
xiaofei:0>incr key1
"QUEUED"
xiaofei:0>get key1
"QUEUED"
xiaofei:0>exec
 1)  "OK"
 2)  "ERR value is not an integer or out of range"
 3)  "va"
xiaofei:0>get key1
"va"
~~~



### watch

悲观锁：

- 很悲观，认为什么时候都会出问题，无论做什么都会加锁！

乐观锁：

- 很乐观，认为什么时候都不会出问题，所以不会上锁！ 更新数据的时候去判断一下，在此期间是否
	有人修改过这个数据，
- 获取version
- 更新的时候比较 version

~~~bash
xiaofei:0>set money 100
"OK"
xiaofei:0>set out 0
"OK"
xiaofei:0>watch money # 监视 money 对象
"OK"
xiaofei:0>multi
"OK"
xiaofei:0>decrby money 10
"QUEUED"
xiaofei:0>incrby out 10
"QUEUED"
xiaofei:0>exec # 事务正常结束，数据期间没有发生变动，这个时候就正常执行成功！
 1)  "90"
 2)  "10"
 
 # 执行之前，另外一个线程，修改了我们的值，这个时候，就会导致事务执行失
败！
xiaofei:0>get money
"70"
xiaofei:0>get out 
"20"
xiaofei:0>watch money
"OK"
xiaofei:0>multi
"OK"
xiaofei:0>decrby money 10
"QUEUED"
xiaofei:0>incrby out 10
"QUEUED"
xiaofei:0>exec # 另一个线程修改了我们的值，事务执行失败

xiaofei:0>get money
"60"
xiaofei:0>get out
"20"
# 事务执行失败时，先解除监视
xiaofei:0>unwatch
"OK"
xiaofei:0>watch money
"OK"
xiaofei:0>multi
"OK"
xiaofei:0>decrby money 10
"QUEUED"
xiaofei:0>incrby out 10
"QUEUED"
xiaofei:0>exec # 执行成功
~~~

# 使用Jedis操作Redis

创建一个maven项目

导包

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.feige</groupId>
    <artifactId>jedis-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.6.3</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.76</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>

</project>

~~~

创建一个测试类

~~~java
package com.feige.redis;

import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisShardInfo;

/**
 * @author feige<br />
 * @ClassName: RedisTest <br/>
 * @Description: <br/>
 * @date: 2021/8/16 15:36<br/>
 */
public class RedisTest {

    private Jedis jedis;

    @Before
    public void init(){
        String host = "xiaofei-cloud-classroom.redis.rds.aliyuncs.com";
        int port = 6379;
        JedisShardInfo jedisShardInfo = new JedisShardInfo(host,port);
        jedisShardInfo.setPassword("Hufei169357@");
        this.jedis = new Jedis(jedisShardInfo);
    }

    @Test
    public void test1(){
        System.out.println(jedis.ping());
        // 各数据结构的api命令和我们上面的差不多，这里就不做测试了
    }

}

~~~



## jedis操作事务

```java
package com.feige.redis;

import com.alibaba.fastjson.JSONObject;
import org.junit.Before;
import org.junit.Test;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisShardInfo;
import redis.clients.jedis.Transaction;

/**
 * @author feige<br />
 * @ClassName: RedisTest <br/>
 * @Description: <br/>
 * @date: 2021/8/16 15:36<br/>
 */
public class RedisTest {

    private Jedis jedis;

    @Before
    public void init(){
        // 我这里是阿里云的redis，大家根据自己的主机配置
        String host = "xiaofei-cloud-classroom.redis.rds.aliyuncs.com";
        int port = 6379;
        JedisShardInfo jedisShardInfo = new JedisShardInfo(host,port);
        jedisShardInfo.setPassword("xxxx");
        this.jedis = new Jedis(jedisShardInfo);
    }

    @Test
    public void test1(){
        JSONObject jsonObject = new JSONObject();
        jsonObject.put("key1","val1");
        jsonObject.put("key2","val2");
        Transaction multi = null;
        try {
            multi = jedis.multi();
            multi.set("user1",jsonObject.toJSONString());
            multi.set("user2",jsonObject.toJSONString());
            int i = 1/0;
            // 执行事务
            multi.exec();
        } catch (Exception e) {
            e.printStackTrace();
            if (multi != null) {
                // 放弃执行事务
                multi.discard();
            }
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            jedis.close();
        }

    }

}
```



# Spring-data-redis操作redis

创建一个springboot项目

导包

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.11.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.feige</groupId>
    <artifactId>spring-boot-redis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring-boot-redis</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

~~~

创建配置

~~~properties

spring.redis.host=xiaofei-cloud-classroom.redis.rds.aliyuncs.com
spring.redis.password=xxxx

~~~

```java
package com.feige.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @author feige<br />
 * @ClassName: RedisConfig <br/>
 * @Description: <br/>
 * @date: 2021/8/16 16:01<br/>
 */
@Configuration
public class RedisConfig {

    @Bean
    @ConditionalOnMissingBean(name = "redisTemplate")
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        // json序列化
        Jackson2JsonRedisSerializer<Object> jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer<>(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.activateDefaultTyping(objectMapper.getPolymorphicTypeValidator(),ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        // String的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        template.setKeySerializer(stringRedisSerializer);
        template.setHashKeySerializer(stringRedisSerializer);
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }

    /**
     * @description: String是最常用的提出来
     * @author: feige
     * @date: 2021/8/16 16:09
     * @param	redisConnectionFactory
     * @return: org.springframework.data.redis.core.StringRedisTemplate
     */
    @Bean
    @ConditionalOnMissingBean
    public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory){
        StringRedisTemplate stringRedisTemplate = new StringRedisTemplate();
        stringRedisTemplate.setConnectionFactory(redisConnectionFactory);
        return stringRedisTemplate;
    }
}

```

```java
package com.feige;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.core.RedisTemplate;

@SpringBootTest
class SpringBootRedisApplicationTests {

    @Autowired
    private RedisTemplate<Object, Object> redisTemplate;

    @Test
    void contextLoads() {
        redisTemplate.opsForValue().set("feige","xiaofei");
        System.out.println(redisTemplate.opsForValue().get("feige"));
    }

}
```

# redis.conf配置详解

~~~conf
bind 127.0.0.1 # 默认情况bind=127.0.0.1只能接受本机的访问请求，不写的情况下，无限制接受任何ip地址的访问

protected-mode yes # 设置本机保护模式 可选值no，开启的情况下，在没有设置bind ip并且没有密码的情况下，只允许本机连接

port 6379 # 配置redis启动端口，默认为6379

tcp-backlog 511 # 设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值（128），所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果

timeout 0 # 一个空闲的客户端维持多少秒会关闭，0表示关闭该功能。即永不关闭。

tcp-keepalive 300 # 对访问客户端的一种心跳检测，每个n秒检测一次。单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60

daemonize no # 是否为后台进程，设置为yes，守护进程，后台启动

pidfile /var/run/redis_6379.pid # 存放pid文件的位置，每个实例会产生一个不同的pid文件

loglevel notice # 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为notice

logfile "" # 日志文件名称

databases 16 # 设定库的数量 默认16，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id

requirepass foobared # 设置密码

maxclients 10000 # 设置redis同时可以与多少个客户端进行连接，如果达到了此限制，redis则会拒绝新的连接请求，并且向这些连接请求方发出“max number of clients reached”以作回应。

maxmemory <bytes> 
建议必须设置，否则，将内存占满，造成服务器宕机。
设置redis可以使用的内存量。一旦到达内存使用上限，redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定。
如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。但是对于无内存申请的指令，仍然会正常响应，比如GET等。
如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。

maxmemory-policy noeviction 
	volatile-lru：使用LRU算法移除key，只对设置了过期时间的键；（最近最少使用）
	allkeys-lru：在所有集合key中，使用LRU算法移除key
	volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键
	allkeys-random：在所有集合key中，移除随机的key
	volatile-ttl：移除那些TTL值最小的key，即那些最近要过期的key
	noeviction：不进行移除。针对写操作，只是返回错误信息
	
	
maxmemory-samples 5
	设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。
    一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。


~~~



# Redis持久化



## RDB

- 在指定的时间间隔内将内存中的数据集快照写入磁盘， 也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里

- Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。**RDB的缺点是最后一次持久化后的数据可能丢失**。

### Fork

- Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程

- 在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，Linux中引入了“**写时复制技术**”

- **一般情况父进程和子进程会共用同一段物理内存**，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

### RDB配置

~~~conf
dbfilename dump.rdb # 配置文件名称，默认为dump.rdb

dir ./ # rdb文件的保存路径，也可以修改。默认为Redis启动时命令行所在的目录下

save 900 1 
save 300 10
save 60 10000 # 默认是1分钟内改了1万次，或5分钟内改了10次，或15分钟内改了1次。save ""表示禁用
 
save ：save时只管保存，其它不管，全部阻塞。手动保存。不建议。
bgsave：Redis会在后台异步进行快照操作， 快照同时还可以响应客户端请求。
可以通过lastsave 命令获取最后一次成功执行快照的时间

stop-writes-on-bgsave-error yes # 当Redis无法写入磁盘的话，直接关掉Redis的写操作

rdbcompression yes # 对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能

rdbchecksum yes # 在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能


~~~



### rdb的备份

先通过`config get dir` 查询rdb文件的目录 

将*.rdb的文件拷贝到别的地方

### rdb的恢复

- 关闭Redis

- 先把备份的文件拷贝到工作目录下 cp dump2.rdb dump.rdb

- 启动Redis, 备份数据会直接加载



### 优势

- 适合大规模的数据恢复

- 对数据完整性和一致性要求不高更适合使用

- 节省磁盘空间

- 恢复速度快

### 劣势

- Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑

- 虽然Redis在fork时使用了**写时拷贝技术**,但是如果数据庞大时还是比较消耗性能。

- 在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改。



## AOF

以**日志**的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(**读操作不记录**)， **只许追加文件但不可以改写文件**，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

### AOF持久化流程

1. 客户端的请求写命令会被append追加到AOF缓冲区内；

2. AOF缓冲区根据AOF持久化策略[always,everysec,no]将操作sync同步到磁盘的AOF文件中；

3. AOF文件大小超过重写策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量；

4. Redis服务重启时，会重新load加载AOF文件中的写操作达到数据恢复的目的；

### AOF配置

~~~conf
appendonly no # 是否开启AOF

appendfilename "appendonly.aof" # AOF的文件名，AOF文件的保存路径，同RDB的路径一致

# appendfsync always
appendfsync everysec # AOF同步频率
# appendfsync no


~~~



### AOF的备份

AOF的备份机制和性能虽然和RDB不同, 但是备份和恢复的操作同RDB一样，都是拷贝备份文件，需要恢复时再拷贝到Redis工作目录下，启动系统即加载。

### AOF的恢复

**AOF和RDB同时开启，系统默认取AOF的数据（数据不会存在丢失）**

- 正常恢复
	- 修改默认的appendonly no，改为yes
	- 将有数据的aof文件复制一份保存到对应目录(查看目录：config get dir)
	- 恢复：重启redis然后重新加载

- 异常恢复
	- 修改默认的appendonly no，改为yes
	- 如遇到**AOF****文件损坏**，通过/usr/local/bin/redis-check-aof--fix appendonly.aof**进行恢复
	- 备份被写坏的AOF文件
	- 恢复：重启redis，然后重新加载



### Rewrite压缩

AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof

AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，redis4.0版本后的重写，是指上就是把rdb 的快照，以二进制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。

- no-appendfsync-on-rewrite：

	- 如果 no-appendfsync-on-rewrite=yes ,不写入aof文件只写入缓存，用户请求不会阻塞，但是在这段时间如果宕机会丢失这段时间的缓存数据。（降低数据安全性，提高性能）

	- 如果 no-appendfsync-on-rewrite=no, 还是会把数据往磁盘里刷，但是遇到重写操作，可能会发生阻塞。（数据安全，但是性能降低）

**触发机制，何时重写**

Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发

重写虽然可以节约大量磁盘空间，减少恢复时间。但是每次重写还是有一定的负担的，因此设定Redis要满足一定条件才会进行重写。 

auto-aof-rewrite-percentage：设置重写的基准值，文件达到100%时开始重写（文件是原来重写后文件的2倍时触发）

auto-aof-rewrite-min-size：设置重写的基准值，最小文件64MB。达到这个值开始重写。

例如：文件达到70MB开始重写，降到50MB，下次什么时候开始重写？100MB

系统载入时或者上次重写完毕时，Redis会记录此时AOF大小，设为base_size,

如果Redis的AOF当前大小>= base_size +base_size*100% (默认)且当前大小>=64mb(默认)的情况下，Redis会对AOF进行重写。 

（1）bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。

（2）主进程fork出子进程执行重写操作，保证主进程不会阻塞。

（3）子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。

（4）子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息。主进程把aof_rewrite_buf中的数据写入到新的AOF文件。

（5）使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。

### 优势

- 备份机制更稳健，丢失数据概率更低。

- 可读的日志文本，通过操作AOF稳健，可以处理误操作。

### 劣势

- 比起RDB占用更多的磁盘空间。

- 恢复备份速度要慢。

- 每次读写都同步的话，有一定的性能压力。

- 存在个别Bug，造成恢复不能。



# Redis集群

## 主从复制

新建三个配置文件redis6379.conf、redis6380.conf、redis6381.conf，写入下面信息，**注意修改端口**

~~~conf
include /usr/local/bin/conf/redis.conf
pidfile /var/run/redis_6379.pid
port 6379
dbfilename dump6379.rdb
~~~

根据配置文件启动三台redis服务，用redis-cli连接

配置集群命令，只需配置从机

`slaveof ip port`在对应的从机上执行ip和port为主机的ip和端口

~~~bash
[root@feige ~]# redis-cli -p 6380
127.0.0.1:6380> info replication
# Replication
role:master
connected_slaves:0
master_replid:047190cb2a2de8a2f6419221bda1a76dff4b76fc
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6380> info replication
# Replication
role:master
connected_slaves:2
slave0:ip=127.0.0.1,port=6379,state=online,offset=154,lag=0
slave1:ip=127.0.0.1,port=6381,state=online,offset=154,lag=0
master_replid:cc79793288b2f8ba12c3b7c0f04e837aa1c68e06
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:154
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:154
127.0.0.1:6380> set key value
OK
127.0.0.1:6380> get key
"value"

~~~



~~~bash
[root@feige bin]# redis-cli -p 6379
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:0
master_replid:71dbcaba4161c1123e336c6c77ff2b160e8347e6
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> slaveof 127.0.0.1 6380
OK
127.0.0.1:6379> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
master_link_status:up
master_last_io_seconds_ago:4
master_sync_in_progress:0
slave_repl_offset:0
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:cc79793288b2f8ba12c3b7c0f04e837aa1c68e06
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:0
127.0.0.1:6379> get key
"value"
~~~



~~~bash
[root@feige ~]# redis-cli -p 6381
127.0.0.1:6381> keys *
(empty array)
127.0.0.1:6381> info replication
# Replication
role:master
connected_slaves:0
master_replid:4fadfa46cf1c36e510394a50e1154a0eab125ea1
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6381> slaveof 127.0.0.1 6380
OK
127.0.0.1:6381> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
master_link_status:up
master_last_io_seconds_ago:1
master_sync_in_progress:0
slave_repl_offset:112
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:cc79793288b2f8ba12c3b7c0f04e837aa1c68e06
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:112
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:99
repl_backlog_histlen:14
127.0.0.1:6381> get key
"value"

~~~



- 只能在主机写数据，从机写数据会报错

- 主机挂掉，重启就行

- 从机重启需重设：slaveof 127.0.0.1 6380
- 中途变更转向:会清除之前的数据，重新建立拷贝最新的
- 用 slaveof no one  将从机变为主机，当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

### 复制原理

- Slave启动成功连接到master后会发送一个sync命令
- Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
- 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
- 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
- 但是只要是重新连接master,一次完全同步（全量复制将被自动执行）

## 哨兵模式

新建sentinel.conf配置文件

~~~conf
sentinel monitor mymaster 127.0.0.1 6380 1 # 其中mymaster为监控对象起的服务器名称， 1 为至少有多少个哨兵同意迁移的数量
~~~

启动哨兵

`redis-sentinel ./conf/sentinel.conf`

当主节点挂了之后，哨兵会根据slave-priority选择一个slave做master，原主机重启后会变为从机

~~~bash
[root@feige bin]# redis-sentinel ./conf/sentinel.conf 
6320:X 18 Aug 2021 16:25:46.892 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
6320:X 18 Aug 2021 16:25:46.892 # Redis version=6.0.6, bits=64, commit=00000000, modified=0, pid=6320, just started
6320:X 18 Aug 2021 16:25:46.892 # Configuration loaded
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 6.0.6 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 6320
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

6320:X 18 Aug 2021 16:25:46.894 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
6320:X 18 Aug 2021 16:25:46.894 # Sentinel ID is 9a2c1e9bc1d01898032d0db8f4a175799f18382a
6320:X 18 Aug 2021 16:25:46.894 # +monitor master mymaster 127.0.0.1 6380 quorum 1
6320:X 18 Aug 2021 16:25:46.895 * +slave slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:25:46.896 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.667 # +sdown master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.667 # +odown master mymaster 127.0.0.1 6380 #quorum 1/1
6320:X 18 Aug 2021 16:26:44.667 # +new-epoch 2
6320:X 18 Aug 2021 16:26:44.667 # +try-failover master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.669 # +vote-for-leader 9a2c1e9bc1d01898032d0db8f4a175799f18382a 2
6320:X 18 Aug 2021 16:26:44.669 # +elected-leader master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.669 # +failover-state-select-slave master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.759 # +selected-slave slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.759 * +failover-state-send-slaveof-noone slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:44.860 * +failover-state-wait-promotion slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:45.258 # +promoted-slave slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:45.258 # +failover-state-reconf-slaves master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:45.322 * +slave-reconf-sent slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:46.274 * +slave-reconf-inprog slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:46.274 * +slave-reconf-done slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:46.329 # +failover-end master mymaster 127.0.0.1 6380
6320:X 18 Aug 2021 16:26:46.329 # +switch-master mymaster 127.0.0.1 6380 127.0.0.1 6381 # 选举
6320:X 18 Aug 2021 16:26:46.330 * +slave slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6381
6320:X 18 Aug 2021 16:26:46.330 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6381
6320:X 18 Aug 2021 16:27:16.349 # +sdown slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6381
6320:X 18 Aug 2021 16:29:53.941 # -sdown slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6381
6320:X 18 Aug 2021 16:30:03.937 * +convert-to-slave slave 127.0.0.1:6380 127.0.0.1 6380 @ mymaster 127.0.0.1 6381
~~~

### 哨兵的一些配置

~~~conf
# Example sentinel.conf
# 哨兵sentinel实例运行的端口 默认26379
port 26379

# 哨兵sentinel的工作目录
dir /tmp

# 哨兵sentinel监控的redis主节点的 ip port
# master-name 可以自己命名的主节点名字 只能由字母A-z、数字0-9 、这三个字符".-_"组成。
# quorum 配置多少个sentinel哨兵统一认为master主节点失联 那么这时客观上认为主节点失联了
# sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 2


# 当在Redis实例中开启了requirepass foobared 授权密码 这样所有连接Redis实例的客户端都要提供
密码
# 设置哨兵sentinel 连接主从的密码 注意必须为主从设置一样的验证密码
# sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123passw0rd


# 指定多少毫秒之后 主节点没有应答哨兵sentinel 此时 哨兵主观上认为主节点下线 默认30秒
# sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000


# 这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，
这个数字越小，完成failover所需的时间就越长，
但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。
可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。
# sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1


# 故障转移的超时时间 failover-timeout 可以用在以下这些方面：
#1. 同一个sentinel对同一个master两次failover之间的间隔时间。
#2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那
里同步数据时。
#3.当想要取消一个正在进行的failover所需要的时间。 
#4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，
slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了
# 默认三分钟
# sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000


# SCRIPTS EXECUTION
#配置当某一事件发生时所需要执行的脚本，可以通过脚本来通知管理员，例如当系统运行不正常时发邮件通知
相关人员。
#对于脚本的运行结果有以下规则：
#若脚本执行后返回1，那么该脚本稍后将会被再次执行，重复次数目前默认为10
#若脚本执行后返回2，或者比2更高的一个返回值，脚本将不会重复执行。
#如果脚本在执行过程中由于收到系统中断信号被终止了，则同返回值为1时的行为相同。
#一个脚本的最大执行时间为60s，如果超过这个时间，脚本将会被一个SIGKILL信号终止，之后重新执行。
#通知型脚本:当sentinel有任何警告级别的事件发生时（比如说redis实例的主观失效和客观失效等等），
将会去调用这个脚本，这时这个脚本应该通过邮件，SMS等方式去通知系统管理员关于系统不正常运行的信
息。调用该脚本时，将传给脚本两个参数，一个是事件的类型，一个是事件的描述。如果sentinel.conf配
置文件中配置了这个脚本路径，那么必须保证这个脚本存在于这个路径，并且是可执行的，否则sentinel无
法正常启动成功。
#通知脚本
# shell编程
# sentinel notification-script <master-name> <script-path>
sentinel notification-script mymaster /var/redis/notify.sh


# 客户端重新配置主节点参数脚本
# 当一个master由于failover而发生改变时，这个脚本将会被调用，通知相关的客户端关于master地址已
经发生改变的信息。
# 以下参数将会在调用脚本时传给脚本:
# <master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
# 目前<state>总是“failover”,
# <role>是“leader”或者“observer”中的一个。
# 参数 from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通
信的
# 这个脚本应该是通用的，能被多次调用，不是针对性的。
# sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh # 一般都是由运维来配
置！
~~~



## cluster模式

通过shell脚本新建配置文件

~~~shell
for port in $(seq 1 6);
do
 mkdir -p /usr/local/bin/conf/cluster/node-${port}/conf;
 touch /usr/local/bin/conf/cluster/node-${port}/conf/redis.conf;
 cat << EOF >/usr/local/bin/conf/cluster/node-${port}/conf/redis.conf
include /usr/local/bin/conf/redis.conf
pidfile "/var/run/redis_${port}.pid"
dbfilename "dump${port}.rdb"
dir "/home/bigdata/redis_cluster"
logfile "/home/bigdata/redis_cluster/redis_err_${port}.log"
port 637${port}
cluster-enabled yes
cluster-config-file node-${port}.conf
cluster-node-timeout 5000
EOF
done
~~~

~~~shell
# 启动脚本
for port in $(seq 1 6);
do
redis-server /usr/local/bin/conf/cluster/node-${port}/conf/redis.conf;

done
~~~

创建集群--replicas 1 采用最简单的方式配置集群，一台主机，一台从机，正好三组。

~~~bash
redis-cli --cluster create --cluster-replicas 1 127.0.0.1:6371 127.0.0.1:6372 127.0.0.1:6373 127.0.0.1:6374 127.0.0.1:6375 127.0.0.1:6376
~~~

-c 采用集群策略连接，设置数据会自动切换到相应的写主机

~~~bash
redis-cli -c -p 6371
~~~

~~~bash
127.0.0.1:6371> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:32
cluster_stats_messages_pong_sent:36
cluster_stats_messages_sent:68
cluster_stats_messages_ping_received:31
cluster_stats_messages_pong_received:32
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:68
127.0.0.1:6371> cluster nodes
236f1b8057d56bfa647e4fafcf278334768e6efb 127.0.0.1:6376@16376 slave 4dc4a15448d4eacddb28c9ff196f833a3452139c 0 1629364037000 3 connected
58d06f327b53276fd428f5788a8138282d82ab3f 127.0.0.1:6372@16372 master - 0 1629364036000 2 connected 5461-10922
b60658f3952d0705d0b6818f2f422c1618c691f4 127.0.0.1:6371@16371 myself,master - 0 1629364036000 1 connected 0-5460
4ee6386916139e6b2bed12170aa0e91505df7be8 127.0.0.1:6375@16375 slave 58d06f327b53276fd428f5788a8138282d82ab3f 0 1629364037675 2 connected
4dc4a15448d4eacddb28c9ff196f833a3452139c 127.0.0.1:6373@16373 master - 0 1629364036000 3 connected 10923-16383
b1e8500f8086e1e624665800bff699f3b9840429 127.0.0.1:6374@16374 slave b60658f3952d0705d0b6818f2f422c1618c691f4 0 1629364037000 1 connected
127.0.0.1:6371> set k v
-> Redirected to slot [7629] located at 127.0.0.1:6372
OK
127.0.0.1:6372> set k1 v1
-> Redirected to slot [12706] located at 127.0.0.1:6373
OK
127.0.0.1:6373> set k2 v3
-> Redirected to slot [449] located at 127.0.0.1:6371
OK
127.0.0.1:6371> set k4 v4
-> Redirected to slot [8455] located at 127.0.0.1:6372
OK
127.0.0.1:6372> get k1
-> Redirected to slot [12706] located at 127.0.0.1:6373
"v1"
127.0.0.1:6373> get k2
-> Redirected to slot [449] located at 127.0.0.1:6371
"v3"
~~~

把6371停了之后，故障转移

~~~bash
127.0.0.1:6372> cluster nodes
4dc4a15448d4eacddb28c9ff196f833a3452139c 127.0.0.1:6373@16373 master - 0 1629364999588 3 connected 10923-16383
b1e8500f8086e1e624665800bff699f3b9840429 127.0.0.1:6374@16374 master - 0 1629364999000 7 connected 0-5460
58d06f327b53276fd428f5788a8138282d82ab3f 127.0.0.1:6372@16372 myself,master - 0 1629364997000 2 connected 5461-10922
b60658f3952d0705d0b6818f2f422c1618c691f4 127.0.0.1:6371@16371 master,fail - 1629364959891 1629364957386 1 connected
4ee6386916139e6b2bed12170aa0e91505df7be8 127.0.0.1:6375@16375 slave 58d06f327b53276fd428f5788a8138282d82ab3f 0 1629364998000 2 connected
236f1b8057d56bfa647e4fafcf278334768e6efb 127.0.0.1:6376@16376 slave 4dc4a15448d4eacddb28c9ff196f833a3452139c 0 1629364999488 3 connected
127.0.0.1:6372> flushdb
OK
127.0.0.1:6372> keys *
(empty array)
127.0.0.1:6372> set k1 k2
-> Redirected to slot [12706] located at 127.0.0.1:6373
OK
127.0.0.1:6373> set k2 v2
-> Redirected to slot [449] located at 127.0.0.1:6374
OK
127.0.0.1:6374> set k3 v3
OK
127.0.0.1:6374> set k4 v4
-> Redirected to slot [8455] located at 127.0.0.1:6372
OK
127.0.0.1:6372> 

~~~

### 多值操作（mset）

~~~bash
127.0.0.1:6372> mset k6{f} v6 k8{f} v8
-> Redirected to slot [3168] located at 127.0.0.1:6374
OK
127.0.0.1:6374> cluster keyslot f
(integer) 3168
127.0.0.1:6374> cluster getkeysinslot 3168 10
1) "k6{f}"
2) "k8{f}"

~~~



### slots（插槽）

一个 Redis 集群包含 16384 个插槽（hash slot）， 数据库中的每个键都属于这 16384 个插槽的其中一个， 

集群使用公式 **CRC16(key) %** 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。

集群中的每个节点负责处理一部分插槽。 举个例子， 如果一个集群可以有主节点， 其中：

节点 A 负责处理 0 号至 5460 号插槽。

节点 B 负责处理 5461 号至 10922 号插槽。

节点 C 负责处理 10923 号至 16383 号插槽。

### 优势

实现扩容

分摊压力

无中心配置相对简单

### 劣势

多键操作是不被支持的 

多键的Redis事务是不被支持的。lua脚本不被支持

由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。



# Redis应用解决问题

## 缓存穿透

key对应的数据在数据源并不存在，每次针对此key的请求从缓存获取不到，请求都会压到数据源，从而可能压垮数据源。比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击可能压垮数据库。

### 解决

一个一定不存在缓存及查询不到的数据，由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。

解决方案：

（1）  **对空值缓存：**如果一个查询返回的数据为空（不管是数据是否不存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超过五分钟

（2）  **设置可访问的名单（白名单）：**

使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmap里面的id进行比较，如果访问id不在bitmaps里面，进行拦截，不允许访问。

（3）  **采用布隆过滤器**：(布隆过滤器（Bloom Filter）是1970年由布隆提出的。它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。

布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。)

将所有可能存在的数据哈希到一个足够大的bitmaps中，一个一定不存在的数据会被 这个bitmaps拦截掉，从而避免了对底层存储系统的查询压力。

**（4）**  **进行实时监控：**当发现Redis的命中率开始急速降低，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务

## 缓存击穿

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

### 解决

key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题。

解决问题：

**（1）预先设置热门数据：**在redis高峰访问之前，把一些热门数据提前存入到redis里面，加大这些热门数据key的时长

**（2）实时调整：**现场监控哪些数据热门，实时调整key的过期时长

**（3）使用锁：**

（1）  就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db。

（2）  先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX）去set一个mutex key

（3）  当操作返回成功时，再进行load db的操作，并回设缓存,最后删除mutex key；

（4）  当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法。

## 缓存雪崩

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。

缓存雪崩与缓存击穿的区别在于这里针对很多key缓存，前者则是某一个key



### 解决

缓存失效时的雪崩效应对底层系统的冲击非常可怕！

解决方案：

**（1）**  **构建多级缓存架构：**nginx缓存 + redis缓存 +其他缓存（ehcache等）

**（2）**  **使用锁或队列：**

用加锁或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。不适用高并发情况

**（3）**  **设置过期标志更新缓存：**

记录缓存数据是否过期（设置提前量），如果过期会触发通知另外的线程在后台去更新实际key的缓存。

**（4）**  **将缓存失效时间分散开：**

比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。