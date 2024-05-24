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

在Spring数据库事务中可以使用编程式事务，也可以使用声明式事务。在大部分的情况下，我们会使用声明式事务，Spring Boot也不推荐我们使用编程式事务，因此本书不再讨论编程式事务。



## 6.1 JDBC的数据库事务

```java
package com.learn.chapter6.service.impl;
/**** imports ****/
@Service
public class JdbcServiceImpl implements JdbcService {
   @Autowired
   private DataSource dataSource;
　
   @Override
   public int insertUser(String userName, String note) {
      Connection conn = null;
      var result = 0;
      try {
         // 获取数据库连接
         conn = dataSource.getConnection();
         // 启用事务
         conn.setAutoCommit(false);
         // 设置隔离级别
         conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
         var sql = "insert into t_user(user_name, note ) values(?, ?)";
         // 运行SQL语句
         var ps = conn.prepareStatement(sql);
         ps.setString(1, userName);
         ps.setString(2, note);
         result = ps.executeUpdate();
         // 提交事务
         conn.commit();
      } catch (Exception e) {
         // 回滚事务
         if (conn != null) {
            try {
               conn.rollback();
            } catch (SQLException e1) {
               e1.printStackTrace();
            }
         }
         e.printStackTrace();
      } finally {
         // 关闭数据库连接
         try {
            if (conn != null && !conn.isClosed()) {
               conn.close();
            }
         } catch (SQLException e) {
            e.printStackTrace();
         }
      }
      return result;
   }
　
}
```



## 6.2 Spring声明式事务的使用

### 6.2.1 Spring声明式事务约定

对于事务，需要通过标注告诉Spring在什么地方启用数据库事务功能。声明式事务是使用注解**@Transactional**进行标注的，这个注解可以标注在类或者方法上，当它标注在类上时，表示这个类所有**公共的(public)**非静态的方法都将启用事务功能。

* @Transactional允许配置很多属性，如事务的隔离级别和传播行为。
* @Transactional还允许配置异常类型，从而确定方法在发生什么异常时回滚事务或者在发生什么异常时不回滚事务等。IoC容器在加载时就会将这些配置信息解析出来，然后把这些信息存到事务定义（TransactionDefinition接口的实现类）里，并且记录哪些类或者方法需要启用事务功能，以及采取什么策略执行事务。在这个过程中，我们需要做的只是给需要事务的类或者方法标注@Transactional和配置其属性而已，并不是很复杂。





* 当Spring启用事务时，就会根据事务定义的配置来设置事务。首先是根据传播行为确定事务的策略，有关传播行为，然后是隔离级别、超时时间、只读等内容的设置，这些事务的设置并不需要开发者完成，而是由Spring事务拦截器根据注解@Transactional配置来完成的。
* Spring通过对注解@Transactional的属性配置来设置数据库事务，接着Spring就会开始调用开发者编写的业务代码。
* 运行业务代码时可能发生异常，也可能不发生异常。在Spring数据库事务约定流程中，Spring会根据是否发生异常采取不同的策略：如果没有发生异常，Spring事务拦截器就会帮助我们提交事务，这一步也并不需要我们进行干预；如果发生异常，就要判断@Transactional的属性配置在该异常下是否满足回滚配置，如果是就回滚事务，如果不是则继续提交事务，这一步也是由事务拦截器完成的。
* 无论发生异常与否，Spring都会释放数据库事务资源，这样就可以保证数据源正常可用了，这也是由Spring事务拦截器完成的。



### 6.2.2 注解@Transactional的配置项

```java
package org.springframework.transaction.annotation;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

import org.springframework.core.annotation.AliasFor;
import org.springframework.transaction.TransactionDefinition;


@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {

	@AliasFor("transactionManager")
	String value() default "";

	@AliasFor("value")
	String transactionManager() default "";

	Propagation propagation() default Propagation.REQUIRED;

	Isolation isolation() default Isolation.DEFAULT;

	int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;

	boolean readOnly() default false;

	Class<? extends Throwable>[] rollbackFor() default {};

	String[] rollbackForClassName() default {};

	Class<? extends Throwable>[] noRollbackFor() default {};

	String[] noRollbackForClassName() default {};

}

```



### 6.2.3 Spring事务管理器

```java
package org.springframework.transaction;

import org.springframework.lang.Nullable;


public interface PlatformTransactionManager extends TransactionManager {

	TransactionStatus getTransaction(@Nullable TransactionDefinition definition)
			throws TransactionException;

	void commit(TransactionStatus status) throws TransactionException;


	void rollback(TransactionStatus status) throws TransactionException;

}
```

Spring做事务管理时，会将这些方法按照约定织入对应的流程中，其中getTransaction()方法的参数是一个事务定义器(TransactionDefinition)，它是依赖于我们配置的@Transactional的配置项生成的，于是通过它就能够设置事务的属性了，而提交和回滚事务则是通过commit()和rollback()方法来执行的。这里需要注意的是事务状态(TransactionStatus)，getTransaction()方法会返回一个事务状态，而commit()方法和rollback()方法的参数是事务状态，在Spring数据库事务机制中，事务状态会影响事务的传播行为的。这里需要注意的是事务状态(TransactionStatus)，getTransaction()方法会返回一个事务状态，而commit()方法和rollback()方法的参数是事务状态，在Spring数据库事务机制中，事务状态会影响事务的传播行为。

在Spring Boot中，添加Maven依赖mybatis-spring-boot-starter之后，会自动创建JdbcTransactionManager对象作为事务管理器。如果依赖spring-boot-starter-data-jpa，则会自动创建JpaTransactionManager对象作为事务管理器。可见事务管理器是Spring Boot自动创建的，我们不需要自己创建它，这就是Spring Boot的主导思想——约定优于配置。

### 6.2.4 测试数据库事务



## 6.3 隔离级别

### 6.3.1 数据库事务的要素

数据库事务具有以下4个基本要素，也就是著名的**ACID**。

* Atomic（原子性）：事务中包含的操作被看作一个整体的业务单元，这个业务单元中的操作要么全部成功，要么全部失败，不会出现部分失败、部分成功的场景。
* Consistency（一致性）：事务在完成时，必须使所有数据都保持一致状态，在数据库中进行的所有修改都基于事务，保证了数据的完整性。
* Isolation（隔离性）：这是我们讨论的核心内容，正如上述，可能多个应用程序线程会同时访问同一数据，这样数据库的同一数据就会在各个不同的事务中被访问，会产生丢失更新。为了压制丢失更新的产生，数据库定义了隔离级别的概念，通过对隔离级别的选择，可以在不同程度上压制丢失更新的发生。因为互联网的应用常常面对高并发的场景，所以隔离性是需要掌握的重点内容。
* Durability（持久性）：事务结束后，所有数据会固化到一个地方，如保存到磁盘当中，即使断电重启后也可以提供给应用程序访问。



### 6.3.2 详解隔离级别

选择隔离级别的时候，既需要考虑数据的一致性，避免脏数据，又要考虑系统性能的问题。

4种隔离级别：

* **未提交读(read uncommitted)**：未提交读是最低的隔离级别，其含义是允许一个事务读取另一个事务没有提交的数据。未提交读是一种危险的隔离级别，所以在实际的开发中应用不广，但是它的优点在于并发能力高，适合那些对数据一致性没有要求而追求高并发的场景，它的最大坏处是可能发生脏读。
* **读写提交(read committed)**：读写提交隔离级别，是指一个事务只能读取另一个事务已经提交的数据，不能读取未提交的数据。
  * 会出现**不可重复读**现象：当一个事务读取一行数据，然后另一个事务修改或删除该行时，当第一个事务尝试再次读取相同行时，它看到不同的值或发现该行不再存在。换句话说，第一个事务无法重复相同的读操作并获得相同的结果，因为数据在之间已被另一个事务更改。
