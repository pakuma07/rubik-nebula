# Maximum Compatibility Score Sum

> Assign students ↔ mentors. LC 1947 · 🟡 Medium

## Problem
`m` students and `m` mentors each answer `n` binary survey questions. Compatibility of a pair = number of matching answers. Assign each student to a distinct mentor to maximize total compatibility.

## 🧮 Math / Recurrence
`dp[mask]` = best score after assigning the first `popcount(mask)` students, with `mask` = used mentors:

$$
dp[mask \,|\, (1 \ll j)] = \max\big(\,\cdot\,,\ dp[mask] + score[\,\text{popcount}(mask)\,][j]\big)
$$

## 🧠 Logic
Identical structure to the assignment problem but **maximizing**. Precompute `score[i][j]` = matching answers between student `i` and mentor `j`. The student index equals the popcount of the mentor mask, so each state assigns the next student to any free mentor.

## 🔢 Iteration trace (`students=[[1,1,0],[1,0,1],[0,0,1]]`, `mentors=[[1,0,0],[0,0,1],[1,1,0]]`)
- Best assignment → **8**.

## 🐍 Python
```python
def max_compatibility_sum(students: list[list[int]], mentors: list[list[int]]) -> int:
    m, n = len(students), len(students[0])
    score = [[sum(a == b for a, b in zip(students[i], mentors[j])) for j in range(m)]
             for i in range(m)]
    dp = [-1] * (1 << m)
    dp[0] = 0
    for mask in range(1 << m):
        if dp[mask] < 0:
            continue
        i = bin(mask).count("1")
        if i >= m:
            continue
        for j in range(m):
            if not (mask >> j) & 1:
                nm = mask | (1 << j)
                dp[nm] = max(dp[nm], dp[mask] + score[i][j])
    return dp[(1 << m) - 1]


if __name__ == "__main__":
    s = [[1, 1, 0], [1, 0, 1], [0, 0, 1]]
    m_ = [[1, 0, 0], [0, 0, 1], [1, 1, 0]]
    print(max_compatibility_sum(s, m_))   # 8
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxCompatibilitySum(vector<vector<int>>& students, vector<vector<int>>& mentors) {
    int m = students.size(), n = students[0].size();
    vector<vector<int>> score(m, vector<int>(m, 0));
    for (int i = 0; i < m; ++i)
        for (int j = 0; j < m; ++j)
            for (int k = 0; k < n; ++k)
                score[i][j] += students[i][k] == mentors[j][k];
    vector<int> dp(1 << m, -1);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << m); ++mask) {
        if (dp[mask] < 0) continue;
        int i = __builtin_popcount(mask);
        if (i >= m) continue;
        for (int j = 0; j < m; ++j)
            if (!((mask >> j) & 1)) {
                int nm = mask | (1 << j);
                dp[nm] = max(dp[nm], dp[mask] + score[i][j]);
            }
    }
    return dp[(1 << m) - 1];
}

int main() {
    vector<vector<int>> s = {{1, 1, 0}, {1, 0, 1}, {0, 0, 1}};
    vector<vector<int>> m = {{1, 0, 0}, {0, 0, 1}, {1, 1, 0}};
    cout << maxCompatibilitySum(s, m) << "\n";   // 8
}
```

## ⏱️ Complexity
- **Time:** `O(2ᵐ · m + m² · n)`.
- **Space:** `O(2ᵐ)`.
