# Redis

[学习资源](https://www.bilibili.com/video/BV1oW411u75R)

## 一、NoSQL

### 1、是什么

NoSQL(NoSQL = Not Only SQL)，意即“不仅仅是SQL”，**泛指非关系型的数据库**。随着互联网web2.0网站的兴起，传统的关系数据库在应付web2.0网站，特别是超大规模和高并发的SNS类型的web2.0纯动态网站已经显得力不从心，暴露了很多难以克服的问题，而非关系型的数据库则由于其本身的特点得到了非常迅速的发展。NoSQL 数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。

**关系型数据库和非关系型数据库区别：**

关系型优点：

 1、易于维护：都是使用表结构，格式一致；

 2、使用方便：SQL语言通用，可用于复杂查询； 

3、复杂操作：支持SQL，可用于一个表以及多个表之间非常复杂的查询。

关系型缺点：

1、读写性能比较差，尤其是海量数据的高效率读写； 

2、固定的表结构，灵活度稍欠；

3、高并发读写需求，传统关系型数据库来说，硬盘I/O是一个很大的瓶颈。

非关系型优点：

1、格式灵活：存储数据的格式可以是key,value形式、文档形式、图片形式等等，文档形式、图片形式等等，使用灵活，应用场景广泛，而关系型数据库则只支持基础类型。 

2、速度快：nosql可以使用硬盘或者随机存储器作为载体，而关系型数据库只能使用硬盘； 

3、高扩展性； 

4、成本低：nosql数据库部署简单，基本都是开源软件。

非关系型缺点：

1、不提供sql支持，学习和使用成本较高； 

2、无事务处理； 

3、数据结构相对复杂，复杂查询方面稍欠。



### 2、能干嘛

- 易扩展

  NoSQL数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。数据之间无关系，这样就非常容易扩展。也无形之间，在架构的层面上带来了可扩展的能力。

- 大数据量高性能

  NoSQL数据库都具有非常高的读写性能，尤其在大数据量下，同样表现优秀。

  这得益于它的无关系性，数据库的结构简单。

  一般MySQL使用Query Cache，每次表的更新Cache就失效，是一种大粒度的Cache，在针对web2.0的交互频繁的应用，Cache性能不高。

  而NoSQL的Cache是记录级的，是一种细粒度的Cache，所以NoSQL在这个层面上来说就要性能高很多了。

- 多样灵活的数据模型

  NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式。

  而在关系数据库里，增删字段是一件非常麻烦的事情。如果是非常大数据量的表，增加字段简直就是一个噩梦。

### 3、四大分类

- KV
  - 新浪：BerkeleyDB + Redis
  - 美团：Redis + tair
  - 阿里、百度：memcache + Redis

- 文档型数据库（bson格式比较多）

  - CouchDB
  - MongoDB
    - MongoDB是一个基于分布式文件存储的数据库。由C++语言编写。旨在为WEB应用提供可扩展的高性能数据存储解决方案。
    - MongoDB是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

- 列存储数据库

  - Cassandra、HBase
  - 分布式文件系统

- 图关系数据库

  - 它不是放图形的、放的是关系比如：朋友圈社交网络、广告推荐系统
  - 社交网络、推荐系统。专注于构建关系图谱
  - Neo4j、InfoGrid

- 四者对比

  ![NoSQL四大分类对比.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/NoSQL%E5%9B%9B%E5%A4%A7%E5%88%86%E7%B1%BB%E5%AF%B9%E6%AF%94.png?raw=true)

### 4、分布式数据库CAP原理

#### 传统的ACID是什么

- A (Atomicity) 原子性
- C (Consistency) 一致性
- I (Isolation) 独立性
- D (Durability) 持久性

关系型数据库遵循ACID规则，事务在英文中是transaction，和现实世界中的交易很类似，它有如下四个特性：

**1、A (Atomicity) 原子性** 原子性很容易理解，也就是说事务里的所有操作要么全部做完，要么都不做，事务成功的条件是事务里的所有操作都成功，只要有一个操作失败，整个事务就失败，需要回滚。比如银行转账，从A账户转100元至B账户，分为两个步骤：1）从A账户取100元；2）存入100元至B账户。这两步要么一起完成，要么一起不完成，如果只完成第一步，第二步失败，钱会莫名其妙少了100元。

**2、C (Consistency) 一致性** 一致性也比较容易理解，也就是说数据库要一直处于一致的状态，事务的运行不会改变数据库原本的一致性约束。

**3、I (Isolation) 独立性** 所谓的独立性是指并发的事务之间不会互相影响，如果一个事务要获取的数据正在被另外一个事务修改，只要另外一个事务未提交，它所获取的数据就不受未提交事务的影响。比如现有有个交易是从A账户转100元至B账户，在这个交易还未完成的情况下，如果此时B查询自己的账户，是看不到新增加的100元的

**4、D (Durability) 持久性** 持久性是指一旦事务提交后，它所做的修改将会永久的保存在数据库上，即使出现宕机也不会丢失。



#### CAP

- C:Consistency（强一致性）
- A:Availability（可用性）
- P:Partition tolerance（分区容错性）

CAP理论就是说在分布式存储系统中，最多只能实现上面的两点。

而由于当前的网络硬件肯定会出现延迟丢包等问题，所以**分区容忍性**是我们必须需要实现的。所以我们只能在**一致性**和**可用性**之间进行权衡，**没有**NoSQL系统能同时保证这三点。

- CA 传统Oracle数据库
- AP 大多数网站架构的选择
- CP Redis、Mongodb

注意：分布式架构的时候必须做出取舍。

一致性和可用性之间取一个平衡。多余大多数web应用，其实并不需要强一致性。因此牺牲C换取P，这是目前分布式数据库产品的方向。

**经典CAP图**

![经典CAP图.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E7%BB%8F%E5%85%B8CAP%E5%9B%BE.png?raw=true)

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，**最多只能同时较好的满足两个**。

因此，根据 CAP 原理将 NoSQL 数据库分成了满足 CA 原则、满足 CP 原则和满足 AP 原则三 大类：

- CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
- CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。
- AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。

#### BASE

BASE就是为了解决关系数据库强一致性引起的问题而引起的可用性降低而提出的解决方案。

BASE其实是下面三个术语的缩写：

- 基本可用（Basically Available）
- 软状态（Soft state）
- 最终一致（Eventually consistent）

它的思想是通过**让系统放松对某一时刻数据一致性的要求来换取系统整体伸缩性和性能上改观**。为什么这么说呢，缘由就在于大型系统往往由于地域分布和极高性能的要求，不可能采用分布式事务来完成这些指标，要想获得这些指标，我们必须采用另外一种方式来完成，这里BASE就是解决这个问题的办法



#### 分布式+集群简介

分布式系统（distributed system）

由多台计算机和通信的软件组件通过计算机网络连接（本地网络或广域网）组成。分布式系统是建立在网络之上的软件系统。正是因为软件的特性，所以分布式系统具有高度的内聚性和透明性。因此，网络和分布式系统之间的区别更多的在于高层软件（特别是操作系统），而不是硬件。分布式系统可以应用在在不同的平台上如：PC、工作站、局域网和广域网上等。

简单来讲：

1. 分布式：不同的多台服务器上面部署**不同**的服务模块（工程），他们之间通过Rpc/Rmi之间通信和调用，对外提供服务和组内协作。
2. 集群：不同的多台服务器上面部署**相同**的服务模块，通过分布式调度软件进行统一的调度，对外提供服务和连接。

## 二、Redis

### 1、是什么

Redis:Remote Dictionary Server(远程字典服务器)是完全开源免费的，用C语言编写的，遵守BSD协议，是一个高性能的(key/value)分布式内存数据库，基于内存运行 并支持持久化的NoSQL数据库，是当前最热门的NoSql数据库之一，也被人们称为数据结构服务器。

Redis 与其他 key - value 缓存产品有以下三个特点：

- Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储
- Redis支持数据的备份，即master-slave模式的数据备份

### 2、能干嘛

- 内存存储和持久化：redis支持异步将内存中的数据写到硬盘上，同时不影响继续服务
- 取最新N个数据的操作，如：可以将最新的10条评论的ID放在Redis的List集合里面
- 模拟类似于HttpSession这种需要设定过期时间的功能
- 发布、订阅消息系统
- 定时器、计数器

## 三、key关键字

| 命令                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| DEL key                                   | 该命令用于在 key 存在时删除 key。                            |
| DUMP key                                  | 序列化给定 key ，并返回被序列化的值。                        |
| EXISTS key                                | 检查给定 key 是否存在。                                      |
| EXPIRE key seconds                        | 为给定 key 设置过期时间，以秒计。                            |
| EXPIREAT key timestamp                    | EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| PEXPIRE key milliseconds                  | 设置 key 的过期时间以毫秒计。                                |
| PEXPIREAT key milliseconds-timestamp      | 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计           |
| KEYS pattern                              | 查找所有符合给定模式( pattern)的 key 。                      |
| MOVE key db                               | 将当前数据库的 key 移动到给定的数据库 db 当中。              |
| PERSIST key                               | 移除 key 的过期时间，key 将持久保持。                        |
| PTTL key                                  | 以毫秒为单位返回 key 的剩余的过期时间。                      |
| TTL key                                   | 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。 |
| RANDOMKEY                                 | 从当前数据库中随机返回一个 key 。                            |
| RENAME key newkey                         | 修改 key 的名称                                              |
| RENAMENX key newkey                       | 仅当 newkey 不存在时，将 key 改名为 newkey 。                |
| SCAN cursor [MATCH pattern] [COUNT count] | 迭代数据库中的数据库键。                                     |
| TYPE key                                  | 返回 key 所储存的值的类型。                                  |

Key和Value最大值512M

集合最多存储元素个数 2^32^ - 1

## 四、持久化

### RDB持久化

在指定的**时间间隔**内将某个时间点的数据集**快照**都存放到硬盘上。可以将快照复制到其它服务器从而创建具有相同数据的服务器副本。如果系统发生故障，将会丢失最后一次创建快照之后的数据。

手动触发：

- save命令，使Redis处于阻塞状态，直到RDB持久化完成，才会响应其他客户端发来的命令。
- bgsave命令，fork一个子进程进行持久化，主进程只在fork过程中有短暂的阻塞，子进程创建后，主进程就可以响应客户端请求了。

优点：

- **RDB**对**Redis**的性能影响非常小，是因为在同步数据的时候他只是**fork**了一个子进程去做持久化的，而且他在数据恢复的时候速度比**AOF**来的快。

- 适合大规模数据恢复
- 节省磁盘空间
- 对数据完整性和一致性要求不高更适合使用

缺点：

- RDB快照间隔一段时间才生成一次，两次同步间的数据会丢失，完整性不好。
- 尽管使用了写时复制技术，如果数据量大还是比较消耗性能的。

备份是如何执行的：

Redis会单独创建（fork）一个子进程来进行持久化，**会先将数据写入到 一个临时文件中**，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。**RDB的缺点是最后一次持久化后的数据可能丢失**。

**Redis的fork：**

- Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程
- **写时复制技术**：通过fork创建子进程不会立刻触发大量的内存拷贝，只有在主进程修改内存数据时会以页为单位进行拷贝。主进程在拷贝后的页修改数据，子进程在原数据页进行存储。

### AOF持久化

以**日志**的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(**读操作不记录**)， **只许追加文件但不可以改写文件**，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

- AOF默认不开启。如果AOF和RDB同时开启，redis默认读取AOF的数据。

使用 AOF 持久化需要设置同步选项，从而确保写命令同步到磁盘文件上的时机。这是因为对文件进行写入并不会马上将内容同步到磁盘上，而是先存储到缓冲区，然后由操作系统决定什么时候同步到磁盘。有以下同步选项：

|   选项   |         同步频率         |
| :------: | :----------------------: |
|  always  |     每个写命令都同步     |
| everysec |       每秒同步一次       |
|    no    | 让操作系统来决定何时同步 |

- always 选项会严重减低服务器的性能；
- everysec 选项比较合适，可以保证系统崩溃时只会丢失一秒左右的数据，并且 Redis 每秒执行一次同步对服务器性能几乎没有任何影响；
- no 选项并不能给服务器性能带来多大的提升，而且也会增加系统崩溃时数据丢失的数量。

随着服务器写请求的增多，AOF 文件会越来越大。Redis 提供了一种将 **AOF 重写**的特性，能够去除 AOF 文件中的冗余写命令，压缩AOF文件容量。直接根据数据库里数据的最新状态，生成这些数据的插入命令，作为新日志。这个过程通过后台线程完成，避免了对主线程的阻塞。

**重写流程：**

- bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。
- 主进程fork出子进程执行重写操作，保证主进程不会阻塞。
- 子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。
- 1).子进程写完新的AOF文件后，向主进程发信号，主进程更新统计信息。2).主进程把aof_rewrite_buf中的数据写入到新的AOF文件。
- 使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。

