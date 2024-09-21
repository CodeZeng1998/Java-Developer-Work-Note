# Java开发-面试题-0038-Spring Boot的starter用过吗，怎么用，原理是什么

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



> PS：这道面试题其实和直接问你Spring Boot的自动装配原理是一致的，只不过多了starter怎么使用。所以回答的时候就可以直接简单说明下使用方式，然后重点讲解Spring Boot的自动装配原理，详细的装配原理文章以及有了，需要的可以直接看一下这篇文章对应的专栏下的文章可以找到详细的Spring Boot自动装配原理的文章，这里只进行简单描述。



<br/>

Spring Boot 的 `starter` 是一组预配置的依赖，用来简化项目中各种常见框架和库的集成。`starter` 提供了一种**自动化配置**的方式，使开发者不必手动配置繁杂的框架参数，Spring Boot 自动完成大部分配置工作。

<br/>

### 1. **Spring Boot Starter 的使用**

使用 Spring Boot `starter` 非常简单，通常只需要在项目的 `pom.xml` 或 `build.gradle` 文件中添加相应的依赖即可。例如：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

这个 `spring-boot-starter-web` starter 包含了用于构建 Web 应用所需的常见依赖，包括 Spring MVC、Tomcat（默认嵌入式容器）等。

<br/>

#### 常见的 Spring Boot Starters：

- **`spring-boot-starter-web`**：用于构建基于 Spring MVC 的 Web 应用。
- **`spring-boot-starter-data-jpa`**：用于简化 JPA 的配置和使用。
- **`spring-boot-starter-security`**：用于集成 Spring Security。
- **`spring-boot-starter-test`**：用于集成测试框架，如 JUnit、Mockito。

<br/>

### 2. **Spring Boot Starter 的原理**

Spring Boot Starter 的核心原理是基于==**自动配置**（Auto-Configuration）和**条件装配**（Conditional Bean Configuration）==。

<br/>

#### **自动配置（Auto-Configuration）**

Spring Boot 自动配置通过扫描项目中的类路径来确定哪些库和配置应该被启用。例如，当你引入了 `spring-boot-starter-web`，Spring Boot 会自动识别出项目需要使用 Spring MVC，并自动配置好相关的组件，如 `DispatcherServlet`、视图解析器（View Resolver）、嵌入式 Tomcat 服务器等。

这种==自动配置是通过 `spring.factories` 文件以及一系列带有 `@Configuration` 和 `@ConditionalOn...` 注解的类实现的。==这些注解会根据条件（比如类路径中是否存在特定的库）来决定是否启用某些配置。

<br/>

#### **条件装配（Conditional Bean Configuration）**

Spring Boot 通过一组条件注解来控制自动配置逻辑，例如：

- **`@ConditionalOnClass`**：如果类路径中存在指定的类，则启用配置。
- **`@ConditionalOnMissingBean`**：如果 Spring 容器中不存在指定类型的 bean，则启用配置。
- **`@ConditionalOnProperty`**：根据配置文件中的属性值决定是否启用某个配置。

例如，`spring-boot-starter-web` 会根据类路径中是否存在 `javax.servlet.Servlet` 来决定是否启用 Web 相关的配置。

<br/>

#### **`spring.factories` 文件**

这是 Spring Boot 自动配置的核心，定义了哪些配置类需要被加载。这个文件通常位于 `META-INF/spring.factories` 中，内容类似如下：

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

这个文件列出了自动配置类的全限定名，当项目启动时，这些类会被加载，并根据条件装配逻辑自动配置需要的组件。

<br/>

### 3. **自定义 Starter**

如果你需要开发一个自定义的 Spring Boot Starter，可以按照以下步骤：

1. **创建 Starter 项目**：新建一个 Maven/Gradle 项目，添加需要的依赖。
2. **编写自动配置类**：创建带有 `@Configuration` 和 `@ConditionalOn...` 注解的类，这些类会根据项目需求自动配置所需的 Bean。
3. **注册 `spring.factories`**：在 `resources/META-INF/` 目录下创建 `spring.factories` 文件，定义自动配置类。

<br/>

### 4. **示例：创建自定义 Starter**

#### 步骤 1：创建 Maven 项目

```xml
<dependency>
    <groupId>com.example</groupId>
    <artifactId>my-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```

<br/>

#### 步骤 2：编写自动配置类

```java
@Configuration
@ConditionalOnClass(MyService.class)
public class MyServiceAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public MyService myService() {
        return new MyService();
    }
}
```

<br/>

#### 步骤 3：注册 `spring.factories`

在 `META-INF/spring.factories` 中配置：

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.example.MyServiceAutoConfiguration
```

这样，在引入 `my-starter` 后，Spring Boot 就会自动配置 `MyService`。

<br/>

### 5. **Spring Boot Starter 的优势**

- **简化配置**：通过引入 starter，开发者不再需要手动配置复杂的框架依赖和 Bean，只需专注于业务逻辑。
- **条件化配置**：自动根据项目需要加载所需的配置，提高了灵活性和可扩展性。
- **易于集成**：通过自定义 starter，可以将企业内部的通用配置打包成 starter，简化多项目的集成开发。

<br/>

### 总结

Spring Boot 的 `starter` 是一种极大简化开发的机制，通过自动配置和条件装配技术，实现了框架的无缝集成。开发者只需引入对应的 starter，即可获得相关功能的自动配置，从而专注于核心业务开发。











<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0038.png?raw=true)

上图是由 Pic 生成的

关键词：Suicide Squad

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



