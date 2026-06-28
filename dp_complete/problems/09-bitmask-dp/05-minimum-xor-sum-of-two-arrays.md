# Minimum XOR Sum of Two Arrays

> Pairing via mask. LC 1879 · 🔴 Hard

## Problem
Given equal-length arrays `nums1` and `nums2`, rearrange `nums2` to minimize `Σ (nums1[i] XOR nums2[i])`.

## 🧮 Math / Recurrence
`dp[mask]` = min XOR sum after pairing the first `popcount(mask)` elements of `nums1`, `mask` = used indices of `nums2`:

$$
dp[mask \,|\, (1 \ll j)] = \min\big(\,\cdot\,,\ dp[mask] + (nums1[i] \oplus nums2[j])\big),\quad i = \text{popcount}(mask)
$$

## 🧠 Logic
Pair `nums1[i]` (where `i` = popcount of the chosen-from-`nums2` mask) with each unused `nums2[j]`, accumulating the XOR. Tracking which `nums2` elements are taken via a bitmask guarantees a perfect matching. The full mask gives the optimum.

## 🔢 Iteration trace (`nums1=[1,2]`, `nums2=[2,3]`)
- 1⊕2=3, 2⊕3=1 → 4; vs 1⊕3=2, 2⊕2=0 → 2 → **2**.

## 🐍 Python
```python
def minimum_xor_sum(nums1: list[int], nums2: list[int]) -> int:
    n = len(nums1)
    INF = float("inf")
    dp = [INF] * (1 << n)
    dp[0] = 0
    for mask in range(1 << n):
        if dp[mask] == INF:
            continue
        i = bin(mask).count("1")
        if i >= n:
            continue
        for j in range(n):
            if not (mask >> j) & 1:
                nm = mask | (1 << j)
                dp[nm] = min(dp[nm], dp[mask] + (nums1[i] ^ nums2[j]))
    return dp[(1 << n) - 1]


if __name__ == "__main__":
    print(minimum_xor_sum([1, 2], [2, 3]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minimumXORSum(vector<int>& nums1, vector<int>& nums2) {
    int n = nums1.size();
    const int INF = 1e9;
    vector<int> dp(1 << n, INF);
    dp[0] = 0;
    for (int mask = 0; mask < (1 << n); ++mask) {
        if (dp[mask] == INF) continue;
        int i = __builtin_popcount(mask);
        if (i >= n) continue;
        for (int j = 0; j < n; ++j)
            if (!((mask >> j) & 1)) {
                int nm = mask | (1 << j);
                dp[nm] = min(dp[nm], dp[mask] + (nums1[i] ^ nums2[j]));
            }
    }
    return dp[(1 << n) - 1];
}

int main() {
    vector<int> a = {1, 2}, b = {2, 3};
    cout << minimumXORSum(a, b) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(2ⁿ · n)`.
- **Space:** `O(2ⁿ)`.
