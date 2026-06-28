# Maximum AND Sum of Array

> Slots with capacity 2, encoded in base-3. LC 2172 · 🔴 Hard

## Problem
Given `nums` and `numSlots` slots (each holds at most **2** numbers), place every number into a slot to maximize `Σ (num AND slotIndex)`.

## 🧮 Math / Recurrence
Encode each slot's occupancy (0, 1, or 2) as a base-3 digit; `mask` is the combined state. Process numbers one at a time (number index = total occupancy = sum of digits):

$$
dp[mask + 3^{s}] = \max\big(\,\cdot\,,\ dp[mask] + (nums[i] \,\&\, (s+1))\big)
$$

for each slot `s` whose digit `< 2`, where `i` = numbers already placed.

## 🧠 Logic
Because a slot can take two numbers, a single bit per slot isn't enough — a **base-3** digit counts 0/1/2 occupants. The number being placed is determined by how many are already placed (the digit-sum of `mask`). Slot indices are 1-based for the AND (`s+1`). We grow `dp` from the empty state to the state where all numbers are placed.

## 🔢 Iteration trace (`nums=[1,2,3,4,5,6]`, `numSlots=3`)
- Optimal pairing → **9**.

## 🐍 Python
```python
def maximum_and_sum(nums: list[int], num_slots: int) -> int:
    pow3 = [1] * (num_slots + 1)
    for i in range(1, num_slots + 1):
        pow3[i] = pow3[i - 1] * 3
    total_states = pow3[num_slots]
    n = len(nums)
    dp = [-1] * total_states
    dp[0] = 0
    best = 0
    for mask in range(total_states):
        if dp[mask] == -1:
            continue
        # number index = sum of base-3 digits
        placed = 0
        m = mask
        for _ in range(num_slots):
            placed += m % 3
            m //= 3
        if placed >= n:
            best = max(best, dp[mask])
            continue
        for s in range(num_slots):
            digit = (mask // pow3[s]) % 3
            if digit < 2:
                nm = mask + pow3[s]
                dp[nm] = max(dp[nm], dp[mask] + (nums[placed] & (s + 1)))
                best = max(best, dp[nm])
    return best


if __name__ == "__main__":
    print(maximum_and_sum([1, 2, 3, 4, 5, 6], 3))   # 9
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maximumANDSum(vector<int>& nums, int numSlots) {
    vector<int> pow3(numSlots + 1, 1);
    for (int i = 1; i <= numSlots; ++i) pow3[i] = pow3[i - 1] * 3;
    int total = pow3[numSlots], n = nums.size(), best = 0;
    vector<int> dp(total, -1);
    dp[0] = 0;
    for (int mask = 0; mask < total; ++mask) {
        if (dp[mask] == -1) continue;
        int placed = 0, m = mask;
        for (int i = 0; i < numSlots; ++i) { placed += m % 3; m /= 3; }
        if (placed >= n) { best = max(best, dp[mask]); continue; }
        for (int s = 0; s < numSlots; ++s) {
            int digit = (mask / pow3[s]) % 3;
            if (digit < 2) {
                int nm = mask + pow3[s];
                dp[nm] = max(dp[nm], dp[mask] + (nums[placed] & (s + 1)));
                best = max(best, dp[nm]);
            }
        }
    }
    return best;
}

int main() {
    vector<int> nums = {1, 2, 3, 4, 5, 6};
    cout << maximumANDSum(nums, 3) << "\n";   // 9
}
```

## ⏱️ Complexity
- **Time:** `O(3^{slots} · slots)`.
- **Space:** `O(3^{slots})`.
