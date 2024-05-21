# 书《深入浅出Spring Boot 3.x》-学习笔记



> PS：本书是记录杨开振的《深入浅出Spring Boot 3.x》这本书的学习笔记，内容较多，建议收藏。
>
> 书籍评语：



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[TOC]





# 第1章 Spring Boot 3.x的来临
## 1.1 Spring框架的历史

复杂的XML -> 约定大于配置



## 1.2 Spring Boot的特点

Spring Boot的特点如下：

* 能够创建独立的Spring应用程序；
* 能够嵌入Tomcat、Jetty或者Undertow等服务器，无须部署WAR文件；
* 允许通过Maven或Gradle来根据需要获取启动器(starter)；
* 尽可能地自动配置Spring；
* 提供生产就绪型功能，如指标、健康检查和外部配置；
* 绝对没有代码生成，对XML没有配置要求。
* 开箱即用



## 1.3 Spring和Spring Boot的关系

Spring Boot的目的不是摆脱Spring框架，而是使Spring框架更容易使用。



## 1.4 开发Spring Boot项目











# 第2章 聊聊开发环境搭建和基本开发
## 2.1 搭建Spring Boot开发环境

## 2.1.1 搭建Eclipse开发环境

## 2.1.2 搭建InteliJ IDEA开发环境

## 2.2 使用自定义配置

进行Spring Boot的参数配置除了使用属性文件，还可以使用yml文件，它会按照以下**优先级进行加载**：

* 命令行参数；
* 来自java:comp/env的JNDI属性；
* Java系统属性（System.getProperties()）；
* 操作系统环境变量；●RandomValuePropertySource配置的random.*属性值；
* jar包外部的application-{profile}.properties或application.yml（带spring.profile）配置文件；
* jar包内部的application-{profile}.properties或application.yml（带spring.profile）配置文件；
* jar包外部的application.properties或application.yml（不带spring.profile）配置文件；
* jar包内部的application.properties或application.yml（不带spring.profile）配置文件；
* @Configuration注解类上的@PropertySource；
* 通过SpringApplication.setDefaultProperties指定的默认属性。



## 2.3 开发自己的Spring Boot项目







# 第3章 全注解下的Spring loC

Spring最成功之处在于其提出的理念，而不是其技术本身。Spring依赖两个核心理念，一个是**控制反转(inversion of control，IoC)**，另一个是**面向方面的程序设计(aspect-oriented programming，AOP)**。IoC是Spring的核心，可以说Spring是一种基于IoC编程的框架。因为Spring Boot是基于注解来应用Spring IoC的。



Spring IoC是一种通过描述来创建或者获取对象的技术，而这种技术不是Spring甚至不是Java独有的。Java初学者更为熟悉的是使用new关键字来创建对象，而Spring是通过描述来创建对象的。Spring Boot并不建议使用XML，而是通过注解的描述生成对象。



在Spring中，每个需要管理的对象称为Spring Bean（简称Bean），而Spring管理这些Bean的容器称为Spring IoC容器（简称IoC容器）。IoC容器需要具备两个基本的功能：

* 通过描述管理Bean，包括定义、发布、装配和销毁Bean；
* 通过描述完成Bean之间的依赖关系。



## 3.1 loC容器简介

IoC容器是一个管理Bean的容器，在Spring的定义中，所有IoC容器都需要实现接口BeanFactory，它是一个顶级容器接口。

