# 《微服务架构基础（Spring Boot+Spring Cloud+Docker）》-黑马程序员-学习笔记



> PS：本书是记录黑马程序员的《微服务架构基础（Spring Boot+Spring Cloud+Docker）》这本书的学习笔记，内容较多，建议收藏。
>
> 书籍评语：非常非常基础的内容，内容有点过于简单了，几乎都是泛泛而谈。




**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[TOC]



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

文中只有简单的使用（依赖、配置信息、注解），所以省略掉笔记内容。



## 3.2 Srping Boot 与 Redis 的集成

文中只有简单的使用（依赖、配置信息、注解），所以省略掉笔记内容。



## 3.3 Spring Boot 与 ActiveMQ 的集成

文中只有简单的使用（依赖、配置信息、注解），所以省略掉笔记内容。



## 3.4 Spring Boot 应用的打包和部署

JAR包、WAR包



ChatGPT：

JAR包和WAR包的主要区别在于它们的用途和内容。

- JAR文件：这些是Java归档文件，主要用于将Java类、元数据和资源打包到一个文件中进行分发。它们通常包含可执行的Java代码、库和配置文件。JAR文件通常用于独立的Java应用程序、库和实用程序。
- WAR文件：另一方面，WAR文件专门用于打包和部署Java Web应用程序。它们不仅包含Java类和资源，还包含与Web相关的组件，如HTML、JSP（JavaServer Pages）、CSS、JavaScript、XML配置文件和静态Web内容。WAR文件按照Java EE（Enterprise Edition）Web应用程序归档标准进行组织，并且通常部署到诸如Apache Tomcat或Java EE应用程序服务器之类的Servlet容器中。

总之，虽然JAR文件是用于各种目的的通用Java归档文件，但WAR文件专门用于部署Web应用程序，并包含额外的与Web相关的组件。

# 四、Spring Cloud（上）

## 4.1 Spring Cloud 简介

Spring Cloud是在Spring Boot的基础上构建的，用于简化分布式系统构建的工具集。该工具集为微服务架构中所涉及的配置管理、服务发现、智能路由、断路器、微代理和控制总线等操作提供了一种简单的开发方式。



Spring Cloud 中包含多个子项目：https://spring.io/projects/spring-cloud

本书中主要涉及到以下几个子项目的内容，具体介绍如下。

* ·Spring Cloud Netflix：集成了各种OSS组件，其中包括Eureka、Ribbon、Hystrix、Zuul、Feign和Archaius等。
* ·Spring Cloud Config：配置管理工具，支持使用Git存储配置内容，可以使用它实现应用配置的外部化存储，并支持客户端配置信息刷新、加密和解密等配置内容。
* ·Spring Cloud Starters：Spring Cloud的基础组件，是基于Spring Boot风格项目的基础依赖模块。



Spring Cloud 特点：

* 使用方便
* 功能齐全
* 易于扩展和维护
* 适用于各种环境



## 4.2 服务发发现

**Eureka介绍**

Eureka是Netflix开发的一个服务发现框架，本身是一个基于REST的服务，主要用于定位运行在AWS（Amazon Web Services ）域中的中间层服务，以达到负载均衡和中间层服务故障转移的目的。Spring Cloud将其集成在自己的子项目Spring Cloud Netflix中，以实现Spring Cloud的服务发现功能。



Eureka 的服务发现包含两大组件：服务端发现组件（Eureka Server）和客户端发现组件（Eureka Client）。服务端发现组件也被称之为服务注册中心，主要提供了服务的注册功能，而客户端发现组件主要用于处理服务的注册与发现。、



当客户端服务通过注解等方式嵌入到程序的代码中运行时，客户端发现组件就会向注册中心注册自身提供的服务，并周期性地发送心跳来更新服务（默认时间为30s，如果连续三次心跳都不能够发现服务，那么Eureka就会将这个服务节点从服务注册表中移除）。与此同时，客户端发现组件还会从服务端查询当前注册的服务信息并缓存到本地，即使 Eureka Server出现了问题，客户端组件也可以通过缓存中的信息调用服务节点的服务。各个服务之间会通过注册中心的注册信息以Rest方式来实现调用，并且可以直接通过服务名进行调用。



