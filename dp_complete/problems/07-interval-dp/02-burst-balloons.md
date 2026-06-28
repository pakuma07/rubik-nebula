# Burst Balloons

> Think which balloon bursts LAST. LC 312 · 🔴 Hard

## Problem
Each balloon `i` has value `nums[i]`. Bursting it yields `nums[i-1]·nums[i]·nums[i+1]` (out-of-range neighbors are `1`), and removes it. Maximize total coins from bursting all balloons.

## 🧮 Math / Recurrence
Pad with `1`s; `dp[i][j]` = max coins from the open interval `(i, j)`:

$$
dp[i][j] = \max_{i < k < j}\big(dp[i][k] + a_i\,a_k\,a_j + dp[k][j]\big)
$$

## 🧠 Logic
Bursting order is hard forward, so ask: which balloon `k` bursts **last** in `(i,j)`? Then its neighbors are exactly the fixed boundaries `a_i` and `a_j` (everything between has already gone), giving gain `a_i·a_k·a_j`, plus the two independent sub-intervals. This decoupling is the key that makes the interval DP work.

## 🔢 Iteration trace (`nums = [3,1,5,8]`)
- Best burst order yields **167**.

## 🐍 Python
```python
def max_coins(nums: list[int]) -> int:
    a = [1] + nums + [1]
    n = len(a)
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n):
        for i in range(n - length):
            j = i + length
            for k in range(i + 1, j):
                dp[i][j] = max(dp[i][j],
                               dp[i][k] + a[i] * a[k] * a[j] + dp[k][j])
    return dp[0][n - 1]


if __name__ == "__main__":
    print(max_coins([3, 1, 5, 8]))   # 167
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxCoins(vector<int>& nums) {
    vector<int> a;
    a.push_back(1);
    for (int x : nums) a.push_back(x);
    a.push_back(1);
    int n = a.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len < n; ++len)
        for (int i = 0; i + len < n; ++i) {
            int j = i + len;
            for (int k = i + 1; k < j; ++k)
                dp[i][j] = max(dp[i][j],
                    dp[i][k] + a[i] * a[k] * a[j] + dp[k][j]);
        }
    return dp[0][n - 1];
}

int main() {
    vector<int> nums = {3, 1, 5, 8};
    cout << maxCoins(nums) << "\n";   // 167
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n²)`.
