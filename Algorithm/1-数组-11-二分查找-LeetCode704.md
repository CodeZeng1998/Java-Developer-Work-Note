# 1-数组-11-二分查找-LeetCode704

LeetCode：https://leetcode.cn/problems/binary-search/description/

参考：代码随想录



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



二分查找（左闭右闭）

```java
    /**
     * 二分查找（左闭右闭）
     *
     * @param nums 升序排序的数组
     * @param target 目标值
     * @return 数组索引，如果未找到则返回 -1
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