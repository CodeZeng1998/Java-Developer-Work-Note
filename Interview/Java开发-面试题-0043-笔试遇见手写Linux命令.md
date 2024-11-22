# Java开发-面试题-0043-笔试遇见手写Linux命令

> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



最近刷了一家广州的公司的1面的笔试，遇见了几道简单的笔试内容，分享出来。（**方法不只一种，大家记得常用的即可**）





**linux命令题目** 

**（1）查看当前目录下文件名含有.log的文件列表**

**（2）重命名文件**

**（3）查看tomcat进城情况**

**（4）压缩/解压**

**（5）查看系统当前负荷（CPU、内存等）**







### **（1）查看当前目录下文件名含有 `.log` 的文件列表**

#### **命令**：

```bash
ls | grep '\.log'
```

#### **详细说明**：

- `ls`：列出当前目录中的所有文件。
- `grep '\.log'`：筛选出文件名中包含 `.log` 的文件。

#### **其他方法**：

- 如果仅需列出当前目录中以 `.log` 结尾的文件：

  ```bash
  ls *.log
  ```

- 如果需要递归查找所有目录下的 `.log` 文件：

  ```bash
  find . -name "*.log"
  ```

------

### **（2）重命名文件**

#### **单个文件重命名**：

```bash
mv old_name.txt new_name.txt
```

#### **批量重命名（扩展名替换）**：

将当前目录下所有 `.txt` 文件重命名为 `.log`：

```bash
for file in *.txt; do
    mv "$file" "${file%.txt}.log"
done
```

#### **使用 `rename` 工具批量重命名**：

- 将 `.txt`文件扩展名改为 `.log`：

  ```bash
  rename 's/\.txt$/\.log/' *.txt
  ```

------

### **（3）查看 Tomcat 进程情况**

#### **方法 1：使用 `ps` 命令**：

```bash
ps -ef | grep tomcat
```

#### **方法 2：查看端口（Tomcat 默认监听 8080）**：

```bash
netstat -tuln | grep 8080
```

#### **方法 3：使用 `jps` 查看 Java 进程**：

```bash
jps -l | grep catalina
```

#### **方法 4：动态查看进程（结合 `top`）**：

1. 输入 `top` 命令后，按 `/` 键搜索 `tomcat`。
2. 或者使用 `htop`（需安装）并在界面中查找 `tomcat`。

------

### **（4）压缩/解压**

#### **压缩文件**

1. **使用 `tar` 创建压缩文件**：

   ```bash
   tar -czvf archive.tar.gz file1 file2 directory/
   ```

   - `-c`：创建压缩文件。
   - `-z`：使用 gzip 压缩。
   - `-v`：显示压缩过程。
   - `-f`：指定压缩文件名。

2. **压缩多个文件为 zip 格式**：

   ```bash
   zip archive.zip file1 file2
   ```

#### **解压文件**

1. **解压 `.tar.gz` 文件**：

   ```bash
   tar -xzvf archive.tar.gz
   ```

2. **解压 `.zip` 文件**：

   ```bash
   unzip archive.zip
   ```

------

### **（5）查看系统当前负荷（CPU、内存等）**

#### **方法 1：使用 `top`**

```bash
top
```

- 动态显示系统运行信息，包括 CPU、内存、负载等。

#### **方法 2：使用 `htop`**

```bash
htop
```

- 更直观的动态监控工具（需安装）。

#### **方法 3：使用 `uptime` 查看系统负载**

```bash
uptime
```

- 输出示例：

  ```
  14:30:15 up 10 days,  4:03,  2 users,  load average: 0.10, 0.20, 0.30
  ```

#### **方法 4：使用 `free` 查看内存使用情况**

```bash
free -h
```

- 输出内存使用的总量、已用量和空闲量。

#### **方法 5：查看 CPU 信息**

- 查看 CPU 使用情况：

  ```bash
  mpstat
  ```

- 查看每个 CPU 核心的使用率：

  ```bash
  mpstat -P ALL
  ```

------

**总结：** 根据任务需求选择合适的命令，以上方法均为高效处理文件和系统监控的常用手段。







<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0043.png?raw=true)

上图是由 Pic 生成的

关键词：Cute electric dinosaur

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



