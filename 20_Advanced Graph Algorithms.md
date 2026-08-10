**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par apna dhyan seedhe focus karo.**  

Pichle chapters mein humne **Graphs** ke representations aur inke basic DFS aur BFS traversals ko bilkul depth mein visual dry runs ke sath seekha. Humne dekha ki kaise dynamic networks ko store aur linearly explore kiya jata hai.

Lekin bacho, real-world networks sirf simple connections ke bane nahi hote. Socho Google Maps, jismein har road par traffic aur different distances (weights) hote hain. Socho computer networks ya college course dependencies, jahan ek subject padhne se pehle doosra clear karna compulsory hai. 

In complex network behaviors ko resolve karne ke liye hum padhte hain—**Advanced Graph Algorithms**! 

Aaj hum advanced graph design spaces ko shuruat se lekar interview level tak completely break karenge. Koi dynamic libraries use nahi karenge—hum khud apna code likhenge! Pen aur register nikal lo, aur whiteboard par focus karo! 🚀

---

## 1. DIRECTED ACYCLIC GRAPHS (DAG) & TOPOLOGICAL SORT

Bacho, sabse pehle hum directed structures ki ek bohot hi special class ko samajhte hain jise hum kehte hain **DAG (Directed Acyclic Graph)**.

### DAG Kya Hai? 📐
1. **Directed:** Edges unidirectional "one-way streets" hoti hain (e.g., A → B).
2. **Acyclic:** Graph ke andar **koi cycle ya loop nahi** ho sakta. Yaani agar tum kisi node A se chalna shuru karo, toh ghoom-firokar tum wapas A par nahi pahunch sakte.

```
                     [ Valid DAG ]                    [ Invalid (Cyclic) ]
                       0 ────► 1                          0 ────► 1
                       │       ▲                          ▲       │
                       ▼       │                          │       ▼
                       2 ──────┘                          2 ◄─────┘
               (No Cycle possible)                    (Cycle 0 -> 1 -> 2 -> 0)
```

### Topological Ordering Intuition 🔗
Topological Sort directed acyclic graph ke vertices ki ek **linear ordering** hoti hai, jismein agar graph mein ek directed edge u → v hai, toh linear list mein node **u hamesha node v se pehle aana chahiye**.

*   **Real-Life Analogy (Course Prerequisites 🎓):**
    College mein programming seekhne ke liye tumhe ye sequential pattern follow karna padta hai:
    `Mathematics -> Introduction to JS -> Advanced DSA -> Build Projects`.
    Yahan tum **Mathematics** ko bina uske basic rules clear kiye direct **Advanced DSA** se start nahi kar sakte.

> **When is Topological Sort possible?**  
> Topological sorting **strictly sirf ek DAG mein hi possible hai**. Agar graph connected undirected ho ya cyclic ho, toh topological ordering completely impossible ho jati hai.

---

### A. Kahn’s Algorithm: The BFS + Indegree Approach 🛠️

#### 1. Understand:
Kahn’s algorithm Queue aur **Indegree (incoming edges count)** ka use karke level-by-level topological order generate karta hai.

#### 2. Why basic DFS can feel tricky?
Basic DFS deep explore karta hai, jismein backtracking logic implement karni padti hai. Kahn’s algorithm, BFS ke structure par based hone ke karan, dynamic indegrees track karke processing ko super clean aur iterative bana deta hai.

#### 3. Indegree Concept:
*   **Indegree:** Kisi node par total kitne edges aakar point kar rahe hain.
*   *Observation:* Agar kisi node ki indegree `0` hai, iska matlab uski koi dependency nahi hai! Hum use sabse pehle process kar sakte hain.

#### 4. Kahn's Algorithm Idea:
1. Graph ke saare vertices ki `indegree` calculate karo.
2. Jin nodes ki indegree `0` hai, unhe **Queue** mein push kar do.
3. Queue se node ko dequeue karo, use apne result order list mein add karo.
4. Us popped node ke saare adjacent neighbors ki indegree ko `1` se **decrement (reduce)** karo.
5. Decrement karne ke baad agar kisi neighbor ki indegree wapas `0` ho jaye, toh use Queue mein push kar do!
6. Repeat step 3-5 until queue is empty.
7. *Cycle Detection Check:* Agar sorted list ki length vertices count V se choti reh gayi, iska matlab graph mein loop (cycle) hai aur topological sort complete nahi ho paya!

