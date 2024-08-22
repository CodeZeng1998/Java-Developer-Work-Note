# Java开发-面试题-0025-Spring MVC 的执行流程

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



Spring MVC 的执行流程分如下两种：

* 视图阶段(老旧 JSP 等)
* 前后端分离阶段(接口开发，异步)





### **视图阶段(老旧 JSP 等)**

1. 用户发送出请求到前端控制器 DispatcherServlet
2. DispatcherServlet 收到请求调用HandlerMapping(处理器映射器)
3. HandlerMapping找到具体的处理器，生成处理器对象及处理器拦截器(如果有)，再一起返回给DispatcherServlet.
4. DispatcherServet调用HandlerAdapter(处理器适配器)
5. HandlerAdapter经过适配调用具体的处理器(Handler/Controller)
6. Controller执行完成返回ModelAndView对象
7. HandlerAdapter将Controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServet将ModelAndView传给ViewReslover(视图解析器)
9. ViewReslover解析后返回具体View(视图)
10. DispatcherServlet根据View进行渲染视图(即将模型数据填充至视图中)
11. DispatcherServlet响应用户

<br/>



### **前后端分离阶段(接口开发)**

1. 用户发送出请求到前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用HandlerMapping(处理器映射器)
3. HandlerMapping找到具体的处理器，生成处理器对象及处理器拦截器(如果有)，再一起返回给DispatcherServlet.
4. DispatcherServlet调用HandlerAdapter(处理器适配器)
5. HandlerAdapter经过适配调用具体的处理器(Handler/Controller)
6. 方法上添加了@ResponseBody
7. 通过**HttpMessageConverter**来返回结果转换为JSON并响应



<br/>



以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0025.png?raw=true)

上图是由 Pic 生成的

关键词：Charge forward blindfolded

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



