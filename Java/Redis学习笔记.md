<div id="title" style="display:flex;
        justify-content:center;
        align-items:center;
        background-image: linear-gradient( 135deg, #FFF6B7 10%, #F6416C 100%);">
   <h1 align="center" style="color:#9b59b6;border-bottom:none;">Redis学习笔记</h1>
</div>



# 基础知识

> Redis 默认有16个数据库

默认使用第0个数据库

```bash
# 切换数据库
select [库id]
# 查看库大小
dbsize
# 查看数据库所有key
key *
# 清空当前数据库
flushdb
# 清空全部数据库
flushall
```



> Redis 是单线程的

6.0以后是多线程





# 基本命令

 ```bash
# 查看过期时间
ttl [key]
# 查看类型
type [key]
# 查看key是否存在
exists [key]
 ```



# 五大数据类型

## String

```bash
# 获取字符串长度
strlen [key]
# 追加字符串
append [key] [value]
# 加1
incr [key]
# 减1
decr [key]
# 步长加
incrby [key] [step]
# 步长减
decrby [key] [step]
# 获取某长度字符串
getrange [key] [start] [end]
# 范围设置字符串
setrange [key] [offset] [value]
# 设置过期时间 set with expired
setex [key] [expired] [value]
# 不存在再设置 set if not exist
setnx [key] [value]
# 同时设置多个
mset [key] [value] ... [key...] [value...]
# 同时获取多个
mget [key] ... [key...]
# 同时设置多个  原子性操作，要么一起成功，要么一起失败
msetnx [key] [value] ... [key...] [value]
# 先get再set  会先获取打印出来，再设置新的值
getset [key] [value]
```



> 保存对象时的另一个技巧

```bash
mset user:1:name zhangsan 
     user:1:age 3
     
mget user:1:name
	 user:1:age
```





## List

```bash
# 添加
lpush [list] [value]
rpush [list] [value]
# 范围取
lrange [list] [offset] [size]
# 移除
lpop [list]
rpop [list]
# 通过下标获取
lindex [list] [index]
rindex [list] [index]
# 获取长度
llen [list]
# 移除list中指定个数的value
lrem [list] [count] [value]
# 截断（保留范围内）
ltrim [list] [start] [end]
# 从一个列表转移到另一个列表
rpoplpush [list] [targetlist]
# 设置
lset [list] [index] [value]
# 插入
linsert [list] [before|after] [target_value] [value]
```



## Set

> Set中的值不能重复

```bash
# 添加
sadd [set] [value]
# 查看
smembers [set]
# 查看是否存在
sismember [set] [value]
# 获取集合中元素的个数
scard [set]
# 移除
srem [set] [value]
# 随机抽取出一个元素
srandmember [set]
# 随机删除一个元素
spop [set]
# 移到到另一个集合
smove [set1] [set2] [value]
# 差集
sdiff [set1] [set2]
# 并集
sinter [set1] [set2]
# 合并
sunion [set1] [set2]
```

## Hash

```bash
# 设置一个
hset [key] [field] [value]
# 获取一个
hget [key] [field]
# 设置多个
hmset [key] [field...] [value...]
# 获取全部
hgetall [key]
# 删除
hdel [key] [field]
# 长度
hlen [key]
# 是否存在
hexists [key] [field]
# 获取所有field
hkeys [key]
# 获取所有的value
hvalues [key]
```



## Zset

```bash
# 添加
zadd [key] [score] [member]
```

