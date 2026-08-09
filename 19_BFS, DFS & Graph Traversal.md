**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par apna dhyan seedhe focus karo.**

Pichle chapter mein humne **Graph Fundamentals & Representation (Chapter 18)** ko poore detail mein seekha aur dekha ki kaise dynamic networks ko Adjacency List ya Adjacency Matrix ke form mein save kiya jata hai [cite: 321, 527]. 

Lekin beta, sirf graph ko represent karna kafi nahi hai. Real-world systems design aur coding interviews mein humare paas aisi situations aati hain jahan humein graph ko explore karna hota hai:
* *"Google Maps mein do locations ke beech sabse shortest rasta kaunsa hai?"* [cite: 6, 221, 485]
* *"Social network mein humare mutual friends ya friend-of-friend suggestions kaise calculate hote hain?"* [cite: 6, 221, 485]
* *"Computer network mein files share karne ke liye closest active host kaunsa hai?"* [cite: 485]

In sabhi problems ko solve karne ke liye humein pure graph ko systematically scan karna padta hai [cite: 466, 482]. Isi systematic exploration ke process ko hum kehte hain—**Graph Traversal** [cite: 322, 466, 482]! 

Aaj hum do sabse important graph traversals—**BFS (Breadth-First Search)** aur **DFS (Depth-First Search)**—ko zero se advanced level tak completely master karenge [cite: 322, 455, 467, 483]. Apni copies aur pen nikal lo, aur shuru karte hain! 🚀

---

## 1. THE ARCHITECTURAL SPLIT: BFS vs. DFS INTUITION

Bacho, jab hum kisi graph ko traverse karte hain, toh humare paas do different psychological ways (paradigms) hote hain explore karne ke [cite: 455, 467, 483, 484]:

```
                     BFS (Breadth-First)                 DFS (Depth-First)
                     Level-by-Level (Wide)               Depth-by-Depth (Deep)
                     
                             [ A ]  ◄── Level 0                [ A ] 1
                            /     \                           /
                         [ B ]───[ C ] ◄── Level 1         [ B ] 2
                          \     /                           /
                           [ D ]  ◄── Level 2            [ D ] 3 (Goes deep first!)
```

### A. BFS (Breadth-First Search) — The Level Explorer 🌐
*   **Intuition:** BFS ka niyam bada seedha hai: **"Pehle apne saare immediate neighbors ko milo, fir unke dosto ke paas jao."** [cite: 455, 483] Yaani hum level-by-level aage badhte hain [cite: 455, 483].
*   **Real-life Analogy:** Maan lo tum ek missing person ko dhoondh rahe ho. Tum pehle apne ghar ke aas-paas ke 1 km ke area ko scan karte ho (Level 1), fir 2 km ke area par jate ho (Level 2), fir 3 km par (Level 3).
*   **Data Structure:** BFS wide explore karta hai, isiliye isme **Queue (FIFO)** data structure ka use hota hai taaki jo node pehle discover ho, wahi pehle process ho [cite: 287, 291, 322, 456, 484].

### B. DFS (Depth-First Search) — The Maze Runner 🌲
*   **Intuition:** DFS ka niyam hai: **"Ek raste par tab tak chalte jao jab tak aage rasta band na ho jaye, fir backtrack karke doosra rasta pakdo."** [cite: 322, 467] Yaani hum gehrai (depth) mein pehle explore karte hain [cite: 322, 455, 467].
*   **Real-life Analogy:** Maan lo tum ek maze (bhool-bhulaiya) mein khade ho. Tum ek raste par aage badhte jate ho jab tak ki tum wall (dead-end) se takra nahi jate [cite: 468]. Phir tum wapas aate ho (backtrack) aur doosra branch check karte ho [cite: 467, 468].
*   **Data Structure:** DFS deep jata hai, isiliye isme **Stack (LIFO)** ya internal recursion call stack ka use hota hai [cite: 291, 322, 468, 478, 484].

---

## 2. THE VISITED TRACKER: WHY IS IT MANDATORY? 🛡️

Bacho, trees aur graphs mein traversal ka sabse bada difference yahi hai [cite: 474, 490]. Tree mein cycles nahi hoti, par graph mein loops/cycles ho sakti hain [cite: 321, 337, 444, 490].

