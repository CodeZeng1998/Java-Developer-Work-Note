# Java开发-面试题-0021-什么是 AOP，你们项目中有没有使用到AOP

[TOC]

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **技术公众号：CodeZeng1998**（纯纯技术文）
>
> **生活公众号：好锅**（Life is more than code）
>
> **其他平台：CodeZeng1998**、**好锅**







## AOP 定义



AOP（面向切面编程，Aspect-Oriented Programming）是一种编程范式，用来将横切关注点（cross-cutting concerns）与业务逻辑分离。横切关注点是指那些跨越多个模块的功能，比如日志记录、安全性、事务管理等。在不使用AOP的情况下，这些功能可能会散布在代码的多个地方，导致代码冗余、难以维护。

通过AOP，可以将这些横切关注点集中管理，使得核心业务逻辑更加清晰、简洁。AOP的核心概念包括：

- **切面（Aspect）**：包含横切关注点的模块。
- **连接点（Join Point）**：程序执行的某个点，比如方法调用或异常抛出。
- **通知（Advice）**：在某个连接点执行的代码。
- **切入点（Pointcut）**：定义哪些连接点会被切面拦截。
- **织入（Weaving）**：将切面应用到目标对象的过程。

在Java中，Spring框架广泛使用AOP来处理事务管理、日志记录等功能。

我们项目中可能会用到AOP，比如在方法执行前后进行日志记录或在某些特定操作时进行权限检查。你可以分享你当前项目的具体需求，我可以帮你看看是否适合用AOP来优化代码。



**AOP 可用于：**

* **日志**
* **限流（Redisson + AOP）**
* **权限验证**







## AOP 使用例子



### **@Aspect + @Around**

```java
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class AroundAspect {

    private static final Logger logger = LoggerFactory.getLogger(AroundAspect.class);

    @Around("execution(* com.example.service..*(..))")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        logger.info("环绕通知：方法开始执行...");
        Object result;
        try {
            result = joinPoint.proceed(); // 执行目标方法
        } catch (Throwable ex) {
            logger.error("环绕通知：方法执行异常！", ex);
            throw ex; // 异常继续抛出
        }
        logger.info("环绕通知：方法执行结束。");
        return result;
    }
}

```





### **@Aspect + @Before + @After + @AfterReturning + @AfterThrowing**

```JAVA
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.AfterThrowing;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class GeneralAspect {

    private static final Logger logger = LoggerFactory.getLogger(GeneralAspect.class);

    @Before("execution(* com.example.service..*(..))")
    public void logBefore() {
        logger.info("一般通知：方法开始之前...");
    }

    @After("execution(* com.example.service..*(..))")
    public void logAfter() {
        logger.info("一般通知：方法执行之后...");
    }

    @AfterReturning("execution(* com.example.service..*(..))")
    public void logAfterReturning() {
        logger.info("一般通知：方法成功返回...");
    }

    @AfterThrowing("execution(* com.example.service..*(..))")
    public void logAfterThrowing() {
        logger.error("一般通知：方法抛出异常...");
    }
}

```



成功执行输出：

```
环绕通知：方法开始执行...
一般通知：方法开始之前...
执行任务: 成功任务
一般通知：方法成功返回...
环绕通知：方法执行结束。
一般通知：方法执行之后...
```



失败执行输出:

```
环绕通知：方法开始执行...
一般通知：方法开始之前...
执行任务: error
环绕通知：方法执行异常！
一般通知：方法抛出异常...
一般通知：方法执行之后...
```







## 环绕通知与一般通知的执行顺序

在Spring AOP中，不同类型通知的执行顺序如下：

1. **`@Around`通知**：最先执行。它包裹了目标方法的执行，可以在目标方法之前和之后都执行逻辑。
2. **`@Before`通知**：在目标方法执行前执行。
3. **目标方法**：执行目标方法本身。
4. **`@AfterReturning`通知**：目标方法成功返回后执行。如果目标方法抛出异常，则不会执行此通知。
5. **`@AfterThrowing`通知**：目标方法抛出异常后执行。如果目标方法正常返回，则不会执行此通知。
6. **`@After`通知**：目标方法执行完毕后无论是否抛出异常都会执行。它是最后执行的。

在多个切面中，通知的执行顺序是根据切面的优先级来决定的。可以通过`@Order`注解指定切面的优先级，数值越小，优先级越高。如果不使用`@Order`注解，则切面的顺序是不确定的。



**执行顺序示例**

假设我们使用上面的两个切面类，不指定`@Order`时，不同通知的默认执行顺序为：

