**Arey bacho! Jaldi se apni seats par baith jao aur dhyan seedhe whiteboard par lagao.**  

Pichle chapters mein humne **Linked List (Chapter 12)** ke dynamic pointer chains aur **Stacks & Queues (Chapter 13)** ke strict linear constraints (LIFO/FIFO) ko poore dhasu tarike se seekha. 

Lekin beta, ab tak humne jitne bhi data structures padhe hain—Arrays, Strings, Linked Lists, Stacks, ya Queues—wo sab ke sab **Linear Data Structures** hain. Yaani unme data ek single sequential line mein chalta hai.  

*"Sir, real world mein kya har cheez sequential hoti hai?"*  

Bilkul nahi bacho! Socho tumhare computer ka **File System**—ek main folder hota hai (Root), uske andar sub-folders hote hain, aur unke andar files hote hain. Humari HTML website ka **DOM (Document Object Model)**—ek HTML tag ke andar Body, uske andar Divs, aur unke andar Paragraphs hote hain.  

Yeh sab sequential nahi hain, ye **Hierarchical (shakhon wale)** hain! Aur isi hierarchical relationship ko efficiently store aur process karne ke liye hum use karte hain—**Binary Trees**!  

Aaj hum trees ko bilkul zero se shuru karke interview/problem-solving level tak leke jaenge. Apne pen-register nikal lo aur dhyan board par lagao! 🚀

---

## 1. TREE TERMINOLOGY: THE HIERARCHICAL MENTAL MODEL

Bacho, sabse pehle tree ke family structure aur uski terminologies ko samajhte hain. Ek tree nodes (blocks) aur edges (connections) ka collection hota hai. 

```
                                  [ A ]  ◄── Root Node (No Parent!)
                                 /     \
                       Edge ──► /       \
                               ▼         ▼
                             [ B ]     [ C ]  ◄── Siblings (Same Parent)
                            /     \         \
                           ▼       ▼         ▼
                         [ D ]   [ E ]     [ F ]  ◄── Leaf Nodes (No Kids!)
```

### The Anatomy of a Tree:
1. **Root Node:** Tree ka sabse topmost node jahan se tree shuru hota hai. Iska koi parent node nahi hota. (e.g., `A` is the Root).
2. **Node:** Ek container jo data store karta hai aur apne child nodes ke links (references) hold karta hai.
3. **Edge:** Do nodes ke beech ka connector link.
4. **Parent & Child:** Kisi node se niche nikalne wale nodes uske *children* hote hain, aur upar wala node unka *parent* hota hai. (e.g., `A` parent hai `B` aur `C` ka).
5. **Sibling:** Aise nodes jinka parent same ho. (e.g., `B` aur `C` siblings hain).
6. **Leaf Node:** Aise nodes jinse aage koi aur node nahi nikalta, yaani jinke koi children nahi hote. (e.g., `D`, `E`, aur `F` leaves hain).
7. **Ancestor & Descendant:** `A` se lekar kisi deep node ke path mein aane wale saare nodes uske *ancestors* hote hain, aur us node ke niche wale saare nodes uske *descendants* hote hain.

### Measurement Metrics (Depth vs. Height) ⚠️:
99% bache isme interview ke waqt mistake karte hain. Whiteboard par dhyan se dekho:
* **Height of a Node:** Kisi node se lekar sabse deepest leaf tak jaane wale path par jitne **edges** aate hain, use us node ki height kehte hain. (Sabse bottom leaves ki height `0` hoti hai, aur pooray tree ki height root node ki height hoti hai).
* **Depth of a Node:** Root node se lekar us particular node tak aane wale edges ka count depth kahlata hai. (Root ki depth `0` hoti hai).
* **Level:** Root se distance. Level of Root is `0` (ya kuch systems mein `1`).
* **Degree of a Node:** Kisi node ke paas max kitne immediate child nodes hain.
* **Subtree:** Kisi node ko root maankar uske niche jo chota tree banta hai, use subtree kehte hain.

---

