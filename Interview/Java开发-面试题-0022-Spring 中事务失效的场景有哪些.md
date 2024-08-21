# Java开发-面试题-0022-Spring 中事务失效的场景有哪些



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**







Spring 中事务失效的场景有哪些？

* 非公共方法上使用`@Transactional`
* 自调用（同一类内部方法调用）
* `@Transactional`注解的传播行为设置不当
* 异常不在被捕获范围或异常未抛出
*  **多线程场景**
* **代理模式选择不当**
* 事务管理器配置不正确
* 使用`final`方法











在Spring中，使用`@Transactional`注解进行声明式事务管理时，可能会出现事务失效的情况。常见的事务失效场景如下：

### 1. **非公共方法上使用`@Transactional`**

Spring的AOP机制默认只会拦截公共（`public`）方法。因此，如果你在非公共方法（如`private`、`protected`或`package-private`）上使用`@Transactional`，事务将不会生效。

**示例**：

```java
@Transactional
private void privateMethod() {
    // 事务不会生效
}
```



### 2. **自调用（同一类内部方法调用）**

如果一个类的方法在调用同类的另一个标记了`@Transactional`的方法时，由于Spring AOP是基于代理实现的，内部调用不会经过代理对象，因此事务不会生效。



**原因**： Spring AOP的事务管理是基于代理模式实现的。当你在一个类中调用同一个类中的另一个方法时，如果这个被调用的方法上有`@Transactional`注解，事务不会生效。这是因为自调用是直接通过`this`调用的，而不是通过代理对象调用的，因此不会触发AOP切面。



**原理**： Spring使用代理对象来管理事务。当你调用一个带有`@Transactional`的方法时，实际上是通过代理对象来执行该方法的，代理对象会负责处理事务逻辑（如开启、提交、回滚事务等）。但是，当同一类内部方法调用时，`this`引用是当前对象的实际实例，而不是代理对象，因此不会触发事务。



**示例代码**：

```java
@Service
public class MyService {

    @Transactional
    public void methodA() {
        methodB(); // 直接调用，事务不会生效
    }

    @Transactional
    public void methodB() {
        // 事务代码
    }
}
```



**解决方案**：

1. **将方法拆分到不同的类中**：将`methodB()`方法放到另一个`@Service`类中，通过注入该类来调用。
2. **通过代理对象调用**：使用`ApplicationContext`或`AopContext`获取代理对象，使用代理对象进行方法调用。

**通过代理对象调用示例**：

```java
@Service
public class MyService {

    @Autowired
    private MyService selfProxy;

    @Transactional
    public void methodA() {
        selfProxy.methodB(); // 使用代理对象调用，事务生效
    }

    @Transactional
    public void methodB() {
        // 事务代码
    }
}
```





### 3. **`@Transactional`注解的传播行为设置不当**

Spring支持多种事务传播行为，例如`REQUIRED`（默认）、`REQUIRES_NEW`、`SUPPORTS`等。如果传播行为设置不当，例如使用`SUPPORTS`或`NOT_SUPPORTED`，可能导致事务失效。



**原因**： Spring的事务传播行为（Propagation）定义了一个事务方法是如何与现有事务协作的。默认的传播行为是`REQUIRED`，即如果当前没有事务，则创建一个新事务；如果已经存在事务，则加入该事务。传播行为设置不当可能导致事务失效。



**传播行为类型**：

- **REQUIRED**（默认）：加入当前事务或创建新事务。
- **REQUIRES_NEW**：无论是否存在当前事务，都创建一个新事务，当前事务会被挂起。
- **SUPPORTS**：如果当前有事务，则加入；如果没有事务，则以非事务方式执行。
- **NOT_SUPPORTED**：总是以非事务方式执行，当前事务被挂起。
- **NEVER**：总是以非事务方式执行，如果当前存在事务则抛出异常。
- **MANDATORY**：必须在当前事务中执行，否则抛出异常。
- **NESTED**：在当前事务内嵌套一个子事务。



**传播行为导致事务失效的场景**： 例如，使用`SUPPORTS`时，如果在没有现有事务的情况下调用该方法，事务将不会被启动。

**示例代码**：

```java
@Transactional(propagation = Propagation.SUPPORTS)
public void method() {
    // 如果没有现有事务，事务不会启动，代码以非事务方式执行
}
```

**解决方案**： 确保正确选择传播行为，理解每种传播行为的适用场景和影响。





### 4. **异常不在被捕获范围或异常未抛出**

Spring 默认只对 `RuntimeException` 和 `Error` 进行事务回滚。如果事务方法抛出的是 `checked exception`（如`Exception`），则不会触发回滚。你可以通过在 `@Transactional` 注解中设置 `rollbackFor` 属性来覆盖这一行为。

**示例**：