```java
package org.springframework.beans.factory;

import org.springframework.beans.BeansException;
import org.springframework.core.ResolvableType;
import org.springframework.lang.Nullable;

public interface BeanFactory {

	/**
	 * Used to dereference a {@link FactoryBean} instance and distinguish it from
	 * beans <i>created</i> by the FactoryBean. For example, if the bean named
	 * {@code myJndiObject} is a FactoryBean, getting {@code &myJndiObject}
	 * will return the factory, not the instance returned by the factory.
	 */
	String FACTORY_BEAN_PREFIX = "&";


	/**
	 * Return an instance, which may be shared or independent, of the specified bean.
	 * <p>This method allows a Spring BeanFactory to be used as a replacement for the
	 * Singleton or Prototype design pattern. Callers may retain references to
	 * returned objects in the case of Singleton beans.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to retrieve
	 * @return an instance of the bean
	 * @throws NoSuchBeanDefinitionException if there is no bean with the specified name
	 * @throws BeansException if the bean could not be obtained
	 */
	Object getBean(String name) throws BeansException;

	/**
	 * Return an instance, which may be shared or independent, of the specified bean.
	 * <p>Behaves the same as {@link #getBean(String)}, but provides a measure of type
	 * safety by throwing a BeanNotOfRequiredTypeException if the bean is not of the
	 * required type. This means that ClassCastException can't be thrown on casting
	 * the result correctly, as can happen with {@link #getBean(String)}.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to retrieve
	 * @param requiredType type the bean must match; can be an interface or superclass
	 * @return an instance of the bean
	 * @throws NoSuchBeanDefinitionException if there is no such bean definition
	 * @throws BeanNotOfRequiredTypeException if the bean is not of the required type
	 * @throws BeansException if the bean could not be created
	 */
	<T> T getBean(String name, Class<T> requiredType) throws BeansException;

	/**
	 * Return an instance, which may be shared or independent, of the specified bean.
	 * <p>Allows for specifying explicit constructor arguments / factory method arguments,
	 * overriding the specified default arguments (if any) in the bean definition.
	 * @param name the name of the bean to retrieve
	 * @param args arguments to use when creating a bean instance using explicit arguments
	 * (only applied when creating a new instance as opposed to retrieving an existing one)
	 * @return an instance of the bean
	 * @throws NoSuchBeanDefinitionException if there is no such bean definition
	 * @throws BeanDefinitionStoreException if arguments have been given but
	 * the affected bean isn't a prototype
	 * @throws BeansException if the bean could not be created
	 * @since 2.5
	 */
	Object getBean(String name, Object... args) throws BeansException;

	/**
	 * Return the bean instance that uniquely matches the given object type, if any.
	 * <p>This method goes into {@link ListableBeanFactory} by-type lookup territory
	 * but may also be translated into a conventional by-name lookup based on the name
	 * of the given type. For more extensive retrieval operations across sets of beans,
	 * use {@link ListableBeanFactory} and/or {@link BeanFactoryUtils}.
	 * @param requiredType type the bean must match; can be an interface or superclass
	 * @return an instance of the single bean matching the required type
	 * @throws NoSuchBeanDefinitionException if no bean of the given type was found
	 * @throws NoUniqueBeanDefinitionException if more than one bean of the given type was found
	 * @throws BeansException if the bean could not be created
	 * @since 3.0
	 * @see ListableBeanFactory
	 */
	<T> T getBean(Class<T> requiredType) throws BeansException;

	/**
	 * Return an instance, which may be shared or independent, of the specified bean.
	 * <p>Allows for specifying explicit constructor arguments / factory method arguments,
	 * overriding the specified default arguments (if any) in the bean definition.
	 * <p>This method goes into {@link ListableBeanFactory} by-type lookup territory
	 * but may also be translated into a conventional by-name lookup based on the name
	 * of the given type. For more extensive retrieval operations across sets of beans,
	 * use {@link ListableBeanFactory} and/or {@link BeanFactoryUtils}.
	 * @param requiredType type the bean must match; can be an interface or superclass
	 * @param args arguments to use when creating a bean instance using explicit arguments
	 * (only applied when creating a new instance as opposed to retrieving an existing one)
	 * @return an instance of the bean
	 * @throws NoSuchBeanDefinitionException if there is no such bean definition
	 * @throws BeanDefinitionStoreException if arguments have been given but
	 * the affected bean isn't a prototype
	 * @throws BeansException if the bean could not be created
	 * @since 4.1
	 */
	<T> T getBean(Class<T> requiredType, Object... args) throws BeansException;

	/**
	 * Return a provider for the specified bean, allowing for lazy on-demand retrieval
	 * of instances, including availability and uniqueness options.
	 * @param requiredType type the bean must match; can be an interface or superclass
	 * @return a corresponding provider handle
	 * @since 5.1
	 * @see #getBeanProvider(ResolvableType)
	 */
	<T> ObjectProvider<T> getBeanProvider(Class<T> requiredType);

	/**
	 * Return a provider for the specified bean, allowing for lazy on-demand retrieval
	 * of instances, including availability and uniqueness options.
	 * @param requiredType type the bean must match; can be a generic type declaration.
	 * Note that collection types are not supported here, in contrast to reflective
	 * injection points. For programmatically retrieving a list of beans matching a
	 * specific type, specify the actual bean type as an argument here and subsequently
	 * use {@link ObjectProvider#orderedStream()} or its lazy streaming/iteration options.
	 * @return a corresponding provider handle
	 * @since 5.1
	 * @see ObjectProvider#iterator()
	 * @see ObjectProvider#stream()
	 * @see ObjectProvider#orderedStream()
	 */
	<T> ObjectProvider<T> getBeanProvider(ResolvableType requiredType);

	/**
	 * Does this bean factory contain a bean definition or externally registered singleton
	 * instance with the given name?
	 * <p>If the given name is an alias, it will be translated back to the corresponding
	 * canonical bean name.
	 * <p>If this factory is hierarchical, will ask any parent factory if the bean cannot
	 * be found in this factory instance.
	 * <p>If a bean definition or singleton instance matching the given name is found,
	 * this method will return {@code true} whether the named bean definition is concrete
	 * or abstract, lazy or eager, in scope or not. Therefore, note that a {@code true}
	 * return value from this method does not necessarily indicate that {@link #getBean}
	 * will be able to obtain an instance for the same name.
	 * @param name the name of the bean to query
	 * @return whether a bean with the given name is present
	 */
	boolean containsBean(String name);

	/**
	 * Is this bean a shared singleton? That is, will {@link #getBean} always
	 * return the same instance?
	 * <p>Note: This method returning {@code false} does not clearly indicate
	 * independent instances. It indicates non-singleton instances, which may correspond
	 * to a scoped bean as well. Use the {@link #isPrototype} operation to explicitly
	 * check for independent instances.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @return whether this bean corresponds to a singleton instance
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @see #getBean
	 * @see #isPrototype
	 */
	boolean isSingleton(String name) throws NoSuchBeanDefinitionException;

	/**
	 * Is this bean a prototype? That is, will {@link #getBean} always return
	 * independent instances?
	 * <p>Note: This method returning {@code false} does not clearly indicate
	 * a singleton object. It indicates non-independent instances, which may correspond
	 * to a scoped bean as well. Use the {@link #isSingleton} operation to explicitly
	 * check for a shared singleton instance.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @return whether this bean will always deliver independent instances
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @since 2.0.3
	 * @see #getBean
	 * @see #isSingleton
	 */
	boolean isPrototype(String name) throws NoSuchBeanDefinitionException;

	/**
	 * Check whether the bean with the given name matches the specified type.
	 * More specifically, check whether a {@link #getBean} call for the given name
	 * would return an object that is assignable to the specified target type.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @param typeToMatch the type to match against (as a {@code ResolvableType})
	 * @return {@code true} if the bean type matches,
	 * {@code false} if it doesn't match or cannot be determined yet
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @since 4.2
	 * @see #getBean
	 * @see #getType
	 */
	boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;

	/**
	 * Check whether the bean with the given name matches the specified type.
	 * More specifically, check whether a {@link #getBean} call for the given name
	 * would return an object that is assignable to the specified target type.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @param typeToMatch the type to match against (as a {@code Class})
	 * @return {@code true} if the bean type matches,
	 * {@code false} if it doesn't match or cannot be determined yet
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @since 2.0.1
	 * @see #getBean
	 * @see #getType
	 */
	boolean isTypeMatch(String name, Class<?> typeToMatch) throws NoSuchBeanDefinitionException;

	/**
	 * Determine the type of the bean with the given name. More specifically,
	 * determine the type of object that {@link #getBean} would return for the given name.
	 * <p>For a {@link FactoryBean}, return the type of object that the FactoryBean creates,
	 * as exposed by {@link FactoryBean#getObjectType()}. This may lead to the initialization
	 * of a previously uninitialized {@code FactoryBean} (see {@link #getType(String, boolean)}).
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @return the type of the bean, or {@code null} if not determinable
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @since 1.1.2
	 * @see #getBean
	 * @see #isTypeMatch
	 */
	@Nullable
	Class<?> getType(String name) throws NoSuchBeanDefinitionException;

	/**
	 * Determine the type of the bean with the given name. More specifically,
	 * determine the type of object that {@link #getBean} would return for the given name.
	 * <p>For a {@link FactoryBean}, return the type of object that the FactoryBean creates,
	 * as exposed by {@link FactoryBean#getObjectType()}. Depending on the
	 * {@code allowFactoryBeanInit} flag, this may lead to the initialization of a previously
	 * uninitialized {@code FactoryBean} if no early type information is available.
	 * <p>Translates aliases back to the corresponding canonical bean name.
	 * Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the name of the bean to query
	 * @param allowFactoryBeanInit whether a {@code FactoryBean} may get initialized
	 * just for the purpose of determining its object type
	 * @return the type of the bean, or {@code null} if not determinable
	 * @throws NoSuchBeanDefinitionException if there is no bean with the given name
	 * @since 5.2
	 * @see #getBean
	 * @see #isTypeMatch
	 */
	@Nullable
	Class<?> getType(String name, boolean allowFactoryBeanInit) throws NoSuchBeanDefinitionException;

	/**
	 * Return the aliases for the given bean name, if any.
	 * All of those aliases point to the same bean when used in a {@link #getBean} call.
	 * <p>If the given name is an alias, the corresponding original bean name
	 * and other aliases (if any) will be returned, with the original bean name
	 * being the first element in the array.
	 * <p>Will ask the parent factory if the bean cannot be found in this factory instance.
	 * @param name the bean name to check for aliases
	 * @return the aliases, or an empty array if none
	 * @see #getBean
	 */
	String[] getAliases(String name);
    
}
```

