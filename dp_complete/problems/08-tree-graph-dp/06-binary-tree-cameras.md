# Binary Tree Cameras

> 3-state greedy DP from leaves up. LC 968 · 🔴 Hard

## Problem
Place cameras on tree nodes; each camera monitors its parent, itself, and its children. Return the minimum number of cameras to monitor all nodes.

## 🧮 Math / Recurrence
Each node returns one of three states: `0` = not covered, `1` = covered without a camera, `2` = has a camera:

$$
\text{state}(node) =
\begin{cases}
2 & \text{some child is uncovered (0)} \\
1 & \text{some child has a camera (2)} \\
0 & \text{both children covered, no camera}
\end{cases}
$$

## 🧠 Logic
Greedily delay placing cameras: install one only when a child is uncovered, since putting it at the **parent** covers more nodes (grandchild, child, self, sibling). Leaves return "uncovered" so their parents get cameras. A `null` is treated as covered. If the root ends uncovered, add one camera.

## 🔢 Iteration trace (`[0,0,null,0,0]`)
- One camera on the second node covers all → **1**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def min_camera_cover(root: TreeNode | None) -> int:
    cameras = 0
    COVERED, CAMERA, UNCOVERED = 1, 2, 0

    def dfs(node: TreeNode | None) -> int:
        nonlocal cameras
        if not node:
            return COVERED
        l = dfs(node.left)
        r = dfs(node.right)
        if l == UNCOVERED or r == UNCOVERED:
            cameras += 1
            return CAMERA
        if l == CAMERA or r == CAMERA:
            return COVERED
        return UNCOVERED

    if dfs(root) == UNCOVERED:
        cameras += 1
    return cameras


if __name__ == "__main__":
    root = TreeNode(0, TreeNode(0, TreeNode(0), TreeNode(0)))
    print(min_camera_cover(root))   # 1
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

int cameras;
const int COVERED = 1, CAMERA = 2, UNCOVERED = 0;

int dfs(TreeNode* node) {
    if (!node) return COVERED;
    int l = dfs(node->left), r = dfs(node->right);
    if (l == UNCOVERED || r == UNCOVERED) { ++cameras; return CAMERA; }
    if (l == CAMERA || r == CAMERA) return COVERED;
    return UNCOVERED;
}

int minCameraCover(TreeNode* root) {
    cameras = 0;
    if (dfs(root) == UNCOVERED) ++cameras;
    return cameras;
}

int main() {
    TreeNode* root = new TreeNode(0);
    root->left = new TreeNode(0);
    root->left->left = new TreeNode(0);
    root->left->right = new TreeNode(0);
    cout << minCameraCover(root) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
