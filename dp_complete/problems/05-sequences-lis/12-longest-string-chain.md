# Longest String Chain

> Sort by length; dp over single-char-deletion predecessors. LC 1048 · 🟡 Medium

## Problem
Word `A` is a predecessor of `B` if inserting exactly one letter into `A` (anywhere) yields `B`. Return the length of the longest possible word chain.

## 🧮 Math / Recurrence
Sort words by length; `dp[w]` = longest chain ending at `w`:

$$
dp[w] = 1 + \max_{p\ =\ w \text{ with one char removed}} dp[p]
$$

## 🧠 Logic
A chain only ever goes from a shorter word to a word one longer, so processing by increasing length means all predecessors are already computed. For each word, try deleting each character to form a candidate predecessor and look it up in the DP map — `O(L²)` per word for length `L`.

## 🔢 Iteration trace (`words = ["a","b","ba","bca","bda","bdca"]`)
- `a→ba→bda→bdca` → length **4**.

## 🐍 Python
```python
def longest_str_chain(words: list[str]) -> int:
    words.sort(key=len)
    dp: dict[str, int] = {}
    best = 0
    for w in words:
        dp[w] = 1
        for i in range(len(w)):
            prev = w[:i] + w[i + 1:]
            if prev in dp:
                dp[w] = max(dp[w], dp[prev] + 1)
        best = max(best, dp[w])
    return best


if __name__ == "__main__":
    print(longest_str_chain(["a", "b", "ba", "bca", "bda", "bdca"]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <unordered_map>
#include <vector>
using namespace std;

int longestStrChain(vector<string>& words) {
    sort(words.begin(), words.end(),
         [](const string& a, const string& b) { return a.size() < b.size(); });
    unordered_map<string, int> dp;
    int best = 0;
    for (const string& w : words) {
        dp[w] = 1;
        for (size_t i = 0; i < w.size(); ++i) {
            string prev = w.substr(0, i) + w.substr(i + 1);
            auto it = dp.find(prev);
            if (it != dp.end()) dp[w] = max(dp[w], it->second + 1);
        }
        best = max(best, dp[w]);
    }
    return best;
}

int main() {
    vector<string> words = {"a", "b", "ba", "bca", "bda", "bdca"};
    cout << longestStrChain(words) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n · L²)`.
- **Space:** `O(n)`.
