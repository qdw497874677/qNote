[**首页**](https://github.com/qdw497874677/myNotes/blob/master/首页检索.md)

# NoSQL

> 90年代，网站的访问量不会大大，单个数据库完全足够用，那个时候更多的去使用静态网页。

> 缓存 + MySQL + 垂直拆分（读写分离）

> 发展过程：优化数据结构和索引——文件缓存（IO）——Memcached

> 分库分表 + 水平拆分 + MySQL集群

> 如今的年代

数据量很多，变化很快，

MySQL等关系型数据库不够用了

如果有一些专门的数据库可以单独存储博客、图片等特殊的数据，那MySQl的压力就变小了

> 目前一个基础的互联网项目

用户先访问企业的防火墙，进行负载均衡，到不同的服务器。。。

## 为什么要用NoSQL

> NoSQL：不仅仅是SQL（非关系型数据库）

传统的关系型数据库很难应付web2.0时代

Redis是NoSQL中发展最快的

很多数据类型，比如用户的个人信息，社交网络，地理位置，他们的存储不需要一个固定的模式。

## NoSQL特点

1. 方便拓展（数据关系没有关系）
2. 大数据量高性能（Redis的缓存是细粒度的，性能很高）
3. 数据类型是多样型的（不需要事先设计数据库）

## 传统的RDBMS和NoSQL的区别

传统的RDBMS
- 结构化组织
- SQL
- 数据和关系都存在单独的表中
- 严格的一致性
- 基础的事务
- 。。。

NoSQL
- 不仅仅是数据
- 没有固定的查询语言
- 键值对存储、列存储、文档存储、图形数据库
- 最终一致性
- CAP定理和BASE （异地多活）
- 高性能，高可用，高可扩展性

公司中的实践 NoSQL+RDBMS

### 



## CAP理论



# NoSQL四大分类

## KV键值对
- 典型应用：内容缓存，主要用于处理大量数据的高访问负载
- 数据模型：一系列键值对
- 优点：快速查询
- 缺点：存储的数据缺少结构化

> 新浪：Redis  
> 美团：Redis+Tair  
>  阿里、百度：Redis+memecache


## 文档型数据库
- 相关产品：MongoDB
    - 基于分布式文件存储的数据库，C++编写，主要用来处理大量文档
    - 是一个介于关系型数据库和非关系型数据库中间的产品
- 典型应用：web应用（与键值对类似，值是结构化的）
- 数据模型：一系列键值对
- 优势：数据结构要求不严格
- 劣势：性能不高，缺少统一的查询语法


## 列存储数据库
- 典型应用：HBase、分布式的文件系统
- 数据模型：以列簇式存储，将同一列数据存在一起
- 优势：查找速度快，可扩展性强，更容易进行分布式扩展
- 劣势：功能相对局限  

## 图关系数据库
- 不是存图形，存的是图关系

# Redis概述

> Redis(Remote Dictionary Server),远程字典服务

> 是一个开源的使用C语言编写，支持网络、可基于内存也可持久化的日志型、Key-Value数据库，并提供多种语言的API。  
> 与memcached一样，为了保证效率，数据都是缓存在内存中。区别的是redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave(主从)同步。

## 能干嘛：  

- 内存存储，持久化
- 效率高，可以用来高速缓存
- 发布订阅系统
- 地图信息分析
- 计数器、计时器
- 。。。

## 特性

- 多样的数据类型
- 持久化
- 集群
- 事务



# Redis安装

## Windows

### 下载安装Redis

### 打开目录

![img](Redis.assets/untitle.png)



### 在cmd中输入

~~~
redis-server redis.windows.conf
~~~

![img](Redis.assets/untitle-1589880130459.png)





### 或者写一个cmd脚本

`````````
redis-server.exe redis.windows.conf
`````````



![img](Redis.assets/untitle-1589880136586.png)



> 如果立即退出，可能是端口被占用，可以查找6379的端口，杀掉占用端口的进程

~~~
netstat -ano |findstr "6379"
~~~





## Linux

### 下载Redis

~~~
wget http://download.redis.io/releases/redis-5.0.8.tar.gz
~~~

移动到/opt

~~~
mv redis-5.0.8.tar.gz /opt
~~~

### 解压

```
tar -zxvf redis-5.0.8.tar.gz
```

![image-20200404144955478](Redis.assets/image-20200404144955478.png)

### 进入redis目录，开始基本安装

安装C++环境

~~~
sudo apt-get install gcc
sudo apt-get install g++
~~~

### 执行make

~~~
make
make install
~~~

完成

![image-20200404150651900](Redis.assets/image-20200404150651900.png)



### 默认安装路径为

~~~
/usr/local/bin
~~~

![image-20200404151109456](Redis.assets/image-20200404151109456.png)



### 将配置文件复制到当前目录下

安装完默认没有配置文件

创建一个文件夹

~~~
mkdir qdwconfig
~~~

把源码目录中的配置文件**复制过来**

~~~
cp /opt/redis-5.0.8/redis.conf ./
~~~

![image-20200404152038272](Redis.assets/image-20200404152038272.png)

以后就用这个配置文件进行启动

### 启动

默认不是后台启动，需要修改配置文件

![image-20200404152230224](Redis.assets/image-20200404152230224.png)

改成yes

启动Redis服务

在bin目录下使用自己的配置文件启动

~~~
root@VM-0-3-ubuntu:/usr/local/bin# redis-server qdwconfig/redis.conf 
~~~

![image-20200404152416985](Redis.assets/image-20200404152416985.png)

使用redis-cli连接测试

~~~
root@VM-0-3-ubuntu:/usr/local/bin# redis-cli -p 6379
# 让终端展示中文
redis-cli -p 6379 --raw
~~~

![image-20200404152743871](Redis.assets/image-20200404152743871.png)



### 查看redis的进程是否开启

~~~
ps -ef|grep redis
~~~



![image-20200404153021490](Redis.assets/image-20200404153021490.png)



### 关闭Redis服务

通过在redis-cli中执行

~~~
shutdown
exit
~~~

![image-20200404153214037](Redis.assets/image-20200404153214037.png)

进程结束了

![image-20200404153337027](Redis.assets/image-20200404153337027.png)



# 性能测试工具

## redis-benchmark 

是官方自带的性能测试工具

### 测试命令

在redis目录下执行

```
redis-benchmark [option] [option value]
```

### 实例

测试：100个并发连接 100000个请求

```
redis-benchmark -h localhost -p 6379 -c 100 -n 10000  -q
```



| 序号 | 选项      | 描述                                       | 默认值    |
| :--- | :-------- | :----------------------------------------- | :-------- |
| 1    | **-h**    | 指定服务器主机名                           | 127.0.0.1 |
| 2    | **-p**    | 指定服务器端口                             | 6379      |
| 3    | **-s**    | 指定服务器 socket                          |           |
| 4    | **-c**    | 指定并发连接数                             | 50        |
| 5    | **-n**    | 指定请求数                                 | 10000     |
| 6    | **-d**    | 以字节的形式指定 SET/GET 值的数据大小      | 3         |
| 7    | **-k**    | 1=keep alive 0=reconnect                   | 1         |
| 8    | **-r**    | SET/GET/INCR 使用随机 key, SADD 使用随机值 |           |
| 9    | **-P**    | 通过管道传输 <numreq> 请求                 | 1         |
| 10   | **-q**    | 强制退出 redis。仅显示 query/sec 值        |           |
| 11   | **--csv** | 以 CSV 格式输出                            |           |
| 12   | **-l**    | 生成循环，永久执行测试                     |           |
| 13   | **-t**    | 仅运行以逗号分隔的测试命令列表。           |           |
| 14   | **-I**    | Idle 模式。仅打开 N 个 idle 连接并等待。   |           |



# Redis基础知识



## 基本的操作



**Redis里的数据库是用来存放数据的一个基本单元，一个库可以存放key-value键值对。**每一个库都有一个唯一的编号，是从0开始。Redis默认有16个数据库（0-15）。默认使用0号库。

可以使用select进行切换

![image-20200404162254210](Redis.assets/image-20200404162254210.png)

~~~bash
#切换到1号数据库
select 1
#查看当前数据库数据库大小
dbsize
#查看当前数据库所有的key
key *
#获取key:name的值
get name
#清空当前数据库
flushdb
#清空全部数据库
flushall
~~~

![image-20200404162815186](Redis.assets/image-20200404162815186.png)

## Redis是单线程的

CPU不是Redis的性能瓶颈，内存和网络带宽是瓶颈

> 为什么单线程还这么快

Redis是C语言写的

CPU的上下文切换有消耗，对于内存系统来说，没有上下文切换效率就会高





# 五大数据类型

> Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 字符串（strings）， 散列（hashes）， 列表（lists）， 集合（sets）， 有序集合（sorted sets） 与范围查询， bitmaps， hyperloglogs 和 地理空间（geospatial） 索引半径查询。 Redis 内置了 复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions） 和不同级别的 磁盘持久化（persistence）， 并通过 Redis哨兵（Sentinel）和自动 分区（Cluster）提供高可用性（high availability）。

Redis中以key-value的形式存储数据。value有很多种类型（string，list，set，zset，hash）

## Key相关指令

~~~bash
#设置key-value
127.0.0.1:6379> set name qdw
OK
127.0.0.1:6379> set age 10
OK
#删除key
del name
#判断key是否存在，存在返回1，
127.0.0.1:6379> exists name
(integer) 1
#把key移动到另一个数据库
127.0.0.1:6379> move name 1 
(integer) 1
#查看所有的key
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> set name qdw
OK
127.0.0.1:6379> keys *
1) "age"
2) "name"
# *表示任意数量任意字符 
# ?表示一个任意字符 
# []匹配括号中的一个字符，比如[ab]匹配一个a或者一个b 


#设置key的过期时间   pexpire用毫秒
127.0.0.1:6379> expire name 10
(integer) 1
#查看key的当前剩余时间  永久存储的返回-1  如果key不存在返回-2  其他的都显示剩余时间  pttl返回剩余毫秒
127.0.0.1:6379> ttl name
#查看key所对应value的数据类型
127.0.0.1:6379> type name
string
# 随机返回一个key
randomkey
# 更改key的名字 把key改成key2
rename key key2
#切换数据库
127.0.0.1:6379> select 1
# 移动key到数据库，比兔移动key到1号数据库
move key 1

~~~



## String（字符串）

### 设置，追加，判断是否存在，获取值，获取长度

~~~bash
#设置key-value
127.0.0.1:6379> set name qdw
OK
127.0.0.1:6379> set age 10
OK
#判断key是否存在
127.0.0.1:6379> exists name
(integer) 1

#追加值的内容，如果key不存在，就相当于set
127.0.0.1:6379> append key1 name
(integer) 6
127.0.0.1:6379> get key1
"v1name"
#获取字符串长度
127.0.0.1:6379> strlen key1
(integer) 6
~~~

### 自增自建

~~~bash
127.0.0.1:6379> set views 0
OK
127.0.0.1:6379> get views
"0"
#+1操作
127.0.0.1:6379> incr views
(integer) 1
127.0.0.1:6379> incr views
(integer) 2
127.0.0.1:6379> get views
"2"
#-1操作
127.0.0.1:6379> decr views
(integer) 1
127.0.0.1:6379> get views
"1"
#加指定的值
127.0.0.1:6379> incrby views 10
(integer) 11
127.0.0.1:6379> decrby views 10
(integer) 1
~~~

### 获取字符串里的一个范围的子串

~~~bash
#取字符串的范围字符
127.0.0.1:6379> getrange key1 0 3
"v1na"
#取字符串的全部字符
127.0.0.1:6379> getrange key1 0 -1
"v1name"

~~~

### 从一个位置替换字符串

~~~bash
#从指定的位置替换
127.0.0.1:6379> setrange key1 2 qdw
(integer) 6
127.0.0.1:6379> get key1
"v1qdwe"

~~~

### 设置定是过期的值

~~~bash
#设置键值对，并设置过期时间
127.0.0.1:6379> setex key2 10 hello
OK
127.0.0.1:6379> ttl key2
(integer) 7

~~~

### 如果不存在，才创建值成功

~~~bash
#如果存在，就不会创建成功（set存在的会覆盖）
127.0.0.1:6379> setnx key2 qqq
(integer) 1
127.0.0.1:6379> setnx key2 www
#返回0表示没有创建成功
(integer) 0
~~~



### 批量设置

~~~bash
#批量设置
127.0.0.1:6379> mset k1 v1 k2 v2
OK
127.0.0.1:6379> keys *
1) "key2"
2) "k1"
3) "k2"
4) "key1"
5) "views"
#批量获取
127.0.0.1:6379> mget k1 k2 
1) "v1"
2) "v2"
#如果存在，就不会创建成功（set存在的会覆盖）
127.0.0.1:6379> msetnx k2 v22 k3 v33
#失败，k2和k3都没设置成功
(integer) 0
~~~

