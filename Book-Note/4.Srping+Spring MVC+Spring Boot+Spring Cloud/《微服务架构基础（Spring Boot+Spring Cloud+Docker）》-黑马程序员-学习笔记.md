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

 Docker的引擎Docker Engine（Docker引擎）是Docker的核心部分，使用的是客户端-服务器（C/S）架构模式。

Docker Engine 中包含了三个核心组件（docker CLI、REST API 和docker daemon），这三个组件的具体说明如下。

* ·docker CLI（command line interface）：表示Docker命令行接口，开发者可以在命令行中使用Docker相关指令与Docker守护进程进行交互，从而管理诸如image（镜像）、container （容器）、network（网络）和data volumes（数据卷）等实体。

* ·REST API：表示应用程序API接口，开发者通过该API接口可以与Docker的守护进程进行交互，从而指示后台进行相关操作。

* ·docker daemon：表示Docker的服务端组件，它是Docker架构中运行在后台的一个守护进程，可以接收并处理来自命令行接口及API接口的指令，然后进行相应的后台操作。

对于开发者而言，既可以使用编写好的脚本文件通过REST API来实现与Docker进程交互，也可以直接使用Docker相关指令，通过命令行接口来与Docker进程交互，而其他一些Docker应用则是通过底层的API和CLI进行交互的。



Docker架构主要包括Client、DOCKER_HOST和Register三部分，关于这三部分的具体说明如下。

* **Client（客户端）**：Client即Docker客户端，也就是上一小节Docker Engine中介绍的docker CLI。开发者通过这个客户端使用Docker的相关指令与Docker守护进程进行交互，从而进行Docker镜像的创建、拉取和运行等操作。
* .**DOCKER_HOST（Docker主机）**：DOCKER_HOST即Docker内部引擎运行的主机，主要指Docker daemon（Docker守护进程）。可以通过Docker守护进程与客户端还有Docker的镜像仓库Registry进行交互，从而管理Images（镜像）和Containers（容器）等。
* Registry（注册中心）：Registry即Docker注册中心，实质就是Docker镜像仓库，默认使用的是Docker官方远程注册中心Docker Hub，也可以使用开发者搭建的本地仓库。Registry中包含了大量的镜像，这些镜像可以是官网基础镜像，也可以是其他开发者上传的镜像。



我们在实际使用 Docker 时，除了会涉及图中的 3 个主要部分外，还会涉及很多 Docker Objects（Docker对象），例如Images（镜像）、Containers（容器）、Networks（网络）、Volumes （数据卷）、Plugins（插件）等。其中常用的两个对象Image和Containers的说明如下。

* ·Images（镜像）Docker镜像就是一个只读的模板，包含了一些创建Docker容器的操作指令。通常情况下，一个Docker镜像是基于另一个基础镜像创建的，并且新创建的镜像会额外包含一些功能配置。例如：开发者可以依赖于一个 Ubuntu 的基础镜像创建一个新镜像，并可以在新镜像中安装Apache等软件或其他应用程序。
* ·Containers（容器）Docker 容器属于镜像的一个可运行实例（镜像与容器的关系其实与 Java 中的类与对象相似），开发者可以通过API接口或者CLI命令行接口来创建、运行、停止、移动、删除一个容器，也可以将一个容器连接到一个或多个网络中，将数据存储与容器进行关联。



## 6.5 Docker 的底层技术

Docker使用了一系列的底层技术来充分发挥其技术特色，这些底层技术包括有Namespaces、Control groups、Union file systems和Container format等，其具体含义如下。

