# Campus Bikes II

> dp[mask] over assigned bikes. LC 1066 · 🟡 Medium

## Problem
`n` workers and `m` bikes on a grid (`n ≤ m`). Assign each worker a distinct bike to minimize the **sum** of Manhattan distances.

## 🧮 Math / Recurrence
`dp[mask]` = min total distance after assigning the first `popcount(mask)` workers, `mask` = used bikes:

$$
dp[mask \,|\, (1 \ll j)] = \min\big(\,\cdot\,,\ dp[mask] + |w_i - b_j|_1\big),\quad i = \text{popcount}(mask)
$$

## 🧠 Logic
The worker index is `popcount(mask)`, so each state assigns the next worker to any free bike `j`, adding their Manhattan distance. We stop once all workers are placed (we don't need a full bike mask, since `n ≤ m`). The minimum over states where exactly `n` bits are set is the answer.

## 🔢 Iteration trace (`workers=[[0,0],[2,1]]`, `bikes=[[1,2],[3,3]]`)
- Worker0→bike0 (3), worker1→bike1 (3) → **6**.

## 🐍 Python
```python
def assign_bikes(workers: list[list[int]], bikes: list[list[int]]) -> int:
    n, m = len(workers), len(bikes)
    INF = float("inf")
    dp = [INF] * (1 << m)
    dp[0] = 0
    best = INF
    for mask in range(1 << m):
        if dp[mask] == INF:
            continue
        i = bin(mask).count("1")
        if i == n:
            best = min(best, dp[mask])
            continue
        for j in range(m):
            if not (mask >> j) & 1:
                d = abs(workers[i][0] - bikes[j][0]) + abs(workers[i][1] - bikes[j][1])
                nm = mask | (1 << j)
                dp[nm] = min(dp[nm], dp[mask] + d)
    return best


if __name__ == "__main__":
    print(assign_bikes([[0, 0], [2, 1]], [[1, 2], [3, 3]]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

int assignBikes(vector<vector<int>>& workers, vector<vector<int>>& bikes) {
    int n = workers.size(), m = bikes.size();
    const int INF = 1e9;
    vector<int> dp(1 << m, INF);
    dp[0] = 0;
    int best = INF;
    for (int mask = 0; mask < (1 << m); ++mask) {
        if (dp[mask] == INF) continue;
        int i = __builtin_popcount(mask);
        if (i == n) { best = min(best, dp[mask]); continue; }
        for (int j = 0; j < m; ++j)
            if (!((mask >> j) & 1)) {
                int d = abs(workers[i][0] - bikes[j][0]) + abs(workers[i][1] - bikes[j][1]);
                int nm = mask | (1 << j);
                dp[nm] = min(dp[nm], dp[mask] + d);
            }
    }
    return best;
}

int main() {
    vector<vector<int>> w = {{0, 0}, {2, 1}}, b = {{1, 2}, {3, 3}};
    cout << assignBikes(w, b) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(2ᵐ · m)`.
- **Space:** `O(2ᵐ)`.