Maan lo humare paas ye cyclic graph hai: `A ──► B ──► C ──► A` [cite: 337]:
```
                                  [ A ] ────► [ B ]
                                    ▲          │
                                    │          ▼
                                    └──────── [ C ]
```
Agar hum visited track nahi karenge:
1. `A` process hoga, wo `B` ko process karega [cite: 493].
2. `B` process ho kar `C` ko bulayega [cite: 493].
3. `C` process ho kar wapas `A` ko queue/stack mein daal dega [cite: 493]!
4. Yeh process kabhi khatam nahi hoga, aur browser memory leak se crash ho jayega [cite: 68, 490]!

### The Solution (Visited Set) 🛡️:
Hum ek **JavaScript Set** ya boolean array maintain karte hain jise hum kehte hain `visited` [cite: 291, 298, 474, 490].
* Kisi node ko process karne se pehle check karo: `visited.has(node)`? [cite: 291, 474, 477]
* If yes, toh us node ko ignore kar do [cite: 291].
* If no, toh `visited.add(node)` karke aage explore karo [cite: 291, 474].

---

## 3. BFS MASTERCLASS: IMPLEMENTATION & TEMPLATE

Chalo bacho, ab BFS ka standard design aur code whiteboard par dekhte hain [cite: 291, 461, 492]. 

### The Queue Latency Problem in JS ⚠️
Normal JavaScript mein jab hum array ke index `0` se element delete karte hain (`array.shift()`), toh engine ko baaki saare elements ko shift karna padta hai, jo **\\(\mathcal{O}(V)\\)** runtime leta hai [cite: 524]. 
Large graphs ke liye interviews mein array-shift use karne se TLE (Time Limit Exceeded) aa sakta hai [cite: 524]. Isiliye ya toh hum custom Pointer-based Queue use karenge ya array index pointing system track karenge [cite: 524]!

### Standard BFS Template (Adjacency List Input):
```javascript
function bfsTemplate(graph, startNode) {
    const visited = new Set(); // To prevent cyclic endless loops [cite: 291, 490]
    const queue = [startNode]; // Queue for level-by-level exploration [cite: 291, 456, 461]
    
    visited.add(startNode); // Mark starter node as visited [cite: 491]
    
    let queuePointer = 0; // O(1) dequeue pointer logic [cite: 524]
    
    while (queuePointer < queue.length) {
        const currentNode = queue[queuePointer]; // Dequeue in O(1) [cite: 524]
        queuePointer++;
        
        console.log(`Processing Node: ${currentNode}`); // Visit action [cite: 461]
        
        // Fetch neighbors [cite: 291, 438]
        const neighbors = graph.getNeighbors(currentNode); // [cite: 438]
        for (let neighbor of neighbors) {
            if (!visited.has(neighbor)) { // If unvisited, process [cite: 291, 494]
                visited.add(neighbor); // Mark visited immediately to prevent duplicates! [cite: 494]
                queue.push(neighbor); // Enqueue [cite: 291, 494]
            }
        }
    }
}
```

---

### Why BFS gives Shortest Path in Unweighted Graphs? 📏
Bacho, dhyan se is mathematical rule ko samjho [cite: 322, 485]. 
BFS level-by-level expand karta hai [cite: 322, 455, 483]. Iska matlab, hum root se jitne step aage jaenge, distance utne hi units se barhega. 
* Level 1 par bache saare nodes root se 1 unit door hain [cite: 455, 483].
* Level 2 par bache saare nodes root se exactly 2 units door hain [cite: 455, 483].
* Isiliye, jab BFS pehli baar kisi target node `T` ko touch karta hai, toh woh path mathematically guaranteed **shortest path (minimum edges)** hota hai [cite: 322, 485]! (Weighted graphs mein ye true nahi hota, wahan Dijkstra lagana padta hai [cite: 322, 528]).

---

## 4. DFS MASTERCLASS: IMPLEMENTATION & TEMPLATE

DFS ko hum do tarike se implement kar sakte hain: **Recursive (using Call Stack)** aur **Iterative (using explicit Stack)** [cite: 291, 468, 478].

