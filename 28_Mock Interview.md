**Arey bacho! Jaldi se apni-apni seats par baith jao aur blackboard par apna dhyan seedhe focus karo.**

Pichle saare chapters mein humne DSA ke raw fundamentals se lekar highly advanced graph routing aur interval structures ko deeply decode kiya hai [cite: 303, 315, 350]. Humne seekha ki kaise dynamic programming ke transition arrays space optimize hote hain aur bit manipulation se micro-second performance bounds kaise capture kiye jaate hain.

Lekin beta, interview room ka pressure ek completely alag beast hai [cite: 15]. Wahan sirf code likhna kafi nahi hota. Wahan dekha jata hai ki naye problem ko padh kar aap kaise clarify karte ho, kaise loud (verbalize) sochte ho, kaise step-by-step optimal transitions derive karte ho, aur badalti hui constraints par kaise pivot karte ho [cite: 10, 11, 15, 756].

Aaj hum **Chapter 28: Mock Interviews** mein enter kar rahe hain. Is chapter ko notes ki tarah mat padhna. Yeh ek **Interactive Whiteboard Workbook** hai jahan hum 6 dynamic rounds chalayenge—Fundamentals se lekar high-level SDE design patterns tak.

Chalo, pen aur rough register taiyaar rakho, aur pehle round ke liye taiyaar ho jao! 🚀

---

## ROUND 1: FUNDAMENTALS (ARRAYS, STRINGS, WINDOWS & PREFIXES)

### 🎙️ The Interview Stage:
*   **Interviewer:** "Welcome! Let's start with a classic. You are given an array of positive integers `nums` and a target integer `k` [cite: 331]. Find the length of the **longest contiguous subarray** that contains **at most `k` distinct elements**."
*   **Teacher's Whiteboard Hint:** *Beta, contiguous subarray pucha hai aur positive elements hain. Kuch dimaag mein chamka? [cite: 524]*

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "Sir, are the elements strictly positive? And can \\(k\\) be greater than the array length?"
    *   *Interviewer:* "Yes, elements are positive integers. If \\(k\\) is greater than the unique elements in the array, the answer should be the total length of the array."
2.  **Brute-Force thinking:**
    *   *Candidate:* "We can generate all possible contiguous subarrays using nested loops (\\(O(N^2)\\)). For each subarray, we insert elements into a `Set` to check if unique count \\(\le k\\) [cite: 273, 292, 310]. This takes \\(O(N^3)\\) time in the worst case."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "Since we are looking for a *contiguous* subarray and the elements are positive, when we expand our subarray to the right, the number of distinct elements can only increase or stay the same [cite: 524]. We don't need to restart our scan from every index. We can maintain a dynamic window defined by two pointers (`left` and `right`) [cite: 524]. We will use a `Map` to track the frequency of elements in our active window [cite: 351, 524]. When the size of the Map exceeds \\(k\\), we shrink the window from the left [cite: 524]!"
4.  **Selected Pattern:** Sliding Window with Frequency Map lookup [cite: 304, 351, 524].

---

### 💻 JavaScript Code (Standard Production Quality):
```javascript
function longestSubarrayAtMostKDistinct(nums, k) {
    if (!nums || nums.length === 0 || k === 0) return 0; // Edge Case [cite: 28, 244]

    const freqMap = new Map(); // Tracks element frequencies [cite: 351]
    let left = 0;
    let maxLen = 0;

    for (let right = 0; right < nums.length; right++) {
        const rightNum = nums[right];
        freqMap.set(rightNum, (freqMap.get(rightNum) || 0) + 1); // Expand window [cite: 351]

        // Shrink window from left if unique elements exceed k
        while (freqMap.size > k) {
            const leftNum = nums[left];
            freqMap.set(leftNum, freqMap.get(leftNum) - 1);
            if (freqMap.get(leftNum) === 0) {
                freqMap.delete(leftNum); // Clean up stale keys [cite: 322]
            }
            left++; // Shrink [cite: 524]
        }

        // Record maximum contiguous window length [cite: 524]
        maxLen = Math.max(maxLen, right - left + 1);
    }

    return maxLen;
}
```

