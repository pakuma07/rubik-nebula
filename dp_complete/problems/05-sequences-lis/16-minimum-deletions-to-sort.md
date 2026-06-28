# Minimum Deletions to Sort

> Keep the LIS, delete the rest: n − LIS. Classic · 🟢 Easy

## Problem
Return the minimum number of elements to delete from `nums` so the remaining elements are sorted in (strictly) increasing order.

## 🧮 Math / Recurrence
$$
\text{answer} = n - \text{LIS}(nums)
$$

## 🧠 Logic
The elements you keep must already be increasing — i.e. they form an increasing subsequence. To delete as few as possible you keep the **largest** such subsequence, the LIS. Everything else (`n − LIS` elements) must go.

## 🔢 Iteration trace (`nums = [5,2,3,1,4]`)
- LIS = `2,3,4` length 3. Deletions = `5 − 3 = ` **2**.

## 🐍 Python
```python
import bisect

def min_deletions_to_sort(nums: list[int]) -> int:
    tails: list[int] = []
    for x in nums:
        i = bisect.bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(nums) - len(tails)


if __name__ == "__main__":
    print(min_deletions_to_sort([5, 2, 3, 1, 4]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minDeletionsToSort(vector<int>& nums) {
    vector<int> tails;
    for (int x : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), x);
        if (it == tails.end()) tails.push_back(x);
        else *it = x;
    }
    return nums.size() - tails.size();
}

int main() {
    vector<int> nums = {5, 2, 3, 1, 4};
    cout << minDeletionsToSort(nums) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
