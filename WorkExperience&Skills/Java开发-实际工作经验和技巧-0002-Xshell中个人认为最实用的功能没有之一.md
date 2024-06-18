# Java开发-实际工作经验和技巧-0002-Xshell中个人认为最实用的功能没有之一

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



相信大家在工作当中都打包部署查看服务器状态等都非常常用Xshell这款个软件，本文是为了分享我个人认为最实用的Xshell功能--**快速命令窗格**。非常建议大家用上。



以下是使用步骤：



**步骤一：设置方式**

以下路径勾选 **查看 - 快速命令 - 快速命令窗格**，Xshell会出现**快速命令窗格**

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0002-Xshell%E4%B8%AD%E4%B8%AA%E4%BA%BA%E8%AE%A4%E4%B8%BA%E6%9C%80%E5%AE%9E%E7%94%A8%E7%9A%84%E5%8A%9F%E8%83%BD%E6%B2%A1%E6%9C%89%E4%B9%8B%E4%B8%80-%E8%AE%BE%E7%BD%AE%E6%96%B9%E5%BC%8F.png?raw=true)



**步骤二：使用方式**

* 双击**快速命令窗格**对应位置即可进行对应的快速命令的设置
* 内容讲解
  * 标签：自定义名称
  * 操作-类型：**发送字符串**
  * 操作-图标：**快速命令窗格**存储的自定义快速命令的图标，建议统一风格服务路径用一种颜色，获取日志实时信息用一种颜色等等
  * 操作-字符：这里就是填写我们需要用到的命令了（记得回车）
* 本文主要讲解“**发送字符串**”的方式（这边也建议优先使用“**发送字符串**”的方式），其余的`菜单、运行脚本、启动应用程序、发送一个文本文件`大家可自行研究使用。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0002-Xshell%E4%B8%AD%E4%B8%AA%E4%BA%BA%E8%AE%A4%E4%B8%BA%E6%9C%80%E5%AE%9E%E7%94%A8%E7%9A%84%E5%8A%9F%E8%83%BD%E6%B2%A1%E6%9C%89%E4%B9%8B%E4%B8%80-%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F.jpg?raw=true)



**步骤三：设置字符**

建议

* 可用于快速**打印某个服务的日志**（以后无需查找服务日志路径）

  ```shell
  tail -f  /usr/local/apps/xxxService/logs/xxxService.log
  ```

  

* 可用于快速**跳转到某台服务器上**（以后无需手动跳转并找到对应目录）

  例如：跳转到Kafka所在服务器打印出对应groupid的消费情况

  ```shell
  ssh -c 3des-cbc [用户名]@ [IP地址]
  [对应IP地址服务器对应用户名的密码]
  cd /usr/local/apps/kafka/bin/
  ll
  pwd
  ./kafka-consumer-groups.sh --bootstrap-server [bootstrap-server:port] --describe --group [groupid]
  ```

  

* 可用于快速**进入某个服务所在路径**

  ```shell
  cd /apps/XXXService
  ```

  

  

  

非常建议将一些常用的命令存储在Xshell的快速命令窗格里，例如多个服务的日志打印、多个服务的所在路径等，需要使用的时候**点击一下**即可，大大节省时间。









![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0002-Xshell%E4%B8%AD%E4%B8%AA%E4%BA%BA%E8%AE%A4%E4%B8%BA%E6%9C%80%E5%AE%9E%E7%94%A8%E7%9A%84%E5%8A%9F%E8%83%BD%E6%B2%A1%E6%9C%89%E4%B9%8B%E4%B8%80.png?raw=true)

上图由 Pic 生成

关键词：pink people





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**