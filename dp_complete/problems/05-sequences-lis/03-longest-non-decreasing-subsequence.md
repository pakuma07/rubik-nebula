# Longest Non-decreasing Subsequence

> Allow equal values via `bisect_right`. GFG · 🟡 Medium

## Problem
Return the length of the longest **non-decreasing** (`≤`) subsequence — duplicates are allowed to extend the chain.

## 🧮 Math / Recurrence
Same `tails` method as LIS, but the binary-search position uses `bisect_right` so equal values append rather than replace:

$$
\text{position} = \texttt{bisect\_right}(tails, x), \qquad \text{length} = |tails|
$$

## 🧠 Logic
With strict LIS we use `bisect_left`, which replaces an equal tail (forbidding repeats). For `≤`, `bisect_right` returns the index *after* equal values, so a duplicate that ties the current tail extends the subsequence by appending. Everything else about the `tails` invariant is unchanged.

## 🔢 Iteration trace (`nums = [1,3,3,2,4]`)
| x | pos (bisect_right) | tails |
|---|--------------------|-------|
| 1 | 0 | [1] |
| 3 | 1 | [1,3] |
| 3 | 2 | [1,3,3] |
| 2 | 1 | [1,2,3] |
| 4 | 3 | [1,2,3,4] |

Answer = **4** (`1,3,3,4`).

## 🐍 Python
```python
import bisect

def longest_non_decreasing(nums: list[int]) -> int:
    tails: list[int] = []
    for x in nums:
        i = bisect.bisect_right(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)


if __name__ == "__main__":
    print(longest_non_decreasing([1, 3, 3, 2, 4]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int longestNonDecreasing(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = upper_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return tails.size();
}

int main() {
    vector<int> nums = {1, 3, 3, 2, 4};
    cout << longestNonDecreasing(nums) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
