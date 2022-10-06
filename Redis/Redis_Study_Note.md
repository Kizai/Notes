# Redis 学习笔记

### 相关文档链接
* [Redis官方文档](https://redis.io/docs/)
* [Redis中文官网](http://redis.cn/)
* [Redis学习笔记](https://www.justtutorial.dev/b/0145A0D3-E8A7-4B64-B789-C10DFCC71087/Redis_%E8%BF%90%E7%BB%B4)
* [Redis学习笔记Github](https://github.com/talliujobs/RedisLearning)

### Redis学习书籍
* [Redis设计与实现_数据库技术丛书_by_黄健宏_著](Books/Redis实战_by_美约西亚_L_卡尔森（Josiah_L_Carlson）_Carlson）,_约西亚_L_卡尔森（Josiah.pdf)
* [Redis实战_by_美约西亚_L_卡尔森（Josiah_L_Carlson）_Carlson)](Books/Redis实战_by_美约西亚_L_卡尔森（Josiah_L_Carlson）_Carlson）,_约西亚_L_卡尔森（Josiah.pdf)

### Redis 是什么？
* Redis(<font color=red>Re</font>mote <font color=red>di</font>ctionary <font color=red>s</font>erver),即远程字典服务！
* Redis是一个使用ANSI C编写的开源、支持网络、基于内存、可选持久性的键值对（Key-Value）存储数据库,并提供多种语言的API。
* Redis是一个开源的，存储在内存中的数据结构存储器，常被用来做数据库、缓存、消息队列。

### Redis优势
* 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* 原子 – Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
* 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。
* 单线程特性，秒杀系统，基于redis是单线程特征，防止出现数据库“爆破”

### Redis与其他key-value存储有什么不同？
* Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径
Redis的数据类型都是基于基本数据结构的同时对程序员透明，无需进行额外的抽象。
* Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时需要权衡内存，因为数据
不能大于硬件内存。在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情。同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问。

### Redis都能干嘛？
1. 内存存储、持久化、内存中是断电即失、所以说持久化很重要（RDB AOF）
   1. RDB持久化:原理是将Reids在内存中的数据库记录定时dump到磁盘上的RDB持久化；RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘，实际操作过程是fork一个子进程，先将数据集写入临时文件，写入成功后，再替换之前的文件，用二进制压缩存储。
   2. AOF持久化：原理是将Reids的操作日志以追加的方式写入文件；AOF持久化以日志的形式记录服务器所处理的每一个写、删除操作，查询操作不会记录，以文本的方式记录，可以打开文件看到详细的操作记录。
2. 效率高，可以用于高速缓存。
3. 可以发布订阅系统。
4. 计时器、计数器（浏览量！）


### Memcached与Redis有什么区别
**1、数据操作不同**

与Memcached仅支持简单的key-value结构的数据记录不同，Redis支持的数据类型要丰富得多。Memcached基本只支持简单的key-value存储，不支持枚举，不支持持久化和复制等功能。Redis支持服务器端的数据操作相比Memcached来说，拥有更多的数据结构和并支持更丰富的数据操作，支持list、set、sorted set、hash等众多数据结构，还同时提供了持久化和复制等功能。而通常在Memcached里，使用者需要将数据拿到客户端来进行类似的修改再set回去，这大大增加了网络IO的次数和数据体积。在Redis中，这些复杂的操作通常和一般的GET/SET一样高效。所以，如果需要缓存能够支持更复杂的结构和操作， Redis会是更好的选择。

**2、内存管理机制不同**

在Redis中，并不是所有的数据都一直存储在内存中的。这是和Memcached相比一个最大的区别。当物理内存用完时，Redis可以将一些很久没用到的value交换到磁盘。Redis只会缓存所有的key的信息，如果Redis发现内存的使用量超过了某一个阀值，将触发swap的操作，Redis根据“swappability = age*log(size_in_memory)”计算出哪些key对应的value需要swap到磁盘。然后再将这些key对应的value持久化到磁盘中，同时在内存中清除。这种特性使得Redis可以保持超过其机器本身内存大小的数据。

而Memcached默认使用Slab Allocation机制管理内存，其主要思想是按照预先规定的大小，将分配的内存分割成特定长度的块以存储相应长度的key-value数据记录，以完全解决内存碎片问题。

从内存利用率来讲，使用简单的key-value存储的话，Memcached的内存利用率更高。而如果Redis采用hash结构来做key-value存储，由于其组合式的压缩，其内存利用率会高于Memcached。

**3、性能不同**

由于Redis只使用单核，而Memcached可以使用多核，所以平均每一个核上Redis在存储小数据时比Memcached性能更高。而在100k以上的数据中，Memcached性能要高于Redis，虽然Redis也在存储大数据的性能上进行了优化，但是比起Memcached，还是稍有逊色。

**4、集群管理不同**

Memcached是全内存的数据缓冲系统，Redis虽然支持数据的持久化，但是全内存毕竟才是其高性能的本质。作为基于内存的存储系统来说，机器物理内存的大小就是系统能够容纳的最大数据量。如果需要处理的数据量超过了单台机器的物理内存大小，就需要构建分布式集群来扩展存储能力。

Memcached本身并不支持分布式，因此只能在客户端通过像一致性哈希这样的分布式算法来实现Memcached的分布式存储。相较于Memcached只能采用客户端实现分布式存储，Redis更偏向于在服务器端构建分布式存储。
## Redis安装：
### CentOS
* 下载地址：https://redis.io/download/
* 下载、解压、编译Rediss
```shell
# 为了编译 Redis 源码，你需要 gcc-c++和 tcl。如果你的系统是 CentOS，可以直接执行命令来安装：
$ yum install -y gcc-c++ tcl
$ wget https://github.com/redis/redis/archive/7.0.2.tar.gz
$ tar xzf redis-7.0.2.tar.gz
$ cd redis-7.0.2
$ make

# 进入到解压后的 src 目录，通过如下命令启动 Redis
$ src/redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```
### UbuntuOS
```shell
# 安装Redis
$ sudo apt-get update
$ sudo apt-get install -y redis-server

# 开机启动
$ echo "/usr/local/bin/redis-server /etc/redis.conf" >> /etc/rc.local

# 启动Redis
$ redis-server

# 查看redis是否启动
$ redis-cli
```
### Redis 配置
Redis 的配置文件位于 Redis 安装目录下，文件名为 `redis.conf`。

## Redis 数据类型
Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

### 三种特殊的数据类型
* geospatial:地理空间
* Hyperloglog:统计基数的利器
* bitmaps：新点阵图

### String（字符串）
string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。
```redis
127.0.0.1:6379> SET name "lemu"
OK
127.0.0.1:6379> GET name
"lemu"
```

### Hash（哈希）
Redis hash 是一个键值对集合。

Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。
```redis
127.0.0.1:6379> HMSET user:1 username lemu password lemu@1215 points 200
OK
127.0.0.1:6379> HGETALL user:1
1) "username"
2) "lemu"
3) "password"
4) "lemu@1215"
5) "points"
6) "200"
```

### List（列表）
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。
```redis
127.0.0.1:6379> LPUSH lemu redis
(integer) 1
127.0.0.1:6379> LPUSH lemu mongodb
(integer) 2
127.0.0.1:6379> LPUSH lemu rabitmq
(integer) 3
127.0.0.1:6379> LRANGE lemu 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
```

### Set（集合）
Redis的Set是string类型的无序集合。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 **sadd 命令**
添加一个string元素到,key对应的set集合中，成功返回1,如果元素已经在集合中返回0,key对应的set不存在返回错误。
```redis
127.0.0.1:6379> SADD lemu redis
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> SADD lemus redis
(integer) 1
127.0.0.1:6379> SADD lemus mongodb
(integer) 1
127.0.0.1:6379> SADD lemus rabitmq
(integer) 1
127.0.0.1:6379> SADD lemus rabitmq
(integer) 0
127.0.0.1:6379> SMEMBERS lemus
1) "mongodb"
2) "redis"
3) "rabitmq"
```

### zset(sorted set：有序集合)
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

zset的成员是唯一的,但分数(score)却可以重复。

zadd 命令 添加元素到集合，元素在集合中存在则更新对应score
```redis
127.0.0.1:6379> ZADD lemuss 0 redis
(integer) 1
127.0.0.1:6379> ZADD lemuss 0 mongodb
(integer) 1
127.0.0.1:6379> ZADD lemuss 0 rabitmq
(integer) 1
127.0.0.1:6379> ZRANGEBYSCORE lemuss 0 1000
1) "mongodb"
2) "rabitmq"
3) "redis"
```

### Redis 命令
```shell
#本地
$ redis-cli
##远程服务器
$ redis-cli -h host -p port -a password
```

## Redis 键(key)
### Redis keys 命令

| 序号 | 命令                                 | 描述                                                                                                                          |
| ---- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| 1    | DEL key                              | 该命令用于在 key 存在时删除 key                                                                                               |
| 2    | DUMP key                             | 序列化给定 key ，并返回被序列化的值                                                                                           |
| 3    | EXISTS key                           | 检查给定 key 是否存在                                                                                                         |
| 4    | EXPIRE key seconds                   | 为给定 key 设置过期时间                                                                                                       |
| 5    | EXPIREAT key timestamp               | EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp) |
| 6    | PEXPIRE key milliseconds             | 设置 key 的过期时间以毫秒计                                                                                                   |
| 7    | PEXPIREAT key milliseconds-timestamp | 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计                                                                            |
| 8    | KEYS pattern                         | 查找所有符合给定模式( pattern)的 key                                                                                          |
| 9    | MOVE key db                          | 将当前数据库的 key 移动到给定的数据库 db 当中                                                                                 |
| 10   | PERSIST key                          | 移除 key 的过期时间，key 将持久保持                                                                                           |
| 11   | PTTL key                             | 以毫秒为单位返回 key 的剩余的过期时间                                                                                         |
| 12   | TTL key                              | 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)                                                                    |
| 13   | RANDOMKEY                            | 从当前数据库中随机返回一个 key                                                                                                |
| 14   | RENAME key newkey                    | 修改 key 的名称                                                                                                               |
| 15   | RENAMENX key newkey                  | 仅当 newkey 不存在时，将 key 改名为 newkey                                                                                    |
| 16   | TYPE key                             | 返回 key 所储存的值的类型                                                                                                     |

### Redis 字符串命令

| 序号 | 命令                             | 描述                                                                                                |
| ---- | -------------------------------- | --------------------------------------------------------------------------------------------------- |
| 1    | SET key value                    | 设置指定 key 的值                                                                                   |
| 2    | GET key                          | 获取指定 key 的值                                                                                   |
| 3    | GETRANGE key start end           | 返回 key 中字符串值的子字符                                                                         |
| 4    | GETSET key value                 | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)                                            |
| 5    | GETBIT key offset                | 对 key 所储存的字符串值，获取指定偏移量上的位(bit)                                                  |
| 6    | MGET key1 [key2..]               | 获取所有(一个或多个)给定 key 的值                                                                   |
| 7    | SETBIT key offset value          | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)                                            |
| 8    | SETEX key seconds value          | 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)                                |
| 9    | SETNX key value                  | 只有在 key 不存在时设置 key 的值                                                                    |
| 10   | SETRANGE key offset value        | 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始                                    |
| 11   | STRLEN key                       | 返回 key 所储存的字符串值的长度                                                                     |
| 12   | MSET key value [key value ...]   | 同时设置一个或多个 key-value 对                                                                     |
| 13   | MSETNX key value [key value ...] | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在                                      |
| 14   | PSETEX key milliseconds value    | 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位 |
| 15   | INCR key                         | 将 key 中储存的数字值增一                                                                           |
| 16   | INCRBY key increment             | 将 key 所储存的值加上给定的增量值（increment）                                                      |
| 17   | INCRBYFLOAT key increment        | 将 key 所储存的值加上给定的浮点增量值（increment）                                                  |
| 18   | DECR key                         | 将 key 中储存的数字值减一                                                                           |
| 19   | DECRBY key decrement             | key 所储存的值减去给定的减量值（decrement）                                                         |
| 20   | APPEND key value                 | 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾                   |

