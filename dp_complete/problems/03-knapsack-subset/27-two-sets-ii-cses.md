# Two Sets II (CSES)

> Count subset splits of 1..n into equal halves. CSES · 🟡 Medium

## Problem
Count the ways to partition `{1, 2, …, n}` into **two subsets with equal sum**. Answer mod `1e9+7`.

## 🧮 Math / Recurrence
Total `= n(n+1)/2`. If odd, the answer is `0`. Otherwise count subsets summing to `half = total/2`, then divide by 2 (each split is counted from both sides):

$$
dp[s] \mathrel{+}= dp[s - i] \quad (i = 1..n), \qquad \text{ans} = \frac{dp[half]}{2}
$$

## 🧠 Logic
A split is determined by one subset's sum being `half`; its complement gives the other set. Counting subsets to `half` double-counts each unordered partition (subset and complement both reach `half`), so halve the result. Each number `1..n` is a 0/1 item, so the inner capacity loop runs downward.

## 🔢 Iteration trace (`n = 7`)
- `total = 28`, `half = 14`.
- Subsets of `{1..7}` summing to 14, divided by 2 ⟹ **4**.

## 🐍 Python
```python
MOD = 10**9 + 7

def two_sets_ii(n: int) -> int:
    total = n * (n + 1) // 2
    if total % 2:
        return 0
    half = total // 2
    dp = [1] + [0] * half
    for i in range(1, n + 1):
        for s in range(half, i - 1, -1):     # 0/1 → downward
            dp[s] = (dp[s] + dp[s - i]) % MOD
    inv2 = pow(2, MOD - 2, MOD)              # modular halving
    return dp[half] * inv2 % MOD


if __name__ == "__main__":
    print(two_sets_ii(7))   # 4
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

long long power(long long b, long long e) {
    long long r = 1; b %= MOD;
    while (e) { if (e & 1) r = r * b % MOD; b = b * b % MOD; e >>= 1; }
    return r;
}

int twoSetsII(int n) {
    long long total = (long long)n * (n + 1) / 2;
    if (total % 2) return 0;
    int half = total / 2;
    vector<long long> dp(half + 1, 0); dp[0] = 1;
    for (int i = 1; i <= n; ++i)
        for (int s = half; s >= i; --s)
            dp[s] = (dp[s] + dp[s - i]) % MOD;
    return (int)(dp[half] * power(2, MOD - 2) % MOD);
}

int main() {
    cout << twoSetsII(7) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n · total)`.
- **Space:** `O(total)`.
