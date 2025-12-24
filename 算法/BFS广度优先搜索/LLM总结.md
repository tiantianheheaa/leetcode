广度优先搜索（Breadth-First Search, BFS）是一种用于遍历或搜索树或图的经典算法，它从根节点（或起始节点）开始，逐层访问所有相邻节点，直到遍历完所有可达节点。以下是BFS的详细介绍，包括算法原理、代码模板、常见使用场景及示例。

---

## **1. 算法原理**
### **核心思想**
- **层级遍历**：从起始节点出发，先访问所有直接相邻的节点（第一层），再依次访问这些节点的相邻节点（第二层），以此类推。
- **队列辅助**：使用队列（FIFO）来管理待访问的节点，确保按层级顺序访问。
- **避免重复访问**：通过标记已访问的节点（如哈希表或数组）防止重复处理。

### **步骤**
1. **初始化**：将起始节点加入队列，并标记为已访问。
2. **循环处理**：
   - 从队列中取出队首节点（当前层节点）。
   - 访问该节点（如打印或处理数据）。
   - 将该节点的所有未访问过的相邻节点加入队列，并标记为已访问。
3. **终止条件**：队列为空时，遍历结束。

---

## **2. 代码模板**
### **伪代码**
```python
from collections import deque

def bfs(start):
    queue = deque([start])          # 初始化队列
    visited = set([start])          # 标记已访问节点（避免重复）
    
    while queue:
        node = queue.popleft()      # 取出队首节点
        print(node)                 # 处理当前节点（如打印）
        
        for neighbor in get_neighbors(node):  # 遍历所有相邻节点
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

### **Python实现（以图为例）**
```python
from collections import deque

def bfs(graph, start):
    queue = deque([start])
    visited = set([start])
    
    while queue:
        node = queue.popleft()
        print(node, end=" ")
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# 示例图（邻接表表示）
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

bfs(graph, 'A')  # 输出：A B C D E F
```

---

## **3. 常见使用场景**
### **1. 无权图的最短路径**
- **问题**：在无权图中找到从起点到目标节点的最短路径（步数最少）。
- **解法**：BFS天然按层级遍历，首次访问到目标节点时的路径即为最短路径。
- **示例**：迷宫寻路、社交网络中的“几度好友”问题。

### **2. 连通分量检测**
- **问题**：判断图中所有连通子图（如岛屿数量、网络分区）。
- **解法**：对每个未访问节点执行BFS，统计遍历次数。
- **示例**：图像处理中的连通区域标记。

### **3. 拓扑排序（有向无环图）**
- **问题**：对有向无环图（DAG）的节点进行线性排序，使得所有依赖关系（边）满足前驱在后继之前。
- **解法**：结合BFS和入度统计（Kahn算法）。
- **示例**：课程安排、任务调度。

### **4. 二叉树层级遍历**
- **问题**：按层级打印二叉树节点（如LeetCode 102题）。
- **解法**：BFS直接实现层级遍历。
- **示例**：
  ```python
  def levelOrder(root):
      if not root: return []
      queue = deque([root])
      result = []
      
      while queue:
          level = []
          for _ in range(len(queue)):
              node = queue.popleft()
              level.append(node.val)
              if node.left: queue.append(node.left)
              if node.right: queue.append(node.right)
          result.append(level)
      return result
  ```

### **5. 状态空间搜索**
- **问题**：在状态空间中寻找目标状态（如八数码问题、华容道）。
- **解法**：BFS探索所有可能的状态转移，直到找到目标。

---

## **4. 复杂度分析**
- **时间复杂度**：  
  - **图**：O(V + E)，其中V为节点数，E为边数（需访问所有节点和边）。  
  - **树**：O(N)，N为节点数（树无环，无需标记已访问）。  
- **空间复杂度**：  
  - 最坏情况下需存储所有节点，即O(V)（队列和访问标记）。

---

## **5. BFS vs DFS**
| **特性**       | **BFS**                          | **DFS**                          |
|----------------|----------------------------------|----------------------------------|
| **数据结构**   | 队列（FIFO）                     | 栈（LIFO）或递归调用栈           |
| **遍历顺序**   | 层级优先（广度优先）             | 深度优先（一条路走到黑）         |
| **空间复杂度** | O(V)（队列可能存储大量节点）     | O(H)（H为树高，递归栈深度）      |
| **适用场景**   | 最短路径、连通性、层级遍历       | 拓扑排序、路径存在性、回溯问题   |

---

## **6. 示例扩展：带权图的最短路径**
BFS仅适用于无权图的最短路径。对于带权图，需使用**Dijkstra算法**（优先队列优化）或**Bellman-Ford算法**。

---

## **总结**
BFS是一种简单但强大的算法，适用于无权图的最短路径、连通性检测、层级遍历等场景。其核心是队列和层级遍历，通过标记已访问节点避免重复处理。理解BFS的原理后，可以灵活应用于树、图、状态空间等多种数据结构的问题中。
