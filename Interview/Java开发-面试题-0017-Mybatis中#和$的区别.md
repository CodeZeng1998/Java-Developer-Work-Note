# Java开发-面试题-0017-Mybatis中#和$的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





在 MyBatis 中，`#` 和 `$` 用于处理 SQL 参数的方式不同，具体区别如下：

### `#` 占位符：

- **作用**：`#` 是 MyBatis 的占位符，它会使用 `PreparedStatement` 预编译 SQL 语句，并将参数值安全地绑定到占位符上。

- **特点**：参数会被自动转义，防止 SQL 注入。

- **使用场景**：适用于传递普通参数，如字符串、数字等。

- 示例：

  ```xml
  SELECT * FROM users WHERE name = #{name}
  ```

  如果 `name = "John"`，实际执行的 SQL 是：

  ```sql
  SELECT * FROM users WHERE name = 'John'
  ```



### `$` 字符串替换：

- **作用**：`$` 是 MyBatis 的字符串替换符，它会将参数的值直接拼接到 SQL 语句中，不会进行转义处理。

- **特点**：容易引发 SQL 注入风险，因此使用时需格外谨慎。

- **使用场景**：通常用于传递==动态表名、字段名==等，而不是普通参数。

- 示例

  ：

  ```xml
  SELECT * FROM ${tableName}
  ```

  如果 `tableName = "users"`，实际执行的 SQL 是：

  ```sql
  SELECT * FROM users
  ```

### 总结：

- **`#`**：安全、推荐使用，大部分情况下应使用 `#` 来避免 SQL 注入风险。
- **`$`**：不安全，**只应在确定输入安全的情况下使用，如动态拼接表名或字段名**。==（一般会在分表的时候使用，主动传入分表表名进行数据操作）==

确保在使用 `$` 时，输入数据来自可信的来源，否则会存在严重的安全隐患。









以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0017.png?raw=true)

上图是由 豆包 生成的

关键词：帮我生成图片：图片风格为赛博朋克，一个人长着一个番茄的脑袋

<br/>



PS：今天的图片相当辣眼睛，哈哈哈。



<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