* **可重复读(repeatable read)**：可重复读的目标是克服读写提交中出现的不可重复读的现象，因为在读写提交的时候，可能出现一些值的变化，影响当前事务的运行。
  * 不会出现**不可重复读**现象
  * 会出现**幻读**问题：当一个事务根据某个特定的条件执行查询，然后另一个事务插入或删除符合条件的行时，当第一个事务再次执行相同的查询时，它会发现额外的行（幻行），这些行之前不存在，或者一些先前检索到的行消失了。基本上，这就像在两次读取之间遇到“幽灵”行，因为其他事务的操作，这些行会出现或消失。
  * **不可重复读**针对的是一条纪录，**幻读**针对的是统计值。
* **串行化(serializable)**：串行化是数据库最高的隔离级别，它会要求所有SQL语句都按照顺序运行，这样就可以克服上述隔离级别出现的各种问题，能够完全保证数据的一致性。



| 隔离级别 | 脏读 | 不可重复读 | 幻读 |
| -------- | ---- | ---------- | ---- |
| 读未提交 | ✅    | ✅          | ✅    |
| 读已提交 | ✖    | ✅          | ✅    |
| 可重复读 | ✖    | ✖          | ✅    |
| 串行化   | ✖    | ✖          | ✖    |



在企业的生产实践中，选择隔离级别一般会以读写提交为主，它能够防止脏读，但不能避免不可重复读和幻读。为了克服数据不一致和性能问题，程序开发者还设计了乐观锁，甚至使用其他数据库，例如使用Redis作为数据载体。对于隔离级别，不同的数据库的支持也是不一样的。例如，Oracle只能支持读写提交和串行化，而MySQL则能够支持4种，Oracle默认的隔离级别为读写提交，MySQL默认的隔离级别则是可重复读。



## 6.4 传播行为

### 6.4.1 传播行为

在Spring事务机制中，通过枚举类Propagation对数据库定义了7种传播行为。

```java
package org.springframework.transaction.annotation;
/**** imports ****/
public enum Propagation {
   /**
    * 需要事务，它是默认传播行为。如果当前存在事务，就沿用当前事务；否则新建一个事务运行该方法
    */
   REQUIRED(TransactionDefinition.PROPAGATION_REQUIRED),
　
   /**
    * 支持事务。如果当前存在事务，就沿用当前事务；否则继续采用无事务的方式运行该方法
    */
   SUPPORTS(TransactionDefinition.PROPAGATION_SUPPORTS),
   /**
    * 必须使用事务。如果当前没有事务，则会抛出异常；如果存在当前事务，则沿用当前事务运行该方法
    */
   MANDATORY(TransactionDefinition.PROPAGATION_MANDATORY),
　
   /**
    * 无论当前事务是否存在，都会创建新事务运行该方法，
    * 这样新事务就可以拥有新的锁和隔离级别等特性，与当前事务相互独立
    */
   REQUIRES_NEW(TransactionDefinition.PROPAGATION_REQUIRES_NEW),
　
   /**
    * 不支持事务，当前存在事务时，将挂起事务，运行方法
    */
   NOT_SUPPORTED(TransactionDefinition.PROPAGATION_NOT_SUPPORTED),
　
   /**
    * 不支持事务，如果当前存在事务，则抛出异常，否则继续采用无事务的方式运行该方法
    */
   NEVER(TransactionDefinition.PROPAGATION_NEVER),
　
   /**
    * 在当前方法调用方法时，如果被调用的方法发生异常，
    * 只回滚被调用的方法运行过的SQL语句，而不回滚当前方法的事务
    */
   NESTED(TransactionDefinition.PROPAGATION_NESTED);
　
   private final int value;
　
   Propagation(int value) { this.value = value; }
　
   public int value() { return this.value; }
}
```



### 6.4.2 测试传播行为

在大部分的数据库中，一段SQL语句中可以设置一个标志位，运行后面的SQL语句时如果有问题，只回滚到这个标志位的数据状态，而不会让这个标志位之前的SQL语句也回滚。这个标志位在数据库概念中被称为**保存点(save point)**。从加粗日志部分可以看到，Spring生成了nested事务，也可以看到保存点的释放，可见Spring也是使用保存点技术来完成让子事务回滚而不致使当前事务回滚的工作。注意，并不是所有数据库都支持保存点技术，因此Spring内部有这样的规则：当数据库支持保存点技术时，就启用保存点技术；如果不能支持，就新建一个事务来运行代码，即等价于REQUIRES_NEW传播行为。

NESTED传播行为和REQUIRES_NEW传播行为是有区别的：NESTED传播行为会沿用当前事务，以保存点技术为主；REQUIRES_NEW传播行为则创建新的事务，事务的提交和回滚也是独立的，它拥有独立上下文（例如隔离级别和超时时间等），这是在应用中需要注意的地方。



### 6.4.3 事务状态
## 6.5 Spring数据库事务实战

### 6.5.1 准确启用Spring数据库事务
### 6.5.2 占用事务时间过长
### 6.5.3 @Transactional自调用失效问题

Spring数据库事务约定，其实现原理是AOP，而AOP的原理是动态代理，在自调用的过程中，是类自身调用，而不是代理对象调用，那么就不会启用AOP，Spring也就不能把你的代码织入约定的流程中，于是就出现了现在看到的失败场景。为了克服这个问题，我们可以用一个服务类调用另一个服务类，这样就是代理对象的调用，Spring才会将你的代码织入事务流程。



# 第7章 使用性能利器Redis

Redis是一个开源、使用ANSI C标准编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、键值数据库，运行在内存中，支持多种数据类型的存储，并提供多种语言的API。由于Redis是基于内存的，所以它的运行速度很快，是关系数据库几倍到几十倍的速度。

Redis是一种键值非关系数据库，而且是以字符串类型为中心的，当前它能够支持多种数据类型，包括字符串、哈希、列表（链表）、集合、有序集合、位图、基数、地理位置和流。

## 7.1 spring-data-redis项目简介

### 7.1.1 spring-data-redis项目的设计
### 7.1.2 RedisTemplate和StringRedisTemplate

Redis是一个基于字符串存储的NoSQL数据库，而Java是基于对象的语言，对象是无法存储到Redis中的。为此，Java提供了序列化机制，在Java中只要类实现了java.io.Serializable接口，就表示类对象能够进行序列化，通过将类对象进行序列化就能够得到各类序列化后的字符串，这样Redis就可以将这些类对象以字符串形式进行存储。Java也可以将那些序列化后的字符串通过反序列化转换为Java对象。

### 7.1.3 Spring对Redis数据类型操作的封装

```java
// 获取Redis数据类型操作接口

// 获取字符串操作接口
redisTemplate.opsForValue();
// 获取哈希操作接口
redisTemplate.opsForHash();
// 获取列表（链表）操作接口
redisTemplate.opsForList();
// 获取集合操作接口
redisTemplate.opsForSet();
// 获取有序集合操作接口
redisTemplate.opsForZSet();


// 获取绑定键的操作类

// 获取字符串绑定键操作接口
redisTemplate.boundValueOps("string");
// 获取哈希绑定键操作接口
redisTemplate.boundHashOps("hash");
// 获取列表（链表）绑定键操作接口
redisTemplate.boundListOps("list");
// 获取集合绑定键操作接口
redisTemplate.boundSetOps("set");
// 获取有序集合绑定键操作接口
redisTemplate.boundZSetOps("zset");
```



