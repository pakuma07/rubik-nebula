# Last Stone Weight II

> Sign-assignment = minimum subset-sum difference. LC 1049 · 🟡 Medium

## Problem
Smash stones together; each smash leaves the difference of two weights. Return the smallest possible weight of the final stone (0 if all cancel).

## 🧮 Math / Recurrence
Each stone ends up with a `+` or `−` sign, so the residual is `|Σ ±stone|`. Minimizing it is exactly [Minimum Subset Sum Difference](05-minimum-subset-sum-difference.md):

$$
\text{ans} = \min_{\substack{s \le total/2 \\ dp[s]}} (total - 2s)
$$

## 🧠 Logic
Repeated pairwise smashing is equivalent to partitioning stones into a `+` group and a `−` group; the final stone equals the absolute difference of the two group sums. So we make one group's sum as close to `total/2` as possible via a reachable-sum DP.

## 🔢 Iteration trace (`stones = [2, 7, 4, 1, 8, 1]`)
- `total = 23`, half = 11.
- Best reachable `s ≤ 11` is `11` (`{2,1,8}` or `{7,4}`).
- diff = `23 − 22 = ` **1**.

## 🐍 Python
```python
def last_stone_weight_ii(stones: list[int]) -> int:
    total = sum(stones)
    half = total // 2
    dp = [False] * (half + 1)
    dp[0] = True
    for s in stones:
        for w in range(half, s - 1, -1):
            if dp[w - s]:
                dp[w] = True
    for s in range(half, -1, -1):
        if dp[s]:
            return total - 2 * s
    return total


if __name__ == "__main__":
    print(last_stone_weight_ii([2, 7, 4, 1, 8, 1]))   # 1
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int lastStoneWeightII(vector<int>& stones) {
    int total = accumulate(stones.begin(), stones.end(), 0);
    int half = total / 2;
    vector<char> dp(half + 1, false);
    dp[0] = true;
    for (int s : stones)
        for (int w = half; w >= s; --w)
            if (dp[w - s]) dp[w] = true;
    for (int s = half; s >= 0; --s)
        if (dp[s]) return total - 2 * s;
    return total;
}

int main() {
    vector<int> stones = {2, 7, 4, 1, 8, 1};
    cout << lastStoneWeightII(stones) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n · total)`.
- **Space:** `O(total)`.
