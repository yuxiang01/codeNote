[所有语法文档](http://redisdoc.com)
## 一、Redis 开篇

### 1. 什么是 Redis?
- Remote Dictionary Server (远程字典服务器)
- Redis 是一个开源的使用 `C语言` 编写的数据库 
- Redis 和 MongoDB 一样是 NoSQL 类型的数据库
  - 不同的是 MongoDB 存储的是文档, 而 Redis 存储的是键值对 (Key-Value)

### 2. Redis 特点
- 速度快
    + Redis 默认情况下将数据存储在内存中
    + 读取速度能达到 10 万次/s 左右, 写入能到到 8 万次/秒左右
- 支持数据的持久化
    + Redis 默认情况下将数据存储在内存中
    + 但是也可以将内存中的数据保存到磁盘中
- 支持多种数据结构
    + Redis 是通过 key-value 形式存储数据的
    + value 不仅支持常见的字符串类型, 整型以外
    + 同时还提供了 list, set ,zset, hash 等数据结构的存储
- 定制性强
    + Redis 虽然强大, 但是它是开源免费的
    + Redis 第一个版本代码在 23000 行左右
    + Redis 当前版本代码在 50000 行左右
- 支持分布式
    + 和 MongoDB 一样, Redis 是支持主从复制, 支持分布式存储的

### 3. Redis 场景应用场景
- 缓存系统
    + 由于 Redis 是将数据存储在内存中的, 所以我们可以使用 Redis 来实现内存缓存
    + 对于经常会被查询，但是不经常被修改或者删除的数据, 存储到 Redis 中
- 排行榜
    + 由于 Redis 支持集合（Set）和有序集合（Sorted Set）
      所以是的我们在实现排行榜的时候变的非常简单
- 计数器
    + 由于 Redis 提供了 incr/decr 指令, 使得我们在实现计数器时非常简单
    + 转发数/评论数/播放数/访问数/... ...
- 存储社交关系
    + 由于 Redis 支持存储集合类型数据, 由于社交关系不会经常发生改变
      所以很多社交网站会使用 Redis 来存储社交关系
- 消息队列系统
    + Redis 天生支持发布订阅模式, 所以天生就是实现消息队列系统的材料
### 4. 安装 Redis 
[下载Redis](http://download.redis.io/releases/) 压缩包, 并解压到任意文件夹
解压后进入 Redis 目录, 运行编译命令 `make && make install`
该目录已经默认配置到环境变量，因此可以在任意目录下运行这些命令。
- 其中：
	- `redis-cli`：是 redis 提供的命令行客户端
	- `redis-server`：是 redis 的服务端启动脚本
	- `redis-sentinel`：是 redis 的哨兵启动脚本

### 5. 启动 Redis
- redis 的启动方式有很多种，例如：
	- 默认启动
	- 指定配置启动
	- 开机自启
#### 5.1 默认启动
安装完成后，在任意目录输入 redis-server 命令即可启动 Redis：`redis-server`
这种启动属于 `前台启动`，会阻塞整个会话窗口，窗口关闭或者按下 `CTRL + C` 则 Redis 停止
不推荐使用
#### 5.2 指定配置启动
如果要让 Redis 以 `后台` 方式启动，则必须修改 Redis 配置文件，就在我们之前解压的 redis 安装包下（`/usr/local/src/redis-6.2.6`），名字叫 `redis.conf`
我们先将这个配置文件备份一份：`cp redis.conf redis.conf.bck`
然后修改 `redis.conf` 文件中的一些配置：
```properties
# 允许访问的地址，默认是127.0.0.1，会导致只能在本地访问。修改为0.0.0.0则可以在任意IP访问，生产环境不要设置为0.0.0.0
bind 0.0.0.0
# 守护进程，修改为yes后即可后台运行
daemonize yes 
# 密码，设置后访问Redis必须输入密码
requirepass 123321
```
Redis 的其它常见配置：
```properties
# 监听的端口
port 6379
# 工作目录，默认是当前目录，也就是运行redis-server时的命令，日志.持久化等文件会保存在这个目录
dir .
# 数据库数量，设置为1，代表只使用1个库，默认有16个库，编号0~15
databases 1
# 设置redis能够使用的最大内存
maxmemory 512mb
# 日志文件，默认为空，不记录日志，可以指定日志文件名
logfile "redis.log"
```
启动 Redis：
```sh
# 进入redis安装目录 
cd /usr/local/src/redis-6.2.6
# 启动
redis-server redis.conf
```
停止服务：
```sh
# 利用redis-cli来执行 shutdown 命令，即可停止 Redis 服务，
# 因为之前配置了密码，因此需要通过 -u 来指定密码
redis-cli -u 123321 shutdown
```
#### 5.3 开机自启
我们也可以通过配置来实现开机自启。
首先，新建一个系统服务文件：
```shell
vi /etc/systemd/system/redis.service
```
内容如下：
```
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /usr/local/src/redis-6.2.6/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
然后重载系统服务：
```shell
systemctl daemon-reload
```
现在，我们可以用下面这组命令来操作 redis 了：
```shell
# 启动
systemctl start redis
# 停止
systemctl stop redis
# 重启
systemctl restart redis
# 查看状态
systemctl status redis
```
执行下面的命令，可以让 redis 开机自启：
```shell
systemctl enable redis
```

## 二、通用命令
- 查询当前数据库中所有的 key `Keys *`
- 清空当前数据库 (开发操作) `Flushdb`
- 清空所有数据库 (离职操作) `Flushall`

注意点: 由于 Redis 是单线程的, 而以上操作都是非常耗时的, 所以不推荐在企业开发中使用

---
- 计算当前数据库 key 的总数 `Dbsize`
	- 注意点: dbsize 并不是通过遍历统计得到当前数据库 key 的总数, 而是每次操作时内部会自动统计
     - 所以 dbsize 并不是一个耗时的操作, 我们可以在企业开发中大胆的使用
- 查看 value 数据类型 `Type key`
- 判断指定 key 是否存在 `Exists key` 
	- 注意点: 如果存在返回 1, 如果不存在返回 0
- 设置 key 过期时间 `Expire key seconds` 
	- 注意点: 如果没有添加过期时间就是添加, 如果已经添加过了过期时间就是修改 
- 查看 key 过期时间 `ttl key`
- 取消 key 过期时间 `persist key`
	- 注意点: 如果 key 不存在或者已经被删除会返回-2, 如果 key 存在并且过期时间已经被删除会返回-1

## 三、Redis 数据类型
- Redis 是以 key-value 的形式存储数据的
- Key 无论如何都是字符串类型
- Value 支持如下的五种数据类型
    + 字符串 (String)
	    + 格式: key value
	    + 示例: name fishx
    + 哈希 (Hash)
	    + 格式: key field value
	    + 示例: user name fishx
	    + 注意点: Hash 是无序的
    + 列表 (list)
	    + 格式: key value 1 value 2 value 3
	    + 示例: names fishx zs ls ww
	    + 注意点: list 是有序的
    + 无序集合 (sets)
	    + 键是 String, 值 set
	    + 一堆无序的数据
	    + 注意点: 存储的数据不能重复
    + 有序集合 (sorted sets)
	    + 一堆有序的数据, 通过权重和实现排序
	    + 注意点: 存储的数据不能重复
### 0、Key 的层级结构
Redis 的 key 允许有多个单词形成层级结构，多个单词之间用': '隔开，格式如下：
![[1652941631682.png]]
例如我们的项目名称叫 heima，有 user 和 product 两种不同类型的数据，我们可以这样定义 key：
- user 相关的 key：**heima:user:1**
- product 相关的 key：**heima:product:1**

如果 Value 是一个 Java 对象，例如一个 User 对象，则可以将对象序列化为 JSON 字符串后存储：

|     KEY      |                  VALUE                  |
|:----------------:|:-------------------------------------------:|
|  heima:user: 1   |    {"id": 1, "name": "Jack", "age": 21}     |
| heima:product: 1 | {"id": 1, "name": "小米 11", "price": 4999} |

`set heima:user:1 '{"id":1, "name": "Jack", "age": 21}'`
![[Pasted image 20230620110203.png]]
### 1、字符串类型操作
1. 默认数据库
默认情况下 Redis 给我们创建了 16 个数据库 (0~15),
如果使用的时候没有明确的选中使用哪个数据库, 那么默认使用第 0 个
2. 切换数据库 `Select 1` 
3. 字符串类型-增删改查
	```shell
	 ## 新增
	set key value
	set name lnj
	
	## 查询
	get key
	get name
	
	## 修改
	set key value
	## 如果key已经存在就是修改
	
	## 删除
	del key
	del name
	```

4. 字符串类型-高级设置
	- `Set 'key' 'value'`
	    + 不管 key 是否存在都设置 (不存在就新增, 存在就覆盖)
	- `Setnx 'key' 'value'`
	    + 只有 key 不存在才设置 (新增)
	- `Set 'key' 'value' xx`
	    + 只有 key 存在才设置 (更新)
5. 字符串类型-批量处理
	- 批量添加值
	    - 语法: `Mset key value key value`
	    - 示例: `Mset name lnj age 98 score 100`
	- 批量查询值
	    - 语法: `Mget key key key`	    
	    - 示例: `Mget name age score`
6. 字符串类型-其它操作
	- 设置新值返回旧值 `Getset key newValue` 
	- 给旧值追加数据 `Append key value`
	- 计算 value 字符串长度 `Strlen key`, <font color="#ff0000">注意中文字符</font> 
	- 获取指定下标范围的值 `Getrange key start end`
	- 从指定下标开始设置字符串的值 `Setrange key offset value`
7. 字符串类型-自增自减操作
	- `Incr`
	    + 作用: 给指定 key 的对应的 Value 自增 1, 如果 key 不存在会自动新增, 并从0开始自增1
	    + 格式: `incr key`
	- `Decr`
	    + 作用: 给指定 key 的对应的 Value 自减 1, 如果 key 不存在会自动新增, 并从0开始自减1
	    + 格式: `decr key`
	- `Incrby`
	    + 作用: 给指定 key 的对应的 Value 增加指定值, 如果 key 不存在会自动新增, 并从0开始增加
	    + 格式: `incrby key number`
	- `Decrby`
	    + 作用: 给指定 key 的对应的 Value 减少指定的值, 如果 key 不存在会自动新增, 并从0开始减少
	    + 格式: `decrby key number`
	- `Incrbyfloat`
	    + 作用: 给指定 key 的对应的 Value 增加指定值, 如果 key 不存在会自动新增, 并从0开始增加
	    + 格式: `incrbyfloat key float`

### 2、Hash 类型操作
Redis 的 Value 除了可以存储普通的字符串类型以外, 还可以存储 Hash 类型
Hash 类型就相当于在 JS 中的对象, 可以把整个对象当做一个 Value 存储起来
- 增删改查
	- 增加
		- 语法: `hset key field value`
		- 示例: `hset user name it666`
	- 查询
		- 语法: `hget key field`
		- 示例: `hget user name`
	- 修改
		- 如果字段不存在就是新增, 如果字段存在就是修改
			- 语法: `hset key field value`
			- 示例: `hset user name it666`
	- 删除
	    + 删除指定的字段
			- 语法: `hdel key field`
			- 示例: `hdel user name`
		+ 删除对应 key 所有的数据
			- 语法: `del key`
			- 示例: `del user`
- 高级操作
	- 批量新增
		- 语法: `hmset key field1 value1 field2 value2`
		- 示例: `hmset user name lnj age 34 score 100`
	- 批量查询
		- 语法: `hmget key field1 field2 field3`
		- 示例: `hmget user name age score`
	- 工具指令
		- 返回 key 存储的 hash 表中有多少条数据
			- 语法: `hlen key` 
			- 示例: `hlen user`
		- 判断指定的 key 存储的 Hash 中是否有指定的字段
			- 语法: `hexists key field` 返回<font color="#00b050">1</font>/<font color="#ff0000">0</font>, 表示<font color="#00b050">存在</font>/<font color="#ff0000">不存在</font>
			- 示例: `hexists user name`
	- 其它操作
		- 查询所有 field
			- 语法: `hkeys key`
			- 示例: `hkeys user`
		- 查询所有 value
			- 语法: `hvals key`
			- 示例: `hvals user`
		- 查询所有的 field 和 value
			- 语法: `hgetall key`
			- 示例: `hgetall user`
		- <font color="#d83931">注意点: </font>
				- <u>由于 Redis 是单线程的, 而以上操作都是非常耗时的, 所以<span style="color:#ff4d4f">不推荐</span>在企业开发中使用</u>

### 3、List 类型操作
Redis 的 Value 除了可以存储字符串和 Hash 类型以外, 还可以存储 List 类型
List 类型就相当于 JavaScript 中的数组, 可以把整个数组当做一个 Value 存储起来
***List 是有序的***
- 增删改查
	- 增加
		+ 从第二个 Value 开始添加到前一个的左边
			- 语法: `lpush key value1 value2 value3`
			- 示例: `lpush arr1 aa bb cc` -> `[cc, bb, aa]`
		+ 从第二个 Value 开始添加到前一个的右边
			- 语法: `rpush key value1 value2 value3`
			- 示例: `rpush arr2 aa bb cc` -> `[aa, bb, cc]`
	- 查询
		+ 查询指定范围数据
			- 语法: `lrange key startIndex endIndex`
			- 索引从0开始, `endIndex == -1` 表示取到最后
		+ 查询指定索引数据
			- 语法: `lindex key index`
			- 从前往后 `索引 > 0` 从0开始, 从后往前 `索引 < 0` 从-1开始
	- 修改
		- 语法: `lset key index value`
		- 注意 index 从0开始
	- 删除
		- `lpop` 删除左边元素 `lpop key`
		- `rpop` 删除右边元素 `rpop key`
		- `lrem` 删除指定个数的指定元素 `lrem key count value`
			+ `count > 0`: 从表头开始向表尾搜索，移除与 value 相等的元素，数量为 count
			+ `count < 0`: 从表尾开始向表头搜索，移除与 value 相等的元素，数量为 count 的绝对值
			+ `count = 0`: 移除表中所有与 value 相等的值
		- `ltrim` 按照索引剪切列表 `ltrim key start end`
- 其它操作
	- 追加数据 (存在追加、不存在添加), 与增加一致
		- 语法: `lpush key value1,value2,...`
	- 插入数据
		- 语法: `linsert key before|after oldValue newValue`
		- 示例: `LINSERT rightArr BEFORE cc ee` 在 cc 的前面插入 ee 
	- 获取列表长度 `llen key`
- 列表 List 实现简单数据结构 
	- 栈结构 (水桶) - 先进后出 `lpush + lpop`
	- 队列结构 (水管) - 先进先出 `lpush + rpop`

### 4、Set 类型操作
- 增删改查
	- 新增 `sadd key value1, [value2, ...]`
	- 查询
	    + 返回集合中所有元素 `smembers key` 
	        * 注意点: 由于 Redis 是单线程的, 而以上操作都是非常耗时的, 所以当元素比较多时需要慎用
	    + 返回集合中 N 个元素 `srandmember key [count]` 
	- 删除
	    + 随机删除 N 个元素 `spop key` 
	    + 删除集合中的指定元素 `srem key value 1, [value 2, ...]` 
- 其它操作
	- 追加元素
	    + sadd: key 不存在就新增, 存在就追加
	    + sadd: 追加的元素不存在就追加, 追加的元素存在会自动忽略
	- 统计集合中元素个数 `scard key`
	- 判断集合中是否有指定元素 `sismember key member` 
	- 集合间的操作
		- 交集
		    + `sinter key [key, ...]`
		    * `{1, 2, 3} ∩ {2, 3, 4} = {2, 3}`
		- 并集
		    + `sunion  key [key, ...]`
		    * `{1, 2, 3} ∪ {2, 3, 4} = {1, 2, 3, 4}`
		- 差集
		    + `sdiff key [key, ...]`
		    * `{1, 2, 3} - {2, 3, 4} = {1}`
		    * `{2, 3, 4} - {1, 2, 3} = {4}`
- 应用场景
	- 抽奖 `srandmember key [count]`
	- 绑定标签 `sadd key value 1, [value 2, ...]` 
	- 社交关系 
		 + `sinter key [key, ...]`
	    + `sunion  key [key, ...]`
	    + `sdiff key [key, ...]`

### 5、ZSet 类型操作
ZSet 是有序集合, Redis 可以把一堆通过权重排序的数据当做一个 Value 存储起来
- 增删改查
	- 新增 `Zadd key 权重 value 权重 value` 
	- 查询
		+ 查询指定排名范围内的数据 `Zrange key start end` 
		+ 查询指定权重范围内的数据 `Zrangebyscore key 权重权重` 
		+ 查询指定元素权重 `Zscore key element` 
		+ 查询指定权重范围内元素个数 `Zcount key minscore maxscore` 
		+ 查询集合中元素个数 `Zcard key` 
	- 删除
		+ 删除指定元素 `zrem key value`
		+ 删除指定排名范围元素 `zremrangbyrank key start end`
		+ 删除指定权重范围元素 `zremrangbyscore key minscore maxscore`
- 其它操作
	- 增加或减少元素权重 `zincrby key score element`
	- 从高到低的操作 `ZREVRANGE names 0 -1`
- 应用场景
	- 存储排行榜数据
## 四、Redis 的 Java 客户端
### 1. Jedis
#### 1.1 初体验
1) 引入依赖：
	```xml
	<!--jedis-->
	<dependency>
	    <groupId>redis.clients</groupId>
	    <artifactId>jedis</artifactId>
	    <version>3.7.0</version>
	</dependency>
	<!--单元测试-->
	<dependency>
	    <groupId>org.junit.jupiter</groupId>
	    <artifactId>junit-jupiter</artifactId>
	    <version>5.7.0</version>
	    <scope>test</scope>
	</dependency>
	```
1) 测试：
	```java
	public class JedisTest {
	  private Jedis jedis;
	
	  @BeforeEach
	  void setup() {
	    // 1. 建立连接
	    // jedis = new Jedis("127.0.0.1", 6379);
	    jedis = JedisConnectionFactory.getJedis();
	    // 2. 设置密码
	    jedis.auth("admin");
	    // 3. 选择库
	    jedis.select(0);
	  }
	
	  @Test
	  void testString() {
	    // 存数据 String
	    String result = jedis.set("gender", "man");
	    System.out.println("result = " + result);
	    // 取数据
	    String name = jedis.get("name");
	    System.out.println("name = " + name);
	  }
	
	  @Test
	  void testHash() {
	    // 存数据 Hash
	    jedis.hset("heima:user:2", "name", "fishx");
	    jedis.hset("heima:user:2", "gender", "man");
	    // 取数据
	    Map<String, String> map = jedis.hgetAll("heima:user:2");
	    System.out.println("map = " + map);
	  }
	
	  @Test
	  void testDel() {
	    long b = jedis.del("heima:user:2");
	    System.out.println("返回结果: " + b);
	    System.out.println(b > 0 ? "删除成功" : "删除失败");
	  }
	
	  @AfterEach
	  void tearDown() {
	    if (jedis != null) jedis.close();
	  }
	}
	```

 #### 1.2 创建 Jedis 的连接池
```java
public class JedisConnectionFactory {
  private static final JedisPool jedisPool;

  static {
    // 配置连接池
    JedisPoolConfig poolConfig = new JedisPoolConfig();
    poolConfig.setMaxTotal(8);
    poolConfig.setMaxIdle(8);
    poolConfig.setMinIdle(1);
    poolConfig.setMaxWaitMillis(1000);
    // 创建连接对象
    jedisPool = new JedisPool(poolConfig, "127.0.0.1", 6379, 1000, "admin");
  }

  public static Jedis getJedis() {
    return jedisPool.getResource();
  }
}
```

### 2. SpringDataRedis 
1) 搭建 SpringBoot 环境
`pom.xml` 添加依赖
```xml
  <parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <groupId>org.springframework.boot</groupId>
    <version>2.5.3</version>
  </parent>

  <dependencies>
    <!--redis依赖-->
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <!--common-pool-->
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-pool2</artifactId>
    </dependency>
    <!--Jackson依赖-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
    </dependency>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <optional>true</optional>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-configuration-processor</artifactId>
      <optional>true</optional>
    </dependency>
  </dependencies>
```
`application.yml` 添加配置信息
```yml
spring:
  redis:
    host: 127.0.0.1
    port: 6379
    password: admin
    lettuce:
      pool:
        max-active: 8  #最大连接
        max-idle: 8   #最大空闲连接
        min-idle: 0   #最小空闲连接
        max-wait: 100ms #连接等待时间
```
编写 SpringBoot 入口 `Application.java`
```java
@SpringBootApplication
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(SpringApplication.class, args);
  }
}
```

2) 测试代码
```java
@SpringBootTest
public class RedisTest {
  @Autowired
  private RedisTemplate redisTemplate;

