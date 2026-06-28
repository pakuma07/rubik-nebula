# Maximum Length of Pair Chain

> Sort by second element, greedily chain. LC 646 · 🟡 Medium

## Problem
Given pairs `[a, b]` with `a < b`, pair `[a,b]` can follow `[c,d]` iff `b < c`. Return the longest chain you can form.

## 🧮 Math / Recurrence
Sort by the **second** coordinate; greedily take a pair whenever its start exceeds the last chosen end:

$$
\text{take } [a,b] \iff a > \text{cur},\quad \text{then } \text{cur} \leftarrow b
$$

## 🧠 Logic
This is interval scheduling: finishing as early as possible leaves the most room for later pairs, so sorting by end value and greedily extending is optimal. (An `O(n²)` LIS-style DP on "can follow" gives the same answer but is unnecessary.)

## 🔢 Iteration trace (`[[1,2],[2,3],[3,4]]`)
- Sorted by end. Take `[1,2]` (cur=2); skip `[2,3]` (2≯2); take `[3,4]` (cur=4).
- Chain length = **2**.

## 🐍 Python
```python
def find_longest_chain(pairs: list[list[int]]) -> int:
    pairs.sort(key=lambda p: p[1])
    cur = float("-inf")
    count = 0
    for a, b in pairs:
        if a > cur:
            count += 1
            cur = b
    return count


if __name__ == "__main__":
    print(find_longest_chain([[1, 2], [2, 3], [3, 4]]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int findLongestChain(vector<vector<int>>& pairs) {
    sort(pairs.begin(), pairs.end(), [](auto& a, auto& b) { return a[1] < b[1]; });
    int cur = INT_MIN, count = 0;
    for (auto& p : pairs)
        if (p[0] > cur) { ++count; cur = p[1]; }
    return count;
}

int main() {
    vector<vector<int>> pairs = {{1, 2}, {2, 3}, {3, 4}};
    cout << findLongestChain(pairs) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(1)`.