msetnx是一个原子性操作，同时成功或者同时失败



### 设置对象

~~~bash
#设置一个user:1对象，用json来存储这个对象
127.0.0.1:6379> set user:1 {name:xiaoming,age:3}
OK
127.0.0.1:6379> get user:1
"{name:xiaoming,age:3}"
#批量设置和获取对象的属性值
127.0.0.1:6379> mset user:1:name xiaoqiang user:1:age 10
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "xiaoqiang"
2) "10"

~~~

### getset操作，先获取再设置（获取后更新）

~~~bash
#先获取 再设置
127.0.0.1:6379> getset db redis
(nil)
127.0.0.1:6379> get db
"redis"
127.0.0.1:6379> getset db mongodb
"redis"
127.0.0.1:6379> get db
"mongodb"

~~~



String类型最大512MB。数值计算最大范围为java的long的最大值。

String类型的应用场景：主页高频访问信息显示控制。例如微博主页显示的粉丝数与微博数。

解决方案：

- 以用户主键和属性值作为key，对应的数据作为value，后台设置定时刷新策略即可。这样可以直接通过incr操作自加。

> user\.id:10101010:fans	1000000
> user\.id:10101010:blogs	1000
> user\.id:10101010:focuss	100

- 以用户主键作为key，用json格式存储用户信息。这种要先取出来再更新。

> user\.id:10101010	{id:10101010,fans:1000000,blogs:1000,focuss:100}



key的设置约定：

表名:主键名:主键值:字段

![image-20200427231613692](Redis.assets/image-20200427231613692.png)



## Hash（哈希）

相当于map集合

用String存数据，基本上就是两种方式，一个是key+json；另一个是把属性加到key中，值存属性对应的值，相当于把json拆开。

Hash类型，把key和value中间加了个field字段，一个key可以有很多个field+value。

也相当于一个Hash存储很多个String类型，一个Hash就是一个Redis。

### 添加/获取

~~~bash
#在hash中设置一个键值对
127.0.0.1:6379> hset hash name qdw
(integer) 1
#获取值
127.0.0.1:6379> hget hash name
"qdw"
#同时设置多个
127.0.0.1:6379> hmset hash age 10 sex 1
OK
#获取多个值
127.0.0.1:6379> hmget hash name age
1) "qdw"
2) "10"
#获取所有键值对
127.0.0.1:6379> hgetall hash
1) "name"
2) "qdw"
3) "age"
4) "10"
5) "sex"
6) "1"

~~~

### 删除

~~~bash
#删除指定键的数据
127.0.0.1:6379> hdel hash sex
(integer) 1
127.0.0.1:6379> hgetall hash
1) "name"
2) "qdw"
3) "age"
4) "10"

~~~

### 获取键值对数量

~~~bash
127.0.0.1:6379> hlen hash
(integer) 2
~~~

### 判断字段（键）是否存在

~~~bash
127.0.0.1:6379> HEXISTS hash name
(integer) 1
~~~

### 只获取所有字段（键）

~~~bash
127.0.0.1:6379> hkeys hash
1) "name"
2) "age"
~~~

### 只获取所有值

~~~bash
127.0.0.1:6379> hvals hash
1) "qdw"
2) "10"
~~~

### 自增自减

~~~bash
27.0.0.1:6379> hset hash money 1
(integer) 1
#指定的字段+1
127.0.0.1:6379> HINCRBY hash money 1
(integer) 2
127.0.0.1:6379> hget hash money
"2"
#指定字段-1
127.0.0.1:6379> HINCRBY hash money -1
(integer) 1
127.0.0.1:6379> hget hash money
"1"
~~~

### 如果存在就创建失败

~~~bash
127.0.0.1:6379> hsetnx hash name qqq
(integer) 0
~~~



hash相比String更适合存储对象

String更是个存储字符串

> 应用场景1：电商网站购物车设计与实现。

解决方案：

- 用户id作为key，每位客户创建一个hash存储结构存储对应的购物车信息。
- 将商品编号作为field，购买数量作为value进行存储。
- 添加商品：追加全新的field和value。
- 浏览：遍历hash。
- 更改数量：自增/自减，设置value值。
- 删除商品：删除field。
- 清空：删除key。

但是用户要想看到具体的商品，还要去数据库查。到此为止没有起到加速的作用。

- 商品记录分成两条field保存
  - 商品id:nums
    - 数值。保存商品数量。
  - 商品id:info
    - json。保存具体商品的数据，比如文字描述，图片等。

这样商品信息会在不同用户的数据中重复。所以要把商品信息独立成一个Hash。

- 专门保存把商品信息作为一组hash保存，不涉及用户。
  - 在存储的时候，可以用hsetnx，如果没有这个field就加，如果有就不加。



> 应用场景2：抢购。商家上架几种商品一定数量，让用户来抢。



- 商家id作为key。
- 参与抢购的商品id作为field。
- 将参与抢购的商品数量作为对应的value。
- 抢购时用降值的方式控制产品数量。hincrby。
- 超卖问题这里不讨论。



> String和hash存对象区别？
>
> String讲究一致性。
>
> hash讲究字段更新方便。





## List（列表）

特点：元素有序，可以重复。

list可以实现成栈，队列，阻塞队列

所有的list命令都是l开头的

### 插入取出

~~~bash
#将一个值从list左边插入
127.0.0.1:6379> lpush list one
(integer) 1
127.0.0.1:6379> lpush list two
(integer) 2
127.0.0.1:6379> lpush list three
(integer) 3
#从左边按范围取出
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> lrange list 0 1
1) "three"
2) "two"
#从右边插入
127.0.0.1:6379> rpush list right
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"

# 向存在的列表中添加，如果不存在也不会创建list
lpushx key a

~~~

### 移除元素

~~~bash
#从左边移除一个元素
127.0.0.1:6379> lpop list
"three"
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
3) "right"
#从右边移除一个元素
127.0.0.1:6379> rpop list
"right"
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"

~~~

### 获取指定下标的元素

~~~bash
#通过下标获取值
127.0.0.1:6379> lindex list 1
"one"
#返回列表的长度
127.0.0.1:6379> llen list
(integer) 2

~~~

### 移除指定的值

~~~bash
127.0.0.1:6379> lrange list 0 -1
1) "one"
2) "two"
3) "one"
#移除一个two
127.0.0.1:6379> lrem list 1 two
(integer) 1
127.0.0.1:6379> lrange list 0 -1
1) "one"
2) "one"
#移除两个one
127.0.0.1:6379> lrem list 2 one
(integer) 2
127.0.0.1:6379> lrange list 0 -1
(empty list or set)

~~~

### 截取list

~~~bash
127.0.0.1:6379> rpush list hello
(integer) 1
127.0.0.1:6379> rpush list hello1
(integer) 2
127.0.0.1:6379> rpush list hello12
(integer) 3
127.0.0.1:6379> rpush list hello123
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "hello"
2) "hello1"
3) "hello12"
4) "hello123"
#通过下标截取指定的长度，列表只剩下截取的部分
127.0.0.1:6379> ltrim list 1 2 
OK
127.0.0.1:6379> lrange list 0 -1
1) "hello1"
2) "hello12"

~~~

### 把元素移到另一个列表

把一个list的左边一个元素，放到另一个list的右边

~~~bash
#把list中的右边的一个元素，移动到mylist的左边
127.0.0.1:6379> rpoplpush list mylist
"hello12"
127.0.0.1:6379> lrange list 0 -1
1) "hello1"
#元素到了mylist里面
127.0.0.1:6379> lrange mylist 0 -1
1) "hello12"

~~~

### 给列表中的指定元素重新赋值

如果列表不存在，或者指定位置无元素就会报错

~~~bash
127.0.0.1:6379> lrange list 0 -1
1) "hello1"
#给列表的指定元素重新赋值
127.0.0.1:6379> lset list 0 world
OK
127.0.0.1:6379> lrange list 0 -1
1) "world"
#指定的位置无元素就会报错
127.0.0.1:6379> lset list 1 world
(error) ERR index out of range

~~~

### 在元素的前或后插入元素

~~~bash
127.0.0.1:6379> lpush list hello
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "hello"
2) "world"
#在一个元素左（后）边插入一个元素
127.0.0.1:6379> linsert list before world my
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "hello"
2) "my"
3) "world"
#在一个元素右（前）边插入一个元素
127.0.0.1:6379> linsert list after my new
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "hello"
2) "my"
3) "new"
4) "world"
~~~

> 他实际上是一个链表
>
> list中存储的数据都是string类型的，数据总容量时有限的。
>
> list可以对数据进行分页操作，通常第一页的信息放在redis的list里，第二页以及更多的信息再通过数据库的形式加载。



> 应用场景：
>
> 1. 微博的应用的个人用户的关注列表，按照用户关注的顺序进行展示。粉丝列表中的最近关注的粉丝排在前面。
> 2. 新闻资讯类，信息通过发生时间展示。
> 3. 多态服务器的产出日志按顺序输出。
>    1. ![image-20200505204752747](Redis.assets/image-20200505204752747.png)
>
> 

解决方案

- 使用栈模型解决最新消息问题
- 使用队列模型解决多录信息汇总合并问题









## Set（集合）

set中的元素不能重复

### 添加元素，查看所有，判断是否在set中，获取元素数量

~~~bash
127.0.0.1:6379> sadd set hello
(integer) 1
127.0.0.1:6379> sadd set qdw world
(integer) 2
127.0.0.1:6379> smembers set
1) "qdw"
2) "world"
3) "hello"
#判断元素是否在set中
127.0.0.1:6379> sismember set qdw
(integer) 1
127.0.0.1:6379> sismember set qqq
(integer) 0
#获取set中元素数量
127.0.0.1:6379> scard set
(integer) 3
~~~

### 移除元素

~~~bash
#移除指定元素
127.0.0.1:6379> srem set qdw
(integer) 1
127.0.0.1:6379> smembers set
1) "world"
2) "hello"

~~~

### 随机获取/移除一个元素

（这里指令是大写，因为用了自动提示，redis不区分大小写）

~~~bash
#随机获取一个元素
127.0.0.1:6379> SRANDMEMBER set
"hello"
127.0.0.1:6379> SRANDMEMBER set
"hello"
127.0.0.1:6379> SRANDMEMBER set
"world"
#随意移除一个元素
127.0.0.1:6379> spop set
"hello"
127.0.0.1:6379> SMEMBERS set
1) "world"

~~~

### 移动到另一个set

~~~bash
127.0.0.1:6379> sadd set qdw hello
(integer) 2
127.0.0.1:6379> SMEMBERS set
1) "qdw"
2) "world"
3) "hello"
#把一个元素移动到另一个set
127.0.0.1:6379> smove set myset qdw
(integer) 1
127.0.0.1:6379> SMEMBERS set
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset
1) "qdw"

~~~

### 差集

~~~bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> sadd set1 a b c
(integer) 3
127.0.0.1:6379> sadd set2 c d e
(integer) 3
#比较两个set，获取差集
127.0.0.1:6379> SDIFF set1 set2
1) "a"
2) "b"

~~~

### 交集

~~~bash
#比较两个set，获取交集
127.0.0.1:6379> SINTER set1 set2
1) "c"
~~~

### 并集

~~~bash
#比较两个set，获取并集
127.0.0.1:6379> SUNION set1 set2
1) "c"
2) "e"
3) "a"
4) "b"
5) "d"

~~~



> 应用场景1：公司每个员工有一个或多个角色，如果进行业务操作的权限校验。一个角色的操作权限有很多，多个角色的权限可能又重叠，怎样解决重叠问题。

解决方案：

- 依赖set中元素不重复，对同类型不重复数据进行合并操作
- 根据用户id获取用户的所有角色
- 根据用户所有角色获取用户所有权限放入这个用户的权限set集合
- 想知道用户是否有权限，从权限set中获取所有操作，去比较。



> 应用场景2：统计网站的PV（访问量），UV（独立访客），IP（独立IP）

解决方案：

- 利用string，用incr统计日访问量（PV）
- 利用set去对同类数据进行去重
- 利用set，记录不同cookie数量（UV）
- 利用set，记录不同IP数量（IP）

> 应用场景3：
>
> 黑名单：
>
> 白名单：

解决方案：

- 利用set，周期性更新用户黑名单
- 用户到达后与黑名单比对，确认去向
- 过滤IP地址：应用于开放游客访问权限的信息源
- 过滤设备信息：应用于限定访问设备的信息源
- 过滤用户：应用于基于访问权限的信息源





## Sorted_set（有序集合）

在set基础上，增加了一个值，来分组排序

![image-20200511214953386](Redis.assets/image-20200511214953386.png)

### 添加

~~~bash
127.0.0.1:6379> zadd zset 1 a
(integer) 1
127.0.0.1:6379> zadd zset 2 b
(integer) 1
127.0.0.1:6379> zadd zset 2 c 3 d
(integer) 2
127.0.0.1:6379> zrange zset 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
~~~

### 排序

