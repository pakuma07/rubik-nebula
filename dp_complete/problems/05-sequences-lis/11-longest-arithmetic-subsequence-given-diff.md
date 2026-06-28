# Longest Arithmetic Subsequence of Given Difference

> Fixed difference → single hashmap, O(n). LC 1218 · 🟡 Medium

## Problem
Given `arr` and an integer `difference`, return the length of the longest subsequence that is arithmetic with that exact common difference.

## 🧮 Math / Recurrence
Because `d` is fixed, one hashmap `dp[value]` = longest chain ending at that value suffices:

$$
dp[x] = dp[x - d] + 1
$$

## 🧠 Logic
With the difference fixed, the predecessor of value `x` in the AP must be exactly `x − d`. So we only need the best chain ending at `x − d`, looked up in `O(1)`. Processing left to right guarantees that predecessor was already recorded. This collapses the `O(n²)` general version to `O(n)`.

## 🔢 Iteration trace (`arr = [1,5,7,8,5,3,4,2,1]`, `difference = -2`)
- `7→5→3→1` → length **4**.

## 🐍 Python
```python
def longest_subsequence(arr: list[int], difference: int) -> int:
    dp: dict[int, int] = {}
    best = 0
    for x in arr:
        dp[x] = dp.get(x - difference, 0) + 1
        best = max(best, dp[x])
    return best


if __name__ == "__main__":
    print(longest_subsequence([1, 5, 7, 8, 5, 3, 4, 2, 1], -2))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int longestSubsequence(vector<int>& arr, int difference) {
    unordered_map<int, int> dp;
    int best = 0;
    for (int x : arr) {
        dp[x] = dp[x - difference] + 1;
        best = max(best, dp[x]);
    }
    return best;
}

int main() {
    vector<int> arr = {1, 5, 7, 8, 5, 3, 4, 2, 1};
    cout << longestSubsequence(arr, -2) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
