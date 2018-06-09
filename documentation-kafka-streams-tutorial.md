### [有错页面](http://kafka.apache.org/11/documentation/streams/tutorial)

在运行第一个Pipe Demo的时候，没有明确指出topic的create命令，和生产者消费者运行的命令。如果按照[前文](http://kafka.apache.org/11/documentation/streams/quickstart)的命令，会抛出`org.apache.kafka.common.errors.CorruptRecordException`，这里正确的写法应该是：

* create topic 
```bash
bin/kafka-topics.sh --create \
--zookeeper localhost:2181 \
--replication-factor 1 \
--partitions 1 \
--topic streams-pipe-output
```

* run console-consumer

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
--topic streams-pipe-output \
--from-beginning \
--formatter kafka.tools.DefaultMessageFormatter \
--property print.key=true \
--property print.value=true \
--property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
--property value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
```

而在运行第三个Wordcount Demo的时候，则需要将消费者启动参数中的值反序列化设置为`org.apache.kafka.common.serialization.LongDeserializer`：

* run console-consumer

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
--topic streams-pipe-output \
--from-beginning \
--formatter kafka.tools.DefaultMessageFormatter \
--property print.key=true \
--property print.value=true \
--property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
--property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```

详细信息参见[此处](https://stackoverflow.com/questions/49098274/kafka-stream-get-corruptrecordexception)。