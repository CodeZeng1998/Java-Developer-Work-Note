# 3-哈希表-61-赎金信-LeetCode383

> LeetCode: 题目序号383



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[383. 赎金信](https://leetcode.cn/problems/ransom-note/)

给你两个字符串：`ransomNote` 和 `magazine` ，判断 `ransomNote` 能不能由 `magazine` 里面的字符构成。

如果可以，返回 `true` ；否则返回 `false` 。

`magazine` 中的每个字符只能在 `ransomNote` 中使用一次。

 

**示例 1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例 2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例 3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

 

**提示：**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` 和 `magazine` 由小写英文字母组成



## 解法一：使用HashMap

```java
    /**
     * 赎金信
     *
     * @param ransomNote 子字符串
     * @param magazine 父字符串
     * @return 子字符串是否可以由父字符串构造 true:是 false:否
     * @author CodeZeng1998
     */
    public boolean canConstruct(String ransomNote, String magazine) {

        boolean canConstruct = true;

        char[] magazineCharArray = magazine.toCharArray();

        Map<Character, Integer> magazineCharAndCountMap = new HashMap<>();

        for (int i = 0; i < magazineCharArray.length; i++) {
            magazineCharAndCountMap.put(
                    magazineCharArray[i],
                    magazineCharAndCountMap.getOrDefault(magazineCharArray[i], 0) + 1);
        }

        char[] ransomNoteCharArray = ransomNote.toCharArray();

        for (int i = 0; i < ransomNoteCharArray.length; i++) {

            if (magazineCharAndCountMap.containsKey(ransomNoteCharArray[i])
                    && magazineCharAndCountMap.get(ransomNoteCharArray[i]) > 0) {
                magazineCharAndCountMap.put(
                        ransomNoteCharArray[i],
                        magazineCharAndCountMap.get(ransomNoteCharArray[i]) - 1);
            } else {
                canConstruct = false;
                break;
            }
        }

        return canConstruct;
    }
```

ChatGPT：

提供的Java代码定义了一个方法 `canConstruct`，用于检查是否可以使用杂志中的字符构建赎金信。该方法返回一个布尔值：如果可以构建赎金信，则返回 `true`，否则返回 `false`。

以下是代码工作原理的逐步解释：

1. **方法签名和变量**：
   - 方法 `canConstruct` 接受两个字符串 `ransomNote` 和 `magazine` 作为输入，返回一个布尔值。
   - 布尔变量 `canConstruct` 初始化为 `true`，用于确定是否可以构建赎金信。
2. **杂志字符频率映射**：
   - 将 `magazine` 字符串转换为字符数组 `magazineCharArray`。
   - 创建一个名为 `magazineCharAndCountMap` 的 `HashMap`，用于存储杂志中每个字符的频率。
   - 循环遍历 `magazineCharArray` 中的每个字符。对于每个字符，更新映射，将字符作为键，频率作为值。如果映射中已经存在该字符，则其频率加1，否则将其添加到映射中，初始频率为1。
3. **检查赎金信字符**：
   - 将 `ransomNote` 字符串转换为字符数组 `ransomNoteCharArray`。
   - 循环遍历 `ransomNoteCharArray` 中的每个字符。
   - 对于每个字符，检查该字符是否存在于 `magazineCharAndCountMap` 中，并且其频率是否大于0。
   - 如果两个条件都满足，则将该字符在映射中的频率减1（表示该字符已被使用）。
   - 如果字符不存在于映射中或其频率为0，则将 `canConstruct` 设为 `false`，并中断循环，因为无法使用可用字符构建赎金信。
4. **返回结果**：
   - 方法返回 `canConstruct` 的值。



## 解法二：使用Array

* 注意题目说明：`ransomNote` 和 `magazine` 由小写英文字母组成
* 小写英文字母只有26个

```java]
    /**
     * 赎金信
     *
     * @param ransomNote 子字符串
     * @param magazine 父字符串
     * @return 子字符串是否可以由父字符串构造 true:是 false:否
     * @author CodeZeng1998
     */
    public boolean canConstruct(String ransomNote, String magazine) {

        boolean canConstruct = true;

        char[] magazineCharArray = magazine.toCharArray();

        int[] letterCountArray = new int[26];

        for (int i = 0; i < magazineCharArray.length; i++) {
            letterCountArray[magazineCharArray[i] - 'a']++;
        }

        char[] ransomNoteCharArray = ransomNote.toCharArray();

        for (int i = 0; i < ransomNoteCharArray.length; i++) {
            if (letterCountArray[ransomNoteCharArray[i] - 'a'] > 0) {
                letterCountArray[ransomNoteCharArray[i] - 'a']--;
            } else {
                canConstruct = false;
                break;
            }
        }

        return canConstruct;
    }
```

ChatGPT：

这段代码定义了一个名为 `canConstruct` 的方法，用于判断是否可以使用给定的 `magazine` 中的字母构建给定的 `ransomNote`。以下是每个步骤的详细解释：

1. **方法定义**：
   - 该方法名为 `canConstruct`，接受两个字符串参数 `ransomNote` 和 `magazine`。
   - 它返回一个布尔值。
2. **初始设置**：
   - 初始化一个布尔变量 `canConstruct` 为 `true`。
3. **字符数组转换**：
   - 将 `magazine` 字符串转换为字符数组。
4. **字母频率计数**：
   - 初始化一个大小为 26 的整数数组 `letterCountArray`，用于记录 `magazine` 中每个字母的频率。
   - 遍历 `magazine` 的字符数组，对于每个字符，通过减去字符的 ASCII 值和 'a' 的 ASCII 值，得到相应的索引，然后增加 `letterCountArray` 中该索引的计数。这样将 'a' 映射到 0，'b' 映射到 1，以此类推。
5. **检查赎金信**：
   - 将 `ransomNote` 字符串转换为字符数组。
   - 遍历这个字符数组，对于每个字符，检查 `letterCountArray` 中相应的计数是否大于零，即是否有该字母。
   - 如果有该字母，则减少 `letterCountArray` 中相应索引的计数。
   - 如果没有该字母（计数为零），则将 `canConstruct` 设置为 `false` 并退出循环，因为无法用 `magazine` 构建 `ransomNote`。
6. **返回结果**：
   - 最后，方法返回 `canConstruct` 的值，如果可以用 `magazine` 构建 `ransomNote`，则返回 `true`，否则返回 `false`。





[383. Ransom Note](https://leetcode.cn/problems/ransom-note/)

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

 

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false
```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false
```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true
```

 

**Constraints:**

- `1 <= ransomNote.length, magazine.length <= 105`
- `ransomNote` and `magazine` consist of lowercase English letters.



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/3-%E5%93%88%E5%B8%8C%E8%A1%A8-61-%E8%B5%8E%E9%87%91%E4%BF%A1-LeetCode383.png?raw=true)

上图由 Pic 生成

关键词：the city of new york



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
