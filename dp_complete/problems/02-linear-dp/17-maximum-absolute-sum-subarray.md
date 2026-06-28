# Maximum Absolute Sum of Any Subarray

> Run Kadane for both the max and the min. LC 1749 · 🟡 Medium

## Problem
Return the maximum of `|sum(subarray)|` over all subarrays.

## 🧮 Math / Recurrence
The largest absolute sum is either the most positive subarray or the most negative one:

$$
\text{ans} = \max\big(\text{maxKadane},\ -\,\text{minKadane}\big)
$$

## 🧠 Logic
`|x|` is large when `x` is very positive **or** very negative. Run Kadane twice in one pass — one tracking the maximum subarray sum, one the minimum — then take the larger of the max and the negated min. (Equivalently, `maxPrefix − minPrefix` over prefix sums.)

## 🔢 Iteration trace (`nums = [1, -3, 2, 3, -4]`)
| i | x | curMax | bestMax | curMin | bestMin |
|---|---|--------|---------|--------|---------|
| 0 | 1  | 1 | 1 | 1 | 1 |
| 1 | -3 | -2 | 1 | -3 | -3 |
| 2 | 2  | 2 | 2 | -1 | -3 |
| 3 | 3  | 5 | **5** | 3 | -3 |
| 4 | -4 | 1 | 5 | -4 | **-4** |

`ans = max(5, −(−4)) = max(5, 4) = ` **5**.

## 🐍 Python
```python
def max_absolute_sum(nums: list[int]) -> int:
    cur_max = best_max = 0
    cur_min = best_min = 0
    for x in nums:
        cur_max = max(0, cur_max + x); best_max = max(best_max, cur_max)
        cur_min = min(0, cur_min + x); best_min = min(best_min, cur_min)
    return max(best_max, -best_min)


if __name__ == "__main__":
    print(max_absolute_sum([1, -3, 2, 3, -4]))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxAbsoluteSum(vector<int>& nums) {
    int curMax = 0, bestMax = 0, curMin = 0, bestMin = 0;
    for (int x : nums) {
        curMax = max(0, curMax + x); bestMax = max(bestMax, curMax);
        curMin = min(0, curMin + x); bestMin = min(bestMin, curMin);
    }
    return max(bestMax, -bestMin);
}

int main() {
    vector<int> nums = {1, -3, 2, 3, -4};
    cout << maxAbsoluteSum(nums) << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
