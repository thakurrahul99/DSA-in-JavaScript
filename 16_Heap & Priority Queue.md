**Arey bacho! Jaldi se apni seats par baith jao aur dhyan seedhe whiteboard par lagao.**

Pichle chapters mein humne **Binary Trees (Chapter 14)** aur **Binary Search Tree (Chapter 15)** ko bohot hi deep logically aur recursively seekha. Humne dekha ki BST mein elements ek strictly left-to-right balanced state mein align hote hain taaki search operations quickly **O(log n)** mein run ho sakein.

Lekin bacho, real-world systems design aur interview programming mein ek bohot hi unique requirement aati hai:
*"Sir, humein pure database ko sort ya search nahi karna. Humein toh bas dynamically aur repeatedly **sabse important/priority element (yaani absolute Minimum ya Absolute Maximum)** ko instantly fetch aur delete karna hai!"*

Maan lo tum ek CPU Scheduler design kar rahe ho jahan high-priority tasks ko pehle run hona hai, ya fir tum LeetCode par streaming data ke beech mein se constantly **K-th largest element** dhoondh rahe ho. 

Agar tum iske liye normal sorted array use karoge, toh elements insert karte hi pooray array ko shift karna padega, jo linear time **O(n)** le lega. Agar normal BST use karoge, toh tree unbalanced (skewed) hote hi operations linear **O(n)** par crash ho jayenge!

Is complex bottleneck ko bypass karne ke liye computer science mein ek bohot hi dhasu, high-performance, aur complete binary tree-based structure design kiya gaya hai jise hum kehte hain—**Heap (Priority Queue)**!

Apne pen-register nikal lo, aur whiteboard par dhyan lagao! 🚀

---

## 1. COMPLEXITY MATRIX: HEAP VS. BST VS. SORTED ARRAY

Bacho, sabse pehle concept ko dimaag mein fit karne ke liye is matrix comparison cheat-sheet ko dekho:

| Operation | Sorted Array 📊 | Balanced BST 🎯 | Binary Heap (Min/Max) 🏔️ |
| :--- | :--- | :--- | :--- |
| **Peek (Min/Max)** | **O(1)** | **O(log n)** | **O(1)** — Invariant strictly top node par hota hai. |
| **Insert** | O(n) — Shift memory shifts. | O(log n). | **O(log n)** — Leaf swap to parent path. |
| **Delete (Min/Max)** | O(n). | O(log n). | **O(log n)** — Swap root with last leaf & sink. |
| **Search (Arbitrary)**| **O(log n)** via Binary Search. | **O(log n)**. | **O(n)** — Kyunki horizontal elements order strictly left-right monotonic nahi hota! |
| **Space Overhead** | **O(1)** — Packed contiguous slots. | High — Left, Right & Parent pointers nodes space. | **O(1) Auxiliary** — Stored directly inside a flat array! |

---

## 2. THE COMPLETE BINARY TREE (CBT) CONNECTION

*"Sir, Heap to ek Tree hai. Phir iska space overhead O(1) kaise ho sakta hai?"*

Bacho, yahi heap ka sabse bada masterstroke hai! Heap ko hum hamesha ek **Complete Binary Tree (CBT)** ke form mein structure karte hain.

### Complete Binary Tree Rule:
1. Tree ke saare levels **completely filled** hone chahiye, siwaye aakhri level ke.
2. Aakhri level par bache saare nodes hamesha **strictly left-to-right** filled hone chahiye.

```
               [ Valid Complete Binary Tree ]          [ Invalid CBT (Gaps in Last Level) ]
                           1                                           1
                        /     \                                     /     \
                       2       3                                   2       3
                      / \     /                                   /       /
                     4   5   6                                   4       6  ◄── (Gap at 5!)
```

### Why Heap is usually implemented using a flat Array? 💡
Kyunki complete binary tree mein koi gaps nahi hote, isiliye humein memory waste karke dynamic pointer objects (Left/Right nodes) generate karne ki zaroorat nahi padti! Hum pure tree ko ek normal **JavaScript Array** mein safely aur compact store kar sakte hain.

```
                    Array Representation Mapping:
                    
                     Tree:         [ 10 ] (index 0)
                                  /      \
                      (index 1) [ 15 ]    [ 30 ] (index 2)
                                /    \
                    (index 3) [ 40 ]  [ 50 ] (index 4)
                    
                     Array:    [ 10,  15,  30,  40,  50 ]
                                 0     1     2     3     4
```