* Namespaces（名称空间）：Docker使用名称空间来为容器提供隔离的工作空间。当一个容器运行时，Docker就会为该容器创建一系列的名称空间，并为名称空间提供一层隔离。每一个容器都运行在相对隔离的环境下，对其他名称空间是相对受限的。
* Control groups（控制组）：基于Linux系统的Docker引擎也依赖于另一项叫做Control groups（cgroups，控制组）的技术。控制组可以对程序进行资源限定，并允许Docker引擎在容器间进行硬件资源共享以及随时进行限制和约束，例如，开发者可以限制某特定容器的可用内存。
* Union file systems（联合文件系统）：联合文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持将文件系统的修改作为一次提交来一层层地叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。不同Docker 容器可以共享一些基础的文件系统层，与自己独有的改动层一起使用，可以大大地提高存储效率。Docker目前支持的联合文件系统包括AUFS、btrfs、 vfs 和 DeviceMapper。
* Container format（容器格式）：Docker 引擎将名称空间、控制组和联合文件系统组合成一个叫做容器格式的整体。当前默认的容器格式是libcontainer，未来Docker可能会通过与其他技术（如BSD Jails或者Solaris Zones）的集成使用来开发其他的容器格式。



# 七、Docker 的使用

## 7.1 Docker 入门程序

```shell
# 查看镜像
docker images

# 创建并启动容器
# -d参数表示在后台运行容器，容器创建成功后会自动返回一个64位的容器ID；-p参数将容器暴露的80端口映射到宿主机的5000端口。
docker run -d -p 5000:80 hellodocker

# 查看运行容器
docker ps

# 停止容器 “653347ecc6df”代表的是生成的容器ID docker ps 可以查看对应容器的id
docker stop 653347ecc6df
```



多学一招　：**配置Docker加速器**

使用 Docker 的时候，需要经常从官网获取镜像，但是由于网络或网速等原因，拉取官网镜像的过程可能会非常缓慢，从而严重影响Docker 的使用体验，因此我们可以通过国内一些网站提供的加速器工具来解决这个难题。这些加速器通过智能路由和缓存机制，极大地提升了国内网络访问DockerHub的速度。Linux系统中，配置Docker加速器的具体指令如下。

```shell
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh |sh -s http://27e6d45b.m.daocloud.io
```

该指令可以将Registry远程注册中心的--registry-mirror配置信息加入到Docker本地配置文件/etc/docker/daemon.json 中。

当配置好Docker加速器后，需要根据提示信息重启Docker服务才可生效。



## 7.2 Dockerfile 介绍

Docker 创建镜像时使用了一个 Dockerfile文件，Docker 就是通过读取 Dockerfile文件中一行行的指令构建Docker 镜像的.

 **Dockerfile基本结构**

Dockerfile是一个普通的文本文件，里面包含了许多可以在命令行接口上执行的用来构建镜像的相关指令，我们通过docker build指令就可以读取Dockerfile文件中的指令并执行自动化镜像构建。

一般情况下，Dockerfile文件可分为四个部分：基础镜像信息、维护者信息、镜像操作指令和容器启动时的执行指令。

```
1　#定义基础镜像信息
2　FROM ubuntu
3　# 定义该镜像的维护者信息
4　MAINTAINER docker_user docker_user@email.com
5　# 一些镜像操作指令
6　RUN echo "deb http://archive.ubuntu.com/ubuntu/raring main universe" \
7　　　　 >> /etc/apt/sources.list
8 RUN apt-get update && apt-get install -y nginx
9 RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
10 # 当容器启动时要执行的指令11 CMD /usr/sbin/nginx
```

* #：注释
* \ : 反斜杠“\”进行指令换行，这样一条较长的指令就会被分为多行显示

小提示

Dockerfile 文件是 Docker 构建镜像的脚本文件，名字可以自定义，但在构建镜像时默认使用的是Dockerfile 文件。当定义为其他名称时，在进行镜像构建时，必须指定该脚本文件的位置和名称。因此，通常情况下，推荐直接使用默认的Dockerfile进行命名。



**Dockerfile常用指令**

在编写Dockerfile脚本文件时，开发者根据实际需要会使用到各种指令，如FROM、CMD、ADD等

12 

