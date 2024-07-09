# Java开发-实际工作经验和技巧-0006-Linux系统编写定时任务

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

<br/>



需求描述：最近工作上遇见了一个需求就是需要直接脱离服务，在 Linux 系统层面上定时执行某些内容，例如定时查看部署服务的服务器与某些服务器的网络是否会由于网络波动出现网络不通的问题，由于部署服务器的某些限制无法给我们上线一些监测服务和工具，所以我们采用最原生的办法直接在Linux上编写对应的定时任务。



## 方式一：调用外部脚本（推荐使用）

调用脚本的方式



```shell
# 编辑当前用户的 crontab 文件的命令
# crontab文件是一个配置文件，指定要在计划时间或间隔时间运行的命令。这些命令由cron守护进程执行。
crontab -e
```

<br/>

往里面增加对应的定时任务：

```shell
*/1 * * * * /shellPath/telnetCheck.sh
```

**定时任务内容**：这个cron作业每分钟运行一次shell脚本(`/shellPath/telnetCheck.sh`)

<br/>

脚本内容如下：telnetCheck.sh（这里简单输出对应的信息到日志文件上）

```shell
#!/bin/bash

# 获取当前时间戳
timestamp=$(date '+%Y-%m-%d %H:%M:%S')

# 执行telnet命令并检查是否成功
if timeout 5 telnet 127.0.0.1 8080 2>&1 | grep -q 'Connected'; then
    echo "$timestamp Success telnet 127.0.0.1 8080" >> /telnetCheckLogPath/telnetCheck.log
else
    echo "$timestamp Failure telnet 127.0.0.1 8080" >> /telnetCheckLogPath/telnetCheck.log
fi
```

这个shell脚本检查到`127.0.0.1`上`8080`端口的telnet连接是否成功，并记录带时间戳的结果。

<br/>

注意：创建的 telnetCheck.sh 脚本需要是可执行的。

```shell
chmod +x /shellPath/telnetCheck.sh
```



后续直接查看对应的日志即可。

<br/>



## 方式二：直接编写执行命令，不调用外部脚本

```shell
# 编辑当前用户的 crontab 文件的命令
# crontab文件是一个配置文件，指定要在计划时间或间隔时间运行的命令。这些命令由cron守护进程执行。
crontab -e
```

<br/>

加上需要的内容，如下：

```shell
*/1 * * * * (echo quit | timeout 5 telnet 127.0.0.1 8080) >> /telnetCheckLogPath/telnetCheck.log 2>&1
```

**定时任务内容**：这个cron作业每分钟运行一次，尝试建立到`127.0.0.1`的`8080`端口的telnet连接。括号内的命令向telnet会话发送`quit`命令，并使用`timeout`确保命令运行时间不超过5秒。此操作的输出随后追加到`/telnetCheckLogPath/telnetCheck.log`，并且捕获标准输出和标准错误（`2>&1`）

<br/>

这种方式适合简单的定时任务的编写，如果是复杂的定时任务还是更加推荐使用`方式一：调用外部脚本`的方式。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0006-Linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E5%86%99%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1.png?raw=true)

上图由 Pic 生成

关键词：Wearing_a_spacesuit_and_riding_a_bicycle_on_Mars_png

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**