# 2-数组-21-移除元素-LeetCode27

> 参考：代码随想录
>
> LeetCode：题目序号27


**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```java
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,3,0,4]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

 

**提示：**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`





```java
    /**
     * 移除元素（双重循环-暴力解法）
     *
     * @param nums 待处理的数组
     * @param val 目标值
     * @return 返回处理后的数组的长度
     * @author CodeZeng1998
     */
    public int removeElement(int[] nums, int val) {
        int size = nums.length;
        for (int i = 0; i < size; i++) {
            if (nums[i] == val) {
                for (int j = i + 1; j < size; j++) {
                    nums[j - 1] = nums[j];
                }
                // 数组元素前移，第一层遍历也需要前移
                i--;
                size--;
            }
        }
        return size;
    }
```



ChatGPT:

这段代码是用来从一个整数数组中移除所有等于指定值的元素，并返回剩余元素的数量。让我逐行解释一下：

1. `public int removeElement(int[] nums, int val) {`: 这是一个公共方法，它接受一个整数数组 `nums` 和一个整数 `val` 作为参数，并返回一个整数。
2. `int size = nums.length;`: 这行代码初始化了一个变量 `size`，它的值等于输入数组 `nums` 的长度，表示数组的初始大小。
3. `for (int i = 0; i < size; i++) {`: 这是一个循环，它从数组的第一个元素开始，一直迭代到数组的最后一个元素。在每次迭代中，使用变量 `i` 表示当前正在检查的元素的索引。
4. `if (nums[i] == val) {`: 在循环内部，首先检查当前元素是否等于指定值 `val`。
5. `for (int j = i + 1; j < size; j++) {`: 如果当前元素等于 `val`，那么内部再次进行一个循环，从当前元素的下一个位置开始，将后面的元素依次向前移动一位，覆盖当前元素。这个内部循环的目的是将所有后续等于 `val` 的元素都向前移动一位，填补掉被移除的元素。
6. `nums[j - 1] = nums[j];`: 这行代码实现了将后续元素向前移动一位的操作。
7. `i--;`: 在外部循环中，当移除一个元素后，需要将外部循环的索引 `i` 减一，以便在下一轮循环中继续检查当前位置的元素，因为在移除元素后，当前位置的元素已经被更新为下一个元素。
8. `size--;`: 同样地，在移除一个元素后，需要将数组的大小减一，以便在返回时反映出剩余元素的数量。
9. `return size;`: 最后，返回更新后的数组大小，即剩余元素的数量。



```java
    /**
     * 移除元素（快慢指针）
     *
     * @param nums 待处理的数组
     * @param val 目标值
     * @return 返回处理后的数组的长度
     * @author CodeZeng1998
     */
    public int removeElement(int[] nums, int val) {
        int slowIndex = 0;
            for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
```

ChatGPT:


这段代码是一个名为`removeElement`的方法，它接受一个整数数组`nums`和一个整数`val`作为输入参数。它的目标是从数组`nums`中移除所有值为`val`的元素，并返回移除后剩余元素的数量。

代码的解释如下：

1. **初始化**：

   - `int slowIndex = 0;`：这行代码将一个变量`slowIndex`初始化为0。它将用作移除元素后数组中的索引位置。
2. **双指针迭代**：

   - `for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {`：这个循环使用一个快指针(`fastIndex`)来遍历数组，从0开始，每次增加1。
- 循环内部：
  
  - if (nums[fastIndex] != val) {：这个条件检查当前位置的元素是否不等于值 val
       - 如果条件为真，表示当前元素不等于 val，因此需要保留在数组中。
         - `nums[slowIndex] = nums[fastIndex];`：它将当前元素(`nums[fastIndex]`)复制到`slowIndex`指示的位置。
        - `slowIndex++;`：在复制元素后，将`slowIndex`递增，指向下一个有效元素应该放置的位置。
3. **返回剩余元素的数量**：

   - `return slowIndex;`：循环完成后，`slowIndex`表示移除所有值为`val`的元素后数组中剩余的有效元素数量。将这个值作为方法的结果返回。

总而言之，这个方法通过使用两个指针(`slowIndex`和`fastIndex`)来遍历数组，高效地移除了数组中所有指定值的实例。它在原地覆盖元素，并返回移除元素后的数组长度。





[27. Remove Element](https://leetcode.cn/problems/remove-element/)

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```java
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

 

**Example 1:**

```
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Example 2:**

```
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

 

**Constraints:**

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
