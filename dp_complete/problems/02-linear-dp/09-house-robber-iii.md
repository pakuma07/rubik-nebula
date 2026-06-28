# House Robber III (Binary Tree)

> Non-adjacent loot, but the houses form a tree. LC 337 · 🟡 Medium

## Problem
Houses form a **binary tree**. Robbing a node forbids robbing its direct children. Return the maximum total. (Tree-DP cousin of [House Robber](07-house-robber.md); see also topic 08.)

## 🧮 Math / Recurrence
Each node returns a pair `(rob, skip)`:

$$
\text{rob}(v) = v.\text{val} + \text{skip}(L) + \text{skip}(R)
$$

$$
\text{skip}(v) = \max(\text{rob}(L), \text{skip}(L)) + \max(\text{rob}(R), \text{skip}(R))
$$

Answer = `max(rob(root), skip(root))`.

## 🧠 Logic
The linear "previous house" constraint becomes "parent/child" on a tree. Post-order DFS returns two numbers per node: the best if we **rob** this node (then children must be skipped) and the best if we **skip** it (children are free to be robbed or not). Combining children's pairs in `O(1)` gives a single bottom-up pass.

## 🔢 Iteration trace
```
        3
       / \
      2   3
       \    \
        3    1
```
| node | skip(L,R) sums | rob = val + skipL + skipR | skip = maxL + maxR |
|------|----------------|---------------------------|--------------------|
| leaf 3 (left subtree)  | (0,0) | 3 | 0 |
| node 2 | skipL=3? no — child is the `3` leaf | rob=2+0=2, skip=max(3,0)=3 | (2,3) |
| leaf 1 | – | 1 | 0 |
| node 3 (right) | – | rob=3+0=3, skip=max(1,0)=1 | (3,1) |
| **root 3** | skipL=3, skipR=1 | rob=3+3+1=**7**, skip=max(2,3)+max(3,1)=3+3=6 | max=**7** |

**Answer = 7** (rob the two leaves valued 3 and the root).

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def rob(root: TreeNode | None) -> int:
    def dfs(node: TreeNode | None) -> tuple[int, int]:
        if not node:
            return (0, 0)                       # (rob, skip)
        lr, ls = dfs(node.left)
        rr, rs = dfs(node.right)
        rob_here = node.val + ls + rs
        skip_here = max(lr, ls) + max(rr, rs)
        return (rob_here, skip_here)

    return max(dfs(root))


if __name__ == "__main__":
    root = TreeNode(3, TreeNode(2, None, TreeNode(3)),
                       TreeNode(3, None, TreeNode(1)))
    print(rob(root))            # 7
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

pair<int, int> dfs(TreeNode* node) {            // {rob, skip}
    if (!node) return {0, 0};
    auto [lr, ls] = dfs(node->left);
    auto [rr, rs] = dfs(node->right);
    int robHere = node->val + ls + rs;
    int skipHere = max(lr, ls) + max(rr, rs);
    return {robHere, skipHere};
}

int rob(TreeNode* root) {
    auto [r, s] = dfs(root);
    return max(r, s);
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(2);  root->left->right = new TreeNode(3);
    root->right = new TreeNode(3); root->right->right = new TreeNode(1);
    cout << rob(root) << "\n";   // 7
}
```

## ⏱️ Complexity
- **Time:** `O(n)` — one post-order visit per node.
- **Space:** `O(h)` recursion (tree height).
