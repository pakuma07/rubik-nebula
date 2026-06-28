# Uncrossed Lines

> Identical to LCS on two arrays. LC 1035 · 🟡 Medium

## Problem
Draw connecting lines between equal numbers in `nums1` and `nums2` so that no two lines **cross**. Return the maximum number of lines.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] + 1 & nums1_i = nums2_j \\
\max(dp[i-1][j],\ dp[i][j-1]) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
Non-crossing connecting lines preserve order on both sides — exactly the definition of a **common subsequence**. So the maximum number of lines equals `LCS(nums1, nums2)`; the recurrence is identical to [LCS](01-longest-common-subsequence.md) applied to integer arrays.

## 🔢 Iteration trace (`nums1 = [1,4,2]`, `nums2 = [1,2,4]`)
- Common subsequences: `[1,4]` or `[1,2]`, length 2.
- `dp[3][3] = ` **2**.

## 🐍 Python
```python
def max_uncrossed_lines(nums1: list[int], nums2: list[int]) -> int:
    n, m = len(nums1), len(nums2)
    prev = [0] * (m + 1)
    for i in range(1, n + 1):
        cur = [0] * (m + 1)
        for j in range(1, m + 1):
            cur[j] = prev[j - 1] + 1 if nums1[i - 1] == nums2[j - 1] else max(prev[j], cur[j - 1])
        prev = cur
    return prev[m]


if __name__ == "__main__":
    print(max_uncrossed_lines([1, 4, 2], [1, 2, 4]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxUncrossedLines(vector<int>& a, vector<int>& b) {
    int n = a.size(), m = b.size();
    vector<int> prev(m + 1, 0), cur(m + 1, 0);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j)
            cur[j] = (a[i - 1] == b[j - 1]) ? prev[j - 1] + 1
                                            : max(prev[j], cur[j - 1]);
        prev = cur;
    }
    return prev[m];
}

int main() {
    vector<int> a = {1, 4, 2}, b = {1, 2, 4};
    cout << maxUncrossedLines(a, b) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)`.
