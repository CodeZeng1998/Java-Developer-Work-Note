# 18-错误-Spring Boot 启动报错Caused by java.net.BindException Address already in use



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：最近项目代码里面将一个模块的代码独立成一个服务，出现java.net.BindException Address already in use的报错导致服务启动失败。



<br/>

**报错信息：**

```txt
[11:53:47:713] [ERROR] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:182) - Failed to start connector [Connector[HTTP/1.1-8088]]
org.apache.catalina.LifecycleException: Failed to start component [Connector[HTTP/1.1-8088]]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:167) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.apache.catalina.core.StandardService.addConnector(StandardService.java:225) [tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.springframework.boot.web.embedded.tomcat.TomcatWebServer.addPreviouslyRemovedConnectors(TomcatWebServer.java:256) [spring-boot-2.0.3.RELEASE.jar:2.0.3.RELEASE]
...
...
...
Caused by: org.apache.catalina.LifecycleException: Protocol handler start failed
	at org.apache.catalina.connector.Connector.startInternal(Connector.java:1020) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150) ~[tomcat-embed-core-8.5.31.jar:8.5.31]
	... 18 more
Caused by: java.net.BindException: Address already in use
...
...
...
	... 18 more
[11:53:47:721] [INFO] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:180) - Pausing ProtocolHandler ["http-nio-8088"]
[11:53:47:721] [INFO] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:180) - Stopping service [Tomcat]
[11:53:47:814] [INFO] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:180) - The stop() method was called on component [StandardServer[-1]] after stop() had already been called. The second call will be ignored.
[11:53:47:815] [INFO] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:180) - Stopping ProtocolHandler ["http-nio-8088"]
[11:53:47:815] [INFO] - org.apache.juli.logging.DirectJDKLog.log(DirectJDKLog.java:180) - Destroying ProtocolHandler ["http-nio-8088"]
[11:53:47:819] [INFO] - org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener.logAutoConfigurationReport(ConditionEvaluationReportLoggingListener.java:101) - 

Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
[11:53:47:821] [ERROR] - org.springframework.boot.diagnostics.LoggingFailureAnalysisReporter.report(LoggingFailureAnalysisReporter.java:42) - 

***************************
APPLICATION FAILED TO START
***************************

Description:

The Tomcat connector configured to listen on port 8088 failed to start. The port may already be in use or the connector may be misconfigured.

Action:

Verify the connector's configuration, identify and stop any process that's listening on port 8088, or configure this application to listen on another port.
```



<br/>

**错误原因**：错误信息表明，应用程序未能启动，因为端口8088已被占用。其实这个看报错信息就可以知道是什么问题了，独立模块的时候使用了和这台服务器端口号有冲突，要么就是其他服务也使用了这个端口号，要么就是无关的服务使用的这个端口号导致服务启动失败。



<br/>

**解决方案**：

* 可以更换端口号的，直接更换端口号即可；
* 不能更换端口号的，只能直接把其他服务给干掉了，并将其他服务的端口号更改掉。



<br/>

以下是解决此问题的步骤：

1. **识别使用端口8088的进程：**

   - 在Windows上：打开命令提示符并运行：

     ```shell
     netstat -ano | findstr :8088
     ```

     此命令将列出使用端口8088的进程。查看输出中的PID（进程ID）。

   - 在Linux/Mac上：打开终端并运行：

     ```shell
     lsof -i :8088
     ```

     此命令将列出使用端口8088的进程的详细信息，包括PID。

2. **终止进程：**

   - 在Windows上：使用前一步骤中获得的PID并运行：

     ```shell
     taskkill /PID <PID> /F
     ```

   - 在Linux/Mac上：使用前一步骤中获得的PID并运行：

     ```shell
     kill -9 <PID>
     ```

3. **更改应用程序的端口：**

   - 如果您不想终止使用端口8088的进程，可以更改应用程序使用的端口。

   - 在Spring Boot应用程序中，您可以在`application.properties`或`application.yml`

     文件中更改端口：

     ```properties
     server.port=8089
     ```







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/18-%E9%94%99%E8%AF%AF-Spring%20Boot%20%E5%90%AF%E5%8A%A8%E6%8A%A5%E9%94%99Caused%20by%20java.net.BindException%20Address%20already%20in%20use.png?raw=true)

上图是由 Viva 生成的

关键词：a girl cycling on the road

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