* 在IoC容器中， Bean默认都是以单例存在的，也就是使用getBean()方法根据名称或者类型获取的对象，在默认的情况下，返回的都是同一个对象。
* 由于BeanFactory接口定义的功能还不够强大，因此Spring在BeanFactory的基础上，还设计了一个更为高级的接口ApplicationContext，它是BeanFactory的子接口之一。在Spring的体系中，BeanFactory和ApplicationContext是最为重要的接口设计，在现实中我们使用的大部分IoC容器是ApplicationContext接口的实现类
* **ApplicationContext**接口通过扩展上级接口，进而扩展了BeanFactory接口，但是在BeanFactory的基础上，扩展了消息国际化接口(**MessageSource**)、环境可配置化接口(**EnvironmentCapable**)、应用事件发布接口(**ApplicationEventPublisher**)和资源模式解析器接口(**ResourcePatternResolver**)，所以ApplicationContext的功能会更为强大。



## 3.2 装配你的Bean

### 3.2.1 通过扫描装配你的Bean

@Component和@ComponentScan的结合。

* @Component：标注扫描哪些类，创建Bean并装配到IoC容器中。
* @ComponentScan：配置采用何种策略扫描并装配Bean。

```java
package org.springframework.context.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.beans.factory.support.BeanNameGenerator;
import org.springframework.core.annotation.AliasFor;
import org.springframework.core.type.filter.TypeFilter;

/**
 * Configures component scanning directives for use with @{@link Configuration} classes.
 * Provides support parallel with Spring XML's {@code <context:component-scan>} element.
 *
 * <p>Either {@link #basePackageClasses} or {@link #basePackages} (or its alias
 * {@link #value}) may be specified to define specific packages to scan. If specific
 * packages are not defined, scanning will occur from the package of the
 * class that declares this annotation.
 *
 * <p>Note that the {@code <context:component-scan>} element has an
 * {@code annotation-config} attribute; however, this annotation does not. This is because
 * in almost all cases when using {@code @ComponentScan}, default annotation config
 * processing (e.g. processing {@code @Autowired} and friends) is assumed. Furthermore,
 * when using {@link AnnotationConfigApplicationContext}, annotation config processors are
 * always registered, meaning that any attempt to disable them at the
 * {@code @ComponentScan} level would be ignored.
 *
 * <p>See {@link Configuration @Configuration}'s Javadoc for usage examples.
 *
 * @author Chris Beams
 * @author Juergen Hoeller
 * @author Sam Brannen
 * @since 3.1
 * @see Configuration
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Repeatable(ComponentScans.class)
public @interface ComponentScan {

	/**
	 * Alias for {@link #basePackages}.
	 * <p>Allows for more concise annotation declarations if no other attributes
	 * are needed &mdash; for example, {@code @ComponentScan("org.my.pkg")}
	 * instead of {@code @ComponentScan(basePackages = "org.my.pkg")}.
	 */
	@AliasFor("basePackages")
	String[] value() default {};

	/**
	 * Base packages to scan for annotated components.
	 * <p>{@link #value} is an alias for (and mutually exclusive with) this
	 * attribute.
	 * <p>Use {@link #basePackageClasses} for a type-safe alternative to
	 * String-based package names.
	 */
	@AliasFor("value")
	String[] basePackages() default {};

	/**
	 * Type-safe alternative to {@link #basePackages} for specifying the packages
	 * to scan for annotated components. The package of each class specified will be scanned.
	 * <p>Consider creating a special no-op marker class or interface in each package
	 * that serves no purpose other than being referenced by this attribute.
	 */
	Class<?>[] basePackageClasses() default {};

	/**
	 * The {@link BeanNameGenerator} class to be used for naming detected components
	 * within the Spring container.
	 * <p>The default value of the {@link BeanNameGenerator} interface itself indicates
	 * that the scanner used to process this {@code @ComponentScan} annotation should
	 * use its inherited bean name generator, e.g. the default
	 * {@link AnnotationBeanNameGenerator} or any custom instance supplied to the
	 * application context at bootstrap time.
	 * @see AnnotationConfigApplicationContext#setBeanNameGenerator(BeanNameGenerator)
	 */
	Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

	/**
	 * The {@link ScopeMetadataResolver} to be used for resolving the scope of detected components.
	 */
	Class<? extends ScopeMetadataResolver> scopeResolver() default AnnotationScopeMetadataResolver.class;

	/**
	 * Indicates whether proxies should be generated for detected components, which may be
	 * necessary when using scopes in a proxy-style fashion.
	 * <p>The default is defer to the default behavior of the component scanner used to
	 * execute the actual scan.
	 * <p>Note that setting this attribute overrides any value set for {@link #scopeResolver}.
	 * @see ClassPathBeanDefinitionScanner#setScopedProxyMode(ScopedProxyMode)
	 */
	ScopedProxyMode scopedProxy() default ScopedProxyMode.DEFAULT;

	/**
	 * Controls the class files eligible for component detection.
	 * <p>Consider use of {@link #includeFilters} and {@link #excludeFilters}
	 * for a more flexible approach.
	 */
	String resourcePattern() default ClassPathScanningCandidateComponentProvider.DEFAULT_RESOURCE_PATTERN;

	/**
	 * Indicates whether automatic detection of classes annotated with {@code @Component}
	 * {@code @Repository}, {@code @Service}, or {@code @Controller} should be enabled.
	 */
	boolean useDefaultFilters() default true;

	/**
	 * Specifies which types are eligible for component scanning.
	 * <p>Further narrows the set of candidate components from everything in {@link #basePackages}
	 * to everything in the base packages that matches the given filter or filters.
	 * <p>Note that these filters will be applied in addition to the default filters, if specified.
	 * Any type under the specified base packages which matches a given filter will be included,
	 * even if it does not match the default filters (i.e. is not annotated with {@code @Component}).
	 * @see #resourcePattern()
	 * @see #useDefaultFilters()
	 */
	Filter[] includeFilters() default {};

	/**
	 * Specifies which types are not eligible for component scanning.
	 * @see #resourcePattern
	 */
	Filter[] excludeFilters() default {};

	/**
	 * Specify whether scanned beans should be registered for lazy initialization.
	 * <p>Default is {@code false}; switch this to {@code true} when desired.
	 * @since 4.1
	 */
	boolean lazyInit() default false;


	/**
	 * Declares the type filter to be used as an {@linkplain ComponentScan#includeFilters
	 * include filter} or {@linkplain ComponentScan#excludeFilters exclude filter}.
	 */
	@Retention(RetentionPolicy.RUNTIME)
	@Target({})
	@interface Filter {

		/**
		 * The type of filter to use.
		 * <p>Default is {@link FilterType#ANNOTATION}.
		 * @see #classes
		 * @see #pattern
		 */
		FilterType type() default FilterType.ANNOTATION;

		/**
		 * Alias for {@link #classes}.
		 * @see #classes
		 */
		@AliasFor("classes")
		Class<?>[] value() default {};

		/**
		 * The class or classes to use as the filter.
		 * <p>The following table explains how the classes will be interpreted
		 * based on the configured value of the {@link #type} attribute.
		 * <table border="1">
		 * <tr><th>{@code FilterType}</th><th>Class Interpreted As</th></tr>
		 * <tr><td>{@link FilterType#ANNOTATION ANNOTATION}</td>
		 * <td>the annotation itself</td></tr>
		 * <tr><td>{@link FilterType#ASSIGNABLE_TYPE ASSIGNABLE_TYPE}</td>
		 * <td>the type that detected components should be assignable to</td></tr>
		 * <tr><td>{@link FilterType#CUSTOM CUSTOM}</td>
		 * <td>an implementation of {@link TypeFilter}</td></tr>
		 * </table>
		 * <p>When multiple classes are specified, <em>OR</em> logic is applied
		 * &mdash; for example, "include types annotated with {@code @Foo} OR {@code @Bar}".
		 * <p>Custom {@link TypeFilter TypeFilters} may optionally implement any of the
		 * following {@link org.springframework.beans.factory.Aware Aware} interfaces, and
		 * their respective methods will be called prior to {@link TypeFilter#match match}:
		 * <ul>
		 * <li>{@link org.springframework.context.EnvironmentAware EnvironmentAware}</li>
		 * <li>{@link org.springframework.beans.factory.BeanFactoryAware BeanFactoryAware}
		 * <li>{@link org.springframework.beans.factory.BeanClassLoaderAware BeanClassLoaderAware}
		 * <li>{@link org.springframework.context.ResourceLoaderAware ResourceLoaderAware}
		 * </ul>
		 * <p>Specifying zero classes is permitted but will have no effect on component
		 * scanning.
		 * @since 4.2
		 * @see #value
		 * @see #type
		 */
		@AliasFor("value")
		Class<?>[] classes() default {};

		/**
		 * The pattern (or patterns) to use for the filter, as an alternative
		 * to specifying a Class {@link #value}.
		 * <p>If {@link #type} is set to {@link FilterType#ASPECTJ ASPECTJ},
		 * this is an AspectJ type pattern expression. If {@link #type} is
		 * set to {@link FilterType#REGEX REGEX}, this is a regex pattern
		 * for the fully-qualified class names to match.
		 * @see #type
		 * @see #classes
		 */
		String[] pattern() default {};

	}

}
```

