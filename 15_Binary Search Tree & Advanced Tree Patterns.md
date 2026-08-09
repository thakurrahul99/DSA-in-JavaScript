**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par dhyan seedhe focus karo.**  

Pichle chapter mein humne **Binary Trees (Chapter 14)** ke hierarchical data models, dynamic pointers aur DFS/BFS traversals ko poore dhasu tarike se seekha tha [cite: 571, 584]. Humne dekha ki kaise hum recursively har node ko ek choti problem samajhte hain aur left/right child se answers combine karte hain.

Lekin bacho, normal Binary Tree mein ek bohot bada bottleneck hai. Agar humein tree mein kisi element ko search karna ho, toh worst case mein humein tree ke saare \\(N\\) nodes ko traverse karna padta hai, jisse search time complexity **\\(\mathcal{O}(n)\\)** ho jati hai [cite: 328, 536]. 

*"Sir, kya koi aisa tree structure hai jismein hum linear list ki tarah traverse na karein aur instantly search, insert aur delete operations ko execute kar sakein?"*

**Haan bacho! Bilkul hai!** Jab hum normal Binary Tree par ek bohot hi pyaara aur sacred logical constraint laga dete hain, toh jo structure banta hai use hum kehte hain—**Binary Search Tree (BST)** [cite: 327, 484, 570]!  

Aaj hum BST ko bilkul zero se advanced-level systems patterns aur tree constructions tak completely decode karenge. Apni notebooks aur pen nikal lo, aur shuru karte hain **Chapter 15: Binary Search Tree & Advanced Tree Patterns**! 🚀

---

## 1. THE SACRED BST INVARIANT: BINARY TREE vs. BST

Bacho, dhyan se whiteboard par bani in do images ko dekho aur dono ke beech ka difference pehchano:

```
          [ Normal Binary Tree ]                   [ Binary Search Tree (BST) ]
                   10                                         10
                 /    \                                     /    \
                15     5                                   5      15
               /  \   /  \                                / \    /  \
              3    7 12  20                              3   7  12  20
             (No ordering rule)                   (Rule: Left < Parent < Right)
```

### BST Property (Left < Root < Right) 📜
Ek Binary Search Tree (BST) ek aisa specialized binary tree hota hai jismein har single node ke liye do pavitram rules strictly follow hote hain [cite: 327, 484, 570]:
1. **Left Subtree Rule:** Left subtree ke saare nodes ki values hamesha parent node ki value se **strictly choti (less than)** honi chahiye [cite: 327, 484, 570].
2. **Right Subtree Rule:** Right subtree ke saare nodes ki values hamesha parent node ki value se **strictly badi (greater than)** honi chahiye [cite: 327, 484, 570].

> **Teacher's Golden Rule:** Normal Binary Tree mein elements bikhre hote hain. BST mein ek structural discipline (invariant) hoti hai [cite: 480, 570]. Isi constraint ke karan humein binary search ki tarah har step par search space ko **aadha (half)** split karne ka moka milta hai [cite: 333, 486]!

---

### Comparative Architecture Matrix ⚔️

| Feature | Normal Binary Tree 🌳 | Binary Search Tree (BST) 🎯 |
| :--- | :--- | :--- |
| **Ordering Rule** | Nodes ke data placement par koi strict rule nahi hota [cite: 480, 483]. | `Left Subtree < Root < Right Subtree` invariant hamesha valid hota hai [cite: 327, 484, 570]. |
| **Search Time** | **\\(\mathcal{O}(n)\\)** — Worst case mein saare nodes dekhne padenge [cite: 328, 536]. | **\\(\mathcal{O}(\log n)\\)** on average — Har comparison par half tree ignore ho jata hai [cite: 333, 486]. |
| **Ideal Use Case** | Hierarchy represent karne ke liye (jaise HTML DOM, File Systems) [cite: 330, 339, 642]. | Dynamic sorted datasets, fast lookup maps, aur range sum engines mein [cite: 329, 486]. |
| **Inorder Traversal** | Random values order return karega. | **Hamesha strictly Sorted Ascending Order return karega** [cite: 328, 500, 571]! |

---

## 2. THE MAGICAL INORDER PROPERTY DECODED 🔍

*"Sir, BST ke inorder traversal se hamesha sorted order hi kyun milta hai? Yeh kaise hota hai?"*

