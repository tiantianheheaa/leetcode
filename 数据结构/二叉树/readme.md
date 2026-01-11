
# 基础知识
## 二叉树的定义
1. 二叉树：每个节点最多有2个子节点。
2. n叉树：每个节点最多有n个子节点。
3. 完全二叉树：二叉树，只有最后一行没填满，其他行都填满了。并且最后一行左边是填满的，没填满的节点在右边。
4. 满二叉树：二叉树，除了叶子节点外，每个节点都填满了（每个节点都有2个子节点）。
   - 满二叉树的深度为k，有2^k - 1个节点。
5. 堆：数值是有序的。根节点的值 >= 或 <= 子节点的值。
   - 大顶堆：根节点的值 >= 左子、右子的值。
   - 小顶堆：根节点的值 <= 左子、右子的值。
   - 堆，可以自己实现。也可以直接用优先队列priority_queue。
6. BST（二叉搜索树）：数值是**有序**的，左子树的值 <= 根节点的值 <= 右子树的值。
7. AVL树（平衡二叉树）：左右子树的高度差不超过1，并且左右子树都是二叉搜索树。
  - C++中的set、map、multiset、multimap的底层实现都是AVL树，所以增删改差的时间复杂度是O(log n)。 unordered_map、unordered_set的底层实现是哈希表。

8. 树的高度和树的深度
    - 树的深度：自上向下。根节点的深度是0（起点），根节点到某个节点的节点数-1（或边数、层数差）。 适合**先序遍历**，通过函数参数，向下传递信息。
    - 树的高度：自下向上。叶子节点的高度是0（起点），叶子节点到根节点的节点数-1（或边数、层数差）。 适合**后序遍历**，通过函数返回值，向上传递信息。
    - 结论：树的高度和树的深度本质是一样的，都是求层数差。但是同一个节点的深度和高度是**两个不同的值**，求解方式也不同，一个是先序遍历，一个是后序遍历。

## 二叉树的存储
1. 我们习惯的是链式存储，每个节点有left指针变量指向左子的地址，right指针变量指向右子的地址。
2. 也可以用数组来存储。如果父节点的下标是i，则左子的下标是2i+1，右子的下标是2i+2。

## 二叉树的构建

## 二叉树的遍历
1. 有深度优先遍历、广度优先遍历2种。
2. 深度优先遍历用递归函数来实现，栈是递归算法/递归函数的一种实现结构。
3. 广度优先搜索用队列来实现。 因为广度优先搜索的一层一层向外扩展的遍历方式 和 队列的元素先入先出的特点 契合。

# 典型题目
1. 二叉树的题目**基本都是考察二叉树的遍历**，因为要求解的问题需要遍历树的每个节点才能求出。 用深度优先遍历和广度优先遍历 2种方式都可以。
2. 二叉树的**属性**，基本用**dfs**，因为写起来方便。 先序、中序、后序中比较常用的是**后序（根节点可以利用左子和右子的返回结果）、先序（根节点向左子和右子传递信息）**。
   - 属性其实就是节点的属性，例如节点的深度、节点的高度、节点的累积和等。节点的属性需要遍历二叉树求得。
4. 二叉搜索树，一般用**中序**比较多，因为可用其有序性。
  - 二叉搜索树单独拎出来，是因为二叉搜索树的**有序性**导致很多**解法和普通二叉树有区别**。
6. 二叉树的构造，不论是普通二叉树还是二叉搜索树，都是先**构造中节点**。

## 二叉树的深度优先遍历（前中后序遍历，递归写法）
1. 二叉树的题目很多都是考察二叉树的遍历，因为二叉树可以很方便的通过递归遍历所有节点。
2. 以二叉树的前中后序的递归遍历为模版，很多题目都是在dfs遍历的基础上增加一些信息传递。
3. dfs函数的信息传递有**函数参数、函数返回值**两种工具。 函数参数又有传值、传引用两种方式。
   - **dfs函数递归的本质就是：先自上向下递归到边界，然后自下向上返回结果。**
   - 函数参数：自上向下传递信息。例如depth参数，会传递到子函数中。
     - 传值：子函数中只能得到一个值。
     - 传引用：子函数能得到一个变量（既可以得到一个值，又可以修改其对应的变量的值）。
   - 函数返回值：自下向上传递信息。例如res，父函数会得到子函数的返回值。
