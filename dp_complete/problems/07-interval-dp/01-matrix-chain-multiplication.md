# Matrix Chain Multiplication

> Split on the last multiply. Classic / GFG · 🔴 Hard

## Problem
Given matrix dimensions `p[0..n]` (matrix `i` is `p[i-1] × p[i]`), find the minimum number of scalar multiplications to compute the product `A₁·A₂·…·Aₙ`.

## 🧮 Math / Recurrence
`dp[i][j]` = min cost to multiply matrices `i..j`:

$$
dp[i][j] = \min_{i \le k < j}\big(dp[i][k] + dp[k+1][j] + p_{i-1}\,p_k\,p_j\big), \qquad dp[i][i] = 0
$$

## 🧠 Logic
The final product is formed by **one last multiplication** that combines the block `A_i..A_k` (a `p_{i-1} × p_k` matrix) with `A_{k+1}..A_j` (a `p_k × p_j` matrix), costing `p_{i-1}·p_k·p_j`. Trying every split `k` and adding the optimal costs of the two halves gives the classic interval DP, filled by increasing chain length.

## 🔢 Iteration trace (`p = [40,20,30,10,30]`)
- Optimal parenthesization cost = **26000**.

## 🐍 Python
```python
def matrix_chain_order(p: list[int]) -> int:
    n = len(p) - 1
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    for length in range(2, n + 1):
        for i in range(1, n - length + 2):
            j = i + length - 1
            dp[i][j] = min(
                dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j]
                for k in range(i, j))
    return dp[1][n]


if __name__ == "__main__":
    print(matrix_chain_order([40, 20, 30, 10, 30]))   # 26000
```

## ⚙️ C++
```cpp
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int matrixChainOrder(vector<int>& p) {
    int n = p.size() - 1;
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
    for (int len = 2; len <= n; ++len)
        for (int i = 1; i + len - 1 <= n; ++i) {
            int j = i + len - 1;
            dp[i][j] = INT_MAX;
            for (int k = i; k < j; ++k)
                dp[i][j] = min(dp[i][j],
                    dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j]);
        }
    return dp[1][n];
}

int main() {
    vector<int> p = {40, 20, 30, 10, 30};
    cout << matrixChainOrder(p) << "\n";   // 26000
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n²)`.