---

### 🔍 Dry Run Table on `nums =`, `k = 2`:
*   **Initial State:** `left = 0`, `maxLen = 0`, `freqMap = Map {}`.

| step | right | nums[right] | Map State (Size) | left | Window Length | maxLen |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 0 | 1 | `{1 => 1}` (Size 1) | 0 | `0 - 0 + 1 = 1` | 1 |
| 2 | 1 | 2 | `{1 => 1, 2 => 1}` (Size 2) | 0 | `1 - 0 + 1 = 2` | 2 |
| 3 | 2 | 1 | `{1 => 2, 2 => 1}` (Size 2) | 0 | `2 - 0 + 1 = 3` | 3 |
| 4 | 3 | 2 | `{1 => 2, 2 => 2}` (Size 2) | 0 | `3 - 0 + 1 = 4` | 4 |
| 5 | 4 | 3 | `{1 => 2, 2 => 2, 3 => 1}` (Size 3 > k) | 0 | *Exceeds! Loop starts.* | 4 |
| 5a | *Shrink*| - | `{1 => 1, 2 => 2, 3 => 1}` (Size 3) | 1 | *Still exceeds!* | 4 |
| 5b | *Shrink*| - | `{1 => 0 (deleted), 2 => 2, 3 => 1}` (Size 2) | 2 | `4 - 2 + 1 = 3` | 4 |

*   **Returns:** `maxLen = 4` (subarray ``).
*   **Complexity:** Time: **\\(O(N)\\)** because each element is visited at most twice (once by `right`, once by `left`). Space: **\\(O(K)\\)** auxiliary memory to store at most \\(k+1\\) distinct elements in the map [cite: 125, 524].

---

### 🧭 Interviewer Follow-ups & Optimization Drills:
*   **"What if values are negative?" [cite: 705]**
    *   *Candidate:* "Since we are checking the count of *distinct* elements rather than the running sum of elements, the sliding window logic remains strictly invariant of whether elements are positive, negative, or zero. It will work flawlessly [cite: 524]!"
*   **"What if the input is streaming?" [cite: 308]**
    *   *Candidate:* "If the stream is dynamic, we can wrap this window state in a streaming class with internal `left` pointer and `freqMap` [cite: 369]. Every time a new element arrives, we perform the insertion and map size validations in amortized \\(O(1)\\) time, allowing real-time queries [cite: 3, 517]."

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 1 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ Exceptional (Handled positive/bounds)   │
  │ Clarifying Questions   │ High-level (Addressed duplicates/size)  │
  │ Pattern Recognition    │ Spot on (Identified Sliding Window)     │
  │ Code Quality & Space   │ Modular, clean, O(K) space bounds       │
  └────────────────────────┴─────────────────────────────────────────┘
