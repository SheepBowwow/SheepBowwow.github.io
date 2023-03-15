---
title: LeetCode基础——数组
date: 2023-03-13
---

# leetcode

## 数据结构

### 数组

#### 1. 存在重复元素

给你一个整数数组 `nums` 。如果任一值在数组中出现 **至少两次** ，返回 `true` ；如果数组中每个元素互不相同，返回 `false` 。



##### 方法一：排序

在对数字从小到大排序之后，数组的重复元素一定出现在相邻位置中。因此，我们可以扫描已排序的数组，每次判断相邻的两个元素是否相等，如果相等则说明存在重复的元素。

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                return true;
            }
        }
        return false;
    }
};
```



##### 方法二：哈希表

对于数组中每个元素，我们将它插入到哈希表中。如果插入一个元素时发现该元素已经存在于哈希表中，则说明存在重复的元素。

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> s;
        for (int x: nums) {
            if (s.find(x) != s.end()) {
                return true;
            }
            s.insert(x);
        }
        return false;
    }
};
```

> set的find方法，如果没有找到指定元素，则返回 s.end()
>
> 借此来判断是否存在同一元素



##### 方法三：set容器

``` c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set<int> test;
        for(int i = 0; i < nums.size(); i++)
        {
            if(test.count(nums[i]))
            {
                return true;
            }
            test.insert(nums[i]);
        }
        return false;
    }
};
```

> 利用count函数，如果数组中没有该数字，返回真，否则插入该数字，利用set容器中不能有重复元素这一特点



#### 2.最大子数组和

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

``` c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int pre = 0, maxAns = nums[0];
        for (const auto &x: nums) {
            pre = max(pre + x, x);
            maxAns = max(maxAns, pre);
        }
        return maxAns;
    }
};
```

> 动态规划，将pre＋x和 x 比较，如果pre ＋ x比x小，说明pre小于0，利用一个变量maxAns来表示当前最大的，连续子数组的和，如果pre比它大，那就替换。



#### 3.两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

方法一：暴力枚举

> 第一遍自己的做法

``` c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int num;
        int a;
        int b;
        for(int i = 0; i < nums.size();i++)
        {
            num = target - nums[i];
            for(int j = 1; j < nums.size(); j ++)
            {
                if(num == nums[j])
                {
                    if(i != j){
                        a = i;
                        b = j;
                        break;
                    }
                }
            }
        }
        vector<int> dest;
        dest.push_back(a);
        dest.push_back(b);
        return dest;
    }
};
```

时间复杂程度:O(n²)



##### 方法二：hashtable(Map容器)

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        map<int,int> a;//提供一对一的hash
        vector<int> b(2,-1);//用来承载结果，初始化一个大小为2，值为-1的容器b
        for(int i=0;i<nums.size();i++)
        {
            if(a.count(target-nums[i])>0)
            {
                b[0]=a[target-nums[i]];
                b[1]=i;
                break;
            }
            a[nums[i]]=i;//反过来放入map中，用来获取结果下标
        }
        return b;
    };
};
```



#### 4.合并两个有序数组

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

##### 方法一：

``` c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for(int i = 0; i != n; i++)
        {
            nums1[m + i] = nums2[i];
        }

        sort(nums1.begin(),nums1.end());
    }
};
```

##### 方法二：双指针

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int p1 = 0, p2 = 0;
        int sorted[m+n];
        int cur;
        while (p1 < m || p2 < n)
        {
            if(p1 == m)
            {
                cur = nums2[p2++];
            }
            else if (p2 == n)
            {
                cur = nums1[p1++];
            }
            else if (nums1[p1] < nums2[p2])
            {
                cur = nums1[p1++];
            }
            else
            {
                cur = nums2[p2++];
            }
            sorted[p1 + p2 - 1] = cur;
        }

        for(int i = 0; i != m + n; i++)
        {
            nums1[i] = sorted[i];
        }
    }
};
```

> 新声明一个数组，利用两个指针分别指向两个数组nums1，nums2，对比二者大小，放入新的数组
>
> 最后将新的数组拷贝到nums1中



#### 5. 两个数组的交集 II

给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

##### 方法一：

``` c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        set<int> counts; //创建一个set容器，利用其count()函数，可以避免数组中出现重复添加同一数的情况
        vector<int> res; //返回的结果
        //双重循环，从nums1的第一个元素开始遍历
        for(int i = 0; i < nums1.size();i++)
        {
            for(int j = 0; j < nums2.size(); j++)
            {
                //判断条件，如果数组1和数组2的某个值相等，那么就判断再counts中是否存在数组2中该数组的下标
                //如果存在说明已经添加过
                if(nums1[i] == nums2[j] && counts.count(j) == 0)
                {
                        res.push_back(nums1[i]); //将相同的值放入res中
                        counts.insert(j); //记录下nums2中数的下标，防止重复添加
                        break;
                }
            }
        }
        return res;
    }
};
```