## 2. WHAT IS A BINARY TREE & ITS SACRED TYPES?

### Binary Tree kya hai?
Aisa tree jismein kisi bhi parent node ke **maximum (at most) 2 children** ho sakte hain. Yaani kisi node ke paas ya toh `0` children honge, ya `1` child, ya `2` children. Unke naam hum hamesha **Left Child** aur **Right Child** rakhte hain.

### Types of Binary Trees (The Interview Classification 📋):

```
     Full Binary Tree         Perfect Binary Tree        Balanced Binary Tree
          [ 1 ]                     [ 1 ]                      [ 1 ]
         /     \                   /     \                    /     \
       [ 2 ]   [ 3 ]             [ 2 ]   [ 3 ]              [ 2 ]   [ 3 ]
              /     \           /   \   /   \              /
            [ 4 ]   [ 5 ]                 [ 4 ]  (Diff <= 1)
```

1. **Full Binary Tree:** Aisa tree jismein har node ke ya toh **0 children** ho ya **2 children** ho. Kisi ka bhi single child nahi hota!
2. **Complete Binary Tree:** Aisa tree jiske saare levels completely bhare hote hain, siwaye last level ke. Aur last level mein bhi nodes hamesha **left-to-right strictly filled** hone chahiye.
3. **Perfect Binary Tree:** Aisa tree jiske saare internal nodes ke strictly 2 children hote hain aur saare leaf nodes bilkul same depth par hote hain.
4. **Balanced Binary Tree:** Aisa tree jiske har single node ke liye left subtree ki height aur right subtree ki height ka difference (absolute value) **at most 1** ho. (Height difference `|lh - rh| <= 1`).
5. **Skewed / Pathological Binary Tree:** Aisa tree jismein har node ke paas max `1` child hota hai. Yeh linear linked list ki tarah behave karta hai aur operations complexity linear **O(n)** par degrade ho jati hai.

---

## 3. REPRESENTING A TREE IN JAVASCRIPT

SDE style mein code likhne ke liye hum JavaScript class use karte hain. Har node ek object hota hai jiske paas teen properties hoti hain—`value/data`, `left` reference child, aur `right` reference child.

```javascript
class Node {
    constructor(value) {
        this.value = value;
        this.left = null;  // Reference to left child node
        this.right = null; // Reference to right child node
    }
}
```

### Manual Binary Tree Building:
Chalo, is class se manually ek simple binary tree build karte hain:

```javascript
// Nodes allocate karo
const root = new Node(1);
root.left = new Node(2);
root.right = new Node(3);

root.left.left = new Node(4);
root.left.right = new Node(5);

/*
               Tree Structure Created:
                       1
                     /   \
                    2     3
                  /   \
                 4     5
*/
```

---

## 4. THE SACRED MENTAL MODEL: "HAR NODE EK CHHOTI PROBLEM HAI"

Bacho, loop chalakar tree traverse karna impossible hai kyunki tree dynamic branches mein divide hota hai. Isiliye tree problems ko solve karne ke liye **Recursion** humara sabse bada sahara hai!  

**Mantra of Tree Recursion (Dimaag mein fit karo 🧠):**  
"Har single node par khade hokar socho ki mujhe lagta hai mera left child aur right child sahi se calculations karke answer la kar de denge. Mujhe bas un dono answers ko current node ke data ke sath combine karke apne parent ko return karna hai!"

```
                                  [ Current Node ]
                                   /            \
                       Left Child /              \ Right Child
                                 ▼                ▼
                       [ Left Subtree ]     [ Right Subtree ]
                       (Compute recursively)(Compute recursively)
                                 \                /
                                  \              /
                                   ▼            ▼
                            Combine at Current & Return!
```

---

## 5. THE GREAT TRAVERSAL MATRIX: DFS VS. BFS

Bacho, tree ke saare nodes ko systematically visit karne ke process ko **Traversal** kehte hain. Humare paas do primary approaches hoti hain:

1. **DFS (Depth-First Search):** Pehle ek branch ki gehrai (depth) mein jaao, end tak explore karo, fir backtrack karke doosri branch par jao.
2. **BFS (Breadth-First Search):** Level-by-level wide explore karo. Pehle level 0, fir level 1, fir level 2.

```
                                  Tree Traversal Methods:
                                            │
                     ┌──────────────────────┴──────────────────────┐
            DFS (Depth-First)                             BFS (Breadth-First)
                     │                                             │
      ┌──────────────┼──────────────┐                              ▼
   Preorder       Inorder       Postorder                    Level Order
(Root-L-R)     (L-Root-R)     (L-R-Root)                    (Level-by-Level)
```

---

### A. Preorder, Inorder, and Postorder DFS (Recursive Blueprint) 🌿:

```javascript
// 1. Preorder Traversal (Root -> Left -> Right)
function preOrder(node, result = []) {
    if (node === null) return; // Base Case: empty branch
    
    result.push(node.value);      // 1. Visit Root
    preOrder(node.left, result);  // 2. Traverse Left Subtree
    preOrder(node.right, result); // 3. Traverse Right Subtree
    return result;
}

// 2. Inorder Traversal (Left -> Root -> Right)
function inOrder(node, result = []) {
    if (node === null) return; // Base Case
    
    inOrder(node.left, result);   // 1. Traverse Left Subtree
    result.push(node.value);      // 2. Visit Root
    inOrder(node.right, result);  // 3. Traverse Right Subtree
    return result;
}

// 3. Postorder Traversal (Left -> Right -> Root)
function postOrder(node, result = []) {
    if (node === null) return; // Base Case
    
    postOrder(node.left, result);   // 1. Traverse Left Subtree
    postOrder(node.right, result);  // 2. Traverse Right Subtree
    result.push(node.value);        // 3. Visit Root
    return result;
}
```

#### DFS Complexity:
* **Time Complexity:** **O(n)** kyunki hum har node ko exactly ek hi baar visit karte hain.
* **Space Complexity:** **O(h)** (where `h` is tree height) due to recursive call stack frames. Worst case skewed tree mein height n ho sakti hai, so **O(n)**.

---

### B. Iterative DFS (Stack replaces Call Stack 🥞):
Bacho, hum recursion ko explicitly ek **Stack** data structure se replace kar sakte hain.

#### Key Problem: Iterative Preorder Traversal (LeetCode 144)
* **Idea:** LIFO properties ke mutabik Stack use karenge. Pehle root ko push karo. Fir loop chalao: pop top node, record it. **Lekin yaad rakho!** Hum stack par pehle **Right Child** push karenge aur fir **Left Child** push karenge!
* *"Sir, right pehle kyun?"* Kyunki stack LIFO hai, jo element sabse aakhir mein push hoga, wahi sabse pehle pop hoga! Agar left last mein push hoga, toh pehle left process hoga.

```javascript
function iterativePreorder(root) {
    if (!root) return [];
    
    const result = [];
    const stack = [root]; // LIFO Stack

    while (stack.length > 0) {
        const curr = stack.pop(); // Get top node
        result.push(curr.value);

        // Push right first so that left stays at the top of stack
        if (curr.right) stack.push(curr.right);
        if (curr.left) stack.push(curr.left);
    }
    return result;
}
```

---

### C. Level Order Traversal BFS (Queue is naturally suited 🐇):

#### Why Queue for BFS?
BFS wide explore karta hai. Level-by-level tracking ke liye **Queue (FIFO)** perfect match hai. Jo node pehle level par add hoga, uske children bhi pehle level order mein queue se process honge.

#### Key Problem: Level Order Traversal (LeetCode 102)

```javascript
function levelOrder(root) {
    if (!root) return [];

    const result = [];
    const queue = [root]; // FIFO Queue

    while (queue.length > 0) {
        const levelSize = queue.length; // Number of elements in current level
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const curr = queue.shift(); // Dequeue from front (Amortized array operations)
            currentLevel.push(curr.value);

            // Left and Right child elements push to queue rear
            if (curr.left) queue.push(curr.left);
            if (curr.right) queue.push(curr.right);
        }
        result.push(currentLevel); // Record current level arrays
    }
    return result;
}
```