### 144-二叉树的先序遍历
1. 先序遍历的顺序是：先操作根节点（操作就是把节点的值放入数组中），然后左子，然后右子。
2. 其实二叉树节点的**访问顺序 和 操作顺序是不同的**。 **访问顺序一定是先根节点，然后子节点**。因为是递归访问的。  但是**操作顺序可能因题目不同，例如中序和后序遍历，对根节点的操作（把节点的值放入数组）就是在操作左子后的**。
3. 本题最终结果是一个数组。所以有传值、传引用两种方式。 由于传引用会少很多信息的拷贝（只拷贝一个地址，而不是整个数组），所以传引用对内存的占用量是更小的。
4. 解法1：函数参数，传引用。 因为dfs遍历的顺序是自上向下，先序遍历的顺序也是自上向下，所以dfs传值是非常自然的。先放入根节点的val到res数组中，然后递归左子，递归右子。
5. 解法2：函数参数，传值。  自上向下，到了叶子节点 arr已经添加了很多节点。但是无法将这个arr返回，有2种方式：（1）将arr赋值给另一个引用类型的函数参数 res。（2）将arr赋值给函数返回值。 但是这2种方式都无法确定res的更新时间。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        return res;
    }
    // 传引用的目的是不用拷贝整个数组（只拷贝一个地址），且可以修改原值。
    void dfs(TreeNode* root, vector<int>& res){
        if(root == nullptr){
            return;
        }
        res.push_back(root->val);
        dfs(root->left, res);
        dfs(root->right, res);
    }
};
```

### 104-二叉树的最大深度（先序）
1. 思路1：**自上向下传递树的深度【先序】**：通过参数传值的方式，根节点的深度是1，每次递归，depth+1。叶子节点的depth就是深度，对所有叶子节点的depth取最大值，就是res。
2. 思路2：**自下向上传递树的深度【后序】**：通过函数返回值，遇到了nullptr（叶子节点的左右子节点），返回0。根节点从左子返回值和右子返回值中取最大值，然后+1，作为自身的最大深度。 最终递归返回到根节点最大深度就是res。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {

        // 解法1
        // return dfs(root);  // 一行代码，函数返回值既是传递的信息，又是最终结果。

        // 解法2
        int res = 0;
        // 既用到了传引用，又用到了传值
        // 如果不用函数返回值作为结果，用函数参数作为结果，必须用引用，才能在递归函数中修改res的值。
        // 传值：自上向下的传递，depth，传值就可以。因为只有递归+1的作用。递归函数中不会修改depth的值。
        dfs2(root, res, 1);  
        return res;
    }
    // 【解法1】传返回值，自下向上
    int dfs(TreeNode* root){
        // 递归边界
        if(root == nullptr){
            return 0;
        }
        // 递归左子和右子，并且加上当前层+1
        return max(dfs(root->left), dfs(root->right)) + 1;
    }
    // 【解法2】传参，自上向下
    void dfs2(TreeNode* root, int &res, int depth){
        if(root == nullptr){
            // 因为nullptr的depth+1已经在上一个函数中操作过了，所以对于传入的参数depth-1。
            res = max(res, depth-1);
        }else{
            dfs2(root->left, res, depth+1);
            dfs2(root->right, res, depth+1);
        }
    }
};
// 最大深度，自下向上传递比较好。  
// 自上向下也可以，最后res更新 所有子节点的最大值。
```
### 111-二叉树的最小深度（先序）
1. 【**深度：用先序更好理解**，从根节点向下传递信息】整体思路和最大深度差不多：从根节点递归到叶子节点，然后每一层depth+1。最后遍历所有叶子节点的最小值，就是res。
2. 细节：（1）叶子节点不是叶子节点的左子（nullptr）和右子(nullptr)，叶子节点是root->left==nullptr && root->right == nullptr。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr){
            return 0;
        }
        int res = INT_MAX;
        dfs(root, res, 1);
        return res;
    }
    void dfs (TreeNode* root, int & res, int depth){
        // 说的是根节点到叶子节点，不是到空节点。 所以递归边界不能是root==nullptr
        if(root->left == nullptr && root->right == nullptr){
            res = min(res, depth);
        }else{
            // 递归边界的判断条件，root->left 和root->right决定了 root不能为nullptr。所以递归左子和右子前，加了条件判断。
            if(root->left != nullptr){
                dfs(root->left, res, depth+1);
            }
            if(root->right != nullptr){
                dfs(root->right, res, depth+1);
            }   
        }
    }
};
// 最大深度，自然的想法是自上向下传递信息，到所有叶子节点 比较最大的深度。
// 最小深度，可以用同样的方式吗？

