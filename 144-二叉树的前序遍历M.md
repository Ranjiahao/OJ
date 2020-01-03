<https://leetcode-cn.com/problems/binary-tree-preorder-traversal/>

给定一个二叉树，返回它的前序遍历。

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
    vector<int> preorderTraversal(TreeNode* root) {
        if (root) {
            vct.push_back(root->val);
            preorderTraversal(root->left);
            preorderTraversal(root->right);
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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> vct;
        stack<TreeNode*> stk;
        TreeNode* cur = root;
        while (!stk.empty() || cur) {
            while (cur) {
                vct.push_back(cur->val);
                stk.push(cur);
                cur = cur->left;
            }
            cur = stk.top()->right;
            stk.pop();
        }
        return vct;
    }
};
```

思路3：同样用栈解决，将root压入栈，每次循环先出栈，然后这个元素左右子树分别压栈，先右后左，出栈元素放入vector中即可。 时间O(n)，空间O(log n)=>最坏情况为完全二叉树

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> vct;
        if (root) {
            stack<TreeNode*> stk;
            stk.push(root);
            while (!stk.empty()) {
                TreeNode* tmp = stk.top();
                stk.pop();
                if (tmp->right)
                    stk.push(tmp->right);
                if (tmp->left)
                    stk.push(tmp->left);
                vct.push_back(tmp->val);
            }
        }
        return vct;
    }
};
```

思路4：莫里斯遍历。 时间O(n)，空间O(1)
我们知道，二叉树遍历时左子树最后遍历的节点一定是一个叶子节点，其左右孩子都是NULL，将其右孩子指向当前根节点存起来，这样就可以回到根节点了。
步骤如下：

1. cur->left不为NULL，找到cur->left这颗子树最右边的节点记做last
     - last->right为NULL，那么将last->right=cur，更新cur=cur->left
     - last->right不为NULL，说明之前已经访问过，第二次来到这里(此时已经遍历完cur的left，然后遍历cur的right)更新cur=cur->right
2. cur->left为NULL，更新cur=cur->right

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> vct;
        TreeNode* cur = root;
        while (cur) {
            if (cur->left) {
                TreeNode* pre = cur->left;
                while (pre->right && pre->right != cur) {
                    pre = pre->right;
                }
                if (pre->right) {
                    pre->right = nullptr;
                    cur = cur->right;
                } else {
                    vct.push_back(cur->val);
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
