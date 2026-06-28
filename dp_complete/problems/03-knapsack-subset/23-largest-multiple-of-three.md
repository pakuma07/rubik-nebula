# Largest Multiple of Three

> Remainder-bucket knapsack on digits. LC 1363 · 🔴 Hard

## Problem
From the multiset of `digits` (0–9), form the **largest number divisible by 3** (no leading zeros except the number "0"). Return it as a string.

## 🧮 Math / Recurrence
A number is divisible by 3 iff its **digit sum** is. Let `r = (Σ digits) mod 3`. To fix the remainder while removing the fewest (and smallest) digits:

- `r == 1`: remove one digit `≡ 1`, or two digits `≡ 2`.
- `r == 2`: remove one digit `≡ 2`, or two digits `≡ 1`.

Then sort the survivors descending.

## 🧠 Logic
We want the most digits (longer number wins) and, among equal lengths, the largest arrangement (descending). Removing the *fewest* and *smallest* offending digits maximizes length and value. The mod-3 buckets act as the knapsack "items" we trim from. Handle the all-zero result ("0") and empty result specially.

## 🔢 Iteration trace (`digits = [8, 1, 9]`)
- Sum = 18, `r = 0` ⟹ remove nothing.
- Sort descending → `"981"` (divisible by 3).

## 🐍 Python
```python
def largest_multiple_of_three(digits: list[int]) -> str:
    digits.sort()
    total = sum(digits)
    r = total % 3
    buckets = {1: [], 2: []}
    for d in digits:                          # ascending → smallest first
        if d % 3 in (1, 2):
            buckets[d % 3].append(d)

    remove: list[int] = []
    if r == 1:
        if buckets[1]:
            remove = [buckets[1][0]]
        elif len(buckets[2]) >= 2:
            remove = buckets[2][:2]
        else:
            return ""
    elif r == 2:
        if buckets[2]:
            remove = [buckets[2][0]]
        elif len(buckets[1]) >= 2:
            remove = buckets[1][:2]
        else:
            return ""

    rem = list(digits)
    for x in remove:
        rem.remove(x)
    if not rem:
        return ""
    rem.sort(reverse=True)
    if rem[0] == 0:
        return "0"
    return "".join(map(str, rem))


if __name__ == "__main__":
    print(largest_multiple_of_three([8, 1, 9]))   # 981
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <numeric>
#include <string>
#include <vector>
using namespace std;

string largestMultipleOfThree(vector<int>& digits) {
    sort(digits.begin(), digits.end());
    int total = accumulate(digits.begin(), digits.end(), 0);
    int r = total % 3;
    vector<int> b1, b2;
    for (int d : digits) {
        if (d % 3 == 1) b1.push_back(d);
        else if (d % 3 == 2) b2.push_back(d);
    }
    vector<int> remove;
    if (r == 1) {
        if (!b1.empty()) remove = {b1[0]};
        else if (b2.size() >= 2) remove = {b2[0], b2[1]};
        else return "";
    } else if (r == 2) {
        if (!b2.empty()) remove = {b2[0]};
        else if (b1.size() >= 2) remove = {b1[0], b1[1]};
        else return "";
    }
    vector<int> rem = digits;
    for (int x : remove) rem.erase(find(rem.begin(), rem.end(), x));
    if (rem.empty()) return "";
    sort(rem.rbegin(), rem.rend());
    if (rem[0] == 0) return "0";
    string s;
    for (int d : rem) s += char('0' + d);
    return s;
}

int main() {
    vector<int> digits = {8, 1, 9};
    cout << largestMultipleOfThree(digits) << "\n";   // 981
}
```

## ⏱️ Complexity
- **Time:** `O(n log n)` for the sort.
- **Space:** `O(n)`.