// 同样的方式计算，可能会有错误的情况。
```

### 110-平衡二叉树（高度，后序）
1. 思路：**树的深度用先序遍历**，因为定义是从根节点到叶子节点的层数。 **树的高度用后序遍历**，因为定义是从叶子节点到根节点的层数（max(左子高度，右子高度)+1）。
2. 后序遍历：通过函数返回值传递树的高度。 所以结果res只能通过传引用的方式，每个节点都判断左子右子的高度差，都可以更新res的值。

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        bool res = true;
        dfs(root, res);
        return res;
    }
    int dfs(TreeNode* root, bool& res){
        if(root == nullptr){
            return 0;
        }
        int left = dfs(root->left, res);
        int right = dfs(root->right, res);
        if(abs((left - right)) > 1){
            res = false;
        }
        return max(left, right) + 1;
    }
};
// 递归的过程求解 左右子树的高度差
// 树的高度  和 树的深度
// 先序比较容易理解：因为遍历的顺序是自上向下，如果操作节点的顺序和遍历节点的顺序是一致的，更容易理解。  不过dfs递归的过程是自上向下遍历+自下向上回溯，有这2个过程，所以先序或后序访问根节点都可以。
// 深度是一样的，但是高度不同。
// 深度可以用自上向下，用先序。 高度是自下向上，用后序。
```

### 257-二叉树的所有路径（先序）
1. 思路：根节点到叶子节点的路径用字符串表示，肯定用先序。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        // 边界情况：根节点的处理
        if(root == nullptr){
            return res;
        }
        if(root->left == nullptr && root->right == nullptr){
            res.push_back(to_string(root->val));
            return res;
        }
        // 常规逻辑
        string s = to_string(root->val);  // 根节点不需要"->"，所以单独处理
        if(root->left != nullptr){
            dfs(root->left, s, res);
        }
        if(root->right != nullptr){
            dfs(root->right, s, res);
        }
        return res;
    }
    void dfs(TreeNode* root, string s, vector<string>& res){
        // 叶子节点
        if(root->left == nullptr && root->right == nullptr){
            s = s + "->" + to_string(root->val);
            res.push_back(s);
            return;
        }
        // 先序遍历
        s = s + "->" + to_string(root->val);
        // 递归左子和右子
        if(root->left != nullptr){
            dfs(root->left, s, res);
        }
        if(root->right != nullptr){
            dfs(root->right, s, res);
        }
    }
};
// 每个节点存储一个string，然后叶子节点的string都push到res数组中。
// 先序遍历
```

### 112-路径总和（先序）
1. 思路：和最大深度、最小深度是一样的，都是在二叉树的遍历过程中传递信息。
   - 自上向下传递信息比较方便，因为每个叶子节点代表一条路径，最后遍历所有叶子节点对应的路径和，得到res。
   - **理解dfs的执行过程**：各个递归函数不是并发执行的，而是按照代码写的顺序，例如先递归左子，然后递归右子。而且是自上而下（递归的本质）。所以各个叶子节点更新同一个res引用变量的值是ok的。
2. 易错点：[1,2]，target=1，返回结果是false，不是true【因为1自身不算，1-2(左子)才算一条路径】。 因为题目规定路径必须是根节点-**叶子节点**，二叉树的递归边界经常是nullptr，应该修改为叶子节点。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        // 边界情况
        if(root == nullptr){
            return false;
        }
        // 正常逻辑
        bool res = false;
        dfs(root, 0, targetSum, res);
        return res;
    }
    void dfs(TreeNode* root, int pathSum, int targetSum, bool &res){
        // 必须是到叶子节点
        if(root->left == nullptr && root->right == nullptr){
            pathSum = pathSum + root->val;
            if(pathSum == targetSum){
                // dfs会有多条路径，但是有顺序执行的(先左子后右子，先上后下[递归])，不是并发执行的。
                // res只会被赋值true，不会被赋值false。
                res = true;
            }
        }else{
            // pathSum+root->val 就是访问父节点，就是先序遍历。
            if(root->left != nullptr){
                dfs(root->left, pathSum+root->val, targetSum, res);
            }
            if(root->right != nullptr){
                dfs(root->right, pathSum+root->val, targetSum, res);
            }
        }
    }
};
// 也是树的遍历。 根节点 到 叶子节点的路径。
// 这个题目和树的深度的题目是类似的，只是传递的信息不同而已
// 自上向下传递信息， 每个叶子节点 就是路径和。 遍历所有叶子节点的路径和，就得到了res。
```