Eureka 的服务发现机制包含了 3 个角色：服务注册中心、服务提供者和服务消费者。

* 服务注册中心即Eureka Server，而服务提供者和服务消费者是Eureka Client。这里的服务提供者是指提供服务的应用，可以是Spring Boot应用，也可以是其他技术平台且遵循Eureka通信机制的应用，应用在运行时会自动地将自己提供的服务注册到Eureka Server以供其他应用发现。
* 服务消费者就是需要服务的应用，该服务在运行时会从服务注册中心获取服务列表，然后通过服务列表知道去何处调用其他服务。服务消费者会与服务注册中心保持心跳连接，一旦服务提供者的地址发生变更时，注册中心会通知服务消费者。
* 需要注意的是，Eureka 服务提供者和服务消费者之间的角色是可以相互转换的，因为一个服务既可能是服务消费者，同时也可能是服务提供者。





## 4.3 客户端负载均衡

在分布式架构中，服务器端负载均衡通常是由Nginx实现分发请求的，而客户端的同一个实例部署在多个应用上时，也需要实现负载均衡。那么Spring Cloud中是否提供了这种负载均衡的功能呢？答案是肯定的。我们可以通过Spring Cloud中的Ribbon来实现此功能。



**Ribbon介绍**

Ribbon是Netflix发布的开源项目，其主要功能是提供客户端的软件负载均衡算法，将Netflix的中间层服务连接在一起。Ribbon 的客户端组件提供了一系列完善的配置项，例如连接超时、重试等。在Eureka的自动配置依赖模块spring-cloud-starter-eureka中，已经集成了Ribbon（其集成后的依赖层级关系如图4-9所示），我们可以直接使用Ribbon来实现客户端的负载均衡。二者同时使用时，Ribbon会利用从Eureka读取到的服务信息列表，在调用服务实例时，以合理的方式进行负载。



其实Ribbon在工作时主要分为两步：第1步先选择 Eureka Server，它会优先选择在同一个区域且负载较少的Server；第2步会根据用户指定的策略（如轮询、随机等）从Server取到的服务注册列表中选择一个地址。



# 五、Spring Cloud（下）

## 5.1 服务容错保护

**Spring Cloud Hystrix**介绍

在微服务架构中，通常会存在多个服务层调用的情况，如果基础服务出现故障可能会发生级联传递，导致整个服务链上的服务不可用



为了解决服务级联失败这种问题，在分布式架构中产生了断路器等一系列的服务保护机制。分布式架构中的断路器，有些类似于我们生活中的空气开关，当电路发生短路等情况时，空气开关会立刻断开电流，以防止用电火灾的发生。

在Spring Cloud中，Spring Cloud Hystrix就是用来实现断路器、线程隔离等服务保护功能的。Spring Cloud Hystrix是基于Netflix的开源框架Hystrix实现的，该框架的使用目标在于通过控制那些访问远程系统、服务和第三方库的节点，从而对延迟和故障提供更强大的容错能力。



与空气开关不能自动重新打开有所不同的是，断路器是可以实现弹性容错的，在一定条件下它能够自动打开和关闭，其使用时主要有三种状态。

断路器的开关由关闭到打开的状态是通过当前服务健康状况（服务的健康状况=请求失败数/请求总数）和设定阈值（默认为10秒内的20次故障）比较决定的。当断路器开关关闭时，请求被允许通过断路器，如果当前健康状况高于设定阈值，开关继续保持关闭；如果当前健康状况低于设定阈值，开关则切换为打开状态。当断路器开关打开时，请求被禁止通过；如果设置了fallback方法，则会进入fallback的流程。当断路器开关处于打开状态，经过一段时间后，断路器会自动进入半开状态，这时断路器只允许一个请求通过；当该请求调用成功时，断路器恢复到关闭状态；若该请求失败，断路器继续保持打开状态，接下来的请求会被禁止通过。

Spring Cloud Hystrix能保证服务调用者在调用异常服务时快速地返回结果，避免大量的同步等待，这是通过HystrixCommand的fallback方法实现的。



**Hystrix Dashboard**的使用

Hystrix除了可以对不可用的服务进行断路隔离外，还能够对服务进行实时监控。Hystrix可以实时、累加地记录所有关于HystrixCommand 的执行信息，包括每秒执行多少、请求成功多少、失败多少等。