---

## 6. PROBLEM-SOLVING MASTERCLASS (STRUCTURED BREAKDOWNS)

🚀 **Whiteboard bilkul clean hai dosto! Chalo basic se advanced problems ko step-by-step resolve karte hain!**

---

### PROBLEM 1 (Easy): Count Nodes & Sum of Nodes recursively

#### 1. Understand:
Humein tree ke total nodes ka count aur unke mathematical values ka sum nikalna hai.

#### 2. Visual Representation:
```
                       1
                     /   \
                    2     3      ==> Count = 3, Sum = 1 + 2 + 3 = 6
```

#### 3. Mental Model & Approach:
* *Count Nodes:* Root node par khade ho jao. Mere left tree mein kitne nodes hain? `count(left)`. Right mein kitne hain? `count(right)`. Total count kitna hoga? `leftCount + rightCount + 1` (for current root).
* *Sum of Nodes:* Total sum = `sum(left) + sum(right) + root.value`.

---

#### 4. JavaScript Code:
```javascript
function countNodes(root) {
    if (root === null) return 0; // Base Case: empty node
    
    return countNodes(root.left) + countNodes(root.right) + 1;
}

function sumNodes(root) {
    if (root === null) return 0; // Base Case
    
    return sumNodes(root.left) + sumNodes(root.right) + root.value;
}
```
* **Complexity:** Time: **O(n)** (visiting all nodes), Space: **O(h)** for stack memory frames.

---

### PROBLEM 2 (Easy): Max Depth / Height of Binary Tree (LeetCode 104)

#### 1. Understand:
Root se lekar deepest leaf tak ka maximum depth nikalna hai.

#### 2. JavaScript Code:
```javascript
function maxDepth(root) {
    if (root === null) return 0; // Base case: null tree height is 0
    
    const leftHeight = maxDepth(root.left);  // Left child heights recursively
    const rightHeight = maxDepth(root.right); // Right child heights recursively
    
    return Math.max(leftHeight, rightHeight) + 1; // Combine: Max of left/right + 1
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)**.

---

### PROBLEM 3 (Easy): Same Tree (LeetCode 100)

#### 1. Understand:
Do binary trees `p` aur `q` diye hain. Check karna hai ki kya dono structurally identical aur identical elements ke bane hain ya nahi.

#### 2. Approach:
* Agar dono roots null hain, toh same hain (`true`).
* Agar ek root null hai aur doosra nahi, toh same nahi hain (`false`).
* Agar dono ki values different hain, toh same nahi hain (`false`).
* Dono ke left segments same hone chahiye aur right segments bhi same hone chahiye recursive checking se.

```javascript
function isSameTree(p, q) {
    if (p === null && q === null) return true;  // Base case: Both null
    if (p === null || q === null) return false; // Structurally asymmetric
    if (p.value !== q.value) return false;      // Value mismatch
    
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)**.

---

### PROBLEM 4 (Medium): Check Balanced Binary Tree (LeetCode 110)

#### 1. Understand:
Humein check karna hai ki kya tree balanced hai. Balanced tree ka matlab hai har node ke left aur right subtree ki height ka difference `<=` 1 hona chahiye.

#### 2. Bottleneck:
Agar hum naive approach se har node par jaakar height nikalenge, toh height function ka time complex operations call har index level par nested recalculate hoga, jisse total complexity **O(n^2)** par degrade ho jayegi.

#### 3. Better/Optimal (Bottom-Up / Postorder Thinking 💡):
Piche se (Bottom-Up) heights calculate karke parent node tak asani se check bubbles transfer karenge!
* Agar left subtree ya right subtree unbalanced ho jaye, toh height ke badle seedhe `-1` bubble-up propagate karo!
* Isse height calculate karte waqt hi balance integrity validation constant time **O(1)** mein single pass mein resolve ho jayegi!

---