### Redis hash 命令

| 序号 | 命令                                           | 描述                                                  |
| ---- | ---------------------------------------------- | ----------------------------------------------------- |
| 1    | HDEL key field2 [field2]                       | 删除一个或多个哈希表字段                              |
| 2    | HEXISTS key field                              | 查看哈希表 key 中，指定的字段是否存在                 |
| 3    | HGET key field                                 | 获取存储在哈希表中指定字段的值                        |
| 4    | HGETALL key                                    | 获取在哈希表中指定 key 的所有字段和值                 |
| 5    | HINCRBY key field increment                    | 为哈希表 key 中的指定字段的整数值加上增量 increment   |
| 6    | HINCRBYFLOAT key field increment               | 为哈希表 key 中的指定字段的浮点数值加上增量 increment |
| 7    | HKEYS key                                      | 获取所有哈希表中的字段                                |
| 8    | HLEN key                                       | 获取哈希表中字段的数量                                |
| 9    | HMGET key field1 [field2]                      | 获取所有给定字段的值                                  |
| 10   | HMSET key field1 value1 [field2 value2 ]       | 同时将多个 field-value (域-值)对设置到哈希表 key 中   |
| 11   | HSET key field value                           | 将哈希表 key 中的字段 field 的值设为 value            |
| 12   | HSETNX key field value                         | 只有在字段 field 不存在时，设置哈希表字段的值         |
| 13   | HVALS key                                      | 获取哈希表中所有值                                    |
| 14   | HSCAN key cursor [MATCH pattern] [COUNT count] | 迭代哈希表中的键值对                                  |

