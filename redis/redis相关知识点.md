# redis相关知识点

## 01_redis入门

### 一、Redis简介

#### 	中国被世界黑的最惨的一天  

​		2007年10月30日，北京奥运会门票面向境内公众第二阶段预售正式启动。上午一开始，公众提交申请空前
​		踊跃。上午9时至10时，官方票务网站的浏览量达到了800万次，票务呼叫中心热线从9时至10时的呼入量超
​		过了380万人次。由于瞬间访问数量过大，技术系统应对不畅，造成很多申购者无法及时提交申请，为此北
​		京奥组委票务中心对广大公众未能及时、便捷地实现奥运门票预订表示歉意  。

#### 	不可回避的问题  

​		奥运会门票预售系统开放第一天，上午9点正式开始售票到中午12点， 3个小时内，票务网站被浏览次数达到2000万次。  

![image-20200721191956129](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200721191956129.png)			 

#### 	一个神奇的网站  

​	![image-20200721192117242](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200721192117242.png)

#### 	大型翻车现场  

​		 ![image-20200721192218318](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200721192218318.png)

#### 	问题现象  

​		海量用户

​		高并发

#### 	罪魁祸首——关系型数据库  

​		性能瓶颈：磁盘IO性能低下  

​		扩展瓶颈：数据关系复杂，扩展性差，不便于大规模集群  

#### 	解决思路  

​		降低磁盘IO次数，越低越好  -------内存储存

​		去除数据间关系，越简单越好  --------不存储关系，仅存储数据

​		合起来就是Nosql

#### 	Nosql

​		NoSQL：即 Not-Only SQL（ 泛指非关系型的数据库），作为关系型数据库的补充。  

​		作用： 应对基于海量用户和海量数据前提下的数据处理问题  

​		特征：  

​			可扩容，可伸缩  

​			大数据量下高性能  

​			灵活的数据模型  

​			高可用  

​		常见 Nosql 数据库：  

​			Redis  

​			memcache  

​			HBase  

​			MongoDB  

#### 解决方案（电商场景）

- MySQL：
  - 商品基本信息：名称、价格、厂商
- MongoDB：
  - 商品附加信息：描述、详情、评论
- 分布式文件系统：
  - 图片信息
- ES、Lucene、solr：
  - 搜索关键字
- Redis、mencache、tair：
  - 热点信息：高频、波段性

#### Rdis

​	**概念**：Redis (REmote DIctionary Server)是用C语言开发的一个开源的高性能键值对（key-value）数据库。

​	**特征**：

		1. 数据间没有必然的关联关系  
  		2. 内部采用单线程机制进行工作  

  3. 高性能。官方提供测试数据， 50个并发执行100000 个请求,读的速度是110000 次/s,写的速度是81000次/s  

  4. 多数据类型支持  

     字符串类型 string  

     列表类型 list  

     散列类型 hash  

     集合类型 set  

     有序集合类型 sorted_set  

5. 持久化支持。可以进行数据灾难恢复  

#### Redis 的应用  

- 为热点数据加速查询（主要场景），如热点商品、热点新闻、热点资讯、推广类等高访问量信息等  
- 任务队列，如秒杀、抢购、购票排队等  
- 即时信息查询，如各位排行榜、各类网站访问统计、公交到站信息、在线人数信息（聊天室、网站）、设备信号等  
- 时效性信息控制，如验证码控制、投票控制等  
- 分布式数据共享，如分布式集群架构中的 session 分离  
- 消息队列  
- 分布式锁  





### 二、Redis的下载与安装

#### Redis 的下载  

- Linux 版  （适用于企业级开发）
  - Redis 高级开始使用  
  - 以4.0 版本作为主版本  
- Windows 版本  ( 适合零基础学习  )
  - Redis 入门使用  
  - 以 3.2 版本作为主版本  
  - 下载地址： https://github.com/MSOpenTech/redis/tags  

#### 安装 Redis  

​	![image-20200722213936737](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200722213936737.png)

​	**核心文件**：

​		redis-server.exe                                    服务器启动命令

​		redis-cli.exe											命令行客户端

​		redis.windows.conf							   redis核心配置文件

​		redis-benchmark.exe							性能测试工具

