# Matchsticks to Square

> K=4 equal-sum partition. LC 473 · 🟡 Medium

## Problem
Given matchstick lengths, decide whether they can form a **square** (use every stick, four equal sides).

## 🧮 Math / Recurrence
This is exactly "Partition to K Equal Sum Subsets" with `k = 4`, `target = perimeter/4`. `dp[mask]` = leftover length on the current side:

$$
dp[mask \,|\, (1 \ll j)] = (dp[mask] + len[j]) \bmod target,\quad \text{if } dp[mask] + len[j] \le target
$$

## 🧠 Logic
Each side of the square is a bucket of size `target`. `dp[mask]` tracks how much of the current side is filled; reaching `target` wraps to a new side via `% target`. If the full mask closes with leftover `0`, all four sides are complete. Early exits: perimeter not divisible by 4, or any stick longer than `target`.

## 🔢 Iteration trace (`[1,1,2,2,2]`, perimeter 8, target 2)
- Sides {2},{2},{2},{1,1} → **True**.

## 🐍 Python
```python
def makesquare(matchsticks: list[int]) -> bool:
    total = sum(matchsticks)
    if total % 4:
        return False
    target = total // 4
    n = len(matchsticks)
    matchsticks.sort(reverse=True)
    if matchsticks[0] > target:
        return False
    dp = [-1] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] == -1:
            continue
        for j in range(n):
            if not (mask >> j) & 1 and dp[mask] + matchsticks[j] <= target:
                nm = mask | (1 << j)
                dp[nm] = (dp[mask] + matchsticks[j]) % target
    return dp[(1 << n) - 1] == 0


if __name__ == "__main__":
    print(makesquare([1, 1, 2, 2, 2]))   # True
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

bool makesquare(vector<int>& matchsticks) {
    int total = accumulate(matchsticks.begin(), matchsticks.end(), 0);
    if (total % 4) return false;
    int target = total / 4, n = matchsticks.size();
    sort(matchsticks.rbegin(), matchsticks.rend());
    if (matchsticks[0] > target) return false;
    vector<int> dp(1 << n, -1);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << n); ++mask) {
        if (dp[mask] == -1) continue;
        for (int j = 0; j < n; ++j)
            if (!((mask >> j) & 1) && dp[mask] + matchsticks[j] <= target) {
                int nm = mask | (1 << j);
                dp[nm] = (dp[mask] + matchsticks[j]) % target;
            }
    }
    return dp[(1 << n) - 1] == 0;
}

int main() {
    vector<int> sticks = {1, 1, 2, 2, 2};
    cout << boolalpha << makesquare(sticks) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(2ⁿ · n)`.
- **Space:** `O(2ⁿ)`.