### Redis 列表命令

| 序号 | 命令                                    | 描述                                                                                                                        |
| ---- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| 1    | BLPOP key1 [key2 ] timeout              | 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。                                   |
| 2    | BRPOP key1 [key2 ] timeout              | 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。                                 |
| 3    | BRPOPLPUSH source destination timeout   | 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4    | LINDEX key index                        | 通过索引获取列表中的元素                                                                                                    |
| 5    | LINSERT key BEFORE or AFTER pivot value | 在列表的元素前或者后插入元素                                                                                                |
| 6    | LLEN key                                | 获取列表长度                                                                                                                |
| 7    | LPOP key                                | 移出并获取列表的第一个元素                                                                                                  |
| 8    | LPUSH key value1 [value2]               | 将一个或多个值插入到列表头部                                                                                                |
| 9    | LPUSHX key value                        | 将一个或多个值插入到已存在的列表头部                                                                                        |
| 10   | LRANGE key start stop                   | 获取列表指定范围内的元素                                                                                                    |
| 11   | LREM key count value                    | 移除列表元素                                                                                                                |
| 12   | LSET key index value                    | 通过索引设置列表元素的值                                                                                                    |
| 13   | LTRIM key start stop                    | 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。                          |
| 14   | RPOP key                                | 移除并获取列表最后一个元素                                                                                                  |
| 15   | RPOPLPUSH source destination            | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回                                                                    |
| 16   | RPUSH key value1 [value2]               | 在列表中添加一个或多个值                                                                                                    |
| 17   | RPUSHX key value                        | 为已存在的列表添加值                                                                                                        |