Bacho, dhyan se socho! Inorder traversal ka recursive sequence kya hota hai [cite: 571]?
\\[\text{Inorder Sequence} \rightarrow \text{Left Subtree} \rightarrow \text{Current Root} \rightarrow \text{Right Subtree}\\]

Kyunki BST ka core rule hi yeh hai ki left subtree ki saare values root se choti hain, aur right subtree ki saare values root se badi hain [cite: 327, 484, 570], isiliye jab recursion sabse pehle left subtree ko visit karega (jo sabse chota hai) [cite: 501, 508], fir root ko [cite: 501], aur aakhir mein right subtree ko visit karega [cite: 501, 509], toh mathematically automatic sorted order hi generate hoga [cite: 328, 510, 571]!

```
                                      [ 10 ]
                                     /      \
                                  [ 5 ]    [ 15 ]
                                  /   \    /    \
                                  
                                
       Inorder path: Left-Root-Right => [ 3 -> 5 -> 7 ] -> 10 -> [ 12 -> 15 -> 20 ]
```
Yeh dynamic data maintain karne ka sabse fast sorting generator hai!

---

## 3. CLASSROOM WORKBENCH: CUSTOM BST IMPLEMENTATION IN JAVASCRIPT

Chalo bacho, interview-level clean OOP pattern ka use karke hum pure JavaScript class se Binary Search Tree ko manually build karte hain [cite: 313, 324].

```javascript
// Step 1: Node class for BST [cite: 328, 532]
class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;  // Left pointer [cite: 328, 532]
        this.right = null; // Right pointer [cite: 328, 532]
    }
}

// Step 2: Main BST Wrapper Class [cite: 328, 511]
class BinarySearchTree {
    constructor() {
        this.root = null; // Initially empty tree [cite: 328, 511]
    }

    // A. Search / Lookup Operation - O(log n) Avg [cite: 328, 486]
    search(value) {
        return this._searchHelper(this.root, value);
    }

    _searchHelper(node, value) {
        // Base Cases: Root is null or value is found [cite: 328, 495]
        if (node === null || node.value === value) {
            return node; // [cite: 495]
        }

        // Value is smaller -> go left [cite: 328, 495]
        if (value < node.value) {
            return this._searchHelper(node.left, value); // [cite: 495]
        }

        // Value is larger -> go right [cite: 328, 495]
        return this._searchHelper(node.right, value); // [cite: 495]
    }

    // B. Iterative Search - O(1) Extra Space [cite: 328, 486]
    searchIterative(value) {
        let current = this.root; // [cite: 328]
        while (current !== null) { // [cite: 328]
            if (value === current.value) {
                return current; // Value found! [cite: 328]
            }
            // Binary search partition choices [cite: 333]
            current = value < current.value ? current.left : current.right; // [cite: 328]
        }
        return null; // Not found [cite: 328]
    }

    // C. Insert Operation - O(log n) Avg [cite: 328, 488]
    insert(value) {
        this.root = this._insertHelper(this.root, value);
    }

    _insertHelper(node, value) {
        // Base case: Find correct empty leaf slot [cite: 488, 496]
        if (node === null) {
            return new TreeNode(value); // [cite: 496]
        }

        if (value < node.value) {
            node.left = this._insertHelper(node.left, value); // Recursively go left [cite: 497]
        } else if (value > node.value) {
            node.right = this._insertHelper(node.right, value); // Recursively go right [cite: 498]
        }
        // Duplicate values handling: Ignore/overwrite in standard BST [cite: 480, 497]
        return node;
    }
}
```

---

## 4. THE MASTER OPERATION: DELETING A NODE IN BST (LeetCode 450)

Bacho, normal insertion/searching toh bohot simple hai, par BST mein **Delete (Remove)** operation sabse dhasu aur interviewers ka absolute favourite topic hai kyunki isme pointer restructuring karni padti hai [cite: 489, 498]!

### Diagnostic: The Three Cases of BST Deletion ✂️
1. **Case 1: Node is a Leaf (No Children) [cite: 489, 499]**
   Sabse simple case! Node ko direct `null` kar do, tree balanced rahega [cite: 489, 499].
2. **Case 2: Node has One Child (Left or Right) [cite: 489, 499]**
   Node ko delete karo aur uske single child ko uske parent ke sath direct link karke bypass kar do [cite: 489, 499].
