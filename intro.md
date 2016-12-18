# Redis

An open source(BSD licensed), in-memory data structure store, used as a database, cache and message broker  

[redis.io](https://redis.io/)

Note:
大家好，今天我跟大家简单介绍一下 Redis。一句话介绍 Redis，redis 是一个开源的，键值对存储系统，你可以把它当做数据库，缓存系统，消息队列来用。Redis 最早是一个个人项目，08的时候，Salvatore Sanfilippo 开始开发，09年开源，到现在也不过7，8年的时间，现在基本是 NoSQL 中最流行的数据库了。12年的时候 hacker news 上有一份关于数据库的调查，有12%的技术公司在用 redis。国内，新浪博客，知乎等，国外有 github，stack overflow，flickr，暴雪，instagram 都是 redis 的用户。后来 VMware 开始赞助 Redis，Redis 开发者们也先后加入了 VMware，全职开发 redis。现在开发还是很活跃的，两周前，antirez 写了一篇博客说 redis 4.0 就快出了。
所以 redis 的理念是什么？antirez 写过一篇博客来总结。

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
这篇文章大致是说：作者收到一些 PR，解释为什么 redis 要这么做而不是那么做感到很厌烦，所以他总结了一份 redis 宣言。   
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
使用 AOF 你可以

%%%

## 集群

* 复制
* 集群
* 哨兵

%%%

## 管理

KEEP it simple

安全层面

管理工具

## More resource

