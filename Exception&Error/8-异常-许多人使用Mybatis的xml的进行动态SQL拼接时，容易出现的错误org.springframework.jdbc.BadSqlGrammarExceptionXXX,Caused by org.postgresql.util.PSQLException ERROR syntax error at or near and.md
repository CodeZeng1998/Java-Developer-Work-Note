# 8-异常-许多人使用Mybatis的xml的进行动态SQL拼接时，容易出现的错误org.springframework.jdbc.BadSqlGrammarExceptionXXX,Caused by org.postgresql.util.PSQLException ERROR syntax error at or near and



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



**问题描述**：许多新手刚开始接触 Mybatis，在 xml 里面配置一些**动态SQL**的时候，很容易忘记某些判断条件，导致如下报错的出现。



**报错信息：**

```txt
  org.springframework.jdbc.BadSqlGrammarException: 
      at org.springframework.jdbc.support.SQLErrorCodeSQLExceptionTranslator.doTranslate(SQLErrorCodeSQLExceptionTranslator.java:235) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
    at org.springframework.jdbc.support.AbstractFallbackSQLExceptionTranslator.translate(AbstractFallbackSQLExceptionTranslator.java:72) ~[spring-jdbc-5.2.1.RELEASE.jar:5.2.1.RELEASE]
  ...
  ...
Caused by: org.postgresql.util.PSQLException: ERROR: syntax error at or near "and"
    Position: 482
    at org.postgresql.core.v3.QueryExecutorImpl.receiveErrorResponse(QueryExecutorImpl.java:2553) ~[postgresql-42.2.18.jar:42.2.18]
    at org.postgresql.core.v3.QueryExecutorImpl.processResults(QueryExecutorImpl.java:2285) ~[postgresql-42.2.18.jar:42.2.18]
```

从报错信息可以看出问题出现在 `and` 的附近。



**xml里的SQ**L如下：（简略版）

```xml
    <select id="countList" resultType="com.XXX.XXX.dto.XXX">
        SELECT
        COUNT ( 1 )
        FROM
        tableName
        WHERE
        1 = 1
        <if test="codeList != null">
            and code in
            <foreach collection="codeList" item="item" separator="," open="(" close=")">#{item}</foreach>
        </if>

        GROUP BY
        iod.monitor_date,
        iod.audit_status
    </select>
```

**错误原因**：当传入的 codeList 不为 null，只是一个空集的时候就会出现上述报错。



**解决方案**：直接在动态SQL里面增加一个集合元素数量的判断即可解决问题。

```
    <select id="countList" resultType="com.XXX.XXX.dto.XXX">
        SELECT
        COUNT ( 1 )
        FROM
        tableName
        WHERE
        1 = 1
        <if test="codeList != null and codeList.size > 0">
            and code in
            <foreach collection="codeList" item="item" separator="," open="(" close=")">#{item}</foreach>
        </if>

        GROUP BY
        iod.monitor_date,
        iod.audit_status
    </select>
```









![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/8-%E5%BC%82%E5%B8%B8-%E8%AE%B8%E5%A4%9A%E4%BA%BA%E4%BD%BF%E7%94%A8Mybatis%E7%9A%84xml%E7%9A%84%E8%BF%9B%E8%A1%8C%E5%8A%A8%E6%80%81SQL%E6%8B%BC%E6%8E%A5%E6%97%B6%EF%BC%8C%E5%AE%B9%E6%98%93%E5%87%BA%E7%8E%B0%E7%9A%84%E9%94%99%E8%AF%AForg.springframework.jdbc.BadSqlGrammarExceptionXXX,Caused%20by%20org.postgresql.util.PSQLException%20ERROR%20syntax%20error%20at%20or%20near%20and.png?raw=true)

上图是由 Pic 生成的

关键词：A serene Japanese garden with a koi pond





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





