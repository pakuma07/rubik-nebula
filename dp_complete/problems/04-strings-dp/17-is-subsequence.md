# Is Subsequence

> Two-pointer scan (DP is overkill). LC 392 · 🟢 Easy

## Problem
Return whether `t` is a subsequence of `s` (characters of `t` appear in `s` in order, not necessarily contiguously).

## 🧮 Math / Recurrence
Greedy two-pointer: advance a pointer `j` over `t` whenever it matches the current `s` char:

$$
j \mathrel{+}= [\,s_i = t_j\,] \quad \text{for each } i; \qquad \text{answer} = (j = |t|)
$$

## 🧠 Logic
Matching the **earliest** possible character of `s` to each character of `t` is always safe — taking an earlier match never blocks a later one. So a single linear scan suffices; if the `t`-pointer reaches the end, all of `t` was matched in order. (The LCS DP gives the same answer but is unnecessary here.)

## 🔢 Iteration trace (`s = "ahbgdc"`, `t = "abc"`)
| s char | a | h | b | g | d | c |
|--------|---|---|---|---|---|---|
| t ptr  | →b | b | →c | c | c | →done |

Pointer reaches end of `t` ⟹ **true**.

## 🐍 Python
```python
def is_subsequence(t: str, s: str) -> bool:
    j = 0
    for ch in s:
        if j < len(t) and ch == t[j]:
            j += 1
    return j == len(t)


if __name__ == "__main__":
    print(is_subsequence("abc", "ahbgdc"))   # True
    print(is_subsequence("axc", "ahbgdc"))   # False
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
using namespace std;

bool isSubsequence(string t, string s) {
    size_t j = 0;
    for (char ch : s)
        if (j < t.size() && ch == t[j]) ++j;
    return j == t.size();
}

int main() {
    cout << boolalpha << isSubsequence("abc", "ahbgdc") << " "
         << isSubsequence("axc", "ahbgdc") << "\n";   // true false
}
```

## ⏱️ Complexity
- **Time:** `O(|s|)`.
- **Space:** `O(1)`.
