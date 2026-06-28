# Count Ways with Steps {1, 2, 3}

> Tribonacci-shaped counting. GFG · 🟢 Easy

## Problem
Climb `n` stairs taking `1`, `2`, or `3` steps at a time. Count the distinct orderings.

## 🧮 Math / Recurrence
$$
dp[i] = dp[i-1] + dp[i-2] + dp[i-3], \qquad dp[0]=1,\ dp[1]=1,\ dp[2]=2
$$

The final move into `i` is a 1-, 2-, or 3-step, and those groups are disjoint → **sum** the three.

## 🧠 Logic
Identical in spirit to Climbing Stairs but with three predecessors. Roll three scalars. (This is `Tribonacci` reindexed with `dp[0]=1`.)

## 🔢 Iteration trace (`n = 5`)
| i | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| dp| 1 | 1 | 2 | 4 | 7 | **13** |

`dp[3]=1+2+1=4`, `dp[4]=4+2+1=7`, `dp[5]=7+4+2=13`.

## 🐍 Python
```python
def count_ways(n: int) -> int:
    if n == 0:
        return 1
    a, b, c = 1, 1, 2           # dp[0], dp[1], dp[2]
    if n < 3:
        return [a, b, c][n]
    for _ in range(n - 2):
        a, b, c = b, c, a + b + c
    return c


if __name__ == "__main__":
    print(count_ways(5))        # 13
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

long long countWays(int n) {
    long long dp[3] = {1, 1, 2};
    if (n < 3) return dp[n];
    long long a = dp[0], b = dp[1], c = dp[2];
    for (int i = 0; i < n - 2; ++i) {
        long long next = a + b + c;
        a = b; b = c; c = next;
    }
    return c;
}

int main() {
    cout << countWays(5) << "\n";   // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
