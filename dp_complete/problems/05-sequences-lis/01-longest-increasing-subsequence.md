# Longest Increasing Subsequence

> The canonical LIS — `O(n²)` DP or `O(n log n)` tails. LC 300 · 🟡 Medium

## Problem
Return the length of the longest **strictly increasing** subsequence of `nums`.

## 🧮 Math / Recurrence
`dp[i]` = length of the LIS ending exactly at index `i`:

$$
dp[i] = 1 + \max_{\substack{j < i \\ nums_j < nums_i}} dp[j], \qquad \text{answer} = \max_i dp[i]
$$

`O(n log n)`: maintain `tails`, where `tails[k]` is the smallest tail of any increasing subsequence of length `k+1`; the LIS length is `len(tails)`.

## 🧠 Logic
Fixing the endpoint makes subproblems composable: any LIS ending at `i` is a shorter LIS ending at a smaller earlier element plus `nums[i]`. The faster method keeps `tails` minimal — a smaller tail can only make future extension easier — and binary-searches the replace point, so each element costs `O(log n)`.

## 🔢 Iteration trace (`nums = [10,9,2,5,3,7,101,18]`)
| x | action | tails |
|---|--------|-------|
| 10 | append | [10] |
| 9 | replace | [9] |
| 2 | replace | [2] |
| 5 | append | [2,5] |
| 3 | replace | [2,3] |
| 7 | append | [2,3,7] |
| 101 | append | [2,3,7,101] |
| 18 | replace | [2,3,7,18] |

Answer = **4**.

## 🐍 Python
```python
import bisect

def length_of_lis(nums: list[int]) -> int:
    tails: list[int] = []
    for x in nums:
        i = bisect.bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)


if __name__ == "__main__":
    print(length_of_lis([10, 9, 2, 5, 3, 7, 101, 18]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int lengthOfLIS(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}

int main() {
    vector<int> nums = {10, 9, 2, 5, 3, 7, 101, 18};
    cout << lengthOfLIS(nums) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)` (tails) or `O(n²)` (DP).
- **Space:** `O(n)`.