---

#### 5. JavaScript Implementation:
```javascript
function topologicalSortKahn(v, edges) {
    const adjList = new Map();
    const indegree = new Array(v).fill(0);

    // Step 1: Initialize adjacency list
    for (let i = 0; i < v; i++) {
        adjList.set(i, []);
    }

    // Step 2: Build graph and calculate indegrees
    for (let [u, target] of edges) {
        adjList.get(u).push(target);
        indegree[target]++; // Dependency count increases
    }

    // Step 3: Enqueue nodes with indegree 0
    const queue = [];
    for (let i = 0; i < v; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }

    const topoOrder = [];
    let queuePointer = 0; // O(1) Dequeue optimization

    // Step 4: Level order reduction loop
    while (queuePointer < queue.length) {
        const curr = queue[queuePointer++];
        topoOrder.push(curr);

        const neighbors = adjList.get(curr) || [];
        for (let neighbor of neighbors) {
            indegree[neighbor]--; // Remove completed dependency link
            
            // If dependency becomes 0, enqueue neighbor
            if (indegree[neighbor] === 0) {
                queue.push(neighbor);
            }
        }
    }

    // Cycle detection check
    if (topoOrder.length !== v) {
        return []; // Cycle detected, Topo sort impossible!
    }

    return topoOrder;
}
```

#### 6. Complete Dry Run on `v = 3, edges = [,,]`:
*   Adjacency List: `0 ->, 1 ->, 2 -> []`
*   Initial Indegrees: `indegree =` (Node 0 has 0, Node 1 has 1, Node 2 has 2)
*   **Queue Init:** Only node 0 has indegree 0. `queue = `. `topoOrder = []`.

*   **Iteration 1:**
    *   Pop `curr = 0`. `topoOrder = `.
    *   Neighbors of 0: `1` and `2`.
        *   Decrement indegree of 1: `indegree = 0`. Since it's 0, push 1. `queue =`.
        *   Decrement indegree of 2: `indegree = 1`.
*   **Iteration 2:**
    *   Pop `curr = 1`. `topoOrder =`.
    *   Neighbors of 1: `2`.
        *   Decrement indegree of 2: `indegree = 0`. Since it's 0, push 2. `queue =`.
*   **Iteration 3:**
    *   Pop `curr = 2`. `topoOrder =`. No neighbors.
*   **End state:** `topoOrder.length === 3`. Loop terminates successfully. Order is ``. Correct!

#### 7. Complexity Analysis:
*   **Time Complexity:** **O(V + E)** because each node is processed once, and edges are visited exactly once.
*   **Space Complexity:** **O(V + E)** to store the adjacency list and visited states.

---

### B. DFS-Based Topological Sort

#### 1. Concept:
DFS traversal gaharai tak explore karta hai. topological sort ko DFS se resolve karne ka trick ye hai ki **jab hum kisi node aur uske saare dependencies (neighbors) ko fully traverse karke khatam kar lete hain, tab use ek Stack mein push kar dete hain**.

```javascript
function topologicalSortDFS(v, edges) {
    const adjList = new Map();
    for (let i = 0; i < v; i++) adjList.set(i, []);
    for (let [u, target] of edges) {
        adjList.get(u).push(target);
    }

    const visited = new Set();
    const stack = []; // To store topological order back-to-front

    function dfs(node) {
        visited.add(node); //
        const neighbors = adjList.get(node) || [];
        for (let neighbor of neighbors) {
            if (!visited.has(neighbor)) { //
                dfs(neighbor);
            }
        }
        stack.push(node); // Post-order: push node after completing neighbors
    }

    for (let i = 0; i < v; i++) {
        if (!visited.has(i)) {
            dfs(i);
        }
    }

    return stack.reverse(); // Flip stack to get correct order
}
```

---

## 2. SHORTEST PATH: DIJKSTRA’S ALGORITHM (LeetCode 743)

