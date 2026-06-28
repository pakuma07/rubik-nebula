# Minimum Cost to Merge Stones

> Merge K piles, prefix-sum cost. LC 1000 · 🔴 Hard

## Problem
Given `stones[]`, merging **exactly `K`** consecutive piles into one costs the sum of those piles. Return the minimum cost to merge all piles into one, or `-1` if impossible.

## 🧮 Math / Recurrence
Possible only if `(n − 1) % (K − 1) == 0`. `dp[i][j]` = min cost to merge `stones[i..j]` into the fewest piles:

$$
dp[i][j] = \min_{\substack{i \le t < j \\ t \equiv i \,(\text{mod } K-1)}}\big(dp[i][t] + dp[t+1][j]\big) + \big[\text{(len−1) divisible}\big]\cdot \text{sum}(i,j)
$$

## 🧠 Logic
Each merge reduces the pile count by `K−1`, so the interval can collapse to one pile only when its length satisfies the divisibility rule. We split into a left part that becomes a single pile (`t` aligned on the `K−1` step) and a right remainder. The `+sum(i,j)` is added once when the whole interval can finally be merged into one pile. A prefix-sum array gives `sum(i,j)` in `O(1)`.

## 🔢 Iteration trace (`stones = [3,2,4,1]`, `K = 2`)
- Merge to one pile costs **20**.

## 🐍 Python
```python
def merge_stones(stones: list[int], k: int) -> int:
    n = len(stones)
    if (n - 1) % (k - 1) != 0:
        return -1
    prefix = [0] * (n + 1)
    for i, x in enumerate(stones):
        prefix[i + 1] = prefix[i] + x
    INF = float("inf")
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
    print(merge_stones([3, 2, 4, 1], 2))   # 20
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
    vector<int> stones = {3, 2, 4, 1};
    cout << mergeStones(stones, 2) << "\n";   // 20
}
```

## ⏱️ Complexity
- **Time:** `O(n³ / K)`.
- **Space:** `O(n²)`.
