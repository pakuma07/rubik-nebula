# Minimum Incompatibility

> Partition into k equal groups via masks. LC 1681 · 🔴 Hard

## Problem
Partition `nums` into `k` subsets each of size `n/k`, where every subset has **distinct** values. A subset's incompatibility = `max − min`. Minimize the total incompatibility, or return `-1` if impossible.

## 🧮 Math / Recurrence
Precompute `cost[sub]` for every mask `sub` that has exactly `n/k` bits and all-distinct values (`= max − min`, else invalid). Then DP over masks combining disjoint valid groups:

$$
dp[mask \cup sub] = \min\big(\,\cdot\,,\ dp[mask] + cost[sub]\big),\quad sub \subseteq \overline{mask}
$$

## 🧠 Logic
Each valid group is a mask of size `n/k` with no repeated value. We grow the partition group by group: from a state `mask` (already-placed elements), the next group is any valid submask of the **complement**. Restricting submask enumeration to the lowest unset element (forcing it into the new group) avoids re-ordering groups and keeps it efficient.

## 🔢 Iteration trace (`nums=[1,2,1,4]`, `k=2`)
- Groups {1,2} and {1,4} → (2−1)+(4−1) = 1+3 = **4**.

## 🐍 Python
```python
def minimum_incompatibility(nums: list[int], k: int) -> int:
    n = len(nums)
    size = n // k
    full = (1 << n) - 1
    cost: dict[int, int] = {}
    for mask in range(1 << n):
        if bin(mask).count("1") != size:
            continue
        vals = [nums[i] for i in range(n) if (mask >> i) & 1]
        if len(set(vals)) == size:
            cost[mask] = max(vals) - min(vals)

    INF = float("inf")
    dp = [INF] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] == INF:
            continue
        # force the lowest unset bit into the next group
        low = 0
        while (mask >> low) & 1:
            low += 1
        if low >= n:
            continue
        rem = full ^ mask
        sub = rem
        while sub:
            if (sub >> low) & 1 and sub in cost:
                nm = mask | sub
                dp[nm] = min(dp[nm], dp[mask] + cost[sub])
            sub = (sub - 1) & rem
    return dp[full] if dp[full] != INF else -1


if __name__ == "__main__":
    print(minimum_incompatibility([1, 2, 1, 4], 2))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int minimumIncompatibility(vector<int>& nums, int k) {
    int n = nums.size(), size = n / k, full = (1 << n) - 1;
    unordered_map<int, int> cost;
    for (int mask = 0; mask <= full; ++mask) {
        if (__builtin_popcount(mask) != size) continue;
        int mn = 100, mx = 0, seen = 0; bool ok = true;
        for (int i = 0; i < n; ++i)
            if ((mask >> i) & 1) {
                int b = 1 << nums[i];
                if (seen & b) { ok = false; break; }
                seen |= b; mn = min(mn, nums[i]); mx = max(mx, nums[i]);
            }
        if (ok) cost[mask] = mx - mn;
    }

    const int INF = 1e9;
    vector<int> dp(1 << n, INF);
    dp[0] = 0;
    for (int mask = 0; mask <= full; ++mask) {
        if (dp[mask] == INF) continue;
        int low = 0;
        while ((mask >> low) & 1) ++low;
        if (low >= n) continue;
        int rem = full ^ mask;
        for (int sub = rem; sub; sub = (sub - 1) & rem)
            if (((sub >> low) & 1) && cost.count(sub)) {
                int nm = mask | sub;
                dp[nm] = min(dp[nm], dp[mask] + cost[sub]);
            }
    }
    return dp[full] == INF ? -1 : dp[full];
}

int main() {
    vector<int> nums = {1, 2, 1, 4};
    cout << minimumIncompatibility(nums, 2) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(3ⁿ)` worst case for submask enumeration.
- **Space:** `O(2ⁿ)`.