Weighted graph mein single-source shortest path calculate karne ke liye Edsger W. Dijkstra (1956) ne ye bohot hi dhasu algorithm design kiya.

### Why BFS fails on Weighted Graphs? 🚨
Bacho, dhyan se whiteboard par bani is image ko dekho:

```
                            (5)
                       Home ───► A 
                        │        │
                       (2)      (1)
                        ▼        ▼
                        C ─────► Office
                            (1)
```
*   **If we run BFS:** BFS kahega: *"Mera path Home -> A -> Office has length 2 edges, and Home -> C -> Office has length 2 edges. Dono equal hain!"*
*   **The Weighted Reality:** 
    *   Path `Home -> A -> Office` has cost 5 + 1 = 6.
    *   Path `Home -> C -> Office` has cost 2 + 1 = 3!
    BFS edge weights ko ignore karta hai, isiliye dynamic weighted networks par iska optimal selection system fail ho jata hai.

---

### Dijkstra’s Greedy Relaxation Principle 🏔️
Dijkstra ek **Greedy approach** use karta hai.
*   Hum ek `distances` array maintain karte hain aur sabhi vertices ki tentative distance ko `Infinity` se initialize karte hain, siwaye starting vertex ke (jo `0` hota hai).
*   Hum hamesha us unvisited node ko select karte hain jiska distance source se **sabse minimum (shortest)** ho.
*   **Relaxation step:** Us node ke neighbor nodes ke distance ko check karte hain. Agar current node se hokar jaane wala rasta pichle calculated raste se chota ho, toh use update kar dete hain:
    \\[dist[v] = min(dist[v], dist[u] + weight(u, v))\\]

> **The Non-Negative Constraints ⚠️:**  
> Dijkstra's Algorithm **strictly negative edge weights ko support nahi karta**! Kyunki agar negative weight cycle aa gayi, toh greedy assumption break ho jati hai aur loop infinitely values decrease karta rahega.

---

### Min Priority Queue (Min Heap) Wrapper Code
Adjacency list ke neighbor updates ko optimal O(log V) limits par pop karne ke liye hum humara custom Min-Heap use karenge taaki framework solid rahe:

```javascript
class MinHeapAdhoc {
    constructor() {
        this.heap = [];
    }
    swap(i, j) {
        const temp = this.heap[i];
        this.heap[i] = this.heap[j];
        this.heap[j] = temp;
    }
    add(node, priority) {
        this.heap.push({ node, priority });
        this.bubbleUp();
    }
    bubbleUp() {
        let idx = this.heap.length - 1;
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2);
            if (this.heap[idx].priority < this.heap[parentIdx].priority) {
                this.swap(idx, parentIdx);
                idx = parentIdx;
            } else {
                break;
            }
        }
    }
    poll() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        const root = this.heap;
        this.heap = this.heap.pop();
        this.sinkDown();
        return root;
    }
    sinkDown() {
        let idx = 0;
        const length = this.heap.length;
        while (2 * idx + 1 < length) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let smallest = leftChildIdx;
            if (rightChildIdx < length && this.heap[rightChildIdx].priority < this.heap[leftChildIdx].priority) {
                smallest = rightChildIdx;
            }
            if (this.heap[idx].priority > this.heap[smallest].priority) {
                this.swap(idx, smallest);
                idx = smallest;
            } else {
                break;
            }
        }
    }
    isEmpty() {
        return this.heap.length === 0;
    }
}
```

---

