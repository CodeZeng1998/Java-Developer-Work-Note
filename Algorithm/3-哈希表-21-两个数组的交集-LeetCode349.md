# 3-哈希表-21-两个数组的交集-LeetCode349

> 参考：代码随想录
>
> LeetCode: 题目序号349



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的* *交集* 。

输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。



**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

 

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`







## 解法一：使用 HashSet



```java
    /**
     * 两个数组的交集
     *
     * @param nums1 第一个数组
     * @param nums2 第二个数组
     * @return 有相交，返回交集，无相交，返回空数组
     * @author CodeZeng1998
     */
    public int[] intersection(int[] nums1, int[] nums2) {

        if (nums1 == null || nums1.length == 0 || nums2 == null || nums2.length == 0) {
            return new int[0];
        }

        Set<Integer> set = new HashSet<>();

        Set<Integer> intersection = new HashSet<>();
        for (int i : nums1) {
            set.add(i);
        }

        for (int i : nums2) {
            if (set.contains(i)) {
                intersection.add(i);
            }
        }

        int[] resultArray = new int[intersection.size()];
        int j = 0;
        for (Integer i : intersection) {
            resultArray[j++] = i;
        }

        return resultArray;
    }
```

ChatGPT：

这段代码定义了一个名为`intersection`的方法，接受两个整数数组`nums1`和`nums2`作为输入，并返回一个包含两个数组交集的整数数组。两个数组的交集是两个数组共有的元素集合。

以下是代码的详细解释：

1. 方法首先检查输入数组是否为null或为空。如果任何一个为null或为空，它将返回一个空数组 `{}`。
2. 初始化一个名为`set`的HashSet，用于存储第一个数组`nums1`中的唯一元素。
3. 另一个HashSet命名为`intersection`，用于存储两个数组中的公共元素。
4. 第一个循环遍历数组`nums1`中的每个元素，并将其添加到`set`中。
5. 第二个循环遍历数组`nums2`中的每个元素。对于每个元素，它检查它是否存在于`set`中。如果存在，则将元素添加到`intersection`集合中。
6. 在找到公共元素之后，使用`intersection`集合的大小初始化一个整数数组`resultArray`。
7. 将`intersection`集合中的元素复制到`resultArray`中。
8. 最后，返回包含交集元素的`resultArray`。





## 解法二：使用 Hash 数组

* 注意题目的数组大小和数值范围

```java
    /**
     * 两个数组的交集
     *
     * @param nums1 第一个数组
     * @param nums2 第二个数组
     * @return 有相交，返回交集，无相交，返回空数组
     * @author CodeZeng1998
     */
    public int[] intersection(int[] nums1, int[] nums2) {

        int[] hash1Array = new int[1001];

        int[] hash2Array = new int[1001];

        for (int i : nums1) {
            hash1Array[i]++;
        }

        for (int i : nums2) {
            hash2Array[i]++;
        }

        List<Integer> resultList = new ArrayList<>();

        for (int i = 0; i < 1001; i++) {
            if (hash1Array[i] > 0 && hash2Array[i] > 0) {
                resultList.add(i);
            }
        }

        int[] resArray = new int[resultList.size()];

        int index = 0;
        for (Integer value : resultList) {
            resArray[index++] = value;
        }

        return resArray;
    }
```

ChatGPT:

这段代码定义了一个名为`intersection`的方法，接受两个整数数组`nums1`和`nums2`作为输入，并返回一个包含两个数组之间交集元素的数组。

以下是代码的详细解释：

1. `int[] hash1Array = new int[1001];` 和 `int[] hash2Array = new int[1001];`: 这些代码初始化了两个大小为1001的整数数组`hash1Array`和`hash2Array`。这些数组用于分别统计`nums1`和`nums2`中数字的出现次数。
2. 第一个循环遍历了`nums1`的元素。对于每个元素`i`，它在`hash1Array`中将`i`的计数递增。
3. 类似地，第二个循环遍历了`nums2`的元素，并递增`hash2Array`中每个元素的计数。
4. 处理了两个数组之后，代码从0到1000遍历每个索引。对于每个索引`i`，如果`hash1Array`和`hash2Array`中`i`的计数都大于0，则意味着`i`同时存在于两个数组中，因此将`i`添加到名为`resultList`的列表中。
5. 在找到所有共同元素后，代码使用`resultList`创建一个新的整数数组`resArray`。
6. 最后，它遍历`resultList`并填充`resArray`，然后返回`resArray`。



[349. Intersection of Two Arrays](https://leetcode.cn/problems/intersection-of-two-arrays/)

Given two integer arrays `nums1` and `nums2`, return *an array of their* 

*intersection*

. Each element in the result must be **unique** and you may return the result in **any order**.



 

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

 

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
