# Lowest Common Ancestor of a Binary Tree

> Post-order propagation of "found" flags. LC 236 · 🟡 Medium

## Problem
Given two nodes `p` and `q` in a binary tree, return their lowest common ancestor (the deepest node that has both as descendants).

## 🧮 Math / Recurrence
Post-order: a subtree reports the LCA if found, else the matched target. The LCA is the node where the two targets surface from **different** sides:

$$
\text{lca}(node) =
\begin{cases}
node & node \in \{p, q\}\ \text{or both children return non-null} \\
\text{the non-null child} & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
Each node asks its left and right subtrees whether they contain `p` or `q`. If both report a hit, the current node is the split point — the LCA. If only one side reports, that result bubbles up. Matching `node` itself to `p`/`q` handles the ancestor-of-the-other case.

## 🔢 Iteration trace (`root=[3,5,1,...]`, p=5, q=1)
- Left subtree finds 5, right finds 1 → LCA = **3**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def lowest_common_ancestor(root: TreeNode | None, p: TreeNode, q: TreeNode) -> TreeNode | None:
    if root is None or root is p or root is q:
        return root
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    if left and right:
        return root
    return left or right


if __name__ == "__main__":
    n5 = TreeNode(5)
    n1 = TreeNode(1)
    root = TreeNode(3, n5, n1)
    print(lowest_common_ancestor(root, n5, n1).val)   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;
    return left ? left : right;
}

int main() {
    TreeNode* n5 = new TreeNode(5);
    TreeNode* n1 = new TreeNode(1);
    TreeNode* root = new TreeNode(3);
    root->left = n5; root->right = n1;
    cout << lowestCommonAncestor(root, n5, n1)->val << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