### Dijkstra’s Algorithm JavaScript Code:
```javascript
function dijkstra(v, edges, startNode) {
    const adjList = new Map();
    for (let i = 0; i < v; i++) adjList.set(i, []);

    // Build weighted adjacency map representation
    for (let [u, target, weight] of edges) {
        adjList.get(u).push({ node: target, weight });
        adjList.get(target).push({ node: u, weight }); // Undirected
    }

    const distances = new Array(v).fill(Infinity); // Distance tracking table
    const visited = new Set(); // To prevent endless redundant loop backtracking
    const parentMap = new Array(v).fill(null); // Previous node mapping for path recovery

    distances[startNode] = 0; // Farthest offset starts with 0

    const pq = new MinHeapAdhoc();
    pq.add(startNode, 0); // Enqueue source

    while (!pq.isEmpty()) {
        const { node: curr, priority: currDist } = pq.poll(); // Fetch min priority node

        if (visited.has(curr)) continue; // Stale Heap Entry skip checks!
        visited.add(curr);

        const neighbors = adjList.get(curr) || [];
        for (let { node: neighbor, weight } of neighbors) {
            if (visited.has(neighbor)) continue;

            const nextDist = currDist + weight; // Total accumulated path cost
            
            // Edge Relaxation
            if (nextDist < distances[neighbor]) {
                distances[neighbor] = nextDist;
                parentMap[neighbor] = curr; // Predecessor hook
                pq.add(neighbor, nextDist); // Push updated shortest distance candidate
            }
        }
    }

    return { distances, parentMap };
}
```

---

### Visual Path Reconstruction Dry Run:
Maan lo humein target node `Office` tak ka shortest path trace karna hai.
*   At the end of loop, parent values map look like this: `parentMap = [ null, 'Home', 'A', 'Home', 'C' ]` (mapping Node ID indices).
*   Office (4) → parent is `C` (3) → parent is `Home` (0).
*   Trace backwards: `Office -> C -> Home`. Reverse the order: **`Home -> C -> Office`**!

#### Complexity:
*   **Time Complexity:** **O((V + E) log V)** optimized using heap prioritizations.
*   **Space Complexity:** **O(V + E)** auxiliary adjacency list allocations.

---

## 3. BELLMAN-FORD ALGORITHM (LeetCode 787 equivalent)

### Why Dijkstra fails with Negative Edge weights? ❌
Dhyan se is graph flow ko dekho bacho:

```
                            (5)
                       A ────────► B 
                       │          ▲
                      (2)        / (-10)
                       ▼        /
                       C ──────┘
```
1. Dijkstra A se start hoga. Tentative: `A=0, B=inf, C=inf`.
2. Pop A (minimum distance). Neighbors: B (5) and C (2). 
   * Update distances: `B=5, C=2`. Mark A as visited.
3. Pop C (distance 2 is smaller than 5). Neighbors: B.
   * Recalculate distance of B through C: 2 + (-10) = -8.
   * Since -8 < 5, B's distance becomes -8. Mark C as visited.
4. **The Greedy Trap:** Kyunki B aur C already visited mark ho chuke hain, agar B se aage koi branch nikal rahi hoti (e.g., B → D), toh Dijkstra wahan wapas backtrack nahi karega! Isse dynamic negative propagation update completely breakdown ho jati hai.

---

### The Bellman-Ford Principle: V-1 Relaxations 🗃️
Dijkstra greedy hai, par Bellman-Ford pure DP constraints follow karta hai. 
*   **Rule:** Hum graph ke saare edges ko exact **V-1 iterations** tak lagataar relax karte hain!
*   *Why V-1?* Kyunki kisi bhi simple shortest path mein maximum number of connections (edges) V-1 hi ho sakte hain.
*   **Negative Cycle Detection:** Agar V-1 iterations complete hone ke baad bhi **V-th iteration** par koi edge aur relax ho jaye, iska matlab graph mein ek aisa closed circle hai jiska total path sum negative ho chuka hai (Negative Cycle)!

---

```javascript
function bellmanFord(v, edges, src) {
    const distances = new Array(v).fill(Infinity);
    distances[src] = 0;

    // Step 1: Relax all edges V-1 times
    for (let i = 1; i <= v - 1; i++) {
        for (let [u, target, weight] of edges) {
            if (distances[u] !== Infinity && distances[u] + weight < distances[target]) {
                distances[target] = distances[u] + weight; // Relaxation
            }
        }
    }

    // Step 2: V-th iteration to check for negative cycles
    for (let [u, target, weight] of edges) {
        if (distances[u] !== Infinity && distances[u] + weight < distances[target]) {
            console.log("Negative Weight Cycle Detected!");
            return null; // Returning null because shortest path is undefined!
        }
    }

    return distances;
}
```
*   **Complexity:** Time: **O(V × E)** (quadratic runtime limits), Space: **O(V)** auxiliary tracking array space.

---

