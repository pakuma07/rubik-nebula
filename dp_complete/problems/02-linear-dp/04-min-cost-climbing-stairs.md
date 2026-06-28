# Min Cost Climbing Stairs

> Fibonacci shape, but minimizing cost. LC 746 · 🟢 Easy

## Problem
`cost[i]` is the price to step on stair `i`. You may start at index `0` or `1` and climb `1` or `2` steps. Reach the top (just past the last stair) for the minimum total cost.

## 🧮 Math / Recurrence
Let `dp[i]` = min cost to **stand on** stair `i`:

$$
dp[i] = cost[i] + \min\big(dp[i-1],\ dp[i-2]\big), \qquad dp[0]=cost[0],\ dp[1]=cost[1]
$$

The answer (one step past the end) is:

$$
\text{ans} = \min\big(dp[n-1],\ dp[n-2]\big)
$$

## 🧠 Logic
Reaching the top is free; only standing on stairs costs. To stand on `i` you arrive from the cheaper of `i−1`/`i−2`, then pay `cost[i]`. The final answer compares the two stairs you can leap to the top from.

## 🔢 Iteration trace (`cost = [10, 15, 20]`)
| i | cost | `dp[i] = cost[i]+min(dp[i-1],dp[i-2])` |
|---|------|----------------------------------------|
| 0 | 10   | 10 |
| 1 | 15   | 15 |
| 2 | 20   | 20 + min(15,10) = **30** |

Top = `min(dp[2], dp[1]) = min(30, 15) = ` **15**.

## 🐍 Python
```python
def min_cost_climbing_stairs(cost: list[int]) -> int:
    a, b = 0, 0                 # cost to reach stairs i-2, i-1 (start free)
    for c in cost:
        a, b = b, c + min(a, b)
    return min(a, b)


if __name__ == "__main__":
    print(min_cost_climbing_stairs([10, 15, 20]))   # 15
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minCostClimbingStairs(vector<int>& cost) {
    int a = 0, b = 0;
    for (int c : cost) {
        int next = c + min(a, b);
        a = b; b = next;
    }
    return min(a, b);
}

int main() {
    vector<int> cost = {10, 15, 20};
    cout << minCostClimbingStairs(cost) << "\n";   // 15
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
