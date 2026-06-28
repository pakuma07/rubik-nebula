# Money Sums (CSES)

> List every reachable subset sum via boolean knapsack. CSES · 🟡 Medium

## Problem
Given coin values `x[]`, report **all distinct sums** that can be formed by some subset, in increasing order.

## 🧮 Math / Recurrence
Boolean subset-sum DP up to `total = Σx`:

$$
dp[w] = dp[w] \ \vee\ dp[w - x_i], \qquad dp[0] = \text{true}
$$

Answer = every `w ≥ 1` with `dp[w] = true`.

## 🧠 Logic
Same machinery as [Subset Sum](02-subset-sum.md), but instead of testing one target we mark **all** reachable sums in a single DP table and then enumerate the true cells. The 0/1 downward loop ensures each coin is used at most once per sum.

## 🔢 Iteration trace (`x = [4, 2, 4]`)
- Total = 10. Reachable: `2, 4, 6, 8, 10` (and combinations).
- Output (distinct, sorted): `2 4 6 8 10`.

## 🐍 Python
```python
def money_sums(x: list[int]) -> list[int]:
    total = sum(x)
    dp = [False] * (total + 1)
    dp[0] = True
    for coin in x:
        for w in range(total, coin - 1, -1):
            if dp[w - coin]:
                dp[w] = True
    return [w for w in range(1, total + 1) if dp[w]]


if __name__ == "__main__":
    print(money_sums([4, 2, 4]))   # [2, 4, 6, 8, 10]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

vector<int> moneySums(vector<int>& x) {
    int total = accumulate(x.begin(), x.end(), 0);
    vector<char> dp(total + 1, false);
    dp[0] = true;
    for (int coin : x)
        for (int w = total; w >= coin; --w)
            if (dp[w - coin]) dp[w] = true;
    vector<int> res;
    for (int w = 1; w <= total; ++w) if (dp[w]) res.push_back(w);
    return res;
}

int main() {
    vector<int> x = {4, 2, 4};
    for (int s : moneySums(x)) cout << s << " ";   // 2 4 6 8 10
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n · total)`.
- **Space:** `O(total)`.
