# 4-字符串-11-反转字符串-LeetCode344

> LeetCode: 题目序号344



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

 

**示例 1：**

```
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

 

**提示：**

- `1 <= s.length <= 105`
- `s[i]` 都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符







```java
    /**
     * 反转字符串（双指针）
     *
     * @param s 待反转的字符串
     * @author CodeZeng1998
     */
    public void reverseString(char[] s) {

        int left = 0;
        int right = s.length - 1;

        while (left <= right) {
            char tempChar = s[right];
            s[right] = s[left];
            s[left] = tempChar;

            left++;
            right--;
        }
    }
```

ChatGPT：

该代码实现了一个字符串反转的功能。输入是一个字符数组 `s`，函数将该数组原地反转，即不使用额外的空间，将字符顺序颠倒。

**1. 初始化指针：** 代码首先初始化了两个指针：

- `left` 指针从数组的起始位置开始，初始值为 `0`。
- `right` 指针从数组的末尾位置开始，初始值为 `s.length - 1`。

**2. 进入循环：** 使用一个 `while` 循环，当 `left` 指针小于或等于 `right` 指针时，循环继续执行。这个条件确保了指针从两端向中间移动，直到相遇或交错。

**3. 交换字符：** 在每次循环迭代中，代码执行以下步骤：

- **暂存字符**：将 `right` 指针对应的字符暂存在一个临时变量 `tempChar` 中。
- **交换字符**：将 `left` 指针对应的字符赋值给 `right` 指针对应的位置。
- **恢复字符**：将 `tempChar` 中暂存的字符赋值给 `left` 指针对应的位置。

**4. 移动指针：** 交换完字符后，移动两个指针：

- **左指针右移**：将 `left` 指针加一，向右移动一位。
- **右指针左移**：将 `right` 指针减一，向左移动一位。

**5. 循环结束：** 当 `left` 指针大于 `right` 指针时，表示所有字符都已经交换完毕，整个数组实现了原地反转。

**6. 示例过程：** 假设输入字符数组为 `['h', 'e', 'l', 'l', 'o']`：

- 初始状态：`left = 0`，`right = 4`，字符数组为 `['h', 'e', 'l', 'l', 'o']`
- 第一次迭代：交换 `s[0]` 和 `s[4]`，得到 `['o', 'e', 'l', 'l', 'h']`，然后移动指针：`left = 1`，`right = 3`
- 第二次迭代：交换 `s[1]` 和 `s[3]`，得到 `['o', 'l', 'l', 'e', 'h']`，然后移动指针：`left = 2`，`right = 2`
- 第三次迭代：由于 `left` 等于 `right`，交换 `s[2]` 和 `s[2]`，字符数组不变，得到 `['o', 'l', 'l', 'e', 'h']`，然后移动指针：`left = 3`，`right = 1`
- 退出循环：此时 `left` 大于 `right`，循环结束，最终数组为 `['o', 'l', 'l', 'e', 'h']`，字符串已被反转。

通过以上步骤，字符数组 `s` 中的字符顺序被原地反转，完成字符串反转的功能。





[344. Reverse String](https://leetcode.cn/problems/reverse-string/)

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

 

**Example 1:**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Example 2:**

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

 

**Constraints:**

- `1 <= s.length <= 105`
- `s[i]` is a [printable ascii character](https://en.wikipedia.org/wiki/ASCII#Printable_characters).



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/4-%E5%AD%97%E7%AC%A6%E4%B8%B2-11-%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2-LeetCode344.png?raw=true)

上图由 Pic 生成

关键词：Near is a desert, far is a snowy mountain



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
