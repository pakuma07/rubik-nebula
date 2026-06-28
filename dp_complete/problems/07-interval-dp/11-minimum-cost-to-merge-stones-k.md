# Minimum Cost to Merge Stones (K groups)

> Merge in groups of K — the partition view. LC 1000 · 🔴 Hard

## Problem
Merge **exactly `K`** consecutive piles at a cost equal to their sum, until one pile remains. Return the minimum total cost, or `-1` if impossible. (Companion to the classic interval form in [04 — Minimum Cost to Merge Stones](04-minimum-cost-to-merge-stones.md).)

## 🧮 Math / Recurrence
Feasible iff `(n − 1) % (K − 1) == 0`. With `prefix` sums and `dp[i][j]` = min cost to merge `stones[i..j]` into the fewest piles:

$$
dp[i][j] = \min_{\substack{i \le t < j \\ (t-i) \equiv 0 \,(\text{mod } K-1)}}\big(dp[i][t] + dp[t+1][j]\big), \quad
\text{add } \text{sum}(i,j)\ \text{if length collapses to one}
$$

## 🧠 Logic
Each merge reduces the pile count by `K−1`; only intervals whose length permits full collapse pay the extra `sum(i,j)`. The split index `t` must leave the left part mergeable into a single pile, hence the `K−1` step. This is the "partition into K-aligned groups" reading of the same recurrence.

## 🔢 Iteration trace (`stones = [3,5,1,2,6]`, `K = 3`)
- Optimal merges cost **25**.

## 🐍 Python
```python
def merge_stones(stones: list[int], k: int) -> int:
    n = len(stones)
    if (n - 1) % (k - 1) != 0:
        return -1
    prefix = [0] * (n + 1)
    for i, x in enumerate(stones):
        prefix[i + 1] = prefix[i] + x
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = min(dp[i][t] + dp[t + 1][j]
                           for t in range(i, j, k - 1))
            if (length - 1) % (k - 1) == 0:
                dp[i][j] += prefix[j + 1] - prefix[i]
    return dp[0][n - 1]


if __name__ == "__main__":
    print(merge_stones([3, 5, 1, 2, 6], 3))   # 25
```

## ⚙️ C++
```cpp
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int mergeStones(vector<int>& stones, int k) {
    int n = stones.size();
    if ((n - 1) % (k - 1) != 0) return -1;
    vector<int> prefix(n + 1, 0);
    for (int i = 0; i < n; ++i) prefix[i + 1] = prefix[i] + stones[i];
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            dp[i][j] = INT_MAX;
            for (int t = i; t < j; t += k - 1)
                dp[i][j] = min(dp[i][j], dp[i][t] + dp[t + 1][j]);
            if ((len - 1) % (k - 1) == 0)
                dp[i][j] += prefix[j + 1] - prefix[i];
        }
    return dp[0][n - 1];
}

int main() {
    vector<int> stones = {3, 5, 1, 2, 6};
    cout << mergeStones(stones, 3) << "\n";   // 25
}
```

## ⏱️ Complexity
- **Time:** `O(n³ / K)`.
- **Space:** `O(n²)`.