## 4. FLOYD-WARSHALL ALGORITHM (ALL-PAIRS SHORTEST PATH)

Dijkstra aur Bellman-Ford sirf single vertex se paths batate hain. Par agar humein **saare vertices ke beech ka shortest path** nikalna ho, toh hum use karte hain—**Floyd-Warshall**!

### The Dynamic Programming Formulation 🧠
Floyd-Warshall DP principles par functional space transition build karta hai.
*   Hum dynamic path check select karte hain: **`shortestPath(i, j, k)`**.
*   *Meaning:* Node i se Node j tak ka shortest path, jab hum intermediate (bich wale) nodes sirf {1, 2, ..., k} ke set se select kar sakte hon.
*   **Transition Equation:**
    \\[dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])\\]

```
                     Floyd-Warshall State Transition:
                     
                              (k - Intermediate vertex)
                                     ●
                                   /   \
                             (i)  /     \  (j)
                                 ● ─────► ●
                                  Original
```

---

### Floyd-Warshall 2D Table Dry Run 📊
Maan lo humein edges di gayi hain, aur initial state (k=0) matrix is:
```
          A   B   C
      A [ 0,  5,  inf ]
      B [ inf, 0,  2  ]
      C [ 1,  inf, 0  ]
```
Ab hum step-by-step k (intermediate node) ko vary karenge:

#### Step 1 (k = A):
*   Can we improve C → B through A?
    `dist[C][B]` is `inf`. `dist[C][A] + dist[A][B] = 1 + 5 = 6`.
    Since `6 < inf`, we update `dist[C][B] = 6`.

#### Final Converged Matrix (k = C):
All-pairs shortest paths successfully mapped.

```javascript
function floydWarshall(v, matrix) {
    const dist = Array.from({ length: v }, () => new Array(v).fill(Infinity));

    // Initialize with direct edges weights and self loops
    for (let i = 0; i < v; i++) {
        for (let j = 0; j < v; j++) {
            if (i === j) dist[i][j] = 0;
            else if (matrix[i][j] !== undefined) dist[i][j] = matrix[i][j];
        }
    }

    // Three nested loops: Intermediate vertex (k) is hamesha outermost loop!
    for (let k = 0; k < v; k++) {
        for (let i = 0; i < v; i++) {
            for (let j = 0; j < v; j++) {
                if (dist[i][k] !== Infinity && dist[k][j] !== Infinity) {
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]); // Transition
                }
            }
        }
    }
    return dist;
}
```
*   **Complexity:** Time: **O(V^3)**, Space: **O(V^2)** to hold the distance grid.

---

## 5. MINIMUM SPANNING TREE (MST) CONCEPTS

Advanced system constraints mein MST ka concept bohot important role play karta hai.

### Spanning Tree Kya Hai? 🌳
Spanning Tree ek connected weighted undirected graph ka ek aisa subtree hota hai jo:
1. Graph ke **saare vertices (V) ko include** kare.
2. Graph mein **koi cycle na ho** (tree properties).
3. Minimum total edge weight select kare.

```
                     MST vs. Shortest Path (Single Source):
                     
                                (5)       (5)
                             A ─────► B ──────► C
                             │                  ▲
                             └──────(2)─────────┘
                     
                     Shortest Path from A:
                     Path to B is A-B (5). Path to C is A-C (2). Total = 7 edges.
                     
                     MST of Graph:
                     Connect A-C (2) and B-C (5). All connected. Weight sum = 7.
```

---

## 6. PRIM’S ALGORITHM (GROWING THE MST GREEDILY)

### Algorithm Idea 💡:
Prim’s algorithm ek **greedy vertex-centric approach** use karta hai.
*   Hum ek arbitrary vertex se shuru karte hain aur use dynamic MST tree pool mein add kar dete hain.
*   Har ek step par, hum un saare edges ko dekhte hain jo already discovered (MST inside nodes) ko undiscovered (outside nodes) ke sath connect karte hain.
*   Hum **sabse minimum weight wale edge** ko select karte hain, neighbor node ko dynamically pool mein add karte hain, aur range updates enqueue karte hain!

