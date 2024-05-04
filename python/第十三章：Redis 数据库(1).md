## 第十三章：Redis 数据库

### 1. 学习目标

- 能够描述出什么是 nosql
- 能够说出 Redis 的特点



##### 1.1. nosql介绍

NoSQL：一类新出现的数据库(not only sql)

- 泛指非关系型的数据库
- 不支持SQL语法
- 存储结构跟传统关系型数据库中的那种关系表完全不同，nosql中存储的数据都是KV形式
- NoSQL的世界中没有一种通用的语言，每种nosql数据库都有自己的api和语法，以及擅长的业务场景
- NoSQL中的产品种类相当多：

- - Redis
  - Mongodb
  - Hbase hadoop
  - Cassandra hadoop



##### 1.2. NoSQL和SQL数据库的比较

- 适用场景不同：sql数据库适合用于关系特别复杂的数据查询场景，nosql反之
- **事务** 特性的支持：sql对事务的支持非常完善，而nosql基本不支持事务
- 两者在不断地取长补短，呈现融合趋势



##### 1.3. Redis简介

- Redis是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。
- Redis是 NoSQL技术阵营中的一员，它通过多种键值数据类型来适应不同场景下的存储需求，借助一些高层级的接口使用其可以胜任，如缓存、队列系统的不同角色



##### 1.4. Redis特性

- Redis 与其他 key - value 缓存产品有以下三个特点：
- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。



##### 1.5. Redis优势

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。



##### 1.6. Redis应用场景

- 用来做缓存(ehcache/memcached)——redis的所有数据是放在内存中的（内存数据库）
- 可以在某些特定应用场景下替代传统数据库——比如社交类的应用
- 在一些大型系统中，巧妙地实现一些特定的功能：session共享、购物车
- 只要你有丰富的想象力，redis可以用在可以给你无限的惊喜…….



##### 1.7. 推荐阅读

- [redis官方网站](https://redis.io/)
- [redis中文官网](http://redis.cn/)



### 2. 客户端和服务端命令

##### 2.1. 服务器端

- 服务器端的命令为redis-server
- 可以使⽤help查看帮助⽂档redis-server --help
- 服务器操作
  - ps aux | grep redis 查看redis服务器进程
  - sudo kill -9 pid 杀死redis服务器
  - sudo redis-server /etc/redis/redis.conf 指定加载的配置文件




##### 2.2. 客户端

- 客户端的命令为redis-cli

- 可以使⽤help查看帮助⽂档

  - redis-cli --help

    

- 连接redis

  - redis-cli

  

- 运⾏测试命令

  - ping

    

- 切换数据库
- 数据库没有名称，默认有16个，通过0-15来标识，连接redis默认选择第一个数据库

- - select 10



### 3. 数据操作

##### 3.1. 学习Redis之前的知识准备

- 文档连接

  - [Redis 参考命令](http://doc.redisfans.com/)

  - [Redis 官方文档](https://redis-py.readthedocs.io/en/latest/#indices-and-tables)



- 数据结构
  - redis是key-value的数据结构，每条数据都是⼀个键值对
  - 键的类型是字符串
  - 注意：键不能重复



- Reids中的数据类型：
  - 字符串string
  - 哈希hash
  - 列表list
  - 集合set
  - 有序集合zset




##### 3.2. string 类型

字符串类型是 Redis 中最为基础的数据存储类型，它在 Redis 中是二进制安全的，这便意味着该类型可以接受任何格式的数据，如JPEG图像数据或Json对象描述信息等。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。



###### 3.2.1. 保存

如果设置的键不存在则为添加，如果设置的键已经存在则修改

- 设置键值

- - set key value

- 例：设置键为name值为tuling的数据

- - set name tuling

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047174381-6be0a516-8b23-41fd-9c9b-532f1e7f7df1.png)

- 设置键值及过期时间，以秒为单位

- - setex key seconds value

- 例：设置键为aa值为aa过期时间为3秒的数据

- - setex aa 3 aa

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047284651-0d13e9d6-2aed-45cd-8f7b-be0ca53ea8ae.png)

- 设置多个键值

- - mset key1 value1 key2 value2 ...

- 例：设置键为a1值为python、键为a2值为java、键为a3值为c

- - mset a1 python a2 java a3 c

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047420893-b7974375-ee76-405a-9546-200e65d4cba7.png)



###### 3.2.2. 获取

- 获取：根据键获取值，如果不存在此键则返回nil

- - get key

- 例：获取键name的值

- - get name

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047532282-e1f35a29-fd73-4c2f-9140-1342639b238c.png)

