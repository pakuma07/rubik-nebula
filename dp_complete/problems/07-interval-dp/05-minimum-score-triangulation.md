# Minimum Score Triangulation of Polygon

> Pick the apex k that splits the fan. LC 1039 · 🟡 Medium

## Problem
Given the vertex values of a convex polygon, triangulate it; each triangle's score is the product of its three vertex values. Return the minimum total score over all triangulations.

## 🧮 Math / Recurrence
`dp[i][j]` = min triangulation score of the sub-polygon spanning vertices `i..j`:

$$
dp[i][j] = \min_{i < k < j}\big(dp[i][k] + v_i\,v_k\,v_j + dp[k][j]\big), \qquad dp[i][i+1] = 0
$$

## 🧠 Logic
Fix the edge `(i, j)`; in any triangulation it belongs to exactly one triangle with some apex `k`. That triangle costs `v_i·v_k·v_j` and splits the polygon into two smaller fans `[i..k]` and `[k..j]`, solved independently. Minimizing over the apex `k` gives the interval recurrence.

## 🔢 Iteration trace (`values = [1,3,1,4,1,5]`)
- Optimal triangulation score = **13**.

## 🐍 Python
```python
def min_score_triangulation(values: list[int]) -> int:
    n = len(values)
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n):
        for i in range(n - length):
            j = i + length
            dp[i][j] = min(
                dp[i][k] + values[i] * values[k] * values[j] + dp[k][j]
                for k in range(i + 1, j))
    return dp[0][n - 1]


if __name__ == "__main__":
    print(min_score_triangulation([1, 3, 1, 4, 1, 5]))   # 13
```

## ⚙️ C++
```cpp
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int minScoreTriangulation(vector<int>& v) {
    int n = v.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int len = 2; len < n; ++len)
        for (int i = 0; i + len < n; ++i) {
            int j = i + len;
            dp[i][j] = INT_MAX;
            for (int k = i + 1; k < j; ++k)
                dp[i][j] = min(dp[i][j],
                    dp[i][k] + v[i] * v[k] * v[j] + dp[k][j]);
        }
    return dp[0][n - 1];
}

int main() {
    vector<int> v = {1, 3, 1, 4, 1, 5};
    cout << minScoreTriangulation(v) << "\n";   // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n²)`.
