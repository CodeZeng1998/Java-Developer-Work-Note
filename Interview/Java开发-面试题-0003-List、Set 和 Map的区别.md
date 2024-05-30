# Java开发-面试题-0003-List、Set 和 Map的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**







List、Set 和 Map的区别：

* 基本特性
  * List
    * **有序**集合，可以包含重复元素。
    * 每个元素都有索引，通过索引可以访问元素。
  * Set
    * 无序集合，不允许包含重复元素。
    * 每个元素都是唯一的。
  * Map
    * **键值对**的集合，**不允许键重复**。
    * 每个键关联一个值，可以通过键来访问对应的值。
* 常用实现
  * List
    * ArrayList、LinkedList、Vector
  * Set
    * HashSet、LinkedHashSet、TreeSet
  * Map
    * HashMap、LinkedHashMap、TreeMap、Hashtable
* 元素访问
  * List
    * 通过索引访问元素，支持随机访问。
    * 在 ArrayList 中通过索引访问元素的时间复杂度为 O(1)，在 LinkedList 中为 O(n)。
  * Set
    * 没有索引，不能通过索引访问元素。
    * 需要通过迭代器或增强 for 循环遍历。
  * Map
    * 通过键访问对应的值，支持高效的键值查找。
    * 在 HashMap 中查找键的时间复杂度为 O(1)， 在 TreeMap 中 为 O(log n)。
* 插入和删除
  * List
    * 在列表末尾进行插入和删除操作高效。
    * 在 ArrayList 中间位置插入和删除操作效率低，因为需要移动元素。在 LinkedList 中任意位置插入和删除操作较高效，但查找元素位置效率低，因为需要逐一遍历。
  * Set
    * 插入和删除操作效率较高。
    * 在 HashSet 中插入和删除的时间复杂度为O(1)，在 TreeSet 中为O(log n)。
  * Map
    * 插入和删除键值对操作效率较高。
    * 在 HashMap 中插入和删除键值对的时间复杂度为O(1)，在 TreeMap 中为 O(log n)。
* 重复元素
  * List
    * **允许包含重复元素**。
  * Set
    * **不允许包含重复元素**。如果尝试添加重复元素，添加操作将失败。
  * Map
    * **键不允许重复。值可以重复。**
* 顺序
  * List
    * **维护元素的插入顺序**。
  * Set
    * 大多数实现（如 HashSet）不维护顺序， LinkedHashSet 维护插入顺序， TreeSet 按自然顺序或指定的比较器顺序排序。
  * Map
    * HashMap 不维护顺序，LinkedHashMap 维护插入顺序，TreeMap按自然顺序或指定的比较器顺序排序。
* 用途
  * List
    * 适用于需要按特定顺序访问元素或需要频繁按索引访问元素的场景。
  * Set
    * 适用于需要保证元素唯一性或需要快速查找元素的场景。
  * Map
    * 适用于需要通过键值对进行快速查找、插入和删除操作的场景。
* 内存和使用
  * List
    * 内存使用取决于具体实现。ArrayList 相对高效，因为它只存储元素的值。LinkedList由于存储了节点的引用，内存开销较大。
  * Set
    * 内存使用取决于具体实现。HashSet 通常比 TreeSet 内存开销小，因为 TreeSet 需要维护红黑树结构。
  * Map
    * 内存使用取决于具体实现。HashMap 通常比 TreeMap内存开销小，因为 TreeMap 需要维护红黑树结构。

总的来说，选择 List、Set 还是 Map 取决于具体需求，例如是否需要允许重复元素、是否需要维护顺序、是否需要快速查找和删除、是否需要键值对的映射等。









![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0003.png?raw=true)

上图由 Pic 生成









**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

