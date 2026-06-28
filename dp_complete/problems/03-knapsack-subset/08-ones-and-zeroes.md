# Ones and Zeroes

> Knapsack with two simultaneous capacities. LC 474 · 🟡 Medium

## Problem
Given binary strings `strs` and budgets `m` zeros and `n` ones, return the size of the largest subset you can form without exceeding either budget.

## 🧮 Math / Recurrence
Each string costs `(z, o)` = its zero/one counts. With two capacities:

$$
dp[z][o] = \max\big(dp[z][o],\ dp[z - z_i][o - o_i] + 1\big)
$$

both capacity dimensions looped **decreasing** (0/1).

## 🧠 Logic
It is a 0/1 knapsack whose "weight" is two-dimensional — consuming both a zero-budget and a one-budget. Each string is taken once (`+1` to the count) or skipped. Because each item has two costs, the DP table is 2D and both loops run downward to avoid reuse.

## 🔢 Iteration trace (`strs=["10","0001","1","0"]`, `m=5, n=3`)
Greedily the optimal subset is `{"10","0001","1","0"}` — wait, that needs 5 zeros? `"10"`(1z,1o)+`"0001"`(3z,1o)+`"1"`(0z,1o)+`"0"`(1z,0o) = 5z,3o ✓ → size **4**.

## 🐍 Python
```python
def find_max_form(strs: list[str], m: int, n: int) -> int:
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for s in strs:
        zeros = s.count("0")
        ones = len(s) - zeros
        for z in range(m, zeros - 1, -1):
            for o in range(n, ones - 1, -1):
                dp[z][o] = max(dp[z][o], dp[z - zeros][o - ones] + 1)
    return dp[m][n]


if __name__ == "__main__":
    print(find_max_form(["10", "0001", "1", "0"], 5, 3))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int findMaxForm(vector<string>& strs, int m, int n) {
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (auto& s : strs) {
        int zeros = count(s.begin(), s.end(), '0');
        int ones = s.size() - zeros;
        for (int z = m; z >= zeros; --z)
            for (int o = n; o >= ones; --o)
                dp[z][o] = max(dp[z][o], dp[z - zeros][o - ones] + 1);
    }
    return dp[m][n];
}

int main() {
    vector<string> strs = {"10", "0001", "1", "0"};
    cout << findMaxForm(strs, 5, 3) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(L · m · n)` for `L` strings.
- **Space:** `O(m·n)`.
