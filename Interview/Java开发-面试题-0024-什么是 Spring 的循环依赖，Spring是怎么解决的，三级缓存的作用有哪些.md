# Java开发-面试题-0024-什么是 Spring 的循环依赖，Spring是怎么解决的，三级缓存的作用有哪些

[TOC]

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



## 1.什么是 Spring 的循环依赖



```java
@Component
public class A {
    @Autowired
    private B b; 
}

@Component
public class B {
    @Autowired
    private A a; 
}
```

循环依赖：

* 在创建A对象的同时需要使用的B对象，在创建B对象的同时需要使用到A对象
* A需要B，B需要C，C需要A
* A需要A



在Spring中，Bean的创建过程主要分为三个步骤：

1. **实例化（Instantiation）**：创建Bean的实例。
2. **依赖注入（Dependency Injection）**：为Bean的属性注入依赖。
3. **初始化（Initialization）**：调用`@PostConstruct`等初始化方法。

如果没有三级缓存，Spring容器在实例化`A`时，会发现`A`依赖`B`，于是先去实例化`B`。但在实例化`B`的过程中，又会发现`B`依赖`A`，于是再次去实例化`A`，这时就会进入一个死循环。



## 2.Spring是怎么解决的循环依赖的

三级缓存解决循环依赖：

Spring 解决循环依赖是通过三级缓存，对应的三级缓存如下所示：

```java
public class DefaultSingletonBeanRegistry extends SimpleAliasRegistry implements SingletonBeanRegistry {
	// 一级缓存
	/** Cache of singleton objects: bean name to bean instance. */
	private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

    // 三级缓存
	/** Cache of singleton factories: bean name to ObjectFactory. */
	private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);

    // 二级缓存
	/** Cache of early singleton objects: bean name to bean instance. */
	private final Map<String, Object> earlySingletonObjects = new HashMap<>(16);
    
    ...
    ...
    ...
}
```



| 缓存名称 | 源码名称              | 作用                                                         |
| -------- | --------------------- | ------------------------------------------------------------ |
| 一级缓存 | singletonObjects      | 单例池，缓存已经经历了完整的生命周期，已经初始化完成的 bean 对象，限制bean在beanFactory 中只存一份，即实现singleton scope |
| 二级缓存 | earlySingletonObjects | 缓存早期的 bean 对象（生命周期还没走完）                     |
| 三级缓存 | singletonFactories    | 缓存的是 ObjectFactory，表示对象工厂，用来创建某个对象       |



**Spring 中的 bean 的生命周期**：

构造函数

-> 依赖注入

-> Aware 接口

-> BeanPostProcessor#before

-> 初始化方法

-> BeanPostProcessor#after

-> 使用 bean

-> 销毁 bean





**Spring 的三级缓存机制是其解决循环依赖的原理：**



1.**三级缓存的结构**

Spring中用于管理单例Bean的缓存有三级：

- **一级缓存（singletonObjects）**：这是一个`Map<String, Object>`，存储已经完全初始化好的单例Bean，供外界直接使用。
- **二级缓存（earlySingletonObjects）**：这是一个`Map<String, Object>`，存储已经实例化但尚未完成属性注入的单例Bean。主要用于解决Bean的代理问题。
- **三级缓存（singletonFactories）**：这是一个`Map<String, ObjectFactory<?>>`，存储可以生成早期单例Bean的工厂对象。工厂用于生成还未进行依赖注入的Bean实例，并允许这些实例提前暴露。




2. **循环依赖的场景分析**

假设我们有两个互相依赖的类`A`和`B`：

```java
@Component
public class A {
    @Autowired
    private B b; 
}

@Component
public class B {
    @Autowired
    private A a; 
}
```



3.**当Spring容器启动时，会按照以下步骤创建和管理这些Bean**

**三级缓存的具体执行流程：**

* **第一步：创建`A`实例**

  - Spring容器检测到需要创建Bean `A`，并开始实例化`A`。

  - `A`的构造方法执行，Bean的实例被创建出来，但此时并未注入依赖（即`B`还未注入）。

  - Spring会将这个实例（实际上是一个工厂）放入==三级缓存（`singletonFactories`）中==，以便在后续的依赖注入过程中能够被其他Bean引用。


