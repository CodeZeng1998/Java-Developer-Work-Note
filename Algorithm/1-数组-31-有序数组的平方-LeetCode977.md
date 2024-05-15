# 1-数组-31-有序数组的平方-LeetCode977

> LeetCode: 题目序号977



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。



**示例 1：**

```
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

**示例 2：**

```
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

 

**提示：**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 已按 **非递减顺序** 排序

 

**进阶：**

- 请你设计时间复杂度为 `O(n)` 的算法解决本问题



这道题目的**关键**点只有一个：**由于是升序数组，数值的平方的最大值只可能在左右两边**。

```java
    /**
     * 有序数组的平方（相向双指针法）
     *
     * @param nums 目标数组
     * @return 升序排序的原数组值的平方
     * @author CodeZeng1998
     */
    public int[] sortedSquares(int[] nums) {

        int[] resultArray = new int[nums.length];
        int left = 0;
        int right = nums.length - 1;

        int resultArrayIndex = nums.length - 1;

        while (left <= right) {
            if (nums[left] * nums[left] < nums[right] * nums[right]) {
                resultArray[resultArrayIndex] = nums[right] * nums[right];
                right--;
                resultArrayIndex--;
            } else {
                resultArray[resultArrayIndex] = nums[left] * nums[left];
                left++;
                resultArrayIndex--;
            }
        }
        return resultArray;
    }
```

ChatGPT：

这个代码定义了一个方法 `sortedSquares`，它接受一个整数数组 `nums` 作为输入，并返回一个新的数组，其中每个元素是 `nums` 对应元素的平方，并且按非递减顺序排列。以下是代码的逐步解释：

1. **初始化**：
   - 创建一个数组 `resultArray` 来存放平方值，长度与 `nums` 相同。
   - 两个指针 `left` 和 `right` 被初始化为分别指向 `nums` 数组的开始和结束。
   - `resultArrayIndex` 初始化为 `resultArray` 的最后一个索引，以从末尾开始填充。
2. **主循环**：
   - 循环运行直到 `left` 小于或等于 `right`。
   - 在循环内，比较 `left` 和 `right` 指向的值的平方。
   - 如果 `nums[left]` 的平方小于 `nums[right]` 的平方，则将 `nums[right]` 的平方放入 `resultArray` 中 `resultArrayIndex` 指向的位置，然后 `right` 减一，`resultArrayIndex` 减一。
   - 如果 `nums[left]` 的平方大于或等于 `nums[right]` 的平方，则将 `nums[left]` 的平方放入 `resultArray` 中 `resultArrayIndex` 指向的位置，然后 `left` 加一，`resultArrayIndex` 减一。
3. **返回结果**：
   - 循环结束后，`resultArray` 包含按非递减顺序排列的平方值，并作为输出返回。





[977. Squares of a Sorted Array](https://leetcode.cn/problems/squares-of-a-sorted-array/)

Given an integer array `nums` sorted in **non-decreasing** order, return *an array of **the squares of each number** sorted in non-decreasing order*.

 

**Example 1:**

```
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].
```

**Example 2:**

```
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

 

**Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
