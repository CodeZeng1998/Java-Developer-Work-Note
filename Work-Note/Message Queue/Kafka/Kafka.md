# Kafka

[TOC]





# 1.基本介绍

Kafka传 统定义：Kafka是一个分布式的基于发布/订阅模式的消息队列（Message Queue），主要应用于大数据实时处理领域。 

发布/订阅：消息的发布者不会将消息直接发送给特定的订阅者，而是将发布的消息 分为不同的类别，订阅者只接收感兴趣的消息。

Kafka最 新定义 ： Kafka是 一个开源的分布式事件流平台 （Event Streaming Platform），被数千家公司用于高性能数据管道、流分析、数据集成和关键任务应用。



传统的消息队列的主要应用场景包括：缓存/消峰、解耦和异步通信。



# 2.Linux 相关


```shell
# 【进入kafka/bin目录】

# 查看kafka版本 
./kafka-topics.sh --version

# 启动 Kafka 服务
./kafka-server-start.sh ../config/server.properties


./kafka-server-stop.sh ../config/server.properties


# 查看所有topic
./kafka-topics.sh --list --zookeeper localhost:2181

# 发送消息到某个 topic
./kafka-console-producer.sh --topic <topic_name> --bootstrap-server localhost:9092
./kafka-console-producer.sh --broker-list localhost:9092 --topic <topic_name>

# 从主题消费消息
./kafka-console-consumer.sh --topic <topic_name> --bootstrap-server <bootstrap_server> [--from-beginning]

# 消费某个 tiopic 最早的一条数据
./kafka-console-consumer.sh --topic <topic_name> --bootstrap-server <bootstrap_server> --from-beginning --max-messages 1

# 消费某个 tiopic 最新的一条数据
./kafka-console-consumer.sh --topic <topic_name> --bootstrap-server <bootstrap_server> --max-messages 1


# 创建 topic 3是分区数量，1是复制因子（副本数）。
./kafka-topics.sh --create --zookeeper localhost:2181 --topic topicName --partitions 3 --replication-factor 1

```



## topic

```shell
# 查看主题详细信息
./kafka-topics.sh --describe --zookeeper localhost:2181 --topic <Topic Name>

# 创建主题
./kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor <副本数量> --partitions <分区数量> --topic <Topic Name>
./kafka-topics.sh --bootstrap-server localhost:9092 --create --replication-factor <副本数量> --partitions <分区数量> --topic <Topic Name>

# 删除 Topic
./kafka-topics.sh --zookeeper localhost:2181 --delete --topic <Topic Name>
```





## offset

```shell
# 将某个 groupid 指定 topic 的 offset 跳转到最新的 offset
./kafka-consumer-groups.sh --bootstrap-server <bootstrap-server> --group <groupId> --reset-offsets --topic <topicName> --to-latest --execute

# 将某个 groupid 指定 topic 的 offset 跳转到指定时间 
#日期时间应该使用 ISO 8601 格式，即 yyyy-MM-ddTHH:mm:ss.xxx，其中 T 是日期和时间之间的分隔符
./kafka-consumer-groups.sh --bootstrap-server <bootstrap-server> --group <groupId> --reset-offsets --topic <topicName> --to-datetime desired-timestamp --execute
```



## groupid

```shell
# 查看指定 groupId 的 Kafka 消费者组的信息
# GROUP：消费者组 ID、TOPIC：消费者组订阅的主题、PARTITION：显示每个主题中消费者订阅的特定分区
# CURRENT-OFFSET：显示每个消费者在每个分区中最后消费的消息偏移量
# LOG-END-OFFSET：显示分区的当前末端偏移量 (即下一个要生成的的消息的偏移量)
# LAG：此处计算 CURRENT-OFFSET 和 LOG-END-OFFSET 之间的差值，指示消费者落后于分区中最新消息的程度
# CONSUMER-ID：每个消费者实例提供唯一的标识符、HOST：运行消费者的机器的主机名或 IP 地址
# CLIENT-ID:使用消费者的应用程序提供标识符
./kafka-consumer-groups.sh --bootstrap-server <localhost:9092> --describe --group <groupId>
```