```

*   **What was done well:** Candidate avoided the nested loop trap and realized that frequency mapping is ideal for element distinctness tracking [cite: 351, 524].
*   **Where reasoning could fail:** If candidate forgot to delete keys when their frequency hit `0`, the `freqMap.size` would remain high, leading to an incorrect result [cite: 322].

---

## ROUND 2: DATA STRUCTURES (LISTS, STACKS, QUEUES & TREES)

### 🎙️ The Interview Stage:
*   **Interviewer:** "Excellent. Let's move to Round 2. Here is a hierarchical problem. Given the root of a binary tree, write a function to determine if it is a **valid Binary Search Tree (BST)**." [cite: 331, 356, 521]

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "Are duplicates allowed in this BST? And do node values fit within normal JavaScript safe integer ranges?" [cite: 420]
    *   *Interviewer:* "A valid BST must have strictly smaller values on the left and strictly greater values on the right [cite: 537]. No duplicates are allowed. Node values can be any 32-bit signed integers [cite: 380]."
2.  **Brute-Force thinking:**
    *   *Candidate:* "For every node, we can check if it is greater than the maximum value in its left subtree and smaller than the minimum value in its right subtree [cite: 537]. This will require traversing subtrees repeatedly, leading to \\(O(N^2)\\) or \\(O(N \log N)\\) complexity depending on balance [cite: 537]."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "A valid BST property means that every node must exist within a valid **interval range** \\((\text{minVal}, \text{maxVal})\\) [cite: 537]. When we transition to the left child, the maximum allowed bound changes to the parent's value [cite: 537]. When we transition to the right child, the minimum allowed bound changes to the parent's value [cite: 537]. We can perform a preorder DFS traversal passing these boundaries [cite: 521]!"
4.  **Selected Pattern:** Recursive Tree Depth-First Search with dynamic interval boundaries [cite: 521].

---

### 💻 JavaScript Code (Standard Production Quality):
```javascript
function isValidBST(root) {
    return validate(root, -Infinity, Infinity);
}

function validate(node, min, max) {
    if (node === null) return true; // Base Case: empty node is valid [cite: 329]

    // Current node value must lie strictly within (min, max) interval
    if (node.data <= min || node.data >= max) {
        return false;
    }

    // Left subtree must be strictly less than node.data [cite: 537]
    // Right subtree must be strictly greater than node.data [cite: 537]
    return validate(node.left, min, node.data) && 
           validate(node.right, node.data, max);
}
```

---

### 🔍 Dry Run Table on `Tree`:
*   *Root node (10)* [cite: 389].

```
                     (min: -Inf, max: Inf)
                    /    \
  (min: -Inf, max: 10)   (min: 10, max: Inf)
                     
```

*   **Step 1:** Call `validate(10, -Inf, Inf)`. Correct, node value `10` is within limits.
*   **Step 2:** Call `validate(5, -Inf, 10)`. Correct, node value `5` is within limits.
*   **Step 3:** Call `validate(15, 10, Inf)`. Correct, node value `15` is within limits.
*   **Returns:** `true`.

---

### 🧭 Interviewer Follow-ups & Optimization Drills:
*   **"Can you do it without recursion?" [cite: 521]**
    *   *Candidate:* "Yes! An **inorder traversal of a valid BST must yield elements in strictly sorted ascending order** [cite: 329]. We can use an iterative inorder traversal with a custom Stack and track the value of the previously visited node [cite: 350, 521]. If the current value is less than or equal to the previous value, the BST is invalid! This avoids recursive call stack overhead [cite: 521]."

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 2 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ Spot on (Derived BST interval boundary) │
  │ Pattern Recognition    │ High (Connected inorder sorted property)│
  │ Code Quality           │ Exceptional, clean boundaries           │
  │ Complexity Analysis    │ Correct: Time O(N), Space O(H)          │
  └────────────────────────┴─────────────────────────────────────────┘
```

*   **What was done well:** Candidate successfully identified that a node's validity depends on its global ancestors, not just its immediate children [cite: 537].
*   **Where reasoning could fail:** If candidate simply wrote `node.left.data < node.data && node.right.data > node.data`, it would fail for deep violation subtrees (e.g., a node `12` sitting on the left side of root `10`).

---

## ROUND 3: ALGORITHMS (SEARCHING, BFS/DFS, SHORTEST PATHS)

### 🎙️ The Interview Stage:
*   **Interviewer:** "Excellent. Let's move to Round 3: Algorithms. You are given a weighted directed graph represented as a list of delay times. Find the **minimum time required** for a signal to propagate from a starting source node \\(K\\) to all other nodes in the network." [cite: 515, 516, 531, 532, 542]

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "Can edge weights be negative? And is it possible that some nodes are completely unreachable from source \\(K\\)?"
    *   *Interviewer:* "All weights are non-negative integers. If any node is unreachable, return `-1`."
