<https://leetcode-cn.com/problems/binary-tree-inorder-traversal/>

给定一个二叉树，返回它的中序遍历。

**进阶:**递归算法很简单，你可以通过迭代算法完成吗？

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
```



思路1：递归，简单明了。 时间O(n)，空间O(n)=>取决于树的结构，最坏情况存储整棵树

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root) {
            inorderTraversal(root->left);
            vct.push_back(root->val);
            inorderTraversal(root->right);
        }
        return vct;
    }
private:
    vector<int> vct;
};
```

思路2：用栈模拟递归。 时间O(n)，空间O(n)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> vct;
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while (!stk.empty() || cur) {
            while (cur) {
                stk.push(cur);
                cur = cur->left;
            }
            vct.push_back(stk.top()->val);
            cur = stk.top()->right;
            stk.pop();
        }
        return vct;
    }
};
```

思路3：莫里斯遍历。 时间O(n)，空间O(1)
我们知道，二叉树遍历时左子树最后遍历的节点一定是一个叶子节点，其左右孩子都是NULL，将其右孩子指向当前根节点存起来，这样就可以回到根节点了。
步骤如下：

1. cur->left不为NULL，找到cur->left这颗子树最右边的节点记做last
     - last->right为NULL，那么将last->right=cur，更新cur=cur->left
     - last->right不为NULL，说明之前已经访问过，第二次来到这里(此时已经遍历完cur的left，然后遍历cur的right)更新cur=cur->right
2. cur->left为NULL，更新cur=cur->right
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> vct;
        TreeNode* cur = root;
        while (cur) {
            if (cur->left) {
                TreeNode* pre = cur->left;
                while (pre->right && pre->right != cur) {
                    pre = pre->right;
                }
                if (pre->right) {
                    vct.push_back(cur->val);
                    pre->right = nullptr;
                    cur = cur->right;
                } else {
                    pre->right = cur;
                    cur = cur->left;
                }
            } else {
                vct.push_back(cur->val);
                cur = cur->right;
            }
        }
        return vct;
    }
};
```