# 3.Spring Boot 配置多数据源Kafka（非集群）

```yaml
spring:
  kafka:
    one:
      bootstrap-servers: IP:PORT
      consumer: # consumer消费者
        group-id: LogSystemGroup # 默认的消费组ID
        enable-auto-commit: true # 是否自动提交offset
        auto-commit-interval: 100  # 提交offset延时(接收到消息后多久提交offset)
        auto-offset-reset: latest
        key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
        value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      properties:
        security:
          protocol: SASL_PLAINTEXT
        sasl:
          mechanism: SCRAM-SHA-256
          jaas:
            config: 'org.apache.kafka.common.security.scram.ScramLoginModule required username="username" password="password";'
    two:
      consumer:
        enable-auto-commit: false
        key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
        value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
        auto-offset-reset: earliest
        bootstrap-servers: IP:PORT
        group-id: 消费组ID
      listener:
        ack-mode: manual
        concurrency: 6
    three:
      producer:
        bootstrap-servers: IP:PORT
        key-serializer: org.apache.kafka.common.serialization.StringSerializer
        value-serializer: org.apache.kafka.common.serialization.StringSerializer
        client-id: 客户端ID
      properties:
        security:
          protocol: SASL_PLAINTEXT
        sasl:
          mechanism: SCRAM-SHA-256
          jaas:
            config: 'org.apache.kafka.common.security.scram.ScramLoginModule required username="username" password="password";'
```



配置类编写：