* basePackages：指定需要扫描的包名，如果不配置它或者包名为空，则只扫描当前包和其子包下的路径。

* basePackageClasses：指定被扫描的类；

* includeFilters：指定满足过滤器(Filter)条件的类将会被IoC容器扫描、装配；

* excludeFilters：指定满足过滤器条件的类将不会被IoC容器扫描、装配。

* lazyInit：延迟初始化

* includeFilters和excludeFilters这两个配置项都需要通过一个注解@Filter定义，这个注解有以下配置项。

  * type：通过它可以选择通过注解或者正则式等进行过滤；

    ```java
    @ComponentScan(basePackages = "com.xxx.*", excludeFilters = @Filter(type=FilterType.ANNOTATION, classes=xxxName.class))
    ```

  * classes：通过它可以指定通过什么注解进行过滤，只有被标注了指定注解的类才会被过滤；

  * pattern：通过它可以定义过滤的正则式。

```java

package org.springframework.boot.autoconfigure;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.boot.SpringBootConfiguration;
import org.springframework.boot.context.TypeExcludeFilter;
import org.springframework.boot.context.properties.ConfigurationPropertiesScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.ComponentScan.Filter;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.core.annotation.AliasFor;
import org.springframework.data.repository.Repository;

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

### 3.2.2 自定义第三方Bean

开发现实的Java应用往往需要引入许多第三方包，并且很有可能希望把第三方包的类对象也装配到IoC容器中，这时注解@Bean就可以发挥作用了。



## 3.3 依赖注入

### 3.3.1 注解@Autowired

在IoC容器的概念中，主要是使用依赖注入来实现Bean之间的依赖关系的。

@Autowired也是Spring中最常用的注解之一，十分重要，它会按属性的类型找到对应的Bean进行注入。

@Autowired的注入策略中最基本的一条是按类型，我们回顾IoC容器的顶级接口BeanFactory，就可以知道IoC容器是通过getBean()方法获取对应的Bean的，而getBean()方法又支持按名称或者按类型。



**@Autowired提供以下规则：按类型找到对应的Bean，如果对应类型的Bean不是唯一的，那么它会根据其属性名称和Bean的名称进行匹配。如果匹配得上，就会使用该Bean；如果还无法匹配，就会抛出异常。**



注意，@Autowired是一个默认必须找到对应Bean的注解，如果不能确定其标注属性一定会存在并且允许这个被标注的属性为null，那么可以配置@Autowired的required属性为false。

当然，在大部分情况下，我都不推这样做，因为这样极其容易抛出“臭名昭著”的空指针异常(NullPointerException)。同样，它除了可以标注属性，还可以标注方法（如set方法）



### 3.3.2 消除歧义性--@Primary和@Qualifier

* **@Primary**：它是一个修改优先权的注解，当发现有多个同样类型的Bean时，请优使用进行注入。
* **@Qualifier**：@Qualifier的配置项value需要用一个字符串定义，它将与@Autowired组合在一起，通过名称和类型一起找到Bean。



### 3.3.3 带有参数的构造方法类的装配

```java
public class XxxAClass {
    private XxxBClass xxxBClass = nu;;
    
