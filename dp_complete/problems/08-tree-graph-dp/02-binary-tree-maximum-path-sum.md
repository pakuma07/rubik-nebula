# Binary Tree Maximum Path Sum

> Downward chain + global bend update. LC 124 · 🔴 Hard

## Problem
Find the maximum path sum in a binary tree, where a path is any sequence of connected nodes (need not pass through the root).

## 🧮 Math / Recurrence
Each node returns its best **downward** chain; a global maximum considers a path bending at the node:

$$
\text{down}(node) = node.val + \max(0,\ \text{down}_L,\ \text{down}_R)
$$
$$
\text{best} = \max\big(\text{best},\ node.val + \max(0, \text{down}_L) + \max(0, \text{down}_R)\big)
$$

## 🧠 Logic
A node can only pass **one** branch up to its parent (a chain), so it returns `val + best single child` (clamping negatives to 0). But the optimal path might *bend* at this node, using both children — that full bent sum updates a global maximum but is never returned upward.

## 🔢 Iteration trace (`[-10,9,20,null,null,15,7]`)
- Best path `15→20→7` = **42**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def max_path_sum(root: TreeNode | None) -> int:
    best = float("-inf")

    def down(node: TreeNode | None) -> int:
        nonlocal best
        if not node:
            return 0
        left = max(0, down(node.left))
        right = max(0, down(node.right))
        best = max(best, node.val + left + right)
        return node.val + max(left, right)

    down(root)
    return best


if __name__ == "__main__":
    root = TreeNode(-10, TreeNode(9), TreeNode(20, TreeNode(15), TreeNode(7)))
    print(max_path_sum(root))   # 42
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

int best;

int down(TreeNode* node) {
    if (!node) return 0;
    int left = max(0, down(node->left));
    int right = max(0, down(node->right));
    best = max(best, node->val + left + right);
    return node->val + max(left, right);
}

int maxPathSum(TreeNode* root) {
    best = INT_MIN;
    down(root);
    return best;
}

int main() {
    TreeNode* root = new TreeNode(-10);
    root->left = new TreeNode(9);
    root->right = new TreeNode(20);
    root->right->left = new TreeNode(15);
    root->right->right = new TreeNode(7);
    cout << maxPathSum(root) << "\n";   // 42
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
