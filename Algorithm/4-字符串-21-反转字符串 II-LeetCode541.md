# 4-字符串-21-反转字符串 II-LeetCode541

> 参考：代码随想录
>
> LeetCode: 题目序号541



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

 

**示例 1：**

```
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

**示例 2：**

```
输入：s = "abcd", k = 2
输出："bacd"
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由小写英文组成
- `1 <= k <= 104`







```java
    /**
     * 反转字符串 II
     *
     * @param s 字符串
     * @param k 步长
     * @return 处理后的字符串
     * @author CodeZeng1998
     */
    public String reverseStr(String s, int k) {
        int start = 0;

        StringBuilder resultStringBuilder = new StringBuilder();

        while (start < s.length()) {

            int reversBeginIndex = (start + k) > s.length() ? s.length() : start + k;
            int reversEndIndex = (start + 2 * k) > s.length() ? s.length() : (start + 2 * k);

            resultStringBuilder.append(
                    reverseString(s.substring(start, reversBeginIndex).toCharArray()));

            if (reversBeginIndex < reversEndIndex) {
                resultStringBuilder.append(s, reversBeginIndex, reversEndIndex);
            }

            start = start + 2 * k;
        }

        return resultStringBuilder.toString();
    }

    /**
     * 反转字符串（双指针）
     *
     * @param s 待反转的字符串
     * @author CodeZeng1998
     */
    public String reverseString(char[] s) {

        int left = 0;
        int right = s.length - 1;

        while (left <= right) {
            char tempChar = s[right];
            s[right] = s[left];
            s[left] = tempChar;

            left++;
            right--;
        }

        return new String(s);
    }
```

ChatGPT：

这段代码的功能是对输入字符串 `s` 进行特定规则的反转。具体来说，每隔 `2k` 个字符反转前 `k` 个字符。如果剩余字符少于 `k` 个，则将剩余字符全部反转；如果剩余字符在 `k` 到 `2k` 之间，则反转前 `k` 个字符，剩余部分保持不变。

**1. `reverseStr` 方法：**

**初始化和准备：**

- `start` 变量初始化为 `0`，用于记录当前处理的起始位置。
- `resultStringBuilder` 被初始化为一个 `StringBuilder` 对象，用于构建最终的结果字符串。

**循环处理字符串：**

- 使用 `while` 循环，条件是 `start` 小于字符串 `s` 的长度。这意味着会逐步处理整个字符串。

**计算反转范围：**

- `reversBeginIndex`：计算反转结束的位置。如果 `start + k` 超过了字符串的长度，则取字符串的长度作为反转结束位置。
- `reversEndIndex`：计算当前处理块的结束位置。如果 `start + 2 * k` 超过了字符串的长度，则取字符串的长度作为处理块结束位置。

**反转和拼接：**

- 将 `s` 从 `start` 到 `reversBeginIndex` 的子串转换为字符数组，然后调用 `reverseString` 方法对其进行反转。反转后的子串添加到 `resultStringBuilder` 中。
- 如果 `reversBeginIndex` 小于 `reversEndIndex`，说明在当前块中还有部分未反转的字符，将这些字符直接添加到 `resultStringBuilder` 中。

**更新起始位置：**

- 将 `start` 更新为 `start + 2 * k`，准备处理下一个块。

**返回结果：**

- 最后，将 `resultStringBuilder` 转换为字符串并返回，得到最终的结果。



**2. `reverseString` 方法：**

**初始化指针：**

- `left` 指针从数组的起始位置开始，初始值为 `0`。
- `right` 指针从数组的末尾位置开始，初始值为 `s.length - 1`。

**进入循环：**

- 使用 `while` 循环，当 `left` 指针小于或等于 `right` 指针时，循环继续执行。这个条件确保了指针从两端向中间移动，直到相遇或交错。

**交换字符：**

- 在每次循环迭代中，代码执行以下步骤：
  - 将 `right` 指针对应的字符暂存在一个临时变量 `tempChar` 中。
  - 将 `left` 指针对应的字符赋值给 `right` 指针对应的位置。
  - 将 `tempChar` 中暂存的字符赋值给 `left` 指针对应的位置。

**移动指针：**

- 交换完字符后，移动两个指针：
  - `left` 指针加一，向右移动一位。
  - `right` 指针减一，向左移动一位。

**循环结束：**

- 当 `left` 指针大于 `right` 指针时，表示所有字符都已经交换完毕，整个数组实现了原地反转。

**返回结果：**

- 将字符数组转换为字符串并返回。

**示例过程：** 假设输入字符串为 `"abcdefg"`，`k` 为 `2`：

- 初始状态：`start = 0`，`resultStringBuilder` 为空。
- 第一次迭代：计算 `reversBeginIndex = 2`, `reversEndIndex = 4`
  - 反转 `"ab"` 得到 `"ba"`，添加到 `resultStringBuilder`。
  - 添加 `"cd"` 到 `resultStringBuilder`，结果为 `"bacd"`。
  - 更新 `start` 为 `4`。
- 第二次迭代：计算 `reversBeginIndex = 6`，`reversEndIndex = 7`
  - 反转 `"ef"` 得到 `"fe"`，添加到 `resultStringBuilder`。
  - 添加 `"g"` 到 `resultStringBuilder`，结果为 `"bacdfe"`。
  - 更新 `start` 为 `8`。
- 循环结束，返回结果 `"bacdfe"`。

通过以上步骤，函数按要求对字符串进行了特定规则的反转。



[541. Reverse String II](https://leetcode.cn/problems/reverse-string-ii/)

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

 

**Example 1:**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"
```

**Example 2:**

```
Input: s = "abcd", k = 2
Output: "bacd"
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 104`



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/4-%E5%AD%97%E7%AC%A6%E4%B8%B2-21-%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%20II-LeetCode541.png?raw=true)

上图是由 Pic 生成的

关键词：a lot of people watching the moon on the snowy mountain



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