2.  **Brute-Force thinking:**
    *   *Candidate:* "We can find all possible paths from source \\(K\\) to every node recursively using DFS backtracking and select the minimum cost [cite: 31]. But this will take exponential \\(O(V!)\\) time and is highly redundant [cite: 32, 405]."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "Since this is a weighted directed graph with non-negative edge weights and we need the shortest path from a single source \\(K\\) to all other nodes, this is the classic **Dijkstra's Algorithm** [cite: 382, 515, 516, 532, 542]! Dijkstra uses a greedy approach, updating vertex distances dynamically and selecting the next closest unvisited node [cite: 382, 436, 733, 764, 768, 779, 789]."
4.  **Selected Pattern:** Dijkstra's Shortest Path Routing using Priority Queue [cite: 382, 515, 516, 532, 542].

---

### 💻 JavaScript Code (Using Minimalist Ad-hoc MinHeap) [cite: 797]:
```javascript
class MinHeap {
    constructor() {
        this.heap = [];
    }
    push(item) {
        this.heap.push(item);
        this.bubbleUp();
    }
    pop() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        const min = this.heap;
        this.heap = this.heap.pop();
        this.sinkDown();
        return min;
    }
    bubbleUp() {
        let idx = this.heap.length - 1;
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2); // [cite: 128]
            if (this.heap[idx].weight < this.heap[parentIdx].weight) {
                [this.heap[idx], this.heap[parentIdx]] = [this.heap[parentIdx], this.heap[idx]];
                idx = parentIdx;
            } else break;
        }
    }
    sinkDown() {
        let idx = 0;
        const length = this.heap.length;
        while (2 * idx + 1 < length) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let smallest = leftChildIdx;
            if (rightChildIdx < length && this.heap[rightChildIdx].weight < this.heap[leftChildIdx].weight) {
                smallest = rightChildIdx;
            }
            if (this.heap[idx].weight > this.heap[smallest].weight) {
                [this.heap[idx], this.heap[smallest]] = [this.heap[smallest], this.heap[idx]];
                idx = smallest;
            } else break;
        }
    }
    isEmpty() {
        return this.heap.length === 0;
    }
}

function networkDelayTime(times, n, k) {
    // Step 1: Build Adjacency List [cite: 417, 534]
    const adj = Array.from({ length: n + 1 }, () => []);
    for (let [u, v, w] of times) {
        adj[u].push({ node: v, weight: w });
    }

    // Step 2: Initialize Distance map
    const dist = new Array(n + 1).fill(Infinity);
    dist[k] = 0;

    const pq = new MinHeap(); // [cite: 797]
    pq.push({ node: k, weight: 0 });

    while (!pq.isEmpty()) {
        const { node: currNode, weight: currWeight } = pq.pop();

        // Stale element check: Skip if we found a shorter path already
        if (currWeight > dist[currNode]) continue;

        for (let edge of adj[currNode]) {
            const nextNode = edge.node;
            const edgeWeight = edge.weight;
            const newDist = currWeight + edgeWeight;

            // Relaxation step
            if (newDist < dist[nextNode]) {
                dist[nextNode] = newDist;
                pq.push({ node: nextNode, weight: newDist });
            }
        }
    }

    // Find the max time taken to reach any node
    let maxDelay = 0;
    for (let i = 1; i <= n; i++) {
        if (dist[i] === Infinity) return -1; // Unreachable node [cite: 531]
        maxDelay = Math.max(maxDelay, dist[i]);
    }

    return maxDelay;
}
```

---

### ⏱️ Complexity:
*   **Time Complexity:** **\\(O((V + E) \log V)\\)** where \\(V\\) is the number of vertices and \\(E\\) is the number of edges [cite: 523]. Pop and push operations on the min-heap take logarithmic time \\(O(\log V)\\) [cite: 502, 518, 533].
*   **Auxiliary Space Complexity:** **\\(O(V + E)\\)** to maintain the adjacency list and distance map [cite: 512, 531].

---

