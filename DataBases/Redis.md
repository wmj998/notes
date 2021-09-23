# Redis

Redis是一种基于键值对（key-value）的NoSQL数据库，与很多键值对数据库不同的是，Redis中的值由string（字符串）、hash（哈希）、list（列表）、set（集合）、zset（sorted set：有序集合）、Bitmaps（位图）、HyperLogLog（基数统计）、GEO（地理信息定位）等多种数据结构和算法组成，因此Redis可以满足很多的应用场景，而且因为Redis会将所有数据都存放在内存中，所以读写性能非常好。



## 消息队列

消息队列系统是一个大型网站的必备基础组件，因为其具有业务解耦、非实时业务削峰等特性。Redis提供了发布订阅功能和阻塞队列的功能。



## 社交功能

点赞、关注、好友、推送、下拉刷新等是社交网站的必备功能，社交网站的访问量通常比较大，而且传统的关系型数据不太适合保存这种类型的数据，Redis提供的数据结构可以相对容易地实现这些功能。



## 计数器

网站中的计数功能作用至关重要，例如视频网站的播放次数、电商网站的浏览数，为了保证数据的实时性，每一次播放和浏览都要做加1的动作，如果并发量很大对于传统关系型数据库的性能来说是种压力。Redis则天然支持计数功能。



## 排行榜功能

排行榜系统几乎存在所有的网站，例如热度排名、按照时间发布的排行，按照复杂维度计算出的排行榜。Redis提供了列表和有序集合数据结构，合理地使用这些数据结构可以很方便地构建出各种排行榜系统。



## 缓存

缓存机制几乎在所有的大型网站都有使用，合理地使用缓存不仅可以加快数据的访问速度，而且能够有效地降低后端数据源的压力。Redis提供了键值过期时间设置，并且提供了灵活控制最大内存和内存溢出后的淘汰策略。



## 分布式和持久化

- Redis服务器主从热备，确保系统稳定性
- Redis分片应对海量数据和高并发



## 数据类型与操作

+ string

  Redis String 是最简单的类型，你可以理解为一个key 对应一个value。string 类型是二进制安全的。意思是redis 的string 可以包含任何数据，比如jpg 图片或者序列化的对象。string类型是Redis最基本的数据类型，一个键最大能存储512MB。

  ```
  设置键
  set key value  #设置单键值对
  mset key value [key value]  #设置多个键值对
  setex key seconds value  #设置键值及过期时间（秒单位）
  
  获取键
  get key  #获取单个键
  mget key1 key2 key3  #获取多个键
  
  查看过期时间
  ttl key
  
  运算
  原来的值必须是数值字符串
  命令：incr key  #将对应的key加1 
  命令：decr key  #将对应的key值减1
  命令：incrby key num   #将对应的key加指定值
  命令：decrby key num   #将对应的key的值减去指定值
  
  其它操作
  append key value  #追加值，redis中值都是字符串，追加就是字符拼接
  strlen key  #获取值得长度
  ```

+ hash

  Redis hash 是一个键值(key=>value)对集合。Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。每个 hash 可以存储 2的32次方 -1 键值对（40多亿）。存储形式：key = {name:'tom',age:18}

  ```
  设置值
  hset key field value  #设置key所指对象的指定属性的值
  hmset key field value  [field value] #设置key所指对象的多个属性值
  hsetnx key field value  #当field字段不存在时 设置key所指对象的field属性值
  
  获取值
  hget key field  #获取key指定的对象的属性值
  hmget key field [field]  #获取key指定对象的多个属性值
  hgetall key  #获取key所指对象的所有属性的名称和值
  hkeys key  #获取key所指对象的所有属性名
  hvals key  #获取key所指对象是的所有属性值
  hlen key  #获取key所指对象的属性个数
  
  其它操作
  hincrby key field increment  #为key所指对象的指定字段的整数值加上increment
  hincrbyfloat key field increment  #为key所指对象的指定字段的实数值加上increment
  hexists key field  #判断当前的字段是否存在在（在返回1 否则返回0）
  hdel key field [field]  #删除字段和值
  ```