#### 4. JavaScript Code:
```javascript
function isBalanced(root) {
    return checkHeight(root) !== -1;
}

function checkHeight(node) {
    if (node === null) return 0; // Base Case

    const leftHeight = checkHeight(node.left);
    if (leftHeight === -1) return -1; // Unbalanced left segment propagate instantly

    const rightHeight = checkHeight(node.right);
    if (rightHeight === -1) return -1; // Unbalanced right segment propagate instantly

    // If height difference violates balance condition, return -1
    if (Math.abs(leftHeight - rightHeight) > 1) {
        return -1;
    }

    // Otherwise return actual node height
    return Math.max(leftHeight, rightHeight) + 1;
}
```
* **Complexity:** Time: Strictly **O(n)** (Single Bottom-Up Pass), Space: **O(h)** auxiliary stack depth.

---

### PROBLEM 5 (Medium): Diameter of Binary Tree (LeetCode 543)

#### 1. Understand:
Binary tree ka **Diameter (Chaudai)** woh longest path distance hota hai jo kisi bhi do nodes ke beech mein exist kare. Path root se pass ho bhi sakta hai, aur nahi bhi.

```
                         Diameter Path Example:
                               1
                             /   \
                            2     3
                          /   \
                         4     5    ==> Path 4-2-5 (Length 2 edges) or 4-2-1-3 (3 edges)
```

#### 2. Optimal Approach (Bottom-Up / Postorder Thinking):
Har node par hum check karenge ki uske through pass hone wala path length kitna hai:
\\[Current Path Length = Height of Left + Height of Right\\]
Hum ek global variable `maxDiameter` maintain karenge jo har node se return hone wale cumulative path updates ko max values par capture karega!

```javascript
function diameterOfBinaryTree(root) {
    let maxDiameter = 0;

    function height(node) {
        if (node === null) return 0; // Base case

        const lh = height(node.left);  // Left height recursive fetch
        const rh = height(node.right); // Right height recursive fetch

        // Global tracker update
        maxDiameter = Math.max(maxDiameter, lh + rh);

        return Math.max(lh, rh) + 1; // Return node height to parent
    }

    height(root);
    return maxDiameter;
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)**.

---

### PROBLEM 6 (Hard): Maximum Path Sum (LeetCode 124)

#### 1. Understand:
Humein tree ke nodes ke values ka maximum sum path find out karna hai. Elements numbers negative bhi ho sakte hain!

#### 2. SDE Bottom-Up Choice 💡:
Har node par khade ho jao. Meray parent ko meray through pass hone wala max single-line path sum return karna hoga. 
* Agar left child ka path negative value laaye, toh hum use completely ignore kar denge (`Math.max(0, leftPath)`).
* Current node ke left-right arms connect karke potential complete path evaluate karo: `lh + rh + node.value`. Use global tracker mein max out save karo.

```javascript
function maxPathSum(root) {
    let globalMax = -Infinity;

    function findMaxGain(node) {
        if (node === null) return 0; // Base case

        // Get max path sum recursively, ignoring negative gains
        const leftGain = Math.max(0, findMaxGain(node.left));
        const rightGain = Math.max(0, findMaxGain(node.right));

        // Total path cost through this node as bridge
        const currentPathSum = node.value + leftGain + rightGain;

        // Update global maximum path sum found so far
        globalMax = Math.max(globalMax, currentPathSum);

        // Return the single-line maximum gain path to parent
        return node.value + Math.max(leftGain, rightGain);
    }

    findMaxGain(root);
    return globalMax;
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)**.

---

### PROBLEM 7 (Hard): Lowest Common Ancestor (LeetCode 236)

#### 1. Understand:
Do nodes `p` aur `q` diye hain. Unka **LCA (Lowest Common Ancestor)** dhoondhna hai.

