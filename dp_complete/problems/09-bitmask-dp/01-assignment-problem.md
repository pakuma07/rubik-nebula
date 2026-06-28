# Assignment Problem (Minimum Cost)

> dp[mask] = min cost assigning the first popcount(mask) people. Classic · 🔴 Hard

## Problem
Given an `n × n` cost matrix where `cost[i][j]` is the cost of person `i` doing job `j`, assign each person exactly one distinct job to minimize total cost.

## 🧮 Math / Recurrence
`dp[mask]` = min cost after assigning the first `popcount(mask)` people, where `mask` holds the used jobs:

$$
dp[mask \,|\, (1 \ll j)] = \min\big(\,\cdot\,,\ dp[mask] + cost[\,\text{popcount}(mask)\,][j]\big)
$$

## 🧠 Logic
The number of set bits in `mask` tells us exactly which person is being assigned next (people are processed in order), so we don't need a separate index. That person tries every free job `j`, marking it used. The full mask `2ⁿ−1` holds the optimal complete assignment.

## 🔢 Iteration trace (`cost = [[9,2],[6,4]]`)
- Person 0 → job 1 (2), person 1 → job 0 (6) → **8**.

## 🐍 Python
```python
def min_assignment(cost: list[list[int]]) -> int:
    n = len(cost)
    INF = float("inf")
    dp = [INF] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] == INF:
            continue
        i = bin(mask).count("1")            # next person
        if i >= n:
            continue
        for j in range(n):
            if not (mask >> j) & 1:
                nm = mask | (1 << j)
                dp[nm] = min(dp[nm], dp[mask] + cost[i][j])
    return dp[(1 << n) - 1]


if __name__ == "__main__":
    print(min_assignment([[9, 2], [6, 4]]))   # 8
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minAssignment(vector<vector<int>>& cost) {
    int n = cost.size();
    const int INF = 1e9;
    vector<int> dp(1 << n, INF);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << n); ++mask) {
        if (dp[mask] == INF) continue;
        int i = __builtin_popcount(mask);
        if (i >= n) continue;
        for (int j = 0; j < n; ++j)
            if (!((mask >> j) & 1)) {
                int nm = mask | (1 << j);
                dp[nm] = min(dp[nm], dp[mask] + cost[i][j]);
            }
    }
    return dp[(1 << n) - 1];
}

int main() {
    vector<vector<int>> cost = {{9, 2}, {6, 4}};
    cout << minAssignment(cost) << "\n";   // 8
}
```

## ⏱️ Complexity
- **Time:** `O(2ⁿ · n)`.
- **Space:** `O(2ⁿ)`.
