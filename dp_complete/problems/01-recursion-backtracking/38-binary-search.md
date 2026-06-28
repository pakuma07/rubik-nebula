# Binary Search

> Halve the search range each step. LC 704 · 🟢 Easy

## Problem
Given a **sorted** array and a target, return the index of the target or `-1` if absent.

## 🧮 Math / Recurrence
$$
T(n) = T(n/2) + O(1) = O(\log n)
$$

Compare the middle element and recurse into one half:

$$
\text{search}(lo, hi) = \begin{cases}
mid & a_{mid} = target \\
\text{search}(mid+1, hi) & a_{mid} < target \\
\text{search}(lo, mid-1) & a_{mid} > target
\end{cases}
$$

## 🧠 Logic
A sorted array lets us discard **half** the candidates with one comparison: if the middle is too small, the target must be on the right; if too big, on the left. Repeating halves the range until it's empty (not found) or the target is hit. Use `lo + (hi-lo)//2` to avoid overflow when computing the midpoint.

## 🔢 Iteration trace (target = 7 in `[1,3,5,7,9,11]`)
| lo | hi | mid | a[mid] | action |
|----|----|-----|--------|--------|
| 0 | 5 | 2 | 5 | `5 < 7` → lo = 3 |
| 3 | 5 | 4 | 9 | `9 > 7` → hi = 3 |
| 3 | 3 | 3 | 7 | found → return 3 |

## 🐍 Python
```python
def binary_search(a: list[int], target: int) -> int:
    lo, hi = 0, len(a) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if a[mid] == target:
            return mid
        if a[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1


if __name__ == "__main__":
    print(binary_search([1, 3, 5, 7, 9, 11], 7))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int binarySearch(vector<int>& a, int target) {
    int lo = 0, hi = (int)a.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (a[mid] == target) return mid;
        if (a[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}

int main() {
    vector<int> a = {1, 3, 5, 7, 9, 11};
    cout << binarySearch(a, 7) << "\n";      // 3
}
```

## ⏱️ Complexity
- **Time:** `O(log n)`.
- **Space:** `O(1)` iterative (or `O(log n)` recursive).