##### 方法二：双指针

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        //先将两个数组分别排序
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        int i = 0; //nums1的下标
        int j = 0; //nums2的下标
        vector<int> res; //要返回的数组
        while(i < nums1.size() && j < nums2.size())
        {
            //对比两个指针分别对应的数的大小
            //1、如果两数相等，那么两个指针同时向后偏移，并将该数添加到res中
            if(nums1[i] == nums2[j])
            {
                res.push_back(nums1[i]);
                i++,j++;
            } 
            //如果两数不相等，那么小的数的指针向后偏移
            else if(nums1[i] > nums2[j])
            {
                j++;
            }
            else
            {
                i++;
            }
        }
        return res;
    }
};
```

> 双指针只需要一个循环，效率远远高于双重循环



#### 6.买卖股票的最佳时机

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

##### 方法一：暴力法

``` c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = (int)prices.size(), ans = 0;
        for (int i = 0; i < n; ++i){
            for (int j = i + 1; j < n; ++j) {
                ans = max(ans, prices[j] - prices[i]);
            }
        }
        return ans;
    }
};
```

##### 方法二：动态规划

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = prices[0];
        int res = 0;
        for(int i = 0; i < prices.size();i++)
        {
            //找到最低价，即买入时机
            minPrice = min(prices[i],minPrice);
            //当天价格减去最低价，取最大值
            res = max(res,prices[i] - minPrice);
        }
        return res;
    }
};
```



#### 7.重塑矩阵

在 MATLAB 中，有一个非常有用的函数 `reshape` ，它可以将一个 `m x n` 矩阵重塑为另一个大小不同（`r x c`）的新矩阵，但保留其原始数据。

给你一个由二维数组 `mat` 表示的 `m x n` 矩阵，以及两个正整数 `r` 和 `c` ，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的 **行遍历顺序** 填充。

如果具有给定参数的 `reshape` 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

  

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg)

```txt
输入：mat = [[1,2],[3,4]], r = 1, c = 4
输出：[[1,2,3,4]]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg)

```txt
输入：mat = [[1,2],[3,4]], r = 2, c = 4
输出：[[1,2],[3,4]]
```


**提示：**

-   `m == mat.length`
-   `n == mat[i].length`
-   `1 <= m, n <= 100`
-   `-1000 <= mat[i][j] <= 1000`
-   `1 <= r, c <= 300`

##### 方法一：

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {
        //先获得元素的总个数
        int totalSize = 0;
        vector<vector<int>> res;
        vector<int> nums;
        for(int i = 0; i < mat.size();i++)
        {
            totalSize += mat[i].size();
            for(int j = 0; j < mat[i].size(); j++)
            {
                nums.push_back(mat[i][j]);
            }
        }
        //将总个数和 r * c 对比 若不相等，说明不合法，输出原数组
        if(totalSize != r*c)
        {
            return mat;
        }

        vector<int> temp; //用于存放每组的数
        for(int i = 0, j = 0; j < totalSize;++j)
        {
            temp.push_back(nums[j]);
            if(i++ == (c-1))
            {
                res.push_back(temp);
                i = 0;
                temp.clear();
            }
        }
        return res;
    }
};
```



#### 8.杨辉三角

给定一个非负整数 *`numRows`，*生成「杨辉三角」的前 *`numRows`* 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

  

**示例 1:**

```txt
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

**示例 2:**

```txt
输入: numRows = 1
输出: [[1]]
```


**提示:**

-   `1 <= numRows <= 30`

##### 方法一：

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ret(numRows);
        for (int i = 0; i < numRows; ++i) {
            ret[i].resize(i + 1);
            ret[i][0] = ret[i][i] = 1;
            for (int j = 1; j < i; ++j) {
                ret[i][j] = ret[i - 1][j] + ret[i - 1][j - 1];
            }
        }
        return ret;
    }
};
```

##### 方法二：

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<int> nums,pre_Nums;
        vector<vector<int>> res;
        if(numRows == 1)
        {
            nums.push_back(1);
            res.push_back(nums);
            return res;
        }
        if(numRows == 2)
        {
            nums.push_back(1);
            res.push_back(nums);
            nums.push_back(1);
            res.push_back(nums);
            return res;
        }

        //将[1]，[1，1]先插入到结果容器中
        nums.push_back(1);
        res.push_back(nums);
        nums.push_back(1);
        res.push_back(nums);
        pre_Nums = nums;
        for(int i = 3; i <= numRows; i++)
        {
            //置空nums
            nums.clear();
            nums.resize(i);
            nums[0] = nums[i-1] = 1;
            for(int j = 1; j < i - 1; j++)
            {   
                nums[j] = pre_Nums[j] + pre_Nums[j - 1];
            }
            pre_Nums = nums; //前一个数组向后偏移
            res.push_back(nums);
        }
        return res;
    }
};
```



#### 9.⭐️有效的数独

请你判断一个 `9 x 9` 的数独是否有效。只需要 **根据以下规则** ，验证已经填入的数字是否有效即可。

1.  数字 `1-9` 在每一行只能出现一次。
2.  数字 `1-9` 在每一列只能出现一次。
3.  数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

  

**注意：**

-   一个有效的数独（部分已被填充）不一定是可解的。
-   只需要根据以上规则，验证已经填入的数字是否有效即可。
-   空白格用 `'.'` 表示。

  

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png)

```txt
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

