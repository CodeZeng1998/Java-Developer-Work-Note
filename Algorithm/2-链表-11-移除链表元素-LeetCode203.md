# 2-链表-11-移除链表元素-LeetCode203

> 参考：代码随想录
>
> LeetCode: 题目序号203



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 **新的头节点** 。

 

**示例 1：**

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例 2：**

```
输入：head = [], val = 1
输出：[]
```

**示例 3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

 

**提示：**

- 列表中的节点数目在范围 `[0, 104]` 内
- `1 <= Node.val <= 50`
- `0 <= val <= 50`



首先**链表结构**如下

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







## **解法一：用原链表进行操作,不设置一个虚拟头结点**

```java
  /**
     * 移除链表元素(用原链表进行操作,不设置一个虚拟头结点)
     *
     * @param head 链表头
     * @param val 待移除的目标值
     * @return 返回链表头
     * @author CodeZeng1998
     */
    public ListNode removeElements(ListNode head, int val) {
        while (head != null && head.val == val) {
            head = head.next;
        }

        if (head == null) {
            return head;
        }

        ListNode pre = head;
        ListNode cur = head.next;

        while (cur != null) {
            if (cur.val == val) {
                pre.next = cur.next;
            } else {
                pre = cur;
            }
            cur = cur.next;
        }

        return head;
    }
```

ChatGPT：

这个 Java 方法用于从由 ListNode 类表示的单链表中删除所有值为 val 的节点。逐步解释如下：

1. **输入参数**：
   - `head`：链表的头节点。
   - `val`：要从链表中删除的值。
2. **第一个 While 循环**：
   - 第一个 while 循环用于处理头节点本身具有值 `val` 的情况。
   - 只要 `head` 不为 null 并且 `head.val` 等于 `val`，它就会迭代。
   - 在循环内部，它将 `head` 更新为其下一个节点。
3. **检查空链表**：
   - 在第一个 while 循环之后，如果 `head` 变为 null，则意味着所有节点都具有值 `val`，因此立即返回 `head`。
4. **初始化指针**：
   - `pre`：与 `head` 具有相同的初始值，用于跟踪前一个节点。
   - `cur`：在 `head` 后的下一个节点初始化，用于遍历列表。
5. **第二个 While 循环**：
   - 此循环遍历列表的剩余节点。
   - 如果当前节点（`cur`）具有值 `val`，则意味着需要删除它。在这种情况下，更新 `pre.next` 指针以跳过当前节点（`cur`）。
   - 如果当前节点不具有值 `val`，则更新 `pre` 指针以指向当前节点（`cur`）。
   - 在每次迭代中，更新 `cur` 为其下一个节点。
6. **返回**：
   - 最后，返回修改后的链表头。





## **解法二：设置一个虚拟头结点**

```java
    /**
     * 移除链表元素(设置一个虚拟头结点)
     *
     * @param head 链表头
     * @param val 待移除的目标值
     * @return 返回链表头
     * @author CodeZeng1998
     */
    public ListNode removeElements(ListNode head, int val) {
        if (head == null) {
            return head;
        }

        ListNode dummy = new ListNode(-1, head);

        ListNode pre = dummy;
        ListNode cur = dummy.next;

        while (cur != null) {
            if (cur.val == val) {
                pre.next = cur.next;
            } else {
                pre = cur;
            }
            cur = cur.next;
        }

        return dummy.next;
    }
```

ChatGPT：

这个 Java 方法也用于从由 ListNode 类表示的单链表中删除所有值为 val 的节点。然而，它使用了一个虚拟节点来简化实现。让我们逐步解释：

1. **输入参数**：
   - `head`：链表的头节点。
   - `val`：要从链表中删除的值。
2. **检查空链表**：
   - 如果 `head` 为 null，表示空列表，立即返回 `head`。
3. **创建虚拟节点**：
   - 创建一个值为 `-1` 的虚拟节点，并将其下一个指针设置为原始列表的 `head`。
   - 这个虚拟节点充当列表的新头，并帮助处理需要删除原始头的边界情况。
4. **初始化指针**：
   - `pre`：用虚拟节点初始化，用于跟踪前一个节点。
   - `cur`：用虚拟节点后的下一个节点初始化，用于遍历列表。
5. **While 循环**：
   - 类似于先前的实现，此循环遍历列表。
   - 如果当前节点（`cur`）具有值 `val`，它更新 `pre.next` 指针以跳过当前节点（`cur`）。
   - 如果当前节点不具有值 `val`，则更新 `pre` 指针以指向当前节点（`cur`）。
   - 在每次迭代中，更新 `cur` 为其下一个节点。
6. **返回**：
   - 最后，返回虚拟节点之后的下一个节点，这实质上是修改后的链表的头。



[203. Remove Linked List Elements](https://leetcode.cn/problems/remove-linked-list-elements/)

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

 

**Example 1:**

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

**Example 2:**

```
Input: head = [], val = 1
Output: []
```

**Example 3:**

```
Input: head = [7,7,7,7], val = 7
Output: []
```

 

**Constraints:**

- The number of nodes in the list is in the range `[0, 104]`.
- `1 <= Node.val <= 50`
- `0 <= val <= 50`



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
