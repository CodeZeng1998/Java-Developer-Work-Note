# 17-异常-BadSqlGrammarException ERROR relation xxxTableName does not exist



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>

**问题描述**：最近在测试环境上线新版本内容时，发现报错（ERROR: relation "xxxTableName" does not exist），经过排查才知道是有一些开发人员的开发环境的数据库表并未在测试环境中创建，导致Kafka接受消息进行新增数据的时候出现报错。

<br/>

**报错信息：**

```txt
nested exception is org.springframework.jdbc.BadSqlGrammarException: com.xxxTableNameMapper.insert (batch index #1) failed. Cause: java.sql.BatchUpdateException: Batch entry 0 INSERT INTO xxxTableName  ( id,
create_time)  VALUES  ( 1808704007527600128,
1720063614 ) was aborted: ERROR: relation "xxxTableName" does not exist
  Position: 13  Call getNextException to see other errors in the batch.

) was aborted: ERROR: relation "xxxTableName" does not exist
  Position: 13  Call getNextException to see other errors in the batch.
; bad SQL grammar []; nested exception is org.postgresql.util.PSQLException: ERROR: relation "xxxTableName" does not exist
  Position: 13
	...
	...
	...
Caused by: org.springframework.jdbc.BadSqlGrammarException: com.xxxTableNameMapper.insert (batch index #1) failed. Cause: java.sql.BatchUpdateException: Batch entry 0 INSERT INTO xxxTableName  ( id,
create_time)  VALUES  ( 1808704007527600128,
1720063614 ) was aborted: ERROR: relation "xxxTableName" does not exist
  Position: 13  Call getNextException to see other errors in the batch.
; bad SQL grammar []; nested exception is org.postgresql.util.PSQLException: ERROR: relation "xxxTableName" does not exist
  Position: 13
	at org.springframework.jdbc.support.SQLErrorCodeSQLExceptionTranslator.doTranslate(SQLErrorCodeSQLExceptionTranslator.java:235) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:72) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.mybatis.spring.MyBatisExceptionTranslator.translateExceptionIfPossible(MyBatisExceptionTranslator.java:73) ~[mybatis-spring-2.0.1.jar:2.0.1]
	...
	...
	...
	... 7 more
```



<br/>

**错误原因**：该错误表示数据库中不存在表 `xxxTableName`。这可能是由几个原因引起的：

1. **数据库中没有创建 `xxxTableName` 表。**
2. **表名 `xxxTableName` 不正确或与实际表名不匹配。**
3. **数据库模式未正确设置，导致表缺失。**

<br/>

**解决方案**：我这里排查到的是开发人员疏忽导致测试环境和开发环境的数据库表结构没同步好（即错误原因一），因此只需要将对应的数据库表创建好即可。



**建议：如果都是多人开发且多个环境进行部署可优先考虑给项目引入数据库表结构同步工具，不要由人工进行数据库表的同步，减少出现错误的可能。**例如：引入 **Flyway** 等



<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/17-%E5%BC%82%E5%B8%B8-BadSqlGrammarException%20ERROR%20relation%20xxxTableName%20does%20not%20exist.png?raw=true)

上图是由 Pic 生成的

关键词：Ghost Slaying Blade

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