### 🧭 Interviewer Follow-ups & Optimization Drills:
*   **"What if edge weights can be negative?"**
    *   *Candidate:* "Dijkstra's greedy choice will fail on negative weights because visited nodes cannot be correctly re-evaluated [cite: 382]. In that case, we must transition to the **Bellman-Ford Algorithm** which relaxes all edges \\(V-1\\) times in \\(O(V \cdot E)\\) time [cite: 420, 732, 751, 763, 769]."

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 3 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ Perfect (Addressed unreachability)      │
  │ Pattern Recognition    │ High (Identified Dijkstra's model)      │
  │ Code Quality           │ Outstanding (Modular heap implementation)│
  │ Complexity Analysis    │ Accurate log-linear edge relaxation     │
  └────────────────────────┴─────────────────────────────────────────┘
```

*   **What was done well:** The candidate correctly implemented a custom Heap and explained relaxation limits [cite: 332, 523, 797].
*   **Where reasoning could fail:** If candidate didn't include `currWeight > dist[currNode]` check, the algorithm would perform redundant evaluations for stale heap entries.

---

## ROUND 4: DYNAMIC PROGRAMMING & ADVANCED

### 🎙️ The Interview Stage:
*   **Interviewer:** "Impressive. Let's step up to Round 4. You are given two strings `word1` and `word2` [cite: 77]. Find the **minimum number of operations** required to convert `word1` to `word2` using **insert, delete, or replace** operations." [cite: 414]

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "Can we assume inputs are lowercase alphabets? And what is the length constraint on the strings?"
    *   *Interviewer:* "Yes, they contain lowercase letters. The lengths are up to \\(500\\)."
2.  **Brute-Force thinking:**
    *   *Candidate:* "We can recursively try all three choices (insert, delete, replace) at every mismatch position [cite: 414]. This creates an exponential recursive tree with a time complexity of \\(O(3^{M+N})\\)."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "This problem contains a lot of **overlapping subproblems** (e.g., comparing suffixes repeatedly). We can define a 2D state where **`dp[i][j]`** is the minimum edits to convert substring `word1[0...i-1]` to `word2[0...j-1]`.
    *   **The Transition Logic:**
        *   If `word1[i-1] === word2[j-1]` \\(\to\\) `dp[i][j] = dp[i-1][j-1]` (No edit cost).
        *   Else \\(\to\\) `dp[i][j] = 1 + min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1])` (Insert, Delete, or Replace) [cite: 414]."
4.  **Selected Pattern:** 2D Dynamic Programming (Edit Distance) [cite: 414, 538].

---

### 💻 JavaScript Code (Standard Tabulation + Space Optimization) [cite: 538]:
```javascript
function minDistance(word1, word2) {
    const m = word1.length;
    const n = word2.length;

    // Space Optimization: We only need two rows of size N + 1
    let prev = Array.from({ length: n + 1 }, (_, j) => j); // Base cases (Insertions)
    let curr = new Array(n + 1).fill(0);

    for (let i = 1; i <= m; i++) {
        curr = i; // Column Base Case (Deletions)
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                curr[j] = prev[j - 1]; // No edit cost
            } else {
                curr[j] = 1 + Math.min(
                    prev[j - 1], // Replace
                    prev[j],     // Delete
                    curr[j - 1]  // Insert
                );
            }
        }
        prev = [...curr]; // Shift states [cite: 48]
    }

    return prev[n];
}
```

---

### 🔍 Dry Run Table on `word1 = "cat"`, `word2 = "cut"`:
*   `prev` initially: ``

| Row `i` (`word1`) | `curr` | `j = 1` ('c') | `j = 2` ('u') | `j = 3` ('t') | `prev` after transition |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1 ('c')** | 1 | `prev = 0` (Match!) | `1 + min(0,2,1) = 1` | `1 + min(1,3,1) = 2` | `` |
| **2 ('a')** | 2 | `1 + min(1,1,0) = 1` | `1 + min(0,1,1) = 1` | `1 + min(1,2,1) = 2` | `` |
| **3 ('t')** | 3 | `1 + min(2,1,1) = 2` | `1 + min(1,1,2) = 2` | `prev = 1` (Match!) | `` |

*   **Returns:** `1` (Replace 'a' with 'u').
*   **Complexity:** Time: **\\(O(M \times N)\\)**, Space: **\\(O(N)\\)** auxiliary optimized [cite: 125, 414].

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 4 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ High (Correct edit operations logic)    │
  │ Pattern Recognition    │ Spot on (Identified overlapping sub)    │
  │ Code Optimization      │ Exceptional (Optimized 2D to 1D space)  │
  │ Complexity Analysis    │ Accurate quadratic time bounds          │
  └────────────────────────┴─────────────────────────────────────────┘
```