- 根据多个键获取多个值

- - mget key1 key2 ...

- 例：获取键a1、a2、a3的值

- - mget a1 a2 a3

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047505634-2c077531-a434-432f-be4a-018424f48075.png)





###### 3.2.3. 删除

详⻅下节键的操作，删除键时会将值删除



##### 3.3. 键命令

- 查找键，参数⽀持正则表达式

- - keys pattern

- 例：查看所有键

- - keys *

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047633488-4a89c4f7-201b-4287-8801-0136e9c6aaef.png)



- 例：查看名称中包含a的键

- - keys a*

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047706199-e7e3dbaf-f5ef-4f70-9d96-44515f19c73b.png)

- 判断键是否存在，如果存在返回1，不存在返回0

- - exists key1

- 例：判断键a1是否存在

- - exists a1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047791765-c425652d-8b88-4741-ad02-af3e9657b465.png)

- 查看键对应的value的类型

- - type key

- 例：查看键a1的值类型，为redis⽀持的五种类型中的⼀种

- - type a1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047851221-9537b889-95ad-4a50-91ee-a659a7464d9b.png)

- 删除键及对应的值

- - del key1 key2 ...

- 例：删除键a2、a3

- - del a2 a3

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651047988153-9de73605-9af5-4b08-aeac-393edc2131d7.png)



- 设置过期时间，以秒为单位
- 如果没有指定过期时间则⼀直存在，直到使⽤DEL移除

- - expire key seconds

- 例：设置键a1的过期时间为3秒

- - expire a1 3

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048091741-8df94059-dd23-4625-b78e-f4978af0c3a0.png)

- 查看有效时间，以秒为单位

- - ttl key

- 例：查看键bb的有效时间

- - ttl bb

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048156826-537ef0b6-3401-4af2-bb97-3934ea0f8e5d.png)



##### 3.4. hash 类型

- **hash**⽤于存储对象，对象的结构为属性、值
- **值**的类型为**string**

###### 3.4.1. 增加、修改

- 设置单个属性

- - hset key field value

- 例：设置键 user的属性name为tuling

- - hset user name tuling

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048352560-a9472606-d8e9-496d-9dba-c96f3bb92b91.png)

- 设置多个属性

- - hmset key field1 value1 field2 value2 ...

- 例：设置键u2的属性name为python、属性age为11

- - hmset u2 name python age 11

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048456591-e8fc186b-adcc-4f89-8be1-a516042203a8.png)



###### 3.4.2. 获取

- 获取指定键所有的属性

- - hkeys key

- 例：获取键u2的所有属性

- - hkeys u2

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048506238-589373dc-a834-4687-a7ad-72f1ca455048.png)

- 获取⼀个属性的值

- - hget key field

- 例：获取键u2属性name的值

- - hget u2 name

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048564463-e3da83ab-92c0-46ef-ac91-de0ea585bf7d.png)

- 获取多个属性的值

- - hmget key field1 field2 ...

- 例：获取键u2属性name、age的值

- - hmget u2 name age

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048648963-ea1923b7-93bb-4e93-a3ff-71ab1667ef58.png)

- 获取所有属性的值

- - hvals key

- 例：获取键u2所有属性的值

- - hvals u2

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048705036-95271be9-0092-47df-9408-f8b3c2a7a354.png)



###### 3.4.3. 删除