### A. Recursive DFS Template (Call Stack Implicit) 📞
Recursion internally system call stack ka use karta hai [cite: 478]. Har naya function recursion step par memory frame push karta hai [cite: 478].

```javascript
function dfsRecursive(graph, currentNode, visited = new Set()) {
    if (visited.has(currentNode)) return; // Base Case [cite: 291]
    
    visited.add(currentNode); // Mark visited [cite: 291]
    console.log(`DFS Visited: ${currentNode}`); // Process node
    
    const neighbors = graph.getNeighbors(currentNode); // [cite: 438]
    for (let neighbor of neighbors) {
        if (!visited.has(neighbor)) { // Traverse deep recursively [cite: 291, 477]
            dfsRecursive(graph, neighbor, visited); // [cite: 291, 477]
        }
    }
}
```

### B. Iterative DFS Template (Explicit Stack) 🥞
Iterative DFS recursion ke replacement mein ek normal JavaScript Array ko **LIFO Stack** ki tarah use karta hai [cite: 291, 478].

```javascript
function dfsIterative(graph, startNode) {
    const visited = new Set();
    const stack = [startNode]; // Push starter [cite: 468]
    
    while (stack.length > 0) {
        const currentNode = stack.pop(); // Pop top (LIFO) [cite: 468, 478]
        
        if (!visited.has(currentNode)) {
            visited.add(currentNode); // Mark visited [cite: 474]
            console.log(`Iterative DFS Visited: ${currentNode}`);
            
            const neighbors = graph.getNeighbors(currentNode); // [cite: 438]
            for (let neighbor of neighbors) {
                if (!visited.has(neighbor)) {
                    stack.push(neighbor); // Push unvisited neighbors [cite: 468]
                }
            }
        }
    }
}
```

---

## 5. PROBLEM-SOLVING GATEWAY (10 INTERVIEW PROBLEMS)

🚀 **Aao dosto! Ab hum in 10 major coding problems ko step-by-step master karenge aur complete visualizations aur line-by-line code dry runs se breakdown karenge!**

---

### PROBLEM 1: Number of Connected Components (LeetCode 323 equivalent)

#### 1. Understand:
Humein `n` vertices aur ek edges array di gayi hai. Humein graph ke total disconnected parts (connected components/islands) ka count return karna hai [cite: 321, 442, 469].

#### 2. Graph Visualization:
```
                      0 ──── 1          2 ──── 3
                      
                      Component 1      Component 2       => Total = 2!
```

#### 3. Brute Force:
Har pair coordinates ke beech path check karo recursively bina visited tracking ke, which leads to exponential runtimes.

#### 4. Observation:
* Agar hum kisi bhi node se start karke complete DFS/BFS run karein, toh us segment se connected **saare nodes** ek hi pass mein visit ho jayenge [cite: 466, 482]!
* Phir hum loop mein agla unvisited node dhoondhenge. Agar koi unvisited mila, iska matlab woh ek naye disconnected part ka hissa hai!
* **Traversal choice:** BFS ya DFS dono chal sakte hain [cite: 322, 469]. Hum recursive DFS choose karenge.

---

#### 5. JavaScript Code:
```javascript
function countComponents(n, edges) {
    // Step 1: Build Adjacency List [cite: 321, 527]
    const adjList = new Map();
    for (let i = 0; i < n; i++) adjList.set(i, []); [cite: 298]
    for (let [u, v] of edges) {
        adjList.get(u).push(v); [cite: 298]
        adjList.get(v).push(u); // Undirected [cite: 298, 467]
    }

    const visited = new Set();
    let componentsCount = 0;

    // Helper DFS function [cite: 291, 475]
    function dfs(node) {
        visited.add(node);
        const neighbors = adjList.get(node) || [];
        for (let neighbor of neighbors) {
            if (!visited.has(neighbor)) {
                dfs(neighbor); // Recursively explore [cite: 291, 477]
            }
        }
    }

    // Step 2: Loop over all vertices
    for (let i = 0; i < n; i++) {
        if (!visited.has(i)) { // Found a new unvisited component!
            componentsCount++;
            dfs(i); // Visit all nodes in this component [cite: 469]
        }
    }

    return componentsCount;
}
```