---

### The Sacred Mathematical Arithmetic Index Formulas 📐
Flat array mein binary relationships find out karne ke liye Williams (1964) ne teen clean index calculations formulas diye hain:

For any node positioned at index **`i`**:
1. **Left Child Index:**
   \\[Left = 2i + 1\\]
2. **Right Child Index:**
   \\[Right = 2i + 2\\]
3. **Parent Index:**
   \\[Parent = ⌊ i - 1/2 ⌋\\]

#### Let's verify!
If we are at `index 1` (`value 15`):
* Left Child = `2 * 1 + 1 = index 3` (`value 40`). Perfect!
* Parent = `Math.floor((1 - 1) / 2) = index 0` (`value 10`). Absolutely correct!

---

## 3. HEAP PROPERTIES: MIN HEAP vs. MAX HEAP

Heap ka binary model do invariants follow karta hai:

```
                     [ Min-Heap Structure ]                    [ Max-Heap Structure ]
                               1                                         100
                            /     \                                    /     \
                           5       8                                  40      50
                          / \     / \                                /  \    /  \
                         10 12   15 20                              10  15  30  20
                     (Parent <= Children)                       (Parent >= Children)
```

1. **Min-Heap Property:** Har parent node ki value hamesha apne left aur right children ki values se **choti ya barabar (<=)** hoti hai. Root node par hamesha array ka **absolute minimum** element betha hota hai.
2. **Max-Heap Property:** Har parent node ki value hamesha apne children se **badi ya barabar (>=)** hoti hai. Root node par absolute maximum element betha hota hai.

---

## 4. CORE MECHANICS: STEP-BY-STEP TRAVERSALS

Bacho, jab hum heap mein koi element add ya delete karte hain, toh Complete Binary Tree ka structural format and Heap property violate ho sakti hain. In invariants ko restore karne ke liye do core operations run hote hain:

### A. Insertion: Heapify Up (Bubble Up / Percolate Up) 🫧
Maan lo humare Min-Heap `` mein ek naya element **`5`** insert hota hai:

1. **Step 1:** Complete Binary Tree property preserve karne ke liye hum `5` ko array ke absolute **end (leaf position)** par place kar dete hain. Array becomes ``.
2. **Step 2 (Compare with Parent):** `5` at index 4 ka parent index `Math.floor((4-1)/2) = 1` (`value 15`) hai. Since `5 < 15`, parent-child rule violate ho raha hai. Dono ko swap kar do. Array: ``.
3. **Step 3 (Compare with Next Parent):** Now `5` at index 1 ka parent index `0` (`value 10`) hai. Since `5 < 10`, swap again! Array: ``.
4. **Step 4:** `5` reached the root node (index 0), bubble-up stops. Perfectly heapified up!

---

### B. Extraction: Heapify Down (Sift Down / Sink Down) ⚓
Maan lo humein Min-Heap `` se root element `5` ko extract (delete) karna hai:

1. **Step 1:** Root ko direct pop out karne se tree do parts mein break ho jayega. Hum structural gap prevent karne ke liye root node ko array ke **last element (`15`)** ke sath replace (swap) kar dete hain aur last node slice out kar dete hain. Array: ``.
2. **Step 2 (Sink Down):** Index 0 (`value 15`) ko sink down karna padega kyunki parent chota hona chahiye. Current index `0` ke left child (`10`) and right child (`30`) ko compare karo. Dono children mein se jo **smallest value (index 1: `10`)** hai use select karo. Since `15 > 10`, swap parent with index 1! Array: ``.
3. **Step 3:** Current `15` at index 1 has left child at index 3 (`40`). Since `15 < 40`, heap property satisfies. Traversal stops!

---

## 5. CLASSROOM WORKBENCH: THE SDE HEAP CODEBLUEPRINTS

Bacho, is generic, highly optimized aur interview-ready class structures ko dhyan se copy par note kar lo. Humne isme ek robust custom comparator callback use kiya hai taaki interviewers objects comparison poochein toh hum standardly generic behave kar sakein.

### A. Robust Min Heap Implementation (No External Libraries)