### 7.1.4 SessionCallback和RedisCallback接口

SessionCallback和RedisCallback接口，它们的作用是让RedisTemplate进行回调，通过这两个接口可以在同一条Redis连接下执行多个命令。SessionCallback提供了良好的封装，对开发者比较友好，因此在实际的开发中应该优先选择使用它。相对而言，RedisCallback接口位于更底层，需要处理的内容也比较多，可读性较差，所以在非必要的时候尽量不选择使用它。

```java
public void useSessionCallback(RedisTemplate redisTemplate) { // SessionCallback接口
   var sessionCallback = new SessionCallback<Object>() {
      @Override
      public Object execute(RedisOperations operations) throws DataAccessException {
         operations.opsForValue().set("key1", "value1");
         operations.opsForHash().put("hash", "field", "hvalue");
         return null;
      }
   };
   redisTemplate.execute(sessionCallback);
}
　
public void useRedisCallback(RedisTemplate redisTemplate) { // RedisCallback接口
   redisTemplate.execute((RedisConnection rc) -> {
      rc.stringCommands().set("key1".getBytes(), "value1".getBytes());
      rc.hashCommands().hSet("hash".getBytes(),
            "field".getBytes(), "hvalue".getBytes());
      return null;
   });
}
```

SessionCallback和RedisCallback接口都能够让RedisTemplate使用同一条Redis连接进行回调，从而可以在同一条Redis连接下执行多个命令，避免RedisTemplate多次获取不同的连接。



## 7.2 在Spring Boot中配置和操作Redis
### 7.2.1 在Spring Boot中配置Redis
### 7.2.2 操作Redis数据类型

Redis不允许集合成员重复，集合在数据结构上是一个哈希表，所以是无序的。对于两个或者以上的集合，Redis还提供了交集、并集和差集的运算。



## 7.3 Redis的一些特殊用法

除了操作数据类型的功能，Redis还支持事务、流水线、发布/订阅和Lua脚本等功能，这些也是Redis常用的功能。在高并发场景中往往需要保证数据的一致性，这时考虑使用Redis事务或者利用Redis运行Lua脚本的原子性来保证数据一致性。在需要大批量执行Redis命令的时候，可以使用流水线来执行命令，这样可以避免网络延时，极大地提升客户端向Redis服务器传送命令的速度。

### 7.3.1 使用Redis事务

Redis是支持一定事务能力的NoSQL数据库，在Redis中使用事务，通常的命令组合是**watch... multi...exec**，也就是要在一个Redis连接中执行多个命令，这时可以考虑使用SessionCallback接口来达到这个目的。watch命令可以监控Redis的一些键。multi命令的作用是开启事务，开启事务后，当前客户端的命令不会马上被执行，而是被存储在一个队列里。例如，这时我们执行一些返回数据的命令，Redis并不会马上执行命令，而是把命令存储在一个队列里。因此，此时调用Redis的命令会返回null，这是初学者容易犯的错误。exec命令的作用是执行事务，它在队列命令执行前会判断被watch监控的Redis的键的数据是否发生过变化（即使赋予与之前相同的值也会被认为变化过），如果它认为发生了变化，那么Redis就会取消事务，否则就会执行事务。Redis在执行事务时，要么全部执行，要么全部不执行，而且不会被其他客户端打断，这样就保证了Redis执行事务时数据的一致性。



Redis的事务执行过程：

* 开始
* watch命令
* multi命令开启事务
* 命令进入队列
* exec命令执行事务
* 监控键值对发生变化（判断watch命令监控的键值对是否发生变化）
  * 是，取消事务
  * 否，执行事务
* 取消对监控的键值对
* 结束



**注意，这就是Redis事务和数据库事务的不同之处，Redis事务先让命令进入队列，所以一开始它并没有检测这个加1命令是否能够成功，只有exec命令执行时才能发现错误，对于出错的命令，Redis只是报出错误，后面的命令依旧被执行，所以key2和key3都存在数据，这就是使用Redis事务需要特别注意的地方。为了克服这个问题，一般我们要在执行Redis事务前严格地检查数据，避免这样的情况发生。**



### 7.3.2 使用Redis流水线

Redis也可以批量执行命令，这便是流水线(pipeline)技术，在很多情况下并不是Redis性能不佳，而是网络传输的速度慢造成瓶颈，使用流水线技术可以在需要执行很多命令时大幅度地提升。



### 7.3.3 使用Redis发布/订阅

### 7.3.4 使用Lua脚本

在Redis中运行Lua脚本具备原子性，且Lua脚本具备更加强大的计算能力，在高并发环境中需要保证数据一致性时，使用Lua脚本方案比使用Redis自身提供的事务更好。

Redis提供了两种运行Lua的方法：一种是直接发送Lua脚本到Redis服务器运行；另一种是先把Lua脚本发送给Redis服务器，Redis服务器对Lua脚本进行缓存，然后返回一个32位的SHA1编码，之后只需要发送SHA1和相关参数给Redis服务器便可以运行了。这里需要解释为什么会存在通过32位编码运行Lua脚本的方法。如果Lua脚本很长，那么就需要通过网络传递脚本给Redis服务器运行，而现实的情况是网络的传递速度往往跟不上Redis的运行速度，因此网络速度就会成为Redis运行的瓶颈。如果只传递32位编码和参数，那么需要通过网络传输的消息就少了许多，这样就可以提高系统的性能。

为了支持Redis的Lua脚本，Spring提供了RedisScript接口，与此同时也提供了一个DefaultRedisScript实现类。

```java
package org.springframework.data.redis.core.script;
　
/**** imports ****/
public interface RedisScript<T> {
　
   String getSha1();
　
   @Nullable
   Class<T> getResultType();
　
   String getScriptAsString();
　
   default boolean returnsRawValue() {
      return getResultType() == null;
   }
　
   // 通过Lua脚本来创建DefaultRedisScript对象
   static <T> RedisScript<T> of(String script) {
      return new DefaultRedisScript<>(script);
   }
　
   // 通过Lua脚本来创建DefaultRedisScript对象，并设置返回类型
   static <T> RedisScript<T> of(String script, Class<T> resultType) {
      Assert.notNull(script, "Script must not be null");
      Assert.notNull(resultType, "ResultType must not be null");
      return new DefaultRedisScript<>(script, resultType);
   }
　
   // 从资源中读取Lua脚本
   static <T> RedisScript<T> of(Resource resource) {
      Assert.notNull(resource, "Resource must not be null");
      DefaultRedisScript<T> script = new DefaultRedisScript<>();
      script.setLocation(resource);
      return script;
   }
　
   // 从资源中读取Lua脚本，并设置返回类型
   static <T> RedisScript<T> of(Resource resource, Class<T> resultType) {
      Assert.notNull(resource, "Resource must not be null");
      Assert.notNull(resultType, "ResultType must not be null");
      DefaultRedisScript<T> script = new DefaultRedisScript<>();
      script.setResultType(resultType);
      script.setLocation(resource);
      return script;
   }
}
```





## 7.4 使用Spring缓存注解操作Redis
### 7.4.1 缓存管理器和缓存的启用
### 7.4.2 开发缓存注解

