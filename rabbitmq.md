### rabbitmq
	1. 介绍：rabbitmq是消息代理,可以接受并转发消息;
	2. rabbitmq的一些术语：
		1. producer生产者，仅负责发送消息，主要是一个发送消息的程序;
		2. queue队列，就如邮箱，只能存储;队列仅受主机内存和磁盘限制的约束，本质上是一个消息缓冲区;
		3. consumer消费者，主要是一个等待接收消息的程序;
	3. 注意：生产者、队列、代理人不必位于同一主机上，一个应用程序既可以是生产者，也可以是消费者;

### 官网一些简单的rabbitmq使用案例
	1. 发布者：与服务器建立连接，创建通道，声明队列，向队列发送消息
	2. rabbitmq服务器
	3. 消费者：与服务器建立连接，创建通道，声明监听的队列，接收服务器传递的消息

### AMQP
	1. 消息传递协议，可以使客户端应用与消息中间件代理进行通信
	2. 因为这是一个网络协议，所以发布者、消费者、代理者，可以位于不同的机器上
	3. publisher --publish> exchange --routes> queue --consumes> consumer
	4. 消息确认概念，当消费者通知代理，代理接收到该通知才会进行从队列移出消息
	5. 在某些情况下，无法路由消息时，消息会返回给发布者，被丢弃，或代理放入被称为“死信队列”中
	6. AMQP实体：队列、交换、绑定
	7. exchange types
		1. Direct exchange （default）路由键与队列名对应
		2. Fanout exchange 路由与所有绑定的队列传递消息，适合应用于广播
		3. Topic exchange 	路由键和被绑定一起的匹配队列进行传递，通常用于发布/订阅
		4. Headers exchange  路由与队列用头属性匹配

### QUEUES
	1. 特性：名称、持久、独占、自动删除、参数
	2. Queue Names：UTF-8的字符最大255bytes，应用可以自起队列名或者让代理者生成一个唯一名称并发会给客户端；队列名称不能以"amq."开头，这已被代理者使用；
	3. Queue Durability：持久性/临时性，持久性储存在磁盘，临时性储存在内存
	4. Bindings：交换路由使用的规则，其中有路由与队列的绑定
	5. Consumers：消费方式，有两种，"push API"推送如订阅，推荐使用；"pull API"轮询，效率低应避免使用
	6. Message Acknowledgements：
		1. 自动确认模型：代理人发送消息后使用 basic.deliver or basic.get-ok
		2. 显式确认模型：应用发送回确认之后使用basic.ack
	7. Rejecting Messages：拒绝消息时，消费者应用程序可以要求代理放弃或重新排队。当只有一个消费者，请确保出现反复拒绝导致无限循环
	8. Prefetching Messages：如消息倾向批量发布，可以指定每一个消费者一次发送多少消息，则可以用作简单的负载均衡和提供吞吐量
	9. Message Attributes and Payload：AMQP模型的消息具备属性
	10. Connections：通常是长连接，使用TCP可靠传递的协议，连接使用身份验证，并且可以使用TLS进行保护
	11. Channels：AMQP连接可视为"共享单个TCP连接的轻型连接"的通道复用。由于某个应用程序需要与代理进行多个连接，但不希望同时打开多个TCP连接，因为这样会消耗系统资源，并使配置防火墙更加困难。
	12. Virtual Hosts：虚拟主机；为了使单个代理可以承载多个隔离的“环境”（用户组，交换，队列等），AMQP 0-9-1包含虚拟主机（vhost）的概念。
	13. AMQP is Extensible：AMQP是可扩展的

### CLI tools、Configuration、等需要用到时候到[官网](https://www.rabbitmq.com/documentation.html)查看吧