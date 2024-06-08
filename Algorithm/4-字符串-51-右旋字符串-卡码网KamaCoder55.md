# 4-字符串-51-右旋字符串-卡码网KamaCoder55

> 卡码网KamaCoder: 题目序号55



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





\55. 右旋字符串（第八期模拟笔试）

时间限制：1.000S 空间限制：128MB

**题目描述**

字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。 

例如，对于输入字符串 "abcdefg" 和整数 2，函数应该将其转换为 "fgabcde"。

**输入描述**

输入共包含两行，第一行为一个正整数 k，代表右旋转的位数。第二行为字符串 s，代表需要旋转的字符串。

**输出描述**

输出共一行，为进行了右旋转操作后的字符串。

**输入示例**

```
2
abcdefg
```

**输出示例**

```
fgabcde
```

**提示信息**

数据范围：
1 <= k < 10000,
1 <= s.length < 10000;





步骤：

* **将整个字符串反转**
* **然后根据给出的大小，切割成两个单词，进行局部反转**

```java
import java.util.Scanner;

public class Main {


    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = Integer.parseInt(in.nextLine());
        String s = in.nextLine();
        char[] sCharArray = s.toCharArray();

        reverseString(sCharArray, 0, sCharArray.length - 1);
        reverseString(sCharArray, 0, n - 1);
        reverseString(sCharArray, n, sCharArray.length - 1);


        System.out.println(new String(sCharArray));
    }


    /**
     * 反转字符串（双指针）
     *
     * @param sCharArray 待反转字符数组
     * @param start      开始索引
     * @param end        结束索引
     * @author CodeZeng1998 
     */
    private static void reverseString(char[] sCharArray, int start, int end) {
        while (start <= end) {
            char tempChar = sCharArray[end];
            sCharArray[end] = sCharArray[start];
            sCharArray[start] = tempChar;
            start++;
            end--;
        }
    }
}
```

ChatGPT：

这段代码实现了对输入字符串的部分反转操作。具体来说，首先将整个字符串反转，然后根据输入的整数 `n`，分别反转字符串的前 `n` 个字符和剩余的字符。最终输出处理后的字符串。

**1. `main` 方法：**

**读取输入：**

- 使用 `Scanner` 从标准输入读取数据。
- 读取第一行输入，将其转换为整数 `n`。
- 读取第二行输入，将其转换为字符串 `s`。

**字符数组转换：**

- 将字符串 `s` 转换为字符数组 `sCharArray`。

**反转字符串：**

- 调用 `reverseString` 方法，反转整个字符数组 `sCharArray`。
- 调用 `reverseString` 方法，反转字符数组 `sCharArray` 的前 `n` 个字符。
- 调用 `reverseString` 方法，反转字符数组 `sCharArray` 中从第 `n` 个字符到末尾的部分。

**输出结果：**

- 将处理后的字符数组 `sCharArray` 转换为字符串并输出。

**反转字符串的方法 `reverseString`**

**参数：**

- `char[] sCharArray`：待反转的字符数组。
- `int start`：反转操作的开始索引。
- `int end`：反转操作的结束索引。

**过程：**

- 使用 `while` 循环，当 `start` 小于或等于 `end` 时，继续循环。
- 交换 `sCharArray[start]` 和 `sCharArray[end]` 的值。
- 将 `start` 递增，将 `end` 递减，直到整个子字符串被反转。

**示例过程**

假设输入如下：

```
3
abcdefg
```

**读取输入：**

- `n` = 3
- `s` = "abcdefg"

**字符数组转换：**

- `sCharArray` = ['a', 'b', 'c', 'd', 'e', 'f', 'g']

**反转整个字符串：**

- 调用 `reverseString(sCharArray, 0, 6)`：
  - 初始状态：['a', 'b', 'c', 'd', 'e', 'f', 'g']
  - 第一步交换：['g', 'b', 'c', 'd', 'e', 'f', 'a']
  - 第二步交换：['g', 'f', 'c', 'd', 'e', 'b', 'a']
  - 第三步交换：['g', 'f', 'e', 'd', 'c', 'b', 'a']
- 结果：['g', 'f', 'e', 'd', 'c', 'b', 'a']

**反转前 `n` 个字符：**

- 调用 `reverseString(sCharArray, 0, 2)`：
  - 初始状态：['g', 'f', 'e', 'd', 'c', 'b', 'a']
  - 第一步交换：['e', 'f', 'g', 'd', 'c', 'b', 'a']
- 结果：['e', 'f', 'g', 'd', 'c', 'b', 'a']

**反转从第 `n` 个字符到末尾的部分：**

- 调用 `reverseString(sCharArray, 3, 6)`：
  - 初始状态：['e', 'f', 'g', 'd', 'c', 'b', 'a']
  - 第一步交换：['e', 'f', 'g', 'a', 'c', 'b', 'd']
  - 第二步交换：['e', 'f', 'g', 'a', 'b', 'c', 'd']
- 结果：['e', 'f', 'g', 'a', 'b', 'c', 'd']

**输出结果：**

- 最终输出字符串："efgabcd"

**总结**

通过以上步骤，输入的字符串 "abcdefg" 被处理为 "efgabcd"，其中前 3 个字符 'abc' 被移动到字符串的末尾，并且保持其顺序。







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/4-%E5%AD%97%E7%AC%A6%E4%B8%B2-51-%E5%8F%B3%E6%97%8B%E5%AD%97%E7%AC%A6%E4%B8%B2-%E5%8D%A1%E7%A0%81%E7%BD%91KamaCoder55.png?raw=true)

上图由 Pic 生成

关键词：Dragon Ball





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
