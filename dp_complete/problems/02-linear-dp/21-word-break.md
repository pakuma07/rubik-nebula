# Word Break

> Reachability DP over split points. LC 139 · 🟡 Medium

## Problem
Given string `s` and a dictionary `wordDict`, decide whether `s` can be segmented into a space-separated sequence of dictionary words.

## 🧮 Math / Recurrence
Let `dp[i]` = "prefix `s[0:i]` is segmentable":

$$
dp[i] = \bigvee_{0 \le j < i} \big(dp[j] \ \wedge\ s[j:i] \in \text{dict}\big), \qquad dp[0] = \text{true}
$$

## 🧠 Logic
A prefix of length `i` is breakable if there is a cut `j` where the left part `s[0:j]` is already breakable (`dp[j]`) **and** the right chunk `s[j:i]` is a dictionary word. Scanning all earlier cut points propagates reachability forward; `dp[n]` is the answer.

## 🔢 Iteration trace (`s = "leetcode"`, dict = {leet, code})
| i | s[0:i] | j with dp[j] & word | dp[i] |
|---|--------|---------------------|-------|
| 0 | "" | base | T |
| 4 | "leet" | j=0, "leet"∈dict | T |
| 8 | "leetcode" | j=4, "code"∈dict | **T** |

(other `i` are F). **Answer = true.**

## 🐍 Python
```python
def word_break(s: str, word_dict: list[str]) -> bool:
    words = set(word_dict)
    n = len(s)
    dp = [False] * (n + 1)
    dp[0] = True
    for i in range(1, n + 1):
        for j in range(i):
            if dp[j] and s[j:i] in words:
                dp[i] = True
                break
    return dp[n]


if __name__ == "__main__":
    print(word_break("leetcode", ["leet", "code"]))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <unordered_set>
#include <vector>
using namespace std;

bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> words(wordDict.begin(), wordDict.end());
    int n = s.size();
    vector<bool> dp(n + 1, false);
    dp[0] = true;
    for (int i = 1; i <= n; ++i)
        for (int j = 0; j < i; ++j)
            if (dp[j] && words.count(s.substr(j, i - j))) { dp[i] = true; break; }
    return dp[n];
}

int main() {
    vector<string> dict = {"leet", "code"};
    cout << boolalpha << wordBreak("leetcode", dict) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` substrings (× substring hashing cost).
- **Space:** `O(n)`.