#### 6. Complexity:
* **Time Complexity:** **\\(\mathcal{O}(V + E)\\)** because each node and edge is processed exactly once with adjacency list [cite: 324, 480, 495].
* **Space Complexity:** **\\(\mathcal{O}(V + E)\\)** to store adjacency map and visited state [cite: 324, 340].

---

### PROBLEM 2: Number of Islands (LeetCode 200)

#### 1. Understand:
Humein ek 2D Grid `grid` di gayi hai jismein `'1'` (land) aur `'0'` (water) hai. Humein total islands ka count return karna hai. Island charo taraf se water se ghirah hota hai (horizontal/vertical connectivity only) [cite: 242, 260].

#### 2. Grid Visualization:
```
                      [ "1", "1", "0" ]  ◄── Island 1
                      [ "1", "1", "0" ]
                      [ "0", "0", "1" ]  ◄── Island 2  => Total = 2!
```

#### 3. Observation:
* 2D Grid ko hum ek **Implicit Graph** ki tarah treat kar sakte hain jahan har cell index `(r, c)` ek vertex hai aur charo contiguous cells (Up, Down, Left, Right) uske neighbors hain!
* Jab hum grid par kisi Land `'1'` par pahunche, toh hum us land se check hone wale pure connected island segment ko DFS/BFS chala kar **water `'0'` mein sink** (in-place mark) kar denge taaki hum use dobara process na karein!

---

#### 4. JavaScript Code:
```javascript
function numIslands(grid) {
    if (!grid || grid.length === 0) return 0;
    
    const rows = grid.length;
    const cols = grid.length;
    let islandCount = 0;

    // Helper Grid DFS [cite: 322]
    function dfs(r, c) {
        // Boundary checks & Water condition check
        if (r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] === '0') {
            return;
        }

        grid[r][c] = '0'; // Sink this land to prevent revisiting (In-place marking!)

        // Explore 4 directions sequentially
        dfs(r + 1, c); // Down
        dfs(r - 1, c); // Up
        dfs(r, c + 1); // Right
        dfs(r, c - 1); // Left
    }

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === '1') { // Found land!
                islandCount++;
                dfs(r, c); // Sink the entire connected island [cite: 242, 260]
            }
        }
    }

    return islandCount;
}
```

#### 5. Complete Dry Run:
*   Input Grid: `[ ["1", "1"], ["0", "1"] ]`.
*   `r=0, c=0`: `grid === "1"`. `islandCount` becomes `1`. Trigger `dfs(0, 0)`.
    *   `dfs(0,0)`: Mark `grid = "0"`. Grid is `[ ["0", "1"], ["0", "1"] ]`.
    *   Call `dfs(1,0)` \\(\rightarrow\\) `grid === "0"` (ignores).
    *   Call `dfs(0,1)` \\(\rightarrow\\) `grid === "1"`. Sinks it `grid = "0"`. 
        *   Inside `dfs(0,1)`, call `dfs(1,1)` \\(\rightarrow\\) `grid === "1"`. Sinks it `grid = "0"`.
*   Grid is now completely `[["0", "0"], ["0", "0"]]`. Loop ends. Returns `1`. Perfect!

#### 6. Complexity:
* **Time Complexity:** **\\(\mathcal{O}(R \times C)\\)** where \\(R\\) is rows and \\(C\\) is columns [cite: 324, 326].
* **Space Complexity:** **\\(\mathcal{O}(R \times C)\\)** worst-case recursive stack depth if entire grid is land.

---

### PROBLEM 3: Flood Fill (LeetCode 733)

#### 1. Understand:
Image di hai as 2D pixels array. Starting coordinate `(sr, sc)` aur ek naya color `newColor` diya hai. Humein starting pixel aur us se matching same-colored connected adjacent pixels ko `newColor` mein convert karna hai.

#### 2. SDE Trap 🚨:
* Agar starting position ka original color already `newColor` ke barabar ho, toh bina base condition check kiye DFS chalane par infinite loop ho jayega!

