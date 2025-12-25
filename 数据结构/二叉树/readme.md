
## 二叉树的定义
1. 二叉树、n叉树、完全二叉树、满二叉树、堆、BST（二叉搜索树）、AVL树（平衡二叉树）等。

## 二叉树的构建

## 二叉树的递归遍历（前中后序遍历，递归写法）
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

### 104-二叉树的最大深度
1. 思路1：自上向下传递树的深度：通过参数传值的方式，根节点的深度是1，每次递归，depth+1。叶子节点的depth就是深度，对所有叶子节点的depth取最大值，就是res。
2. 思路2：自下向上传递树的深度：通过函数返回值，遇到了nullptr（叶子节点的左右子节点），返回0。根节点从左子返回值和右子返回值中取最大值，然后+1，作为自身的最大深度。 最终递归返回到根节点最大深度就是res。
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
### 111-二叉树的最小深度
1. 整体思路和最大思路差不多：从根节点递归到叶子节点，然后每一层depth+1。最后遍历所有叶子节点的最小值，就是res。
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

## 二叉树的非递归遍历（前中后序遍历，非递归写法）

## 二叉树的层序遍历

## 二叉树的路径

## 二叉树的还原
