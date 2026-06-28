# Decode Ways II (Wildcards)

> Decode Ways where `*` expands to digits 1–9. LC 639 · 🔴 Hard

## Problem
Like [Decode Ways](19-decode-ways.md), but `*` is a wildcard standing for any digit `1..9`. Count decodings mod `1e9+7`.

## 🧮 Math / Recurrence
Same two-term shape, but each term is **multiplied by how many concrete characters the wildcard represents**:

$$
dp[i] = \text{single}(s_i)\cdot dp[i-1] + \text{pair}(s_{i-1}, s_i)\cdot dp[i-2]
$$

- `single('*') = 9`, `single('0') = 0`, otherwise `1`.
- `pair` counts two-digit codes in `10..26` consistent with the (possibly wildcard) characters — e.g. `pair('*','*') = 15` (codes 11–19 and 21–26).

## 🧠 Logic
Decode Ways adds two indicator-gated terms; here each indicator becomes a **multiplicity** equal to the number of valid digit assignments the wildcard(s) allow. Enumerate the small set of cases for `single` and `pair`, then accumulate with modular arithmetic.

## 🔢 Iteration trace (`s = "1*"`)
- `single('*') = 9` ⟹ first term `9 · dp[i−1] = 9·1 = 9`.
- `pair('1','*')`: codes `11..19` are all ≤ 26 ⟹ `9` ⟹ second term `9 · dp[i−2] = 9·1 = 9`.
- `dp = 9 + 9 = ` **18**.

## 🐍 Python
```python
MOD = 10**9 + 7

def num_decodings(s: str) -> int:
    def single(c: str) -> int:
        if c == "*":
            return 9
        return 0 if c == "0" else 1

    def pair(a: str, b: str) -> int:
        if a == "*" and b == "*":
            return 15                       # 11-19, 21-26
        if a == "*":
            return 2 if b <= "6" else 1     # 1b and 2b (if b<=6)
        if b == "*":
            if a == "1": return 9           # 11-19
            if a == "2": return 6           # 21-26
            return 0
        return 1 if 10 <= int(a + b) <= 26 else 0

    prev2, prev1 = 1, single(s[0])
    for i in range(1, len(s)):
        cur = (single(s[i]) * prev1 + pair(s[i - 1], s[i]) * prev2) % MOD
        prev2, prev1 = prev1, cur
    return prev1


if __name__ == "__main__":
    print(num_decodings("1*"))   # 18
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
using namespace std;
const long long MOD = 1e9 + 7;

int single(char c) {
    if (c == '*') return 9;
    return c == '0' ? 0 : 1;
}
int pairCount(char a, char b) {
    if (a == '*' && b == '*') return 15;
    if (a == '*') return b <= '6' ? 2 : 1;
    if (b == '*') { if (a == '1') return 9; if (a == '2') return 6; return 0; }
    int v = (a - '0') * 10 + (b - '0');
    return (v >= 10 && v <= 26) ? 1 : 0;
}

int numDecodings(string s) {
    long long prev2 = 1, prev1 = single(s[0]);
    for (size_t i = 1; i < s.size(); ++i) {
        long long cur = (single(s[i]) * prev1 + (long long)pairCount(s[i - 1], s[i]) * prev2) % MOD;
        prev2 = prev1; prev1 = cur;
    }
    return (int)prev1;
}

int main() {
    cout << numDecodings("1*") << "\n";   // 18
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
