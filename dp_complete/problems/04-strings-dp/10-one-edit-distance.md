# One Edit Distance

> Check for exactly one edit in linear time. LC 161 · 🟡 Medium

## Problem
Return whether `a` and `b` are **exactly one** edit (insert, delete, or replace) apart.

## 🧮 Math / Recurrence
No full table is needed; with `|n − m| > 1` it is immediately false. Otherwise scan to the first mismatch and branch:

$$
\text{equal length} \Rightarrow \text{rest must match (replace)}
$$
$$
\text{differ by one} \Rightarrow \text{skip one char of the longer, rest must match}
$$

## 🧠 Logic
"Exactly one edit" forbids identical strings and length gaps above 1. A single replace keeps lengths equal and requires the suffixes after the first mismatch to match. A single insert/delete makes lengths differ by 1, so skipping the extra character in the longer string must leave equal tails.

## 🔢 Iteration trace (`a = "abcde"`, `b = "abXde"`)
- Same length; first mismatch at index 2 (`c` vs `X`).
- Suffix `"de" == "de"` ⟹ one replace ⟹ **true**.

## 🐍 Python
```python
def is_one_edit_distance(a: str, b: str) -> bool:
    n, m = len(a), len(b)
    if abs(n - m) > 1:
        return False
    if n > m:                                # ensure a is shorter or equal
        return is_one_edit_distance(b, a)
    for i in range(n):
        if a[i] != b[i]:
            if n == m:
                return a[i + 1:] == b[i + 1:]     # replace
            return a[i:] == b[i + 1:]             # insert into a
    return n + 1 == m                        # b has one extra trailing char


if __name__ == "__main__":
    print(is_one_edit_distance("abcde", "abXde"))   # True
    print(is_one_edit_distance("abc", "abc"))       # False
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
using namespace std;

bool isOneEditDistance(string a, string b) {
    int n = a.size(), m = b.size();
    if (abs(n - m) > 1) return false;
    if (n > m) return isOneEditDistance(b, a);   // a shorter or equal
    for (int i = 0; i < n; ++i)
        if (a[i] != b[i]) {
            if (n == m) return a.substr(i + 1) == b.substr(i + 1);  // replace
            return a.substr(i) == b.substr(i + 1);                  // insert
        }
    return n + 1 == m;
}

int main() {
    cout << boolalpha << isOneEditDistance("abcde", "abXde") << " "
         << isOneEditDistance("abc", "abc") << "\n";   // true false
}
```

## ⏱️ Complexity
- **Time:** `O(n + m)`.
- **Space:** `O(1)` (ignoring substring copies).