+ list

  Redis list 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。列表最多可存储 2的32次方 - 1 元素 (4294967295, 每个列表可存储40多亿)。

  ```
  添加数据
  lpush key value [value]  #头部插入数据
  lpushx key value  #如果列表存在则在列表头部插入数据
  rpush key value [value]  #在列表尾部添加数据
  rpushx key value  #如果列表存在，则在尾部添加数据
  linsert key before|after value value  #在指定值前或后插入数据
  lset key index value  #设定指定索引元素的值
  注意：索引的值从左边开始，向右增加，左边第一个是0，从右边向左索引编号为：-1 -2...
  
  获取数据
  lpop key  #左侧出队并返回出队元素
  rpop key  #右侧出队并返回出队元素
  lindex key index  #返回指定索引的值
  lrange key start end  #返回存储列表中的指定范围的元素[start,end]
  lrem key count value  #从列表里移除前 count 次出现的值为 value 的元素
           count > 0: 从头往尾移除值为 value 的元素。
           count < 0: 从尾往头移除值为 value 的元素。
           count = 0: 移除所有值为 value 的元素。
  
  其它操作
  llen key  #获取列表长度
  ltrim key start stop  #裁剪列表 保留start到stop之间的元素，其它都删除
  ```

+ set

  Redis Set 是string类型的无序集合，元素具有唯一性不重复。集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

  ```
  添加元素
  sadd key member [member]  #添加多个元素
  
  获取元素
  smembers key  #获取集合中所有的元素
  scard key  #返回集合元素的个数
  srandmember key [count]  #返回集合中随机元素的值，可以返回count个
  
  其它操作
  spop key [count]  #移除集合中随机的count个元素,并返回
  srem key member1 [member2]  #移除集合中一个或者多个成员
  sismember key member  #判断元素是否在集合中 存在返回1 不在返回0
  
  集合操作
  求多个集合的交集：sinter key [key...]
  求多个集合的差集 (注意比较顺序)：sdiff key [key...]
  求 多个集合的并集：sunion key [key....]
  ```

+ zset

  Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset 的成员是唯一的,但分数(score)却可以重复。

  ```
  添加元素
  zadd key score member [score member]  #添加多个元素
  zincrby key increment member  #对指定的成员增加权重increment
  
  获取元素
  zrange key start end  #返回指定范围的元素
  zcard key  #返回元素的个数
  zcount key min max  #返回有序集合中权重在min和max之间的元素的个数
  zscore key member  #返回有序集合中 member(元素)  的权重(score)
  zrange key start end withscores  #返回当前key中 所有的权重(score)和元素(member)
  
  
  ```

+ 其它

  ```
  keys *  #查看所有的key
  keys u*  #查以u开始的key
  keys n???  #查找以n为开头长度为4个的key
  keys n  #查找包含n的所有的key
  
  支持的正则表达式:
  h?llo  #匹配第二位为任意的字符
  h*llo  #匹配第二位为任意字符 0个 或多个
  h[ab]llo  #匹配第二位为 a或者b的字符的key
  hello  #匹配第二位除了e字符以外的任意的key
  h[a-z]llo  #匹配第二位为a-z的小写字母的key
  
  exists key  #判断键是否存在
  type key  #查看key对应的value的类型
  del key  #删除指定key
  expire key 10  #设置过期时间，秒
  persist key  #移除key的过期时间
  rename key newkey  #修改key的名称(如果新的key的名字存在 则会把存在的key的值 覆盖掉)
  randomkey  #随机返回一个 key
  move key db  #将键移动到指定库
  
  flushdb  #清空当前库所有key 
  flushall  #清空所有库里的key
  
  exit  #退出redis客户端
  quit  #退出客户端
  
  info  #查看服务器信息
  
  dbsize  #当前库中有多少key
  ```

  

