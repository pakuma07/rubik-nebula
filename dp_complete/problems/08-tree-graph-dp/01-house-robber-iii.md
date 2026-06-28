# House Robber III

> Per node return (rob, skip). LC 337 · 🟡 Medium

## Problem
Houses form a binary tree; you cannot rob two directly-connected houses. Return the maximum amount you can rob. (Also appears as [02 — House Robber III](../02-linear-dp/09-house-robber-iii.md).)

## 🧮 Math / Recurrence
Post-order, each node returns a pair `(rob, skip)`:

$$
rob = node.val + skip_L + skip_R, \qquad skip = \max(rob_L, skip_L) + \max(rob_R, skip_R)
$$

Answer = `max(rob_root, skip_root)`.

## 🧠 Logic
Robbing a node forbids robbing its children, so it pairs with the children's **skip** values. Skipping a node lets each child independently choose its better option. Returning both states avoids recomputation and resolves the constraint locally in one DFS.

## 🔢 Iteration trace (`[3,2,3,null,3,null,1]`)
- Rob the root-level `3`s and the inner `3` → **7**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def rob(root: TreeNode | None) -> int:
    def dfs(node: TreeNode | None) -> tuple[int, int]:
        if not node:
            return (0, 0)
        l = dfs(node.left)
        r = dfs(node.right)
        rob_node = node.val + l[1] + r[1]
        skip_node = max(l) + max(r)
        return (rob_node, skip_node)

    return max(dfs(root))


if __name__ == "__main__":
    root = TreeNode(3, TreeNode(2, None, TreeNode(3)), TreeNode(3, None, TreeNode(1)))
    print(rob(root))   # 7
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

pair<int, int> dfs(TreeNode* node) {
    if (!node) return {0, 0};
    auto l = dfs(node->left), r = dfs(node->right);
    int robNode = node->val + l.second + r.second;
    int skipNode = max(l.first, l.second) + max(r.first, r.second);
    return {robNode, skipNode};
}

int rob(TreeNode* root) {
    auto res = dfs(root);
    return max(res.first, res.second);
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(2);
    root->left->right = new TreeNode(3);
    root->right = new TreeNode(3);
    root->right->right = new TreeNode(1);
    cout << rob(root) << "\n";   // 7
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)` recursion.
