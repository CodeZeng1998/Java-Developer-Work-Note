# 2-链表-51-删除链表的倒数第 N 个结点-LeetCode19

> 参考：代码随想录
>
> LeetCode: 题目序号19



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。



**示例 1：**

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

- 链表中结点的数目为 `sz`
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

 

**进阶：**你能尝试使用一趟扫描实现吗？





**链表结构**如下：

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
     * 删除链表的倒数第 N 个结点
     *
     * @param head 待处理的链表头节点
     * @param n 待删除节点位置
     * @return 返回处理后的链表头节点
     * @author CodeZeng1998
     */
    public ListNode removeNthFromEnd(ListNode head, int n) {

        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        ListNode slowListNode = dummy;
        ListNode fastListNode = dummy;

        for (int i = 0; i <= n; i++) {
            fastListNode = fastListNode.next;
        }

        while (fastListNode != null) {
            fastListNode = fastListNode.next;
            slowListNode = slowListNode.next;
        }

        if (slowListNode.next != null) {
            slowListNode.next = slowListNode.next.next;
        }

        return dummy.next;
    }
```

ChatGPT：

这段代码定义了一个名为 `removeNthFromEnd` 的方法，它接受一个 ListNode 类型的 head 和一个整数 n 作为输入参数，并返回一个 ListNode。

方法内部：

1. 创建了一个值为 -1 的新 ListNode，称为 `dummy`，并将其下一个节点设置为输入的 `head`。这个节点用作虚拟节点，以处理待删除的节点是头节点的边界情况。
2. 初始化了两个 ListNode 变量：`slowListNode` 和 `fastListNode`，两者最初都指向虚拟节点。
3. 使用 for 循环将 `fastListNode` 指针向前移动 `n` 个节点。
4. 进入一个 while 循环，在循环的每次迭代中，`fastListNode` 和 `slowListNode` 都向前移动一步，直到 `fastListNode` 达到链表的末尾。
5. 循环结束后，如果 `slowListNode` 有下一个节点，则通过跳过它来删除倒数第 `n` 个节点。
6. 最后，返回虚拟头节点之后的下一个节点，这实际上是修改后链表的头节点。







[19. Remove Nth Node From End of List](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

 

**Example 1:**

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

 

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

 

**Follow up:** Could you do this in one pass?



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