## 5.2 API网关服务

API Gateway是一个服务器，也可以说是进入系统的唯一节点，它封装了内部系统的架构，并且提供了API给各个客户端。它还可以有其他功能，如授权、监控、负载均衡、缓存、请求分片和管理、静态响应处理等。

API Gateway负责请求转发、合成和协议转换。所有来自客户端的请求都要先经过API Gateway，然后负载均衡这些请求到对应的微服务。API Gateway的一个最大好处是封装了应用的内部结构，与调用指定的服务相比，客户端直接跟Gateway交互会更简单。API Gateway提供给每一个客户端一个特定API，这样减少了客户端与服务器端的通信次数，也简化了客户端代码。API Gateway还可以在Web协议与内部使用的非Web协议间进行转换，如HTTP协议、WebSocket协议。API Gateway可以有很多实现方法，如Nginx、Zuul、Node.js等。



**Zuul** 

Zuul 原是 Netflix 公司开发的基于 JVM 的路由器和服务器端负载均衡器，后来被加入到了Spring Cloud中。Zuul属于边缘服务，可以用来执行认证、动态路由、服务迁移、负载均衡、安全和动态响应处理等操作。



虽然单独使用 Zuul 也可以实现网关的路由功能，但在实际应用中并不推荐使用。因为此种方式需要运维人员花费大量时间来维护各个路由的path与url的关系，而与Eureka整合的方式中，路由的path 不再是映射到具体的 url，而是映射到了具体的服务上，具体的 url 会交由Eureka来维护管理，显然使用与Eureka整合的方式更加得方便和实用。这里Zuul的单独使用读者作为了解即可。



## 5.3 分布式配置管理

**Spring Cloud Config**简介



为了便于集中配置的统一管理，在分布式架构中通常会使用分布式配置中心组件，目前比较流行的分布式配置中心组件有百度的disconf、阿里的diamond、携程的apollo和Spring Cloud的Config等。相对于同类产品而言，Spring Cloud Config最大的优势就是和Spring的无缝集成，对于已有的Spring应用程序的迁移成本非常低，结合Spring Boot可使项目有更加统一的标准（包括依赖版本和约束规范），避免了因集成不同开发软件造成的版本依赖冲突等问题。本书中的所讲解的分布式配置中心组件就是Spring Cloud Config。

Spring Cloud Config是Spring Cloud团队创建的一个全新的项目，该项目主要用来为分布式系统中的外部配置提供服务器（Config Server）和客户端（Config Client）支持。

·服务器端（Config Server）：也被称之为分布式配置中心，它是一个独立的微服务应用，主要用于集中管理应用程序各个环境下的配置，默认使用Git存储配置文件内容，也可以使用SVN存储，或者是本地文件存储。

·客户端（Config Client）：是Config Server的客户端，即微服务架构中的各个微服务应用。它们通过指定的配置中心（Config Server）来管理应用资源以及与业务相关的配置内容，并在启动时从配置中心获取和加载配置信息。



Spring Cloud Config的工作流程：用户会先将配置文件推送到Git或SVN中，然后在微服务应用（Config Client）启动时，会从配置中心（Config Server）中获取配置信息，而配置中心会根据配置从Git或SVN中获取相应的配置信息。



多学一招　：**手动更新运行中的配置文件**

在实际项目应用中，我们可能需要对配置文件的内容做一些修改，而要想使修改的配置文件生效，通常做法是将应用重启。此种方式对于小型应用，以及使用人数不多的应用来说比较适用，但是对于大型企业和互联网应用来说，重启应用是行不通的。这也就要求运维人员在修改完应用的配置后，要保证配置及时生效。Spring Cloud Config正好提供了这种功能，我们可以在客户端用POST请求refresh方法来刷新配置内容。

步骤如下：

* 在客户端的 pom.xml 中添加依赖 spring-boot-starter-actuator。该依赖可以监控程序在运行时的状态，其中包括/refresh的功能。
* 在启动类上添加@RefreshScope注解，开启refresh机制。添加此注解后，在执行/refresh时会更新该注解标注类下的所有变量值，包括Config Client从Git仓库中所获取的配置。
* 在配置文件中将安全认证信息的enabled属性设置为false
* 调用接口



