# 1-数组-12-搜索插入位置-LeetCode35

> 参考：代码随想录
>
> LeetCode：题目序号35



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





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





```java
    /**
     * 搜索插入位置（左闭右闭） [left, right]
     *
     * @param nums 升序排序的数据
     * @param target 目标值（目标值不存在与数组中）
     * @return 返回目标值插入的数组索引
     * @Author: CodeZeng1998
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



ChatGPT：

这段代码实现了一个搜索插入位置的算法，也是基于二分查找的思想。和之前的算法不同的是，这里的目标是要插入一个元素到数组中，并返回它在数组中的插入位置。以下是代码的解释：

- `nums`：升序排序的整数数组。
- `target`：要插入的目标值，假设目标值不存在于数组中。
- 方法返回目标值应该插入的数组索引。

算法步骤如下：

1. 使用两个指针 `left` 和 `right` 分别指向数组的开头和结尾。
2. 进入循环，当 `left` 小于等于 `right` 时进行查找。
3. 计算中间索引 `middle`，采用了防止整数溢出的方式计算，即 `left + ((right - left) >> 1)`。
4. 如果 `nums[middle]` 大于目标值 `target`，则更新 `right` 为 `middle - 1`，因为目标值应该插入到 `middle` 的左侧。
5. 如果 `nums[middle]` 小于目标值 `target`，则更新 `left` 为 `middle + 1`，因为目标值应该插入到 `middle` 的右侧。
6. 如果循环结束仍未找到目标值，则根据不同情况返回插入位置：
   - 如果目标值小于数组中所有元素，则插入到数组开头，返回 `left`。
   - 如果目标值大于数组中所有元素，则插入到数组末尾的下一个位置，返回 `right + 1`。
   - 否则，返回 `right + 1`，即插入到 `right` 的下一个位置。

这个算法的时间复杂度仍然是 O(log n)，其中 n 是数组的长度，因为每次查找都将搜索范围减半。



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

ChatGPT：

这段代码实现了搜索插入位置的算法，与之前的算法相比，不同之处在于区间的定义是左闭右开的 `[left, right)`，而不是左闭右闭的 `[left, right]`。以下是代码的解释：

- `nums`：升序排序的整数数组。
- `target`：要插入的目标值，假设目标值不存在于数组中。
- 方法返回目标值应该插入的数组索引。

算法步骤如下：

1. 使用两个指针 `left` 和 `right` 分别指向数组的开头和结尾的下一个位置（即 `right = nums.length`）。
2. 进入循环，当 `left` 小于 `right` 时进行查找。
3. 计算中间索引 `middle`，采用了防止整数溢出的方式计算，即 `left + ((right - left) >> 1)`。
4. 如果 `nums[middle]` 大于目标值 `target`，则更新 `right` 为 `middle`，因为目标值应该插入到 `middle` 的左侧。
5. 如果 `nums[middle]` 小于目标值 `target`，则更新 `left` 为 `middle + 1`，因为目标值应该插入到 `middle` 的右侧。
6. 如果循环结束仍未找到目标值，则根据不同情况返回插入位置：
   - 如果目标值小于数组中所有元素，则插入到数组开头，返回 `left`。
   - 如果目标值大于数组中所有元素，则插入到数组末尾的下一个位置，返回 `right`。

这个算法的时间复杂度仍然是 O(log n)，其中 n 是数组的长度，因为每次查找都将搜索范围减半。





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



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