```javascript
function floodFill(image, sr, sc, color) {
    const originalColor = image[sr][sc];
    if (originalColor === color) return image; // Avoid infinite loop!
    
    const rows = image.length;
    const cols = image.length;

    function dfs(r, c) {
        if (r < 0 || c < 0 || r >= rows || c >= cols || image[r][c] !== originalColor) {
            return;
        }
        
        image[r][c] = color; // Paint with new color
        
        dfs(r + 1, c);
        dfs(r - 1, c);
        dfs(r, c + 1);
        dfs(r, c - 1);
    }

    dfs(sr, sc);
    return image;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(R \times C)\\)**, Space: **\\(\mathcal{O}(R \times C)\\)** stack depth [cite: 324, 326].

---

### PROBLEM 4: Rotten Oranges (LeetCode 994)

#### 1. Understand:
Grid mein `0` (empty), `1` (fresh orange), aur `2` (rotten orange) hain. Har 1 minute mein rotten oranges apne adjacent 4-directional fresh oranges ko rots (rot) kar dete hain. Minimum minutes nikalne hain jab saare oranges rotten ho jayein. Agar ye impossible hai, return `-1` [cite: 235, 253].

#### 2. Why Multi-Source BFS is appropriate? 💡
* *Single DFS can fail!* Kyunki multiple rotten oranges ek sath fresh oranges ko rots karte hain [cite: 235, 253].
* Humein saare rotten oranges se **ek sath (parallelly/multi-source)** level expansion chalani hogi!
* Isiliye hum initial state ke saare Rotten Oranges `2` ko shuruat mein hi Queue mein push kar denge, and then standard Level Order traversal apply karenge!

---

#### 3. JavaScript Code:
```javascript
function orangesRotting(grid) {
    const rows = grid.length;
    const cols = grid.length;
    const queue = [];
    let freshOranges = 0;

    // Step 1: Collect all rotten sources & count fresh oranges
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === 2) {
                queue.push([r, c, 0]); // Store [row, col, minutes]
            } else if (grid[r][c] === 1) {
                freshOranges++;
            }
        }
    }

    let minutesElapsed = 0;
    let queuePointer = 0;
    const directions = [, [-1,0],, [0,-1]]; // 4 directions

    // Step 2: Multi-source BFS
    while (queuePointer < queue.length) {
        const [r, c, mins] = queue[queuePointer];
        queuePointer++;
        minutesElapsed = mins;

        for (let [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;

            // If we find a fresh orange, rot it and enqueue it!
            if (nr >= 0 && nc >= 0 && nr < rows && nc < cols && grid[nr][nc] === 1) {
                grid[nr][nc] = 2; // Rot the fresh orange
                freshOranges--;
                queue.push([nr, nc, mins + 1]); // Enqueue next wave
            }
        }
    }

    return freshOranges === 0 ? minutesElapsed : -1;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(R \times C)\\)**, Space: **\\(\mathcal{O}(R \times C)\\)** [cite: 324, 326].

---

### PROBLEM 5: Shortest Path in Unweighted Graph (BFS Path Reconstruction)

#### 1. Understand:
Adjacency List aur sources and target nodes diye hain. Humein shortest path distance ke sath-sath **poora path sequence** reconstruct karke array return karna hai [cite: 504, 505].

#### 2. Visual Path Reconstruction Blueprint 🗺️:
```
                     Shortest Path: A ──► B ──► C
                     Previous Pointer Map: { C: 'B', B: 'A', A: null }
                     Reconstruct back-to-front: C -> B -> A => Reverse!
```

---

