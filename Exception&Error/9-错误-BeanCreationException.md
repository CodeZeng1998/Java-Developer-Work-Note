# 9-错误-BeanCreationException Error creating bean with name 'xxxServiceImpl' Injection of autowired dependencies failed; nested exception is java.lang.IllegalArgumentException Could not resolve placeholder 'api.xxx'



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

<br/>

**问题描述**：今天拆分服务的时候遇见了一个如下的报错，某个类中找不到对应的配置报错。

<br/>

**报错信息：**

```txt
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'xxxServiceImpl': Injection of autowired dependencies failed; nested exception is java.lang.IllegalArgumentException: Could not resolve placeholder 'api.xxx' in value "${api.xxx}"
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessProperties(AutowiredAnnotationBeanPostProcessor.java:403) ~[spring-beans-5.2.1.RELEASE.jar:5.2.1.RELEASE]
	...
	...
Caused by: java.lang.IllegalArgumentException: Could not resolve placeholder 'api.xxx' in value "${api.xxx}"
	...
	...
```

<br/>

**错误原因**：配置文件中缺少对应的配置。

遇到的错误 `Could not resolve placeholder 'api.xxx' in value "${api.xxx}"` 表明 Spring 无法找到 `api.xxx` 的属性值。这是一个常见问题，当所需的配置属性缺失或未正确定义在应用程序的属性文件中时会发生这种情况。

<br/>

**解决方案**：

1. **检查属性文件**: 确保 `api.xxx` 定义在你的属性文件中，例如 `application.properties` 或 `application.yml`。例如：

   **application.properties**:

   ```properties
   api.xxx=http://example.com/xxx
   ```

   **application.yml**:

   ```yaml
   api:
     xxx: http://example.com/xxx
   ```

2. **特定配置文件**: 如果你使用了不同的配置文件（例如 `dev`, `prod`），确保 `api.xxx` 定义在活动配置文件的属性文件中。例如，如果你在使用 `dev` 配置文件，请检查 `application-dev.properties` 或 `application-dev.yml`。

3. **检查环境变量**: 确保没有环境变量覆盖或缺少 `api.xxx`。环境变量可以用来覆盖配置文件中的属性。

4. **正确的占位符语法**: 确保配置文件中的占位符语法没有拼写错误。

5. **确保配置已加载**: 确保配置文件已正确加载并包含在应用程序的类路径中。

6. **PropertySourcesPlaceholderConfigurer**: 如果使用了自定义的 `PropertySourcesPlaceholderConfigurer`，确保它已正确设置以加载属性文件。

<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/9-%E9%94%99%E8%AF%AF-BeanCreationException%20Error%20creating%20bean%20with%20name%20'xxxServiceImpl'%20Injection%20of%20autowired%20dependencies%20failed;%20nested.png?raw=true)

上图是由 Pic 生成的

关键词：A sleek futuristic car speeding through a neon-lit city

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
