# Patience Sorting

> The card game that IS the O(n log n) LIS. CF / Classic · 🟡 Medium

## Problem
Deal cards one by one onto piles: a card goes on the leftmost pile whose top is `≥` it, or starts a new pile if none exists. The number of piles equals the LIS length. Return that pile count.

## 🧮 Math / Recurrence
Each card `x` is placed at the binary-search position over the pile tops; the LIS length is the final number of piles:

$$
\text{pile}(x) = \texttt{bisect\_left}(\text{tops}, x), \qquad \text{LIS} = \#\text{piles}
$$

## 🧠 Logic
The pile tops form an increasing array — exactly the `tails` array of the LIS algorithm. Starting a new pile corresponds to extending the longest increasing subsequence by one; placing on an existing pile lowers that pile's top without growing the count. Hence "number of piles = LIS length" (this is the combinatorial origin of the `tails` method).

## 🔢 Iteration trace (`cards = [3,1,4,1,5,9,2,6]`)
| card | tops |
|------|------|
| 3 | [3] |
| 1 | [1] |
| 4 | [1,4] |
| 1 | [1,4] (replace) |
| 5 | [1,4,5] |
| 9 | [1,4,5,9] |
| 2 | [1,2,5,9] |
| 6 | [1,2,5,6] |

Piles = **4** = LIS length.

## 🐍 Python
```python
import bisect

def patience_piles(cards: list[int]) -> int:
    tops: list[int] = []
    for x in cards:
        i = bisect.bisect_left(tops, x)
        if i == len(tops):
            tops.append(x)
        else:
            tops[i] = x
    return len(tops)


if __name__ == "__main__":
    print(patience_piles([3, 1, 4, 1, 5, 9, 2, 6]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int patiencePiles(vector<int>& cards) {
    vector<int> tops;
    for (int x : cards) {
        auto it = lower_bound(tops.begin(), tops.end(), x);
        if (it == tops.end()) tops.push_back(x);
        else *it = x;
    }
    return tops.size();
}

int main() {
    vector<int> cards = {3, 1, 4, 1, 5, 9, 2, 6};
    cout << patiencePiles(cards) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
