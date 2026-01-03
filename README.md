##  刷题顺序
1. c++ / python 常用语法。
- 《算法笔记》是一个很好的复习资料，有C++ API语法、数据结构和算法的知识。
3. hot-100、代码随想录分专题、labuladong分专题。
4. 其他题目总结：例如二叉树的先序、中序、后序遍历的非递归写法，归并排序思想的应用。

## 待学习
- 二叉树的非递归遍历（代码细节&总结）。
- 编辑距离2个题目：再次理解递推公式中的3种操作、dp数组的官方定义版本。
- 打家劫舍3：树。刷了二叉树的题目，然后再做。
- 494-目标和：4种解法。dp(n+1,value+1)的二维dp,一维dp。 dp(n,value+1)的二维dp,一维dp。
- 162题目，二分，再好好研究，单边峰的定义。  mid和mid-1比较的写法？ 应该如何写？
- 1095题目，山峰，二分。

## C++ api
1. 线性结构：vector、stack、queue、string(字符串)的定义、初始化和增删改查。
- vector的初始化size大小。
- 常用的：vector的push_back，stack和queue的push和pop。
- 一维数组的定义和初始化: vector\<int\> arr(n, 0)； 定义size=n，初始化元素为0的数组。
- 二维数组的定义和初始化: vector<vector\<int\>> arr(n, vector<int>(m, 0)); 定义n行，每行是size=m，初始化为0的数组。
2. 结构体：链表、二叉树、图的定义、初始化和增删改查。
3. c++在线运行
- https://www.cainiaojc.com/tool/cpp/
- https://www.onlinegdb.com/online_c++_compiler



## 知识点
1. 递归是分治的思想，递归函数写起来很简洁，是一种不错的用于入门的算法，应用广泛（链表、二叉树、图的遍历，dfs写法）。
- 一种算法串联起来很多数据结构：链表、二叉树、图等
- 一种算法串联起来很多算法：二分查找、归并排序、快排等。
- 一种算法的多个升级算法：回溯、动态规划等。

<img width="1304" height="680" alt="image" src="https://github.com/user-attachments/assets/8238bfa6-8ef3-491b-baca-0510f5fe7503" />



- 二叉树的先中后序遍历的非递归写法，对应的就是深度优先搜索dfs的非递归实现。
- 递归是深度优先搜索dfs的一种实现方式，由于递归写起来简单，更常用。所以递归和dfs通常等价。
