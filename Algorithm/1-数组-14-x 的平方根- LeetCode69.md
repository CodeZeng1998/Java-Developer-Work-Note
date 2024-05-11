# [1-数组-14-x 的平方根- LeetCode69](https://leetcode.cn/problems/sqrtx/)



> LeetCode：题目序号69



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[69. x 的平方根 ](https://leetcode.cn/problems/sqrtx/)

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：**不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

 

**示例 1：**

```
输入：x = 4
输出：2
```

**示例 2：**

```
Input: x = 8 Output: 2 Explanation: The arithmetic square root of 8 is 2.82842..., since the return type is an integer, the decimal part will be rounded off.
```

 

**提示：**

- `0 <= x <= 231 - 1`





```java
    /**
     * x 的平方根（左闭右闭）[left, right]
     *
     * @param x 待获取的值
     * @return 返回符合条件的值
     * @Author: CodeZeng1998
     */
    public int sqrt(int x) {
        // 一个整数的平方根肯定不会超过它自己的一半，但是 0 和 1 除外，需特殊处理
        if (x == 0 || x == 1) {
            return x;
        }

        int left = 1;
        int right = x >> 1;

        while (left <= right) {
            int middle = left + ((right - left) >> 1);
            // long 强制转化，防止 int 溢出
            if ((long) middle * middle > x) {
                right = middle - 1;
            } else if ((long) middle * middle < x) {
                left = middle + 1;
            } else {
                return middle;
            }
        }

        return right;
    }
```



ChatGPT:


这段代码是一个用二分查找法求一个整数的平方根的函数。以下是对代码的详细解释：

1. `sqrt(int x)` 方法接收一个整数 `x` 作为输入，返回 `x` 的平方根。
2. 首先，程序对特殊情况进行了处理：当 `x` 为 0 或 1 时，它们的平方根分别也是 0 和 1，因此直接返回 `x`。
3. 然后，程序初始化了两个变量 `left` 和 `right`。`left` 初始化为 1，因为任何正整数的平方根都不会小于 1。`right` 初始化为 `x` 的一半，因为一个整数的平方根不会超过它自己的一半，但要注意 0 和 1 的情况已经在上一步处理了。
4. 接下来是二分查找的主循环。在每次迭代中，程序计算 `middle`，它是 `left` 和 `right` 之间的中间值。通过 `(right - left) >> 1` 计算中间值可以防止在计算 `middle` 时发生整数溢出。
5. 在每次迭代中，程序通过将 `middle` 的平方与 `x` 进行比较，来确定二分查找的下一步方向。如果 `middle * middle` 大于 `x`，则将 `right` 更新为 `middle - 1`，因为平方根一定在 `left` 和 `middle - 1` 之间。如果 `middle * middle` 小于 `x`，则将 `left` 更新为 `middle + 1`，因为平方根一定在 `middle + 1` 和 `right` 之间。如果 `middle * middle` 等于 `x`，则找到了平方根，直接返回 `middle`。
6. 循环继续直到 `left > right`，此时返回 `right`，因为在这种情况下，`right` 的值即为 `x` 的平方根的整数部分。

这段代码利用了二分查找的思想，在时间复杂度上达到了 O(logn)，相比于简单的遍历，效率更高。







[69. Sqrt(x)](https://leetcode.cn/problems/sqrtx/)

Given a non-negative integer `x`, return *the square root of* `x` *rounded down to the nearest integer*. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

 

**Example 1:**

```
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.
```

**Example 2:**

```
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.
```

 

**Constraints:**

- `0 <= x <= 231 - 1`





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**