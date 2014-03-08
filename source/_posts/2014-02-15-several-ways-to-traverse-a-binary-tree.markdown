---
layout: post
title: "几种遍历二叉树的方法"
date: 2014-02-15 19:05
comments: true
categories: [LeetCode,Algorithm]
---

遍历二叉树是学习算法与数据结构必学的内容，在leetcode网站上有好几道相关的题，本文也是从那里引出的：

1. [Binary Tree Preorder Traversal ](http://oj.leetcode.com/problems/binary-tree-preorder-traversal/)
2. [Binary Tree Inorder Traversal](http://oj.leetcode.com/problems/binary-tree-inorder-traversal/)
3. [Binary Tree Postorder Traversal](http://oj.leetcode.com/problems/binary-tree-postorder-traversal/)
4. [Binary Tree Level Order Traversal](http://oj.leetcode.com/problems/binary-tree-level-order-traversal/)
5. [Binary Tree Level Order Traversal II](http://oj.leetcode.com/problems/binary-tree-level-order-traversal-ii/)
6. [Binary Tree Zigzag Level Order Traversal](http://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)


以上就是leetcode上相关的题目。二叉树的先序、中序和后序遍历(对应上面的1，2，3)的递归实现非常简单。如下：


```cpp
void preorder(TreeNode *root)
{
	visit(root);
	preorder(root->left);
	preorder(root->right);
}

void inorder(TreeNode *root)
{
	inorder(root->left);
	visit(root);
	inorder(root->right);
}

void postorder(TreeNode *root)
{
	postorder(root->left);
	postorder(root->right);
	visit(root);
}
```

简单明了。但它们的迭代实现却要写上好几倍的代码。
下面根据leetcode的那几道题说说几种遍历方法的迭代实现。

### 先序遍历的迭代实现

从其递归实现中，可以清晰的找到迭代实现的思路：访问跟节点，再访问左子结点，访问完左子结点后再访问右子结点。因此，我们需要一个栈来保存右子结点。其实现比较简单，如下：


```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode *root) {
        stack<TreeNode*> tn_stack;
        if (root != NULL) {
            tn_stack.push(root);
        }
        
        vector<int> result;
        while (!tn_stack.empty()) {
            root = tn_stack.top();
            tn_stack.pop();
            result.push_back(root->val);
            if (root->right != NULL) {
                tn_stack.push(root->right);
            }
            if (root->left != NULL) {
                tn_stack.push(root->left);
            }
            
        }
        return result;
    }
};
```

### 中序遍历的迭代实现

中序遍历的迭代实现和先序的有些类似。1. 先将所有左子结点（包括最开始的根节点）入栈，2. 访问栈顶元素，3. 然后又将栈顶元素的右子结点入栈，重复1。

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode *root) {
        vector<int> result;
        stack<TreeNode*> tn_stack;
        while (root != NULL || !tn_stack.empty()) {
            while (root != NULL) {
                tn_stack.push(root);
                root = root->left;
            }
            if (!tn_stack.empty()) {
                root = tn_stack.top();
                tn_stack.pop();
                result.push_back(root->val);
                root = root->right;
            }
          
        }
        return result;
    }
};
```

### 后序遍历的迭代实现

后序遍历的迭代实现有点麻烦。这里需要为每个节点添加一个标记位，用于标识访问了左子结点还是右子结点，且每个节点需要入栈两次。我用个例子说明一下吧。

假设我们有如下的一颗二叉树：

```bash
# tree
     1 
    /  \ 
   /    \
  2      3
 / \    / \
4   5  6   7
```

其后序遍历的结果应为`4526731`。我们用一个栈保存添加了标志位的节点。依旧是所有左节点都需要入栈。先将`124`入栈：

```bash
[<1, L>, <2, L>, <4, L>]
```

这个时候左子结点已经为NULL了，我们需要读栈了，将栈顶pop出来，即`<4, L>`。我们发现其标志位为`L`，现在将它置为`R`，并重新入栈，同时拿到它的右子结点，`4`的右子结点为`NULL`，所以这个时候再次读栈顶，得`<4, R>`，由于标志位为`R`，所以访问该节点。读下一个栈顶元素`<2, L>`，置标志位为`R`，重新入栈，得到其右子结点`5`，将该节点入栈`<5, L>`，将节点`5`得所有左子结点入栈。

重复做这种置标志位，入栈，重置标志位，入栈，当标识位位`R`时访问得操作，最终后序遍历完整棵树。

例子的栈的变化如下

```bash
[<1, L>]
[<1, L>, <2, L>]
[<1, L>, <2, L>, <4, L>]
[<1, L>, <2, L>, <4, R>]
[<1, L>, <2, L>]
[<1, L>, <2, R>]
[<1, L>, <2, R>, <5, L>]
[<1, L>, <2, R>, <5, R>]
[<1, L>, <2, R>]
[<1, L>]
[<1, R>]
[<1, R>, <3, L>, <6, L>]
[<1, R>, <3, L>, <6, R>]
[<1, R>, <3, L>]
[<1, R>, <3, R>]
[<1, R>, <3, R>, <7, L>]
[<1, R>, <3, R>, <7, R>]
[<1, R>, <3, R>]
[<1, R>]
[]
```

实现如下：

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode *root) {
        pair<TreeNode *, char> node_with_flag;
        stack<pair<TreeNode *, char>> s;
        vector<int> result;
        TreeNode *node = root;
        if (root == NULL) {
            return result;
        }
        do {
            while (node != NULL) {
                node_with_flag.first = node;
                node_with_flag.second = 'L';
                s.push(node_with_flag);
                node = node->left;
            }
            bool is_continue = true;
            while (is_continue && !s.empty()) {
                node_with_flag = s.top();
                node = node_with_flag.first;
                s.pop();
                if (node_with_flag.second == 'L') {
                    node_with_flag.second = 'R';
                    s.push(node_with_flag);
                    is_continue = false;
                    node = node->right;
                } else {
                    result.push_back(node->val);
                }
            }
        } while (!s.empty());
        return result;
    }
};
```

### 按层遍历

开头列的几道leetcode的题，后面三道都属于按层遍历，解法一样，我用一个队列用于存放要访问的节点，和两个数（一个记录当前层需要访问多少个节点，一个记录下一层需要访问的节点数）来解决。

[Binary Tree Level Order Traversal](http://oj.leetcode.com/problems/binary-tree-level-order-traversal/)，我的解法如下：

```cpp
class Solution {
public:
    vector<vector<int> > levelOrder(TreeNode *root) {
        vector<vector<int>> result;
        queue<TreeNode *> q;
        int curr_level_node_count = 0;
        int next_level_node_count = 0;
        if (root != NULL) {
            curr_level_node_count = 1;
            q.push(root);
        }
        while (!q.empty()) {
            next_level_node_count = 0;
            vector<int> curr_level_value;
            while (curr_level_node_count--) {
                root = q.front();
                q.pop();
                if (root->left) {
                    q.push(root->left);
                    next_level_node_count++;
                }
                if (root->right) {
                    q.push(root->right);
                    next_level_node_count++;
                }
                curr_level_value.push_back(root->val);
            }
            result.push_back(curr_level_value);
            curr_level_node_count = next_level_node_count;
        }
        return result;
    }
};
```

[Binary Tree Level Order Traversal II](http://oj.leetcode.com/problems/binary-tree-level-order-traversal-ii/)，只需要将Binary Tree Level Order Traversal的结果在返回前将其反转就行了。

```cpp
// add before return statement
reverse(result.begin(), result.end());
```

[Binary Tree Zigzag Level Order Traversal](http://oj.leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)，则是将Binary Tree Level Order Traversal的结果在返回前隔层反转就行了。

```cpp
// add before return statement
for (int i = 0; i < result.size(); i++) {
    if (i % 2 == 1) {
        reverse(result[i].begin(), result[i].end());
    }
}
```