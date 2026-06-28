# Student Attendance Record II

> A small finite-state machine counted with DP. LC 552 · 🔴 Hard

## Problem
Count length-`n` strings over `{A, L, P}` that are "rewardable": **fewer than 2 `A`s total** and **no 3 consecutive `L`s**. Answer mod `1e9+7`.

## 🧮 Math / Recurrence
State = `(a, l)` where `a ∈ {0,1}` = absences so far, `l ∈ {0,1,2}` = current trailing `L` streak. From each state, appending a character transitions:

$$
\text{P: } (a, l) \to (a, 0) \qquad \text{L: } (a, l) \to (a, l{+}1)\ \text{if } l<2 \qquad \text{A: } (a, l) \to (a{+}1, 0)\ \text{if } a<1
$$

Sum all 6 states after `n` steps.

## 🧠 Logic
The two constraints depend only on **bounded history**: how many `A`s total (cap 1) and how many trailing `L`s (cap 2). That makes a 6-state automaton; `dp[a][l]` counts strings landing in each state, and we push counts forward one character at a time with modular addition.

## 🔢 Iteration trace (`n = 2`, states `(a,l)`)
Start (`n=0`): only `(0,0)=1`. After 1 char:
| state | count |
|-------|-------|
| (0,0) via P | 1 |
| (0,1) via L | 1 |
| (1,0) via A | 1 |

After 2 chars, summing all reachable states gives **8** rewardable strings of length 2 (all `9` combos minus `"AA"`).

## 🐍 Python
```python
MOD = 10**9 + 7

def check_record(n: int) -> int:
    # dp[a][l]
    dp = [[0, 0, 0], [0, 0, 0]]
    dp[0][0] = 1
    for _ in range(n):
        ndp = [[0, 0, 0], [0, 0, 0]]
        for a in range(2):
            for l in range(3):
                cur = dp[a][l]
                if cur == 0:
                    continue
                ndp[a][0] = (ndp[a][0] + cur) % MOD          # append P
                if l < 2:
                    ndp[a][l + 1] = (ndp[a][l + 1] + cur) % MOD  # append L
                if a < 1:
                    ndp[a + 1][0] = (ndp[a + 1][0] + cur) % MOD  # append A
        dp = ndp
    return sum(dp[a][l] for a in range(2) for l in range(3)) % MOD


if __name__ == "__main__":
    print(check_record(2))   # 8
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;
const long long MOD = 1e9 + 7;

int checkRecord(int n) {
    long long dp[2][3] = {{1, 0, 0}, {0, 0, 0}};
    for (int t = 0; t < n; ++t) {
        long long ndp[2][3] = {{0, 0, 0}, {0, 0, 0}};
        for (int a = 0; a < 2; ++a)
            for (int l = 0; l < 3; ++l) {
                long long cur = dp[a][l];
                if (!cur) continue;
                ndp[a][0] = (ndp[a][0] + cur) % MOD;             // P
                if (l < 2) ndp[a][l + 1] = (ndp[a][l + 1] + cur) % MOD;  // L
                if (a < 1) ndp[a + 1][0] = (ndp[a + 1][0] + cur) % MOD;  // A
            }
        for (int a = 0; a < 2; ++a) for (int l = 0; l < 3; ++l) dp[a][l] = ndp[a][l];
    }
    long long ans = 0;
    for (int a = 0; a < 2; ++a) for (int l = 0; l < 3; ++l) ans = (ans + dp[a][l]) % MOD;
    return (int)ans;
}

int main() {
    cout << checkRecord(2) << "\n";   // 8
}
```

## ⏱️ Complexity
- **Time:** `O(n)` (constant 6-state work per step).
- **Space:** `O(1)`.
