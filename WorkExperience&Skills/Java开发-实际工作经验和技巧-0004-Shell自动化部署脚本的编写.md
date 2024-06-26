# Java开发-实际工作经验和技巧-0004-Shell自动化部署脚本的编写

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

<br/>



需求说明：编写一个自动化部署脚本，用于快速备份部署项目。



以下直接使用最简单的单体服务当做例子，我这里使用的测试环境和正式环境的有些许区别：

* 测试环境：资源有限，只需备份一份
* 生产环境：保证数据有迹可循，每次更新都需要备份一份





**测试环境自动化部署 Shell 脚本如下：auto-deplot-xxxApp.sh**

```shell
# 进入服务目录
cd /usr/local/apps/xxxApp

# 执行停止服务脚本
sh xxxApp-1.0/bin/xxxService.sh stop

# 备份
cp -r xxxApp-1.0 xxxApp-1.0_bak

# 删除原有内容
rm -rf xxxApp-1.0

# 解压
unzip xxxApp-1.0.zip

# 执行服务启动脚本
sh /usr/local/apps/xxxApp/xxxApp-1.0/bin/xxxService.sh start

# 等待 15 秒
sleep 15s

# 打印实时日志
tail -f  /usr/local/apps/xxxApp/xxxApp-1.0/logs/xxxApp.log
```

* 备份和删除的步骤可直接替换成 `mv`

<br/>

**正式环境自动化部署  Shell 脚本如下：auto-deplot-xxxApp.sh**

```shell
# 进入服务目录
cd /usr/local/apps/xxxApp

# 获取当前时间
current_time=$(date +"%Y-%m-%d_%H:%M:%S")

# 执行停止服务脚本
sh xxxApp-1.0/bin/xxxService.sh stop

# 备份
cp -r xxxApp-1.0 "xxxApp-1.0_bak_${current_time}"

# 删除原有内容
rm -rvf xxxApp-1.0

# 解压
unzip xxxApp-1.0.zip

# 执行服务启动脚本
sh /usr/local/apps/xxxApp/xxxApp-1.0/bin/xxxService.sh start

# 等待 10 秒
sleep 10s

# 打印实时日志
tail -f  /usr/local/apps/xxxApp/xxxApp-1.0/logs/xxxApp.log
```

* 备份和删除的步骤可直接替换成 `mv`





以上就是本文的所有内容了，希望对你有所帮助。



<br/>

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0004-Shell%E8%87%AA%E5%8A%A8%E5%8C%96%E9%83%A8%E7%BD%B2%E8%84%9A%E6%9C%AC%E7%9A%84%E7%BC%96%E5%86%99.png?raw=true)

上图由 Pic 生成

关键词：Pocket Monster

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**