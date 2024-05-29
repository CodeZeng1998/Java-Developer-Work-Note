# 3-哈希表-31-快乐数-LeetCode202

> 参考：代码随想录
>
> LeetCode: 题目序号202



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[202. 快乐数](https://leetcode.cn/problems/happy-number/)

编写一个算法来判断一个数 `n` 是不是快乐数。

**「快乐数」** 定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为 1，也可能是 **无限循环** 但始终变不到 1。
- 如果这个过程 **结果为** 1，那么这个数就是快乐数。

如果 `n` 是 *快乐数* 就返回 `true` ；不是，则返回 `false` 。

 

**示例 1：**

```
输入：n = 19
输出：true
解释：
1²^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

**示例 2：**

```
输入：n = 2
输出：false
```

 

**提示：**

- `1 <= n <= 2^31 - 1`





* **如果是快乐数，中间过程的值不会出现相同的数值。**

```java
    /**
     * 快乐数
     *
     * @param n 数值
     * @return 是否为快乐数，true：是、false：否
     * @author CodeZeng1998
     */
    public boolean isHappy(int n) {
        Set<Integer> record = new HashSet<>();
        while (n != 1 && !record.contains(n)) {
            record.add(n);
            n = getNextNumber(n);
        }

        return n == 1;
    }

    /**
     * 获取下一个数值
     *
     * @param n 数值
     * @return 返回下一个数值
     * @author CodeZeng1998
     */
    private int getNextNumber(int n) {
        int result = 0;

        while (n > 0) {
            int temp = n % 10;
            result = result + temp * temp;
            n = n / 10;
        }

        return result;
    }
```

ChatGPT：

这段代码定义了一个名为`isHappy`的方法，它接受一个整数`n`作为输入，并返回一个布尔值，指示`n`是否是一个快乐数。快乐数的定义如下：从任意正整数开始，用其每个数字的平方之和替换该数，重复此过程，直到该数变为1（此时它将保持不变），或者它在一个循环中无限循环，而该循环不包括1。

以下是代码的详细解释：

1. `Set<Integer> record = new HashSet<>();`: 这一行初始化了一个名为`record`的HashSet，用于跟踪在过程中遇到的数字。
2. `while`循环会持续进行，直到`n`变为1（表示它是一个快乐数），或者`n`在`record`集合中找到（表示它陷入循环中且不是一个快乐数）。
3. 在循环内部，它将当前数字`n`添加到`record`集合中。
4. 然后，它通过调用`getNextNumber`方法更新`n`，该方法计算其各位数字的平方和。
5. `getNextNumber`方法遍历`n`的每个数字，对其求平方并将结果加到结果中。
6. 最后，它返回结果，该结果成为`isHappy`方法中`n`的新值。
7. 在`while`循环之后，如果`n`为1，则返回`true`，表示它是一个快乐数，否则返回`false`。



[202. Happy Number](https://leetcode.cn/problems/happy-number/)

简单



相关标签

相关企业



Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.

Return `true` *if* `n` *is a happy number, and* `false` *if not*.

 

**Example 1:**

```
Input: n = 19
Output: true
Explanation:
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

**Example 2:**

```
Input: n = 2
Output: false
```

 

**Constraints:**

- `1 <= n <= 2^31 - 1`



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
