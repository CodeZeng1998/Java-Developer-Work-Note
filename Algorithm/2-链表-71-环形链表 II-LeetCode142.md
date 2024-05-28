# 2-链表-71-环形链表 II-LeetCode142

> 参考：代码随想录
>
> LeetCode: 题目序号142



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 

**提示：**

- 链表中节点的数目范围在范围 `[0, 104]` 内
- `-105 <= Node.val <= 105`
- `pos` 的值为 `-1` 或者链表中的一个有效索引

 

**进阶：**你是否可以使用 `O(1)` 空间解决此题？





**链表结构**如下：

```java
    class ListNode {
        int val;
        ListNode next;

        ListNode(int x) {
            val = x;
            next = null;
        }
    }
```



**解题关键：**

* **判断链表是否环：快慢指针，一次循环慢指针往前一步，快指针往前两步，相当于一次性快指针只比慢指针走多了一步，有环就一定能碰见。**
* **如果有环，如何找到这个环的入口：快慢指针碰见的地方和链表头节点同时往前一步，直到碰见就能确定对应节点为进入循环链表的第一个节点位置。**



```java
    /**
     * 环形链表 II
     *
     * @param head 链表头节点
     * @return 返回链表开始入环的第一个节点。 如果链表无环，则返回 null。
     * @author CodeZeng1998
     */
    public ListNode detectCycle(ListNode head) {
        ListNode slowListNode = head;
        ListNode fastListNode = head;

        while (fastListNode != null && fastListNode.next != null) {
            slowListNode = slowListNode.next;
            fastListNode = fastListNode.next.next;

            if (fastListNode == slowListNode) {
                ListNode headListNode = head;
                ListNode firstDetectCycleListNode = fastListNode;

                while (headListNode != firstDetectCycleListNode) {
                    headListNode = headListNode.next;
                    firstDetectCycleListNode = firstDetectCycleListNode.next;
                }
                return firstDetectCycleListNode;
            }
        }

        return null;
    }
```

ChatGPT：

这段代码用于检测链表中循环的起始位置。它使用了弗洛伊德的乌龟和兔子算法。以下是详细解释：

1. 我们初始化两个指针，`slowListNode` 和 `fastListNode`，它们都指向链表的头部。这些指针将用于以不同的速度遍历链表。
2. 我们进入一个 while 循环，该循环会持续直到 `fastListNode` 变为 null（表示列表的末尾）或 `fastListNode.next` 变为 null（表示如果 `fastListNode` 是奇数长度列表中的最后一个节点，则表示列表的末尾）。
3. 在循环内部，`slowListNode` 向前移动一步（`slowListNode = slowListNode.next`），而 `fastListNode` 向前移动两步（`fastListNode = fastListNode.next.next`）。这有效地使 `fastListNode` 的移动速度是 `slowListNode` 的两倍。
4. 如果链表中有循环，最终 `fastListNode` 将追上 `slowListNode`，表示存在循环。这是通过条件 `fastListNode == slowListNode` 检测的。
5. 一旦检测到循环，我们初始化两个额外的指针，`headListNode` 和 `firstDetectCycleListNode`，它们分别指向列表的头部和检测到循环的节点。
6. 我们进入另一个 while 循环来找到循环的起始位置。我们不断地将两个指针向前移动，直到它们再次相遇为止。这是通过每次将每个指针向前移动一步（`headListNode = headListNode.next` 和 `firstDetectCycleListNode = firstDetectCycleListNode.next`）直到它们相遇。
7. 一旦 `headListNode` 和 `firstDetectCycleListNode` 相遇，表示我们已经找到了循环的起始位置。我们返回 `firstDetectCycleListNode`，它代表循环的起始位置。
8. 如果未检测到循环，我们返回 null，表示链表中没有循环。







[142. Linked List Cycle II](https://leetcode.cn/problems/linked-list-cycle-ii/)

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

 

**Example 1:**

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

 

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

 

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
