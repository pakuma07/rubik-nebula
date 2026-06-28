# Number of Matching Subsequences

> Bucket words by next-needed letter; one scan of S. LC 792 · 🟡 Medium

## Problem
Given string `s` and an array `words`, count how many words are subsequences of `s`.

## 🧮 Math / Recurrence
Not a table DP — an **incremental matching** technique. Keep 26 buckets; each bucket `c` holds iterators of words currently waiting for character `c`. Scanning `s` char by char:

$$
\text{for each word waiting on } c:\ \text{advance it; if finished, count++, else re-bucket on its new letter}
$$

## 🧠 Logic
Running [Is Subsequence](17-is-subsequence.md) per word costs `O(|s|·Σ|word|)`. Instead, process all words **simultaneously**: a word only cares about its *next* needed letter, so park it in that letter's bucket. One pass over `s` advances every word exactly as its needed letters appear, sharing the scan.

## 🔢 Iteration trace (`s = "abcde"`, words = `["a","bb","acd","ace"]`)
- `"a"` finishes at the `a`. `"bb"` advances once at `b`, never the second time → fails.
- `"acd"` and `"ace"` both finish.
- Count = **3**.

## 🐍 Python
```python
from collections import defaultdict

def num_matching_subseq(s: str, words: list[str]) -> int:
    waiting = defaultdict(list)              # letter -> list of (word, index)
    for w in words:
        waiting[w[0]].append((w, 0))
    count = 0
    for ch in s:
        advancers = waiting[ch]
        waiting[ch] = []
        for w, idx in advancers:
            idx += 1
            if idx == len(w):
                count += 1
            else:
                waiting[w[idx]].append((w, idx))
    return count


if __name__ == "__main__":
    print(num_matching_subseq("abcde", ["a", "bb", "acd", "ace"]))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int numMatchingSubseq(string s, vector<string>& words) {
    vector<vector<pair<int, int>>> waiting(26);   // letter -> (wordIdx, charIdx)
    for (int i = 0; i < (int)words.size(); ++i)
        waiting[words[i][0] - 'a'].push_back({i, 0});
    int count = 0;
    for (char ch : s) {
        auto advancers = waiting[ch - 'a'];
        waiting[ch - 'a'].clear();
        for (auto [wi, ci] : advancers) {
            ++ci;
            if (ci == (int)words[wi].size()) ++count;
            else waiting[words[wi][ci] - 'a'].push_back({wi, ci});
        }
    }
    return count;
}

int main() {
    vector<string> words = {"a", "bb", "acd", "ace"};
    cout << numMatchingSubseq("abcde", words) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(|s| + Σ|word|)`.
- **Space:** `O(Σ|word|)`.
