# Remove Boxes

> 3D interval state dp[i][j][k]. LC 546 · 🔴 Hard

## Problem
Given colored boxes, repeatedly remove a contiguous run of `k` same-colored boxes for `k²` points. Maximize the total points after removing all boxes.

## 🧮 Math / Recurrence
`dp[i][j][k]` = max points for `boxes[i..j]` with `k` boxes equal to `boxes[i]` already attached on its left:

$$
dp[i][j][k] = \max
\begin{cases}
(k+1)^2 + dp[i+1][j][0] & \text{remove } boxes[i] \text{ block now} \\
dp[i+1][m-1][0] + dp[m][j][k+1] & \exists\, m > i:\ boxes[m] = boxes[i]
\end{cases}
$$

## 🧠 Logic
The reward `k²` depends on how many same-colored boxes are removed *together*, which 2D state can't capture. The third dimension `k` records the count of `boxes[i]`-colored boxes glued to the left. We either cash in that block now, or defer by clearing the gap up to a later same-colored box `m` and merging, growing `k`.

## 🔢 Iteration trace (`[1,3,2,2,2,3,4,3,1]`)
- Optimal removal yields **23**.

## 🐍 Python
```python
from functools import lru_cache

def remove_boxes(boxes: list[int]) -> int:
    n = len(boxes)

    @lru_cache(maxsize=None)
    def dp(i: int, j: int, k: int) -> int:
        if i > j:
            return 0
        # merge leading equal boxes into k
        while i < j and boxes[i + 1] == boxes[i]:
            i += 1
            k += 1
        res = (k + 1) ** 2 + dp(i + 1, j, 0)
        for m in range(i + 1, j + 1):
            if boxes[m] == boxes[i]:
                res = max(res, dp(i + 1, m - 1, 0) + dp(m, j, k + 1))
        return res

    return dp(0, n - 1, 0)


if __name__ == "__main__":
    print(remove_boxes([1, 3, 2, 2, 2, 3, 4, 3, 1]))   # 23
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <cstring>
#include <iostream>
#include <vector>
using namespace std;

int memo[100][100][100];

int dp(vector<int>& b, int i, int j, int k) {
    if (i > j) return 0;
    while (i < j && b[i + 1] == b[i]) { ++i; ++k; }
    int& res = memo[i][j][k];
    if (res) return res;
    res = (k + 1) * (k + 1) + dp(b, i + 1, j, 0);
    for (int m = i + 1; m <= j; ++m)
        if (b[m] == b[i])
            res = max(res, dp(b, i + 1, m - 1, 0) + dp(b, m, j, k + 1));
    return res;
}

int removeBoxes(vector<int>& boxes) {
    memset(memo, 0, sizeof(memo));
    return dp(boxes, 0, boxes.size() - 1, 0);
}

int main() {
    vector<int> boxes = {1, 3, 2, 2, 2, 3, 4, 3, 1};
    cout << removeBoxes(boxes) << "\n";   // 23
}
```

## ⏱️ Complexity
- **Time:** `O(n⁴)`.
- **Space:** `O(n³)`.