```java

@Configuration
@EnableKafka
public class KafkaOneConfig {

    @Value("${spring.kafka.one.bootstrap-servers}")
    private String bootstrapServers;

    @Value("${spring.kafka.one.consumer.group-id}")
    private String groupId;

    @Value("${spring.kafka.one.consumer.enable-auto-commit}")
    private boolean enableAutoCommit;

    @Value("${spring.kafka.one.consumer.auto-commit-interval}")
    private int autoCommitInterval;

    @Value("${spring.kafka.one.consumer.auto-offset-reset}")
    private String autoOffsetReset;

    @Value("${spring.kafka.one.consumer.key-deserializer}")
    private Class<?> keyDeserializer;

    @Value("${spring.kafka.one.consumer.value-deserializer}")
    private Class<?> valueDeserializer;

    @Value("${spring.kafka.one.properties.security.protocol}")
    private String securityProtocol;

    @Value("${spring.kafka.one.properties.sasl.mechanism}")
    private String saslMechanism;

    @Value("${spring.kafka.one.properties.sasl.jaas.config}")
    private String jaasConfig;

    @Bean(name = "kafkaOneTemplate")
    public KafkaTemplate<Object, Object> kafkaOneTemplate() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        return new KafkaTemplate<>(new DefaultKafkaProducerFactory<>(configProps));
    }

    @Bean(name = "kafkaOneConsumerFactory")
    public ConsumerFactory<Object, Object> kafkaOneConsumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapServers);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, enableAutoCommit);
        props.put(ConsumerConfig.AUTO_COMMIT_INTERVAL_MS_CONFIG, autoCommitInterval);
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, autoOffsetReset);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, keyDeserializer);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, valueDeserializer);
        props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, securityProtocol);
        props.put(SaslConfigs.SASL_MECHANISM, saslMechanism);
        props.put(SaslConfigs.SASL_JAAS_CONFIG, jaasConfig);
        return new DefaultKafkaConsumerFactory<>(props);
    }


    @Bean(name = "kafkaOneListenerContainerFactory")
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<Object, Object>> kafkaOneListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<Object, Object> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(kafkaOneConsumerFactory());
        return factory;
    }

}


@Configuration
@EnableKafka
public class KafkaTwoConfig {
   @Value("${spring.kafka.two.consumer.bootstrap-servers}")
    private String consumerBootstrapServers;

    @Value("${spring.kafka.two.consumer.enable-auto-commit}")
    private boolean enableAutoCommit;

    @Value("${spring.kafka.two.consumer.auto-offset-reset}")
    private String autoOffsetReset;

    @Value("${spring.kafka.two.consumer.key-deserializer}")
    private Class<?> keyDeserializer;

    @Value("${spring.kafka.two.consumer.value-deserializer}")
    private Class<?> valueDeserializer;

    @Value("${spring.kafka.two.consumer.group-id}")
    private String groupId;

    @Value("${spring.kafka.two.listener.ack-mode}")
    private String ackMode;

    @Value("${spring.kafka.two.listener.concurrency}")
    private int concurrency;

    @Bean(name = "kafkaTwoConsumerFactory")
    public ConsumerFactory<String, String> kafkaTwoConsumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, consumerBootstrapServers);
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, enableAutoCommit);
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, autoOffsetReset);
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, keyDeserializer);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, valueDeserializer);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, groupId);
        return new DefaultKafkaConsumerFactory<>(props);
    }

    @Bean(name = "kafkaTwoListenerContainerFactory")
    public KafkaListenerContainerFactory<ConcurrentMessageListenerContainer<String, String>> kafkaTwoListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(kafkaTwoConsumerFactory());
        factory.getContainerProperties().setAckMode(AbstractMessageListenerContainer.AckMode.MANUAL_IMMEDIATE);
        factory.setConcurrency(concurrency);
        return factory;
    }

}



@Configuration
@EnableKafka
public class KafkaThreeConfig {

    @Value("${spring.kafka.three.producer.bootstrap-servers}")
    private String producerBootstrapServers;


    @Value("${spring.kafka.three.producer.client-id}")
    private String producerClientId;


    @Value("${spring.kafka.three.producer.key-serializer}")
    private Class<?> keySerializer;

    @Value("${spring.kafka.three.producer.value-serializer}")
    private Class<?> valueSerializer;


    @Value("${spring.kafka.three.properties.security.protocol}")
    private String securityProtocol;

    @Value("${spring.kafka.three.properties.sasl.mechanism}")
    private String saslMechanism;

    @Value("${spring.kafka.three.properties.sasl.jaas.config}")
    private String jaasConfig;


    @Bean(name = "kafkaThreeProducerFactory")
    public ProducerFactory<String, String> kafkaThreeProducerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, producerBootstrapServers);
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, keySerializer);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, valueSerializer);
        configProps.put(ProducerConfig.CLIENT_ID_CONFIG, producerClientId);

        configProps.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, securityProtocol);
        configProps.put(SaslConfigs.SASL_MECHANISM, saslMechanism);
        configProps.put(SaslConfigs.SASL_JAAS_CONFIG, jaasConfig);

        return new DefaultKafkaProducerFactory<>(configProps);
    }

    @Bean(name = "kafkaThreeTemplate")
    public KafkaTemplate<String, String> kafkaThreeTemplate() {
        return new KafkaTemplate<>(kafkaThreeProducerFactory());
    }

}
```


生产者使用例子：
```java
@Autowired
private KafkaTemplate<String, String> kafkaThreeTemplate;


// 使用 KafkaTemplate 发送消息，并获取发送结果的 ListenableFuture 对象
ListenableFuture<SendResult<String, String>> future = kafkaThreeTemplate.send(topic, jsonobject.toJSONString());

// 添加回调函数来处理发送结果
future.addCallback(new ListenableFutureCallback<SendResult<String, String>>() {
    @Override
    public void onSuccess(SendResult<String, String> result) {
        // 消息发送成功的处理逻辑
        log.info("【Success】【转发消息】成功，发送的消息为：key:{},value:{}", result.getProducerRecord().key(), result.getProducerRecord().value());
    }

    @Override
    public void onFailure(Throwable ex) {
        // 消息发送失败的处理逻辑
        log.error("【Error】【转发消息】失败", ex);
    }
});
```
消费者使用例子：

```java
    @KafkaListener(topics = "topic", containerFactory = "kafkaTwoListenerContainerFactory")
    public void onMessage(ConsumerRecord<String, String> record, Acknowledgment acknowledgment) {}
```







