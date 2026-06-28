# Maximum Product of Splitted Binary Tree

> Subtree sums; cut an edge, maximize product. LC 1339 · 🟡 Medium

## Problem
Remove one edge to split a binary tree into two subtrees. Maximize the product of the two subtree sums, modulo `10^9 + 7`.

## 🧮 Math / Recurrence
First compute the total sum; then for every subtree sum `s`, cutting its parent edge gives product `s · (total − s)`:

$$
\text{answer} = \max_{s\ \in\ \text{subtree sums}} s \cdot (total - s) \quad (\text{mod taken at the end})
$$

## 🧠 Logic
Cutting the edge above a subtree splits the tree into that subtree (sum `s`) and the rest (`total − s`). Two DFS passes: one to get the total, one to enumerate every subtree sum and track the best product. Apply the modulus only to the final answer to keep the product comparison correct.

## 🔢 Iteration trace (`[1,2,3,4,5,6]`)
- Total 21; best split sum 11 → `11 × 10 = ` **110**.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def max_product(root: TreeNode | None) -> int:
    MOD = 10**9 + 7
    sums: list[int] = []

    def subtree_sum(node: TreeNode | None) -> int:
        if not node:
            return 0
        s = node.val + subtree_sum(node.left) + subtree_sum(node.right)
        sums.append(s)
        return s

    total = subtree_sum(root)
    best = max(s * (total - s) for s in sums)
    return best % MOD


if __name__ == "__main__":
    root = TreeNode(1, TreeNode(2, TreeNode(4), TreeNode(5)), TreeNode(3, TreeNode(6)))
    print(max_product(root))   # 110
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

vector<long long> sums;

long long subtreeSum(TreeNode* node) {
    if (!node) return 0;
    long long s = node->val + subtreeSum(node->left) + subtreeSum(node->right);
    sums.push_back(s);
    return s;
}

int maxProduct(TreeNode* root) {
    sums.clear();
    long long total = subtreeSum(root), best = 0;
    for (long long s : sums) best = max(best, s * (total - s));
    return (int)(best % MOD);
}

int main() {
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);
    root->right = new TreeNode(3);
    root->right->left = new TreeNode(6);
    cout << maxProduct(root) << "\n";   // 110
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