**示例 2：**

```txt
输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false
解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```


**提示：**

-   `board.length == 9`
-   `board[i].length == 9`
-   `board[i][j]` 是一位数字（`1-9`）或者 `'.'`

##### 方法一：暴力枚举

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        //将每一行的元素放到set中，并判断set容器中是否存在该元素
        set<char> judge; //judge 判断
        for(int i = 0; i < 9; i++)
        {
            for(int j = 0; j < 9; j++)
            {
                if(judge.count(board[i][j]) != 0)
                {
                    return false;
                }
                else
                {
                    if(board[i][j] != '.')
                    {
                        judge.insert(board[i][j]);
                    }
                }
            }
            judge.clear();
        }
        //判断列
        for(int i = 0; i < 9; i++)
        {
            for(int j = 0; j < 9; j++)
            {
                if(judge.count(board[j][i]) != 0)
                {
                    return false;
                }
                else
                {
                    if(board[j][i] != '.')
                    {
                        judge.insert(board[j][i]);
                    }
                }
            }
            judge.clear();
        }

        //九个大格子
        int m = 0;
        int n = 0;
        int m_limit = 3;
        int n_limit = 3;
        for(int i = 0; i < 9; i++)
        {
            for(m; m < m_limit; m++)
            {
                for(n; n < n_limit; n++)
                {
                    if(judge.count(board[m][n]) != 0)
                    {
                        return false;
                    }
                    else
                    {
                        if(board[m][n] != '.')
                        {
                            judge.insert(board[m][n]);
                        }
                    }
                }
                n-=3;
            }
            judge.clear();
            
            if(m < 9)
            {
                m_limit += 3;
            }
            else
            {
                m = 0;
                m_limit = 3;
                n+=3;
                n_limit+=3;
            }
        }
        return true;
    }
};
```

##### ⭐️方法二：一次遍历

来自力扣评论区

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int row[9][10] = {0};// 哈希表存储每一行的每个数是否出现过，默认初始情况下，每一行每一个数都没有出现过
        // 整个board有9行，第二维的维数10是为了让下标有9，和数独中的数字9对应。
        int col[9][10] = {0};// 存储每一列的每个数是否出现过，默认初始情况下，每一列的每一个数都没有出现过
        int box[9][10] = {0};// 存储每一个box的每个数是否出现过，默认初始情况下，在每个box中，每个数都没有出现过。整个board有9个box。
        for(int i=0; i<9; i++){
            for(int j = 0; j<9; j++){
                // 遍历到第i行第j列的那个数,我们要判断这个数在其所在的行有没有出现过，
                // 同时判断这个数在其所在的列有没有出现过
                // 同时判断这个数在其所在的box中有没有出现过
                if(board[i][j] == '.') continue;
                int curNumber = board[i][j]-'0';
                if(row[i][curNumber]) return false; 
                if(col[j][curNumber]) return false;
                if(box[j/3 + (i/3)*3][curNumber]) return false;

                row[i][curNumber] = 1;// 之前都没出现过，现在出现了，就给它置为1，下次再遇见就能够直接返回false了。
                col[j][curNumber] = 1;
                box[j/3 + (i/3)*3][curNumber] = 1;
            }
        }
        return true;
    }
};
```



#### 10.矩阵置零

给定一个 `*m* x *n*` 的矩阵，如果一个元素为 **0** ，则将其所在行和列的所有元素都设为 **0** 。请使用 **[原地](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)** 算法**。**

  

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

```txt
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

```txt
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```


**提示：**

-   `m == matrix.length`
-   `n == matrix[0].length`
-   `1 <= m, n <= 200`
-   `-2^31 <= matrix[i][j] <= 2^31 - 1`

  

**进阶：**

-   一个直观的解决方案是使用  `O(*m**n*)` 的额外空间，但这并不是一个好的解决方案。
-   一个简单的改进方案是使用 `O(*m* + *n*)` 的额外空间，但这仍然不是最好的解决方案。
-   你能想出一个仅使用常量空间的解决方案吗？

##### 方法一：

```c++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> row(m),col(n);
        for(int i = 0; i < m;i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(!matrix[i][j])
                {
                    row[i] = 1; //因为原来的row容器已经把所有值都初始化为0，所以不能用0做判断
                    col[j] = 1;
                }
            }
        }

        for(int i = 0; i < m; i++)
        {
            for(int j = 0; j < n; j++)
            {
                if(row[i] == 1 || col[j] == 1)
                {
                    matrix[i][j] = 0;
                }
            }
        }
    }
};
```

> 标记数组的方法