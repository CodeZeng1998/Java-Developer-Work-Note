# 3-哈希表-51-四数相加 II-LeetCode454

> LeetCode: 题目序号454



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)

给你四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` ，数组长度都是 `n` ，请你计算有多少个元组 `(i, j, k, l)` 能满足：

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`



**示例 1：**

```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例 2：**

```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

 

 **提示：**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`





```java
    /**
     * 四数相加 II
     *
     * @param nums1 数组一
     * @param nums2 数组二
     * @param nums3 数组三
     * @param nums4 数组四
     * @return 四个数字各一个数相加等于0 的数量
     * @author CodeZeng1998
     */
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int count = 0;

        Map<Integer, Integer> firstAndSecondArraySumAndCountMap = new HashMap<>();
        for (int i = 0; i < nums1.length; i++) {
            for (int j = 0; j < nums2.length; j++) {
                if (!firstAndSecondArraySumAndCountMap.containsKey(nums1[i] + nums2[j])) {
                    firstAndSecondArraySumAndCountMap.put(nums1[i] + nums2[j], 1);
                } else {
                    firstAndSecondArraySumAndCountMap.put(
                            nums1[i] + nums2[j],
                            firstAndSecondArraySumAndCountMap.get(nums1[i] + nums2[j]) + 1);
                }
            }
        }

        for (int i = 0; i < nums3.length; i++) {
            for (int j = 0; j < nums4.length; j++) {

                if (firstAndSecondArraySumAndCountMap.containsKey(-nums3[i] - nums4[j])) {
                    count += firstAndSecondArraySumAndCountMap.get(-nums3[i] - nums4[j]);
                }
            }
        }

        return count;
    }
```

ChatGPT：

提供的Java代码定义了一个方法 `fourSumCount`，用于计算有多少个元组 (i,j,k,l)(i, j, k, l)(i,j,k,l) 满足 `nums1[i] + nums2[j] + nums3[k] + nums4[l] = 0`。

以下是代码工作原理的逐步解释：

1. **方法签名和变量**：
   - 方法 `fourSumCount` 接受四个整数数组 `nums1`、`nums2`、`nums3` 和 `nums4` 作为输入，返回一个整数。
   - 初始化一个计数器 `count` 用于记录有效元组的数量。
   - 创建一个名为 `firstAndSecondArraySumAndCountMap` 的 `HashMap`，用于存储 `nums1` 和 `nums2` 元素的和及其频率。
2. **第一个循环 - 构建HashMap**：
   - 外层循环遍历 `nums1` 的所有元素，内层循环遍历 `nums2` 的所有元素。
   - 对于每一对元素 (`nums1[i]` 和 `nums2[j]`)，计算它们的和。
   - 如果此和不在map中作为键存在，则将其添加到map中，值为1（表示第一次出现）。
   - 如果此和已经是map中的键，则将其值（频率）加1。
3. **第二个循环 - 查找匹配的和**：
   - 另一对嵌套循环遍历 `nums3` 和 `nums4` 的所有元素。
   - 对于每一对元素 (`nums3[i]` 和 `nums4[j]`)，计算它们的负和（`-nums3[i] - nums4[j]`）。
   - 如果此负和在 `firstAndSecondArraySumAndCountMap` 中作为键存在，意味着存在 `nums1` 和 `nums2` 的和，加上当前 `nums3` 和 `nums4` 的和结果为零。
   - 将此键关联的值（这样的对数）加到 `count` 变量中。
4. **返回结果**：
   - 方法返回有效元组的总数量。





[454. 4Sum II](https://leetcode.cn/problems/4sum-ii/)

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

 

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**Example 2:**

```
Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
Output: 1
```

 

**Constraints:**

- `n == nums1.length`
- `n == nums2.length`
- `n == nums3.length`
- `n == nums4.length`
- `1 <= n <= 200`
- `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/3-%E5%93%88%E5%B8%8C%E8%A1%A8-51-%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0%20II-LeetCode454.png?raw=true)

上图由 Pic 生成

关键词：the city of guangzhou





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