  @Test
  void testString() {
    // save
    redisTemplate.opsForValue().set("name", "虎哥");
    // get
    Object name = redisTemplate.opsForValue().get("name");
    System.out.println("name = " + name);
  }
}
```
### 3 数据序列化器
#### 3.1 自定义 RedisTemplate
RedisTemplate 可以接收任意 Object 作为值写入 Redis, 只不过写入前会把 Object 序列化为字节形式，默认是采用 JDK 序列化. 
可读性差, 内存占用较大
我们可以自定义 RedisTemplate 的序列化方式
- `config.RedisConfig` 注入自定义 `RedisTemplate` bean 并添加 Json 自动序列化
	```java
	@Configuration
	public class RedisConfig {
	  @Bean
	  public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
	    // 创建RedisTemplate对象
	    RedisTemplate<String, Object> template = new RedisTemplate<>();
	    // 设置连接工厂
	    template.setConnectionFactory(connectionFactory);
	    // 创建JSON序列化工具
	    GenericJackson2JsonRedisSerializer jsonRedisSerializer = new GenericJackson2JsonRedisSerializer();
	    // 设置key的序列化
	    template.setKeySerializer(RedisSerializer.string());
	    template.setHashKeySerializer(RedisSerializer.string());
	    // 设置value的序列化
	    template.setValueSerializer(jsonRedisSerializer);
	    template.setHashValueSerializer(jsonRedisSerializer);
	    // 返回
	    return template;
	  }
	}
	```
- 测试代码
	```java
	@SpringBootTest
	public class RedisTest {
	  @Autowired
	  private RedisTemplate<String, Object> redisTemplate;
	
	  @Test
	  void testSaveUser() {
	    User user = new User("fishx", "男", 21, new BigDecimal("11000.50"));
	    // save
	    redisTemplate.opsForValue().set("user:3", user);
	    // get
	    User o = (User) redisTemplate.opsForValue().get("user:3");
	    System.out.println("o = " + o);
	  }
	}
	```
	
![[Pasted image 20230620170906.png]]
整体可读性有了很大提升，并且能将 Java 对象自动的序列化为 JSON 字符串，并且查询时能自动把 JSON 反序列化为 Java 对象。不过，其中记录了序列化时对应的 class 名称，<u>目的是为了查询时实现自动反序列化。这会带来<font color="#ff0000">额外的内存开销</font></u>。

#### 3.2 StringRedisTemplate
在反序列化时知道对象的类型，JSON 序列化器会将类的 class 类型写入 json 结果中，存入 Redis，会带来额外的内存开销
不借助默认的序列化器，手动控制序列化的动作, 只采用 <font color="#ff0000">String 的序列化器</font>.
这样，在存储 value 时，我们就不需要在内存中就不用多存储数据，从而节约我们的内存空间

SpringDataRedis 提供了 RedisTemplate 的子类：StringRedisTemplate
它的 key 和 value 的序列化方式默认就是 String 方式

```java
@SpringBootTest
public class RedisStringTest {
  @Autowired
  private StringRedisTemplate stringRedisTemplate;

