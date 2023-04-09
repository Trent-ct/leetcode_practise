# DAY39

## 1、不同路径

#### [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

用深搜求叶子节点的方法超时

```c++
class Solution {
private:
    int dfs(int down,int right,int m,int n){
        if(down>m || right >n) return 0;
        if(down == m&& right == n) return 1;
        return dfs(down+1,right,m,n)+dfs(down,right+1,m,n);
    }
public:
    int uniquePaths(int m, int n) {
        return dfs(1,1,m,n);
    }
};
```

动态规划方法，每一步都有上或左走到，所以当前步的路径数等于上和左路径数的和

初始化第一行和第一列

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++){
            dp[i][0]=1;
        }
        for(int j=0;j<n;j++){
            dp[0][j]=1;
        }
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```



## 2、不同路径 II

#### [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

与上一题不同的是加入了障碍物，所以初始化和动规过程中需要注意障碍物的判断

```c++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m=obstacleGrid.size();
        int n=obstacleGrid[0].size();
        if(obstacleGrid[0][0]==1 || obstacleGrid[m-1][n-1]==1) return 0;
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m && obstacleGrid[i][0]==0;i++) dp[i][0]=1;
        for(int j=0;j<n && obstacleGrid[0][j]==0;j++) dp[0][j]=1;
        for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                if(obstacleGrid[i][j] == 0){
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```



