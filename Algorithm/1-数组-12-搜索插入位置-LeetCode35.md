# 1-数组-12-搜索插入位置-LeetCode35

[35. Search Insert Position](https://leetcode.cn/problems/search-insert-position/)

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

 

**Example 1:**

```
Input: nums = [1,3,5,6], target = 5
Output: 2
```

**Example 2:**

```
Input: nums = [1,3,5,6], target = 2
Output: 1
```

**Example 3:**

```
Input: nums = [1,3,5,6], target = 7
Output: 4
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-104 <= target <= 104`





```java
    /**
     * 搜索插入位置（左闭右闭） [left, right]
     *
     * @param nums 升序排序的数据
     * @param target 目标值（目标值不存在与数组中）
     * @return 返回目标值插入的数组索引
     */
    public int searchInsertPosition(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {
            int middle = left + ((right - left) >> 1);

            if (nums[middle] > target) {
                right = middle - 1;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }

        // 2.目标值在数组所有元素之前
        // 3.目标值插入数组中 
        // 4.目标值在数组所有元素之后 return right + 1;
        return right + 1;
    }
```





```java
    /**
     * 搜索插入位置（左闭右开） [left, right)
     *
     * @param nums 升序排序的数据
     * @param target 目标值（目标值不存在与数组中）
     * @return 返回目标值插入的数组索引
     */
    public int searchInsertPosition(int[] nums, int target) {

        int left = 0;
        int right = nums.length;

        while (left < right) {
            int middle = left + ((right - left) >> 1);

            if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }
        // 目标值在数组所有元素之前 [0,0)
        // 目标值插入数组中的位置 [left, right) ，return right 即可
        // 目标值在数组所有元素之后的情况 [left, right)，因为是右开区间，所以 return right
        return right;
    }
```









[35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-104 <= target <= 104`