  @Test
  void testSaveUser() {
    // save
    User user = new User("晓霞", "女", 20, new BigDecimal("9000.50"));
    // 手动序列化
    stringRedisTemplate.opsForValue().set("user:4", JSON.toJSONString(user));

    // get
    String jsonUser = stringRedisTemplate.opsForValue().get("user:4");
    // 手动反序列化
    User o = JSON.parseObject(jsonUser, User.class);
    System.out.println("o = " + o);
  }

 @Test
  void testHash() {
    stringRedisTemplate.opsForHash().put("user:10", "name", "fish");
    stringRedisTemplate.opsForHash().put("user:10", "age", "21");

    Map<Object, Object> entries = stringRedisTemplate.opsForHash().entries("user:10");
    System.out.println("entries = " + entries);
  }
}
```
#### 3.3 总结
- RedisTemplate 的两种序列化实践方案：
	* 方案一：
	  * 自定义 RedisTemplate
	  * 修改 RedisTemplate 的序列化器为 `GenericJackson2JsonRedisSerializer`
	* 方案二：
	  * 使用 StringRedisTemplate
	  * 写入 Redis 时，手动把对象序列化为 JSON
	  * 读取 Redis 时，手动把读取到的 JSON 反序列化为对象

## 五、发布订阅
在发布订阅中有三个角色: 发布者 (publisher)/订阅者 (subscriber)/频道 (channel)
只要发布者将消息发送到对应的频道中, 那么所有的订阅者都能收到这个消息, 这个就是 Redis 的发布订阅
- 订阅频道
    + `subscribe channel [channel …]`
- 发布消息
    + `publish channel message`
- 退订频道
    + `unsubscribe [channel [channel …]]`
## 六、数据持久化
- 数据持久化就是将内存中的数据写入到磁盘中
- 默认情况下 Redis 是将数据保存在内存中的, 保存在内存中的数据有一个特点, 那就是机器重启之后数据就会丢失
- 所以为了避免服务器重启死机等问题发生的时候, Redis 中保存的数据丢失, Redis 提供了数据持久化功能

![[Pasted image 20230619201543.png]]
### 1. RDB (快照)
 将内存中所有内容写入到一个文件中
 - 触发生成 RDB 三种机制
	- 手动执行 save 命令
	    + 同步执行
	    + 如果已经存在旧的 RDB 文件, 会利用新的覆盖旧的
	- 手动执行 bgsave 命令
	    + 异步执行
	    + 如果已经存在旧的 RDB 文件, 会利用新的覆盖旧的
	- 通过配置文件自动生成
	    + 通过配置文件指定自动生成条件, 一旦满足条件就会自动执行 bgsave 生成 RDB 文件
			```shell
			dir ./                  #RDB文件保存的路径
			dbfilename dump.rdb     #RDB文件的名称
			#save 900 1              #自动生成条件
			#save 300 10
			#save 60  10000
			stop-writes-on-bgsave-error yes #bgsave发生错误是否停止写入
			rdbcompression yes              #是否采用压缩模式写入
			rdbchecksum yes                 #是否对生成的RDB文件进行校验
			```
		- 自动生成弊端
		    + 无法控制生成的频率, 如果频率过高会导致性能消耗较大 
		- 推荐配置
			```shell
			dir /rdbdiskpath                #由于备份是比较占用磁盘空的, 所以推荐使用一个比较大的磁盘路径
			dbfilename dump-${prot}.rdb     #由于一台服务器上可能部署多个Redis, 所以可以给RDB文件添加端口号作为区分
			stop-writes-on-bgsave-error yes #bgsave发生错误是否停止写入
			rdbcompression yes              #是否采用压缩模式写入
			rdbchecksum yes                 #是否对生成的RDB文件进行校验
			```
- RDB 存在的问题
	- 不可控、数据丢失
		+ 服务器宕机之前的数据, 如果未写入 RDB 文件就会丢失	
	 - 耗时、耗性能
		 + RDB 是一次性把内存中所有的内容写入到磁盘中, 是一个比较重的操作
		 + 如果需要写入的数据比较多, 那么就比较耗时
	    + RDB 每次都是把内存中的所有内容全部写入到磁盘中,
	      哪怕内存中的数据已经写入过了也会再次完整写入,
	      重复写入相同的数据, 也比较浪费 I/O 资源
	    + 如果通过 save 命令写入, 会阻塞后续命令执行
	      如果是通过 bgsave 写入, 不会阻塞后续命令执行,
	      会开启子进程去专门负责写入, 但是开启子进程会消耗内存空间

### 2. AOF (日志)
将每一条操作 Redis 的指令都写入到文件中
- 触发生成 AOF 三种机制
	- `always`
	    + 每条命令都写入一条命令到文件中
	    + 优点: 不会丢失数据
	    + 缺点: 磁盘 I/O 消耗较大
	- `everysec`
	    + 每隔 1 秒写入一次, 也就是先收集 1 秒钟的命令, 然后再一次性写入到文件中
	    + 优点: 磁盘 I/O 消耗相对较小
	    + 缺点: 可能丢失 1 秒数据
	- `no`
	    + 让操作系统决定什么时候写入, 操作系统想什么时候写入就什么时候写入
	    + 不可控, 可能丢失大量数据
- AOF 文件重写
	- 随着时间的推移 AOF 文件会越来越大
		- 文件越来越大带来的问题就是
		    - 磁盘消耗越来越大
		    - 写入速度会越来越慢
		    - 恢复的时间越来越慢 
	- AOF 提供了重写的机制
		 - 我们可以对 AOF 文件中保存的内容进行优化
		    - 从而降低文件的体积
		    - 从而提升文件的恢复速度
		- 在 AOF 的重写机制中
		    - 可以将自动去除冗余命令
		    - 可以自动删除没有用的命令
	- 触发 AOF 重写两种机制
		- `bgrewriteaof` 命令
			+ 开启一个新的子进程, 根据内容中的数据生成命令写入到 AOF 文件中
		- 配置文件设置 
			```shell
			auto-aof-rewrite-min-size 200mb   #AOF文件体积达到多大时进行重写
			auto-aof-rewrite-percentage 100   #对比上一次重写后, 增长了百分之多少再次进行重写
			#例如上一次重写后大小是100MB, 如果设置为50, 那么下一次就是增长0.5倍(150M)之后再重写
			#例如上一次重写后大小是100MB, 如果设置为100, 那么下一次就是增长两倍(200M)之后再重写
			```
- AOF 推荐配置
	```shell
	appendonly yes                           #是否使用AOF
	appendfilename "appendonly-${prot}.aof"  #AOF文件名称
	appendfsync everysec                     #写入命令的同步机制
	dir /rdbdiskpath                         #保存AOF文件路径
	auto-aof-rewrite-min-size 64mb           #AOF文件重写体积
	auto-aof-rewrite-percentage 100          #AOF文件增长率
	no-appendfsync-on-rewrite yes            #AOF重写时是否正常写入当前操作的命令
	```
### 3. RDB 和 AOF 对比
- AOF 优先级高于 RDB
    + 如果 Redis 服务器同时开启了 RDB 和 AOF, 那么宕机重启之后会优先从 AOF 中恢复数据
- RDB 体积小于 AOF
    + 由于 RDB 在备份的时候会对数据进行压缩, 而 AOF 是逐条保存命令, 所以 RDB 体积比 AOF 小
- RDB 恢复速度比 AOF 恢复速度快
    + 由于 AOF 是通过逐条执行命令的方式恢复数据, 而 RDB 是通过加载预先保存的数据恢复数据
      所以 RDB 的恢复速度比 AOF 快
- AOF 数据安全性高于 RDB
    + 由于 AOF 可以逐条写入命令, 而 RDB 只能定期备份数据, 所以 AOF 数据安全性高于 RDB
- 所以综上所述, 两者各有所长, 两者不是替代关系而是互补关系

## 七、短信登录
### 1. 基于 Session 实现登录
![[Pasted image 20230725092754.png]]
```java
@Override
  public Result sendCode(String phone, HttpSession session) {
    // 1. 校验手机号
    if (RegexUtils.isPhoneInvalid(phone)) {
      // 2. 如果不符合,返回错误信息
      return Result.fail("手机号格式错误!");
    }
    // 3. 符合,生成验证码
    String code = RandomUtil.randomNumbers(6);
    // 4. 保存验证码到session
    session.setAttribute("code", code);
    // 5. 发送验证码
    log.debug("发送短信验证码成功,验证码: {}", code);
    // 返回
    return Result.ok();
  }

  @Override
  public Result login(LoginFormDTO loginForm, HttpSession session) {
    String phone = loginForm.getPhone();
    // 1. 校验手机号
    if (RegexUtils.isPhoneInvalid(phone)) {
      return Result.fail("手机号格式错误!");
    }
    // 2. 校验验证码
    Object cacheCode = session.getAttribute("code");
    if (cacheCode == null || !cacheCode.toString().equals(loginForm.getCode())) {
      // 3. 不一致,报错
      return Result.fail("验证码校验失败!");
    }
    // 4. 一致,根据手机号查询用户
    User user = query().eq("phone", phone).one();
    // 5. 判断用户是否存在
    if (user == null) {
      // 6. 不存在,创建新用户并保存
      user = createUserWithPhone(phone);
    }
    // 7. 保存用户信息到session
    session.setAttribute("user", BeanUtil.copyProperties(user, UserDTO.class));
    return Result.ok();
  }
