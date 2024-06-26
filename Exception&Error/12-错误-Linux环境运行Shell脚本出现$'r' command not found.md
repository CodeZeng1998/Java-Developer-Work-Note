# 12-错误-Linux环境运行Shell脚本出现$'r' command not found



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：Linux 环境运行 Windows 环境编写的 Shell 脚本，出现如下报错。



<br/>

**报错信息：**

```shell
[127.0.0.1@localhost bin]# sh xxx.sh start
xxx.sh: line 4: $'\r': command not found
xxx.sh: line 6: syntax error near unexpected token $'{\r''
'xxx.sh: line 6: usage(){
```



<br/>

**错误原因**：存在换行符 (`\r`) 的问题。这种情况通常发生在脚本在 Windows 系统上编辑或创建后移动到类 Unix 系统而没有正确转换行尾换行符的情况下。



<br/>

**解决方案**：

**方案一：**

```shell
sed -i 's/\r$//' xxx.sh
```

这个命令使用 `sed` 工具来从一个文件中移除 Windows 操作系统中的行尾回车符（Carriage Return，\r），通常在类Unix系统中使用。

- `sed`: 是一个流编辑器，用于处理和转换文本。
- `-i`: 是 sed 命令的选项，表示直接在文件中进行编辑（in-place）。
- `'s/\r$//'`: 是 sed 的替换命令，用来查找每行结尾的 `\r`（回车符）并将其替换为空字符串（即删除）。
- `xxx.sh`: 是要编辑的文件名，这里假设是一个 Shell 脚本文件。

这条命令的作用是在 `xxx.sh` 文件中去除每行结尾的 Windows 格式的回车符，使得文件在类Unix系统中能够正常显示和运行。

<br/>



**方案二：**

```shell
dos2unix xxx.sh
```

**这个命令 `dos2unix xxx.sh` 用于将一个文本文件从 DOS 或 Windows 格式转换为 Unix 格式。**在 Unix 系统中，换行符只使用换行（Line Feed，\n），而在 DOS 和 Windows 系统中，换行符使用回车和换行（Carriage Return + Line Feed，\r\n）的组合。

- `dos2unix`: 是一个命令行工具，专门用来将 DOS 或 Windows 格式的文本文件转换为 Unix 格式。
- `xxx.sh`: 是要转换的文件名，这里假设是一个 Shell 脚本文件。

执行这个命令后，`dos2unix` 会读取 `xxx.sh` 文件，并将其中的每个 `\r\n` 组合（DOS/Windows 格式的换行符）转换为 Unix 格式的 `\n` 换行符，这样文件就可以在 Unix 系统上正确显示和处理。

<br/>



执行成功会有如下输出：

```shell
[127.0.0.1@localhost bin]# dos2unix xxx.sh
dos2unix: converting file xxx.sh to Unix format ...
```

<br/>



注意：如果执行 `dos2unix xxx.sh` 出现如下报错，则表明当前环境没有安装 dos2unix

```shell
[127.0.0.1@localhost bin]# dos2unix xxx.sh
-bash: dos2unix: command not found
```

<br/>



**dos2unix 安装步骤**：出现如下日志则表明安装成功了，可安装上述步骤对脚本进行格式化。

```shell
[127.0.0.1@localhost bin]# yum install -y dos2unix
Loaded plugins: fastestmirror
Determining fastest mirrors
Could not get metalink https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=x86_64 error was
14: curl#7 - "Failed to connect to 2406:da1a:fcb:2f01:f381:af1a:f922:c519: Network is unreachable"
 * base: mirrors.ustc.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.ustc.edu.cn
 * updates: mirrors.ustc.edu.cn
base                                                                                                                                         | 3.6 kB  00:00:00     
extras                                                                                                                                       | 2.9 kB  00:00:00     
updates                                                                                                                                      | 2.9 kB  00:00:00     
(1/2): extras/7/x86_64/primary_db                                                                                                            | 253 kB  00:00:00     
(2/2): updates/7/x86_64/primary_db                                                                                                           |  27 MB  00:00:03     
Resolving Dependencies
--> Running transaction check
---> Package dos2unix.x86_64 0:6.0.3-7.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

====================================================================================================================================================================
 Package                                 Arch                                  Version                                    Repository                           Size
====================================================================================================================================================================
Installing:
 dos2unix                                x86_64                                6.0.3-7.el7                                base                                 74 k

Transaction Summary
====================================================================================================================================================================
Install  1 Package

Total download size: 74 k
Installed size: 190 k
Downloading packages:
dos2unix-6.0.3-7.el7.x86_64.rpm                                                                                                              |  74 kB  00:00:01     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : dos2unix-6.0.3-7.el7.x86_64                                                                                                                      1/1 
  Verifying  : dos2unix-6.0.3-7.el7.x86_64                                                                                                                      1/1 

Installed:
  dos2unix.x86_64 0:6.0.3-7.el7                                                                                                                                     

Complete!
```





<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/12-%E9%94%99%E8%AF%AF-Linux%E7%8E%AF%E5%A2%83%E8%BF%90%E8%A1%8CShell%E8%84%9A%E6%9C%AC%E5%87%BA%E7%8E%B0$'r'%20command%20not%20found.png?raw=true)

上图是由 Pic 生成的

关键词：A peaceful beach at sunset with gentle waves and a colorful sky

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





