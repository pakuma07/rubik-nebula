# Fibonacci Number (O(1) space)

> The defining linear recurrence, rolled to two variables. LC 509 · 🟢 Easy

## Problem
Return `f(n)` where `f(0)=0`, `f(1)=1`, `f(n)=f(n-1)+f(n-2)`.

## 🧮 Math / Recurrence
$$
f(n) = f(n-1) + f(n-2), \qquad f(0)=0,\ f(1)=1
$$

Closed form (Binet, for reference):

$$
f(n) = \frac{\varphi^n - \psi^n}{\sqrt5}, \quad \varphi = \frac{1+\sqrt5}{2},\ \psi = \frac{1-\sqrt5}{2}
$$

## 🧠 Logic
Each value needs only the previous two, so a full `dp` array is wasteful — keep two rolling scalars. This is the prototypical "naive recursion → memo → two variables" collapse: `O(φⁿ)` → `O(n)` → `O(n)` time with `O(1)` space.

## 🔢 Iteration trace (`n = 7`)
| n | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| f | 0 | 1 | 1 | 2 | 3 | 5 | 8 | **13** |

## 🐍 Python
```python
def fib(n: int) -> int:
    a, b = 0, 1                 # f(0), f(1)
    for _ in range(n):
        a, b = b, a + b
    return a


if __name__ == "__main__":
    print(fib(7))               # 13
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

long long fib(int n) {
    long long a = 0, b = 1;     // f(0), f(1)
    for (int i = 0; i < n; ++i) {
        long long next = a + b;
        a = b; b = next;
    }
    return a;
}

int main() {
    cout << fib(7) << "\n";     // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.

> Matrix exponentiation computes `f(n)` in `O(log n)` for very large `n`.
