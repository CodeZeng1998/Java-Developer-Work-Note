# Java开发-面试题-0036-Spring 事务管理方式有哪些

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**





> PS：之前遇见过的一家小公司的面试题，刚开始有点小懵逼，太久没背八股文了，突然有点忘记这块内容了，后续找面试官要了点提示才想起来。自己回答的时候顺便扩展了一些事务失效的情况，以及被延伸到Spring 代理方式相关的内容，因为回答事务失效的情况的时候项目里面一般使用注解 `@Transactional` 比较多，里面会涉及到 `@Transactional` 的实现是基于Spring AOP 实现的，所以当类的自调用的情况会导致事务失效，因为AOP是需要基于代理类来实现对应的功能，所以各位回答这道面试题的时候可以扩展说明这块内容。





Spring 的事务管理方式主要有两种：==**编程式事务管理**和**声明式事务管理**。==这两种方式都有各自的应用场景和优缺点，Spring 强烈推荐使用 **声明式事务管理**，但了解编程式事务管理同样有助于理解事务的工作原理。下面是对这两种事务管理方式的详细说明。

<br/>

### 1. 编程式事务管理

编程式事务管理是指通过手动控制事务的开始、提交和回滚。这种方式更灵活，因为开发者可以完全控制事务的生命周期和行为，通常是通过 Spring 提供的 `TransactionTemplate` 或 `PlatformTransactionManager` 来实现。

<br/>

#### 编程式事务管理的实现方式：

- **TransactionTemplate**：Spring 提供的一个方便的事务管理类，它简化了事务的控制，开发者只需定义事务逻辑，事务的开始、提交和回滚由 `TransactionTemplate` 处理。
- **PlatformTransactionManager**：这是 Spring 中用于管理事务的核心接口，不同的事务管理器实现了这个接口，比如 `DataSourceTransactionManager` 用于 JDBC 事务管理，`JpaTransactionManager` 用于 JPA 事务管理等。通过这个接口，开发者可以手动控制事务。

<br/>

#### 编程式事务管理的优缺点：

- 优点：
  - 灵活，适合需要手动控制事务的复杂场景。
  - 开发者可以精确控制事务边界（如部分提交、回滚）。
- 缺点：
  - 代码侵入性强，事务代码和业务逻辑混在一起，难以维护。
  - 不适合大部分日常开发任务，容易导致代码重复。

<br/>

#### 使用示例：

```java
@Autowired
private TransactionTemplate transactionTemplate;

public void someServiceMethod() {
    transactionTemplate.execute(status -> {
        try {
            // 业务逻辑
            performDatabaseOperation();
            // 根据需要可以手动回滚
            if (someCondition) {
                status.setRollbackOnly();
            }
        } catch (Exception e) {
            status.setRollbackOnly(); // 遇到异常时回滚事务
        }
        return null; // 返回值可用于返回结果
    });
}
```

<br/>

#### 手动使用 `PlatformTransactionManager`：

```java
@Autowired
private PlatformTransactionManager transactionManager;

public void someServiceMethod() {
    TransactionDefinition def = new DefaultTransactionDefinition();
    TransactionStatus status = transactionManager.getTransaction(def);
    try {
        // 业务逻辑
        performDatabaseOperation();
        transactionManager.commit(status); // 手动提交
    } catch (Exception e) {
        transactionManager.rollback(status); // 手动回滚
    }
}
```

<br/>

### 2. 声明式事务管理

声明式事务管理是 Spring 框架推荐的事务管理方式，使用==基于 **AOP** 的机制来实现事务的管理==。开发者只需在配置文件或方法上添加事务注解，事务的开启、提交和回滚将自动进行，业务逻辑与事务管理解耦，代码更简洁。

<br/>

#### 声明式事务管理的实现方式：

- **基于 XML 配置**：较为老旧的方式，通过 XML 配置事务代理。
- **基于注解的配置**：现代 Spring 项目中通常使用 `@Transactional` 注解，这种方式更简洁、高效。通过在方法或类上添加 `@Transactional` 注解来声明该方法或类中的所有方法需要事务管理。

<br/>

#### 声明式事务管理的优缺点：

- 优点：
  - 简单、易用，代码更加简洁。
  - 事务管理与业务逻辑分离，便于维护。
  - 使用注解即可轻松控制事务的传播行为、隔离级别、回滚策略等。
