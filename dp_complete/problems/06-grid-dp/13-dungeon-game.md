# Dungeon Game

> Reverse DP — minimum HP to enter each cell. LC 174 · 🔴 Hard

## Problem
A knight starts top-left and must reach the bottom-right princess, moving right/down. Each cell adds/subtracts health; health must stay `≥ 1` at all times. Return the minimum initial health.

## 🧮 Math / Recurrence
Fill **backward** from the destination. `dp[i][j]` = minimum HP needed upon **entering** `(i,j)`:

$$
dp[i][j] = \max\big(1,\ \min(dp[i+1][j],\ dp[i][j+1]) - grid[i][j]\big)
$$

## 🧠 Logic
Forward DP fails because the health you need at a cell depends on the *future* damage, not the past. Computing backward, you know the HP required to survive the rest of the path; subtract the current cell's value and clamp to `1` (HP can never drop below one). The answer is `dp[0][0]`.

## 🔢 Iteration trace (`[[-2,-3,3],[-5,-10,1],[10,30,-5]]`)
- Backward fill yields `dp[0][0] = ` **7**.

## 🐍 Python
```python
def calculate_minimum_hp(dungeon: list[list[int]]) -> int:
    m, n = len(dungeon), len(dungeon[0])
    INF = float("inf")
    dp = [[INF] * (n + 1) for _ in range(m + 1)]
    dp[m][n - 1] = dp[m - 1][n] = 1
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            need = min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j]
            dp[i][j] = max(1, need)
    return dp[0][0]


if __name__ == "__main__":
    print(calculate_minimum_hp([[-2, -3, 3], [-5, -10, 1], [10, 30, -5]]))   # 7
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int calculateMinimumHP(vector<vector<int>>& dungeon) {
    int m = dungeon.size(), n = dungeon[0].size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, INT_MAX));
    dp[m][n - 1] = dp[m - 1][n] = 1;
    for (int i = m - 1; i >= 0; --i)
        for (int j = n - 1; j >= 0; --j) {
            int need = min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j];
            dp[i][j] = max(1, need);
        }
    return dp[0][0];
}

int main() {
    vector<vector<int>> d = {{-2, -3, 3}, {-5, -10, 1}, {10, 30, -5}};
    cout << calculateMinimumHP(d) << "\n";   // 7
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(m·n)`.