```java
package com.learn.chapter7.service.impl;
/**** imports ****/
@Service
public class UserServiceImpl implements UserService {
　
   @Autowired
   private UserDao userDao = null;
　
   // 新增用户，最后MyBatis机制会回填id值，取结果id缓存用户
   @Override
   @Transactional
   @CachePut(value ="redisCache", key = "'redis_user_'+#result.id")
   public User insertUser(User user) {
      userDao.insertUser(user);
      return user;
   }
　
   // 获取id，取参数id缓存用户
   @Override
   @Transactional
   @Cacheable(value ="redisCache", key = "'redis_user_'+#id")
   public User getUser(Long id) {
      return userDao.getUser(id);
   }
　
   // 更新数据后，更新缓存，如果condition配置项使结果返回为null，不缓存
   @Override
   @Transactional
   @CachePut(value ="redisCache",
          condition="#result != 'null'", key = "'redis_user_'+#id")
   public User updateUserName(Long id, String userName) {
      // 此处调用getUser()方法，该方法缓存注解失效
      // 所以还会运行SQL语句，将查询到数据库中的最新数据
      var user =this.getUser(id);
      if (user == null) {
         return null;
      }
      user.setUserName(userName);
      userDao.updateUser(user);
      return user;
　
   }
　
   // 命中率低，所以不采用缓存机制
   @Override
   @Transactional
   public List<User> findUsers(String userName, String note) {
      return userDao.findUsers(userName, note);
   }
　
   // 移除缓存
   @Override
   @Transactional
   @CacheEvict(value ="redisCache", key = "'redis_user_'+#id",
         beforeInvocation = false)
   public int deleteUser(Long id) {
      return userDao.deleteUser(id);
   }
}
```

注解@CachePut、@Cacheable和@CacheEvict的含义。

* @CachePut表示将方法返回的结果存储到缓存中。
* @Cacheable表示先通过定义的键从缓存中查询，如果可以查询到数据则返回，否则运行该方法，返回数据，并且将返回的结果存储到缓存中。
* @CacheEvict通过定义的键移除缓存，它有一个Boolean类型的配置项beforeInvocation，表示在运行方法之前或者之后移除缓存。因为其默认值为false，所以默认为在运行方法之后移除缓存。

上述3个缓存中都配置了value ="redisCache"，我们在Spring Boot中配置对应的缓存名称为redisCache（见代码清单7-29）后，值配置项就能够引用到对应的缓存管理器了，而键配置项则是一个SpEL，很多时候可以看到配置为'redis_user_'+#id，其中#id表示参数，由于它是通过参数名称来匹配的，所以要求方法存在一个参数且名称为id，通过这样定义，Spring就会用表达式返回字符串作为键来操作缓存了。除此之外，还可以这样引用参数：如#a[0]或者#p[0]表示第一个参数，#a[1]或者#p[1]表示第二个参数……但是这样引用的可读性较差，所以我们一般不这么写。

有时候我们希望使用返回结果的一些属性缓存数据，如insertUser()方法。在将用户插入数据库前，对应的用户是没有id的，这个id值会在新增后由MyBatis的机制回填，因此我们希望使用返回结果，这样使用#result就表示返回的结果对象了，#result是一个User对象，#result.id的作用是取出它的属性id，这样就可以引用这个由数据库生成的id了。

更新数据需要慎重一些，一般情况下我们不要轻易地相信缓存，因为缓存存在脏读的可能性，在需要更新数据时我们往往考虑先从数据库查询出最新数据，再进行操作，因此updateUserName()方法先调用了getUser()方法。由于上述代码中的getUser()方法标注了注解@Cacheable，所以很多读者可能会认为getUser()方法会从缓存中读取数据，进而受到脏数据的影响。然而，这里的事实是这个注解@Cacheable失效了，也就是说使用updateUserName()方法调用getUser()方法的逻辑并不存在读取缓存的可能，它每次都会运行SQL语句来查询数据库中的最新数据。关于这个缓存注解失效的问题，这里只是提醒读者，更新数据时应该谨慎一些，尽量避免读取缓存数据，因为缓存会存在脏数据的可能。



```properties
# Redis配置
spring.redis.data.port=6379
spring.data.redis.password=abcdefg
spring.data.redis.host=192.168.80.137
spring.data.redis.lettuce.pool.min-idle=5
spring.data.redis.lettuce.pool.max-active=10
spring.data.redis.lettuce.pool.max-idle=10
spring.data.redis.lettuce.pool.max-wait=2s
　
#缓存配置
spring.cache.type=REDIS
spring.cache.cache-names=redisCache
```



### 7.4.3 测试缓存注解
### 7.4.4 缓存注解自调用失效问题

缓存注解自调用失效问题在上述代码中，使用updateUserName()方法调用getUser()方法，7.4.2节说明过这会使得在getUser()方法上的缓存注解失效，为什么会这样呢？其实6.5.3节在讨论@Transactional自调用失效问题时已经探讨过其原理，这是因为**Spring的缓存机制也是基于AOP的，而在Spring中AOP是通过动态代理技术来实现的，updateUserName()方法调用getUser()方法是类内部的自调用，并不存在代理对象的调用，这样便不会出现AOP，也就不会使用标注在getUser()上的缓存注解来获取缓存的值了。要克服缓存注解失效的问题，可以参考6.5.3节，像数据库事务那样用两个服务类(Service)相互调用，或者直接从IoC容器中获取代理对象来操作。**



### 7.4.5 缓存脏数据说明

* 对缓存来说，我们可以给定一个时间让它失效，也就是说在Redis中可以设置超时时间，当缓存超过超时时间后，应用不再能够从缓存中获取数据，而只能从数据库中重新获取最新数据，以保证数据及时刷新。对于那些实时性要求比较高的数据，我们可以把缓存时间设置得短一些，这样就会更加频繁地刷新缓存，但不利的是会增加数据库的压力。对于那些要求不是那么高的数据，则可以把超时时间设置得长一些，这样可以降低数据库的压力。
* 相比于读操作，数据的写操作采取的策略就完全不一样，需要谨慎一些。一般会认为缓存不可信，所以会考虑先从数据库中读取最新数据，再更新数据，以免将缓存的脏数据写入数据库中，导致业务问题的出现。



### 7.4.6 自定义缓存管理器

Spring有两种定制缓存管理器的方法：一种是像代码清单7-23那样通过**配置消除缓存键的前缀和自定义超时时间的属性来定制生成RedisCacheManager；**另一种是不采用Spring Boot生成的方式，而是完全**通过自己的代码创建缓存管理器**，尤其是当需要进行比较多的自定义的时候，更加推荐开发者采用自定义的代码。

```java
// 注入连接工厂，由Spring Boot自动配置生成
@Autowired
private RedisConnectionFactory connectionFactory = null;
　
// 自定义Redis缓存管理器
@Bean(name = "redisCacheManager" )
public RedisCacheManager initRedisCacheManager() {
   //不加锁的Redis写入器
   var writer= RedisCacheWriter.nonLockingRedisCacheWriter(connectionFactory);
   // 启用Redis缓存的默认设置
   var config = RedisCacheConfiguration.defaultCacheConfig()
          // 设置值采用JDK序列化器
         .serializeValuesWith(
                RedisSerializationContext.java().getValueSerializationPair())
         // 设置键采用String序列化器
         .serializeKeysWith(
                RedisSerializationContext.string().getValueSerializationPair())
         // 禁用前缀
         .disableKeyPrefix()
         //设置10 m的键超时时间
         .entryTtl(Duration.ofMinutes(10));
   // 创建缓Redis存管理器
   var redisCacheManager = new RedisCacheManager(writer, config);
   return redisCacheManager;
}
```

