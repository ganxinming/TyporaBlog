![1564303899174](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1564303899174.png)

# Eureka和zookeeper

RDBMS: mysql/oracle   ：ACID

Nosql: redis/mogoDB :  CAP

CAP:C(强一致性)，A（高可用性） ，P (分区容错性)

CAP定理:三者只能取其中两个，CP,AP,CA。

##### 但是因为我们是分布式，所以P一定满足。因此只能从CP和AP中选。（）

zookeeper：CP

![1564321830983](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1564321830983.png)

他只能满足一致性（用户拿到的永远是最新的），不能满足高可用性。当他master宕机时，子节点全部失效。

AP：

![1564321940178](C:\Users\12714\AppData\Roaming\Typora\typora-user-images\1564321940178.png)

他只能满足高可用性（假如一台宕机，其他还能正常工作），不能满足一致性（当出现网络不稳定问题，注册的微服务不能被立即同步其他节点）。

## 那为什么CA和CP只能出现一个？

C:一致性，即保证得到的数据是最新的。如：A进行插入修改后，B去取则取得更改后的数据。那么此时，A必须锁定数据，让B无法读取，才能满足之后读到的数据是最新的。那么这个违背了A定理（B不可用了）。



A：可用性。一样的道理，如果B想要在更改数据的同时，能够查表，那么得到的数据肯定不是最新的。即C定理失效。