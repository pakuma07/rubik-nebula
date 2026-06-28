# Number of Ways to Split a String

> Counting balanced partitions of a binary string. LC 1573 · 🟡 Medium

## Problem
Split a binary string into **three non-empty** parts with **equal numbers of `1`s**. Count the ways (mod `1e9+7`).

## 🧮 Math / Recurrence
Let `T` = total ones. If `T % 3 ≠ 0`, answer is `0`. Otherwise each part needs `T/3` ones. Let:
- `a` = number of gap positions between the **first** and **second** blocks of `T/3` ones,
- `b` = number of gap positions between the **second** and **third** blocks.

$$
\text{ans} = a \cdot b \pmod{10^9+7}
$$

**All-zeros special case** (`T = 0`): choose 2 cut points among `n−1` gaps: $\binom{n-1}{2}$.

## 🧠 Logic
The two cuts are independent: the first cut may sit anywhere in the run of zeros separating the 1st and 2nd thirds of ones, and likewise the second cut in the zeros between the 2nd and 3rd thirds. Multiplying the freedoms gives the count. When there are no ones, any two of the `n−1` gaps work.

## 🔢 Iteration trace (`s = "10101"`)
- `T = 3`, each part needs `1` one. Ones at indices `0, 2, 4`.
- Between 1st one (idx 0) and 2nd one (idx 2): zeros at idx 1 → `a = 2` cut placements (after idx 0 or after idx 1).
- Between 2nd one (idx 2) and 3rd one (idx 4): zeros at idx 3 → `b = 2`.
- `ans = a · b = 2 · 2 = ` **4**.

## 🐍 Python
```python
MOD = 10**9 + 7

def num_ways(s: str) -> int:
    ones = [i for i, c in enumerate(s) if c == "1"]
    total = len(ones)
    n = len(s)
    if total % 3 != 0:
        return 0
    if total == 0:                          # all zeros: C(n-1, 2)
        return (n - 1) * (n - 2) // 2 % MOD
    k = total // 3
    a = ones[k] - ones[k - 1]               # gap after first block
    b = ones[2 * k] - ones[2 * k - 1]       # gap after second block
    return (a * b) % MOD


if __name__ == "__main__":
    print(num_ways("10101"))   # 4
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int numWays(string s) {
    vector<int> ones;
    for (int i = 0; i < (int)s.size(); ++i) if (s[i] == '1') ones.push_back(i);
    int total = ones.size(), n = s.size();
    if (total % 3 != 0) return 0;
    if (total == 0) return (long long)(n - 1) * (n - 2) / 2 % MOD;
    int k = total / 3;
    long long a = ones[k] - ones[k - 1];
    long long b = ones[2 * k] - ones[2 * k - 1];
    return (a * b) % MOD;
}

int main() {
    cout << numWays("10101") << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)` for the positions of ones (or `O(1)` with a two-pass count).
