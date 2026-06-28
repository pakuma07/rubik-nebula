# Allocate Minimum Number of Pages

> Binary search on the answer + feasibility partition. GFG · 🟡 Medium

## Problem
Given books with page counts `arr` and `m` students, allocate **contiguous** blocks of books so each student reads a block. Minimize the **maximum** pages any student reads.

## 🧮 Math / Recurrence
Binary search the answer `cap` over `[max(arr), sum(arr)]`; a `cap` is feasible if the array can be split into `≤ m` contiguous parts each with sum `≤ cap`:

$$
\text{feasible}(cap) = \big(\text{greedy parts needed} \le m\big)
$$

## 🧠 Logic
The maximum-pages objective is **monotone**: if some cap works, any larger cap also works. So binary-search the smallest feasible cap. Feasibility is checked greedily — start a new student whenever adding the next book would exceed `cap` — counting the required students in `O(n)`.

## 🔢 Iteration trace (`arr = [12,34,67,90]`, `m = 2`)
- Best split `[12,34,67] | [90]` → max **113**.

## 🐍 Python
```python
def find_pages(arr: list[int], m: int) -> int:
    if m > len(arr):
        return -1

    def students_needed(cap: int) -> int:
        count, cur = 1, 0
        for pages in arr:
            if cur + pages > cap:
                count += 1
                cur = pages
            else:
                cur += pages
        return count

    lo, hi = max(arr), sum(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if students_needed(mid) <= m:
            hi = mid
        else:
            lo = mid + 1
    return lo


if __name__ == "__main__":
    print(find_pages([12, 34, 67, 90], 2))   # 113
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int studentsNeeded(vector<int>& arr, int cap) {
    int count = 1, cur = 0;
    for (int pages : arr) {
        if (cur + pages > cap) { ++count; cur = pages; }
        else cur += pages;
    }
    return count;
}

int findPages(vector<int>& arr, int m) {
    if (m > (int)arr.size()) return -1;
    int lo = *max_element(arr.begin(), arr.end());
    int hi = accumulate(arr.begin(), arr.end(), 0);
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (studentsNeeded(arr, mid) <= m) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}

int main() {
    vector<int> arr = {12, 34, 67, 90};
    cout << findPages(arr, 2) << "\n";   // 113
}
```

## ⏱️ Complexity
- **Time:** `O(n log(sum))`.
- **Space:** `O(1)`.
