# Painter's Partition Problem

> Split into k segments, minimize the maximum. GFG · 🟡 Medium

## Problem
Given `k` painters and boards of lengths `arr` (each painter paints a **contiguous** block, all working in parallel), minimize the time to paint all boards. Time = max block sum × unit.

## 🧮 Math / Recurrence
Identical structure to [Allocate Minimum Pages](12-allocate-minimum-pages.md): binary-search the maximum block sum, feasible if `≤ k` contiguous segments each `≤ cap`:

$$
\text{answer} = \min\{\,cap : \text{segments}(cap) \le k\,\}
$$

## 🧠 Logic
With parallel painters the finishing time is the heaviest block, so we minimize the maximum segment sum — a classic "minimize the max" that is monotone in `cap`. Binary search the cap and greedily count segments; the smallest feasible cap is the answer. (An `O(n²k)` partition DP also works but is slower.)

## 🔢 Iteration trace (`arr = [10,20,30,40]`, `k = 2`)
- Split `[10,20,30] | [40]` → max 60, or `[10,20] | [30,40]` → max 70; best is **60**.

## 🐍 Python
```python
def painter_partition(arr: list[int], k: int) -> int:
    def segments(cap: int) -> int:
        count, cur = 1, 0
        for length in arr:
            if cur + length > cap:
                count += 1
                cur = length
            else:
                cur += length
        return count

    lo, hi = max(arr), sum(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if segments(mid) <= k:
            hi = mid
        else:
            lo = mid + 1
    return lo


if __name__ == "__main__":
    print(painter_partition([10, 20, 30, 40], 2))   # 60
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int segments(vector<int>& arr, int cap) {
    int count = 1, cur = 0;
    for (int length : arr) {
        if (cur + length > cap) { ++count; cur = length; }
        else cur += length;
    }
    return count;
}

int painterPartition(vector<int>& arr, int k) {
    int lo = *max_element(arr.begin(), arr.end());
    int hi = accumulate(arr.begin(), arr.end(), 0);
    while (lo < hi) {
        int mid = (lo + hi) / 2;
        if (segments(arr, mid) <= k) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}

int main() {
    vector<int> arr = {10, 20, 30, 40};
    cout << painterPartition(arr, 2) << "\n";   // 60
}
```

## ⏱️ Complexity
- **Time:** `O(n log(sum))`.
- **Space:** `O(1)`.
