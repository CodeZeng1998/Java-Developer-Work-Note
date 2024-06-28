# 14-异常-NoSuchBeanDefinitionException No qualifying bean of type 'cn.hutool.core.lang.Snowflake' available expected at least 1 bean which qualifies as autowire candidate



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：最近项目组加入了新的开发，由于项目有多个服务，有些服务不是公用，当他在其他服务直接注入没有引入对应模块的配置时，出现了下面的报错。

```java
import cn.hutool.core.lang.Snowflake;

@Resource 
private Snowflake snowflake;
```



<br/>

**报错信息：**

```txt
rg.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'cn.hutool.core.lang.Snowflake' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.raiseNoMatchingBeanFound(DefaultListableBeanFactory.java:1695) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1253) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1207) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:636) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:116) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:397) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1429) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:594) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:517) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:323) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:321) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:202) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:879) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:878) ~[spring-context-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:550) ~[spring-context-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:141) ~[spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:747) [spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:397) [spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:315) [spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1226) [spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1215) [spring-boot-2.2.1.RELEASE.jar:2.2.1.RELEASE]
	at com.xxx.dispatxxxch.XXXXApplication.main(XXXXApplication.java:19) [classes/:?]

2024-06-28 09:55:23.057 ERROR 24856 --- [           main] org.springframework.boot.diagnostics.LoggingFailureAnalysisReporter.report(LoggingFailureAnalysisReporter.java:40) :

***************************
APPLICATION FAILED TO START
***************************

Description:

Field snowflake in com.xxx.XXX required a bean of type 'cn.hutool.core.lang.Snowflake' that could not be found.

The injection point has the following annotations:
	- @org.springframework.beans.factory.annotation.Autowired(required=true)


Action:

Consider defining a bean of type 'cn.hutool.core.lang.Snowflake' in your configuration.


Process finished with exit code 130
```



<br/>

**错误原因**：编写代码的模块的服务并未引入有 **Snowflake** 配置的模块内容。



<br/>

**解决方案**：

错误信息表明，Spring Boot 无法找到类型为 `cn.hutool.core.lang.Snowflake` 的 bean 以注入到一个类的字段中。要解决此问题，您需要在 Spring 配置中定义一个 `Snowflake` bean。

以下是定义 `Snowflake` bean 的步骤：

1. **创建配置类:**
   - 定义一个配置类，在其中创建一个 `Snowflake` bean。
2. **定义 Snowflake Bean:**
   - 使用 `@Bean` 注解创建和配置 `Snowflake` bean。



**配置类示例**

创建一个新的 Java 类，例如 `SnowflakeConfig`，并使用 `@Configuration` 注解。

```java
import cn.hutool.core.lang.Snowflake;
import cn.hutool.core.util.IdUtil;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SnowflakeConfig {

    @Bean
    public Snowflake snowflake() {
        // 根据需要自定义 workerId 和 datacenterId
        return IdUtil.createSnowflake(1, 1);
    }
}
```

**确保组件扫描**

确保您的主应用程序类或其他配置类包含 `SnowflakeConfig` 所在包的组件扫描。默认情况下，Spring Boot 扫描主应用程序类的包及其子包。

例如，如果您的 `SnowflakeConfig` 在 `com.xxx.config` 包中，而您的主应用程序类在 `com.xxx` 包中，您不需要做任何特别的操作。但是，如果它们在不同的包中，您可能需要指定组件扫描的基础包。

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ComponentScan;

@SpringBootApplication
@ComponentScan(basePackages = {"com.xxx", "com.xxx.config"}) // 根据需要调整包名称
public class XxxAppApplication {
    public static void main(String[] args) {
        SpringApplication.run(XxxAppApplication.class, args);
    }
}
```

**总结**

通过在配置类中定义 `Snowflake` bean 并确保您的应用程序扫描适当的包，您应该能够解决 `NoSuchBeanDefinitionException` 错误。







<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/14-%E5%BC%82%E5%B8%B8-NoSuchBeanDefinitionException%20No%20qualifying%20bean%20of%20type%20'cn.hutool.core.lang.Snowflake'%20available%20expected%20at%20least%201%20bean%20which%20qualifies%20as%20autowire%20candidate.png?raw=true)

上图是由 Pic 生成的

关键词：A proud wolf dog stands in the boundless desert waiting for death

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





