# Java开发-面试题-0023-Spring 的 bean 的生命周期



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**







**Bean的流程**：



BeanDefinition

-> 构造函数

**-> 依赖注入**

**-> Aware 接口 -> BeanNameAware -> BeanFactoryAware -> ApplicationContextAware**

**-> BeanPostProcessor#before**

**-> 初始化方法 -> InitializingBean -> 自定义 init 方法**

**-> BeanPostProcessor#after -> AOP -> 动态代理（JDK 动态代理、CGLIB 动态代理）**

-> 销毁 bean



* **BeanDefinition**：Spring容器在进行实例化时，会将xml配置的`<bean>`的信息封装成一个BeanDefinition对象，Spring根据BeanDefinition来创建Bean对象，里面有很多的属性用来描述Bean。
* ==Bean 的创建和初始化赋值是分开的==（加粗部分均为“初始化赋值”）





**Spring的bean的生命周期**：

* 通过BeanDefinition获取bean的定义信息
* 调用构造函数实例化bean
* bean的依赖注入
* 处理Aware接囗(BeanNameAware、BeanFactoryAware、ApplicationContextAware)
* Bean的后置处理器BeanPostProcessor-前置
* 初始化方法(InitializingBean、init-method)
* Bean的后置处理器BeanPostProcessor-后置
* 销毁bean



构造函数
 ->	依赖注入
 ->	Aware接口
 ->	BeanPostProcessor#before
 ->	初始化方法
 ->	BeanPostProcessor#after
 ->	销毁bean





Spring 的 `Bean` 生命周期非常丰富，包含了多个步骤和扩展点。在应用程序运行过程中，Spring 通过其 IoC 容器管理 `Bean` 的整个生命周期。以下是详细的生命周期流程：

### 1. **实例化（Instantiation）**

- Spring 容器通过反射机制创建 `Bean` 实例。在此步骤中，`Bean` 实例已经被创建，但还没有进行任何属性的赋值。

### 2. **设置对象属性（Populate Properties）**

- Spring 容器会根据 `Bean` 配置文件中的定义或者注解，完成依赖注入。此时，如果有 `@Autowired`、`@Value` 或 `@Resource` 注解的属性，会被注入相应的值或依赖。

### 3. **处理 `Aware` 接口（Aware Interfaces）**

- 如果 `Bean` 实现了某些 `Aware` 接口（例如 `BeanNameAware`、`BeanFactoryAware`、`ApplicationContextAware` 等），Spring 容器会调用这些接口的方法，使 `Bean` 获得 Spring 容器的相关资源或信息。
- 例如，`BeanNameAware` 可以让 `Bean` 获取它在容器中的名称，`BeanFactoryAware` 可以让 `Bean` 获取到所属的 `BeanFactory`。

### 4. **`BeanPostProcessor` 前置处理（BeanPostProcessor Before Initialization）**

- 在 `Bean` 初始化前，Spring 容器会调用所有实现了 ==`BeanPostProcessor` 接口的类的 `postProcessBeforeInitialization()` 方法==。这个方法可以在 `Bean` 初始化之前，对 `Bean` 进行一些处理，例如修改或替换 `Bean` 实例。

### 5. **初始化（Initialization）**

- `Bean` 初始化主要包括以下步骤：
  1. ==调用 `InitializingBean` 接口的 `afterPropertiesSet()` 方法==：
     - 如果 `Bean` 实现了 `InitializingBean` 接口，Spring 容器会调用其 `afterPropertiesSet()` 方法。
  2. 调用自定义的初始化方法：
     - ==如果在 `Bean` 的配置文件中指定了 `init-method`，Spring 容器会调用这个自定义的初始化方法。这个方法可以在 `Bean` 准备好使用之前做一些初始化工作。==
  3. 处理 `@PostConstruct` 注解：
     - 如果 `Bean` 上有 `@PostConstruct` 注解的方法，Spring 容器也会在这个阶段调用该方法。

### 6. **`BeanPostProcessor` 后置处理（BeanPostProcessor After Initialization）**

- 在 `Bean` 初始化后，Spring 容器会再次调用实现了==`BeanPostProcessor` 接口的类的 `postProcessAfterInitialization()` 方法==。这个方法可以用于对已经初始化完成的 `Bean` 进行修改或增强功能，例如为 `Bean` 创建代理对象（AOP 切面就是在这里处理的）。

### 7. **准备就绪和使用（Ready for Use）**

- 经过前面的步骤，`Bean` 已经准备好，可以被应用程序正常使用了。此时，`Bean` 会被注入到应用程序的各个部分，提供预期的功能。

### 8. **销毁（Destruction）**

- 当 Spring 容器关闭或 `Bean` 被销毁时，会触发销毁阶段的操作：
  1. ==调用 `DisposableBean` 接口的 `destroy()` 方法==：
     - 如果 `Bean` 实现了 `DisposableBean` 接口，Spring 容器会调用其 `destroy()` 方法。
  2. 调用自定义的销毁方法：
     - ==如果在 `Bean` 的配置文件中指定了 `destroy-method`，Spring 容器会调用这个自定义的销毁方法，用于清理资源==。
  3. 处理 `@PreDestroy` 注解：
     - ==如果 `Bean` 上有 `@PreDestroy` 注解的方法，Spring 容器会在销毁 `Bean` 之前调用该方法==。

### 总结

Spring 的 `Bean` 生命周期从实例化到销毁，经历了多个步骤，每个步骤都有特定的扩展点，可以在需要时插入自定义逻辑。通过理解这些步骤和接口，开发者可以在 `Bean` 的生命周期中控制和定制 `Bean` 的行为，从而满足复杂的业务需求。





## 全篇总结

**Spring的bean的生命周期**：

BeanDefinition

-> 构造函数（`Bean` 实例已经被创建，但还没有进行任何属性的赋值。）

**-> 依赖注入**（如果有 `@Autowired`、`@Value` 或 `@Resource` 注解的属性，会被注入相应的值或依赖）

**-> Aware 接口 -> BeanNameAware -> BeanFactoryAware -> ApplicationContextAware**（`BeanNameAware` 可以让 `Bean` 获取它在容器中的名称，`BeanFactoryAware` 可以让 `Bean` 获取到所属的 `BeanFactory`）

**-> BeanPostProcessor#before**

**-> 初始化方法 -> InitializingBean（调用 `InitializingBean` 接口的 `afterPropertiesSet()` 方法） -> 自定义 init 方法**（如果在 `Bean` 的配置文件中指定了 `init-method`，Spring 容器会调用这个自定义的初始化方法）

**-> BeanPostProcessor#after（在 `Bean` 初始化后，Spring 容器会再次调用实现了`BeanPostProcessor` 接口的类的 `postProcessAfterInitialization()` 方法） -> AOP -> 动态代理（JDK 动态代理、CGLIB 动态代理）**

--> 使用 bean

-> 销毁 bean（调用 `DisposableBean` 接口的 `destroy()` 方法、如果在 `Bean` 的配置文件中指定了 `destroy-method`，Spring 容器会调用这个自定义的销毁方法，用于清理资源）









以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0023.png?raw=true)

上图是由 Pic 生成的

关键词：Cute slime

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

