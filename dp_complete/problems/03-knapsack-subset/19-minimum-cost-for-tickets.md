# Minimum Cost For Tickets

> DP over travel days with 1/7/30-day passes. LC 983 · 🟡 Medium

## Problem
You travel on the days in `days[]`. Passes cost `costs = [c1, c7, c30]` covering 1, 7, or 30 consecutive days. Minimize the total cost to cover all travel days.

## 🧮 Math / Recurrence
Let `dp[d]` = min cost to cover all travel up to day `d`. On a travel day:

$$
dp[d] = \min\big(dp[d-1] + c_1,\ dp[d-7] + c_7,\ dp[d-30] + c_{30}\big)
$$

On a non-travel day `dp[d] = dp[d-1]` (use `dp[max(0, d-k)]`).

## 🧠 Logic
Each pass bought on day `d` "jumps back" a different number of days, so the cost to be covered through `d` is the cheapest of: extend a 1-day pass from yesterday, a 7-day pass from a week ago, or a 30-day pass from a month ago. Non-travel days inherit the previous cost for free.

## 🔢 Iteration trace (`days=[1,4,6,7,8,20], costs=[2,7,15]`)
- Through day 8, buying a 7-day pass (cost 7) covers days 1–8 better than singles.
- Day 20 adds a single (cost 2).
- Total = `7 + ... = ` **11**.

## 🐍 Python
```python
def mincost_tickets(days: list[int], costs: list[int]) -> int:
    travel = set(days)
    last = days[-1]
    dp = [0] * (last + 1)
    for d in range(1, last + 1):
        if d not in travel:
            dp[d] = dp[d - 1]
        else:
            dp[d] = min(
                dp[d - 1] + costs[0],
                dp[max(0, d - 7)] + costs[1],
                dp[max(0, d - 30)] + costs[2],
            )
    return dp[last]


if __name__ == "__main__":
    print(mincost_tickets([1, 4, 6, 7, 8, 20], [2, 7, 15]))   # 11
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <unordered_set>
#include <vector>
using namespace std;

int mincostTickets(vector<int>& days, vector<int>& costs) {
    unordered_set<int> travel(days.begin(), days.end());
    int last = days.back();
    vector<int> dp(last + 1, 0);
    for (int d = 1; d <= last; ++d) {
        if (!travel.count(d)) { dp[d] = dp[d - 1]; continue; }
        dp[d] = min({dp[d - 1] + costs[0],
                     dp[max(0, d - 7)] + costs[1],
                     dp[max(0, d - 30)] + costs[2]});
    }
    return dp[last];
}

int main() {
    vector<int> days = {1, 4, 6, 7, 8, 20}, costs = {2, 7, 15};
    cout << mincostTickets(days, costs) << "\n";   // 11
}
```

## ⏱️ Complexity
- **Time:** `O(last_day)`.
- **Space:** `O(last_day)`.
