
# 定义
  递归是一种函数，自己调用自己，求得最终问题的解。
# 模版
```cpp
void dfs(){
  // 递归出口 return
  if(...){
    return ;
  }
  // 递归调用
  dfs(...);
  dfs(...);
}
```

# 应用
递归函数的应用非常多，例如二叉树的遍历。首先访问当前节点，然后递归调用当前节点的左子树，递归调用当前节点的右子树。
```cpp
void dfs(TreeNode* root){
  if(root == nullptr){
      cout << root->val;
      return;
  }
  dfs(root->left);
  dfs(root->right);
}
```