~~~bash
#升序排列
127.0.0.1:6379> ZRANGE zset 0 -1
1) "a"
2) "b"
3) "c"

#降序排列
127.0.0.1:6379> ZREVRANGE zset 0 -1
1) "c"
2) "b"
3) "a"

#在范围里，获取升序排列
127.0.0.1:6379> ZRANGEBYSCORE zset -inf +inf
1) "a"
2) "b"
3) "c"
4) "d"
#带分数来排序
127.0.0.1:6379> ZRANGEBYSCORE zset 0 10 withscores
1) "a"
2) "1"
3) "b"
4) "2"
5) "c"
6) "2"
7) "d"
8) "3"
#在范围里降序排序
127.0.0.1:6379> ZREVRANGEBYSCORE zset 10 0
1) "d"
2) "c"
3) "b"
4) "a"

~~~

可以用 (0 表示开区间

### 移除

~~~bash
127.0.0.1:6379> ZRANGE zset 0 -1
1) "a"
2) "b"
3) "c"
4) "d"
#移除元素
127.0.0.1:6379> zrem zset d
(integer) 1
127.0.0.1:6379> ZRANGE zset 0 -1
1) "a"
2) "b"
3) "c"
~~~

### 获取个数

~~~bash
#获取集合的元素个数
127.0.0.1:6379>  zcard zset
(integer) 3
#获取范围里元素的个数
127.0.0.1:6379> zcount zset 1 2
(integer) 3

~~~



> 业务场景：各种排序：活跃度、各种排行榜。

解决方案

- 获取数据对应的索引（排名）
  - zrank
  - zrevrank
- score值获取与修改
  - zscore
  - zincrby



# 三种特殊数据类型

## geospatial 地理位置

只有六个命令

![image-20200406153614156](Redis.assets/image-20200406153614156.png)

- 有效的经度从-180度到180度。
- 有效的纬度从-85.05112878度到85.05112878度。

### 添加地理位置

~~~bash
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 108.96 34.26 xian 114.05 22.52 shenzhen
(integer) 2
~~~

### 获取指定城市的经度和纬度

~~~bash
127.0.0.1:6379> geopos china:city beijing xian
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
2) 1) "108.96000176668167114"
   2) "34.25999964418929977"

~~~

### 获取两个位置之间的距离

如果两个位置之间的其中一个不存在， 那么命令返回空值。

指定单位的参数 unit 必须是以下单位的其中一个：

- **m** 表示单位为米。
- **km** 表示单位为千米。
- **mi** 表示单位为英里。
- **ft** 表示单位为英尺。

~~~bash
127.0.0.1:6379> geodist china:city beijing xian
"910056.5237"
#指定了单位
127.0.0.1:6379> geodist china:city beijing xian km
"910.0565"

~~~

### 按指定坐标的半径搜索包含的位置

~~~bash
127.0.0.1:6379> georadius china:city 110 30 1000 km
1) "xian"
2) "shenzhen"
127.0.0.1:6379> georadius china:city 110 30 1500 km
1) "xian"
2) "shenzhen"
3) "shanghai"
4) "beijing"
#显示距离
127.0.0.1:6379> georadius china:city 110 30 1000 km withdist
1) 1) "xian"
   2) "483.8340"
2) 1) "shenzhen"
   2) "924.6408"
#显示经纬度
127.0.0.1:6379> georadius china:city 110 30 1000 km withcoord
1) 1) "xian"
   2) 1) "108.96000176668167114"
      2) "34.25999964418929977"
2) 1) "shenzhen"
   2) 1) "114.04999762773513794"
      2) "22.5200000879503861"
#规定数量
127.0.0.1:6379> georadius china:city 110 30 1000 km withdist withcoord count 1
1) 1) "xian"
   2) "483.8340"
   3) 1) "108.96000176668167114"
      2) "34.25999964418929977"

~~~



### 按指定位置的半径搜索包含的位置

~~~bash
127.0.0.1:6379> GEORADIUSBYMEMBER china:city xian 1000 km
1) "xian"
2) "beijing"
~~~

### 返回11个字符的geohash字符串

将经纬度转换为11个字符的hash值

~~~bash
127.0.0.1:6379> geohash china:city beijing xian
1) "wx4fbxxfke0"
2) "wqj6zky6bn0"
~~~



### 底层是zset，可以用一些zset的命令

~~~bash
127.0.0.1:6379> zrange china:city 0 -1
1) "xian"
2) "shenzhen"
3) "shanghai"
4) "beijing"
127.0.0.1:6379> zrem china:city shenzhen
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "xian"
2) "shanghai"
3) "beijing"

~~~



## Hyperloglog

> 什么是基数？

找不重复的元素的个数

### Hyperloglog用来做基数统计

传统方式，set保存，统计set容器中的元素数量

如果元素数量过大，会占过多资源

用Hyperloglog占用内存很小，但是有0.81%的错误率

~~~bash
#添加元素
127.0.0.1:6379> pfadd key a b c d e f g
(integer) 1
#查看数量
127.0.0.1:6379> pfcount key
(integer) 7
#加进去重复的，并不算
127.0.0.1:6379> pfcount key a b c
(integer) 7
127.0.0.1:6379> pfcount key
(integer) 7
127.0.0.1:6379> pfadd key2 a b c h i j
(integer) 1
#合并两个key，把结果放到新key中
127.0.0.1:6379> pfmerge key3 key key2
OK
127.0.0.1:6379> pfcount key3
(integer) 10

~~~



## Bitmap

位图。通过一个bit位来表示某个元素的状态

~~~bash
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 0
(integer) 0
127.0.0.1:6379> setbit sign 3 1 
(integer) 0
#统计值为1的个数
127.0.0.1:6379> bitcount sign
(integer) 2
~~~

最后判断什么位置是0 什么位置是1

### 应用

签到。key位年份+用户id，offset为hash%365



统计日活。key为天，每天的用户日活过就设置为1。可以通过集合操作来表示一定日期内的日活

# Redis事务

### Redis的事务和关系型数据库中事务的区别

- 不保证原子性

- 没有隔离级别的概念

- 单条命令是保证原子性的

Redis的一个事务中的所有命令会被序列化，在事务执行过程中，会按照顺序执行

- 一次性
- 顺序性
- 排他性

所有的命令在事务中，没有直接执行，只有发起执行命令才会执行



### 流程

- 开启事务（multi）
- 命令入队（。。。）
- 执行事务（exec）

~~~bash
#开启事务
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k1 v1
QUEUED
127.0.0.1:6379> set k2 v2
QUEUED
127.0.0.1:6379> get k2
QUEUED
#执行事务
127.0.0.1:6379> exec
1) OK
2) OK
3) "v2"

~~~

- 放弃事务

事务中的命令不会执行

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k3 v3
QUEUED
127.0.0.1:6379> get k3
QUEUED
127.0.0.1:6379> DISCARD
OK
127.0.0.1:6379> get k3
(nil)

~~~



### 遇到错误

- 编译型异常

事务中的所有命令都不会执行

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> incr k4
QUEUED
127.0.0.1:6379> set k5 v5
QUEUED
127.0.0.1:6379> get k5
QUEUED
127.0.0.1:6379> exec
1) OK
2) (error) ERR value is not an integer or out of range
3) OK
4) "v5"

~~~



- 运行时异常

**如果存在语法型错误，执行命令时，其他命令可以正常执行**

~~~bash
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set k4 v4
QUEUED
127.0.0.1:6379> set k5
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379> set k6 v6
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get k4
(nil)
127.0.0.1:6379> get k5
(nil)

~~~



## 乐观锁、悲观锁

悲观锁：

- 悲观，认为什么时候都会出问题，做什么都会加锁

乐观锁：

- 乐观，认为什么时候都不会出问题，所以不上锁，更新数据时区判断一下，此期间是否有人修改过这个数据



### 实现乐观锁

**使用watch相当于乐观锁**



正常执行

~~~bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
#监视money
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> DECRBY money 20
QUEUED
127.0.0.1:6379> INCRBY out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20

~~~

事务从开始到执行前，数据没有发生变动，这个时候执行成功

如果同时有两个事务，在一个事务开始后，提交前，修改是watch的对象，这个事务提交就不能执行成功

~~~bash
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> set money 100
OK
127.0.0.1:6379> set out 0
OK
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
#没有提交事务
127.0.0.1:6379> 

~~~

连接另一个cli，对money操作

~~~bash
127.0.0.1:6379> keys *
1) "money"
2) "out"
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20

~~~

前面watch的那个事务执行，会失败

~~~bash
127.0.0.1:6379> exec
(nil)

~~~



如果想把接下来的事做完

先取消监视，然后从新监视，然后接着做要做的事

~~~bash
127.0.0.1:6379> unwatch
OK
127.0.0.1:6379> watch money
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 10
QUEUED
127.0.0.1:6379> incrby out 10
QUEUED
127.0.0.1:6379> exec
1) (integer) 70
2) (integer) 30

~~~

# Jedis

官方推荐的java连接开发工具。使用java操作Redis中间件

## 导入依赖

~~~
    <dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.2.0</version>
        </dependency>
        <dependency>
            <groupId>cn.bestwu</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2-unwrap</version>
        </dependency>
    </dependencies>
~~~

## 命令测试

默认redis只允许本地访问，这里测试本地访问

~~~java
public class TestPing {
    public static void main(String[] args) {
        //1.new一个对象
        Jedis jedis = new Jedis("127.0.0.1",6379);
        String ping = jedis.ping();
        System.out.println(ping);
    }
}
~~~



### 常用API

- String
- List
- Set
- Hash
- Zset

和上面的指令是一样的

Stirng的例子

~~~java
public class TestKey {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost",6379);
        System.out.println(jedis.ping());
        System.out.println(jedis.flushDB());
        System.out.println(jedis.set("name", "qdw"));
        System.out.println(jedis.set("age", "10"));
        System.out.println(jedis.keys("*"));
        System.out.println(jedis.del("age"));
        System.out.println(jedis.exists("age"));
        System.out.println(jedis.type("name"));
        System.out.println(jedis.randomKey());
        jedis.rename("name","name1");
        System.out.println(jedis.get("name1"));
        System.out.println(jedis.select(1));
        System.out.println(jedis.dbSize());
        System.out.println(jedis.keys("*"));
        System.out.println(jedis.flushAll());
    }
}
~~~



### 事务

~~~java
public class TestTx {
    public static void main(String[] args) {

        Jedis jedis = new Jedis("127.0.0.1",6379);

        JSONObject jsonObject = new JSONObject();
        jsonObject.put("hello","world");
        jsonObject.put("name","qdw");
        String res = jsonObject.toJSONString();
        jedis.watch("user1");
        //开启事务
        Transaction multi = jedis.multi();
        try {
            multi.set("user1",res);
            multi.set("user2",res);
            multi.exec();
        } catch (Exception e) {
            //放弃事务
            multi.discard();
            e.printStackTrace();
        } finally {
            System.out.println(jedis.get("user1"));
            System.out.println(jedis.get("user2"));
            //关闭连接
            jedis.close();
        }
    }
}
~~~



# Springboot整合





![image-20200407232548705](Redis.assets/image-20200407232548705.png)

或者直接添加这个依赖

~~~
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

配置

~~~
spring:
  redis:
    database: 1
    host: 49.233.90.47
    port: 6379
~~~





> springboot2.0以后jedis被替换哼了lettuce

- jedis：采用直连，多个线程操作时不安全的，使用jedis pool连接池解决
- lettuce：采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况





## 自动配置的源码



~~~java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
@Import({ LettuceConnectionConfiguration.class, JedisConnectionConfiguration.class })
public class RedisAutoConfiguration {

	@Bean
    //可以替换redisTemplate
	@ConditionalOnMissingBean(name = "redisTemplate")
	public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
			throws UnknownHostException {
		RedisTemplate<Object, Object> template = new RedisTemplate<>();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

	@Bean
	@ConditionalOnMissingBean
    //因为String类型很常用，说以单独成一个bean
	public StringRedisTemplate stringRedisTemplate(RedisConnectionFactory redisConnectionFactory)
			throws UnknownHostException {
		StringRedisTemplate template = new StringRedisTemplate();
		template.setConnectionFactory(redisConnectionFactory);
		return template;
	}

}
~~~





## 测试



配置

~~~
spring:
  redis:
    host: 127.0.0.1
    port: 6379
~~~



测试

~~~java
@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void contextLoads() {

        //opsForValue(),操作字符串
        redisTemplate.opsForValue().set("name","qdw");
        //opsForList(),操作list
        redisTemplate.opsForList().leftPush("user","qdw");
    }

}
~~~







![image-20200408000149153](Redis.assets/image-20200408000149153.png)





配置RedisTemplate

~~~java
@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }
}
~~~





# 自定义RedisTemplate



## 序列化

如果不对存储的对象序列化，会报错

