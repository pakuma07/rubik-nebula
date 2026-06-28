# Target Sum (+/−)

> Assign signs → count subsets to a derived target. LC 494 · 🟡 Medium

## Problem
Assign `+` or `−` to each of `nums` so the signed sum equals `target`. Count the assignments.

## 🧮 Math / Recurrence
Let `P` = sum of positives, `N` = sum of negatives. Then `P − N = target` and `P + N = total`, so:

$$
P = \frac{target + total}{2}
$$

Count subsets summing to `P` (infeasible if `target+total` is odd or `|target| > total`):

$$
dp[w] \mathrel{+}= dp[w - num], \qquad dp[0] = 1
$$

## 🧠 Logic
The set assigned `+` determines everything, and its required sum `P` is fixed by the two linear equations above. So the sign problem collapses to [Count of Subsets with Sum P](04-count-subsets-with-sum.md). Parity/range checks rule out impossible targets up front.

## 🔢 Iteration trace (`nums = [1,1,1,1,1]`, `target = 3`)
- `total = 5`, `P = (3+5)/2 = 4`.
- Count subsets summing to 4 → choose 4 of the five 1's → `C(5,4) = ` **5**.

## 🐍 Python
```python
def find_target_sum_ways(nums: list[int], target: int) -> int:
    total = sum(nums)
    if (total + target) % 2 or abs(target) > total:
        return 0
    p = (total + target) // 2
    dp = [1] + [0] * p
    for num in nums:
        for w in range(p, num - 1, -1):
            dp[w] += dp[w - num]
    return dp[p]


if __name__ == "__main__":
    print(find_target_sum_ways([1, 1, 1, 1, 1], 3))   # 5
```

## ⚙️ C++
```cpp
#include <cstdlib>
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int findTargetSumWays(vector<int>& nums, int target) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if ((total + target) % 2 || abs(target) > total) return 0;
    int p = (total + target) / 2;
    vector<int> dp(p + 1, 0);
    dp[0] = 1;
    for (int num : nums)
        for (int w = p; w >= num; --w)
            dp[w] += dp[w - num];
    return dp[p];
}

int main() {
    vector<int> nums = {1, 1, 1, 1, 1};
    cout << findTargetSumWays(nums, 3) << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n·P)`.
- **Space:** `O(P)`.
