# Fair Distribution of Cookies

> Submask enumeration. LC 2305 · 🟡 Medium

## Problem
Distribute all cookie bags (`cookies[i]`) to `k` children. Each bag goes to exactly one child; a child's unfairness is the sum of their bags. Minimize the **maximum** unfairness across children.

## 🧮 Math / Recurrence
`dp[c][mask]` = min possible maximum after giving child `c` one share carved from the bags in `mask`. Enumerate every **submask** `sub ⊆ mask` as that child's share:

$$
dp[c][mask] = \min_{sub \subseteq mask} \max\big(sum[sub],\ dp[c-1][mask \setminus sub]\big)
$$

## 🧠 Logic
We hand out shares child by child. For child `c`, the share is any subset of bags **not yet given** — iterate submasks of the remaining mask. The cost is the larger of this child's sum and the best max for the earlier children on the leftover bags. Submask iteration `sub = (sub - 1) & mask` enumerates all subsets of `mask` in `O(2^{popcount})`, and over all masks totals `O(3ⁿ)`.

## 🔢 Iteration trace (`cookies=[8,15,10,20,8]`, `k=2`)
- Best split → **31**.

## 🐍 Python
```python
def distribute_cookies(cookies: list[int], k: int) -> int:
    n = len(cookies)
    full = (1 << n) - 1
    subset_sum = [0] * (1 << n)
    for mask in range(1 << n):
        subset_sum[mask] = sum(cookies[i] for i in range(n) if (mask >> i) & 1)

    INF = float("inf")
    dp = [[INF] * (1 << n) for _ in range(k + 1)]
    dp[0][0] = 0
    for c in range(1, k + 1):
        for mask in range(1 << n):
            sub = mask
            while True:
                prev = dp[c - 1][mask ^ sub]
                if prev != INF:
                    dp[c][mask] = min(dp[c][mask], max(prev, subset_sum[sub]))
                if sub == 0:
                    break
                sub = (sub - 1) & mask
    return dp[k][full]


if __name__ == "__main__":
    print(distribute_cookies([8, 15, 10, 20, 8], 2))   # 31
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int distributeCookies(vector<int>& cookies, int k) {
    int n = cookies.size(), full = (1 << n) - 1;
    vector<int> sum(1 << n, 0);
    for (int mask = 0; mask <= full; ++mask)
        for (int i = 0; i < n; ++i)
            if ((mask >> i) & 1) sum[mask] += cookies[i];

    const int INF = 1e9;
    vector<vector<int>> dp(k + 1, vector<int>(1 << n, INF));
    dp[0][0] = 0;
    for (int c = 1; c <= k; ++c)
        for (int mask = 0; mask <= full; ++mask)
            for (int sub = mask; ; sub = (sub - 1) & mask) {
                int prev = dp[c - 1][mask ^ sub];
                if (prev != INF)
                    dp[c][mask] = min(dp[c][mask], max(prev, sum[sub]));
                if (sub == 0) break;
            }
    return dp[k][full];
}

int main() {
    vector<int> cookies = {8, 15, 10, 20, 8};
    cout << distributeCookies(cookies, 2) << "\n";   // 31
}
```

## ⏱️ Complexity
- **Time:** `O(k · 3ⁿ)`.
- **Space:** `O(k · 2ⁿ)`.