优点：

- 最多丢失1s数据
- 追加方式写数据，性能好

缺点：

- 一样的数据AOF文件比RDB大很多
- 恢复备份速度比RDB慢

### redis持久化 aof文件出错怎么办

当发生这种情况时， 可以用以下方法来修复出错的 AOF 文件：

1. 为现有的 AOF 文件创建一个备份。
2. 使用 Redis 附带的 `redis-check-aof` 程序，对原来的 AOF 文件进行修复。

> ```
> $ redis-check-aof --fix
> ```

1. （可选）使用 `diff -u` 对比修复后的 AOF 文件和原始 AOF 文件的备份，查看两个文件之间的不同之处。
2. 重启 Redis 服务器，等待服务器载入修复后的 AOF 文件，并进行数据恢复。

## 五、IO模型

redis基于Reactor模式开发了自己的文件事件处理器，`file event handler`，这个文件事件处理器是单线程的，所以 redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，根据 socket 上的事件来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

- 多个socket
- IO多路复用程序
- 文件事件分派器
- 事件处理器

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO 多路复用程序会监听多个 socket，会将 socket 产生的事件放入队列中排队，事件分派器每次从队列中取出一个事件，把该事件交给对应的事件处理器进行处理。

**多路复用IO模型：**select poll epoll

