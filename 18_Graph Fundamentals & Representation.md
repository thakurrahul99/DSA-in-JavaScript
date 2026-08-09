**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par apna dhyan seedhe focus karo.**

Pichle chapter mein humne **Greedy Algorithms (Chapter 17)** ko completely breakdown kiya aur seekha ki kaise hum har step par locally optimal choices banakar globally optimal solutions tak pahunchte hain [cite: 706]. 

Lekin beta, ab tak humne jitne bhi data structures padhe hain—chahe wo Arrays hon, Linked Lists hon, ya hierarchical Trees hon—un sab mein ek structural limitation thi [cite: 220, 221]. 

*"Sir, Trees bhi to hierarchical hote hain, unme kya limitation hai?"*

Bacho, **Trees** mein humesha ek strict parent-to-child flow hota hai [cite: 221, 444]. Tree mein kabhi koi cycle (loop) nahi ho sakti, koi child apne parent ko wapas point nahi kar sakta, aur na hi do siblings ke beech direct edge ho sakti hai [cite: 443, 444].

Lekin real life mein, relationships itne restricted nahi hote! 
* Jaise **Social Networks (Facebook/LinkedIn)**: Tumhara dost tumhara sibling bhi ho sakta hai, aur uske dosto ka dost tumhara bhi dost ho sakta hai [cite: 6, 221, 297].
* Jaise **Google Maps/Road Networks**: Cities (Cities) aapas mein kisi bhi direction se roads se connect ho sakti hain [cite: 6, 221, 297].
* Jaise **Web Pages**: Ek website se doosri website par hyperlinks se jaa sakte hain, jo wapas pehli website ko point kar sakti hain [cite: 221, 299, 340].

In sabhi multi-directional networks ko dynamically represent karne ke liye hum computer science ka sabse powerful aur non-linear data structure padhte hain—**Graphs** [cite: 221, 336]! 

Apni copy aur pen nikal lo, aur whiteboard par dhyan lagao! 🚀

---

## 1. THE ARCHITECTURAL LEAP: TREE VS. GRAPH

Bacho, interviewers ka ek bohot hi favorite conceptual question hota hai: *"What is the difference between a Tree and a Graph?"* [cite: 18]

Dhyan se is visual blueprint aur statements ko samjho [cite: 532]:
> **"Every Tree is mathematically a special case of a Directed, Connected, and Acyclic Graph (DAG) [cite: 532]! Lekin har Graph ek Tree nahi hota [cite: 444]."**

```
           [ Strictly Hierarchical Tree ]               [ Free-flowing Network Graph ]
                     [ Root ]                                     [ A ]
                     /      \                                    /  \  \
                  [ B ]    [ C ]                                [ B ]──[ C ]
                  /   \                                          \    /
                [ D ]  [ E ]                                      [ D ]
          (1 Parent, No Cycles, Connected)              (Multiple paths, Cycle B-C-D-B possible)
```

### Core Architecture Differences:
1. **Root Node Constraint:** Tree mein hamesha ek unique starting node hota hai jise root node kehte hain, jiska koi parent nahi hota [cite: 443, 444]. Graph mein koi root node jaisa concept nahi hota [cite: 443, 539]; saare vertices barabar authority hold karte hain.
2. **Cycle Constraint:** Trees strictly acyclic hote hain (cycles mathematically forbidden hain) [cite: 444]. Graphs cyclic bhi ho sakte hain [cite: 337].
3. **Connectivity Constraint:** Trees hamesha single connected components hote hain (yaani har node tak pahunchne ka strictly ek hi path hota hai) [cite: 444]. Graphs connected bhi ho sakte hain aur disconnected (alag-alag disconnected islands) bhi [cite: 337, 462].

---

## 2. GRAPH TERMINOLOGY USING A SINGLE BLUEPRINT GRAPH

Bacho, terminology ko rट्टा (memorize) maarne ke bajay hum is single **Undirected Blueprint Graph** ko standard baseline banayenge aur saare terms isi se baar-baar reference karenge:

```
                          Our Consistent Example Graph:
                          
                                    [ A ]
                                   /     \
                                [ B ]───[ C ]
                                  \     /
                                   [ D ]      [ E ]  ◄── Isolated node!
```

