# Cheapest Flights Within K Stops

> Bellman-Ford style dp[k][node]. LC 787 · 🟡 Medium

## Problem
Given flights `[from, to, price]`, find the cheapest price from `src` to `dst` using **at most `k` stops** (i.e. `k+1` edges), or `-1`.

## 🧮 Math / Recurrence
`dp[node]` = cheapest cost to reach `node`; relax all edges `k+1` times (Bellman-Ford bounded by hops):

$$
dp^{(i)}[v] = \min\big(dp^{(i)}[v],\ dp^{(i-1)}[u] + price(u, v)\big)
$$

## 🧠 Logic
Limiting stops means limiting edges to `k+1`, exactly the number of Bellman-Ford relaxation rounds. Crucially, each round must relax from a **snapshot** of the previous round's distances so a single round never uses two new edges. After `k+1` rounds, `dp[dst]` is the cheapest bounded-hop cost.

## 🔢 Iteration trace (`n=3`, flights `[[0,1,100],[1,2,100],[0,2,500]]`, src=0, dst=2, k=1)
- Direct `0→2 = 500` vs `0→1→2 = 200` (1 stop) → **200**.

## 🐍 Python
```python
def find_cheapest_price(n: int, flights: list[list[int]], src: int, dst: int, k: int) -> int:
    INF = float("inf")
    dp = [INF] * n
    dp[src] = 0
    for _ in range(k + 1):
        snapshot = dp[:]
        for u, v, w in flights:
            if snapshot[u] + w < dp[v]:
                dp[v] = snapshot[u] + w
    return dp[dst] if dp[dst] != INF else -1


if __name__ == "__main__":
    flights = [[0, 1, 100], [1, 2, 100], [0, 2, 500]]
    print(find_cheapest_price(3, flights, 0, 2, 1))   # 200
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
    const int INF = 1e9;
    vector<int> dp(n, INF);
    dp[src] = 0;
    for (int i = 0; i <= k; ++i) {
        vector<int> snapshot = dp;
        for (auto& f : flights) {
            int u = f[0], v = f[1], w = f[2];
            if (snapshot[u] != INF && snapshot[u] + w < dp[v])
                dp[v] = snapshot[u] + w;
        }
    }
    return dp[dst] == INF ? -1 : dp[dst];
}

int main() {
    vector<vector<int>> flights = {{0, 1, 100}, {1, 2, 100}, {0, 2, 500}};
    cout << findCheapestPrice(3, flights, 0, 2, 1) << "\n";   // 200
}
```

## ⏱️ Complexity
- **Time:** `O(k·E)`.
- **Space:** `O(n)`.
