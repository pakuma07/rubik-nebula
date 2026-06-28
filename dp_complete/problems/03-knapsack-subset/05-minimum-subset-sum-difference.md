# Minimum Subset Sum Difference

> Find reachable subset sums near total/2. GFG · 🟡 Medium

## Problem
Partition `nums` into two subsets so the **absolute difference** of their sums is minimized; return that difference.

## 🧮 Math / Recurrence
Compute all reachable subset sums `s ≤ total/2` (Subset-Sum DP). For each reachable `s`, one part is `s`, the other `total − s`:

$$
\text{ans} = \min_{\substack{s \le total/2 \\ dp[s] = \text{true}}} \big(total - 2s\big)
$$

## 🧠 Logic
A split is fully described by one subset's sum `s`; the difference is `|total − 2s|`, minimized when `s` is as close to `total/2` as possible. So enumerate every achievable `s` up to half and pick the largest one — it gives the smallest gap.

## 🔢 Iteration trace (`nums = [1, 6, 11, 5]`)
- `total = 23`, half = 11.
- Reachable sums ≤ 11 include `11` (`{6,5}` or `{11}`).
- Best `s = 11` ⟹ diff = `23 − 22 = ` **1**.

## 🐍 Python
```python
def min_subset_sum_diff(nums: list[int]) -> int:
    total = sum(nums)
    half = total // 2
    dp = [False] * (half + 1)
    dp[0] = True
    for num in nums:
        for w in range(half, num - 1, -1):
            if dp[w - num]:
                dp[w] = True
    for s in range(half, -1, -1):
        if dp[s]:
            return total - 2 * s
    return total


if __name__ == "__main__":
    print(min_subset_sum_diff([1, 6, 11, 5]))   # 1
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int minSubsetSumDiff(vector<int>& nums) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    int half = total / 2;
    vector<char> dp(half + 1, false);
    dp[0] = true;
    for (int num : nums)
        for (int w = half; w >= num; --w)
            if (dp[w - num]) dp[w] = true;
    for (int s = half; s >= 0; --s)
        if (dp[s]) return total - 2 * s;
    return total;
}

int main() {
    vector<int> nums = {1, 6, 11, 5};
    cout << minSubsetSumDiff(nums) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n · total)`.
- **Space:** `O(total)`.