#### 3. JavaScript Code:
```javascript
function findShortestPath(n, edges, start, target) {
    // Build Adjacency List [cite: 321, 527]
    const adj = new Map();
    for (let i = 0; i < n; i++) adj.set(i, []); [cite: 298]
    for (let [u, v] of edges) {
        adj.get(u).push(v); [cite: 298]
        adj.get(v).push(u); // Bidirectional [cite: 298, 467]
    }

    const visited = new Set();
    const parentMap = new Map(); // To store parent-child relationship [cite: 511]
    const queue = [start];

    visited.add(start);
    parentMap.set(start, null); // Start node has no parent [cite: 511, 513]

    let queuePtr = 0;
    let found = false;

    while (queuePtr < queue.length) {
        const curr = queue[queuePtr];
        queuePtr++;

        if (curr === target) {
            found = true;
            break;
        }

        const neighbors = adj.get(curr) || [];
        for (let neighbor of neighbors) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor); // Mark visited [cite: 494]
                parentMap.set(neighbor, curr); // Map: neighbor -> current (Parent) [cite: 511, 515]
                queue.push(neighbor); // Enqueue [cite: 494]
            }
        }
    }

    if (!found) return []; // Target unreachable

    // Path Reconstruction [cite: 509, 511]
    const path = [];
    let temp = target;
    while (temp !== null) {
        path.push(temp);
        temp = parentMap.get(temp); // Jump back to parent [cite: 509, 511]
    }
    return path.reverse(); // Flip to correct order
}
```
* **Complexity:** Time: **\\(\mathcal{O}(V + E)\\)**, Space: **\\(\mathcal{O}(V + E)\\)** [cite: 324, 340].

---

### PROBLEM 6: Walls and Gates (Multi-Source BFS on Grid)

#### 1. Understand:
Grid mein `INF` (empty room), `-1` (wall/obstacle), aur `0` (gate) hai. Har empty room ko uske nearest gate ka distance assign karna hai.

#### 2. Observation:
Rotten Oranges ki tarah hi, saare gates `0` se parallel distance wave chalayenge [cite: 235, 253]!

```javascript
function wallsAndGates(rooms) {
    if (!rooms || rooms.length === 0) return;
    const rows = rooms.length;
    const cols = rooms.length;
    const queue = [];

    // Enqueue all gates as multi-source starters
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (rooms[r][c] === 0) {
                queue.push([r, c]);
            }
        }
    }

    let queuePtr = 0;
    const directions = [, [-1,0],, [0,-1]];

    while (queuePtr < queue.length) {
        const [r, c] = queue[queuePtr];
        queuePtr++;

        for (let [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;

            // If coordinates are inside bounds & room has INF (unprocessed)
            if (nr >= 0 && nc >= 0 && nr < rows && nc < cols && rooms[nr][nc] === 2147483647) {
                rooms[nr][nc] = rooms[r][c] + 1; // Update distance
                queue.push([nr, nc]); // Enqueue next room
            }
        }
    }
}
```
* **Complexity:** Time: **\\(\mathcal{O}(R \times C)\\)**, Space: **\\(\mathcal{O}(R \times C)\\)** [cite: 324, 326].

---

### PROBLEM 7: Cycle Detection in Undirected Graph (LeetCode 141 equivalent for Graphs)

#### 1. Understand:
Undirected graph mein find out karna hai ki kya koi cycle exist karti hai [cite: 321, 359, 442, 469].

#### 2. The SDE Dilemma 🚨:
*"Sir, undirected graph mein aage jaenge toh peeche wala node visited bolega. Toh har edge cycle lagegi!"*
Bilkul sahi, bacho! Is trap se bachne ke liye humein **Parent Node** tracking karni hogi.
* Agar koi node visited hai, par woh **current node ka direct parent nahi hai**, iska matlab hum doosre raste se ghoom kar wahan aaye hain! Cycle detected!

```
                      Parent Tracker check:
                      0 ──── 1 ──── 2
                             └──────┘ (Cycle)
                      When at 2, neighbor is 1 (parent - OK), neighbor is 1 (already visited - cycle!)
```

---

