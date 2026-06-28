# Frog Jump

> State = (stone, last jump size); memoize the pair. LC 403 · 🔴 Hard

## Problem
A frog crosses a river on stones at positions `stones[]`. The first jump must be `1` unit. If the last jump was `k`, the next must be `k−1`, `k`, or `k+1` (and land on a stone). Can it reach the last stone?

## 🧮 Math / Recurrence
State is `(position, k)` where `k` is the size of the jump that landed here:

$$
\text{can}(p, k) = \bigvee_{d \in \{k-1, k, k+1\},\ d > 0} \text{can}(p + d, d) \quad \text{if } p+d \text{ is a stone}
$$

Base: reaching the last stone ⟹ true.

## 🧠 Logic
The reachable next jumps depend on the **previous jump length**, so position alone is insufficient state — pair it with `k`. The same `(stone, k)` recurs across many paths, so memoize it (a hash set of reachable jump sizes per stone). This prunes the exponential search to polynomial.

## 🔢 Iteration trace (`stones = [0,1,3,5,6,8,12,17]`)
- Stone 0: jumps {0} → can leap 1 to stone 1.
- Stone 1: jumps {1} → 1±1 → reaches stone 3 (jump 2).
- Stone 3: {2} → reaches stone 5 (jump 2).
- … propagates to stone 17.

`reachable[17]` non-empty ⟹ **true**.

## 🐍 Python
```python
def can_cross(stones: list[int]) -> bool:
    pos = set(stones)
    last = stones[-1]
    from functools import lru_cache

    @lru_cache(maxsize=None)
    def dfs(p: int, k: int) -> bool:
        if p == last:
            return True
        for d in (k - 1, k, k + 1):
            if d > 0 and (p + d) in pos and dfs(p + d, d):
                return True
        return False

    return dfs(stones[0], 0)


if __name__ == "__main__":
    print(can_cross([0, 1, 3, 5, 6, 8, 12, 17]))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <unordered_map>
#include <unordered_set>
#include <vector>
using namespace std;

unordered_set<int> pos;
int last;
unordered_map<long long, bool> memo;

bool dfs(int p, int k) {
    if (p == last) return true;
    long long key = (long long)p * 100000 + k;
    auto it = memo.find(key);
    if (it != memo.end()) return it->second;
    bool ok = false;
    for (int d : {k - 1, k, k + 1})
        if (d > 0 && pos.count(p + d) && dfs(p + d, d)) { ok = true; break; }
    return memo[key] = ok;
}

int main() {
    vector<int> stones = {0, 1, 3, 5, 6, 8, 12, 17};
    pos = unordered_set<int>(stones.begin(), stones.end());
    last = stones.back();
    cout << boolalpha << dfs(stones[0], 0) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` distinct `(stone, k)` states.
- **Space:** `O(n²)` memo.