​		redis-check-aof.exe								AOF文件修复工具

​		redis-check-dump.exe						    RDB文件检查工具（快照持久化文件）

#### 启动 Redis

​	**服务器启动**

​		![image-20200722214440445](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200722214440445.png)

​	**客户端连接**

​		![image-20200722214610874](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200722214610874.png)

### 三、Redis的基本操作

#### 命令行模式工具使用思考  

- 功能性命令  
- 清除屏幕信息  
- 帮助信息查阅  
- 退出指令  

#### 信息添加  

- 功能：设置 key， value 数据  

- 命令  

  ```java
  set key value
  ```

- 范例  

  ```
  set name zhangsan
  ```

#### 信息查询  

- 功能：根据 key 查询对应的 value，如果不存在，返回空（ nil）  

- 命令  

  ```
  get key
  ```

- 范例  

  ```
  get name
  ```

#### 清除屏幕信息  

- 功能：清除屏幕中的信息  

- 命令  

  ```
  clear
  ```

#### 退出客户端命令行模式  

- 功能：退出客户端  

- 命令  

  ```
  quit
  exit
  <ESC>
  ```

#### 帮助  

- 功能：获取命令帮助文档，获取组中所有命令信息名称  

- 命令  

  ```
  help 命令名称
  help @组名
  ```

![image-20200722215600914](C:\Users\kty\AppData\Roaming\Typora\typora-user-images\image-20200722215600914.png)

### 四、总结

#### 	Redis入门

		1. Redis简介
  		2. Redis的下载与安装
  		3. Redis的基本操作
       - set/get
       - clear
       - help



## 02_redis 数据类型

### 一、数据存储类型介绍

#### 业务数据的特殊性

​	**作为缓存使用**：

​		1.原始业务功能设计

​			a. 秒杀

​			b. 618活动

​			c. 双11活动

​			d. 排队购票

​		2.运营平台监控到的突发高频访问数据  

​			突发时政要闻，被强势关注围观  

​		3.高频、复杂的统计数据  

​			a. 在线人数  

​			b. 投票排行榜  

​	**附加功能**  

​		系统功能优化或升级  

​		1.单服务器升级集群  

​		2.Session 管理  

​		3.Token 管理  

#### 	Redis 数据类型（ 5种常用）  

​		string                  String

​		hash                    HashMap

​		list                        LinkedList

​		set                        HashSet

​		sorted_set           TreeSet

### 二、string

#### redis 数据存储格式  

- redis 自身是一个 Map，其中所有的数据都是采用 key : value 的形式存储  

- 数据类型指的是存储的数据的类型，也就是 value 部分的类型， key 部分永远都是字符串  

- Redis 存储空间

  | key  | value  |
  | ---- | ------ |
  | name | 爪哇酱 |
  | age  | 101    |
  | 名称 | 数据   |
  | key  | value  |

#### string 类型

- 存储的数据：单个数据，最简单的数据存储类型，也是最常用的数据存储类型  

- 存储数据的格式：一个存储空间保存一个数据  

- 存储内容：通常使用字符串，如果字符串以整数的形式展示，可以作为数字操作使用  

- Redis 存储空间

  key1   ----------------->    zhangsan

  key2   ----------------->     1234567

#### string 类型数据的基本操作  

- 添加/修改数据  

  ```
  set key value
  ```

- 获取数据  

  ```
  get key
  ```

- 删除数据  

  ```
  del key
  ```

- 添加/修改多个数据  

  ```
  mset key1 value1 key2 value2...
  ```

- 获取多个数据  

  ```
  mget key1 key2 ...
  ```

- 获取数据字符个数（字符串长度）  

  ```
  strlen key
  ```

- 追加信息到原始信息后部（如果原始信息存在就追加，否则新建）  

  ```
  append key value
  ```

  

### 三、hash

### 四、list

### 五、set

### 六、sorted_set

### 七、数据类型实践案例

## 03_redis 通用指令

## 04_jedis

## 05_linux环境安装redis

## 06_redis 持久化

## 07_redis 事务

## 08_删除策略

## 09_redis服务器配置

## 10_高级数据类型

## 11_主从复制

## 12_哨兵模式

## 13_cluster

## 14_企业级解决方案