    public XxxAClass(@Autowired @Qualifier("xxxB") XxxBClass xxxBClass){
        this.xxxBClass = xxxBClass;
    } 
}
```



## 3.4 生命周期

Bean的生命周期大致包括定义、初始化、生存期和销毁4个部分。

* Bean的定义过程大致如下。
  * (1)Spring通过我们的配置，到@ComponentScan定义的扫描路径中找到标注@Component的类，这个过程就是一个资源定位的过程。
  * (2)一旦找到了资源，Spring就会解析这些资源，并将其保存为Bean的定义(BeanDefinition)。注意，此时还没有初始化Bean，也就没有Bean的实例，有的仅仅是Bean的定义。
  * (3)把Bean的定义发布到IoC容器中。此时，IoC容器中装载的也只有Bean的定义，还没有生成Bean的实例。
* 初始化、生存期、销毁的详细流程如下：
  * 初始化
  * 依赖注入
  * （接口BeanNameAware）setBeanName()方法
  * （接口BeanFactoryAware）setBeanFactory()方法
  * （接口ApplicationContextAware（需要容器实现接口ApplicationContext才会被调用））setApplicationContext()方法
  * （BeanPostProcessor的预初始化方法（注意，它针对所有Bean生效））postProcessBeforeInitialization()方法
  * （@PostConstruct标注方法）自定义初始化方法
  * （接口InitializingBean）afterPropertiesSet()方法
  * （BeanPostProcessor的后初始化方法（注意，它正对所有Bean生效））postProcessAfterInitialization()方法
  * 生存期
  * （@PreDestory标注的方法）自定义销毁方法
  * （接口DisposableBean）destroy()方法





## 3.5 使用属性文件

如果将所有配置项都放在文件application.properties中，显然会使这个文件配置的内容太多。为了减少application.properties的内容，Spring允许引入属性文件。

**@Value**：@Value，使用${......}这样的占位符读取配置在属性文件的内容。这里的@Value既可以加载属性，也可以加在方法上。

**@ConfigurationProperties**

**@EnableConfigurationProperties**：在Spring Boot启动文件中加入注解@EnableConfigurationProperties，这个注解表示启用属性文件的配置机制，加入注解后才能将这些配置读入POJO中。

**@PropertySource**：@PropertySource来定义对应的属性文件，把它加载到Spring的上下文中。



## 3.6 条件装配Bean

有时候某些客观的因素会使一些Bean无法进行初始化。例如，漏掉数据源的一些配置会造成数据库无法连接。在这样的情况下，IoC容器如果继续进行数据源的装配，系统将会抛出异常，导致应用无法继续运行。这时，我们希望IoC容器不要装配数据源。

为了应对这样的场景，Spring提供了注解@Conditional，该注解需要配合另一个接口Condition(org.springframework.context.annotation.Condition)来实现对应的功能。

PS：某个类被@Conditional修饰，@Conditional里面的value的值指向的注解需要实现Condition接口。



## 3.7 Bean的作用域

Bean在IoC容器中以单例模式存在，这也是IoC容器的默认值；如果isPrototype()方法返回true，则每次获取Bean时，IoC容器都会创建一个新的Bean并返回给调用者，这两种情况显然存在很大的区别，这便是Bean的作用域的问题。在一般的容器中，Bean都会存在单例(singleton)和原型(prototype)两种作用域，Jakarta EE被广泛地使用在互联网中；而在Web容器中，存在页面(page)、请求(request)、会话(session)和应用(application)4种作用域，但是页面作用域的范围是JSP，在Spring中无法支持。



| 作用域        | 使用范围       | 作用域描述                                                   |
| ------------- | -------------- | ------------------------------------------------------------ |
| singleton     | 所有Spring应用 | 默认值，IoC容器只存在单例模式的Bean                          |
| prototype     | 所有Spring应用 | 每次从IoC容器中取出一个Bean时，创建一个新的Bean              |
| session       | Spring Web应用 | HTTP会话                                                     |
| application   | Spring Web应用 | Web工程生命周期                                              |
| request       | Spring Web应用 | Web工程单词请求                                              |
| globalSession | Spring Web应用 | 在一个全局的HTTP会话中，一个Bean定义对应一个实例。实践中不使用。 |



## 3.8 使用注解@Profile

在Spring中可以通过配置两个参数来启用Profile机制，一个是spring.profiles.active，另一个是spring.profiles.default。在这两个参数都没有配置的情况下，Spring不会启用Profile机制，这就意味着被@Profile标注的Bean将不会被Spring装配到IoC容器中。**spring.profiles.active**的优先级大于**spring.profiles.default**，Spring先判断是否存在spring.profiles.active配置，再查找spring.profiles.default配置。



按照Spring Boot的规则，假设把选项-Dspring.profiles.active配置的值记为{profile}，则它会用application-{profile}.properties文件代替原来默认的application.properties文件。





## 3.9 使用SpEL

```java
// 读取属性文件的值
@Value("${baseUrl}")

