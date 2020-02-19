---
title: 提升数据库性能
art: a
id: 17062408
comments: false
tags:
  - 数据库
  - 性能
date: 2017-06-24 08:35:30
categories: 数据库
---


数据库在动态网站扮演者关键角色，所有的流入流出的数据都会和数据库进行交互。

<!-- more -->

优化 PHP 应用数据库的各种方法，包含不限于

### Mysql 

#### 查询缓存（Query-Caching）
进入 MySql 输入命令

<code> show variables like 'have_query_cache'; </code>

![mysql_query_cache](https://static.shijinrong.cn/blog/mysql_query_cache.png)

查询缓存是 MySql 的一个重要性能特性，他缓存了 selete 查询及查询结果集，当同一个 selete 语句查询时， MySQL 直接从内存中直接取出结果。这样，可以提高执行速度，同时减轻数据库的压力。

> 如何开启（默认是开启的）呢？

```
	打开 my.conf 文件
	
	query_cache_type = 1 // 0 为禁用

	query_cache_size = 128MB // 将会分配内存大小

	query_cache_limit = 1MB //定义能被缓存查询结果的最大体积

```
#### MyISAM 和 InnoDB 存储引擎区别


 -| MyISAM | InnoDB
---|:---:|:---:
锁 | 表级锁 | 行级锁
外键| ❌ | ✅
事物| ❌ | ✅
全文检索| ✅ |  ❌ 
数据压缩、自我复制 | ✅ | ✅
查询缓存、数据加密 | ✅ | ✅
集群| ❌ | ✅


> InnoDB 的几个重要配置参数

- innodb_buffer_pool_size

	该配置定义了 InnoDB 数据和载入内存的索引可以使用多少内存空间。对于一个 “全职” 的 MySql 服务器，推荐此配置值为服务器安装内存的 50%～80%。 

- innodb_buffer_pool_instances

- innodb_log_file_size


InnoDB 是事物安全的 MySql 的引擎索引，设计上采用类似于 Oracle 数据架构设计。通常来说，InnoDB 存储引擎是OLTP应用中核心表的首选核心引擎。同时也正是 InnoDB 的存在，才使 MySql 数据库变得更加有魅力。

#### InnoDB 体系架构

![InnoDB 体系架构](https://static.shijinrong.cn/blog/E0252133-6E53-4E74-B80B-69CB0070C971.png)


从图中可看出，InnoDB 存储引擎有很多个内存块，可以认为许多内存块组成了一个大的内存池，由他们维护所有进程；线程需要访问的多个内部数据结构；缓存磁盘数据，方便快速地读取；同时在对磁盘文件的数据修改之前在这里缓存；重做日志（redo log）缓冲……后台线程主要负责刷新内存池中的数据，保证缓冲池中的内存缓存的是最近的数据。此外将已修改的数据文件刷新到磁盘文件，保证数据在发生异常时 InnoDB 能够恢复到正常运行状态。

#### InnoDB 关键特性

- 插入缓冲（Insert Buffer）

- 两次写（Double Write）

- 自适应哈希索引（Adaptive Hash Index）

- 异步IO（Async IO）

- 刷新临接页（Flush Neighbor Page）

#### 聚集索引（primary key）

InnoDB的主键（primary key）是用到的最多的聚集索引，表中数据按照主键顺序存放，**其值自动增长且不需要磁盘的随机读取**。因此，对于这类的**插入操作速度非常快**。

#### 联合索引（key）

对表的多个列进行索引、例如

```
(a, b) 
select * from table_name where a = ? and b = ?;   ✅
select * from table_name where a = ? ORDER BY b;(因为在联合索引中b已经经过排序)  ✅

(a, b, c)
select * from table_name where a = ? and b = ?;  ✅
select * from table_name where a = ? and b = ? ORDER BY c;  ✅
select * from table_name where a = ? and b in (?,?) and c = ?;  ✅


下面这句SQL联合索引不能够直接得到结果，还需要执行一次排序
select * from table_name where a = ? ORDER BY c;  ❌

//联合索引越靠近左侧越优先匹配，其中的列不可以被跳过（如上一句SQL）。

select * from table_name where a = ? and c = ? and b in (?,?)  ✅
select * from table_name where a = ? and b in (?,?) ORDER BY c;  ❌ (按照 c 做一个排序 尽管索引中含c，但b会存在多种情况，最终不会起作用，所以查询还会做一个排序)
```

#### 锁和事务

##### 锁
> InnoDB 存储引擎会在行级别上对表数据上锁，数据库中可以在多个地方使用，可以不同资源提供并发访问。虽然锁定机制可以实现事务的隔离要求，使得事务可以并发工作，但是会锁会带来三个问题，分别是：脏读、不可重复读、丢失更新；


> 锁的安全策略越高其内存开销就越大，性能就越低。所以说，在实际业务中我们需要在高性能和数据安全性之间寻求平衡。




**表锁和行级锁**

表锁是 MySQL 中最基本的锁策略，并且是开销最小的策略。一个用户在对表进行写操作（插入、修改、删除）前，需要先获取写锁，这回阻塞其他用户对该表的所有写锁操作。只有没有写锁时，其他读取的用户才能获得读锁，读锁之间时不相互阻塞的。

表锁有两种模式：读锁、写锁。


行级锁 可以最大程度地支持并发处理，同时也会带来最大锁开销。


表锁： 开销小，加锁快；不会出现死锁；锁定力度大，发生锁冲突概率高，并发度最低
行锁： 开销大，加锁慢；会出现死锁；锁定粒度小，发生锁冲突的概率低，并发度高
页锁： 开销和加锁速度介于表锁和行锁之间；会出现死锁；锁定粒度介于表锁和行锁之间，并发度一般


**读写锁**

> 读写锁也被称为共享锁(shared lock | S锁)和排他锁(exclusive lock | X锁)
MyISAM在执行查询语句（SELECT）前，会自动给涉及的所有表加读锁，在执行更新操作（UPDATE、DELETE、INSERT等）前，会自动给涉及的表加写锁，这个过程并不需要用户干预，因此，用户一般不需要直接用LOCK TABLE命令给MyISAM表显式加锁。



读锁：是共享的，或者说相互不阻塞。多个客户在同一时间可以同时读取同一个资源，而不相互干扰。

写锁：是排他的，也就是说一个写锁会阻塞其他的写锁和读锁。



**乐观锁和悲观锁**

乐观锁（Optimistic Lock）

乐观锁的特点先进行业务操作，不到万不得已不去拿锁。即“乐观”的认为拿锁多半是会成功的，因此在进行完业务操作需要实际更新数据的最后一步再去拿一下锁就好。
乐观锁在数据库上的实现完全是逻辑的，不需要数据库提供特殊的支持。一般的做法是在需要锁的数据上增加一个版本号，或者时间戳，然后按照如下方式实现：

```sql
select ctime, version as old_version from version_tablename;

update set data = ?, version = new_version where version = old_version;
```


> 数据库内部update同一行的时候是不允许并发的，即数据库每次执行一条update语句时会获取被update行的写锁，直到这一行被成功更新后才释放


悲观锁（Pessimistic Lock）

正如其名，具有强烈的独占和排他特性。它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。

```sql
	select * from account where name="Erica" for update
```



> 如果在mysql中用悲观锁务必要确定走了索引，否则全表扫描。所有扫描过的行都会被锁上。



**死锁**

> 死锁就是两个或者多个事务在同一个资源上相互占用，并请求锁定对方占用的资源，从而导致恶性循环的现象。特别容易在批量处理中发生死锁。

事务1
```sql
	start transaction
	update tablename set price = 30 where orderid = 100012345;
	update tablename set price = 28 where orderid = 100012346;
	commit;
```


事务2
```sql
	start transaction
	update tablename set price = 45 where orderid = 100012346;
	update tablename set price = 88 where orderid = 100012345;
	commit;
```


如果凑巧，两个事务同时执行了第一条 update 语句，接下来第二条数据发现已经被锁定。两个事务都等待对方释放锁，同时双方有对方需要的锁，则陷入死循环。除非有外部介入。



**脏读**

脏读是指在一个事务处理过程里读取了另一个未提交的事务中的数据。

当一个事务正在多次修改某个数据，而在这个事务中这多次的修改都还未提交，这时一个并发的事务来访问该数据，就会造成两个事务得到的数据不一致。例如：用户A向用户B转账100元，对应SQL命令如下

update account set money=money + 100 where name= B ;  (此时A通知B)

update account set money=money - 100 where name= A ;
　　
当只执行第一条SQL时，A通知B查看账户，B发现确实钱已到账（此时即发生了脏读），而之后无论第二条SQL是否执行，只要该事务不提交，则所有操作都将回滚，那么当B以后再次查看账户时就会发现钱其实并没有转。

**不可重复读**

不可重复读是指在对于数据库中的某个数据，一个事务范围内多次查询却返回了不同的数据值，这是由于在查询间隔，被另一个事务修改并提交了。

例如事务T1在读取某一数据，而事务T2立马修改了这个数据并且提交事务给数据库，事务T1再次读取该数据就得到了不同的结果，发送了不可重复读。

不可重复读和脏读的区别是，脏读是读到未提交的数据，而不可重复读则是读取了前一事务提交的数据。

在某些情况下，不可重复读并不是问题，比如我们多次查询某个数据当然以最后查询得到的结果为主。但在另一些情况下就有可能发生问题，例如对于同一个数据A和B依次查询就可能不同，A和B就可能打起来了


**幻读**

幻读问题是指一个事务的两次不同时间的相同查询返回了不同的的结果集。例如:一个 select 语句执行了两次，但是在第二次返回了第一次没有返回的行，这就是通常说的幻读。（第一次查询中，向数据表中插入了新的数据，导致第二次select的数据不一致）




**丢失更新**

是另一个锁导致的问题，简单来说其就是一个事务的更新操作会被另一个事务的更新操作所覆盖，从而导致数据的不一致 


> 使用过程中，还会发生其他一些问题 如 阻塞、死锁


##### 事务

> InnoDB 存储引擎中的事务完全符合ACID的特性

```
	原子性（Atomicity）：原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚，如果成功就必须要完全应用到数据库，如果操作失败则不能对数据库有任何影响。
	
	一致性（Consistency）：指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。
	
	隔离性（Isolation）：是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。
	
	持久性（Durability）：是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。
```


> 一个用户请求要么成功、要么失败，不能处于中间状态(Atomic)；一旦一个事务完成，将来所有事务都必须要基于这个完成后的状态(Consistent)；未完成的事务不会相互影响(Isolated)；一旦一个事务完成，就是持久的(Durable)。




> MySQL 数据库为我们提供的四种隔离级别：

　　① Serializable (串行化)：可避免脏读、不可重复读、幻读的发生。

　　② Repeatable read (可重复读)：可避免脏读、不可重复读的发生，但可能会发生幻读。

　　③ Read committed (读已提交)：可避免脏读的发生。

　　④ Read uncommitted (读未提交)：最低级别，任何情况都无法保证。

以上四种隔离级别最高的是Serializable级别，最低的是Read uncommitted级别，当然级别越高，执行效率就越低。像Serializable这样的级别，就是以锁表的方式(类似于Java多线程中的锁)使得其他的线程只能在锁外等待，所以平时选用何种隔离级别应该根据实际情况。在MySQL数据库中默认的隔离级别为Repeatable read (可重复读)。

> 在MySQL数据库中查看当前事务的隔离级别：

```sql
	select @@tx_isolation;
```
![tx_isolation](https://static.shijinrong.cn/blog/tx_isolation.png)


> 查看先当前库的线程情况：

```sql
	show full processlist;
```


> innodb的事务表INNODB_TRX看下里面是否有正在锁定的事务线程，看看ID是否在show full processlist里面的sleep线程中,如果是，就证明这个sleep的线程事务一直没有commit或者rollback而是卡住了，我们需要手动kill掉。

```sql
	SELECT * FROM information_schema.INNODB_TRX\G;
```


#### 性能调优

> 性能调优不是一项简单的工作，但也不是复杂的难事。InnoDB 性能问题可以从如下方面着手

- 选择合适的CPU

- 内存的重要性

- 硬盘对数据库性能的影响

- 合理地设置RAID

- 操作系统的选择也很重要

- 不同文件系统对数据库的影响

- 选择合适的基准测试工具

#### 问题

{% note danger %}  
联合索引 KEY `t_u_s` (`ctime`,`uid`,`source`) 与  KEY `idx_uid_ctime_source` (`uid`,`ctime`,`source`) 之间的区别大吗？（该表用来记录用户访问日志，uid是10位整型，ctime TIMESTAMP）
{% endnote %}


{% note success %}  

答案是，用户量不大访问量不高，是不会有太大的问题，但是当 QPS 上来了 就 gg 了。这是为什么呢？看索引区别

前者先是 ctime 字段（ TIMESTAMP ）数据会按照时间先后顺序排列成 B+ 树 （只可以精确到秒）但是每秒有上千的数据呢？它的排列还起作用吗？不起作用，没有大小之分了。


后者先是 uid 字段 （10位整型） 唯一 成千上万的数据也可以先按照 uid 排列、再按照时间，假设一秒钟一个 uid 发了1000个请求 这时候会不会出现像前者的情况呢？


这块在我优化之后，采用队列处理 确实处理能力上来了，优化时没有太注意这个问题，是等到我们搞活动时候，QPS 1.5W 就明显暴露出来了 🙄

其实这里是当每秒新增数据 120 （或者更低 需要看自己的业务流程）就会感觉乏力，处理能力几乎到 每几秒一个。采用后者则不会
{% endnote %}




### MySql 性能监控工具

> 工欲善其事、必先利其器。

- phpMyAdmin 

- Navicat for MySql

- Seuquel Pro



### Percona 数据库和 Percona XtraDB 存储引擎

Percona XtraDB 集群提供了高性能的集群环境，能轻松配置和管理多台数据库服务器，是的数据库之间能使用二进制日志来互相通信。集群环境能将负载分赛道不同的数据库服务器中，并提供灾备，以防止服务器死机。

官方文档： [https://www.percona.com](https://www.percona.com/software/mysql-database/percona-server/xtradb)

### Redis

官方文档： [https://redis.io/](https://redis.io/) 中文文档： [http://www.redis.cn/](http://www.redis.cn/)

据说微博全球最大 redis 集群，所以这里面的文章，我将会入门开始。

### Memcached

官方文档： [http://memcached.org/](http://memcached.org/)

Memcached 是一个高性能的分布式内存对象缓存系统，可以用于动态 Web 应用以减轻数据库负载。它通过在内存中缓存数据和对象来减少读取数据库的次数，从而提高动态、数据库驱动网站的速度。

** Memcached 主要的使用场景有以下两个：**

- 需要共享某些 Key-Value 形式的小数据时。（因为微博 Web 服务是分布式环境，所以使用全局变量方式等方式是不行的）。

- 缓存 MySQL 等后端存储的数据。快速进行数据响应，减轻后端存储的压力，同时，还可以为这些缓存数据指定过期时间。

Memcached 的实现决定了缓存的数据不是永久有效的，因此应用程序必须有针对 Memcached 失效时的向后端存取数据的重试方案。Memcached 不适合存放大文件，目前仅允许存放小于 1MB 的数据。



### 参考资料

- MySql 技术内幕 · InnoDB 存储引擎