上述代码先注入RedisConnectionFactory对象，该对象是由Spring Boot自动生成的。然后在创建Redis缓存管理器对象RedisCacheManager的时候进行如下操作：

* (1)创建不加锁的RedisCacheWriter对象：
* (2)使用RedisCacheConfiguration对RedisCacheWriter对象属性进行配置，上述代码设置了禁用前缀，并且超时时间为10 m；
* (3)使用RedisCacheWriter对象和RedisCacheConfiguration对象来构建RedisCacheManager对象。这样就完成了对Redis缓存管理器的自定义。





# 第8章 文档数据库:--MongoDB

对于那些需要缓存而且经常需要统计、查询和分析的数据，使用Redis这样简单的NoSQL数据库显然就不是那么便捷了，这时另一个NoSQL数据库就派上用场了，它就是本章的主题MongoDB。MongoDB对于需要统计、按条件查询和分析的数据提供了支持，可以说是一个最接近关系数据库的NoSQL数据库。



## 8.1 配置MongoDB

```properties
# MONGODB (MongoProperties)
# MongoDB服务器名称或者IP
spring.data.mongodb.host=
# MongoDB数据库名称
spring.data.mongodb.database=
# MongoDB数据库用户名和密码
spring.data.mongodb.username=
spring.data.mongodb.password=
# MongoDB服务端口
spring.data.mongodb.port=
# MongoDB网格文件数据库名称
spring.data.mongodb.gridfs.database=
# MongoDB网格文件桶名称
spring.data.mongodb.gridfs.bucket=
# MongoDB其他服务器名称，如果当前服务器连接不上就选用这些备用服务器
spring.data.mongodb.additional-hosts=
# MongoDB签名数据库名称
spring.data.mongodb.authentication-database=
# 是否让MongoDB自动生成索引
spring.data.mongodb.auto-index-creation=true
# MongoDB字段命名策略
spring.data.mongodb.field-naming-strategy=
# MongoDB数据库复制集合名称
spring.data.mongodb.replica-set-name=
# 连接MongoDB的URI，将服务上面配置的服务器、用户名、密码和端口等值
spring.data.mongodb.uri=
# 是否启用MongoDB关于JPA规范的编程
spring.data.mongodb.repositories.type=auto
#  将UUID转换为BSON二进制值时使用的标识。
spring.data.mongodb.uuid-representation=java_legacy
```



## 8.2 使用MongoTemplate实例
### 8.2.1 准备MongoDB的文档

```java
package com.learn.chapter8.pojo;
import java.io.Serializable;
import java.util.List;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;
// 标识为MongoDB文档
@Document
public class User implements Serializable {
   private static final long serialVersionUID = -7895435231819517614L;
   // MongoDB文档编号，主键
   @Id
   private Long id;
　
   // 在MongoDB中使用user_name保存属性
   @Field("user_name")
   private String userName = null;
　
   private String note = null;
　
   // 角色列表
   private List<Role> roles = null;
   /**** setters and getters ****/
}
```

User类被标识为@Document，这说明它将作为MongoDB的文档存在。注解@id则将对应的字段设置为主键，因为MongoDB的规范采用下画线分隔，而Java一般采用驼峰式命名，所以这里使用了@Field进行设置，这样属性userName就与MongoDB中的user_name属性对应起来了。上述代码里还有一个角色列表（属性roles），如果只是想保存其引用，可以使用@DBRef标注，这样它只会保存引用信息，而不会保存具体的角色信息。



### 8.2.2 使用MongoTemplate操作文档

```java
package com.learn.chapter8.service.impl;
/**** imports ****/
@Service
public class UserServiceImpl implements UserService {
　
   // 注入MongoTemplate对象
   @Autowired
   private MongoTemplate mongoTmpl = null;
　
   @Override
   public void saveUser(User user) {
      mongoTmpl.save(user);
   }
　
   @Override
   public User getUser(Long id) {
      return mongoTmpl.findById(id, User.class);
      /**
       // 如果只需要获取第一个，也可以采用如下查询方法
       var  criteriaId  = Criteria.where("id").is(id);
       var queryId = Query.query(criteriaId);
       return mongoTmpl.findOne(queryId, User.class);
       */
   }
　
   @Override
   public List<User> findUser(
          String userName, String note, int skip, int limit) {
      // 将用户名和备注设置为模糊查询准则
      var criteria  = Criteria.where("userName").regex(userName)
             .and("note").regex(note);
      // 构建查询条件,并设置分页跳过前skip个，至多返回limit个
      var query = Query.query(criteria).limit(limit).skip(skip);
      // 运行MongoDB的查询并返回结果
      var userList = mongoTmpl.find(query, User.class);
      return userList;
   }
   ......
       
@Override
public UpdateResult updateUser(Long id, String userName, String note) {
   // 确定要更新的对象
   var criteriaId  = Criteria.where("id").is(id);
   var query = Query.query(criteriaId);
   // 定义更新对象
   var update = Update.update("userName", userName);
   update.set("note", note); //设置更新属性
   // 更新第一个文档
   var result = mongoTmpl.updateFirst(query, update, User.class);
   // 更新多个文档
   // var result = mongoTmpl.updateMulti(query, update, User.class);
   // 如果不存在文档，则新建文档，否则只更新文档
   // mongoTmpl.upsert(query, update, User.class);
   return result;
}
    
@Override
public DeleteResult deleteUser(Long id) {
   // 构建id相等的条件
   var  criteriaId  = Criteria.where("id").is(id);
   // 查询对象
   var queryId = Query.query(criteriaId);
   // 删除用户
   var result = mongoTmpl.remove(queryId, User.class);
   return result;
}
}
```

MongoTemplate又有updateFirst()、updateMulti()和upsert()这3个方法，其中updateFirst()方法表示只更新第一个文档，updateMulti()方法表示更新多个满足Query对象限定的文档，upsert()方法表示如果不存在文档，则新建文档，否则只更新文档。



## 8.3 使用JPA

### 8.3.1 基本用法

```java
package com.learn.chapter8.repository;
/**** imports ****/
// 标识为JPA接口
@Repository
// 扩展MongoRepository接口
public interface UserRepository extends MongoRepository<User, Long> {
   /**
    * 符合JPA规范命名方法，则不需要再实现该方法也可用
    * 意在对满足条件的文档按照用户名进行模糊查询
    * @param userName -- 用户名
    * @return 满足条件的用户信息
    */
   List<User> findByUserNameLike(String userName);
}
```

这个接口先使用@Repository进行标识，表示这是一个JPA接口，而接口扩展了MongoRepository接口，MongoRepository接口指定了两个泛型：

* 一个是实体类型，这个实体类型要求标注@Document
* 另一个是其主键的类型，这个类型要求标注@Id，这里指定为User类和Long。首先该类标注了@Document，其次该类的id属性上标注了注解@Id，说明它是文档的主键，且类型为Long。findByUserNameLike()方法是一个符合JPA命名方式的接口方法，表示对用户名进行模糊查询。

```java
package com.learn.chapter8.main;
/**** imports ****/
// 指定扫描的包
@SpringBootApplication(scanBasePackages = "com.learn.chapter8")
// 指定扫描的包，用于扫描扩展了MongoRepository的接口
@EnableMongoRepositories(basePackages="com.learn.chapter8.repository")
public class Chapter8Application {
　
   public static void main(String[] args) {
      SpringApplication.run(Chapter8Application.class, args);
   }
}
```

上述代码定义了注解@EnableMongoRepositories，并且通过basePackages配置项指定了JPA接口所在的包，这样Spring就能够将UserRepository接口扫描为对应的Bean并装配到IoC容器中。



