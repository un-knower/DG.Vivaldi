rabbitmq基本原理详析

2018年03月27日 14:32:55

eagle-hang	

https://blog.csdn.net/zhangxing52077/article/details/79710277



### rabbitmq应用场景

①异步处理

场景：用户注册后，发送短信和邮箱通知；

1.串行逻辑耗时150s



2.并行逻辑耗时100s



3.mq消息队列处理耗时55s

发送注册短信及邮箱通知不是必须的逻辑，所以放在mq中慢慢的消费，提高了系统的整体的性能



②应用解耦

场景：双11购物节的时候，用户下单后，订单系统会及时通知库存系统

1.传统的做法：订单系统接口调用库存系统



设想下，如果订单系统正在调用库存系统的时候，库存系统发生崩溃了，那么这个订单就没有成功，直接的造成了成交单的损失。

2.mq逻辑解耦



订单系统:用户下单后,订单系统完成持久化处理,将消息写入消息队列,返回用户订单下单成功。

库存系统:订阅下单的消息,获取下单消息,进行库操作。 

这样即使库存系统发生崩溃，但是订单的下单信息仍然存在消息队列中，等库存系统恢复正常后，继续调用库存接口，消息不会丢失，保证了成交量。

③并发削峰

     针对高并发请求，比如双11并发量超大的情况下，可以将用户的下单消息先存到消息队列中，这就极大的削弱了请求的并发量，然后慢慢消费消息队列中的消息；很好的避免了卡着不动的局面，用户体验也会好很多
---------------------
作者：eagle-zhang 
来源：CSDN 
原文：https://blog.csdn.net/zhangxing52077/article/details/79710277 
版权声明：本文为博主原创文章，转载请附上博文链接！