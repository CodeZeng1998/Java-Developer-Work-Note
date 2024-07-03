# 16-异常-造成org.postgresql.util.PSQLException ERROR multiple assignments to same column xxx的两种可能原因及解决办法



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：运行程序时，某个功能出现了org.postgresql.util.PSQLException ERROR multiple assignments to same column "xxx"的报错。



<br/>

**报错信息：**

```txt
org.springframework.jdbc.BadSqlGrammarException: 
### Error updating database.  Cause: org.postgresql.util.PSQLException: ERROR: multiple assignments to same column "xxx"
### The error may exist in com/XxxMapper.java (best guess)
### The error may involve com.XxxMapper.update-Inline
### The error occurred while setting parameters
### SQL: UPDATE tableName  SET xxx=?, xxx=? WHERE  id=0  AND  flag=0
### Cause: org.postgresql.util.PSQLException: ERROR: multiple assignments to same column "xxx"
; bad SQL grammar []; nested exception is org.postgresql.util.PSQLException: ERROR: multiple assignments to same column "xxx"
	at org.springframework.jdbc.support.SQLErrorCodeSQLExceptionTranslator.doTranslate(SQLErrorCodeSQLExceptionTranslator.java:235) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:72) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.mybatis.spring.MyBatisExceptionTranslator.translateExceptionIfPossible(MyBatisExceptionTranslator.java:73) ~[mybatis-spring-2.0.1.jar:2.0.1]
	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:446) ~[mybatis-spring-2.0.1.jar:2.0.1]
	at com.sun.proxy.$Proxy126.update(Unknown Source) ~[?:?]
	...
	...
	...
Caused by: org.postgresql.util.PSQLException: ERROR: multiple assignments to same column "xxx"
	at org.postgresql.core.v3.QueryExecutorImpl.receiveErrorResponse(QueryExecutorImpl.java:2553) ~[postgresql-42.2.18.jar:42.2.18]
	...
	...
	...
	at org.mybatis.spring.SqlSessionTemplate$SqlSessionInterceptor.invoke(SqlSessionTemplate.java:433) ~[mybatis-spring-2.0.1.jar:2.0.1]
	... 59 more
```

<br/>

简略版SQL：

```sql
UPDATE tableName  SET xxx=1, xxx=1 WHERE  id=0  AND  flag=0;
```



<br/>

**错误原因和解决方案**：（**最终原因：对同一个字段进行了多次赋值操作。**）

* **可能的原因一**：Mybatis 对应的 xml 文件编写的 SQL 直接对同一个属性进行了两次 set 设置值的操作或者满足多个条件导致拼接了两条属性一样的 set 设置值的 SQL，因此出现上述报错。

  * **解决方案**：删除多余的 set 语句。

  * 例如下面的SQL：

    ```sql
    // 修改前
    UPDATE tableName  SET xxx=1, xxx=1 WHERE  id=0  AND  flag=0;
    
    // 修改后
    UPDATE tableName  SET xxx=1 WHERE  id=0  AND  flag=0;
    ```

    

* **可能的原因二**：使用**Mybatis Plus** 的 **Wrappe** 进行数据的修改，用到了对应的如下方法对数据进行修改并且在传入的 **updateWrapper** 里面使用了 set 的动态拼接，因此出现上述报错。

  ```java
  //com.baomidou.mybatisplus.extension.service.IService#update(T,com.baomidou.mybatisplus.core.conditions.Wrapper<T>)
  
  default boolean update(T entity, Wrapper<T> updateWrapper) {
      return SqlHelper.retBool(this.getBaseMapper().update(entity, updateWrapper));
  }
  ```

  * **解决方案**：方法`com.baomidou.mybatisplus.extension.service.IService#update(T,com.baomidou.mybatisplus.core.conditions.Wrapper<T>)`会自动根据传入的 **T**（实体类）是否有值进行 SQL 的设置值的动态拼接，如果这时候再在 updateWrapper 又再次进行 SQL 的动态拼接就会造成上述问题的出现，所以如果使用这个自带的方法进行数据的更新操作时，要注意不要在 updateWrapper 里面又进行一次动态设置值的 SQL 的拼接。



<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/16-%E5%BC%82%E5%B8%B8-%E9%80%A0%E6%88%90org.postgresql.util.PSQLException%20ERROR%20multiple%20assignments%20to%20same%20column%20xxx%E7%9A%84%E4%B8%A4%E7%A7%8D%E5%8F%AF%E8%83%BD%E5%8E%9F%E5%9B%A0%E5%8F%8A%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95.png?raw=true)

上图是由 Pic 生成的

关键词：Watching Sharks in the Sea of Antarctica

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





