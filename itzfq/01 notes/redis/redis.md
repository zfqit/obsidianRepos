# Spring Resis

## 概念

> Spring Redis 是什么?

```markdown
Spring Data 支持的键值存储之一是Redis。引用Redis项目主页：
    Redis是一个开源（BSD许可），内存存储的数据结构服务器，可用作数据库，高速缓存和消息队列代理。它支持字符
    串(String)、哈希表(Hash)、列表(List)、集合(Set)、有序集合(sorted set)，位图，hyperloglogs等数据类型。内置复
    制、Lua脚本、LRU收回、事务以及不同级别磁盘持久化功能，同时通过Redis Sentinel提供高可用，通过Redis 
    Cluster提供自动分区。
```

> 为什么要用缓存

```markdown
1.提高存储读写的性能（高性能）：统计数据 --mysql    解决mysql查询速度慢的问题
2.提供并发（高并发）： 解决mysql并发不足的问题
```

> 缓存和数据库相比

```markdown
（1）操作：
	关系型数据库(表，通过sql执行操作) ，redis是缓存数据库（Key value， 通过redis命令get、set...执行操作)
（2）作用上
       mysql用于持久化的存储数据到硬盘，功能强大，但是速度较慢
       redis用于存储使用较为频繁的数据到缓存中，读取速度快
```

> 为什么使用 Spring Data Redis？

```markdown
1. Spring Framework 是领先的全栈 Java/JEE 应用程序框架。它通过使用依赖注入、AOP 和可移植服务抽象提供了一个轻量级容器和一个非侵入式编程模型。
2. NoSQL存储系统提供了经典 RDBMS 的替代方案，以实现水平可扩展性和速度。在实现方面，键值存储代表 NoSQL 空间中最大（也是最古老）的成员之一。
3. Spring Data Redis (SDR) 框架通过 Spring 出色的基础架构支持消除了与存储交互所需的冗余任务和样板代码，从而可以轻松编写使用 Redis 键值存储的 Spring 应用程序。
```

```java
 //连接本地的 Redis 服务
Jedis jedis = new Jedis("localhost",6379);
System.out.println("连接成功");
try {
    //设置 redis 字符串数据
    jedis.set("name", "徐庶");
    // 获取存储的数据并输出
    System.out.println("redis 存储的字符串为: "+ jedis.get("name"));
}
finally {
    jedis.close();
}

```

> spring-data-redis提供了如下功能

```markdown
连接池自动管理，RedisTemplate为执行各种 Redis 操作、异常转换和序列化支持提供高级抽象。
ValueOperations：简单K-V操作
SetOperations：set类型数据操作
ZSetOperations：zset类型数据操作
HashOperations：针对map类型的数据操作
ListOperations：针对list类型的数据操作 
支持多个 Redis 驱动程序（Lettuce和Jedis）的低级抽象
支持Repository接口的自动实现，包括使用@EnableRedisRepositories.
发布订阅支持（例如用于消息驱动的 POJO 的 MessageListenerContainer）。
针对jedis客户端中大量api进行了归类封装,将同一类型操作封装为operation接口
参考官网 `https://spring.io/projects/spring-data-redis`
```

## 环境搭建

### 基于linux — docker

1. 下载镜像

2. 创建实例并启动,并挂载配置

   ```shell
   # 1.创建目录和配置文件redis.conf
   mkdir /docker
   mkdir /docker/redis
   mkdir /docker/redis/conf
   mkdir /docker/redis/data
   
   # 创建redis.conf配置文件
   touch /docker/redis/conf/redis.conf
   
   # 2.创建启动容器，加载配置文件并持久化数据
   docker run ‐d ‐‐privileged=true ‐p 6379:6379 ‐v /docker/redis/conf/redis.conf:/etc/redis/redis.conf ‐v /docker/r
   edis/data:/data ‐‐name redis redis:latest redis‐server /etc/redis/redis.conf ‐‐appendonly yes
   
   # 参数说明
       # ‐‐privileged=true：容器内的root拥有真正root权限，否则容器内root只是外部普通用户权限
       # ‐v /docker/redis/conf/redis.conf:/etc/redis/redis.conf：映射配置文件
       # ‐v /docker/redis/data:/data：映射数据目录
       # redis‐server /etc/redis/redis.conf：指定配置文件启动redis‐server进程
       # ‐‐appendonly yes：开启数据持久化
   ```

3. 测试

   ```shell
   docker exec ‐it redis redis‐cli
   set xxx xx
   get xxx
   ```

## 快速开始

**pom.xml**

1. 父maven

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring‐data‐bom</artifactId>
            <version>2021.0.5</version>
            <scope>import</scope>
            <type>pom</type>
        </dependency>
    </dependencies>
</dependencyManagement>
```

