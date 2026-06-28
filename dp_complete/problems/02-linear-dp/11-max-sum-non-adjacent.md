# Maximum Sum of Non-Adjacent Elements

> The pure House Robber recurrence (Stickler Thief). GFG · 🟡 Medium

## Problem
Pick a subset of array elements with **no two adjacent** so the sum is maximized.

## 🧮 Math / Recurrence
$$
dp[i] = \max\big(dp[i-1],\ dp[i-2] + a[i]\big), \qquad dp[0]=a[0],\ dp[1]=\max(a[0],a[1])
$$

Identical to [House Robber](07-house-robber.md).

## 🧠 Logic
Each element is either taken (then skip its neighbor, add to `dp[i−2]`) or left (carry `dp[i−1]`). The maximum of those two choices propagates forward; two scalars give `O(1)` space.

## 🔢 Iteration trace (`a = [5, 5, 10, 100, 10, 5]`)
| i | a[i] | rob | skip | dp[i] |
|---|------|-----|------|-------|
| 0 | 5    | –   | –    | 5 |
| 1 | 5    | –   | –    | 5 |
| 2 | 10   | 5+10=15 | 5 | 15 |
| 3 | 100  | 5+100=105 | 15 | **105** |
| 4 | 10   | 15+10=25 | 105 | 105 |
| 5 | 5    | 105+5=110 | 105 | **110** |

**Answer = 110** (pick 5, 100, 5).

## 🐍 Python
```python
def max_non_adjacent(a: list[int]) -> int:
    prev2, prev1 = 0, 0
    for x in a:
        prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1


if __name__ == "__main__":
    print(max_non_adjacent([5, 5, 10, 100, 10, 5]))   # 110
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxNonAdjacent(vector<int>& a) {
    int prev2 = 0, prev1 = 0;
    for (int x : a) {
        int cur = max(prev1, prev2 + x);
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int main() {
    vector<int> a = {5, 5, 10, 100, 10, 5};
    cout << maxNonAdjacent(a) << "\n";   // 110
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
