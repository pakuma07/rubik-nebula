# Count Numbers with Unique Digits

> Combinatorial / digit DP. LC 357 · 🟡 Medium

## Problem
Given `n`, count all numbers `x` with `0 ≤ x < 10^n` whose decimal digits are all **distinct**.

## 🧮 Math / Recurrence
For each length `k` (1..n) the count of distinct-digit numbers is a falling product:

$$
f(k) = 9 \times 9 \times 8 \times \dots \times (11 - k)
$$

Total $= 1 + \sum_{k=1}^{n} f(k)$ (the `1` counts `0`).

## 🧠 Logic
A `k`-digit number has 9 choices for the leading digit (1–9), then 9 for the next (0–9 minus the one used), 8, 7, … . Summing over all lengths up to `n` plus the single number `0` gives the answer. For `n ≥ 10` the count saturates (only 10 distinct digits exist). This closed form is the collapsed digit-DP.

```mermaid
flowchart LR
    A["k=1: 10"] --> B["k=2: +9·9"]
    B --> C["k=3: +9·9·8"]
    C --> D["… up to n"]
```

## 🔢 Iteration trace (`n=2`)
- 10 (length 1) + 81 (length 2) = **91**.

## 🐍 Python
```python
def count_numbers_with_unique_digits(n: int) -> int:
    if n == 0:
        return 1
    total = 10
    unique = 9
    available = 9
    for _ in range(2, n + 1):
        unique *= available
        total += unique
        available -= 1
        if available == 0:
            break
    return total


if __name__ == "__main__":
    print(count_numbers_with_unique_digits(2))   # 91
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

int countNumbersWithUniqueDigits(int n) {
    if (n == 0) return 1;
    int total = 10, unique = 9, available = 9;
    for (int i = 2; i <= n && available > 0; ++i) {
        unique *= available;
        total += unique;
        --available;
    }
    return total;
}

int main() {
    cout << countNumbersWithUniqueDigits(2) << "\n";   // 91
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