~~~java
    public void test1() throws JsonProcessingException {
        User user = new User("qdw",10);
//        String jsonUser = new ObjectMapper().writeValueAsString(user);
        //开发一般用json来传递对象
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
~~~

![image-20200408150454700](Redis.assets/image-20200408150454700.png)



序列化后就不会报错了

~~~java
@Component
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User implements Serializable {
    private String name;
    private int age;
}
~~~



一般会用json传对象

~~~java
@Test
    public void test1() throws JsonProcessingException {
        User user = new User("qdw",10);
        String jsonUser = new ObjectMapper().writeValueAsString(user);
        //开发一般用json来传递对象
        redisTemplate.opsForValue().set("user",jsonUser);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }
~~~



默认是用JDK的序列化方式，也可以使用别的方式

## 配置如下

~~~java
@Configuration
public class RedisConfig {

    @Bean
    //前面的类型换成String，为了使用方便
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        //json序列化配置
        Jackson2JsonRedisSerializer objectJackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        objectJackson2JsonRedisSerializer.setObjectMapper(objectMapper);
        //String序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        //key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        //hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        //value采用json的序列化方式
        template.setValueSerializer(objectJackson2JsonRedisSerializer);
        //hash的value也采用json的序列化方式
        template.setHashValueSerializer(objectJackson2JsonRedisSerializer);

        template.afterPropertiesSet();
        return template;
    }
}
~~~



测试

~~~java
@SpringBootTest
class Redis02SpringbootApplicationTests {
    @Autowired
    @Qualifier("redisTemplate")
    private RedisTemplate redisTemplate;

    @Test
    public void test1() throws JsonProcessingException {
        User user = new User("qdw",10);
//        String jsonUser = new ObjectMapper().writeValueAsString(user);
        //开发一般用json来传递对象
        redisTemplate.opsForValue().set("user",user);
        System.out.println(redisTemplate.opsForValue().get("user"));
    }

}
~~~

![image-20200408154435174](C:\Users\qdw49\AppData\Roaming\Typora\typora-user-images\image-20200408154435174.png)

~~~bash
127.0.0.1:6379> keys *
1) "user"
~~~



## 编写工具类

~~~java
package com.qdw.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

// 在我们真实的分发中，或者你们在公司，一般都可以看到一个公司自己封装RedisUtil
@Component
public final class RedisUtil {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;
    