### 8.3.2 使用自定义查询

```java
/**
 * 使用id和userName查询
 * 注解@Query中的阿拉伯数字指定参数的下标，以0开始
 * @param id -- 编号
 * @param userName
 * @return 用户信息
 */
@Query("{'id': ?0, 'userName' : ?1}")
User find(Long id, String userName);
```

Spring提供了新的约定，在Spring中只要定义一个名称为“接口名称+Impl”的类并且提供与接口定义相同的方法，Spring就会自动找到这个类对应的方法来作为JPA接口定义方法的实现。

```java
package com.learn.chapter8.repository.impl;
　
/****imports ****/
// 定义为数据访问层
@Repository
// 注意这里的类名称，由于接口名称为UserRepository，按要求定义，类名称则为“接口名称+Impl”
// JPA会自动找到这个类对应的方法，作为JPA接口定义方法的实现
public class UserRepositoryImpl {
   @Autowired// 注入MongoTemplate
   private MongoTemplate mongoTmpl = null;
　
   // 注意，方法名称与接口定义也需要保持一致
   public User findUserByIdOrUserName(Long id, String userName) {
      // 构造id查询准则
      var criteriaId = Criteria.where("id").is(id);
      // 构造用户名查询准则
      var criteriaUserName = Criteria.where("userName").is(userName);
      var criteria = new Criteria();
      // 使用$or操作符关联两个条件，形成或关系
      criteria.orOperator(criteriaId, criteriaUserName);
      var query = Query.query(criteria);
      // 执行查询，返回结果
      return mongoTmpl.findOne(query, User.class);
   }
}
```

# 第9章 初识Spring MVC

Spring MVC一开始就定位为一个较为松散的组合，展示给用户的**视图(View)**、控制器返回的**数据模型(Model)**、定位视图的**视图解析器(ViewResolver)**和**处理器适配器(HandlerAdapter)**等内容都是相对独立的。

## 9.1 Spring MVC框架的设计

## 9.2 Spring MVC流程

ChatGPT:

Spring MVC（Model-View-Controller）框架在Java中设计用于简化Web应用程序的开发。以下是其工作流程的基本概述：

1. **请求处理**：当客户端发送请求时，它被**DispatcherServlet**拦截。此Servlet充当前端控制器，并根据URL映射将请求委派给适当的处理程序。
2. **DispatcherServlet**：它接收所有传入的请求，然后将它们分派到适当的控制器以进行处理。它还处理异常、区域设置、主题解析和其他与MVC相关的任务。
3. **处理程序映射**：此组件根据应用程序配置文件中配置的URL模式，将传入的请求映射到适当的处理程序（控制器）。
4. **控制器**：一旦DispatcherServlet确定要使用的控制器，它就会调用适当的控制器方法来处理请求。控制器负责处理用户输入、准备模型并选择视图。
5. **模型**：控制器准备数据（模型），该数据将由视图生成输出。这可能涉及查询数据库、调用业务逻辑或与其他服务交互。
6. **视图解析**：在控制器处理请求并填充模型之后，DispatcherServlet根据控制器返回的逻辑视图名称确定适当的视图。
7. **视图渲染**：最后，选择的视图使用模型中的数据进行渲染。渲染的输出然后作为响应发送回客户端。







* 注解@Controller表明这是一个控制器，@RequestMapping配置请求路径和控制器（或其方法）的映射关系。在Web服务器启动Spring MVC时，映射关系就会被扫描到**处理器映射(HandlerMapping)**机制中存储，之后在用户发起请求被**DispatcherServlet**拦截后，根据URI和其他的条件，通过**HandlerMapping**机制就能找到对应的控制器（或其方法）进行响应。但是，通过HandlerMapping机制返回的是一个HandlerExecutionChain对象。
* HandlerExecutionChain对象包含一个处理器(handler)，处理器会封装好控制器方法。处理器还会处理请求参数的转换，并且在控制器方法返回后，处理器也可以对返回值进行进一步的处理。从这段描述可以看出，处理器包含控制器方法的逻辑，并且增强了控制器的功能。此外，HandlerExecutionChain包含处理器的拦截器(HandlerInterceptor)，这样就能够通过拦截处理器来增强功能了。
* 得到了处理器，还需要运行，但是我们有普通的HTTP请求或者别的请求（如WebSocket的请求），还需要一个处理器适配器来运行HandlerExecutionChain对象，这就是适配器机制，在Spring MVC中是通过HandlerAdapter接口定义的。



**SpringMVC 运行流程：**

* **HTTP请求**
* **被DispatchServlet拦截**
* **在HandlerMapping（处理器映射：定位到控制器及其方法）（由注解@RequestMapping提供URI和其他配置）--> HandlerExecutionChain（处理器执行链：包含处理器及其拦截器）**
* **HttpRequestHandlerAdapter --> 运行处理器，返回结果**
* **ModelAndView（视图名称、数据模型）**
* **DispatchServlet**
* **视图解析器：定位视图**
* **View（将用户模型渲染展示）**

**在我们加入@ResponseBody将返回结果转变为JSON数据集时，没有经过视图解析器和视图渲染。**



## 9.3 定制Spring MVC的初始化
## 9.4 Spring MVC实例

Spring MVC的开发核心是控制器开发，分为以下几步：

* (1)定义请求分发，让Spring MVC能够产生HandlerMapping；
* (2)接收请求并获取参数；
* (3)处理业务逻辑并获取数据模型；
* (4)绑定视图和数据模型并返回。

定位视图和将数据模型渲染到视图中，则不属于控制器开发的步骤。



### 9.4.1 开发控制器
### 9.4.2 视图和视图渲染



# 第10章 深入Spring MVC开发
## 10.1 处理器映射

如果Web工程使用了Spring MVC，那么它在启动阶段就会将注解@RequestMapping配置的内容保存到处理器映射(HandlerMapping)机制中，然后等待请求的到来，通过拦截请求信息与HandlerMapping进行匹配，找到对应的处理器（处理器包装了控制器），并将处理器及其拦截器保存到HandlerExecutionChain对象中，返回给DispatcherServlet，这样DispatcherServlet就可以运行它们了。可以看到，HandlerMapping的主要任务是将请求定位到具体的处理器上。

* **@RequestMapping**
* **@PostMapping**
* **@GetMapping**
* **@PutMapping**
* **@PatchMapping**
* **@DeleteMapping**



## 10.2 获取控制器参数

### 10.2.1 在无注解的情况下获取参数

在无注解的情况下，Spring MVC也可以获取参数，且允许参数为空，唯一的要求是**参数名称和HTTP参数名称一致**



### 10.2.2 使用@RequestParam获取参数

* @RequestParam
* **默认的情况下@RequestParam标注的参数是不能为空的，如果允许参数为空，可以配置其属性required为false**



### 10.2.3 传递数组

Spring MVC内部已经支持用逗号分隔的数组参数



### 10.2.4 传递JSON数据集

* @RequestBody



### 10.2.5 通过URL传递参数

有一些网站使用了REST风格，这时往往通过URL传递参数。

Spring MVC对此也提供了良好的支持，可以通过处理器映射和注解@PathVariable的组合来获取URL参数。先通过处理器映射定位参数的位置和名称，然后通过@PathVariable依靠URL定制的参数名称来获取参数

* @PathVariable