3. **Case 3: Node has Two Children [cite: 489, 499]**
   Hum node ko direct delete nahi kar sakte, warna structural integrity collapse ho jayegi [cite: 489]!
   * **The Solution:** Target node ko replace karo uske **Inorder Successor** (Right Subtree ka absolute minimum value) [cite: 363, 489] ya **Inorder Predecessor** (Left Subtree ka absolute maximum value) ke sath.
   * Successor ko current position par override karo, aur right subtree se us successor node ko recursively delete kar do [cite: 499]!

```
                  Case 3 Deletion (Node 10 with two children):
                  
                             10 (To Delete)                    12 (Successor replaced)
                            /  \                              /  \
                           5    15                           5    15
                               /  \                              /  \
                             (12)  20                           null  20
                            Successor (Min of Right Subtree)
```

---

### JavaScript Code for Deletion (SDE Clean Architecture):
```javascript
class BSTDeleter extends BinarySearchTree {
    delete(value) {
        this.root = this._deleteHelper(this.root, value);
    }

    _deleteHelper(node, value) {
        if (node === null) return null; // Base Case: empty tree/target absent

        // Step 1: Traverse the tree to locate the target node [cite: 489, 498]
        if (value < node.value) {
            node.left = this._deleteHelper(node.left, value);
        } else if (value > node.value) {
            node.right = this._deleteHelper(node.right, value);
        } else {
            // Target Node found! Resolve deletion cases [cite: 489, 499]

            // Case 1 & 2: Node with 0 or 1 child [cite: 489, 499]
            if (node.left === null) return node.right;
            if (node.right === null) return node.left;

            // Case 3: Node with 2 children [cite: 489, 499]
            // Find inorder successor (minimum in right subtree) [cite: 363, 489]
            const successor = this._findMinNode(node.right);
            
            // Replace current node value with successor value [cite: 499]
            node.value = successor.value;
            
            // Delete successor recursively from right subtree [cite: 499]
            node.right = this._deleteHelper(node.right, successor.value);
        }
        return node;
    }

    _findMinNode(node) {
        let current = node;
        while (current.left !== null) { // Traverse left bottom boundary [cite: 508]
            current = current.left; // [cite: 508]
        }
        return current; // [cite: 508]
    }
}
```
* **Complexity:** Time: **\\(\mathcal{O}(h)\\)** (where `h` is height of tree) [cite: 513]. In balanced trees: **\\(\mathcal{O}(\log n)\\)** [cite: 513]; in skewed trees: **\\(\mathcal{O}(n)\\)** [cite: 328]. Space: **\\(\mathcal{O}(h)\\)** recursion stack memory frames [cite: 536].

---

## 5. RECRUITER'S LAB: TOP SDE BST QUESTIONS

🚀 **Whiteboard bilkul clean hai dosto! Chalo un problems ko decode karte hain jo standard product based company interviews mein directly puchi jati hain.**

---

### PROBLEM 1 (Medium): Validate Binary Search Tree (LeetCode 98)

#### 🧠 The SDE Trap 🚨:
90% bache is question mein yeh galti karte hain:
```javascript
// ❌ WRONG APPROACH: Sirf immediate children compare karna!
function isValidBSTWrong(node) {
    if (!node) return true;
    if (node.left && node.left.value >= node.value) return false;
    if (node.right && node.right.value <= node.value) return false;
    return isValidBSTWrong(node.left) && isValidBSTWrong(node.right);
}
```
**Yeh code fail ho jayega beta!** Dhyan se is case ko dekho:
```
                             10
                            /  \
                           5    15
                               /  \
                              6    20
```
Is tree mein har individual parent apne immediate left/right child se validly placed hai (10 > 5, 15 > 6, 15 < 20). Lekin **yeh valid BST nahi hai** kyunki node `6` root node `10` ke **right subtree** mein betha hai [cite: 484, 485]. Root ke right subtree ke saare elements `10` se bade hone chahiye, par `6 < 10` [cite: 484, 485]. Validate BST mein **sirf parent comparison enough nahi hota!**