### 1. Vertices (Nodes) & Edges:
* **Vertex / Node (\\(V\\)):** Network ke points ya entities [cite: 245, 539]. Humare graph mein: \\(\{A, B, C, D, E\}\\) vertices hain [cite: 245, 539].
* **Edge (\\(E\\)):** Do vertices ke beech ka link ya connection line [cite: 245, 539]. Humare graph mein edges hain: \\(\{(A, B), (A, C), (B, C), (B, D), (C, D)\}\\) [cite: 245, 539].

### 2. Directed vs. Undirected Graphs:
* **Undirected Graph:** Edges bidirectional "two-way streets" hoti hain [cite: 449, 514]. Agar edge \\((A, B)\\) hai, toh tum \\(A\\) se \\(B\\) bhi ja sakte ho aur \\(B\\) se \\(A\\) bhi [cite: 161, 449]. (e.g., Facebook friendships [cite: 221, 297, 448]).
* **Directed Graph (Digraph):** Edges unidirectional "one-way streets" hoti hain, jise arrows se show kiya jata hai [cite: 337, 449]. Edge \\(A \rightarrow B\\) ka matlab hai hum sirf \\(A\\) se \\(B\\) ja sakte hain, \\(B\\) se \\(A\\) nahi [cite: 161, 449]. (e.g., Twitter/Instagram followers).

### 3. Weighted vs. Unweighted Graphs:
* **Unweighted Graph:** Saare connections binary hote hain. Yaani do points connected hain (1) ya nahi (0) [cite: 539].
* **Weighted Graph:** Har edge ke sath ek cost, cost distance ya weight associated hota hai [cite: 245, 514, 539]. (e.g., Google Maps jahan edge weights do cities ke beech ka actual physical distance represent karte hain [cite: 6, 221, 514]).

### 4. Degree, In-Degree, & Out-Degree:
* **Degree (Undirected):** Kisi vertex ke sath directly connected edges ka total count [cite: 448].
  * Humare blueprint graph mein: Vertex \\(B\\) ki degree **3** hai (edges connected: \\(BA, BC, BD\\)) [cite: 448]. Vertex \\(E\\) ki degree **0** hai.
* **In-degree (Directed only):** Kisi node ki taraf aane wali incoming edges ka count.
* **Out-degree (Directed only):** Kisi node se nikalne wali outgoing edges ka count.

### 5. Connected vs. Disconnected & Connected Components:
* **Connected Graph:** Agar graph ke kisi bhi vertex se kisi bhi doosre vertex tak jaane ka koi na koi rasta (path) ho, toh wo connected graph hai.
* **Disconnected Graph:** Agar graph mein koi aisa node ho jahan tak koi path na ho (jaise humare blueprint mein node \\(E\\) completely isolated betha hai), toh wo disconnected graph hai [cite: 462, 532].
* **Connected Component:** Disconnected graph ke andar ke un sub-graphs ko connected components (islands) kehte hain jo internally completely connected hote hain [cite: 337, 485]. Humare blueprint mein do connected components hain: \\(\{A, B, C, D\}\\) aur \\(\{E\}\\) [cite: 337].

### 6. Path, Simple Path, & Cycle:
* **Path:** Ek sequence of vertices jahan har consecutive pair ek valid edge se connected ho [cite: 337, 482]. (e.g., \\(A \rightarrow B \rightarrow D \rightarrow C\\) is a path).
* **Simple Path:** Aisa path jismein koi bhi vertex **repeat nahi hota** [cite: 337]. (e.g., \\(A \rightarrow B \rightarrow C\\) is simple).
* **Cycle:** Ek aisa path jo jahan se shuru ho, ghoom kar usi node par end ho jaye [cite: 337, 444, 485]. (e.g., \\(B \rightarrow C \rightarrow D \rightarrow B\\) is a cycle) [cite: 337, 444].
* **DAG (Directed Acyclic Graph):** Ek aisa directed graph jismein directional flows follow karne par koi cycle generate na ho [cite: 337, 339]. (Very useful for course prerequisites scheduling [cite: 221, 299, 339]).

---

## 3. CORE GRAPH REPRESENTATIONS & JS IMPLEMENTATIONS

Bacho, graphs ko memory mein represent karne ke teen standard tarike hote hain [cite: 472, 532, 539]. Har ek representation ke apne trade-offs hain. Humein in representations ko generic objects/arrays se implement karna seekhna hai bina kisi heavy library ke [cite: 245, 539].

