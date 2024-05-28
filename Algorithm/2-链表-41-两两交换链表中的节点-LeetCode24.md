# 2-链表-41-两两交换链表中的节点-LeetCode24

> 参考：代码随想录
>
> LeetCode: 题目序号24



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

 

**提示：**

- 链表中节点的数目在范围 `[0, 100]` 内
- `0 <= Node.val <= 100`



**链表结构**如下:

```java
    public class ListNode {
        int val;
        ListNode next;

        ListNode() {}

        ListNode(int val) {
            this.val = val;
        }

        ListNode(int val, ListNode next) {
            this.val = val;
            this.next = next;
        }
    }
```





```java
    /**
     * 两两交换链表中的节点
     *
     * @param head 待两两交换链表中的头节点
     * @return 两两交换链表后的头节点
     * @author CodeZeng1998
     */
    public ListNode swapPairs(ListNode head) {
        ListNode dumyhead = new ListNode(-1);
        dumyhead.next = head;
        ListNode tempListNode;
        ListNode firstListNode;
        ListNode secondListNode;
        ListNode currentListNode = dumyhead;

        while (currentListNode.next != null && currentListNode.next.next != null) {
            tempListNode = currentListNode.next.next.next;
            firstListNode = currentListNode.next;
            secondListNode = currentListNode.next.next;

            currentListNode.next = secondListNode;
            secondListNode.next = firstListNode;
            firstListNode.next = tempListNode;
            currentListNode = firstListNode;
        }

        return dumyhead.next;
    }
```

ChatGPT：

这段代码定义了一个名为 `swapPairs` 的方法，它接受一个 ListNode 类型的 head 作为输入，并返回一个 ListNode。

方法内部：

1. 创建了一个值为 -1 的新 ListNode，称为 `dumyhead`，并将其下一个节点设置为输入的 `head`。这个节点用作虚拟节点，以简化边界情况的处理。
2. 初始化了四个 ListNode 变量：`tempListNode`、`firstListNode`、`secondListNode` 和 `currentListNode`。`currentListNode` 最初设置为 `dumyhead`。
3. 进入一个 while 循环，只要在当前节点（`currentListNode`）之后的链表中还有至少两个节点，就会继续执行。
4. 在循环内部，它交换了成对的节点：
   - 将当前对之后的第三个节点（`tempListNode`）存储起来。
   - 确定了对中的第一个和第二个节点（`firstListNode` 和 `secondListNode`）。
   - 重新排列指针，使当前节点指向第二个节点，第二个节点指向第一个节点，第一个节点指向第三个节点。
   - 将 `currentListNode` 指针移动到下一对的第一个节点上。
5. 最后，返回虚拟头节点之后的下一个节点，这实际上是修改后链表的新头节点。







[24. Swap Nodes in Pairs](https://leetcode.cn/problems/swap-nodes-in-pairs/)

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

**Example 1:**

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [1]
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
