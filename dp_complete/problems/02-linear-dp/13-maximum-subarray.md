# Maximum Subarray (Kadane)

> The canonical running-sum DP. LC 53 · 🟡 Medium

## Problem
Find the contiguous subarray with the largest sum and return that sum.

## 🧮 Math / Recurrence
Let `cur[i]` = best subarray sum **ending at** `i`:

$$
cur[i] = \max\big(nums[i],\ cur[i-1] + nums[i]\big)
$$

$$
\text{ans} = \max_i cur[i]
$$

## 🧠 Logic
Either extend the running subarray or **restart** at the current element — you restart exactly when the running sum has gone negative (it can only hurt). The global answer is the best `cur` ever observed, tracked alongside the rolling sum.

## 🔢 Iteration trace (`nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`)
| i | nums[i] | `cur = max(x, cur+x)` | best |
|---|---------|-----------------------|------|
| 0 | -2 | -2 | -2 |
| 1 | 1  | max(1,-1)=1 | 1 |
| 2 | -3 | max(-3,-2)=-2 | 1 |
| 3 | 4  | max(4,2)=4 | 4 |
| 4 | -1 | 3 | 4 |
| 5 | 2  | 5 | 5 |
| 6 | 1  | 6 | **6** |
| 7 | -5 | 1 | 6 |
| 8 | 4  | 5 | 6 |

**Answer = 6** (subarray `[4,-1,2,1]`).

## 🐍 Python
```python
def max_sub_array(nums: list[int]) -> int:
    cur = best = nums[0]
    for x in nums[1:]:
        cur = max(x, cur + x)
        best = max(best, cur)
    return best


if __name__ == "__main__":
    print(max_sub_array([-2, 1, -3, 4, -1, 2, 1, -5, 4]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxSubArray(vector<int>& nums) {
    int cur = nums[0], best = nums[0];
    for (size_t i = 1; i < nums.size(); ++i) {
        cur = max(nums[i], cur + nums[i]);
        best = max(best, cur);
    }
    return best;
}

int main() {
    vector<int> nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    cout << maxSubArray(nums) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
