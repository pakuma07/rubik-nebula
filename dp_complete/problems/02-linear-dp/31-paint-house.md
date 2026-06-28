# Paint House

> Per-color DP where adjacent houses must differ. LC 256 · 🟡 Medium

## Problem
Paint `n` houses with 3 colors (red/blue/green) so **no two adjacent houses share a color**; `costs[i][c]` is the cost. Minimize total cost.

## 🧮 Math / Recurrence
Let `dp[i][c]` = min cost to paint house `i` color `c`:

$$
dp[i][c] = costs[i][c] + \min_{c' \ne c} dp[i-1][c']
$$

Answer = `min_c dp[n-1][c]`.

## 🧠 Logic
Painting house `i` color `c` forbids color `c` on `i−1`, so add `costs[i][c]` to the cheapest of the **other two** colors at `i−1`. Only the previous row matters, so keep three rolling values.

## 🔢 Iteration trace (`costs = [[17,2,17],[16,16,5],[14,3,19]]`)
| i | red | blue | green |
|---|-----|------|-------|
| 0 | 17 | 2 | 17 |
| 1 | 16+min(2,17)=18 | 16+min(17,17)=33 | 5+min(17,2)=7 |
| 2 | 14+min(33,7)=21 | 3+min(18,7)=10 | 19+min(18,33)=37 |

`min(21, 10, 37) = ` **10**.

## 🐍 Python
```python
def min_cost(costs: list[list[int]]) -> int:
    r, b, g = costs[0]
    for i in range(1, len(costs)):
        cr, cb, cg = costs[i]
        r, b, g = cr + min(b, g), cb + min(r, g), cg + min(r, b)
    return min(r, b, g)


if __name__ == "__main__":
    print(min_cost([[17, 2, 17], [16, 16, 5], [14, 3, 19]]))   # 10
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minCost(vector<vector<int>>& costs) {
    int r = costs[0][0], b = costs[0][1], g = costs[0][2];
    for (size_t i = 1; i < costs.size(); ++i) {
        int nr = costs[i][0] + min(b, g);
        int nb = costs[i][1] + min(r, g);
        int ng = costs[i][2] + min(r, b);
        r = nr; b = nb; g = ng;
    }
    return min({r, b, g});
}

int main() {
    vector<vector<int>> costs = {{17, 2, 17}, {16, 16, 5}, {14, 3, 19}};
    cout << minCost(costs) << "\n";   // 10
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
