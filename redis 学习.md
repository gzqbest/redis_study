## redis 学习

### redis 概述

redis 是 远程字典服务器，是一个高性能的key-value 内存数据库，支持事务，发布|订阅，lua脚本等,可以持久化到内存中

redis 比 mysql 查询快 ，基于内存，基于 key 查询，nosql 数据库 ,没有关系约束；redis 基于内存可以处理场景： 点赞、排行榜、计算器，抢红包；redis 自身数据可持久化到磁盘；redis 可以分布锁、实现队列（排行榜）

### redis安装配置

+ 下载redis7.0-tar

+ 在linux 上 安装 gcc 

+ 在opt 目录下操作

  ~~~shell
  tar -zxvf redis-xx.tar
  cd redis/
  # 执行make命令
  make && make install
  ~~~

  ![image-20250723222618444](https://raw.githubusercontent.com/gzqbest/img/master/20250723222627525.png)

  表明安装成功，并安装到linux 下的/usr/local/bin下

  ![image-20250723222749879](https://raw.githubusercontent.com/gzqbest/img/master/20250723222750053.png)

  redis-benchmark ： 性能测试工具

  redis-check-aof\rdb持久化，修复对应的持久化文件

  redis-cli: 客户端操作

  redis-server: 服务端

​		启动 redis 服务器

        ~~~shell
        # 修改redis 配置文件redis.conf
        # 后台启动
        daemonize no -> daemonize yes
        # 访问权限关闭
        protected-mode yes -> protected-mode no
        # 允许外网访问
        bind 127.0.0.1 -> ##
        # 添加redis密码
        requirepass xxx
        # 启动redis 
        redis-server redis.conf
        # 连接redis
        redis-cli -a password -p 6379
        # 关闭redis server
        redis-cli -a password SHUTDOWN -p

 ### redis 十大数据类型

#### 常用操作

~~~shell
keys * # 获取所有的key
exists k1 # 判断k1 是否存在 1存在
del k1 # 原子阻塞删除
expire k1  3 # 设置超时时间
help @type # 获取对应类型操作命令
~~~



#### string类型

~~~shell
set k1 hello;
get k1
> "hello"

set k1 aa nx get ex 10 # nx 如果不存在，则创建; 如果存在则不修改
set k1 cc xx get ex 20 # xx 如果存在，则只改键值

expire k1 20
set k1 bb keepttl # 保留之前的设置的过期时间

## 设置多个键值对
mset k1 v1 k2 v2
msetnx k1 v1 k2 v2 (键不存在时才创建--整体，要么全部成功，要么全部失败)

## 数字增减
set k1 100
incr k1
incr k1 3 #跨的步伐 3
decr k1 3

## 获取字符串长度 或增加
strlen k1 
append k1 aaa
## 分布式锁
setnx k3 v1 # 不存在时创建
setex k3 10(3) v2 # 
~~~