```javascript
class MinHeap {
    constructor(compareFn = (a, b) => a - b) {
        this.heapContainer = []; // Dynamic Array representation
        this.compare = compareFn; // Custom comparator callback
    }

    // O(1) Fetch
    peek() {
        if (this.heapContainer.length === 0) return null; //
        return this.heapContainer; // Head is always at index 0
    }

    // O(log n) Insert
    add(item) {
        this.heapContainer.push(item); // Append at the absolute leaf
        this.heapifyUp(); // Rebalance upwards
        return this;
    }

    // O(log n) Extract Root
    poll() {
        if (this.heapContainer.length === 0) return null; //
        if (this.heapContainer.length === 1) return this.heapContainer.pop(); //

        const rootVal = this.heapContainer; //
        this.heapContainer = this.heapContainer.pop(); // Swap root with last leaf
        this.heapifyDown(); // Sink down new root
        return rootVal;
    }

    // Bubble Up Logic
    heapifyUp(startIndex) {
        let currentIndex = startIndex !== undefined ? startIndex : this.heapContainer.length - 1; //

        while (currentIndex > 0) {
            const parentIndex = Math.floor((currentIndex - 1) / 2); //
            // If parent is greater than child, swap (Min-Heap invariant)
            if (this.compare(this.heapContainer[parentIndex], this.heapContainer[currentIndex]) > 0) {
                this.swap(parentIndex, currentIndex);
                currentIndex = parentIndex; // Move tracking pointer
            } else {
                break;
            }
        }
    }

    // Sink Down Logic
    heapifyDown(startIndex = 0) {
        let currentIndex = startIndex;
        const length = this.heapContainer.length;

        while (2 * currentIndex + 1 < length) { // While left child exists
            let leftChildIndex = 2 * currentIndex + 1; //
            let rightChildIndex = 2 * currentIndex + 2; //
            let smallestChildIndex = leftChildIndex;

            // If right child exists and is smaller than left child, select right
            if (rightChildIndex < length && 
                this.compare(this.heapContainer[rightChildIndex], this.heapContainer[leftChildIndex]) < 0) {
                smallestChildIndex = rightChildIndex; //
            }

            // If current element is larger than smallest child, swap
            if (this.compare(this.heapContainer[currentIndex], this.heapContainer[smallestChildIndex]) > 0) {
                this.swap(currentIndex, smallestChildIndex);
                currentIndex = smallestChildIndex; // Move tracking pointer
            } else {
                break; // Heap property restored
            }
        }
    }

    swap(i, j) {
        const temp = this.heapContainer[i];
        this.heapContainer[i] = this.heapContainer[j];
        this.heapContainer[j] = temp; //
    }

    size() {
        return this.heapContainer.length;
    }

    isEmpty() {
        return this.heapContainer.length === 0; //
    }
}
```

---

### B. Robust Max Heap Implementation (Custom Comparator Bypass)
Bacho, Max Heap implement karne ke liye humein alag se poora code likhne ki koi zaroorat nahi hai! Hum dynamic reverse comparator pass karke same code ko instant Max Heap ki tarah run kar sakte hain:

```javascript
// Max-Heap: comparator changes from (a-b) to (b-a)
const maxHeap = new MinHeap((a, b) => b - a); 
```

---

### C. Generic Priority Queue Design Blueprint
System integrations jahan nodes priority data models (jaise `{ task: "A", priority: 1 }`) par map ho:

```javascript
class PriorityQueue {
    constructor() {
        // MinHeap customized for priority key comparison
        this.heap = new MinHeap((nodeA, nodeB) => nodeA.priority - nodeB.priority);
    }

    enqueue(element, priority) {
        this.heap.add({ element, priority }); //
    }

    dequeue() {
        const node = this.heap.poll(); //
        return node ? node.element : null;
    }

    peek() {
        const node = this.heap.peek(); //
        return node ? node.element : null;
    }

    size() {
        return this.heap.size();
    }
}
```

---

## 6. MATHEMATICAL PROOFS: INTERVIEW EXPLANATION DRILLS

Product companies ke interviewers code run karwane ke baad deep mathematics aur complexity derivations par bacho ko trap karte hain. Whiteboard par bana ek-ek mathematical proof dhyan se samajhna:

```
                            HEIGHT VS. NODES RELATIONSHIP
                                       
                                        ●             <-- Height h (1 node)
                                      /   \
                                     ●     ●          <-- Height h-1 (2 nodes)
                                    / \   / \
                                   ●   ● ●   ●        <-- Height 0 (N/2 nodes)
```

### A. Why Build Heap is O(n) and NOT O(n log n)? 📐
Agar hum array ke elements par sequentially `add()` loop chalayein, toh overall Build Heap time complexity **O(n log n)** aayegi. Lekin ek highly optimized approach hai jise hum kehte hain **Floyd's Heap Construction**:
* Hum array ke aakhir se shuru karke middle element `Math.floor(N/2)` tak reverse direction mein hamesha **`heapifyDown`** run karte hain.
* *Why middle?* Kyunki CBT ke last level ke saare elements leaf nodes hote hain, unki height `0` hoti hai so unhe sink karne ki zaroorat nahi hai!
* Let's derive total work:
  \\[Total Work = Σ_{h=0}^{log n} Nodes at height  h × Work to Heapify Down  h\\]
  \\[Nodes at height  h <= \frac{n}{2^{h+1}}\\]
  \\[Total Work <= Σ_{h=0}^{log n} \frac{n}{2^{h+1}} × O(h) = n/2 Σ_{h=0}^{log n} h/2^h\\]
* Calculus ke infinite arithmetico-geometric series approximation rule se sum of Σ_{h=0}^{∞} h/2^h converges to strictly **`2`**.
* Therefore, total work becomes:
  \\[Total Work <= n/2 × 2 = O(n)\\]
* Floyd's construction build heap time is strictly **O(n)**.

---

### B. Why is searching an arbitrary element O(n) and NOT O(log n)?
BST mein humein pata hota hai ki chote elements strictly left side par hain aur bade right side. But heap mein left aur right dynamic child positions par koi sorted alignment rule nahi hota.
* Hum sirf parent se chote elements verify kar sakte hain. Isiliye target element kis branch mein betha hai, yeh bin comparisons predict karna impossible hai.
* Pure heap tree ko target check karne ke liye sequentially traverse karna padega, isiliye arbitrary searching **O(n)** linear checks leti hai.

---

## 7. HEAP SORT DEMYSTIFIED: IN-PLACE ALIGNMENT

Bacho, Williams (1964) ne Heap concept ke sath hi pure in-place sorting algorithm design kiya jise hum **Heap Sort** kehte hain.

### How Heap Sort Works:
1. **Phase 1:** Flat jumbled input array par Floyd's Heap construction chalakar use flat **Max Heap** mein in-place transform karo. Build time: **O(n)**.
2. **Phase 2 (Repeated Extraction):** Max Heap ka root node hamesha array ka largest element hota hai.
   * Root (index 0) ko last active boundary elements (index `i`) ke sath swap kar do.
   * Heap active boundary sizes range ko decrement (`i--`) karo.
   * Root index `0` par `heapifyDown` run karke balancing restore karo.
3. Array automatic reverse ascending sorted transform ho jata hai!

---

### Complete In-Place JS Code:
```javascript
function heapSort(arr) {
    const n = arr.length;

    // Phase 1: Build Max-Heap in-place from bottom-up
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapifyDown(arr, n, i);
    }

    // Phase 2: Repeated Extraction of maximum element
    for (let i = n - 1; i > 0; i--) {
        // Swap root (max element) with last unsorted leaf element
        const temp = arr;
        arr = arr[i];
        arr[i] = temp;

        // Restore Max-Heap property on reduced heap size range i
        heapifyDown(arr, i, 0);
    }
    return arr;
}

function heapifyDown(arr, size, currentIndex) {
    let largest = currentIndex;
    const left = 2 * currentIndex + 1;
    const right = 2 * currentIndex + 2;

    if (left < size && arr[left] > arr[largest]) {
        largest = left;
    }
    if (right < size && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest !== currentIndex) {
        const swap = arr[currentIndex];
        arr[currentIndex] = arr[largest];
        arr[largest] = swap;

        heapifyDown(arr, size, largest); // Recursively sink down
    }
}
```

---

### SDE Algorithm Battle: Heap Sort vs. Merge vs. Quick Sort ⚔️