---

## ROUND 5: MIXED SDE INTERVIEW

### 🎙️ The Interview Stage:
*   **Interviewer:** "Let's test your adaptive pattern recognition. You are given a set of **intervals** where `intervals[i] = [start_i, end_i]` [cite: 276]. You want to insert a `newInterval` into this sorted list, merging any overlapping intervals, and return the updated list sorted by start times." [cite: 276]

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "Are the intervals already sorted by their start coordinates?"
    *   *Interviewer:* "Yes, they are already sorted in ascending order."
2.  **Brute-Force thinking:**
    *   *Candidate:* "We can append the `newInterval` to the list, sort the entire collection again, and then merge overlapping intervals linearly [cite: 276]. This will take \\(O(N \log N)\\) time."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "Since the original list is **already sorted**, sorting again is redundant! We can do it in \\(O(N)\\) by dividing the process into three linear phases:
        1. Add all intervals that end *before* the `newInterval` starts.
        2. Merge all overlapping intervals by updating start and end boundaries.
        3. Add all remaining intervals that start *after* the merged interval ends [cite: 276]."
4.  **Selected Pattern:** Sorted interval scanning with chronological merging [cite: 276].

---

### 💻 JavaScript Code (Standard Production Quality):
```javascript
function insertInterval(intervals, newInterval) {
    const result = [];
    let i = 0;
    const n = intervals.length;

    // Phase 1: Add intervals that end before the new interval starts
    while (i < n && intervals[i] < newInterval) {
        result.push(intervals[i]);
        i++;
    }

    // Phase 2: Merge overlapping intervals
    while (i < n && intervals[i] <= newInterval) {
        newInterval = Math.min(newInterval, intervals[i]);
        newInterval = Math.max(newInterval, intervals[i]);
        i++;
    }
    result.push(newInterval); // Push the merged interval [cite: 276]

    // Phase 3: Add remaining intervals
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }

    return result;
}
```

---

### 🔍 Dry Run Table on `intervals = [,]`, `newInterval =`:
*   `newInterval =`, `result = []`, `i = 0`.

| phase | step | i | intervals[i] | Condition | Action | Result State |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Phase 1** | 1 | 0 | `` | `intervals (3) < newInterval (2)` is False. | Stop Phase 1 | `[]` |
| **Phase 2** | 2 | 0 | `` | `intervals (1) <= newInterval (5)` is True. | Merge: `newInterval` becomes `` | `[]` |
| **Phase 2** | 3 | 1 | `` | `intervals (6) <= newInterval (5)` is False. | Push ``, Stop Phase 2 | `[]` |
| **Phase 3** | 4 | 1 | `` | `i < n` is True. | Push `` | `[,]` |

*   **Returns:** `[,]`.
*   **Complexity:** Time: **\\(O(N)\\)** linear scan, Space: **\\(O(N)\\)** to return the final list [cite: 125, 276].

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 5 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ High (Correct chronological sorting use)│
  │ Pattern Recognition    │ Spot on (Avoided redundant sorting)     │
  │ Code Quality           │ Highly modular, pure linear phases      │
  │ Complexity Analysis    │ Accurate linear bounds                  │
  └────────────────────────┴─────────────────────────────────────────┘