// 调用一次System.currentTimeMillis()方法来为属性赋值。
@Value("#{T(System).currentTimeMillis()}")
private Long initTime =null;

// 注意，在上述SpEL中，引用str属性后，跟着是一个?，这个符号的含义是判断这个属性是否为空。如果不为空才会运行toUpperCase()方法，进而把引用到的属性的字符串转换为大写并赋予当前属性。
@Value("#{beanName.str?.toUpperCase()}")
private String otherBeanProp = null;
```





# 第4章 开始约定编程--Spring AOP
## 4.1 约定编程

### 4.1.1 约定

### 4.1.2 ProxyBean的实现

**动态代理：JDK 和 CGLB**（在Spring Boot应用中，默认使用的是CGLIB动态代理）





## 4.2 AOP的知识
### 4.2.1 为什么要使用AOP

使用AOP可以处理一些无法使用面向对象编程实现的业务逻辑。其次，通过约定可以将一些业务逻辑织入流程中，并且可以将一些通用的逻辑抽取出来给予默认实现，开发者只需要完成部分功能。这样做可以使开发代码更加简短，同时可读性和可维护性也得到提高。

### 4.2.2 AOP的术语和流程

* 连接点(join point)：并非所有地方都需要启用AOP，而连接点就是告诉AOP在哪里需要通过包装将方法织入流程。因为Spring只能支持方法，所以被拦截的往往就是指定的方法。
* 切点(point cut)：有时候需要启用AOP的地方不是单个方法，而是多个类的不同方法。这时，可以通过正则式和指示器的规则来定义切点，让AOP根据切点的定义匹配多个需要AOP拦截的方法，将它们包装为成一个个连接点。
* 通知(advice)：约定的流程中的方法，分为前置通知(before advice)、后置通知(after advice)、环绕通知(around advice)、返回通知(afterReturning advice)和异常通知(afterThrowing advice)，这些通知会根据约定织入流程中，需要弄明白它们在流程中的运行顺序和运行的条件。
* 目标对象(target)：即被代理对象。
* 引入(introduction)：指引入新的类（接口）和其方法，可以增强现有Bean的功能。
* 织入(weaving)：它是一个通过动态代理技术，为目标对象生成代理对象，然后将与切点定义匹配的连接点拦截，并按约定将切面定义的各类通知织入流程的过程。
* 切面(aspect)：它是一个类，通过它和注解可以定义AOP的切点、各类通知和引入，AOP将通过它的信息来增强现有Bean的功能，并且将它定义的内容织入约定的流程中。



## 4.3 AOP开发详解
### 4.3.1 确定拦截目标

### 4.3.2 开发切面

* @Aspect：声明切面类
* @Component：将切面扫描到IoC容器中，这样切面类才能生效，所以类上面还标注了。
* @Before：前置通知
* @After：后置通知
* @AfterReturning：返回通知
* @AfterThrowing：异常通知

### 4.3.3 定义切点

@PointCut("")

```java
package com.learn.chapter4.aspect;
　
/**** imports ****/
// 声明为切面
@Aspect
// 将切面扫描到IoC容器中，这样切面类才能生效
@Component
public class MyAspect {
   // 通过正则式指定连接点（即哪些类的哪些方法）
   private static final String aopExp = "execution(* "
      + "com.learn.chapter4.aspect.service.impl.UserServiceImpl.printUser(..))";
   // 使用@Pointcut定义切点，后续的通知注解就可以使用这个方法名来描述需要拦截的方法了
   @Pointcut(aopExp)
   public void pointCut() {
   }
   @Before("pointCut()") // 使用切点
   public void before() {
      System.out.println("before ......");
   }
   @After("pointCut()") // 使用切点
   public void after() {
      System.out.println("after ......");
   }
　
   @AfterReturning("pointCut()") // 使用切点
   public void afterReturning() {
      System.out.println("afterReturning ......");
   }
   @AfterThrowing("pointCut()") // 使用切点
   public void afterThrowing() {
      System.out.println("afterThrowing ......");
   }
}
```



### 4.3.4 测试AOP
### 4.3.5 环绕通知

@Around("")



### 4.3.6 引入

```java
package com.learn.chapter4.aspect;
　
/**** imports ****/
// 声明为切面
@Aspect
// 将切面扫描到IoC容器中，这样切面类才能生效
@Component
public class MyAspect {
　
   @DeclareParents( // 定义引入增强
         // 需要引入增强的Bean
         value = "com.learn.chapter4.aspect.service.impl.UserServiceImpl",
         // 使用指定的类进行增强
         defaultImpl = UserValidatorImpl.class)
   // 增强接口
   public UserValidator userValidator;
   ......
}
```



### 4.3.7 通知获取参数

```java
// 指示器args(user)表示传递的参数
@Before("pointCut() && args(user)")
public void beforeParam(JoinPoint jp, User user) {
   System.out.println("传参前置通知，before ......");
}  
```

在正则式pointCut() && args(user)中，pointCut()表示启用原来定义切点的规则，args(user)表示指示器指示将连接点（目标对象方法）的名称为user的参数传递进来。注意：对于非环绕通知，AOP会自动地把JoinPoint类型的参数传递到通知中；对于环绕通知，则不能使用JoinPoint类型的参数，只能使用ProceedingJoinPoint类型的参数。



### 4.3.8 织入

织入是一个生成动态代理对象并且将切面和目标对象方法编入约定流程的过程。

Spring会按照这样的一条规则处理：当需要使用AOP的类拥有接口时，它会以JDK动态代理的方式运行，否则以CGLIB动态代理的方式运行。



## 4.4 多个切面

**@Order**：可以指定切面执行顺序

指定切面顺序后，前置通知都是按编号从小到大的顺序运行的，而后置通知和返回通知都是按编号从大到小的顺序运行的，这就是一个典型的责任链模式的顺序。





# 第5章 访问数据库
## 5.1 配置数据源
### 5.1.1 配置默认数据源

### 5.1.2 配置自定义数据源

## 5.2 使用JdbcTemplate操作数据库

## 5.3 使用JPA(Hibernate)操作数据库
### 5.3.1 概述
### 5.3.2 开发JPA

## 5.4 整合MyBatis框架
### 5.4.1 MyBatis简介

MyBatis的官方定义为：MyBatis是一款优秀的持久层框架，它支持自定义SQL语句、存储过程以及高级映射。MyBatis免除了几乎所有JDBC代码以及设置参数和获取结果集的工作。MyBatis可以通过简单的XML或注解来配置和映射原始类型、接口和Java POJO（Plain Old Java Objects，普通老式Java对象）为数据库中的记录。

MyBatis的配置文件包括两类：一是基础配置文件；二是映射文件。MyBatis也可以使用注解来实现映射，只是由于功能和可读性的限制，在实际的企业中使用得比较少。



### 5.4.2 MyBatis的配置
### 5.4.3 Spring Boot整合MyBatis

在Spring Boot中可以使用以下3种方式创建Mapper接口的实例。

* MapperFactoryBean：创建单个Mapper接口实例；
* MapperScannerConfigurer：通过扫描将Mapper接口实例装配到IoC容器中；
* @MapperScan：通过注解定义扫描，将Mapper接口实例装配到IoC容器中。

MapperFactoryBean单一处理某个Mapper接口，使用起来比较麻烦，目前使用不多，所以后续不再介绍这种方法。

```java
/***
 * 配置MyBatis接口扫描
 * @return 返回扫描器
 */
