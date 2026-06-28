# Longest Univalue Path

> Extend a chain only when child value equals node. LC 687 · 🟡 Medium

## Problem
Return the length (in edges) of the longest path where **all nodes have the same value**.

## 🧮 Math / Recurrence
Each node returns the longest same-value chain going down; the global answer bends at a node:

$$
\text{arrow}_{L} =
\begin{cases}
\text{down}_L + 1 & val_L = val \\
0 & \text{otherwise}
\end{cases}
\qquad \text{best} = \max(\text{best},\ \text{arrow}_L + \text{arrow}_R)
$$

## 🧠 Logic
Like diameter, but an edge only counts if the child shares the node's value — otherwise the chain resets to 0. The node passes up the longer qualifying arrow, while the global tracks both arrows combined (a univalue path bending at the node).

## 🔢 Iteration trace (`[5,4,5,1,1,5]`)
- Path of `5`s `5→5→5` → length **2**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def longest_univalue_path(root: TreeNode | None) -> int:
    best = 0

    def arrow(node: TreeNode | None) -> int:
        nonlocal best
        if not node:
            return 0
        left = arrow(node.left)
        right = arrow(node.right)
        left = left + 1 if node.left and node.left.val == node.val else 0
        right = right + 1 if node.right and node.right.val == node.val else 0
        best = max(best, left + right)
        return max(left, right)

    arrow(root)
    return best


if __name__ == "__main__":
    root = TreeNode(5, TreeNode(4, TreeNode(1), TreeNode(1)), TreeNode(5, None, TreeNode(5)))
    print(longest_univalue_path(root))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

int best;

int arrow(TreeNode* node) {
    if (!node) return 0;
    int left = arrow(node->left), right = arrow(node->right);
    left = (node->left && node->left->val == node->val) ? left + 1 : 0;
    right = (node->right && node->right->val == node->val) ? right + 1 : 0;
    best = max(best, left + right);
    return max(left, right);
}

int longestUnivaluePath(TreeNode* root) {
    best = 0;
    arrow(root);
    return best;
}

int main() {
    TreeNode* root = new TreeNode(5);
    root->left = new TreeNode(4);
    root->left->left = new TreeNode(1);
    root->left->right = new TreeNode(1);
    root->right = new TreeNode(5);
    root->right->right = new TreeNode(5);
    cout << longestUnivaluePath(root) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