```javascript
function primsMST(v, edges) {
    const adj = new Map();
    for (let i = 0; i < v; i++) adj.set(i, []);
    for (let [u, target, weight] of edges) {
        adj.get(u).push({ to: target, weight });
        adj.get(target).push({ to: u, weight }); // Undirected
    }

    const inMST = new Set();
    const pq = new MinHeapAdhoc();
    let mstCost = 0;

    pq.add(0, 0); // Start growing from node 0

    while (!pq.isEmpty()) {
        const { node: curr, priority: cost } = pq.poll();

        if (inMST.has(curr)) continue;
        inMST.add(curr); // Add node to MST pool
        mstCost += cost;

        const neighbors = adj.get(curr) || [];
        for (let { to: neighbor, weight } of neighbors) {
            if (!inMST.has(neighbor)) {
                pq.add(neighbor, weight); // Push edge candidates
            }
        }
    }
    return mstCost;
}
```
*   **Complexity:** Time: **O((V + E) log V)** via Heap extraction, Space: **O(V + E)**.

---

## 7. KRUSKAL’S ALGORITHM & DISJOINT SET UNION (DSU)

### Kruskal's Greedy Strategy 🔗:
Prim node ko grow karta hai, par Kruskal directly **edge weights par play** karta hai!
1. Graph ke saare edges ko unke weights ke according **ascending sorted order** mein arrange karo.
2. Sabse saste edge se start karo, use MST mein append karte jao.
3. **The Cycle Constraint:** Agar naya edge insert karne se graph mein cycle banti ho, toh use ignore (skip) kar do!

*Cycle check karne ke liye DFS/BFS chalana bohot slow ho jayega. Is dynamic tracking ko optimized constant limits par handle karne ke liye hum padhte hain—**DSU (Disjoint Set Union)**.*

---

### DSU (Union-Find) Deep-Dive ⚙️:
DSU disjoint elements ke groups ko dynamically manage karta hai.
*   **Path Compression (Lookup optimization):** dynamic roots ko find out karte waqt, nodes pointers ko directly group parent ke sath link bypass kar do. Lookup becomes amortized **O(1)**.
*   **Union by Rank (Balanced trees):** Hamesha choti rank/height wale group tree structure ko bade groups roots ke sath link append karo.

```
                     Path Compression Optimization:
                     
                          Before:                          After (Flattened):
                             3                                     3
                            /                                    /   \
                           2                                    1     2
                          /
                         1
```

---

### JavaScript Class DSU & Kruskal’s MST:
```javascript
class DisjointSetUnion {
    constructor(size) {
        this.parent = Array.from({ length: size }, (_, i) => i);
        this.rank = new Array(size).fill(0);
    }

    // Find with Path Compression
    find(u) {
        if (this.parent[u] === u) return u;
        // Flatten reference pointer recursively
        return this.parent[u] = this.find(this.parent[u]);
    }

    // Union by Rank
    union(u, v) {
        const rootU = this.find(u);
        const rootV = this.find(v);

        if (rootU !== rootV) {
            // Attach smaller height tree under larger tree
            if (this.rank[rootU] < this.rank[rootV]) {
                this.parent[rootU] = rootV;
            } else if (this.rank[rootU] > this.rank[rootV]) {
                this.parent[rootV] = rootU;
            } else {
                this.parent[rootV] = rootU;
                this.rank[rootU]++;
            }
            return true; // Successfully merged (No cycles existed)
        }
        return false; // Cycle detected! u and v are already connected.
    }
}

function kruskalsMST(v, edges) {
    // Step 1: Sort edges by weight ascending
    // NOTE: each edge is a [u, target, weight] triple, so we must compare
    // the weight field (index 2), not the whole array (a - b on arrays is NaN).
    edges.sort((a, b) => a[2] - b[2]);

    const dsu = new DisjointSetUnion(v);
    let mstWeight = 0;
    let edgesCount = 0;

    // Step 2: Iterate over sorted edges
    for (let [u, target, weight] of edges) {
        // Union successfully returns true if no cycle forms
        if (dsu.union(u, target)) {
            mstWeight += weight;
            edgesCount++;
            if (edgesCount === v - 1) break; // MST complete
        }
    }

    return mstWeight;
}
```
*   **Complexity:** Time: **O(E log E)** dominated by the sorting step, Space: **O(V)** to hold disjoint parent ranks.

