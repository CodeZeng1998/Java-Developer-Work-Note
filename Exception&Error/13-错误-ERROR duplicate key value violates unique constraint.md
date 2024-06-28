# 13-错误-ERROR: duplicate key value violates unique constraint "ux_xxx"



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：批量插入数据报错，出现插入数据重复的key已经存在的报错。



<br/>

**报错信息：**

```txt
Caused by: org.postgresql.util.PSQLException: ERROR: duplicate key value violates unique constraint "ux_xxx"
  详细：Key (id, date)=(1234d08083824f0592241c10f2241234, 2024-05-23) already exists.
	at org.postgresql.core.v3.QueryExecutorImpl.receiveErrorResponse(QueryExecutorImpl.java:2553)
	at org.postgresql.core.v3.QueryExecutorImpl.processResults(QueryExecutorImpl.java:2285)
	at org.postgresql.core.v3.QueryExecutorImpl.execute(QueryExecutorImpl.java:521)
	at org.postgresql.jdbc.PgStatement.internalExecuteBatch(PgStatement.java:870)
	at org.postgresql.jdbc.PgStatement.executeBatch(PgStatement.java:893)
```



<br/>

**错误原因**：数据库表结构设置了对应的`唯一索引`，当插入的数据不满足唯一索引的条件时，出现上述报错。



<br/>

**解决方案**：

* 如果数据库里的数据是脏数据，则**删掉数据库里的数据**；
* 如果数据库里的数据不是脏数据，则在代码层面插入数据的时候做对应的过滤操作，预防插入重复数据。
* 如果数据库里的数据不是脏数据，且是数据业务允许的操作，则直接对插入出现异常的操作进行对应的异常捕获处理即可。



<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/13-%E9%94%99%E8%AF%AF-ERROR%20duplicate%20key%20value%20violates%20unique%20constraint.png?raw=true)

上图是由 Pic 生成的

关键词：Standing on a volcano crater on a snowy mountain

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





