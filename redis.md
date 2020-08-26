## 简介
1. redis是一个数据结构服务器，支持存储不同类型的值
2. 支持的数据结构
	1. String：二进制安全字符串
	2. List：有序集合（按插入的顺序），基于链表
	3. Set：唯一集合，无序
	4. zset：有序唯一集合，排序按元素的分数，与set不同，还能范围检索
	5. Hash：哈希，由键值（都是字符串）组合映射，可理解为Map
	6. Bit arrays（bitmaps）：位数组（或简称为位图）
	7. HyperLogLog：概率数据结构，用于估计集合的基数	
	8. Stream：流，提供抽象日志数据类型的类地图项的仅追加集合

## 命令
* SET
	* SET key value EX seconds
* GET
* INCR
* INCRBY
* DECR
* DECRBY
* DEL
* EXPIRE
* TTL
* PERSIST

## list 有序
* RPUSH : 右加
* LPUSH ： 左加
* LRANGE : 范围读
* LPOP : 左抛出
* RPOP : 右抛出
* LLEN : 长度

## set 无序、单一
* SADD : 加
* SREM : 移除
* SISMEMBER : 元素是否存在
* SMEMBERS : 返回集合的元素列表
* SUNION : 返回并集的元素列表
* SRANDMEMBER : 随机返回
* SPOP : 随机删除并返回

## zset （用元素的分数排序）有序、单一
* ZADD
* ZRANGE

## hashes 哈希
* HSET
* HGETALL
* HMSET
* HGET
* HINCRBY
* HDEL