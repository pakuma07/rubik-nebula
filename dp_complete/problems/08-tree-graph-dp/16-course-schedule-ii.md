# Course Schedule II (Ordering)

> Topological sort via Kahn's algorithm. LC 210 · 🟡 Medium

## Problem
Given `numCourses` and prerequisite pairs `[a, b]` (take `b` before `a`), return a valid course order, or an empty array if impossible (a cycle exists).

## 🧮 Math / Recurrence
Kahn's BFS: repeatedly emit zero-indegree nodes and decrement their successors:

$$
\text{order} = \text{sequence of nodes removed when } indeg = 0
$$

A valid full order exists iff all `n` nodes are emitted.

## 🧠 Logic
A topological order exists exactly when the prerequisite graph is a DAG. Starting from courses with no prerequisites, taking one frees its dependents; when a dependent's remaining prerequisites hit zero, it becomes available. If a cycle exists, some nodes never reach indegree 0 and the order is short — signaling impossibility.

## 🔢 Iteration trace (`numCourses=4`, prereqs `[[1,0],[2,0],[3,1],[3,2]]`)
- Valid order e.g. `[0, 1, 2, 3]`.

## 🐍 Python
```python
from collections import deque

def find_order(num_courses: int, prerequisites: list[list[int]]) -> list[int]:
    graph: list[list[int]] = [[] for _ in range(num_courses)]
    indeg = [0] * num_courses
    for a, b in prerequisites:
        graph[b].append(a)
        indeg[a] += 1
    queue = deque(i for i in range(num_courses) if indeg[i] == 0)
    order: list[int] = []
    while queue:
        u = queue.popleft()
        order.append(u)
        for v in graph[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                queue.append(v)
    return order if len(order) == num_courses else []


if __name__ == "__main__":
    print(find_order(4, [[1, 0], [2, 0], [3, 1], [3, 2]]))   # [0, 1, 2, 3]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> graph(numCourses);
    vector<int> indeg(numCourses, 0), order;
    for (auto& p : prerequisites) { graph[p[1]].push_back(p[0]); ++indeg[p[0]]; }
    queue<int> q;
    for (int i = 0; i < numCourses; ++i) if (indeg[i] == 0) q.push(i);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        order.push_back(u);
        for (int v : graph[u]) if (--indeg[v] == 0) q.push(v);
    }
    return (int)order.size() == numCourses ? order : vector<int>{};
}

int main() {
    vector<vector<int>> prereq = {{1, 0}, {2, 0}, {3, 1}, {3, 2}};
    for (int x : findOrder(4, prereq)) cout << x << " ";   // 0 1 2 3
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(V + E)`.
- **Space:** `O(V + E)`.
