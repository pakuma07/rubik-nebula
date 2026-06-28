# Count of Subsets with Sum S

> Subset Sum, but counting instead of OR-ing. GFG · 🟡 Medium

## Problem
Count the number of subsets of `nums` whose elements sum to exactly `S`.

## 🧮 Math / Recurrence
$$
dp[w] \mathrel{+}= dp[w - num], \qquad dp[0] = 1
$$

processed with capacity **decreasing** (each number used once).

## 🧠 Logic
Where [Subset Sum](02-subset-sum.md) asks *whether* a sum is reachable (OR), counting asks *how many ways* — so the boolean OR becomes integer **addition**. Each number either joins a subset (adding `dp[w−num]` ways) or not. The downward loop keeps it a 0/1 count.

## 🔢 Iteration trace (`nums = [1, 1, 2, 3]`, `S = 4`)
After processing all numbers, `dp[4]` counts: `{1,3}, {1,3}, {1,1,2}` → **3** subsets (the two 1's are distinct positions).

| w | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| dp| 1 | 2 | 2 | 3 | **3** |

## 🐍 Python
```python
def count_subsets(nums: list[int], target: int) -> int:
    dp = [0] * (target + 1)
    dp[0] = 1
    for num in nums:
        for w in range(target, num - 1, -1):
            dp[w] += dp[w - num]
    return dp[target]


if __name__ == "__main__":
    print(count_subsets([1, 1, 2, 3], 4))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int countSubsets(vector<int>& nums, int target) {
    vector<int> dp(target + 1, 0);
    dp[0] = 1;
    for (int num : nums)
        for (int w = target; w >= num; --w)
            dp[w] += dp[w - num];
    return dp[target];
}

int main() {
    vector<int> nums = {1, 1, 2, 3};
    cout << countSubsets(nums, 4) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n·S)`.
- **Space:** `O(S)`.
