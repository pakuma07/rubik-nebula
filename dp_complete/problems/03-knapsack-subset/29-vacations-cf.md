# Vacations (Codeforces 698A)

> Per-day constrained selection DP. CF 698A · 🟡 Medium

## Problem
Over `n` days each day allows some of {rest, gym, contest} (encoded `0/1/2/3`). You may not do the **same activity two days in a row** (resting is always allowed). Minimize total rest days.

## 🧮 Math / Recurrence
`dp[i][a]` = min rest days through day `i` ending in activity `a ∈ {rest, gym, contest}`:

$$
dp[i][a] = \min_{b \ne a \text{ or } a=\text{rest}} dp[i-1][b] + [a = \text{rest}]
$$

only allowing `a` permitted by day `i`.

## 🧠 Logic
Each day's choice depends only on the **previous day's** activity (the no-repeat constraint), so a 3-state DP suffices. Resting always costs `+1` rest day and has no repeat restriction; gym/contest cost 0 but cannot follow the same activity. Minimizing over valid previous states gives the optimum.

## 🔢 Iteration trace (`days = [1, 3, 2, 0]`, gym=1, contest=2, both=3)
- Day0 (gym available): rest=1, gym=0.
- Day1 (both): can contest after gym → 0; gym after gym forbidden.
- Day2 (contest): rest or gym; contest after contest forbidden.
- Day3 (nothing): must rest (+1).
- Min total rest = **1**.

## 🐍 Python
```python
def min_rest_days(days: list[int]) -> int:
    INF = float("inf")
    # states: 0 rest, 1 gym, 2 contest
    dp = [0, INF, INF]
    for d in days:
        ndp = [INF, INF, INF]
        # rest always possible (+1)
        ndp[0] = min(dp) + 1
        if d & 1:                            # gym available
            ndp[1] = min(dp[0], dp[2])
        if d & 2:                            # contest available
            ndp[2] = min(dp[0], dp[1])
        dp = ndp
    return min(dp)


if __name__ == "__main__":
    print(min_rest_days([1, 3, 2, 0]))   # 1
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minRestDays(vector<int>& days) {
    const int INF = 1e9;
    vector<int> dp = {0, INF, INF};          // rest, gym, contest
    for (int d : days) {
        vector<int> ndp(3, INF);
        ndp[0] = min({dp[0], dp[1], dp[2]}) + 1;
        if (d & 1) ndp[1] = min(dp[0], dp[2]);
        if (d & 2) ndp[2] = min(dp[0], dp[1]);
        dp = ndp;
    }
    return min({dp[0], dp[1], dp[2]});
}

int main() {
    vector<int> days = {1, 3, 2, 0};
    cout << minRestDays(days) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n)` (3 states/day).
- **Space:** `O(1)`.
