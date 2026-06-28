# Minimum Number of Increasing Subsequences

> Dilworth: = longest non-increasing subsequence. Classic · 🟡 Medium

## Problem
Partition `nums` into the **fewest** strictly increasing subsequences. Return that minimum count.

## 🧮 Math / Recurrence
By **Dilworth's theorem**, the minimum number of increasing chains equals the length of the longest **non-increasing** subsequence (the largest antichain):

$$
\min(\#\text{increasing chains}) = \text{length of longest non-increasing subsequence}
$$

Compute it greedily: maintain pile-tails and place each value on the leftmost pile whose tail is `< x`... equivalently run a patience-style binary search.

## 🧠 Logic
Any non-increasing subsequence forces every one of its elements into a *different* increasing chain (no two can share one), so its length is a lower bound. Dilworth's theorem says that bound is achievable. Operationally, greedily assigning each element to an existing increasing pile (the one with the largest tail still `< x`) uses exactly that many piles.

## 🔢 Iteration trace (`nums = [2,1,4,3,5]`)
- Longest non-increasing: `2,1` or `4,3` → length **2**. So 2 increasing chains: `[2,4,5]` and `[1,3]`.

## 🐍 Python
```python
import bisect

def min_increasing_subsequences(nums: list[int]) -> int:
    # length of longest non-increasing subsequence
    # = LIS on reversed-and-negated with non-strict handling
    tails: list[int] = []
    for x in nums:
        neg = -x
        i = bisect.bisect_right(tails, neg)
        if i == len(tails):
            tails.append(neg)
        else:
            tails[i] = neg
    return len(tails)


if __name__ == "__main__":
    print(min_increasing_subsequences([2, 1, 4, 3, 5]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minIncreasingSubsequences(vector<int>& nums) {
    vector<int> tails;                       // longest non-increasing subsequence
    for (int x : nums) {
        int neg = -x;
        auto it = upper_bound(tails.begin(), tails.end(), neg);
        if (it == tails.end()) tails.push_back(neg);
        else *it = neg;
    }
    return tails.size();
}

int main() {
    vector<int> nums = {2, 1, 4, 3, 5};
    cout << minIncreasingSubsequences(nums) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
