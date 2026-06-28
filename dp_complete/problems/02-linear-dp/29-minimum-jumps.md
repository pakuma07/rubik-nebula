# Minimum Jumps to Reach End

> `dp[i]` = fewest jumps to index `i`. GFG · 🟡 Medium

## Problem
`arr[i]` is the maximum jump length from `i`. Return the minimum number of jumps to reach the last index, or `-1` if unreachable.

## 🧮 Math / Recurrence
$$
dp[i] = 1 + \min_{\substack{j < i \\ j + arr[j] \ge i}} dp[j], \qquad dp[0] = 0
$$

If no such `j` exists, `dp[i] = \infty` (unreachable).

## 🧠 Logic
The fewest jumps to reach `i` is one more than the best of all earlier indices `j` that can directly jump to `i`. This `O(n²)` DP makes the structure explicit (the [Jump Game II](26-jump-game-ii.md) greedy is the `O(n)` optimization of the same quantity). Unreachable cells stay `∞`.

## 🔢 Iteration trace (`arr = [1, 3, 5, 8, 9, 2, 6, 7, 6, 8, 9]`)
| i | reachable j (j+arr[j] ≥ i) | dp[i] |
|---|----------------------------|-------|
| 0 | – | 0 |
| 1 | j=0 | 1 |
| 2 | j=1 | 2 |
| 3 | j=1 | 2 |
| 4 | j=2 or 3 | 3 |
| … | … | … |
| 10 | j=3 (reaches 11) | **3** |

**Answer = 3.**

## 🐍 Python
```python
def min_jumps(arr: list[int]) -> int:
    n = len(arr)
    INF = float("inf")
    dp = [INF] * n
    dp[0] = 0
    for i in range(1, n):
        for j in range(i):
            if j + arr[j] >= i and dp[j] + 1 < dp[i]:
                dp[i] = dp[j] + 1
    return dp[n - 1] if dp[n - 1] != INF else -1


if __name__ == "__main__":
    print(min_jumps([1, 3, 5, 8, 9, 2, 6, 7, 6, 8, 9]))   # 3
```

## ⚙️ C++
```cpp
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int minJumps(vector<int>& arr) {
    int n = arr.size();
    const int INF = INT_MAX;
    vector<int> dp(n, INF);
    dp[0] = 0;
    for (int i = 1; i < n; ++i)
        for (int j = 0; j < i; ++j)
            if (j + arr[j] >= i && dp[j] != INF && dp[j] + 1 < dp[i])
                dp[i] = dp[j] + 1;
    return dp[n - 1] == INF ? -1 : dp[n - 1];
}

int main() {
    vector<int> arr = {1, 3, 5, 8, 9, 2, 6, 7, 6, 8, 9};
    cout << minJumps(arr) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` (greedy [Jump Game II](26-jump-game-ii.md) does it in `O(n)`).
- **Space:** `O(n)`.