* **第二步：创建`B`实例**

  - `A`的创建未完成，因为`A`依赖于`B`，Spring开始创建`B`。

  - 同样地，`B`的构造方法执行，实例化`B`，但此时`B`还未注入其依赖（即`A`）。

  - Spring发现`B`依赖于`A`，于是会去三级缓存（`singletonFactories`）中查找`A`的工厂对象。

  - ==Spring通过三级缓存中的`ObjectFactory`获取`A`的早期实例，并将其放入二级缓存（`earlySingletonObjects`）中。这一阶段，`A`并未完全初始化，但可以被`B`使用。==

  - 然后Spring将`A`注入到`B`中，`B`的依赖注入完成。


* **第三步：完成`A`的创建**

  - 回到`A`的创建流程，`B`已经被创建且注入完成，可以安全地注入到`A`中。

  - Spring完成`A`的依赖注入和初始化，并将`A`从二级缓存移动到一级缓存（`singletonObjects`）中。


* **第四步：完成`B`的创建**
  - 最后，Spring完成`B`的初始化并将其放入一级缓存。




4. **循环依赖的解决**

通过三级缓存机制，Spring可以在创建过程中将未完全初始化的Bean提前暴露，并允许依赖方引用它。具体来说：

- **三级缓存（`singletonFactories`）** 提供了一个回调机制，可以让其他Bean在需要时获取一个尚未完全初始化的Bean。
- **二级缓存（`earlySingletonObjects`）** 用于存储从三级缓存中获取的早期实例，使得已经在依赖关系中使用的实例不会再次通过工厂生成，避免重复。
- **一级缓存（`singletonObjects`）** 最终存储完全初始化的Bean，供全局使用。

这种机制确保了即使在存在循环依赖的情况下，Spring也能够顺利完成Bean的创建和依赖注入。



**Spring中的循环引用**

* 循环依赖:循环依赖其实就是循环引用,也就是两个或两个以上的bean互相持有对方,最终形成闭环。比如A依赖于B,B依赖于A
* 循环依赖在spring中是允许存在，spring框架依据三级缓存已经解决了大部分的循环依赖
* 一级缓存:单例池，缓存已经经历了完整的生命周期，已经初始化完成的bean对象
* 二级缓存:缓存早期的bean对象(生命周期还没走完)
* 三级缓存:缓存的是ObjectFactory，表示对象工厂，用来创建某个对象的



## 3.三级缓存解决不了循环依赖的情况

三级缓存能解决大部分的循环依赖问题，但是有些是解决不了的，需要我们手动解决。



**构造方法出现了循环依赖怎么解决?**

A依赖于B，B依赖于A，注入的方式是构造函数

```java
@Component
public class A {
    private B b; 
    public A(B b){
        System.out.println("A的构造方法执行了.");
            this.b = b;
    }
}

@Component
public class B {
    private A a; 
    public B(A a){
        System.out.println("B的构造方法执行了.");
            this.a = a;
    }
}
```



**报错信息**：Is there an unresolvable circular reference?



**原因**:由于bean的生命周期中构造函数是第一个执行的，spring框架并不能解决构造函数的的依赖注入



**解决方案**:使用**@Lazy**进行懒加载，什么时候需要对象再进行bean对象的创建

```java
public A(@Lazy B b){
    System.out.printin("A的构造方法执行了.");
    this.b=b;
}
```





## 4.Spring 三级缓存的作用

* **解决循环依赖**（上文所述）
* **提高性能**：已经完成创建的 Bean 对象存储在==一级缓存==当中，后续需要使用时直接获取即可，避免重复创建，提供了系统性能和响应速度
* **实现懒加载**：==二级缓存==可以实现 Bean的懒加载，当需要使用到 Bean 时才进行对应的初始化操作，而不是启动时直接创建对象。
* **保证单例**：Spring 中的对象默认是单例的，完成创建的 Bean 对象全部存储在一级缓存当中。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0024.png?raw=true)

上图是由 Pic 生成的

关键词：Cute turtle

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