---

### REPRESENTATION 1: Adjacency Matrix 📊

#### A. Concept:
Adjacency Matrix ek 2D array (grid) hota hai jiska size \\(V \times V\\) hota hai [cite: 342, 539]. Row indices aur Column indices vertices represent karte hain [cite: 539].
* If `matrix[i][j] === 1`, it means vertex \\(i\\) se vertex \\(j\\) tak ek edge exist karti hai [cite: 539].
* If `matrix[i][j] === 0`, no edge exists [cite: 539].
* Weighted graph ke liye, we store the actual `weight` instead of `1` [cite: 539].

#### B. Example Graph Representation (Our Blueprint):
```
Vertices: A=0, B=1, C=2, D=3, E=4

          A   B   C   D   E
      A [ 0,  1,  1,  0,  0 ]
      B [ 1,  0,  1,  1,  0 ]
      C [ 1,  1,  0,  1,  0 ]
      D [ 0,  1,  1,  0,  0 ]
      E [ 0,  0,  0,  0,  0 ]
```
*(Notice bacho! Undirected graph ka adjacency matrix hamesha principal diagonal ke symmetric hota hai!)*

#### C. JavaScript Code (Clean Matrix implementation):
```javascript
class GraphMatrix {
    constructor(numVertices) {
        this.V = numVertices;
        // Allocate a V x V matrix filled with 0s [cite: 539]
        this.matrix = Array.from({ length: numVertices }, () => new Array(numVertices).fill(0));
    }

    // Add Edge - O(1) Time [cite: 471, 535]
    addEdge(u, v, isDirected = false) {
        this.matrix[u][v] = 1; // Mark link [cite: 539]
        if (!isDirected) {
            this.matrix[v][u] = 1; // For undirected graphs, mark symmetric back-link [cite: 467]
        }
    }

    // Access Neighbors - O(V) Time
    getNeighbors(u) {
        const neighbors = [];
        for (let v = 0; v < this.V; v++) {
            if (this.matrix[u][v] === 1) {
                neighbors.push(v);
            }
        }
        return neighbors;
    }
}
```

#### D. Complexity:
* **Space Complexity:** **\\(\mathcal{O}(V^2)\\)** hamesha, chahe graph mein edges hon ya na hon [cite: 342, 539].
* **Time Complexity:**
  * Add Edge: **\\(\mathcal{O}(1)\\)** [cite: 471, 535].
  * Remove Edge: **\\(\mathcal{O}(1)\\)** [cite: 471, 535].
  * Check if Edge \\((u, v)\\) exists: **\\(\mathcal{O}(1)\\)** [cite: 539].
  * Find Neighbors of \\(u\\): **\\(\mathcal{O}(V)\\)** linear check.

#### E. When to use?
Jab graph **Highly Dense** ho (yaani total edges almost \\(V^2\\) ke barabar hon) aur humein constant time edge lookup chahiye ho [cite: 337, 342].

---

### REPRESENTATION 2: Adjacency List 🔗 (The SDE Gold Standard!)

#### A. Concept:
Adjacency List ek Map, Object ya Array of arrays hota hai jahan har vertex as a key store hota hai, aur uski value ek array (list) hoti hai jismein uske saare immediate connected neighbor vertices store hote hain [cite: 10, 245, 539].

#### B. Data Structure Model (Our Blueprint):
```javascript
// Adjacency List using standard JavaScript Map
const adjacencyList = {
    'A': ['B', 'C'],
    'B': ['A', 'C', 'D'],
    'C': ['A', 'B', 'D'],
    'D': ['B', 'C'],
    'E': [] // Isolated node stays empty!
};
```

#### C. JavaScript Code (Clean Adjacency List):
```javascript
class GraphList {
    constructor() {
        this.adjacencyList = new Map(); // Store as vertex -> neighbor array map [cite: 245, 339]
    }

    // Add Vertex - O(1) Time [cite: 340, 471]
    addVertex(vertex) {
        if (!this.adjacencyList.has(vertex)) {
            this.adjacencyList.set(vertex, []); // Initialize empty array [cite: 298, 339]
        }
    }

    // Add Edge - O(1) Time [cite: 340, 471]
    addEdge(u, v, isDirected = false) {
        // Ensure both vertices exist in our map [cite: 465]
        this.addVertex(u);
        this.addVertex(v);

        this.adjacencyList.get(u).push(v); // u -> v connection [cite: 298]
        if (!isDirected) {
            this.adjacencyList.get(v).push(u); // v -> u symmetric back-link [cite: 298, 467]
        }
    }

    // Access Neighbors - O(1) Fetch
    getNeighbors(u) {
        return this.adjacencyList.get(u) || []; // Simply return neighbor array [cite: 298]
    }
}
```