```java
@Transactional
public void method() throws Exception {
    throw new Exception("Checked exception"); // 不会触发回滚
}
```

解决方法：

```java
@Transactional(rollbackFor = Exception.class)
public void method() throws Exception {
    throw new Exception("Checked exception"); // 现在会触发回滚
}
```



**示例**：

```java
@Transactional(rollbackFor = Exception.class)
public void method() throws Exception {
    try {
        dosomething();
    } catch(Exception e) {
        // 不会触发回滚
    }
}
```

解决方法：

```java
@Transactional(rollbackFor = Exception.class)
public void method() throws Exception {
        dosomething(); // 会触发回滚 
}
```





### 5. **多线程场景**

Spring的事务是基于线程绑定的。如果在多线程环境中启动事务，而事务方法在其他线程中被执行，那么事务不会生效。

**示例**：

```java
@Transactional
public void method() {
    new Thread(() -> {
        // 此处事务不会生效
    }).start();
}
```



### 6. **代理模式选择不当**

Spring的AOP默认使用JDK动态代理（基于接口）或CGLIB代理（基于子类）。如果类没有实现接口，并且强制使用JDK动态代理，事务会失效，因为代理无法工作。

**示例**：

```java
@Configuration
@EnableTransactionManagement(proxyTargetClass = false) // 强制使用JDK动态代理
public class AppConfig {
}
```

如果类没有实现接口并且强制使用JDK代理，事务将失效。



在Spring框架中，AOP（包括事务管理）是通过代理机制实现的。Spring主要有两种代理模式：**JDK动态代理**和**CGLIB代理**。代理模式选择不当可能导致事务失效。让我们深入了解这两种代理机制及其对事务管理的影响。



#### 代理机制概述

1. **JDK动态代理（JDK Dynamic Proxy）**：

   - 适用于==基于接口==的代理。
   - ==如果目标类实现了至少一个接口，Spring默认会使用JDK动态代理。==
   - ==JDK动态代理只能代理接口中的方法，而不能代理没有在接口中声明的方法。==

2. **CGLIB代理（Code Generation Library）**：

   - 适用于==基于子类==的代理。
   - ==如果目标类没有实现任何接口，Spring会使用CGLIB代理。==
   - ==CGLIB通过生成目标类的子类来创建代理对象，因此它可以代理所有非`final`方法。==

   

#### 代理机制与事务管理的关系

Spring事务管理依赖于AOP机制，将事务逻辑织入到目标方法中。AOP的工作方式取决于所使用的代理模式。因此，选择不当的代理模式可能导致事务失效。

##### 关键场景与事务失效的原因：

**场景1：类没有实现接口，但强制使用JDK动态代理**

- 如果类没有实现接口，而Spring配置为强制使用JDK动态代理，那么Spring无法生成该类的代理对象，导致AOP切面无法应用，事务因此失效。

**示例代码**：

```java
@Configuration
@EnableTransactionManagement(proxyTargetClass = false) // 强制使用JDK动态代理
public class AppConfig {
}

@Service
public class MyService {
    @Transactional
    public void process() {
        // 事务不会生效，因为MyService没有实现任何接口
    }
}
```

在这个例子中，由于`MyService`类没有实现接口，而`proxyTargetClass`被设置为`false`（表示强制使用JDK动态代理），Spring无法为`MyService`创建代理对象。因此，`@Transactional`注解的事务管理逻辑不会生效。



**场景2：类实现了接口，但需要代理接口以外的方法**

- JDK动态代理只能代理接口中的方法。如果类实现了接口，但你希望代理接口以外的方法，JDK代理将无法处理这些方法，导致事务逻辑不适用于这些方法。



**场景3：`final`方法或类**

- CGLIB代理通过生成目标类的子类来创建代理对象，因此无法代理`final`方法或`final`类。事务逻辑在这种情况下也不会生效。



#### 如何正确选择代理模式

1. **基于接口开发**：如果你的类实现了接口，并且只需要对接口中的方法进行代理，JDK动态代理是合适的选择。它性能更好，并且是Spring的默认选择。
2. **需要代理非接口方法**：如果需要代理类中不属于接口的方法，或者类没有实现接口，可以使用CGLIB代理。通过将`proxyTargetClass`设置为`true`，强制Spring使用CGLIB代理。

**配置CGLIB代理**：

```java
@Configuration
@EnableTransactionManagement(proxyTargetClass = true) // 强制使用CGLIB代理
public class AppConfig {
}
```

1. **避免使用`final`方法和类**：如果使用CGLIB代理，不要将需要代理的方法或类声明为`final`，否则事务将无法生效。



#### 总结

正确选择代理模式对于确保Spring AOP和事务管理的有效性至关重要。对于接口驱动的开发，JDK动态代理是默认且性能优越的选择。然而，在需要代理非接口方法或没有实现接口的情况下，使用CGLIB代理可以确保事务逻辑生效。通过合理配置Spring AOP代理模式，可以避免事务失效的问题。



