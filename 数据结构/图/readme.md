## 基本定义
1. **点**和**边**是构成图的2个基本元素。图是不同的点通过边连接起来的。
2. 有向图和无向图
   - 无向图：所有的边是双向的，即a->b同时b->a。
   - 有向图：边是单向的，a->b。但是b不一定->a。
3. 度
   - 顶点连接的边的数量。
   - 有向图：入度：顶点连接的到达本节点的边的数量。出度：从该顶点出去的边的数量。
4. 权值
   - 点权：每个点可能有自己的权重。【此时每个点的存储就不能是一个顶点号了，需要用结构体存储顶点号+权值。】
   - 边权：每个边有自己的权重。
5. **联通分量**：无向图中的**极大联通子图**。
   - 顶点联通：2个顶点之间相互联通。
   - 联通图：图中任意2个顶点都联通。
   - 极大联通子图：一个图中能联通的节点都包含到图中了。
6. 强联通分量：有向图中的**极大强联通子图**。
    - 顶点强联通：有向图中的2个顶点，可以互相到达对方。
    - 强联通图：图中任意2个顶点都强联通。
    - 极大强联通子图：图中能联通的节点都包含到图中了。
11. 

## 图的构建
1. 邻接矩阵
   - 例如图有n个节点，则矩阵的大小是n*2。arr[i][j]表示节点i到节点j 是否联通 / 联通权值。
   - 优点：方便。可以快速获得任意2个点是否联通。
   - 缺点：适用于稠密表，如果用于稀疏表，会有很多数组的空间用不到。
2. 邻接表
   - 例如图有n个节点。构建一维数组arr[n]。 数组的元素arr[i]是一个链表或数组，用来存储所有和节点i相连的节点。
   - 优点：节省内存空间，不浪费。
   - 缺点：不能快速得到2个节点是否联通，需要遍历。

## 图的遍历
1. 有dfs和bfs两种遍历方式，都可以。dfs写起来代码少一些。

### 200-岛屿数量
1. 思路1：岛屿数量就是图的联通分量的数量。
   - 遍历图中的联通分量，每遍历一个联通分量，res+1。
   - 为了保证不重复遍历已遍历过的联通分量，所以需要对遍历过的节点做标记。
2. 易错点：dfs的**边界条件很多容易遗漏**。（1）先判断坐标是否在图内。（2）水是边界。（3）陆地不能被访问过。
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        if(m == 0) return 0;
        int n = grid[0].size();
        if(n == 0) return 0;
        // 不改变原二维数组的值，需要额外一个二维数组来表示节点是否访问过
        vector<vector<int>> flag(m, vector<int>(n, 0));
        int res = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                // 陆地，并且该节点没有被访问过，则是一个dfs的起点
                if(grid[i][j] == '1' && flag[i][j] == 0){
                    dfs(i, j, m, n, grid, flag);
                    res++;
                }
            }
        }
        return res;
    }
    void dfs(int i, int j, int m, int n, vector<vector<char>>& grid, vector<vector<int>>& flag){
        // 递归边界
        // 递归边界有很多种情况：哪一样都不能少。（1）先判断坐标i和j是否在边界内，保证后面的grid[i][j]和flag[i][j]有效。（2）水是边界，grid[i][j]=='0' (3)已访问过的陆地 不能再次访问，也是边界。flag[i][j]==1
        if(i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0' || flag[i][j] == 1){
            return;
        }
        // 没有访问过的陆地节点，标记已访问过。
        flag[i][j] = 1;
        dfs(i+1, j, m, n, grid, flag);
        dfs(i-1, j, m, n, grid, flag);
        dfs(i, j+1, m, n, grid, flag);
        dfs(i, j-1, m, n, grid, flag);
    }
};
// dfs
// 联通分量的个数
```
3. 思路2：bfs。 整体思路和dfs是一样的，就是遍历grid中每个节点，如果是陆地且没有被访问，则当做一个联通分量的起点，将所在的联通分量遍历完&标记已访问。 最终联通分量的个数就是岛屿的数量。
4. bfs的思路：定义队列，起始节点入队。 while循环（队列非空）：取出队首节点并访问，下一层周围4个节点入队。
5. 易错点：**节点入队即标记已访问**。 出队时标记已访问，就晚了，从而**会导致节点重复入队**。
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        if(m == 0) return 0;
        int n = grid[0].size();
        if(n == 0) return 0;
        // flag数组，0表示未访问，1表示访问
        vector<vector<int>> flag(m, vector<int>(n, 0));
        int res = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == '1' && flag[i][j] == 0){
                    bfs(i, j, m, n, grid, flag);
                    res++;
                }
            }
        }
        return res;
    }
    bool valid_node(int i, int j, int m, int n, vector<vector<char>>& grid, vector<vector<int>>& flag){
        if(i < 0 || i >= m || j < 0 || j >= n || grid[i][j] == '0' || flag[i][j] == 1){
            return false;
        }else{
            return true;
        }
    }
    void bfs(int i, int j, int m, int n, vector<vector<char>>& grid, vector<vector<int>>& flag){
        pair<int,int> p;
        p = make_pair(i, j);
        queue<pair<int, int>> que;
        que.push(p);
        // 【易错！！！】入队即标记，否则会重复入队。
        // 如果出队才标记已访问，可能被周围4个节点作为中心 bfs的时候，再次入队。
        flag[i][j] = 1;
        while(!que.empty()){
            // 【1】取出队首节点，并访问
            p = que.front();
            que.pop();
            int cur_i = p.first;
            int cur_j = p.second;
            // 标记节点已访问【出队才标记已访问，已经晚了！！！】
            // flag[cur_i][cur_j] = 1;

            // 【2】周围4个节点，如果有效，则加入队列
            int next_i = cur_i + 1;
            int next_j = cur_j;
            if(valid_node(next_i, next_j, m, n, grid, flag)){
                que.push(make_pair(next_i, next_j));
                flag[next_i][next_j] = 1;
            }
            next_i = cur_i - 1;
            next_j = cur_j;
            if(valid_node(next_i, next_j, m, n, grid, flag)){
                que.push(make_pair(next_i, next_j));
                flag[next_i][next_j] = 1;
            }
            next_i = cur_i;
            next_j = cur_j + 1;
            if(valid_node(next_i, next_j, m, n, grid, flag)){
                que.push(make_pair(next_i, next_j));
                flag[next_i][next_j] = 1;
            }
            next_i = cur_i;
            next_j = cur_j - 1;
            if(valid_node(next_i, next_j, m, n, grid, flag)){
                que.push(make_pair(next_i, next_j));
                flag[next_i][next_j] = 1;
            }
        }
    }
};
// 广度优先搜索
// 队列。 先入队，然后是下一层入队，也就是周围4个。
// 已经入队的，标记一下。

// 整体思路和dfs是一样的。
// 只是遍历每个联通分量块的方式不同而已。
```


## 