#### D. Complexity:
* **Space Complexity:** **\\(\mathcal{O}(V + E)\\)** completely optimized [cite: 340, 539]. Hum sirf wahi connections store kar rahe hain jo actual exist karte hain [cite: 14, 539]!
* **Time Complexity:**
  * Add Edge / Vertex: **\\(\mathcal{O}(1)\\)** [cite: 340, 471].
  * Remove Edge: **\\(\mathcal{O}(E)\\)** worst case (needs array search to slice connection).
  * Check if Edge \\((u, v)\\) exists: **\\(\mathcal{O}(V)\\)** in worst-case dense structures.
  * Find Neighbors of \\(u\\): **\\(\mathcal{O}(\text{Degree}(u))\\)** which is super fast.

#### E. When to use?
This is the **default choices for 95% of interview questions** [cite: 342], specially when the graph is **Sparse** (Edges \\(E \ll V^2\\)) [cite: 337, 539].

---

### REPRESENTATION 3: Edge List 🗺️

#### A. Concept:
Edge List ek simple flat array hota hai jismein saare edges pairs or triplets ke form mein bikhre hote hain [cite: 342, 539]. Koi explicit node tracking nahi hoti, bas connected links stored hote hain.

#### B. JavaScript Code & Weighted Representation:
```javascript
// Unweighted Edge List
const edgeList = [
    ['A', 'B'],
    ['A', 'C'],
    ['B', 'C'],
    ['B', 'D'],
    ['C', 'D']
];

// Weighted Edge List (Vertex 1, Vertex 2, Edge Weight) [cite: 245, 514]
const weightedEdgeList = [
    ['A', 'B', 5], // A connected to B with weight 5 [cite: 519]
    ['A', 'C', 3], // A connected to C with weight 3 [cite: 519]
    ['B', 'D', 9]  // B connected to D with weight 9 [cite: 519]
];
```

#### C. Complexity:
* **Space Complexity:** **\\(\mathcal{O}(E)\\)** [cite: 342].
* **Time Complexity:**
  * Edge Lookup: **\\(\mathcal{O}(E)\\)** (puray array par scan lagana padega) [cite: 342].
  * Neighbor Lookup: **\\(\mathcal{O}(E)\\)**.

#### D. When to use?
Kruskal's Algorithm aur Bellman-Ford algorithms jaise edge-based processing scenarios mein ye pattern sabse fast hota hai [cite: 376, 792, 798, 803].

---

## 4. INTEGRITY CHECK: WHY WE NEED VISITED TRACKING? ⚠️

Trees mein hum recursive steps par aage jaate the bina kisi history tracker ke, kyunki tree mein cycle hona impossible hai [cite: 444, 490]. Lekin bacho, graphs mein cycles ho sakti hain [cite: 337]!

Maan lo tumhare paas ek cyclic graph hai: `A ──► B ──► C ──► A` [cite: 337].

```
                                  [ A ] ────► [ B ]
                                    ▲          │
                                    │          ▼
                                    └──────── [ C ]
```

### The Infinite Loop Disaster 🚨:
Agar tum \\(A\\) se traversal shuru karoge bina kisi tracker ke:
1. \\(A\\) kahega: *"Mera neighbor \\(B\\) hai, use visit karo!"* [cite: 493]
2. \\(B\\) kahega: *"Mera neighbor \\(C\\) hai, use visit karo!"* [cite: 493]
3. \\(C\\) kahega: *"Mera neighbor \\(A\\) hai, use visit karo!"* [cite: 493]
4. \\(A\\) kahega: *"Mera neighbor \\(B\\) hai..."* [cite: 493]

Yeh loop infinite chalta rahega jab tak ki call stack overflow ho kar crash nahi ho jata [cite: 68, 490]!

