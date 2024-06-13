# 3-异常-出现 PSQLException: Connection refused的8种可能



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



报错信息如下：

```
[09:19:59:553] [ERROR] - com.alibaba.druid.pool.DruidDataSource$CreateConnectionThread.run(DruidDataSource.java:2787) - create connection SQLException, url: jdbc:postgresql://127.0.0.1:5432/databaseName?useUnicode=true&useSSL=false&characterEncoding=utf8&useAffectedRows=true&allowMultiQueries=true&serverTimezone=GMT%2B8&useTimezone=true, errorCode 0, state 08001
org.postgresql.util.PSQLException: Connection to 127.0.0.1:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections.
	at org.postgresql.core.v3.ConnectionFactoryImpl.openConnectionImpl(ConnectionFactoryImpl.java:303) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.core.ConnectionFactory.openConnection(ConnectionFactory.java:51) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.jdbc.PgConnection.<init>(PgConnection.java:225) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.Driver.makeConnection(Driver.java:465) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.Driver.connect(Driver.java:264) ~[postgresql-42.2.18.jar:42.2.18]
	at com.alibaba.druid.filter.FilterChainImpl.connection_connect(FilterChainImpl.java:156) ~[druid-1.2.2.jar:1.2.2]
	at com.alibaba.druid.filter.stat.StatFilter.connection_connect(StatFilter.java:227) ~[druid-1.2.2.jar:1.2.2]
	at com.alibaba.druid.filter.FilterChainImpl.connection_connect(FilterChainImpl.java:150) ~[druid-1.2.2.jar:1.2.2]
	at com.alibaba.druid.pool.DruidAbstractDataSource.createPhysicalConnection(DruidAbstractDataSource.java:1654) ~[druid-1.2.2.jar:1.2.2]
	at com.alibaba.druid.pool.DruidAbstractDataSource.createPhysicalConnection(DruidAbstractDataSource.java:1718) ~[druid-1.2.2.jar:1.2.2]
	at com.alibaba.druid.pool.DruidDataSource$CreateConnectionThread.run(DruidDataSource.java:2785) [druid-1.2.2.jar:1.2.2]
Caused by: java.net.ConnectException: Connection refused (Connection refused)
	at java.net.PlainSocketImpl.socketConnect(Native Method) ~[?:1.8.0_333]
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:476) ~[?:1.8.0_333]
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:218) ~[?:1.8.0_333]
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:200) ~[?:1.8.0_333]
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:394) ~[?:1.8.0_333]
	at java.net.Socket.connect(Socket.java:606) ~[?:1.8.0_333]
	at org.postgresql.core.PGStream.createSocket(PGStream.java:231) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.core.PGStream.<init>(PGStream.java:95) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.core.v3.ConnectionFactoryImpl.tryConnect(ConnectionFactoryImpl.java:98) ~[postgresql-42.2.18.jar:42.2.18]
	at org.postgresql.core.v3.ConnectionFactoryImpl.openConnectionImpl(ConnectionFactoryImpl.java:213) ~[postgresql-42.2.18.jar:42.2.18]
	... 10 more
```





错误消息表明应用程序无法建立与位于 `127.0.0.1:5432` 的 PostgreSQL 数据库的连接。根本原因是连接被拒绝。以下是一些可能的原因及对应的解决方案：

1. **数据库服务器未运行**：

   - 确保 PostgreSQL 服务器在主机 `127.0.0.1` 上运行。
   - 使用类似 `systemctl status postgresql` 的命令在服务器上检查 PostgreSQL 服务的状态。

2. **端口未监听**：

   - 确认 PostgreSQL 配置为监听端口 `5432`。
   - 检查 `postgresql.conf` 文件（通常位于 PostgreSQL 数据目录中）的 `port` 配置，确保其设置为 `5432`。

3. **网络问题**：

   - 验证您的应用服务器与数据库服务器之间的网络连接。使用 `ping` 或 `telnet` 等工具检查连接。
   - 例如：`telnet 127.0.0.1 5432`（如果可用）。

