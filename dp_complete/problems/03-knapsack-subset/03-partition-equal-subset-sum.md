# Partition Equal Subset Sum

> Reduce to Subset Sum at half the total. LC 416 · 🟡 Medium

## Problem
Decide whether `nums` can be split into two subsets with **equal sums**.

## 🧮 Math / Recurrence
Equal halves exist iff `total` is even and some subset sums to `total/2`:

$$
\text{total odd} \Rightarrow \text{false}, \qquad \text{else solve Subset-Sum}(nums,\ total/2)
$$

$$
dp[w] = dp[w] \vee dp[w - num], \quad dp[0] = \text{true}
$$

## 🧠 Logic
If one subset sums to `total/2`, its complement automatically sums to the other half. An odd total can never split evenly. So the problem collapses to [Subset Sum](02-subset-sum.md) with target `total/2`.

## 🔢 Iteration trace (`nums = [1, 5, 11, 5]`)
- `total = 22`, half = `11`.
- Subsets reaching 11: `{11}` or `{1,5,5}` ✓.

`dp[11] = ` **true** ⟹ partition exists.

## 🐍 Python
```python
def can_partition(nums: list[int]) -> bool:
    total = sum(nums)
    if total % 2:
        return False
    half = total // 2
    dp = [False] * (half + 1)
    dp[0] = True
    for num in nums:
        for w in range(half, num - 1, -1):
            if dp[w - num]:
                dp[w] = True
    return dp[half]


if __name__ == "__main__":
    print(can_partition([1, 5, 11, 5]))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

bool canPartition(vector<int>& nums) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if (total % 2) return false;
    int half = total / 2;
    vector<char> dp(half + 1, false);
    dp[0] = true;
    for (int num : nums)
        for (int w = half; w >= num; --w)
            if (dp[w - num]) dp[w] = true;
    return dp[half];
}

int main() {
    vector<int> nums = {1, 5, 11, 5};
    cout << boolalpha << canPartition(nums) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n · total)`.
- **Space:** `O(total)`.
