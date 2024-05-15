# 1-数组-24-比较含退格的字符串-LeetCode844

> LeetCode：题目序号844



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[844. 比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)

给定 `s` 和 `t` 两个字符串，当它们分别被输入到空白的文本编辑器后，如果两者相等，返回 `true` 。`#` 代表退格字符。

**注意：**如果对空文本输入退格字符，文本继续为空。

 

**示例 1：**

```
输入：s = "ab#c", t = "ad#c"
输出：true
解释：s 和 t 都会变成 "ac"。
```

**示例 2：**

```
输入：s = "ab##", t = "c#d#"
输出：true
解释：s 和 t 都会变成 ""。
```

**示例 3：**

```
输入：s = "a#c", t = "b"
输出：false
解释：s 会变成 "c"，但 t 仍然是 "b"。
```

 

**提示：**

- `1 <= s.length, t.length <= 200`
- `s` 和 `t` 只含有小写字母以及字符 `'#'`

 

**进阶：**

- 你可以用 `O(n)` 的时间复杂度和 `O(1)` 的空间复杂度解决该问题吗？





```java
   /**
     * 比较含退格的字符串（快慢指针）
     *
     * @param s 字符串一
     * @param t 字符串二
     * @return 是否相等 true：相等、false：不相等
     * @author CodeZeng1998
     */
    public boolean backspaceCompare(String s, String t) {

        char[] sCharArray = s.toCharArray();

        int sSlowIndex = 0;

        for (int fastIndex = 0; fastIndex < sCharArray.length; fastIndex++) {
            if (sCharArray[fastIndex] != '#') {
                sCharArray[sSlowIndex] = sCharArray[fastIndex];
                sSlowIndex++;
            } else {
                if (sSlowIndex != 0) {
                    sSlowIndex--;
                }
            }
        }

        char[] tCharArray = t.toCharArray();
        int tSlowIndex = 0;

        for (int fastIndex = 0; fastIndex < tCharArray.length; fastIndex++) {
            if (tCharArray[fastIndex] != '#') {
                tCharArray[tSlowIndex] = tCharArray[fastIndex];
                tSlowIndex++;
            } else {
                if (tSlowIndex != 0) {
                    tSlowIndex--;
                }
            }
        }

        boolean twoStringEqual = true;

        if (sSlowIndex != tSlowIndex) {
            twoStringEqual = false;
        } else {

            for (int i = 0; i < tSlowIndex; i++) {
                if (sCharArray[i] != tCharArray[i]) {
                    twoStringEqual = false;
                    break;
                }
            }
        }
        return twoStringEqual;
    }
```



ChatGPT:

提供的代码定义了一个名为 `backspaceCompare` 的方法，用于确定两个输入字符串 `s` 和 `t` 在处理退格（由 `#` 字符表示）后是否相等。以下是代码的详细解释：

**方法签名**

```java
public boolean backspaceCompare(String s, String t) {
```

该方法接受两个字符串 `s` 和 `t` 作为输入，并返回一个布尔值，指示在应用退格操作后它们是否相等。

**将字符串转换为字符数组**

```java
char[] sCharArray = s.toCharArray();
char[] tCharArray = t.toCharArray();
```

将字符串 `s` 和 `t` 转换为字符数组，以便进行就地修改。

**初始化慢速索引**

```java
	int sSlowIndex = 0;
    int tSlowIndex = 0;
```

初始化两个索引 `sSlowIndex` 和 `tSlowIndex`，以跟踪 `s` 和 `t` 的修改数组中的当前位置。

**处理第一个字符串** `s`

```java
    for (int fastIndex = 0; fastIndex < sCharArray.length; fastIndex++) {
        if (sCharArray[fastIndex] != '#') {
            sCharArray[sSlowIndex] = sCharArray[fastIndex];
            sSlowIndex++;
        } else {
            if (sSlowIndex != 0) {
                sSlowIndex--;
            }
        }
    }
```

这个循环处理 `sCharArray`：

1. 如果当前字符不是退格符（`#`），则将其复制到 `sSlowIndex` 指示的位置，并递增 `sSlowIndex`。
2. 如果当前字符是退格符（`#`），且 `sSlowIndex` 不为零，则递减 `sSlowIndex` 以模拟退格效果。

**处理第二个字符串** `t`

```java
    for (int fastIndex = 0; fastIndex < tCharArray.length; fastIndex++) {
        if (tCharArray[fastIndex] != '#') {
            tCharArray[tSlowIndex] = tCharArray[fastIndex];
            tSlowIndex++;
        } else {
            if (tSlowIndex != 0) {
                tSlowIndex--;
            }
        }
    }
```

这个循环以相同的方式处理 `tCharArray`，处理退格并相应地复制字符。

**比较结果字符串**

```java
  boolean twoStringEqual = true;

    if (sSlowIndex != tSlowIndex) {
        twoStringEqual = false;
    } else {
        for (int i = 0; i < tSlowIndex; i++) {
            if (sCharArray[i] != tCharArray[i]) {
                twoStringEqual = false;
                break;
            }
        }
    }
    return twoStringEqual;
}
```

1. **长度检查**: 如果处理后的 `s` 和 `t` 的有效长度（由 `sSlowIndex` 和 `tSlowIndex` 指示）不同，则字符串不相等。
2. **内容检查**: 如果长度相同，检查处理后的数组中的每个字符，直到 `tSlowIndex`（或 `sSlowIndex`）。如果任何字符不同，则将 `twoStringEqual` 设置为 `false` 并退出循环。
3. **返回结果**: 方法返回 `twoStringEqual` 的值，指示在处理退格后两个字符串是否相等。

**总结**

`backspaceCompare` 方法使用双指针技术通过就地修改字符数组来模拟输入字符串上的退格效果。然后，它比较处理后的数组以确定在应用退格后字符串是否等效。









[844. Backspace String Compare](https://leetcode.cn/problems/backspace-string-compare/)

Given two strings `s` and `t`, return `true` *if they are equal when both are typed into empty text editors*. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

 

**Example 1:**

```
Input: s = "ab#c", t = "ad#c"
Output: true
Explanation: Both s and t become "ac".
```

**Example 2:**

```
Input: s = "ab##", t = "c#d#"
Output: true
Explanation: Both s and t become "".
```

**Example 3:**

```
Input: s = "a#c", t = "b"
Output: false
Explanation: s becomes "c" while t becomes "b".
```

 

**Constraints:**

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

 

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?







**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
