

[TOC]



# Linux



# 1.防火墙-相关命令

```shell
# 查看防火墙状态
firewall-cmd --state
# 开启防火墙
systemctl start firewalld.service
# 开启指定端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent

# 显示 success 表示成功
# –zone=public 表示作用域为公共的
# –add-port=443/tcp 添加 tcp 协议的端口端口号为 443
# –permanent 永久生效，如果没有此参数，则只能维持当前 服 务生命周期内，重新启动后失效；

# 重启防火墙
systemctl restart firewalld.service

# 重新加载防火墙
firewall-cmd --reload

#查看已开启的端口
firewall-cmd --list-ports

#关闭指定端口
firewall-cmd --zone=public --remove-port=8080/tcp --permanent
systemctl restart firewalld.service
firewall-cmd --reload

#查看端口被哪一个进程占用
netstat -lnpt |grep 5672
# centos7默认没有 netstat 命令，需要安装 net-tools 工具：
# 安装 net-tools
yum install -y net-tools

# 临时关闭防火墙
systemctl stop firewalld.service
# 或者
systemctl stop firewalld


# 永久关闭防火墙（必须先临时关闭防火墙，再执行该命令，进行永久关闭）
systemctl disable firewalld.service
# 或者
systemctl disable firewalld
```



# 2.Linux信息

```shell
# 内存
cat /proc/meminfo | head -n 1 | awk '{print $2/(1024 * 1024),"G"}'
# 硬盘
fdisk -l |grep Disk
# CPU
lscpu
```





# 3.文件相关

## （1）tar

```shell
# 解压文件 tar -zxcf archive.tar.gz
tar -zxcf 解压文件名.tar.gz

# 压缩文件 tar -zcvf my_archive.tar.gz my_directory
tar -zcvf 压缩文件名.tar.gz 文件或目录路径
```



## （2）xz

```shell
# 压缩tar.xz文件（tar.xz文件：只要先 tar cvf xxx.tar xxx/ 这样创建xxx.tar文件先，然后使用 xz -z xxx.tar 来将 xxx.tar压缩成为 xxx.tar.xz）
tar -zcvf 压缩文件名.tar.gz 文件或目录路径
xz -z 压缩文件名.tar

# 解压tar.xz文件（解压tar.xz文件：先 xz -d xxx.tar.xz 将 xxx.tar.xz解压成 xxx.tar 然后，再用 tar xvf xxx.tar来解包）
xz -d 解压文件名.tar.xz
tar -zxcf 解压文件名.tar
```



## （3）zip

```shell
# 解压文件
unzip 压缩文件名.zip
# 解压文件到指定目录
unzip 压缩文件名.zip -d 目标目录路径

# 压缩文件（将文件1 文件2 文件3 压缩到zip中）
zip 压缩文件名.zip 文件1 文件2 文件3 ...
# 压缩文件（将整个目录及其所有子目录和文件压缩到zip中）
zip -r 压缩文件名.zip 目录路径
```



   



# 4.日期时间

## （1）修改系统时间

```shell
# 查看当前时区
date -R

# 修改设置Linux服务器时区 Asia -> China -> Beijing Time
tzselect

# 复制相应的时区文件，替换系统时区文件；或者创建链接文件 
cp /usr/share/zoneinfo/$主时区/$次时区 /etc/localtime

# 在设置中国时区使用亚洲/上海（+8）
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime


# 查看时间和日期
date

# 将系统日期设定成2023年07月12日的命令   月/日/年
date -s 07/12/2023
# 将系统时间设定成下午5点55分55秒的命令 时:分:秒
date -s 17:55:55
# 将当前时间和日期写入BIOS，避免重启后失效
hwclock -w
```









# 5. 查找相关

## （1）查找文件

```shell
# 查找 filePath 下的名字包含 searchName 的文件
find <filePath> -name "*<searchName>*"
```



## （2）查找内存

```shell
# 找出 Linux 系统上内存占用最多的前 5 个进程
ps aux --sort -rss | head -5
```





# 6. 统相关关

## （1）统计数量

```shell
# 统计当前目录下包含XXX文件的数量 
# wc -l 命令统计找到的文件数
find . -name "*XXX*" | wc -l
find <filePath> -name "*<searchName>*" | wc -l

```

























