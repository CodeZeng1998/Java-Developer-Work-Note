# 4-字符串-41-反转字符串中的单词-LeetCode151

> 参考：代码随想录
>
> LeetCode: 题目序号151
>
> PS：如果需要在线上做这道题需要将对应的代码类名和输入输出带上。



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**







[151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 包含英文大小写字母、数字和空格 `' '`
- `s` 中 **至少存在一个** 单词



 

**进阶：**如果字符串在你使用的编程语言中是一种可变数据类型，请尝试使用 `O(1)` 额外空间复杂度的 **原地** 解法。



步骤：

* 去掉不必要的空格字符
* 整个字符串反转
* 针对单个单词反转

```java
    /**
     * 反转字符串中的单词
     *
     * @param s 待 反转的字符串
     * @return 反转后的字符串
     * @author CodeZeng1998
     */
    public String reverseWords(String s) {
        // 1.去除首尾以及中间多余空格
        StringBuilder sb = removeSpace(s);
        // 2.反转整个字符串
        reverseString(sb, 0, sb.length() - 1);
        // 3.反转各个单词
        reverseEachWord(sb);
        return sb.toString();
    }

    /**
     * 反转各个单词
     *
     * @param sb 待处理的字符串
     * @author CodeZeng1998
     */
    private void reverseEachWord(StringBuilder sb) {
        int left = 0;
        int right = sb.length() - 1;


        while (left <= right) {
            int slowIndex = left;
            int fastIndex = left;

            while (fastIndex < sb.length() && sb.charAt(fastIndex) != ' ') {
                fastIndex++;
            }

            if (slowIndex != fastIndex) {
                reverseString(sb, slowIndex, fastIndex - 1);
                left = fastIndex;
            } else {
                left++;
            }


        }
    }

    /**
     * 反转字符串（双指针）
     *
     * @param sb 待处理的字符串
     * @author CodeZeng1998
     */
    public void reverseString(StringBuilder sb, int start, int end) {
        int left = start;
        int right = end;


        while (left <= right) {
            char tempChar = sb.charAt(right);
            sb.setCharAt(right, sb.charAt(left));
            sb.setCharAt(left, tempChar);
            left++;
            right--;
        }
    }

    /**
     * 按要求移除字符串的空格（双指针）
     *
     * @param s 待处理的字符串
     * @return 返回按要求移除字符串的空格后的字符串
     * @author CodeZeng1998
     */
    private StringBuilder removeSpace(String s) {
        StringBuilder noSpaceStringBuilder = new StringBuilder();

        int left = 0;
        int right = s.length() - 1;

        // 移除首空格
        while (s.charAt(left) == ' ') {
            left++;
        }

        // 移除尾空格
        while (s.charAt(right) == ' ') {
            right--;
        }

        while (left <= right) {

            if (s.charAt(left) != ' ') {
                noSpaceStringBuilder.append(s.charAt(left));
            } else if (s.charAt(left) == ' ' && noSpaceStringBuilder.charAt(noSpaceStringBuilder.length() - 1) != ' ') {
                noSpaceStringBuilder.append(s.charAt(left));
            }

            left++;
        }

        return noSpaceStringBuilder;
    }
```

ChatGPT：

这段代码实现了一个字符串反转的功能，具体是对字符串中的每个单词进行反转，并且去除首尾以及中间多余的空格。最终返回处理后的字符串。

**1. `reverseWords` 方法：**

**去除空格：**

- 首先，调用 `removeSpace` 方法去除字符串 `s` 首尾以及中间多余的空格，得到一个 `StringBuilder` 对象 `sb`。

**反转整个字符串：**

- 调用 `reverseString` 方法，反转整个 `sb` 字符串。

**反转各个单词：**

- 最后，调用 `reverseEachWord` 方法，对 `sb` 字符串中的每个单词进行反转。

**返回结果：**

- 返回反转处理后的字符串。

**2. `reverseEachWord` 方法：**

**初始化指针：**

- 初始化两个指针 `left` 和 `right`，分别指向字符串的起始位置和末尾位置。

**循环处理字符串：**

- 使用 `while` 循环，当 `left` 小于或等于 `right` 时，继续循环。

**定位单词边界：**

- 使用两个指针 `slowIndex` 和 `fastIndex`，从 `left` 开始定位单词的边界。`fastIndex` 用于查找空格，`slowIndex` 指向单词的起始位置。

**反转单词：**

- 如果找到了单词的边界（即 `slowIndex` 不等于 `fastIndex`），调用 `reverseString` 方法反转单词，然后将 `left` 更新为 `fastIndex`。否则，将 `left` 自增，继续查找下一个单词。

**3. `reverseString` 方法：**

**初始化指针：**

- 初始化两个指针 `left` 和 `right`，分别指向需要反转的子字符串的起始位置 `start` 和结束位置 `end`。

**交换字符：**

- 使用 `while` 循环，当 `left` 小于或等于 `right` 时，交换 `left` 和 `right` 指针对应的字符，并移动指针，直到整个子字符串被反转。

**4. `removeSpace` 方法：**

**初始化：**

- 创建一个 `StringBuilder` 对象 `noSpaceStringBuilder`，用于存储去除空格后的字符串。

**移除首尾空格：**

- 使用 `while` 循环移除字符串 `s` 的首尾空格。`left` 从字符串起始位置开始向右移动，直到找到第一个非空字符；`right` 从字符串末尾位置开始向左移动，直到找到最后一个非空字符。

**移除中间多余空格：**

- 使用 `while` 循环从 `left` 到 `right` 遍历字符串 `s`。如果当前字符不是空格，则将其添加到 `noSpaceStringBuilder` 中；如果当前字符是空格且 `noSpaceStringBuilder` 的最后一个字符不是空格，则将空格添加到 `noSpaceStringBuilder` 中。这一步确保了中间不会有多余的空格。

**返回结果：**

- 返回去除多余空格后的 `StringBuilder` 对象 `noSpaceStringBuilder`。

**示例过程**

假设输入字符串为 `" the sky is blue "`：

1. 调用 `removeSpace`：
   - 去除首尾空格后得到 `"the sky is blue"`
   - 去除中间多余空格后得到 `"the sky is blue"`
2. 调用 `reverseString`：
   - 反转整个字符串后得到 `"eulb si yks eht"`
3. 调用 `reverseEachWord`：
   - 反转每个单词后得到 `"blue is sky the"`
4. 返回结果：
   - 最终返回 `"blue is sky the"`

通过以上步骤，输入字符串 `" the sky is blue "` 被处理为 `"blue is sky the"`。





[151. Reverse Words in a String](https://leetcode.cn/problems/reverse-words-in-a-string/)

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

 

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"
```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

 

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

 

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/4-%E5%AD%97%E7%AC%A6%E4%B8%B2-41-%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E5%8D%95%E8%AF%8D-LeetCode151.png?raw=true)

上图是由 Pic 生成

关键词：ONE PIECE，Dragon Ball AND HUNTERxHUNTER





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
