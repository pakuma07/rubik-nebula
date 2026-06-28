# Best Team With No Conflicts

> Sort by age, then max-sum increasing subsequence. LC 1626 · 🟡 Medium

## Problem
Pick players to maximize total `scores` with **no conflict**: a younger player must not have a strictly higher score than an older teammate.

## 🧮 Math / Recurrence
Sort players by `(age, score)`. Then a valid team is a subsequence whose **scores are non-decreasing**. Let `dp[i]` = best team sum ending with player `i`:

$$
dp[i] = score_i + \max_{\substack{j < i \\ score_j \le score_i}} dp[j]
$$

## 🧠 Logic
After sorting by age, age conflicts vanish for equal-or-older predecessors; the only remaining rule is that scores must not decrease. So it becomes a **maximum-sum non-decreasing subsequence** (an LIS-flavored knapsack) where each player either extends a compatible earlier team or starts fresh.

## 🔢 Iteration trace (`scores=[1,3,5,10,15], ages=[1,2,3,4,5]`)
Already sorted; scores strictly increasing ⟹ take everyone → `1+3+5+10+15 = ` **34**.

## 🐍 Python
```python
def best_team_score(scores: list[int], ages: list[int]) -> int:
    players = sorted(zip(ages, scores))
    n = len(players)
    dp = [0] * n
    best = 0
    for i in range(n):
        s_i = players[i][1]
        dp[i] = s_i
        for j in range(i):
            if players[j][1] <= s_i:
                dp[i] = max(dp[i], dp[j] + s_i)
        best = max(best, dp[i])
    return best


if __name__ == "__main__":
    print(best_team_score([1, 3, 5, 10, 15], [1, 2, 3, 4, 5]))   # 34
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int bestTeamScore(vector<int>& scores, vector<int>& ages) {
    int n = scores.size();
    vector<pair<int, int>> players(n);      // {age, score}
    for (int i = 0; i < n; ++i) players[i] = {ages[i], scores[i]};
    sort(players.begin(), players.end());
    vector<int> dp(n);
    int best = 0;
    for (int i = 0; i < n; ++i) {
        int s = players[i].second;
        dp[i] = s;
        for (int j = 0; j < i; ++j)
            if (players[j].second <= s) dp[i] = max(dp[i], dp[j] + s);
        best = max(best, dp[i]);
    }
    return best;
}

int main() {
    vector<int> scores = {1, 3, 5, 10, 15}, ages = {1, 2, 3, 4, 5};
    cout << bestTeamScore(scores, ages) << "\n";   // 34
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
