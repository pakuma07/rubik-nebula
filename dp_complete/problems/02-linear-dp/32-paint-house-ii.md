# Paint House II (k Colors)

> Drop O(nk²) to O(nk) with best + second-best. LC 265 · 🔴 Hard

## Problem
Like [Paint House](31-paint-house.md) but with `k` colors. Minimize total cost with no two adjacent houses sharing a color.

## 🧮 Math / Recurrence
Naively `dp[i][c] = costs[i][c] + min_{c'≠c} dp[i-1][c']` is `O(nk²)`. Track the previous row's **smallest** and **second-smallest** values (`min1` at index `idx1`, `min2`):

$$
dp[i][c] = costs[i][c] +
\begin{cases}
min2 & c = idx1 \\
min1 & c \ne idx1
\end{cases}
$$

## 🧠 Logic
The only forbidden predecessor color is `c` itself. So for every color except the previous best you may use the previous **best**; only the previous-best color is forced to fall back to the **second-best**. Two scalars (`min1`, `min2`) replace the inner `O(k)` minimum, giving `O(nk)`.

## 🔢 Iteration trace (`costs = [[1,5,3],[2,9,4]]`)
- Row 0: min1=1 (idx1=0), min2=3.
- Row 1: color0 → 2+min2=2+3=5; color1 → 9+min1=9+1=10; color2 → 4+min1=4+1=5.
- Row-1 best = `min(5,10,5) = ` **5**.

## 🐍 Python
```python
def min_cost_ii(costs: list[list[int]]) -> int:
    if not costs:
        return 0
    k = len(costs[0])
    prev = costs[0][:]
    for i in range(1, len(costs)):
        # find smallest and second smallest of prev
        idx1 = min(range(k), key=lambda c: prev[c])
        min1 = prev[idx1]
        min2 = min(prev[c] for c in range(k) if c != idx1)
        cur = [costs[i][c] + (min2 if c == idx1 else min1) for c in range(k)]
        prev = cur
    return min(prev)


if __name__ == "__main__":
    print(min_cost_ii([[1, 5, 3], [2, 9, 4]]))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int minCostII(vector<vector<int>>& costs) {
    if (costs.empty()) return 0;
    int k = costs[0].size();
    vector<int> prev = costs[0];
    for (size_t i = 1; i < costs.size(); ++i) {
        int idx1 = 0;
        for (int c = 1; c < k; ++c) if (prev[c] < prev[idx1]) idx1 = c;
        int min1 = prev[idx1], min2 = INT_MAX;
        for (int c = 0; c < k; ++c) if (c != idx1) min2 = min(min2, prev[c]);
        vector<int> cur(k);
        for (int c = 0; c < k; ++c)
            cur[c] = costs[i][c] + (c == idx1 ? min2 : min1);
        prev = cur;
    }
    return *min_element(prev.begin(), prev.end());
}

int main() {
    vector<vector<int>> costs = {{1, 5, 3}, {2, 9, 4}};
    cout << minCostII(costs) << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(nk)`.
- **Space:** `O(k)` (or `O(1)` extra if you track the two minima inline).
