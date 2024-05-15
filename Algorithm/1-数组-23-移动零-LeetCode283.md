# 1-数组-23-移动零-LeetCode283

> LeetCode：题目序号283



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**


[283. 移动零](https://leetcode.cn/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

 

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

 

**提示**:

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

 

**进阶：**你能尽量减少完成的操作次数吗？







```java
    /**
     * 移动零（快慢指针）
     *
     * @param nums 待处理的目标数组
     * @author CodeZeng1998
     */
    public void moveZeroes(int[] nums) {

        if (nums.length == 0 || nums.length == 1) {
            return;
        }

        int slowIndex = 0;
        // 将非零数据前移
        for (int fastindex = 0; fastindex <= nums.length - 1; fastindex++) {
            if (nums[fastindex] != 0) {
                nums[slowIndex] = nums[fastindex];
                slowIndex++;
            }
        }

      	// 非零数据已经全部前移，将数组后面的数据设置成 0
        while (slowIndex <= nums.length - 1) {
            nums[slowIndex] = 0;
            slowIndex++;
        }
    }
```



ChatGPT：

**目的:**

该方法将数组 `nums` 中的所有零移动到末尾，同时保持非零元素的相对顺序。

**逐步解释:**

1. **检查特殊情况**:

   ```java
   if (nums.length == 0 || nums.length == 1) {
       return;
   }
   ```

   - 如果数组为空或仅包含一个元素，则无需移动，因此方法立即返回。

2. **初始化指针**:

   ```java
   int slowIndex = 0;
   ```

   - `slowIndex` 用于跟踪下一个非零元素应该放置的位置。

3. **使用 `fastindex` 遍历数组**:

   ```java
   for (int fastindex = 0; fastindex <= nums.length - 1; fastindex++) {
       if (nums[fastindex] != 0) {
           nums[slowIndex] = nums[fastindex];
           slowIndex++;
       }
   }
   ```

   - 使用 `fastindex` 遍历数组。
   - 如果 `fastindex` 位置的元素不为零，则将其移动到 `slowIndex` 指示的位置。
   - 然后递增 `slowIndex` 以指向下一个可能的非零元素位置。
   - 这有效地将所有非零元素按原顺序移到数组前部。

4. **用零填充剩余位置**:

   ```java
   while (slowIndex <= nums.length - 1) {
       nums[slowIndex] = 0;
       slowIndex++;
   }
   ```

   - 在所有非零元素移到前部后，`slowIndex` 将指向应放置第一个零的位置。
   - 该循环用零填充数组的其余部分。

**总结:**

该方法使用两个指针：一个用于遍历数组 (`fastindex`)，一个用于放置非零元素 (`slowIndex`)。它确保所有非零元素按顺序移到数组前部，然后用零填充数组的其余部分。









[283. Move Zeroes](https://leetcode.cn/problems/move-zeroes/)

Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

 

**Example 1:**

```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Example 2:**

```
Input: nums = [0]
Output: [0]
```

 

**Constraints:**

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

 

**Follow up:** Could you minimize the total number of operations done?





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
