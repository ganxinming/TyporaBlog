# 使用：

Linux开启：

1.进入activemq 的bin目录下的activemq命令启动：

[root@localhost bin]# ./activemq start

关闭：

[root@localhost bin]# ./activemq stop

查看状态：

[root@localhost bin]# ./activemq status

进入管理后台：

<<http://120.79.189.242:8161/admin/>>

用户名：admin

密码：admin

可能出现

2.

```
* ActiveMq基本用法：1.publish/subscribe ：发布/订阅
*              2.point-to-point :点到点
* 两者区别:
* 点到点：发布者发送10条消息，假如有三个接受者，他们只能抢着去接消息，
* 并不能得到全部消息，消息分配在各个接受者上。(一个消息只能被一个消费者接受)
* 发布/订阅：每个接受者可以接受发布者的所有消息，而且可以订阅多个发布者。(一个消息可以被多个消费者接受)
* (需要先订阅，然后发布才有效！)
```

代码如下：

```
public class ActiveMqTest {
	private  static String URL = "tcp://120.79.189.242:61616";
	/**"
	 * 点到点形式发送消息
	 * <p>Title: testQueueProducer</p>
	 * <p>Description: </p>
	 * @throws Exception
	 */
	//Producer发送方发送消息
	@Test
	public void testQueueProducer() throws Exception {
		//1、创建一个连接工厂对象，需要指定服务的ip及端口。
		ConnectionFactory connectionFactory = new ActiveMQConnectionFactory(URL);
		//2、使用工厂对象创建一个Connection对象。
		Connection connection = connectionFactory.createConnection();
		//3、开启连接，调用Connection对象的start方法。
		connection.start();
		//4、创建一个Session对象。
		//第一个参数：是否开启事务。如果true开启事务，第二个参数无意义。一般不开启事务false。
		//第二个参数：应答模式。自动应答或者手动应答。一般自动应答。
		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		//5、使用Session对象创建一个Destination对象。两种形式queue、topic，现在应该使用queue
		Queue queue = session.createQueue("test-queue");
		//6、使用Session对象创建一个Producer对象。
		MessageProducer producer = session.createProducer(queue);
		//7、创建一个Message对象，可以使用TextMessage。
		/*TextMessage textMessage = new ActiveMQTextMessage();
		textMessage.setText("hello Activemq");*/
		TextMessage textMessage = session.createTextMessage("hello activemq");
		//8、发送消息
		producer.send(textMessage);
		//9、关闭资源
		producer.close();
		session.close();
		connection.close();
	}
	//Consumer消费方接受消息
	@Test
	public void testQueueConsumer() throws Exception {
		//创建一个ConnectionFactory对象连接MQ服务器
		ConnectionFactory connectionFactory = new ActiveMQConnectionFactory(URL);
		//创建一个连接对象
		Connection connection = connectionFactory.createConnection();
		//开启连接
		connection.start();
		//使用Connection对象创建一个Session对象
		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		//创建一个Destination对象。queue对象
		Queue queue = session.createQueue("test-queue");
		//使用Session对象创建一个消费者对象。
		MessageConsumer consumer = session.createConsumer(queue);
		//接收消息，设置一个消息监听器
		consumer.setMessageListener(new MessageListener() {
			
			@Override
			public void onMessage(Message message) {
				//打印结果
				TextMessage textMessage = (TextMessage) message;
				String text;
				try {
					text = textMessage.getText();
					System.out.println(text);
				} catch (JMSException e) {
					e.printStackTrace();
				}
				
			}
		});
		//等待接收消息
		System.in.read();
		//关闭资源
		consumer.close();
		session.close();
		connection.close();
	}

	/**
	 * publish/subscribe ：发布/订阅 方式
	 * @throws Exception
	 */
	@Test
	public void testTopicProducer() throws Exception {
		//1、创建一个连接工厂对象，需要指定服务的ip及端口。
		ConnectionFactory connectionFactory = new ActiveMQConnectionFactory(URL);
		//2、使用工厂对象创建一个Connection对象。
		Connection connection = connectionFactory.createConnection();
		//3、开启连接，调用Connection对象的start方法。
		connection.start();
		//4、创建一个Session对象。
		//第一个参数：是否开启事务。如果true开启事务，第二个参数无意义。一般不开启事务false。
		//第二个参数：应答模式。自动应答或者手动应答。一般自动应答。
		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		//5、使用Session对象创建一个Destination对象。两种形式queue、topic，现在应该使用topic
		Topic topic = session.createTopic("test-topic");
		//6、使用Session对象创建一个Producer对象。
		MessageProducer producer = session.createProducer(topic);
		//7、创建一个Message对象，可以使用TextMessage。
		/*TextMessage textMessage = new ActiveMQTextMessage();
		textMessage.setText("hello Activemq");*/
		TextMessage textMessage = session.createTextMessage("topic message");
		//8、发送消息
		producer.send(textMessage);
		//9、关闭资源
		producer.close();
		session.close();
		connection.close();
	}
	
	@Test
	public void testTopicConsumer() throws Exception {
		//创建一个ConnectionFactory对象连接MQ服务器
		ConnectionFactory connectionFactory = new ActiveMQConnectionFactory(URL);
		//创建一个连接对象
		Connection connection = connectionFactory.createConnection();
		//开启连接
		connection.start();
		//使用Connection对象创建一个Session对象
		Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
		//创建一个Destination对象。topic对象
		Topic topic = session.createTopic("test-topic");
		//使用Session对象创建一个消费者对象。
		MessageConsumer consumer = session.createConsumer(topic);
		//接收消息
		consumer.setMessageListener(new MessageListener() {
			
			@Override
			public void onMessage(Message message) {
				//打印结果
				TextMessage textMessage = (TextMessage) message;
				String text;
				try {
					text = textMessage.getText();
					System.out.println(text);
				} catch (JMSException e) {
					e.printStackTrace();
				}
				
			}
		});
		System.out.println("topic消费者3启动。。。。");
		//等待接收消息
		System.in.read();
		//关闭资源
		consumer.close();
		session.close();
		connection.close();
	}
}
```

