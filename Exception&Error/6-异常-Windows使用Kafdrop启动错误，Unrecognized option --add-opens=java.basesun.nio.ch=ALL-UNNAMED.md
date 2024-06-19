# 6-异常-Windows使用Kafdrop启动错误，Unrecognized option --add-opens=java.basesun.nio.ch=ALL-UNNAMED



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



**问题描述**：Windows使用命令行的方式启动Kafdrop报错。

* 启动Kafdrop如下：

```shell
java --add-opens=java.base/sun.nio.ch=ALL-UNNAMED -jar "C:\work\developer\development tool\kafdrop\kafdrop-4.0.0.jar" --kafka.brokerConnect=[kafkaIpAddress]:[kafkaPort]
```

* 报错信息如下:

```
Unrecognized option: --add-opens=java.base/sun.nio.ch=ALL-UNNAMED
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
```

* 错误原因：Java 版本不支持。



错误消息表明，Java虚拟机（JVM）不识别`--add-opens=java.base/sun.nio.ch=ALL-UNNAMED`选项。可能的原因有以下几种：

1. **Java版本**：`--add-opens`选项在Java 9及以上版本中支持。如果您使用的是早于Java 9的版本，该选项将不会被识别。请确保您使用的是合适的Java版本。
2. **正确的语法**：确保选项的语法和拼写正确。应该是`--add-opens=java.base/sun.nio.ch=ALL-UNNAMED`。
3. **JVM参数的位置**：验证JVM参数的位置是否正确。它们应该放在`-jar`选项之前。



以下是故障排除和解决问题的方法：

**解决步骤：**

1. **检查Java版本**： 确保您使用的是Java 9或更高版本。您可以运行以下命令检查Java版本：

   ```shell
   java -version
   ```

2. **更正命令**： 确保您的命令格式正确。它应如下所示：

   ```shell
   java --add-opens=java.base/sun.nio.ch=ALL-UNNAMED -jar "C:\work\developer\development tool\kafdrop\kafdrop-4.0.0.jar" --kafka.brokerConnect=[kafkaIpAddress]:[kafkaPort]
   ```

3. **安装/升级Java**： 如果您发现使用的是旧版本的Java，请升级到最新版本。您可以从[Oracle官网](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html)下载最新的Java开发工具包（JDK），或者使用开源替代品如[AdoptOpenJDK](https://adoptopenjdk.net/)。

4. **环境变量**： 确保您的JAVA_HOME环境变量设置为正确的JDK安装路径，并且您的PATH变量包含JDK的bin目录。

5. **检查命令行外壳**： 如果您使用的是不同的命令行外壳（如PowerShell或命令提示符），请确保命令语法适用于该外壳。





* **解决方式一：直接更改本地Java版本**，我这边由于项目比较多都是采用Java1.8的版本，所以出现了上述问题。（不推荐使用）

* **解决方式二：直接指定运行Java来启动Kafdrop**。（推荐使用）

例如：我这里直接指定的Java 21的版本

```shell
"C:\work\developer\environment\jdk\jdk-21.0.1\bin\java" --add-opens=java.base/sun.nio.ch=ALL-UNNAMED -jar "C:\work\developer\development tool\kafdrop\kafdrop-4.0.0.jar" --kafka.brokerConnect=[kafkaIpAddress]:[kafkaPort]
```







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/6-%E5%BC%82%E5%B8%B8-Windows%E4%BD%BF%E7%94%A8Kafdrop%E5%90%AF%E5%8A%A8%E9%94%99%E8%AF%AF%EF%BC%8CUnrecognized%20option%20--add-opens=java.basesun.nio.ch=ALL-UNNAMED.png?raw=true)

上图是由 Pic 生成的

关键词：a large group of people strolled along Wall Street





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





