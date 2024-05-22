# 1-数组-61-螺旋矩阵 II-LeetCode59

> LeetCode: 题目序号59



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

 

**示例 1：**

| 1    | 2    | 3    |
| ---- | ---- | ---- |
| 8    | 9    | 4    |
| 7    | 6    | 5    |

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

 

**提示：**

- `1 <= n <= 20`

```java
    /**
     * 螺旋矩阵 II
     *
     * @param n 正整数
     * @return 顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix
     * @author CodeZeng1998
     */
    public int[][] generateMatrix(int n) {
        // 左边界、右边界、上边界、下边界（注意：top 和 bottom，对应二维数据的结构）
        int left = 0;
        int right = n - 1;
        int top = 0;
        int bottom = n - 1;

        // 顺序螺旋排列的 n x n 正方形矩阵
        int[][] matrix = new int[n][n];

        int num = 1;
        int target = n * n;

        while (num <= target) {
            // 按照顺时针的顺序填充数据
            // 上行：从左到右
            for (int i = left; i <= right; i++) {
                matrix[top][i] = num++;
            }
            top++;

            // 右列：从上到下
            for (int i = top; i <= bottom; i++) {
                matrix[i][right] = num++; // top to bottom.
            }
            right--;

            // 下行：从右到左
            for (int i = right; i >= left; i--) {
                matrix[bottom][i] = num++; // right to left.
            }
            bottom--;

            // 左列：从下到上
            for (int i = bottom; i >= top; i--) {
                matrix[i][left] = num++; // bottom to top.
            }
            left++;
        }
        return matrix;
    }
```

ChatGPT：

这段代码定义了一个名为 `generateMatrix` 的方法，接受一个整数 `n` 作为输入，并返回一个 `n x n` 大小的二维数组（矩阵），按螺旋顺序填充了从 1 到 `n * n` 的数字。

- `left`、`right`、`top` 和 `bottom` 是变量，用于跟踪当前螺旋层在矩阵内的边界。
- `matrix` 是存储生成矩阵的二维数组。
- `num` 是用于填充矩阵中数字的变量。
- `target` 是矩阵中的总元素数，即 `n * n`。

然后，代码进入一个 while 循环，直到矩阵中的所有元素都被填充为止。

循环内部：
- 第一个 for 循环填充当前螺旋层的顶部行。
- 第二个 for 循环填充当前螺旋层的最右侧列。
- 第三个 for 循环填充当前螺旋层的底部行。
- 第四个 for 循环填充当前螺旋层的最左侧列。

在每组四个 for 循环之后，边界变量（`left`、`right`、`top`、`bottom`）被更新，以移动到内部螺旋层。这个过程一直持续，直到矩阵中的所有元素都被填充，然后返回填充后的矩阵。





[59. Spiral Matrix II](https://leetcode.cn/problems/spiral-matrix-ii/)

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

 

**Example 1:**

| 1    | 2    | 3    |
| ---- | ---- | ---- |
| 8    | 9    | 4    |
| 7    | 6    | 5    |

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]
```

**Example 2:**

```
Input: n = 1
Output: [[1]]
```

 

**Constraints:**

- `1 <= n <= 20`





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**