### 226-翻转二叉树（先中后序都可以）
1. 思路：很有意思的一个题目。画图可知，先序和后序都可以，也就是先翻转父节点的左子和右子，和先递归都可以。
   - 中序也可以：递归左子，翻转父节点，递归左子【父节点翻转后，此时的左子是翻转前未递归的右子】。
2. **先中后序都可以，中序需要2次递归左子**。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(root == nullptr){
            return root;
        }
        dfs(root);
        return root;
    }
    void dfs(TreeNode* root){
        if(root == nullptr){
            return;
        }
        // 中序遍历也可以
        dfs(root->left);
        TreeNode* tmp = root->left;
        root->left = root->right;
        root->right = tmp;
        dfs(root->left);  // 2次dfs左子
    }
};
// dfs遍历就可以。 先序 和 后序 都可以。 中序可以？
```

## 二叉树的深度优先遍历（前中后序遍历，非递归写法）

## 二叉树的层序遍历/广度优先遍历
### 102-二叉树的层序遍历
1. 思路：广度优先遍历/层序遍历 和 队列的先进先出的结果是契合的，所以用队列来模型过程。
   - 只不过每一层的节点数量，需要int n = que.size(); 来分隔每层的节点。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> que;
        if(root == nullptr){
            return res;
        }
        // 队列初始化，下面的逻辑决定了队列初始不能为空。
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> tmp_res;
            // 遍历当前队列（也就是当前层）中的每个元素，访问每个元素，并将其左子和右子push到队列中。
            for(int i = 0; i < size; i++){
                TreeNode* tmp_node = que.front();
                que.pop();
                tmp_res.push_back(tmp_node->val);
                if(tmp_node->left != nullptr) que.push(tmp_node->left);
                if(tmp_node->right != nullptr) que.push(tmp_node->right);
            }
            res.push_back(tmp_res);  
        }
        return res;
    }
};
```

## 二叉树的路径
### 112-路径总和1（递归）
1. 同上。先序遍历，已总结。
### 113-路径总和2（回溯）
### 437-路径总和3（前缀和）

## 二叉树的公共祖先（后序）
### 236-二叉树的最近公共祖先
1. **思路是最重要的**，有了思路，代码只是实现。 不用死记硬背代码模版。
2. 思路：
   - 需要传递信息：“祖先”，所以需要**遍历**。用**dfs**遍历。
   - **先中后序**遍历中，选一个。 **普通的二叉树**用**先序后序**比较多，**二叉搜索树**用**中序**遍历比较多。
   - 所以这个题目用先序或后序。 **根节点向子节点传递信息**，用先序。 **子节点向根节点传递信息**，用后序。
   - **“祖先” 需要自下向上传递信息**，所以用后序。