```
- <font color="#ff0000">集群 session 共享问题</font>
	- 多台 Tomcat 并不共享 session 存储空间, 当请求切换到不同 Tomcat 服务时导致数据丢失问题

### 2. 基于 Redis 实现登录
![[Pasted image 20230725143437.png]]
![[Pasted image 20230725143810.png]]
业务逻辑:
```java
  @Resource
  private StringRedisTemplate stringRedisTemplate;

  @Override
  public Result sendCode(String phone, HttpSession session) {
    // 1. 校验手机号
    if (RegexUtils.isPhoneInvalid(phone)) {
      // 2. 如果不符合,返回错误信息
      return Result.fail("手机号格式错误!");
    }
    // 3. 符合,生成验证码
    String code = RandomUtil.randomNumbers(6);
    // 4. 保存验证码到redis
    stringRedisTemplate.opsForValue()
        .set(LOGIN_CODE_KEY + phone, code, LOGIN_CODE_TTL, TimeUnit.MINUTES);
    session.setAttribute("code", code);
    // 5. 发送验证码
    log.debug("发送短信验证码成功,验证码: {}", code);
    // 返回
    return Result.ok();
  }

  @Override
  public Result login(LoginFormDTO loginForm, HttpSession session) {
    String phone = loginForm.getPhone();
    // 1. 校验手机号
    if (RegexUtils.isPhoneInvalid(phone)) {
      return Result.fail("手机号格式错误!");
    }
    // 2. 从Redis获取验证码,校验验证码
    String cacheCode = stringRedisTemplate.opsForValue().get(LOGIN_CODE_KEY + phone);
    if (cacheCode == null || !cacheCode.equals(loginForm.getCode())) {
      // 3. 不一致,报错
      return Result.fail("验证码校验失败!");
    }
    // 4. 一致,根据手机号查询用户
    User user = query().eq("phone", phone).one();
    // 5. 判断用户是否存在
    if (user == null) {
      // 6. 不存在,创建新用户并保存
      user = createUserWithPhone(phone);
    }
    // 7. 保存用户信息到Redis
    //   7.1 随机生成token,作为登录令牌
    String token = UUID.randomUUID().toString(true);
    //   7.2 将User对象转为HashMap存储
    UserDTO userDTO = BeanUtil.copyProperties(user, UserDTO.class);
    Map<String, Object> map = BeanUtil.beanToMap(userDTO, new HashMap<>(),
        CopyOptions.create()
            .setIgnoreNullValue(true)
            .setFieldValueEditor((fileName, fileValue) -> fileValue.toString()));
    //   7.3 存储
    String tokenKey = LOGIN_USER_KEY + token;
    stringRedisTemplate.opsForHash().putAll(tokenKey, map);
    //   7.4 设置有效期
    stringRedisTemplate.expire(tokenKey, LOGIN_USER_TTL, TimeUnit.MINUTES);
    // 8. 返回Token
    return Result.ok(token);
  }