### The Solution (Visited Tracker 🛡️):
Hum traversal ke dauran ek helper **Set ya Array** maintain karte hain jise hum kehte hain **`visited`** [cite: 298, 491].
* Pehle check karo: *"Kya ye node pehle visited ho chuka hai?"* [cite: 298, 493]
* Agar `visited.has(node)` is `true`, toh direct backtrack return kar jao [cite: 298]!
* Agar nahi hai, toh use `visited.add(node)` karke aage explore karo [cite: 298].

Is clear logic ko hum next chapter (BFS/DFS traversals) mein deep programmatic implement karenge [cite: 298, 506].

---

## 5. INTERVIEW PATTERN: INPUT FORMAT CONVERSION

Coding platforms (LeetCode/GFG) par graphs kabhi bhi direct graph classes ke form mein nahi diye hote [cite: 245, 342]. Interviewer hamesha tumhein do integers `V` (Vertices) aur `E` (Edges), aur ek **Edge List 2D Array** input dega [cite: 245, 342].

Tumhara sabse pehla step hamesha isi raw Edge List ko ek clean **Adjacency List** mein convert karna hoga [cite: 245, 342]!

### 🧠 Step-by-Step Diagnostic Block: Edge List to Adj List

#### 1. Understand:
Do inputs diye hain: total vertices count `n` aur ek 2D array `edges` representing undirected unweighted paths. Ise standard Adjacency List Map format mein construct karo.

#### 2. Visual Representation:
```
Input: n = 4, edges = [,,,]
Output: Map { 0 =>, 1 =>, 2 =>, 3 => }
```

#### 3. JavaScript Code:
```javascript
function buildAdjacencyList(n, edges) {
    const adjList = new Map();

    // Step 1: Initialize list for each vertex [cite: 298]
    for (let i = 0; i < n; i++) {
        adjList.set(i, []);
    }

    // Step 2: Populate bidirectional links linearly [cite: 298]
    for (let [u, v] of edges) {
        adjList.get(u).push(v); // Forward Link [cite: 298]
        adjList.get(v).push(u); // Backward Link (as undirected) [cite: 298, 467]
    }

    return adjList;
}
```

#### 4. Dry Run:
Input: `n = 3, edges = [,]`
* **Init Map:** `0 -> [], 1 -> [], 2 -> []`
* **Iteration 1 (``):** 
  * Push `1` into `0`'s array \\(\rightarrow\\) `0 ->`
  * Push `0` into `1`'s array \\(\rightarrow\\) `1 -> `
* **Iteration 2 (``):**
  * Push `2` into `1`'s array \\(\rightarrow\\) `1 ->`
  * Push `1` into `2`'s array \\(\rightarrow\\) `2 ->`
* **Returns:** `Map { 0 =>, 1 =>, 2 => }`. Perfect!

#### 5. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(V + E)\\)** linear initialization and edge tracing.
* **Space Complexity:** **\\(\mathcal{O}(V + E)\\)** auxiliary to store the graph inside map [cite: 340].

---

## 6. PROGRESSIVE PRACTICE BOARD (FOUNDATION QUESTIONS)

🚀 **Arey bacho! Whiteboard clean hai aur basic concepts ready hain. Traversals se pehle in foundation logical questions par haath saaf karo!**

---

### Problem 1 (Easy): Find Degree of a Node in Undirected Graph
*Given an Adjacency List of an undirected graph and a vertex target node `u`, return its degree.*

#### 🧠 Diagnostics:
* *Choices:* Undirected graph mein degree node ke neighbor connections array ka total size hoti hai [cite: 448].
* *Optimal Approach:* Map se array fetch karke direct `length` read kar lo [cite: 298]!

```javascript
function getDegree(adjList, u) {
    if (!adjList.has(u)) {
        return 0; // Node absent case [cite: 469]
    }
    return adjList.get(u).length; // Array length is degree [cite: 448]
}
```
* **Complexity:** Time: **\\(\mathcal{O}(1)\\)** constant retrieval, Space: **\\(\mathcal{O}(1)\\)**.

---

### Problem 2 (Medium): Find the Town Judge (LeetCode 997)
*In a town of `n` people, there is a rumor that one of them is a town judge. The judge has 2 properties: (1) Judge trusts nobody, (2) Everyone trusts the judge. You are given a trust edge list. Find the judge.*

