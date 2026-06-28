# GCD (Euclid's Algorithm)

> Greatest common divisor via repeated remainder. Classic · 🟢 Easy

## Problem
Given non-negative integers `a` and `b` (not both zero), return their greatest common divisor.

## 🧮 Math / Recurrence
$$
\gcd(a, b) = \begin{cases}
a & b = 0 \\
\gcd(b,\ a \bmod b) & b > 0
\end{cases}
$$

**Why it works:** any common divisor of `a` and `b` also divides `a − qb = a mod b`, so the set of common divisors (and hence the *greatest*) is preserved at each step. The arguments shrink fast — the worst case is consecutive Fibonacci numbers — giving:

$$
T(a,b) = O(\log \min(a,b))
$$

## 🧠 Logic
Replace the larger number by the remainder of dividing it by the smaller. The remainder is strictly smaller than `b`, so the pair keeps shrinking until `b` hits `0`, at which point `a` is the answer. No multiplication or factorization needed.

## 🔢 Iteration trace (`gcd(48, 18)`)
| Call | a | b | a mod b |
|------|---|---|---------|
| `gcd(48,18)` | 48 | 18 | 12 |
| `gcd(18,12)` | 18 | 12 | 6 |
| `gcd(12,6)`  | 12 | 6  | 0 |
| `gcd(6,0)`   | 6  | 0  | base |

**Answer = 6.**

## 🐍 Python
```python
def gcd(a: int, b: int) -> int:
    if b == 0:
        return a
    return gcd(b, a % b)


if __name__ == "__main__":
    print(gcd(48, 18))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

long long gcd(long long a, long long b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}

int main() {
    cout << gcd(48, 18) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(log min(a, b))`.
- **Space:** `O(log min(a, b))` recursion stack (or `O(1)` iteratively).

> `lcm(a, b) = a / gcd(a, b) * b` builds directly on this.
