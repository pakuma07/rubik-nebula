# N-th Tribonacci Number

> Fibonacci with a window of three. LC 1137 · 🟢 Easy

## Problem
`T(0)=0, T(1)=1, T(2)=1`, and `T(n)=T(n-1)+T(n-2)+T(n-3)`. Return `T(n)`.

## 🧮 Math / Recurrence
$$
T(n) = T(n-1) + T(n-2) + T(n-3), \qquad T(0)=0,\ T(1)=T(2)=1
$$

## 🧠 Logic
Same collapse as Fibonacci, but the sliding window now holds **three** values. Keep `a, b, c` and shift each step: `a, b, c = b, c, a+b+c`.

## 🔢 Iteration trace (`n = 6`)
| n | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|---|
| T | 0 | 1 | 1 | 2 | 4 | 7 | **13** |

## 🐍 Python
```python
def tribonacci(n: int) -> int:
    if n == 0:
        return 0
    a, b, c = 0, 1, 1           # T(0), T(1), T(2)
    for _ in range(n - 2):
        a, b, c = b, c, a + b + c
    return c if n >= 2 else b


if __name__ == "__main__":
    print(tribonacci(6))        # 13
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

long long tribonacci(int n) {
    if (n == 0) return 0;
    long long a = 0, b = 1, c = 1;
    for (int i = 0; i < n - 2; ++i) {
        long long next = a + b + c;
        a = b; b = c; c = next;
    }
    return n >= 2 ? c : b;
}

int main() {
    cout << tribonacci(6) << "\n";   // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)` with three rolling variables.
