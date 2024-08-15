# Java开发-面试题-0016-HashMap的遍历方式，知道几种说几种



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



很简单的一道面试题，问HashMap的几种遍历方式。



HashMap的遍历方式：

1. **通过 `entrySet()` 使用 `for-each` 循环：**

   ```java
   HashMap<String, Integer> map = new HashMap<>();
   for (Map.Entry<String, Integer> entry : map.entrySet()) {
       String key = entry.getKey();
       Integer value = entry.getValue();
       // 对 key 和 value 进行处理
   }
   ```

   这种方式最常见且高效，可以同时获取键和值。

   

2. **通过 `keySet()` 使用 `for-each` 循环：**

   ```java
   HashMap<String, Integer> map = new HashMap<>();
   for (String key : map.keySet()) {
       Integer value = map.get(key);
       // 对 key 和 value 进行处理
   }
   ```

   这种方式只遍历键，使用 `map.get(key)` 获取值。相对效率较低，因为每次获取值时都要查找一次。

   

3. **通过 `values()` 使用 `for-each` 循环：**

   ```java
   HashMap<String, Integer> map = new HashMap<>();
   for (Integer value : map.values()) {
       // 只处理值
   }
   ```

   这种方式仅遍历所有值，如果只关心值，这种方式是合适的。

   

4. **通过 `Iterator` 遍历 `entrySet()`：**

   ```java
   HashMap<String, Integer> map = new HashMap<>();
   Iterator<Map.Entry<String, Integer>> iterator = map.entrySet().iterator();
   while (iterator.hasNext()) {
       Map.Entry<String, Integer> entry = iterator.next();
       String key = entry.getKey();
       Integer value = entry.getValue();
       // 对 key 和 value 进行处理
   }
   ```

   这种方式与 `for-each` 类似，但可以在遍历时安全地移除元素。

   

5. **通过 Java 8 的 `forEach()` 方法：**

   ```java
   HashMap<String, Integer> map = new HashMap<>();
   map.forEach((key, value) -> {
       // 对 key 和 value 进行处理
   });
   ```

   使用 `Lambda` 表达式，非常简洁且现代化。

这些方法各有优势，选择哪种方式主要取决于具体场景。









以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0016.png?raw=true)

上图是由 豆包 生成的

关键词：帮我生成图片：图片风格为模仿名画《呐喊》，西装暴徒叼着香烟带着墨镜

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

