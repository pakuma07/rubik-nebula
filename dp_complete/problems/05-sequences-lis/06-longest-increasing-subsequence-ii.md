# Longest Increasing Subsequence II

> LIS with gap ≤ k → segment tree over values. LC 2407 · 🔴 Hard

## Problem
Find the longest strictly increasing subsequence such that consecutive chosen values differ by **at most `k`**.

## 🧮 Math / Recurrence
`dp[v]` = longest valid subsequence ending with value `v`. Processing each value `x` left to right:

$$
dp[x] = 1 + \max_{x-k \le v < x} dp[v]
$$

The range-max query and point update are served by a **segment tree** keyed on value.

## 🧠 Logic
A plain `O(n²)` scan is too slow; the constraint "previous value in `[x−k, x−1]`" is a sliding window over the *value* axis. A max segment tree answers `max dp[v]` on that window in `O(log V)` and updates `dp[x]` in `O(log V)`, giving `O(n log V)` overall.

## 🔢 Iteration trace (`nums = [4,2,1,4,3,4,5,8,15]`, `k = 3`)
- Best chain `1,3,4,5,8` (each step ≤3) → length **5**.

## 🐍 Python
```python
def length_of_lis(nums: list[int], k: int) -> int:
    size = max(nums) + 1
    tree = [0] * (2 * size)              # iterative max segment tree

    def query(lo: int, hi: int) -> int:  # max on [lo, hi)
        res = 0
        lo += size; hi += size
        while lo < hi:
            if lo & 1:
                res = max(res, tree[lo]); lo += 1
            if hi & 1:
                hi -= 1; res = max(res, tree[hi])
            lo >>= 1; hi >>= 1
        return res

    def update(i: int, val: int) -> None:
        i += size
        tree[i] = val
        i >>= 1
        while i:
            tree[i] = max(tree[2 * i], tree[2 * i + 1])
            i >>= 1

    best = 0
    for x in nums:
        lo = max(0, x - k)
        cur = query(lo, x) + 1
        update(x, cur)
        best = max(best, cur)
    return best


if __name__ == "__main__":
    print(length_of_lis([4, 2, 1, 4, 3, 4, 5, 8, 15], 3))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

struct SegTree {
    int n;
    vector<int> t;
    SegTree(int n) : n(n), t(2 * n, 0) {}
    int query(int lo, int hi) {                 // max on [lo, hi)
        int res = 0;
        for (lo += n, hi += n; lo < hi; lo >>= 1, hi >>= 1) {
            if (lo & 1) res = max(res, t[lo++]);
            if (hi & 1) res = max(res, t[--hi]);
        }
        return res;
    }
    void update(int i, int val) {
        for (t[i += n] = val, i >>= 1; i; i >>= 1)
            t[i] = max(t[2 * i], t[2 * i + 1]);
    }
};

int lengthOfLIS(vector<int>& nums, int k) {
    int size = *max_element(nums.begin(), nums.end()) + 1;
    SegTree seg(size);
    int best = 0;
    for (int x : nums) {
        int lo = max(0, x - k);
        int cur = seg.query(lo, x) + 1;
        seg.update(x, cur);
        best = max(best, cur);
    }
    return best;
}

int main() {
    vector<int> nums = {4, 2, 1, 4, 3, 4, 5, 8, 15};
    cout << lengthOfLIS(nums, 3) << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n log V)`.
- **Space:** `O(V)`.
