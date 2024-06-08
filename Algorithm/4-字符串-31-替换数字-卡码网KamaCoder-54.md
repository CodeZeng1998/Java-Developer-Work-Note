# 4-字符串-31-替换数字-卡码网KamaCoder-54

> 卡码网KamaCoder: 题目序号54
>
> PS：如果需要在线上做这道题需要将对应的代码类名和输入输出带上。



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**







54. 替换数字（第八期模拟笔试）

时间限制：1.000S 空间限制：128MB

###### 题目描述

给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。 例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。

###### 输入描述

输入一个字符串 s,s 仅包含小写字母和数字字符。

###### 输出描述

打印一个新的字符串，其中每个数字字符都被替换为了number

###### 输入示例

```
a1b2c3
```

###### 输出示例

```
anumberbnumbercnumber
```

###### 提示信息

数据范围：
1 <= s.length < 10000。







## 解法一：允许使用StringBuilder

```java
import java.util.Scanner;

public class Main {
    /**
     * 替换数字
     *
     * @param s 待处理的字符串
     * @return 替换后的字符串
     * @author CodeZeng1998
     */
    public static String replaceNumbers(String s) {
        char[] charArray = s.toCharArray();

        StringBuilder resultStringBuilder = new StringBuilder();

        for (int i = 0; i < charArray.length; i++) {
            if (charArray[i] - 'a' >= 0) {
                resultStringBuilder.append(charArray[i]);
            } else {
                resultStringBuilder.append("number");
            }
        }

        return resultStringBuilder.toString();
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String s = scanner.next();
        System.out.println(replaceNumbers(s));
        scanner.close();
    }
}
```

ChatGPT：

这段代码的功能是将输入字符串中的数字替换为字符串 "number"。具体来说，如果输入字符串中的字符是小写字母，则保持不变；如果是数字，则替换为字符串 "number"。

**1. `replaceNumbers` 方法：**

**准备工作：**

- 将输入字符串 `s` 转换为字符数组 `charArray`。

**循环处理字符：**

- 使用 `for` 循环遍历字符数组 `charArray` 中的每个字符。

**检查字符类型：**

- 对于每个字符，通过检查 `(charArray[i] - 'a') >= 0` 来判断是否是小写字母。这是因为小写字母的 ASCII 值大于或等于 `'a'`，而数字的 ASCII 值小于 `'a'`。如果是小写字母，则直接将其添加到结果字符串中；如果是数字，则添加字符串 "number" 到结果字符串中。

**返回结果：**

- 返回结果字符串，即将所有数字替换为 "number" 后的字符串。

**2. `main` 方法：**

**接受用户输入：**

- 使用 `Scanner` 从标准输入中读取用户输入的字符串 `s`。

**调用 `replaceNumbers` 方法：**

- 调用 `replaceNumbers` 方法，将用户输入的字符串作为参数传递给它，并获取返回的结果字符串。

**输出结果：**

- 将替换完成后的字符串打印到标准输出。





## 解法二：声明一个新数组

```java

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.next();


        int newArraySize = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                newArraySize += 6;
            } else {
                newArraySize++;
            }
        }

        char[] newArray = new char[newArraySize];


        int oldArrayIndex = 0;
        for (int i = 0; i < newArray.length; i++) {
            if (s.charAt(oldArrayIndex) >= 0 && s.charAt(oldArrayIndex) <= '9') {
                newArray[i] = 'n';
                newArray[++i] = 'u';
                newArray[++i] = 'm';
                newArray[++i] = 'b';
                newArray[++i] = 'e';
                newArray[++i] = 'r';
            } else {
                newArray[i] = s.charAt(oldArrayIndex);
            }
            oldArrayIndex++;

        }

        System.out.println(new String(newArray));
    }
}
```

ChatGPT:

这段代码实现了将输入字符串中的每个数字字符替换为字符串 "number"，并输出处理后的新字符串。

**1. `main` 方法：**

**读取输入：**

- 使用 `Scanner` 类从标准输入读取字符串 `s`。

**计算新数组大小：**

- 初始化变量 `newArraySize` 为 `0`。
- 使用 `for` 循环遍历字符串 `s` 的每个字符。
- 如果当前字符是数字（字符在 `'0'` 到 `'9'` 之间），则将 `newArraySize` 增加 `6`，因为 "number" 有 6 个字符。
- 否则，`newArraySize` 增加 `1`，因为非数字字符保持不变。

**创建新字符数组：**

- 创建一个大小为 `newArraySize` 的字符数组 `newArray`，用于存储处理后的字符。

**填充新字符数组：**

- 初始化变量 `oldArrayIndex` 为 `0`，用于遍历原始字符串 `s`。
- 使用 `for` 循环遍历新字符数组 `newArray` 的每个位置。
- 如果当前处理的原始字符串字符是数字：
  - 将 "number" 的每个字符依次赋值给 `newArray` 的当前位置，并递增 `i` 以跳过已经赋值的位置。
- 否则，将当前原始字符串字符直接赋值给 `newArray` 的当前位置。
- 每次处理完一个字符后，将 `oldArrayIndex` 增加 `1`，指向原始字符串的下一个字符。

**输出结果：**

- 使用 `System.out.println` 将新字符数组 `newArray` 转换为字符串并输出。

示例过程

假设输入字符串为 `"a1b2c3"`：

1. **读取输入：**
   - 字符串 `s` 为 `"a1b2c3"`。
2. **计算新数组大小：**
   - 遍历字符串 `"a1b2c3"`：
     - `'a'` 不是数字，`newArraySize` 增加 `1`。
     - `'1'` 是数字，`newArraySize` 增加 `6`。
     - `'b'` 不是数字，`newArraySize` 增加 `1`。
     - `'2'` 是数字，`newArraySize` 增加 `6`。
     - `'c'` 不是数字，`newArraySize` 增加 `1`。
     - `'3'` 是数字，`newArraySize` 增加 `6`。
   - 计算结果 `newArraySize = 21`。
3. **创建新字符数组：**
   - 创建一个大小为 `21` 的字符数组 `newArray`。
4. **填充新字符数组：**
   - 初始化 `oldArrayIndex = 0`。
   - 遍历新字符数组：
     - 处理 `'a'`，`newArray[0] = 'a'`。
     - 处理 `'1'`，将 "number" 赋值到 `newArray[1]` 到 `newArray[6]`。
     - 处理 `'b'`，`newArray[7] = 'b'`。
     - 处理 `'2'`，将 "number" 赋值到 `newArray[8]` 到 `newArray[13]`。
     - 处理 `'c'`，`newArray[14] = 'c'`。
     - 处理 `'3'`，将 "number" 赋值到 `newArray[15]` 到 `newArray[20]`。
5. **输出结果：**
   - 输出 `newArray` 转换后的字符串，即 `"anumberbnumbercnumber"`。

通过上述步骤，输入字符串中的数字被替换为 "number" 并输出处理后的新字符串。









![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/4-%E5%AD%97%E7%AC%A6%E4%B8%B2-31-%E6%9B%BF%E6%8D%A2%E6%95%B0%E5%AD%97-%E5%8D%A1%E7%A0%81%E7%BD%91KamaCoder-54.png?raw=true)

上图是由 Pic 生成的

关键词：HUNTERxHUNTER





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
