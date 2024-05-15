# Nginx

[TOC]







# 1.Linux相关

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





