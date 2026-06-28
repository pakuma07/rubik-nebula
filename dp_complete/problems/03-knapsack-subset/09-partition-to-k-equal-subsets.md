# Partition to K Equal Sum Subsets

> Assign items to k buckets — bitmask DP. LC 698 · 🔴 Hard

## Problem
Decide whether `nums` can be partitioned into `k` subsets all with the **same sum**. (Bitmask DP; see also topic 09.)

## 🧮 Math / Recurrence
Each bucket targets `target = total/k`. State = bitmask of used elements; `dp[mask]` = the **running sum within the current bucket** of the chosen elements (mod `target`), or "unreachable":

$$
dp[mask \,|\, (1{<}{<}i)] = (dp[mask] + nums_i) \bmod target
$$

allowed only when `dp[mask] + nums_i ≤ target`.

## 🧠 Logic
Whenever a bucket exactly fills (`sum == target`), we reset and start the next, tracked implicitly by taking the running sum mod `target`. The bitmask remembers which elements are placed; `dp[mask]` records how full the current bucket is. Reaching the full mask with remainder 0 means all `k` buckets filled. Feasibility requires `total % k == 0` and every element `≤ target`.

## 🔢 Iteration trace (`nums=[4,3,2,3,5,2,1]`, `k=4`)
- `total = 20`, `target = 5`.
- Buckets `{5}, {4,1}, {3,2}, {3,2}` each sum to 5 ⟹ **true**.

## 🐍 Python
```python
def can_partition_k_subsets(nums: list[int], k: int) -> bool:
    total = sum(nums)
    if total % k:
        return False
    target = total // k
    if max(nums) > target:
        return False
    n = len(nums)
    dp: list[int | None] = [None] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] is None:
            continue
        for i in range(n):
            if mask & (1 << i):
                continue
            if dp[mask] + nums[i] <= target:
                dp[mask | (1 << i)] = (dp[mask] + nums[i]) % target
    return dp[(1 << n) - 1] == 0


if __name__ == "__main__":
    print(can_partition_k_subsets([4, 3, 2, 3, 5, 2, 1], 4))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

bool canPartitionKSubsets(vector<int>& nums, int k) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if (total % k) return false;
    int target = total / k, n = nums.size();
    for (int x : nums) if (x > target) return false;
    vector<int> dp(1 << n, -1);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << n); ++mask) {
        if (dp[mask] < 0) continue;
        for (int i = 0; i < n; ++i) {
            if (mask & (1 << i)) continue;
            if (dp[mask] + nums[i] <= target)
                dp[mask | (1 << i)] = (dp[mask] + nums[i]) % target;
        }
    }
    return dp[(1 << n) - 1] == 0;
}

int main() {
    vector<int> nums = {4, 3, 2, 3, 5, 2, 1};
    cout << boolalpha << canPartitionKSubsets(nums, 4) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(2ⁿ · n)`.
- **Space:** `O(2ⁿ)`.
