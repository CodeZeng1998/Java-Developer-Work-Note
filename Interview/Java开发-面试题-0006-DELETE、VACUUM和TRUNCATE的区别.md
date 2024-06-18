# Java开发-面试题-0006-DELETE、VACUUM和TRUNCATE的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



### DELETE 和 TRUNCATE 的区别

**DELETE** 和 **TRUNCATE** 是两种在关系数据库中用于删除数据的操作，它们的主要区别如下：

1. **DELETE**：
   - **操作**：DELETE 是一种 DML（数据操作语言）命令，用于从表中逐行或者批量删除数据。
   - **事务**：DELETE 可以作为事务的一部分，可以回滚（ROLLBACK），因此可以恢复被误删的数据。
   - **触发器**：DELETE 操作会触发相关的触发器（如果定义了触发器）。
   - **空间释放**：**DELETE 删除的数据页并不会立即释放磁盘空间，而是被标记为可重用。实际的磁盘空间会在以后的操作中被重用，或者在执行 VACUUM 操作时才会被释放。**
   
2. **TRUNCATE**：
   - **操作**：TRUNCATE 是一种 DDL（数据定义语言）命令，用于快速删除表中的所有数据。
   - **事务**：TRUNCATE 不能回滚，即使在事务内部也不例外。
   - **触发器**：TRUNCATE 不会触发相关的触发器。
   - **空间释放**：**TRUNCATE 立即释放表所占用的磁盘空间，因为它直接操作底层存储结构，将数据页全部标记为空闲状态。**
   
   

### DELETE 和 VACUUM 实现类似 TRUNCATE 的功能

虽然 DELETE 和 TRUNCATE 有明显的区别，但可以通过结合 DELETE 和 VACUUM 来实现类似 TRUNCATE 的功能，即删除表中的所有数据并立即释放磁盘空间。这种方法是在执行 DELETE 后立即执行 VACUUM 操作。

示例 SQL 语句如下：

```sql
-- 删除表中的所有数据
DELETE FROM 表名;

-- 执行 VACUUM 操作以释放磁盘空间
VACUUM 表名;
```

上述 SQL 语句中，首先使用 DELETE 删除表中的所有数据，然后立即执行 VACUUM 操作。这样可以立即释放已删除数据所占用的磁盘空间，达到类似 TRUNCATE 的效果。

### 注意事项：

- DELETE 和 TRUNCATE 适用于不同的场景和需求，应根据具体情况选择合适的操作。
- 使用 DELETE 后立即执行 VACUUM 可以释放磁盘空间，但可能会导致一定的性能开销，特别是在大型表上。
- 在某些数据库管理系统中，也许有其他优化方法或者自动化工具可以帮助优化数据管理和空间利用。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0006.png?raw=true)

上图由 Pic 生成

关键词：Aurora, Snowy Mountains, Blues Moments





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**