3. 后序遍历
   - 递归边界：（1）root==nullptr，返回nullptr。（2）root==p或q，则直接返回root。不用再向下dfs了。 因为公共祖先不可能是p或q的子节点。
   - dfs获得左子和右子的返回结果。
   - 后续遍历处理根节点：
      - 左子和右子返回值都不为空，此时返回root。【左子和右子一定是一个p，另一个q。因为在遇到公共祖先前不可能有其他信息，只有null、p、q这三种信息可以向上传递。在遇到公共祖先后，公共祖先会向上传递其本身root，公共祖先的兄弟节点一定没遇到p或q，一定返回null】。
      - 左子为空，右子不为空，返回右子。【左子为空，说明没信息，左子及其子节点都没遇到p或q】
      - 右子为空，左子不为空，返回左子。
      - 左子和右子都为空，返回空。【说明左子和右子都没遇到p或q】

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return dfs(root, p, q);
    }
    TreeNode* dfs(TreeNode* root, TreeNode* p, TreeNode* q){
        // 递归边界（1）
        if(root == nullptr){
            return root;
        }
        // 递归边界（2）：因为最近的公共祖先，一定不会是p和q的子节点，所以遇到p或q就返回。
        if(root == p || root == q){
            return root;
        }

        // 后序遍历：获得左子和右子的返回值
        TreeNode* left = dfs(root->left, p, q);
        TreeNode* right = dfs(root->right, p, q);
        
        // （1）左子、右子都不为空
        // 只能是一个为p，另一个为q。
        if((left == p && right == q) || (left == q && right == p)){
            return root;
        }
        // （2）左子和右子，一个为空，另一个不为空
        if(left == nullptr){
            return right;
        }
        if(right == nullptr){
            return left;
        }
        // （3）左子和右子都为空，则返回空
        return nullptr;
    }
};
// 应该也是二叉树的遍历
// x的p和q的公共祖先，且x的深度尽可能大。
// 首先需要找到节点p和q。然后需要向上回溯。  这个过程就是二叉树的遍历。
// 二叉树的遍历有 先中后序，其中普通二叉树用先序和后序比较多。 二叉搜索树用中序比较多。
// 先序是 根节点 传递信息给 子节点。 后序是 子节点 传递信息 给根节点。
// 最近公共祖先 需要 传递信息 给 父节点，所以用后序遍历。 【其实也可以试试先序遍历是否能解】
// 当前root分几种情况：
// 返回值中一个是p，一个是q，则root是最近公共节点。 返回root。
// 返回值中一个p或q，另一个是null，则返回p或q。
```
### 235-二叉搜索树的最近公共祖先


## 二叉树的构建（递归）
1. 二叉树的构造还是在考察**递归**，因为二叉树就是递归定义的。
2. **结论**（来自《算法笔记》）：
   - 中序序列 和 先序、后序、层序序列中的任意一个，都可以唯一重建一颗二叉树。
   - 后三者中的任意2者或者全部3者，都无法唯一重构一颗二叉树。
   - 因为只有中序序列可以区分左子树和右子树。

### 105-从中序和前序遍历序列构造二叉树
1. **思路清晰，一遍就过**。
2. 思路：树的构造是**先序遍历**，即先构造根节点，然后递归构造左子树和右子树。
   - 根节点：先序序列的第1个元素。
   - 在中序序列中找到根节点，并将中序序列分为左子序列和右子序列。
   - 根据左子序列的长度（上一步求得），在先序序列中划分出左子序列和右子序列（第1个元素是根节点，然后往后数左子序列的长度，就得到了左子序列。后面剩下的是右子序列）。
3. 代码：先序序列和中序序列是2个数组，传值和传引用。
   - 传值：参数占用内存大。 传整个数组，或者左子数组、右子数组。
   - **传引用**：根据引用找到整个数组。然后根据下标在整个数组中划分，得到可用区间。
  
<img width="802" height="362" alt="image" src="https://github.com/user-attachments/assets/5a75b17a-1f24-4cf7-a98f-ad6e97d23e8b" />

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* dfs(vector<int>& preorder, vector<int>& inorder, int pre_left, int pre_right, int in_left, int in_right){
        if(pre_left > pre_right){
            return nullptr;
        }
        // 自上向下，先序遍历
        // 根节点：先序序列的第一个元素
        int val = preorder[pre_left];
        TreeNode* node = new TreeNode(val);

        // dfs函数需要4个坐标。 dfs左子和右子，2个函数一共需要8个坐标。
        int in_left_left = in_left;
        int in_left_right;
        int in_right_left;
        int in_right_right = in_right;

        int pre_left_left = pre_left + 1;
        int pre_left_right;
        int pre_right_left;
        int pre_right_right = pre_right;

        // 在中序序列中找根节点，然后将中序序列分为2半，就是左子序列和右子序列。
        for(int i = in_left; i <= in_right; i++){
            if(inorder[i] == val){
                in_left_right = i - 1;
                in_right_left = i + 1;
                break;
            }
        }
        // 根据左子序列的长度（中序序列中求得），在先序序列中划分出左子序列 和 右子序列。
        int left_len = in_left_right - in_left_left + 1;
        pre_left_right = pre_left_left + left_len - 1;
        pre_right_left = pre_left_right + 1;

        // 递归左子和右子。二叉树的构建就是递归构建。
        node->left = dfs(preorder, inorder, pre_left_left, pre_left_right, in_left_left, in_left_right);
        node->right = dfs(preorder, inorder, pre_right_left, pre_right_right, in_right_left, in_right_right);
        return node;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int pre_size = preorder.size();
        int in_size = inorder.size();
        if(pre_size == 0 || in_size == 0){
            return nullptr;
        }
        int pre_left = 0;
        int pre_right = pre_size - 1;
        int in_left = 0;
        int in_right = in_size - 1;
        return dfs(preorder, inorder, pre_left, pre_right, in_left, in_right);
    }
};
// 思路清晰，一遍就过。
```
### 106-从中序和后序遍历序列构造二叉树
1. 思路：同上。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* dfs(vector<int>& inorder, vector<int>& postorder, int in_left, int in_right, int post_left, int post_right){
        // 因为是闭区间，所以>才返回nullptr。当in_left==in_right时，这个元素是构造一个节点的。
        if(in_left > in_right){
            return nullptr;
        }
        // 先序遍历，构造二叉树。 先构造根节点，然后递归构建左子树和右子树。
        // 后序序列的最后一个元素，就是根节点的值。
        int val = postorder[post_right];
        TreeNode* node = new TreeNode(val);
        
        // 2个dfs，定义8个下标变量。
        int in_left_left = in_left;
        int in_left_right;
        int in_right_left;
        int in_right_right = in_right;

        int post_left_left = post_left;
        int post_left_right;
        int post_right_left;
        int post_right_right = post_right - 1;

        // 在中序序列中找根节点位置，然后划分出左子序列和右子序列
        for(int i = in_left; i <= in_right; i++){
            if(inorder[i] == val){
                in_left_right = i - 1;
                in_right_left = i + 1;
                break;
            }
        }
        int left_len = in_left_right - in_left_left + 1;
        post_left_right = post_left_left + left_len - 1;
        post_right_left = post_left_right + 1;
        
        // 递归构造左子树和右子树
        node->left = dfs(inorder, postorder, in_left_left, in_left_right, post_left_left, post_left_right);
        node->right = dfs(inorder, postorder, in_right_left, in_right_right, post_right_left, post_right_right);
        // 返回根节点
        return node;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int in_len = inorder.size();
        int post_len = postorder.size();
        int in_left = 0;
        int in_right = in_len - 1;
        int post_left = 0;
        int post_right = post_len - 1;
        return dfs(inorder, postorder, in_left, in_right, post_left, post_right);
    }
};
// 给定2个array，然后
// post数组的最后一个元素，去in数组中找，分为左右2个子树。【是一个dfs的过程】
// 后序：左子，右子，根节点。
// 中序：左子，根节点，右子。
```
### 654-最大二叉树
1. 思路：通过这个题目发现：**根据数组构建二叉树的模版是固定的，都是先序顺序构建：先构建根节点，然后递归构建左子树和右子树**。
   - 给定一个数组构建二叉树。和105、106题是一样的。都是在数组中找到根节点，将数组分为左子序列和右子序列，然后递归构建左子树和右子树。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // 因为传递的是数组的引用，所以获取的是整个数组。 所以需要left和right下标来划分出可用区间。
    TreeNode* dfs(vector<int>& nums, int left, int right){
        if(left > right){
            return nullptr;
        }
        // 先序顺序构建：先构建根节点，然后构建左子树和右子树。
        int max_val = nums[left];
        int max_val_index = left;
        for(int i = left; i <= right; i++){
            if(nums[i] > max_val){
                max_val = nums[i];
                max_val_index = i;
            }
        }
        // 根据最大值，构造根节点
        TreeNode* node = new TreeNode(max_val);

        // 递归构造左子树和右子树
        int left_left = left;
        int left_right = max_val_index - 1;
        int right_left = max_val_index + 1;
        int right_right = right;

        node->left = dfs(nums, left_left, left_right);
        node->right = dfs(nums, right_left, right_right);

        // 返回根节点
        return node;
    }
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        int size = nums.size();
        int left = 0;
        int right = size - 1;
        return dfs(nums, left, right);
    }
};
// 递归构造
// 找出最大值，最大值将nums分为左右2个子数组。
```

