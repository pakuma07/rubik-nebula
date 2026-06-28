# Maximum Sum Circular Subarray

> Best of normal Kadane vs the wrap-around complement. LC 918 · 🟡 Medium

## Problem
The array is **circular** (the end wraps to the start). Find the maximum non-empty subarray sum.

## 🧮 Math / Recurrence
The optimum is either a normal subarray or a wrap-around one. A wrap subarray equals `total − (some interior subarray)`, minimized by the **minimum** subarray:

$$
\text{ans} = \max\big(\text{maxKadane},\ \text{total} - \text{minKadane}\big)
$$

**Edge case:** if all numbers are negative, `total − minKadane = 0` (empty), which is invalid → return `maxKadane`.

## 🧠 Logic
Run Kadane twice in one pass: one tracking the maximum subarray, one the minimum. Removing the minimum interior subarray from the total leaves the best wrap-around segment. Guard the all-negative case where the "complement" would be empty.

## 🔢 Iteration trace (`nums = [5, -3, 5]`)
- `maxKadane = 5` (e.g. last element, or first).
- `total = 7`, `minKadane = -3` ⟹ wrap = `7 − (−3) = 10`.
- Not all negative ⟹ `ans = max(5, 10) = ` **10** (wrap `[5,(-3 skipped),5]` = front 5 + back 5).

## 🐍 Python
```python
def max_subarray_circular(nums: list[int]) -> int:
    total = 0
    cur_max = best_max = nums[0]
    cur_min = best_min = nums[0]
    for x in nums:
        total += x
    cur_max = best_max = cur_min = best_min = 0  # reset for clean pass
    cur_max = best_max = nums[0]
    cur_min = best_min = nums[0]
    for x in nums[1:]:
        cur_max = max(x, cur_max + x); best_max = max(best_max, cur_max)
        cur_min = min(x, cur_min + x); best_min = min(best_min, cur_min)
    if best_max < 0:                 # all negative
        return best_max
    return max(best_max, total - best_min)


if __name__ == "__main__":
    print(max_subarray_circular([5, -3, 5]))   # 10
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxSubarraySumCircular(vector<int>& nums) {
    int total = 0;
    int curMax = nums[0], bestMax = nums[0];
    int curMin = nums[0], bestMin = nums[0];
    total = nums[0];
    for (size_t i = 1; i < nums.size(); ++i) {
        int x = nums[i];
        total += x;
        curMax = max(x, curMax + x); bestMax = max(bestMax, curMax);
        curMin = min(x, curMin + x); bestMin = min(bestMin, curMin);
    }
    if (bestMax < 0) return bestMax;          // all negative
    return max(bestMax, total - bestMin);
}

int main() {
    vector<int> nums = {5, -3, 5};
    cout << maxSubarraySumCircular(nums) << "\n";   // 10
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
