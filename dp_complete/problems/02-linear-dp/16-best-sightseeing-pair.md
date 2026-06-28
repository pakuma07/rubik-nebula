# Best Sightseeing Pair

> Running-best DP for a separable score. LC 1014 · 🟡 Medium

## Problem
For spots `values`, the score of pair `i < j` is `values[i] + values[j] + i − j`. Return the maximum score.

## 🧮 Math / Recurrence
Split the score into an `i`-part and a `j`-part:

$$
\text{score}(i, j) = \underbrace{(values[i] + i)}_{\text{depends on } i} + \underbrace{(values[j] - j)}_{\text{depends on } j}
$$

Maintain the best left term seen so far:

$$
best\_i = \max_{k < j}(values[k] + k), \qquad ans = \max_j\big(best\_i + values[j] - j\big)
$$

## 🧠 Logic
Because the score **separates** into an `i`-only and `j`-only term, you never need a double loop: as you sweep `j`, keep the maximum `values[i]+i` from the left, then combine with `values[j]−j`. Update the running best after using it.

## 🔢 Iteration trace (`values = [8, 1, 5, 2, 6]`)
| j | values[j] | best_i (max of values[i]+i, i<j) | score = best_i + values[j]−j |
|---|-----------|----------------------------------|------------------------------|
| 1 | 1 | 8+0=8 | 8 + 1 − 1 = 8 |
| 2 | 5 | max(8, 1+1)=8 | 8 + 5 − 2 = **11** |
| 3 | 2 | max(8, 5+2)=8 | 8 + 2 − 3 = 7 |
| 4 | 6 | max(8, 2+3)=8 | 8 + 6 − 4 = 10 |

**Answer = 11** (pair `i=0, j=2`).

## 🐍 Python
```python
def max_score_sightseeing_pair(values: list[int]) -> int:
    best_i = values[0] + 0
    ans = float("-inf")
    for j in range(1, len(values)):
        ans = max(ans, best_i + values[j] - j)
        best_i = max(best_i, values[j] + j)
    return ans


if __name__ == "__main__":
    print(max_score_sightseeing_pair([8, 1, 5, 2, 6]))   # 11
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int maxScoreSightseeingPair(vector<int>& values) {
    int bestI = values[0] + 0, ans = INT_MIN;
    for (size_t j = 1; j < values.size(); ++j) {
        ans = max(ans, bestI + values[j] - (int)j);
        bestI = max(bestI, values[j] + (int)j);
    }
    return ans;
}

int main() {
    vector<int> values = {8, 1, 5, 2, 6};
    cout << maxScoreSightseeingPair(values) << "\n";   // 11
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
