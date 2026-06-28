# Minimum Operations to Make Array Increasing

> Greedy linear bump. LC 1827 · 🟢 Easy

## Problem
In one operation increment any element by 1. Return the minimum operations to make `nums` **strictly increasing**.

## 🧮 Math / Recurrence
Scan left to right, forcing each element above its predecessor:

$$
nums_i \leftarrow \max(nums_i,\ nums_{i-1} + 1), \qquad \text{ops} \mathrel{+}= \max(0,\ nums_{i-1}+1 - nums_i)
$$

## 🧠 Logic
Raising an element only ever helps later elements (a larger predecessor floor is never beneficial), so the greedy minimum is to lift each element to **exactly** `prev+1` when it is too small — never higher. Summing those forced increments gives the optimum in one pass.

## 🔢 Iteration trace (`nums = [1,1,1]`)
- `i=1`: need 2 → +1. `i=2`: need 3 → +2. Total = **3**.

## 🐍 Python
```python
def min_operations(nums: list[int]) -> int:
    ops = 0
    prev = nums[0]
    for x in nums[1:]:
        need = prev + 1
        if x < need:
            ops += need - x
            prev = need
        else:
            prev = x
    return ops


if __name__ == "__main__":
    print(min_operations([1, 1, 1]))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int minOperations(vector<int>& nums) {
    int ops = 0, prev = nums[0];
    for (size_t i = 1; i < nums.size(); ++i) {
        int need = prev + 1;
        if (nums[i] < need) { ops += need - nums[i]; prev = need; }
        else prev = nums[i];
    }
    return ops;
}

int main() {
    vector<int> nums = {1, 1, 1};
    cout << minOperations(nums) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
