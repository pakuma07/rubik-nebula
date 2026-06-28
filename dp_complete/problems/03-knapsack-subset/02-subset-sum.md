# Subset Sum

> Boolean feasibility knapsack. GFG · 🟡 Medium

## Problem
Given positive integers `nums` and a target `S`, decide whether some subset sums exactly to `S`.

## 🧮 Math / Recurrence
$$
dp[w] = dp[w] \ \vee\ dp[w - num], \qquad dp[0] = \text{true}
$$

processed with capacity **decreasing** (0/1 — each number once).

## 🧠 Logic
This is 0/1 knapsack where we only need **feasibility**, not a maximum, so `max` becomes a logical **OR**: sum `w` is reachable if it already was (skip `num`) or if `w−num` was reachable (take `num`). The downward capacity loop keeps each number used at most once.

## 🔢 Iteration trace (`nums = [3, 34, 4, 12, 5, 2]`, `S = 9`)
Reachable sums build up; after processing `3,4,5,2,…`:
- `3` ✓, `3+4=7` ✓, `4+5=9` ✓.

`dp[9] = ` **true** (subset `{4,5}`).

## 🐍 Python
```python
def subset_sum(nums: list[int], target: int) -> bool:
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for w in range(target, num - 1, -1):
            if dp[w - num]:
                dp[w] = True
    return dp[target]


if __name__ == "__main__":
    print(subset_sum([3, 34, 4, 12, 5, 2], 9))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

bool subsetSum(vector<int>& nums, int target) {
    vector<char> dp(target + 1, false);
    dp[0] = true;
    for (int num : nums)
        for (int w = target; w >= num; --w)
            if (dp[w - num]) dp[w] = true;
    return dp[target];
}

int main() {
    vector<int> nums = {3, 34, 4, 12, 5, 2};
    cout << boolalpha << subsetSum(nums, 9) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n·S)`.
- **Space:** `O(S)`.
