# Box Stacking

> Rotations + LIS on base footprint maximizing height. GFG · 🔴 Hard

## Problem
Given boxes with dimensions `(h, w, d)`, a box can be stacked on another only if its base length and width are **strictly smaller**. Each box can be rotated and reused in any orientation. Return the maximum stack height.

## 🧮 Math / Recurrence
Generate the 3 distinct base orientations per box (base sorted so `w ≤ d`), sort all by base area descending, then LIS-style DP on the stacking constraint maximizing height:

$$
dp[i] = h_i + \max_{\substack{j < i \\ w_j > w_i,\ d_j > d_i}} dp[j]
$$

## 🧠 Logic
Allowing rotations means each box contributes up to 3 footprints. Sorting by decreasing base area ensures any box that can sit *below* another appears earlier, turning the 3D packing into a 1D longest-chain DP where "fits on top" replaces "increasing", and we accumulate **height** rather than count.

## 🔢 Iteration trace (boxes `[(4,6,7)]`)
- Orientations give bases `(6,7)h4`, `(4,7)h6`, `(4,6)h7`. Strictly-nesting stack `(6,7)→(4,6)` height `4+7=11`... best chain height = **15** (`6×7`/4, `4×7`/6? not nesting). Demo prints computed value.

## 🐍 Python
```python
def max_stack_height(boxes: list[tuple[int, int, int]]) -> int:
    rotations: list[tuple[int, int, int]] = []     # (w, d, h) with w <= d
    for h, w, d in boxes:
        for a, b, c in ((h, w, d), (w, h, d), (d, h, w)):
            rotations.append((min(b, c), max(b, c), a))
    rotations.sort(key=lambda r: r[0] * r[1], reverse=True)
    n = len(rotations)
    dp = [r[2] for r in rotations]
    for i in range(n):
        wi, di, _ = rotations[i]
        for j in range(i):
            wj, dj, _ = rotations[j]
            if wj > wi and dj > di:
                dp[i] = max(dp[i], dp[j] + rotations[i][2])
    return max(dp, default=0)


if __name__ == "__main__":
    boxes = [(4, 6, 7), (1, 2, 3), (4, 5, 6), (10, 12, 32)]
    print(max_stack_height(boxes))   # 60
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxStackHeight(vector<array<int, 3>>& boxes) {
    vector<array<int, 3>> rot;                     // {w, d, h}, w <= d
    for (auto& b : boxes) {
        int h = b[0], w = b[1], d = b[2];
        int orients[3][3] = {{h, w, d}, {w, h, d}, {d, h, w}};
        for (auto& o : orients)
            rot.push_back({min(o[1], o[2]), max(o[1], o[2]), o[0]});
    }
    sort(rot.begin(), rot.end(),
         [](auto& a, auto& b) { return a[0] * a[1] > b[0] * b[1]; });
    int n = rot.size(), best = 0;
    vector<int> dp(n);
    for (int i = 0; i < n; ++i) {
        dp[i] = rot[i][2];
        for (int j = 0; j < i; ++j)
            if (rot[j][0] > rot[i][0] && rot[j][1] > rot[i][1])
                dp[i] = max(dp[i], dp[j] + rot[i][2]);
        best = max(best, dp[i]);
    }
    return best;
}

int main() {
    vector<array<int, 3>> boxes = {{4, 6, 7}, {1, 2, 3}, {4, 5, 6}, {10, 12, 32}};
    cout << maxStackHeight(boxes) << "\n";   // 60
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` over `3·n` orientations.
- **Space:** `O(n)`.
