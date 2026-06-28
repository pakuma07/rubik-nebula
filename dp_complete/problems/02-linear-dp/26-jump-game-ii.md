# Jump Game II

> Minimum jumps via BFS-style range levels. LC 45 · 🟡 Medium

## Problem
Each `nums[i]` is the max jump from `i`. Return the **minimum number of jumps** to reach the last index (a solution is guaranteed).

## 🧮 Math / Recurrence
Greedy "implicit BFS" over reachable ranges. Track `cur_end` (boundary of the current jump's reach) and `farthest`:

$$
farthest = \max_{i \le cur\_end}(i + nums[i])
$$

Each time `i` hits `cur_end`, spend one jump and set `cur_end = farthest`.

## 🧠 Logic
Indices reachable in exactly `k` jumps form a contiguous **level** (like BFS layers). While scanning a level, compute how far the *next* level extends. When you exhaust the current level boundary, increment the jump count and advance the boundary — guaranteeing the minimum because each level is the maximal reach for that jump count.

## 🔢 Iteration trace (`nums = [2, 3, 1, 1, 4]`)
| i | nums[i] | farthest | cur_end | jumps |
|---|---------|----------|---------|-------|
| 0 | 2 | 2 | 0→2 | 1 (hit end 0) |
| 1 | 3 | 4 | 2 | 1 |
| 2 | 1 | 4 | 2→4 | 2 (hit end 2) |
| 3 | 1 | 4 | 4 | 2 |

Reaches index 4 in **2** jumps (`0 → 1 → 4`).

## 🐍 Python
```python
def jump(nums: list[int]) -> int:
    jumps = cur_end = farthest = 0
    for i in range(len(nums) - 1):
        farthest = max(farthest, i + nums[i])
        if i == cur_end:                # exhausted current level
            jumps += 1
            cur_end = farthest
    return jumps


if __name__ == "__main__":
    print(jump([2, 3, 1, 1, 4]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int jump(vector<int>& nums) {
    int jumps = 0, curEnd = 0, farthest = 0;
    for (int i = 0; i < (int)nums.size() - 1; ++i) {
        farthest = max(farthest, i + nums[i]);
        if (i == curEnd) { ++jumps; curEnd = farthest; }
    }
    return jumps;
}

int main() {
    vector<int> nums = {2, 3, 1, 1, 4};
    cout << jump(nums) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
