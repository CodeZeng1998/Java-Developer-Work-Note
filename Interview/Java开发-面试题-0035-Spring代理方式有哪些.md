# Java开发-面试题-0035-Spring代理方式有哪些

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

Spring 中的代理方式主要有两种：==**JDK 动态代理**和**CGLIB 代理**==。两者在 Spring ==AOP 和 事务管理== 中都被广泛使用。下面是对这两种代理方式的详细说明：

<br/>

### 1. JDK 动态代理

#### 概念：

JDK 动态代理是基于 Java 的 `java.lang.reflect.Proxy` 类和 `InvocationHandler` ==接口==来实现的。它只针对接口进行代理，因此代理类必须实现某个接口。

<br/>

#### 工作原理：

- JDK 动态代理通过在运行时生成一个代理类，该代理类实现了目标对象所实现的接口。
- 当代理对象调用接口中的方法时，代理类会拦截该方法调用，并将其交由 `InvocationHandler` 处理。
- 在 `InvocationHandler` 中，开发者可以定义代理逻辑，例如方法执行前后增强、权限检查、事务管理等。

<br/>

#### 特点：

- ==只能代理实现了接口的类。==
- 代理是基于接口调用的，因此目标对象必须实现一个或多个接口。
- 代理类是在运行时动态生成的，无需在编译时定义。

<br/>

#### 使用场景：

Spring 会优先选择使用 JDK 动态代理，前提是目标类有实现接口，比如通过 AOP 进行事务控制时，常见的 service 层使用的是接口。

<br/>

#### 示例代码：

```java
public interface Service {
    void perform();
}

public class ServiceImpl implements Service {
    @Override
    public void perform() {
        System.out.println("Performing service operation...");
    }
}

public class ServiceInvocationHandler implements InvocationHandler {
    private final Object target;
    
    public ServiceInvocationHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("Before method execution...");
        Object result = method.invoke(target, args);
        System.out.println("After method execution...");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Service service = new ServiceImpl();
        Service proxy = (Service) Proxy.newProxyInstance(
                service.getClass().getClassLoader(),
                service.getClass().getInterfaces(),
                new ServiceInvocationHandler(service));
        
        proxy.perform();
    }
}
```

<br/>

### 2. CGLIB 代理

#### 概念：

CGLIB（Code Generation Library）代理是基于底层字节码生成的代理技术。它通过扩展（继承）目标类并在运行时生成代理类来实现方法拦截。CGLIB 代理不需要目标对象实现接口，因此它可以代理任何类。

<br/>

#### 工作原理：

- CGLIB 使用 ASM 字节码操作框架在运行时生成目标类的子类，并重写目标类中的方法来实现代理。
- 当代理类调用被代理类的方法时，代理类通过方法拦截器（MethodInterceptor）来增强方法逻辑。

<br/>

#### 特点：

- ==能够代理没有实现接口的类，即可以代理普通的类。==
- 代理是基于子类的，代理类实际上是目标类的子类，因此==不能代理 `final` 类以及 `final` 方法。==
- 性能通常比 JDK 动态代理稍高，但由于生成类字节码的开销，代理类的生成较为复杂。

<br/>

#### 使用场景：

==当目标类没有实现接口时，Spring 会选择使用 CGLIB 代理，例如在纯 POJO 类或者只实现抽象类的情况下，事务管理和 AOP 增强都需要使用 CGLIB 来代理。==

<br/>

#### 示例代码：

```java
public class Service {
    public void perform() {
        System.out.println("Performing service operation...");
    }
}

public class ServiceMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        System.out.println("Before method execution...");
        Object result = proxy.invokeSuper(obj, args);
        System.out.println("After method execution...");
        return result;
    }
}

public class Main {
    public static void main(String[] args) {
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(Service.class);
        enhancer.setCallback(new ServiceMethodInterceptor());
        
        Service proxy = (Service) enhancer.create();
        proxy.perform();
    }
}
```

<br/>

### 3. Spring 中的代理选择机制

Spring 默认优先使用 **JDK 动态代理**，但如果目标对象没有实现任何接口，它会自动切换到 **CGLIB 代理**。具体的选择机制是：

- 如果目标对象实现了接口，Spring 会使用 JDK 动态代理。
- 如果目标对象没有实现接口，Spring 会使用 CGLIB 代理。

可以通过 `@EnableAspectJAutoProxy(proxyTargetClass = true)` 强制 Spring 使用 CGLIB 代理，即使目标类实现了接口。

<br/>

### 4. 代理方式的选择

- **JDK 动态代理**：适合目标对象实现了接口的情况。优点是代理类轻量、生成简单，缺点是只能代理接口。
- **CGLIB 代理**：适合目标对象没有实现接口或希望通过继承的方式进行代理的情况。优点是代理范围广，能够代理任何类，缺点是不能代理 `final` 类或 `final` 方法，生成代理类较复杂。

总结来看，Spring 的代理机制在灵活性和性能上都非常适合于不同的业务场景，而 JDK 动态代理和 CGLIB 各有其适用场景，开发者可以根据需要选择合适的代理方式。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0035.png?raw=true)

上图是由 Pic 生成的

关键词：Iron Man

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



