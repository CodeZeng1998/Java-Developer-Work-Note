# 1-数组-51-水果成篮-LeetCode904

> LeetCode: 题目序号904



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 `fruits` 表示，其中 `fruits[i]` 是第 `i` 棵树上的水果 **种类** 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

- 你只有 **两个** 篮子，并且每个篮子只能装 **单一类型** 的水果。每个篮子能够装的水果总量没有限制。
- 你可以选择任意一棵树开始采摘，你必须从 **每棵** 树（包括开始采摘的树）上 **恰好摘一个水果** 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
- 一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 `fruits` ，返回你可以收集的水果的 **最大** 数目。

 

**示例 1：**

```
输入：fruits = [1,2,1]
输出：3
解释：可以采摘全部 3 棵树。
```

**示例 2：**

```
输入：fruits = [0,1,2,2]
输出：3
解释：可以采摘 [1,2,2] 这三棵树。
如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。
```

**示例 3：**

```
输入：fruits = [1,2,3,2,2]
输出：4
解释：可以采摘 [2,3,2,2] 这四棵树。
如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。
```

**示例 4：**

```
输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
输出：5
解释：可以采摘 [1,2,1,1,2] 这五棵树。
```

 

**提示：**

- `1 <= fruits.length <= 105`
- `0 <= fruits[i] < fruits.length`



```java
    /**
     * 水果成篮（滑动窗口）
     *
     * @param fruits 待处理的数组
     * @return 返回符合条件的最大取到的水果的数量
     * @author CodeZeng1998
     */
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> fruitsTypeAndNumberMap = new HashMap<>();

        int left = 0;
        int maxFruitsNumber = 0;

        for (int right = 0; right < fruits.length; right++) {
            fruitsTypeAndNumberMap.put(
                    fruits[right], fruitsTypeAndNumberMap.getOrDefault(fruits[right], 0) + 1);
            while (fruitsTypeAndNumberMap.size() > 2) {
                fruitsTypeAndNumberMap.put(
                        fruits[left], fruitsTypeAndNumberMap.get(fruits[left]) - 1);
                if (fruitsTypeAndNumberMap.get(fruits[left]) == 0) {
                    fruitsTypeAndNumberMap.remove(fruits[left]);
                }
                left++;
            }
            maxFruitsNumber = Math.max(maxFruitsNumber, right - left + 1);
        }
        return maxFruitsNumber;
    }
```



ChatGPT：

这段代码定义了一个名为`totalFruit`的方法，它以整数数组`fruits`作为输入，并返回一个整数。目标是找到最大数量的水果，这些水果只有两种类型。

以下是代码的分解：

1. 它初始化了一个HashMap `fruitsTypeAndNumberMap`，用于跟踪水果的类型和数量。
2. 它初始化了两个指针`left`和`right`，用于跟踪滑动窗口。
3. 它初始化一个变量`maxFruitsNumber`，用于跟踪可以用两种类型收集的最大水果数量。
4. 使用`right`指针迭代`fruits`数组。
5. 在循环内部，使用当前水果类型和其计数更新`fruitsTypeAndNumberMap`。
6. 如果地图的大小超过2，则进入while循环，表示当前窗口中有两种以上的水果类型。
7. 在while循环内部，通过移动`left`指针并相应地更新地图，调整窗口，直到窗口中只有两种水果。
8. 使用当前值和当前窗口大小的最大值更新`maxFruitsNumber`。
9. 最后，返回`maxFruitsNumber`，它表示在`fruits`的任何子数组中只有两种类型时可以收集到的最大水果数量。





[904. Fruit Into Baskets](https://leetcode.cn/problems/fruit-into-baskets/)

You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array `fruits` where `fruits[i]` is the **type** of fruit the `ith` tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

- You only have **two** baskets, and each basket can only hold a **single type** of fruit. There is no limit on the amount of fruit each basket can hold.
- Starting from any tree of your choice, you must pick **exactly one fruit** from **every** tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
- Once you reach a tree with fruit that cannot fit in your baskets, you must stop.

Given the integer array `fruits`, return *the **maximum** number of fruits you can pick*.

 

**Example 1:**

```
Input: fruits = [1,2,1]
Output: 3
Explanation: We can pick from all 3 trees.
```

**Example 2:**

```
Input: fruits = [0,1,2,2]
Output: 3
Explanation: We can pick from trees [1,2,2].
If we had started at the first tree, we would only pick from trees [0,1].
```

**Example 3:**

```
Input: fruits = [1,2,3,2,2]
Output: 4
Explanation: We can pick from trees [2,3,2,2].
If we had started at the first tree, we would only pick from trees [1,2].
```

 

**Constraints:**

- `1 <= fruits.length <= 105`
- `0 <= fruits[i] < fruits.length`



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
