# Partition Array for Maximum Sum

> Last group size ≤ k, fill with the group max. LC 1043 · 🟡 Medium

## Problem
Partition `arr` into contiguous subarrays of length at most `k`. Each subarray's elements all become its **maximum** value. Return the largest possible sum of the transformed array.

## 🧮 Math / Recurrence
`dp[i]` = best sum for the prefix `arr[:i]`:

$$
dp[i] = \max_{1 \le L \le \min(k, i)}\big(dp[i-L] + L \cdot \max(arr[i-L..i-1])\big)
$$

## 🧠 Logic
This is a 1D partition DP: the last group ends at index `i−1` and has some length `L ≤ k`. Its contribution is `L` copies of the group's maximum. Scanning `L` from 1 upward lets us extend the running max incrementally, so each `dp[i]` is `O(k)`.

## 🔢 Iteration trace (`arr = [1,15,7,9,2,5,10]`, `k = 3`)
- Optimal grouping yields **84**.

## 🐍 Python
```python
def max_sum_after_partitioning(arr: list[int], k: int) -> int:
    n = len(arr)
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        cur_max = 0
        for L in range(1, min(k, i) + 1):
            cur_max = max(cur_max, arr[i - L])
            dp[i] = max(dp[i], dp[i - L] + L * cur_max)
    return dp[n]


if __name__ == "__main__":
    print(max_sum_after_partitioning([1, 15, 7, 9, 2, 5, 10], 3))   # 84
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxSumAfterPartitioning(vector<int>& arr, int k) {
    int n = arr.size();
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= n; ++i) {
        int curMax = 0;
        for (int L = 1; L <= min(k, i); ++L) {
            curMax = max(curMax, arr[i - L]);
            dp[i] = max(dp[i], dp[i - L] + L * curMax);
        }
    }
    return dp[n];
}

int main() {
    vector<int> arr = {1, 15, 7, 9, 2, 5, 10};
    cout << maxSumAfterPartitioning(arr, 3) << "\n";   // 84
}
```

## ⏱️ Complexity
- **Time:** `O(n·k)`.
- **Space:** `O(n)`.
