1.连接不上，报错

1.将防火墙关了，或者只打开需要的端口。命令查看centos7.

2.查看阿里云安全规则，是否配置了端口

3.在本机上用cmd ：telnet ip地址 6379  （注意ip地址和端口之间是端口）

如果第三步成功还是有问题，重启服务器.

## 启动

1.在安装目录下./redis-server 。

2.把/root/redis-3.0.0/redis.conf复制到/usr/local/redis/bin目录下

![img](file:///C:\Users\12714\AppData\Local\Temp\ksohtml20132\wps1.jpg)

./redis-server redis.conf

## 关闭

./redis-cli shutdown

# 配置文件中

```
<!-- 连接redis单机版，JedisClientPool是个工具类，封装了常用的方法，方便 -->
	<bean id="jedisClientPool" class="cn.e3mall.common.jedis.JedisClientPool">
		<property name="jedisPool" ref="jedisPool"></property>
	</bean>
	<!--配置jedisPool，然后注入到JedisClientPool中-->
	<bean id="jedisPool" class="redis.clients.jedis.JedisPool">
		<constructor-arg name="host" value="120.79.189.242"/>
		<constructor-arg name="port" value="6379"/>
	</bean>
```

# 使用

```
@Override
	public List<TbContent> getContentListByCid(long cid) {
		//先从redis中查缓存
		try {
			String json=jedisClient.hget(CONTENT_LIST,cid+"");
			/**
			 * StringUtils.isNotBlank()方法，判断json字符串是否为空，比普通判断要好点
			 * 详细查看API
			 */
			if (StringUtils.isNotBlank(json)) {
				List<TbContent> list = JsonUtils.jsonToList(json, TbContent.class);
				System.out.println("从redis取的list");
				return  list;
			}
		}
		catch (Exception e){
			e.printStackTrace();
		}

		//若redis没有，从数据库查找
		TbContentExample example = new TbContentExample();
		Criteria criteria = example.createCriteria();
		//设置查询条件
		criteria.andCategoryIdEqualTo(cid);
		//执行查询
		List<TbContent> list = contentMapper.selectByExampleWithBLOBs(example);
		//将结果list放到redis中，注意redis中只有String，所以对象要装json字符串
		try {
			jedisClient.hset(CONTENT_LIST,cid+"", JsonUtils.objectToJson(list));
		}
		catch (Exception e){
			e.printStackTrace();
		}
		System.out.println("从数据库取的list");
		return list;
	}
```

使用注意：

对于查询数据：使用前，先去redis查询，查到了就返回。没查到就从数据库查询，然后加入redis.

对于删除和修改，增加等请求，需要把redis的数据删除。