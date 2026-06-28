# Largest Divisible Subset

> LIS on divisibility + reconstruct. LC 368 · 🟡 Medium

## Problem
Return the largest subset of `nums` such that every pair `(a, b)` in it satisfies `a % b == 0` or `b % a == 0`.

## 🧮 Math / Recurrence
Sort ascending, then LIS where "increasing" means "divisible by the previous":

$$
dp[i] = 1 + \max_{\substack{j < i \\ nums_i \,\%\, nums_j = 0}} dp[j], \qquad \text{rebuild via parent pointers}
$$

## 🧠 Logic
After sorting, if `nums[i] % nums[j] == 0` then `nums[i]` is divisible by every element of the chain ending at `j` (divisibility is transitive). So a valid divisible subset is exactly a "divisibility-increasing" subsequence, solvable by the `O(n²)` LIS DP; parent pointers reconstruct the actual subset.

## 🔢 Iteration trace (`nums = [1,2,4,8]`)
- Each divides the next → chain `1,2,4,8`, size **4**.

## 🐍 Python
```python
def largest_divisible_subset(nums: list[int]) -> list[int]:
    if not nums:
        return []
    nums.sort()
    n = len(nums)
    dp = [1] * n
    parent = [-1] * n
    best = 0
    for i in range(n):
        for j in range(i):
            if nums[i] % nums[j] == 0 and dp[j] + 1 > dp[i]:
                dp[i] = dp[j] + 1
                parent[i] = j
        if dp[i] > dp[best]:
            best = i
    res: list[int] = []
    while best != -1:
        res.append(nums[best])
        best = parent[best]
    return res[::-1]


if __name__ == "__main__":
    print(largest_divisible_subset([1, 2, 4, 8]))   # [1, 2, 4, 8]
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

vector<int> largestDivisibleSubset(vector<int>& nums) {
    if (nums.empty()) return {};
    sort(nums.begin(), nums.end());
    int n = nums.size(), best = 0;
    vector<int> dp(n, 1), parent(n, -1);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < i; ++j)
            if (nums[i] % nums[j] == 0 && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                parent[i] = j;
            }
        if (dp[i] > dp[best]) best = i;
    }
    vector<int> res;
    for (int k = best; k != -1; k = parent[k]) res.push_back(nums[k]);
    reverse(res.begin(), res.end());
    return res;
}

int main() {
    vector<int> nums = {1, 2, 4, 8};
    for (int x : largestDivisibleSubset(nums)) cout << x << " ";   // 1 2 4 8
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
