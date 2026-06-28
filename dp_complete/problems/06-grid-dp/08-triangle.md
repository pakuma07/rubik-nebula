# Triangle

> Bottom-up min path through a triangle. LC 120 · 🟡 Medium

## Problem
Given a triangle array, return the minimum path sum from top to bottom, where from index `j` in a row you may move to index `j` or `j+1` in the next row.

## 🧮 Math / Recurrence
Process rows bottom-up, collapsing into a 1D array:

$$
dp[j] \mathrel{+}= \min\big(dp[j],\ dp[j+1]\big) + tri[i][j]
$$

starting `dp` as the last row.

## 🧠 Logic
Working bottom-up means each cell already knows the best sums of the two children directly below it, so it simply adds the smaller. This avoids tracking which downward branch was taken and reuses a single array of size equal to the base row, ending with the answer at `dp[0]`.

## 🔢 Iteration trace (`[[2],[3,4],[6,5,7],[4,1,8,3]]`)
- Bottom row `[4,1,8,3]`. → `[7,6,10]` → `[9,10]` → `[11]`.
- Minimum = **11** (`2→3→5→1`).

## 🐍 Python
```python
def minimum_total(triangle: list[list[int]]) -> int:
    dp = triangle[-1][:]
    for i in range(len(triangle) - 2, -1, -1):
        for j in range(len(triangle[i])):
            dp[j] = triangle[i][j] + min(dp[j], dp[j + 1])
    return dp[0]


if __name__ == "__main__":
    print(minimum_total([[2], [3, 4], [6, 5, 7], [4, 1, 8, 3]]))   # 11
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minimumTotal(vector<vector<int>>& triangle) {
    vector<int> dp = triangle.back();
    for (int i = (int)triangle.size() - 2; i >= 0; --i)
        for (int j = 0; j < (int)triangle[i].size(); ++j)
            dp[j] = triangle[i][j] + min(dp[j], dp[j + 1]);
    return dp[0];
}

int main() {
    vector<vector<int>> tri = {{2}, {3, 4}, {6, 5, 7}, {4, 1, 8, 3}};
    cout << minimumTotal(tri) << "\n";   // 11
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` for `n` rows.
- **Space:** `O(n)`.
