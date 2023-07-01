# MongoDB

[TOC]



# 1.Linux 相关

```shell
# 检查MongoDB是否正在运行
systemctl status mongod

# 查看mongodb部署位置
ps -ef | grep mongod
# mongodb   1234     1  0 Jun26 ?        00:00:10 /usr/bin/mongod --config /etc/mongod.conf
# 部署位置 /usr/bin/mongod 配置文件位置 /etc/mongod.conf


# 查看MongoDB版本
mongod --version

# 连接到MongoDB Shell
mongo

# 显示所有数据库
show dbs

# 创建或切换数据库
use <database_name>

# 显示当前数据库中的集合（表）
show collections

# 插入文档（数据）
db.<collection_name>.insertOne({ field1: value1, field2: value2 })

# 查询文档
# (1)查询所有
db.<collection_name>.find()
# （2）匹配筛选条件查询
db.<collection_name>.find({ field: value })
# （3）匹配筛选条件查询前十条
db.<collection_name>.find({ field: value }).limit(10)


# 更新文档
db.<collection_name>.updateOne({ field: value }, { $set: { field2: new_value } })

# 删除文档
db.<collection_name>.deleteOne({ field: value })


# 删除集合
db.<collection_name>.drop()

# 备份数据库
mongodump --db <database_name> --out <backup_directory>

# 恢复数据库
mongorestore --db <database_name> <backup_directory>
```
