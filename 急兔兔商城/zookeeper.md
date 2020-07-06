### 1.启动zookeeper（开启zookeeper必须要）

#### /zookeeper-3.4.6/bin/zkServer.sh

启动:./zkServer.sh start

关闭:./zkServer.sh stop

### 2.启动fastdfs

##### 1.启动nginx(cd /usr/local/nginx/sbin       ./nginx，配置路径group1/M00已经配置好了)

2.查看是否开启过fds,如果开启了，就杀死所有该相关进程(kill -9)

netstat -aux|grep fdfs

3.启动traker(/usr/bin/fdfs_trackerd :为脚本程序. /etc/fdfs/tracker.conf:为我的配置文件的位置)

```
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf start
```

4.启动storage（支持start|stop|restart）

```
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf start
```

5.查看是否连接成功traker和storage

netstat -apn|grep fdfs

### 3.启动redis

**cd /usr/local/redis/src**  

**./redis-server ../redis.conf**

查看是否开启:

ps aux | grep redis

杀死进程就是关闭