```
登录拦截器:
```java
public class LoginInterceptor implements HandlerInterceptor {
  private StringRedisTemplate redisTemplate;

  // 登录拦截器不是由IOC自动注入,而是手动@Bean注入
  public LoginInterceptor(StringRedisTemplate redisTemplate) {
    this.redisTemplate = redisTemplate;
  }

  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    // 1. 获取请求头中的token
    String token = request.getHeader("Authorization");
    if (StrUtil.isBlank(token)) {
      // 不存在 拦截
      response.setStatus(401);
      return false;
    }
    // 2. 基于token获取redis中的用户
    String tokenKey = RedisConstants.LOGIN_USER_KEY + token;
    System.out.println(tokenKey);
    Map<Object, Object> userMap = redisTemplate.opsForHash().entries(tokenKey);
    // 3. 判断用户是否存在
    if (userMap.isEmpty()) {
      // 4. 不存在 拦截
      response.setStatus(401);
      return false;
    }
    // 5. 将查询到的Hash数据转为UserDTO对象
    UserDTO user = BeanUtil.fillBeanWithMap(userMap, new UserDTO(), false);
    // 6. 存在,保存用户信息到ThreadLocal
    UserHolder.saveUser(user);
    // 7. 刷新token有效期
    redisTemplate.expire(tokenKey, RedisConstants.LOGIN_USER_TTL, TimeUnit.MINUTES);
    // 8. 放行
    return true;
  }

  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    UserHolder.removeUser();
  }
}
```
```java
@Configuration
public class MvcConfig implements WebMvcConfigurer {
  @Autowired
  private StringRedisTemplate redisTemplate;

