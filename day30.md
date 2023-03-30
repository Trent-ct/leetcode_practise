# DAY30

## 1、重新安排行程

#### [332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)

无序map里面嵌套了一个有序map，用于存放每个出发机场可以通过机票到达的目的机场，同时记录可以到达的次数

回溯遍历时，从起始机场“JFK”开始，每次都寻找上一个到达机场能够到达的机场，如果能找到一个符合终止条件的叶子节点

有返回值的写法

```c++
class Solution {
private:
    unordered_map<string,map<string,int>> targets;
    vector<string> result;
    bool backtracking(int ticketsNum,vector<string>& result){
        if(result.size() == ticketsNum + 1){
            return true;
        }
        for(pair<const string,int>& target : targets[result[result.size()-1]]){
            if(target.second>0){
                result.push_back(target.first);
                target.second --;
                if(backtracking(ticketsNum,result)) return true;
                result.pop_back();
                target.second ++;
            }
        }
        return false;

    }
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        targets.clear();
        result.clear();
        for(auto vec:tickets){
            targets[vec[0]][vec[1]]++;
        }
        result.push_back("JFK");
        backtracking(tickets.size(),result);
        return result;
    }
};
```

没有返回值的写法(由于一定存在一个解)

```c++
class Solution {
private:
    unordered_map<string,map<string,int>> targets;
    vector<string> result;
    void backtracking(int ticketsNum,vector<string>& result){
        for(pair<const string,int>& target : targets[result[result.size()-1]]){
            if(target.second>0){
                result.push_back(target.first);
                target.second --;
                backtracking(ticketsNum,result);
                if(result.size() == ticketsNum + 1) break;
                result.pop_back();
                target.second ++;
            }
        }
        return ;

    }
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        targets.clear();
        result.clear();
        for(auto vec:tickets){
            targets[vec[0]][vec[1]]++;
        }
        result.push_back("JFK");
        backtracking(tickets.size(),result);
        return result;
    }
};
```





## 2、N 皇后

#### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

一行一行往下递归遍历，每一行处理过程中，判断这个位置是否满足条件，若满足接着往下遍历

注意存储形式，chessboard看上去是一个一维数组，但由于其存储的是字符串，所以其实是二维数组

```c++
class Solution {
private:
    vector<vector<string>> result;
    //row行，col列
    void backtracking(int n,int row,vector<string>& chessboard){
        if(row == n){
            result.push_back(chessboard);
            return ;
        }
        for(int col=0;col < n;col++){
            if(isValid(row,col,n,chessboard)){
                chessboard[row][col]='Q';
                backtracking(n,row+1,chessboard);
                chessboard[row][col]='.';
            }
        }
        return ;
    }

    bool isValid(int row,int col,int n,vector<string>& chessboard){
        for(int i=0;i<row;i++){
            if(chessboard[i][col]=='Q') return false;
        }
        for(int i=row-1,j=col-1;i>=0&&j>=0;i--,j--){
            if(chessboard[i][j]=='Q') return false;
        }
        for(int i=row-1,j=col+1;i>=0&&j<n;i--,j++){
            if(chessboard[i][j]=='Q') return false;
        }
        return true;
    }
public:
    vector<vector<string>> solveNQueens(int n) {
        result.clear();
        vector<string> chessboard(n,string(n,'.'));
        backtracking(n,0,chessboard);
        return result;
    }
};
```



## 3、解数独

#### [37. 解数独](https://leetcode.cn/problems/sudoku-solver/)

注意return false 的位置，当9个数填进去都不能满足条件时，则表明当前的分支是错的，需要去寻找别的分支

```c++
class Solution {
private:
    bool backtracking(vector<vector<char>>& board){
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                if(board[i][j]=='.'){
                    for(char k='1';k<='9';k++){
                        if(isValid(i,j,k,board)){
                            board[i][j]=k;
                            if(backtracking(board)) return true;
                            board[i][j]='.';
                        }
                    }
                    return false;
                }
            }
        }
    return true;
    }
    bool isValid(int row,int col,char val,vector<vector<char>>& board){
        for(int i=0;i<9;i++){
            if(board[row][i]==val) return false;
        }
        for(int i=0;i<9;i++){
            if(board[i][col]==val) return false;
        }

        int startRow=(row/3)*3;
        int startCol=(col/3)*3;
        for(int i=startRow;i<startRow+3;i++){
            for(int j=startCol;j<startCol+3;j++){
                if(board[i][j]==val) return false;
            }
        }
        return true;
    }
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};
```

