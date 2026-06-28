# Wiggle Subsequence

> Two-state up/down DP. LC 376 · 🟡 Medium

## Problem
A wiggle sequence alternates between strictly increasing and strictly decreasing differences. Return the length of the longest wiggle subsequence of `nums`.

## 🧮 Math / Recurrence
Track two running lengths — `up` (last step went up) and `down` (last step went down):

$$
up = down + 1\ \text{if } nums_i > nums_{i-1}, \qquad
down = up + 1\ \text{if } nums_i < nums_{i-1}
$$

Answer = `max(up, down)`.

## 🧠 Logic
Each new element extends the *opposite* state: a rise must follow a fall, so a new `up` builds on the previous `down`, and vice-versa. Equal consecutive values change nothing. The two counters need only `O(1)` space and capture the longest alternating run greedily.

## 🔢 Iteration trace (`nums = [1,7,4,9,2,5]`)
- All differences alternate (+ − + − +) → length **6**.

## 🐍 Python
```python
def wiggle_max_length(nums: list[int]) -> int:
    if not nums:
        return 0
    up = down = 1
    for i in range(1, len(nums)):
        if nums[i] > nums[i - 1]:
            up = down + 1
        elif nums[i] < nums[i - 1]:
            down = up + 1
    return max(up, down)


if __name__ == "__main__":
    print(wiggle_max_length([1, 7, 4, 9, 2, 5]))   # 6
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int wiggleMaxLength(vector<int>& nums) {
    if (nums.empty()) return 0;
    int up = 1, down = 1;
    for (size_t i = 1; i < nums.size(); ++i) {
        if (nums[i] > nums[i - 1]) up = down + 1;
        else if (nums[i] < nums[i - 1]) down = up + 1;
    }
    return max(up, down);
}

int main() {
    vector<int> nums = {1, 7, 4, 9, 2, 5};
    cout << wiggleMaxLength(nums) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