2. 子maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring‐data‐redis</artifactId>
    </dependency>

    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.7.0</version>
    </dependency>

    <!‐‐junit5‐‐>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit‐jupiter</artifactId>
        <version>5.6.3</version>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring‐test</artifactId>
        <version>5.3.10</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**配置**

基于xml

```xml
<?xml version="1.0" encoding="UTF‐8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema‐instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/sp
                           ring‐beans.xsd">

    <bean id="jedisConnectionFactory"
          class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" p:use‐pool="true"/>

    <bean id="stringRedisTemplate" class="org.springframework.data.redis.core.StringRedisTemplate" p:connection‐fac
          tory‐ref="jedisConnectionFactory"/>

</beans>

```

基于注解@Configuration javaconfig

```java
@Configuration
public class RedisConfig {

    // 1. redis的连接工厂
    @Bean
    public RedisConnectionFactory redisConnectionFactory(){

        // 设置单机redis远程服务地址
        RedisStandaloneConfiguration standaloneConfiguration = new RedisStandaloneConfiguration("127.0.0.1",6379);

        // 集群
        // new RedisClusterConfiguration(传入多个远程地址ip)

        // 连接池相关配置 (过期）
        JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
        // 最大连接数（根据并发量估算）
        jedisPoolConfig.setMaxTotal(100);
        // 最大连接数= 最大空闲数
        jedisPoolConfig.setMaxIdle(100);
        // 最小空闲连接数
        jedisPoolConfig.setMinIdle(10);
        老师
            // 最长等待时间 0无限等待 解决线程堆积问题 最好设置
            jedisPoolConfig.setMaxWaitMillis(2000);

        // 在SDR2.0+ 建议使用JedisClientConfiguration 来设置连接池
        // 默认设置了读取超时和连接超时为2秒
        JedisClientConfiguration clientConfiguration=
            34  JedisClientConfiguration.builder()
            .usePooling()
            .poolConfig(jedisPoolConfig).build();


        JedisConnectionFactory jedisConnectionFactory = new JedisConnectionFactory(standaloneConfiguration,clientConfi
                                                                                   guration);
        return jedisConnectionFactory;
    }

    // 2. redis模板类（工具类）
    @Bean
    public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory){
        RedisTemplate redisTemplate = new RedisTemplate();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        // StringRedisSerializer 只能传入String类型的值（存入redis之前就要转换成String)
        //redisTemplate.setDefaultSerializer(new StringRedisSerializer());

        // 改成Jackson2JsonRedisSerializer 推荐
        redisTemplate.setDefaultSerializer(new Jackson2JsonRedisSerializer(Object.class));

        return redisTemplate;
    }

}
```

**测试**

```java

@SpringJUnitConfig(classes = RedisConfig.class)
public class SpringDataRedis {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    public void testKeyBoundOperations(){
        redisTemplate.boundValueOps("name").set("徐庶");
        Object name = redisTemplate.boundValueOps("name").get();
        System.out.println(name);
    }


    @Test
    public void testKeyTypeOperations(){
        ValueOperations opsForValue = redisTemplate.opsForValue();
        opsForValue.set("name","xushu");

        Object name =opsForValue.get("name");
        System.out.println(name);
    }
}
```

## redis常用数据类型的CRUD操作

通过 RedisTemplate 

