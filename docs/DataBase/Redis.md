# Redis

NoSQL: Not Only SQL

Redis 是一款 key-value 型的  NoSQL 数据库

Redis 将数据存储在内存中，同时也能持久化到磁盘

特性：高性能

Redis 常用于缓存，利用内存的高效提高程序的处理速度

- 速度快
- 广泛的语言支持
- 持久化
  - RDD 全量备份
  - AOF 日志更新
- 多种数据结构
- 主从复制
- 分布式与高可用

## 安装

```bash
# dependencies
apt-get install gcc make

# download
wget http://download.redis.io/releases/redis-5.0.8.tar.gz
# unzip
tar xzf redis-5.0.8.tar.gz

# compile
cd redis-5.0.8 && make

# start server
src/redis-server

# start client
src/redis-cli

# stop server
src/redis-cli shutdown

# 查看系统的占用端口
netstat -tplun
```

## 常用配置

| 配置        | 示例                   | 说明                        |
|:------------|:----------------------|:----------------------------|
| daemonize   | daemonize yes         | 设置是否启用后台运行，默认 no |
| port        | port 3306             | 设置端口，默认 6379          |
| logfile     | logfile /path/to/log  | 设置日志文件路径             |
| dir         | dir /path/to/datafile | 设置数据文件路径             |
| databases   | databases 255         | 设置数据库数量               |
| requirepass | requirepass 123456    | 设置使用密码                 |

## 通用命令

| 命令   | 示例                | 说明                         |
|:-------|:-------------------|:-----------------------------|
| select | select 0           | 选择 0 号数据库               |
| set    | set key value      | 设置键的值                    |
| get    | get key            | 获取键的值                    |
| keys   | keys he*           | 查询匹配 Pattern 表达式的 key |
| dbsize | dbsize             | 返回 key 的总数               |
| exists | exists key         | 检查 key 是否存在             |
| del    | del key            | 删除 key                     |
| expire | expire key timeout | 设置 key 的过期时间           |
| ttl    | ttl key            | 查询 key 的剩余时间           |

## 数据类型

- 字符串型 String
  - String 最大 512MB，建议单个kv不超过100kb
- 哈希类型 Hash
- 列表类型 List
- 集合类型 Set
- 有序集合 Zset

### 字符串命令

| 命令          | 示例                          | 说明               |
|:--------------|:-----------------------------|:--------------------|
| get           | get key                      | 获取键的值          |
| set           | set key value                | 设置键的值          |
| mget          | mget key1 key2               | 一次性获取多个键的值 |
| mset          | mset key1 value1 key2 value2 | 一次性设置多个键的值 |
| del           | del key1 key2                | 删除键              |
| incr/decr     | incr/decr key                | 键值自增/自减 1     |
| incrby/decrby | incrby/decrby key n          | 键值自增/自减 n     |

### Hash 命令

| 命令    | 示例                                 | 说明                                    |
|:--------|:------------------------------------|:----------------------------------------|
| hget    | hget hdata key                      | 获取 Hash 类型数据 hdata 中 key=key 的值 |
| hset    | hset hdata key value                | 设置 Hash 类型数据 hdata 中 key=value    |
| hmget   | hmget hdata key1 key2               | 一次性获取多个键的值                     |
| hmset   | hmset hdata key1 value1 key2 value2 | 一次性设置多个键的值                     |
| hgetall | hgetall hdata                       | 获取 Hash 类型数据 hdata 中的所有键值对   |
| hdel    | hdel hdata key1 key2                | 删除 Hash 类型数据 hdata 中的指定键      |
| hexists | hexists hdata key                   | 判断 Hash 类型数据 hdata 中是否存在 key  |
| hlen    | hlen hdata                          | 获取 Hash 类型数据 hdata 中键值对的长度   |

## List 命令

- 右侧插入 rpush listkey d e f
- 左侧插入 lpush listkey c b a
- 插入结果 a b c d e f
- 右侧弹出 rpop listkey
- 左侧弹出 lpop listkey
- 范围查询 [0,n] lrange 0 n

## Set 命令

Set 是字符串的无序集合，元素唯一
ZSet 是字符串的有序集合，元素唯一

- 添加元素 sadd setkey value1 value2
- 查询元素 smembers
- 交集 sinter setkey1 setkey2
- 并集 sunion setkey1 setkey2
- 差集 sdiff setkey1 setkey2 (set1 中有，set2 中没有的元素)

- 添加元素 zadd zset1 score value (元素默认按照 score 升序排列)
- 范围查询 zrangebyscore zset minScore maxScore

## Jedis

Jedis 是 Java 语言开发的 Redis 客户端工具包，对 Redis 命令进行了封装

`protected-mode` 保护模式，开启时只允许指定 IP 的客户端进行连接
`bind` 指定允许连接的客户端 IP, `0.0.0.0` 表示允许所有 IP