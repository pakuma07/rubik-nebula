# Russian Doll Envelopes

> Sort width asc, height desc → LIS on heights. LC 354 · 🔴 Hard

## Problem
Each envelope has width and height; one fits inside another only if **both** dimensions are strictly larger. Return the maximum number of nested envelopes.

## 🧮 Math / Recurrence
Sort by width ascending, breaking ties by height **descending**, then run LIS on the height sequence:

$$
\text{answer} = \text{LIS}\big(h_{\sigma(1)}, h_{\sigma(2)}, \dots\big), \quad \sigma:\ w\ \uparrow,\ h\ \downarrow\ \text{on ties}
$$

## 🧠 Logic
Sorting by width turns the 2D nesting into a 1D LIS on heights — a later envelope already has `≥` width. The descending-height tiebreak is the trick: among equal widths, a *decreasing* height order means no two equal-width envelopes can both appear in an increasing-height subsequence, correctly enforcing **strict** width nesting.

## 🔢 Iteration trace (`[[5,4],[6,4],[6,7],[2,3]]`)
- Sorted: `[2,3],[5,4],[6,7],[6,4]`. Heights: `[3,4,7,4]`.
- LIS of heights = `3,4,7` → **3**.

## 🐍 Python
```python
import bisect

def max_envelopes(envelopes: list[list[int]]) -> int:
    envelopes.sort(key=lambda e: (e[0], -e[1]))
    tails: list[int] = []
    for _, h in envelopes:
        i = bisect.bisect_left(tails, h)
        if i == len(tails):
            tails.append(h)
        else:
            tails[i] = h
    return len(tails)


if __name__ == "__main__":
    print(max_envelopes([[5, 4], [6, 4], [6, 7], [2, 3]]))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxEnvelopes(vector<vector<int>>& envelopes) {
    sort(envelopes.begin(), envelopes.end(), [](auto& a, auto& b) {
        return a[0] != b[0] ? a[0] < b[0] : a[1] > b[1];
    });
    vector<int> tails;
    for (auto& e : envelopes) {
        int h = e[1];
        auto it = lower_bound(tails.begin(), tails.end(), h);
        if (it == tails.end()) tails.push_back(h);
        else *it = h;
    }
    return tails.size();
}

int main() {
    vector<vector<int>> env = {{5, 4}, {6, 4}, {6, 7}, {2, 3}};
    cout << maxEnvelopes(env) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
