
## 算法原理

## 代码模版
- 2种实现方式：递归实现，非递归实现。
- 递归实现代码简洁。 非递归实现对每一步的细节理解更透彻。

##  典型题目分类
###  树
- 二叉树的层序遍历。
```cpp
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
### 矩阵

###  图
