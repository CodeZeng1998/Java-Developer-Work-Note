# Java开发-面试题-0032-平时日志使用的什么框架，有哪些级别，info 用在哪里？有什么作用

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**





在 Java 开发中，常用的日志框架有以下几种：

### 常用的日志框架：

1. **Log4j 2**：这是一个功能强大且灵活的日志框架，支持异步日志、插件扩展等。
2. **SLF4J**（Simple Logging Facade for Java）：它并不是一个具体的日志框架，而是一个日志门面，可以与多种日志框架（如 Log4j、Logback）结合使用。
3. **Logback**：这是 Log4j 的后继者，由 Log4j 的作者设计，性能和配置更加出色，通常与 SLF4J 一起使用。
4. **java.util.logging**：这是 Java 标准库自带的日志框架，功能简单但使用较少。
5. **Commons Logging**：Apache 提供的日志门面，类似 SLF4J，但使用较少。

<br/>

### 日志的级别：

日志通常有以下几个级别，从低到高依次是：

1. **TRACE**：最细粒度的信息，通常用于追踪程序的执行过程。
2. **DEBUG**：用于开发阶段调试，包含比 INFO 更多的详细信息。
3. **INFO**：一般的信息，表示系统运行的正常状态。
4. **WARN**：警告信息，表示可能出现问题，但不影响系统的继续运行。
5. **ERROR**：错误信息，表示系统发生错误，但可能不致命，系统可以继续运行。
6. **FATAL**：严重错误，表示系统可能无法继续运行。

<br/>

### `INFO` 级别的使用：

`INFO` 级别的日志通常用于记录系统的关键操作或状态信息，这些信息表明系统正在按预期运行。例如：

- 系统关键业务操作
- 系统配置的加载或初始化完成
- 重要任务的开始和结束（如批处理任务）
- 缓存加载进度
- 应用程序启动或关闭

<br/>

`INFO` 级别的日志在生产环境中通常是启用的，因为它提供了足够的系统运行信息，而不会像 `DEBUG` 或 `TRACE` 那样产生大量日志，影响性能。

### `INFO` 日志的作用：

- **监控系统运行状态**：通过 `INFO` 日志，运维人员可以了解系统是否正常运行，是否执行了预期的操作。
- **问题排查**：在排查问题时，`INFO` 日志可以帮助快速定位系统的运行状态和执行过的关键操作。
- **业务审计**：对于一些业务操作，可以通过 `INFO` 日志记录关键步骤，便于审计和追踪。

**示例**：

```java
private static final Logger logger = LoggerFactory.getLogger(MyClass.class);

public void processOrder(Order order) {
    logger.info("Processing order id: {}", order.getId());
    // 处理订单的逻辑
    logger.info("Order id: {} processed successfully", order.getId());
}
```

在上面的示例中，`INFO` 日志记录了订单处理的开始和结束，这对于了解系统的运行状态和业务流程非常有用。



<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0032.png?raw=true)

上图是由 Pic 生成的

关键词：Female Captain America

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