**Reactor-IO模型**

Reactor模式**首先是事件驱动的，有一个或多个并发输入源，有一个Service Handler，有多个Request Handlers**；Service Handler会对输入的请求（Event）进行多路复用，并同步地将它们分发给相应的Request Handler。

![img](https://pic3.zhimg.com/80/v2-cfd7ed4a76c7b1df386bc4bd9576faca_720w.jpg)

**list实现原理、应用场景**

**hash实现原理、应用场景、rehash、渐进式hash**

## 六、主从复制

主机数据更新后根据配置和策略， 自动同步到备机的master/slaver机制，**Master**以写为主，**Slave**以读为主

优点：

- 读写分离，性能扩展
- 容灾快速恢复

![主从复制.jpg](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6.jpg?raw=true)

- 从服务器挂掉后，重启后变成了主服务器。

- 主服务器挂掉后，重启还是主服务器，原来的从服务器关系继续保持

**主从同步过程：**

你启动一台slave 的时候，他会发送一个**psync**命令给master ，如果是这个slave第一次连接到master，他会触发一个全量复制。master就会启动一个线程，生成**RDB**快照，还会**把新的写请求都缓存在内存中**，**RDB**文件生成后，master会将这个**RDB**发送给slave的，slave拿到之后做的第一件事情就是写进本地的磁盘，然后加载进内存，然后master会把内存里面缓存的那些新命名都发给slave。

每次主服务器进行写操作，会主动同步到从服务器。

**薪火相传：**

优点：上一个Slave可以是下一个slave的Master，Slave同样可以接收其他 slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力,去中心化降低风险。

缺点：风险是一旦某个slave宕机，后面的slave都没法备份。主机挂了，从机还是从机，无法写数据了

**反客为主：**

当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改。

用 slaveof no one  将从机变为主机。

### 从节点申请同步的时候主节点做了什么？

fork 出来的子进程指向父进程相同的内存地址空间，能完整读到父进程的内存，如果父进程后续有修改的话，修改前 kernel 会复制一份给子进程，这样子进程能读到 fork 时的内存页，继续进行生成 RDB 的过程

## 七、哨兵机制

**反客为主的自动版**，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库

1. 哨兵集群至少要 3 个节点，来确保自己的健壮性。（因为当主机宕机后，需要进行投票，需要半数以上同意，如果是两个哨兵，有一个宕机后无法满足要求，哨兵机制无法工作）
2. redis主从 + sentinel的架构，是不会保证数据的零丢失的，它是为了保证redis集群的高可用。
3. 假设master所在的机器不可用的话，那么哨兵还剩2个，sentinel 2 和sentinel3 就会认为master宕机，然后选举一个来处理故障转移

## 八、集群

问题：

- 容量不够，redis如何进行扩容？

- 并发写操作， redis如何分摊？

Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。

Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

## 九、Redis为什么在6.0引入了多线程？

**Redis 6.0中的多线程，也只是针对处理网络请求过程采用了多线程，而数据的读写命令，仍然是单线程处理的。**

在多路复用的IO模型中，在处理网络请求时，调用 select （其他函数同理）的过程是阻塞的，也就是说这个过程会阻塞线程，如果并发量很高，此处可能会成为瓶颈。

如果能采用多线程，使得网络处理的请求并发进行，就可以大大的提升性能。多线程除了可以减少由于网络 I/O 等待造成的影响，还可以充分利用 CPU 的多核优势。

## 十、数据结构

#### 1、String

Redis 中的字符串是一种 **动态字符串**，这意味着使用者可以修改，它的底层实现有点类似于 Java 中的 **ArrayList**，有一个字符数组，从源码的 **sds.h/sdshdr 文件** 中可以看到 Redis 底层对于字符串的定义 **SDS**，即 *Simple Dynamic String* 结构

**为什么不直接用C的char数组？**

- 计数方式不同

  C语言对字符串长度的统计，就完全来自遍历，从头遍历到末尾，直到发现空字符就停止，以此统计出字符串的长度，这样获取长度的时间复杂度来说是0（n）

  而Redis我在上面已经给大家看过结构了，他自己本身就保存了长度的信息，所以我们获取长度的时间复杂度为0(1)

- 杜绝缓冲区溢出

  C是不记录字符串长度的，一旦我们调用了拼接的函数，如果没有提前计算好内存，是会产生缓存区溢出的。

  SDS结构存储了当前长度，还有free未使用的长度，那简单呀，你现在做了拼接操作，我去判断一些是否可以放得下，如果长度够就直接执行，如果不够，那我就进行扩容。

- 减少修改字符串时带来的内存重分配次数

  C修改字符串需要重新分配内存

  SDS有**空间预分配**+**惰性空间释放**(当我们执行完一个字符串缩减的操作，redis并不会马上收回我们的空间，因为可以预防你继续添加的操作，这样可以减少分配空间带来的消耗，但是当你再次操作还是没用到多余空间的时候，Redis也还是会收回对于的空间，防止内存的浪费的。)

- 二进制安全

  C语言是判断空字符去判断一个字符的长度的，但是有很多数据结构经常会穿插空字符在中间，比如图片，音频，视频，压缩文件的二进制数据

  Redis就不存在这个问题了，他保存了字符串的长度，他不判断空字符，他就判断长度就可以了。所以二进制数据很安全。

**用途：**计数

#### 2、list

Redis 的列表相当于 Java 语言中的 **LinkedList**，注意它是链表而不是数组。这意味着 list 的插入和删除操作非常快，时间复杂度为 O(1)，但是索引定位很慢，时间复杂度为 O(n)。

**用途：**队列、栈

#### 3、Hash

Redis 中的字典相当于 Java 中的 **HashMap**，内部实现也差不多类似，都是通过 **"数组 + 链表"** 的链地址法来解决部分 **哈希冲突**，同时这样的结构也吸收了两种不同数据结构的优点。

**实际上字典结构的内部包含两个 hashtable**，通常情况下只有一个 hashtable 是有值的，但是在字典扩容缩容时，需要分配新的 hashtable，然后进行 **渐进式搬迁** *(下面说原因)*。

**用途：**购物车（用户id，商品id，数量）

**渐进式hash：**

大字典的扩容是比较耗时间的，需要重新申请新的数组，然后将旧字典所有链表中的元素重新挂接到新的数组下面，这是一个 O(n) 级别的操作，作为单线程的 Redis 很难承受这样耗时的过程，所以 Redis 使用 **渐进式 rehash** 小步搬迁：

渐进式 rehash 会在 rehash 的同时，保留新旧两个 hash 结构，如上图所示，查询时会同时查询两个 hash 结构，然后在后续的定时任务以及 hash 操作指令中，循序渐进的把旧字典的内容迁移到新字典中。当搬迁完成了，就会使用新的 hash 结构取而代之。

#### 4、set

Redis 的集合相当于 Java 语言中的 **HashSet**，它内部的键值对是无序、唯一的。它的内部实现相当于一个特殊的字典，字典中所有的 value 都是一个值 NULL。

**用途：**好友集合

#### 5、zset

它的内部实现用的是一种叫做 **「跳跃表」**和  **「字典」**的数据结构

skiplist编码的有序集合底层是一个命名为zset的结构体，而一个zset结构同时包含一个字典和一个跳跃表。跳跃表按score从小到大保存所有集合元素。而字典则保存着从member到score的映射，这样就可以用O(1)的复杂度来查找member对应的score值。虽然同时使用两种结构，但它们会通过指针来共享相同元素的member和score，因此不会浪费额外的内存。

###### 为什么不直接用跳跃表

假如我们单独使用 字典，虽然能以 O(1) 的时间复杂度查找成员的分值，但是因为字典是以无序的方式来保存集合元素，所以每次进行范围操作的时候都要进行排序；假如我们单独使用跳跃表来实现，虽然能执行范围操作，但是查找操作有 O(1)的复杂度变为了O(logN)。因此Redis使用了两种数据结构来共同实现有序集合。

跳表与平衡树和哈希表比较：

- skiplist和各种平衡树（如AVL、红黑树等）的元素是有序排列的，而哈希表不是有序的。因此，在哈希表上只能做单个key的查找，不适宜做范围查找。所谓范围查找，指的是查找那些大小在指定的两个值之间的所有节点。
- 在做范围查找的时候，平衡树比skiplist操作要复杂。在平衡树上，我们找到指定范围的小值之后，还需要以中序遍历的顺序继续寻找其它不超过大值的节点。如果不对平衡树进行一定的改造，这里的中序遍历并不容易实现。而在skiplist上进行范围查找就非常简单，只需要在找到小值之后，对第1层链表进行若干步的遍历就可以实现。
- 平衡树的插入和删除操作可能引发子树的调整，逻辑复杂，而skiplist的插入和删除只需要修改相邻节点的指针，操作简单又快速。
- 从内存占用上来说，skiplist比平衡树更灵活一些。一般来说，平衡树每个节点包含2个指针（分别指向左右子树），而skiplist每个节点包含的指针数目平均为1/(1-p)，具体取决于参数p的大小。如果像Redis里的实现一样，取p=1/4，那么平均每个节点包含1.33个指针，比平衡树更有优势。
- 查找单个key，skiplist和平衡树的时间复杂度都为O(log n)，大体相当；而哈希表在保持较低的哈希值冲突概率的前提下，查找时间复杂度接近O(1)，性能更高一些。所以我们平常使用的各种Map或dictionary结构，大都是基于哈希表实现的。
- 从算法实现难度上来比较，skiplist比平衡树要简单得多。

**用途：**排行榜



## 十一、面试题

### 1、Redis单线程为什么这么快？

- 纯内存操作
- 核心是基于非阻塞的IO多路复用机制
- 单线程反而避免了多线程的频繁上下文切换带来的性能问题

### 2、Redis的过期键的删除策略

- 惰性过期：只有当访问一个key时，才会判断key是否过期，过期则清除。对CPU友好，对内存不友好。极端情况可能出现大量的过期key没有再次被访问，从而不会被清除，占用大量内存。
- 定期过期：每隔一定的时间，会扫描一定数量的数据库的expires字典中一定数量的key，并清除其中已过期的key。对内存友好，CPU不友好。

### 3、缓存雪崩、缓存击穿、缓存穿透

缓存雪崩是指缓存同一时间大面积失效，后面的请求都会落到数据库上，造成数据库短时间内承受大量请求而崩掉。

**解决方案：**

- 缓存数据的过期时间设置随机，防止同一时间大量数据过期现象发生。



缓存穿透是指缓存和数据库中都没有数据，导致所有的请求都落到数据库上，造成数据库短时间内承受大量请求而崩掉。

**解决方案：**

- 把取不到的数据key的value写为null
- 布隆过滤器，将可能存在的数据哈希到一个足够大的map中，一个一定不存在的数据会被map拦截掉，避免了底层的压力。



缓存击穿是指缓存中没有但数据库有的数据。一般是因为缓存时间到期，由于并发用户特别多，全部落到了数据库，导致数据库压力过大。

**解决方案：**

- 设置热点数据永远不过期
- 加互斥锁