| 指令       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| FROM       | 指定基础镜像                                                 |
| MAINTAINER | 指定镜像维护者信息                                           |
| RUN        | 用于执行指定的脚本命令                                       |
| CMD        | 指定启动容器时执行的命令                                     |
| EXPOSE     | 指定容器暴露的端口                                           |
| ENV        | 指定环境变量                                                 |
| ADD        | 将文件从宿主机复制到容器指定位置,同时对压缩文件有自动解压功能 |
| COPY       | 将文件从宿舍机复制到容器指定位置                             |
| ENTRYPOINT | 设置容器启动时需要运行的命令                                 |
| WORKDIR    | 为后续的如 RUN、CMD、ENTRYPOINT、COPY、ADD指定工作目录       |



**FROM**指令使用的语法格式如下：

```
FROM <image>
FROM <image>:<tag>
```

在使用FROM指令时，需要注意以下几点。·一个有效的Dockerfile文件必须以FROM指令开头（除了ARG指令）。

* ·为了创建多重镜像或者互相依赖的镜像，在同一个 Dockerfile 文件中可能会出现多个FROM指令。
* ·<tag>参数是可选的，其作用主要是进一步对镜像区分，例如版本、型号等。如果没有使用该参数，则默认是latest；如果设置的<tag>参数不存在，则构建镜像也会失败。



**MAINTAINER**

MAINTAINER 指令用于指定当前构建的镜像维护者信息，该指令没有具体的格式要求，通常建议使用用户名和邮箱进行标识，

```
MAINTAINER "shitou"<shitou@163.com>
```



**RUN**

RUN指令用于执行指定的脚本命令，有两种格式，其语法格式如下。

```
RUN <command>
RUN ["executable", "param1", "param2"]
```

前者将在 shell 终端中运行命令，即 /bin/sh -c；后者则使用 exec 执行。指定使用其他终端可以通过第二种方式实现，例如 RUN ["/bin/bash", "-c", "echo hello"]。

其中每条 RUN 指令将在当前镜像基础上执行指定命令，并提交为新的镜像。如果要执行多条RUN指令，通常会将多条RUN指令合成一条，并使用斜杠“\” 来换行，这样将减小所构建的镜像的体积。



**CMD**

CMD指令用于指定启动容器时执行的命令，该指令有三种格式，其语法格式如下。

```shell
#使用 exec 执行，也是推荐方式；
CMD ["executable","param1","param2"]

#在 /bin/sh 中执行，提供给需要交互的应用；
CMD command param1 param2　

#提供给 ENTRYPOINT 的默认参数；
CMD ["param1","param2"]
```

需要注意的是，在使用CMD指令时，每个 Dockerfile 只能有一条CMD 指令，如果有多条CMD指令，则只有最后一条生效。如果用户启动容器时指定了运行的指令，则会覆盖掉CMD指定的指令。



**EXPOSE**

EXPOSE指令用于声明容器内部暴露的端口号，供容器访问连接使用，其语法格式如下。

```
EXPOSE <port> [<port>...]
```



**ENV**

ENV 指令用于为下文设定一个环境变量，该变量值在后续指令或内联文件中都可以使用。ENV指令有两种语法格式，具体如下。

```
ENV <key> <value>
ENV <key>=<value> <key>=<value> ..
```

.在上述两种语法格式中，第一种格式为一个属性设置唯一的属性值，<key>属性第一个空格之后的所有字符串（包括空格、引号）都将被视为该属性的值；第二种格式允许同时为多个属性赋值，而这种方式里面的引号、反斜杠等将被解析掉。



**ADD**

ADD指令用于复制指定的 src资源文件到容器中的 dest目录下，复制的资源可以是文件、目录以及远程URLs资源。其语法格式如下。

```
ADD <src>...<dest>
```

在使用ADD指令时，复制的src资源文件必须是当前上下文目录或其子目录，而复制的内容实际上是该目录下的所有内容，其中包括文件系统元数据，而目录本身不会被复制。当 dest目录不存在时，会在复制文件时自动创建。需要注意的是，当使用ADD指令复制的文件是一个压缩包时，ADD指令会在复制好该文件后，自动进行解压。

