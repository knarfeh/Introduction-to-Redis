# Redis

An open source(BSD licensed), in-memory data structure store, used as a database, cache and message broker  

[redis.io](https://redis.io/)

Note:
今天跟大家简单介绍一下 Redis。一句话介绍 Redis，redis 是一个开源的，键值对存储系统，你可以把它当做数据库，缓存系统，消息队列来用。Redis 最早是一个个人项目，08的时候， 由一个意大利人开发，叫做 Salvatore Sanfilippo...(不会读，就叫 antirez 吧) ，09年开源，到现在也不过7，8年的时间，现在基本是 NoSQL 中最流行的数据库了。12年的时候 hacker news 上有一份关于数据库的调查，有12%的技术公司在用 redis。国内，新浪博客，知乎等，国外有 github，stack overflow，flickr，暴雪，instagram 都是 redis 的用户。后来 VMware 开始赞助 Redis，Redis 开发者们也先后加入了 VMware，全职开发 redis。现在开发还是很活跃的，两周前，antirez 写了一篇博客说 redis 4.0 就快出了。

为什么 redis 会越来越火，我觉得它代表了未来的趋势，磁盘变成以前的磁带，内存会变成现在的磁盘，说了这么多，redis 到底是什么？redis 的作者写过一篇博客来说明 redis 的理念。

%%%

## [Redis manifesto](http://oldblog.antirez.com/post/redis-manifesto.html)

1. A DSL for Abstract Data Types.
2. Memory storage is #1.
3. Fundamental data structures for a fundamental API.
4. Code is like a poem.
5. We're against complexity.
6. Two levels of API.
7. We optimize for joy.

Note:
这篇宣言其实也放到了 redis 源代码的 README 里面，这篇文章大致是说：作者收到一些 PR，解释为什么 redis 要这么做而不是那么做感到很厌烦，所以他总结了一份 redis 宣言。   
第1点，antirez 说 Redis 是一种实现了 TCP 协议的，操作抽闲数据类型的守护进程，本质上是 DSL，他还谈到 Redis 定义的数据结构非常重要，不仅仅与 Redis 的数据类型相关联，还关系着整个系统的时间复杂度和空间复杂度。有一本书叫Redis 设计与实现，第一部分就是数据结构和对象，花了7章，分别从简单动态字符串，链表，字典，跳跃表，整数集合，压缩列表，对象来进行剖析。
第2点，内存存储是第一位的。因为内存操作很快，保证了 Redis 的性能。Redis 会尝试其他的存储方式，实现持久化等，但主线剧情一定在内存上。  
第3点：为最基本的 API 提供最基本的数据结构，Redis 会避免直接与 API 层接触。『Keep It Simple, Stupid』  
第4点：代码像诗一样。我们乐于使用其他的库，但是在 Redis 这个小故事里，我们想尽可能地自己执笔。『Keep It Simple, Stupid』
第5点：我们讨厌复杂。设计一个系统的过程就是跟复杂做斗争的过程。一个 feature 最好在1000行以内，保持简单的方式是直接不去考虑这个 feature. 『Keep It Simple, Stupid』
第6点：有两种层次的 API，一种是为了分布式 Redis 而诞生的，一种是为了支持多键值而诞生的（更复杂）
第7点：把优化性能作为一种爱好
从这篇文章，我们可以了解 Redis 的内核，理念是怎么样的，那么 redis 到底是什么，它有什么特点

%%% 

## What is redis
* in-memory key/value store
* and it also has very cool value types. each type:
  * strings
  * hashes
  * lists
  * sets
  * sorted sets
* OSS: very helpful and friendly community.Development is very active and responsive to requests
* sponsored by VMWare
* used in the real world
* used heavily as front-end database, search...

Note:
最被人熟知的是，他是一个 key-value 存储的数据库。其实 redis 不只是 key-value 的存储，它是一个简单而强大的基于内存的数据库，基本类型包括字符串，散列，列表，集合，有序集合。而且，redis 是开源的，有 VMware 背书，一直都在稳定的开发中，应用广泛，国内有新浪博客，知乎等，国外有 github，stack overflow，flickr，暴雪，instagram 等等。

%%%

## Why redis

* all data is in memory(data can be optionally stored on disk)
* handles huge workloads easily
* mostly O(1) behavior
* ideal for write-heavy workloads
* support for atomic operations
* support for transactions
* has pub/sub functionality
* tons of client libraries for all major languages
* single thread, uses aync. IO
* internal scripting with LUA

Note:
简单来说，因为 redis 简单而且强大，和 redis 的功能类似的还有 Memcached，Redis 是单线程的模型，而 Memcached 支持多线程，所以对于多核的单例来说，Memecached 的性能会好一点，但是 redi 的性能已经足够好了，在绝大多数情况下不会有性能瓶颈，除非是数据量太大，IO 会花费一些时间。而 Redis 3.0 推出之后，Memcached 几乎所以的功能都成了 redis 的子集，而且 redis 自带了集群功能，虽然还不够成熟。基本上 redis 的性能是非常好的，大多数操作的时间复杂度是常数，支持原子操作，支持事务，发布/订阅的功能，几乎所以流行的语言都有客户端，支持 lua 语言脚本

%%%
![compare](http://7xi5vu.com1.z0.glb.clouddn.com/2016-12-18/redis_memcached_mysql.png)

Note:
这张图是 Redis 和其他数据库的对比，基本上，Redis 3.0 推出之后，Memcached 几乎所以的功能都成了 redis 的子集，而且 redis 自带了集群功能，虽然还不够成熟。基本上 redis 的性能是非常好的，大多数操作的时间复杂度是常数，支持原子操作，支持事务，发布/订阅的功能，几乎所以流行的语言都有客户端，支持 lua 语言脚本
。

%%%

## Redis tutorial

Note:
先简单地尝试一下 Redis

%%%

### Data types

> Redis is not plain key-value store, actually it is a data structures server, supoerting different kind of values.

Note:
上面有谈到，Redis 有5种基本类型，字符串，列表，散列，集合，有序集合。Redis 可以存储键和5种不同数据类型之间的映射。最简单的，也最基本的是字符串：

%%%

#### Strings

get/sets, nothing fancy. keys are strings, anything goes
```
$ redis-cli
127.0.0.1:6379> set hello "world"
OK
127.0.0.1:6379> get hello
"world"
127.0.0.1:6379> del hello
(integer) 1
127.0.0.1:6379> get hello
(nil)
```

%%%

you can atomically increment numbers
```
127.0.0.1:6379> set secret 42
OK
127.0.0.1:6379> incrby secret 100
(integer) 142
```

getting multiple values at once
```
127.0.0.1:6379> mget hello secret
1) (nil)
2) "142"
```

Note:
我们可以使用 redis 自带的客户端来实验一下。这里有几个简单的例子，set 可以设置存储在给定键中的值，get 可以获取给定键中的值，del 删除存储在给定键中的值，可以通过 mget 获得多个键的值。

%%%

Atomic Operations：

```
127.0.0.1:6379> set foo bar
OK
127.0.0.1:6379> getset foo baz
"bar"
127.0.0.1:6379> get foo
"baz"
```

```
127.0.0.1:6379> setnx foo bar
(integer) 1
127.0.0.1:6379> setnx foo baz
(integer) 0
127.0.0.1:6379> get foo
"bar"
```

Note:
一般的组合命令在 redis 中已经有原生的实现了，可以直接用这些原子操作。比如 get 了一个值，set 一个新的值；如果某个键不存在，才进行 set 操作(setnx 是 Set if Not eXists)

%%%

#### Lists

rpush, lrange, lindex, lpop 
```
127.0.0.1:6379> rpush list-key item
(integer) 1
127.0.0.1:6379> rpush list-key item1
(integer) 2
127.0.0.1:6379> rpush list-key item2
(integer) 3
127.0.0.1:6379> lrange list-key 0 -1
1) "item"
2) "item1"
3) "item2"
127.0.0.1:6379> lindex list-key 1
"item1"
127.0.0.1:6379> lpop list-key
"item"
127.0.0.1:6379> lrange list-key 0 -1
1) "item1"
2) "item2"
127.0.0.1:6379> RPOP list-key
"item2"
127.0.0.1:6379> lrange list-key 0 -1
1) "item1"
```

Note:
redis 支持链表的操作，这里有几个比较典型的命令，你可以在左右两端进行 push，pop 的操作，时间复杂度是O(n)

%%%

#### Sets

```
127.0.0.1:6379> sadd set-key item
(integer) 1
127.0.0.1:6379> sadd set-key item1
(integer) 1
127.0.0.1:6379> sadd set-key item2
(integer) 1
127.0.0.1:6379> sadd set-key item
(integer) 0
127.0.0.1:6379> smembers set-key
1) "item"
2) "item2"
3) "item1"
127.0.0.1:6379> sismember set-key item3
(integer) 0
127.0.0.1:6379> sismember set-key item
(integer) 1
127.0.0.1:6379> srem set-key item1
(integer) 1
127.0.0.1:6379> srem set-key item1
(integer) 0
127.0.0.1:6379> smembers set-key
1) "item"
2) "item2"
```

Note:
Redis 的集合和列表都可以存储多个字符串，它们之间的不同在于，列表可以存储多个相同的字符串，而集合通过散列表来保证自己存储的每个字符串都是各不相同的。集合采用的是无序方式存储元素。smembers 可以获取包含所有元素的集合，使用 srem 移除元素时，会返回被移除元素的数量。

%%% 

#### Hashes

```
127.0.0.1:6379> hset hash-key sub-key0 value0
(integer) 1
127.0.0.1:6379> hset hash-key sub-key1 value1
(integer) 1
127.0.0.1:6379> hset hash-key sub-key2 value2
(integer) 1
127.0.0.1:6379> hgetall hash-key
1) "sub-key0"
2) "value0"
3) "sub-key1"
4) "value1"
5) "sub-key2"
6) "value2"
127.0.0.1:6379> hdel hash-key sub-key1
(integer) 1
127.0.0.1:6379> hgetall hash-key
1) "sub-key0"
2) "value0"
3) "sub-key2"
4) "value2"
```

Note:
Redis 的散列可以存储多个键值对之间的映射。散列其实像是一个缩略版的 redis，看起来也像是关系数据库中的行。

%%% 

#### Sorted sets

```
127.0.0.1:6379> zadd zset-key 728 member1
(integer) 1
127.0.0.1:6379> zadd zset-key 729 member0
(integer) 1
127.0.0.1:6379> zadd zset-key 729 member0
(integer) 0
127.0.0.1:6379> zadd zset-key 730 member2
(integer) 1
127.0.0.1:6379> zrange zset-key 0 -1 withscores
1) "member1"
2) "728"
3) "member0"
4) "729"
5) "member2"
6) "730"
127.0.0.1:6379> zrangebyscore zset-key 0 800 withscores
```

Note:
有序集合和散列有点像，都存储键值对，但是有序集合的键叫做成员（member），每个成员都是各不相同的，有序集合的值称为分值，分值必须为浮点数。注意，有序集合的键和值跟集合的键和值的位置是相反的。

%%%

```
1) "member1"
2) "728"
3) "member0"
4) "729"
5) "member2"
6) "730"
127.0.0.1:6379> zrangebyscore zset-key 0 729 withscores
1) "member1"
2) "728"
3) "member0"
4) "729"
127.0.0.1:6379> zrem zset-key member1
(integer) 1
127.0.0.1:6379> zrem zset-key member1
(integer) 0
127.0.0.1:6379> zrange zset-key 0 -1 withscores
1) "member0"
2) "729"
3) "member2"
4) "730"
```


Note:
有序集合和散列有点像，都存储键值对，但是有序集合的键叫做成员（member），每个成员都是各不相同的，有序集合的值称为分值，分值必须为浮点数。注意，有序集合的键和值跟集合的键和值的位置是相反的。了解了有序集合是什么，我们就能想到它能做什么了，比如像 hackernews，twitter 这样的页面，就可以用的。

%%%

## Advanced

%%%

### Expires

```
127.0.0.1:6379> expire foo 20
(integer) 1
127.0.0.1:6379> ttl foo
(integer) 18
127.0.0.1:6379> ttl foo
(integer) 10
127.0.0.1:6379> ttl foo
(integer) 7
127.0.0.1:6379> ttl foo
(integer) -2
```

Note:
我们在实际的开发中经常会遇到一些有时效的数据，比如限时的优惠活动、缓存、验证码等，这些数据可能过了一定时间就要删除。如果是用关系型数据库，需要一个额外的字段来记录到期的时间，定期检测过期的数据，在 redis 中可以用 expire 来设置一个键的过期时间，到时间后 redis 会自动删除

%%%

### Publish/Subscribe

publish

```
127.0.0.1:6379> publish channel1.1 'hello'
(integer) 1
```

subscribe
```
127.0.0.1:6379> subscribe channel1.1
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "channel1.1"
3) (integer) 1
1) "message"
2) "channel1.1"
3) "hello"
```

Note:
Redis 提供了一组命令让开发者实现『发布/订阅』模式，订阅者可以订阅一个或多个频道（可以通过 pattern 进行订阅），发布者可以向指定的频道发送消息，所以订阅该频道的订阅者都会收到此消息。需要注意的是，这里发布的消息是不会持久化的。订阅的复杂度是：O(1)，常数，发布消息的复杂度是 O(n)

%%%

### lua scripts

```
$ cat random_list.lua
--[[
Random lpush a list key-value
KEYS[1]:key name
ARGV[1]:ramdom seed value
ARGV[2]:add element count
--]]

math.randomseed(ARGV[1]);
for i=1, ARGV[2], 1 do
    redis.call("lpush", KEYS[1], math.random());
end
redis.log(redis.LOG_NOTICE, "lpush " .. KEYS[1]);
return true;
$ redis-cli --eval random_list.lua r_list , 3 6
```

```
$ redis-cli lrange r_list 0 -1
1) "0.43370221947957865"
2) "0.54186119723220416"
3) "0.26702763618297298"
4) "0.31170834336043723"
5) "0.86367337352767282"
6) "0.7832349621612742"
```

Note:
Redis 的脚本功能在2.6版推出，它允许开发者写 lua 脚本上传到 redis server 端执行，使用 lua 脚本的好处有：
1. 减少网络开销
2. 原子操作
3. 可以复用脚本

%%%

### Persistence

* RDB
* AOF

Note:
Redis 支持两种方式的持久化，一种是 RDB（Relational database） 方式，一种是 AOF(Append-only file) 方式。简单概括一下就是：RDB 是根据指定的规则『定时』将内存中的数据存储在硬盘上，AOF 是指每次执行完命令后，将命令本身记录下来。

%%%

### RDB

advantages:

* compact, single-file, point-in-time
* very good for disaster recovery
* maximizes Redis performances, just fork a child
* RDB allows faster restarts with big datasets compared to AOF

disadvantages:

* is NOT good if you need to minimize the chance of data loss in case Redis stops working
* Fork() can be time consuming if the dataset is big

Note:
RDB 是一个非常紧凑的文件，它保存了 redis 在某个时间点上的数据集。这个时间点你是可以自己点的，比如每小时，每个月第一天，它非常适用于灾难恢复，父进程在保存 RDB 文件的时候，唯一需要做的是fork 出一个子进程，父进程不需要进行 IO 操作。在数据集比较大的情况下，RDB 的恢复速度比 AOF 的恢复速度要快，缺点是有可能丢数据，因为这是一个完全备份，而不是一个增量备份。 如果发生故障，那么你至少会丢几分钟的数据。还有一个缺点是，每次保存 RDB 的时候，Redis 都需要 fork 出一个进程，如果数据量比较大，CPU 时间比较紧张，fork 本身这个操作可能会比较费时，可能会导致暂时的不响应，

%%%

### AOF

advantages:

* much more durable: you can have different fsync policies
* AOF log is an append only log
* automatically rewrite the AOF in background when it gets too big
* AOF contains a log of all the operations one after the other in an easy to understand and parse format

%%%

disadvantages:

* usually bigger than the equivalent RDB files 
* AOF can be slower than RDB depending on the exact fsync policy
* experienced rare bugs in specific commands 

Note:
使用 AOF 你可以让数据持久化更灵活，比如 fsync 的策略，你可以每秒 sync，或者每次写入命令的时候 sync，默认是每秒 sync，这种配置下，redis 还是可以保持良好的性能，就算发生了宕机，最多丢一秒数据，AOF 文件是一个只进行追加操作的日志文件（append only log），如果磁盘满了，redis-check-aof 工具可以修复这种问题。AOF 文件体积过大时，可以自动在后台对 AOF 文件重写。重写后的新 AOF 文件包含了恢复当前数据集所需的最小命令集合，因为是最小集合，所以容易被人读懂，容易被解析。缺点是，AOF 的体积一般要比 RDB 的体积大，如果是使用 fsync，使用 AOF 的性能一般要比 RDB 的性能差。还有一个缺点是，有可能出现比较严重的 bug，因为个别命令的原因，导致 AOF 文件在重新载入时，无法将数据集恢复成保存时的原样，比如 BRPOPLPUSH 这种阻塞命令，曾经引起过这样的 bug，这种 bug 当然是比较少的，而 RDB 就基本不会有这样的 bug。

%%%

### AOF or RDB？

Note:
antirez 的建议是两种持久化的功能都用，如果你可以忍受几分钟之内的数据丢失，你可以只使用 RDB 持久化，不推荐只使用 AOF 的方式，因为定时生成 RDB 快照（snapshot）非常便于进行数据库备份，RDB 恢复数据集的速度也要比 AOF 恢复的速度要快。


%%%

## cluster

* Replication
* Sentinel
* Redis cluster

Note:
刚才我们谈到持久化功能，Redis 保证了即使在服务器重启的情况下也不会损失（或少量损失）数据，然而单点的情况还是没有改变，如果这台服务器出现了硬盘故障，就会导致数据丢失，为了避免单点故障，我们需要集群的功能。

%%%

### Replication

![redis-master-slave](http://7xi5vu.com1.z0.glb.clouddn.com/redis-master-slave.jpg)

Note:
Redis 提供了复制（replication）的功能，可以实现当一台数据库中的数据更新后，自动将更新的数据同步到其他数据库上。 master 数据库可以进行读写操作，当写操作导致数据发生变化的时候，自动将数据同步给从数据库， slave 数据库一般是只读的。一个主数据库可以有多个从数据库，从数据库只能有一个主数据库。从数据库不仅可以接收主数据库的同步数据，自己也可以作为主数据库。需要特别注意的是：开启了复制功能，并且主数据库关闭持久化功能时，一定不要用 Supervisor 以及类似的进程工具自动重启主数据库，否则一旦进行了同步，所有的数据都会被清空。为了提高性能，一般从数据库会启用持久化，主数据库会禁用持久化，从数据库崩溃重启了，主数据库会把数据同步过来。主数据库崩溃了，如果是手工通过从数据库恢复，需要严格按照两个步骤进行，1 在从数据库中使用 slaveof no one 把从数据库提升为主数据库 2 启动崩溃的主数据库，用 slaveof 把主数据库变为主数据库的从数据库，数据就可以恢复回来。 所以说手工恢复是比较麻烦的，我们可以用 redis 提供的哨兵功能来实现这个过程。

%%%

### Sentinel

* Monitoring
* Notification
* Automatic failover
* Configuration provider

Note:
哨兵是 Redis 在2.8版本推出的功能。主要包括四个方面：1. 监控，不断检查主服务器和从服务器是否运作正常， 2. 提醒，当被监控的某个 Redis 服务器出现问题时， Sentinel 可以通过 API 向管理员或者其他应用程序发送通知。3. 自动故障迁移， 当一个主服务器不能正常工作时， Sentinel 会开始一次自动故障迁移操作， 它会将失效主服务器的其中一个从服务器升级为新的主服务器， 并让失效主服务器的其他从服务器改为复制新的主服务器。4. 提供配置信息，哨兵可以用作客户端的服务发现，他可以高速客户端 master 的地址。Redis Sentinel  也是一个分布式的系统，可以单独起一个哨兵程序，也可以启动一个运行在 sentinel 模式下的 Redis 服务器。即使是使用哨兵的功能，这时的 redis 集群的每个数据库仍然存有集群中的所有数据，从而导致集群的总数据存储量受限于可用存储内存最小的数据库节点，因为 redis 中的所有数据都是基于内存的，所以这个问题会比较突出。 因此 Redis 在3.0版本正式推出了官方支持的集群模式

%%%

### Redis cluster

Note:
以往的 redis 集群，我们可以通过客户端分片来解决，有多个数据库节点的时候，由客户端决定每个键交由哪个数据库节点存储。这样就实现了整个数据库分布在 N 个数据库节点中，每个节点只存放总数据量的1/N。这样你需要扩容的时候，得对数据做手工的迁移，为了保证数据的一致性，还有可能让集群暂时下线，相对比较复杂。客户端分片有很多缺点，维护成本高，增加，移除节点比较繁琐，而且维护相同的分片逻辑成本很大，比如系统中有一个 redis 集群，一套业务系统是 Python 写的，一套的 golang 写的，为了保证分片逻辑的一致性，分片逻辑在两套系统中都得实现，开发成本比较大。于是在 redis cluster 模式推出之前，有一些公司做了 redis 集群的代理模式，比较有名的是 twitter 的 Twemproxy：

%%%

![twemproxy](http://7xi5vu.com1.z0.glb.clouddn.com/twemproxy.jpg)

Note:
Twemproxy的优点是：1. 客户端像连接Redis实例一样连接Twemproxy，不需要改任何的代码逻辑。2. 支持无效Redis实例的自动删除。3. Twemproxy与Redis实例保持连接，减少了客户端与Redis实例的连接数。 也有以下不足：1. 由于Redis客户端的每个请求都经过Twemproxy代理才能到达Redis服务器，这个过程中会产生性能损失。2. 没有友好的监控管理后台界面，不利于运维监控。3. 最大的问题是Twemproxy无法平滑地增加Redis实例。对于运维人员来说，当因为业务需要增加Redis实例时工作量非常大。也就是说，仍然没有水平扩容的功能。当然，这个是最被广泛使用，稳定性最高的 redis 代理。Codis 可以解决这个问题，可以平滑增加 redis 实例，这里我就不赘述了，我没有最太多研究，也没有什么经验。

%%%

![redis-cluster-workflow](http://7xi5vu.com1.z0.glb.clouddn.com/redis-cluster-workflow.jpg)

Note:
Redis 的工作模式如图，Redis 集群使用数据分片（sharding）而非一致性哈希（consistency hashing）来实现： 一个 Redis 集群包含 16384 个哈希槽（hash slot）， 数据库中的每个键都属于这 16384 个哈希槽的其中一个， 集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。如果用户将新节点 D 添加到集群中， 那么集群只需要将节点 A 、B 、 C 中的某些槽移动到节点 D 就可以了。因为将一个哈希槽从一个节点移动到另一个节点不会造成节点阻塞， 所以无论是添加新节点还是移除已存在节点， 又或者改变某个节点包含的哈希槽数量， 都不会造成集群下线。 我们这里假设1负责处理 0 号至 5500 号哈希槽，2负责处理 5501 号至 11000 号哈希槽，3负责处理 11001 号至 16384 号哈希槽。如果节点 2 下线了， 那么集群将无法正常运行， 因为集群找不到节点来处理 5501 号至 11000 号的哈希槽。另一方面， Redis 集群对节点使用了主从复制功能，我们可以给2号设置一个从节点，这种情况就跟 replication 的情况一样了，如果节点 2号的 master 和 slave 都下线，Redis 集群还是会停止工作。 

%%%

Note:
这里解释一下应用市场中的 redis 集群为什么需要6个节点。如果要使用集群中的自动故障转移功能， 我们至少需要3个节点，为了保证高可靠，我们给每个 master 都添加了 slave，既然是高可靠， master 和 slave 就不应该在一起。所以，其实这6个 redis 实例是 可以部署在一台机器上的，只要用不同端口就行了。但原则是 master 和 slave 不应该在一起，所以理论上可以选2，3，4，5，6台机器来部署6个 redis 实例，这些情况组合比较多，所以我干脆一个实例部署在一台机器上了。

%%%

### How to take advantages of Redis

Note:
最后，抛砖引玉，说说我们可以怎么最大程度地利用好 redis？

%%% 

![example](http://7xi5vu.com1.z0.glb.clouddn.com/2016-12-13/lizi.jpg)

%%%

#### Slow latest items listings in your home page

without redis
```
SELECT * FROM foo WHERE ... ORDER BY time DESC LIMIT 10
```

with redis
```
FUNCTION get_latest_comments(start,num_items):
    id_list = redis.lrange("latest.comments",start,start+num_items-1)
    IF id_list.length < num_items
        id_list = SQL_DB("SELECT ... ORDER BY time LIMIT ...")
    END
    RETURN id_list
END
```

Note:
比如有这样一类问题，我们需要查询最新的10条数据，这里就抽象为最新的10条评论吧，如果没有 redis，我们每次都需要从数据库里面查询数据，如果我们用 redis 的话，可以将最新的 评论保存在 redis 的列表里，有新的评论就 push 进入，当然 push 进 redis 的列表后也需要写数据库，取最新评论的时候从 redis 去就行了。

%%%

#### Leaderboards and related problems

with redis
```
ZADD leaderboard <score> <username>
```

```
ZRANK leaderboard <username>
```

Note:
redis 还可以解决跟排名相关的应用，利用 redis 我们可以高效地实现排序，比如游戏榜单的实时排序，我们可以用一个有序集合来存储，有一个新的数据时，可以用 ZADD 添加数据，需要获得某个用户的排名时，可以用 ZRANK 来获得。 如果只用数据库，比如苹果的 game center，热门的游戏可能有几百万人在玩，如果查数据库的话，实时排名基本就不可能了。

%%%

#### Order by user votes and time

Note:
Redis 可以用来进行实时的排序，比如 hacker news，通过一定的公式来对所有的文章进行排序。

%%% 

#### Implement expires on items

Note:
实时排序的问题。实现定时 expires 的功能。 还是举 hacker news 的例子，定期删掉一些数据，我们可以有 expires 关键字。

%%%

#### Counting stuff

```
INCR ip:<id>
EXPIRE ip:<id> 60
```

Note:
我相信大家都有过这样的需求：给某个值加一个计数器。有一个跟我们平台相关的例子，比如我们需要统计某个时间段内的注册人数，如果5分钟内有几千个注册用户，那么很有可能是被人攻击了，或者我们要分析某个被频繁请求的 api 是否正常，我们可以统计60秒内的独立 ip 访问次数，如果是恶意的请求，我们可以把这个 ip 加入黑名单。如果是存数据库的话，我们确实可以加一个字段，但这是一个频繁写的操作，会比较影响性能。如果是用 redis 的话，可以很简单地实现这个功能，而且性能比较高

%%%

#### Unique N items in a given amount of time

```
SADD page:day1:<page_id> <user_id>
```

```
SCARD page:day1:<page_id>
```

```
SISMEMBER page:day1:<page_id> <user_id>
```

Note:
当我们需要判断一个时间段内某个集合内是否已经存在某个值，比如记录今天访问过某个页面的用户，我们可以用一个集合来记录，当一个用户访问了某个页面，我们把这个值记录在集合里，当我们需要知道某个页面的独立用户个数，可以用：SCARD，当我们需要了解某个用户是否访问过某个页面，可以用：SISMEMBER 

%%% 

#### Real time analysis of what is happening, for stats, anti spam, or whatever

Note:
这类实时分析的服务，一般不需要考虑数据的持久化的问题，但你可能需要大量缓存数据。所以我们可以用 redis 进行数据的实时分析。

%%% 

#### Pub/Sub

>Redis Pub/Sub is very very simple to use, stable, and fast, with support for pattern matching, ability to subscribe/unsubscribe to channels on the run,

Note:
Redis 的订阅/发布功能非常容易使用，简单的订阅/发布功能可以用 redis，

%%% 

#### Queues

A common usage of Redis as a queue is the [Resque](https://github.com/blog/542-introducing-resque) library, implemented and popularized by Github's folks.

Note:
Redis 可以当做队列使用。github 的 resque 就是基于 redis 实现的队列系统，这个东西是 ruby 栈的。 

%%%


## More resource

* Books
* website

%%% 
[![Redis 入门指南](https://img5.doubanio.com/lpic/s28104066.jpg)](https://book.douban.com/subject/26419240/)

%%%

[![Redis 实战](https://img3.doubanio.com/lpic/s28296984.jpg)](https://book.douban.com/subject/26612779/)

%%%

[![Redis 设计与实现](https://img1.doubanio.com/lpic/s27297117.jpg)](https://book.douban.com/subject/25900156/)


Note:
一本入门，打开新世界的大门，一本实战，教你怎么使用这门功夫，一本内功心法，教你如何融会贯通，三本书都非常好。

%%%

* [redis.io](https://redis.io/)
* [中文文档](https://redis.io/)
* [antirez.com](http://antirez.com/latest/0)
* [source code](https://github.com/antirez/redis)
