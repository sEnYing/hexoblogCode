---
title: redis基础命令
date: 2023-09-26 16:47:23
description: redis使用的基础命令
tags:
- redis
categories:
- redis
---

<h1>
基础命令
</h1>

```text
● flushdb：清空当前库
● flushall：清空所有16个库
● get key名：查询键
● set 键名 值：创建键值对
● select 1：选择1号库 共16个库
● 模糊查询keys命令，三个通配符：
  ○ *：通配任意多个字符
    ■ 查询所有的键： keys *
    ■ 模糊查询k开头，后面随意：keys k*
    ■ 模糊查询e结尾，前边随意：keys *e
    ■ 任意多个字符，包含k的键：keys *k*
  ○ ?：通配单个字符
    ■ 模糊查询K开头，并匹配一个字符：keys k?
    ■ 模糊查询K开头，长度3位：keys k??
  ○ []：通配括号内的某一个字符
    ■ 记得其他字母，但第二个可能是a或e：r[ae]dis
● 键（key）:
  ○ exists key名：判断某个Key是否存在
  ○ move key名 db名：移动（剪切、粘贴）键到几号库
  ○ ttl key名：查看键还有多久过期（-1永不过期，-2已过期）
    ■ time to live：还能活多久
  ○ expire key名 秒：为键设置过期时间
  ○ type key名：查看键的数据类型
```

<h1>
数据类型
</h1>
<h4>
字符串String
</h4>

```text
  ○ set/get/del/append/strlen
    ■ append：向键的值中追加值  append 键名 追加值
    ■ strlen：获取键的值的长度  strlen 键名
  ○ incr/decr/incrby/decrby：加减操作，操作的必须是数字类型
    ■ incr 键名  效果：键值自增1
    ■ decr键名  效果：键值自减1
    ■ incrby 键名 数字  效果：键值增加数字的值
    ■ decrby键名 数字  效果：键值减少数字的值
  ○ getrange/setrange：类似between....and.....
    ■ getrange 键名 0 -1   效果：获取全字段
    ■ getrange 键名 0 数字   效果：获取0到数字代表索引位置字段
    ■ setrange 键名 数字 替换值   效果：从数字代表索引位置开始内容替换为替换值
  ○ setex/setnx
    ■ set with expir：添加数据的同时设置生命周期
      ● setex 键名 存活时间 键值
    ■ set if not exist：添加数据的同时判断是否已经存在，防止已存在的数据被覆盖掉
      ● setnx 键名 键值
  ○ mset/mget/msetnx
    ■ mset 键1 值1 键2 值2 键3 值3 。。。。。
    ■ mget 键1 键2 键3 键4 。。。。。
    ■ msetnx 键1 值1 键2 值2 键3 值3.。。。。。：添加键值对的同时判断是否存在重复
  ○ getset：先get后set
    ■ getset 键名 键值：先获取原值没有就是nil，在覆盖掉原值
```

<h4>
列表List
</h4>

```text
  ○ lpush/rpush/lrange
    ■ l：自左向右添加
    ■ r：自右向左添加
      ● lrange list名 0 -1：从左往右读所有
  ○ lpop/rpop：移除第一个元素
  ○ lindex：根据下标查询元素
  ○ llen：返回集合长度
  ○ lrem：删除n个value    lrem list名  删除个数 删除的值
  ○ ltrim：截取指定范围的值，别的都扔掉
    ■ ltrim list名 开始索引 结束索引
  ○ rpoplpush：从一个集合搞一个元素到另一个集合中（右出一个，左进一个）
    ■ rpoplpush  list1  list2
  ○ lset：改变某个下标的值
    ■ lset list名 index value
  ○ linsert：插入元素
    ■ linsert list名 before/after index  value
```

<h4>
集合set
</h4>

```text
  ○ sadd/smembers/sismember：添加/查看/判断是否存在
    ■ sadd set名 值 值 值 值
    ■ smembers set名
    ■ sismember set名  要查的值 ：  返回1存在 0不存在
  ○ scard：获得集合中的元素个数
    ■ scard set名
  ○ srem：删除集合中的元素
    ■ srem set名  value
  ○ srandmember：从集合中随机获取几个元素
    ■ srandmember set名  整数
  ○ spop：随机出栈（移除）
    ■ spop set名
  ○ smove：移动元素，将key1某个值赋值给key2
    ■ smove set名1 set名2 值
  ○ 数学集合类：
    ■ 交集：sinter
      ● sinter set名1 set名2
    ■ 并集：sunion
      ● sunion set名1 set名2
    ■ 差集：sdiff
      ● sdiff set名1 set名2
```

<h4>
哈希Hash
</h4>

```text
● hset/hget/hmset/hmget/hgetall/hdel：添加/得到/多添加/多得到/得到全部/删除属性
  ○ 举例：
    ■ hset user id 1001
    ■ hget user id   =》1001 
    ■ hmset student id 101 name tom age 22
    ■ hmget student id name age =》101   tom  22
    ■ hgetall student =》 id 101  name tom age 22
    ■ hdel student name
● hlen：返回元素的属性个数
  ○ hlen student
● hexists：判断元素是否存在某个属性
  ○ hexists student name =》1 有 0 没有
● hkeys/hvals：获得属性的所有key/获得属性的所有value
  ○ hkeys student  =》 id  name age
  ○ hvals student =》101 tom 22
● hincrby/hincrbyfloat：自增（整数）/自增（小数）
  ○ hincrby student age 2 =》 24
  ○ hincrbyfloat student  age  2,2 =》26.2
● hsetnx：添加时判断是否存在
  ○ hsetnx student sex 男  =》1 成功 0 已存在
```

<h4>
有序集合Zset
</h4>

```text
● zadd/zrange：添加/查询
  ○ zadd zset名 10 vip1 20 vip2 30 vip3
  ○ zrange zset01 0 -1 =》vip1 vip2 vip3
  ○ zrange zset01 0 -1 withscores =》vip1 10 vip2 20 vip3 30
● zrangebyscore：模糊查询
  ○ （：不包含
    ■ zrangebyscore zset01 20 （30 =》vip2
    ■ zrangebyscore zset01 （10 （30 =》vip2
  ○ limit：跳过几个截取几个
    ■ zrangebyscore zset01 10 30  limit 2 1  =》vip3
● zrem：删除元素
  ○ zrem zset01 vip1
● zcard/zcount/zrank/zscore：集合长度/范围内元素个数/得元素下标/通过值得分数
  ○ zcard zset01
  ○ zcount zset01 20 30 =》2
  ○ zrank zset01 vip3 =》2
  ○ zscore zset01  vip1 =》10
● zrevrank：逆序找下标
  ○ zrevrank zset01 vip3 =》0
● zrevrange：逆序查询
  ○ zrevrange zset01 0 -1 =》vip3 vip2 vip1
```
