# Pow(x, n) with Negative Exponents

> Fast power extended to handle `n < 0`. LC 50 · 🟡 Medium

## Problem
Compute `xⁿ` for any integer exponent `n`, including negatives (e.g. `2⁻³ = 0.125`).

## 🧮 Math / Recurrence
Reduce negatives to the positive case via the reciprocal, then apply [fast exponentiation](03-power-x-n.md):

$$
x^n = \begin{cases}
\dfrac{1}{x^{-n}} & n < 0 \\[6pt]
\left(x^{n/2}\right)^2 & n \ge 0,\ n \text{ even} \\[4pt]
x \cdot \left(x^{\lfloor n/2 \rfloor}\right)^2 & n \ge 0,\ n \text{ odd}
\end{cases}
$$

## 🧠 Logic
Negative exponents mean division: `x⁻ⁿ = 1 / xⁿ`. So flip the sign, compute the positive power in `O(log n)`, and take the reciprocal once at the end. The only subtlety is overflow when negating `INT_MIN` — promote to a 64-bit (`long`) exponent before negating.

## 🔢 Iteration trace (`x = 2, n = -3`)
1. `n < 0` → compute `pow(2, 3)` then return `1 / result`.
2. `pow(2,3) = 2 · pow(2,1)² = 2 · 4 = 8`.
3. Final: `1 / 8 = 0.125`.

| Stage | Expression | Value |
|-------|------------|-------|
| sign fix | `1 / pow(2, 3)` | — |
| fast power | `pow(2,3)` | 8 |
| reciprocal | `1 / 8` | **0.125** |

## 🐍 Python
```python
def my_pow(x: float, n: int) -> float:
    if n < 0:
        x, n = 1 / x, -n      # reciprocal handles negatives
    result = 1.0
    while n:
        if n & 1:
            result *= x
        x *= x
        n >>= 1
    return result


if __name__ == "__main__":
    print(my_pow(2, -3))      # 0.125
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

double myPow(double x, int n) {
    long long e = n;          // widen to avoid INT_MIN overflow
    if (e < 0) { x = 1 / x; e = -e; }
    double result = 1.0;
    while (e) {
        if (e & 1) result *= x;
        x *= x;
        e >>= 1;
    }
    return result;
}

int main() {
    cout << myPow(2, -3) << "\n";   // 0.125
}
```

## ⏱️ Complexity
- **Time:** `O(log |n|)`.
- **Space:** `O(1)` (iterative) or `O(log |n|)` (recursive).

> ⚠️ Widen `n` to `long long` *before* negating to dodge the `INT_MIN` overflow trap.
