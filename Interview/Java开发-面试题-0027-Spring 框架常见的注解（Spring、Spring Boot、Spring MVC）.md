# Java开发-面试题-0027-Spring 框架常见的注解（Spring、Spring Boot、Spring MVC）

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**





## **Spring 常见的注解**

| 注解                                           | 说明                                                         |
| ---------------------------------------------- | ------------------------------------------------------------ |
| @Component、@Controller、@Service、@Repository | 是用在类上用于实例化 Bean                                    |
| @Autowired                                     | 使用在字段上用于根据类型依赖注入                             |
| @Qualifier                                     | 结合@Autowired一起使用用于根据名称进行依赖注入               |
| @Scope                                         | 标准 Bean 的作用范围                                         |
| @Configuration                                 | 指定当前累是一个 Spring 配置类，当创建容器时会从该类上加载注解 |
| @ComponentScan                                 | 用于指定 Spring 在初始化容器时要扫描的包                     |
| @Bean                                          | 使用在方法上，标注将该方法的返回值存储到 Spring 容器中       |
| @Import                                        | 是用@Import导入的类会被 Spring 加载到 IOC 容器中             |
| @Aspect、@Before、@After、@Around、@Pointcut   | 用于切面编程（AOP）                                          |



## Spring MVC 常见注解

| 注解            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @RequestMapping | 用于映射请求路径，可以定义在类上和方法上。用于类上，则表示类中的所有方法都是以该地址作为父路径 |
| @RequestBody    | 注解实现接收 http 请求的 json 数据，将 json 转换为 java 对象 |
| @RequestParam   | 指定请求参数的名称                                           |
| @PathVariable   | 从请求路径下中获取请求参数，传递给方法的形式参数             |
| @ResponseBody   | 注解实现将 controller 方法返回对象转化为 json 对象响应给客户端 |
| @RequestHeader  | 获取指定的请求头数据                                         |
| @RestController | @Controller + @ResponseBody                                  |



## Spring Boot 常见注解

| 注解                     | 说明                                             |
| ------------------------ | ------------------------------------------------ |
| @SpringBootConfiguration | 组合了 @Configuration 注解，实现配置文件的功能   |
| @EnableAutoConfiguration | 打开自动配置的功能，也可以关闭某个自动配置的选项 |
| @ComponentScan           | Spring 组件扫描                                  |

<br/>



以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0027.png?raw=true)

上图是由 Pic 生成的

关键词：Black Myth Wukong

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