#### 🧠 Diagnostics:
* *Observation:* Yeh directed graph representation ka problem hai! 
  * Judge trusts nobody \\(\rightarrow\\) Judge has **Out-degree = 0**.
  * Everyone trusts judge \\(\rightarrow\\) Judge has **In-degree = n - 1**.
* *Optimal Approach:* Hum ek single array coordinate system maintain karenge: `trustScore = InDegree - OutDegree`. Jis individual ka total trust score `n - 1` hoga, wahi Town Judge hai!

```javascript
function findJudge(n, trust) {
    // Array size (n + 1) to match 1-indexed values
    const trustScore = new Array(n + 1).fill(0);

    for (let [u, v] of trust) {
        trustScore[u]--; // Outgoing edge from u (decrements score)
        trustScore[v]++; // Incoming edge to v (increments score)
    }

    for (let i = 1; i <= n; i++) {
        if (trustScore[i] === n - 1) {
            return i; // Found the judge!
        }
    }

    return -1; // No judge found
}
```
* **Complexity:** Time: **\\(\mathcal{O}(V + E)\\)** single-pass, Space: **\\(\mathcal{O}(V)\\)** for trackers.

---

## 7. CORE GRAPH COMPLEXITY & TRADE-OFF SUMMARY

Whiteboard ke is matrix grid comparison ko dhyan se note karo bacho, ye competitive analysis mein absolute SDE benchmark hai [cite: 342, 535]:

| Representation | Space Complexity 💾 | Check Edge \\((u, v)\\) exists ⏱️ | Find Neighbors of \\(u\\) ⏱ | Best Suited Scenario |
| :--- | :--- | :--- | :--- | :--- |
| **Adjacency Matrix** | **\\(\mathcal{O}(V^2)\\)** [cite: 342, 539] | **\\(\mathcal{O}(1)\\)** [cite: 539] | **\\(\mathcal{O}(V)\\)** | Dense graphs, frequent edge validation checks [cite: 337, 342]. |
| **Adjacency List** | **\\(\mathcal{O}(V + E)\\)** [cite: 340, 539] | **\\(\mathcal{O}(\text{Degree}(u))\\)** | **\\(\mathcal{O}(\text{Degree}(u))\\)** | Sparse graphs, BFS/DFS traversal operations [cite: 337, 340, 342]. |
| **Edge List** | **\\(\mathcal{O}(E)\\)** [cite: 342] | **\\(\mathcal{O}(E)\\)** [cite: 342] | **\\(\mathcal{O}(E)\\)** | Edge-centric sorted algorithms (Jaise MST Kruskal) [cite: 342, 359, 803]. |

---

## 8. SDE TRAPS & COMMON MISTAKES TO AVOID ⚠️

Graph structural questions code karte waqt in 3 bugs se hamesha satark rehna:

1. **Undirected Edge Misalignment (Half-edge connection):**
   Undirected graphs build karte waqt edge \\(u \rightarrow v\\) to set kar dena, par symmetric edge \\(v \rightarrow u\\) set karna bhool jana [cite: 467]. Isse graph directed ban jata hai aur traversal output corrupt ho jata hai [cite: 32].
2. **Missing Visited Tracking (Memory leak crash):**
   Cyclic graph traversal par visited tracker arrays checks missing rakhna, jisse stack frames collapse ho jate hain [cite: 68, 490].
3. **Array index allocations bounds errors:**
   Edge lists conversion par coordinates ko incorrect `0` index base ya standard limits offset parameters assumptions par trace map arrays crash karna.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Transition of hierarchical restricted Trees into open network Graphs [cite: 532].
* Fundamental terminologies: Vertices, Edges, Degrees, Cycles, Connected Components, DAG [cite: 337, 444, 471, 485].
* Dynamic implementations of Adjacency Matrix, Adjacency List, and Edge List formats [cite: 245, 342, 539].
* The mathematical necessity of `visited` states tracking blocks [cite: 490, 506].

### SDE Graph Patterns Mastered:
* **The InDegree-OutDegree differential score matrix** to resolve networking trust lookups.
* **Bi-directional list building pipeline** from jumbled 2D edges arrays.

---

### SDE Practice Roadmap:
1. Implement Graph representations using custom arrays and maps [cite: 245, 342, 539].
2. Solve *Find the Town Judge* on LeetCode 997.
3. Dry run standard cycles infinite loops issues on cyclical graphs [cite: 490].

---

