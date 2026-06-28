# Get Maximum in Generated Array

> A directed, rule-based recurrence (not a plain sum). LC 1646 · 🟢 Easy

## Problem
Build array `a` of length `n+1`: `a[0]=0`, `a[1]=1`, and for `2 ≤ 2i ≤ n`: `a[2i]=a[i]`, `a[2i+1]=a[i]+a[i+1]`. Return `max(a)`.

## 🧮 Math / Recurrence
$$
a[2i] = a[i], \qquad a[2i+1] = a[i] + a[i+1]
$$

Fill indices in increasing order; every right-hand index `i, i+1` is `< 2i`, so it is already known.

## 🧠 Logic
Unlike Fibonacci this recurrence is **indexed by halving**: even positions copy a smaller index, odd positions add two smaller indices. Because each rule references strictly smaller indices, a single left-to-right pass fills the whole array; track the running max as you go.

## 🔢 Iteration trace (`n = 7`)
| i | rule | value |
|---|------|-------|
| 0 | base | 0 |
| 1 | base | 1 |
| 2 | a[2]=a[1] | 1 |
| 3 | a[3]=a[1]+a[2] | 2 |
| 4 | a[4]=a[2] | 1 |
| 5 | a[5]=a[2]+a[3] | 3 |
| 6 | a[6]=a[3] | 2 |
| 7 | a[7]=a[3]+a[4] | 3 |

`max = ` **3**.

## 🐍 Python
```python
def get_maximum_generated(n: int) -> int:
    if n == 0:
        return 0
    a = [0] * (n + 1)
    a[1] = 1
    for i in range(2, n + 1):
        a[i] = a[i // 2] if i % 2 == 0 else a[i // 2] + a[i // 2 + 1]
    return max(a)


if __name__ == "__main__":
    print(get_maximum_generated(7))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int getMaximumGenerated(int n) {
    if (n == 0) return 0;
    vector<int> a(n + 1, 0);
    a[1] = 1;
    int best = 1;
    for (int i = 2; i <= n; ++i) {
        a[i] = (i % 2 == 0) ? a[i / 2] : a[i / 2] + a[i / 2 + 1];
        best = max(best, a[i]);
    }
    return best;
}

int main() {
    cout << getMaximumGenerated(7) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)` for the generated array.
