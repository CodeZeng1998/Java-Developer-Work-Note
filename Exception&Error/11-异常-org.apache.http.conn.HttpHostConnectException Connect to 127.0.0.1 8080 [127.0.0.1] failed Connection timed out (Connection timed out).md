# 11-异常-org.apache.http.conn.HttpHostConnectException Connect to 127.0.0.1 8080 [127.0.0.1] failed Connection timed out (Connection timed out)



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：调用接口出现连接超时问题。



<br/>

**报错信息：**

```txt
[14:46:34:372] [ERROR] - com.xxx.utils.xxx.printErrorLog(xxx.java:25) - 系统发生错误，信息如下:
org.springframework.web.client.ResourceAccessException: I/O error on POST request for "http://127.0.0.1:8080/xxx/api/xxx": Connect to 127.0.0.1:8080 [/127.0.0.1] failed: Connection timed out (Connection timed out); nested exception is org.apache.http.conn.HttpHostConnectException: Connect to 127.0.0.1:8080 [/127.0.0.1] failed: Connection timed out (Connection timed out)
	at org.springframework.web.client.RestTemplate.doExecute(RestTemplate.java:751) ~[spring-web-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.web.client.RestTemplate.execute(RestTemplate.java:677) ~[spring-web-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.web.client.RestTemplate.postForObject(RestTemplate.java:421) ~[spring-web-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	... 152 more
Caused by: org.apache.http.conn.HttpHostConnectException: Connect to 127.0.0.1:8080 [/127.0.0.1] failed: Connection timed out (Connection timed out)
	... 152 more
Caused by: java.net.ConnectException: Connection timed out (Connection timed out)
	... 152 more
```



<br/>

**错误原因**：错误信息表明，连接到 `127.0.0.1:8088` 服务器时出现了问题。具体的异常是 `Connection timed out`，这意味着应用程序在允许的时间内无法建立连接。



<br/>

**解决方案**：

1. **检查服务器可用性：**

   - 验证 `127.0.0.1` 服务器是否正在运行。
   - 确保服务器正在监听 `8088` 端口。

2. **网络问题：**

   - 检查是否存在导致超时的网络问题。这包括防火墙设置、网络配置或 VPN 问题。
   - 使用 `ping` 或 `telnet` 工具测试到 `127.0.0.1:8088` 的连接。

3. **服务器配置：**

   - 确保服务器应用程序（在 `127.0.0.1:8088`）配置正确，能够接受请求。
   - 检查服务器是否有任何访问限制或速率限制导致问题。

4. **应用程序配置：**

   - 确认 Spring 应用程序中的 `RestTemplate` 配置正确。
   - 检查是否需要任何代理设置以建立连接。

5. **超时设置：**

   - 增加 `RestTemplate` 中的超时设置，以允许更多时间来建立连接。例如：

   ```java
   SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
   factory.setConnectTimeout(5000); // 5 秒
   factory.setReadTimeout(5000); // 5 秒
   RestTemplate restTemplate = new RestTemplate(factory);
   ```

6. **日志和监控：**

   - 检查服务器日志中的任何错误或警告，以获取更多信息。
   - 使用监控工具观察服务器的健康状况和性能。

   

* 示例：增加 RestTemplate 中的超时

```java
@Bean
public RestTemplate restTemplate() {
    SimpleClientHttpRequestFactory factory = new SimpleClientHttpRequestFactory();
    factory.setConnectTimeout(10000); // 10 秒
    factory.setReadTimeout(10000); // 10 秒
    return new RestTemplate(factory);
}
```



* 示例：使用 Telnet 测试连接

```shell
telnet 127.0.0.1 8088
```

如果 `telnet` 成功连接，则确认服务器可达。如果不成功，可能存在网络问题或服务器宕机。



* 示例：使用 Ping 检查服务器

```shell
ping 127.0.0.1
```

这有助于确定服务器在基本网络层级是否可达。

<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/11-%E5%BC%82%E5%B8%B8-org.apache.http.conn.HttpHostConnectException%20Connect%20to%20127.0.0.1%208080%20%5B127.0.0.1%5D%20failed%20Connection%20timed%20out%20(Connection%20timed%20out).png?raw=true)

上图是由 Pic 生成的

关键词：A cozy cabin in the snowy mountains

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





