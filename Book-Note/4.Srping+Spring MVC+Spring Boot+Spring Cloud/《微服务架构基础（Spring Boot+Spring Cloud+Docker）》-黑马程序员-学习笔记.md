# 《微服务架构基础（Spring Boot+Spring Cloud+Docker）》-黑马程序员-学习笔记



> PS：本书是记录黑马程序员的《微服务架构基础（Spring Boot+Spring Cloud+Docker）》这本书的学习笔记，内容较多，建议收藏。



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[TOC]





# 一、认识微服务架构

## 1.1 为什么需要微服务架构

随着互联网技术的发展，传统的应用架构已满足不了实际需求，微服务架构就随之产生。

传统单体应用架构的问题：

* 应用复杂度增加，更新、维护困难
* 易造成系统资源浪费
* 影响开发效率
* 应用可靠性低
* 不利于技术的更新



## 1.2 微服务架构是什么

在传统软件应用架构的基础上，将系统业务按照功能拆分为更加细粒度的服务，所拆分的每一个服务都是一个独立的应用，这些应用对外提供公共的API，可以独立承担对外服务的职责，通过此种思想方式所开发的软件服务实体就是“微服务”，而围绕着微服务思想构建的一系列体系结构（包括开发、测试、部署等），我们可以将它称之为“微服务架构”。



微服务架构的优点：

* 复杂度可控
* 可独立部署
* 技术选型灵活
* 易于容错
* 易于扩展
* 功能特定



微服务架构的不足：

* 开发人员必须处理创建分布式系统的复杂性
* 部署的复杂性
* 增加内存消耗



## 1.3 如何构建微服务架构

拆分微服务建议：

* 通过业务功能分解并定义与业务功能相对应的服务。
* 将域驱动设计分解为多个子域。
* 按照动词或用例分解，并定义负责特定操作的服务，例如一个负责完成订单的航运服务。
* 通过定义一个对给定类型的实体或资源的所有操作负责的服务来分解名词或资源，例如一个负责管理用户账户的账户服务。



微服务架构的组件：

* ·服务注册中心：注册系统中所有服务的地方。
* 服务注册：服务提供方将自己调用地址注册到服务注册中心，让服务调用方能够方便地找到自己。
* 服务发现：服务调用方从服务注册中心找到自己需要调用服务的地址。
* 负载均衡：服务提供方一般以多实例的形式提供服务，使用负载均衡能够让服务调用方连接到合适的服务节点。
* 服务容错：通过断路器（也称熔断器）等一系列的服务保护机制，保证服务调用者在调用异常服务时快速地返回结果，避免大量的同步等待。
* 服务网关：也称为API网关，是服务调用的唯一入口，可以在这个组件中实现用户鉴权、动态路由、灰度发布、负载限流等功能。
* 分布式配置中心：将本地化的配置信息（properties、yml、yaml 等）注册到配置中心，实现程序包在开发、测试、生产环境的无差别性，方便程序包的迁移。



技术选型：

* 微服务实例的开发：Spring Boot
* 服务的注册与发现：Spring Cloud Eureka、Apache Zookeeper、Consul、Etcd和Dubbo
* 负载均衡：Spring Cloud Ribbon和Dubbo
* 服务容错：Hystrix、Spring Cloud Hystrix
* API网关：Spring Cloud Zuul、Spring Reactor、Netty或NodeJS
* 分布式配置中心：Spring Cloud Config
* 调试：Swagger
* 部署：Docker
* 持续集成：为了实现服务的自动化部署，我们可以通过 Jenkins 搭建自动化部署系统，并使用 Docker进行容器化封装。







# 二、初识Spring Boot



## 2.1 Spring Boot 介绍

Spring Boot框架优点：

* 可快速构建独立的Spring应用程序
* 内嵌Servlet容器，无需单独安装容器即可独立运行项目
* 对主流开发框架的无配置集成
* 提供开箱即用的Spring插件，简化了Maven、Gradle的配置（“Starter”）
* 自动配置Spring，极大地提高了开发、部署效率
* 无需任何XML配置



Spring Boot是Spring框架对“约定优先于配置”理念的最佳实践产物。



## 2.2 Spring Boot 入门

Spring 项目创建：https://start.spring.io/

## 2.3 Spring Boot的工作机制

@SpringBootApplication是Spring Boot的核心注解

