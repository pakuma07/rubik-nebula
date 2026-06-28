# Pow(x, n) — Exponentiation by Squaring

> The divide-and-conquer fast power. LC 50 · 🟡 Medium

## Problem
Implement `pow(x, n)` computing `xⁿ` for integer `n` (positive, zero, or negative) in `O(log n)`.

## 🧮 Math / Recurrence
$$
x^n = \begin{cases}
1 & n = 0 \\
\left(x^{n/2}\right)^2 & n > 0,\ n \text{ even} \\
x \cdot \left(x^{\lfloor n/2 \rfloor}\right)^2 & n > 0,\ n \text{ odd} \\
1 / x^{-n} & n < 0
\end{cases}
$$

This is the same identity as problems [03](03-power-x-n.md) and [09](09-pow-with-negatives.md), framed here as **divide & conquer**: split the exponent in half, solve once, combine by squaring.

## 🧠 Logic
"Divide" the exponent by 2, "conquer" the subproblem `x^(n/2)`, and "combine" by squaring (plus one extra `x` if `n` is odd). Each level halves `n`, so the depth — and the number of multiplications — is `log n`. Negative exponents reduce to the reciprocal of the positive case.

## 🔢 Iteration trace (`x=3, n=5`)
| call | n | combine |
|------|---|---------|
| `pow(3,5)` | 5 odd | `3 · pow(3,2)²` |
| `pow(3,2)` | 2 even | `pow(3,1)²` |
| `pow(3,1)` | 1 odd | `3 · pow(3,0)²` |
| `pow(3,0)` | 0 | `1` |

Unwind: `1 → 3 → 9 → 243`. **Answer = 243.**

## 🐍 Python
```python
def my_pow(x: float, n: int) -> float:
    if n < 0:
        x, n = 1 / x, -n
    if n == 0:
        return 1.0
    half = my_pow(x, n // 2)
    sq = half * half
    return sq * x if n % 2 else sq


if __name__ == "__main__":
    print(my_pow(3, 5))   # 243.0
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

double myPow(double x, long long n) {
    if (n < 0) { x = 1 / x; n = -n; }
    if (n == 0) return 1.0;
    double half = myPow(x, n / 2);
    double sq = half * half;
    return (n % 2) ? sq * x : sq;
}

int main() {
    cout << myPow(3, 5) << "\n";   // 243
}
```

## ⏱️ Complexity
- **Time:** `O(log |n|)`.
- **Space:** `O(log |n|)` recursion (or `O(1)` iterative).
