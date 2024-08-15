# Java开发-面试题-0013-@Resource和@Autowired的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



@Resource 和 @Autowired 两个注解源码：

```java
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Autowired {

	/**
	 * Declares whether the annotated dependency is required.
	 * <p>Defaults to {@code true}.
	 */
	boolean required() default true;

}

@Target({TYPE, FIELD, METHOD})
@Retention(RUNTIME)
public @interface Resource {
    String name() default "";

    String lookup() default "";

    Class<?> type() default java.lang.Object.class;

    enum AuthenticationType {
            CONTAINER,
            APPLICATION
    }

    AuthenticationType authenticationType() default AuthenticationType.CONTAINER;

    boolean shareable() default true;

    String mappedName() default "";

    String description() default "";
}
```



**@Resource和@Autowired的区别**：

 1. 来源

- **`@Autowired`**: 这是 Spring Framework 提供的注解，常用于 Spring 项目中。
- **`@Resource`**: 这是 JSR-250 规范中的注解，属于 Java 标准，且可以在任何支持 JSR-250 的容器中使用，包括 Spring。



 2. 注入方式

- **`@Autowired`**: 默认按**类型（by type**）进行依赖注入。可以通过 `@Qualifier` 注解指定注入的具体 Bean。当容器中有多个相同类型的 Bean 时，Spring 会抛出异常，除非设置 `required = false` 或使用 `@Qualifier` 明确指定。

- **`@Resource`**: 默认按**名称（by name**）进行注入，如果未指定名称，会尝试按类型注入。如果按名称找不到对应的 Bean 且按类型也无法找到匹配的 Bean，则会抛出异常。

  - ==@Resource装配顺序==:

    1.如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常。
  
    2.如果指定了name，则从上下文中查找名称(id)四配的bean进行装配，找不到则抛出异常。
  
    3.如果指定了type，则从上下文中找到类似匹配的唯一bean进行装配，找不到或是找到多个，都会抛出异常。
  
    4.如果既没有指定name，又没有指定type，则自动按照byName方式进行装配*;如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。*
  
    @Resource的作用相当于@Autowired，只不过@Autowired按照byType自动注入。
  
  
  

3. 使用场景

- **`@Autowired`**: 更多用于 Spring 的项目中，并且可以结合 `@Primary` 和 `@Qualifier` 进行复杂的 Bean 注入配置。
- **`@Resource`**: 在需要更精细的控制时，比如明确按名称注入 Bean，或在需要确保兼容 JSR-250 的场景下使用。

4. 兼容性

- **`@Autowired`**: 依赖于 Spring 容器管理，主要用于 Spring 应用中。
- **`@Resource`**: 作为 JSR-250 的一部分，可以在更广泛的容器环境中使用，具有更广的兼容性。

5. 属性

- **`@Autowired`**: 属性 `required`（默认为 true），可以用于控制是否必须注入 Bean。
- **`@Resource`**: 有 `name` 和 `type` 属性，可以指定注入 Bean 的名称和类型。

### 总结

`@Autowired` 和 `@Resource` 在依赖注入的细节上有所不同，选择哪个取决于具体的需求和项目的依赖框架。一般来说，如果是在 Spring 项目中，`@Autowired` 是更常用的选择；如果需要跨框架的兼容性或更细粒度的控制，可以考虑使用 `@Resource`。



<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0013.png?raw=true)

上图是由 Pic 生成的

关键词：Trump raises his arm high

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