@Bean
public MapperScannerConfigurer mapperScannerConfig() {
   // 定义扫描器实例
   var mapperScannerConfigurer = new MapperScannerConfigurer();
   // 设置SqlSessionFactory，Spring Boot会自动创建SqlSessionFactory实例
   mapperScannerConfigurer.setSqlSessionFactoryBeanName("sqlSessionFactory");
   // 定义扫描的包
   mapperScannerConfigurer.setBasePackage("com.learn.chapter5.*"); // ①
   // 限定被标注@Mapper的接口才被扫描
   mapperScannerConfigurer.setAnnotationClass(Mapper.class); // ②
   // 通过继承某个接口限定扫描，一般使用不多
   // mapperScannerConfigurer.setMarkerInterface(......); // ③
   return mapperScannerConfigurer;
}
```

```java
package com.learn.chapter5.main;
　
/**** imports ****/
// 定义Spring Boot扫描包路径
@SpringBootApplication(scanBasePackages = {"com.learn.chapter5"})
// 定义MyBatis的扫描策略
@MapperScan(
       // 指定扫描包
       basePackages = "com.learn.chapter5.*",
       // 指定SqlSessionFactory，如果sqlSessionTemplate被指定，则作废
       sqlSessionFactoryRef = "sqlSessionFactory",
       // 指定sqlSessionTemplate，将忽略sqlSessionFactory的配置
       sqlSessionTemplateRef = "sqlSessionTemplate",
       // 限定扫描的接口标注有@Mapper
       annotationClass = Mapper.class
       // markerInterface = XXX.class,// 限定扫描接口，不常用
)
public class Chapter5Application {
　
   public static void main(String[] args) {
      SpringApplication.run(Chapter5Application.class, args);
   }
　
}
```



### 5.4.4 MyBatis的其他配置

```properties
#定义Mapper的XML路径
mybatis.mapper-locations=......
#定义别名扫描的包，需要与@Alias联合使用
mybatis.type-aliases-package=......
#MyBatis配置文件，当你的配置比较复杂的时候可以使用它
mybatis.config-location=......
#具体类需要与@MappedJdbcTypes联合使用
mybatis.type-handlers-package=......
#级联延迟加载属性配置
mybatis.configuration.aggressive-lazy-loading=......
#执行器（Executor），可以配置为SIMPLE、REUSE、BATCH，默认为SIMPLE
mybatis.executor-type=......
```



自定义插件

```java
package com.learn.chapter5.plugin;
/**** imports ****/
//定义拦截签名
@Intercepts({
      @Signature(type = StatementHandler.class,
      method = "prepare",
      args = { Connection.class, Integer.class }) })
