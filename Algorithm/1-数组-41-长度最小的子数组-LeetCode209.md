# 1-数组-41-长度最小的子数组-LeetCode209

> 参考：代码随想录
>
> LeetCode: 题目序号



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其总和大于等于 `target` 的长度最小的 **连续**

**子数组**

`[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。



 

**示例 1：**

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**

```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**

```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

 

**提示：**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

 

**进阶：**

- 如果你已经实现 `O(n)` 时间复杂度的解法, 请尝试设计一个 `O(n log(n))` 时间复杂度的解法。



```java
    /**
     * 长度最小的子数组 (滑动窗口)
     *
     * @param target 目标值
     * @param nums 目标数组
     * @return 总和大于等于 target 的长度最小的 连续子数组的长度，没有则返回0
     * @author CodeZeng1998
     */
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int resultArrayLength = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++) {
            sum = sum + nums[right];

            while (sum >= target) {
                resultArrayLength = Math.min(resultArrayLength, right - left + 1);
                sum = sum - nums[left];
                left++;
            }
        }

        return resultArrayLength == Integer.MAX_VALUE ? 0 : resultArrayLength;
    }
```

ChatGPT：

这个代码定义了一个方法 `minSubArrayLen`，它接受一个整数 `target` 和一个整数数组 `nums`。该方法返回和大于或等于 `target` 的最小连续子数组的长度。如果不存在这样的子数组，则返回 0。以下是代码的逐步解释：

1. **初始化**：
   - `left` 初始化为 0，表示当前子数组的左边界。
   - `sum` 初始化为 0，用于存储当前子数组的和。
   - `resultArrayLength` 初始化为 `Integer.MAX_VALUE`，用于存储找到的最小子数组的长度。
2. **主循环**：
   - 循环变量 `right` 从 0 到 `nums.length - 1`，`right` 表示当前子数组的右边界。
   - 对于 `nums` 中的每个元素，将当前元素 (`nums[right]`) 加到 `sum` 中。
3. **内部 While 循环**：
   - 当当前子数组的和大于或等于 target 时，执行以下操作：
     - 更新 `resultArrayLength` 为当前 `resultArrayLength` 和当前子数组长度 (`right - left + 1`) 中的较小值。
     - 从 `sum` 中减去左边界的元素 (`nums[left]`)。
     - 增加 `left` 以从左边缩小子数组。
4. **返回结果**：
   - 退出循环后，检查 `resultArrayLength` 是否仍然是 `Integer.MAX_VALUE`。如果是，表示没有找到符合条件的子数组，返回 0。
   - 否则，返回 `resultArrayLength`，它包含和大于或等于 `target` 的最小子数组的长度。





[209. Minimum Size Subarray Sum](https://leetcode.cn/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a* 

*subarray*

 *whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.



**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.
```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1
```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0
```

 

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

 

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution of which the time complexity is `O(n log(n))`.







**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