- 缺点：
  - 较难处理非常复杂的事务逻辑，如需要手动控制提交、回滚的情况。
  - 由于事务由 Spring 框架自动管理，某些时候可能会导致开发者对事务控制的细节缺乏理解。

<br/>

#### 使用示例：

```java
@Service
public class SomeService {

    @Transactional
    public void someTransactionalMethod() {
        // 业务逻辑
        performDatabaseOperation();
    }
}
```

<br/>

#### @Transactional 注解的详细配置：

`@Transactional` 注解有多个参数，用来控制事务的具体行为，常用参数包括：

1. **propagation（事务传播行为）**： 控制事务如何传播，常见的传播行为包括：
   - `REQUIRED`（默认值）：如果存在当前事务，则加入当前事务；否则新建一个事务。
   - `REQUIRES_NEW`：总是创建一个新的事务，如果存在当前事务，则挂起当前事务。
   - `SUPPORTS`：支持当前事务，如果存在事务，则加入事务；如果不存在，则以非事务方式执行。
   - `NOT_SUPPORTED`：以非事务方式执行，如果存在当前事务，则挂起事务。
   - `MANDATORY`：必须在事务中执行，如果当前没有事务存在，则抛出异常。
   - `NEVER`：以非事务方式执行，如果当前存在事务，则抛出异常。
   - `NESTED`：如果当前存在事务，则嵌套事务中执行（嵌套事务有独立的提交和回滚点）。
2. **isolation（事务隔离级别）**： 控制事务的隔离级别，决定多个事务之间数据的可见性：
   - `DEFAULT`：使用数据库默认的隔离级别。
   - `READ_UNCOMMITTED`：最低的隔离级别，允许脏读、不可重复读和幻读。
   - `READ_COMMITTED`：允许不可重复读和幻读，但不允许脏读（大多数数据库默认的隔离级别）。
   - `REPEATABLE_READ`：允许幻读，但不允许脏读和不可重复读。
   - `SERIALIZABLE`：最高的隔离级别，事务串行执行，避免任何数据并发问题。
3. **timeout（超时）**： 指定事务的超时时间（秒）。如果超过该时间，事务将回滚。
4. **readOnly（只读事务）**： 如果设置为 `true`，则事务将作为只读执行，旨在优化性能（适用于查询操作）。
5. **rollbackFor / rollbackForClassName**： 指定哪些异常将导致事务回滚。通常 `RuntimeException` 导致回滚，但可以通过该参数指定其他异常。
6. **noRollbackFor / noRollbackForClassName**： 指定哪些异常不会导致事务回滚。

<br/>

#### 使用 `@Transactional` 的例子：

```java
@Transactional(
    propagation = Propagation.REQUIRED, 
    isolation = Isolation.REPEATABLE_READ, 
    timeout = 30, 
    rollbackFor = {SQLException.class}, 
    readOnly = false)
public void someServiceMethod() {
    // 业务逻辑
    performDatabaseOperation();
}
```

<br/>

### 3. Spring 事务管理的底层原理

Spring 的声明式事务管理是基于 AOP 实现的。通过 `@Transactional` 注解，Spring 会在运行时为目标对象生成代理对象，代理对象会拦截方法调用并进行事务管理。

- **JDK 动态代理** 或 **CGLIB** 代理用于代理目标类的方法调用。
- 在代理对象的方法中，Spring 会在方法执行前开启事务，方法执行结束后根据情况决定是提交事务还是回滚事务。

<br/>

### 4. 常见的事务管理器

Spring 提供了多种事务管理器，用于支持不同的数据访问技术：

- **DataSourceTransactionManager**：用于 JDBC 数据源的事务管理。
- **JpaTransactionManager**：用于 JPA 的事务管理。
- **HibernateTransactionManager**：用于 Hibernate 的事务管理。
- **JtaTransactionManager**：用于分布式事务管理。

<br/>

总结来看，Spring 提供了灵活、易用的事务管理机制。声明式事务管理通过 `@Transactional` 注解，使得开发者可以轻松配置事务，而编程式事务管理则在需要精确控制事务边界时表现出更大的灵活性。



<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0036.png?raw=true)

上图是由 Pic 生成的

关键词：SpiderMan

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