  @Bean
  public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor(redisTemplate))
            .excludePathPatterns(
                "/user/code", "/user/login", "/blog/hot",
                "/shop/**", "/shop-type/**", "/upload/**",
                "/voucher/**"
            );
      }
    };
  }
}
```
### 3. 解决登录状态刷新问题
![[Pasted image 20230728151724.png]]
```java
public class RefreshTokenInterceptor implements HandlerInterceptor {
  private StringRedisTemplate redisTemplate;

  // 登录拦截器不是由IOC自动注入,而是手动@Bean注入
  public RefreshTokenInterceptor(StringRedisTemplate redisTemplate) {
    this.redisTemplate = redisTemplate;
  }

  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    // 1. 获取请求头中的token
    String token = request.getHeader("Authorization");
    if (StrUtil.isBlank(token)) return true;
    // 2. 基于token获取redis中的用户
    String tokenKey = RedisConstants.LOGIN_USER_KEY + token;
    System.out.println(tokenKey);
    Map<Object, Object> userMap = redisTemplate.opsForHash().entries(tokenKey);
    // 3. 判断用户是否存在
    if (userMap.isEmpty()) return true;
    // 5. 将查询到的Hash数据转为UserDTO对象
    UserDTO user = BeanUtil.fillBeanWithMap(userMap, new UserDTO(), false);
    // 6. 存在,保存用户信息到ThreadLocal
    UserHolder.saveUser(user);
    // 7. 刷新token有效期
    redisTemplate.expire(tokenKey, RedisConstants.LOGIN_USER_TTL, TimeUnit.MINUTES);
    // 8. 放行
    return true;
  }

  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    UserHolder.removeUser();
  }
}
```
```java
public class LoginInterceptor implements HandlerInterceptor {
  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    // 判断是否需要拦截(也就是判断ThreadLocal中是否存在)
    if (UserHolder.getUser() == null) {
      // 不存在 => 设置状态码,拦截
      response.setStatus(401);
      return false;
    }
    return true;
  }
}
```
```java

