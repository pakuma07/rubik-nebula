# Combination Sum IV (Ordered)

> Amount outer loop ⟹ permutations. LC 377 · 🟡 Medium

## Problem
Count the number of **ordered** sequences of `nums` (reusable) that sum to `target`. Different orderings count separately.

## 🧮 Math / Recurrence
$$
dp[a] \mathrel{+}= dp[a - num], \qquad dp[0] = 1
$$

with **amount on the outer loop**, numbers inside.

## 🧠 Logic
Same recurrence as [Coin Change II](12-coin-change-ii.md) but the loop nesting is flipped. With amount outer, at each target `a` we try *every* number as the **last** element, so `1+2` and `2+1` are both counted — exactly the permutation semantics this problem wants.

## 🔢 Iteration trace (`nums = [1, 2, 3]`, `target = 4`)
| a | dp[a] = Σ dp[a−num] | value |
|---|----------------------|-------|
| 0 | base | 1 |
| 1 | dp[0] | 1 |
| 2 | dp[1]+dp[0] | 2 |
| 3 | dp[2]+dp[1]+dp[0] | 4 |
| 4 | dp[3]+dp[2]+dp[1] | **7** |

7 ordered ways (e.g. `1+3`, `3+1`, `2+2`, `1+1+2`, …).

## 🐍 Python
```python
def combination_sum4(nums: list[int], target: int) -> int:
    dp = [1] + [0] * target
    for a in range(1, target + 1):           # amount outer → permutations
        for num in nums:
            if num <= a:
                dp[a] += dp[a - num]
    return dp[target]


if __name__ == "__main__":
    print(combination_sum4([1, 2, 3], 4))   # 7
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int combinationSum4(vector<int>& nums, int target) {
    vector<unsigned long long> dp(target + 1, 0);
    dp[0] = 1;
    for (int a = 1; a <= target; ++a)        // amount outer → permutations
        for (int num : nums)
            if (num <= a) dp[a] += dp[a - num];
    return (int)dp[target];
}

int main() {
    vector<int> nums = {1, 2, 3};
    cout << combinationSum4(nums, 4) << "\n";   // 7
}
```

## ⏱️ Complexity
- **Time:** `O(target · |nums|)`.
- **Space:** `O(target)`.
