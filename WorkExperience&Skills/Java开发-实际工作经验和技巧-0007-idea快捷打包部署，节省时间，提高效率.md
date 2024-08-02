# Java开发-实际工作经验和技巧-0007-idea快捷打包部署，节省时间，提高效率

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

<br/>





问题描述：对应小需求不断，并且需要频繁更新测试环境以供前端人员、测试人员进行联调以及测试的项目来说，快速的从本地打包部署到测试环境不仅可以节省时间，提高效率，也可以减少自己的麻烦。（这里没有使用 **Jenkins**）

<br/>

前置条件：

* 下载好 Idea 对应的插件 ==Alibaba Cloud Toolkit==
* 测试环境服务器编辑好对应对的服务执行脚本

<br/>

步骤如下：

1. Tools -> Alibaba Cloud -> Deploy to Host

2. 参照下图

   1. 选择部署方式，以 ==Upload File== 的形式发布

   2. 选择要  ==上传的文件== （jar包、zip包等）

   3. 配置好 ==Target Host==（目标服务器）

   4. 配置好 ==Target Directory==（目标服务器目录）

   5. ==After deploy==（部署完要执行的命令）,

      我这里执行的是服务器上的脚本：**sh XXX.sh**

      脚本内容如下：(**停止服务、备份、部署重启服务、查看日志**)

      ```shell
      cd /usr/local/apps/xxxApp
      sh /usr/local/apps/xxxApp/bin/service.sh stop
      cp -r  xxxApp xxxApp_bak
      rm -rf xxxApp
      unzip xxxApp.zip
      cd xxxApp/bin/
      sh service.sh restart
      sleep 15s
      tail -f  /usr/local/apps/xxxApp/logs/xxxApp.log

   6. 配置==Before launch==（启动前），我这里配置的是启动前使用maven打包新的包。

   7. 所有配置好之后直接==Run==即可。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0007-idea%E5%BF%AB%E6%8D%B7%E6%89%93%E5%8C%85%E9%83%A8%E7%BD%B2%EF%BC%8C%E8%8A%82%E7%9C%81%E6%97%B6%E9%97%B4%EF%BC%8C%E6%8F%90%E9%AB%98%E6%95%88%E7%8E%87-%E6%AD%A5%E9%AA%A4001.png?raw=true)

<br/>

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0007-idea%E5%BF%AB%E6%8D%B7%E6%89%93%E5%8C%85%E9%83%A8%E7%BD%B2%EF%BC%8C%E8%8A%82%E7%9C%81%E6%97%B6%E9%97%B4%EF%BC%8C%E6%8F%90%E9%AB%98%E6%95%88%E7%8E%87-%E6%AD%A5%E9%AA%A4002.png?raw=true)



<br/>

以上就是简单使用插件 ==Alibaba Cloud Toolkit== 进行便捷的打包部署安装到测试环境运行的内容了，有了这个就可以在我们需要快速测试时使用，节省时间，给自己较少麻烦，偷偷懒。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0007-idea%E5%BF%AB%E6%8D%B7%E6%89%93%E5%8C%85%E9%83%A8%E7%BD%B2%EF%BC%8C%E8%8A%82%E7%9C%81%E6%97%B6%E9%97%B4%EF%BC%8C%E6%8F%90%E9%AB%98%E6%95%88%E7%8E%87.png?raw=true)

上图由 Pic 生成

关键词：Cyber style programmers are typing code at their desks

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**