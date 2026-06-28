# Reverse a String

> Reverse a string by swapping ends inward. Classic · 🟢 Easy

## Problem
Reverse a string (or char array) in place — e.g. `"hello" → "olleh"`.

## 🧮 Math / Recurrence
Treat the string as indices `[lo, hi]`. Swap the two ends, then recurse on the shrinking middle:

$$
\text{rev}(lo, hi) = \begin{cases}
\text{done} & lo \ge hi \\
\text{swap}(lo, hi);\ \text{rev}(lo+1, hi-1) & lo < hi
\end{cases}
$$

There are $\lfloor n/2 \rfloor$ swaps, so the work is $O(n)$.

## 🧠 Logic
The first and last characters must trade places; once they're fixed, the **same** problem remains on the inner substring. The recursion stops when the two pointers meet or cross (an empty or single middle char is already reversed).

## 🔢 Iteration trace (`"hello"`, indices 0..4)
| Step | lo | hi | swap | string |
|------|----|----|------|--------|
| 1 | 0 | 4 | h↔o | `oellh` |
| 2 | 1 | 3 | e↔l | `olleh` |
| 3 | 2 | 2 | stop (`lo==hi`) | `olleh` |

**Answer = `"olleh"`.**

## 🐍 Python
```python
def reverse(s: list[str], lo: int = 0, hi: int | None = None) -> None:
    if hi is None:
        hi = len(s) - 1
    if lo >= hi:
        return
    s[lo], s[hi] = s[hi], s[lo]
    reverse(s, lo + 1, hi - 1)


if __name__ == "__main__":
    chars = list("hello")
    reverse(chars)
    print("".join(chars))   # olleh
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
using namespace std;

void reverseStr(string& s, int lo, int hi) {
    if (lo >= hi) return;
    swap(s[lo], s[hi]);
    reverseStr(s, lo + 1, hi - 1);
}

int main() {
    string s = "hello";
    reverseStr(s, 0, (int)s.size() - 1);
    cout << s << "\n";   // olleh
}
```

## ⏱️ Complexity
- **Time:** `O(n)` — `n/2` swaps.
- **Space:** `O(n)` recursion stack (or `O(1)` with an iterative two-pointer loop).