### Redis 集合命令

| 序号 | 命令                                           | 描述                                                |
| ---- | ---------------------------------------------- | --------------------------------------------------- |
| 1    | SADD key member1 [member2]                     | 向集合添加一个或多个成员                            |
| 2    | SCARD key                                      | 获取集合的成员数                                    |
| 3    | SDIFF key1 [key2]                              | 返回给定所有集合的差集                              |
| 4    | SDIFFSTORE destination key1 [key2]             | 返回给定所有集合的差集并存储在 destination 中       |
| 5    | SINTER key1 [key2]                             | 返回给定所有集合的交集                              |
| 6    | SINTERSTORE destination key1 [key2]            | 返回给定所有集合的交集并存储在 destination 中       |
| 7    | SISMEMBER key member                           | 判断 member 元素是否是集合 key 的成员               |
| 8    | SMEMBERS key                                   | 返回集合中的所有成员                                |
| 9    | SMOVE source destination member                | 将 member 元素从 source 集合移动到 destination 集合 |
| 10   | SPOP key                                       | 移除并返回集合中的一个随机元素                      |
| 11   | SRANDMEMBER key [count]                        | 返回集合中一个或多个随机数                          |
| 12   | SREM key member1 [member2]                     | 移除集合中一个或多个成员                            |
| 13   | SUNION key1 [key2]                             | 返回所有给定集合的并集                              |
| 14   | SUNIONSTORE destination key1 [key2]            | 所有给定集合的并集存储在 destination 集合中         |
| 15   | SSCAN key cursor [MATCH pattern] [COUNT count] | 迭代集合中的元素                                    |

### Redis 有序集合命令

| 序号 | 命令                                           | 描述                                                                |
| ---- | ---------------------------------------------- | ------------------------------------------------------------------- |
| 1    | ZADD key score1 member1 [score2 member2]       | 向有序集合添加一个或多个成员，或者更新已存在成员的分数              |
| 2    | ZCARD key                                      | 获取有序集合的成员数                                                |
| 3    | ZCOUNT key min max                             | 计算在有序集合中指定区间分数的成员数                                |
| 4    | ZINCRBY key increment member                   | 有序集合中对指定成员的分数加上增量 increment                        |
| 5    | ZINTERSTORE destination numkeys key [key ...]  | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| 6    | ZLEXCOUNT key min max                          | 在有序集合中计算指定字典区间内成员数量                              |
| 7    | ZRANGE key start stop [WITHSCORES]             | 通过索引区间返回有序集合成指定区间内的成员                          |
| 8    | ZRANGEBYLEX key min max [LIMIT offset count]   | 通过字典区间返回有序集合的成员                                      |
| 9    | ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT] | 通过分数返回有序集合指定区间内的成员                                |
| 10   | ZRANK key member                               | 返回有序集合中指定成员的索引                                        |
| 11   | ZREM key member [member ...]                   | 移除有序集合中的一个或多个成员                                      |
| 12   | ZREMRANGEBYLEX key min max                     | 移除有序集合中给定的字典区间的所有成员                              |
| 13   | ZREMRANGEBYRANK key start stop                 | 移除有序集合中给定的排名区间的所有成员                              |
| 14   | ZREMRANGEBYSCORE key min max                   | 移除有序集合中给定的分数区间的所有成员                              |
| 15   | ZREVRANGE key start stop [WITHSCORES]          | 返回有序集中指定区间内的成员，通过索引，分数从高到底                |
| 16   | ZREVRANGEBYSCORE key max min [WITHSCORES]      | 返回有序集中指定分数区间内的成员，分数从高到低排序                  |
| 17   | ZREVRANK key member                            | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序  |
| 18   | ZSCORE key member                              | 返回有序集中，成员的分数值                                          |
| 19   | ZUNIONSTORE destination numkeys key [key ...]  | 计算给定的一个或多个有序集的并集，并存储在新的 key 中               |
| 20   | ZSCAN key cursor [MATCH pattern] [COUNT count] | 迭代有序集合中的元素（包括元素成员和元素分值）                      |

