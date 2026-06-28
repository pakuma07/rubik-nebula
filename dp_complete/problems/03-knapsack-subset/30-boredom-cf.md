# Boredom (Codeforces 455A)

> Delete-and-earn / House Robber on value counts. CF 455A · 🟡 Medium

## Problem
Repeatedly pick a value `v`: gain `v` points and **delete all elements equal to `v−1` and `v+1`**. Maximize total points.

## 🧮 Math / Recurrence
Bucket points by value: `gain[v] = v · count(v)`. Choosing `v` forbids `v±1`, the non-adjacency rule on the value axis:

$$
dp[v] = \max\big(dp[v-1],\ dp[v-2] + gain[v]\big)
$$

## 🧠 Logic
This is identical to [Delete and Earn](../02-linear-dp/10-delete-and-earn.md): the conflict is between **adjacent values**, so collapse duplicates into per-value earnings and run House Robber over `0..max`. Taking value `v` means skipping `v−1`, so fall back to `dp[v−2]`.

## 🔢 Iteration trace (`a = [1, 2, 3]`)
- `gain = {1:1, 2:2, 3:3}`.
| v | gain | rob | skip | dp |
|---|------|-----|------|----|
| 1 | 1 | 1 | 0 | 1 |
| 2 | 2 | 0+2 | 1 | 2 |
| 3 | 3 | 1+3 | 2 | **4** |

Take 1 and 3 (skip 2) ⟹ **4**.

## 🐍 Python
```python
def boredom(a: list[int]) -> int:
    if not a:
        return 0
    top = max(a)
    gain = [0] * (top + 1)
    for x in a:
        gain[x] += x
    prev2, prev1 = 0, 0
    for v in range(top + 1):
        prev2, prev1 = prev1, max(prev1, prev2 + gain[v])
    return prev1


if __name__ == "__main__":
    print(boredom([1, 2, 3]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

long long boredom(vector<int>& a) {
    if (a.empty()) return 0;
    int top = *max_element(a.begin(), a.end());
    vector<long long> gain(top + 1, 0);
    for (int x : a) gain[x] += x;
    long long prev2 = 0, prev1 = 0;
    for (int v = 0; v <= top; ++v) {
        long long cur = max(prev1, prev2 + gain[v]);
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int main() {
    vector<int> a = {1, 2, 3};
    cout << boredom(a) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n + max)`.
- **Space:** `O(max)`.
