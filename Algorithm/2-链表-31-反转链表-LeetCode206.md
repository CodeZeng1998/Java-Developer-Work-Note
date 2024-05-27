# 2-链表-31-反转链表-LeetCode206

> 参考：代码随想录
>
> LeetCode: 题目序号206



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

 

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

**进阶：**链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？



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
     * 反转链表（双指针法）
     *
     * @param head 待反转的链表的表头
     * @return 反转后的链表的表头
     * @author CodeZeng1998
     */
    public ListNode reverseList(ListNode head) {

        ListNode preListNode = null;
        ListNode currentListNode = head;
        ListNode tempListNode = null;

        while (currentListNode != null) {
            tempListNode = currentListNode.next;
            currentListNode.next = preListNode;
            preListNode = currentListNode;
            currentListNode = tempListNode;
        }

        return preListNode;
    }
```

ChatGPT：

这个 Java 方法用于反转一个单链表。让我们逐步解释代码：

1. **输入参数**：
   - `head`：要反转的单链表的头节点。
2. **初始化**：
   - `preListNode`：最初设置为 `null`，最终将成为反转后链表的新头。
   - `currentListNode`：最初设置为原始链表的头节点。
   - `tempListNode`：初始化为 `null`，用于临时存储 `currentListNode` 的下一个节点。
3. **While 循环**：
   - 只要 `currentListNode` 不为 `null`，循环将继续。
   - 在循环内部：
     - `tempListNode` 被赋值为 `currentListNode.next`，保留原始列表中的下一个节点。
     - `currentListNode.next` 被更新为指向前一个节点（`preListNode`），有效地反转链接方向。
     - `preListNode` 被更新为 `currentListNode`。
     - `currentListNode` 被更新为 `tempListNode`，移动到原始列表中的下一个节点。
4. **返回**：
   - 一旦循环退出，`preListNode` 将指向反转后链表的新头，因此返回 `preListNode`。

总体上，这个算法遍历原始链表，反转每个链接的方向。最后，它返回反转后链表的新头。





[206. Reverse Linked List](https://leetcode.cn/problems/reverse-linked-list/)

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

 

**Example 1:**

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

 

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

 

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