---

## 8. GRAPH ALGORITHMS SELECTION CHEAT SHEET 🗺️

**Bacho, interview room mein target identify karne ke liye is quick logic flowchart ko hamesha dimaag mein rakhna:**

```
                                 ALGORITHM ROUTING TREE
                                           │
         ┌─────────────────────────────────┴─────────────────────────────────┐
  Is Graph Weighted?                                                Is Graph Unweighted?
         │                                                                   │
         ├──────────────────────────┐                                        ▼
  Single-Source shortest path?   All-pairs shortest path?               Use BFS Traversal!
         │                          │
         ├──────────────┐           ▼
  Positive weights?  Negative?   Floyd-Warshall
         │              │
         ▼              ▼
     Dijkstra      Bellman-Ford
```

---

## 9. SDE CORNER: HARDCORE PRACTICAL INTERVIEW PROBLEMS

🚀 **Arey bacho! Mixed conceptual problems par dynamic algorithms recognize karo!**

---

### Problem A (Medium): Course Schedule II (LeetCode 210)
*Total `numCourses` aur prerequisites di hain. Hum kis sequence order mein courses complete karein ki dependencies valid rahein?*

#### 🧠 Diagnostics:
*   *Observation:* Courses have prerequisite dependencies (directed relationships), aur loops (cycles) allow nahi hain. This is a clear **DAG & Topological Sort** pattern!
*   *Paradigm:* Use **Kahn’s Indegree BFS** for easy cycle validation.

```javascript
function findOrder(numCourses, prerequisites) {
    const topo = topologicalSortKahn(numCourses, prerequisites);
    return topo; // Kahn returns empty if cycle detected
}
```
*   **Complexity:** Time: **O(V + E)**, Space: **O(V + E)**.

---

### Problem B (Hard): Network Delay Time (LeetCode 743)
*Network coordinates `K` se signal throw hota hai. Vertices nodes directions weights hold karte hain. Minimum time nikalna hai jab network ke saare active hosts tak signal successfully reach kar jaye.*

#### 🧠 Diagnostics:
*   *Observation:* Weighted network graph diya hai jahan edges non-negative times metrics (weights) hold karte hain. Humein single source `K` se shortest delay time map calculate karna hai.
*   *Paradigm:* Strictly **Dijkstra's Algorithm** with priority queues!

```javascript
function networkDelayTime(times, n, k) {
    const { distances } = dijkstra(n, times.map(([u, v, w]) => [u - 1, v - 1, w]), k - 1);
    const maxDelay = Math.max(...distances);
    return maxDelay === Infinity ? -1 : maxDelay;
}
```
*   **Complexity:** Time: **O((V + E) log V)**, Space: **O(V + E)**.

---

## 10. COMMON SDE TRAPS & INTERVIEW MISTAKES TO AVOID ⚠️

1.  **Dijkstra negative relaxation crash:**
    Dijkstra's algorithm ko negative edges wale graph par apply kar dena. Negative weights ke aate hi hamesha **Bellman-Ford** use karein.
2.  **Outer loop ordering inside Floyd-Warshall:**
    Floyd-Warshall code karte waqt, intermediate node k ko inner loops mein define karna. **Remember: k hamesha outermost loop mein hona chahiye** taaki matrices states safely propagate ho sakein.
3.  **Forgetting DSU path compression returns:**
    DSU class implementations mein normal recursive search array pointers set karna bina returned variables updates update coordinates links key compress kiye.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Strictly ordered Directed Acyclic Graphs and Kahn's indegree computations.
* Dijkstra's Single Source greedy path recovery limits.
* Bellman-Ford multi-relaxations and all-pairs Floyd-Warshall grids.
* Disjoint Set Union representations with rank compression techniques.

---

### SDE Practice Roadmap:
1.  Complete LeetCode 207 *Course Schedule* using Kahn's algorithm.
2.  Solve LeetCode 743 *Network Delay Time* using Dijkstra.
3.  Implement *Kruskal's Algorithm* on weighted datasets.

---


