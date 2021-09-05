# 第6周题

### 第一题 70. 爬楼梯

思路
```
递推 f(n) = f(n-1) + f(n-2) 第n步等于第n-1 和第n-2的总和
```
code
```
class Solution {
public:
    int climbStairs(int n) {
        //滚动数组求值
        vector<int> f(3,0);
        f[0] = 0;
        f[1] = 1;
        f[2] = 2;
        for (int i = 3; i <= n; i++) {
            f[i % 3] = f[(i - 1) % 3] + f[(i - 2) % 3 ];
        }
        return f[n % 3];
    }
};
```
这里使用滚动数组，开节约空间，都是相邻的3个数的操作。

### 120. 三角形最小路径和

```
给定一个三角形 triangle ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 

输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```
思路，状态方程是f[i][j] = min(f[i - 1][j],f[i - 1][j - 1]) + nums[i][j]
但是由于数组是三角形的需要处理左右两边的值的问题。

CODE
```
	class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> f(n,vector<int>(n,0));
        f[0][0] = triangle[0][0];
        for (int i = 1; i < n; i++) {
            f[i][0] = f[i - 1][0] + triangle[i][0]; //第一个数据
            for(int j = 1; j <= i; j++) {
                f[i][j] = min(f[i - 1][j],f[i - 1][j - 1]) + triangle[i][j];
            }
            f[i][i] = f[i - 1][i - 1] + triangle[i][i]; //补齐追右边的值
        }
        int ans = 1000000;
        for (int i = 0 ; i < n; i++) {
            ans = min(ans,f[n - 1][i]);
        }
        return ans;
}
};

```
### 第四题 673 最长递增子序列的个数
```
给定一个未排序的整数数组，找到最长递增子序列的个数。

示例 1:

输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
示例 2:

输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5

```

思路 两个数组，分别记录第i个数据的最长子序列长度dp 和个数count
当 j > i && nums[j] > nums[i]的时候，判断 dp[j] < dp[i] + 1 的时候，那么 dp[j] = dp[j] + 1 和 cout[j] = count[i]; 如果dp[j] == dp[i] + 1的时候，count[j] += count[i]; 
```
code
```
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        // f(i,j) => count[j]
        // f(i,j) = f[i - 1,j] + 1; 

        //dp[i] nums[i]到i的最大序列长度
        //count[i] nums[i] 最大序列个数
        vector<int> dp(nums.size(),1);
        vector<int> count(nums.size(),1);
        for(int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) { //当前值小于当前值的情况下满足条件
                    if (dp[i] < dp[j] + 1) { //当下标的最大长度小于前值的时候，需要递增最大子序列值，并且cout值也赋值过来
                        dp[i] = dp[j] + 1;
                        count[i] = count[j];
                    } else if (dp[i] == dp[j] + 1) { //当前值和前值的长度相等的时候，长度没有变化，那么统计值相加
                        count[i] += count[j];
                    }
                }
            }
        }
        int maxVal = *max_element(dp.begin(),dp.end()); //查找最大长度
        int ans = 0;
        for (int i = 0; i < nums.size(); i++){
            if (dp[i] == maxVal) {  //统计最大值的个数
                ans+= count[i];
            }
        }
        return ans;
    }
};
```

```

# 279. 完全平方数
```
给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

 

示例 1：

输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
示例 2：

输入：n = 13
输出：2
解释：13 = 4 + 9

```
思路

定义状态f[i] 是i个数据的最小平方根数，那么f[i] = min(f[i],f[i - j * j] + 1)
那么f[i]的最小平方根数等于f[i - j * j ]的平方根数+1,需要枚举j从1到i - j * j的情况决策。
code
```
class Solution {
public:
    int numSquares(int n) {
        // n << e4 => n <= 100;
        // min 个数越小，那么就值就约接近n的值。找到第一个小于等于n值的最大值的平方数。
        // f[i] = min(f[i],f(i - j * j) + 1);
        vector<int> f(n + 1,0);
        for (int i = 1; i <= n; i++) {
            f[i] = i;
            for (int j = 1 ; i - j * j >= 0; j++) {
                f[i] = min(f[i],f[i - j * j] + 1);
            }
        }
        return f[n];
    }
};

```


