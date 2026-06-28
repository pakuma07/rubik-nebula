# Number of Ways to Wear Different Hats

> Iterate hats, mask over people. LC 1434 · 🔴 Hard

## Problem
`n` people each have a list of acceptable hats (40 types). Count the ways to assign hats so everyone gets a **different** hat, modulo `10^9 + 7`.

## 🧮 Math / Recurrence
Invert the assignment: iterate hats `1..40`, masking which **people** are covered. `dp[mask]` = ways to dress the people in `mask`:

$$
dp[h][mask \,|\, (1 \ll p)] \mathrel{+}= dp[h-1][mask] \quad \text{for each person } p \text{ who likes hat } h
$$

## 🧠 Logic
Masking over people (≤10) instead of hats (≤40) keeps the state at `2¹⁰`. Each hat is offered to at most one person; processing hats one at a time, a person may take hat `h` only if currently uncovered. Carrying forward `dp[mask]` (skip the hat) plus assignments fills all subsets.

## 🔢 Iteration trace (`[[3,4],[4,5],[5]]`)
- Valid distinct assignments → **1**.

## 🐍 Python
```python
def number_ways(hats: list[list[int]]) -> int:
    MOD = 10**9 + 7
    n = len(hats)
    hat_to_people: list[list[int]] = [[] for _ in range(41)]
    for person, hl in enumerate(hats):
        for h in hl:
            hat_to_people[h].append(person)
    full = (1 << n) - 1
    dp = [0] * (1 << n)
    dp[0] = 1
    for h in range(1, 41):
        nxt = dp[:]
        for mask in range(1 << n):
            if dp[mask] == 0:
                continue
            for p in hat_to_people[h]:
                if not (mask >> p) & 1:
                    nm = mask | (1 << p)
                    nxt[nm] = (nxt[nm] + dp[mask]) % MOD
        dp = nxt
    return dp[full]


if __name__ == "__main__":
    print(number_ways([[3, 4], [4, 5], [5]]))   # 1
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int numberWays(vector<vector<int>>& hats) {
    int n = hats.size();
    vector<vector<int>> hatToPeople(41);
    for (int p = 0; p < n; ++p)
        for (int h : hats[p]) hatToPeople[h].push_back(p);
    int full = (1 << n) - 1;
    vector<long long> dp(1 << n, 0);
    dp[0] = 1;
    for (int h = 1; h <= 40; ++h) {
        vector<long long> nxt = dp;
        for (int mask = 0; mask <= full; ++mask) {
            if (!dp[mask]) continue;
            for (int p : hatToPeople[h])
                if (!((mask >> p) & 1)) {
                    int nm = mask | (1 << p);
                    nxt[nm] = (nxt[nm] + dp[mask]) % MOD;
                }
        }
        dp = nxt;
    }
    return (int)dp[full];
}

int main() {
    vector<vector<int>> hats = {{3, 4}, {4, 5}, {5}};
    cout << numberWays(hats) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(40 · 2ⁿ · n)`.
- **Space:** `O(2ⁿ)`.
