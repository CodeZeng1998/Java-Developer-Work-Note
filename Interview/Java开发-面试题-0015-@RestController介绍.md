# Java开发-面试题-0015-@RestController介绍



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



**@RestController介绍**：

`@RestController` 是 Spring Framework 中的一个注解，用于创建 RESTful Web 服务。它是 `@Controller` 和 `@ResponseBody` 注解的组合。以下是 `@RestController` 的详细说明：



### 1. **作用**

- **`@RestController`**: 标识一个类为 ==RESTful Web== 服务的控制器。它告诉 Spring 这个类中的所有方法的返回值都应该直接作为 HTTP 响应的主体（Response Body），而不是返回视图（如 JSP、Thymeleaf 等）。



### 2. **语义和功能**

- **`@Controller`**: 用于标识一个 Spring MVC 控制器类，处理 Web 请求，并返回视图名称。
- **`@ResponseBody`**: 作用于方法或类上，表示返回的对象会自动序列化为 JSON 或 XML，并直接写入 HTTP 响应体中，而不是返回视图名称。

`@RestController` 相当于同时使用 `@Controller` 和 `@ResponseBody`，意味着类中每个方法的返回值都是以 JSON、XML 等格式直接输出到 HTTP 响应体中。

==@RestController = @Controller + @ResponseBody==



### 3. **适用场景**

- **API 开发**: 当你开发 RESTful 风格的 Web 服务时，`@RestController` 是非常合适的选择。它简化了代码，因为不需要在每个方法上都添加 `@ResponseBody` 注解。

### 4. **与 `@Controller` 的区别**

- **`@RestController`**: 适用于 RESTful 服务，默认情况下每个方法都返回数据（如 JSON、XML）。
- **`@Controller`**: 适用于传统的 Web 应用，方法通常返回视图名，需要与 `@ResponseBody` 结合使用以返回数据而非视图。



### 总结

`@RestController` 是一个专门用于开发 RESTful Web 服务的注解，它简化了 REST API 的开发流程，使得代码更简洁易读。



<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0015.png?raw=true)

上图是由 Pic 生成的

关键词：A large group of highend suit zombies knelt down to worship the men on the throne

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

