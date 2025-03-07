# 23-异常-IllegalStateException Duplicate key



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：敲代码敲快了，出现的报错。报错内容很简单，就是 stream 流将 list 转换成map的时候出现了重复的 key 值，我这里是直接正确的属性对上然后group by一下就行了。大家按照自己的业务需求进行修改。



<br/>

**报错信息：**

```txt
2025-03-03 09:48:26.961 ERROR 26952 [TID: N/A] --- [nio-8080-exec-1] 返回异常内容:

java.lang.IllegalStateException: Duplicate key 252966831687782400 (attempted merging values 251528275941982208 and 252178618943610880)
	at java.base/java.util.stream.Collectors.duplicateKeyException(Collectors.java:135) ~[na:na]
	at java.base/java.util.stream.Collectors.lambda$uniqKeysMapAccumulator$1(Collectors.java:182) ~[na:na]
	at java.base/java.util.stream.ReduceOps$3ReducingSink.accept(ReduceOps.java:169) ~[na:na]
	at java.base/java.util.ArrayList$ArrayListSpliterator.forEachRemaining(ArrayList.java:1625) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499) ~[na:na]
	at java.base/java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:921) ~[na:na]
	at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234) ~[na:na]
	at java.base/java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:682) ~[na:na]
	at 
```



<br/>

**错误原因**：

该异常的直接原因是 **Map键冲突**。当使用`Collectors.toMap()`将流元素转换为Map时，若多个元素的键（由`keyMapper`生成）重复，且未定义合并策略，则会抛出`IllegalStateException`

<br/>

**解决方案**：

* **使用合并函数处理冲突**

  通过`Collectors.toMap()`的第三个参数（合并函数）明确重复键的合并逻辑。例如：

  ```java
  Map<Long, ValueType> resultMap = list.stream()
      .collect(Collectors.toMap(
          Entity::getKey,      // Key生成逻辑
          Entity::getValue,    // Value生成逻辑
          (existing, newValue) -> existing // 合并策略：保留旧值
      ));
  ```

* **使用分组方法替代**

  若需保留重复键的所有值，改用`Collectors.groupingBy()`将键对应的值分组到集合中：

  ```java
  Map<Long, List<ValueType>> groupedMap = list.stream()
      .collect(Collectors.groupingBy(
          Entity::getKey,
          Collectors.mapping(Entity::getValue, Collectors.toList())
      ));
  ```

* **数据源去重**

  在流处理前清理重复数据：

  ```java
  List<Entity> uniqueList = list.stream()
      .filter(distinctByKey(Entity::getKey))  // 自定义去重方法
      .collect(Collectors.toList());
  ```



该异常本质是**键冲突的未处理状态**，需通过合并函数、数据清理或分组策略解决。建议优先使用合并函数（如保留最新值或累加值），并结合业务场景选择最合适的处理方式。

<br/>



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



