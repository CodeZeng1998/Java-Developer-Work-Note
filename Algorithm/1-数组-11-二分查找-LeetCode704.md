# 1-数组-11-二分查找-LeetCode704

> 参考：代码随想录
>
> LeetCode：题目序号35



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



题目描述：

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。

**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。
2. `n` 将在 `[1, 10000]`之间。
3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。





二分查找（左闭右闭）

```java
    /**
     * 二分查找（左闭右闭）
     *
     * @param nums 升序排序的数组
     * @param target 目标值
     * @return 数组索引，如果未找到则返回 -1
     * @Author: CodeZeng1998
     */
    public int binarySearch(int[] nums, int target) {

        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            // 当 left、right为较大整数时可能出现溢出，防止溢出，等同于(left + right)/2
            int middle = left + ((right - left) >> 1);

            if (nums[middle] > target) {
                right = middle - 1;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }

        return -1;
    }
```



ChatGPT：

这段代码实现了一个二分查找算法，用于在升序排序的数组中查找目标值。以下是代码的解释：

- `nums`：升序排序的整数数组。
- `target`：要查找的目标值。
- 方法返回目标值在数组中的索引，如果未找到则返回 -1。

算法步骤如下：

1. 首先，检查目标值是否小于数组的第一个元素或大于数组的最后一个元素，如果是，则返回 -1，因为目标值不可能存在于数组中。
2. 使用两个指针 `left` 和 `right` 分别指向数组的开头和结尾。
3. 进入循环，当 `left` 小于等于 `right` 时进行查找。
4. 计算中间索引 `middle`，采用了防止整数溢出的方式计算，即 `left + ((right - left) >> 1)`。
5. 如果 `nums[middle]` 大于目标值 `target`，则更新 `right` 为 `middle - 1`，因为目标值可能在左半部分。
6. 如果 `nums[middle]` 小于目标值 `target`，则更新 `left` 为 `middle + 1`，因为目标值可能在右半部分。
7. 如果 `nums[middle]` 等于目标值 `target`，则直接返回 `middle`，表示找到了目标值的索引。
8. 如果循环结束仍未找到目标值，则返回 -1，表示未找到目标值。

这个算法的时间复杂度为 O(log n)，其中 n 是数组的长度，因为每次查找都将搜索范围减半。







二分查找（左闭右开）

```java
    /**
     * 二分查找（左闭右开）
     *
     * @param nums 升序排序的数组
     * @param target 目标值
     * @return 数组索引，如果未找到则返回 -1
     */
    public int binarySearch(int[] nums, int target) {
        int left = 0;
        // 定义target在左闭右开的区间里，即：[left, right)
        int right = nums.length;

        // 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
        while (left < right) {
            // 当 left、right为较大整数时可能出现溢出，防止溢出，等同于(left + right)/2
            int middle = left + ((right - left) >> 1);

            if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }

        return -1;
    }
```



GhatGPT：

这段代码实现了一个二分查找算法，与之前的代码相比，不同之处在于区间的定义是左闭右开的 `[left, right)`，而不是左闭右闭的 `[left, right]`。以下是代码的解释：

- `nums`：升序排序的整数数组。
- `target`：要查找的目标值。
- 方法返回目标值在数组中的索引，如果未找到则返回 -1。

算法步骤如下：

1. 定义两个指针 `left` 和 `right`，分别指向数组的开头和结尾（但是 `right` 指向的是无效的位置，即数组最后一个元素的下一个位置）。
2. 进入循环，当 `left` 小于 `right` 时进行查找。
3. 计算中间索引 `middle`，采用了防止整数溢出的方式计算，即 `left + ((right - left) >> 1)`。
4. 如果 `nums[middle]` 大于目标值 `target`，则更新 `right` 为 `middle`，因为目标值可能在左半部分，但是 `middle` 不包含在搜索范围内。
5. 如果 `nums[middle]` 小于目标值 `target`，则更新 `left` 为 `middle + 1`，因为目标值可能在右半部分。
6. 如果 `nums[middle]` 等于目标值 `target`，则直接返回 `middle`，表示找到了目标值的索引。
7. 如果循环结束仍未找到目标值，则返回 -1，表示未找到目标值。

这个算法与之前的算法相比，在终止条件和区间的定义上有所不同，但是原理和时间复杂度仍然是相同的，都是 O(log n)。





Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
```

**Example 2:**

```
Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