* **Heap Sort:** Strictly **O(n log n)** in all cases, **O(1)** auxiliary space (pure in-place). Slower constant factor and cache-inefficient due to pointer address fragmentation. Unstable Sort.
* **Merge Sort:** Strictly **O(n log n)** time, but takes **O(n)** extra space allocations. Stable sort.
* **Quick Sort:** Average **O(n log n)**, worst case **O(n^2)**. Highly cache-friendly and faster in-memory constant runtimes. Unstable Sort.

---

## 8. SDE PATTERN RECOGNITION CHEATSHEET 🗺️

**Beta, interview room mein dhyan se questions ko observe karke in keywords ko dhoondhna:**

1. **"Find the K-th Largest / Smallest Element" Clue:**
   Pure array ko sort karne ki zaroorat nahi hai (which takes O(n log n)). Size `K` ka heap maintain karke hum linear checks **O(n log k)** mein targets nikal sakte hain!
2. **"Streaming Data / Continuous dynamic updates" Clue:**
   Jab elements dynamic continuously arrays streams mein push ho rahe hon, aur constant median ya priority elements fetch karne hon.
3. **"Merge Multiple Sorted lists" Clue:**
   Pointers nodes range checks ke liye hamesha heap comparisons patterns track coordinates use karein.

---

## 9. CLASSROOM PRACTICE ROOM (EASY → MEDIUM → HARD)

🚀 **Arey bacho! Board completely clean hai. Pehle solution par haath rakhkar khud try karo!**

---

### Problem 1 (Easy): K-th Largest Element in an Array (LeetCode 215)

*   **Problem Statement:** Unsorted integer array diya hai, iska **K-th largest element** return karo strictly dynamic heap execution calculations ke sath.

#### 🧠 Diagnostics:
* **The Trick 💡:** K-th Largest element nikalne ke liye hum ek **Min-Heap** size `K` ka maintain karenge!
* **Why Min-Heap?** Kyunki Min-Heap top par hamesha apne elements ka *minimum* element hold karega. Jab elements count limits `K` ko exceed karegi, toh hum top se chote element ko poll out remove kar denge. 
* Aakhir mein size `K` ke pool mein strictly **sabse bade K elements** bachenge, aur un bade elements ka minimum (root node) hi hamara absolute **K-th largest element** hoga!

```javascript
function findKthLargest(nums, k) {
    // Instantiate Min-Heap using standard constructor
    const minHeap = new MinHeap((a, b) => a - b);

    for (let num of nums) {
        minHeap.add(num); // Add element
        // If heap size exceeds k, drop the smallest element
        if (minHeap.size() > k) {
            minHeap.poll(); // O(log k) operation
        }
    }
    return minHeap.peek(); // Topmost is the K-th largest!
}
```
* **Complexity:** Time: **O(n log k)**, Space: **O(k)** auxiliary heap buffer.

---

### Problem 2 (Medium): Merge K Sorted Lists (LeetCode 23)

*   **Problem Statement:** `K` sorted linked lists di gayi hain, unhe combine karke ek single sorted linked list return karni hai pointers node swaps aur heap comparisons se.

#### 🧠 Diagnostics:
* **Brute Force:** Saare nodes ko direct array mein copy karo, sort method run karo aur final pointers list update generate karo. Complexity: **O(N log N)** (where N is total elements count).
* **Optimal (Min-Heap Multi-way Merge 💡):**
  * Hum saari sorted lists ke starting heads pointer elements ko **Min-Heap** mein add kar dete hain.
  * Constant step par heap se absolute minimum element pop/poll out karenge, use merged linked list dummy tail par attach karenge.
  * Aur jis popped list node ka item poll hua, usi node list branch ke agle element pointers node block `node.next` ko heap par push kar denge!

```javascript
function mergeKLists(lists) {
    const minHeap = new MinHeap((nodeA, nodeB) => nodeA.val - nodeB.val);
    const dummy = { val: -1, next: null }; // Dummy node setup
    let tail = dummy;

    // Step 1: Push head nodes of all K lists into the heap
    for (let list of lists) {
        if (list !== null) {
            minHeap.add(list);
        }
    }

    // Step 2: Extract minimum recursively and push adjacent successor
    while (!minHeap.isEmpty()) {
        const minNode = minHeap.poll(); // Fetch min pointer node
        tail.next = minNode; // Append to merged list tail
        tail = tail.next; // Move tail

        // If successor node exists, push to heap
        if (minNode.next !== null) {
            minHeap.add(minNode.next);
        }
    }
    return dummy.next;
}
```
* **Complexity:** Time: **O(N log k)** (where N is total nodes count and k is number of linked lists), Space: **O(k)** active elements within heap container map.

