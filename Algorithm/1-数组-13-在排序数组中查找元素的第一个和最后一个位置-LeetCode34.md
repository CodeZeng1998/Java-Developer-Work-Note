# 1-数组-13-在排序数组中查找元素的第一个和最后一个位置-LeetCode34



更多内容欢迎关注我的Github（持续更新中，欢迎Star✨）[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)

参考：代码随想录（做题顺序很重要，后续算法代码优先以这个题目顺序为主）



[34. Find First and Last Position of Element in Sorted Array](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**

```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**

```
Input: nums = [], target = 0
Output: [-1,-1]
```

 

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`



```java
    /**
     * 在排序数组中查找元素的第一个和最后一个位置 (LeetCode 34)
     *
     *三种情况：
     * （1）目标值大于数组中的最大值，小于数组中的最小值；
     * （2）目标值在数组中且不唯一；
     * （3）目标值不存在数组中或者目标值在数组中只有一个；
     *
     * @param nums 升序排序的数据
     * @param target 目标值
     * @return 返回目标值出现的第一个和最后一个位置，不存在则返回 [-1, -1]
     */
    public int[] findFirstAndLastPositionOfElementInSortedArray(int[] nums, int target) {
        int firstPosition = findFirstPositionOfElementInSortedArray(nums, target);
        int lastPosition = findLastPositionOfElementInSortedArray(nums, target);

        if (firstPosition == -2 || lastPosition == -2) {
            return new int[] {-1, -1};
        }

        if (lastPosition - firstPosition > 1) {
            return new int[] {firstPosition + 1, lastPosition - 1};
        }

        return new int[] {-1, -1};
    }

    /**
     * 获取元素在数组中第一次出现的位置
     *
     * @param nums 升序排序的数据
     * @param target 目标值
     * @return 找到元素第一次出现的位置
     */
    public int findFirstPositionOfElementInSortedArray(int[] nums, int target) {
        int firstPosition = -2;

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int middle = left + ((right - left) >> 1);

            if (nums[middle] < target) {
                left = middle + 1;
            } else {
                right = middle - 1;
                firstPosition = right;
            }
        }

        return firstPosition;
    }

    /**
     * 获取元素在数组中最后一次出现的位置
     *
     * @param nums 升序排序的数据
     * @param target 目标值
     * @return 找到元素最后一次出现的位置
     */
    public int findLastPositionOfElementInSortedArray(int[] nums, int target) {
        int lastPosition = -2;

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int middle = left + ((right - left) >> 1);

            if (nums[middle] > target) {
                right = middle - 1;
            } else {
                left = middle + 1;
                lastPosition = left;
            }
        }

        return lastPosition;
    }
```





ChatGPT 解释如下：

这段Java代码用于在排序数组中查找目标元素的第一个和最后一个位置。以下是代码的详细解释：

1. `findFirstAndLastPositionOfElementInSortedArray` 方法：
   - 接受一个整数数组 `nums` 和一个目标整数 `target` 作为输入。
   - 调用两个辅助方法 `findFirstPositionOfElementInSortedArray` 和 `findLastPositionOfElementInSortedArray` 来找到排序数组中目标元素的第一个和最后一个位置。
   - 如果第一个位置或最后一个位置未找到（表示为 -2），则返回 [-1, -1]。
   - 如果最后一个位置和第一个位置之间的差大于1，则意味着它们之间有元素，因此返回范围不包括第一个和最后一个位置。
   - 否则，返回 [-1, -1]。
2. `findFirstPositionOfElementInSortedArray` 方法：
   - 接受一个整数数组 `nums` 和一个目标整数 `target` 作为输入。
   - 使用二分查找来找到目标元素的第一次出现位置。
   - 将 `firstPosition` 初始化为 -2，表示尚未找到该元素。
   - 将 `left` 指针设置为 0，将 `right` 指针设置为数组长度减 1。
   - 在 while 循环中，计算 `middle` 索引并将该索引处的值与目标进行比较。
   - 如果 `middle` 处的值小于目标，则将 `left` 指针更新为 `middle + 1`。
   - 如果 `middle` 处的值大于或等于目标，则将 `right` 指针更新为 `middle - 1`，并将 `firstPosition` 更新为 `right`。
   - 循环直到 `left` 大于 `right`。
   - 返回 `firstPosition`。
3. `findLastPositionOfElementInSortedArray` 方法：
   - 和 `findFirstPositionOfElementInSortedArray` 方法接受相同的输入参数。
   - 使用二分查找来找到目标元素的最后一次出现位置。
   - 将 `lastPosition` 初始化为 -2。
   - 和 `findFirstPositionOfElementInSortedArray` 方法类似，但比较方式相反。
   - 返回 `lastPosition`。

总的来说，这些方法利用二分查找高效地在排序数组中找到目标元素的第一个和最后一个位置。如果未找到该元素，则返回 [-1, -1]。如果第一个和最后一个位置之间有元素，则返回范围不包括它们。











[34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` 是一个非递减数组
- `-109 <= target <= 109`











