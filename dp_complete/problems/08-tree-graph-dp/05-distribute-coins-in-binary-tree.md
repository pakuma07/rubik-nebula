# Distribute Coins in Binary Tree

> Sum |subtree balance| over edges. LC 979 · 🟡 Medium

## Problem
A binary tree has `n` nodes and exactly `n` coins distributed among them. In one move you shift a coin to an adjacent node. Return the minimum number of moves so every node holds exactly one coin.

## 🧮 Math / Recurrence
Each node returns its balance (coins − nodes) for its subtree; each edge must carry the absolute balance:

$$
\text{balance}(node) = node.val - 1 + b_L + b_R, \qquad \text{moves} \mathrel{+}= |b_L| + |b_R|
$$

## 🧠 Logic
The edge between a child and its parent must transport exactly `|child subtree balance|` coins (excess pushed up, deficit pulled down). Summing those absolute flows over all edges counts every required move. The net balance bubbles upward so the parent knows its own surplus/deficit.

## 🔢 Iteration trace (`[3,0,0]`)
- Root has 2 extra coins; one to each child → **2** moves.

## 🐍 Python
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val, self.left, self.right = val, left, right


def distribute_coins(root: TreeNode | None) -> int:
    moves = 0

    def balance(node: TreeNode | None) -> int:
        nonlocal moves
        if not node:
            return 0
        l = balance(node.left)
        r = balance(node.right)
        moves += abs(l) + abs(r)
        return node.val - 1 + l + r

    balance(root)
    return moves


if __name__ == "__main__":
    root = TreeNode(3, TreeNode(0), TreeNode(0))
    print(distribute_coins(root))   # 2
```

## ⚙️ C++
```cpp
#include <cstdlib>
#include <iostream>
using namespace std;

struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int v) : val(v), left(nullptr), right(nullptr) {}
};

int moves;

int balance(TreeNode* node) {
    if (!node) return 0;
    int l = balance(node->left), r = balance(node->right);
    moves += abs(l) + abs(r);
    return node->val - 1 + l + r;
}

int distributeCoins(TreeNode* root) {
    moves = 0;
    balance(root);
    return moves;
}

int main() {
    TreeNode* root = new TreeNode(3);
    root->left = new TreeNode(0);
    root->right = new TreeNode(0);
    cout << distributeCoins(root) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(h)`.
