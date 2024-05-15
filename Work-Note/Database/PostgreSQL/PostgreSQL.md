# PostgreSQL

[TOC]





# 1.用户权限相关

```sql
# 连接到数据库
psql -U postgres

# 创建用户
CREATE USER 用户名 WITH PASSWORD '密码';

# 授予用户读取数据库中所有的数据
GRANT pg_read_all_data TO 用户名;

# 删除用户
DROP USER 用户名;

# 重新加载 PostgreSQL 配置文件的 SQL 命令 
SELECT pg_reload_conf();
```

