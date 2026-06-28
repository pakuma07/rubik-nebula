# Median of Two Sorted Arrays

> `O(log(m+n))` median by partition binary search. LC 4 · 🔴 Hard

## Problem
Given two sorted arrays, return the median of their combined sorted order in `O(log(m+n))` time.

## 🧮 Math / Recurrence
Binary-search a partition `i` of the **smaller** array `A` (size `m`); the partition `j` of `B` is forced so the two left halves hold exactly half the elements:

$$
j = \frac{m+n+1}{2} - i
$$

A correct partition satisfies the cross conditions:

$$
A[i-1] \le B[j] \quad \wedge \quad B[j-1] \le A[i]
$$

## 🧠 Logic
We don't merge — we just need where the median *splits* the data. Try a cut `i` in `A`; the complementary cut `j` in `B` keeps the combined left side at half size. If `A[i-1] > B[j]` we cut too far right in `A` (move left); if `B[j-1] > A[i]` cut too far left (move right). When both hold, the median comes from the four boundary values `A[i-1], A[i], B[j-1], B[j]`. Searching the smaller array guarantees `O(log min(m,n))`.

## 🔢 Iteration trace (`A=[1,3], B=[2]`)
| i | j | A[i-1] | A[i] | B[j-1] | B[j] | check |
|---|---|--------|------|--------|------|-------|
| 1 | 1 | 1 | 3 | 2 | ∞ | `1≤∞` & `2≤3` ✅ |

Odd total → median = `max(A[i-1], B[j-1]) = max(1,2) = 2`.

## 🐍 Python
```python
def find_median_sorted_arrays(a: list[int], b: list[int]) -> float:
    if len(a) > len(b):
        a, b = b, a                          # ensure a is smaller
    m, n = len(a), len(b)
    lo, hi, half = 0, m, (m + n + 1) // 2
    INF = float("inf")
    while lo <= hi:
        i = (lo + hi) // 2
        j = half - i
        a_left = a[i - 1] if i > 0 else -INF
        a_right = a[i] if i < m else INF
        b_left = b[j - 1] if j > 0 else -INF
        b_right = b[j] if j < n else INF
        if a_left <= b_right and b_left <= a_right:
            if (m + n) % 2:
                return max(a_left, b_left)
            return (max(a_left, b_left) + min(a_right, b_right)) / 2
        elif a_left > b_right:
            hi = i - 1
        else:
            lo = i + 1
    return 0.0


if __name__ == "__main__":
    print(find_median_sorted_arrays([1, 3], [2]))   # 2.0
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

double findMedianSortedArrays(vector<int>& A, vector<int>& B) {
    if (A.size() > B.size()) swap(A, B);
    int m = A.size(), n = B.size();
    int lo = 0, hi = m, half = (m + n + 1) / 2;
    const long long INF = LLONG_MAX;
    while (lo <= hi) {
        int i = (lo + hi) / 2, j = half - i;
        long long aL = i > 0 ? A[i - 1] : -INF, aR = i < m ? A[i] : INF;
        long long bL = j > 0 ? B[j - 1] : -INF, bR = j < n ? B[j] : INF;
        if (aL <= bR && bL <= aR) {
            if ((m + n) & 1) return max(aL, bL);
            return (max(aL, bL) + min(aR, bR)) / 2.0;
        } else if (aL > bR) hi = i - 1;
        else lo = i + 1;
    }
    return 0.0;
}

int main() {
    vector<int> a = {1, 3}, b = {2};
    cout << findMedianSortedArrays(a, b) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(log min(m, n))`.
- **Space:** `O(1)`.