```java
// {......}表示占位符，还可以配置参数名称
@GetMapping("/{id}")
// 响应为JSON数据集
@ResponseBody
// @PathVariable通过参数名称来获取参数
public User get(@PathVariable("id") Long id) {
   return userService.getUser(id);
}
```



### 10.2.6 获取格式化参数

* @DateTimeFormat是针对日期进行格式化的
* @NumberFormat则是针对数字类型进行格式化的

```java
// 映射页面
@GetMapping("/format/form")
public String showFormat() {
   return "/format/formatter";
}
　
// 获取提交参数
@PostMapping("/format/commit")
@ResponseBody
public Map<String, Object> format(
       @DateTimeFormat(iso=ISO.DATE) Date date,
       @NumberFormat(pattern = "#,###.##") Double number) {
   var dataMap = new HashMap<String, Object>();
   dataMap.put("date", date);
   dataMap.put("number", number);
   return dataMap;
}
```

在Spring Boot中，日期和时间参数的格式化也可以不使用@DateTimeFormat，而只在配置文件application.properties中加入如下配置项：

```properties
# 日期格式化
spring.mvc.format.date=yyyy-MM-dd
# 时间格式化
spring.mvc.format.date-time=yyyy-MM-dd HH:mm:ss
```



## 10.3 自定义参数转换规则

### 10.3.1 处理器转换参数逻辑

```java
package org.springframework.http.converter;
/**** imports ****/
public interface HttpMessageConverter<T> {
   // 是否可读，其中clazz为Java类型，mediaType为HTTP请求类型
   boolean canRead(Class<?> clazz, MediaType mediaType);
　
   // 判断clazz类型是否能够转换为媒体类型（mediaType）
   // 其中clazz为java类型，mediaType为HTTP响应类型
   boolean canWrite(Class<?> clazz, MediaType mediaType);
　
   // 可支持的媒体类型列表
   List<MediaType> getSupportedMediaTypes();
　
   // 在canRead()方法验证通过后，读入HTTP请求信息
   T read(Class<? extends T> clazz, HttpInputMessage inputMessage)
         throws IOException, HttpMessageNotReadableException;
　
   // 在canWrite()方法验证通过后，写入响应
   void write(T t, MediaType contentType, HttpOutputMessage outputMessage)
         throws IOException, HttpMessageNotWritableException;
}
```



控制器的参数类型是处理器通过Converter、Formatter、GenericConverter、Printer和Parser这5个接口转换出来的。其中，Converter、Formatter和GenericConverter的不同之处如下。

* Converter：普通的转换器，例如，有一个Integer类型的控制器参数，而HTTP请求发过来的是字符串，此时Converter就会将字符串转换为Integer类型。
* Formatter：格式化转换器，类似日期和货币等字符串就是通过它按照约定的格式转换出来的。
* GenericConverter：数组转换器，可将HTTP参数转换为数组或者集合类型。



**Spring Boot的自动注册机制**

Spring Boot还提供了管理这些转换器的特殊机制。Spring Boot的自动配置类WebMvcAutoConfiguration定义了一个内部类WebMvcAutoConfigurationAdapter，它的configureMessageConverters()和addFormatters()方法。

```java
// 注册HttpMessageConverter对象，实现请求体到HTTP参数的转换
@Override
public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
   this.messageConvertersProvider
      .ifAvailable((customConverters) ->
         converters.addAll(customConverters.getConverters()));
}
　
......
　
//注册数据转换器，可注册的类型为Converter、Formatter、GenericConverter、Printer和Parser
@Override
public void addFormatters(FormatterRegistry registry) {
   /**
   深入读这个方法的源码，可以看到它会自动将IoC容器中实现了Converter、Formatter、GenericConverter、
   Printer和Parser接口的Bean都注册到注册机中
   */
   ApplicationConversionService.addBeans(registry, this.beanFactory);
}
```

configureMessageConverters()方法可以帮助开发者自定义对请求体的处理。注意，addFormatters()方法比较简单，它的作用是在类型转换的注册机中注册数据转换器，在Spring Boot启动Spring MVC的过程中，将Converter、Formatter、GenericConverter、Printer和Parser这5个接口的实现类所创建的Bean自动地注册到类型转换的注册机中。开发者只需要自定义Converter、Formatter和GenericConverter的接口的Bean，Spring Boot就会**自动**地将它们注册到ConversionService对象中。

### 10.3.2 一对一转换器
### 10.3.3 GenericConverter集合和数组转换





## 10.4 数据验证

### 10.4.1 JSR-303验证

```java
package com.learn.chapter10.pojo;
/**** imports ****/
public class ValidatorPojo {
   // 非空判断
   @NotNull(message ="id不能为空")
   private Long id;
　
   @Future(message = "需要一个将来的日期") // 只能是将来的日期
   // @Past // 只能是过去的日期
   @DateTimeFormat(pattern = "yyyy-MM-dd") // 日期格式化转换
   @NotNull // 不能为空
   private Date date;
　
   @NotNull // 不能为空
   @DecimalMin(value = "0.1") // 最小值为0.1元
   @DecimalMax(value = "10000.00") // 最大值为10,000元
   private Double doubleValue = null;
　
   @Min(value = 1, message = "最小值为1") // 最小值为1
   @Max(value = 88, message = "最大值为88") // 最大值为88
   @NotNull // 不能为空
   private Integer integer;
　
   @Range(min = 1, max = 888, message = "范围为1～888") // 限定范围
   private Long range;
　
   // 邮箱验证
   @Email(message = "邮箱格式错误")
   private String email;
　
   @Size(min = 20, max = 30, message = "字符串长度要求为20～30")
   private String size;
   /**** setters and getters ****/
}
```



### 10.4.2 参数验证机制

为了能够更加灵活地进行验证，Spring还提供自己的参数验证机制。在进行参数转换时，Spring MVC使用WebDataBinder机制进行管理，在默认的情况下Spring会自动地根据上下文通过注册的转换器转换出控制器所需的参数。在WebDataBinder中除了可以注册转换器，还允许注册验证器(Validator)。

在Spring控制器中，还允许使用注解@InitBinder，这个注解的作用是允许在进入控制器方法前修改WebDataBinder机制。

```java
package org.springframework.validation;
/****imports ****/
public interface Validator {
   /**
    *  判定当前验证器是否支持该Class类型的验证
    * @param clazz --POJO类型
    * @return 当前验证器是否支持该POJO验证
    */
   boolean supports(Class<?> clazz);
　
   /**
    *  如果supports返回true，则这个方法执行验证逻辑
    * @param target 被验证POJO
    * @param errors 错误对象
    */
   void validate(Object target, Errors errors);
}
```

 **自定义用户验证器**

```java
package com.learn.chapter10.validator;
/**** imports ****/
public class UserValidator implements Validator {
   // 该验证器只支持User类验证
   @Override
   public boolean supports(Class<?> clazz) {
      return clazz.equals(User.class);
   }
　
   // 验证逻辑
   @Override
   public void validate(Object target, Errors errors) {
      // 对象为空
      if (target == null) {
         // 直接在参数处报错，这样就不能进入控制器方法
         errors.rejectValue("", null, "用户不能为空");
         return;
      }
      // 强制转换
      var user = (User) target;
      // 用户名非空串
      if (!StringUtils.hasText(user.getUserName())) {
         // 增加错误，可以进入控制器方法
         errors.rejectValue("userName", null, "用户名不能为空");
      }
   }
}
```