### ActiveMQ和Spring整合：

#### 发布者配置文件样例：applicationContext-activemq.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">
	
	<!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
	<bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
		<property name="brokerURL" value="tcp://120.79.189.242:61616" />
	</bean>
	<!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
	<bean id="connectionFactory"
		class="org.springframework.jms.connection.SingleConnectionFactory">
		<!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
		<property name="targetConnectionFactory" ref="targetConnectionFactory" />
	</bean>
	<!-- 配置生产者 -->
	<!-- Spring提供的JMS工具类，它可以进行消息发送、接收等 -->
	<bean id="jmsTemplate" class="org.springframework.jms.core.JmsTemplate">
		<!-- 这个connectionFactory对应的是我们定义的Spring提供的那个ConnectionFactory对象 -->
		<property name="connectionFactory" ref="connectionFactory" />
	</bean>
	<!--这个是队列目的地，点对点的 -->
	<bean id="queueDestination" class="org.apache.activemq.command.ActiveMQQueue">
		<constructor-arg>
		<!--以value中的spring-queue作为key-->
			<value>spring-queue</value>
		</constructor-arg>
	</bean>
	<!--这个是主题目的地，一对多的 -->
	<bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
		<!--以value中的itemAddTopic作为key-->
		<constructor-arg value="itemAddTopic" />
	</bean>
	
</beans>
```

#### 发送者发送消息：

```
@Override
	public E3Result addItem(TbItem item, String desc) {
		//生成随机id
		final long id= IDUtils.genItemId();
		item.setId(id);
		//商品状态，1-正常，2-下架，3-删除
		item.setStatus((byte) 1);
		Date date=new Date();
		item.setCreated(date);
		item.setUpdated(date);
		itemMapper.insert(item);
		//补充TbItemDesc属性
		TbItemDesc itemDesc=new TbItemDesc();
		itemDesc.setItemId(id);
		itemDesc.setCreated(date);
		itemDesc.setUpdated(date);
		itemDesc.setItemDesc(desc);
		itemDescMapper.insert(itemDesc);


		/**
		 * activeMQ发送消息，采用是主题目的地，一对多的
		 * 当item插入时，把他的id作为消息发送，向solr索引库添加
		 */
		jmsTemplate.send(topicDestination, new MessageCreator() {
			@Override
			public Message createMessage(Session session) throws JMSException {
				TextMessage message=session.createTextMessage(id+"");
				return message;
			}
		});

		return E3Result.ok();
	}
```



#### 接受者配置文件：使用前还需定义消息监听器，具体在下下面。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">
	
	<!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
	<bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
		<property name="brokerURL" value="tcp://120.79.189.242:61616" />
	</bean>
	<!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
	<bean id="connectionFactory"
		class="org.springframework.jms.connection.SingleConnectionFactory">
		<!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
		<property name="targetConnectionFactory" ref="targetConnectionFactory" />
	</bean>

	<!--这个是主题目的地，一对多的 -->
	<bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
		<constructor-arg value="itemAddTopic" />
	</bean>
	<!-- 监听商品添加消息，同步索引库 -->
	<bean id="htmlGenListener" class="cn.e3mall.item.message.HtmlGenListener"/>
	<bean class="org.springframework.jms.listener.DefaultMessageListenerContainer">
		<property name="connectionFactory" ref="connectionFactory" />
		<property name="destination" ref="topicDestination" />
		<property name="messageListener" ref="htmlGenListener" />
	</bean>
</beans>
```

#### 定义消息监听器(实现MessageListener监听器接口)：(这里是完成一添加商品就添加索引)

#### 具体功能可以根据自己需要来实现：

```
public class ItemAddMessageListener implements MessageListener {
	
	@Autowired
	private ItemMapper itemMapper;
	@Autowired
	private SolrServer solrServer;

	@Override
	public void onMessage(Message message) {

		try {
			TextMessage textMessage= (TextMessage) message;
			//获取消息内容
			String itemId=textMessage.getText();
			/**
			 * 重点:等待事务提交,因为对面可能还未提交数据，或数据库还没添加成功等，
			 * 这时候根据id就查不出来了。
			 * 需等待对方提交事务。（消息传递很快的）
			 */
			Thread.sleep(1000);
			//根据id查出SearchItem
			SearchItem searchItem = itemMapper.getItemById(Long.valueOf(itemId));
			//创建document对象
			SolrInputDocument document=new SolrInputDocument();
			//向文档添加域
			document.addField("id", searchItem.getId());
			document.addField("item_title", searchItem.getTitle());
			document.addField("item_sell_point", searchItem.getSell_point());
			document.addField("item_price", searchItem.getPrice());
			document.addField("item_image", searchItem.getImage());
			document.addField("item_category_name", searchItem.getCategory_name());
			//将文档写入索引库
			solrServer.add(document);
			//提交
			System.out.println("新增商品通过ActiveMQ消息，成功接收,并成功添加添加索引库");
			solrServer.commit();
		} catch (Exception e) {
			e.printStackTrace();
		}

	}

}

```

# 思考：

整合后的ActiveMQ挺简单的

发布者：无非就是发布者配置下连接消息队列，主题等，然后在需要的时候使用jmsTemplate.send()模板发送消息。

接受者：配置文件就是比发布者多配置了个消息监听器(实现MessageListener接口)，在消息监听器中完成逻辑。