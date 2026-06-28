# Minimum Falling Path Sum

> Min of the 3 cells above. LC 931 · 🟡 Medium

## Problem
Given an `n × n` matrix, a falling path starts at any cell in the first row and each step moves to the cell directly below or diagonally below-left/right. Return the minimum falling path sum.

## 🧮 Math / Recurrence
$$
dp[i][j] = matrix[i][j] + \min\big(dp[i-1][j-1],\ dp[i-1][j],\ dp[i-1][j+1]\big)
$$

## 🧠 Logic
Each cell can be fallen into from the cell directly above or the two diagonals above, so it adds the minimum of those three (skipping out-of-range columns). Processing top to bottom, the previous row is fully resolved; the answer is the minimum of the last row.

## 🔢 Iteration trace (`[[2,1,3],[6,5,4],[7,8,9]]`)
- Row1 `[2,1,3]`. Row2 `[7,6,5]`. Row3 `[13,13,14]`.
- Minimum = **13** (`1→4→... ` best `1→5→7`=13).

## 🐍 Python
```python
def min_falling_path_sum(matrix: list[list[int]]) -> int:
    n = len(matrix)
    dp = matrix[0][:]
    for i in range(1, n):
        nxt = [0] * n
        for j in range(n):
            best = dp[j]
            if j > 0:
                best = min(best, dp[j - 1])
            if j < n - 1:
                best = min(best, dp[j + 1])
            nxt[j] = matrix[i][j] + best
        dp = nxt
    return min(dp)


if __name__ == "__main__":
    print(min_falling_path_sum([[2, 1, 3], [6, 5, 4], [7, 8, 9]]))   # 13
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minFallingPathSum(vector<vector<int>>& matrix) {
    int n = matrix.size();
    vector<int> dp = matrix[0];
    for (int i = 1; i < n; ++i) {
        vector<int> nxt(n);
        for (int j = 0; j < n; ++j) {
            int best = dp[j];
            if (j > 0) best = min(best, dp[j - 1]);
            if (j < n - 1) best = min(best, dp[j + 1]);
            nxt[j] = matrix[i][j] + best;
        }
        dp = nxt;
    }
    return *min_element(dp.begin(), dp.end());
}

int main() {
    vector<vector<int>> m = {{2, 1, 3}, {6, 5, 4}, {7, 8, 9}};
    cout << minFallingPathSum(m) << "\n";   // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
