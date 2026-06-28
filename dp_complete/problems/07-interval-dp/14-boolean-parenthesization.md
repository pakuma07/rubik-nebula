# Boolean Parenthesization

> Count true/false ways over operators. GFG · 🔴 Hard

## Problem
Given a boolean expression of `T`/`F` operands separated by operators `&`, `|`, `^`, count the ways to parenthesize it so it evaluates to **true**, modulo `1003`.

## 🧮 Math / Recurrence
`T[i][j]` / `F[i][j]` = ways `expr[i..j]` is true / false. For each operator at position `k`:

$$
\begin{aligned}
\&:\ &T \mathrel{+}= T_l T_r, & F \mathrel{+}= F_l F_r + F_l T_r + T_l F_r \\
|:\ &T \mathrel{+}= T_l T_r + T_l F_r + F_l T_r, & F \mathrel{+}= F_l F_r \\
\wedge:\ &T \mathrel{+}= T_l F_r + F_l T_r, & F \mathrel{+}= T_l T_r + F_l F_r
\end{aligned}
$$

## 🧠 Logic
Like matrix-chain, split on the **last operator** evaluated at index `k`, combining the truth counts of the two sides per the operator's truth table. We track both true and false counts because each operator mixes them (e.g. XOR true needs one side true, one false). Sum over all split points.

## 🔢 Iteration trace (`T|T&F^T`)
- Number of parenthesizations evaluating to true = **4**.

## 🐍 Python
```python
from functools import lru_cache

def count_ways(expr: str) -> int:
    MOD = 1003
    operands = expr[::2]
    operators = expr[1::2]

    @lru_cache(maxsize=None)
    def solve(i: int, j: int) -> tuple[int, int]:
        if i == j:
            return (1, 0) if operands[i] == "T" else (0, 1)
        t = f = 0
        for k in range(i, j):
            lt, lf = solve(i, k)
            rt, rf = solve(k + 1, j)
            op = operators[k]
            if op == "&":
                t += lt * rt
                f += lf * rf + lf * rt + lt * rf
            elif op == "|":
                t += lt * rt + lt * rf + lf * rt
                f += lf * rf
            else:  # ^
                t += lt * rf + lf * rt
                f += lt * rt + lf * rf
        return (t % MOD, f % MOD)

    return solve(0, len(operands) - 1)[0]


if __name__ == "__main__":
    print(count_ways("T|T&F^T"))   # 4
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;
const int MOD = 1003;

int main() {
    string expr = "T|T&F^T";
    string operands, operators;
    for (int i = 0; i < (int)expr.size(); ++i)
        (i % 2 == 0 ? operands : operators) += expr[i];
    int n = operands.size();
    vector<vector<int>> T(n, vector<int>(n, 0)), F(n, vector<int>(n, 0));
    for (int i = 0; i < n; ++i) {
        T[i][i] = operands[i] == 'T';
        F[i][i] = operands[i] == 'F';
    }
    for (int len = 2; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            for (int k = i; k < j; ++k) {
                int lt = T[i][k], lf = F[i][k], rt = T[k + 1][j], rf = F[k + 1][j];
                char op = operators[k];
                if (op == '&') {
                    T[i][j] = (T[i][j] + lt * rt) % MOD;
                    F[i][j] = (F[i][j] + lf * rf + lf * rt + lt * rf) % MOD;
                } else if (op == '|') {
                    T[i][j] = (T[i][j] + lt * rt + lt * rf + lf * rt) % MOD;
                    F[i][j] = (F[i][j] + lf * rf) % MOD;
                } else {
                    T[i][j] = (T[i][j] + lt * rf + lf * rt) % MOD;
                    F[i][j] = (F[i][j] + lt * rt + lf * rf) % MOD;
                }
            }
        }
    cout << T[0][n - 1] << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n²)`.