@Configuration
public class MvcConfig implements WebMvcConfigurer {
  @Autowired
  private StringRedisTemplate redisTemplate;

  @Bean
  public WebMvcConfigurer webMvcConfigurer() {
    return new WebMvcConfigurer() {
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new RefreshTokenInterceptor(redisTemplate))
            .addPathPatterns("/**").order(0);
        registry.addInterceptor(new LoginInterceptor())
            .excludePathPatterns(
                "/user/code", "/user/login", "/blog/hot",
                "/shop/**", "/shop-type/**", "/upload/**",
                "/voucher/**"
            ).order(1);
      }
    };
  }
}
```
## 八、Redis 缓存
![[Pasted image 20230728163555.png]]
缓存就是数据交换的缓冲区（称作 Cache [ kæʃ ] ），是存贮数据的临时地方，一般读写性能较高
![[Pasted image 20230728163733.png]]
![[Pasted image 20230728163800.png]]
### 8.1 添加 Redis 缓存
![[Pasted image 20230728164109.png]]
练习: 给店铺类型查询业务添加缓存
```java
  @Resource
  private StringRedisTemplate stringRedisTemplate;

  @Override
  public Result getShopTypeList() {
    // 1. 从redis中查询商铺类型列表缓存
    String listJson = stringRedisTemplate.opsForValue().get(RedisConstants.CACHE_TYPE_LIST_KEY);
    // 2. 判断redis中是否存在
    if (StrUtil.isNotBlank(listJson)) {
      // 3. redis中存在,返回结果
      return Result.ok(JSONUtil.toList(listJson, ShopType.class));
    }
    // 4. redis中不存在,就查询数据库
    List<ShopType> typeList = query().orderByAsc("sort").list();
    // 5. 再写入redis缓存中
    stringRedisTemplate.opsForValue().set(RedisConstants.CACHE_TYPE_LIST_KEY, JSONUtil.toJsonStr(typeList));
    return Result.ok(typeList);
  }