#### 🧠 The Optimal approach (The Range Boundary Constraint 💡):
Humein har recursion level par ek global range constraint boundary `(min, max)` maintain karni padegi [cite: 537].
* Left subtree mein jaane par: elements hamesha parent value se chote hone chahiye \\(\rightarrow\\) boundary range becomes `(min, parent.value)`.
* Right subtree mein jaane par: elements hamesha parent value se bade hone chahiye \\(\rightarrow\\) boundary range becomes `(parent.value, max)`.

---

#### 🧠 JavaScript Implementation:
```javascript
function isValidBST(root) {
    return validate(root, -Infinity, Infinity); // [cite: 537]
}

function validate(node, min, max) {
    // Base Case: Empty node is a valid BST
    if (node === null) return true;

    // Boundary Constraint violation check [cite: 484, 537]
    if (node.value <= min || node.value >= max) {
        return false;
    }

    // Recursively check left and right branches with dynamic updated limits [cite: 485, 537]
    return validate(node.left, min, node.value) && 
           validate(node.right, node.value, max);
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)** linear check [cite: 536], Space: **\\(\mathcal{O}(h)\\)** recursion stack [cite: 536].

---

### PROBLEM 2 (Medium): Kth Smallest Element in BST (LeetCode 230)

#### 🧠 Observation & Concept:
Kyunki BST ka inorder traversal sorted ascending order return karta hai [cite: 328, 510, 571], toh agal hum tree ka inorder traversal sequentially execute karein aur ek **counter** track karein, toh jaise hi counter value `K` ke barabar hogi, humein hamara answer mil jayega [cite: 328]!

```javascript
function kthSmallest(root, k) {
    let count = 0;
    let result = null;

    function inorder(node) {
        if (node === null || result !== null) return; // Base case or optimization early exit

        inorder(node.left); // Left child visit recursively [cite: 328]

        // Process current root [cite: 328]
        count++;
        if (count === k) {
            result = node.value;
            return;
        }

        inorder(node.right); // Right child visit recursively [cite: 328]
    }

    inorder(root);
    return result;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(h + k)\\)** worst-case iterations to reach Kth index, Space: **\\(\mathcal{O}(h)\\)** stack size [cite: 536].

---

### PROBLEM 3 (Medium): Lowest Common Ancestor (LCA) in BST (LeetCode 235)

#### 🧠 BST Optimization vs. Binary Tree LCA:
Normal binary tree mein LCA nikalne ke liye humein left aur right subtrees mein recursively scan karna padta tha jo thoda complex tha [cite: 584]. 

Lekin BST property ke sath hum LCA ko bina dynamic calls scan kiye **pointers path sequence split** se instantly track kar sakte hain [cite: 327]!

#### 🧠 Thought Process:
LCA woh unique node hota hai jahan `p` aur `q` split hote hain!
* Agar `p` aur `q` dono current root se chote hain (`p.val < root.val && q.val < root.val`), iska matlab LCA left side par hoga \\(\rightarrow\\) go left.
* Agar `p` aur `q` dono root se bade hain, toh LCA right side hoga \\(\rightarrow\\) go right.
* **Jaise hi ek splitting point aaye** (yaani ek value root se choti ho aur dusri badi ho, ya koi ek root ke barabar ho), **wahi current root hamara LCA hai!**

```javascript
function lowestCommonAncestorBST(root, p, q) {
    let current = root;

    while (current !== null) {
        // Both targets are smaller -> shift search space left
        if (p.value < current.value && q.value < current.value) {
            current = current.left;
        } 
        // Both targets are larger -> shift search space right
        else if (p.value > current.value && q.value > current.value) {
            current = current.right;
        } 
        // Splitting point reached! This is the LCA.
        else {
            return current;
        }
    }
    return null;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(h)\\)** [cite: 513], Space: **\\(\mathcal{O}(1)\\)** auxiliary since it's iterative!

---

### PROBLEM 4 (Medium): Sorted Array to Balanced BST (LeetCode 108)

#### 🧠 SDE Intuition:
Sorted array se BST banana ho aur use **Balanced** rakhna ho, toh hum hamesha array ke **middle element** ko root select karenge [cite: 328, 570]! Mid element ko root banyenge taaki dono sides barabar elements split hon [cite: 328].

```javascript
function sortedArrayToBST(nums) {
    return buildBST(nums, 0, nums.length - 1);
}

