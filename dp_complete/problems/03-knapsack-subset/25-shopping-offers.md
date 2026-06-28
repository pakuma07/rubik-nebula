# Shopping Offers

> Knapsack over a multi-dimensional needs vector. LC 638 · 🟡 Medium

## Problem
Buy exactly `needs[i]` of each item at unit `price[i]`, but `special` bundle offers may be cheaper. Minimize total spend (offers reusable, never over-buy).

## 🧮 Math / Recurrence
State = the remaining-needs vector. `dp[needs]` = min cost:

$$
dp[needs] = \min\Big(\underbrace{\textstyle\sum_i needs_i \cdot price_i}_{\text{buy singly}},\ \min_{\text{offer } o \le needs} \big(cost_o + dp[needs - o]\big)\Big)
$$

## 🧠 Logic
This is knapsack where the "capacity" is a whole vector of item counts. From a needs state, either pay retail for everything left, or apply any affordable offer (that doesn't exceed any need) and recurse on the reduced needs. Memoize on the tuple of remaining needs.

## 🔢 Iteration trace (`price=[2,5], needs=[3,2], special=[[3,0,5],[1,2,10]]`)
- Offer `[3,0,5]` covers 3 of item A for 5; then need `[0,2]` retail = `2·5=10`. Total 15.
- Offer `[1,2,10]` covers 1 A + 2 B for 10; need `[2,0]` retail = `2·2=4`. Total 14.
- Best = **14**.

## 🐍 Python
```python
from functools import lru_cache

def shopping_offers(price: list[int], special: list[list[int]],
                    needs: list[int]) -> int:
    n = len(price)

    @lru_cache(maxsize=None)
    def dp(state: tuple[int, ...]) -> int:
        best = sum(state[i] * price[i] for i in range(n))    # buy singly
        for offer in special:
            nxt = []
            for i in range(n):
                if offer[i] > state[i]:
                    break
                nxt.append(state[i] - offer[i])
            else:
                best = min(best, offer[n] + dp(tuple(nxt)))
        return best

    return dp(tuple(needs))


if __name__ == "__main__":
    print(shopping_offers([2, 5], [[3, 0, 5], [1, 2, 10]], [3, 2]))   # 14
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <map>
#include <vector>
using namespace std;

vector<int> PRICE;
vector<vector<int>> SPECIAL;
map<vector<int>, int> memo;

int dp(vector<int> state) {
    if (memo.count(state)) return memo[state];
    int n = PRICE.size(), best = 0;
    for (int i = 0; i < n; ++i) best += state[i] * PRICE[i];   // singly
    for (auto& offer : SPECIAL) {
        vector<int> nxt(n);
        bool ok = true;
        for (int i = 0; i < n; ++i) {
            if (offer[i] > state[i]) { ok = false; break; }
            nxt[i] = state[i] - offer[i];
        }
        if (ok) best = min(best, offer[n] + dp(nxt));
    }
    return memo[state] = best;
}

int main() {
    PRICE = {2, 5};
    SPECIAL = {{3, 0, 5}, {1, 2, 10}};
    cout << dp({3, 2}) << "\n";   // 14
}
```

## ⏱️ Complexity
- **Time:** `O(∏(needs_i+1) · |special| · n)`.
- **Space:** product of needs (memo size).
