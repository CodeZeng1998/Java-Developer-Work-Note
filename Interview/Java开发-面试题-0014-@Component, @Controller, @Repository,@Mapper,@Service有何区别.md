# Java开发-面试题-0014-@Component, @Controller, @Repository,@Mapper,@Service有何区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



@Component, @Controller, @Repository,@Mapper,@Service有何区别：

- `@Component`
  - **作用**: `@Component` 是一个通用的注解，标识一个类为 Spring 管理的 Bean。它没有特定的语义，只是表示该类是一个组件，可以被 Spring 容器扫描并注册为 Bean。
  - **适用场景**: 适用于所有没有特定角色的组件类。
- `@Controller`
  - **作用**: `@Controller` 是专门用于标识控制器类的注解，通常用于 Spring MVC 框架中。它用于处理用户请求，并返回视图或数据。
  - **适用场景**: 主要用于 Web 层，处理 HTTP 请求并返回响应。
- `@Repository`
  - **作用**: `@Repository` 用于标识数据访问层（DAO）类。Spring 容器会自动将 `@Repository` 注解的类识别为数据访问组件，并为其提供数据库操作异常的转换支持。
  - **适用场景**: 用于持久层，负责与数据库交互。
- `@Service`
  - **作用**: `@Service` 用于标识服务层的类，封装业务逻辑。虽然它的功能与 `@Component` 类似，但 `@Service` 注解为类赋予了更明确的业务含义。
  - **适用场景**: 用于业务逻辑层，封装业务规则和服务。
- `@Mapper`
  - **作用**: `@Mapper` 是 MyBatis 框架中的注解，专门用于标识 Mapper 接口，MyBatis 会自动生成对应的实现类，并将它注册为 Spring 的 Bean。`@Mapper` 通常与 MyBatis 或其他 ORM 框架结合使用。
  - **适用场景**: 用于 MyBatis 的数据访问层，与数据库表进行映射操作。





以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0014.png?raw=true)

上图是由 Pic 生成的

关键词：A man holding a submachine gun and smoking a cigarette is walking on an empty street, with a large group of zombies chasing it

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

