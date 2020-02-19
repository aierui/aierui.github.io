---
title: Redis-Basics
art: a
id: 17062301
comments: false
tags:
  - Redis
  - 数据库
  - NoSQL
date: 2017-06-23 00:05:34
categories: 开发者手册
---


{% cq %} 
当你需要以接近实时的速度访问快㞘变动的数据流时，Redis 这个的键值数据库就是你的最佳选择。Redis 是一个可以用来解决问题的工具，它既其他数据库不具备的数据结构，又拥有内存存储（这让 Redis 速度非常快）、远程（这使得可以与多个客户端和服务器进行连接）、持久化（这使得服务器重启之后任然可以保持重启之前的数据） 和可扩展性（主从复制和分片）等多个特性。

{% endcq %}

<!-- more -->


### Redis 数据结构


数据结构| 结构存储的值 | 特点| 结构的读写能力
---|---|---
string | 字符串、整数、浮点数 | 字符串 | 对整个字符串或者字符串的一部分执行操作，对证书和浮点数执行自增(increment)或者自减(decrement)操作
list | 链表，链表上的每个节点都包含了一个字符串 | 列表元素可以重复出现 | 从链表的两端推入或者弹出元素；根据偏移量对链表进行修剪(trim)；读取单个或者多个元素；根据值查找或者移除元素
set | 包含字符串的无序收集器(unordered collection),每个元素唯一| 元素不可以重复出现| 添加、获取、移除单个元素；检查一个元素是否存在于集合中；计算交集、差集、并集；从集合中随机获取元素
hash | 包含键值对的无序散列列表 | 键值对 | 添加、或者、移除单个键值对、获取所有键值对
zset | 字符串成员(member)与浮点数分值(score)之间的有序映射，元素的排列顺序由分值大小决定 | 键值对 | 添加、获取、删除单个元素；根据分值范围(range)或者成员来获取元素


### Redis 和其他数据库对比

名称 | 类型 | 数据存储类型 | 查询类型 | 附件功能
---| ---| ---| ---| ---
Redis | 内存存储(in-memory) NOSQL | string、list、set、hash、zset | 每种数据结构都有自己的专属命令、还有批量操作和不完全的事物支持 | 发布(pub)与订阅(sub)、主从复制(master\slave replication)、持久化、脚本(存储过程、stored procedure)
memcached | 只用内存存储的键值缓存 | 键值之间的映射 | 创建、读取、更新、删除以及其他几个命令 | 为提升性能而设的多线程服务器
MongoDB | 硬盘存储(on-disk) 非关系文档存储 | 每个数据库可以包含多个表、每个表可以包含多个无 schema(schema-less) 的BSON文档 | 创建、更新、删除、读取、条件查询等命令 | 支持 map-reduce 操作、主从复制、分片、空间索引(spatial index)
MySQL | 关系数据库(relational database) | 多库、一库多表、一表多行、多表视图、支持空间扩展和第三方扩展 |  select、update、insert、replace、delete、内置函数、存储过程 | 支持ACID性质(InnoDB)、主从复制和主主复制(master/master replication)
PostgreSQL | 关系型数据库 | 多库、一库多表、一表多行、多表视图、支持空间扩展、第三方扩展和自定制类型 | select、update、insert、replace、delete、内置函数、自定义的存储过程 | 支持ACID性质、主从复制、第三方支持的多主复制(multi-msater replication)


### Redis 命令

