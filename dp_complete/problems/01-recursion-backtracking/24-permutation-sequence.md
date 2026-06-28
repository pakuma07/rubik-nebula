# Permutation Sequence

> The k-th permutation of `1…n` without listing them all. LC 60 · 🔴 Hard

## Problem
Return the `k`-th permutation (1-indexed) of the numbers `1…n` in lexicographic order. For `n=3, k=3`: `"213"`.

## 🧮 Math / Recurrence
Use the **factorial number system**. Fixing the first digit accounts for `(n−1)!` permutations, so:

$$
\text{index of first element} = \left\lfloor \frac{k-1}{(n-1)!} \right\rfloor, \qquad
k \leftarrow (k-1) \bmod (n-1)!
$$

Repeat on the shrinking pool of remaining digits.

## 🧠 Logic
Listing all `n!` permutations is wasteful. Instead, count how many permutations each leading digit "skips." With `(n−1)!` permutations per first-digit choice, integer division picks the correct first digit directly; the remainder selects within the rest. Remove the used digit and recurse with `n−1`.

## 🔢 Iteration trace (`n=3, k=3`, pool `[1,2,3]`)
`(n-1)! = 2!` = 2. Work with `k-1 = 2`.
| step | block size | idx = k//size | digit | new k |
|------|-----------|---------------|-------|-------|
| 1 | 2 | 2//2 = 1 | pool[1]=**2** | 2%2 = 0 |
| 2 | 1 | 0//1 = 0 | pool[0]=**1** | 0 |
| 3 | — | last | pool[0]=**3** | — |

**Answer = `"213"`.**

## 🐍 Python
```python
from math import factorial


def get_permutation(n: int, k: int) -> str:
    pool = list(range(1, n + 1))
    k -= 1                                   # 0-indexed
    res = []
    for size in range(n - 1, -1, -1):
        f = factorial(size)
        idx, k = divmod(k, f)
        res.append(str(pool.pop(idx)))
    return "".join(res)


if __name__ == "__main__":
    print(get_permutation(3, 3))   # "213"
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string getPermutation(int n, int k) {
    vector<int> fact(n + 1, 1);
    for (int i = 1; i <= n; ++i) fact[i] = fact[i - 1] * i;
    vector<int> pool;
    for (int i = 1; i <= n; ++i) pool.push_back(i);

    --k;                                     // 0-indexed
    string res;
    for (int size = n - 1; size >= 0; --size) {
        int idx = k / fact[size];
        k %= fact[size];
        res += char('0' + pool[idx]);
        pool.erase(pool.begin() + idx);
    }
    return res;
}

int main() {
    cout << getPermutation(3, 3) << "\n";   // 213
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` — `n` picks, each an `O(n)` erase from the pool.
- **Space:** `O(n)` for the pool and factorials.
