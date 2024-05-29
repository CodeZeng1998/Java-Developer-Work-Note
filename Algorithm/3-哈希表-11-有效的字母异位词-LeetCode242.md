# 3-哈希表-11-有效的字母异位词-LeetCode242

> LeetCode: 题目序号242



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

 

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

 

**提示:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` 和 `t` 仅包含小写字母

 

**进阶:** 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？





## 解法一：允许使用HashMap

* 遍历第一个字符串，使用HashMap对出现的单词进行计数 +1；
* 遍历第二个字符串，使用HashMap对出现的单词进行计数 -1；
* 最终遍历一遍 HashMap 的 value 值全部为0则互为异位词，否则不是异位词。

```java
    /**
     * 有效的字母异位词
     *
     * @param s 第一个字符串
     * @param t 第二个字符串
     * @return 是否为异位词 true：是、false：否
     * @author CodeZeng1998
     */
    public boolean isAnagram(String s, String t) {

        char[] scharArray = s.toCharArray();
        char[] tCharArray = t.toCharArray();

        Map<String, Integer> charAndCountMap = new HashMap<>();

        for (int i = 0; i < scharArray.length; i++) {
            if (charAndCountMap.containsKey(String.valueOf(scharArray[i]))) {
                charAndCountMap.put(
                        String.valueOf(scharArray[i]),
                        charAndCountMap.get(String.valueOf(scharArray[i])) + 1);
            } else {
                charAndCountMap.put(String.valueOf(scharArray[i]), 1);
            }
        }

        for (int i = 0; i < tCharArray.length; i++) {
            if (!charAndCountMap.containsKey(String.valueOf(tCharArray[i]))) {
                return false;
            } else {
                charAndCountMap.put(
                        String.valueOf(tCharArray[i]),
                        charAndCountMap.get(String.valueOf(tCharArray[i])) - 1);
            }
        }

        AtomicBoolean falg = new AtomicBoolean(true);

        charAndCountMap
                .values()
                .forEach(
                        item -> {
                            if (item != 0) {
                                falg.set(false);
                            }
                        });

        return falg.get();
    }
```



ChatGPT:

这段代码定义了一个名为`isAnagram`的方法，接受两个字符串`s`和`t`作为输入，并返回一个布尔值，指示它们是否是彼此的异位词。异位词是通过重新排列不同单词或短语的字母而形成的单词或短语，通常要使用所有原始字母一次。

以下是代码的详细解释：

1. `char[] scharArray = s.toCharArray();` 和 `char[] tCharArray = t.toCharArray();`: 这些代码将输入字符串`s`和`t`转换为字符数组，以便更容易地遍历每个字符。
2. `Map<String, Integer> charAndCountMap = new HashMap<>();`: 这一行初始化了一个HashMap，用于将字符作为键和它们的计数作为值存储。
3. 接下来的几行代码遍历了字符串`s`中的每个字符，并更新了`charAndCountMap`中的字符计数。如果字符已经存在于映射中，则其计数递增。否则，它将被添加到映射中，计数为1。
4. 类似地，另一个循环遍历了字符串`t`中的每个字符。在此循环内部，它检查字符是否存在于`charAndCountMap`中。如果不存在，则意味着字符不存在于字符串`s`中，因此方法返回`false`。如果字符存在，则其计数递减。
5. 在处理了两个字符串之后，代码检查是否在`charAndCountMap`中有任何剩余字符具有非零计数。如果发现任何这样的字符，则意味着`s`和`t`中的字符计数不匹配，方法返回`false`。
6. 最后，如果在计数中未发现任何不匹配，表示`s`和`t`确实是彼此的异位词，方法返回`true`。







## 解法二：不允许使用HashMap，使用数组解决

* 根据提示可知，两个字符串只为小写字母，所以最多出现26个不同的小写字母
* 使用一个数组来存储字符出现的次数，存储和查找时字符位置需要减去'a',变成正确的位置
* 最后也是遍历一遍 数组中的值全部为0则互为异位词，否则不是异位词。

```java
    /**
     * 有效的字母异位词
     *
     * @param s 第一个字符串
     * @param t 第二个字符串
     * @return 是否为异位词 true：是、false：否
     * @author CodeZeng1998
     */
    public boolean isAnagram(String s, String t) {
        int[] letterCountArray = new int[26];

        for (int i = 0; i < s.length(); i++) {
            letterCountArray[s.charAt(i) - 'a']++;
        }

        for (int i = 0; i < t.length(); i++) {
            letterCountArray[t.charAt(i) - 'a']--;
        }

        for (int i = 0; i < letterCountArray.length; i++) {
            if (letterCountArray[i] != 0) {
                return false;
            }
        }

        return true;
    }
```

ChatGPT：

这段代码定义了一个名为`isAnagram`的方法，接受两个字符串`s`和`t`作为输入，并返回一个布尔值，指示它们是否是彼此的异位词。异位词是通过重新排列不同单词或短语的字母而形成的单词或短语，通常要使用所有原始字母一次。

以下是代码的详细解释：

1. `int[] letterCountArray = new int[26];`: 这一行初始化了一个大小为26的整数数组，表示英文字母表中每个字母的计数。
2. 第一个循环遍历了字符串`s`中的每个字符。对于每个字符，它通过将字符的ASCII值减去'a'的ASCII值来计算其在英文字母表中的位置，并相应地增加`letterCountArray`中的计数。
3. 第二个循环对字符串`t`执行相同的操作，但是在计数时是递减操作。
4. 在处理了两个字符串之后，代码遍历了`letterCountArray`并检查所有计数是否为零。如果任何计数不为零，则意味着字符串`s`和`t`中的字符计数不匹配，方法返回`false`。
5. 最后，如果所有计数都为零，则方法返回`true`，表示`s`和`t`确实是彼此的异位词。





[242. Valid Anagram](https://leetcode.cn/problems/valid-anagram/)

Given two strings `s` and `t`, return `true` *if* `t` *is an anagram of* `s`*, and* `false` *otherwise*.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

 

**Constraints:**

- `1 <= s.length, t.length <= 5 * 104`
- `s` and `t` consist of lowercase English letters.

 

**Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