> 详细见 [英文官方文档](https://redis.io/commands) [中文翻译文档](http://www.redis.cn/commands.html)

#### 字符串(string)

命令 | 语法 | 解释
--- | --- | ---
SET | set key-name 'string' | 设置存储在给定键中的值
GET | get key-name | 获取存储在给定键中的值
DEL | del key-name | 删除存储在给定键中的值
INCR | incr key-name 1 | 将键存储的值加上1
DECR | decr key-name 1 | 将键存储的值减上1
INCRBY | incrby key-name amount |  将键存储的值加整数 amount
DECRBY | decrby key-name amount |  将键存储的值减整数 amount
INCRBYFLOAT | incrbyfloat key-name amount | 将键存储的值加浮点数 amount Redis2.6以上可用
|下面是处理子串和二进制 |
APPEND | append key-name value| 将值 value 追加到给定键 key-name 当前存储的值的末尾
GETRANGE | getrange key-name start end | 获取一个由偏移量start至偏移量end范围内所有的字符组成的子串，包括start、end
SETRANGE | setrange key-name offset value | 将葱start偏移量开始的子串设置为给定值
GETBIT | getbit key-name offset| 将字节串看作是二进制位串(bit string)，并返回位串中偏移量为offset的二进制位的值
SETBIT | setbit key-name offset value|将字串看作是二进制位串，并将位串中偏移量为offset的二进制位的值设置为value
BITCOUNT | bitcount key-name [start end] |统计二进制位串里面值为1的二进制位的数量，如何给定了可选的start、end偏移量，那么支队该范围内进行统计
BITOP | bitop operation dest-key key-name | 对一个或者对个二进制位串执行包括并(and)、或(or)、异或(xor)、非(not)在内的仁义一种按位运算操作，并将计算得出结果保存在dest-key键里面


#### 列表(list)

命令 | 语法 | 解释
--- | --- | ---
RPUSH | rpush key-name value [value ...] | 将一个值或者多个值推入列表的右端
LPUSH| lpush key-name value [value ...] | 将一个值或者多个值推入列表的右端 
RPOP| rpop key-name | 移除并返回列表最右端的元素
LPOP| lpop key-name | 移除并返回列表最左端的元素
LINDEX| lindex key-name offset | 返回列表中偏移量为offset的元素
LRANGE| lange key-name start end |返回列表从start、end范围内所有的元素（含start、end）
LTRIM| ltrim key-name start end | 对列表进行修剪
|阻塞式的列表弹出命令以及在列表之间移动元素的命令|
BLPOP | blpop key-name [key-name ...] timeout | 从第一个非空列表中弹出位于最左端的元素或者在 timeout 秒之内阻塞并等待可弹出的元素出现
BRPOP | brpop key-name [key-name ...] timeout | 第一个非空列表中弹出位于最右端的元素或者在 timeout 秒之内阻塞并等待可弹出的元素出现
RPOPLPUSH | rpoplpush source-key dest-key | 从 source-key 列表元素中弹出位于最右端的元素，然后将这个元素推入 dest-key 列表的最左端，并向用户返回这个元素
BRPOPLPUSH | brpoplpush source-key dest-key timeout |  从 source-key 列表元素中弹出位于最右端的元素，然后将这个元素推入 dest-key 列表的最左端，并向用户返回这个元素；如果 source-key 为空，那么在 timeout 秒之内阻塞并等待可弹出的元素出现

#### 集合(set)

命令 | 语法 | 解释
--- | --- | ---
SADD | sadd key-name item [item ...] | 将一个或多个元素添加到集合里面，并返回被添加元素当中原本并不存在于集合里面的元素数量
SREM | srem key-name item [item ...] | 从集合里面移除一个或者多个元素，并返回被移除元素的数量
SISMEMBER | sismember key-name item | 检查元素 item 是否存在于集合 key-name 里
SCARD | scard key-name | 返回集合包含的元素数量
SMEMBERS | smemebers key-name | 返回集合包含的所有元素
SRANDMEMBER | srandmember key-name [count] | 从集合里面随机的返回一个或者多个元素，当 count > 0 命令返回的随机元素不会重复；当 count < 0 命令返回随机元素可能会重复
SPOP | spop key-name | 随机地移除集合中的一个元素，并返回被移除的元素
SMOVE | smove source-key dest-key item | 如果集合 source-key包含元素 item，那么从集合source-key里面移除的元素item，并将元素item添加到集合 dest-key ；如果 item 被成功移除，那么命令返回1，否则返回0
|用于组合和处理多个集合的 Redis 命令|
SDIFF | sdiff key-name [key-name ...] | 差集运算，返回存在第一个集合中，但不存在其他集合中的元素
SDIFFSTORE | sdiff dest-key key-name [key-name ...] | 将那些存在第一个集合但并不存在其他集合中的元素存储到 dest-key 键里面
SINTER | sinter key-name [key-name ...] | 交集运算，返回那些同时存在于所有集合中的元素
SINTERSTORE | sinterstore dest-key key-name [key-name ...] | 将那些同时存在于所有集合的元素存储到 dest-key 键里面
SUNION | sunion key-name [key-name ...] | 并集运算，返回那些至少存在一个集合中的元素
SUNIONSTORE | sunionstore dest-key key-name [key-name ...] |将那些至少存在于一个集合中的元素存储到 dest-key 键里面


#### 散列(hash)

命令 | 语法 | 解释
--- | --- | ---
HMGET | hmget key-name key [key ...] | 从散列获取一个或者多个键的值
HMSET | hmset key-name key value [key value ...] | 为散列里面的一个或者多个键设置值
HDEL | hdel key-name key [key ...] | 删除散列里面的一个或多个键值对，返回成功找到并删除的键值对数量
HLEN | hlen key-name | 返回散列包含的键值对数量
HEXISTS | hexists key-name key | 检查给定键是否存在于散列中
HKEYS | hkeys key-name | 获取散列包含的所有键
HVALS | hvals key-name | 获取散列包含的所有值
HGETALL | hgetall key-name | 获取散列包含的所有键值对
HINCRBY | hincrby key-name key increment | 将键 key 存储的值加上整数 increment
HINCRBYFLOAT | hincrbyfloat key-name key incrment | 将键 key 存储的值加上浮点数 increment



#### 有序集合(zset)

命令 | 语法 | 解释
--- | --- | ---
ZADD | zadd key-name score member [score member ...] | 将带有给定分值的成员添加到有序集合里面
ZREM | zrem key-name member [member ...] | 从有序集合里面移除给定的成员、并返回被移除成员数量
ZCARD | zcard key-name | 返回有序集合包含的成员数量
ZINCRBY | zincrby key-name increment member | 将member成员的分值加上increment
ZCOUNT |zcount key-name min max |返回分值介于min和max之间的成员数量
ZRANK | zrank key-name member |返回成员member在有序集合中的排名
ZSCORE | zscore key-name member |返回成员member的分值
ZRANGE | zrange key-name start stop [withscores] | 返回有序集合中排名介于 start、stop之间的成员，如果给定了可选的 withscores 选项，那么命令将成员的分值也一并返回。
|高级|
ZREVRANK | zrevrank key-name member | 返回有序集合里成员的排名，成员按照分值从大到小排列
ZREVRANGE | zrevrange key-name start stop [withscores] | 返回有序集合给定范围内里成员的排名，成员按照分值从大到小排列
ZRANGEBYSCORE | zrangebyscore key min max [withscores] [limit offset count] | 返回有序集合介于 min max之间所有成员
ZREVRANGEBYSCORE | zrevrangebyscore key max min [withscores] [limit offset count] | 返回有序集合介于 min max之间所有成员，并按照分值从大到小排列
ZREMRANGEBYRANK | zremrangebyrand key-name start stop | 移除有序集合中排名介于start、stop之间的所有成员
ZREMRANGEBYSCORE | zremrangebyscore key-name min max | 移除有序集合中分值介于min max之间所有成员
ZINTERSTORE | zinterstore dest-key key-count key [key ...] | 给定的有序集合执行类似于集合的交集运算
ZUNIONSTORE | zunionstore dest-key key-count key [key ...] | 给定的有序集合执行类似于集合的并集运算


#### 发布与订阅(pub/sub)

命令 | 语法 | 解释
--- | --- | ---
SUBSCRIBE | subscribe channel [channel ...] | 订阅给定的一个或多个频道
UNSUBSCRIBE | unsubscribe [channel [channel ...] ] | 退订给定的一个或多个频道，如果执行时没有给定任何频道，那么退订所有频道
PUBLISH | publish channel message | 向给定频道发送消息
PSUBSCRIBE | psubscribe pattern [pattern ...] | 订阅给定模式相匹配的所有频道
PUNSUBSCRIBE | punsubscribe [pattern [pattern ...] ] | 退订给定的模式，如果执行时没有给定任何模式，那么退订所有模式


#### 其他

命令 | 语法 | 解释
--- | --- | ---
SORT | sort soucre-key [BY pattern] [limit offset count] [get pattern [get pattern ...]] [asc/desc] [alpha] [store dest-key] | 根据给定的选项，对输入的列表、集合、有序集合进行排序，然后返回或者存储排序的结果


### Redis 数据安全和性能保障


### Redis 疑难杂症



### 参考

- [英文官方文档](https://redis.io)

- [中文翻译文档](http://www.redis.cn)
