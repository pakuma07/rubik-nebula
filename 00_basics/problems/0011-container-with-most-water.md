# Container With Most Water (LeetCode 11)

| Field | Value |
|---|---|
| Source | [LeetCode 11](https://leetcode.com/problems/container-with-most-water/) |
| Difficulty | Medium |
| Primary topic | **Two pointers — opposite ends, greedy elimination** |
| Secondary topic | Area maximization, monotone bottleneck argument |
| Key constraint | $2 \le n \le 10^5$, $0 \le \text{height}[i] \le 10^4$ |

---

## Statement

You are given an integer array `height` of length $n$. Each `height[i]` is a vertical line
at x-coordinate `i`. Pick **two** lines that, together with the x-axis, form a container.
Return the **maximum** amount of water it can store.

The water held by lines at indices `i < j` is:

$$
\text{area}(i, j) = (j - i) \times \min\big(\text{height}[i], \text{height}[j]\big)
$$

### Example

```text
Input:  height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
Output: 49
# lines at index 1 (h=8) and index 8 (h=7):
# width = 8-1 = 7, height = min(8,7) = 7, area = 7*7 = 49
```

---

## WHY: Move the Shorter Wall

Start as **wide** as possible: `left = 0`, `right = n-1`. Width is maximal, so the only way
a *different* pair can beat the current area is by having a taller bottleneck. The bottleneck
is the **shorter** of the two walls — so moving the **taller** wall inward only shrinks width
without raising the cap. We therefore always discard the shorter wall.

```mermaid
flowchart TD
    A["area = (right-left) × min(h[left], h[right])"] --> B{"which wall is shorter?"}
    B -->|"h[left] &lt; h[right]"| C["move left++ (drop shorter wall)"]
    B -->|"h[right] ≤ h[left]"| D["move right-- (drop shorter wall)"]
    C --> E["width shrinks, but bottleneck might rise"]
    D --> E
```

Moving the taller wall is provably useless:

```mermaid
flowchart TD
    A["suppose h[left] &lt; h[right], we move right-- instead"] --> B["new width &lt; old width"]
    B --> C["new height = min(h[left], h[new right]) ≤ h[left]"]
    C --> D["both factors ≤ old ⇒ area can't increase"]
    D --> E["so moving the taller wall is wasted — move the SHORTER one"]
```

---

## Code

```python
def max_area(height):
    left, right = 0, len(height) - 1
    best = 0
    while left < right:
        h = min(height[left], height[right])
        best = max(best, h * (right - left))
        if height[left] < height[right]:
            left += 1          # discard shorter wall
        else:
            right -= 1         # discard shorter (or equal) wall
    return best
```

```cpp
#include <bits/stdc++.h>
using namespace std;

long long maxArea(const vector<int>& height) {
    int left = 0, right = (int)height.size() - 1;
    long long best = 0;
    while (left < right) {
        long long h = min(height[left], height[right]);
        best = max(best, h * (long long)(right - left));
        if (height[left] < height[right]) {
            ++left;            // discard shorter wall
        } else {
            --right;           // discard shorter (or equal) wall
        }
    }
    return best;
}
```

---

## Trace

On `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]`:

| Step | left | right | h[left] | h[right] | width | area | best | Move |
|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 8 | 1 | 7 | 8 | 8 | 8 | left++ (1<7) |
| 1 | 1 | 8 | 8 | 7 | 7 | 49 | 49 | right-- (7≤8) |
| 2 | 1 | 7 | 8 | 3 | 6 | 18 | 49 | right-- |
| 3 | 1 | 6 | 8 | 8 | 5 | 40 | 49 | right-- (tie) |
| 4 | 1 | 5 | 8 | 4 | 4 | 16 | 49 | right-- |
| 5 | 1 | 4 | 8 | 5 | 3 | 15 | 49 | right-- |
| 6 | 1 | 3 | 8 | 2 | 2 | 4 | 49 | right-- |
| 7 | 1 | 2 | 8 | 6 | 1 | 6 | 49 | right-- |
| 8 | 1 | 1 | — | — | — | — | 49 | stop (left==right) |

Pointer movement across the first three steps:

```mermaid
graph LR
    subgraph "step 0  area=8 → left++"
        a0["1(L)"] --- b0["8"] --- c0["6"] --- d0["2"] --- e0["5"] --- f0["4"] --- g0["8"] --- h0["3"] --- i0["7(R)"]
    end
    subgraph "step 1  area=49 ✓ → right--"
        a1["1"] --- b1["8(L)"] --- c1["6"] --- d1["2"] --- e1["5"] --- f1["4"] --- g1["8"] --- h1["3"] --- i1["7(R)"]
    end
    subgraph "step 2  area=18 → right--"
        a2["1"] --- b2["8(L)"] --- c2["6"] --- d2["2"] --- e2["5"] --- f2["4"] --- g2["8"] --- h2["3(R)"] --- i2["7"]
    end
```

The convergence as a flow:

```mermaid
flowchart LR
    S["left=0, right=n-1<br/>best=0"] --> L{"left &lt; right?"}
    L -->|"yes"| U["update best with current area"]
    U --> M["move the shorter wall inward"]
    M --> L
    L -->|"no"| D["return best"]
```

---

## Math & Complexity

Each iteration advances exactly one pointer inward; they begin $n-1$ apart and stop when they
meet, so:

$$
T(n) = O(n), \qquad S(n) = O(1)
$$

The greedy is correct because at every step we hold the **widest remaining** container for
the current bottleneck, and we only ever sacrifice width in exchange for a *chance* at a taller
bottleneck. Brute force over all pairs is $\binom{n}{2} = O(n^2)$ — infeasible at $n = 10^5$.

```mermaid
graph TD
    A["O(n²) all-pairs area scan"] -->|"greedy shorter-wall elimination"| B["O(n) single converging sweep"]
```

---

## Takeaway

When maximizing something governed by a **min/bottleneck** between two ends, converging two
pointers plus a greedy "drop the limiting side" rule gives $O(n)$. The proof pattern — *moving
the non-bottleneck side can never improve the result* — recurs throughout two-pointer optimization
problems.
