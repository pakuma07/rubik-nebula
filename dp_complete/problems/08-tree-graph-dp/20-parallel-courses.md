# Parallel Courses

> Topological levels = longest chain. LC 1136 · 🟡 Medium

## Problem
Given `n` courses and prerequisite edges, each semester you may take **all** courses whose prerequisites are met. Return the minimum number of semesters, or `-1` if impossible (cycle).

## 🧮 Math / Recurrence
BFS by topological levels; the answer is the number of levels (the longest prerequisite chain):

$$
level[v] = 1 + \max_{(u, v) \in E} level[u]
$$

## 🧠 Logic
Since every available course is taken in parallel, a semester corresponds to one layer of the topological order. The total semesters equal the **longest path** in the DAG (the critical chain). Kahn's BFS naturally processes the graph level by level; if a cycle blocks some courses, fewer than `n` are processed and the answer is `−1`.

## 🔢 Iteration trace (`n=3`, relations `[[1,3],[2,3]]`)
- Semester 1: courses 1,2. Semester 2: course 3. → **2**.

## 🐍 Python
```python
from collections import deque

def minimum_semesters(n: int, relations: list[list[int]]) -> int:
    graph: list[list[int]] = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)
    for u, v in relations:
        graph[u].append(v)
        indeg[v] += 1
    queue = deque(i for i in range(1, n + 1) if indeg[i] == 0)
    semesters = 0
    studied = 0
    while queue:
        semesters += 1
        for _ in range(len(queue)):
            u = queue.popleft()
            studied += 1
            for v in graph[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    queue.append(v)
    return semesters if studied == n else -1


if __name__ == "__main__":
    print(minimum_semesters(3, [[1, 3], [2, 3]]))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int minimumSemesters(int n, vector<vector<int>>& relations) {
    vector<vector<int>> graph(n + 1);
    vector<int> indeg(n + 1, 0);
    for (auto& r : relations) { graph[r[0]].push_back(r[1]); ++indeg[r[1]]; }
    queue<int> q;
    for (int i = 1; i <= n; ++i) if (indeg[i] == 0) q.push(i);
    int semesters = 0, studied = 0;
    while (!q.empty()) {
        ++semesters;
        for (int sz = q.size(); sz > 0; --sz) {
            int u = q.front(); q.pop();
            ++studied;
            for (int v : graph[u]) if (--indeg[v] == 0) q.push(v);
        }
    }
    return studied == n ? semesters : -1;
}

int main() {
    vector<vector<int>> relations = {{1, 3}, {2, 3}};
    cout << minimumSemesters(3, relations) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(V + E)`.
- **Space:** `O(V + E)`.
