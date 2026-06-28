# Maximum Product Subarray

> Kadane with sign-tracking: keep max AND min. LC 152 · 🟡 Medium

## Problem
Find the contiguous subarray with the largest **product**.

## 🧮 Math / Recurrence
A negative element swaps the roles of the largest and smallest products, so carry **both**:

$$
hi[i] = \max\big(nums[i],\ hi[i-1]\cdot nums[i],\ lo[i-1]\cdot nums[i]\big)
$$
$$
lo[i] = \min\big(nums[i],\ hi[i-1]\cdot nums[i],\ lo[i-1]\cdot nums[i]\big)
$$

Answer = `max_i hi[i]`.

## 🧠 Logic
Unlike sums, multiplying by a negative flips magnitude ordering — today's *minimum* (a large negative) can become tomorrow's *maximum*. Maintaining the running max **and** min lets a sign change promote the min into the new max.

## 🔢 Iteration trace (`nums = [2, 3, -2, 4]`)
| i | x | candidates (x, hi·x, lo·x) | hi | lo | best |
|---|---|----------------------------|----|----|------|
| 0 | 2  | – | 2 | 2 | 2 |
| 1 | 3  | 3, 6, 6 | 6 | 3 | **6** |
| 2 | -2 | -2, -12, -6 | -2 | -12 | 6 |
| 3 | 4  | 4, -8, -48 | 4 | -48 | 6 |

**Answer = 6** (subarray `[2,3]`).

## 🐍 Python
```python
def max_product(nums: list[int]) -> int:
    best = hi = lo = nums[0]
    for x in nums[1:]:
        cands = (x, hi * x, lo * x)
        hi, lo = max(cands), min(cands)
        best = max(best, hi)
    return best


if __name__ == "__main__":
    print(max_product([2, 3, -2, 4]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxProduct(vector<int>& nums) {
    int best = nums[0], hi = nums[0], lo = nums[0];
    for (size_t i = 1; i < nums.size(); ++i) {
        int x = nums[i];
        int a = x, b = hi * x, c = lo * x;
        hi = max({a, b, c});
        lo = min({a, b, c});
        best = max(best, hi);
    }
    return best;
}

int main() {
    vector<int> nums = {2, 3, -2, 4};
    cout << maxProduct(nums) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