#### 3. JavaScript Code (using BFS):
```javascript
function hasCycleUndirected(n, edges) {
    const adj = new Map();
    for (let i = 0; i < n; i++) adj.set(i, []); [cite: 298]
    for (let [u, v] of edges) {
        adj.get(u).push(v); [cite: 298]
        adj.get(v).push(u); // Bidirectional [cite: 298, 467]
    }

    const visited = new Set();

    function bfsCheckCycle(start) {
        const queue = [[start, -1]]; // Store pair: [currentNode, parentNode]
        visited.add(start);

        let queuePtr = 0;
        while (queuePtr < queue.length) {
            const [curr, parent] = queue[queuePtr];
            queuePtr++;

            const neighbors = adj.get(curr) || [];
            for (let neighbor of neighbors) {
                if (!visited.has(neighbor)) {
                    visited.add(neighbor);
                    queue.push([neighbor, curr]); // Map neighbor with parent as curr
                } else if (neighbor !== parent) { // Cycle matched! [cite: 359, 469]
                    return true;
                }
            }
        }
        return false;
    }

    // Handles disconnected components [cite: 321, 442, 469]
    for (let i = 0; i < n; i++) {
        if (!visited.has(i)) {
            if (bfsCheckCycle(i)) return true;
        }
    }
    return false;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(V + E)\\)**, Space: **\\(\mathcal{O}(V + E)\\)** [cite: 324, 340].

---

### PROBLEM 8: Cycle Detection in Directed Graph (LeetCode 207)

#### 1. Understand:
Directed graph mein cycle detection kaise hogi?

#### 2. Why Undirected Logic fails in Directed Graphs? 🚨:
Dhyan se whiteboard par bani is image ko dekho:
```
                             0 ───► 1 ───► 2 
                             └───► 2
```
Undirected logic ke hisab se `2` check par cycle bol deta kyunki use do paths dikhte. Lekin ye cycle nahi hai kyunki edges directed hain aur flows safe hain!

#### 3. SDE State Paradigm (Three-state Coloring Logic 🎨):
Hum har vertex ko teen categories mein colors (states) assign karenge:
*   **White (0 / Unvisited):** Node ko abhi tak visit nahi kiya hai [cite: 291].
*   **Gray (1 / Visiting):** Node recursion/call stack ke andar active hai. Hum iske neighbors explore kar rahe hain [cite: 291, 478].
*   **Black (2 / Visited):** Node aur iske saare neighbors fully explored ho chuke hain [cite: 291].

**Cycle Rule:** Exploring phase ke dauran agar humein koi doosra **Gray (1) node** dobara touch hota hai, iska matlab **active recursion path par cycle exist karti hai!** [cite: 359, 469]

---

#### 4. JavaScript Code:
```javascript
function hasCycleDirected(numCourses, prerequisites) {
    const adj = new Map();
    for (let i = 0; i < numCourses; i++) adj.set(i, []); [cite: 298]
    for (let [u, v] of prerequisites) {
        adj.get(v).push(u); // Directed Course pre-requisite graph representation [cite: 447]
    }

    const state = new Array(numCourses).fill(0); // 0=White, 1=Gray, 2=Black

    function dfsCheck(node) {
        state[node] = 1; // Mark active (Gray)

        const neighbors = adj.get(node) || [];
        for (let neighbor of neighbors) {
            if (state[neighbor] === 1) {
                return true; // Active cycle intersection check matched! [cite: 359, 469]
            }
            if (state[neighbor] === 0) {
                if (dfsCheck(neighbor)) return true;
            }
        }

        state[node] = 2; // Fully processed (Black)
        return false;
    }

    for (let i = 0; i < numCourses; i++) {
        if (state[i] === 0) {
            if (dfsCheck(i)) return true;
        }
    }
    return false;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(V + E)\\)**, Space: **\\(\mathcal{O}(V + E)\\)** [cite: 324, 340].

---

### PROBLEM 9: Bipartite Graph (LeetCode 785)

#### 1. Understand:
Humein check karna hai ki kya ek graph **Bipartite** hai. Bipartite graph ka matlab hota hai ki hum vertices ko do mutually independent groups mein partition kar sakein taaki koi bhi do adjacent nodes same group (color) se connect na hon [cite: 242, 260].

#### 2. Two-Coloring Intuition 🎨:
* Hum graph ko **BFS** ya **DFS** se traverse karenge aur adjacent nodes ko opposite colors assign karenge (`1` aur `-1`) [cite: 242, 260]!
* Agar kisi process par humein koi neighbor dikhe jo already colored hai aur uski color current node ke barabar ho, iska matlab conflict! Graph is not Bipartite [cite: 242, 260]!

