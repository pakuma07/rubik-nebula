# Maximal Rectangle

> Histogram + monotonic stack per row. LC 85 · 🔴 Hard

## Problem
Given a binary matrix, find the area of the largest rectangle containing only `1`s.

## 🧮 Math / Recurrence
Treat each row as the base of a histogram where `heights[j]` = consecutive `1`s ending at row `i`:

$$
heights[j] = \begin{cases} heights[j]+1 & grid[i][j]=1 \\ 0 & grid[i][j]=0 \end{cases}
$$

then apply *Largest Rectangle in Histogram* per row.

## 🧠 Logic
A maximal rectangle's bottom edge lies on some row; for that row the rectangle is exactly a bar-spanning rectangle in the column-height histogram. Building heights incrementally per row and solving the histogram with a monotonic stack (each bar pops when a shorter bar arrives, computing its widest span) yields `O(m·n)` overall.

## 🔢 Iteration trace
- For the classic 4-row example the largest all-ones rectangle has area **6**.

## 🐍 Python
```python
def maximal_rectangle(matrix: list[list[str]]) -> int:
    if not matrix:
        return 0
    n = len(matrix[0])
    heights = [0] * n
    best = 0
    for row in matrix:
        for j in range(n):
            heights[j] = heights[j] + 1 if row[j] == "1" else 0
        best = max(best, _largest_in_histogram(heights))
    return best


def _largest_in_histogram(heights: list[int]) -> int:
    stack: list[int] = []                       # indices, increasing heights
    best = 0
    for i, h in enumerate(heights + [0]):
        while stack and heights[stack[-1]] >= h:
            top = stack.pop()
            width = i if not stack else i - stack[-1] - 1
            best = max(best, heights[top] * width)
        stack.append(i)
    return best


if __name__ == "__main__":
    grid = [["1", "0", "1", "0", "0"],
            ["1", "0", "1", "1", "1"],
            ["1", "1", "1", "1", "1"],
            ["1", "0", "0", "1", "0"]]
    print(maximal_rectangle(grid))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

int largestInHistogram(vector<int>& heights) {
    heights.push_back(0);
    stack<int> st;
    int best = 0;
    for (int i = 0; i < (int)heights.size(); ++i) {
        while (!st.empty() && heights[st.top()] >= heights[i]) {
            int top = st.top(); st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            best = max(best, heights[top] * width);
        }
        st.push(i);
    }
    heights.pop_back();
    return best;
}

int maximalRectangle(vector<vector<char>>& matrix) {
    if (matrix.empty()) return 0;
    int n = matrix[0].size(), best = 0;
    vector<int> heights(n, 0);
    for (auto& row : matrix) {
        for (int j = 0; j < n; ++j)
            heights[j] = row[j] == '1' ? heights[j] + 1 : 0;
        best = max(best, largestInHistogram(heights));
    }
    return best;
}

int main() {
    vector<vector<char>> grid = {{'1', '0', '1', '0', '0'},
                                 {'1', '0', '1', '1', '1'},
                                 {'1', '1', '1', '1', '1'},
                                 {'1', '0', '0', '1', '0'}};
    cout << maximalRectangle(grid) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(n)`.
