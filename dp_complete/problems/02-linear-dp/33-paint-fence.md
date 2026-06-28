# Paint Fence

> Split into "same as previous" vs "different" counts. LC 276 · 🟡 Medium

## Problem
Paint `n` posts with `k` colors so that **no three consecutive posts** share the same color. Count the ways.

## 🧮 Math / Recurrence
Track `same[i]` (post `i` equals `i−1`) and `diff[i]` (post `i` differs from `i−1`):

$$
same[i] = diff[i-1], \qquad diff[i] = (same[i-1] + diff[i-1]) \cdot (k-1)
$$

Total = `same[n] + diff[n]`. Base: `same[1]=0`, `diff[1]=k`.

## 🧠 Logic
You may repeat a color only if the prior pair *differed* (else three in a row), so `same[i] = diff[i−1]`. To differ from post `i−1`, pick any of the `k−1` other colors regardless of the earlier history: `diff[i] = total[i−1]·(k−1)`. Two rolling counts suffice.

## 🔢 Iteration trace (`n = 3, k = 2`)
| i | same | diff | total |
|---|------|------|-------|
| 1 | 0 | 2 | 2 |
| 2 | 2 | (0+2)·1=2 | 4 |
| 3 | 2 | (2+2)·1=4 | **6** |

**Answer = 6** (the only forbidden patterns of `2³=8` are `AAA`, `BBB`).

## 🐍 Python
```python
def num_ways(n: int, k: int) -> int:
    if n == 0:
        return 0
    if n == 1:
        return k
    same, diff = 0, k                       # post 1
    for _ in range(2, n + 1):
        same, diff = diff, (same + diff) * (k - 1)
    return same + diff


if __name__ == "__main__":
    print(num_ways(3, 2))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

int numWays(int n, int k) {
    if (n == 0) return 0;
    if (n == 1) return k;
    long long same = 0, diff = k;
    for (int i = 2; i <= n; ++i) {
        long long ndiff = (same + diff) * (k - 1);
        same = diff;
        diff = ndiff;
    }
    return (int)(same + diff);
}

int main() {
    cout << numWays(3, 2) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
