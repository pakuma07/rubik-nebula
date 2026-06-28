# Minimum Cost to Cut a Stick

> Cut positions become interval boundaries. LC 1547 · 🔴 Hard

## Problem
A stick of length `n` must be cut at given positions `cuts`. Each cut costs the **length of the current piece** being cut. Cuts can be done in any order; minimize total cost.

## 🧮 Math / Recurrence
Sort cuts and pad with `0` and `n`. `dp[i][j]` = min cost to make all cuts strictly between boundary cuts `i` and `j`:

$$
dp[i][j] = \min_{i < k < j}\big(dp[i][k] + dp[k][j] + (c_j - c_i)\big)
$$

## 🧠 Logic
The cost of a cut depends on the current piece length, which depends on order — but if we decide which cut `k` is made **first** within `(c_i, c_j)`, that cut spans the whole current piece `c_j − c_i`, and the two resulting pieces are solved independently. Adding `0` and `n` as fixed boundaries turns positions into an interval DP.

## 🔢 Iteration trace (`n = 7`, `cuts = [1,3,4,5]`)
- Optimal order gives total cost **16**.

## 🐍 Python
```python
def min_cost(n: int, cuts: list[int]) -> int:
    c = sorted([0] + cuts + [n])
    m = len(c)
    dp = [[0] * m for _ in range(m)]
    for length in range(2, m):
        for i in range(m - length):
            j = i + length
            dp[i][j] = min(dp[i][k] + dp[k][j] for k in range(i + 1, j)) + (c[j] - c[i])
    return dp[0][m - 1]


if __name__ == "__main__":
    print(min_cost(7, [1, 3, 4, 5]))   # 16
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int minCost(int n, vector<int>& cuts) {
    vector<int> c = {0};
    for (int x : cuts) c.push_back(x);
    c.push_back(n);
    sort(c.begin(), c.end());
    int m = c.size();
    vector<vector<int>> dp(m, vector<int>(m, 0));
    for (int len = 2; len < m; ++len)
        for (int i = 0; i + len < m; ++i) {
            int j = i + len, best = INT_MAX;
            for (int k = i + 1; k < j; ++k)
                best = min(best, dp[i][k] + dp[k][j]);
            dp[i][j] = best + (c[j] - c[i]);
        }
    return dp[0][m - 1];
}

int main() {
    vector<int> cuts = {1, 3, 4, 5};
    cout << minCost(7, cuts) << "\n";   // 16
}
```

## ⏱️ Complexity
- **Time:** `O(m³)` for `m` cut boundaries.
- **Space:** `O(m²)`.
