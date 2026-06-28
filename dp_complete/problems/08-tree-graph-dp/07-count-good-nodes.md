# Count Good Nodes in Binary Tree

> Pass max-so-far down the path. LC 1448 · 🟡 Medium

## Problem
A node is **good** if no node on the path from the root to it has a greater value. Count the good nodes.

## 🧮 Math / Recurrence
Carry the running maximum along the root-to-node path:

$$
\text{good}(node, M) = [\,node.val \ge M\,] + \text{good}(L, \max(M, node.val)) + \text{good}(R, \max(M, node.val))
$$

## 🧠 Logic
This is top-down tree DP: the only relevant history is the maximum value seen on the path so far. A node is counted when its value is at least that maximum; the updated maximum then flows to both children. The root is always good.

## 🔢 Iteration trace (`[3,1,4,3,null,1,5]`)
- Good nodes: 3, 4, 3, 5 → **4**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def good_nodes(root: TreeNode | None) -> int:
    def dfs(node: TreeNode | None, max_so_far: int) -> int:
        if not node:
            return 0
        good = 1 if node.val >= max_so_far else 0
        new_max = max(max_so_far, node.val)
        return good + dfs(node.left, new_max) + dfs(node.right, new_max)

    return dfs(root, float("-inf"))


if __name__ == "__main__":
    root = TreeNode(3, TreeNode(1, TreeNode(3)), TreeNode(4, TreeNode(1), TreeNode(5)))
    print(good_nodes(root))   # 4
```

## ⚙️ C++
```cpp
#include <climits>
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

int dfs(TreeNode* node, int maxSoFar) {
    if (!node) return 0;
    int good = node->val >= maxSoFar ? 1 : 0;
    int newMax = max(maxSoFar, node->val);
    return good + dfs(node->left, newMax) + dfs(node->right, newMax);
}

int goodNodes(TreeNode* root) {
    return dfs(root, INT_MIN);
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(1);
    root->left->left = new TreeNode(3);
    root->right = new TreeNode(4);
    root->right->left = new TreeNode(1);
    root->right->right = new TreeNode(5);
    cout << goodNodes(root) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
