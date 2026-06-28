# Maximum Profit in Job Scheduling

> Weighted interval scheduling = sort + binary search + DP. LC 1235 · 🔴 Hard

## Problem
Each job has `[start, end, profit]`. Select non-overlapping jobs to maximize total profit.

## 🧮 Math / Recurrence
Sort jobs by `end`. Let `dp[i]` = best profit using the first `i` jobs (sorted). With `p(i)` = the largest index `j < i` whose job ends `≤ start_i`:

$$
dp[i] = \max\big(\underbrace{dp[i-1]}_{\text{skip } i},\ \underbrace{dp[\,p(i)\,] + profit_i}_{\text{take } i}\big)
$$

## 🧠 Logic
This is knapsack over **time**: taking job `i` forbids any job overlapping it, so we jump back to the last job that finished before `i` starts (found by binary search on end times). Skipping inherits `dp[i−1]`. Sorting by end time makes the "last compatible job" well-defined.

## 🔢 Iteration trace (`jobs = [(1,3,50),(2,4,10),(3,5,40),(3,6,70)]`)
Sorted by end: same order. 
- Take `(1,3,50)`; then `(3,5,40)` compatible → 90; or `(3,6,70)` → 50+70=120.
- Best = **120**.

## 🐍 Python
```python
from bisect import bisect_right

def job_scheduling(start: list[int], end: list[int], profit: list[int]) -> int:
    jobs = sorted(zip(end, start, profit))
    ends = [e for e, _, _ in jobs]
    dp = [0] * (len(jobs) + 1)
    for i, (e, s, p) in enumerate(jobs, 1):
        j = bisect_right(ends, s, 0, i - 1)   # last job ending <= s
        dp[i] = max(dp[i - 1], dp[j] + p)
    return dp[-1]


if __name__ == "__main__":
    print(job_scheduling([1, 2, 3, 3], [3, 4, 5, 6], [50, 10, 40, 70]))   # 120
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int jobScheduling(vector<int>& start, vector<int>& end, vector<int>& profit) {
    int n = start.size();
    vector<array<int, 3>> jobs(n);          // {end, start, profit}
    for (int i = 0; i < n; ++i) jobs[i] = {end[i], start[i], profit[i]};
    sort(jobs.begin(), jobs.end());
    vector<int> ends(n), dp(n + 1, 0);
    for (int i = 0; i < n; ++i) ends[i] = jobs[i][0];
    for (int i = 1; i <= n; ++i) {
        int s = jobs[i - 1][1], p = jobs[i - 1][2];
        int j = upper_bound(ends.begin(), ends.begin() + (i - 1), s) - ends.begin();
        dp[i] = max(dp[i - 1], dp[j] + p);
    }
    return dp[n];
}

int main() {
    vector<int> s = {1, 2, 3, 3}, e = {3, 4, 5, 6}, p = {50, 10, 40, 70};
    cout << jobScheduling(s, e, p) << "\n";   // 120
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)`.
- **Space:** `O(n)`.
