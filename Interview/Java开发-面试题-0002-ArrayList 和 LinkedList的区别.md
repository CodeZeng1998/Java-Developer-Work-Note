# Java开发-面试题-0002-ArrayList 和 LinkedList的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





ArrayList 和 LinkedList的区别：

* 底层数据结构
  * ArrayList：使用**动态数组**存储元素。它在内部使用数组来存储元素，并支持根据需要动态调整数组大小。
  * LinkedList：实现了**双向链表**数据结构，LinkedList 中的每个元素都作为一个名为“节点”的单独对象存储，每个节点都包含对前一个和后一个节点的引用。
* 访问时间
  * ArrayList：通过索引对元素进行快速随机访问。在 ArrayList 中通过索引检索元素的时间复杂度为 **O(1)**。
  * LinkedList：在 LinkedList 中通过索引访问元素较慢，因为它需要从链表的开头或结尾遍历列表以达到所需的索引。在 LinkedList 中通过索引访问元素的时间复杂度为 **O(n)**。
* 插入和删除
  * ArrayList：在列表末尾进行插入和删除操作非常高效，因为它不需要移动元素。但是，在列表开头或中间进行插入和删除元素可能效率偏低，因为它需要**移动后续元素**。
  * LinkedList：相比 ArrayList，在 LinkedList 中插入和删除元素通常更高效，特别是在列表的开头或中间进行操作。这是因为在 LinkedList 中添加或者删除节点只需要**更新相邻节点的引用**。
* 内存使用
  * ArrayList：与 LinkedList 相比，每个元素的内存开销较低，因为它不需要存储对前一个和后一个元素的引用。
  * LinkedList：需要额外的内存来存储对前一个和后一个元素的引用，因此内存开销较大。
* 遍历元素
  * ArrayList：使用迭代起或增强 for 循环遍历元素在 ArrayList 中非常高效，因为它的存储是连续的。
  * LinkedList：与 ArrayList 相比，在 LinkedList 中遍历元素较慢，因为它涉及从一个节点到另一个节点的引用跟随。

总的来说，选择 ArrayList 还是 LinkedList 取决于应用程序的具体要求，例如列表上执行的操作平率和类型、内存约束以及性能考虑。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0002.png?raw=true)

上图由 Pic 生成



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

