# Palindrome Partitioning (All)

> List every palindrome partition (backtracking). LC 131 · 🟡 Medium

## Problem
Return **all** ways to partition `s` so that every substring of the partition is a palindrome. (The minimization counterpart is [Palindrome Partitioning II](22-palindrome-partitioning-ii.md).)

## 🧮 Math / Recurrence
Backtracking over cut positions; explore each palindromic prefix:

$$
\text{partition}(start) = \bigcup_{\substack{end \ge start \\ s[start..end]\ \text{pal}}} \big\{\, s[start..end] \,\big\} \times \text{partition}(end + 1)
$$

A precomputed `pal[i][j]` table makes each palindrome test `O(1)`.

## 🧠 Logic
At each `start`, try every prefix `s[start..end]`; if it is a palindrome, fix it as the next piece and recurse on the remainder. Reaching the end of the string yields one complete partition. This enumerates the full solution set — exponential in count, so no DP collapse, but the palindrome table prunes invalid branches early.

## 🔢 Iteration trace (`s = "aab"`)
- `"a" | "a" | "b"` and `"aa" | "b"`.
- Output = `[["a","a","b"], ["aa","b"]]`.

## 🐍 Python
```python
def partition(s: str) -> list[list[str]]:
    n = len(s)
    pal = [[False] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        for j in range(i, n):
            if s[i] == s[j] and (j - i < 2 or pal[i + 1][j - 1]):
                pal[i][j] = True

    result: list[list[str]] = []
    path: list[str] = []

    def backtrack(start: int) -> None:
        if start == n:
            result.append(path[:])
            return
        for end in range(start, n):
            if pal[start][end]:
                path.append(s[start:end + 1])
                backtrack(end + 1)
                path.pop()

    backtrack(0)
    return result


if __name__ == "__main__":
    print(partition("aab"))   # [['a', 'a', 'b'], ['aa', 'b']]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

void backtrack(int start, const string& s, vector<vector<char>>& pal,
               vector<string>& path, vector<vector<string>>& result) {
    if (start == (int)s.size()) { result.push_back(path); return; }
    for (int end = start; end < (int)s.size(); ++end)
        if (pal[start][end]) {
            path.push_back(s.substr(start, end - start + 1));
            backtrack(end + 1, s, pal, path, result);
            path.pop_back();
        }
}

vector<vector<string>> partition(string s) {
    int n = s.size();
    vector<vector<char>> pal(n, vector<char>(n, false));
    for (int i = n - 1; i >= 0; --i)
        for (int j = i; j < n; ++j)
            if (s[i] == s[j] && (j - i < 2 || pal[i + 1][j - 1])) pal[i][j] = true;
    vector<vector<string>> result;
    vector<string> path;
    backtrack(0, s, pal, path, result);
    return result;
}

int main() {
    for (auto& part : partition("aab")) {
        for (auto& p : part) cout << p << " ";
        cout << "\n";
    }
    // a a b
    // aa b
}
```

## ⏱️ Complexity
- **Time:** `O(n·2^n)` worst case (number of partitions).
- **Space:** `O(n²)` for the palindrome table plus recursion.