1. `AroundAspect`中的`@Around`通知开始。
2. `GeneralAspect`中的`@Before`通知执行。
3. 目标方法执行。
4. 如果成功返回，依次执行：
   - `GeneralAspect`中的`@AfterReturning`通知。
   - `AroundAspect`中的`@Around`通知结束部分。
5. 如果抛出异常，依次执行：
   - `GeneralAspect`中的`@AfterThrowing`通知。
   - `AroundAspect`中的`@Around`通知捕获异常部分。
6. `GeneralAspect`中的`@After`通知执行。



**指定顺序**

你可以通过`@Order`注解指定切面优先级，确保它们按照期望的顺序执行。例如：

```java
@Aspect
@Component
@Order(1)
public class AroundAspect { ... }

@Aspect
@Component
@Order(2)
public class GeneralAspect { ... }
```

在这种情况下，`AroundAspect`中的`@Around`通知会先执行，而`GeneralAspect`中的通知将根据各自类型顺序执行。



## Spring 中的事务是如何实现的



在Spring中，事务管理主要是通过AOP（面向切面编程）机制和代理模式实现的。Spring提供了声明式事务管理和编程式事务管理两种方式，其中声明式事务管理是最常用的方式。

Spring 中的事务其实本质就是使用 AOP 功能，对方法前后进行拦截，在执行方法之前开启事务，在执行完目标方法之后根据执行情况提交或者回滚事务。



1. **声明式事务管理**

声明式事务管理通过注解或XML配置来管理事务，通常使用`@Transactional`注解。Spring会自动为标记了`@Transactional`的方法或类创建事务。

**示例**：

```java
@Service
public class MyService {

    @Transactional
    public void performTransactionalOperation() {
        // 事务性操作，例如数据库增删改查
    }
}
```



2. **事务实现的核心机制**

Spring的事务管理依赖于以下核心机制：



2.1. **AOP和代理**

Spring使用AOP来拦截带有`@Transactional`注解的方法，通过动态代理在方法执行前后管理事务。

- **代理对象**：当Spring扫描到`@Transactional`注解时，会为该类创建一个代理对象。代理对象负责在目标方法执行前启动事务，在执行后提交或回滚事务。
- **连接点拦截**：代理对象在方法调用时拦截连接点，应用事务逻辑。例如，在方法开始时开启事务，在方法成功完成时提交事务，在方法抛出异常时回滚事务。



2.2. **事务管理器（Transaction Manager）**

Spring支持多种事务管理器，如`DataSourceTransactionManager`、`JpaTransactionManager`等。事务管理器负责实际的事务操作，包括启动、提交、回滚等。

- **DataSourceTransactionManager**：用于管理JDBC连接的事务。
- **JpaTransactionManager**：用于管理JPA（如Hibernate）的事务。

Spring会自动选择合适的事务管理器，或者通过配置明确指定。



2.3. **事务传播行为**

Spring支持多种事务传播行为，控制事务在方法嵌套调用时的处理方式。常见的传播行为包括：

- **REQUIRED**（默认）：如果当前没有事务，则开启一个新事务；如果已有事务，则加入该事务。
- **REQUIRES_NEW**：无论当前是否存在事务，总是创建一个新事务，暂停当前事务。
- **SUPPORTS**：如果当前有事务，则加入该事务；如果没有事务，则以非事务方式执行。



2.4. **事务隔离级别**

Spring支持设置事务的隔离级别，控制事务之间的相互影响。常见的隔离级别包括：

- **READ_UNCOMMITTED**：允许读取未提交的数据，可能会出现脏读。
- **READ_COMMITTED**：只能读取已提交的数据，防止脏读。
- **REPEATABLE_READ**：确保同一事务中多次读取的数据一致，防止不可重复读。
- **SERIALIZABLE**：最高级别的隔离，防止脏读、不可重复读和幻读。



3. **事务管理的工作流程**

Spring事务管理的工作流程可以概括如下：

1. Spring AOP拦截带有`@Transactional`注解的方法调用。
2. 代理对象通过事务管理器创建或加入一个事务。
3. 执行目标方法，如果方法抛出异常，根据配置回滚事务。
4. 如果方法执行成功且没有异常，提交事务。
5. 目标方法返回结果或异常。



**4. 总结**

Spring中的事务管理通过AOP、动态代理和事务管理器的组合实现。Spring将事务管理从业务代码中解耦，使得开发者可以通过注解或配置轻松控制事务的创建、提交和回滚，确保数据一致性和完整性。











以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0021.png?raw=true)

上图是由 Pic 生成的

关键词：Cute Ghost Dinosaur

<br/>

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **技术公众号：CodeZeng1998**（纯纯技术文）
>
> **生活公众号：好锅**（Life is more than code）
>
> **其他平台：CodeZeng1998**、**好锅**



