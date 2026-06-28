# Predict the Winner / Stone Game

> dp[i][j] = best score difference current player can force. LC 486 / 877 · 🟡 Medium

## Problem
Two players alternately take a number from **either end** of `nums`, adding it to their score. Return whether player 1 can guarantee a win (score ≥ player 2). See also game-theory variants in [10 — Advanced DP](../10-advanced-dp.md).

## 🧮 Math / Recurrence
`dp[i][j]` = the maximum score **difference** (current player minus opponent) achievable on `nums[i..j]`:

$$
dp[i][j] = \max\big(nums_i - dp[i+1][j],\ nums_j - dp[i][j-1]\big), \qquad dp[i][i] = nums_i
$$

Player 1 wins iff `dp[0][n-1] ≥ 0`.

## 🧠 Logic
By symmetry both players play optimally, so we track the **relative** advantage. Taking `nums[i]` gives that value minus whatever advantage the opponent then secures on the remaining interval (`dp[i+1][j]`), and similarly for the right end. The current player maximizes this difference.

## 🔢 Iteration trace (`nums = [1,5,2]`)
- `dp[0][2] = -2 < 0` ⟹ player 1 **cannot** force a win → `False`.

## 🐍 Python
```python
def predict_the_winner(nums: list[int]) -> bool:
    n = len(nums)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = nums[i]
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])
    return dp[0][n - 1] >= 0


if __name__ == "__main__":
    print(predict_the_winner([1, 5, 2]))   # False
    print(predict_the_winner([1, 5, 233, 7]))   # True
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

bool predictTheWinner(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i = 0; i < n; ++i) dp[i][i] = nums[i];
    for (int len = 2; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
        }
    return dp[0][n - 1] >= 0;
}

int main() {
    vector<int> a = {1, 5, 2}, b = {1, 5, 233, 7};
    cout << boolalpha << predictTheWinner(a) << " " << predictTheWinner(b) << "\n";
    // false true
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
