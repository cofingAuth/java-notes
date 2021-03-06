### 简介
1. redis是一个数据结构服务器，支持存储不同类型的值
2. 支持的数据结构
	1. String：二进制安全字符串
	2. List：有序集合（按插入的顺序），基于链表
	3. Set：唯一集合，无序
	4. zset：有序唯一集合，排序按元素的分数，与set不同，还能范围检索
	5. Hash：哈希，由键值（都是字符串）组合映射，可理解为Map
	6. bitmaps：位图，本身不是数据库结构，而是在String类型上定义的一组面向位的操作，可以想象为以位为单位的数组
	7. HyperLogLog：概率数据结构，用于估计集合的基数，根据输入的元素进行计算，而不会存储实际数据	
	8. geospatial

### 部分命令，官网查看更多

* SET key value
	* SET key value EX seconds
* GET key
* INCR
* INCRBY
* DECR
* DECRBY
* DEL key [key...] 删除键，已删除1，没有可删除0
  * 通配符删除式：redis-cli DEL \`redis-cli KEYS "user.*"\` 或者 redis cli KEYS "user.\*" | xargs redis-cli DEL
* EXPIRE
* TTL
* PERSIST
* TYPE key 键类型
* EXISTS key 键是否存在
* MGET / MSET 同时设置/获取多个键值
* GETBIT / SETBIT / BITCOUNT / BITOP 位操作

### list 有序性
* RPUSH : 右加
* LPUSH ： 左加
* LRANGE key start stop : 范围读，0...-1
* LPOP : 左抛出
* RPOP : 右抛出
* LLEN : 长度
* LREM key count value：删除列表中前count个值为value的元素
* LINDEX key index : 通过索引获取值
* LSET key index value：通过索引设置值
* LTRIM key start end：获取范围值，并删除其他范围值
* LINSERT key BEFORE|AFTER pivot value ：在pivot插入 前/后 插入值
* RPOPLPUSH source destination：在source右抛出，向destination左插入

### set 无序性、唯一性
* SADD key member [member...]: 增加元素
* SREM key member [member...] : 移除
* SISMEMBER key member : 元素是否存在
* SMEMBERS key : 返回集合的元素列表
* SUNION : 返回并集的元素列表
* SRANDMEMBER key [count] : 随机返回
* SPOP key : 随机删除并返回
* SMOVE 指定元素移动到另外一个集合
* 集合间运算并返回结果
  * SDIFF key [key...]  差集，即存在集合A，但不存在集合B的元素
  * SINTER key [key...] 交集，即存在集合A，也存在集合B的元素
  * SUNION key [key...] 并集，即存在集合A，或者存在B的元素
* SCARD key 获取集合中元素个数
* 集合运算并将结果存储
  * SDIFFSTORE destination key [key...]
  * SINTERSTORE destination key [key...]
  * SUNIONSTORE destination key [key...]

### zset （用元素的分数排序）有序、单一

> 有序集合中每个元素都关联了一个分数，-inf 负无穷、+inf正无穷

* ZADD key score member [score member ...] 添加元素
* ZRANGE key start stop [WITHSCORES] 获取排名在某个范围的元素列表
* ZREVRANGE 类上并从大到小
* ZSCORE key member 获取元素的分数
* ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count] 从小到大返回分数在min和max之间（包含边界）的元素
* ZINCRBY key increment member 给元素增加/减少分数 
* ZCARD key 获取集合元素的数量
* ZCOUNT key min max 获取指定分数范围内的元素个数
* ZREM key member [member...] 删除一个或多个元素

### hash 哈希
> 散列类型，字典结构，存储字段和字段值映射，字段值只能是字符串，不能嵌套其他数据类型。常用于保存对象

* HSET key field value
* HMSET key field value [field value...]
* HGET key field
* HMGET key field [field...]
* HGETALL
* HINCRBY
* HDEL 
* HEXISTS key field
* HSETNX 不存在则插入
* HKEYS 获取所有键
* HVALS 获取所有值

### geospatial 地理空间索引半径查询

**相关命令**

- [GEOADD](http://www.redis.cn/commands/geoadd.html)
- [GEODIST](http://www.redis.cn/commands/geodist.html)
- [GEOHASH](http://www.redis.cn/commands/geohash.html)
- [GEOPOS](http://www.redis.cn/commands/geopos.html)
- [GEORADIUS](http://www.redis.cn/commands/georadius.html)
- [GEORADIUSBYMEMBER](http://www.redis.cn/commands/georadiusbymember.html)

![redis](D:\mygit\java-notes\image\redis\redis.jpg)

### HyperLogLog

![HyperLogLog](D:\mygit\java-notes\image\redis\HyperLogLog.jpg)

![HyperLogLog2](D:\mygit\java-notes\image\redis\HyperLogLog2.jpg)

### Bitmaps

![Bitmap1](D:\mygit\java-notes\image\redis\Bitmap1.jpg)

![Bitmap2](D:\mygit\java-notes\image\redis\Bitmap2.jpg)

### 1. 进阶

#### 事务

> 开启事务，将命令存在等待执行的事务队列中，最后按顺序执行：MULTI 命令... EXEC
>
> 监控键 WATCH，取消监控 UNWATCH ; 监控键后一旦键被修改，之后的一个事务就不会执行

####  过期时间

> EXPIRE key seconds 单位秒（整数，即最少1s） ；设置过期时间
>
> PEXPIRE 类上功能，不过单位为毫秒
>
> TTL 返回键的剩余时间（单位秒），不存在时返回-2，永久存在返回-1
>
> PERSIST 清除过期时间变永久，或者用set命令重新赋值

#### 排序

> 有序集合、SORT命令，时间复杂度是O(n+mlog(m)) ，n指要排序的列表元素个数，m指要返回的元素个数

#### 消息通知

> 任务队列：BRPOP key1 ... TIMEOUT  假如在指定时间内没有任何元素被弹出，则返回一个 nil 和等待时长。超时时间单位秒，超过则不阻塞了，0表示永久阻塞；多个键按照从左到右顺序取，即所谓的优先级
>
> ”发布/订阅“模式，包含两个角色，分别是发布者和订阅者。订阅者可以订阅一个或若干个频道，而发布者可以指定频道发送消息，所有订阅者则收到消息。
> 发布命令：PUBLISH channel message
> 订阅频道：SUBSCRIBE channel [channel...]   或者支持通配符的 PSUBSCRIBE
> 取消订阅：UNSUBSCRIBE [channel...]
> 如果进入订阅状态的客户端，只能使用 发布/订阅 模式的命令，并且可能收到三种类型消息（subscribe订阅成功的反馈信息、message接收到的消息、unsubscribe成功取消订阅频道）

#### 管道

> 通过管道可以一次性发送多条命令并在执行完后一次性将结果返回。管道通过减少客户端与redis的通信次数来实现降低往返时延累计值的目的

#### 节省空间

> redis是一个基于内存的数据库，所有的数据都存储在内存中。
>
> - 精简键名和键值
> - 内部编码优化，redis内部编码对外是透明的，查看OBJECT ENCODING key

### 2. 脚本

> Lua脚本，以脚本在redis作为原子运行

### 3. 持久化

> 两种方式的持久化
>
> - RDB，指定的规则“定时”将内存中的数据存储到磁盘上；通过快照完成的，即内存中的数据生成副本并存储在磁盘上；
>   - 触发快照情况如下
>     - 根据配置规则进行自动快照（配置文件，配合快照条件）
>     - 用户执行SAVE（阻塞）或BGSAVE（异步快照，推荐使用）命令
>     - 执行FLUSHALL命令，只要有配置自动快照，则会触发
>     - 执行复制时，当设置了主从模式时，redis会在复制初始化时进行自动快照。
> - AOF，防止重要数据丢失，AOF将redis执行的每一条写命令追加到磁盘文件中
>   - 开启AOF参数：appendonly year
>   - AOF文件保存位置和RDB文件位置相同，目录dir参数设置，默认文件名appendonly.aof，可以设置参数appendfilename
>   - AOF的实现，以纯文本形式，每当达到一定条件时redis会自动重写AOF文件，这个条件可以配置：auto-aof-rewrite-percentage 100、auto-aof-rewrite-min-size 64mb；可以主动执行AOF重写，使用BGREWRITEAOF
>   - redis启动时会逐个执行AOF文件的命令来将数据载入内存，速度相较RDB会慢一些
>
> ------
>
> 快照原理：
>
> - redis默认会将快照文件存储在redis当前进程的工作目录中的dump.rdb文件中，可以通过配置dir和dbfilename两个参数分别指定存储路径和文件名。
> - 快照过程：
>   1. redis使用fork函数复制一份当前进程（父进程）的副本（子进程）
>   2. 父进程继续接收并处理客户端发来的命令，而子进程开始将内存中的数据写入硬盘中的临时文件
>   3. 当子进程写入完所有数据后会用该临时文件替换旧的RDB文件，至此一次快照操作完成
> - 注意：
>   - 在执行fork的时候操作系统（如unix）会使用写时复制策略，即fork函数发生的一刻父子进程共享同一内存数据，当父进程要更改某片数据时，操作系统会将该片数据复制一份以保证子进程的数据不受影响，所以新的RDB文件存储的是执行fork一刻的内存数据。
>   -  reids启动后会读取RBD快照文件，将数据从磁盘载入内存。 通过RDB方式持久化，一旦redis异常退出，就会丢失最后一次快照以后更改的所有数据。可以通过结合AOF设置，或根据具体应用场景，看数据重要性。
>   - AOF，由于系统的缓存机制，数据并没有真正地马上写入磁盘，而是进入了系统的磁盘缓存，默认系统每30S自动执行一次同步，所以这样还是会丢失数据，所以可以主动设置系统将缓存内容同步到磁盘，设置参数：appendfsync always | everysec | no ; 默认是everysec每秒同步一次；always每次写入都同步，这最安全最慢；no由操作系统默认自动同步
> - redis允许同时开启AOF和RDB，这保证了数据安全又使得进行备份等操作十分容易

### 4. 集群

> 1. 从结构上，单个redis服务器会发生单点故障，同时一台服务器需要承受所有的请求负载。这就需要为数据生成多个副本并分配在不同机器上；
>
> 2. 从容量上，单个redis服务器的内存非常容易成为存储瓶颈，所以需要进行数据分片
>
> 同时拥有多个redis服务器会面临如何管理集群问题，包含如何增加节点、故障恢复等操作

#### 复制	

>主从复制，主数据库可以进行读写操作，当写操作导致数据变化时会自动将数据同步给从数据库，而从数据库一般是只读的，并接受主数据库同步过来的数据。一主可以有多个从，而一个从数据库只能拥有一个主数据库。
>
>通过复制可以实现读写分离，以提高服务器的负载能力
>
>------
>
>实现：
>
>1. 配置文件加入 slaveof 主数据库地址与端口
>2. 命令行参数设置slaveof参数
>3. 运行时使用SLAVEOF命令、
>
>从数据库如需要停止同步，使用命令SLAVEOF NO ONE
>
>------
>
>原理：由于redis服务器使用TCP协议通信，所以当从数据库启动后，向主数据库发送SYNC命令。同时主数据库接收到SYNC命令后开始在后台保存快照（即RDB持久化的过程），并将保存快照期间接收到的命令缓存起来。当快照完成后，redis会将快照文件和所有缓存的命令发送到从数据库。从数据库收到后，会载入快照文件并执行收到的缓存的命令。以上过程称为复制初始化。复制初始化结束后，主数据库每当收到写命令时就会将命令同步给从数据库，从而保证主从数据库数据一致性。
>从服务器复制初始化阶段：从数据库收到内容后，写入硬盘的临时文件，写完后把临时文件替换RDB快照文件，之后的操作和RDB持久化时启动恢复的过程一样
>
>当主从数据库之间的连接断开重连后，主数据库只需要将断开期间执行的命令传送给从服务器。这就是增量数据传输。
>
>------
>
>持久化问题：为提高性能，主数据库禁用持久化，从数据库启用持久化；
>
>- 当从数据库崩溃重启后主数据库会自动将数据库同步过来
>- 当主数据库崩溃时，手工恢复步骤
>  1. 在从数据库使用SLAVEOF NO ONE命令将从数据库提升成主数据库继续服务
>  2. 启动之前崩溃的主数据库，然后使用SLAVEOF命令成为新的主数据库的从数据库，即可将数据同步回来
>- 注意：当开启复制且主数据库关闭持久化时，一定不能使用任何方式令主数据库崩溃后自动重启。同样当主数据库所在的服务器因故障关闭时，也要避免直接重新启动。所以redis提供一种自动化方案哨兵来解决手工维护主从复制的麻烦问题
>
>------
>
>无硬盘复制：默认关闭，可以修改配置进行开启
>
>- 意思是redis在与从数据库进行复制初次化时将不会将快照内容存储都磁盘上，而是直接通过网络发送给从数据库
>- 起因：
>  - 当主数据库禁止用RDB快照时，如果执行了复制初始化草走，redis依然会生成RDB文件，所以下次启动后主数据库会以该快照恢复数据。因为复制发生的事件不确定，这使得恢复的数据可能是任何事件点的
>  - 由于复制初始化会在磁盘创建RDB文件，如果磁盘性能很慢时这一个过程会对性能产生影响。举个例子，当使用redis做缓存系统时，因为不需要持久化，所以服务器的磁盘读写速度可能较差。但当使用在一主多从的集群架构时，每次和从数据库同步，redis都会执行一次快照，同时对磁盘进行读写，导致性能降低
>
>------
>
>增量复制：对于开发员可以说是完成透明的，能修改是积压队列大小，其越大，允许主从数据库断线时间越长。其基于以下3点实现
>
>- 从数据库会存储主数据库的运行ID
>- 在复制同步阶段，主数据库每将一个命令传送给从数据库时，都会同时把该命令存放到一个积压队列（backlog）中，并记录下当前积压队列中存放的命令的偏移量范围
>- 同时，从数据库接收到主数据库传来的命令时，会记录下该命令的偏移量

#### 哨兵

> 哨兵是一个独立的进程，其作用是监控redis系统的允许状况，实现自动化系统监控和故障恢复功能。功能如下
>
> - 监控主数据库和从数据库是否正常运行
> - 主数据库出现故障时自动将从数据库转换为主数据库。
>
> ------
>
> 哨兵进程启动时会读取配置文件内容
>
> 使用命令redis-sentinel /path/to/sentinel.cof；配置文件内容：sentinel monitor master-name ip port quorum 这样找到需要监控的主数据库，参数quorum表示执行故障恢复操作前至少需要几个哨兵节点同意；一个哨兵节点可以监控多个redis主从系统，在内容多配置多个即可，同时多个哨兵节点可以同时监控同一个redis主从系统。
>
> ------
>
> 原理：哨兵启动后，会与监控的主数据库建立两条连接，一条连接用来订阅主数据库的\__sentienl__:hello频道以获取其他同样监控该数据库的哨兵的节点信息，另外哨兵也需要定期向主数据库发送INFO等命令来获取主数据库本身的信息。
>
> 哨兵与主数据库连接后，定时执行以下操作
>
> 1. 每10秒哨兵会向主数据库和从数据库发送INFO命令
> 2. 每2秒哨兵会向主数据库和从数据库的\__sentienl__:hello频道发送自己的信息
> 3. 每1秒哨兵会向主数据库、从数据库和其他哨兵节点发送PING命令
>
> 哨兵与哨兵的连接只会建立一条连接用来发送PING命令。实现自动发现从数据库和其他哨兵节点有运行状况，配置参数sentienl down-after-milliseconds master-name ms表示示当ms>1s时，则哨兵每隔1S发送一次PING命令，当ms<1s时，则每隔ms时间发送一次PING命令。
>
> ------
>
> 哨兵的部署：
>
> - 为每一个节点部署一个哨兵
> - 使每一个哨兵与其对应的节点的网络环境相同或相近
>
> 这个的部署方案可以保证哨兵的视角拥有较高的代表性和可靠性。同时设置quorum的值为N/2+1（其中N为哨兵节点数量），这样使得只有当大部分哨兵节点同意后才进行故障恢复；
>
> 主观下线：当前哨兵进程看来，该节点已经下线
>
> 客观下线，当哨兵达到指定数量都认为该节点已经下线
>
> 哨兵故障恢复：首先需要选举领头哨兵，因为这样保证只有同一时间只有一个哨兵节点来执行故障恢复，选举领头哨兵使用Raft算法（当前发现的哨兵，要求其他节点选自己称为领头哨兵，如果其他节点没有选过其他人，则成功成为；如果同意数超过quorum，也时成功成为；如果有多个节点也参选，则需要重新选举下一论）；领头哨兵，选故障的一个从节点来充当主数据库，通过一系列命令，最后故障节点更新为从节点。

#### 集群

> 配置集群，在每个节点配置开启 cluster-enabled，每个集群中至少需要3个主数据库才能正常运行。
>
> 集群会将当前节点记录的集群状态持久化地存储在指定文件中，默认名称nodes.conf，可以通过参数修改：cluster-config-file 
>
> redis提供一个辅助工具redis.trib.rb来进行集群管理，其使用Ruby语言；
>
> redis.trib.rb以客户端的形式尝试连接所有节点，并发送PING命令确认节点能够正常服务，同时发送INFO获取节点的运行ID和是否开启了集群功能（cluster-enabled为1），准备就绪后集群向每个节点发送CLUSTER MEET命令说明当前节点是集群的一部分。分配完主从节点后，会为每个主数据库分配插槽，之后对每个子数据库节点发送CLUSTER REPLICATE主数据库的运行ID来将当前节点转换成从数据库并复制指定运行ID的节点。
>
> ------
>
> 插槽：一个集群中，所有的键会被分配给16384个插槽；
>
> 键与插槽的关系，redis将每个键的键名的有效部分使用CRC16算法计算出散列值，然后取16384的余数。这样使得每个键都可以分配到16384个插槽中，进而分配的指定的一个节点中处理。
>
> 插槽分配给指定节点：情况分为两种：1.插槽没有被分配过 2.插槽分配过，现需要移动；可以继续用redis.trib.rb的命令进行操作

### 5. 管理

> 安全
>
> 1. 可信环境：bind 地址
> 2. 数据库密码：requirepass 密码
> 3. 重命名命令：rename 
>
> 通信协议
>
> 1. 简单协议：telnet程序输入
> 2. 统一请求协议：二进制安全，用于主从复制传输内容、AOF文件