### 7. **事务管理器配置不正确**

在使用多个数据源时，如果没有正确配置事务管理器，可能会导致事务失效。每个数据源需要对应的事务管理器，并且需要通过`@Transactional`注解的`transactionManager`属性指定使用哪个事务管理器。



**原因**： 在Spring中，当应用程序使用多个数据源时，每个数据源需要独立的事务管理器。如果没有正确配置事务管理器或没有指定事务管理器，事务可能不会生效，甚至会导致数据不一致。



**示例场景**：

- 应用程序同时使用两个数据源（如`DataSource1`和`DataSource2`），但只配置了一个`DataSourceTransactionManager`。
- `@Transactional`注解没有指定使用哪个事务管理器，导致事务使用错误的数据源或根本没有启用。



**示例代码**：

```java
@Bean
public PlatformTransactionManager transactionManager1() {
    return new DataSourceTransactionManager(dataSource1());
}

@Bean
public PlatformTransactionManager transactionManager2() {
    return new DataSourceTransactionManager(dataSource2());
}
```

```java
@Service
public class MyService {

    @Transactional("transactionManager1")
    public void method1() {
        // 使用transactionManager1管理的事务
    }

    @Transactional("transactionManager2")
    public void method2() {
        // 使用transactionManager2管理的事务
    }
}
```



**解决方案**：

- **正确配置多个事务管理器**：为每个数据源配置不同的事务管理器。
- **指定事务管理器**：在`@Transactional`注解中指定`transactionManager`属性。

**示例**：

```java
@Transactional(transactionManager = "transactionManager1")
public void method1() {
    // 使用指定的事务管理器
}
```

通过理解这些细节，开发者可以有效避免Spring事务失效的问题，确保数据的一致性和应用的正确性。





### 8. **使用`final`方法**

Spring AOP基于代理，无法代理`final`方法。因此，`final`方法上的`@Transactional`注解不会生效。



**背景**： 在Spring中，事务管理依赖于AOP（面向切面编程），而AOP的实现基于代理模式。当一个类的方法被标记为`@Transactional`时，Spring通过代理对象来增强该方法的行为，以实现事务控制。然而，当你使用`final`修饰方法时，可能导致事务失效。



**原因**： Spring AOP有两种代理模式：JDK动态代理和CGLIB代理。

- **JDK动态代理**：只能代理接口中的方法。因此，如果使用JDK动态代理时，这个问题和`final`无关。
- **CGLIB代理**：通过生成目标类的子类来创建代理对象，因此可以代理目标类的所有非`final`方法。然而，`final`方法在Java中是不能被重写的，这意味着CGLIB无法为这些方法生成代理。因此，当你使用`final`修饰一个方法时，Spring无法代理该方法，也就无法应用事务管理。



**具体场景**： 如果你在一个方法上使用了`@Transactional`注解，并且该方法被声明为`final`，Spring无法对这个方法进行事务增强，事务将不会生效。



#### 示例代码

```java
@Service
public class MyService {

    @Transactional
    public final void processTransaction() {
        // 事务不会生效，因为方法是final的
    }
}
```

在上述示例中，由于`processTransaction()`方法被声明为`final`，Spring无法对其进行代理，因此事务管理功能将不会应用于该方法。



#### 为什么使用`final`会导致事务失效？

`final`关键字禁止了方法的重写，而CGLIB代理的本质是通过子类重写目标类的方法来实现代理。如果方法被声明为`final`，子类就无法重写它，自然也无法在方法中添加事务逻辑。



#### 如何避免这个问题？

1. **避免在需要事务管理的方法上使用`final`修饰符**：
   - 如果你需要事务管理，确保方法不是`final`的，这样Spring的代理机制才能正常工作。
2. **使用接口和JDK动态代理**：
   - 如果你的类实现了接口，并且你使用JDK动态代理（默认情况下），`final`方法的问题不会出现，因为JDK动态代理依赖接口而不是子类。

**示例**：

```java
public interface MyServiceInterface {
    void processTransaction();
}

@Service
public class MyService implements MyServiceInterface {

    @Override
    @Transactional
    public void processTransaction() {
        // 事务将生效
    }
}
```



#### 总结

使用`final`方法会导致Spring AOP无法为其生成代理，从而导致事务失效。为避免这种情况，不要将`final`修饰符应用于需要事务管理的方法，并考虑使用接口以利用JDK动态代理的优势。







### 总结

在Spring中，事务失效的原因通常与Spring AOP的工作原理、代理机制、异常处理方式和传播行为有关。理解这些机制并正确配置`@Transactional`注解，可以避免常见的事务失效问题。

















以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0022.png?raw=true)

上图是由 Pic 生成的

关键词：Cute mountain pig

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



