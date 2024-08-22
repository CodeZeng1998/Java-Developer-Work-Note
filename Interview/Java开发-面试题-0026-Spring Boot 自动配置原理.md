# Java开发-面试题-0026-Spring Boot 自动配置原理

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



**Spring Boot 自动配置原理**



Spring Boot 的自动配置原理是通过 `@EnableAutoConfiguration` 注解及其背后的机制来实现的，它能够自动配置应用程序的基础设施和常用组件，减少了开发者手动配置的工作量。



### 自动配置的核心原理

1. **`@EnableAutoConfiguration` 注解**：

   - 这是 Spring Boot 自动配置的核心注解。它用于启用自动配置功能，通常与 `@SpringBootApplication` 注解一起使用。`@SpringBootApplication` 实际上是一个组合注解，包含了 `@EnableAutoConfiguration`、`@ComponentScan` 和 `@Configuration`。

2. **`spring.factories` 文件**：

   - Spring Boot 自动配置是通过 `spring-boot-autoconfigure` 包中的 `META-INF/spring.factories` 文件实现的。该文件中定义了许多 `EnableAutoConfiguration` 的自动配置类。这些类会在应用启动时被 Spring Boot 自动加载并初始化。

   - `spring.factories` 文件格式如下：

     ```properties
     org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
     org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
     org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
     ...
     ```

   - 这些类实现了具体的自动配置逻辑。

3. **`@Conditional` 注解族**：

   - 每个自动配置类通常使用 `@Conditional` 系列注解（如 `@ConditionalOnClass`、`@ConditionalOnMissingBean`、`@ConditionalOnProperty` 等）来决定是否加载某个配置类。这些注解会根据类路径中是否存在某个类、Spring 环境中是否存在某个 `Bean`、是否有特定的属性配置等条件，决定是否进行相应的配置。

4. **自动配置流程**：

   - **启动阶段**：当 Spring Boot 应用启动时，`@EnableAutoConfiguration` 注解触发自动配置机制，Spring Boot 扫描 `spring.factories` 文件，并加载其中的所有自动配置类。
   - **条件匹配**：对于每个自动配置类，Spring Boot 逐个检查其上的条件注解。如果所有条件都满足，则会将该自动配置类中的 `@Bean` 方法加载到 Spring 容器中，注册为 `Bean`。
   - **最终配置**：通过这种机制，Spring Boot 自动配置了应用所需的许多常见组件，例如数据源、事务管理器、MVC 配置等。



### 自动配置的示例

例如，`DataSourceAutoConfiguration` 是一个自动配置数据源的类，它的原理如下：

- 在 `spring.factories`文件中有如下定义：

  ```properties
  org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
  ```

- `DataSourceAutoConfiguration` 类使用了 `@ConditionalOnClass(DataSource.class)` 注解，这意味着只有在类路径中存在 `DataSource` 类时，才会激活数据源的自动配置。



### 自定义与排除自动配置

1. **自定义自动配置**：
   - 开发者可以通过在类路径下定义自定义配置类并使用 `@Conditional` 注解来创建自己的自动配置逻辑。
2. **排除自动配置**：
   - 可以使用 `@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})` 来排除某些不需要的自动配置类。



### 源代码

```java
@SpringBootApplication(scanBasePackages = {"com.xxx"})
@ComponentScan(basePackages = {"com.xxx"},excludeFilters =
@ComponentScan.Filter(type = FilterType.REGEX,pattern = {"com.xxx.service.quartz.*"}))
public class XxxApplication {
    public static void main(String[] args) {
        SpringApplication.run(XxxApplication.class, args);
    }
}
```

​	

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
 ...
 ...
 ...
}
```

* @SpringBootConfiguration：该注解与 @Configuration 注解作用相同，用来声明当前也是一个配置类。
* @ComponentScan：组件扫描，默认扫描当前引导类所在包及其子包。
* @EnableAutoConfiguration：Spring Boot实现自动化配置的核心注解。



```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import(AutoConfigurationImportSelector.class)
public @interface EnableAutoConfiguration {

	String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	Class<?>[] exclude() default {};

	/**
	 * Exclude specific auto-configuration class names such that they will never be
	 * applied.
	 * @return the class names to exclude
	 * @since 1.3.0
	 */
	String[] excludeName() default {};

}
```

* @Import(AutoConfigurationImportSelector.class)：==Spring Boot 自动配置是通过 `spring-boot-autoconfigure` 包中的 `META-INF/spring.factories` 文件实现的。==该文件中定义了许多 `EnableAutoConfiguration` 的自动配置类。这些类会在应用启动时被 Spring Boot 自动加载并初始化。
* 每个自动配置类通常使用 `@Conditional` 系列注解（如 `@ConditionalOnClass`、`@ConditionalOnMissingBean`、`@ConditionalOnProperty` 等）来决定是否加载某个配置类。这些注解会根据类路径中是否存在某个类、Spring 环境中是否存在某个 `Bean`、是否有特定的属性配置等条件，决定是否进行相应的配置。





### 总结

**Springboot自动配置原理**

* 在Spring Boot项目中的引导类上有一个注解@SpringBootApplication，这个注解是对三个注解进行了封装，分别是:
  * **@SpringBootConfiguration**
  * **@EnableAutoConfiguration**
  * **@ComponentScan**
* 其中@EnableAutoConfiquration是实现自动化配置的核心注解。该注解通过**@Import**注解导入对应的配置选择器。内部就是读取了该项目和该项目引用的Jar包的的claspath路径下==META-INF/spring.factories文件==中的所配置的类的全类名。 在这些配置类中所定义的Bean会根据条件注解所指定的条件来决定是否需要将其导入到Spring容器中。
* 条件判断会有像**@ConditionalnClass**这样的注解，判断是否有对应的clas文件，如果有则加载该类，把这个配置类的所有的Bean放入spring容器中使用。



Spring Boot 的自动配置机制通过 `@EnableAutoConfiguration`、`spring.factories` 文件、以及 `@Conditional` 注解族，自动装配了应用所需的各种组件，使开发者可以专注于业务逻辑而不必为基础配置费心。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0026.png?raw=true)

上图是由 Pic 生成的

关键词：Cute whale

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



