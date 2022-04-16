### demo说明
demo的目的仅仅是把应用跑起来作为参考，更多参数和程序健壮性请参考官方文档设置以保证客户端的稳定和性能。
相关文档请参考[filebeat-kafka](https://www.elastic.co/guide/en/beats/filebeat/current/kafka-output.html)和[Kafka官网](https://kafka.apache.org/0100/documentation.html)

#### 前置检查
请确保k8s集群和kafka集群在同一个vpc内。  

#### 步骤一：参数设置(filebeat-k8s.yaml文件中XXX的地方需要设置成实例相关的实际值)
1. KAFKA_HOSTS, 请参考文档[获取接入点](https://help.aliyun.com/document_detail/68342.html?spm=a2c4g.11186623.6.554.X2a7Ga) 
2. KAFKA_TOPIC, topic请参考文档[创建资源](https://help.aliyun.com/document_detail/68328.html?spm=a2c4g.11186623.6.549.xvKAt6)
3. KAFKA_VERSION, 设置版本，0.10.x系列设置"0.10.2", 2.x系列设置"2.0.0"


#### 步骤二: 运行
```
## create
kubectl create -f filebeat-k8s.yaml
## make sure all the pods are running
kubectl get pod -n kube-system|grep filebeat
```

#### 步骤三：查看日志
可以在控制台[查询消息](https://help.aliyun.com/document_detail/113172.html)