```
### 8.2 缓存更新策略
|       |  内存淘汰                                 |  超时剔除                          |  主动更新                 |
|:-----:|:-------------------------------------:|:------------------------------:|:---------------------:|
| 说明    | Redis 自带无需维护 (当内存不足时自动淘汰部分数据, 下次查询时更新缓存) | 给缓存数据添加 TTL 时间, 到期后自动删除. 下次查询时更新缓存 | 编写业务逻辑, 在修改数据库的同时, 更新缓存 |
| 一致性   | 差                                     | 一般                             | 好                     |
| 维护成本  | 无                                     | 低                              | 高                     |  

- 业务场景:
	- 低一致性需求:
		- 使用内存淘汰机制 
			- 例如: 店铺类型的查询缓存
	- 高一致性需求:
		- 主动更新, 并以超时剔除作为兜底方案
			- 例如: 店铺详情查询的缓存

#### 主动更新策略
- 由缓存的调用者, 在更新数据库的同时缓存
	- 更新数据库时让缓存失效, 查询时再更新缓存
	- 单体系统将缓存与数据库操作放在一个事务; 分布式系统利用 TCC 等分布式事务方案
	- 先操作数据库,再删除缓存

#### 最佳实践方案
1. 低一致性需求: 使用 Redis 自带的内存淘汰机制
2. 高一致性需求: 主动更新, 并以超时剔除作为兜底方案
	- 读操作: 
		- 缓存命中则直接返回
		- 缓存未命中则查询数据库, 并写入缓存, 设定超时时间
	- 写操作:
		- 先写数据库, 然后再删除缓存
		- 要确保数据库与缓存操作的原子性

```java
  @Override
  @Transactional
  public Result update(Shop shop) {
    Long id = shop.getId();
    if (id == null) Result.fail("店铺id不能为空!");
    // 1. 更新数据库
    updateById(shop);
    // 2. 删除缓存
    stringRedisTemplate.delete(RedisConstants.CACHE_SHOP_KEY + id);
    return Result.ok();
  }
```

### 8.3 缓存穿透
缓存穿透是指客户端请求的数据在缓存中和数据库中都不存在，这样缓存永远不会生效，这些请求都会打到数据库。
![[Pasted image 20230729195316.png]]
- 常见的解决方案有两种:
	- 缓存空对象
		- 优点: 实现简单, 维护方便
		- 缺点: 额外的内存消耗, 可能造成短期的不一致
	- 布隆过滤
		- 优点: 内存占用较少, 没有多余 key
		- 缺点: 实现复杂, 存在误判可能

![[Pasted image 20230729200042.png]]
```java
  @Override
  public Result queryById(Long id) {
    String key = RedisConstants.CACHE_SHOP_KEY + id;
    // 1. 从redis查询商铺缓存
    String shopJson = stringRedisTemplate.opsForValue().get(key);
    // 2. 判断是否存在
    if (StrUtil.isNotBlank(shopJson)) {
      // 3. redis存在,直接返回
      return Result.ok(JSONUtil.toBean(shopJson, Shop.class));
    }
    // 判断命中的是否是空值,返回错误信息
    if (shopJson != null) return Result.fail("店铺信息不存在!");
    // 4. redis不存在,根据id查询数据库
    Shop shop = getById(id);
    // 5. mysql不存在
    if (shop == null) {
      // 将空值写入redis
      stringRedisTemplate.opsForValue().set(key, "", RedisConstants.CACHE_NULL_TTL, TimeUnit.MINUTES);
      // 返回错误
      return Result.fail("店铺不存在!");
    }
    // 6. mysql存在,写入redis
    stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop),
        RedisConstants.CACHE_SHOP_TTL, TimeUnit.MINUTES);
    return Result.ok(shop);
  }
```
- 缓存穿透的解决方案有哪些？
	- 缓存 null 值
	- 布隆过滤
	- 增强 id 的复杂度，避免被猜测 id 规律
	- 做好数据的基础格式校验
	- 加强用户权限校验
	- 做好热点参数的限流

### 8.4 缓存雪崩
缓存雪崩是指在同一时段大量的缓存 key 同时失效或者 Redis 服务宕机，导致大量请求到达数据库，带来巨大压力。
![[Pasted image 20230729201730.png]]
- 解决方案：
	- 给不同的 Key 的 TTL 添加随机值
	- 利用 Redis 集群提高服务的可用性
	- 给缓存业务添加降级限流策略
	- 给业务添加多级缓存

### 8.5 缓存击穿
缓存击穿问题也叫热点 Key 问题，就是一个被高并发访问并且缓存重建业务较复杂的 key 突然失效了，无数的请求访问会在瞬间给数据库带来巨大的冲击
![[Pasted image 20230729202118.png]]
- 常见的解决方案有两种：
	- 互斥锁
	- 逻辑过期

![[Pasted image 20230729213935.png]]

| 解决方案 |                   优点                   |                   缺点                   |
|:--------:|:----------------------------------------:|:----------------------------------------:|
|  互斥锁  | 没有额外的内存消耗、保证一致性、实现简单 | 线程需要等待，性能受影响、可能有死锁风险 |
| 逻辑过期 |          线程无需等待，性能较好          |  不保证一致性、有额外内存消耗、实现复杂  |

#### 基于互斥锁解决击穿问题
![[Pasted image 20230731100246.png]]

```java
public Shop queryWithMutex(Long id) {
    String key = RedisConstants.CACHE_SHOP_KEY + id;
    // 1. 从redis查询商铺缓存
    String shopJson = stringRedisTemplate.opsForValue().get(key);
    // 2. 判断是否存在
    if (StrUtil.isNotBlank(shopJson)) {
      // 3. redis存在,直接返回
      return JSONUtil.toBean(shopJson, Shop.class);
    }
    // 判断命中的是否是空值,返回错误信息
    if (shopJson != null) return null;

    // redis不存在, 实现缓存重建
    // (1) 获取互斥锁
    String lockKey = "lock:shop:" + id;
    Shop shop = null;
    try {
      boolean isLock = tryLock(lockKey);
      // (2) 判断是否持有锁
      if (!isLock) {
        // (3) 失败,则休眠重试
        Thread.sleep(50);
        return queryWithMutex(id);
      }
      // (4) 成功,根据id查询数据库
      shop = getById(id);
      // TODO:模拟重建延迟
      Thread.sleep(200);
      // 5. mysql不存在
      if (shop == null) {
        // 将空值写入redis
        stringRedisTemplate.opsForValue().set(key, "", RedisConstants.CACHE_NULL_TTL, TimeUnit.MINUTES);
        // 返回错误
        return null;
      }
      // 6. mysql存在,写入redis
      stringRedisTemplate.opsForValue().set(key, JSONUtil.toJsonStr(shop), RedisConstants.CACHE_SHOP_TTL, TimeUnit.MINUTES);
    } catch (InterruptedException e) {
      throw new RuntimeException(e);
    } finally {
      // 7. 释放互斥锁
      unLock(lockKey);
    }
    return shop;
  }
```


