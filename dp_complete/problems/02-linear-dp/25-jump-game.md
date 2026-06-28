# Jump Game

> Track the furthest reachable index. LC 55 · 🟡 Medium

## Problem
Each `nums[i]` is the max jump length from `i`. Starting at index `0`, can you reach the last index?

## 🧮 Math / Recurrence
Maintain `reach` = furthest index reachable so far:

$$
reach = \max(reach,\ i + nums[i]) \quad \text{for each reachable } i
$$

Reachable to the end iff `reach ≥ n−1` and no `i > reach` is ever encountered.

## 🧠 Logic
This greedy *is* the DP collapsed: instead of asking "is index `i` reachable?" for every `i`, track a single frontier. If your scan index ever exceeds the frontier, a gap blocks you. Otherwise expand the frontier and continue.

## 🔢 Iteration trace (`nums = [2, 3, 1, 1, 4]`)
| i | nums[i] | i+nums[i] | reach |
|---|---------|-----------|-------|
| 0 | 2 | 2 | 2 |
| 1 | 3 | 4 | 4 |
| 2 | 1 | 3 | 4 |
| 3 | 1 | 4 | 4 |
| 4 | 4 | – | reach ≥ 4 |

`reach = 4 ≥ n−1 = 4` ⟹ **true**.

## 🐍 Python
```python
def can_jump(nums: list[int]) -> bool:
    reach = 0
    for i, step in enumerate(nums):
        if i > reach:
            return False
        reach = max(reach, i + step)
    return True


if __name__ == "__main__":
    print(can_jump([2, 3, 1, 1, 4]))   # True
    print(can_jump([3, 2, 1, 0, 4]))   # False
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

bool canJump(vector<int>& nums) {
    int reach = 0;
    for (int i = 0; i < (int)nums.size(); ++i) {
        if (i > reach) return false;
        reach = max(reach, i + nums[i]);
    }
    return true;
}

int main() {
    vector<int> a = {2, 3, 1, 1, 4}, b = {3, 2, 1, 0, 4};
    cout << boolalpha << canJump(a) << " " << canJump(b) << "\n";  // true false
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
