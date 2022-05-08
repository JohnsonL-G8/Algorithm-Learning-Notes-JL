## 搜索(BFS+DFS)

### 03. 相关题目

#### [200.岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

【题目描述】：

​		给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。此外，你可以假设该网格的四条边均被水包围。

![image-20220313154534826](搜索(BFS+DFS).assets/image-20220313154534826.png)

【思路】：

**DFS:**

​		我们可以将二维网格看成一个无向图，竖直或水平相邻的 11 之间有边相连。

​		为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 11，则以其为起始节点开始进行深度优先搜索。在深度优先搜索的过程中，每个搜索到的 11 都会被重新标记为 00。

​		最终岛屿的数量就是我们进行深度优先搜索的次数。

~~~JAVA
class Solution { 
    public int numIslands(char[][] grid) {
        int m = grid.length, n = grid[0].length;
        int count = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    count += 1;
                }
            }
        }
        return count;
    }

    public void dfs(char[][] grid, int r, int c){
        int m = grid.length, n = grid[0].length;
        //边界处理(return)
        if(r < 0 || r >= m || c < 0 || c >= n || grid[r][c] == '0'){
            return;
        }
        grid[r][c] = '0';
        dfs(grid, r-1, c);
        dfs(grid, r+1, c);
        dfs(grid, r, c-1);
        dfs(grid, r, c+1);
    }
}
~~~



**BFS:**

​		同样地，我们也可以使用广度优先搜索代替深度优先搜索。

​		为了求出岛屿的数量，我们可以扫描整个二维网格。如果一个位置为 11，则将其加入队列，开始进行广度优先搜索。在广度优先搜索的过程中，每个搜索到的 11 都会被重新标记为 00。直到队列为空，搜索结束。

​		最终岛屿的数量就是我们进行广度优先搜索的次数。

~~~JAVA
class Solution {
    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;

        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                //找到一个新岛屿
                if (grid[r][c] == '1') {
                    ++num_islands;
                    grid[r][c] = '0';
                    //维护队列，寻找相邻岛屿(BFS)
                    Queue<Integer> neighbors = new LinkedList<>();
                    neighbors.add(r * nc + c);
                    while (!neighbors.isEmpty()) {
                        int id = neighbors.remove();
                        int row = id / nc;
                        int col = id % nc;
                        if (row - 1 >= 0 && grid[row-1][col] == '1') {
                            neighbors.add((row-1) * nc + col);
                            grid[row-1][col] = '0';
                        }
                        if (row + 1 < nr && grid[row+1][col] == '1') {
                            neighbors.add((row+1) * nc + col);
                            grid[row+1][col] = '0';
                        }
                        if (col - 1 >= 0 && grid[row][col-1] == '1') {
                            neighbors.add(row * nc + col-1);
                            grid[row][col-1] = '0';
                        }
                        if (col + 1 < nc && grid[row][col+1] == '1') {
                            neighbors.add(row * nc + col+1);
                            grid[row][col+1] = '0';
                        }
                    }
                }
            }
        }

        return num_islands;
    }
}
~~~