---

### Problem 3 (Hard): Find Median from Data Stream (LeetCode 295)

*   **Problem Statement:** Dynamic stream of integers chal raha hai. Kisi bhi moment par stream ka absolute **Median** constant time fetch operation par evaluate karna hai.

#### 🧠 Diagnostics:
* **The "Two Heaps" SDE Secret Pattern 💡:**
  * Hum data stream ko do parts mein partition kar denge: Left Half (holding small elements) aur Right Half (holding large elements).
  * Small elements ko hum store karenge ek **Max-Heap (Left Portion)** mein, taaki Left Half ka *maximum value* root par direct mil sake.
  * Large elements ko hum store karenge ek **Min-Heap (Right Portion)** mein, taaki Right Half ka *minimum value* top par direct visible ho.

```
                     Left Portion (Max-Heap)   │   Right Portion (Min-Heap)
                           [ 10, 15 ]          │          [ 30, 40 ]
                               ▲               │              ▲
                       Max element (15)        │       Min element (30)
                                               ▼
                                      Median = (15 + 30) / 2 = 22.5
```

---

#### 🧠 JavaScript Implementation:
```javascript
class MedianFinder {
    constructor() {
        this.maxHeap = new MinHeap((a, b) => b - a); // Left half (small elements)
        this.minHeap = new MinHeap((a, b) => a - b); // Right half (large elements)
    }

    addNum(num) {
        // Step 1: Push to Left Half Max-Heap
        this.maxHeap.add(num);

        // Balancing Phase: Left elements must be smaller than right elements
        this.minHeap.add(this.maxHeap.poll());

        // Balance heap sizes: Left size must be >= Right size
        if (this.maxHeap.size() < this.minHeap.size()) {
            this.maxHeap.add(this.minHeap.poll());
        }
    }

    findMedian() {
        if (this.maxHeap.size() > this.minHeap.size()) {
            return this.maxHeap.peek(); // Odd size total elements
        }
        return (this.maxHeap.peek() + this.minHeap.peek()) / 2; // Even size
    }
}
```
* **Complexity:** Time: **O(log n)** for `addNum`, strictly **O(1)** constant lookup for `findMedian`! Space: **O(n)** auxiliary.

---

## 10. COMMON SDE MISTAKES & INTERVIEW TRAPS ⚠️

Bacho, interviews ke emotional stress mein in 4 bugs se hamesha bacho:

1. **Incorrect Left/Right Child Index Math:**
   Index `0` calculations boundary par `2*i` and `2*i + 1` likhna galti hai bacho! If root is at index `0`, left child must be `2*i + 1` and right child must be `2*i + 2`.
2. **Assuming Heap elements are fully Sorted:**
   *"Sir, Max-Heap array format mein data complete descending sequence par visible kyun nahi hai?"*  
   Remember: Heap property only guarantees parent-child relationships, elements internally sorting format sequences follow nahi karte! Descending elements fetch karne ke liye hamesha repeated pop elements extract operation lagana compulsory hai.
3. **Full sorting overhead on Top-K queries:**
   Top-K queries par pure array par sorted algorithms execute karna quadratic overhead scale par performance crash triggers karta hai. Hamesha size `K` dynamic heap check maintain karein.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Complete Binary Trees properties and packed storage array mappings.
* Parent and child index formulas via floor division checks.
* Upward bubble-up and downward sift-down heapify rebalancing.
* In-place Heap Sort implementation mechanics.

### Mastered Patterns:
* **Size-K Heap buffers** to execute K-th queries in logarithmic bounds.
* **Two Heaps (Min/Max)** split systems for constant lookup medians.
* **Multi-way merge steps** with pointers arrays priority queues.

---

### SDE Practice Roadmap:
1. Complete *K-th Largest Element* on LeetCode 215.
2. Solve *Top K Frequent Elements* (LeetCode 347).
3. Build *Median from Data Stream* (LeetCode 295) using custom heaps.

---


