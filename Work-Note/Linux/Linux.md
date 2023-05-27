

[TOC]



# Linux



## 1.防火墙-相关命令

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



## 2.Linux信息

```shell
# 内存
cat /proc/meminfo | head -n 1 | awk '{print $2/(1024 * 1024),"G"}'
# 硬盘
fdisk -l |grep Disk
# CPU
lscpu
```





## 3.Nginx相关

```shell
# 1.启动Nginx
# /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
nginx安装目录地址 -c nginx配置文件地址


# 2.停止Nginx

# （1）从容停止
# 查看进程号
ps -ef|grep nginx
# 杀死进程
kill -quit 进程号

# （2）快速停止
ps -ef|grep nginx
# 杀死进程
kill -term 进程号/ kill -int 进程号

# （3）强制停止
pkill -9 nginx


# 3.重启
# 3.1验证nginx配置文件是否正确
# （1）方法一：进入nginx安装目录sbin下，输入命令./nginx -t
./nginx -t
# （2）方法二：在启动命令-c前加-t
/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf

# 3.2重启nginx服务
# （1）方法一：进入nginx安装目录sbin下，输入命令./nginx -s reload 即可
./nginx -s reload
# （2）方法二：查找当前nginx进程号，然后输入命令：kill -HUP 进程号 实现重启nginx服务
ps -ef|grep nginx
kill -HUP 进程号

# 查看nginx版本
nginx -version
nginx -v

# 快速关机
nginx -s stop

#正常关机
nginx -s quit
#重新打开日志文件
nginx -s reopen
```

