# Java开发-实际工作经验和技巧-0001-PostgreSQL数据库存储磁盘满了重启以及应急措施

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





**PostgreSQL数据库所在磁盘满了，要怎么重启？**

* 问题描述：PostgreSQL数据库所在磁盘满了，导致服务挂了，现需重启。

```shell
[root@0004 config]# df -hT
Filesystem                 Type      Size  Used Avail Use% Mounted on
/dev/vdb1                  xfs       2.0T  2.0T  20M  100% /data
```

* 解决方案：删除掉一些数据库存储数据所在磁盘路径中无用的文件。(不腾出一点空间无法正常重启)

进行对应的重启操作

```shell
# 进入PostgreSQL数据库所在的路径
cd /xxx/postgresql/bin/

# 使用超级用户（输入密码）
su postgres

# 启动 PostgreSQL 数据库服务
# -D 选项指定 PostgreSQL 实例的数据目录位置。
# -l 选项指定服务器日志输出应写入的文件
./pg_ctl -D 数据存储目录 -l XXX.log start

# 关闭 PostgreSQL 数据库服务
./pg_ctl stop
```



**应急方案：**

重启之后就需要将数据库存储数据所在目录腾出空间，否则运行一下会又会出现上述问题。

* 可直接删除无用的备份表或者数据（记得使用**TRUNCATE**或者**DELETE+VACUUM**的组合，条件允许建议使用**TRUNCATE**）

* 如果删除无用数据或者无用的备份表还是没法让服务运行起来的话，可以暂时先将不常用的数据备份到非数据库存储路径所在磁盘上，然后使用**TRUNCATE**或者**DELETE+VACUUM**的组合，将对应的备份好的的数据空间空出来，保证服务正常运行，后续进行对应的备份数据即可。

  ```shell
  # PostgreSQL 数据库可以使用 pg_dump 直接对数据进行快速备份
  pg_dump -h 主机名 -U 用户名 -d 数据库名 -t 待备份的数据库表名 -f /输出路径/XXX.sql
  ```

* 后续肯定是需要进行对应的磁盘扩容的，从根本上解决问题。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0001-PostgreSQL%E6%95%B0%E6%8D%AE%E5%BA%93%E5%AD%98%E5%82%A8%E7%A3%81%E7%9B%98%E6%BB%A1%E4%BA%86%E9%87%8D%E5%90%AF%E4%BB%A5%E5%8F%8A%E5%BA%94%E6%80%A5%E6%8E%AA%E6%96%BD.png?raw=true)

上图由 Pic 生成

关键词：zoo





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**