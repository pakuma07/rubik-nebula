# Stickers to Spell Word

> Bitmask state + unbounded sticker reuse. LC 691 · 🔴 Hard

## Problem
Given `stickers` (each a word whose letters you can use, unlimited copies) and a `target`, return the fewest stickers needed to spell `target`, or `-1`.

## 🧮 Math / Recurrence
State = bitmask of which `target` letters are still **missing**. `dp[mask]` = min stickers to cover those letters:

$$
dp[mask] = 1 + \min_{\text{sticker } s} dp[\,mask \setminus \text{letters}(s)\,]
$$

Base `dp[0] = 0`.

## 🧠 Logic
Each `target` position is a bit; applying a sticker clears the bits it can fill (consuming its letters greedily against the still-needed positions). Stickers are reusable, so this is unbounded knapsack where the "capacity" is a subset of needed letters. Memoizing over the `2^len(target)` masks keeps it tractable for small targets.

## 🔢 Iteration trace (`stickers = ["with","example","science"], target = "thehat"`)
- Need letters of "thehat". `"with"` supplies t,h; `"example"` supplies e,a; combine across a few stickers.
- Optimal = **3** stickers.

## 🐍 Python
```python
from collections import Counter
from functools import lru_cache

def min_stickers(stickers: list[str], target: str) -> int:
    counts = [Counter(s) for s in stickers]

    @lru_cache(maxsize=None)
    def dfs(remaining: str) -> int:
        if not remaining:
            return 0
        need = Counter(remaining)
        best = float("inf")
        for c in counts:
            if remaining[0] not in c:         # prune: must cover first letter
                continue
            left = []
            for ch, k in need.items():
                left.append(ch * max(0, k - c.get(ch, 0)))
            res = dfs("".join(sorted(left)))
            if res != float("inf"):
                best = min(best, res + 1)
        return best

    ans = dfs("".join(sorted(target)))
    return -1 if ans == float("inf") else ans


if __name__ == "__main__":
    print(min_stickers(["with", "example", "science"], "thehat"))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
using namespace std;

unordered_map<string, int> memo;
vector<array<int, 26>> counts;

int dfs(string remaining) {
    if (remaining.empty()) return 0;
    if (memo.count(remaining)) return memo[remaining];
    int need[26] = {0};
    for (char ch : remaining) need[ch - 'a']++;
    int best = 1e9;
    for (auto& c : counts) {
        if (c[remaining[0] - 'a'] == 0) continue;   // prune
        string left;
        for (int i = 0; i < 26; ++i) {
            int k = max(0, need[i] - c[i]);
            left += string(k, 'a' + i);
        }
        int res = dfs(left);
        if (res < 1e9) best = min(best, res + 1);
    }
    return memo[remaining] = best;
}

int main() {
    vector<string> stickers = {"with", "example", "science"};
    string target = "thehat";
    for (auto& s : stickers) {
        array<int, 26> c{};
        for (char ch : s) c[ch - 'a']++;
        counts.push_back(c);
    }
    sort(target.begin(), target.end());
    int ans = dfs(target);
    cout << (ans >= 1e9 ? -1 : ans) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** exponential in `len(target)` worst case, pruned heavily by memoization on remaining letters.
- **Space:** `O(2^{len(target)})` memo.
