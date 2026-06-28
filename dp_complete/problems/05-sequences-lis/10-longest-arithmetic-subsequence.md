# Longest Arithmetic Subsequence

> `dp[i][d]` keyed by common difference. LC 1027 · 🟡 Medium

## Problem
Return the length of the longest subsequence of `nums` that forms an arithmetic progression (constant difference between consecutive terms).

## 🧮 Math / Recurrence
`dp[i][d]` = length of the AP ending at index `i` with common difference `d`:

$$
dp[i][d] = dp[j][d] + 1 \quad\text{where } d = nums_i - nums_j,\ j < i
$$

(default 1 if no earlier `j`).

## 🧠 Logic
An arithmetic subsequence is determined by its difference `d`, so we key the DP by `(endpoint, difference)`. For each pair `(j, i)`, the difference `d = nums[i]−nums[j]` extends the best AP ending at `j` with that same difference. A per-index hashmap stores the differences compactly.

## 🔢 Iteration trace (`nums = [3,6,9,12]`)
- Difference 3 throughout → length **4**.

## 🐍 Python
```python
def longest_arith_seq_length(nums: list[int]) -> int:
    dp: list[dict[int, int]] = [dict() for _ in nums]
    best = 1
    for i in range(len(nums)):
        for j in range(i):
            d = nums[i] - nums[j]
            dp[i][d] = dp[j].get(d, 1) + 1
            best = max(best, dp[i][d])
    return best


if __name__ == "__main__":
    print(longest_arith_seq_length([3, 6, 9, 12]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int longestArithSeqLength(vector<int>& nums) {
    int n = nums.size(), best = 1;
    vector<unordered_map<int, int>> dp(n);
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < i; ++j) {
            int d = nums[i] - nums[j];
            auto it = dp[j].find(d);
            dp[i][d] = (it == dp[j].end() ? 1 : it->second) + 1;
            best = max(best, dp[i][d]);
        }
    return best;
}

int main() {
    vector<int> nums = {3, 6, 9, 12};
    cout << longestArithSeqLength(nums) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