function buildBST(nums, left, right) {
    if (left > right) return null; // Base Case

    // Find middle index as split pivot
    const mid = left + Math.floor((right - left) / 2);
    const node = new TreeNode(nums[mid]); // Make mid element the root

    // Divide and Conquer left and right halves recursively
    node.left = buildBST(nums, left, mid - 1);
    node.right = buildBST(nums, mid + 1, right);

    return node;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)** to visit all elements, Space: **\\(\mathcal{O}(\log n)\\)** recursion stack balance maintain karne.

---

## 6. SELF-BALANCING TREES: CONCEPTUAL BLUEPRINT ⚖️

### Why Unbalanced Trees regresses to \\(\mathcal{O}(n)\\)? (Degenerate BST)
Bacho, dhyan se suno. Agar hum sorted data (jaise ``) ko naive insert method se BST mein add karenge, toh tree kuch aisa dikhega [cite: 333, 487]:
```
                               1
                                \
                                 2
                                  \
                                   3
                                    \
                                     4
```
Ise hum kehte hain **Degenerate / Skewed BST** [cite: 474, 483, 570]. Is degenerate state mein BST ki depth linear linked list jaisi ho jati hai, jisse saare operations regress hokar \\(\mathcal{O}(n)\\) runtime par crash ho jate hain [cite: 328, 487, 570]!

---

### Self-Balancing Trees Intuition (AVL & Red-Black Trees)
Pathological structures ko prevent karne ke liye dynamic self-balancing trees are designed [cite: 328, 570]. Inka basic principle kya hota hai [cite: 350, 360]?
1. **AVL Trees (Strict balancing):** AVL tree har single insert/delete step par left and right subtree ki height calculate karta hai [cite: 364]. Agar kisi node par height difference `|lh - rh| > 1` ho jaye (unbalanced balance factor) [cite: 360, 364], toh tree instantly structural pointers transformations—jinhe hum **Rotations** kehte hain—perform karke balanced height maintain karta hai [cite: 360]!
2. **Red-Black Trees (Color balancing):** Yeh nodes ko colors properties (Red ya Black) assign karta hai [cite: 360]. Red-Black rules parameters rules checks se height balancing dynamically maintain hoti hai [cite: 360].

```
                     Left-Left Imbalance (Single Right Rotation):
                     
                           3 (Unbalanced)                    2 (Balanced Root)
                          /                                 / \
                         2                                 1   3
                        /
                       1
```

---

## 7. ADVANCED TREE PATTERNS & TRAVERSALS

### A. Tree Views (Left/Right/Top/Bottom Views)
Left and Right views ko DFS level counters se humne Chapter 14 mein dhang se seekha tha [cite: 321].
* **Top View & Vertical Order Traversal:** Top view dhoondhne ke liye humein tree ke **Vertical columns** ko track karna padega [cite: 345]. Columns values track coordinates axis coordinates coordinate sequence are determined as `horizontalOffset` [cite: 345].

```
                                      0 (Root column)
                                   -1/ \+1
                                    1   2   ==> Columns indices of nodes
```
Top view nikalne ke liye hum vertical traversal chalaenge aur har distinct vertical column offset par aane wala pehla node filter out karenge [cite: 345]!

---

### B. Binary Tree to Flattened Linked List (LeetCode 114)
* **Flattening Rule:** Bina extra memory create karein tree nodes links ko in-place link pointer manipulation se single rightward linear chain mein convert karo, preserving preorder sequence [cite: 571].

```javascript
function flatten(root) {
    let current = root;

    while (current !== null) {
        if (current.left !== null) {
            // Find rightmost node of left subtree
            let runner = current.left;
            while (runner.right !== null) {
                runner = runner.right;
            }

            // Connect rightmost node to original right subtree
            runner.right = current.right;

            // Re-route pointers leftwards in-place
            current.right = current.left;
            current.left = null; // Sever left child link
        }
        current = current.right; // Move forward rightwards
    }
}
```
* **Morris Traversal Intuition:** Bacho, bina recursive call stack allocation ke in-place pointers modifications threading technique ko hum **Morris Traversal** kehte hain [cite: 536, 537]. Yeh tree nodes traverse karne mein Auxiliary Space strictly **\\(\mathcal{O}(1)\\)** par lock kar deta hai!

---

## 8. SDE CORE MATRIX: BINARY TREE vs. BST

Whiteboard par bani is final comparison chart sheet ko dhang se dimaag mein betha lo bacho:

