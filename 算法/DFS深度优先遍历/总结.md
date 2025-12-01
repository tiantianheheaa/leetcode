深度优先搜索（Depth-First Search, DFS）是一种经典的图和树遍历算法，它沿着一条路径尽可能深入地探索，直到无法继续为止，然后回溯到上一个节点，继续探索其他路径。DFS以其简洁的递归实现和高效的内存使用（相比BFS）在许多场景中广泛应用。以下是DFS的详细介绍，包括算法原理、代码模板、常见使用场景及示例。

---

## **1. 算法原理**
### **核心思想**
- **递归或栈实现**：DFS通过递归调用栈或显式使用栈（LIFO）来管理待访问的节点。
- **深入优先**：从起始节点出发，选择一个方向深入探索，直到无法继续（如遇到叶子节点或已访问节点），然后回溯到上一个节点，继续探索其他分支。
- **避免重复访问**：通过标记已访问的节点（如哈希表或数组）防止重复处理。

### **步骤**
1. **初始化**：将起始节点标记为已访问，并压入栈（或递归调用）。
2. **循环处理**：
   - **递归版**：访问当前节点，然后递归访问所有未访问的相邻节点。
   - **迭代版**：从栈中弹出栈顶节点，访问它，并将其未访问的相邻节点压入栈。
3. **终止条件**：栈为空（迭代版）或递归调用结束（递归版）。

---

## **2. 代码模板**
### **递归实现（通用模板）**
```python
def dfs_recursive(node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)  # 标记当前节点为已访问
    print(node)         # 处理当前节点（如打印）
    
    for neighbor in get_neighbors(node):  # 遍历所有相邻节点
        if neighbor not in visited:
            dfs_recursive(neighbor, visited)  # 递归访问未访问的节点
```

### **迭代实现（使用栈）**
```python
def dfs_iterative(start):
    stack = [start]      # 初始化栈
    visited = set([start])  # 标记已访问节点
    
    while stack:
        node = stack.pop()  # 弹出栈顶节点
        print(node)         # 处理当前节点
        
        # 注意：为了与递归顺序一致，需逆序压入相邻节点
        for neighbor in reversed(get_neighbors(node)):
            if neighbor not in visited:
                visited.add(neighbor)
                stack.append(neighbor)
```

### **Python示例（以图为例）**
```python
# 图的邻接表表示
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'F'],
    'F': ['C', 'E']
}

# 递归DFS
def dfs_recursive(node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    print(node, end=" ")
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(neighbor, visited)

# 迭代DFS
def dfs_iterative(start):
    stack = [start]
    visited = set([start])
    while stack:
        node = stack.pop()
        print(node, end=" ")
        for neighbor in reversed(graph[node]):
            if neighbor not in visited:
                visited.add(neighbor)
                stack.append(neighbor)

print("递归DFS:")
dfs_recursive('A')  # 输出：A B D E F C
print("\n迭代DFS:")
dfs_iterative('A')  # 输出：A B D E F C
```

---

## **3. 常见使用场景**
### **1. 图的连通性检测**
- **问题**：判断图中所有节点是否连通（如检测网络是否分区）。
- **解法**：从任意节点出发执行DFS，统计访问的节点数。若与总节点数相同，则图连通。
- **示例**：
  ```python
  def is_connected(graph):
      start = next(iter(graph))  # 取任意起点
      visited = set()
      dfs_recursive(start, visited)
      return len(visited) == len(graph)
  ```

### **2. 拓扑排序（有向无环图）**
- **问题**：对有向无环图（DAG）的节点进行线性排序，使得所有依赖关系（边）满足前驱在后继之前。
- **解法**：DFS遍历节点，在回溯时记录节点（逆序即为拓扑排序）。
- **示例**：
  ```python
  def topological_sort(graph):
      visited = set()
      order = []
      
      def dfs(node):
          visited.add(node)
          for neighbor in graph[node]:
              if neighbor not in visited:
                  dfs(neighbor)
          order.append(node)  # 回溯时记录节点
      
      for node in graph:
          if node not in visited:
              dfs(node)
      return order[::-1]  # 逆序输出
  ```

### **3. 路径查找与回溯问题**
- **问题**：在图中寻找从起点到目标节点的路径（如迷宫路径、数独求解）。
- **解法**：DFS递归尝试所有可能路径，找到目标后返回。
- **示例**：
  ```python
  def find_path(graph, start, end, path=None):
      if path is None:
          path = []
      path = path + [start]
      if start == end:
          return path
      for neighbor in graph[start]:
          if neighbor not in path:  # 避免循环
              new_path = find_path(graph, neighbor, end, path)
              if new_path:
                  return new_path
      return None
  ```

### **4. 二叉树的所有路径**
- **问题**：打印二叉树中所有从根节点到叶子节点的路径（如LeetCode 257题）。
- **解法**：DFS递归遍历左右子树，记录路径。
- **示例**：
  ```python
  class TreeNode:
      def __init__(self, val=0, left=None, right=None):
          self.val = val
          self.left = left
          self.right = right

  def binary_tree_paths(root):
      def dfs(node, path):
          if not node:
              return
          path += str(node.val)
          if not node.left and not node.right:  # 叶子节点
              paths.append(path)
              return
          dfs(node.left, path + "->")
          dfs(node.right, path + "->")
      
      paths = []
      dfs(root, "")
      return paths
  ```

### **5. 组合与排列问题**
- **问题**：生成所有可能的组合或排列（如全排列、子集问题）。
- **解法**：DFS递归枚举所有选择，通过回溯剪枝避免重复。
- **示例（全排列）**：
  ```python
  def permute(nums):
      def dfs(path, remaining):
          if not remaining:
              result.append(path)
              return
          for i in range(len(remaining)):
              dfs(path + [remaining[i]], remaining[:i] + remaining[i+1:])
      
      result = []
      dfs([], nums)
      return result
  ```

---

## **4. 复杂度分析**
- **时间复杂度**：  
  - **图**：O(V + E)，其中V为节点数，E为边数（需访问所有节点和边）。  
  - **树**：O(N)，N为节点数（树无环，无需标记已访问）。  
- **空间复杂度**：  
  - **递归版**：O(H)，H为树的高度（递归栈深度）。最坏情况下（如链表）为O(N)。  
  - **迭代版**：O(V)，栈可能存储所有节点（如完全图）。

---

## **5. DFS vs BFS**
| **特性**       | **DFS**                          | **BFS**                          |
|----------------|----------------------------------|----------------------------------|
| **数据结构**   | 栈（递归栈或显式栈）             | 队列（FIFO）                     |
| **遍历顺序**   | 深度优先（一条路走到黑）         | 广度优先（层级遍历）             |
| **空间复杂度** | O(H)（递归栈深度）               | O(V)（队列可能存储大量节点）     |
| **适用场景**   | 拓扑排序、路径回溯、组合问题     | 最短路径、连通性、层级遍历       |

---

## **6. 注意事项**
1. **避免循环**：在图中需标记已访问节点，否则会陷入无限循环（如A→B→A→...）。
2. **递归深度**：递归实现可能因栈溢出导致错误（如深度过大的树或图），此时需改用迭代版。
3. **路径记录**：在回溯问题中，需在递归返回时撤销选择（如移除路径中的当前节点）。

---

## **总结**
DFS是一种简洁高效的算法，适用于图的连通性检测、拓扑排序、路径回溯、组合问题等场景。其递归实现直观易懂，迭代实现（栈）则避免了递归的栈溢出风险。理解DFS的原理后，可以灵活应用于树、图、状态空间等多种数据结构的问题中。
