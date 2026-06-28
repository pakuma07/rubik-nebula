# Delete and Earn

> House Robber disguised on the value axis. LC 740 · 🟡 Medium

## Problem
Pick any `nums[i]` to earn `nums[i]` points, but doing so **deletes every element equal to `nums[i]−1` and `nums[i]+1`**. Maximize points.

## 🧮 Math / Recurrence
Bucket by value: `gain[v] = v × count(v)`. Choosing value `v` forbids `v−1` and `v+1` — exactly the non-adjacency rule on the value line:

$$
dp[v] = \max\big(dp[v-1],\ dp[v-2] + gain[v]\big)
$$

## 🧠 Logic
The conflict is between **adjacent values**, not adjacent indices, so first collapse duplicates into total earnings per value, then run House Robber over the value range `0..max`. Taking `v` means giving up `v−1`, so you fall back to `dp[v−2]`.

## 🔢 Iteration trace (`nums = [2, 2, 3, 3, 3, 4]`)
`gain = {2:4, 3:9, 4:4}` (e.g. three 3's → 9).
| v | gain[v] | rob = dp[v-2]+gain | skip = dp[v-1] | dp[v] |
|---|---------|--------------------|----------------|-------|
| 0 | 0 | – | – | 0 |
| 1 | 0 | – | – | 0 |
| 2 | 4 | 0+4 | 0 | 4 |
| 3 | 9 | 0+9 | 4 | **9** |
| 4 | 4 | 4+4 | 9 | **9** |

**Answer = 9** (take all 3's; deleting 2's and 4's is fine since 9 > 8).

## 🐍 Python
```python
def delete_and_earn(nums: list[int]) -> int:
    if not nums:
        return 0
    top = max(nums)
    gain = [0] * (top + 1)
    for x in nums:
        gain[x] += x
    prev2, prev1 = 0, 0
    for v in range(top + 1):
        prev2, prev1 = prev1, max(prev1, prev2 + gain[v])
    return prev1


if __name__ == "__main__":
    print(delete_and_earn([2, 2, 3, 3, 3, 4]))   # 9
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int deleteAndEarn(vector<int>& nums) {
    if (nums.empty()) return 0;
    int top = *max_element(nums.begin(), nums.end());
    vector<long long> gain(top + 1, 0);
    for (int x : nums) gain[x] += x;
    long long prev2 = 0, prev1 = 0;
    for (int v = 0; v <= top; ++v) {
        long long cur = max(prev1, prev2 + gain[v]);
        prev2 = prev1; prev1 = cur;
    }
    return (int)prev1;
}

int main() {
    vector<int> nums = {2, 2, 3, 3, 3, 4};
    cout << deleteAndEarn(nums) << "\n";   // 9
}
```

## ⏱️ Complexity
- **Time:** `O(n + max)`.
- **Space:** `O(max)` for the gain buckets.
