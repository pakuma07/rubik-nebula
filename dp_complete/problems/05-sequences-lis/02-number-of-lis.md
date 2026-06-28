# Number of Longest Increasing Subsequences

> Track length AND count per index. LC 673 · 🟡 Medium

## Problem
Return the number of longest strictly increasing subsequences in `nums`.

## 🧮 Math / Recurrence
Keep two arrays — `len[i]` (LIS length ending at `i`) and `cnt[i]` (how many achieve it):

$$
\text{for } j < i,\ nums_j < nums_i:\quad
\begin{cases}
len[j] + 1 > len[i] & \Rightarrow len[i] = len[j]+1,\ cnt[i] = cnt[j] \\
len[j] + 1 = len[i] & \Rightarrow cnt[i] \mathrel{+}= cnt[j]
\end{cases}
$$

## 🧠 Logic
A strictly longer chain through `j` **resets** both the best length and its count; an equal-length chain **adds** its count. The answer sums `cnt[i]` over all `i` whose `len[i]` equals the global maximum.

## 🔢 Iteration trace (`nums = [1,3,5,4,7]`)
- LIS length 4. Chains: `1,3,5,7` and `1,3,4,7` → count **2**.

## 🐍 Python
```python
def find_number_of_lis(nums: list[int]) -> int:
    n = len(nums)
    length = [1] * n
    count = [1] * n
    for i in range(n):
        for j in range(i):
            if nums[j] < nums[i]:
                if length[j] + 1 > length[i]:
                    length[i] = length[j] + 1
                    count[i] = count[j]
                elif length[j] + 1 == length[i]:
                    count[i] += count[j]
    longest = max(length)
    return sum(c for l, c in zip(length, count) if l == longest)


if __name__ == "__main__":
    print(find_number_of_lis([1, 3, 5, 4, 7]))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int findNumberOfLIS(vector<int>& nums) {
    int n = nums.size();
    vector<int> length(n, 1), count(n, 1);
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < i; ++j)
            if (nums[j] < nums[i]) {
                if (length[j] + 1 > length[i]) {
                    length[i] = length[j] + 1;
                    count[i] = count[j];
                } else if (length[j] + 1 == length[i]) {
                    count[i] += count[j];
                }
            }
    int longest = *max_element(length.begin(), length.end()), res = 0;
    for (int i = 0; i < n; ++i)
        if (length[i] == longest) res += count[i];
    return res;
}

int main() {
    vector<int> nums = {1, 3, 5, 4, 7};
    cout << findNumberOfLIS(nums) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
