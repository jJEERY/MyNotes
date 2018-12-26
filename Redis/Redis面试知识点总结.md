# Redis知识点总结

> 转载自 [Java面试宝典(数据库)](https://blog.csdn.net/Lonely_survivor/article/details/85011465)

### 使用Redis有哪些好处？
+ 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)
+ 支持丰富数据类型，支持string(字符串), list(列表), set(集合), sorted set(有序集合), hash(哈希)
+ 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行
+ 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后会自动删除

### Redis相比Memcached(简洁的key-value存储系统)有哪些优势
+ Memcached所有的值均是简单的字符串，Redis作为其替代者，支持更为丰富的数据结构
+ Redis的速度比Memcached快很多
+ Redis可以持久化其数据

### Redis常见性问题和解决方案
+ Master最好不要做任何持久化工作，如RDB内存快照和AOF日志文件
+ 如果数据比较重要，某个Slave开启AOF备份数据，策略设置为每秒同步一次
+ 为了主从复制的速度和连接的稳定性，Master和Slave最好在同一个局域网内
+ 主从复制不要用图状结构，用单向链表结构更为稳定，即Master<- Slave1 <- Slave2 <- Slave3...
这样的结构方便解决单点故障问题，实现Slave对Master的替换。如果Master挂了，可以立刻启用Slave1做Master，其他不变

### Redis的六种数据淘汰策略：
+ volatile-lru: 从已设置过期时间的数据集(server.db[i].expires)中挑选最近最少使用的数据淘汰
+ volatile-ttl：从已设置过期时间的数据集(server.db[i].expires)中挑选将要过期的数据淘汰
+ volatile-random：从已设置过期时间的数据集(server.db[i].expires)中任意选择数据淘汰
+ allkeys-lru：从数据集(server.db[i].dict)中选择最近最少使用的数据淘汰
+ allkeys-random：从数据集(server.db[i].dict)中任意选择数据淘汰
+ no-eviction(驱逐)：禁止驱逐数据