# 六、初识 DOcker

## 6.1 Docker 概述

Docker 特点：

* 更快速的交付和部署
* 更高效的虚拟化
* 更轻松的迁移和扩展
* 更简单的管理



**Docker与虚拟机对比**

* 虚拟机是运行在每个应用层级的客户端操作系统上的，这是资源密集型的。由于产生的磁盘镜像和应用程序的操作系统设置相互交叉，所以导致虚拟机对系统的依赖性很强，一旦系统出现问题，虚拟机依赖的文件以及安全补丁等都可能会出现文件丢失的情况。
* Docker 中的容器是基于进程的隔离，多个容器可以共享单个内核，并且创建 Docker 容器的镜像所需要的配置并不依赖于宿主机系统。正是因为容器之间配置的隔离性，容器之间就没有配置交叉，所以Docker的应用可以运行在任何地方。



## 6.2 Docker 的安装要求



## 6.3 Docker 的安装方式

在Linux系统上安装Docker有3种方式，分别为在线安装、离线安装以及脚本文件安装，其中最常用也是官方推荐的安装方式就是在线安装。

#### 6.3.1**在线安装**

先设置一个 Docker 仓库，然后通过该仓库进行安装和后续更新。



**1.设置Docker仓库**

```shell
# 更新apt的索引包
sudo apt-get update

# 安装软件包允许apt通过HTTPS方式使用Docker仓库。
sudo apt-get install \　apt-transport-https \　ca-certificates \　curl \　software-properties-common

# 添加Docker官网的GPG key。
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add

# 添加Docker稳定的仓库源（根据Ubuntu镜像版本的不同进行选择安装）
# amd64：
sudo add-apt-repository \　"deb [arch=amd64]https://download.docker.com/linux/ubuntu \　$(lsb_release -cs) \　stable"
# armhf：
sudo add-apt-repository \　"deb [arch=armhf]https://download.docker.com/linux/ubuntu \　$(lsb_release -cs) \　stable"
# s390x:
sudo add-apt-repository \　"deb [arch=s390x]https://download.docker.com/linux/ubuntu \　$(lsb_release -cs) \　stable"
```

**2.安装Docker CE**

```shell
# 更新apt的索引包。
sudo apt-get update
# 安装不同版本的 Docker。
# 安装最新版本的Docker
sudo apt-get install docker-ce
# 安装指定版本的Docker
sudo apt-get install docker-ce=<VERSION>
# 安装完成后，可以使用sudo docker run hello-world指令运行测试
sudo docker run hello-world
```



#### 6.3.2**离线安装**

* 1.下载离线安装文件：https://download.docker.com/linux/ubuntu/dists/

* 2.使用离线文件安装Docker

  ```shell
  sudo dpkg -i /path/to/package.deb
  ```



#### **6.3.3脚本文件安装**

在开发和测试环境下，我们还可以使用Docker官方提供的自动化脚本文件来安装Docker，其中开发环境和测试环境下的脚本文件下载地址分别为 https://get.docker.com/和https://test.docker.com/

安装步骤：https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/#install-using-the-convenience-script



多学一招　：**Docker的开机启动和添加当前用户可执行权限**

在Docker安装完成后，开发者可以根据实际需求进行其他一些相关设置，例如Docker开机启动、当前用户可执行Docker等，具体设置方式如下。

（1）配置Docker开机启动$ sudo systemctl enable docker

（2）配置当前用户执行Docker权限（username是自己的用户名）$ sudo usermod -aG docker username完成上述配置后，需要重启Ubuntu系统来查看效果。



**Docker更新资源失败**：问题的原因可能是由于另外一个程序（上次运行安装或更新没有正常完成）正在使用该程序，从而导致资源被锁不可用。

```shell
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```



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









# 九、微服务项目的整合与测试

## 9.1 微服务项目整合



## 9.2 接口可视化工具-Swagger-UI





# 十、微服务的部署

## 10.1 Docker Compose 编排工具





## 10.2 微服务与 Docker 的整合





## 10.3 环境搭建以及镜像准备





## 10.4微服务的手动部署





## 10.5使用 Jenkins 自动部署微服务







**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