4. **防火墙设置**：

   - 检查数据库服务器上的防火墙设置，确保端口 `5432` 已打开。
   - 使用 `iptables` 或 `firewalld` 等命令（取决于Linux发行版）检查和更新防火墙规则。

5. **PostgreSQL 配置**：

   - 确保 PostgreSQL 配置为接受 TCP/IP 连接。在 `postgresql.conf` 文件中，确保 `listen_addresses` 参数包含服务器的 IP 地址或设置为 `'*'` 以接受任何 IP 地址的连接。
   - 例如：`listen_addresses = '*'`

6. **客户端认证配置**：

   - 检查 `pg_hba.conf` 文件（位于 PostgreSQL 数据目录中），确保允许客户端 IP 地址或网络连接到数据库。

   - 在 `pg_hba.conf`中添加适当的条目以允许客户端 IP：

     ```css
     host    all             all             19.25.34.0/24            md5
     ```

7. **数据库凭证**：

   - 确保 JDBC URL 中使用的用户名和密码正确，并且用户具有连接到数据库的必要权限。







除此以外还有一个可能也是我这边遇见的问题，那就是**数据库的磁盘满了**。如下，通过linux命令查看数据库存储所用的，当数据库磁盘满了

```shell
[root@0004 data]# df -hT
Filesystem                 Type      Size  Used Avail Use% Mounted on
/dev/vdb1                  xfs       2.0T  2.0T  7.0G 100% /data
```

当**数据库服务器的磁盘空间用尽**时，PostgreSQL 可能无法正常运行或无法处理新的连接请求。这是因为数据库需要磁盘空间来执行各种操作，如写入日志、处理查询和事务等。如果磁盘空间不足，PostgreSQL 可能会拒绝新的连接或无法执行正常操作。



**解决方案**

1. **检查磁盘使用情况**：
   - 在数据库服务器上使用命令检查磁盘使用情况，例如：`df -h`
   - 查找 PostgreSQL 数据目录所在的磁盘分区，查看是否已满。
2. **清理磁盘空间**：
   - 删除不再需要的文件或日志，以释放磁盘空间。例如，删除旧的备份文件或归档日志：
   - 考虑启用日志轮换（log rotation）来自动管理日志文件。
3. **扩展磁盘分区**：
   - 如果可能，可以增加磁盘分区的大小或添加额外的存储空间。例如，在虚拟化环境中，可以分配更多的磁盘空间并扩展分区。
4. **调整数据库配置**：
   - 配置 PostgreSQL 以使用较少的磁盘空间。例如，通过调整 `log_rotation_size` 和 `log_rotation_age` 参数来控制日志文件的大小和轮换频率。
5. **定期维护**：
   - 定期监控和维护数据库服务器的磁盘使用情况，确保始终有足够的可用空间。
6. **重启数据库服务**：（**慎用！！！慎用！！！慎用！！**！）（**一般情况没必要重启数据库服务，只需要临时将无用的数据或者备份表先删除即可，后序进行对应的扩容。**）
   - 在清理或扩展磁盘空间后，重启 PostgreSQL 服务以恢复正常操作：`systemctl restart postgresql`

检查是否由于磁盘满引起的连接问题

要确认问题是否由于磁盘满引起，您可以查看 PostgreSQL 的日志文件。日志文件通常位于数据目录中的 `pg_log` 目录下，或者在配置文件中指定的路径。查找是否有与磁盘空间相关的错误消息。



找到对应的数据库服务的日志，PostgreSQL的数据库服务日志名称一般为postgresql.log 或者 postgresql-<date>.log

例如：`postgresql-Thu.log`，可以从日志信息中确定**数据库磁盘确实满了**

```
2024-06-13 06:54:27.264 CST [1443605] ERROR:  could not extend file "base/16385/799264.1": No space left on device
```





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/3-%E5%BC%82%E5%B8%B8-%E5%87%BA%E7%8E%B0%20PSQLException%20Connection%20refused%E7%9A%848%E7%A7%8D%E5%8F%AF%E8%83%BD.png?raw=true)

上图是由 Pic 生成的

关键词：A hesitant Tom sat by the beach smoking cigarettes and drinking, watching the sunset





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





