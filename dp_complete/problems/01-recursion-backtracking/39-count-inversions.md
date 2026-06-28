# Count Inversions

> Count out-of-order pairs while merge-sorting. Classic / CF · 🟡 Medium

## Problem
Count the pairs `(i, j)` with `i < j` and `a[i] > a[j]` (inversions) — a measure of how unsorted the array is.

## 🧮 Math / Recurrence
Piggyback on merge sort. Inversions split into three groups:

$$
\text{inv}(a) = \text{inv}(\text{left}) + \text{inv}(\text{right}) + \text{cross}(\text{left}, \text{right})
$$

During the merge, when `right[j]` is placed before the remaining `left[i…]`, those `(mid - i)` left elements are each greater → count them all at once.

## 🧠 Logic
Brute force checks all `O(n²)` pairs. Merge sort already compares elements across the two sorted halves; whenever an element from the right half is smaller than a left-half element, **every** remaining left element forms an inversion with it. Summing these cross-inversions during merges gives the total in `O(n log n)`.

## 🔢 Iteration trace (merge `[3,5] + [2,4]`)
| compare | place | cross inversions added |
|---------|-------|------------------------|
| 3 vs 2 | 2 (right) | left has `{3,5}` → **+2** |
| 3 vs 4 | 3 (left)  | 0 |
| 5 vs 4 | 4 (right) | left has `{5}` → **+1** |
| — | 5 (left) | 0 |

Cross total = 3. (Plus any inversions inside each half.)

## 🐍 Python
```python
def count_inversions(a: list[int]) -> int:
    def sort_count(arr: list[int]) -> tuple[list[int], int]:
        if len(arr) <= 1:
            return arr, 0
        mid = len(arr) // 2
        left, lc = sort_count(arr[:mid])
        right, rc = sort_count(arr[mid:])
        merged, i, j, cross = [], 0, 0, 0
        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                merged.append(left[i]); i += 1
            else:
                merged.append(right[j]); j += 1
                cross += len(left) - i        # remaining left > right[j]
        merged.extend(left[i:]); merged.extend(right[j:])
        return merged, lc + rc + cross

    return sort_count(a)[1]


if __name__ == "__main__":
    print(count_inversions([3, 5, 2, 4]))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

long long sortCount(vector<int>& a, int lo, int hi) {
    if (hi - lo <= 1) return 0;
    int mid = (lo + hi) / 2;
    long long inv = sortCount(a, lo, mid) + sortCount(a, mid, hi);
    vector<int> tmp;
    int i = lo, j = mid;
    while (i < mid && j < hi) {
        if (a[i] <= a[j]) tmp.push_back(a[i++]);
        else { tmp.push_back(a[j++]); inv += mid - i; }  // count remaining left
    }
    while (i < mid) tmp.push_back(a[i++]);
    while (j < hi)  tmp.push_back(a[j++]);
    for (int k = 0; k < (int)tmp.size(); ++k) a[lo + k] = tmp[k];
    return inv;
}

int main() {
    vector<int> a = {3, 5, 2, 4};
    cout << sortCount(a, 0, a.size()) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)` auxiliary.
