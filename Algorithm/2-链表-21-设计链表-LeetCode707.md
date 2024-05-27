# 2-链表-21-设计链表-LeetCode707

> 参考：代码随想录
>
> LeetCode: 题目序号707



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

你可以选择使用单链表或者双链表，设计并实现自己的链表。

单链表中的节点应该具备两个属性：`val` 和 `next` 。`val` 是当前节点的值，`next` 是指向下一个节点的指针/引用。

如果是双向链表，则还需要属性 `prev` 以指示链表中的上一个节点。假设链表中的所有节点下标从 **0** 开始。

实现 `MyLinkedList` 类：

- `MyLinkedList()` 初始化 `MyLinkedList` 对象。
- `int get(int index)` 获取链表中下标为 `index` 的节点的值。如果下标无效，则返回 `-1` 。
- `void addAtHead(int val)` 将一个值为 `val` 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
- `void addAtTail(int val)` 将一个值为 `val` 的节点追加到链表中作为链表的最后一个元素。
- `void addAtIndex(int index, int val)` 将一个值为 `val` 的节点插入到链表中下标为 `index` 的节点之前。如果 `index` 等于链表的长度，那么该节点会被追加到链表的末尾。如果 `index` 比长度更大，该节点将 **不会插入** 到链表中。
- `void deleteAtIndex(int index)` 如果下标有效，则删除链表中下标为 `index` 的节点。

 

**示例：**

```
输入
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
输出
[null, null, null, null, 2, null, 3]

解释
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // 链表变为 1->2->3
myLinkedList.get(1);              // 返回 2
myLinkedList.deleteAtIndex(1);    // 现在，链表变为 1->3
myLinkedList.get(1);              // 返回 3
```

 

**提示：**

- `0 <= index, val <= 1000`
- 请不要使用内置的 LinkedList 库。
- 调用 `get`、`addAtHead`、`addAtTail`、`addAtIndex` 和 `deleteAtIndex` 的次数不超过 `2000` 。



```java
class MyLinkedList {

    // 链表元素个数
    int size;
    // 虚拟头结点，方便操作
    ListNode head;

    /**
     * 初始化链表
     *
     * @author: CodeZeng1998
     */
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    /**
     * 获取指定索引的元素值。
     *
     * @param index 元素的索引
     * @return 如果索引有效，返回对应元素的值，否则返回-1。
     * @author: CodeZeng1998
     */
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode currentListNode = head;

        // 遍历至目标索引位置
        for (int i = 0; i <= index; i++) {
            currentListNode = currentListNode.next;
        }
        return currentListNode.val;
    }

    /**
     * 在链表头部添加一个元素。
     *
     * @param val 要添加的元素值
     * @author: CodeZeng1998
     */
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    /**
     * 在链表尾部添加一个元素。
     *
     * @param val 要添加的元素值
     * @author: CodeZeng1998
     */
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    /**
     * 在指定位置添加一个元素。
     *
     * @param index 插入位置的索引
     * @param val 要添加的元素值
     * @author: CodeZeng1998
     */
    public void addAtIndex(int index, int val) {
        // 如果索引超出范围，则直接返回
        if (index > size) {
            return;
        }

        // 如果索引小于0，则将索引调整为0
        if (index < 0) {
            index = 0;
        }

        size++;

        ListNode preListNode = head;
        // 定位到插入位置的前一个节点
        for (int i = 0; i < index; i++) {
            preListNode = preListNode.next;
        }

        ListNode toAddListNode = new ListNode(val);
        // 进行插入操作
        toAddListNode.next = preListNode.next;
        preListNode.next = toAddListNode;
    }

    /**
     * 删除指定索引位置的元素。
     *
     * @param index 要删除元素的索引
     * @author: CodeZeng1998
     */
    public void deleteAtIndex(int index) {
        // 如果索引无效，则直接返回
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        // 如果删除的是头结点
        if (index == 0) {
            head = head.next;
            return;
        }

        ListNode pred = head;
        // 定位到待删除节点的前一个节点
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        // 进行删除操作
        pred.next = pred.next.next;
    }

    // 链表节点类
    class ListNode {
        int val;
        ListNode next;

        ListNode() {}

        ListNode(int val) {
            this.val = val;
        }
    }
}
```

ChatGPT：

这个 Java 类表示了一个名为 `MyLinkedList` 的单链表数据结构。让我们逐个解释每个方法的功能：

1. **构造方法**：
   - 初始化链表的大小为 0，并创建一个值为 0 的虚拟头节点。这个虚拟头节点简化了对链表的操作。
2. **get(int index)**：
   - 获取指定索引处节点的值。
   - 如果索引超出了范围，则返回 -1。
   - 它从虚拟头节点开始迭代链表，直到达到所需的索引。
3. **addAtHead(int val)**：
   - 在链表开头插入一个新节点，给定值为 val。这相当于在索引 0 处添加一个节点。
4. **addAtTail(int val)**：
   - 在链表末尾插入一个新节点，给定值为 val。这相当于在索引 `size` 处添加一个节点。
5. **addAtIndex(int index, int val)**：
   - 在链表的指定索引处插入一个新节点，给定值为 val。
   - 如果索引超出了范围，则不执行任何操作。
   - 它通过遍历链表来找到所需位置前一个索引处的节点，然后在其后插入新节点。
6. **deleteAtIndex(int index)**：
   - 删除链表中指定索引处的节点。
   - 如果索引超出了范围，则不执行任何操作。
   - 如果索引为 0，则更新虚拟头节点指向第二个节点。
   - 它通过遍历链表来找到所需位置前一个索引处的节点，然后更新其 `next` 指针以跳过要删除的节点。
7. **ListNode 类**：
   - 表示链表中的一个节点。
   - 包含一个整数值（`val`）和一个指向下一个节点的引用（`next`）。

这个类提供了对单链表进行基本操作的方法，如插入、删除和检索。





[707. Design Linked List](https://leetcode.cn/problems/design-linked-list/)

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

- `MyLinkedList()` Initializes the `MyLinkedList` object.
- `int get(int index)` Get the value of the `indexth` node in the linked list. If the index is invalid, return `-1`.
- `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
- `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
- `void addAtIndex(int index, int val)` Add a node of value `val` before the `indexth` node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
- `void deleteAtIndex(int index)` Delete the `indexth` node in the linked list, if the index is valid.

 

**Example 1:**

```
Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
```

 

**Constraints:**

- `0 <= index, val <= 1000`
- Please do not use the built-in LinkedList library.
- At most `2000` calls will be made to `get`, `addAtHead`, `addAtTail`, `addAtIndex` and `deleteAtIndex`.





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
