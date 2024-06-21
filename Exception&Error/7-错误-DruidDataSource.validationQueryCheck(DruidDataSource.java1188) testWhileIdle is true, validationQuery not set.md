# 7-错误-DruidDataSource.validationQueryCheck(DruidDataSource.java1188) testWhileIdle is true, validationQuery not set



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



**问题描述**：Spring Boot 项目启动之后出现一条报错信息，不影响运行。



**报错信息**:

```
2024-06-20 12:20:13.851 ERROR 11468 --- [           main] com.alibaba.druid.pool.DruidDataSource.validationQueryCheck(DruidDataSource.java:1188) :testWhileIdle is true, validationQuery not set
```



**配置如下：**

```yaml
spring:
  datasource:
    dynamic:
      primary: master 
      strict: false 
      datasource:
        master:
          type: com.alibaba.druid.pool.DruidDataSource
          url: jdbc:postgresql://127.0.0.1:5432/databaseName?useUnicode=true&useSSL=false&characterEncoding=utf8&useAffectedRows=true&allowMultiQueries=true&serverTimezone=GMT%2B8&useTimezone=true
          username: username
          password: 123456
          driver-class-name: org.postgresql.Driver
          druid:
            test-while-idle: true
            max-active: 300
```



**错误原因**：对应的数据库连接指定了使用 **druid** 的数据连接池，并且 **test-while-idle** 值设置为**true**，即测试数据库连接状态，但是没有指定好对应的 **validation-query**（**验证数据库连接状态的查询语句**），所以才出现上述报错。



**解决方案**：

* 去掉数据库对应的druid数据连接池；（一般不会使用）

* **配置好对应的验证数据库连接状态的查询语句**，例如上述配置可加上如下配置，即可解决这个报错。

  ```yaml
  spring:
    datasource:
      dynamic:
        primary: master
        strict: false 
        datasource:
          master:
            type: com.alibaba.druid.pool.DruidDataSource
            url: jdbc:postgresql://127.0.0.1:5432/databaseName?useUnicode=true&useSSL=false&characterEncoding=utf8&useAffectedRows=true&allowMultiQueries=true&serverTimezone=GMT%2B8&useTimezone=true
            username: username
            password: 123456
            driver-class-name: org.postgresql.Driver
            druid:
              test-while-idle: true
              validation-query: SELECT 1
              max-active: 300
  ```

  

注意：如果项目中配置了多个数据源并且都指定了使用druid连接池并且 `test-while-idle`都设置为 true 的话，记得给每个数据库连接都配置好对应的`validation-query`(验证数据库连接状态的查询语句)。



PS：

* MySQL、PostgreSQL 数据库使用的`validation-query`为`SELECT 1`;

* Oracle 数据库需要使用`SELECT 1 FROM DUAL`;否则会出现如下报错信息。

  ```txt
  [17:59:24:138] [ERROR] - com.alibaba.druid.pool.DruidDataSource.oracleValidationQueryCheck(DruidDataSource.java:1263) - invalid oracle validationQuery. SELECT 1, may should be : SELECT 1 FROM DUAL
  ```

  







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/7-%E9%94%99%E8%AF%AF-DruidDataSource.validationQueryCheck(DruidDataSource.java1188)%20testWhileIdle%20is%20true,%20validationQuery%20not%20set.png?raw=true)

上图是由 Pic 生成的

关键词：A serene mountain landscape with a clear blue lake, surrounded by lush green forests, under a bright blue sky with fluffy white clouds.





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





