# Game of Life

| Meta | Value |
|------|-------|
| **Problem** | Game of Life |
| **Source** | LeetCode 289 |
| **Link** | https://leetcode.com/problems/game-of-life/ |
| **Difficulty** | Medium |
| **Topics** | Implementation, Matrix, Simulation, In-place |
| **Time** | $O(R \cdot C)$ |
| **Space** | $O(1)$ extra (in-place) |

---

## Problem Statement

The board is an $R \times C$ grid of cells, each **live** (`1`) or **dead** (`0`). Every cell interacts with its **eight** neighbors. The next state is computed for all cells **simultaneously** from the current state, using four rules:

1. A live cell with **fewer than two** live neighbors dies (underpopulation).
2. A live cell with **two or three** live neighbors lives on.
3. A live cell with **more than three** live neighbors dies (overpopulation).
4. A dead cell with **exactly three** live neighbors becomes live (reproduction).

Update the board **in place** to the next generation. The challenge: you must not let an already-updated cell corrupt the neighbor count of a cell you process later.

```text
Input board:        Next generation:
 0 1 0               0 0 0
 0 0 1               1 0 1
 1 1 1               0 1 1
 0 0 0               0 1 1
```

---

## Approach (WHY)

The naive fix for the simultaneity rule is to copy the whole board ($O(R \cdot C)$ extra space). We can do better with an **encoding trick** that keeps both the old and new state in a single integer per cell, achieving $O(1)$ extra space.

We use two bits: the **low bit** holds the current state, the **high bit (value 2)** holds the next state. Because every cell's current value is `0` or `1`, reading `cell & 1` always recovers the original state even after we have written the next state into bit 2. After the full sweep, we shift every cell right by one (`cell >>= 1`) to drop the old state and reveal the new one.

```mermaid
flowchart LR
    A["cell value"] --> B["bit 0 = current state<br/>(old)"]
    A --> C["bit 1 = next state<br/>(new)"]
    B --> D["neighbor counting<br/>uses cell &amp; 1"]
    C --> E["final pass:<br/>cell &gt;&gt;= 1"]
```

This works because counting neighbors and writing the future state are kept **independent**: counting only ever looks at bit 0 (the untouched present), while the future is parked in bit 1 until the very end.

```mermaid
stateDiagram-v2
    [*] --> Dead0: 0
    [*] --> Live1: 1
    Dead0 --> Dead0: stays dead (00)
    Dead0 --> WillLive: 3 live neighbors (10)
    Live1 --> Live1: 2 or 3 neighbors (11)
    Live1 --> WillDie: under/over pop (01)
    WillLive --> [*]: shift &rarr; 1
    WillDie --> [*]: shift &rarr; 0
```

---

## Solution

```python
from typing import List

def gameOfLife(board: List[List[int]]) -> None:
    R, C = len(board), len(board[0])
    DIRS = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]

    for r in range(R):
        for c in range(C):
            live = 0
            for dr, dc in DIRS:
                nr, nc = r + dr, c + dc
                if 0 <= nr < R and 0 <= nc < C:
                    live += board[nr][nc] & 1   # read OLD bit only
            # decide next state, store it in bit 1
            if board[r][c] & 1:
                if live == 2 or live == 3:
                    board[r][c] |= 2            # stays live -> 11
            else:
                if live == 3:
                    board[r][c] |= 2            # becomes live -> 10

    for r in range(R):
        for c in range(C):
            board[r][c] >>= 1                   # reveal new state
```