- **删除整个hash键及值，使⽤del命令**
- 删除属性，属性对应的值会被⼀起删除

- - hdel key field1 field2 ...

- 例：删除键u2的属性age

- - hdel u2 age

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651048797213-e9df56aa-001c-4729-8826-8915aebcea3d.png)



##### 3.5. list 类型

- 列表的元素类型为string

- 按照插⼊顺序排序

  

###### 3.5.1. 增加

- 在左侧插⼊数据

- - lpush key value1 value2 ...

- 例：从键为a1的列表左侧加⼊数据a 、 b 、c

- - lpush a1 a b c

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049011105-c2f01542-b5cf-4bc6-a6ec-7d2630b412a4.png)

- 在右侧插⼊数据

- - rpush key value1 value2 ...

- 例：从键为a1的列表右侧加⼊数据0、1

- - rpush a1 0 1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049131724-ad8a8b9a-f382-42db-92a3-f85a65e4aac1.png)



###### 3.5.2. 获取

- 返回列表⾥指定范围内的元素

- - start、stop为元素的下标索引
  - 索引从左侧开始，第⼀个元素为0
  - 索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素

- 语法形式

- - lrange key start stop

- 例：获取键为a1的列表所有元素

- - lrange a1 0 -1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049287939-92606230-d056-43d7-86c6-4f5df4ab403a.png)



###### 3.5.3. 删除

- 删除指定元素

- - 将列表中前count次出现的值为value的元素移除
  - count >0: 从头往尾移除
  - count < 0: 从尾往头移除
  - count = 0: 移除所有

- 语法形式

- - lrem key count value

- 例：向列表a2中加⼊元素a、b、a、b、a、b

- - lpush a2 a b a b a b

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049404815-61bb3721-a513-4ef0-85bc-21901427705d.png)

- 例：从a2列表右侧开始删除2个b

- - lrem a2 -2 b

- 例：查看列表a2的所有元素

- - lrange a2 0 -1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049556464-a865c13a-3f82-4bcf-8cc1-9693545da393.png)



##### 3.6. set 类型

- ⽆序集合
- 元素为string类型
- 元素具有唯⼀性，不重复
- 说明：对于集合没有修改操作



###### 3.6.1. 增加

- 添加元素

- - sadd key member1 member2 ...

- 例：向键a3的集合中添加元素zhangsan、lisi、wangwu

- - sadd a3 zhangsan sili wangwu

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651049705795-0b1ec8ab-5149-45cb-9507-be7a6964f692.png)



###### 3.6.2. 获取

- 返回所有的元素

- - smembers key

- 例：获取键a3的集合中所有元素

- - smembers a3

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651050115610-4423c194-dc79-4f77-8ea3-ede6cfe9de8c.png)



###### 3.6.3. 删除

- 删除指定元素

- - srem key

- 例：删除键a3的集合中元素wangwu

- - srem a3 wangwu

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651050179396-f848b135-69d3-407d-a8bb-7444f9d8116b.png)



##### 3.7. zset 类型

- sorted set，有序集合
- 元素为string类型
- 元素具有唯⼀性，不重复
- 每个元素都会关联⼀个double类型的score，表示权重，通过权重将元素从⼩到⼤排序
- 说明：没有修改操作



###### 3.7.1. 增加

- 添加

- - zadd key score1 member1 score2 member2 ...

- 例：向键a4的集合中添加元素lisi、wangwu、zhaoliu、zhangsan，权重分别为4、5、6、3

- - zadd a4 4 lisi 5 wangwu 6 zhaoliu 3 zhangsan

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651050406292-c08ac5c2-af47-4e46-bc7c-94e626dc562e.png)



###### 3.7.2. 获取

- 返回指定范围内的元素
- start、stop为元素的下标索引
- 索引从左侧开始，第⼀个元素为0
- 索引可以是负数，表示从尾部开始计数，如-1表示最后⼀个元素

- - zrange key start stop