public class MyPlugin implements Interceptor {
　
   Properties properties = null;
　
   // 拦截方法逻辑
   @Override
   public Object intercept(Invocation invocation) throws Throwable {
      System.out.println("插件拦截方法......");
      return invocation.proceed();
   }
   // 设置插件属性
   @Override
   public void setProperties(Properties properties) {
      this.properties = properties;
   }
}
```



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
   <plugins>
      <plugin interceptor="com.learn.chapter5.plugin.MyPlugin">
         <property name="key1" value="value1" />
         <property name="key2" value="value2" />
         <property name="key3" value="value3" />
      </plugin>
   </plugins>
</configuration>
```



# 第6章 聊聊数据库事务处理
## 6.1 JDBC的数据库事务
## 6.2 Spring声明式事务的使用

### 6.2.1 Spring声明式事务约定
### 6.2.2 注解@Transactional的配置项
### 6.2.3 Spring事务管理器
### 6.2.4 测试数据库事务

## 6.3 隔离级别

### 6.3.1 数据库事务的要素
### 6.3.2 详解隔离级别
## 6.4 传播行为

### 6.4.1 传播行为
### 6.4.2 测试传播行为
### 6.4.3 事务状态
## 6.5 Spring数据库事务实战

### 6.5.1 准确启用Spring数据库事务
### 6.5.2 占用事务时间过长
### 6.5.3 @Transactional自调用失效问题

































































**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**