```

---

## ROUND 6: HIGH-LEVEL INTERVIEW (STREAMING, CACHING & SCALING)

### 🎙️ The Interview Stage:
*   **Interviewer:** "Fantastic. Let's finish with a highly realistic SDE problem. Design a data structure for a **Least Recently Used (LRU) Cache** [cite: 443]. It should support `get` and `put` operations in strictly **\\(O(1)\\) constant time** [cite: 542]."

---

### 🧠 SDE Diagnostic Sandbox (Learner's Thoughts):
1.  **Clarifying Questions:**
    *   *Candidate:* "What is the key type? And do we have a fixed capacity constraint?"
    *   *Interviewer:* "Keys are positive integers. The cache has a fixed positive capacity."
2.  **Brute-Force thinking:**
    *   *Candidate:* "We can use an Array of objects to store key-value pairs and update their access timestamps [cite: 308]. But searching for a key or updating the least recently used element will take \\(O(N)\\) time [cite: 517]."
3.  **Optimal Observation & Pattern Recognition:**
    *   *Candidate:* "To achieve \\(O(1)\\) lookup, we need a **HashMap / Map** [cite: 542]. To maintain the access order of elements in \\(O(1)\\) time, we need a **Doubly Linked List** where we can easily remove and insert elements at the head/tail [cite: 542].
    *   **The Blueprint:**
        *   `Map` maps key \\(\to\\) Node reference in the Doubly Linked List [cite: 351, 542].
        *   `Doubly Linked List` holds the actual values [cite: 542]. The head represents the *Most Recently Used*, and the tail represents the *Least Recently Used*."
4.  **Selected Pattern:** HashMap combined with a Doubly Linked List [cite: 351, 542].

---

### 💻 JavaScript Code (High-Performance Implementation):
```javascript
class DoubleNode {
    constructor(key, val) {
        this.key = key;
        this.val = val;
        this.prev = null;
        this.next = null;
    }
}

class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.map = new Map(); // Key -> Node mapping [cite: 351, 542]
        
        // Dummy head & tail to avoid null pointer checks [cite: 242, 244, 245, 249]
        this.head = new DoubleNode(0, 0);
        this.tail = new DoubleNode(0, 0);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    _remove(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    _insertAtHead(node) {
        node.next = this.head.next;
        node.next.prev = node;
        this.head.next = node;
        node.prev = this.head;
    }

    get(key) {
        if (!this.map.has(key)) return -1; // [cite: 322]
        
        const node = this.map.get(key);
        this._remove(node);
        this._insertAtHead(node); // Move to head (Recently Used)
        return node.val;
    }

    put(key, value) {
        if (this.map.has(key)) {
            const node = this.map.get(key);
            node.val = value; // Update value
            this._remove(node);
            this._insertAtHead(node);
        } else {
            if (this.map.size === this.capacity) {
                // Remove Least Recently Used (tail.prev node)
                const lru = this.tail.prev;
                this.map.delete(lru.key); // Delete from hash map [cite: 322]
                this._remove(lru); // Remove from list
            }
            const newNode = new DoubleNode(key, value);
            this.map.set(key, newNode); // Insert to map [cite: 322]
            this._insertAtHead(newNode);
        }
    }
}
```

---

### 🧭 Interviewer Follow-ups & Optimization Drills:
*   **"In modern JS, can we simplify this using JavaScript's native Map?"**
    *   *Candidate:* "Yes! **JavaScript's native `Map` preserves keys in their insertion order** [cite: 320]. We can use this property to simulate an LRU cache by deleting and re-inserting keys on every access to make them 'recently used'!"

```javascript
// Native JS Map-based LRU Cache
class LRUCacheNative {
    constructor(capacity) {
        this.capacity = capacity;
        this.map = new Map(); // [cite: 322]
    }
    get(key) {
        if (!this.map.has(key)) return -1; // [cite: 322]
        const val = this.map.get(key);
        this.map.delete(key); // Remove and re-insert to refresh order [cite: 322]
        this.map.set(key, val);
        return val;
    }
    put(key, value) {
        if (this.map.has(key)) {
            this.map.delete(key); // Refresh key
        } else if (this.map.size === this.capacity) {
            // map.keys().next().value returns the oldest inserted key
            const oldestKey = this.map.keys().next().value;
            this.map.delete(oldestKey);
        }
        this.map.set(key, value);
    }
}
```
*   **Complexity:** Both implementations run strictly in **\\(O(1)\\)** time and **\\(O(\text{Capacity})\\)** space [cite: 125, 542]!

---

### 📊 SDE Evaluation Matrix:

```
                  ROUND 6 PERFORMANCE EVALUATION
  ┌────────────────────────┬─────────────────────────────────────────┐
  │ Understanding          │ Outstanding (Handled order preservation)│
  │ Pattern Recognition    │ Spot on (HashMap + DLL combination)     │
  │ Code Quality           │ Clean, encapsulated internal methods    │
  │ Complexity Analysis    │ Accurate constant time proofs           │
  └────────────────────────┴─────────────────────────────────────────┘
