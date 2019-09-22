## 你必须知道的Rabbit

RabbitMq 是一个开源的 基于AMQP协议实现的一个完整的企业级消息中间件，服务端语言由Erlang（面向并发编程）语言编写 对于高并发的处理有着天然的优势，客户端支持非常多的语言：

想要真正的了解Rabbit有些名词是你必须知道的。

包括：ConnectionFactory（连接管理器）、Channel（信道）、Exchange（交换器）、Queue（队列）、RoutingKey（路由键）、BindingKey（绑定键）。

![1569152083942](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1569152083942.png)

![1569152170596](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1569152170596.png)

**Broker**: 接收和分发消息的应用，消息中间件的服务节点，可以看成是一个MQ服务器。我们在介绍消息中间件的时候所说的消息系统就是Message       Broker。

**ConnectionFactory（连接管理器）：**应用程序与Rabbit之间建立连接的管理器，程序代码中使用。

**Channel（信道）：**消息推送使用的通道；Channel是在connection内部建立的逻辑连接，如果应用程序支持多线程，通常每个thread创建单独的channel进行通讯。

**Exchange（交换器）：**用于接受、分配消息；message到达broker的第一站，根据分发规则，匹配查询表中的routing key，分发消息到queue中去。常用的类型有：direct (point-to-point), topic (publish-subscribe) and fanout (multicast)。

**direct直接连接交换机**: 	这种类型的交换机的路由规则是根据一个routingKey的标识，交换机通过一个routingKey与队列绑定 ，在生产者生产消息的时候 指定一个routingKey 当绑定的队列的routingKey 与生产者发送的一样 那么交换机会吧这个消息发送给对应的队列。

**topic主题路由匹配交换机**:	那么我生产一条消息routingKey 为： error.user 那么此时 2个队列都能接受到， 如果改为 topic.error.user 那么这时候 只有B能接受到了。

**fanout无路由交换机**:	路由广播的形式,将会把消息发给绑定它的全部队列,即便设置了key,也会被忽略.

**headers 比对headers属性**
不依赖路由键的匹配来路由消息，而是根据发送的消息内容中的headers属性(键值对)进行匹配，在绑定队列和交换器时也指定一组键值对，两者匹配时，即路由
headers类型性能会很差，也不实用，很少用到。

**Queue（队列）**：用于存储生产者的消息；消息最终被送到这里等待consumer取走。一个message可以被同时拷贝到多个queue中。

**RoutingKey（路由键）**：生产者将消息发给交换器时，会指定RoutingKey，用来指定该消息的路由规则，这个RoutingKeyf需要与交换器类型和绑定键BindingKey联合使用才能最终生效。

**BindingKey（绑定键）**：通过绑定将交换器与队列关联起来，在绑定时一般会指定绑定键BindingKey，这样，MQ就可以正确将消息路由到队列。





### 消息发布接收流程：

-----发送消息-----
1、生产者和Broker建立TCP连接。
2、生产者和Broker建立通道。
3、生产者通过通道消息发送给Broker，由Exchange将消息进行转发。
4、Exchange将消息转发到指定的Queue（队列）

----接收消息-----
1、消费者和Broker建立TCP连接
2、消费者和Broker建立通道
3、消费者监听指定的Queue（队列）
4、当有消息到达Queue时Broker默认将消息推送给消费者。
5、消费者接收到消息。



## 消息，交换机，队列的持久化

- 消息持久化是在消息投递前定义的，设置了持久化后，消息会被保存在磁盘，当被消费后会从磁盘中删除，但是也不能保证100%持久化成功，因为消息是先放到内存中的，如果此时主机崩溃，还是会有一部分来不及写入磁盘；
- 交换机持久化是在声明该交换机的时候设置的，当主机崩溃或重启后，mq一重新上线会自动重新声明一个和原来完全一样的交换机；
- 队列持久化是在队列被声明的时候设置的,mq重新上线会自动重新声明一个和原来完全一样的queue，但是队列里的消息会全部丢失；



### **总结:解耦，异步，削峰。**

### **RabbitMQ的具体特点：**

### **可靠性，持久化、传输确认、发布确认。**

