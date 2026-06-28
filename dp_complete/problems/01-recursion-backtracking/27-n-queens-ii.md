# N-Queens II (Count Solutions)

> Count, don't store, the valid queen placements. LC 52 · 🔴 Hard

## Problem
Return **how many** distinct ways `n` queens can be placed on an `n×n` board with none attacking another. Same rules as [N-Queens](26-n-queens.md), but only the count is needed.

## 🧮 Math / Recurrence
Identical search; sum leaves instead of recording boards:

$$
\text{count}(r) = \begin{cases}
1 & r = n \\
\displaystyle\sum_{\substack{c:\ \text{safe}(r,c)}} \text{count}(r+1) & r < n
\end{cases}
$$

## 🧠 Logic
Because we don't need the board, we drop the `board` array entirely and use **bitmasks** for columns and diagonals — the fastest standard formulation. A set bit means "attacked." The available cells in a row are `~(cols | d1 | d2)` masked to `n` bits; we iterate over them by extracting the lowest set bit.

## 🔢 Iteration trace (bit trick, one row)
| quantity | meaning |
|----------|---------|
| `cols` | columns already used |
| `d1` (`<<` each row) | ↘ diagonals, shifted left per descent |
| `d2` (`>>` each row) | ↙ diagonals, shifted right per descent |
| `free = ~(cols|d1|d2) & full` | placeable cells |
| `p = free & -free` | lowest available cell (a single bit) |

For `n=4` the recursion yields **2**; `n=8` yields **92**.

## 🐍 Python
```python
def total_n_queens(n: int) -> int:
    full = (1 << n) - 1

    def dfs(cols: int, d1: int, d2: int) -> int:
        if cols == full:
            return 1
        free = ~(cols | d1 | d2) & full
        count = 0
        while free:
            p = free & -free                 # lowest available cell
            free -= p
            count += dfs(cols | p, (d1 | p) << 1, (d2 | p) >> 1)
        return count

    return dfs(0, 0, 0)


if __name__ == "__main__":
    print(total_n_queens(8))   # 92
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;

int full;
int dfs(int cols, int d1, int d2) {
    if (cols == full) return 1;
    int free = ~(cols | d1 | d2) & full;
    int count = 0;
    while (free) {
        int p = free & (-free);              // lowest available cell
        free -= p;
        count += dfs(cols | p, (d1 | p) << 1, (d2 | p) >> 1);
    }
    return count;
}

int totalNQueens(int n) {
    full = (1 << n) - 1;
    return dfs(0, 0, 0);
}

int main() {
    cout << totalNQueens(8) << "\n";   // 92
}
```

## ⏱️ Complexity
- **Time:** `O(n!)` upper bound; bitmask pruning makes it very fast in practice.
- **Space:** `O(n)` recursion depth.
