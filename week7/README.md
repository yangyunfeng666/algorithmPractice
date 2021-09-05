 # 第7周作业

### 第一题 684. 冗余连接

```
树可以看成是一个连通且 无环 的 无向 图。

给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图。添加的边的两个顶点包含在 1 到 n 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 n 的二维数组 edges ，edges[i] = [ai, bi] 表示图中在 ai 和 bi 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 n 个节点的树。如果有多个答案，则返回数组 edges 中最后出现的边。
```
思路： 这里使用并查集，数组都是开始边到结束边，我们把结束边的父指向开始边。当出现相同并查集的时候，那么就找到了这样的环边。


CODE
```
class Solution {
public:

    //并查集
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        //查找最大的边长
        int n = edges.size();
        fa = vector(n + 1,0);
        for (int i = 0; i < n; i++) {
            fa[i] = i;
        }
        for (auto &b : edges) {
            int start = b[0];
            int end = b[1];
            if (find(start) == find(end)) {
                return {start,end};
            } else {
                fa[find(start)] = find(end);
            }
        }
        return {};
    }

    int find(int x) {
        if (x == fa[x]) return  x;
        return fa[x] = find(fa[x]);
    }

private:
 vector<int> fa;
}
```

### 第二题 200 岛屿数量
```
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1：

输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
示例 2：

输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3

```
思路：使用并查集，把相邻1的岛屿的父都指向第一1的岛屿，而把海的父指向一个外围的值，然后统计并查集里面父等于自己的就是岛屿数量
COED
```
class Solution {
public:  
     int numIslands(vector<vector<char>>& grid) {
        this->n = grid.size(); //确定数组x,y值
        this->m = grid[0].size(); 
        fa = vector<int>(n * m + 1);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                    fa[num(i,j)] = num(i,j);
            }
        }
        visited =vector<vector<bool>>(n,vector<bool>(m,false));

        fa[n * m] = n * m;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == '0') {
                    fa[num(i,j)] = n * m; //定义外围的父
                    continue;
                }
                for (int k = 0; k < 4; k++) {
                    int nx = i + dx[k];
                    int ny = j + dy[k];
                    if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
                    if (grid[nx][ny] == '1' ) { //如果是岛屿，指向第一个岛屿的父
                         fa[find(num(nx,ny))] = find(num(i,j));
                    }
                }
            }
         }
         int ans = 0;
         for (int i = 0; i < n * m ; i ++) {
             if (fa[i] == i) { ans++;}
         }
         return ans;
    }
private:
   int n,m;
   //定义方向数组
   const int dx[4] = {0,1,0,-1};
   const int dy[4] = {1,0,-1,0};
   vector<int> fa;

    int find (int x) {
        if (x == fa[x]) return x;
        return fa[x] = find(fa[x]);
    }

    int num(int i,int j) {
        return i * m + j;
    }

}
```