```javascript
function isBipartite(graph) {
    const n = graph.length;
    const colors = new Array(n).fill(0); // 0=Uncolored, 1=Red, -1=Blue

    for (let i = 0; i < n; i++) {
        if (colors[i] !== 0) continue;

        // BFS traversal wave [cite: 242, 260]
        const queue = [i];
        colors[i] = 1; // Start coloring

        let queuePtr = 0;
        while (queuePtr < queue.length) {
            const curr = queue[queuePtr];
            queuePtr++;

            for (let neighbor of graph[curr]) {
                if (colors[neighbor] === colors[curr]) {
                    return false; // Conflict found! [cite: 242, 260]
                }
                if (colors[neighbor] === 0) {
                    colors[neighbor] = -colors[curr]; // Assign opposite color
                    queue.push(neighbor);
                }
            }
        }
    }
    return true;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(V + E)\\)**, Space: **\\(\mathcal{O}(V)\\)** [cite: 324, 340].

---

## 6. SDE COMPLEXITY COMPARISON SHEET

Bacho, is operational breakdown sheets ko dhang se register mein draw karo:

| Algorithm / Traversal | Adjacency List ⏱️ | Adjacency Matrix ⏱️ | Grid Representation ⏱️ | Space Complexity 💾 |
| :--- | :--- | :--- | :--- | :--- |
| **BFS** [cite: 322] | **\\(\mathcal{O}(V + E)\\)** [cite: 324, 495] | **\\(\mathcal{O}(V^2)\\)** [cite: 326] | **\\(\mathcal{O}(R \times C)\\)** [cite: 324, 326] | **\\(\mathcal{O}(V)\\)** [cite: 326, 496] |
| **DFS** [cite: 322] | **\\(\mathcal{O}(V + E)\\)** [cite: 324, 480] | **\\(\mathcal{O}(V^2)\\)** [cite: 326] | **\\(\mathcal{O}(R \times C)\\)** [cite: 324, 326] | **\\(\mathcal{O}(V)\\)** [cite: 326, 480] |

*Why does complexity change for Grid?* Kyunki grid mein vertices count is exactly \\(R \times C\\), aur max neighbors boundaries hamesha 4 hi limit hoti hain, isiliye edges constant limit behave karti hain [cite: 324, 326].

---

## 7. CORE SDE TRAPS & COMMON MISTAKES TO AVOID ⚠️

1. **Marking Visited at the Wrong Time (Queue Explosion 💥):**
   *   *Common Mistake:* Node ko processed/visited tab mark karna jab use shift se pop out kiya jaye, na ki push karte waqt.
   *   *Disaster:* Isse same adjacent node queue mein duplicates copy bar-bar push ho jata hai, jisse queue memory overflow ho jati hai.
   *   *The Fix:* Hamesha visited ko **push/enqueue operation par hi mark** karein [cite: 494]!
2. **Mixing Undirected and Directed Cycle logic:**
   Directed graphs mein cycle validation ke liye simple parent tracking code run karna fail ho jata hai bacho! Hamesha three-state coloring recursive rules use karein [cite: 359, 469].
3. **Missing Grid Boundary limits validations:**
   Grid cell indices lookups `r` and `c` par checks missing hone se standard runtime execution bounds overflow exceptions drop out crashing produce hoti hain.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Standard Graph traversals core mechanisms and V8 implicit frames allocations [cite: 322, 461].
* Implicit 2D grids transformation models into explicit neighbors sweeps [cite: 321, 322].
* Level order BFS limits vs deepest backtracking recursive calls DFS [cite: 455, 467, 483, 484].
* Parallel multi-source BFS grids wave boundaries expansions [cite: 235, 253].

### Mastered Traversal Templates:
* **The visited tracking shield** to prevent dynamic cyclic looping [cite: 474, 490].
* **Gray-Black state-space colors tracking** for course schedule dependencies validations.
* **Parent reconstruct linkages pointers** to decode shortest paths unweighted profiles [cite: 511].

---

### SDE Practice Roadmap:
1. Solve *Number of Islands* on LeetCode 200 using in-place sink DFS.
2. Complete *Rotten Oranges* (LeetCode 994) using Multi-source BFS [cite: 236, 254].
3. Practice *Bipartite Graph* coloring validations [cite: 242, 260].

---