- 例：获取键a4的集合中所有元素

- - zrange a4 0 -1

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651050509100-aaccef12-98a2-4e00-b68d-27d17c69b430.png)



###### 3.7.3. 删除

- 删除指定元素

- - zrem key member1 member2 ...

- 例：删除集合a4中元素zhangsan

- - zrem a4 zhangsan

![img](https://cdn.nlark.com/yuque/0/2022/png/22230102/1651050670068-8978e4f0-2151-4f3a-8014-ecbdbf94ad52.png)



### 4. Python 操作 Redis 的相关方法

##### 4.1. Redis对象⽅法

- 通过init创建对象，指定参数host、port与指定的服务器和端⼝连接，host默认为localhost，port默认为6379，db默认为0

```python
sr = Redis(host='localhost', port=6379, db=0)

# 简写
sr = Redis()
```

- 根据不同的类型，拥有不同的实例⽅法可以调⽤，与前⾯学的redis命令对应，⽅法需要的参数与命令的参数⼀致



##### 4.2. string

- set
- setex
- mset
- append
- get
- mget
- key



##### 4.3. keys

- exists
- type
- delete
- expire
- getrange
- ttl

##### 4.4. hash

- hset
- hmset
- hkeys
- hget
- hmget
- hvals
- hdel



##### 4.5. list

- lpush
- rpush
- linsert
- lrange
- lset
- lrem



##### 4.6. set

- sadd
- smembers
- srem



##### 4.7. zset

- zadd
- zrange
- zrangebyscore
- zscore
- zrem
- zremrangebyscore



### 5. Pyhton操作String类型示例

##### 5.1. 准备

- 在桌面上创建代码目录
- 使用pycharm打开代码目录
- 创建redis_string.py文件

```python
from redis import *
if __name__=="__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()

    except Exception as e:
        print(e)
```



##### 5.2. string - 增加

- ⽅法set，添加键、值，如果添加成功则返回True，如果添加失败则返回False
- 编写代码如下

```python
from redis import *


if __name__ == "__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()
        # 添加键name，值为itheima
        result = sr.set('name','tuling')
        # 输出响应结果，如果添加成功则返回True，否则返回False
        print(result)
    except Exception as e:
        print(e)
```



##### 5.3. string - 获取

- ⽅法get，添加键对应的值，如果键存在则返回对应的值，如果键不存在则返回None
- 编写代码如下

```python
from redis import *


if __name__=="__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()
        # 获取键name的值
        result = sr.get('name')
        # 输出键的值，如果键不存在则返回None
        print(result)
    except Exception as e:
        print(e)
```



##### 5.4. string - 修改

- ⽅法set，如果键已经存在则进⾏修改，如果键不存在则进⾏添加
- 编写代码如下

```python
from redis import *


if __name__=="__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()
        # 设置键name的值，如果键已经存在则进⾏修改，如果键不存在则进⾏添加
        result = sr.set('name','python')
        # 输出响应结果，如果操作成功则返回True，否则返回False
        print(result)
    except Exception as e:
        print(e)
```



##### 5.5. string - 删除

- ⽅法delete，删除键及对应的值，如果删除成功则返回受影响的键数，否则则返 回0
- 编写代码如下

```python
from redis import *


if __name__=="__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()
        # 设置键name的值，如果键已经存在则进⾏修改，如果键不存在则进⾏添加
        result = sr.delete('name')
        # 输出响应结果，如果删除成功则返回受影响的键数，否则则返回0
        print(result)
    except Exception as e:
        print(e)
```



##### 5.6. 获取键

- ⽅法keys，根据正则表达式获取键
- 编写代码如下

```python
from redis import *


if __name__=="__main__":
    try:
        # 创建Redis对象，与redis服务器建⽴连接
        sr = Redis()
        # 获取所有的键
        result = sr.keys()
        # 输出响应结果，所有的键构成⼀个列表，如果没有键则返回空列表
        print(result)
    except Exception as e:
        print(e)
```