虽然定义了这个验证器，但Spring不会自动启用它，因为还没有将它与WebDataBinder机制进行绑定。Spring MVC提供了一个注解@InitBinder，在运行控制器方法前，处理器会先运行被@InitBinder标注的方法。这时可以将WebDataBinder对象作为参数传递到控制器方法中，通过这层关系得到WebDataBinder对象，这个对象有一个setValidator()方法，它可以绑定自定义的验证器，这样就可以在获取参数之后，通过自定义的验证器来验证参数。WebDataBinder除了可以绑定验证器，还可以进行参数的自定义，例如，不使用@DateTimeFormat获取日期参数。

**绑定验证器**

```java
/**
 * 调用控制器前先运行这个方法
 * @param binder
 */
@InitBinder
public void initBinder(WebDataBinder binder) {
   // 添加验证器
   binder.addValidators(new UserValidator());
   // 定义日期参数格式，参数不再需要使用注解@DateTimeFormat，boolean参数表示是否允许为空
   binder.registerCustomEditor(Date.class,
         new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false));
}
　
/**
 *
 * @param user -- 用户对象用StringToUserConverter转换
 * @param Errors --验证器返回的错误
 * @param date -- 因为WebDataBinder已经绑定了格式，所以不再需要注解
 * @return 各类数据
 */
@GetMapping("/validator")
@ResponseBody
public Map<String, Object> validator(@Valid User user,
                               Errors Errors, Date date) {
   var map = new HashMap<String, Object>();
   map.put("user", user);
   map.put("date", DateFormat.getDateInstance().format(date));
   // 判断是否存在错误
   if (Errors.hasErrors()) {
      // 获取全部错误
      var oes = Errors.getAllErrors();
      for (var oe : oes) {
         // 判定是否字段错误
         if (oe instanceof FieldError fieldError) {
            map.put(fieldError.getField(), fieldError.getDefaultMessage());
         } else {
            // 对象错误
            map.put(oe.getObjectName(), oe.getDefaultMessage());
         }
      }
   }
   return map;
}
```

上述代码中的initBinder()方法因为标注了注解@InitBinder，所以会在控制器方法前被运行，并且将WebDataBinder对象传递进去。initBinder()方法里添加了自定义的验证器UserValidator，而且设置了日期参数格式，所以在控制器方法中不再需要使用注解@DateTimeFormat定义日期格式化。通过这样的自定义，在使用注解@Valid标注User参数后，Spring MVC就会遍历对应的验证器，当遍历到UserValidator时，会运行它的supports()方法。因为该方法会返回true，所以Spring MVC会用这个验证器来验证User类的数据。initBinder()方法对于日期类型也指定了对应的格式，这样控制器的Date类型的参数也不需要再使用注解的协作。我们还要关注一下控制器方法中的Errors参数。它是Spring MVC通过验证器验证后得到的错误信息，由Spring MVC执行验证规则后进行传递。控制器方法首先判断是否存在错误，如果存在错误，则遍历错误，然后将错误信息放入Map中返回。因为控制器方法标注了@ResponseBody，所以最后会将方法返回的结果转化为JSON数据集响应请求。



## 10.5 数据模型

## 10.6 视图和视图解析器

### 10.6.1 视图设计
### 10.6.2 视图实例--导出Excel文档
## 10.7 文件上传

### 10.7.1 文件上传的配置项
### 10.7.2 开发文件上传功能

## 10.8 拦截器

### 10.8.1 设计拦截器
### 10.8.2 开发拦截器
### 10.8.3 多个拦截器方法的运行顺序
## 10.9 国际化

### 10.9.1 国际化消息源
### 10.9.2 国际化解析器
### 10.9.3 国际化实例--SessionLocaleResolver

## 10.10 Spring MVC拾遗

### 10.10.1 @ResponseBody转换为JSON的秘密
### 10.10.2 重定向
### 10.10.3 操作会话属性
### 10.10.4 给控制器增加通知
### 10.10.5 获取请求头参数

# 第11章 构建REST风格网站
## 11.1 REST简述

### 11.1.1 REST名词解释
### 11.1.2 HTTP的动作
### 11.1.3 REST风格的一些误区
## 11.2 使用Spring MVC开发REST风格端点

### 11.2.1 Spring MVC整合REST
### 11.2.2 使用Spring开发REST风格的端点
### 11.2.3 使用@RestController

### 11.2.4 渲染结果
### 11.2.5 处理HTTP状态码、响应头和异常
## 11.3 客户端请求RestTemplate

### 11.3.1 使用RestTemplate请求后端
### 11.3.2 获取状态码和响应头
### 11.3.3 定制请求体和响应类型

# 第12章 安全--Spring Security
## 12.1 概述和简单安全验证

### 12.1.1 使用用户密码登录系统
### 12.1.2 Spring Security的配置项
### 12.1.3 开发Spring Security的主要的类
## 12.2 使用UserDetailsService接囗定制用户信息

### 12.2.1 使用内存保存用户信息
### 12.2.2 从数据库中读取用户信息
### 12.2.3 使用自定义UserDetailsService对象

### 12.2.4 密码编码器
## 12.3 限制请求

### 12.3.1 配置请求路径访问权限
### 12.3.2 自定义验证方法
### 12.3.3 不拦截的请求
### 12.3.4 防止跨站点请求伪造
## 12.4 登录和登出设置

### 12.4.1 自定义登录页面
### 12.4.2 启用HTTP Basic验证

### 12.4.3 登出配置

# 第13章 学点Spring其他的技术
## 13.1 异步线程池

### 13.1.1 定义线程池和开启异步可用
### 13.1.2 异步实例
## 13.2 异步消息-I-RabbitMo
## 13.3 定时任务

# 第14章 实践-下--抢购商品
## 14.1 设计与开发

### 14.1.1 数据库表设计
### 14.1.2 使用MyBatis开发持久层
### 14.1.3 使用Spring开发业务层和控制层
### 14.1.4 测试和配置
## 14.2 高并发开发

### 14.2.1 超发现象
### 14.2.2 悲观锁

### 14.2.3 乐观锁

# 第15章 打包、测试、监控、预先编译和容器部署
## 15.1 打包和运行

### 15.1.1 打包项目
### 15.1.2 运行项目
### 15.1.3 热部署
## 15.2 测试

### 15.2.1 构建测试类
### 15.2.2 使用随机端口和REST风格测试

### 15.2.3 Mock测试
## 15.3 Actuator监控端点
## 15.4 HTTP监控

### 15.4.1 查看敏感信息
### 15.4.2 shutdown端点
### 15.4.3 配置端点
### 15.4.4 自定义端点
### 15.4.5 健康指标项

## 15.5 JMX监控
## 15.6 预先编译

### 15.6.1 搭建GraalVM环境
### 15.6.2 创建项目
### 15.6.3 生成和运行原生文件
## 15.7 部署到Docker容器中

# 第16章 Spring Cloud Alibaba微服务开发
## 16.1 服务治理--Alibaba Nacos

## 16.1.1 下载、安装、配置和启动Nacos

### 16.1.2 服务发现
### 16.1.3 搭建Nacos集群
## 16.2 服务调用

### 16.2.1 客户端负载均衡
### 16.2.2 OpenFeign声明式服务调用
## 16.3 容错机制--Spring Cloud Alibaba Sentinel

### 16.3.1 设置埋点
### 16.3.2 Sentinel控制台
### 16.3.3 流控
### 16.3.4 熔断

### 16.3.5 在OpenFeign中使用Sentinel
## 16.4 API网关--Spring Cloud Gateway

### 16.4.1 Gateway的工作原理
### 16.4.2 配置路由规则
### 16.4.3 过滤器
### 16.4.4 使用Sentinel管控Gateway

















































**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**