```cpp
#include <bits/stdc++.h>
using namespace std;

void gameOfLife(vector<vector<int>> &board) {
    int R = (int)board.size(), C = (int)board[0].size();
    const int DIRS[8][2] = {{-1,-1},{-1,0},{-1,1},{0,-1},
                            {0,1},{1,-1},{1,0},{1,1}};

    for (int r = 0; r < R; r++) {
        for (int c = 0; c < C; c++) {
            int live = 0;
            for (auto &d : DIRS) {
                int nr = r + d[0], nc = c + d[1];
                if (0 <= nr && nr < R && 0 <= nc && nc < C)
                    live += board[nr][nc] & 1;   // read OLD bit only
            }
            if (board[r][c] & 1) {
                if (live == 2 || live == 3)
                    board[r][c] |= 2;            // stays live -> 11
            } else {
                if (live == 3)
                    board[r][c] |= 2;            // becomes live -> 10
            }
        }
    }

    for (int r = 0; r < R; r++)
        for (int c = 0; c < C; c++)
            board[r][c] >>= 1;                   // reveal new state
}

int main() {
    vector<vector<int>> b = {{0,1,0},{0,0,1},{1,1,1},{0,0,0}};
    gameOfLife(b);
    for (auto &row : b) {
        for (int x : row) cout << x << ' ';
        cout << "\n";
    }
    return 0;
}
```

---

## Trace

Take the cell at `(1, 1)` (value `0`) in the example. Its eight neighbors in the **old** board are `(0,0)=0, (0,1)=1, (0,2)=0, (1,0)=0, (1,2)=1, (2,0)=1, (2,1)=1, (2,2)=1`, summing to `live = 5`. A dead cell needs exactly `3` to revive, and `5 ≠ 3`, so it stays dead — bit 1 is never set, and after the shift it remains `0`.

Now the cell at `(2, 0)` (value `1`): neighbors are `(1,0)=0, (1,1)=0, (2,1)=1, (3,0)=0, (3,1)=0` (edge cell, only five neighbors), so `live = 1`. A live cell with fewer than two live neighbors dies, so we leave bit 1 unset; after the shift it becomes `0`.

```mermaid
flowchart TD
    Cell["cell (r,c)"] --> Count["count live neighbors<br/>using bit 0"]
    Count --> Q1{"currently live?"}
    Q1 -- "yes" --> Q2{"live in {2,3}?"}
    Q1 -- "no" --> Q3{"live == 3?"}
    Q2 -- "yes" --> Set["set bit 1"]
    Q2 -- "no" --> Keep["leave bit 1 = 0"]
    Q3 -- "yes" --> Set
    Q3 -- "no" --> Keep
    Set --> Shift["final: cell &gt;&gt;= 1"]
    Keep --> Shift
```

---

## Diagrams

The 8-neighbor stencil around the center cell:

```mermaid
graph TD
    subgraph "neighborhood of (r,c)"
        nw["(r-1,c-1)"] --- n["(r-1,c)"] --- ne["(r-1,c+1)"]
        w["(r,c-1)"] --- ctr["(r,c)"] --- e["(r,c+1)"]
        sw["(r+1,c-1)"] --- s["(r+1,c)"] --- se["(r+1,c+1)"]
    end
```

Two-pass pipeline — encode the future during the sweep, then reveal it:

```mermaid
sequenceDiagram
    participant Sweep as Pass 1 (encode)
    participant Board as Board (2-bit cells)
    participant Reveal as Pass 2 (shift)
    Sweep->>Board: read bit 0 of neighbors
    Sweep->>Board: write next state into bit 1
    Reveal->>Board: cell >>= 1 for every cell
    Board-->>Reveal: now holds next generation
```

Bit layout legend:

```mermaid
flowchart LR
    z00["00<br/>dead &rarr; dead"] 
    z01["01<br/>live &rarr; dead"]
    z10["10<br/>dead &rarr; live"]
    z11["11<br/>live &rarr; live"]
    z00 --- z01 --- z10 --- z11
```

---

## Math / Complexity

Each cell inspects at most eight neighbors (a constant), and there are two linear passes over the grid:

$$T(R, C) = O(8 \cdot R \cdot C) = O(R \cdot C).$$

The encoding stores the future state inside the existing cell, so no auxiliary board is allocated:

$$S = O(1).$$

The number of cells that flip in one generation is bounded by $R \cdot C$, but that does not change the asymptotic cost.

---

## Takeaway

When a simulation requires **simultaneous** updates, never mutate state you still need to read. Either snapshot it or, as here, **encode old and new in disjoint bits** (`bit 0` = present, `bit 1` = future) and reveal the future with a final `>> 1` sweep — turning an $O(R \cdot C)$-space copy into an $O(1)$-space trick.
