# Diameter of Binary Tree

> Height + global max(left+right). LC 543 · 🟢 Easy

## Problem
Return the length (in edges) of the longest path between any two nodes in a binary tree.

## 🧮 Math / Recurrence
Each node returns its height; the diameter passing **through** the node equals the sum of child heights:

$$
\text{height}(node) = 1 + \max(h_L, h_R), \qquad \text{best} = \max(\text{best},\ h_L + h_R)
$$

## 🧠 Logic
The longest path through a node uses its deepest left chain plus its deepest right chain — `h_L + h_R` edges. A node still passes only its taller side upward as the height. Tracking the global maximum of `h_L + h_R` over all nodes gives the diameter in one post-order pass.

## 🔢 Iteration trace (`[1,2,3,4,5]`)
- Path `4→2→5` or `4→2→1→3` → diameter **3**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def diameter_of_binary_tree(root: TreeNode | None) -> int:
    best = 0

    def height(node: TreeNode | None) -> int:
        nonlocal best
        if not node:
            return 0
        l = height(node.left)
        r = height(node.right)
        best = max(best, l + r)
        return 1 + max(l, r)

    height(root)
    return best


if __name__ == "__main__":
    root = TreeNode(1, TreeNode(2, TreeNode(4), TreeNode(5)), TreeNode(3))
    print(diameter_of_binary_tree(root))   # 3
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

int height(TreeNode* node) {
    if (!node) return 0;
    int l = height(node->left), r = height(node->right);
    best = max(best, l + r);
    return 1 + max(l, r);
}

int diameterOfBinaryTree(TreeNode* root) {
    best = 0;
    height(root);
    return best;
}

int main() {
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right = new TreeNode(3);
    cout << diameterOfBinaryTree(root) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
