##process
### Kafka <div id='kafka'></div>
* windows平台运行
```
    //开启kafka依赖Zookeeper
    zkserver
    
    //进入kafka安装目录
    cd D:\kafka_2.12-2.0.0
    
    //开启kafka
    bin\windows\kafka-server-start.bat config\server.properties
    
    //创建主题
    bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic 
    test1
    
    //查看主题
    bin\windows\kafka-topics.bat --list --zookeeper localhost:2181
    
    //生产者（写入）
    bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test1
    
    //消费者（读取）
    bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test1 --from-beginning
    
    //查看版本
    find ./libs/ -name \*kafka_\* | head -1 | grep -o '\kafka[^\n]*'
        kafka_2.12-2.0.0-javadoc.jar
        如上所示，2.12是Scala 的版本，2.0.0就是kafka的版本
    或
    find ./libs/ -name 'kafka_*.jar.asc' |head -n1 | cut -d'/' -f3
        kafka_2.12-2.0.0-javadoc.jar.asc
        如上所示，2.12是Scala 的版本，2.0.0就是kafka的版本
            
    //查看消费group
    bin\windows\kafka-consumer-groups.bat --bootstrap-server 127.0.0.1:9092 --list
    
    
```







