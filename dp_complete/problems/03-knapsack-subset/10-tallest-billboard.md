# Tallest Billboard

> DP over the difference of two growing heights. LC 956 · 🔴 Hard

## Problem
From rods `rods`, build two **equal-height** supports (using a subset of rods, each on at most one side). Return the maximum common height (0 if impossible).

## 🧮 Math / Recurrence
State = the **difference** `d = left − right`; value = the **taller** side's height for that difference. For each rod `r`, three choices — skip, add to left, add to right:

$$
dp[d] \to dp[d+r] \ (\text{left}),\quad dp[d-r]\ (\text{right}),\quad dp[d]\ (\text{skip})
$$

Answer = `dp[0]` (equal heights).

## 🧠 Logic
Only the *gap* between the two supports matters, not their absolute heights, so we key the DP by `d`. Storing the larger side's height lets us recover the common height when `d` returns to 0. Each rod can lengthen the lead, shrink it, or be discarded. Using a dictionary keyed by difference keeps the state space small.

## 🔢 Iteration trace (`rods = [1, 2, 3, 6]`)
- Put `{1,2,3}` on one side (sum 6) and `{6}` on the other → both height 6, `d = 0`.

`dp[0] = ` **6**.

## 🐍 Python
```python
def tallest_billboard(rods: list[int]) -> int:
    dp = {0: 0}                              # diff -> taller height
    for r in rods:
        cur = dict(dp)
        for d, tall in cur.items():
            # add to taller side
            dp[d + r] = max(dp.get(d + r, 0), tall + r)
            # add to shorter side
            nd = abs(d - r)
            shorter = tall - d
            new_tall = max(tall, shorter + r)
            dp[nd] = max(dp.get(nd, 0), new_tall)
    return dp[0]


if __name__ == "__main__":
    print(tallest_billboard([1, 2, 3, 6]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <cstdlib>
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int tallestBillboard(vector<int>& rods) {
    unordered_map<int, int> dp;              // diff -> taller height
    dp[0] = 0;
    for (int r : rods) {
        unordered_map<int, int> cur = dp;
        for (auto& [d, tall] : cur) {
            dp[d + r] = max(dp.count(d + r) ? dp[d + r] : 0, tall + r);
            int nd = abs(d - r);
            int shorter = tall - d;
            int newTall = max(tall, shorter + r);
            dp[nd] = max(dp.count(nd) ? dp[nd] : 0, newTall);
        }
    }
    return dp[0];
}

int main() {
    vector<int> rods = {1, 2, 3, 6};
    cout << tallestBillboard(rods) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n · S)` where `S = Σrods` (number of reachable differences).
- **Space:** `O(S)`.
