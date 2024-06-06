# 3-哈希表-81-四数之和-LeetCode18

> 参考：代码随想录
>
> LeetCode: 题目序号18



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[18. 四数之和](https://leetcode.cn/problems/4sum/)

给你一个由 `n` 个整数组成的数组 `nums` ，和一个目标值 `target` 。请你找出并返回满足下述全部条件且**不重复**的四元组 `[nums[a], nums[b], nums[c], nums[d]]` （若两个四元组元素一一对应，则认为两个四元组重复）：

- `0 <= a, b, c, d < n`
- `a`、`b`、`c` 和 `d` **互不相同**
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

你可以按 **任意顺序** 返回答案 。

 

**示例 1：**

```
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**示例 2：**

```
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

 

**提示：**

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`









```java
    /**
     * 四数之和（双指针）
     *
     * @param nums 目标数组
     * @param target 目标值
     * @return 不重复的四元组
     * @author CodeZeng1998
     */
    public List<List<Integer>> fourSum(int[] nums, int target) {

        List<List<Integer>> result = new ArrayList<>();

        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] > target && nums[i] > 0) {
                return result;
            }

            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            for (int j = i + 1; j < nums.length; j++) {

                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }

                int left = j + 1;
                int right = nums.length - 1;

                while (left < right) {
                    int sum = nums[i] + nums[j] + nums[left] + nums[right];

                    if (sum > target) {
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {

                        result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));

                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }

                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }

                        left++;
                        right--;
                    }
                }
            }
        }

        return result;
    }
```

ChatGPT：

该代码旨在找到一个整数数组 `nums` 中所有和为 `target` 且不重复的四元组。通过排序和双重双指针技术，代码高效地解决了这个问题。

**1. 初始化和排序：** 首先，代码初始化了一个空的列表 `result`，用于存储所有找到的符合条件的四元组。然后，使用 `Arrays.sort(nums)` 对输入的数组进行升序排序。这一步有助于后续使用双指针方法，同时有助于去除重复的四元组。

**2. 遍历数组（第一层循环）：** 代码使用一个 `for` 循环遍历数组，从第一个元素到倒数第四个元素。对于每个元素，执行以下步骤：

- **提前结束条件：** 在循环内部，首先检查当前元素是否大于 `target` 且大于零。如果当前元素大于 `target`，由于数组已经排序，后续的元素也必然大于零，因此不可能再找到和为 `target` 的四元组，直接返回结果。
- **跳过重复元素：** 为了避免重复的四元组，检查当前元素是否和前一个元素相同。如果相同，跳过本次循环，继续下一个元素的处理。这一步确保每个元素只作为第一个数处理一次，避免重复。

**3. 遍历数组（第二层循环）：** 对于每个固定的第一个元素，代码使用另一个 `for` 循环遍历数组，从当前元素的下一个元素到倒数第三个元素。执行以下步骤：

- **跳过重复元素：** 为了避免重复的四元组，检查当前元素是否和前一个元素相同。如果相同，跳过本次循环，继续下一个元素的处理。这一步确保每个元素只作为第二个数处理一次，避免重复。

**4. 使用双指针寻找四元组：** 对于每个固定的前两个元素，使用两个指针 `left` 和 `right` 分别指向剩余数组的头部和尾部。使用 `while` 循环，直到两个指针相遇。具体步骤如下：

- 计算当前四元组的和 `sum`，即 `nums[i] + nums[j] + nums[left] + nums[right]`。
- 根据 `sum` 的值调整指针：
  - 如果 `sum` 大于 `target`，说明和太大，需要减小和，因此将 `right` 指针左移一位。
  - 如果 `sum` 小于 `target`，说明和太小，需要增大和，因此将 `left` 指针右移一位。
  - 如果 `sum` 等于 `target`，说明找到了一个满足条件的四元组，将其添加到结果列表 `result` 中。

**5. 去重处理：** 找到一个符合条件的四元组后，需要继续调整指针 `left` 和 `right`，以避免重复的四元组：

- 将 `left` 指针右移，直到遇到不同的元素，确保每个四元组中的第三个数唯一。
- 将 `right` 指针左移，直到遇到不同的元素，确保每个四元组中的第四个数唯一。

**6. 继续移动指针：** 完成去重处理后，再次移动 `left` 和 `right` 指针，进行下一次查找。

**7. 返回结果：** 遍历完成后，返回结果列表 `result`，其中包含所有找到的不重复的和为 `target` 的四元组。



[18. 4Sum](https://leetcode.cn/problems/4sum/)

Given an array `nums` of `n` integers, return *an array of all the **unique** quadruplets* `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

 

**Example 1:**

```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

**Example 2:**

```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

 

**Constraints:**

- `1 <= nums.length <= 200`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/3-%E5%93%88%E5%B8%8C%E8%A1%A8-81-%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C-LeetCode18.jpeg?raw=true)

上图是由 RealVisXL 生成

关键词：Near is a desert, far is a snowy mountain



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
