# House Robber

> Maximize loot with the non-adjacent constraint. LC 198 · 🟡 Medium

## Problem
Given house values `nums`, rob houses for the maximum total, but you **cannot rob two adjacent** houses.

## 🧮 Math / Recurrence
$$
dp[i] = \max\big(\underbrace{dp[i-1]}_{\text{skip }i},\ \underbrace{dp[i-2] + nums[i]}_{\text{rob }i}\big)
$$

$$
dp[0] = nums[0], \qquad dp[1] = \max(nums[0], nums[1])
$$

## 🧠 Logic
At each house there are exactly two choices: **skip it** (keep `dp[i−1]`) or **rob it** (then `i−1` is forbidden, so add `nums[i]` to `dp[i−2]`). The only history that matters is whether the previous house was taken — captured by comparing `dp[i−1]` vs `dp[i−2]`. Two rolling scalars suffice.

## 🔢 Iteration trace (`nums = [2, 7, 9, 3, 1]`)
| i | nums[i] | rob = dp[i-2]+nums[i] | skip = dp[i-1] | `dp[i]` |
|---|---------|-----------------------|----------------|---------|
| 0 | 2       | –                     | –              | 2 |
| 1 | 7       | –                     | –              | 7 |
| 2 | 9       | 2 + 9 = 11            | 7              | **11** |
| 3 | 3       | 7 + 3 = 10            | 11             | 11 |
| 4 | 1       | 11 + 1 = 12           | 11             | **12** |

**Answer = 12** (rob houses 0, 2, 4).

## 🐍 Python
```python
def rob(nums: list[int]) -> int:
    prev2, prev1 = 0, 0         # dp[i-2], dp[i-1]
    for x in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1


if __name__ == "__main__":
    print(rob([2, 7, 9, 3, 1]))   # 12
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int rob(vector<int>& nums) {
    int prev2 = 0, prev1 = 0;
    for (int x : nums) {
        int cur = max(prev1, prev2 + x);
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int main() {
    vector<int> nums = {2, 7, 9, 3, 1};
    cout << rob(nums) << "\n";   // 12
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