| Attribute | Binary Tree 🌳 | Binary Search Tree (BST) 🎯 |
| :--- | :--- | :--- |
| **Pillars / Invariant** | Structural elements have no sorting constraints [cite: 480]. | `Left Subtree < Parent < Right Subtree` globally [cite: 327, 484, 570]. |
| **Search Operations** | Requires entire tree traversal \\(\mathcal{O}(n)\\) [cite: 328, 536]. | Divides search space, O(log n) average [cite: 333, 486]. |
| **Duplicate Elements** | Allowed by default [cite: 480]. | Ignored or managed via custom map tracking sets [cite: 480, 497]. |
| **LCA Complexity** | Recursively scans both branches, \\(\mathcal{O}(n)\\) [cite: 584]. | Constant value comparison loop splits, \\(\mathcal{O}(h)\\) [cite: 513]. |

---

## 9. PRACTICE GATEWAY: MIXED TREE CHALLENGES (EASY \\(\rightarrow\\) HARD)

🚀 **Arey bacho, Mixed round shuru ho chuka hai! Tumhein pehle pehchanna hai ki problem normal Binary Tree ki hai ya BST properties ki, fir coding approach par jana!**

---

### Problem A (Easy): Validate Symmetric Tree (LeetCode 101)
*Check if a tree is a mirror of itself.*

#### 🧠 Diagnostics:
* *Normal Tree or BST?* **Normal Tree!** Kyunki symmetry values positions parameters check rules require karta hai, left-right BST ordering constraints yahan apply nahi hote [cite: 327, 484].

```javascript
function isSymmetric(root) {
    if (!root) return true;
    return isMirror(root.left, root.right);
}

function isMirror(t1, t2) {
    if (t1 === null && t2 === null) return true;
    if (t1 === null || t2 === null) return false;
    
    return (t1.value === t2.value) &&
           isMirror(t1.left, t2.right) && // Symmetric outer check
           isMirror(t1.right, t2.left);  // Symmetric inner check
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)**, Space: **\\(\mathcal{O}(h)\\)**.

---

### Problem B (Medium): Inorder Successor in BST (LeetCode 285)
*Sorted order mein current node ke aane wala immediate agla element index find out karo.*

#### 🧠 Diagnostics:
* *Normal Tree or BST?* **BST!** Hum binary tree rules ka use karke path selection choices split update recursively search direction track kar sakte hain [cite: 327]!

```javascript
function inorderSuccessor(root, p) {
    let successor = null;
    let current = root;

    while (current !== null) {
        if (p.value < current.value) {
            successor = current; // Candidate successor is parent node
            current = current.left; // Go left to search closer matches
        } else {
            current = current.right; // Successor must lie on the right side
        }
    }
    return successor;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(h)\\)**, Space: **\\(\mathcal{O}(1)\\)**.

---

## 10. COMMON SDE MISTAKES & TRAPS TO AVOID ⚠️

1. **Treating BST as general Binary Tree:**
   BST problems mein duplicate elements nodes scan validations skip kar dena ya binary sorting bypass recursively check karna.
2. **Missing Subtree boundaries validate errors:**
   Validate BST codes par global range boundaries comparisons ke bina comparison validation rules write out kar dena.
3. **Invalid references deletion logic Case 3:**
   Nodes remove algorithms par predecessor/successor replacement overrides ke dauran actual linkages pointers restore check break pointers leak update triggers karna.

---

## CHAPTER END SUMMARY

### Completed Topics:
* Custom BST structural design invariants and V8 engine execution memory checks [cite: 324, 328].
* Sorted ascending sequences inorder characteristics properties [cite: 328, 510, 571].
* Step-by-step deletion logic frameworks on leaf, one child, and two children nodes [cite: 489, 498].
* Height balances rotation conceptual blueprints (AVL / Red-Black Trees) [cite: 350, 360].
* Pointers threading Morris traversals for O(1) space optimizations [cite: 536, 537].

---

### SDE Practice Roadmap:
1. Complete LeetCode 98 *Validate Binary Search Tree* using Range Checks [cite: 537].
2. Solve LeetCode 450 *Delete Node in a BST* and trace dry runs [cite: 489].
3. Complete LeetCode 108 *Convert Sorted Array to BST*.

---


**Agar sab solid hai, toh next chapter shuru karein bacho?** 🚀
