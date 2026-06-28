# Make Array Strictly Increasing

> DP keyed by (replacements, last value). LC 1187 · 🔴 Hard

## Problem
Given `arr1` and `arr2`, in one operation replace any `arr1[i]` with any element of `arr2`. Return the minimum number of operations to make `arr1` strictly increasing, or `-1`.

## 🧮 Math / Recurrence
Sort and dedupe `arr2`. State `dp[prev]` = min operations to make the processed prefix strictly increasing while the last value is `prev`:

$$
dp'[x] = \min
\begin{cases}
dp[prev] & x = arr1[i],\ x > prev \quad(\text{keep}) \\
dp[prev] + 1 & x = \text{next } arr2 \text{ value} > prev \quad(\text{replace})
\end{cases}
$$

## 🧠 Logic
Tracking the **last placed value** is essential because "strictly increasing" only constrains the previous element. At each index, either keep `arr1[i]` (if it stays above `prev`, free) or replace it with the smallest `arr2` value greater than `prev` (cost 1). A hashmap of reachable `prev → cost` keeps the state space small.

## 🔢 Iteration trace (`arr1 = [1,5,3,6,7]`, `arr2 = [1,3,2,4]`)
- Replace `5` with `4`: `1,4,?` — but `3 < 4`, replace `3`... best is replace `5→2`? Optimal: replace `5` with `2`? not >1... Keep `1`, replace `5→2`? 2>1 ok but then 3>2,6,7 → 1 op.
- Minimum operations = **1**.

## 🐍 Python
```python
def make_array_increasing(arr1: list[int], arr2: list[int]) -> int:
    arr2 = sorted(set(arr2))
    import bisect
    dp = {-1: 0}                                  # last value -> min ops
    for a in arr1:
        new_dp: dict[int, int] = {}
        for prev, ops in dp.items():
            if a > prev:                          # keep a
                new_dp[a] = min(new_dp.get(a, float("inf")), ops)
            j = bisect.bisect_right(arr2, prev)   # replace with arr2[j]
            if j < len(arr2):
                v = arr2[j]
                new_dp[v] = min(new_dp.get(v, float("inf")), ops + 1)
        dp = new_dp
        if not dp:
            return -1
    return min(dp.values())


if __name__ == "__main__":
    print(make_array_increasing([1, 5, 3, 6, 7], [1, 3, 2, 4]))   # 1
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <map>
#include <set>
#include <vector>
using namespace std;

int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
    set<int> s(arr2.begin(), arr2.end());
    vector<int> b(s.begin(), s.end());
    map<int, int> dp{{-1, 0}};                    // last value -> min ops
    for (int a : arr1) {
        map<int, int> nd;
        for (auto [prev, ops] : dp) {
            if (a > prev) {
                auto it = nd.find(a);
                nd[a] = (it == nd.end()) ? ops : min(it->second, ops);
            }
            auto j = upper_bound(b.begin(), b.end(), prev);
            if (j != b.end()) {
                int v = *j;
                auto it = nd.find(v);
                nd[v] = (it == nd.end()) ? ops + 1 : min(it->second, ops + 1);
            }
        }
        dp = nd;
        if (dp.empty()) return -1;
    }
    int best = INT_MAX;
    for (auto [_, ops] : dp) best = min(best, ops);
    return best;
}

int main() {
    vector<int> a = {1, 5, 3, 6, 7}, b = {1, 3, 2, 4};
    cout << makeArrayIncreasing(a, b) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n · m log m)`.
- **Space:** `O(m)`.
