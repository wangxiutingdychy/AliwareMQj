Spring Cloud Stream Kafka Demo
==========
Demo project of Spring Cloud Stream with [Aliyun Kafka](https://help.aliyun.com/document_detail/52374.html?spm=5176.product29530.6.559.4WA0eL)
使用前请先阅读[上一层目录的README](https://github.com/AliwareMQ/aliware-kafka-demos)
## Demo跑起来
1. 将项目导入IDE(如MyEclipse, IntelliJ)中
2. 添加自己的AccessKey，SecretKey到src/main/resources/kafka_client_jaas.conf中
3. 在[Aliyun MQ控制台](https://ons.console.aliyun.com)申请Topic和CID
4. 将Topic与CID添加到src/main/resources/application.properties
5. spring.cloud.stream.kafka.binder.brokers请根据region列表进行选取
6. 修改src/main/resources/application.properties中的kafka.ssl.truststore.location为自己的路径
7. 执行KafkaDemoApplication.main,以启动消息消费的监听器，将直接打印消息内容
8. 执行MessageOutputTest.testSend测试发送，看Console中是否打印"Hello Aliyun Kafka"
9. 遇到问题先参考[Kafka常见报错及排查](https://help.aliyun.com/document_detail/57058.html)
10. 欢迎加钉钉群咨询，用钉钉扫描[群二维码](http://img3.tbcdn.cn/5476e8b07b923/TB1HEQgQpXXXXbdXVXXXXXXXXXX) 

## 生产环境部署问题
### 1. SASL配置系统变量
Aliyun Kafka采用SASL机制对通道进行鉴权，在此之前，需要配置JVM属性java.security.auth.login.config
首先将src/main/resources/kafka_client_jaas.conf（注意配置自己的AccessKey，SecretKey）放置在某个路径下，如/home/admin；
然后可以采用以下方式配置(二选一，建议采用第二种)
#### 1.1 -D配置方式
jvm启动时加上 -Djava.security.auth.login.config=/home/admin/kafka_client_jaas.conf
#### 1.2 设置Spring的启动监听器
编写自己类实现ApplicationListener，可以参考Demo工程中的KafkaConfigListener，只是记得要把路径改成自己的；
然后在src/main/resources/META-INF/spring.factories中配置类的全称，参考demo：
org.springframework.context.ApplicationListener=com.alibaba.cloud.KafkaConfigListener
### 2. SSL配置Kafka属性
将src/main/resources/kafka.client.truststore.jks放在某个目录下，然后参考
application.properties进行配置

## 报错“Failed to send SSL close message”
该错误后面通常还会跟“connection reset by peer”或“broken pipe”。该错误可以忽略，不影响使用。服务端是VIP网络环境，会主动掐掉空闲连接。
你可以通过修改日志级别来避免该错误，以log4j为例，加上下面这行配置：
`log4j.logger.org.apache.kafka.common.network.SslTransportLayer=ERROR`