在使用ADD指令时，复制的src资源文件路径允许使用通配符，而dest目标目录可以使用绝对路径，也可以使用预先用WORKDIR指令定义的相对路径。



**COPY**

COPY指令的作用与ADD指令类似，都是复制指定的src资源文件到容器中的 dest目录下。区别在于，COPY指令不能复制远程URL路径文件，也不能解压文件，而ADD指令则可以。其语法格式如下。

```shell
COPY <src>...<dest>
```





**ENTRYPOINT**

ENTRYPOINT 指令是配置容器启动后执行的命令，每个Dockerfile 中只能有一个ENTRYPOINT，当指定多个ENTRYPOINT指令时，只有最后一个生效。该指令有两种语法格式，其语法格式如下。

```shell
#exec 格式, 推荐的
ENTRYPOINT ["executable", "param1", "param2"] 

#shell 格式
ENTRYPOINT command param1 param2 
```



**WORKDIR**

WORKDIR 指令用于为后续的指令（如 RUN、CMD、ENTRYPOINT、COPY、ADD）指定工作目录，在同一个Dockerfile文件中可以多次使用WORKDIR指令，其语法格式如下。

```
WORKDIR /path/to/workdir
```





**dockerignore文件**

* 在实际情况下，Docker在读取应用上下文中的Dockerfile文件进行镜像构建之前，都会先查看当前应用上下文中是否包含一个名为.dockerignore 的文件，如果该文件存在，则 Docker会先将

* .dockerignore文件中声明的文件或目录进行排除，然后再读取Dockerfile进行镜像构建。使用.dockerignore 将有助于在进行文件复制过程中避免向进程中加入过大或者敏感的无用文件和目录。.dockerignore 文件同 Dockerfile 文件一样，也是一个文本文件。二者的主要区别在于.dockerignore中存放的是被排除的文件，而Dockerfile中存放的是需要执行的指令。



 **.dockerignore**

```
1 # comment
2 */temp*
3 */ */temp*
4 temp?
```

* 第1行代码表示注释内容，其余3行代码均为被排除的文件。从被排除文件的编写方式可以看出，.dockerignore文件中可以使用通配符排除匹配路径下的文件。下面针对使用通配符排除匹配路径下的文件进行具体分析。
* ·*/temp*：排除根目录下任意子目录中所有名字以 temp 开头的文件或目录。例如文件/somedir/temporary.txt会被排除。
* ·*/ */temp*：排除根目录下任意两级子目录中所有名字以temp开头的文件或目录。例如文件/somedir/subdir/temporary.txt会被排除。
* ·temp?：排除根目录下名字以temp开头，后面为任意一个字符的文件或目录。例如目录/tempa和/tempb都将被排除。



## 7.3 Docker客户端常用命令

**Docker常用操作指令**



| 指令          | 说明           |
| ------------- | -------------- |
| docker images | 列出镜像       |
| docker search | 搜索镜像       |
| docker pull   | 拉取镜像       |
| docker build  | 构建镜像       |
| docker rmi    | 删除镜像       |
| docker run    | 创建并启动容器 |
| docker ps     | 列出容器       |
| docker exec   | 执行容器       |
| docker stop   | 停止容器       |
| docker start  | 启动容器       |
| docker rm     | 删除容器       |



**Docker管理指令**

| 管理指令         | 说明                     |
| ---------------- | ------------------------ |
| docker container | 用于管理容器             |
| docker image     | 用于管理镜像             |
| docker network   | 用于管理 Docker 网络     |
| docker node      | 用于管理 Swarm 集群节点  |
| docker plugin    | 用于管理插件             |
| docker secret    | 用于管理 Docker 机密     |
| docker service   | 用于管理 Docker 一些服务 |
| docker stack     | 用于管理 Docker 堆栈     |
| docker swarm     | 用于管理 Swarm           |
| docker system    | 用于管理 Docker          |
| docker volume    | 用于管理数据卷           |





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
