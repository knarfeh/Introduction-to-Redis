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

%%%

## Redis tutorial

%%%

### Data types

> Redis is not plain key-value store, actually it is a data structures server, supoerting differentw kind of values.

Note: 
test

%%%

#### Strings

%%%

#### Lists

%%%

#### Sets

%%% 

#### Hashes

%%% 

#### Sorted sets

%%%

## Advanced

%%%

### Expires

%%%

#### Expires 应用的例子

%%%

### Publish/Subscribe

%%%

### lua scripts

%%%

### 持久化

* RDB
* AOF

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

