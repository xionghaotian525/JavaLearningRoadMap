# Redis

## 目录
[TOC]

## 一、个人总结
### 1.安装配置
> 极简版，详细参考优质博客 [Windows下Redis安装与配置教程](https://blog.csdn.net/B11050729/article/details/131185533?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171308794116800222829974%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171308794116800222829974&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~top_positive~default-1-131185533-null-null.nonecase&utm_term=redis%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AEwindows&spm=1018.2226.3001.4450)
1. redis
   - 下载地址 [Redis for Windows 5.0.14.1](https://github.com/tporadowski/redis/releases)
   - 解压缩
   - 解压路径下`cmd`-->`redis-server`-->**启动redis服务**
   - 解压路径下`cmd`-->`redis-cli`-->**连接到redis服务**
2. redisInsight(可视化工具)
   - 下载地址 [redisInsight for windows](https://app.redislabs.com/#/rlec-downloads)

### 2.redis概述
1. **什么是redis**

Redis是一款**内存高速缓存数据库**。Redis全称为：Remote Dictionary Server（远程数据服务），使用C语言编写，Redis是一个**key-value存储系统**（键值存储系统），支持丰富的数据类型，如：String、list、set、zset、hash。

Redis是一种支持key-value等多种数据结构的存储系统。可用于缓存，事件发布或订阅，高速队列等场景。支持网络，提供字符串，哈希，列表，队列，集合结构直接存取，基于内存，可持久化。

2. **为什么使用redis**

- 读写性能优异:Redis能读的速度是110000次/s,写的速度是81000次/s
- 数据类型丰富:Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子性:Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
- 丰富的特性:Redis支持发布订阅、持久化、事件机制、事务，以及高可用&可扩展（分片模式、哨兵、主从复制）


3. **redis为什么快**

- **内存存储**：Redis是一个基于内存的数据存储系统，所有的数据操作都是直接在内存中进行的，相对于传统的磁盘存储，内存的读写速度要快得多。
- **数据结构简单**：Redis支持的数据结构相对简单（如字符串、列表、集合等），这些数据结构的操作都非常高效。
- **单线程模型**：Redis采用单线程模型来处理客户端的请求，这样可以避免线程切换和竞争状态的开销，尽管是单线程，但由于大多数操作都是内存操作，其性能依旧非常高。
- **非阻塞I/O**：Redis使用了非阻塞的网络I/O模型，例如epoll作为I/O多路复用技术的一部分，这使得Redis可以高效地处理多个客户端的连接和请求。
- **优化的数据操作算法**：Redis针对其支持的数据结构和操作进行了高度优化，许多操作的时间复杂度都很低（例如O(1)或O(log n)）。
- **持久化策略**：尽管是基于内存的系统，Redis也提供了RDB和AOF两种持久化方式，这些持久化操作是以非阻塞方式执行的，不会影响主线程的性能。
- **支持管道化**（Pipelining）：客户端可以一次性发送多个命令到服务器，减少网络延迟，服务器处理完这些命令后，一次性将结果返回给客户端。
- **发布/订阅模式**：提供了有效的消息广播/订阅支持，通过有效的内存数据分发，加快了消息传递的速度。

总结：通过**内存操作、简单的数据结构、单线程模型，以及高效的网络I/O处理等技术，Redis能够提供极快的数据读写性能**，这使其成为在需要高速数据存取场景下的首选数据库。

4. **哪些场景下使用redis**
- **热点数据的缓存**

   缓存是Redis最常见的应用场景，之所有这么使用，主要是因为Redis读写性能优异。而且逐渐有取代memcached，成为首选服务端缓存的组件。而且，Redis内部是支持事务的，在使用时候能有效保证数据的一致性。

   作为缓存使用时，一般有两种方式保存数据：
   - 读取前，先去读Redis，如果没有数据，读取数据库，将数据拉入Redis。
   - 插入数据时，同时写入Redis。

   方案一：实施起来简单，但是有两个需要注意的地方：
   - 避免缓存击穿。（数据库没有就需要命中的数据，导致Redis一直没有数据，而一直命中数据库。）
   - 数据的实时性相对会差一点。

   方案二：数据实时性强，但是开发时不便于统一处理。当然，两种方式根据实际情况来适用。

   如：方案一适用于对于数据实时性要求不是特别高的场景。方案二适用于字典表、数据量不大的数据存储。

- **限时业务的运用**

   redis中可以使用expire命令设置一个键的生存时间，到时间后redis会删除它。利用这一特性可以运用在限时的优惠活动信息、手机验证码等业务场景。

- **计数器相关问题**

   redis由于incrby命令可以实现原子性的递增，所以可以运用于高并发的秒杀活动、分布式序列号的生成、具体业务还体现在比如限制一个手机号发多少条短信、一个接口一分钟限制多少请求、一个接口一天限制调用多少次等等。

- **分布式锁**

   这个主要利用redis的setnx命令进行，setnx："set if not exists"就是如果不存在则成功设置缓存同时返回1，否则返回0 ，这个特性在很多后台中都有所运用，因为我们服务器是集群的，定时任务可能在两台机器上都会运行，所以在定时任务中首先 通过setnx设置一个lock， 如果成功设置则执行，如果没有成功设置，则表明该定时任务已执行。 当然结合具体业务，我们可以给这个lock加一个过期时间，比如说30分钟执行一次的定时任务，那么这个过期时间设置为小于30分钟的一个时间就可以，这个与定时任务的周期以及定时任务执行消耗时间相关。

   在分布式锁的场景中，主要用在比如秒杀系统等。

- **延时操作**

   比如在订单生产后我们占用了库存，10分钟后去检验用户是否真正购买，如果没有购买将该单据设置无效，同时还原库存。 由于redis自2.8.0之后版本提供Keyspace Notifications功能，允许客户订阅Pub/Sub频道，以便以某种方式接收影响Redis数据集的事件。 所以我们对于上面的需求就可以用以下解决方案，我们在订单生产时，设置一个key，同时设置10分钟后过期， 我们在后台实现一个监听器，监听key的实效，监听到key失效时将后续逻辑加上。当然我们**也可以利用rabbitmq、activemq等消息中间件的延迟队列服务**实现该需求。

- **排行榜相关问题**

   关系型数据库在排行榜方面查询速度普遍偏慢，所以可以借助redis的`SortedSet`进行热点数据的排序。比如点赞排行榜，**做一个SortedSet, 然后以用户的openid作为上面的username, 以用户的点赞数作为上面的score, 然后针对每个用户做一个hash, 通过zrangebyscore就可以按照点赞数获取排行榜，然后再根据username获取用户的hash信息**，这个当时在实际运用中性能体验也蛮不错的。

- **点赞、好友等相互关系的存储**
   
   Redis 利用集合的一些命令，比如求交集、并集、差集等。在微博应用中，每个用户关注的人存在一个集合中，就很容易实现求两个人的共同好友功能。 

- **简单队列**
   
   由于Redis有`list push`和`list pop`这样的命令，所以能够很方便的执行队列操作

### 3.redis常用命令

1. 键值相关命令：
- `KEYS pattern`: 查找所有符合给定模式的键。
- `EXISTS key`: 检查给定键是否存在。
- `DEL key`: 删除给定的一个或多个键。
- `EXPIRE key seconds`: 设置键的过期时间。
- `TTL key`: 获取键的剩余生存时间。
- `TYPE key`: 获取键对应值的类型。
2. 字符串(**String**)相关命令:
- `SET key value`：设置存储在给定键中的值。
- `GET key`：获取指定键的值。
- `INCR key`：将键的整数值增加一。
- `INCRBY key amount`: 将键的整数值加amount。
- `DECR key`：将键的整数值减少一。
- `MSET key1 value1 [key2 value2 ...]`：同时设置一个或多个键值对。
- `MGET key1 key2 ...`: 获取多个键的值。
- `APPEND key value`: 将给定的值追加到原值的末尾。
3. 列表(**List**)相关命令:
- `LPUSH key value1 [value2]`：将一个或多个值插入到列表头部。
- `RPUSH key value1 [value2]`：将一个或多个值插入到列表尾部。
- `LPOP key`：移除并获取列表最左边的元素。
- `RPOP key`：移除并获取列表最右边的元素。
- `LRANGE key start stop`：获取列表指定范围内的元素。
- `LLEN key`: 获取列表的长度。
4. 集合(**Set**)相关命令:
- `SADD key member1 [member2]`：向集合添加一个或多个成员。
- `SREM key member1 [member2]`：移除集合中一个或多个成员。
- `SMEMBERS key`：获取集合中的所有成员。
- `SISMEMBER key member`：判断成员元素是否是集合的成员。
- `SINTER key1 [key2]`: 返回所有给定集合的交集。
- `SUNION key1 [key2]`: 返回所有给定集合的并集。
5. 有序集合(**Zset**, Sorted Set)相关命令:
- `ZADD key score1 member1 [score2 member2]`：向有序集合添加一个或多个成员，或者更新已存在成员的分数。
- `ZREM key member [member ...]`：移除有序集合中的一个或多个成员。
- `ZRANGE key start stop [WITHSCORES]`：按照索引范围返回有序集合指定区间内的成员。
- `ZREVRANGE key start stop [WITHSCORES]`：返回有序集合中指定区间内的成员，通过索引，分数从高到底。
- `ZSCORE key member`: 返回有序集中指定成员的分数。
6. 散列(**Hash**)相关命令:
- `HSET key field value`：给哈希表中的字段赋值。
- `HGET key field`：获取存储在哈希表中指定字段的值。
- `HGETALL key`：获取在哈希表中指定 key 的所有字段和值。
- `HMSET key field1 value1 [field2 value2 ...]`：同时将多个 field-value (字段-值)对设置到哈希表中。
- `HDEL key field1 [field2]`：删除一个或多个哈希表字段。
- `HEXISTS key field`: 检查哈希表中指定字段是否存在。
- `HKEYS key`: 获取哈希表中所有字段的名字。

7. HyperLogLog相关命令
- `PFADD`: 添加元素到HyperLogLog中。
- `PFCOUNT`: 返回HyperLogLog的近似基数，即估计的唯一元素数量。
- `PFMERGE`: 合并多个HyperLogLog为一个。
8. Gitmap相关命令
- `SETBIT`: 设置字符串指定偏移量的位(bit)值。
- `GETBIT`: 获取字符串指定偏移量的位(bit)值。
- `BITFIELD`: 对字符串的二进制位执行多个操作。
- `BITOP`: 对一个或多个Bitmap进行AND、OR、NOT、XOR操作。
- `BITCOUNT`: 计算字符串被设置为1的位的数量。
- `BITPOS`: 查找第一个设置为0或1的位的位置。
9.  geospatial相关命令
- `GEOADD`: 添加具有给定地理空间位置（经度、纬度）的元素。
- `GEODIST`: 返回两个给定位置之间的距离。
- `GEORADIUS`: 以给定的经纬度为中心，找出某个半径内的元素。
- `GEORADIUSBYMEMBER`: 以某个已存储的位置为中心，找出某个半径内的元素。
- `GEOHASH`: 返回一个或多个位置元素的Geohash字符串。
- `GEOPOS`: 返回一个或多个位置元素的经度和纬度。
10. stream

消息队列相关命令
- `XADD`: 用于向Stream中追加消息，如果Stream不存在，此命令会自动创建Stream。
- `XTRIM` 对流进行修剪，限制长度
- `XDEL` 删除消息
- `XLEN` 获取流包含的元素数量，即消息长度
- `XRANGE` 获取消息列表，会自动过滤已经删除的消息
- `XREVRANGE` 反向获取消息列表，ID 从大到小
- `XREAD` 以阻塞或非阻塞方式获取消息列表，用于读取Stream中的消息，可以指定从某个ID开始读取。

消费者组相关命令：

- `XGROUP CREATE` 创建消费者组
- `XREADGROUP GROUP` 读取消费者组中的消息
- `XACK` 将消息标记为"已处理"
- `XGROUP SETID` 为消费者组设置新的最后递送消息
- `IDXGROUP DELCONSUMER` 删除消费者
- `XGROUP DESTROY` 删除消费者组
- `XPENDING` 显示待处理消息的相关信息
- `XCLAIM` 转移消息的归属权
- `XINFO` 查看流和消费者组的相关信息；
- `XINFO GROUPS` 打印消费者组的信息；
- `XINFO STREAM` 打印流信息

### 4.redis配置文件详解

Redis可执行文件说明：
文件名|说明
---|---
redis-server|Redis服务器
redis-cli|Redis命令行客户端
redis-benchmark|Redis性能测试工具
redis-check-aof|Redis文件修复工具
redis-check-dump|Redis文件检查工具

redis.conf配置文件：
1. 网络设置:
`bind`: 指定Redis监听的网络接口（如127.0.0.1, 192.168.1.1等）。
`port`: 设置Redis监听的端口，默认是6379。
`timeout`: 设置客户端闲置多长时间后关闭连接。
`tcp-keepalive`: 设置TCP连接的keepalive参数。
2. 通用设置:
`daemonize`: 是否以守护进程运行。
`pidfile`: 当Redis以守护进程运行时，pidfile指定了文件写入PID的位置。
`loglevel` 和 `logfile`: 设置日志记录的详细级别（如debug、verbose、notice、warning）以及日志文件的位置。
`databases`: 设置数据库数量，默认通常为16。
3. 安全:
`requirepass`: 设置客户端连接到Redis服务器所需的密码。
`rename-command`: 重命名特定的命令，以提高安全性。
4. 持久化:
使用 `save` 参数设置自动快照的间隔。
`appendonly`: 是否开启AOF（Append Only File）持久化模式。
`appendfsync`: AOF文件写入磁盘的频率。
5. 性能和资源管理:
`maxmemory`: 设置最大内存使用限制，超出限制时Redis会根据maxmemory-policy策略来删除键。
`maxclients`: 设置同时连接的最大客户端数。
6. 集群:
`cluster-enabled`: 是否启用Redis集群模式。
`cluster-config-file`: 指定集群的配置文件。
`cluster-node-timeout`: 指定节点超时的毫秒数。
6. 哨兵(Sentinel)模式:
`sentinel monitor`: 当运行在哨兵模式下，监控指定名字的master。
`sentinel auth-pass`: 哨兵认证主服务器所需的密码。
7. 附加参数:
`include`: 允许包含其他配置文件，可用于模块化配置管理。

### 5.redis数据结构

![](/Res/images/Redis-数据结构.png)
#### 5.1 redis五种基础数据类型

首先对redis来说，所有的key（键）都是字符串。我们在谈基础数据结构时，讨论的是存储值的数据类型，主要包括常见的5种数据类型，分别是：**String**、**List**、**Set**、**Zset**、**Hash**。
![](/Res/images/Redis-数据结构-五种基本数据类型.jpeg)

**1. 字符串 (String):**

* 最基本的数据类型，可以是**字符串、整数或浮点数**，存储文本字符串，例如数字，邮件地址，图片，甚至是序列化后的对象。
* 常用命令: `SET`, `GET`, `INCR`, `DECR`, `MGET`等。
* 读写能力：对整个字符串或字符串的一部分进行操作；对整数或浮点数进行自增或自减操作；
* 使用场景: 缓存数据，计数器，分布式锁等。

**2. 列表 (List):**

* **字符串列表**，按插入顺序排序，链表上的每个节点都包含一个字符串。
* 常用命令: `LPUSH`, `RPUSH`, `LPOP`, `RPOP`, `LLEN`等。
* 读写能力：对链表的两端进行push和pop操作，读取单个或多个元素；根据值查找或删除元素；
* 使用场景: 消息队列，文章列表等需要顺序处理的数据。

**3. 集合 (Set):**

* **存储不重复且无序的字符串集合**。
* 常用命令: `SADD`, `SREM`, `SMEMBERS`, `SINTER`等。
* 读写能力：字符串的集合，包含基础的方法有看是否存在添加、获取、删除；还包含计算交集、并集、差集等
* 使用场景: 标签，社交关系，唯一性检查、点赞、点踩、收藏等。

**4. 有序集合 (Sorted Set):**

* **它可以保存唯一的字符串元素，并且每个元素都关联一个浮点数分数**，Redis根据这个分数将所有元素从小到大排序。
* 常用命令: `ZADD`, `ZREM`, `ZRANGE`, `ZSCORE`等。
* 读写能力：字符串成员与浮点数分数之间的有序映射；元素的排列顺序由分数的大小决定；包含方法有添加、获取、删除单个元素以及根据分值范围或成员来获取元素
* 使用场景: **排行榜**，**优先级队列**等需要排序的数据。

**5. 哈希 (Hash):**

* 键值对集合，**存储键值对的无序散列表**，类似于 Python 中的字典。
* 常用命令: `HSET`, `HGET`, `HGETALL`, `HDEL`等。
* 读写能力：包含方法有添加、获取、删除单个元素
* 使用场景: 适合存储对象，存储用户信息，商品信息等结构化数据。

#### 5.2 redis三种特殊数据类型
**1. HyperLogLogs**

* HyperLogLog是一种概率数据结构，用于估计一个集合中的唯一元素的数量（基数）。
* 常用命令：HyperLogLog主要相关的命令有`PFADD`、`PFCOUNT`和`PFMERGE`。
* 使用场景：HyperLogLog提供了不确切但在错误率可接受的情况下（标准误差0.81%）对基数进行近似计算的能力，这使得它在**需要统计大量数据并且对空间使用非常敏感的应用场景**下非常有用，如计数大量数据流中的唯一访问者数量。这个结构可以非常省内存的去统计各种计数，比如注册 IP 数、每日访问 IP 数、页面实时UV、在线用户数，共同好友数等。

**2. Bitmaps**

* Bitmaps并非Redis的一种独立数据类型，而是**字符串（String）数据类型的一种特殊使用方式**。在Bitmaps中，每个元素只占用一个二进制位(bit)，可以表示两种状态（0或1）。
* 常用命令：Bitmap提供了一套位级别的操作命令，如SETBIT、GETBIT、BITCOUNT和BITOP。
* 使用场景：它们非常**适用于需要大量布尔值的场景**，例如在线状态、特征标记、日活跃用户等，因为它们能够以极小的空间利用率进行存储和操作。

**3. geospatial**

* Redis的Geo数据类型基于**有序集合（Zset）**
* 常用命令：包括GEOADD、GEODIST、GEORADIUS和GEORADIUSBYMEMBER。
* 使用场景：它允许将**地理位置信息（经度和纬度）与名称关联起来**，并执行范围查询、半径查询和查找给定位置的周围元素等操作。例如，可以使用Geo数据类型来存储店铺的位置，并查询特定范围内的所有店铺，或计算两个地点之间的距离。

#### 5.3 stream(v5.0)
![](/Res/images/Redis-数据结构-stream结构2.png)
**1. 为什么会设计stream**

stream借鉴了Kafka的设计，是一个新的强大的支持多播的可持久化的**消息队列**。从字面上看是流类型，但其实从功能上看，是Redis对消息队列（MQ，Message Queue）的完善实现。

**2. stream应用场景**

* **消息队列**（Message Queuing）
   - 分布式任务队列：可以将Redis Streams作为后台作业或任务的队列，系统中的生产者将任务发布到Stream中，消费者从中读取并处理这些任务。
   - 应用解耦：在微服务架构中，使用Streams来传递消息，可以减少服务间的直接依赖，提高系统的可扩展性和可维护性。
* **事件驱动架构**（Event-driven Architecture）
  - 事件通知：在基于事件的系统中，可以使用Streams来发布和订阅事件，使得当事件发生时，相关的服务可以立即做出响应。
  - 实时数据处理：例如，实时分析用户行为、监控数据或物联网（IoT）设备的状态变化。
* **日志收集**（Log Aggregation）
将来自不同服务或应用的日志聚合到一个中心位置，方便实时监控和后续的日志分析处理。
* **流式数据分析**（Stream Analytics）
对实时产生的数据流（比如来自社交媒体、电商平台的交易数据）进行实时分析和处理，以便快速做出决策或洞察消费者行为等。
* **聊天应用和实时通信**（Chat Apps and Real-time Communication）
用于构建高性能、可扩展的聊天应用，使用Stream来传递消息和事件，实现实时通信。
* **时间序列数据**
收集和分析时间序列数据（例如金融市场数据、气象数据等），Redis Streams可以通过其消息ID来确保数据的时间顺序性。

Redis Streams特别适合处理业务场景足够简单，对于数据丢失不敏感，而且消息积压概率比较小的情况，(需要高吞吐量、低延迟和可靠性的应用场景？)。此外，通过将数据持久化和实现消费群组的概念，它还提供了一种有效的方式来平衡消息的生产和消费，保证了数据处理的可靠性和效率。

**3. stream结构**
![](/Res/images/Redis-数据结构-stream结构.png)
* `Consumer Group` ：消费组，使用 XGROUP CREATE 命令创建，一个消费组有多个消费者(Consumer), 这些消费者之间是竞争关系。
* `last_delivered_id` ：游标，每个消费组会有个游标 last_delivered_id，任意一个消费者读取了消息都会使游标 last_delivered_id 往前移动。
* `pending_ids` ：消费者(Consumer)的状态变量，作用是维护消费者的未确认的 id。 pending_ids 记录了当前已经被客户端读取的消息，但是还没有 ack (Acknowledge character：确认字符)。如果客户端没有ack，这个变量里面的消息ID会越来越多，一旦某个消息被ack，它就开始减少。这个pending_ids变量在Redis官方被称之为PEL，也就是Pending Entries List，这是一个很核心的数据结构，它用来确保客户端至少消费了消息一次，而不会在网络传输的中途丢失了没处理。

此外我们还需要理解两点：
* 消息ID: 消息ID的形式是timestampInMillis-sequence，例如1527846880572-5，它表示当前的消息在毫米时间戳1527846880572时产生，并且是该毫秒内产生的第5条消息。消息ID可以由服务器自动生成，也可以由客户端自己指定，但是形式必须是整数-整数，而且必须是后面加入的消息的ID要大于前面的消息ID。
* 消息内容: 消息内容就是键值对，形如hash结构的键值对，这没什么特别之处。

**4. 独立消费**

我们可以在不定义消费组的情况下进行Stream消息的独立消费，当Stream没有新消息时，甚至可以阻塞等待。Redis设计了一个单独的消费指令`xread`，可以将Stream当成普通的消息队列(list)来使用。使用xread时，我们可以完全忽略消费组(Consumer Group)的存在，**就好比Stream就是一个普通的列表(list)**。

**5. 消费组消费**
![](/Res/images/Redis-数据结构-stream-消费组消费.png)
* 创建消费组
Stream通过`xgroup create`指令创建消费组(Consumer Group)，需要传递起始消息ID参数用来初始化`last_delivered_id`变量。
* 消费组消费
Stream提供了`xreadgroup`指令可以进行消费组的组内消费，需要提供消费组名称、消费者名称和起始消息ID。它同xread一样，也可以阻塞等待新消息。读到新消息后，对应的消息ID就会进入消费者的PEL(正在处理的消息)结构里，客户端处理完毕后使用xack指令通知服务器，本条消息已经处理完毕，该消息ID就会从PEL中移除。
#### 5.4 对象机制
![](/Res/images/Redis-数据结构-RedisObject.png)
**1. 为什么会设计RedisObject**
- Redis 必须让每个键都带有类型信息, 使得程序可以检查键的类型, 并为它选择合适的处理方式

在redis的命令中，用于对键进行处理的命令占了很大一部分，而对于键所保存的值的类型（键的类型），键能执行的命令又各不相同。如： `LPUSH` 和 `LLEN` 只能用于列表键, 而 `SADD` 和 `SRANDMEMBER` 只能用于集合键, 等等; 另外一些命令, 比如 `DEL`、 `TTL` 和 `TYPE`, 可以用于任何类型的键；但是要正确实现这些命令, 必须为不同类型的键设置不同的处理方式: 比如说, 删除一个列表键和删除一个字符串键的操作过程就不太一样。

- 操作数据类型的命令除了要对键的类型进行检查之外, 还需要根据数据类型的不同编码进行多态处理

比如说， 集合类型就可以由字典和整数集合两种不同的数据结构实现， 但是， 当用户执行 `ZADD` 命令时， 他/她应该不必关心集合使用的是什么编码， 只要 Redis 能按照 `ZADD` 命令的指示， 将新元素添加到集合就可以了。

**2. redisObject数据结构**
```java
/*
 * Redis 对象
 */
typedef struct redisObject {

    // 类型
    unsigned type:4;

    // 编码方式
    unsigned encoding:4;

    // LRU - 24位, 记录最末一次访问时间（相对于lru_clock）; 或者 LFU（最少使用的数据：8位频率，16位访问时间）
    unsigned lru:LRU_BITS; // LRU_BITS: 24

    // 引用计数
    int refcount;

    // 指向底层数据结构实例
    void *ptr;

} robj;
```

![](/Res/images/Redis-数据结构-RedisObject数据结构.png)

- `type`记录了对象所保存的值的类型，它的值可能是以下常量中的一个：

```java
/*
* 对象类型
*/
#define OBJ_STRING 0 // 字符串
#define OBJ_LIST 1 // 列表
#define OBJ_SET 2 // 集合
#define OBJ_ZSET 3 // 有序集
#define OBJ_HASH 4 // 哈希表
```

- `encoding`记录了对象所保存的值的编码，它的值可能是以下常量中的一个：

```java
/*
* 对象编码
*/
#define OBJ_ENCODING_RAW 0     /* Raw representation */
#define OBJ_ENCODING_INT 1     /* Encoded as integer */
#define OBJ_ENCODING_HT 2      /* Encoded as hash table */
#define OBJ_ENCODING_ZIPMAP 3  /* 注意：版本2.6后不再使用. */
#define OBJ_ENCODING_LINKEDLIST 4 /* 注意：不再使用了，旧版本2.x中String的底层之一. */
#define OBJ_ENCODING_ZIPLIST 5 /* Encoded as ziplist */
#define OBJ_ENCODING_INTSET 6  /* Encoded as intset */
#define OBJ_ENCODING_SKIPLIST 7  /* Encoded as skiplist */
#define OBJ_ENCODING_EMBSTR 8  /* Embedded sds string encoding */
#define OBJ_ENCODING_QUICKLIST 9 /* Encoded as linked list of ziplists */
#define OBJ_ENCODING_STREAM 10 /* Encoded as a radix tree of listpacks */
```

- `ptr`是一个指针，指向实际保存值的数据结构，这个数据结构由`type`和`encoding`属性决定。举个例子， 如果一个redisObject 的`type` 属性为`OBJ_LIST` ， `encoding` 属性为 `OBJ_ENCODING_QUICKLIST` ，那么这个对象就是一个Redis 列表(List)，**它的值保存在一个QuickList的数据结构内，而ptr 指针就指向quicklist的对象**；

- `lru`属性: 记录了对象最后一次被命令程序访问的时间

**3. Redis执行处理数据类型命令的步骤**
- 根据给定的key，在数据库字典中查找和他相对应的redisObject，如果没找到，就返回NULL；
- 检查redisObject的type属性和执行命令所需的类型是否相符，如果不相符，返回类型错误；
- 根据redisObject的encoding属性所指定的编码，选择合适的操作函数来处理底层的数据结构；
- 返回数据结构的操作结果作为命令的返回值。

例：LPOP
![](/Res/images/Redis-数据结构-对象机制-处理数据类型命令步骤LPOP.png)
**4. 对象共享**

redis一般会把一些常见的值放到一个共享对象中，这样可使程序避免了重复分配的麻烦，也节约了一些CPU时间。

redis预分配的值对象：
- 小而常见的字符串，各种命令的返回值，比如成功时返回的OK，错误时返回的ERROR，命令入队事务时返回的QUEUE，等等
- 包括0 在内，小于`REDIS_SHARED_INTEGERS`的所有整数（`REDIS_SHARED_INTEGERS`的默认值是10000）

![](/Res/images/Redis-数据结构-RedisObject对象机制-对象共享.png)


**共享对象通常是跟指针结合使用的**，因为共享对象的目的是要让多个地方都能引用到同一个内存地址，而防止重复创建相同内容的对象。

但是，需要注意的是，对于 Redis 的字符串数据类型，引用共享对象还受到某些操作的限制。例如，一旦对一个共享的字符串进行修改，它就不再共享，因为字符串在 Redis 中是不可变的。这时，系统会创建一个新的对象以存储修改后的字符串，这是一种被称作写时复制（Copy-On-Write）的机制

**为什么redis不共享列表对象、哈希对象、集合对象、有序集合对象，只共享字符串对象？**

列表对象、哈希对象、集合对象、有序集合对象，本身可以包含字符串对象，复杂度较高。如果共享对象是保存字符串对象，那么验证操作的复杂度为O(1)如果共享对象是保存字符串值的字符串对象，那么验证操作的复杂度为O(N)如果共享对象是包含多个值的对象，其中值本身又是字符串对象，即其它对象中嵌套了字符串对象，比如列表对象、哈希对象，那么验证操作的复杂度将会是O(N的平方)如果对复杂度较高的对象创建共享对象，需要消耗很大的CPU，用这种消耗去换取内存空间，是不合适的

**5.   引用计数以及对象的销毁**

- 每个redisObject结构都带有一个`refcount`属性，指示这个对象被引用了多少次；
- 当新**创建**一个对象时，它的`refcount`属性被设置为1；
- 当对一个对象进行**共享**时，redis将这个对象的`refcount`加一；
- 当**使用完**一个对象后，或者**消除对一个对象的引用**之后，程序将对象的`refcount`减一；
- 当对象的`refcount`降至0 时，这个RedisObject结构，以及它引用的数据结构的内存都会被释放。

#### 5.5 底层数据结构
**1. 简单动态字符串** - sds

* 概述

Redis 中的 SDS（Simple Dynamic String，简单动态字符串）是 Redis 用于存储字符串值的底层实现，是对 C 语言传统字符串（以 null 结尾的字符数组）的改良，内部结构类似于 C 语言的字符数组，但**额外存储了字符串长度和分配空间大小**，避免了 C 字符串的缺陷。SDS **用于解决 C 字符串的一些限制和安全性问题**, 具有动态扩容的特点。SDS 除了保存数据库中的字符串值以外，SDS 还可以作为缓冲区（buffer）：包括 AOF 模块中的AOF缓冲区以及客户端状态中的输入缓冲区。

* 结构
```java
struct sdshdr {
    uint32_t len;    //字符串长度
    uint32_t alloc;  //字符串空间大小
    unsigned char flags; //表示sds的类型（8位）
    char buf[];  //用于存储字符串数据
};
```
![](/Res/images/Redis-数据结构-底层数据结构-SDS.png)
![](/Res/images\Redis-数据结构-底层数据结构-SDS2.png)

* 为什么使用SDS

   - **长度灵活**： 
  SDS 在内存中的表示包含了字符串的长度信息，因此它可以包含任意长度的数据，包括空字符('\0')。这解决了 C 字符串无法存储空字符的问题。
  
  - **快速获取长度**：
  由于 `len` 属性的存在，我们获取 SDS 字符串的长度只需要读取 `len` 属性，时间复杂度为 O(1)。而对于 C 语言，获取字符串的长度通常是经过遍历计数来实现的，时间复杂度为 O(n)。通过 `strlen key` 命令可以获取 key 的字符串长度。

  - **防止缓冲区溢出**：
   在 C 语言中使用 strcat 函数来进行两个字符串的拼接，**一旦没有分配足够长度的内存空间，就会造成缓冲区溢出**。而对于 SDS 数据类型，在进行字符修改的时候，会**首先根据记录的 len 属性检查内存空间是否满足需求, 如果不满足，会进行相应的空间扩展，然后在进行修改操作**，所以不会出现缓冲区溢出。

  - **减少修改字符串时的内存重新分配次数**：       
   C语言由于不记录字符串的长度，所以如果要修改字符串，必须要重新分配内存（**先释放再申请**），因为如果没有重新分配，字符串长度增大时会造成内存缓冲区溢出，字符串长度减小时会造成内存泄露。而对于SDS，由于`len`属性和`alloc`属性的存在，对于修改字符串SDS实现了空间预分配和惰性空间释放两种策略：   
      - **空间预分配**：对字符串进行空间扩展的时候，扩展的内存比实际需要的多，这样可以减少连续执行字符串增长操作所需的内存重分配次数。
        - 如果对 SDS 进行修改之后， SDS 的长度（也即是 len 属性的值）将小于 1 MB ， 那么程序分配和 len 属性同样大小的未使用空间， 这时 SDS len 属性的值将和 free 属性的值相同。 举个例子， 如果进行修改之后， SDS 的 len 将变成 13 字节， 那么程序也会分配 13 字节的未使用空间， SDS 的 buf 数组的实际长度将变成 13 + 13 + 1 = 27 字节（额外的一字节用于保存空字符）。
        - 如果对 SDS 进行修改之后， SDS 的长度将大于等于 1 MB ， 那么程序会分配 1 MB 的未使用空间。 举个例子， 如果进行修改之后， SDS 的 len 将变成 30 MB ， 那么程序会分配 1 MB 的未使用空间， SDS 的 buf 数组的实际长度将为 30 MB + 1 MB + 1 byte 。
      - **惰性空间释放**：对字符串进行缩短操作时，程序不立即使用内存重新分配来回收缩短后多余的字节，而是使用 alloc 属性将这些字节的数量记录下来，等待后续使用。（当然SDS也提供了相应的API，当我们有需要时，也可以手动释放这些未使用的空间。）

  - **二进制安全**：SDS 可以存储任何二进制数据，包括图片、音频等非文本数据。这是因为 SDS 不依赖 null 字符来标识字符串的结束，而是以 `len` 属性表示的长度来判断字符串是否结束，因此数据中可以包含任意的二进制序列。
  - **兼容部分 C 字符串函数**：尽管 SDS 是对 C 字符串的扩展，但它的存储布局确保了在以 \0 结尾处的内存地址上的内容可以被视为一个正常的 C 字符串，这意味着可以在不需要复制字符串的前提下，利用 C 的一些标准库函数来操作 SDS。

**2. 压缩列表** - ZipList

- 概述

一种连续内存空间存储的顺序数据结构，每个元素可以是字符串或整数。优点:节省内存空间。适用于存储小规模的列表或有序集合。缺点:修改操作可能引发连锁更新，影响性能。使用场景: 在列表键元素较少或元素都是小整数时使用。

![](/Res/images/Redis-数据结构-底层数据结构-ziplist.png)

- ziplist结构(直达源码-->[src/ziplist.c](https://github.com/redis/redis/blob/unstable/src/ziplist.c))
   - **Header**（头部）: 包含了ziplist的总字节长度、尾部（最后一个元素）的偏移量，以及ziplist中元素的数量。这些信息有助于快速地访问ziplist的基本属性和迅速找到ziplist的尾部。
   - **Entry**（项&条目）: 每个项可以存储一个整数或者一个字符串。  
   - **End**（结束符）: 一个特定的字节（通常是0xFF），标记着ziplist的末尾，确保了ziplist结构的正确解析和遍历的终止。
   ```XML
   * ZIPLIST OVERALL LAYOUT
   * ======================
   *
   * The general layout of the ziplist is as follows:
   *
   * <zlbytes> <zltail> <zllen> <entry> <entry> ... <entry> <zlend>
   *
   * NOTE: all fields are stored in little endian, if not specified otherwise.
   *
   * <uint32_t zlbytes> is an unsigned integer to hold the number of bytes that
   * the ziplist occupies, including the four bytes of the zlbytes field itself.
   * This value needs to be stored to be able to resize the entire structure
   * without the need to traverse it first.
   *
   * <uint32_t zltail> is the offset to the last entry in the list. This allows
   * a pop operation on the far side of the list without the need for full
   * traversal.
   *
   * <uint16_t zllen> is the number of entries. When there are more than
   * 2^16-2 entries, this value is set to 2^16-1 and we need to traverse the
   * entire list to know how many items it holds.
   *
   * <uint8_t zlend> is a special entry representing the end of the ziplist.
   * Is encoded as a single byte equal to 255. No other normal entry starts
   * with a byte set to the value of 255.
   ```

- Entry结构
   - **Previous Entry Length**（前一项的长度）: 存储前一项的长度，使得ziplist可以被双向遍历。
   - **Encoding**（编码）: 指定了存储的值是整数还是字符串，以及使用了哪一种格式或长度。
   - **Content**（内容）: 实际存储的数据，可以是一个整数的二进制表示，或者是一个字符串的字节序列。

   ```XML
   * ZIPLIST ENTRIES
   * ===============
   *
   * Every entry in the ziplist is prefixed by metadata that contains two pieces
   * of information. First, the length of the previous entry is stored to be
   * able to traverse the list from back to front. Second, the entry encoding is
   * provided. It represents the entry type, integer or string, and in the case
   * of strings it also represents the length of the string payload.
   * So a complete entry is stored like this:
   *
   * <prevlen> <encoding> <entry-data>
   ```
   在entry中存储的是比较小的int类型时，encoding和entry-data会合并一起表示，此时会没有entry-data字段
   ```XML
   * Sometimes the encoding represents the entry itself, like for small integers
   * as we'll see later. In such a case the <entry-data> part is missing, and we
   * could have just:
   *
   * <prevlen> <encoding>
   ```
   前一个entry长度`prevlen`编码方式：如果这个长度小于 254 字节，它只会消耗一个字节，将长度表示为无符号 8 位整数。 当长度大于等于254时，会消耗5个字节。 第一个字节设置为 254 (FE) 以指示后面有更大的值。 其余 4 个字节以前一个条目的长度为值。
   ```XML
   * So practically an entry is encoded in the following way:
   *
   * <prevlen from 0 to 253> <encoding> <entry>
   *
   * Or alternatively if the previous entry length is greater than 253 bytes
   * the following encoding is used:
   *
   * 0xFE <4 bytes unsigned little endian prevlen> <encoding> <entry>
   ```
   encoding字段内容：条目的encoding字段取决于条目的内容。 当条目是**字符串**时，encoding第一个字节的前 2 位将保存用于存储字符串长度的编码类型，后面是字符串的实际长度。 当条目是**整数**时，encoding前 2 位都设置为 1。接下来的 2 位用于指定此标头之后将存储哪种整数。 不同类型和编码的概述如下。 第一个字节始终足以确定条目的类型。
   ```XML
   * |00pppppp| - 1 byte
   *      String value with length less than or equal to 63 bytes (6 bits).
   *      "pppppp" represents the unsigned 6 bit length.
   * |01pppppp|qqqqqqqq| - 2 bytes
   *      String value with length less than or equal to 16383 bytes (14 bits).
   *      IMPORTANT: The 14 bit number is stored in big endian.
   * |10000000|qqqqqqqq|rrrrrrrr|ssssssss|tttttttt| - 5 bytes
   *      String value with length greater than or equal to 16384 bytes.
   *      Only the 4 bytes following the first byte represents the length
   *      up to 2^32-1. The 6 lower bits of the first byte are not used and
   *      are set to zero.
   *      IMPORTANT: The 32 bit number is stored in big endian.
   * |11000000| - 3 bytes
   *      Integer encoded as int16_t (2 bytes).
   * |11010000| - 5 bytes
   *      Integer encoded as int32_t (4 bytes).
   * |11100000| - 9 bytes
   *      Integer encoded as int64_t (8 bytes).
   * |11110000| - 4 bytes
   *      Integer encoded as 24 bit signed (3 bytes).
   * |11111110| - 2 bytes
   *      Integer encoded as 8 bit signed (1 byte).
   * |1111xxxx| - (with xxxx between 0001 and 1101) immediate 4 bit integer.
   *      Unsigned integer from 0 to 12. The encoded value is actually from
   *      1 to 13 because 0000 and 1111 can not be used, so 1 should be
   *      subtracted from the encoded 4 bit value to obtain the right value.
   * |11111111| - End of ziplist special entry.
   *
   * Like for the ziplist header, all the integers are represented in little
   * endian byte order, even when this code is compiled in big endian systems.
   ```

- 为什么ZipList特别省内存
   - ziplist节省内存是相对于普通的list来说的，如果是普通的数组，那么它每个元素占用的内存是一样的且取决于最大的那个元素（很明显它是需要预留空间的）；
   - 所以ziplist在设计时就很容易想到要尽量让每个元素按照实际的内容大小存储，**所以增加encoding字段**，**针对不同的encoding来细化存储大小**；
   - 这时候还需要解决的一个问题是遍历元素时如何定位下一个元素呢？在普通数组中每个元素定长，所以不需要考虑这个问题；但是ziplist中每个data占据的内存不一样，**所以为了解决遍历，需要增加记录上一个元素的length，所以增加了prelen字段**

- ziplist的缺点

  - **不预留内存空间**：ziplist 不预留额外的内存空间，在写操作时可能需要频繁进行内存分配和释放操作，影响性能。

  - **立即缩容**：在移除节点后，ziplist 立即缩容，可能导致频繁的内存分配和释放操作。

  - **链式扩容**：节点如果扩容，可能导致节点占用的内存增长，并且在超过一定字节后，可能会导致链式扩容。链式扩容的时间复杂度为 O(N)，虽然在大多数情况下概率较小，但在恶劣情况下会导致性能下降。
  
      > 链式扩容（级联更新）这种现象发生在 ziplist 数据结构中，主要是因为 ziplist 存储了关于前一个元素长度的元数据信息。**当一个元素被插入或修改导致其长度增加，并且这个长度增加导致存储前一个元素长度的空间（prevlen）不足以容纳新的长度时**，就需要对当前元素进行重新分配以适应长度的变化。这种重新分配可能会影响到相邻的元素，导致它们也需要进行重新分配来适应自己前一个元素的长度变化，从而形成一种“级联”效应。级联更新是 ziplist 的一个潜在缺陷，因为它可能会导致对多个连续元素进行重复的内存分配操作，从而影响性能。这是在 Redis 的后续版本中**引入 listpack 作为替代**的重要原因之一，listpack 设计上试图减少这种级联更新的可能性，提高数据操作的效率。 

   综上所述，尽管 ziplist 能够有效地节省内存空间，但在写操作频繁、节点删除较多或节点扩容较大时，可能会出现性能问题。

**3. 快表** - QuickList

- 概述

QuickList是由多个 ziplist 组成的双向链表，每个 ziplist 存储一定数量的元素。优点:结合了 ziplist 和双向链表的优点，既节省空间，又提升了修改操作的性能。使用场景: 在列表键元素较多或包含较大元素时使用。

![](/Res/images/Redis-数据结构-底层数据结构-QuickList2.png)
   
> ziplist补充(ziplist缺点-链式扩容&级联更新)
> 当一个entry被插入的时候，我们需要设置下一个entry中的prevlen字段为新插入entry的长度。会发生如下的情况：新插入entry的长度超过了254[>=254]，那么下一个entry的prelen需要5个字节记录该长度，但是呢，此时下一个entry的prevlen字段此时只有一个字节，所以需要对下一个entry进行grown[扩容]，更糟糕的是，下个entry因为扩容导致长度超过254，还会引起下下个entry的扩容…，这种现象呢就是级联更新，简单点来说就是，**因为一个entry的插入导致之后的entry都需要重新扩容和记录前一个entry的prevlen现象称之为“级联更新”**。

从 Redis 6.2 版本开始，quicklist 的底层实现由原来的 ziplist 改为了 listpack。Listpack 是 ziplist 的升级版本，主要是为了解决ziplist中存在的一些问题，比如，ziplist中扩展元素长度时可能需要进行昂贵的重新分配操作。listpack 提供了更好的性能和内存使用效率，在保持与 ziplist 类似的密集存储方式的同时，允许更大的单个元素大小，并且有更强的扩展性。**listpack和ziplist对象结构的不同是，listpack将prevlen替换为了curlen，从而有效避免级联更新。并将且将culen字段放在entry结构的最后面。这样做是为了，能够通过total-bytes定位到最后一个element的末尾位置然后获取到curlen从而找到前一个element的位置，从而实现从后往前遍历。**


> 补充说明listpack和ziplist
> 参考博客 [ziplist和listpack](https://blog.csdn.net/weixin_46290302/article/details/134088080?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171375528216800180626501%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=171375528216800180626501&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~sobaiduend~default-1-134088080-null-null.nonecase&utm_term=listpack%E5%92%8Cziplist&spm=1018.2226.3001.4450)
> - ziplist包括三大部分，<头部，entry，尾部>。头部包含"zlbytes,zltail,zllen"；尾部包含"zlend标记ziplist的结尾"；entry包括"prevlen,encoding,entry_data"。由于prevlen记录方式存在级联更新的问题，小于254字节需要1字节内存，大于等于254字节需要5字节内存。
> - listpack包括三大部分，<头部，entry，尾部>。头部包含"zlbytes,zllen"相比与ziplist少了zltail；entry包括"encoding,entry_data,curlen"；尾部包含"zlend标记listpack的结尾"。由于entry中的prevlen由curlen取代，所以不再有级联更新的问题。
> - 不论是"ziplist"还是"listpack"获取len的方式都是一样的：
   先判断头部中zllen字段存储的数值与UNIT16_MAX的关系
   小于UNIT16_MAX，直接返回zllen
   大于等于UNIT16_MAX需要从头到尾遍历获取元素总数
   如果新得到的元素总数小于UINT16_MAX，重新设置zllen的数值


- quicklist结构(直达源码-->[src/quicklist.h](https://github.com/redis/redis/blob/unstable/src/quicklist.h))
  
   基于listpack(V6.2)
   ```java
   /* quicklist is a 40 byte struct (on 64-bit systems) describing a quicklist.
   * 'count' is the number of total entries.
   * 'len' is the number of quicklist nodes.
   * 'compress' is: 0 if compression disabled, otherwise it's the number
   *                of quicklistNodes to leave uncompressed at ends of quicklist.
   * 'fill' is the user-requested (or default) fill factor.
   * 'bookmarks are an optional feature that is used by realloc this struct,
   *      so that they don't consume memory when not used. */
   typedef struct quicklist {
      quicklistNode *head;
      quicklistNode *tail;
      unsigned long count;        /* total count of all entries in all listpacks */
      unsigned long len;          /* number of quicklistNodes */
      signed int fill : QL_FILL_BITS;       /* fill factor for individual nodes */
      unsigned int compress : QL_COMP_BITS; /* depth of end nodes not to compress;0=off */
      unsigned int bookmark_count: QL_BM_BITS;
      quicklistBookmark bookmarks[];
   } quicklist;
   ```
   基于ziplist
   ```java
   typedef struct quicklist {
      // quicklist头结点
      quicklistNode *head;
      // quicklist 尾节点
      quicklistNode *tail;
      // 所有的ziplist中的entry总数量
      unsigned long count; /* total count of all entries in all ziplists */
      // ziplist节点数量
      unsigned long len;   /* number of quicklistNodes */
      // ziplist中entry的上限，默认为-2，和redis中配置的 list-max-ziplist-size 一致
      int fill : QL_FILL_BITS;  /* fill factor for individual nodes */
      // 首尾节点不压缩的个数，若设置为1，则首尾节点各有一节点不压缩；设置为0，则代表所有节点不压缩
      unsigned int compress : QL_COMP_BITS; /* depth of end nodes not to compress;0=off */
      // 内存重分配时的书签数量及数组，一般用不到
      unsigned int bookmark_count: QL_BM_BITS;
      quicklistBookmark bookmarks[];
   } quicklist;

   ```
- quicklistNode结构

   基于listpack(V6.2)
   ```java
   /* quicklistNode is a 32 byte struct describing a listpack for a quicklist.
   * We use bit fields keep the quicklistNode at 32 bytes.
   * count: 16 bits, max 65536 (max lp bytes is 65k, so max count actually < 32k).
   * encoding: 2 bits, RAW=1, LZF=2.
   * container: 2 bits, PLAIN=1 (a single item as char array), PACKED=2 (listpack with multiple items).
   * recompress: 1 bit, bool, true if node is temporary decompressed for usage.
   * attempted_compress: 1 bit, boolean, used for verifying during testing.
   * dont_compress: 1 bit, boolean, used for preventing compression of entry.
   * extra: 9 bits, free for future use; pads out the remainder of 32 bits */
   typedef struct quicklistNode {
      struct quicklistNode *prev;
      struct quicklistNode *next;
      unsigned char *entry;
      size_t sz;             /* entry size in bytes */
      unsigned int count : 16;     /* count of items in listpack */
      unsigned int encoding : 2;   /* RAW==1 or LZF==2 */
      unsigned int container : 2;  /* PLAIN==1 or PACKED==2 */
      unsigned int recompress : 1; /* was this node previous compressed? */
      unsigned int attempted_compress : 1; /* node can't compress; too small */
      unsigned int dont_compress : 1; /* prevent compression of entry that will be used later */
      unsigned int extra : 9; /* more bits to steal for future usage */
   } quicklistNode;
   ```
   基于ziplist
   ```java
   typedef struct quicklistNode {
      // 前一个节点（ziplist）指针
      struct quicklistNode *prev;
      // 后一个节点（ziplist）指针
      struct quicklistNode *next;
      // 当前节点ziplist指针
      unsigned char *zl;
      // 当前节点ziplist的字节大小，即zlbytes
      unsigned int sz;             /* ziplist size in bytes */
      // 当前节点ziplist中entry的数量
      unsigned int count : 16;     /* count of items in ziplist */
      // 编码方式:1-ziplist; 2-lzf压缩模式
      unsigned int encoding : 2;   /* RAW==1 or LZF==2 */
      // 数据容器类型：1-其他（预留扩展类型）；2-ziplist
      unsigned int container : 2;  /* NONE==1 or ZIPLIST==2 */
      // 是否被压缩：1-说明被解压，将来要重新压缩。
      unsigned int recompress : 1; /* was this node previous compressed? */
      // 测试字段
      unsigned int attempted_compress : 1; /* node can't compress; too small */
      // 预留字段
      unsigned int extra : 10; /* more bits to steal for future usage */
   } quicklistNode;
   ```

- 优缺点

   优点：
   - 快表的节点大小固定，可以有效地避免内存碎片的发生。
   -  快表支持动态增加和删除节点，可以随着数据的增长而自动扩容或缩容，不需要预先分配空间。
   - 快表的节点采用ziplist的紧凑存储方式，使得节点访问和遍历的效率较高。同时，快表支持从头和尾部两个方向同时遍历节点。

   缺点：
   -  快表的节点大小固定，如果节点中的元素数量较少，会造成一定的空间浪费。
   -  快表中的元素只能是整数或字节数组，不支持其他数据类型的存储。
   - 快表的插入和删除操作的效率较低，因为在插入或删除元素时，需要移动后面的元素，可能会导致大量的内存复制操作。如果需要频繁进行插入和删除操作，建议使用其他数据结构，例如链表。
   - 当快表中的元素数量较大时，遍历整个快表的效率也可能较低，因为快表是由多个节点组成的链表，需要依次遍历每个节点才能访问所有元素。


**4. 字典**/哈希表 - Dict
> [redis底层数据结构-Dict](https://axlgrep.github.io/tech/redis-dict.html)
- 概述

Redis 的 Dict 是一个高效的键值对映射数据结构，采用双哈希表实现以支持无锁的渐进式 Rehash，确保扩容或缩容时的高效性能。它通过哈希表节点以链表形式解决哈希冲突，允许快速的查找、插入和删除操作，是实现 Redis 各种数据类型和高级功能的基础架构之一。
- Dict结构

![](/Res/images/Redis-数据结构-底层数据结构-Dict.png)

**`type`**:这是一个指向 dictType 结构的指针，**为该字典提供一套特定的操作函数**。dictType 结构包含了一系列函数指针，用于定义键值对的复制、释放、比较等操作。这使得 dict 结构能够以通用的方式操作不同类型的键和值。

**`privdata`**:这是一个指向任意类型数据的指针，该数据会被传递给 dictType 结构中的各种函数。它允许这些函数拥有一个通用的接口，同时又能进行针对特定情境的操作。

**`ht[0]`** 和 **`ht[1]`**:这是两个 dictht（哈希表）数组，用于存储字典中的元素。Redis 使用一个**渐进式的 rehashing 过程**来扩展或缩小这个数据结构的容量。**通常，所有的操作都在 ht[0] 上进行，当字典需要扩展或缩小时，元素会逐渐从 ht[0] 移动到 ht[1]**，这一过程是**逐渐进行**的，以**避免长时间的阻塞**。

**`rehashidx`**:这是一个标记，用来表示** rehashing 进程的进度**。当不在 rehashing 时，该值为 -1。当开始 rehashing 时，该值会被设置为 0，并随着 rehashing 过程的进行逐渐增加，直到 rehashing 完成，此时再次将该值设置为 -1。

**`iterators`**:这是**正在对字典进行迭代的迭代器的数量**。这个计数有助于管理对字典的迭代，确保即使在 rehashing 过程中也能安全地进行迭代操作。有活跃的迭代器时，可能会暂停 rehashing 过程，以保证迭代器的一致性和准确性。

- Dictht结构

![](/Res/images/Redis-数据结构-底层数据结构-Dict-Dictht.png)
```java
/* This is our hash table structure. Every dictionary has two of this as we
 * implement incremental rehashing, for the old to the new table. */
typedef struct dictht {
    dictEntry **table;      // HashTable数组
    unsigned long size;     // HashTable的大小
    unsigned long sizemask; // HashTable大小掩码,总是等于size - 1, 通常用来计算索引
    unsigned long used;     // 已经使用的节点数,实际上就是HashTable中已经存在的dictEntry数量
} dictht;
```
Redis的字典使用HashTable作为底层实现,一个HashTable中可以保存多个哈希表结点(dictEntry), 而每个dictEntry中就保存着字典中的一个键值对, `table`属性就是一个数组, 数组中每个元素的类型都是指向dictEntry的指针,而`size`属性便是table数组的长度, `sizemask`总是等于size - 1, 用于计算索引信息, `used`属性记录当前HashTable中dictEntry的总数量

**`table`**:这是一个数组，具体类型是指向字典条目（dictEntry）指针的数组。每一个 dictEntry 包含了键值对信息。这个 table 是哈希表的本质存储结构，实际的数据（键值对）都存储在这里。

**`size`**:这个成员表示哈希表中 table 数组的大小，也就是可以容纳的 dictEntry 数目。它总是2的幂次，这是为了使用位掩码（sizemask）与运算来替代取模运算，提升计算效率。

**`sizemask`**:它是与 size 成员配合使用的位掩码。它总是等于 size - 1。当计算一个键的哈希值来确定其在 table 数组中的位置时，用哈希值对 size 取模，等价于与 sizemask 进行按位与运算。后者由于只涉及二进制位运算，因此效率更高。

**`used`**:代表哈希表中已有的元素数量，即实际已经使用的 dictEntry 数目。这个数值在添加或删除键值对时会相应地增加或减少。

- DictEntry结构

![](/Res/images/Redis-数据结构-底层数据结构-Dict-DictEntry.png)
```java
struct dictEntry {
    void *key;
    union {
        void *val;
        uint64_t u64;
        int64_t s64;
        double d;
    } v;
    struct dictEntry *next;     /* Next entry in the same hash bucket. */
};
typedef struct {
    void *key;
    dictEntry *next;
} dictEntryNoValue;
```
dictEntry是Dictht中结点的表现形式, 每个dictEntry都保存着一个键值对, key属性指向键值对的键对象, 而v属性则保存着键值对的值, **Redis采用了联合体来定义v**, 使键值对的值既可以存储一个指针, 也可以存储有符号/无符号整形数据,甚至可以存储浮点形数据, Redis使用联合体的形式来存储键值对的值可以让内存使用更加精细灵活, 另外, 既然是HashTable, 不可避免会发生两个键不同但是计算出来存放索引相同的情况, **为了解决Hash冲突的问题, dictEntry还有一个next属性, 用来指向与当前dictEntry在同一个索引的下一个dictEntry.**

- 核心特性

==哈希算法== 
   
   Redis计算哈希值和索引值方法如下：
  ```java
   #1、使用字典设置的哈希函数，计算键 key 的哈希值
   hash = dict->type->hashFunction(key);

   #2、使用哈希表的sizemask属性和第一步得到的哈希值，计算索引值
   index = hash & dict->ht[x].sizemask;
  ``` 
==解决哈希冲突==

   当两个或更多的键被哈希到同一位置时（即发生哈希冲突），Redis 通过**链地址法**来解决冲突，即在这个位置维护一个链表，所有哈希到这个位置的键值对都会被加入到这个链表中。

==自动扩容和缩容==

可以预见的是,随着我们不断的对HashTable进行操作,可能会发生以下两种情况:

   -  不断的向HashTable中添加数据,HashTable中每个索引上的dictEntry数量会越来越多,也就是单链表会越来越长,这会十分影响字典的查询效率(最坏的场景可能要把整个单链表遍历完毕才能确定一个Key对应的dictEntry是否存在), 而Redis通常被当做缓存,这种低性能的场景是不被容许的.

   - 向一个本身已经十分巨大的HashTable执行删除节点的操作,由于原先这个HashTable的size很大(也就是说table数组十分巨大,我们假设size为M), 但是执行了大量的删除操作之后,table数组中很多元素指向了NULL(由于对应的索引上已经没有任何dictEntry, 相当于一个空的单链表), HashTable中剩余结点我们假设为N,这时M远大于N,也就是说之上table数组中至少有M - N个元素指向了NULL,这是对内存空间的巨大浪费,而Redis是内存型数据库,这种浪费内存的场景也是不被容许的.

针对以上两种场景,**为了让HashTable的负载因子(HashTable中所有dictEntry的数量/HashTable的size值)维持在一个合理的范围内**,Redis在HashTable保存的dictEntry数量太多或者太少的时候,会对HashTable的大小进行扩展或者收缩,在没有执行Rehash操作时,字典的所有数据都存储在ht[0]所指向的HashTable中,而在Rehash操作过程中,Redis会创建一个新的HashTable, 并且令ht[1]指向它,然后逐步的将ht[0]指向的HashTable的数据迁移到ht[1]上来

**判断扩展HashTable的逻辑**：
```java
/* Expand the hash table if needed */
static int _dictExpandIfNeeded(dict *d)
{
   /* Incremental rehashing already in progress. Return. */
   if (dictIsRehashing(d)) return DICT_OK;

   /* If the hash table is empty expand it to the initial size. */
   if (d->ht[0].size == 0) return dictExpand(d, DICT_HT_INITIAL_SIZE);

   /* If we reached the 1:1 ratio, and we are allowed to resize the hash
   * table (global setting) or we should avoid it but the ratio between
   * elements/buckets is over the "safe" threshold, we resize doubling
   * the number of buckets. */
   if (d->ht[0].used >= d->ht[0].size &&
      (dict_can_resize ||
         d->ht[0].used/d->ht[0].size > dict_force_resize_ratio))
   {
      return dictExpand(d, d->ht[0].used*2);
   }
   return DICT_OK;
}
```
每次获取一个Key的索引信息时,都会调用上述的_dictExpandIfNeeded(dict *d)方法判断是否需要对当前HashTable执行扩展操作,满足下列任意条件之一,便会执行扩展操作:

- 当前ht[0]所指向的HashTable大小为0
- 服务器目前没有执行BGSAVE或者BGREWRITEAOF操作,并且HashTable的负载因子大于等于1(d->ht[0].used >= d->ht[0].size)
- 服务器目前正在执行BGSAVE或者BGREWRITEAOF操作,并且HashTable的负载因子大于5(dict_force_resize_ratio = 5)

**判断收缩HashTable的逻辑**：
```java
int htNeedsResize(dict *dict) {
   long long size, used;

   size = dictSlots(dict);
   used = dictSize(dict);
   return (size > DICT_HT_INITIAL_SIZE &&
            (used*100/size < HASHTABLE_MIN_FILL));
}

/* Resize the table to the minimal size that contains all the elements,
* but with the invariant of a USED/BUCKETS ratio near to <= 1 */
int dictResize(dict *d)
{
   int minimal;

   if (!dict_can_resize || dictIsRehashing(d)) return DICT_ERR;
   minimal = d->ht[0].used;
   if (minimal < DICT_HT_INITIAL_SIZE)
      minimal = DICT_HT_INITIAL_SIZE;
   return dictExpand(d, minimal);
}
```
Redis会定期的检查数据库字典HashTable的状态,当HashTable的负载因子小于0.1时,会自动的对HashTable执行收缩操作

 ==rehash过程==
  
  - **分配空间**：为ht[1]指向的HashTable分配空间, 分配空间的大小取决于要执行的操作,以及ht[0]所指向HashTable中dictEntry结点的数量, 也就是ht[0].used中记录的值:
    - 如果当前执行的是扩展操作,那么新HashTable的大小为第一个大于等于2 * ht[0].used的2的n次方幂
    - 如果当前执行的是收缩操作,那么新HashTable的大小为第一个大于等于ht[0].used的2的n次方幂
   - **渐进式rehash**：将ht[0]所指向HashTable中的所有dictEntry节点都迁移到ht[1]所指向的新HashTable上(由于两个HashTable的size不同,所以在迁移过程中要重新计算dictEntry的索引,这也就是rehash的关键所在)
   - **迁移完成**：当迁移完成之后,ht[0]所指向的HashTable中已经没有任何节点,释放该HashTable, 并且令ht[0]指向迁入节点的新HashTable, 最后为ht[1]创建一个空白的HashTable,为下一次rehash做准备

![](/Res/images/Redis-数据结构-底层数据结构-Dict-rehash.png)
说明：可以看到当前rehashidx指向ht[0]的索引2,这说明ht[0]的[0, rehashidx - 1]对应buckets上的dictEntry都已经迁移完毕,另外我们发现正在执行渐进式rehash字典中的数据一部分在ht[0]中,而另一部分在ht[1]中,所以在渐进式rehash执行期间,字典的删除,查找以及更新操作,都会在两个HashTable上执行(先尝试在ht[0]上执行操作,如果没有成功,则再尝试在ht[1]上进行操作),而在渐进式rehash执行期间,如果我们需要往字典中添加新的结点,则会一律添加到ht[1]上,这样可以保证ht[0]上的结点只减不增(也就是已经迁移过的buckets不会再出现新的结点)

==渐进式rehash==

   为了避免rehash过程阻塞服务器，Redis采用渐进式rehash策略。在进行rehash期间，所有的读写操作都会同时在ht[0]和ht[1]上进行，并逐步将ht[0]上的键值对迁移到ht[1]。
   
==rehash触发条件==
   - 负载因子过高(扩容): 
     - 当前ht[0]所指向的HashTable大小为0
     - 服务器目前没有执行BGSAVE或者BGREWRITEAOF操作,并且HashTable的负载因子大于等于1(d->ht[0].used >= d->ht[0].size)
     - 服务器目前正在执行BGSAVE或者BGREWRITEAOF操作,并且HashTable的负载因子大于5(dict_force_resize_ratio = 5) 
   - 负载因子过低(缩容): 
     - Redis会定期的检查数据库字典HashTable的状态,当HashTable的负载因子小于0.1时,会自动的对HashTable执行收缩操作

==Dict优缺点==

 Dict 提供了快速的键值对查找和插入性能，能高效地支持数据的增删改查操作；然而，在内存使用上它可能不如一些更紧凑的数据结构高效，尤其是在存储大量小对象时，还有当哈希表发生扩容或收缩时可能会有短暂的性能下降。


**5. 整数集** - IntSet

==概述==

IntSet是一个存储整数值的集合，内部使用**有序、无重复**的数组保存数据。
优点:节省内存空间。
高效的查找、插入和删除操作。
使用场景: 在集合键只包含整数值且数量较少时使用。

==IntSet结构==

```java
typedef struct intset {
    uint32_t encoding;
    uint32_t length;
    int8_t contents[];
} intset;

#define INTSET_ENC_INT16 (sizeof(int16_t))
#define INTSET_ENC_INT32 (sizeof(int32_t))
#define INTSET_ENC_INT64 (sizeof(int64_t))
```

**`encoding`**: 数据编码，表示intset中的每个数据元素用几个字节来存储。它有三种可能的取值：`INTSET_ENC_INT16`表示每个元素用2个字节存储，`INTSET_ENC_INT32`表示每个元素用4个字节存储，`INTSET_ENC_INT64`表示每个元素用8个字节存储。因此，intset中存储的整数最多只能占用64bit。

**`length`**: 表示intset中的元素个数。encoding和length两个字段构成了intset的头部（header）。

**`contents`**: 是一个柔性数组（flexible array member），表示intset的header后面紧跟着数据元素。这个数组的总长度（即总字节数）等于encoding * length。柔性数组在Redis的很多数据结构的定义中都出现过（例如sds, quicklist, skiplist），用于表达一个偏移量。contents需要单独为其分配空间，这部分内存不包含在intset结构当中

![](/Res/images/Redis-数据结构-底层数据结构-IntSet.png)

说明：
- 新创建的intset只有一个header，总共8个字节。其中`encoding` = 2, `length` = 0。
- 添加13, 5两个元素之后，因为它们是比较小的整数，都能使用2个字节表示，所以`encoding`不变，值还是2。
- 当添加32768的时候，它不再能用2个字节来表示了（2个字节能表达的数据范围是-215~215-1，而32768等于215，超出范围了），因此`encoding`必须升级到INTSET_ENC_INT32（值为4），即用4个字节表示一个元素。
- 在添加每个元素的过程中，intset**始终保持从小到大有序**。
- 与ziplist类似，intset也是按小端（little endian）模式存储的（参见维基百科词条Endianness）。比如，在上图中intset添加完所有数据之后，表示`encoding`字段的4个字节应该解释成0x00000004，而第5个数据应该解释成0x000186A0 = 100000。

intset与ziplist相比：
- ziplist可以存储任意二进制串，而intset只能存储整数。
- ziplist是无序的，而intset是从小到大有序的。因此，在ziplist上查找只能遍历，而在intset上可以进行二分查找，性能更高。
- ziplist可以对每个数据项进行不同的变长编码（每个数据项前面都有数据长度字段len），而intset只能整体使用一个统一的编码（encoding）。

==自动升级==

当在一个int16类型的整数集合中插入一个int32类型的值，整个集合的所有元素都会转换成32类型。 整个过程有三步：

- 根据新元素的类型（比如int32），扩展整数集合底层数组的空间大小，并为新元素分配空间。
- 将底层数组现有的所有元素都转换成与新元素相同的类型， 并将类型转换后的元素放置到正确的位上， 而且在放置元素的过程中， 需要继续维持底层数组的有序性质不变。
- 最后改变encoding的值，length+1。


**6. 跳表** - ZSkipList   

==概述==

ZSkipList是一种**有序**数据结构，支持平均 O(logN) 复杂度的查找、插入和删除操作。优点:高效的范围查询。使用场景: 存储有序集合数据，例如 Sorted Set 类型。

==ZSkipList结构==
```java
/* ZSETs use a specialized version of Skiplists */
typedef struct zskiplistNode {
    sds ele;
    double score;
    struct zskiplistNode *backward;
    struct zskiplistLevel {
        struct zskiplistNode *forward;
        unsigned int span;
    } level[];
} zskiplistNode;

typedef struct zskiplist {
    struct zskiplistNode *header, *tail;
    unsigned long length;
    int level;
} zskiplist;
```
![](/Res/images/Redis-数据结构-底层数据结构-ZSkipList.png)

ZSkipList:
**`header`**:header 是指向zskiplist的第一个节点（也就是跳表的头节点）的指针。它本质上是zskiplist的入口点，所有的操作（插入、查找、删除等）都是从这个节点开始的。header节点包含了多个指向不同层次第一个实际节点的指针，这些层级的指针帮助实现跳表的快速访问特性。
**`tail`**:tail 指向zskiplist中的最后一个节点。
**`length`**:length 表示zskiplist中节点的总数（不包括header节点）。这个数量随着节点的插入和删除动态变化。通过length可以快速获取到跳表的大小，无需遍历整个结构。
**`level`**:level 表示当前zskiplist中最高层级的层数。在跳表中，层级是动态管理的，每次插入新节点时，都会通过一定的随机化算法为其分配一个层数，level保留的是所有节点层数中的最大值。这个值允许zskiplist动态调整自身的索引结构，以适应不同的数据变化情形，同时保持操作的效率。

ZSkipListNode:
**`ele`**:ele 是存储在此节点中的实际值，即有序集合的成员。在Redis中，它通常是一个字符串。
**`score`**:score是一个浮点数，跟每个元素（即`ele`）相关联，表示元素在有序集合中的排序分数。按照score从小到大的顺序来为元素排序，同样的score值不会影响元素的位置。
**`backward`**:backward 是指向此节点在最低层的前一个节点的指针。这有助于实现从后向前的导航，并可支持有序集合的逆向查询。
**`level[]`**:level是一个数组，数组的元素是代表每一层的结构体。每一个结构体包含两个属性：forward和span。
   - `forward`:forward是一个指针，指向当前节点在这一层的下一个节点。
   - `span`:span表示当前节点到forward指针所指向的节点之间的间距， 紧邻的两个结点之间的距离定义为1。

![](/Res/images/Redis-数据结构-底层数据结构-ZSkipList搜索路径.png)


==和平衡树和哈希表的对比==

- **有序性**：Skip List和平衡树支持元素的有序排列，而哈希表不支持。有序性使得Skip List和平衡树适合于范围查找操作，而哈希表则适用于单个键的快速查找。
- **范围查找**：范围查找在Skip List中实现相当直观和高效，仅需线性遍历链表即可完成。相较之下，平衡树实现范围查找较为复杂，可能需要进行中序遍历来访问指定范围的所有节点。
- **插入和删除操作**：平衡树的插入和删除操作可能导致树结构调整，以保持树的平衡性，逻辑较为复杂。而Skip List在插入和删除时仅需调整相邻节点的指针，操作较为简单且效率较高。
- **内存占用**：Skip List的内存占用相对于平衡树更为灵活，其每个节点包含的指针数量平均为1/(1-p)，具体取决于参数p的大小。相对地，平衡树的每个节点通常包含两个指针（分别指向左右子树）。
- **查找性能**：对于单个键值的查找，Skip List和平衡树的时间复杂度大致相同，都是O(log n)。而哈希表的查找性能在理想情况下接近O(1)，通常更高效。
- **实现难度**：从算法实现上来看，Skip List的实现比平衡树简单许多，这对于开发者来说是一个重要的优势。

### 6.redis核心知识
#### 6.1 持久化
#### 6.2 订阅/发布
#### 6.3 事件机制
#### 6.4 事务

### 7.高可用&可拓展
#### 7.1 redis集群之如何配置主从复制模式
#### 7.2 redis集群之哨兵模式
#### 7.3 redis分片技术

### 8.redis应用实践
#### 8.1 redis在jedis中如何使用和操作
#### 8.2 springboot整合redis
#### 8.3 redis缓存问题
#### 8.4 性能调优

## 二、八股问题整理
### redis概述
#### (1).为什么要用缓存
使用缓存的目的就是**提升读写性能**。而实际业务场景下，更多的是为了提升读性能，带来更好的性能，**带来更高的并发量**。 Redis 的读写性能比 Mysql 好的多，我们就可以把 Mysql 中的热点数据缓存到 Redis 中，提升读取性能，同时也减轻了 Mysql 的读取压力。
#### (2).什么是redis
Redis是一款内存**高速缓存数据库**。Redis全称为：Remote Dictionary Server（远程数据服务），使用C语言编写，Redis是一个`key-value`存储系统（键值存储系统），支持丰富的数据类型，如：String、list、set、zset、hash。

Redis是一种支持key-value等多种数据结构的存储系统。可用于缓存，事件发布或订阅，高速队列等场景。支持网络，提供字符串，哈希，列表，队列，集合结构直接存取，基于内存，可持久化。

#### (3).为什么使用redis
- 读写性能优异
- 数据类型丰富
- 原子性
- 多种特性，持久化、订阅、发布、事务、主从复制、哨兵、分片

#### (4).redis为什么快
1. **内存存储**：Redis是一个基于内存的数据存储系统，所有的数据操作都是直接在内存中进行的，相对于传统的磁盘存储，内存的读写速度要快得多。
2. **数据结构简单**：Redis支持的数据结构相对简单（如字符串、列表、集合等），这些数据结构的操作都非常高效。
3. **单线程模型**：Redis采用单线程模型来处理客户端的请求，这样可以避免线程切换和竞争状态的开销，尽管是单线程，但由于大多数操作都是内存操作，其性能依旧非常高。
4. **非阻塞I/O**：Redis使用了非阻塞的网络I/O模型，例如epoll作为I/O多路复用技术的一部分，这使得Redis可以高效地处理多个客户端的连接和请求。
5. **优化的数据操作算法**：Redis针对其支持的数据结构和操作进行了高度优化，许多操作的时间复杂度都很低（例如O(1)或O(log n)）。
6. **持久化策略**：尽管是基于内存的系统，Redis也提供了RDB和AOF两种持久化方式，这些持久化操作是以非阻塞方式执行的，不会影响主线程的性能。
7. **支持管道化**（Pipelining）：客户端可以一次性发送多个命令到服务器，减少网络延迟，服务器处理完这些命令后，一次性将结果返回给客户端。
8. **发布/订阅模式**：提供了有效的消息广播/订阅支持，通过有效的内存数据分发，加快了消息传递的速度。

综上所述，通过**内存操作、简单的数据结构、单线程模型，以及高效的网络I/O处理等技术，Redis能够提供极快的数据读写性能**，这使其成为在需要高速数据存取场景下的首选数据库。
#### (5).哪些场景下使用redis
1. **热点数据的缓存**

缓存是Redis最常见的应用场景，之所有这么使用，主要是因为Redis读写性能优异。而且逐渐有取代memcached，成为首选服务端缓存的组件。而且，Redis内部是支持事务的，在使用时候能有效保证数据的一致性。

作为缓存使用时，一般有两种方式保存数据：
- 读取前，先去读Redis，如果没有数据，读取数据库，将数据拉入Redis。
- 插入数据时，同时写入Redis。

方案一：实施起来简单，但是有两个需要注意的地方：
  - 避免缓存击穿。（数据库没有就需要命中的数据，导致Redis一直没有数据，而一直命中数据库。）
  - 数据的实时性相对会差一点。

方案二：数据实时性强，但是开发时不便于统一处理。当然，两种方式根据实际情况来适用。

如：方案一适用于对于数据实时性要求不是特别高的场景。方案二适用于字典表、数据量不大的数据存储。

2. **限时业务的运用**

redis中可以使用expire命令设置一个键的生存时间，到时间后redis会删除它。利用这一特性可以运用在限时的优惠活动信息、手机验证码等业务场景。

3. **计数器相关问题**

redis由于incrby命令可以实现原子性的递增，所以可以运用于高并发的秒杀活动、分布式序列号的生成、具体业务还体现在比如限制一个手机号发多少条短信、一个接口一分钟限制多少请求、一个接口一天限制调用多少次等等。

4. **分布式锁**

这个主要利用redis的setnx命令进行，setnx："set if not exists"就是如果不存在则成功设置缓存同时返回1，否则返回0 ，这个特性在很多后台中都有所运用，因为我们服务器是集群的，定时任务可能在两台机器上都会运行，所以在定时任务中首先 通过setnx设置一个lock， 如果成功设置则执行，如果没有成功设置，则表明该定时任务已执行。 当然结合具体业务，我们可以给这个lock加一个过期时间，比如说30分钟执行一次的定时任务，那么这个过期时间设置为小于30分钟的一个时间就可以，这个与定时任务的周期以及定时任务执行消耗时间相关。

在分布式锁的场景中，主要用在比如秒杀系统等。

5. **延时操作**

比如在订单生产后我们占用了库存，10分钟后去检验用户是否真正购买，如果没有购买将该单据设置无效，同时还原库存。 由于redis自2.8.0之后版本提供Keyspace Notifications功能，允许客户订阅Pub/Sub频道，以便以某种方式接收影响Redis数据集的事件。 所以我们对于上面的需求就可以用以下解决方案，我们在订单生产时，设置一个key，同时设置10分钟后过期， 我们在后台实现一个监听器，监听key的实效，监听到key失效时将后续逻辑加上。当然我们**也可以利用rabbitmq、activemq等消息中间件的延迟队列服务**实现该需求。

6. **排行榜相关问题**

关系型数据库在排行榜方面查询速度普遍偏慢，所以可以借助redis的`SortedSet`进行热点数据的排序。比如点赞排行榜，**做一个SortedSet, 然后以用户的openid作为上面的username, 以用户的点赞数作为上面的score, 然后针对每个用户做一个hash, 通过zrangebyscore就可以按照点赞数获取排行榜，然后再根据username获取用户的hash信息**，这个当时在实际运用中性能体验也蛮不错的。

7. **点赞、好友等相互关系的存储**
   
Redis 利用集合的一些命令，比如求交集、并集、差集等。在微博应用中，每个用户关注的人存在一个集合中，就很容易实现求两个人的共同好友功能。 

8. **简单队列**
   
由于Redis有`list push`和`list pop`这样的命令，所以能够很方便的执行队列操作
#### (6).为什么使用Redis而不是用Memcache呢？
![](/Res/images/Redis-memcachedVSredis.jpg)
- 数据类型的支持：Redis 提供了**丰富的数据类型**，如字符串（Strings）、列表（Lists）、集合（Sets）、有序集合（Sorted Sets）以及哈希（Hashes），而 Memcached 主要支持简单的键值对。
- 持久化：Redis 支持**数据的持久化**，可以将内存中的数据保存到磁盘中，通过此特性可以在服务器重启后恢复数据。Memcached 不支持持久化，它纯粹是一个内存缓存服务，重启后数据将丢失。
- 高可用和分片：Redis 提供了 Sentinel 和 Redis Cluster 来支持**高可用和分片**，以实现故障转移和数据的自动分片，从而提高系统的可扩展性。Memcached 的分布式主要依赖客户端来实现，它本身不提供这种原生支持。
- 事务：Redis **支持事务**功能，允许通过 MULTI、EXEC、WATCH 等命令来执行一组命令的原子操作。Memcached 不支持事务。
- 内存管理机制：Redis 提供了更为**丰富的内部数据管理和存储的数据结构**；而 Memcached 使用简单的 LRU（最近最少使用）机制来管理内存中的对象。
- 发布/订阅模型：Redis **支持发布/订阅消息队列模型**，允许客户端之间进行消息的推送和订阅，而 Memcached 不支持此功能。

#### (7).为什么Redis单线程模型效率也能那么高？
- **纯内存操作**：Redis 的所有数据都存储在内存中，避开了磁盘 I/O 等操作的慢速，而且内存的读写速度非常快，从而大大提高了速度。
- **高效的数据结构**：Redis 使用了高效的数据结构（如字符串（Strings）、列表（Lists）、集合（Sets）、有序集合（Sorted Sets）和哈希（Hashes）等），设计适当的算法可以使得操作的时间复杂度降低，在处理请求时非常高效。
- **非阻塞 I/O**：Redis 使用了非阻塞的 I/O 操作，能同时处理多个连接和请求，从而保证了游离时间的利用以及高吞吐量。
- **单线程避免了并发问题**：由于 Redis 是单线程的，所以在它的设计中完全避免了常见的多线程编程的问题，如线程切换的系统开销以及死锁等，并发冲突也不存在。
- **简单的操作**：在 Redis 中，大部分命令只是对内存操作，并且还可以减少系统上下文切换以及锁的争抢。
### 3.redis常用命令

### 4.redis配置文件详解

### 5.redis数据结构


### 6.redis核心知识


### 7.高可用&可拓展
主从复制，哨兵，分片

### 8.redis应用实践



6，说说Redis的线程模型
7，为什么 Redis 需要把所有数据放到内存中？
8，Redis 的同步机制了解是什么？
9， pipeline 有什么好处，为什么要用 pipeline？
10，说一下Redis有什么优点和缺点
11Redis缓存刷新策略有哪些？
12，Redis持久化方式有哪些？以及有什么区别？
13，持久化有两种，那应该怎么选择呢？
14，怎么使用 Redis 实现消息队列？
15，说说你对Redis事务的理解
16，Redis 为什么设计成单线程的？
17，什么是 bigkey？会存在什么影响？
18，熟悉哪些Redis集群模式？
19，是否使用过 Redis Cluster 集群，集群的原理是什么？
20，Redis Cluster集群方案什么情况下会导致整个集群不可用？
21，Redis 集群架构模式有哪几种？
22，说说 Redis 哈希槽的概念？
23，Redis 常见性能问题和解决方案有哪些？
24，假如 Redis 里面有 1 亿个 key，其中有 10w 个 key 是以某个固定的已知的前缀开头的，如果将它们全部找出来？
25，如果有大量的 key 需要设置同一时间过期，一般需要注意什么？
26，什么情况下可能会导致 Redis 阻塞？
27，缓存和数据库谁先更新呢？
28，怎么提高缓存命中率？
29，Redis 如何解决 key 冲突？
30，Redis 报内存不足怎么处理？
31、说说Redis持久化机制
32、缓存雪崩、缓存穿透、缓存预热、缓存更新、缓存降级等问题
33、热点数据和冷数据是什么

35、单线程的redis为什么这么快
36、redis的数据类型，以及每种数据类型的使用场景
37、redis的过期策略以及内存淘汰机制
38、Redis 为什么是单线程的
39、Redis 常见性能问题和解决方案？
40、为什么Redis的操作是原子性的，怎么保证原子性的？
41、了解Redis的事务吗？
42、Redis 的数据类型及使用场景

