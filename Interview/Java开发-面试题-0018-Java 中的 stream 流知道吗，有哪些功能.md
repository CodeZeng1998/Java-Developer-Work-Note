# Java开发-面试题-0018-Java 中的 stream 流知道吗，有哪些功能



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





Java 中的 stream 流知道吗，有哪些功能？



Java 中的 `Stream` 流是 Java 8 引入的一种新功能，用于对集合（如 `List`、`Set` 等）进行复杂的数据处理。`Stream` 提供了一种声明性的方法来处理数据，类似于 SQL 查询，可以极大简化代码。以下是 `Stream` 的主要功能：

### 1. **创建流（Creating Streams）**

- 从集合创建流：

  ```java
  List<String> list = Arrays.asList("a", "b", "c");
  Stream<String> stream = list.stream();
  ```

- 从数组创建流：

  ```java
  String[] array = {"a", "b", "c"};
  Stream<String> stream = Arrays.stream(array);
  ```

- 通过 `Stream.of()` 创建流：

  ```java
  Stream<String> stream = Stream.of("a", "b", "c");
  ```

### 2. **中间操作（Intermediate Operations）**

中间操作返回的是一个新的 `Stream`，它们是惰性求值的，即只有在终止操作执行时才会真正处理数据。常见的中间操作有：

- `filter()`：根据条件过滤元素。

  ```java
  Stream<String> filtered = stream.filter(s -> s.startsWith("a"));
  ```

- `map()`：将元素转换为另一种形式。

  ```java
  Stream<Integer> lengths = stream.map(String::length);
  ```

- `sorted()`：对元素进行排序。

  ```java
  Stream<String> sorted = stream.sorted();
  ```

- `distinct()`：去重操作。

  ```java
  Stream<String> distinct = stream.distinct();
  ```

- `limit()` 和 `skip()`：限制流中元素数量或跳过前 N 个元素。

  ```java
  Stream<String> limited = stream.limit(3);
  ```

### 3. **终止操作（Terminal Operations）**

终止操作会触发流的处理，并返回结果，终止操作执行后流会关闭。常见的终止操作有：

- `forEach()`：对每个元素执行操作。

  ```java
  stream.forEach(System.out::println);
  ```

- `collect()`：将流转换为集合或其他类型的数据。

  ```java
  List<String> list = stream.collect(Collectors.toList());
  ```

- `count()`：计算流中的元素数量。

  ```java
  long count = stream.count();
  ```

- `anyMatch()`、`allMatch()`、`noneMatch()`：根据条件检查流中的元素是否匹配。

  ```java
  boolean anyMatch = stream.anyMatch(s -> s.startsWith("a"));
  ```

- `findFirst()` 和 `findAny()`：返回流中的第一个或任意一个元素。

  ```java
  Optional<String> first = stream.findFirst();
  ```

### 4. **短路操作（Short-circuiting Operations）**

- 某些操作，如 `limit()`、`findFirst()`、`anyMatch()` 等，可能在处理部分元素后就得到结果并终止后续的处理，从而提高效率。

### 5. **并行流（Parallel Streams）**

- `Stream` 允许并行处理数据，通过 `parallelStream()` 可以轻松创建并行流。

  ```java
  List<String> list = Arrays.asList("a", "b", "c");
  list.parallelStream().forEach(System.out::println);
  ```

### 总结

Java 的 `Stream` 流提供了丰富的操作集合，通过链式调用可以轻松实现复杂的数据处理逻辑，同时使代码更加简洁和易读。





以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0018.png?raw=true)

上图是由 Pic 生成的

关键词：A man holding a submachine gun and smoking a cigarette walking on an empty street

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