    // =============================common============================
    /**
     * 指定缓存失效时间
     * @param key  键
     * @param time 时间(秒)
     */
    public boolean expire(String key, long time) {
        try {
            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }


    /**
     * 判断key是否存在
     * @param key 键
     * @return true 存在 false不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除缓存
     * @param key 可以传一个值 或多个
     */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete(CollectionUtils.arrayToList(key));
            }
        }
    }


    // ============================String=============================

    /**
     * 普通缓存获取
     * @param key 键
     * @return 值
     */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }
    
    /**
     * 普通缓存放入
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */

    public boolean set(String key, Object value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 普通缓存放入并设置时间
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */

    public boolean set(String key, Object value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 递增
     * @param key   键
     * @param delta 要增加几(大于0)
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }


    /**
     * 递减
     * @param key   键
     * @param delta 要减少几(小于0)
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }


    // ================================Map=================================

    /**
     * HashGet
     * @param key  键 不能为null
     * @param item 项 不能为null
     */
    public Object hget(String key, String item) {
        return redisTemplate.opsForHash().get(key, item);
    }
    
    /**
     * 获取hashKey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }
    
    /**
     * HashSet
     * @param key 键
     * @param map 对应多个键值
     */
    public boolean hmset(String key, Map<String, Object> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * HashSet 并设置时间
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, Object> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, Object... item) {
        redisTemplate.opsForHash().delete(key, item);
    }


    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }


    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }


    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }


    // ============================set=============================

    /**
     * 根据key获取Set中的所有值
     * @param key 键
     */
    public Set<Object> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, Object value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, Object... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSetAndTime(String key, long time, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0)
                expire(key, time);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 获取set缓存的长度
     *
     * @param key 键
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */

    public long setRemove(String key, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    // ===============================list=================================
    
    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束 0 到 -1代表所有值
     */
    public List<Object> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 获取list缓存的长度
     *
     * @param key 键
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     */
    public Object lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     */
    public boolean lSet(String key, Object value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将list放入缓存
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     */
    public boolean lSet(String key, Object value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, List<Object> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */

    public boolean lUpdateIndex(String key, long index, Object value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */

    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }

    }

}
~~~



测试

~~~java
@Test
    public void test2(){
        User user = new User("qdw",11);
        redisUtil.set("user",user);
        System.out.println(redisUtil.get("user"));
    }
~~~

![image-20200408155833750](Redis.assets/image-20200408155833750.png)



# 配置文件详解

打开配置文件，这里打开远程linux的redis原始目录里的配置文件redis.conf

## 单位

![image-20200408163219505](Redis.assets/image-20200408163219505.png)



### 单位的写法，对大小写不敏感



## 包含

![image-20200408163434801](Redis.assets/image-20200408163434801.png)



## 网络

绑定ip

~~~bash
bind 127.0.0.1
~~~



![image-20200408163718876](Redis.assets/image-20200408163718876.png)



保护模式

~~~bash
protected-mode yes
~~~

端口

~~~bash
port 6379
~~~

![image-20200408164854373](Redis.assets/image-20200408164854373.png)



## 通用

守护模式（后台开启）

~~~bash
daemonize no
~~~



如果后台开启，就需要指定一个pid文件

~~~bash
pidfile /var/run/redis_6379.pid
~~~



日志级别，和文件名

~~~bash
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice
# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile ""
~~~



![image-20200408165158019](Redis.assets/image-20200408165158019.png)



数据库数量

~~~bash
databases 16
~~~

是否显示logo

~~~bash
always-show-logo yes
~~~



## 快照

在规定的时间内，执行了多少次操作，会持久化到文件

持久化配置

~~~bash
#如果在900秒内，如果至少有1个key修改，就进行持久化操作
save 900 1
#如果在300秒内，如果至少有10个key修改，就进行持久化操作
save 300 10
#如果在60秒内，如果至少有10000个key修改，就进行持久化操作
save 60 10000
#持久化出错，是否继续工作
stop-writes-on-bgsave-error yes
#是否压缩rdb文件
rdbcompression yes
#保存rdb文件的时候，进行错误检查
rdbchecksum yes
#文件保存目录
dir ./
~~~





## 复制

主从复制再讲





## 安全

设置密码

默认没有密码

~~~bash
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
~~~

通过命令设置密码

~~~bash
127.0.0.1:6379> config set requirepass 123321
OK
127.0.0.1:6379> config get requirepass
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123321
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "123321"
~~~





## 客户端

最大客户端连接数量

~~~bash
maxclients 10000

~~~



## 内存



~~~bash
#最大内存限制
maxmemory <bytes>
#内存满了的处理策略
#1、volatile-lru：只对设置了过期时间的key进行LRU（默认值） 
#2、allkeys-lru ： 删除lru算法的key   
#3、volatile-random：随机删除即将过期key   
#4、allkeys-random：随机删除   
#5、volatile-ttl ： 删除即将过期的   
#6、noeviction ： 永不过期，返回错误
maxmemory-policy noeviction
~~~



## APPEND ONLY MODE （AOF模式配置）



~~~bash
#默认不开启，默认使用rdb来做持久化
appendonly no
#持久化文件名字
appendfilename "appendonly.aof"

#同步的频率
# appendfsync always
appendfsync everysec
# appendfsync no

~~~



# 进阶

## 总览

### 技术线

Redis技术分为三个主线

- 高性能主线
  - 线程模型
  - 数据结构
  - 持久化
  - 网络框架
- 高可靠主线
  - 主从复制
  - 哨兵机制
- 高可扩展主线
  - 数据分片
  - 负载均衡

### 问题画像

![img](https://static001.geekbang.org/resource/image/70/b4/70a5bc1ddc9e3579a2fcb8a5d44118b4.jpeg)



## 存储数据结构

### 数据类型和底层数据结构的对应关系

value的五种基础数据类型（String、List、Hash、Set、Sorted Set）底层的数据结构有六种：简单动态字符串、双向链表、压缩列表、哈希表、跳表、整数数组。

对应关系如下：

![img](https://static001.geekbang.org/resource/image/82/01/8219f7yy651e566d47cc9f661b399f01.jpg)

其中只有String类型只对应一个数据结构，而剩下四种类型都有两种数据结构的实现，这是个类型属于集合类型。

### key和value的组织结构，渐进性Hash

从key到value的查找通过哈希表，也就是一个数组，哈希表中的哈希桶就是数组中的元素，一个哈希桶中有key的指针和value的指针。

![img](https://static001.geekbang.org/resource/image/1c/5f/1cc8eaed5d1ca4e3cdbaa5a3d48dfb5f.jpg)

随着存入Redis的key变多，查询也变得越慢，因为要解决Hash冲突。Redis解决hash冲突使用拉链法。

![img](https://static001.geekbang.org/resource/image/8a/28/8ac4cc6cf94968a502161f85d072e428.jpg)

为了防止哈希链过长，Redis会通过rehash来增加桶数量。Redis创使用两个哈希表，当表1需要rehash扩容时，对表2增大容量，然后将表1中的数据进行转移。分为三个过程：

1. 为表2增加更大的空间
2. 把表1的数据重新映射并拷贝到表2
3. 把表1空间释放

为了减少rehash过程中转移数据所导致的的阻塞，Redis采用**渐进式rehash**：

在把表1的数据映射到表2的过程，分散给多个请求。当请求的key对应的桶还没有转移，还在表1时，将key映射到桶上所有的键值对重新进行hash转移到新的表2中。

对于找到value的操作的时间复杂度是O(1)。对于value是集合类型的要想找到集合中的数值，的复杂度要另外分析了。

### 底层数据结构

六种：简单动态字符串、整数数组、双向链表、哈希表、压缩列表、跳表

**简单动态字符串**：

保存有：

- 字符串长度
- 字符串每个元素
- 数组中未使用的字节数量

#### 相比于C字符串的优势

- 获取长度：C语言字符串需要遍历计数
- 缓冲区溢出：C语言字符串中使用strcat函数进行字符串拼接，一旦空间不足可能会造成字符串以外覆盖。



整数数组和双向链表都是顺序读写，通过数组下标或者链表的指针逐个元素访问，查询的时间复杂度都是O(N)。哈希表查询时间复杂度为O(1)。

**压缩列表**：类似数组，每个元素相邻保存一个数据。和数组不同的是，压缩列表**表头有三个字段**：zlbytes、zltail和zllen，分别存储列表长度，列表尾部偏移量和列表中元素enrty的个数；**尾部**还有一个zlend字段，表示列表结束。在查找开头或者结尾的元素的复杂度为O(1)，其他元素为O(N)。

![img](https://static001.geekbang.org/resource/image/95/a0/9587e483f6ea82f560ff10484aaca4a0.jpg)



#### 跳表

在链表的基础上，增加了多级索引，通过索引位置的跳转，加速数据的定位。

对原始链表中的部分节点添加索引，作为一级索引，在一级索引中的部分元素作为二级索引。整个查找的过程就是再多级索引上进行跳转，最终定位到原始链表节点。跳表查询的复杂度为O(logN).

##### sorted set为什么用跳表作为底层数据结构

需要**支持随机插入和删除**，不适合用数组。还需要有排序功能，那为什么不能用平衡树。

1. 性能考虑：高并发下，树形结构的重新平衡的操作可能涉及整个树，而**跳跃表只涉及局部**
2. 实现考虑：跳跃表实现相对简单。

##### 索引的创建原则

随机层数算法：50%创建一级索引，25%创建到二级索引，12.5%创建到三级索引，每一层的晋升率都是50%。默认允许的最大层数时32层。



## Redis为什么快

### 为什么使用单线程

Redis是单线程的。这里的单线程指Redis的网络IO和键值对读写是由一个线程完成的。但是Redis的其他功能，比如持久化、异步删除、集群数据同步等，是由额外的线程执行的。

如果使用多线程，面临着**共享资源的并发访问控制问题**。如果没有精细的设计，可能因为资源互斥而导致吞吐量并没有增加，并且并发设计会降低代码的可维护性，所以Redis直接采用单线程模式。

### 为什么单线程也快

为什么单线程的Redis可以达到每秒数十万级别的处理能力。一方面是因为在内存中存储数据，另一方面就是采用了**多路复用机制**，能使其在网络IO操作中处理大量客户端请求，实现高吞吐量。

#### 基本IO模型

客户端发送请求，服务器监听客户端请求（bind/listen），和客户端建立连接（accept）。从socket中读取请求（recv），解析客户端发送的请求（parse），根据请求类型读取数据（get），最后把结果返回给客户端，即向socket中写回数据（send）。

其中的阻塞点为accept()和recv()。在建立连接时，如果一直还没有成功建立，则会阻塞在accept()。在从客户端读取数据时，如果数据一直没有到达，则会阻塞在recv()。

#### 基于多路复用的IO模型

IO多路复用机制就是指一个线程处理多个IO流，就是常常听到的select/epoll机制。该机制允许内核中同时存在多个监听套接字和已连接套接字，并且提供了基于事件的回调机制，即对应不同事件的发生，调用相应的处理函数。

事件发生时加入处理队列，Redis线程针对队列中的事件，不断地处理事件回调函数。

![img](https://static001.geekbang.org/resource/image/00/ea/00ff790d4f6225aaeeebba34a71d8bea.jpg)



## 数据持久化

Redis有两种持久化机制：RDB和AOF

### RDB

RDB通过保存内存数据的快照文件，来持久化数据。RDB文件记录的是某一刻的数据，在数据恢复时，直接把RDB文件数据读到内存中。

#### 快照方式

Redis执行**全量快照**。全量数据越多，RDB文件越大，写入磁盘速度越慢。

Redis提供两种生成RDB文件的命令：save和bgsave。可以手动执行，也可以自动触发。

- 手动触发：在客户端中使用下面的命令
  - save：在主线程中执行生成RDB文件，阻塞进程，直到rdb文件创建完成。
  - bgsave：fork出一个子进程来创建rdb文件，主进程继续接收指令，避免了主线程的阻塞，是默认配置。
    - 使用子进程好处：
      - 避免主进程阻塞。
      - fork出的子进程在开始时会与父进程共享内存（直到对内存做写操作，共享结束），读取速度快
- 自动触发：
  - **在redis.conf中设置save选项**（save <seconds> <changes>），在save满足时（多长时间多有key发生变化）自动触发bgsave。如果设置多个，任意一个触发都会触发**bgsave**
  - **执行shutdown**且没有开启AOF持久化，执行的**save**命令
  - **主从复制**时，主节点自动触发



通过写时复制技术，在执行快照的同时，保证主线程可以继续处理写操作。没有修改数据的情况下，子线程共享主线程的内存，将数据写入磁盘中的RDB文件，当某个数据被修改后，将生成数据的副本，子线程把副本中的数据写入RDB。

#### 快照的频率

两次快照之间相隔的时间越短，在宕机是，丢失的数据越少。如果全量快照过于频繁，可能会造成如下两个问题：

- 磁盘压力：频繁地向磁盘写入数据，如果前一个快照没有做完，后一个又看是，会造成恶性循环。
- 主线程阻塞：在fork子线程时，会造成主线程阻塞，主线程的内存越大，阻塞时间越长。

#### 适合的场景

- 适合大规模的数据恢复
- 对数据的完整性要求不高

#### 优势

- 恢复数据速度快

#### 劣势

- 在两次生成快照的间隔中的数据可能会丢失。
- fork进程的时候，会导致主线程阻塞
- 快照频繁时，磁盘压力大



### AOF

在Redis执行完命令，将数据写入内存后，记录命令存在磁盘中的aof文件中。

用AOF恢复数据时，是将记录的操作重新执行一遍。

*3表示命令有三个部分，#3表示这部分有多少字节。

<img src="https://static001.geekbang.org/resource/image/4d/9f/4d120bee623642e75fdf1c0700623a9f.jpg" alt="img" style="zoom: 25%;" />

写后日志的好处：

- 不会阻塞当前的写操作
- 不会记录错误命令

风险：

- 执行完命令后，没来得急写日志就宕机了，数据无法恢复。
- AOF写入会影响下一个命令的执行。

风险都是和写回磁盘时机相关。

#### 写回策略

提供了三个选择，也就是AOF配置项的appendfsync的三个可选值。

- Always，同步写回：每个写命令执行完，立刻同步地写回到磁盘。
- Everysec，每秒写回：每个写命令执行完，把日志写到AOF文件的内存缓冲区，每隔一秒把缓冲区数据写入磁盘。
- No，操作系统控制的写回：每个写命令执行完，写入AOF文件缓冲区，由操作系统决定何时写入磁盘。

三种写回策略的优缺点

![img](https://static001.geekbang.org/resource/image/72/f8/72f547f18dbac788c7d11yy167d7ebf8.jpg)





#### 重写机制

aof方式会把所有写操作追加文件，造成文件越来越大，为了压缩aof文件，Redis提供了aof重写机制。

触发机制分为手动和自动。

- 客户端方式手动触发
  - 执行bgrewriteaof命令，子进程完成重写过程，不会造成主线程阻塞。
- 服务端配置方式自动触发
  - 配置redis.conf
    - 设置auto-aof-rewrite-minsize的值为64mb：表示到达64mb会做第一次重写，之后就不看这个了
    - 设置auto-aof-rewrite-percentage的值为100：表示容量大于上次重写后容量的百分之多少后，进行一次重写。数值越小越频繁。

重写aof不会读取旧的aof文件，而是将内存中的数据库内容用命令的方式重写了一个新的aof文件，替换原有文件。

流程：

1. redis调用fork，生成子进程，子进程根据内存数据生成快照文件，然后往临时文件中写入重建快照中的数据状态的命令。**（子进程生成快照，转换为写命令写入临时文件）**
2. 父进程继续处理客户端请求，把写命令写到原来的aof文件中，同时把命令缓存起来。**（父进程把新的命令写入旧aof文件并同时缓存起来）**
3. 当子进程将快照以命令的形式写入临时文件后，通知父进程。然后父进程把缓存的命令也追加到临时文件。**（子进程写好后父进程把缓存的新命令加入临时文件）**
4. 父进程把临时文件替换为旧的aof文件。**（父进程把临时文件替换旧aof文件）**

![img](https://static001.geekbang.org/resource/image/6b/e8/6b054eb1aed0734bd81ddab9a31d0be8.jpg)

#### 优点

- 数据完整性更好

缺点

- 恢复速度慢
- 日志文件体积大，需要额外进行重写







### 混合方式

Redis4.0提供了混合方式，结合RDB和AOF。快照以一定频率进行，在两次快照之间，使用AOF记录所有命令操作。

这样加快了恢复速度，避免了AOF重写，同时又能减少频繁快照带来的磁盘压力。

恢复数据时，先加载RDB，然后加载AOF

## 持久化拓展

![image-20200409194937718](Redis.assets/image-20200409194937718.png)

![image-20200409194948437](Redis.assets/image-20200409194948437.png)



## 主从复制

Redis的高可靠主要体现在：

- 数据尽量少丢失
- 服务尽量少中断

持久化解决前者，后者，Redis通过**增加副本冗余**来解决。一份数据同时保存在多个实例上。

### 如何保证多实例中的数据一致

Redis采用主从库模式，主从库之间采用**读写分离**的方式。

- 读操作：主库从库都可以接收。
- 写操作：首先到主库执行，然后将写操作同步给从库。

![img](https://static001.geekbang.org/resource/image/80/2f/809d6707404731f7e493b832aa573a2f.jpg)

如果不是主从库，保持多实例的数据一致性，需要涉及到加锁，会带来巨额的开销。

### 同步机制

在启动Redis多实例时，通过replicaof命令形成主库和从库的关系，然后按照三个阶段完成数据第一次同步。

![img](https://static001.geekbang.org/resource/image/63/a1/63d18fd41efc9635e7e9105ce1c33da1.jpg)

#### 第一阶段

主从建立连接，告诉主库即将进行同步。

从库给主库发送psync命令，表示要进行数据同步。命令包含两个参数：runID、offset。主库根据这个命令的参数来启动复制。

- runID：每个Redis实例启动时都会自动生成一个随机ID作为唯一标识。第一次复制时，因为不知道主库的runID，所以设为？。
- offset：值为-1，标识第一次复制。

主库用FULLRESYNC命令响应，带上主库的runID和主库目前的复制进度offset。

#### 第二阶段

主库将所有数据同步给从库，从库收到数据后，在本地完成数据加载。加载通过RDB文件进行。

过程：

- 主库执行**bgsave命令生成RDB文件**，发送给从库。
- 从库接收到后**清除数据库**，然后加载RDB文件。
- 在这个过程中主库继续接收请求，将RDB文件之后的所有写操作存储在replication buffer中。

#### 第三阶段

主库把第二阶段接收到的新的写命令发送给从库。从库重新执行，实现同步。

### 主从级联模式

主从全量复制时，如果从库很多，主库会忙于fork子进程来生成RDB文件，会阻塞处理请求。

通过“主-从-从”模式，将主库创建和传输RDB文件的压力，分摊到从库上。选择从库作为一个从库的从库。这种模式就是主从级联模式。

![img](https://static001.geekbang.org/resource/image/40/45/403c2ab725dca8d44439f8994959af45.jpg)



### 主从网络断开怎么解决

2.8之后，主从库采用**增量复制**来继续同步。

主从断连后，主库把期间收到的写操作命令，写入repl_backlog_buffer缓冲区。

主节点会写指令记录在buffer中，异步同步到从节点。buffer满后会循环覆盖，如果同步时间过长，就会丢失旧的数据。所以还需要快照同步。



## 哨兵模式

如果主库无法提供服务了，就需要哨兵模式解决主从复制模式下的故障转移。

哨兵主要负责三个任务：监控、选主、通知。

### 基本流程

监控：哨兵进程在运行时，周期性地给所有的主从库发送PING命令，检测是否在线。如果规定时间内没有相应，哨兵将其标记为下线状态，如果主库下线，则开始**自动切换主库**流程。

选主：哨兵从多个从库中，按照规则选择一个从库作为新的主库。

通知：哨兵把新主库的连接信息发送给其他从库，让他们执行replicaof命令，和新主库建立连接，并进行数据复制。同时哨兵会把新主库的连接信息通知给客户端，让他们把请求发送到新主库上。

![img](https://static001.geekbang.org/resource/image/ef/a1/efcfa517d0f09d057be7da32a84cf2a1.jpg)



### 主观下线和客观下线

对于主库的下线有两种判断：**主观下线**和**客观下线**。

哨兵进程通过ping来判断库是否在线，如果ping超时，则把库标记为**主观下线**。

为了防止对主库误判，利用哨兵集群，引入多个哨兵来判断主库是否真的下线了。大多数哨兵认为主库主观下线了，那么主库被**客观下线**。

### 如何选择新主库

一般哨兵选主库的方式为：**筛选 + 打分**。

- 通过在线状态、网络状况筛选掉一部分从库 
  - 不在线的和经常和主库断连的从库肯定是要被筛选掉的。
- 再根据优先级、复制进度、从库的id进行打分，得分最高的就是新的主库
  - 打分是分三轮，前一轮出现平分，才会继续下一轮打分。
  - 优先级：可以给从库设置不同的优先级
  - 复制进度：选择与主库最数据最接近的从库。通过master_repl_offset和slave_repl_offset比较，后者越接近前者说明数据越接近。
  - id：选择id最小的从库



## 哨兵集群

哨兵实例之间相互发现，主要通过发布/订阅机制。

### 建立集群

哨兵实例在主库上发布自己的频道，订阅其他哨兵频道，最终获取其他哨兵实例的地址。

![img](https://static001.geekbang.org/resource/image/ca/b1/ca42698128aa4c8a374efbc575ea22b1.jpg)

### 连接从库

哨兵实例向主库发送INFO命令，获取从库列表，从而连接到所有从库。

### 与客户端交互

本质上，哨兵是**运行在特定模式下的Redis实例，并不服务请求操作**，只完成**监控、选主、通知**。所以每个哨兵也提供pub/sub机制，客户端可以从哨兵订阅消息。

哨兵提供的订阅频道很多，不同频道包含了主从库切换过程中的不同关键事件。

![img](Redis.assets/4e9665694a9565abbce1a63cf111f725.jpg)

客户端可以通过订阅切换主库频道，来获取。



### 那个哨兵执行主从切换

和确定客观下线类似。哨兵通过投票选举出要进行主从切换的leader。一个哨兵要想成为leader需要获取过半的票数并且大于等于配置的quorum值。



## 数据不一致

缓存数据的一致性表现为：

- 缓存中有数据，且数值和数据库中的一致
- 缓存中无数据，数据库中的值为最新值

缓存分为**只读缓存和读写缓存**，根据缓存类型不同，不一致请求也不同。

**一般场景下，使用只读缓存，优先使用先更新数据库，再删除缓存**。前提是，允许短时间的数据不一致。

### 读写缓存

读写缓存需要在缓存中修改数据，根据采取的写回策略，决定什么时候写到数据库中。

两种写回策略

- 同步直写策略：写缓存时，同步写数据库，缓存和数据库中的数据一致
- 异步写回策略：写缓存时，异步写数据库

对于读写缓存，想保证数据一致，要使用同步直写策略。这种策略需要同时更新缓存的数据库，时写缓存和数据库具有原子性。

有些场景对一致性要求不那么高，可以使用异步写回策略。

### 只读缓存

对于只读缓存，如果有新增数据，直接新增到数据库；如果有数据修改，则在数据库中修改，然后将缓存置为无效。

分别考虑可能出现数据不一致的情况。

#### 新增数据

此时，数据库中新增的数据是最新值，缓存没有对应数据，所以具有一致性。

#### 修改数据

要修改数据库中数据，同时还要删除缓存中数据，如果无法保证原子性，就会出现数据不一致的问题。

- 先修改数据库，再删除缓存：
  - 如果修改缓存失败，再访问缓存是，读取的是旧值。
  - 刚修改完数据库，还没有删除缓存是，请求访问到缓存中的旧数据。对业务影响小。
- 先删除缓存，再修改数据库：
  - 如果修改数据库失败，缓存重新从数据库获取值是拿到的是旧值。
  - 刚删除缓存，还没有更新数据库，请求访问到了缓存，替换到了数据库中的旧值。

### 如何解决数据不一致

#### 对于两次修改，其中有失败导致的不一致

通过**重试机制**。

具体来说，将删除缓存或者修改数据库的操作暂存到消息队列，如果操作失败，可以从消息队列中重新读取这些值，再存进行更新。

如果操作成功，就将这些消息删除，避免重复操作。

#### 对于高并发下导致的不一致

通过**延时双删**。

影响较大的场景是，先删除缓存，后修改数据库。在刚删除缓存后，还没有修改数据库时，请求去访问了缓存，从数据库中拿到了旧数据。

在更新数据库后，sleep一小会，然后重新删除缓存。sleep是为了等其他线程去将缓存数据更新为数据库中的旧数据，然后再将缓存删除。一句话说：**先删除缓存，然后在数据更新到数据库后等一会再次删除缓存，叫做延时双删。**

这个sleep时间的确定需要统计线程读数据和缓存的时间，然后估算。



## 缓存更新方式

- 读写缓存：对缓存数据主动进行更新
  - 更新缓存，然后写回到数据库：只要能保证两个操作具有一致性，即要么全成功，要么全失败。优化：给缓存添加超时时间。
    - 写回策略：
      - 同步写回：写缓存时，同步写回到DB，一致性很强
      - 异步写回：写缓存后，异步写回到DB，一致性稍弱，但是查缓存就还好
- 只读缓存：不去主动更新缓存中的数据，只允许删除。当查询缓存中没有对应数据，会将DB中的数据自动添加到缓存。对于修改数据库的操作可以通过一些中间流程保证修改成功，比如**消息队列重试**。
  - 超时删除：只设置超时删除，完全依赖被动更新。适用于低一致性业务。
  - 对于新增数据：直接在DB中新增，具有一致性。（因为缓存中没有新增数据，会从DB中取）
  - 对于修改数据：删除缓存，被动更新缓存数据。
    - 先修改DB，再删除缓存：短暂时间的不一致，可以容忍。
    - 先删除缓存，再修改DB：为了防止修改DB前，被动更新了缓存，导致缓存中为旧数据，使用**延迟双删**机制。先删除缓存，在修改数据库成功后，等待一小段时间，再删除一次缓存。这段时间取决于，一个请求从DB更新数据到缓存的时间。



## 如何解决缓存穿透、击穿、雪崩

### 穿透

缓存穿透指：要访问的数据既不在缓存也不在数据库，导致在访问缓存时发生缓存确实，然后再去访问数据库，发现数据库中也没有，没发更新缓存，给数据库带来巨大压力。

![img](Redis.assets/46c49dd155665579c5204a66da8ffc2e.jpg)

发生原因：

- 业务层误操作：缓存和数据库中的数据被误删除了。
- 恶意攻击：专门访问数据库中没有的数据。

应对方案：

#### 缓存空值

查询数据库发现没有数据，可以在缓存中对应位置放置一个空值或者默认值。之后的请求查询直接从缓存中读取这个值。

对于设置空值前，这段时间为了防止大量请求同时访问，可以将查询数据库并且更新缓存的操作作为同步区。



#### 布隆过滤器判断

布隆过滤器是一种**Hash和Bitmap**结合的数据结构。可以把一个值经过不同的Hash得到几个索引值，映射到Bitmap中，设置为1。如果查询时发现对应Hash后的几个索引位都不全是1，说明肯定是不存在的，但是如果全是1，是不一定存在的。

基于布隆过滤器的快速检测特性，可以把数据写入数据库时，在布隆过滤器中做个标记。在应用查询数据库前先通过布隆过滤器进行判断，如果不存在就不查询数据库了。

布隆过滤器可以本地或者Redis实现。



#### 前端请求检测

在请求入口处，先过滤掉恶意请求。



### 缓存击穿

某个热点数据无法在缓存中提供，然后对数据库造成巨大压力。

![img](Redis.assets/d4c77da4yy7d6e34aca460642923ab4b.jpg)

经常发生在热点数据过期失效时。

解决方案

- 不设置过期时间





### 缓存雪崩

大量请求的数据无法在缓存中提供，造成数据库承担巨大压力。

发生原因和解决方案：

- **大量缓存数据同时过期**：通过下面两种方案降低同一时间的数据库访问量
  - 设置过期时间加入随机数
  - 服务降级，在发生雪崩时，根据不同数据采取不同处理方式
    - 非核心数据：提供空值、默认值、错误信息等
    - 核心数据：仍然允许查询缓存
- Redis实例故障：
  - 在业务系统中实现服务熔断或者限流
  - 提前预防：构建更加可靠的Redis集群



## 并发访问

“读取-修改-写回”操作（RMW操作）：对数据先查询，然后在本地修改，然后写回。

客户端对Redis中的数据进行并发更新，也就是先查询再更新，可能导致数据错误。比如扣库存错误，导致之后的下单异常。

Redis 提供了两种方法：加锁和原子操作。

加锁会降低并发性能。Redis加锁需要用到分布式锁，实现复杂。

### 原子操作

- 单命令操作：将多个操作在Redis中实现成一个操作。利用Redis处理请求单线程的原理。比如扣库存可以使用decr；分布式锁的加锁用set key value ex 10 nx
- Lua脚本：脚本中的命令会作为一个整体执行。



## 数据过期和淘汰策略

### 过期策略

#### 惰性删除

在客户端访问这个key时，对key做过期检查，如果过期了就立刻删除。

#### 定期删除

Redis会将所有设置了过期时间的key放入一个字典中。

Redis默认每秒进行10次过期扫描：每次扫描从过期字典中随机选出20个key，删除其中过期的key，如果过期key超过1/4则循环该过程。为了保证不会循环过渡，每次扫描默认25ms。

过期扫描下是无法响应客户端请求的。

#### 读从库

**从节点是不处理过期数据**的，只是被动的同步主节点的数据。主节点key到期删除后会在AOF文件中添加删除的指令。

所以读从库可能会读到脏数据

解决办法

- 通过一个程序循环遍历主节点的所有key。比如用scan
- 少数数据读主节点。
- 读从库通过tt判断。
- 升级到redis3.2。



### 内存淘汰

当Redis内存超出物理内存限制时，内存的数据会开始和磁盘产生频繁的交换（swap），让性能急速下降。

生产环境中不允许使用Redis出现交换行为，通过配置参数maxmemory来限制内存超出期望大小。

当实际内存超出maxmemory时，Redis提供几种可选策略，让用户决定如何淘汰数据。

- noeviction：不服务写请求，读和删请求可以继续进行，是默认的淘汰策略。
- allkeys-lru：淘汰最久没有访问的key
- volatile-lru：淘汰设置了过期时间数据中的最久没有访问的key
- allkeys-random：随机淘汰key
- volatile-random：随机淘汰设置了过期时间的key
- volatile-ttl：淘汰设置了过期时间的最先要过期的key
- volatile-lfu：淘汰设置了过期时间的访问频率最低的key
- allkeys-lfu：淘汰访问频率最低的key

### Redis的LRU实现

Redis使用的是一个近似的LRU算法。**对少量的键进行取样，淘汰其中最久未被使用的键。**通过调整采样的数量来调整算法的精读。

Redis对象维护了一个**24位空间，记录最后一次被访问的时间戳。最长存储194天的**。

当需要淘汰时，根据配置的策略从所有key中（或者是设置了过期时间的key中）随机选择n个（默认5个），从n个键中根据时钟选出最久没有使用的一个key进行淘汰。



### LFU实现

LFU是Redis4.0后出现的。LFU把之前key对象中的24位分成两部分：

前16位表示时钟，以时钟为单位。

后8位表示一个计数器，表示key的访问频率。8位只能表示255，Redis没有采用线性上升的方式，而是通过一个公式，再通过可配置的两个参数来调整速度递增的速度。



## 异步删除

### unlink

因为Redis是单线程的。如果被删除的是一个非常大的对象，比如很大的Hash或者list，那么删除操作会导致单线程卡顿。

为了解决这个卡顿问题，在4.0加入unlink指令，能对删除操作进行懒处理，都给后台线程来异步回收内存。

不是所有的unlink都会延后处理，如果key对象占用很小，就会立即删除。

### flush

flush指令后面加入async来进行异步删除

### AOF Sync

调用sync将缓存日志数据落盘的操作也让异步线程完成，这个异步线程是另外独立的，和懒惰删除线程不是一个线程。

# Redis发布订阅

## 简介

Redis发布订阅（pub/sub）是一种消息通信模式。发送者发送消息，订阅者接收消息。

Redis客户端可以订阅任意数量的频道



## 模型

![image-20200409211919304](Redis.assets/image-20200409211919304.png)

![image-20200409212316747](Redis.assets/image-20200409212316747.png)

![image-20200409212443524](Redis.assets/image-20200409212443524.png)



## 测试

在一个redis客户端上订阅一个频道

此时，这个客户端就会监听这个频道中的信息

~~~bash
127.0.0.1:6379> SUBSCRIBE qdw123
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "qdw123"
3) (integer) 1
~~~

打开另外一个客户端，在频道中发布信息

~~~bash
127.0.0.1:6379> PUBLISH qdw123 helloqdw
(integer) 1

~~~

订阅的客户端就会收到消息

~~~bash
127.0.0.1:6379> SUBSCRIBE qdw123
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "qdw123"
3) (integer) 1
1) "message"
2) "qdw123"
3) "helloqdw"
~~~



## 原理

> redis-server里维护了一个字典，字典的键就是一个个频道。字典的值是一个链表，链表保存了所有订阅这个频道的客户端。  
>
> 在Redis中，可以在一个key发布或者订阅一个key。当在一个key上发布消息后，所有订阅这个key的客户端都会收到消息。



# 主从复制

## 简介

> 主从复制，指将一台Redis服务器的数据，复制到其他的Redis服务器。前者称为主节点（master/leader），后者称为节点（slave/follower）  
>
> 数据的复制是单向的，只能从主节点到从节点。支持主从同步和从从同步。
>
> 默认每台Redis服务器都是主节点，一个主节点可以有多个从节点，一个从节点只能有一个主节点。
>
> 主从复制是Redis分布式的基础。

- 降低一致性：主从数据的同步是异步的（wait指令可体用一定的一致性）。读从机可能读到还未同步的旧数据。
- 满足可用性：即使在主从网络断开的情况下，主节点依然可以正常对外提供修改服务。
- 保证最终一致性：从节点会尽力将自己与主机同步。



## 作用

- 数据冗余：实现了数据的热备份
- 故障恢复：**主节点出问题，可以让从节点提供服务**
- 负载均衡：**读写分离**，分担服务器负载，多个从节点可以分担大量的读操作
- 高可用基石：是哨兵模式和集群能够实施的基础



![img](Redis.assets/14795543-6053e5014355dfd3.webp)



## 复制原理



### 增量同步

主节点会写指令记录在buffer中，异步同步到从节点。buffer满后会循环覆盖，如果同步时间过长，就会丢失旧的数据。所以还需要快照同步。



### 快照同步

主节点执行bgsave，从节点根据bgsave进行全量加载，然后接着进行增量同步。增量加载过程中如果时间过长，导致增量buffer中数据再次被覆盖，又会导致需要进行全量同步。可能会陷入快照同步死循环。



从节点成功连接到主节点后，会发送一个sync同步命令



- 全量复制：主节点接到后，启动后台的存盘进程，同时把当前接收到的修改数据命令也存起来，执行完毕后，主节点把收集好的整个数据发送到从节点，完成一次完全的同步，即全量复制
- 增量复制：主节点继续将新收集到的修改命令依次传给从节点，完成同步

从机每次连接到主机，都会自动执行一次全量复制



## 环境配置

查看信息

~~~bash
127.0.0.1:6379> info replication
# Replication
role:master #角色
connected_slaves:0 #没有从机
master_replid:e3366133ef5f5bc8d7e9615bd5c83becd1fca695
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
~~~

模拟多个服务器

复制多个配置文件，把配置文件中的一些配置修改，不能重复

~~~bash
port 6380
pidfile /var/run/redis_6380.pid
logfile "80.log"
dbfilename dump80.rdb
~~~

在不同的窗口使用不同的配置文件开启redis-server

查看

~~~bash
root@VM-0-3-ubuntu:/usr/local/bin/qdwconfig# ps -ef|grep redis
root     14498     1  0 19:31 ?        00:00:18 redis-server 127.0.0.1:6379
root     15216 11034  0 23:18 pts/0    00:00:00 redis-cli
root     17482     1  0 23:30 ?        00:00:00 redis-server 127.0.0.1:6380
root     17598     1  0 23:30 ?        00:00:00 redis-server 127.0.0.1:6381
root     17859 11058  0 23:32 pts/1    00:00:00 grep --color=auto redis

~~~



## 一主二从

默认每台都是主节点

一般只配置从机，找自己跟随的主机

### 客户端上配置主从

在想作为从机的客户端上配置自己的主机，把两台都连上

~~~bash
127.0.0.1:6380> SLAVEOF 127.0.0.1 6379
OK
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:up
master_last_io_seconds_ago:3
master_sync_in_progress:0
slave_repl_offset:14
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:50187f0d1c769525395350dc1554d45972bfffae
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:14
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:14

~~~

主机的信息

~~~bash
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6380,state=online,offset=280,lag=1
master_replid:50187f0d1c769525395350dc1554d45972bfffae
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:294
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:294
~~~



> 这样的配置时暂时的，实际上一般是在配置文件中配置主从的，是永久的

### 在配置中配置主从

~~~bash
#配置主机ip和端口
replicaof <masterip> <masterport>
#如果有密码，配置密码
asterauth <master-password>
~~~



### 作用

主机中写入，从机可以读到（主机中本来就有的数据，也会复制到从机）

~~~bash
127.0.0.1:6379> keys *
1) "k3"
2) "k1"
3) "k2"

127.0.0.1:6380> keys *
1) "k1"
2) "k3"
3) "k2"

~~~

从机不能写入

~~~bash
127.0.0.1:6380> set k4 v4
(error) READONLY You can't write against a read only replica.
~~~



没有设置哨兵模式，主机宕机后，从机的配置不会改变，依旧连接到主机

因为没有在配置文件中配置自己的从机，所以从机宕机重启后，不再是从机，而变回了主机





## 另一种主从复制模式

把81设置为80的从节点

~~~bash
127.0.0.1:6381> SLAVEOF 127.0.0.1 6380
OK
127.0.0.1:6381> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6380
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:0
。。。
~~~

此时的80依旧是79的从节点，依旧不能写入

~~~bash
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:up
~~~

此时79的数据，80和81依旧可以拿到



如果主机宕机，80可以在客户端上指定自己成为主机

~~~bash
127.0.0.1:6380> SLAVEOF no one
OK
127.0.0.1:6380> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=2936,lag=0
master_replid:140b98305121c10cd9ff08b73b0bbc615cd69499
。。。
~~~

之前的主机79重启了，但是79没有从机了



# 哨兵模式（Sentinel）

## 简介

哨兵监控主从节点的健康，如果故障，根据投票数自动将从节点转换为主节点

单个哨兵进程也可能会出问题，可以采用多个哨兵进行监控，各个哨兵之间还会进行监控，形成了多哨兵模式。**哨兵集群就像是Zookeeper集群。**

### 过程

![image-20200410103921840](Redis.assets/image-20200410103921840.png)

假设主机宕机，哨兵1最先检测到主机失效、不在线，这个现象为**主观下线**。接着其他的哨兵也监测主机下线，当监测到下线的哨兵数量到达一定值时，哨兵之间会进行一次投票，选出一个从机作为主机，投票的结果由一个哨兵发起，进行故障转移操作。切换成功后，就会通过发布订阅模式，让各个哨兵，把这个从机服务器切换为主机，这个过程叫**客观下线**。

## 消息丢失

通过配置参数尽量减少数据丢失。

~~~
min-slaves-to-write 1
min-alaves-max-lag 10
~~~

前面表示主节点必须有一个从节点在正常复制，否则停止对外的写服务。

第二行表示10秒内有反馈就表示正常。





## 测试

目前服务器状态是一主二从

创建一个sentinel.config配置文件

~~~bash
#指定监测的主机
#后面的数字，表示多少个哨兵监测主机失效后开始投票，这里1表示一个哨兵监测主机失效就任务主机真的失效然后进行投票
sentinel monitor myredis 127.0.0.1 6379 1
~~~

启动哨兵

~~~bash
redis-sentinel qdwconfig/sentinel.conf
~~~

![image-20200410105826918](Redis.assets/image-20200410105826918.png)

![image-20200410105906189](Redis.assets/image-20200410105906189.png)

把主机停止，过一会后，哨兵自动进行处理

![image-20200410110224443](Redis.assets/image-20200410110224443.png)

![image-20200410110524848](Redis.assets/image-20200410110524848.png)

81变成了主机

~~~bash
127.0.0.1:6381> info replication
# Replication
role:master
connected_slaves:1
slave0:ip=127.0.0.1,port=6380,state=online,offset=70180,lag=1
master_replid:c4c5f01a6e75c1964c46bc05e802c6ce3b78ee7f
~~~

主机上线后，作为新主机的从机

~~~bash
127.0.0.1:6379> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6381
master_link_status:up

~~~

## 优点

- 哨兵基于主从复制，有主从复制模式的优点
- 主从可以自动切换，可用性好

## 缺点

- 很难在线扩容，集群数量一旦达到上限，扩容很麻烦
- 哨兵模式的配置很麻烦

## 全部配置

~~~bash
修改sentinel配置文件
vim /usr/local/redis/6379/26379.conf

修改内容：
# 添加守护进程模式
daemonize yes

# 添加指明日志文件名
logfile "/usr/local/redis/6379/sentinel26379.log"

# 修改工作目录
dir "/usr/local/redis/6379"

# 修改启动端口
port 26379

# 添加关闭保护模式
protected-mode no

# 修改sentinel monitor
sentinel monitor macrog-master 192.168.24.131 6379 2

# 将配置文件中mymaster全部替换macrog-master
# 在末行模式下 输入 :%s/mymaster/macrog-master/g

依次修改26380,26381配置

说明：
macrog-master:监控主数据的名称,自定义即可,可以使用大小写字母和“.-_”符号
192.168.24.131:监控的主数据库的IP
6379:监控的主数据库的端口
2:最低通过票数


sentinel auth-pass <master-name> <password>
设置连接master和slave时的密码，注意的是sentinel不能分别为master和slave设置不同的密码，因此master和slave的密码应该设置相同。

sentinel down-after-milliseconds <master-name> <milliseconds> 
这个配置项指定了需要多少失效时间，一个master才会被这个sentinel主观地认为是不可用的。 单位是毫秒，默认为30秒

sentinel parallel-syncs <master-name> <numslaves> 
这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行 同步，这个数字越小，完成failover所需的时间就越长，但是如果这个数字越大，就意味着越 多的slave因为replication而不可用。可以通过将这个值设为 1 来保证每次只有一个slave 处于不能处理命令请求的状态。

sentinel failover-timeout <master-name> <milliseconds>
failover-timeout 可以用在以下这些方面：     
1. 同一个sentinel对同一个master两次failover之间的间隔时间。   
2. 当一个slave从一个错误的master那里同步数据开始计算时间。直到slave被纠正为向正确的master那里同步数据时。    
3.当想要取消一个正在进行的failover所需要的时间。    
4.当进行failover时，配置所有slaves指向新的master所需的最大时间。不过，即使过了这个超时，slaves依然会被正确配置为指向master，但是就不按parallel-syncs所配置的规则来了。
~~~



# 缓存出现问题

## 缓存穿透

指用户不断**请求缓存和数据库都没有的数据。**

用户查询数据，在redis缓存中没有找到，就去持久层数据库去查找。如果请求很多都没用命中缓存，持久层数据库就会承担巨大压力。在数据库中没有查到，所以缓存中还是没有，如果这样的请求很多，持久层数据库可能无法承担这样的压力。

> 解决方法

### 缓存空标记

![image-20200410140523072](Redis.assets/image-20200410140523072.png)

在持久层数据库中查到的空值也放到缓存（比如"NULL"）中，这样就可以在缓存中知道数据库也没有这个值，不过过期时间设置的**短一些**

可能存在问题

- 存很多空值的键，占用很多空间
- 缓存和持久数据，存在无法保证一致性的问题



### 布隆过滤器

![image-20200410140327521](Redis.assets/image-20200410140327521.png)

布隆过滤器是一种数据结构。将所有可能的数据映射到BitMap中。用户的请求先通过布隆过滤器检验，一定不存在的数据就被直接拦截掉。





## 缓存击穿

指一个key很热门，大量的并发请求都查询这个key，当这个key失效的瞬间，大量的并发请求访问数据库。

击穿单指对一个key

> 解决方法

### 设置热点数据永不过期

不设置过期时间

### 加互斥锁

在缓存失效时，使用分布式锁，保证对于每一个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的的权限， 只能等待。将高并发的压力转移到了分布式锁，对分布式锁的考验很大



## 缓存雪崩

指，某一时间段，缓存集中过期失效，redis宕机

在某些情况下，集中的放入缓存一些数据，到了这些数据的过期时间，集体过期，造成数据库的压力突然变大。这种是一种缓存雪崩

最严重的就是缓存直接宕机。

> 解决方法

### 设置随机过期时间

对于集中过期，可以为数据设置不同的过期时间，使数据的过期时刻不会太集中

### 不设置过期时间

热点数据永不过期，有更新操作就更新缓存

### redis高可用

多用几台redis服务器，搭建集群

### 限流降级

缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如某个key只允许一个线程查询数据和写缓存，其他线程等待

### 数据预热

在预测高并发场景要到来时之前，让热点数据先加载到缓存中，设置不同的过期时间，让数据失效的时间点尽量均匀







# 限流

4.0提供了一个限流模块，Redis-Cell。该模块使用了漏斗算法。提供了原子的限流指令。

~~~
cl.throttle key:reply 20 30 60 1
~~~

- 20为漏斗容量：先获取20个配额后，才受到滴水速率影响
- 30和60定义流失速率：表示60秒内可以流30滴水
- 1为当前请求的配额。

# 缓存一致性

常见的四个方案

> 方案一

通过Redis的过期时间来更新缓存，MySQL数据库更新不会触发Redis更新。只有Redis的key过期后才会重新加载。

缺点：

- 数据不一致的时间比较长，会产生一定的脏数据
- 完全依赖过期时间，太短频繁失效，太长有延迟



> 方案二

在方案一的基础上，增加了更新MySQL同时更新Redis

缺点

- 更新MySQL成功，更新Redis失败，就成了方案一

> 方案三

在方案三的基础上，将更新Redis的操作交给MQ由消息队列保证可靠性，异步更新Redis

缺点

- 解决不了时序问题，如果多个业务实例对同一数据进行更新，数据的先后顺序可能会乱
- 增加了维护MQ的成本

> 方案四

将MySQL更新和Redis更新放到一个事务中

缺点

- MySQL或Redis任何一个环节出问题，都会造成数据回滚或撤销
- 如果网络出现超时，不仅可能会造成数据回滚或撤销，还会引起并发问题。



> 方案五

通过订阅Binlog来更新Redis，把我们搭建的消费服务，作为MySQL的一个slave，订阅Binlog，解析出更新内容，再更新到Redis

缺点

- 需要单独搭建一个同步服务，并引入Binlog同步机制，成本较大

![image-20200726150009330](Redis.assets/image-20200726150009330.png)





# 缓存

特点：读写快，断电丢失

对于很少发生修改的数据，适合放到缓存中。



## 本地和分布式缓存

本地缓存：存在应用服务器内存中的缓存数据

分布式缓存：存储在应用服务器内存之外的缓存数据



## 自定义mybatis的cache接口



# 实现消息队列（待更新！！！）

- 生产者
  - 用lpush
- 消费者
  - 用rpop

需要不停地rpop来确认有没有消息。可以设置sleep，但是时间不好掌控。

可以用brpop或blpop。阻塞的pop还可以接收多个键，是按照key的顺序，可以实现具有优先级的队列。





在消息队列中，并没有JMS的ack机制，如果消费者把job给Pop走了又没处理完就死机了怎么办？

解决方法之一是加多一个sorted set，分发的时候同时发到list与sorted set，以分发时间为score，用户把job做完了之后要用ZREM消掉sorted set里的job，并且定时从sorted set中取出超时没有完成的任务，重新放回list。

另一个做法是为每个worker多加一个的list，弹出任务时改用RPopLPush，将job同时放到worker自己的list中，完成时用LREM消掉。

如果集群管理(如zookeeper)发现worker已经挂掉，就将worker的list内容重新放回主list。

# 实现延时队列

自己实现要考虑什么

- 消息存储
- 过期延时消息实时获取
- 高可用性

实现思路

- 将整个Redis当做消息池，key为id，value为具体的消息body
- 用ZSET做优先队列，按照score维持优先级（用当前时间+需要延时的时间作为score）
- 轮询ZSET，拿出score比当前时间戳大的数据（过期的）
- 根据id拿到消息池中具体的消息进行消费
- 消费成功删除队列和消息
- 消费失败让该消息重新回到队列

![image-20200726153151216](Redis.assets/image-20200726153151216.png)





# 分布式锁（待更新）

## Redis命令实现

用Redis实现分布式锁。

用一个String类型，表示锁互斥量。同步操作需要先去成功获取锁，才能进行。

规定String的key的名称，和每次设置时的过期时间。

### 加锁

~~~
setnx key value 超时设置
~~~

通过setnx设置一个键值对，key为表示锁的名称，value为线程自己存的uuid，为了避免死锁，给定一个过期时间。保证设置值和过期时间在一个原子操作中。

### 解锁

解锁需要完整两步

- 判断锁的值是不是自己
- 如果是自己获取的锁，就去删除锁

这两部需要合在一起，保证原子性。

可以写在lua脚本中

~~~lua
-- lua删除锁：
-- KEYS和ARGV分别是以集合方式传入的参数，对应上文的Test和uuid。
-- 如果对应的value等于传入的uuid。
if redis.call('get', KEYS[1]) == ARGV[1] 
    then 
    -- 执行删除操作
        return redis.call('del', KEYS[1]) 
    else 
    -- 不成功，返回0
        return 0 
end
~~~

缺点：锁不是可重入锁。

## RedLock

RedLock是Redis官方提出的一种分布式锁的算法。RedLock算法需要多个Redis实例，并且每个实例是单独部署的，没有主从关系。为了避免异步同步造成锁丢失。

对于N个Redis实例，加锁流程为：

- 顺序向对个节点请求加锁
- 根据超时时间判断是否跳过节点
- 

# I/O模型

## Blocking I/O

![image-20200823002705078](Redis.assets/image-20200823002705078.png)

## I/O多路复用

![image-20200823002730017](Redis.assets/image-20200823002730017.png)

## Redis的Reactor设计模式

![image-20200823002909046](Redis.assets/image-20200823002909046.png)

- 多个socket
- 多路复用模块
  - 实现有：select、epoll、evport、kqueue
- 事件分发器
- 事件处理器





# 数据类型实现原理

## 对象的类型和编码

每创建一个键值对，至少会创建两个对象，一个是键对象，一个是值对象。

每个对象都是由redisObject结构表示：

~~~c
typedef struct redisObject{
     //类型
     unsigned type:4;
     //编码
     unsigned encoding:4;
     //指向底层数据结构的指针
     void *ptr;
     //引用计数
     int refcount;
     //记录最后一次被程序访问的时间
     unsigned lru:22;
 
}robj
~~~

- type属性

对象的type属性记录了对象的类型，这里的类型就是五大数据类型

![image-20200515233401047](Redis.assets/image-20200515233401047.png)



> 键总是一个String对象，值可以使字符串、列表等对象。和通常说的五大类型的键值不一样。

- encoding属性和*ptr指针

ptr指针指向对象底层的数据结构，而数据结构由encoding属性来决定。

![image-20200515234138222](Redis.assets/image-20200515234138222.png)

每种类型的对象使用的编码：（都至少使用了两种不同的编码）

![image-20200515234322745](Redis.assets/image-20200515234322745.png)



每个Redis对象都有一个类型（5中类型）和一个编码方式。

## 字符串对象

所有的key都是字符串类型的，字符串的值也是字符串类型的，其他几种数据结构构成的元素也是字符串。字符串的长度不能超过512M

### 编码

字符串对象的编码可以是int、raw或者embstr。

1. int：保存的是可以用long类型表示的整数值。
2. raw：保存长度大于44字节的字符串。
   1. ![image-20200517211856023](Redis.assets/image-20200517211856023.png)
3. embstr：保存长度小于44字节的字符串。
   1. ![image-20200517211909640](Redis.assets/image-20200517211909640.png)

Redis中对于浮点数类型作为字符串保存，在需要的时候转化成浮点数。

### 编码转换

- 字符串内容可转为long，就采用int编码
- 字符串内容不再是long，且长度<39（3.2之后为44）用embstr
- 其他时候用raw

int编码保存的值不再是整数，或者大小超过了long的范围，会自动转化为raw。

Redis没有对embstr编写任何修改程序（embstr是只读的），在对embstr对象进行修改时，都会先转化为raw在进行修改，修改之后会变成raw。

### 动态字符串

Redis中的字符串不是C语言中的字符串（'\0'结尾的字符数组），它是自己构建了一种名为简单动态字符串（simple dynamic string，SDS）的抽象类型，最为Redis的默认字符串表示

~~~c
struct sdshdr{
     //记录buf数组中已使用字节的数量
     //等于 SDS 保存字符串的长度 4byte
     int len;
     //记录 buf 数组中未使用字节的数量 4byte
     int free;
     //字节数组，用于保存字符串 字节\0结尾的字符串占用了1byte
     char buf[];
}
~~~

保存有：

- 字符串长度
- 字符串每个元素
- 数组中未使用的字节数量

#### 相比于C字符串的优势

- 获取长度：C语言字符串需要遍历计数
- 缓冲区溢出：C语言字符串中使用strcat函数进行字符串拼接，一旦空间不足可能会造成字符串以外覆盖。



## 列表对象

list列表，它是简单的字符串列表，按照插入循序排序，可以在头部（左边）或者尾部（右边）添加元素，它的底层实际上是个链表结构。

- 编码

列表对象的编码可以使**ziplist**（压缩列表）和**linkedlist**（双端链表）。

1. ziplist：Redis为了节省内存，将数据安装一定的规则编码在一块连续的内存区域。有一系列特殊编码的连续内存块组成的循序型数据结构。一个压缩列表可以包含任意多个节点（entry），每个节点可以保存一个字节数组或者一个整型值。
   1. ![image-20200517211925098](Redis.assets/image-20200517211925098.png)
2. linkedlist：双端链表，**节点为StringObject**
   1. ![image-20200517211935371](Redis.assets/image-20200517211935371.png)



- 编码转换

当满足下面两个条件时，使用ziplist编码：

1. 列表保存元素个数小于512
2. 每个元素长度小于64字节

不满足这两个条件时使用linkedlist编码

上面两个条件可以在redis.conf 配置文件中的 list-max-ziplist-value选项和 list-max-ziplist-entries 选项进行配置。



## 哈希对象

哈希对象的键是一个字符串类型，值是一个键值对集合

- 编码

1. **ziplist**：当使用压缩列表时，新增的键值对是保存到压缩列表的尾部。
   1. ![image-20200516002240487](Redis.assets/image-20200516002240487.png)
2. **hashtable**：hashtable编码的哈希表对象底层使用**字典数据结构**，哈希对象中的每个键值对都是用一个字典键值对。
   1. ![image-20200516002310110](Redis.assets/image-20200516002310110.png)



- 编码转换

和列表对象的一样

满足下面两个条件就会用压缩列表

1. 列表保存元素个数小于512个
2. 每个元素长度小于64字节

不能满足这两个条件的时候使用 hashtable 编码。第一个条件可以通过配置文件中的 set-max-intset-entries 进行修改。



## 集合对象

是string类型（整数也会转换成string）的无序集合。

- 编码

1. intset：使用整数集合作为底层实现，集合对象包含的所有元素都被保存在整数集合中。
   1. ![image-20200517205708212](Redis.assets/image-20200517205708212.png)
2. hashtable：使用字典作为底层实现。字典的每个键都是一个字符串对象，这里的每个字符串对象就是一个集合中的元素，字典的值设置为null，类似java中的HashSet。
   1. ![image-20200517205722187](Redis.assets/image-20200517205722187.png)

- 编码转换

当集合同时满足以下两个条件时，使用intset编码

1. 集合对象中所有元素都是整数
2. 集合对象所有元素数量不超过512

不满足就是用hashtable。第二个条件可以通过配置文件的 set-max-intset-entries 进行配置。

## 有序集合对象

对象是有序的。与列表使用索引下标作为排序依据不同，有序集合通过每个元素的分数作为排序依据。

- 编码

1. ziplist：使用压缩列表作为底层。每个集合元素用两个挨在一起的压缩列表节点来保存，第一个存元素成员，第二个阶段保存分值。压缩列表内的元素按照分值从小到大排序，小的靠近表头，大的靠近表尾。
   1. ![image-20200517210832301](Redis.assets/image-20200517210832301.png)
2. skiplist：用shiplist编码的有序结合使用**zset结构体**作为底层实现。zset结构体中**包含一个字典和一个跳跃表**。字典的**键保存元素值**，**值保存元素的分值**；跳跃表按照score从小到大保存所有集合元素。两种数据结构通过指针来共享相同元素的成员和分值，所以不会产生重复数据。单独使用跳表的查找复杂度是O(log(N))，插入复杂度是O(log(N))，结合两种结构既保证查找的复杂度是O(1)，又保证是有序的。
   1. 跳跃表结构：几个链表，其中有些节点与后面多个节点相连。level0：是存储原始数据的，是一个有序链表。level0+：通过指针串联起节点，是原始数据的子集，level越大，节点的指针越多，对应高层相连的数据越少，这样可以显著提高查找效率（加快遍历速度）。
   2. ![在这里插入图片描述](Redis.assets/2019040921393634.png)

- 编码转换

当满足下面两个条件，使用ziplist编码

1. 保存的元素小于128
2. 保存的所有元素长度都小于64字节

不满足就用skiplist编码。以上两个条件也可以通过Redis配置文件zset-max-ziplist-entries 选项和 zset-max-ziplist-value 进行修改。



## 跳跃列表

https://www.cnblogs.com/hunternet/p/11248192.html

![image-20200518100157195](Redis.assets/image-20200518100157195.png)

# 应用场景

- string：可以用来存放图片，视频等内容。value可以是数字，可以用来做计数器，比如在线人数，访问量。
  - 存储手机验证码
  - 存储xxx具有时效性的业务功能
    - 订单：具有时效性，如果超时了就自动删除了
  - 集群中的Session共享
- hash：value可以存放键值对，可以做单点登录存放用户信息。
- list：可以实现简单的消息队列，另外可以利用lrange命令。做基于redis的分页功能。
- set：底层是字典实现，查找元素快，不允许重复，可以进行全局去重。比如注册模块判断用户名是否注册。另外可以利用交集并集差集等操作，计算共同喜好、共同好友之类的。
- zset：有序，可以做范围查找，排行版，topn之类的操作。
  - 排行榜
- bitmaps：存用户集、用户打卡

分布式缓存

存储token

分布式锁

# 跳跃表

zset的数据结构的效果类似于，Java中的SortedSet和HashMap的结合体。一方面提供唯一性，另一方面给每个value赋予一个排序的权重值来排序。

它的内部实现依赖于**跳跃列表**的数据结构

## 跳跃列表

zset需要**支持随机插入和删除**，不适合用数组。还需要有排序功能，那为什么不能用平衡树。

1. 性能考虑：高并发下，树形结构的重新平衡的操作可能涉及整个树，而**跳跃表只涉及局部**
2. 实现考虑：跳跃表实现相对简单。

> 同层的节点指针相连，一个节点可能身兼数职具，高层节点同时也是底层，在底层同层中相连。

## 选择的过程

对于一个普通链表，每个节点包含数据和分值。如果插入或者删除，只能去遍历链表找到对应的位置。

为了提高遍历效率，可以对相隔一个节点的两个节点**添加一个指针，这样来增加遍历的速度。**同样的可以对相隔两个节点的两个节点用指针相连。如果要找的节点过了，就用低层的去尝试。让链表中元素的查找类似于数组的二分法。

跳跃表（skiplist）就是利用这种多层链表的结构设计的。但是要解决一个问题，在新插入一个节点时，会打乱上下相邻两层链表上节点个数严格的2：1的对应关系。这样假如**他们之间节点多**了，对于这些节点的查找就无法通过高增的指针来加速遍历了。

解决办法：为插入的每个节点**随机层数**。这样他的插入不会影响其他节点的层数，插入只需要修改该节点前后的指针，就能保证一个相对平衡的分布。

## 实现

插入节点时随机一个层数，层数从1层开始越大随机的概率越小。每一层的节点都会相连。比如3层的节点会在一层、二层、三层的链表上，对于高层的链表不影响

随机层数算法：50%分配到1层，25分配到二层，12.5%分配到三层，每一层的晋升率都是50%。默认允许的最大层数时32层。

# 项目案例

## 浏览量

用

添加文章的最近七天热门文章

设计Redis

- 用zset存储文章id和标题，分数存储文章阅读量。

项目启动时候初始化，实现springboot默认的监听接口，该方法在spring容器加载完自动监听。

从数据库中把最近七天的文章查出来，把他们的id，标题和阅读量放到Redis里。

## 存用户集、记录打卡

用bigmaps

~~~shell
setbit key 0 1
bitcount key/bitcount key 0 -1
~~~

