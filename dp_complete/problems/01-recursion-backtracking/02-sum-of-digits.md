# Sum of Digits

> Add the decimal digits of `n` recursively. Classic · 🟢 Easy

## Problem
Given a non-negative integer `n`, return the sum of its decimal digits (e.g. `1234 → 1+2+3+4 = 10`).

## 🧮 Math / Recurrence
$$
S(n) = \begin{cases} 0 & n = 0 \\ (n \bmod 10) + S\!\left(\left\lfloor n/10 \right\rfloor\right) & n > 0 \end{cases}
$$

`n mod 10` peels off the **last** digit; `⌊n/10⌋` removes it. Each step shortens the number by one digit, so the recursion depth is the digit count $\lfloor \log_{10} n \rfloor + 1$.

## 🧠 Logic
Any number is "its last digit" plus "everything to the left of it." Recursion expresses exactly that: take `n % 10` now, and trust the recursive call to sum the rest `n // 10`. The base case `n = 0` contributes nothing.

## 🔢 Iteration trace (`n = 1234`)
| Call | `n % 10` | recurse on |
|------|----------|------------|
| `S(1234)` | 4 | `S(123)` |
| `S(123)`  | 3 | `S(12)` |
| `S(12)`   | 2 | `S(1)` |
| `S(1)`    | 1 | `S(0)` |
| `S(0)`    | — | `0` (base) |

Unwinding: `0 → 1 → 3 → 6 → 10`. **Answer = 10.**

## 🐍 Python
```python
def sum_of_digits(n: int) -> int:
    if n == 0:
        return 0
    return (n % 10) + sum_of_digits(n // 10)


if __name__ == "__main__":
    print(sum_of_digits(1234))   # 10
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

int sumOfDigits(long long n) {
    if (n == 0) return 0;
    return (int)(n % 10) + sumOfDigits(n / 10);
}

int main() {
    cout << sumOfDigits(1234) << "\n";   // 10
}
```

## ⏱️ Complexity
- **Time:** `O(log₁₀ n)` — one step per digit.
- **Space:** `O(log₁₀ n)` recursion stack.

> Edge case: `n = 0` returns `0` directly via the base case.
