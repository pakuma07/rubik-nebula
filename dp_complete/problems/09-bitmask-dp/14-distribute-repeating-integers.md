# Distribute Repeating Integers

> Submask over customer demands. LC 1655 · 🔴 Hard

## Problem
Given `nums` (with repeats) and an array `quantity` of customer demands, can we satisfy **all** customers? Each customer must receive `quantity[i]` copies of a **single** value; different customers may share a value only if enough copies exist.

## 🧮 Math / Recurrence
Let `cnt` = list of distinct-value counts, `need[mask]` = total demand of the customers in `mask`. `dp[i][mask]` = can we satisfy customer-set `mask` using value types `i..` :

$$
dp[i][mask] = dp[i+1][mask] \ \lor\ \bigvee_{\substack{sub \subseteq mask \\ need[sub] \le cnt[i]}} dp[i+1][mask \setminus sub]
$$

## 🧠 Logic
At most 50 customers but only ≤ 50 distinct values, each with a count. For value type `i`, we choose a **submask** of the still-unsatisfied customers whose combined demand fits in `cnt[i]`, assign them all this value, and recurse on the rest with the next value. Precomputing `need[mask]` (sum of demands) lets each submask check be O(1). Either skip value `i` or use it for some affordable subset.

## 🔢 Iteration trace (`nums=[1,1,2,2]`, `quantity=[2,2]`)
- Customer0 ← two 1's, customer1 ← two 2's → **True**.

## 🐍 Python
```python
from collections import Counter

def can_distribute(nums: list[int], quantity: list[int]) -> bool:
    cnt = list(Counter(nums).values())
    m = len(quantity)
    full = (1 << m) - 1
    need = [0] * (1 << m)
    for mask in range(1 << m):
        need[mask] = sum(quantity[i] for i in range(m) if (mask >> i) & 1)

    from functools import lru_cache

    @lru_cache(maxsize=None)
    def solve(i: int, mask: int) -> bool:
        if mask == 0:
            return True
        if i == len(cnt):
            return False
        if solve(i + 1, mask):       # skip this value
            return True
        rem = mask
        sub = rem
        while sub:
            if need[sub] <= cnt[i] and solve(i + 1, mask ^ sub):
                return True
            sub = (sub - 1) & rem
        return False

    return solve(0, full)


if __name__ == "__main__":
    print(can_distribute([1, 1, 2, 2], [2, 2]))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

vector<int> cnt, need;
int M;
unordered_map<long long, bool> memo;

bool solve(int i, int mask) {
    if (mask == 0) return true;
    if (i == (int)cnt.size()) return false;
    long long key = (long long)i * (1 << M) + mask;
    auto it = memo.find(key);
    if (it != memo.end()) return it->second;
    bool ans = solve(i + 1, mask);
    if (!ans) {
        int rem = mask;
        for (int sub = rem; sub; sub = (sub - 1) & rem)
            if (need[sub] <= cnt[i] && solve(i + 1, mask ^ sub)) { ans = true; break; }
    }
    memo[key] = ans;
    return ans;
}

bool canDistribute(vector<int>& nums, vector<int>& quantity) {
    unordered_map<int, int> freq;
    for (int x : nums) ++freq[x];
    cnt.clear();
    for (auto& [k, v] : freq) cnt.push_back(v);
    M = quantity.size();
    need.assign(1 << M, 0);
    for (int mask = 0; mask < (1 << M); ++mask)
        for (int i = 0; i < M; ++i)
            if ((mask >> i) & 1) need[mask] += quantity[i];
    memo.clear();
    return solve(0, (1 << M) - 1);
}

int main() {
    vector<int> nums = {1, 1, 2, 2}, quantity = {2, 2};
    cout << boolalpha << canDistribute(nums, quantity) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(distinct · 3ᵐ)` where `m = customers`.
- **Space:** `O(distinct · 2ᵐ)`.