## 二叉搜索树的基本操作
### 108-将有序数组转化为二叉搜索树（先序）
1. 思路：树的构建的过程是一样的：先构建根节点，然后递归左子树和右子树。
2. 和上面的根据2个遍历数组，构造二叉树的过程是一样的。
   
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int n = nums.size();
        return dfs(nums, 0, n-1);
    }
    TreeNode* dfs(vector<int>& nums, int left, int right){
        if(left > right){
            return nullptr;
        }
        // 先序遍历，先构造根节点
        int mid = (left+ right)/ 2;
        TreeNode* root = new TreeNode(nums[mid]);
        // 然后递归左子树和右子树
        root->left = dfs(nums, left, mid-1);
        root->right = dfs(nums, mid+1, right);
        return root;
    }
};
// 平衡的二叉搜索树
// 递归构建吧， 数组的中间元素作为根节点，然后左边序列 和 右边序列 分别递归构建左子树和右子树
```

### 700-二叉搜索树的搜索
1. 思路：在二叉搜索树中找值为val的节点。就是简单的二叉数的先序遍历，根据root->val和要查找的val值，判断递归左子还是右子。

### 701-二叉搜索树的插入
1. 思路：在700题目的基础上，先搜索，根据root->val判断应该递归左子还是右子。**如果左子或右子 正好为空，就插入该节点**。
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        
        return dfs(root, val);
    }
    TreeNode* dfs(TreeNode* root, int val){
        // 递归边界的情况是：本来的二叉树是一颗空树。new TreeNode(val) 一个节点，返回。
        if(root == nullptr){
            root = new TreeNode(val);
            return root;
        }
        // 没有节点的值是val，所以root->val和val的关系只有大于 和 小于
        // 每次只递归一条路径，所以不会有多个dfs并行，只有一个。 所以最后val会被添加到一个位置，而不是多个位置。
        if(root->val > val){
            if(root->left == nullptr){
                // val被插入
                root->left = new TreeNode(val);
            }else{
                dfs(root->left, val);
            }
        }else{
            if(root->right == nullptr){
                // val被插入
                root->right = new TreeNode(val);
            }else{
                dfs(root->right, val);
            }
        }
        // 由于函数被设计为了TreeNode*的返回值，所以返回最开始的root根节点
        return root;
    }
};
// 遍历，根据有序性 找到要插入的位置。然后插入。
// 由于可以保证val不在原始的二叉搜索树中，所以查找val的结果 一定是Null，这个位置就是待插入的位置。
```
### 450-二叉搜索树的删除

