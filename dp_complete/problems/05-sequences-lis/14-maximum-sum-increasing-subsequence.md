# Maximum Sum Increasing Subsequence

> LIS that maximizes sum instead of length. GFG · 🟡 Medium

## Problem
Return the maximum **sum** of a strictly increasing subsequence of `nums`.

## 🧮 Math / Recurrence
`dp[i]` = max sum of an increasing subsequence ending at `i`:

$$
dp[i] = nums_i + \max\Big(0,\ \max_{\substack{j < i \\ nums_j < nums_i}} dp[j]\Big), \qquad \text{answer} = \max_i dp[i]
$$

## 🧠 Logic
Identical skeleton to the `O(n²)` LIS DP, but instead of adding 1 for each extension we add the actual element value, and `dp[i]` stores the best **sum** of a valid chain ending at `i`. The answer is the maximum over all endpoints.

## 🔢 Iteration trace (`nums = [1,101,2,3,100]`)
- Chain `1,2,3,100` sum = **106** (beats `1,101 = 102`).

## 🐍 Python
```python
def max_sum_increasing_subseq(nums: list[int]) -> int:
    n = len(nums)
    dp = list(nums)
    for i in range(n):
        for j in range(i):
            if nums[j] < nums[i]:
                dp[i] = max(dp[i], dp[j] + nums[i])
    return max(dp, default=0)


if __name__ == "__main__":
    print(max_sum_increasing_subseq([1, 101, 2, 3, 100]))   # 106
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxSumIncreasingSubseq(vector<int>& nums) {
    int n = nums.size(), best = 0;
    vector<int> dp(nums);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < i; ++j)
            if (nums[j] < nums[i]) dp[i] = max(dp[i], dp[j] + nums[i]);
        best = max(best, dp[i]);
    }
    return best;
}

int main() {
    vector<int> nums = {1, 101, 2, 3, 100};
    cout << maxSumIncreasingSubseq(nums) << "\n";   // 106
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