```javascript
function lowestCommonAncestor(root, p, q) {
    // Base Case: null, or node matched
    if (root === null || root === p || root === q) {
        return root;
    }

    // Traverse left and right subtrees recursively
    const leftLCA = lowestCommonAncestor(root.left, p, q);
    const rightLCA = lowestCommonAncestor(root.right, p, q);

    // If both left and right return non-null, this root is the LCA!
    if (leftLCA !== null && rightLCA !== null) {
        return root;
    }

    // Otherwise return whichever subtree found p or q
    return leftLCA !== null ? leftLCA : rightLCA;
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)**.

---

### PROBLEM 8 (Hard): Binary Tree Left / Right View

#### 1. Understand:
Left/Right direction se dekhne par tree ke jo elements visible honge, unhe order mein return karna hai.

```
                         Right View Pattern:
                               1  ◄──
                             /   \
                            2     3  ◄──
                          /
                         4  ◄──
```

#### 2. Optimal Approach (The Recursion Level Counter 💡):
Hum Level tracking recursive DFS traversal use karenge!
* Hum level tracking `level` aur `result` array maintain karenge.
* Agar right view banana hai: hum traversal sequence ko **Root -> Right -> Left** order mein recursively run karenge!
* Har nayi deep depth `level === result.length` par aane wala pehla element hi rightmost visual element hoga, use direct record push kar lenge!

```javascript
function rightSideView(root) {
    const result = [];
    
    function dfs(node, level) {
        if (node === null) return;

        // First time hitting this depth level, record this node value
        if (level === result.length) {
            result.push(node.value);
        }

        // Search Right branch first for Right View! (Left first for Left View)
        dfs(node.right, level + 1);
        dfs(node.left, level + 1);
    }

    dfs(root, 0);
    return result;
}
```
* **Complexity:** Time: **O(n)**, Space: **O(h)** stack depth memory.

---

## 7. TREE TRAVERSAL CHEAT SHEET 📝

| Traversal | Sequence | Under-the-hood mechanism | Best Suited Use Cases / Applications |
| :--- | :--- | :--- | :--- |
| **Preorder** | Root → L → R | Recursion / Stack | Tree duplication, structure serialization/copying. |
| **Inorder** | L → Root → R | Leftmost Recursive Sweep | BST sorting verification, outputting elements in sorted order. |
| **Postorder**| L → R → Root | Bottom-Up validation | Height/Diameter evaluation, structural node deletions. |
| **BFS** | Level-by-level | Queue (FIFO) | Shortest path discovery in unweighted trees, level properties. |

---

## 8. SDE TRAPS & COMMON MISTAKES TO AVOID ⚠️

Bacho, trees ke questions solve karte waqt technical tests mein in classic bugs se hamesha bacho:

1. **Forgetting Null References Base Case:**
   Writing recursive code without `if (root === null) return` checks. This throws `TypeError: Cannot read properties of null` crash instantly!
2. **Confusing Depth and Height:**
   Taking bottom-up values of height and subtracting from root levels erroneously during level boundary calculations.
3. **Array Splices Stack Overflow inside loops:**
   Modifying the traversal results incorrectly with in-place array slices inside the loop recursions, collapsing complexity bounds.
4. **Incorrect Traversal Sequence Calls:**
   Swapping left-right child visit order inside standard Inorder algorithms, corrupting BST sorting alignments.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Foundational definitions of Tree hierarchies, Depth, and heights boundaries metrics.
* Binary trees structural constraints and classifications.
* Class constructors representation and manual structures assembly.
* DFS Pre/In/Post recursively and iterative stack configurations.
* BFS queue integrations to sweep wide level order sequences.

### SDE Tree Patterns Mastered:
* ** Har Node ko Ek Chhoti Problem Samjho** framework.
* **Bottom-Up Postorder validation bubble** to calculate Diameter, Balanced conditions, and Path sums.
* **Depth level coordinate count tracking** for side views lookups.

---

### SDE Practice Roadmap:
1. Complete LeetCode 104 *Maximum Depth of Binary Tree* recursively.
2. Solve LeetCode 226 *Invert Binary Tree*.
3. Implement *Same Tree* (LeetCode 100) and *Diameter of Binary Tree* Bottom-Up.

---

