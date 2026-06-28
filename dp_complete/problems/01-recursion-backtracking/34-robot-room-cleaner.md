# Robot Room Cleaner

> Clean a grid with only relative movement. LC 489 · 🔴 Hard

## Problem
A robot in an unknown room (grid of open/blocked cells) exposes only an API: `move()` (step forward, returns `false` if blocked), `turnLeft()`, `turnRight()`, and `clean()`. Clean every reachable cell. You never get absolute coordinates.

## 🧮 Math / Recurrence
DFS over **relative** orientation, tracking visited cells in your own coordinate frame:

$$
\text{dfs}(r,c,dir) \to \text{clean};\ \forall\ 4\ \text{turns}:\ \text{if move ok, dfs(next)};\ \text{then "go back"}
$$

Directions cycle as `(dr,dc)`: up `(-1,0)`, right `(0,1)`, down `(1,0)`, left `(0,-1)`.

## 🧠 Logic
Maintain your own `(r,c)` and facing `dir`. Clean the current cell, mark it visited, then for each of the 4 directions: turn to it, try `move()`; if it succeeds recurse, then **physically return** (turn around, move, turn around) so the robot is restored to `(r,c,dir)` before trying the next direction. That "return" step is what makes relative-only navigation work.

## 🔢 Iteration trace (return maneuver)
```mermaid
flowchart TD
    A["clean (r,c), mark visited"] --> B["for each of 4 dirs"]
    B --> C{move() ok & unvisited?}
    C -- yes --> D["dfs(next cell)"]
    D --> E["GO BACK: turn 180°, move, turn 180°"]
    C -- no --> F["turnRight, try next dir"]
    E --> F
```

## 🐍 Python
```python
def clean_room(robot) -> None:
    visited = set()
    # directions clockwise: up, right, down, left
    dirs = [(-1, 0), (0, 1), (1, 0), (0, -1)]

    def go_back() -> None:
        robot.turnRight(); robot.turnRight()
        robot.move()
        robot.turnRight(); robot.turnRight()

    def dfs(r: int, c: int, d: int) -> None:
        robot.clean()
        visited.add((r, c))
        for k in range(4):
            nd = (d + k) % 4
            nr, nc = r + dirs[nd][0], c + dirs[nd][1]
            if (nr, nc) not in visited and robot.move():
                dfs(nr, nc, nd)
                go_back()
            robot.turnRight()               # rotate to next direction

    dfs(0, 0, 0)
```

## ⚙️ C++
```cpp
#include <set>
#include <utility>
using namespace std;

// Provided API:
// struct Robot { bool move(); void turnLeft(); void turnRight(); void clean(); };

set<pair<int,int>> visited;
int dirs[4][2] = {{-1,0},{0,1},{1,0},{0,-1}};   // up,right,down,left

void goBack(Robot& robot) {
    robot.turnRight(); robot.turnRight();
    robot.move();
    robot.turnRight(); robot.turnRight();
}

void dfs(Robot& robot, int r, int c, int d) {
    robot.clean();
    visited.insert({r, c});
    for (int k = 0; k < 4; ++k) {
        int nd = (d + k) % 4;
        int nr = r + dirs[nd][0], nc = c + dirs[nd][1];
        if (!visited.count({nr, nc}) && robot.move()) {
            dfs(robot, nr, nc, nd);
            goBack(robot);
        }
        robot.turnRight();
    }
}

void cleanRoom(Robot& robot) { dfs(robot, 0, 0, 0); }
```

## ⏱️ Complexity
- **Time:** `O(cells)` — each reachable cell cleaned once, with `O(1)` turns/moves around it.
- **Space:** `O(cells)` for the visited set + recursion.