## Redis的开源项目
* [CacheCloud](https://github.com/sohutv/cachecloud):CacheCloud是一个Redis云管理平台,看了一下源码是使用Java写的。
* [redis-py](https://github.com/redis/redis-py):redis 使用python封装好的python库
* [RQ（Redis Queue](https://github.com/rq/rq)：RQ（Redis Queue）是一个简单的 Python 库，用于对作业进行排队并在后台与工作人员一起处理它们。
* [flower](https://github.com/mher/flower):Flower 是一个基于 Web 的工具，用于监控和管理 Celery 集群。
* [huey](https://github.com/coleifer/huey):一个轻量级的替代品。
* [dramatiq](https://github.com/Bogdanp/dramatiq):一个快速可靠的 Python 3 分布式任务处理库。
* [kombu](https://github.com/celery/kombu): Python 的消息传递库
* [aioredis-py](https://github.com/aio-libs/aioredis-py)


### 使用redis-py
```shell
# 安装redis库
$ pip install redis

# 测试是否安装成功
>>> import redis # 导入redis 模块
>>> r = redis.StrictRedis(host='localhost', port=6379, decode_responses=True)
>>> r.set('name', 'lemu-redis') # 设置 name 对应的值
True
>>> print(r['name'])  # 设置 name 对应的值
lemu-redis
>>> print(r.get('name')) # 取出键 name 对应的值
lemu-redis
>>> print(type(r.get('name')))  # 查看类型
<class 'str'>
```
* 练习：[redis_test1](redis_test1.py)

### redis命令行操作
#### 字符串操作
```shell

# test1
127.0.0.1:6379> FLUSHALL # 清除所有的Key值
OK
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name  lemu
OK
127.0.0.1:6379> set age 2
OK
127.0.0.1:6379> keys * # 查看key的全部值
1) "age"
2) "name"
127.0.0.1:6379> get name
"lemu"
127.0.0.1:6379> EXPIRE name 10 # 设置key的过期时间，单位是s
(integer) 1
127.0.0.1:6379> ttl name # 查看key的剩余时间
(integer) 7
127.0.0.1:6379> ttl name
(integer) 5
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
(integer) 1
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> get name
(nil)
127.0.0.1:6379> TYPE name # 查看Key的数据类型
none
127.0.0.1:6379> TYPE age
string
127.0.0.1:6379> EXISTS name
(integer) 0
127.0.0.1:6379> keys *
1) "age"
127.0.0.1:6379> EXISTS age # 判断Key是否存在，1代表存在
(integer) 1
127.0.0.1:6379> MOVE age 1 # 移除Key
(integer) 1
127.0.0.1:6379> EXISTS age
(integer) 0

# test2
127.0.0.1:6379> FLUSHALL
OK
127.0.0.1:6379> set key1 v1 # 设置值
OK
127.0.0.1:6379> get key1    # 获得值
"v1"
127.0.0.1:6379> EXISTS key1 # 判断Key是否存在
(integer) 1
127.0.0.1:6379> APPEND key1 "lemu" # 追加字符串，如果当前Key不存在，就相当于SET key
(integer) 6
127.0.0.1:6379> get key1
"v1lemu"
127.0.0.1:6379> STRLEN key1 # 获得字符串长度
(integer) 6
127.0.0.1:6379> APPEND key1 ",tech"
(integer) 11
127.0.0.1:6379> STRLEN key1
(integer) 11
127.0.0.1:6379> get key1
"v1lemu,tech"

# test3
127.0.0.1:6379> SET views 0 # 设置浏览量为0
OK
127.0.0.1:6379> get views
"0"
127.0.0.1:6379> INCR views # 自增1 浏览量+1
(integer) 1
127.0.0.1:6379> INCR views
(integer) 2
127.0.0.1:6379> GET views
"2"
127.0.0.1:6379> DECR views # 自减1
(integer) 1
127.0.0.1:6379> GET views
"1"
127.0.0.1:6379> INCRBY views 10 # 设置自增量为10 浏览量+10
(integer) 11
127.0.0.1:6379> GET views
"11"
127.0.0.1:6379> DECRBY views 10 # 设置自减量为10 浏览量-10
(integer) 1
127.0.0.1:6379> GET views
"1"

# test4 字符串范围range
127.0.0.1:6379> set key1 "hello lemu" # 设置key1的值
OK
127.0.0.1:6379> get key1
"hello lemu"
127.0.0.1:6379> GETRANGE key1 0 4 # 截取字符串[0,4]
"hello"
127.0.0.1:6379> GETRANGE key1 0 -1 # 获取全部的字符串
"hello lemu"

# test5 替换指定位置的字符串
127.0.0.1:6379> set key2 abcdfgh
OK
127.0.0.1:6379> get key2
"abcdfgh"
127.0.0.1:6379> SETRANGE key2 1 xx # 替换指定位置的字符串
(integer) 7
127.0.0.1:6379> get key2
"axxdfgh"

# test6
# SETEX(set with expire) # 设置过期时间
# SETNX(set if no exist) # 不存在设置 (在分布式锁中会常常使用)

127.0.0.1:6379> setex key3 30 "hello" # 设置key3 为hello 30s过期
OK
127.0.0.1:6379> ttl name
(integer) -2
127.0.0.1:6379> ttl key3
(integer) 23
127.0.0.1:6379> get key3
"hello"
127.0.0.1:6379> setnx mykey 'redis' # 如果mykey 不存在就创建myKey
(integer) 1
127.0.0.1:6379> keys *
1) "key3"
2) "key1"
3) "mykey"
4) "key2"
127.0.0.1:6379> ttl key3
(integer) -2
127.0.0.1:6379> setnx mykey 'monogodb' # 如果mykey存在，创建失败！
(integer) 0
127.0.0.1:6379> get mykey
"redis"

# mget mset
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3 # 同时设置多个值
OK
127.0.0.1:6379> keys *
1) "k3"
2) "k2"
3) "k1"
127.0.0.1:6379> mget k1 k2 k3   # 同时获取多个值
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4  # msetnx 是一个原子性的操作，要么一起成功，要么一起失败
(integer) 0
127.0.0.1:6379> get k4
(nil)

# 对象
set user:1 {name:zhangsan,age:3} # 设置一个user:1 对象 值为json字符来保存一个对象
# 这里的key是一个巧妙的设计 user:{id}:{filed},如此设计在Redis中是完全OK的
127.0.0.1:6379> mset user:1:name zhangsan user:1:age 12
OK
127.0.0.1:6379> mget user:1:name user:1:age
1) "zhangsan"
2) "12"

# getset 先get然后set
127.0.0.1:6379> getset db redis  # 如果不存在值，则返回nil
(nil)
127.0.0.1:6379> getset db redis
"redis"
127.0.0.1:6379> getset db mongodb # 如果存在值，获取原来的值，并设置新的值
"redis"
127.0.0.1:6379> get db
"mongodb"
```
#### list操作
在redis中，list可以用来做栈、队列、阻塞队列
```shell
127.0.0.1:6379> LPUSH list one   # 将一个值或多个值,插入到列表的头部（左）
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LRANGE list 0 -1  # 获取list中的值！
1) "three"
2) "two"
3) "one"
127.0.0.1:6379> LRANGE list 0 1   # 通过区间获取具体的值
1) "three"
2) "two"
127.0.0.1:6379> RPUSH list right  # 将一个值或多个值,插入到列表的尾部（右）
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
3) "one"
4) "right"

# LPOP RPOP 移除
127.0.0.1:6379> LPOP list # 移除list的第一个元素
"three"
127.0.0.1:6379> RPOP list # 移除list的最后一个元素
"right"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"

# LINDEX 获取下标值
127.0.0.1:6379> LINDEX list 1
"one"
127.0.0.1:6379> LINDEX list 0
"two"
127.0.0.1:6379> LRANGE list 0 -1
1) "two"
2) "one"

# LLEN 
127.0.0.1:6379> LPUSH list one
(integer) 1
127.0.0.1:6379> LPUSH list two
(integer) 2
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LLEN list # 返回列表的长度
(integer) 3

# LREM  移除指定的值
127.0.0.1:6379> LPUSH list three
(integer) 4
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> LREM list 1 one # 移除list集合中指定个数的value，精确匹配
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "three"
3) "two"
127.0.0.1:6379> LREM list 1 three
(integer) 1
127.0.0.1:6379> LRANGE list 0 -1
1) "three"
2) "two"
127.0.0.1:6379> LPUSH list three
(integer) 3
127.0.0.1:6379> LREM list 2 three
(integer) 2
127.0.0.1:6379> LRANGE list 0 -1
1) "two"

# trim 修剪： list 截断
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 1
127.0.0.1:6379> RPUSH mylist "hello1"
(integer) 2
127.0.0.1:6379> RPUSH mylist "hello2"
(integer) 3
127.0.0.1:6379> RPUSH mylist "hello3"
(integer) 4
127.0.0.1:6379> LTRIM mylist 1 2  # 通过下标截取制定的长度，这个list已经被改变了，截断了只剩下截取的元素
OK
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello1"
2) "hello2"

# rpoplpush # 移除列表最后一个元素并添加到新的列表中
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 1
127.0.0.1:6379> RPUSH mylist "hello1"
(integer) 2
127.0.0.1:6379> RPUSH mylist "hello2"
(integer) 3
127.0.0.1:6379> RPOPLPUSH mylist myotherlist # 移除列表最后一个元素并添加到新的列表中
"hello2"
127.0.0.1:6379> LRANGE mylist 0 -1 # 查看原来的列表
1) "hello"
2) "hello1"
127.0.0.1:6379> LRANGE myotherlist 0 -1 # 查看目标列表中，确实存在该指
1) "hello2"

# lset 将列表中指定下标的值替换为另外一个值，更新操作
127.0.0.1:6379> EXISTS list
(integer) 0
127.0.0.1:6379> lset list 0 item   # 如果不存在列表我们去更新就会报错
(error) ERR no such key
127.0.0.1:6379> LPUSH list value1
(integer) 1
127.0.0.1:6379> LRANGE list 0 0
1) "value1"
127.0.0.1:6379> LSET list 0 item  # 如果存在，替换下标的值
OK
127.0.0.1:6379> LRANGE list 0 0
1) "item"
127.0.0.1:6379> LRANGE list 1 other # 如果不存在则会报错
(error) ERR value is not an integer or out of range

# LINSERT 
127.0.0.1:6379> RPUSH mylist "hello" # 将某个具体的value插入到列表中某个元素的前面或者后面
(integer) 1
127.0.0.1:6379> RPUSH mylist "world"
(integer) 2
127.0.0.1:6379> LINSERT mylist before "world" "other" # before在前面插入值
(integer) 3
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello"
2) "other"
3) "world"
127.0.0.1:6379> LINSERT mylist after "world" "new" # after 在后面插入值
(integer) 4
127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello"
2) "other"
3) "world"
4) "new"
```
* list小结
  * 它实际上是一个链表，before Node after,left ，right 都可以插入值
  * 如果key不存在，创建新的链表
  * 如果key存在，新增内容
  * 如果移除了所有值，空链表，也代表不存在
  * 在两边插入或者改动值，效率最高！中间元素，相对来说效率会低一点！
  * 可以做消息队列、（LPUSH RPOP），栈（LPUSH LPOP）

### Set(集合)
set中的值是不能重复的
```powershell
127.0.0.1:6379> SADD myset "hello"  # set集合中添加元素
(integer) 1
127.0.0.1:6379> SADD myset "lemu"
(integer) 1
127.0.0.1:6379> SADD myset "lemutech"
(integer) 1
127.0.0.1:6379> SMEMBERS myset   # 查看set的所有值
1) "lemutech"
2) "lemu"
3) "hello"
127.0.0.1:6379>  SISMEMBER myset hello  # 判断某一个值是不是在set集合中
(integer) 1
127.0.0.1:6379>  SISMEMBER myset ll
(integer) 0

# SCARD 获取set集合中的个数
127.0.0.1:6379> SCARD myset
(integer) 3
127.0.0.1:6379> SADD myset "lemutech2"
(integer) 1
127.0.0.1:6379> SADD myset "lemutech2"
(integer) 0
127.0.0.1:6379> SCARD myset
(integer) 4

# SREM 移除set集合的指定元素
127.0.0.1:6379> SREM myset hello
(integer) 1
127.0.0.1:6379> SCARD myset
(integer) 3
127.0.0.1:6379> SMEMBERS myset
1) "lemu"
2) "lemutech"
3) "lemutech2"

# SRANDMEMBER： set无序不重复,抽随机
127.0.0.1:6379> SRANDMEMBER myset 
"lemu"
127.0.0.1:6379> SRANDMEMBER myset 
"lemu"
127.0.0.1:6379> SRANDMEMBER myset 
"lemutech"
127.0.0.1:6379> SRANDMEMBER myset 
"lemu"
127.0.0.1:6379> SRANDMEMBER myset 
"lemutech"
127.0.0.1:6379> SRANDMEMBER myset 
"lemu"
127.0.0.1:6379> SRANDMEMBER myset 
"lemutech"
127.0.0.1:6379> SRANDMEMBER myset 
"lemu"

# SPOP: 随机删除一些set的元素
127.0.0.1:6379> SMEMBERS myset
1) "lemu"
2) "lemutech"
3) "lemutech2"
127.0.0.1:6379> SPOP myset
"lemu"
127.0.0.1:6379> SMEMBERS myset
1) "lemutech"
2) "lemutech2"

# SMOVE 将一个制定的值移动到另外一个set集合中
127.0.0.1:6379> SADD myset "hello"
(integer) 1
127.0.0.1:6379> SADD myset "lemu"
(integer) 1
127.0.0.1:6379> SADD myset "tech"
(integer) 1
127.0.0.1:6379> SADD myset2 "set2"
(integer) 1
127.0.0.1:6379> SMOVE myset myset2 "lemu"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "tech"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "set2"
2) "lemu"

# 集合的
# 差集 SDIFF
# 交集 SINTER
# 并集 SUNION
127.0.0.1:6379> SADD key1 a
(integer) 1
127.0.0.1:6379> SADD key1 b
(integer) 1
127.0.0.1:6379> SADD key1 c
(integer) 1
127.0.0.1:6379> SADD key2 c
(integer) 1
127.0.0.1:6379> SADD key2 d
(integer) 1
127.0.0.1:6379> SADD key2 e
(integer) 1
127.0.0.1:6379> SDIFF key1 key2 # 差集
1) "b"
2) "a"
127.0.0.1:6379> SINTER key1 key2 # 交集  # 共同点提取
1) "c"
127.0.0.1:6379> SUNION key1 key2 # 并集
1) "a"
2) "c"
3) "b"
4) "e"
5) "d"
```

## Redis 常用命令
### 一、全局命令
1. 查询键
   * `keys *` 查询所有的键，会遍历所有的键值，复杂度O(n)
2. 键总数
   * `dbsize` 查询键总数，直接获取redis内置的键总数变量，复杂度O(1)

3. 检查键是否存在
   * `exists key` 存在返回1，不存在返回0
4. 删除键O(k)
   * `del key [key...]` 返回结果为成功删除键的个数
5. 键过期
   * `expire key seconds` 当超过过期时间，会自动删除，key在seconds秒后过期
   * `expireat key timestamp` 键在秒级时间戳timestamp后过期
   * `pexpire key milliseconds` 当超过过期时间，会自动删除，key在milliseconds毫秒后过期
   * `pexpireat key milliseconds-timestamp` key在豪秒级时间戳timestamp后过期
   * `ttl` 命令可以查看键hello的剩余过期时间，单位：秒（>0剩余过期时间；-1没设置过期时间；-2键不存在）
   * `pttl`是毫秒
6. 键的数据结构类型
   * `type key` 如果键hello是字符串类型，则返回string；如果键不存在，则返回none
7. 键重命名
   * `rename key newkey`
   * `renamenx key newkey` 只有newkey不存在时才会被覆盖
8. 随机返回一个键
   * `randomkey`
9. 迁移键
   * `move key db` （不建议再生产环境中使用）把指定的键从源数据库移动到目标数据库
   * `dump+restore`
   * `dump key`
   * `Restore key ttl value`
   * `Dump+restore`可以实现在不同的redis实例之间进行数据迁移的功能，整个迁移的过程分为两步;
      * 1)在源redis上，dump命令会将键值序列化，格式采用的是RDB格式
      * 2)在目标redis上，restore命令将上面序列化的值进行复原，其中ttl参数代表过期时间，ttl=0代表没有过期时间
   * `migrate`:migrate实际上是吧dump、restore、del 3个命令进行组合，从而简化了操作步骤。
```shell
Migrate host port key [ key ......] destination-db timeout [replace]

127.0.0.1:6379> migrate 127.0.0.1 6379 flower 0 1000 replace
# （将键flower迁移至目标127.0.0.1:6379的库0中，超时时间为1000毫秒，replace表示目标库如果存在键flower，则覆盖）
```

### 数据库管理
1. 切换数据库
* `select dbIndex`默认16个数据库：0-15，进入redis后默认是0库。不建议使用多个数据库

2. flushdb / flushall
* 用于清除数据库，flushdb只清除当前数据库，flushall清除所有数据库。