## 二叉搜索树的遍历
1. 优先考虑中序，因为中序可以利用BST的有序性。
2. 由于BST本身是一个二叉树，所以在普通二叉树中常用的先序、后序，也可以用。
### 98-验证二叉搜索树（先中后序遍历）
1. 中序思路：一个简单的思路，dfs获得中序序列，判断中序序列是否有序。
2. **先序思路**：根节点的值是 左子树的上限，右子树的下限。 判断当前节点的值是否在[下限，上限]内，并且判断左子树是否有效 && 右子树是否有效。
3. 后序思路：根节点需要知道 左子树的最大值，右子树的最小值，从而判断根节点的值是否有效。  所以dfs函数需要返回2个值，一个是最大值，一个是最小值。供根节点选择。

```cpp
// 先序
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        return dfs(root, LONG_MIN, LONG_MAX);
    }
    bool dfs(TreeNode* root, long long lower, long long upper){
        if(root == nullptr){
            return true;
        }
        // 先序遍历，判断根节点是否有效
        if(root->val <= lower || root->val >= upper){
            return false;
        }
        // 同时还要判断左子树 和 右子树 是否有效
        return dfs(root->left, lower, root->val) && dfs(root->right, root->val, upper);
    }
};
// 先中后序遍历都可以
// 中序遍历就是判断 序列是否有序。
// 先序遍历更简单，就是把父节点的值，作为 左子的上界，和 右子的下界。 从而判断每个节点是否在[下界，上界]范围内，判断该节点是否有效。
// 后序遍历，需要得到左子的最大值、 右子的最小值，然后判断根节点是否有效。 dfs需要返回2个值，一个是最大值，一个是最小值。
```

```cpp
// 中序
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        vector<int> res;
        dfs(root, res);
        int n = res.size();
        // 验证中序遍历序列，是否从小到大的升序
        for(int i = 0; i < n-1; i++){
            if(res[i] >= res[i+1]){
                return false;
            }
        }
        return true;
    }
    void dfs(TreeNode* root, vector<int>& res){
        if(root == nullptr){
            return;
        }
        dfs(root->left, res);
        res.push_back(root->val);
        dfs(root->right, res);
    }

};
// 不是平衡树，只需要验证 左中右的值的大小 即可
// 就是二叉树的遍历，
// 后序遍历比较好， 因为可以知道 左子 和 右子的最小值， 就可以判断 BST了。

// 中序遍历，验证中序遍历序列是否有序 就可以。
```


## 二叉搜索树的构建
