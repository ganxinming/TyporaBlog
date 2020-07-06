docker pull elasticsearch:6.4.0 拉取镜像

docker images 查看镜像版本

```
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d --name elasticsearch  -p 9200:9200 -p 9300:9300  *****[注：这是要运行的镜像id]
```

浏览器输入：http://127.0.0.1:9200/ 你的服务器ip 端口号

或者 ：curl -v 120.79.189.242:9200 测试



浏览器返回信息，证明安装成功

### 错误：假如删除容器后，重新安装报错记得重启docker，重新开始。





https://www.cnblogs.com/afeige/p/10771140.html