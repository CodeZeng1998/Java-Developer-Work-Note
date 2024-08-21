# 20-异常-Kafka创建Topic时出现UnrecognizedOptionException zookeeper is not a recognized option



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：在使用如下命令行参数创建 Kafka 的 Topic 时，出现报错 `UnrecognizedOptionException: zookeeper is not a recognized option`。(Kafka 所在的 bin 目录执行命令)

```shell
./kafka-topics.sh --create --topic [TopicName] --zookeeper localhost:2181 --replication-factor 2 --partitions 3 
```

<br/>

**报错信息：**

```txt
Exception in thread "main" joptsimple.UnrecognizedOptionException: zookeeper is not a recognized option
	at joptsimple.OptionException.unrecognizedOption(OptionException.java:108)
	at joptsimple.OptionParser.handleLongOptionToken(OptionParser.java:510)
	at joptsimple.OptionParserState$2.handleArgument(OptionParserState.java:56)
	at joptsimple.OptionParser.parse(OptionParser.java:396)
	at kafka.admin.TopicCommand$TopicCommandOptions.<init>(TopicCommand.scala:557)
	at kafka.admin.TopicCommand$.main(TopicCommand.scala:48)
	at kafka.admin.TopicCommand.main(TopicCommand.scala)
```



<br/>

**错误原因**：Kafka命令行工具的问题，其中`zookeeper`选项未被识别。这是因为从Kafka 2.4版本开始，`zookeeper`参数已被弃用，改为使用Kafka自身的内部代理连接参数。



<br/>

**解决方案**：使用新语法创建主题的方法。

```shell
./kafka-topics.sh --create --topic [TopicName] --bootstrap-server localhost:9092 --replication-factor 2 --partitions 3
```



执行结果如下表示创建 Topic 成功

```shell
Created topic [TopicName].
```





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/20-%E5%BC%82%E5%B8%B8-Kafka%E5%88%9B%E5%BB%BATopic%E6%97%B6%E5%87%BA%E7%8E%B0UnrecognizedOptionException%20zookeeper%20is%20not%20a%20recognized%20option.png?raw=true)

上图是由 Pic 生成的

关键词：A female programmer with a cyber style is typing code at her desk wearing headphones

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