```

---

## 7. DSA READINESS ASSESSMENT & ROADMAP

Bacho, saare rounds khatam hone ke baad, ab aati hai self-introspection aur revision strategy ki baari!

### SDE Weak-Area Diagnostic Table 🩺:
*   **Weakness:** Tree traversals stack coordinate tracking mismatch \\(\to\\) **Action:** Practice DFS/BFS postorder traversal patterns [cite: 521].
*   **Weakness:** Wrong loop directions in Knapsack \\(\to\\) **Action:** Revise Chapter 22 & 23 transition indices.
*   **Weakness:** Unsorted Two-Pointer errors \\(\to\\) **Action:** Study Sorting complexities and array boundary checks [cite: 299].

---

### 30 / 60 / 90 Days SDE Practice Blueprint 📅

```
                             THE 90-DAY STUDY ROADMAP
                                        │
      ┌────────────────────────┬────────┴───────────────┬────────────────────────┐
      ▼                        ▼                        ▼                        ▼
  Days 0–30                Days 31–60               Days 61–90               SDE Target
  Linear & Hashing         Trees, Graphs, BFS/DFS   Advanced DP & Masks      Practice & Mocks
  [cite: 304, 351]         [cite: 305, 350]         [cite: 302, 375]         [cite: 307]
```

*   **Days 0–30 (Foundations):** Complete 10 Easy, 25 Medium problems on Arrays, Sliding Windows, and Hashing [cite: 309, 311].
*   **Days 31–60 (Structural):** Solve topological sort, Multi-source BFS, and priority queues [cite: 301, 305].
*   **Days 61–90 (Advanced):** Code Edit Distance, Matrix Chain Multiplication, and Bit manipulation [cite: 302, 375, 411].

---

## MOCK INTERVIEW CHECKLIST (THE FINAL STAND 📋)

1.  **Do NOT code immediately:** Always clarify boundaries first [cite: 10, 520]!
2.  **Think out loud:** Verbalize your thought choices to keep the interviewer engaged [cite: 11, 314].
3.  **Propose Brute Force first:** It establishes a working baseline and prevents locking up [cite: 10, 302, 520, 524].
4.  **Dry run manually:** Walk through the code with test cases before declaring victory [cite: 11, 34, 134].

---

⏮️ **Pichle Chapters ka Connection:** Chapter 1 to 27 ke saare data structures, recursion models, heap operations aur DP transitions ko humne ek complete functional SDE interview engine mein bundle kar diya hai [cite: 13, 315]!

⏭️ **Agle Chapter ka Setup: Chapter 29 — Behavioral SDE Interviews (How to handle 'Tell me about a time when...', project architecture walkthroughs, conflicts resolutions, and salary negotiations!) [cite: 16].**

---
