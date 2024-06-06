# 3-哈希表-71-三数之和-LeetCode15

> 参考：代码随想录
>
> LeetCode: 题目序号15



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





[15. 三数之和](https://leetcode.cn/problems/3sum/)

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`





关键内容：

* 答案中**不可以包含重复的三元组**

```java

    /**
     * 三数之和（双指针）
     *
     * @param nums 目标数组
     * @return 不可以包含重复的三元组
     * @author CodeZeng1998
     */
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> result = new ArrayList<>();
        // 数组升序排序
        Arrays.sort(nums);

        for (int i = 0; i < nums.length; i++) {
            // 数组是升序排序，第一个数大于0，就说明后序都没有满足条件的数据了
            if (nums[i] > 0) {
                return result;
            }

            // 题目要求不可以包含重复的三元组，所以需要去重，如果当前数和前一个数相等，就跳过本次循环
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];

                if (sum > 0) {
                    right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    // 去重
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }

                    // 去重
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
```

ChatGPT：

该代码的功能是找到一个整数数组 `nums` 中所有和为零且不重复的三元组。通过排序和双指针技术，代码能够高效地找到符合条件的三元组。

**1. 初始化和排序：** 首先，代码初始化了一个空的列表 `result`，用于存储所有找到的符合条件的三元组。然后，使用 `Arrays.sort(nums)` 对输入的数组进行升序排序。排序的目的是为了方便后续使用双指针方法，同时有助于去除重复的三元组。

**2. 遍历数组：** 代码使用一个 `for` 循环遍历数组，从第一个元素到倒数第三个元素。对于每个元素，执行以下步骤：

- **提前结束条件：** 在循环内部，首先检查当前元素是否大于零。如果当前元素大于零，由于数组已经排序，后续的元素也必然大于零，因此不可能再找到和为零的三元组，直接返回结果。
- **跳过重复元素：** 为了避免重复的三元组，检查当前元素是否和前一个元素相同。如果相同，跳过本次循环，继续下一个元素的处理。这一步确保每个元素只作为第一个数处理一次，避免重复。

**3. 使用双指针寻找三元组：** 对于每个固定的第一个元素，使用两个指针 `left` 和 `right` 分别指向剩余数组的头部和尾部。使用 `while` 循环，直到两个指针相遇。具体步骤如下：

- 计算当前三元组的和 `sum`，即 `nums[i] + nums[left] + nums[right]`。
- 根据 `sum` 的值调整指针：
  - 如果 `sum` 大于零，说明和太大，需要减小和，因此将 `right` 指针左移一位。
  - 如果 `sum` 小于零，说明和太小，需要增大和，因此将 `left` 指针右移一位。
  - 如果 `sum` 等于零，说明找到了一个满足条件的三元组，将其添加到结果列表 `result` 中。

**4. 去重处理：** 找到一个符合条件的三元组后，需要继续调整指针 `left` 和 `right`，以避免重复的三元组：

- 将 `left` 指针右移，直到遇到不同的元素，确保每个三元组中的第二个数唯一。
- 将 `right` 指针左移，直到遇到不同的元素，确保每个三元组中的第三个数唯一。

**5. 继续移动指针：** 完成去重处理后，再次移动 `left` 和 `right` 指针，进行下一次查找。

**6. 返回结果：** 遍历完成后，返回结果列表 `result`，其中包含所有找到的不重复的和为零的三元组。



[15. 3Sum](https://leetcode.cn/problems/3sum/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

 

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

**Example 2:**

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

**Example 3:**

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

 

**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Algorithm/image/3-%E5%93%88%E5%B8%8C%E8%A1%A8-71-%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C-LeetCode15.jpeg?raw=true)

上图是由 RealVisXL 生成

关键词：a girl seeing the sunset on the beach





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