```java

/**
 * Indicates a {@link Configuration configuration} class that declares one or more
 * {@link Bean @Bean} methods and also triggers {@link EnableAutoConfiguration
 * auto-configuration}, {@link ComponentScan component scanning}, and
 * {@link ConfigurationPropertiesScan configuration properties scanning}. This is a
 * convenience annotation that is equivalent to declaring {@code @Configuration},
 * {@code @EnableAutoConfiguration}, {@code @ComponentScan}.
 *
 * @author Phillip Webb
 * @author Stephane Nicoll
 * @author Andy Wilkinson
 * @since 1.2.0
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {

	/**
	 * Exclude specific auto-configuration classes such that they will never be applied.
	 * @return the classes to exclude
	 */
	@AliasFor(annotation = EnableAutoConfiguration.class)
	Class<?>[] exclude() default {};

	/**
	 * Exclude specific auto-configuration class names such that they will never be
	 * applied.
	 * @return the class names to exclude
	 * @since 1.3.0
	 */
	@AliasFor(annotation = EnableAutoConfiguration.class)
	String[] excludeName() default {};

	/**
	 * Base packages to scan for annotated components. Use {@link #scanBasePackageClasses}
	 * for a type-safe alternative to String-based package names.
	 * <p>
	 * <strong>Note:</strong> this setting is an alias for
	 * {@link ComponentScan @ComponentScan} only. It has no effect on {@code @Entity}
	 * scanning or Spring Data {@link Repository} scanning. For those you should add
	 * {@link org.springframework.boot.autoconfigure.domain.EntityScan @EntityScan} and
	 * {@code @Enable...Repositories} annotations.
	 * @return base packages to scan
	 * @since 1.3.0
	 */
	@AliasFor(annotation = ComponentScan.class, attribute = "basePackages")
	String[] scanBasePackages() default {};

	/**
	 * Type-safe alternative to {@link #scanBasePackages} for specifying the packages to
	 * scan for annotated components. The package of each class specified will be scanned.
	 * <p>
	 * Consider creating a special no-op marker class or interface in each package that
	 * serves no purpose other than being referenced by this attribute.
	 * <p>
	 * <strong>Note:</strong> this setting is an alias for
	 * {@link ComponentScan @ComponentScan} only. It has no effect on {@code @Entity}
	 * scanning or Spring Data {@link Repository} scanning. For those you should add
	 * {@link org.springframework.boot.autoconfigure.domain.EntityScan @EntityScan} and
	 * {@code @Enable...Repositories} annotations.
	 * @return base packages to scan
	 * @since 1.3.0
	 */
	@AliasFor(annotation = ComponentScan.class, attribute = "basePackageClasses")
	Class<?>[] scanBasePackageClasses() default {};

	/**
	 * Specify whether {@link Bean @Bean} methods should get proxied in order to enforce
	 * bean lifecycle behavior, e.g. to return shared singleton bean instances even in
	 * case of direct {@code @Bean} method calls in user code. This feature requires
	 * method interception, implemented through a runtime-generated CGLIB subclass which
	 * comes with limitations such as the configuration class and its methods not being
	 * allowed to declare {@code final}.
	 * <p>
	 * The default is {@code true}, allowing for 'inter-bean references' within the
	 * configuration class as well as for external calls to this configuration's
	 * {@code @Bean} methods, e.g. from another configuration class. If this is not needed
	 * since each of this particular configuration's {@code @Bean} methods is
	 * self-contained and designed as a plain factory method for container use, switch
	 * this flag to {@code false} in order to avoid CGLIB subclass processing.
	 * <p>
	 * Turning off bean method interception effectively processes {@code @Bean} methods
	 * individually like when declared on non-{@code @Configuration} classes, a.k.a.
	 * "@Bean Lite Mode" (see {@link Bean @Bean's javadoc}). It is therefore behaviorally
	 * equivalent to removing the {@code @Configuration} stereotype.
	 * @since 2.2
	 * @return whether to proxy {@code @Bean} methods
	 */
	@AliasFor(annotation = Configuration.class)
	boolean proxyBeanMethods() default true;

}
```

* ·@SpringBootConfiguration：该注解与@Configuration的作用相同，它表示其标注的类是IoC容器的配置类。
* ·@EnableAutoConfiguration：用于将所有符合自动配置的Bean加载到当前Spring Boot创建并使用的IoC容器中。
* ·@ComponentScan：用于自动扫描和加载符合条件的组件或Bean，并将Bean加载到IoC容器中。



