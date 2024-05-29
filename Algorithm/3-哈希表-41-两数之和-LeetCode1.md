# 3-哈希表-41-两数之和-LeetCode1

> LeetCode: 题目序号1



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[1. 两数之和](https://leetcode.cn/problems/two-sum/)

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

 

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？



```java
    /**
     * 两数之和
     *
     * @param nums 数组
     * @param target 目标值
     * @return 两个数值之和为目标值的索引数组，无则返回空数组
     * @author CodeZeng1998
     */
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }

        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[] {i, map.get(target - nums[i])};
            }
            map.put(nums[i], i);
        }

        return new int[0];
    }
```

ChatGPT：

这段代码定义了一个名为`twoSum`的方法，该方法接受一个整数数组`nums`和一个整数`target`作为输入，并返回一个包含两个数的索引的数组，这两个数的和等于`target`。它假设只有一个解，并且不能重复使用相同的元素。

以下是代码的详细解释：

1. `if (nums == null || nums.length == 0) { return new int[0]; }`: 这一行检查输入数组`nums`是否为null或空。如果是，它返回一个空数组，因为无法找到有效的索引。
2. `Map<Integer, Integer> map = new HashMap<>();`: 这一行初始化了一个HashMap，其中键是来自`nums`的元素，值是其索引。
3. 循环遍历`nums`数组中的每个元素。
4. 在循环内部，它检查`target - nums[i]`是否存在于`map`中。如果存在，则意味着`nums`中存在一对数字的和等于`target`。它返回一个数组，其中包含当前索引`i`和`map`中存储的补值`target - nums[i]`的索引。
5. 如果补值在`map`中未找到，则将当前元素`nums[i]`及其索引`i`添加到`map`中。
6. 如果循环中未找到有效的配对，则返回一个空数组。



[1. Two Sum](https://leetcode.cn/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly\* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

 

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

 

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
