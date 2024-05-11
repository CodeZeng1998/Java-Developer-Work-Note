# 1-数组-15-有效的完全平方数- LeetCode367

> LeetCode：题目序号367

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

给你一个正整数 `num` 。如果 `num` 是一个完全平方数，则返回 `true` ，否则返回 `false` 。

**完全平方数** 是一个可以写成某个整数的平方的整数。换句话说，它可以写成某个整数和自身的乘积。

不能使用任何内置的库函数，如 `sqrt` 。

 

**示例 1：**

```
输入：num = 16
输出：true
解释：返回 true ，因为 4 * 4 = 16 且 4 是一个整数。
```

**示例 2：**

```
输入：num = 14
输出：false
解释：返回 false ，因为 3.742 * 3.742 = 14 但 3.742 不是一个整数。
```

 

**提示：**

- `1 <= num <= 231 - 1`



```java
    /**
     * 有效的完全平方数
     *
     * @param num 目标数值
     * @return 是否存在有效的完全平方数 true:存在、false：不存在 
     * @Author: CodeZeng1998
     */
    public boolean isPerfectSquare(int num) {
        // 1 的算数平方是 1
        if (num == 1) {
            return true;
        }

        boolean hasPerfectSquare = false;

        int left = 1;
        int right = num >> 1;

        while (left <= right) {
            int middle = left + ((right - left) >> 1);

            // long 强制类型转换，防止 int 类型的数值出现溢出
            if ((long) middle * middle > num) {
                right = middle - 1;
            } else if ((long) middle * middle < num) {
                left = middle + 1;
            } else {
                // 值相等说明存在有效的完全平方数，跳出循环
                hasPerfectSquare = true;
                break;
            }
        }
        return hasPerfectSquare;
    }
```



ChatGPT：

这段代码是用二分查找算法来确定一个给定的整数是否是一个完全平方数。它的逻辑看起来是正确的，使用二分查找算法来缩小可能的完全平方数的范围，直到找到平方根或确定不存在。

简要解释如下：

- `isPerfectSquare` 方法接受一个整数 `num` 作为输入，并返回一个布尔值，指示 `num` 是否是一个完全平方数。
- 它将 `hasPerfectSquare` 初始化为 `false`，最初假设 `num` 不是一个完全平方数。
- 如果 `num` 等于 1，它立即返回 `true`，因为 1 是一个完全平方数。
- 然后设置一个二分查找范围，介于 1 和 `num / 2` 之间（使用位操作进行除以 2）。
- 在二分查找循环内部，它计算中间点，并检查中间值的平方是否大于、小于或等于 `num`。
- 根据结果，更新搜索范围，并继续直到找到完全平方数或耗尽搜索空间。

总的来说，你的代码看起来是高效的，应该能够正确地确定一个给定的整数是否是一个完全平方数。







[367. Valid Perfect Square](https://leetcode.cn/problems/valid-perfect-square/)

Given a positive integer num, return `true` *if* `num` *is a perfect square or* `false` *otherwise*.

A **perfect square** is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as `sqrt`.

 

**Example 1:**

```
Input: num = 16
Output: true
Explanation: We return true because 4 * 4 = 16 and 4 is an integer.
```

**Example 2:**

```
Input: num = 14
Output: false
Explanation: We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.
```

 

**Constraints:**

- `1 <= num <= 231 - 1`





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



![公众号：CodeZeng1998](./CodeZeng1998.png)

![公众号：好锅](.\好锅.png)