在Spring Boot项目的main()方法中，SpringApplication.run(HelloApplication.class, args)是唯一执行的方法体，该方法体的执行过程可分
为两部分来看，具体如下。

1. 创建SpringApplication对象在SpringApplication实例初始化时，它会做如下几项工作。
   1. 根据classpath内是否存在某个特征类来判断是否为Web应用，并使用webEnvironment标记是否为Web应用。
   2. 使用SpringFactoriesLoader在classpath中的spring.factories文件查找并加载所有可用的ApplicationContextInitializer。
   3. 使用SpringFactoriesLoader在classpath中的spring.factories文件查找并加载所有可用的ApplicationListener。
   4. 推断并设置main()方法的定义类。
2. 调用实例的run()方法run()方法是Spring Boot执行流程的主要方法，该方法执行时，主要做了如下工作。
   1. 查找并加载 spring.factories 中配置的SpringApplicationRunListener，并调用它们的started()方法。告诉SpringApplicationRunListener，Spring Boot应用要执行了。
   2. 创建并配置当前 Spring Boot 应用要使用的 Environment，然后遍历调用所有SpringApplicationRunListener的environmentPrepared()方法。告诉SpringBoot应用使用的环境准备好了。
   3. 如果SpringApplication的showBanner属性被设置为true，则打印banner。
   4. 根据用户是否明确设置了 applicationContextClass 类型，以及初始化阶段（创建SpringApplication对象的第（1）步）的推断结果来决定当前Spring Boot应用创建什么类型的ApplicationContext。
   5. 创建故障分析器。故障分析器用于提供错误和诊断信息。
   6. 对ApplicationContext进行后置处理。对所有可用的ApplicationContextInitializer遍历执行initialize()方法；遍历调用所有SpringApplicationRunListener的environmentPrepared()方法；将之前通过@EnableAutoConfiguration 获取的所有配置以及其他形式的 IoC 容器配置加载到已经准备完毕的ApplicationContext；遍历调用所有SpringApplicationRunListeners 的contextLoaded()方法。
   7. 调用refreshContext()方法执行applicationContext的refresh()方法。
   8. 查找当前ApplicationContext中是否有ApplicationRunner和CommandLineRunner，如果有，则遍历执行它们。
   9. 正常情况下，会遍历执行所有SpringApplicationRunListener 的 finished()方法；如果出现异常，也会调用该方法，只不过这种情况下会将异常信息一并传入处理。经过上述步骤后，一个完整的Spring Boot项目就已经启动完成。虽然整个过程看起来比较复杂，但大部分内容都是事件通知的扩展点。如果将这些扩展点忽略，那么Spring Boot的整个启动流程将如下所示。



Spring Boot 的整个启动流程如下：

* 开始
* ——--> 收集各种条件和回调接口，例如：ApplicationContextIntializer、ApplicationListener
* ——通告 started() -->  创建并准备 Environment
* ——通告 environmentPrepared() --> 创建并初始化 ApplicationContext，例如：设置 Environment，加载配置等
* ——通告 contextPrepared()、通告 contextLoaded() --> 调用 ApplicationRunner 的 refresh() 方法，完成最终程序启动
* ——执行 ApplicationRunner 和 CommandLineRunner 通告 finished() -->
* 结束



# 三、Spring Boot 应用开发



## 3.1 Spring Boot 与 Mybatis 的集成



## 3.2 Srping Boot 与 Redis 的集成



## 3.3 Spring Boot 与 ActiveMQ 的集成



## 3.4 Spring Boot 应用的打包和部署





# 四、Spring Cloud（上）

## 4.1 Spring Cloud 简介

## 4.2 服务发发现

## 4.3 客户端负载均衡





# 五、Spring Cloud（下）

## 5.1 服务容错保护

## 5.2 API网关服务

## 5.3 分布式配置管理





# 六、 初识 DOcker

## 6.1 Docker 概述

## 6.2 Docker 的安装要求

## 6.3 Docker 的安装方式

## 6.4 Docker  的运行机制

## 6.5 Docker 的底层技术





# 七、Docker 的使用

## 7.1 Docker 入门程序

## 7.2 Dockerfile 介绍

## 7.3 Docker客户端常用命令

## 7.4 Docker 镜像管理



# 八、Docker 中的网络与数据管理

## 8.1 Docker 网络管理

## 8.2 Docker Swarm 集群

## 8.3 Docker 数据管理

## 8.4 Volumes 数据卷管理





# 、

## .1

## .2

## .3





# 、

## .1

## .2

## .3







**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