```java
    //注入template对象
    @Autowired
    private RedisTemplate redisTemplate;

//‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐Hash类型的操作:经常用‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐
//‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐值类型的操作:因为操作数量少,所以不长用‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐
//‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐set类型的操作:因为操作数量少,所以不长用‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐

    /**
     * 存入值
     */
    @Test
    public void boundSetOpsAdd() {
        redisTemplate.boundSetOps("nameset").add("曹操"); //放入值
        redisTemplate.boundSetOps("nameset").add("刘备");
        redisTemplate.boundSetOps("nameset").add("孙权");
    }

    /**
     * 提取值
     */
    @Test
    public void boundSetOpsGet() {
        Set names = redisTemplate.boundSetOps("nameset").members();//取出值
        System.out.println(names);
    }

    /**
     * 删除集合中的某一个值
     */
    @Test
    public void boundSetOpsDelete() {
        redisTemplate.boundSetOps("nameset").remove("曹操");
    }

    /**
     * 删除整个集合
     */
    @Test
    public void boundSetOpsDeleteAll() {
        redisTemplate.delete("nameset");
    }

//‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐list类型的操作:因为操作数量少,所以不长用‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐‐

}
```

![image-20220411221343876](https://s2.loli.net/2022/04/11/oFh2mBuSAtWvCTf.png)

### string

```markdown
1. Redis支持5种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。
2. string 是 redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value。value其实不仅是 String，也可以是数字，string 类型的值最大能存储 512MB。
常用命令:

	字符串常用操作
    SET  key  value   //存入字符串键值对
    MSET  key  value [key value ...]   //批量存储字符串键值对（不支持）
    SETNX  key  value   //存入一个不存在的字符串键值对
    setIfAbsent
    GET  key   //获取一个字符串键值
    MGET  key  [key ...]    //批量获取字符串键值（不支持）
    DEL  key  [key ...]   //删除一个键
    redisTemplate.delete(key)
    EXPIRE  key  seconds   //设置一个键的过期时间(秒)
    username.set("徐庶",10, TimeUnit.SECONDS);
    username.expire()
    
    原子加减(原子性 将整个操作视为一个整体)
    INCR  key   //将key中储存的数字值加1
    DECR  key   //将key中储存的数字值减1
    INCRBY  key  increment   //将key所储存的值加上increment
    DECRBY  key  decrement   //将key所储存的值减去decrement
    
```

```markdown
使用场景：常规key-value缓存应用（缓存单个值，缓存对象）。常规计数: 微博数, 粉丝数。
计数器
    INCR article:readcount:{文章id}   
    GET article:readcount:{文章id}
```



![image-20220411221943235](https://s2.loli.net/2022/04/11/kO4QyaXLNm1fjun.png)

**代码实现**

```java
@SpringJUnitConfig(classes = RedisConfig.class)
public class RedisStringTest {

    // 它是线程安全的
    @Autowired
    RedisTemplate redisTemplate;


    @Test
    public void test01(){
        // String类型
        BoundValueOperations username = redisTemplate.boundValueOps("username");
        // 10秒过期
        username.set("徐庶",10, TimeUnit.SECONDS);
    }


    @Test
    public void test02(){
        // String类型
        BoundValueOperations username = redisTemplate.boundValueOps("username");
        // 设置不存在的字符串 true=存入成功 false=失败
        System.out.println(username.setIfAbsent("诸葛"));
    }

    @Test
    public void test03(){
        // String类型
        System.out.println(redisTemplate.delete("username"));
    }


    @Test
    public void test04(){
        // String类型
        BoundValueOperations count = redisTemplate.boundValueOps("count");
        // 原子性累加 +1
        count.increment();
    }



    /**
 * 秒杀 1小时
 * 预热 100库存
 */
    @Test
    public void test05(){

        BoundValueOperations stock = redisTemplate.boundValueOps("stock");
        // 预热100个库存 1小时候过期
        stock.set(1,1,TimeUnit.HOURS);
    }

    // 秒杀的接口
    @Test
    public void test06(){

        BoundValueOperations stock = redisTemplate.boundValueOps("stock");

        if(stock.decrement()<0){
            System.out.println("库存不足");
        }
        else {
            System.out.println("秒杀成功");
        }
    }



    @Test
    public void test08(){

        BoundValueOperations stock = redisTemplate.boundValueOps("user");
        User user = new User();
        user.setId(1);
        user.setUsername("徐庶");
        stock.set(user);
    }
}

```

### hash

```markdown
是一个键值(key => value)对集合。Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。(可以把value当做map)
```

常用命令

```markdown
HSET  key  field  value   //存储一个哈希表key的键值
HSETNX  key  field  value   //存储一个不存在的哈希表key的键值
HMSET  key  field  value [field value ...]   //在一个哈希表key中存储多个键值对
支持根据map 批量存储
HGET  key  field   //获取哈希表key对应的field键值
HMGET  key  field  [field ...]   //批量获取哈希表key中多个field键值
不支持根据指定key获取map, 仅支持获取所有键或获取所有值.keys() .values()

HDEL  key  field  [field ...]   //删除哈希表key中的field键值
HLEN  key  //返回哈希表key中field的数量
HGETALL  key  //返回哈希表key中所有的键值
HINCRBY  key  field  increment   //为哈希表key中field键的值加上原子增量incremen
```

 使用场景：存储部分变更数据，如用户登录信息、做购物车列表等。相对于string来说，对于对象存储，不用来回进行序列化， 减少内存和CPU的消耗。

![image-20220411222516239](https://s2.loli.net/2022/04/11/3zZPCjmwsM5gYGS.png)

```java
//（1）存入值
@Test
public void boundHashOpsSet(){
    redisTemplate.boundHashOps("namehash").put("country1","中国");
    redisTemplate.boundHashOps("namehash").put("country2","日本");
    redisTemplate.boundHashOps("namehash").put("country3","韩国");
}
//（2）提取所有的KEY
@Test
public void boundHashOpsKeys(){
    Set keys = redisTemplate.boundHashOps("namehash").keys();
    System.out.println(keys);
}
//（3）提取所有的值
@Test
public void boundHashOpsValues(){
    List values = redisTemplate.boundHashOps("namehash").values();
    System.out.println(values);
}
//(4) 根据KEY提取值
@Test
public void boundHashOpsByKey(){
    Object name = redisTemplate.boundHashOps("namehash").get("country1");
    System.out.println(name);
}
//根据KEY移除值
@Test
public void boundHashOpsDelByKey(){
    redisTemplate.boundHashOps("namehash").delete("country2");
}
```

### list

```markdown
列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。（相当于java的 LinkedList(链表)  和ArrayList ，允许重复
```

常用命令

```markdown
1 LPUSH  key  value [value ...]   //将一个或多个值value插入到key列表的表头(最左边)
2 RPUSH  key  value [value ...]    //将一个或多个值value插入到key列表的表尾(最右边)
3 LPOP  key  //移除并返回key列表的头元素
4 RPOP  key  //移除并返回key列表的尾元素
5 LRANGE  key  start  stop  //返回列表key中指定区间内的元素，区间以偏移量start和stop指定
```

应用场景：Redis list的应用场景非常多，也是Redis最重要的数据结构之一，比如微博的关注列表，粉丝列表、热搜、top榜单等都可 以用Redis的list结构来实现。

![image-20220411222718534](https://s2.loli.net/2022/04/11/34MlTXJ1sH9taB8.png)

```java
@Test
public void test01() {

    BoundListOperations list = redisTemplate.boundListOps("list");
    for (int i = 0; i < 10; i++) {
        // 往尾部插入
        list.rightPush(i);
        // 往头部插入
        // list.leftPush()
        // 往尾部批量插入
        //list.rightPushAll(1,2,3,4);
        //list.leftPushAll(1,2,3,4);
    }
}

@Test
public void test02() {

    BoundListOperations list = redisTemplate.boundListOps("list");
    // 范围获取
    // System.out.println(list.range(0, list.size()));
    // 根据下标获取某一个
    System.out.println(list.index(5));
}

@Test
public void test03() {

    BoundListOperations list = redisTemplate.boundListOps("list");

    // 从头部删除指定数量
    //list.leftPop(2);
    // 从尾部删除指定数量
    //list.rightPop()

    // 根据值进行删除
    //list.remove(1,5);

    // 保留指定范围， 其余的都删掉
    //list.trim(1,2);

    // 数量
    // list.size();

    // 往指定的索引 添加/修改值
    list.set(1,10);
}

```

### set

```markdown
set是string类型的无序集合。 不允许重复
```

基本命令

```markdown
*SADD  key  member  [member ...]  //往集合key中存入元素，元素存在则忽略，若key不存在则新建
	add(V... values);

*SREM  key  member  [member ...]  //从集合key中删除元素
	remove(Object... values)

*SMEMBERS  key  //获取集合key中所有元素
	members()

*SCARD  key  //获取集合key的元素个数
	size()

*SISMEMBER  key  member  //判断member元素是否存在于集合key中
	isMember(Object o);

*SRANDMEMBER  key  [count]  //从集合key中选出count个随机元素，元素不从key中删除
	randomMembers(long count); distinctRandomMembers(long count);

*SPOP  key  [count]  //从集合key中选出count个元素，元素从key中删除
	pop()

Set运算操作
*SINTER  key  [key ...]   //交集运算
	set1: 123456    set2:123789     //交集：123
	intersect(K key);
*SINTERSTORE  destination  key  [key ..]  //将交集结果存入新集合destination中
	intersectAndStore(K key, K destKey);
*SUNION  key  [key ..]   //并集运算
	set1: 123456    set2:123789    并集：123456789
	union(K key);
*SUNIONSTORE  destination  key  [key ...]  //将并集结果存入新集合destination中
	unionAndStore(K key, K destKey);
*SDIFF  key  [key ...]   //差集运算
	set1: 123456    set2:123789    差集：  set1的差集456   set2的差集789
	diff(K key);
*SDIFFSTORE  destination  key  [key ...]  //将差集结果存入新集合destination中
	diffAndStore(K keys, K destKey);
```

应用场景： 抽奖

![image-20220411223142038](https://s2.loli.net/2022/04/11/kL89Chxot1zHfW3.png)

应用场景： 共同关注

![image-20220411223203958](https://s2.loli.net/2022/04/11/a9H6P1DSl4EhQC3.png)

```java
// 添加
@Test
public void testAdd(){
    BoundSetOperations set = redisTemplate.boundSetOps("set");
    set.add(1,2,3,4,5,6);
}

// 获取
@Test
public void testGet(){
    BoundSetOperations set = redisTemplate.boundSetOps("set");

    // 获取整个set
    System.out.println(set.members());
    // 获取个数
    System.out.println(set.size());
    // 根据某个元素判断是否在set中
    System.out.println(set.isMember(1));

    // 根据count获取随机元素 不带删除
    System.out.println(set.randomMembers(2));
    System.out.println(set.randomMembers(2));

    // 带删除
    //System.out.println(set.pop());

    SetOperations setOperations = redisTemplate.opsForSet();
    System.out.println(setOperations.pop("set", 2));
}

// Set运算操作


@Test
public void testAdd2(){
    BoundSetOperations set = redisTemplate.boundSetOps("set2");

    set.add(1,2,3,7,8,9);
}

// 交集
@Test
public void testCacl1() {

    BoundSetOperations set = redisTemplate.boundSetOps("set");
    set.intersectAndStore("set2","set3");

    BoundSetOperations set3 = redisTemplate.boundSetOps("set3");
    System.out.println(set3.members());
}

// 并集
@Test
public void testCacl2() {

    BoundSetOperations set = redisTemplate.boundSetOps("set");

    System.out.println(set.union("set2"));
}

// 差集
@Test
public void testCacl3() {

    BoundSetOperations set = redisTemplate.boundSetOps("set2");

    System.out.println(set.diff("set"));
}

```

### Zset

```markdown
Redis sorted set（Zset） 与set类似，区别是set是无序的， sorted set是有序的并且不重复的集合列表,可以通过用户额外提供一个优先级(score)的参数来为成员排序，并且是插入有序的，即自动排序。 
```

ZSet常用操作

```markdown
* ZADD key score member [[score member]…]  //往有序集合key中加入带分值元素
	Boolean add(V value, double score);
* ZREM key member [member …]  //从有序集合key中删除元素
    Long remove(Object... values);
    Long removeRange(long start, long end); // 根据顺序范围
    Long removeRangeByScore(double min, double max); // 根据分数范围
* ZSCORE key member   //返回有序集合key中元素member的分值
	List<Double> score(Object... o);
	Double score(Object o);
* ZINCRBY key increment member  //为有序集合key中元素member的分值加上increment
	Double incrementScore(V value, double delta); 原子添加
* ZCARD key  //返回有序集合key中元素个数
	Long size();
* ZRANGE key start stop [WITHSCORES]  //正序获取有序集合key从start下标到stop下标的元素
	Set<V> range(long start, long end); // 根据顺序范围
	Set<V> rangeByScore(double min, double max); // 根据分数范围
* ZREVRANGE key start stop [WITHSCORES]  //倒序获取有序集合key从start下标到stop下标的元素
	Set<V> reverseRange(long start, long end);
	Set<V> reverseRangeByScore(double min, double max);

Zset集合操作 和set一样差不多
* ZUNIONSTORE destkey numkeys key [key ...]   //并集计算
	Set<V> union(K otherKey)
* ZINTERSTORE destkey numkeys key [key …]  //交集计算
	Set<V> intersect(K otherKey)
```

实现场景

![image-20220411223753174](https://s2.loli.net/2022/04/11/y5FYRuJjhn8mPtE.png)

## Redis事务

```markdown
简单介绍下redis的几个事务命令：
redis事务四大指令: MULTI（开启事务）、EXEC（提交）、DISCARD（回滚）、WATCH。
这四个指令构成了redis事务处理的基础。
1.MULTI用来组装一个事务；
2.EXEC用来执行一个事务；
3.DISCARD用来取消一个事务；
4.WATCH类似于乐观锁机制里的版本号。
被WATCH的key如果在事务执行过程中被并发修改，则事务失败。需要重试或取消。
```

SpringDataRedis提供了2种事务的支持方式：

1. execute(SessionCallback session)方法

```java
T execute(SessionCallback session);
```

2. @Transactional的支持

```markdown
另一种实现事务的是@Transactional注解，这种方法是把事务交由给spring事务管理器进行自动管理。使用这种方法之前，跟jdbc事务一
样，要先注入事务管理器，如果工程中已经有配置了事务管理器，就可以复用这个事务管理器，不用另外进行配置。另外，需要注意的
是，跟第一种事务操作方法不一样的地方就是RedisTemplate的setEnableTransactionSupport(boolean enableTransactionSupport)方
法要set为true，此处贴出官方的配置框架

注意:
1. 这种方法的使用方法比较简单，就在要使用事务的方法注解@Transactional，这跟jdbc事务使用是一样的，这样就不用手工的执行multi、
exec方法了，这些事务控制方法会由spring事务管理器自动完成。
2. 这种便利的使用方法有局限性，就是不支持只读操作，如果执行get之类的操作，将会返回null，所以使用的时候要多加注意！

```

```java
@Configuration
@EnableTransactionManagement
public class RedisTxContextConfiguration {

    @Bean
    public StringRedisTemplate redisTemplate() {
        StringRedisTemplate template = new StringRedisTemplate(redisConnectionFactory());
        // explicitly enable transaction support
        template.setEnableTransactionSupport(true);//此处必须设置为true，不然没法实现事务管理
        return template;
    }

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        // jedis || Lettuce
    }

    @Bean
    public PlatformTransactionManager transactionManager() throws SQLException {
        return new DataSourceTransactionManager(dataSource());
    }

    @Bean
    public DataSource dataSource() throws SQLException {
        // ...
    }
}
```

```java
@Transactional
public void testRedisTx2() {
    template.opsForValue().set("name", "xushu");
    System.out.println(template.opsForValue().get("hxm"));//此处打印为null
}
```

两种方法的对比

1. execute(SessionCallback session)方法
   1. 事务代码块的范围灵活可控 
2. @Transactional注解方法
   1. 使用方便，代码比较优雅
   2. 不够灵活，事务控制范围是整个方法

```markdown
综合了以上的对比，两种方法各有优缺点，但个人更偏向于使用execute方法。如果使用@Transactional这种注解式方法，有个建议是初始化两个RedisTemplate，一个支持事务的，一个不支持事务的，即enableTransactionSupport一个设为true，一个设为false(默认是false)。不然，如果用支持事务的RedisTemplate来进行非事务性操作时，有些地方要注意，比如要手工的关闭连接等，不然是会踩坑的！因为我就是过来人！
```

## Redis 消息传递（发布/订阅）

```markdown
Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (PUBLISH ) 发送消息，订阅者 (SUBSCRIBE\PSUBSCRIBE  ) 接收消息。 
下图展示了三个频道 news-word1、news-message、sms-message， 以及订阅这个频道的三个客户端之间的关系： 
```

![image-20220411224507239](https://s2.loli.net/2022/04/11/KxI9W27rJLglGXT.png)

当有新消息通过 发布者 发送给频道时， 这个消息就会被发送给订阅它的客户端： Redis 客户端可以订阅任意数量的频道。

### 发布消息Publishing 

```java
RedisTemplate template = ...
template.convertAndSend("hello!", "world");
```

### 接收消息Subscriing 

```java
// 侦听器容器负责消息接收的所有线程并分派到侦听器中进行处理。
@Bean
RedisMessageListenerContainer container(MessageListenerAdapter listenerAdapter) {
    RedisMessageListenerContainer container = new RedisMessageListenerContainer();
    container.setConnectionFactory(redisTemplate.getConnectionFactory());
    List<Topic> topicList = new ArrayList<>();
    topicList.add(new PatternTopic("testChannel"));
    container.addMessageListener(listenerAdapter, topicList);
    return container;
}

/**
 * 消息侦听器适配器,能将消息委托给目标侦听器方法
 * @return
 */
@Bean
public MessageListenerAdapter listenerAdapter(MessageListener listener) {
    return new MessageListenerAdapter(listener);
}

/**
 * 设置redis序列化
 */
@Bean
public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
    RedisTemplate<Object, Object> template = new RedisTemplate<>();
    template.setConnectionFactory(redisConnectionFactory);

    template.setKeySerializer(new StringRedisSerializer());
    template.setValueSerializer(new Jackson2JsonRedisSerializer<Object>(Object.class));
    return template;
}
```

```java
@Component
public class Subscriber implements MessageListener {
    @Autowired
    private RedisTemplate redisTemplate;

    /**
     * 每次新消息到达时，都会调用回调
     * @param message
     * @param pattern
     */
    @Override
    public void onMessage(Message message, byte[] pattern) {
        System.out.println("11111");
        RedisSerializer<?> keySerializer = redisTemplate.getKeySerializer();
        RedisSerializer<?> valueSerializer = redisTemplate.getValueSerializer();
        Object channel = keySerializer.deserialize(message.getChannel());
        Object body = valueSerializer.deserialize(message.getBody());
        System.out.println("渠道: " + channel);
        System.out.println("消息内容: " + String.valueOf(body));
    }
}
```



### redis发布订阅和mq的区别

> 可靠性

redis ：没有相应的机制保证消息的可靠消费，如果发布者发布一条消息，而没有对应的订阅者的话，这条消息将丢失，不会存在内存中； 

rabbitmq：具有消息消费确认机制，如果发布一条消息，还没有消费者消费该队列，那么这条消息将一直存放在队列中，直到有消费者消 费了该条消息，以此可以保证消息的可靠消费；

> 实时性

redis:实时性高，redis作为高效的缓存服务器，所有数据都存在在服务器中，所以它具有更高的实时性

> 消费者负载均衡

rabbitmq队列可以被多个消费者同时监控消费，但是每一条消息只能被消费一次，由于rabbitmq的消费确认机制，因此它能够根据消费者 的消费能力而调整它的负载； redis发布订阅模式，一个队列可以被多个消费者同时订阅，当有消息到达时，会将该消息依次发送给每个订阅者；

> 持久性

redis：redis的持久化是针对于整个redis缓存的内容，它有RDB和AOF两种持久化方式（redis持久化方式，后续更新），可以将整个redis实 例持久化到磁盘，以此来做数据备份，防止异常情况下导致数据丢失。 

rabbitmq：队列，消息都可以选择性持久化，持久化粒度更小，更灵活；

> 队列监控

rabbitmq实现了后台监控平台，可以在该平台上看到所有创建的队列的详细情况，良好的后台管理平台可以方面我们更好的使用； redis没有所谓的监控平台

### 总结

redis： 轻量级，低延迟，高并发，低可靠性； 

rabbitmq：重量级，高可靠，异步，不保证实时；

rabbitmq是一个专门的AMQP协议队列，他的优势就在于提供可靠的队列服务，并且可做到异步，而redis主要是用于缓存的，redis的发布 订阅模块，可用于实现及时性，且可靠性低的功能。

