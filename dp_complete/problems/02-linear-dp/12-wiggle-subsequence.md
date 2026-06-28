# Wiggle Subsequence

> Two-state linear DP: track up/down runs. LC 376 · 🟡 Medium

## Problem
A *wiggle* sequence has strictly alternating positive/negative differences. Return the length of the **longest wiggle subsequence** of `nums`.

## 🧮 Math / Recurrence
Track two lengths — the longest wiggle ending on a **rising** step (`up`) and on a **falling** step (`down`):

$$
up = down + 1 \ \text{ if } nums[i] > nums[i-1]
$$
$$
down = up + 1 \ \text{ if } nums[i] < nums[i-1]
$$

Answer = `max(up, down)`.

## 🧠 Logic
A new element can only **extend the opposite direction** of the previous wiggle: after a fall you may rise and vice versa. Equal neighbors leave both states unchanged. Because each state depends only on the other's prior value, two scalars track everything.

## 🔢 Iteration trace (`nums = [1, 7, 4, 9, 2, 5]`)
| i | nums[i] | compare to prev | up | down |
|---|---------|-----------------|----|------|
| 0 | 1 | – | 1 | 1 |
| 1 | 7 | up (7>1) | 2 | 1 |
| 2 | 4 | down (4<7) | 2 | 3 |
| 3 | 9 | up | 4 | 3 |
| 4 | 2 | down | 4 | 5 |
| 5 | 5 | up | 6 | 5 |

**Answer = 6** (the whole array already wiggles).

## 🐍 Python
```python
def wiggle_max_length(nums: list[int]) -> int:
    if not nums:
        return 0
    up = down = 1
    for i in range(1, len(nums)):
        if nums[i] > nums[i - 1]:
            up = down + 1
        elif nums[i] < nums[i - 1]:
            down = up + 1
    return max(up, down)


if __name__ == "__main__":
    print(wiggle_max_length([1, 7, 4, 9, 2, 5]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int wiggleMaxLength(vector<int>& nums) {
    if (nums.empty()) return 0;
    int up = 1, down = 1;
    for (size_t i = 1; i < nums.size(); ++i) {
        if (nums[i] > nums[i - 1]) up = down + 1;
        else if (nums[i] < nums[i - 1]) down = up + 1;
    }
    return max(up, down);
}

int main() {
    vector<int> nums = {1, 7, 4, 9, 2, 5};
    cout << wiggleMaxLength(nums) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
