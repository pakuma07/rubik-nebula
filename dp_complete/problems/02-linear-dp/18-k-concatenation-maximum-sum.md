# K-Concatenation Maximum Sum

> Kadane combined with whole-array repetition. LC 1191 · 🟡 Medium

## Problem
Form an array by repeating `arr` `k` times, then return its maximum subarray sum (mod `1e9+7`). The subarray may be empty (sum 0).

## 🧮 Math / Recurrence
Let `total = sum(arr)`. Three cases collapse into one formula:

$$
\text{ans} =
\begin{cases}
\text{Kadane}(arr) & k = 1 \\
\text{Kadane}(arr \,\Vert\, arr) & k \ge 2,\ total \le 0 \\
\text{Kadane}(arr \,\Vert\, arr) + (k-2)\cdot total & k \ge 2,\ total > 0
\end{cases}
$$

(`\Vert` = concatenation; take `max(ans, 0)` for the empty subarray.)

## 🧠 Logic
The best subarray spans at most **two** copies' boundary plus any number of *whole* middle copies. Kadane over two concatenated copies captures the boundary effect; if one full copy has positive `total`, each of the `k−2` interior copies adds `total`. A non-positive total means repeating never helps, so two copies suffice.

## 🔢 Iteration trace (`arr = [1, 2], k = 3`)
- `total = 3 > 0`.
- `Kadane([1,2,1,2]) = 6`.
- `ans = 6 + (3−2)·3 = 9`.

`max(9, 0) = ` **9** (the whole `[1,2,1,2,1,2]`).

## 🐍 Python
```python
MOD = 10**9 + 7

def k_concatenation_max_sum(arr: list[int], k: int) -> int:
    def kadane(a: list[int]) -> int:
        cur = best = 0
        for x in a:
            cur = max(0, cur + x)
            best = max(best, cur)
        return best

    total = sum(arr)
    if k == 1:
        return kadane(arr) % MOD
    two = kadane(arr + arr)
    if total > 0:
        return (two + (k - 2) * total) % MOD
    return two % MOD


if __name__ == "__main__":
    print(k_concatenation_max_sum([1, 2], 3))   # 9
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

long long kadane(const vector<int>& a) {
    long long cur = 0, best = 0;
    for (int x : a) { cur = max(0LL, cur + x); best = max(best, cur); }
    return best;
}

int kConcatenationMaxSum(vector<int>& arr, int k) {
    long long total = 0;
    for (int x : arr) total += x;
    if (k == 1) return kadane(arr) % MOD;
    vector<int> two = arr; two.insert(two.end(), arr.begin(), arr.end());
    long long ans = kadane(two);
    if (total > 0) ans += (long long)(k - 2) * total;
    return (int)(ans % MOD);
}

int main() {
    vector<int> arr = {1, 2};
    cout << kConcatenationMaxSum(arr, 3) << "\n";   // 9
}
```

## ⏱️ Complexity
- **Time:** `O(n)` — Kadane over at most two copies.
- **Space:** `O(n)` for the doubled array.
