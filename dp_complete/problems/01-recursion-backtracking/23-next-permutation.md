# Next Permutation

> In-place next lexicographic arrangement. LC 31 · 🟡 Medium

## Problem
Rearrange numbers into the **next** greater permutation in lexicographic order, in place. If none exists (the array is descending), wrap to the smallest (ascending). E.g. `[1,2,3] → [1,3,2]`, `[3,2,1] → [1,2,3]`.

## 🧮 Math / Recurrence
This is **iterative**, not recursive. Three steps:
1. Find the largest `i` with `a[i] < a[i+1]` (the **pivot**).
2. Find the largest `j > i` with `a[j] > a[i]`; swap `a[i]`, `a[j]`.
3. Reverse the suffix `a[i+1 …]`.

$$
\text{pivot } i = \max\{\, i : a_i < a_{i+1} \,\}
$$

## 🧠 Logic
To get the *next* permutation we make the smallest possible increase:
- The suffix after the pivot is non-increasing (already the largest arrangement), so the pivot is where we can still grow.
- Swapping the pivot with the **smallest element larger than it** to its right increases the prefix minimally.
- Reversing the suffix turns it into its smallest (ascending) order, completing the minimal next step.

If no pivot exists, the array is the last permutation → reverse the whole thing.

## 🔢 Iteration trace (`[1,2,3,6,5,4]`)
| Step | Action | Array |
|------|--------|-------|
| start | — | `1 2 3 6 5 4` |
| 1 find pivot | `a[2]=3 < a[3]=6` → i=2 | pivot=3 |
| 2 find j | largest `>3` on right = `4` (index 5) | swap 3↔4 → `1 2 4 6 5 3` |
| 3 reverse suffix | reverse `[6,5,3]` | `1 2 4 3 5 6` |

**Answer = `[1,2,4,3,5,6]`.**

## 🐍 Python
```python
def next_permutation(a: list[int]) -> None:
    n = len(a)
    i = n - 2
    while i >= 0 and a[i] >= a[i + 1]:      # 1. find pivot
        i -= 1
    if i >= 0:
        j = n - 1
        while a[j] <= a[i]:                 # 2. find next-larger
            j -= 1
        a[i], a[j] = a[j], a[i]
    a[i + 1:] = reversed(a[i + 1:])         # 3. reverse suffix


if __name__ == "__main__":
    arr = [1, 2, 3, 6, 5, 4]
    next_permutation(arr)
    print(arr)   # [1, 2, 4, 3, 5, 6]
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

void nextPermutation(vector<int>& a) {
    int n = a.size(), i = n - 2;
    while (i >= 0 && a[i] >= a[i + 1]) --i;       // 1. pivot
    if (i >= 0) {
        int j = n - 1;
        while (a[j] <= a[i]) --j;                 // 2. next-larger
        swap(a[i], a[j]);
    }
    reverse(a.begin() + i + 1, a.end());          // 3. reverse suffix
}

int main() {
    vector<int> a = {1, 2, 3, 6, 5, 4};
    nextPermutation(a);
    for (int x : a) cout << x << ' ';             // 1 2 4 3 5 6
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n)` — a few linear scans.
- **Space:** `O(1)` in place.
