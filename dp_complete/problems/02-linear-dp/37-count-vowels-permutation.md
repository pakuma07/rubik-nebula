# Count Vowels Permutation

> Per-vowel state machine summed over allowed predecessors. LC 1220 · 🟡 Medium

## Problem
Count strings of length `n` over vowels `{a, e, i, o, u}` obeying the follow-rules: `a→e`; `e→a|i`; `i→` any but `i`; `o→i|u`; `u→a`. Answer mod `1e9+7`.

## 🧮 Math / Recurrence
Let `dp[v]` = number of valid strings ending in vowel `v`. Invert the follow-rules to find each vowel's allowed **predecessors**:

$$
\begin{aligned}
a' &= e + i + u \\
e' &= a + i \\
i' &= e + o \\
o' &= i \\
u' &= i + o
\end{aligned}
$$

Total = sum of all five after `n` steps; base (`n=1`) all ones.

## 🧠 Logic
The constraint only couples **adjacent** vowels, so the count ending in each vowel is the sum of counts ending in vowels that may legally precede it. Inverting "x→y" rules into "who can come before y" yields the five transitions above, applied `n−1` times.

## 🔢 Iteration trace (`n = 2`)
Start (`n=1`): `a=e=i=o=u=1` (total 5).
| vowel | predecessors | n=2 value |
|-------|--------------|-----------|
| a | e+i+u | 3 |
| e | a+i | 2 |
| i | e+o | 2 |
| o | i | 1 |
| u | i+o | 2 |

Total = `3+2+2+1+2 = ` **10**.

## 🐍 Python
```python
MOD = 10**9 + 7

def count_vowel_permutation(n: int) -> int:
    a = e = i = o = u = 1
    for _ in range(n - 1):
        a, e, i, o, u = (
            (e + i + u) % MOD,
            (a + i) % MOD,
            (e + o) % MOD,
            i % MOD,
            (i + o) % MOD,
        )
    return (a + e + i + o + u) % MOD


if __name__ == "__main__":
    print(count_vowel_permutation(2))   # 10
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;
const long long MOD = 1e9 + 7;

int countVowelPermutation(int n) {
    long long a = 1, e = 1, i = 1, o = 1, u = 1;
    for (int t = 1; t < n; ++t) {
        long long na = (e + i + u) % MOD;
        long long ne = (a + i) % MOD;
        long long ni = (e + o) % MOD;
        long long no = i % MOD;
        long long nu = (i + o) % MOD;
        a = na; e = ne; i = ni; o = no; u = nu;
    }
    return (int)((a + e + i + o + u) % MOD);
}

int main() {
    cout << countVowelPermutation(2) << "\n";   // 10
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
