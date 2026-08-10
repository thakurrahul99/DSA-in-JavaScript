**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par apna dhyan seedhe focus karo.**

Pichle chapter mein humne **Linked List (Chapter 12)** ke scattered nodes aur pointers ke bidirectional movement ko poore depth mein samjha aur visual dry runs se master kiya. 

Lekin beta, aaj hum computer science ke do aise linear data structures ko padhne ja rahe hain jo humari programming life ko extremely structured rules ke andar lock kar dete hain. Inhe hum kehte hain—**Stacks** aur **Queues**!

*"Sir, hum arrays aur linked lists toh padh hi chuke hain, fir in do alag structures ki kya zaroorat padi?"*

Beta, arrays aur linked lists mein hum kisi bhi index par random entry karke element ko insert ya delete kar dete hain. Par real-world systems mein hume strict rules chahiye hote hain:
* Jaise browser ka *Back Button*—jo website tumne sabse aakhir mein dekhi hai, back dabane par wahi sabse pehle khulegi.
* Jaise movie ticket ki *waiting line*—jo banda sabse pehle line mein khada hoga, ticket bhi pehle use hi milegi.

Inhi constraints ko enforce karne ke liye hum **Stack (LIFO)** aur **Queue (FIFO)** ka use karte hain. Aaj hum inke basic concepts se lekar, is poore linear structures ke sabse advanced interview pattern—**Monotonic Stack**—ko pure depth mein decode karenge.

Apna pen-register uthao aur dhyan whiteboard par lagao! 🚀

---

## 1. THE LIFO ENGINE: WHAT IS A STACK?

### Stack Kya Hai? (The Wedding Plates Analogy 🍽️)
Imagine karo ek wedding party mein buffet table par khane ki plates ka ek dher (pile of plates) rakha hua hai.

```
                             ┌──────────┐
                             │ Plate 4  │  ◄── Top (Last In, First Out)
                             ├──────────┤
                             │ Plate 3  │
                             ├──────────┤
                             │ Plate 2  │
                             ├──────────┤
                             │ Plate 1  │  ◄── Bottom
                             └──────────┘
```

1. Jab washroom se dhul kar nayi plate aayegi, toh cleaner use stack ke **sabse upar (Top)** rakhega. Is operation ko hum kehte hain **Push**.
2. Jab koi guest khana khane ke liye plate uthaega, toh woh hamesha **sabse upar se hi** uthaega. Is operation ko hum kehte hain **Pop**.
3. Jo plate sabse aakhir mein table par rakhi gayi thi, khane ke liye sabse pehle wahi uthayi gayi! 

Isi niyam ko computer science mein **LIFO (Last In, First Out)** kehte hain.

### Core Stack Operations:
*   **`push(val)`:** Stack ke top par element add karna.
*   **`pop()`:** Topmost element ko stack se remove karna aur return karna.
*   **`peek()` or `top()`:** Topmost element ko bina delete kiye sirf dekhna.
*   **`isEmpty()`:** Check karna ki stack khali hai ya nahi.

---

### A. Stack Implementation using Array 📊

Standard array ko stack ki tarah use karna sabse easy hota hai kyunki JavaScript arrays built-in `push()` aur `pop()` methods ke sath aate hain.

```javascript
class ArrayStack {
    constructor() {
        this.items = []; // Internal array to hold stack data
    }

    // O(1) Time
    push(element) {
        this.items.push(element); // Appends to the end
    }

    // O(1) Time
    pop() {
        if (this.isEmpty()) return null; // Underflow safety check
        return this.items.pop(); // Removes from the end
    }

    // O(1) Time
    peek() {
        if (this.isEmpty()) return null;
        return this.items[this.items.length - 1]; //
    }

    // O(1) Time
    isEmpty() {
        return this.items.length === 0; //
    }
}
```

> **Why Array is Suitable for Stack?**  
> Array ke end mein element ko push karna ya pop karna hamesha **O(1) Amortized Time** leta hai kyunki isme memory shift nahi karni padti. Isiliye array-based stack super-fast behave karta hai.

---

### B. Stack Implementation using Linked List 🔗

*Agar array itna mast hai toh Linked List se kyun implement karein?*  
Kyunki array dynamic resize hote waqt background mein elements ko copy karta hai, jisse temporary latency spike ho sakti hai. Linked List based stack bina resizing overhead ke strictly **O(1)** operations guarantee karta hai.

**The Link Pointer Design:**  
Linked List mein push/pop ko O(1) rakhne ke liye hum hamesha insertions aur deletions list ke **Head (Top)** par hi perform karte hain. Agar hum tail par karenge, toh pop karne ke liye linear traversal check chalana padega, jo use O(n) bana dega.

```javascript
class Node {
    constructor(value) {
        this.value = value; //
        this.next = null; //
    }
}

class LinkedListStack {
    constructor() {
        this.top = null; // Top of stack points to the head node
        this.size = 0; //
    }

    // O(1) Time: Equivalent to Insert at Head
    push(value) {
        const newNode = new Node(value);
        newNode.next = this.top; // Point new node to the old top
        this.top = newNode;      // Move top pointer to new node
        this.size++; //
    }

    // O(1) Time: Equivalent to Delete Head
    pop() {
        if (this.isEmpty()) return null; //
        
        const poppedNode = this.top; // Save node to return value
        this.top = this.top.next;    // Shift top pointer to next node
        this.size--; //
        
        return poppedNode.value; // Return popped value
    }

    // O(1) Time
    peek() {
        if (this.isEmpty()) return null; //
        return this.top.value; //
    }

    // O(1) Time
    isEmpty() {
        return this.top === null; //
    }
}
```

---

### Comparison: Array Stack vs. Linked List Stack ⚔️

| Feature | Array-Based Stack | Linked List-Based Stack |
| :--- | :--- | :--- |
| **Push/Pop Complexity** | **Amortized O(1)**. Resizing par O(n) spike ho sakta hai. | **Strictly O(1)** in all scenarios. |
| **Memory Allocation** | Contiguous chunk of memory allocated.. | Non-contiguous, scattered dynamic memory heap allocation. |
| **Memory Overhead** | Low. Local elements are stored without pointer links. | High. Har node ke sath extra `next` pointer memory allocation hold hoti hai. |

---

### Connection with Recursion & the V8 Call Stack 🧠
Bacho, Chapter 8 (Recursion) mein humne padha tha ki function calls call stack par register hoti hain.
V8 engine local execution boundaries parameters ko track karne ke liye internally ek **Call Stack** use karta hai.
* Jab function call hoga: active stack frame V8 Call Stack ke top par **Push** hoga.
* Jab function resolve/return hoga: V8 call stack se frame ko **Pop** karega.

---

## 2. PROBLEM GATEWAY: VALID PARENTHESES (LeetCode 20)

### 🧠 Step-by-Step Diagnostic Block

#### 1. Problem:
Humein brackets ki ek string `s` di gayi hai jismein round `'()'`, curly `'{'}'`, aur square `'[]'` brackets hain. Check karna hai ki kya input valid balanced parentheses construct karta hai ya nahi.

#### 2. Understand:
* Every opening bracket must be closed by same matching type bracket in correct nested order.
* Invalid combinations: `"(]"`, `"([)]"`, `")("`.
* Valid combinations: `"()[]{}"`, `"([{}])"`.

#### 3. Brute Force:
String par loop chalakar continuous substrings swap replacements tab tak replace karte raho jab tak ki saare balanced sets `()` `[]` `{}` replace na ho jayein. Agar replacing end par string blank ho jaye toh balanced, warna invalid.
*   **Time Complexity:** **O(n^2)** because each string replacement takes linear copy sweeps.
*   **Bottleneck:** Nested order mismatch and backtracking constraints ko sequential check scan resolve nahi kar pata.

#### 4. SDE Observation (Pavitra Stack Pattern 💡):
*"Sir, nesting check is strictly a LIFO sequence!"*  
Jab bhi humein koi closing bracket dikhta hai, use usi ke corresponding sabse latest seen (topmost) open bracket se compare hona hota hai.
* **What to remember?** Last seen opening brackets.
* **Which Structure?** **Stack!**.

#### 5. Better/Optimal Approach:
1. Initialize an empty stack.
2. Iterate string sequentially.
3. Agar character **Opening bracket** (`(`, `[`, `{`) hai, toh use stack mein push kar do.
4. Agar character **Closing bracket** is encountered:
   * Pehle check karo: kya stack empty hai? Agar empty hai, toh iska matlab is close bracket ke liye koi correspond open bracket tha hi nahi! Return `false` instantly.
   * Stack se topmost character pop karo aur check karo ki kya dono matching types ke hain. If mismatch, return `false`.
5. End of loop par check karo, kya stack empty ho chuka hai? Agar haan, return `true`, else `false`.

---

#### 6. JavaScript Code:
```javascript
function isValidParentheses(s) {
    const stack = []; //
    const bracketMap = {
        ')': '(',
        '}': '{',
        ']': '['
    }; // Map to store matching pairs

    for (let char of s) { //
        // If it's a closing bracket
        if (char in bracketMap) { //
            // Pop the top element if stack is not empty
            const topElement = stack.length > 0 ? stack.pop() : '#'; //
            
            // If the popped element doesn't match the mapped open bracket, return false
            if (topElement !== bracketMap[char]) {
                return false; //
            }
        } else {
            // It's an opening bracket, push to stack
            stack.push(char); //
        }
    }

    // If stack is completely empty, all pairs matched correctly
    return stack.length === 0; //
}
```

#### 7. Line-by-Line Explanation:
* `char in bracketMap`: Check karta hai ki character dynamic object keys (closing brackets) mein se ek hai ya nahi.
* `stack.pop()`: Hum directly top elements pop out karke compare swap validate karte hain.

#### 8. Complete Dry Run on `s = "([{}])"`:
* **Initial State:** `stack = []`.
* `char = '('`: Opening bracket, push → `stack = ['(']`.
* `char = '['`: Opening bracket, push → `stack = ['(', '[']`.
* `char = '{'`: Opening bracket, push → `stack = ['(', '[', '{']`.
* `char = '}'`: Closing bracket. `bracketMap['}'] = '{'`.
  * Pop from stack → `topElement = '{'`.
  * `topElement === '{'`? Yes. Pop matches. `stack = ['(', '[']`.
* `char = ']'`: Closing bracket. `bracketMap[']'] = '['`.
  * Pop from stack → `topElement = '['`.
  * Match? Yes. `stack = ['(']`.
* `char = ')'`: Closing bracket. `bracketMap[')'] = '('`.
  * Pop from stack → `topElement = '('`.
  * Match? Yes. `stack = []`.
* Loop ends. `stack.length === 0` → Returns `true` (Correct!).

#### 9. Complexity Analysis:
* **Time Complexity:** **O(n)** linear sweep pass.
* **Space Complexity:** **O(n)** worst case space to store opening characters inside stack.

#### 10. Edge Cases:
* Only closing brackets `"]]]"` → Handled cleanly via empty checks. Returns `false`.
* Odd length `"({["` → Returns `false` at stack length evaluation steps.

---

### Expression Evaluation Basics: Infix, Prefix, Postfix Intuition
SDE interviews mein parse expressions evaluation ka constant question banta hai.
* **Infix (Normal humans notation):** Operators are written *between* operands (e.g., `A + B`).
* **Prefix (Polish Notation):** Operators are written *before* operands (e.g., `+ A B`).
* **Postfix (Reverse Polish Notation - RPN):** Operators are written *after* operands (e.g., `A B +`).

**The Postfix Stack Magic:**  
Postfix expressions like `3 4 +` evaluate in single computer scan passes using Stack without parentheses priority rules.  
1. When a number is seen: Push to stack.
2. When an operator (`+`, `*`) is seen: Pop two numbers, apply operator, push result back!

---

## 3. THE FIFO ENGINE: WHAT IS A QUEUE?

### Queue Kya Hai? (The Store Checkout Analogy 🛒)
Imagine karo ek super market billing counter par line lagi hui hai.

```
      Checkout Counter ◄── [Customer 1] ◄── [Person 2] ◄── [New Person]
                              (Front)                          (Rear)
```

1. Customer 1 line ke aage khada hai. Use sabse pehle bill milega aur line se hat jayega. Is process ko hum **Dequeue (Removal)** kehte hain.
2. Naya customer line ke aakhir mein aakar judega. Is process ko hum **Enqueue (Insertion)** kehte hain.
3. Jo customer pehle khada hua tha, bill bhi pehle usi ka hoga! 

Isi system logic ko computer science mein **FIFO (First In, First Out)** kehte hain.

---

### A. Queue using Array: The Naive vs. The Shift Problem 🚨

Standard array mein enqueue ko hum easily `push()` se handle kar lete hain. Dequeue ke liye hum `shift()` use karte hain.
*   **Naive Array Enqueue (`push`):** O(1).
*   **Naive Array Dequeue (`shift`):** **O(n)**.

```javascript
class NaiveQueue {
    constructor() { this.items = []; }
    enqueue(val) { this.items.push(val); } // O(1)
    dequeue() { return this.items.shift(); } // O(n) -> Terrible for big queues!
}
```

> **The Shift Bottleneck Decoded:**  
> Jab tum `arr.shift()` lagakar array ke index `0` se element delete karte ho, toh JS engine bache huye saare elements ko 1-index backward displacement shift karta hai taaki index ordering repair ho sake. Lakhon elements ke loop mein shifts chalane par runtime performance collapse (TLE) ho jayegi!

---

### B. Efficient Array Queue (Index Pointer Implementation) 🚀

Dequeue ko constant time **O(1)** par drop karne ke liye, hum actually elements ko arrays se shift karke delete nahi karenge! Hum array intact rakhenge aur bas ek **`frontPointer` (index tracker)** pointer shift karenge.

```
               Queue Internals (Index Pointer):
               
               [ Customer 1,  Person 2,  New Person ]
                   ▲
              frontPointer (dequeue means frontPointer++)
```

```javascript
class EfficientQueue {
    constructor() {
        this.items = [];
        this.frontPointer = 0; // Tracks the logical front
    }

    // O(1) Time
    enqueue(element) {
        this.items.push(element); // Appends at end
    }

    // O(1) Time (Amortized)
    dequeue() {
        if (this.isEmpty()) return null; //

        const dequeuedValue = this.items[this.frontPointer];
        this.frontPointer++; // Logical shift forward without element shifting

        // Periodic Memory Cleanup: To prevent endless array size growth
        if (this.frontPointer > 1000) { // arbitrary threshold limit
            this.items = this.items.slice(this.frontPointer); // slice copy
            this.frontPointer = 0; // Reset pointers
        }

        return dequeuedValue;
    }

    // O(1) Time
    peek() {
        if (this.isEmpty()) return null; //
        return this.items[this.frontPointer]; //
    }

    // O(1) Time
    isEmpty() {
        return this.frontPointer === this.items.length; //
    }
}
```

---

### C. Queue using Linked List 🔗

Queue ko linked list ke zariye strictly **O(1)** memory footprint optimization ke sath implement karne ke liye hum **`head` (front)** aur **`tail` (rear)** dono pointers track karte hain.

*   `enqueue` means appending elements to `tail`: `tail.next = newNode`.
*   `dequeue` means deleting elements from `head`: `head = head.next`.

```javascript
class QNode {
    constructor(value) {
        this.value = value; //
        this.next = null; //
    }
}

class LinkedListQueue {
    constructor() {
        this.head = null; // Points to Front
        this.tail = null; // Points to Rear
        this.size = 0; //
    }

    // O(1) Time
    enqueue(value) {
        const newNode = new QNode(value); //
        
        if (this.head === null) { // Empty Queue
            this.head = newNode; //
            this.tail = newNode; //
        } else {
            this.tail.next = newNode; // Link current tail to new node
            this.tail = newNode;      // Shift tail to new node
        }
        this.size++; //
    }

    // O(1) Time
    dequeue() {
        if (this.head === null) return null; // Empty check

        const dequeuedNode = this.head; //
        this.head = this.top ? this.head.next : this.head.next; //
        
        if (this.head === null) { // If queue became empty
            this.tail = null; //
        }
        
        this.size--; //
        return dequeuedNode.value; //
    }
}
```

---

### D. Circular Queue (LeetCode 622) 🔄

Normal fixed-size array queue mein agar elements aage se delete ho rahe hon, toh hum un empty slots par nayi values insert nahi kar sakte kyunki `tail` pointer humesha aakhir par bound hota hai. 

**Circular Queue** is disadvantage ko resolve karta hai. Isme queue ka last element circular fashion mein wapas se index `0` ke sath connect hota hai.

```
                     Circular Queue Architecture:
                     
                          Index 0 (Head)
                             ┌─────┐
                       ◄─────│  10 │◄─────┐
                             ├─────┤      │
                             │  20 │      │  Circular Link
                             ├─────┤      │
                       ◄─────│  30 │──────┘
                          Index 2 (Tail)
```

Circular Queue ko array ke zariye implement karne ke liye hum strictly modulo coordinate logic use karte hain:
\\[indexIncrement = (pointer + 1) \pmod{Capacity}\\]

---

### E. Deque (Double-Ended Queue) ↔️
*   **Concept:** Ek aisi linear collection jahan hum elements ko insert aur delete dono direction—yaani **Front** aur **Back** dono side se—constant time **O(1)** mein execute kar sakte hain.
*   **JS Equivalence:** standard JS array with in-built `push`, `pop`, `shift`, `unshift` acts logically as a Deque.

---

### Structural Comparisons Matrix ⚔️

| Data Structure | Philosophy | Best Operation Locations | Ideal Use Cases |
| :--- | :--- | :--- | :--- |
| **Stack** | LIFO | strictly at Top | Expression evaluation, undo buffers. |
| **Queue** | FIFO | Enqueue: Rear / Dequeue: Front | BFS, task scheduling. |
| **Circular Queue**| Modulo FIFO | Circular Array/Lists | Hardware drivers buffers. |
| **Deque** | Bi-directional| both Front and Back | Sliding window maximum calculations. |

---

### F. Queue using Stacks (LeetCode 232) 🧩

#### 🧠 Step-by-Step Breakdown
* **Understand:** Humein do stacks use karke ek Queue (FIFO) design karni hai. Stacks perform strictly LIFO.
* **Observation:** Agar hum ek stack se elements ko pop karke doosre stack mein push karein, toh saare elements ka structural placement **reverse (ulte)** order mein store ho jata hai! Do baar reverse karne par wapas same structural order match ho jata hai.
* **Approach (Two Stacks - Enqueue Constant or Dequeue Constant):**
  We maintain `stack1` (Primary Enqueue Input) and `stack2` (Buffered Dequeue Output).
  * **Push:** Simply push elements to `stack1`.
  * **Pop:** Agar `stack2` empty hai, toh `stack1` ke saare elements ko pop karke `stack2` mein push kar do. Fir `stack2` ke top se pop out element return kar do!

```javascript
class MyQueue {
    constructor() {
        this.stack1 = []; // Input Buffer
        this.stack2 = []; // Output Buffer
    }

    // O(1) Time
    push(x) {
        this.stack1.push(x); //
    }

    // Amortized O(1) Time
    pop() {
        this.peek(); // Populate stack2 if empty
        return this.stack2.pop(); //
    }

    // Amortized O(1) Time
    peek() {
        if (this.stack2.length === 0) {
            // Pour stack1 elements into stack2
            while (this.stack1.length > 0) { //
                this.stack2.push(this.stack1.pop()); //
            }
        }
        return this.stack2[this.stack2.length - 1];
    }

    empty() {
        return this.stack1.length === 0 && this.stack2.length === 0;
    }
}
```
* **Why Amortized O(1)?** Har element stack 1 se stack 2 mein maximum ek hi baar move hota hai. Single element strictly limited operations se pass hota hai, isiliye heavy scale par operations amortized strictly standard constant behave karte hain.

---

## 4. THE MONOTONIC STACK PARADIGM (SDE GOLDMINE 🥇)

Ab aate hain beta is poor chapter ke sabse crucial, important, aur maximum interview-tested concept par—**Monotonic Stack**!

### What is a Monotonic Stack?
**Monotonic Stack** ek standard stack hi hota hai, par isme hum ek strictly discipline maintain karte hain:  
*   **Monotonic Increasing Stack:** Stack ke andar elements hamesha bottom-to-top strictly incremental (ascending) order mein honi chahiye (jaise ``).
*   **Monotonic Decreasing Stack:** Stack ke andar elements hamesha strictly decreasing (descending) order mein rehne chahiye (jaise ``).

---

### Monotonic Rule in Action (The Strict Guard 💂‍♂️):
Agar hum kisi Monotonic Increasing Stack `` mein ek naya element **`15`** push karna chahte hain, toh stack rule break ho jayega (kyunki 15 push hone par `30` ke upar `15` aa jayega, jo ascending sequence break kar dega).

Humein stack ko preserve karne ke liye, un saare elements ko **pop (nikalna)** padega jo `15` se bade hain!
1. Pop `30` → Stack: ``.
2. Pop `20` → Stack: ``.
3. Ab `15` safely push ho sakta hai → New Stack State: ``.

```
                    Monotonic Stack Element Displacement:
                    
                     Pushing 15 to:
                     
                     [ 30 ]  ──► Popped because 30 > 15
                     [ 20 ]  ──► Popped because 20 > 15
                     [ 10 ]  ──► Stays
                     
                     Final Stack State: [ 10, 15 ] (Strictly Monotonic!)
```

---

### Why Apparently O(n^2) problems become Amortized O(n)? 📐
Unsorted arrays mein nearest greater elements nikalne ka brute force double loop checks O(n^2) time leta hai. 

Lekin Monotonic Stack lagakar yeh linear **O(n)** ho jata hai!

*"Sir, nested loop toh isme bhi chalega jab hum pop operation trigger karenge. Fir yeh O(n) kaise hua?"*

Bacho, dhyan se is line ko dimaag mein betha lo:  
**Pure code execution ke dauran, array ka har ek element stack par maximum exactly 1 baar hi push ho sakta hai, aur maximum exactly 1 baar hi pop ho sakta hai.**  
Saare index passes elements ka absolute push/pop budget limit maximum 2N operations hota hai. Isiliye overall computation strictly amortized linear **O(n)** hi behave karegi!

---

### KEY PROBLEM: Next Greater Element (LeetCode 496 / 503 equivalent)

#### 1. Understand:
Humein ek integer array `arr` diya hai. Humein har element ke liye uske right side par aane wala **Next Greater Element (sabse pehla bada element)** dhoondhna hai. Agar kisi element ke liye right mein koi bada element nahi hai, toh use `-1` assign karo.

#### 2. Example:
Input: `arr =`  
Output: `[5, 5, -1, -1]`  
*(Explanation: 2 ke right mein first greater is 5. 1 ke right mein is 5. 5 ke right mein koi greater nahi, return -1).*

---

#### 3. Brute Force:
Har element ke liye ek inner loop chalakar rightmost elements traverse check karo jab tak greater element na mil jaye.
```javascript
for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
        if (arr[j] > arr[i]) {
            res[i] = arr[j];
            break;
        }
    }
}
```
* **Complexity:** Time: **O(n^2)**, Space: **O(1)**.
* **Bottleneck:** Same future elements check scan comparison loops baar-baar redundancies trigger karte hain.

---

#### 4. Optimal Approach (The Backward Decreasing Stack Sweep 💡):
* We traverse the array **backwards (right-to-left)**. Kyunki backwards aane par future history elements stack par pehle se mapped hote hain.
* Hum ek **Monotonic Decreasing Stack** maintain karenge.
* Har element `arr[i]` ke liye:
  1. Pop elements from stack as long as stack top is smaller than or equal to `arr[i]`.
  2. Pop checks complete hone par: agar stack empty bache, toh is element ke liye koi greater elements future mein nahi hai → map `-1`. Else, stack ka topmost element hi iska **Next Greater Element** hai!
  3. Current element `arr[i]` ko stack par push kar do taaki yeh left wale elements ke liye candidate greater target ban sake.

---

#### 5. JavaScript Code:
```javascript
function nextGreaterElement(arr) {
    const n = arr.length;
    const result = new Array(n).fill(-1); // Initialize result array
    const stack = []; // Monotonic Stack to hold index/value candidates

    // Backward sweep traversal
    for (let i = n - 1; i >= 0; i--) {
        const currentVal = arr[i];

        // Step 1: Pop elements smaller than current element
        while (stack.length > 0 && stack[stack.length - 1] <= currentVal) {
            stack.pop(); // Pop elements that cannot be Next Greater
        }

        // Step 2: Record next greater candidate
        if (stack.length > 0) {
            result[i] = stack[stack.length - 1]; // Top of stack is next greater!
        }

        // Step 3: Push current element to stack
        stack.push(currentVal);
    }

    return result;
}
```

#### 6. Complete visual Dry Run on `arr =`:
*   Initialize: `result = [-1, -1, -1, -1]`, `stack = []`.

*   **`i = 3` (`val = 3`):**
    *   Stack empty? Yes. No pops.
    *   Stack is empty, so `result = -1`.
    *   Push `3` → `stack =`.

*   **`i = 2` (`val = 5`):**
    *   Stack is ``. Since `3 <= 5` → `stack.pop()`. Stack becomes `[]`.
    *   Stack is empty, so `result = -1`.
    *   Push `5` → `stack =`.

*   **`i = 1` (`val = 1`):**
    *   Stack is ``. Top of stack `5 > 1`. No pop triggers.
    *   Stack is not empty, so `result = stack[top] = 5`.
    *   Push `1` → `stack =`.

*   **`i = 0` (`val = 2`):**
    *   Stack is ``. Since top `1 <= 2` → `stack.pop()`. Stack becomes ``.
    *   Since top `5 > 2`. Pop stops.
    *   `result = stack[top] = 5`.
    *   Push `2` → `stack =`.

*   **Converged Result:** `[5, 5, -1, -1]`. Absolutely correct!

#### 7. Complexity Analysis:
* **Time Complexity:** Amortized **O(n)**. (Push-pop operations counts is linearly bounded).
* **Space Complexity:** **O(n)** auxiliary stack storage boundaries limit.

---

## 5. HARDCORE SDE PATTERNS: PRACTICE CORNER

🚀 **Chalo bacho, whiteboard clear hai aur markers tayaar hain. Progressive challenges par identify karo ki choices kya hain, aur kis monotonic configuration ko choose karna hai!**

---

### Problem 1 (Medium): Daily Temperatures (LeetCode 739)
*Temperatures array diya hai (jaise ``). Humein har index par check karna hai ki aage kitne dino ke baad warm temperature dekhne ko milega (higher temperature).*

#### 🧠 Step 1: Let the Learner Identify!
*   *Choices:* Humein har temperature index par check karna hai ki rightward first warm (greater temperature) kis index coordinate par aayega.
*   *What to store on Stack?* Value ke bajay **Indices** store karenge taaki index offsets `(WarmIndex - CurrentIndex)` calculate kiya ja sake!
*   *Monotonic Stack Choice:* **Monotonic Decreasing Stack** (Pop as long as current temperature is warmer than top index temperature).

```javascript
function dailyTemperatures(temperatures) {
    const n = temperatures.length;
    const answer = new Array(n).fill(0);
    const stack = []; // To store INDICES of temperatures

    for (let i = 0; i < n; i++) {
        // Pop index as long as current temp is warmer than stack top index temp
        while (stack.length > 0 && temperatures[i] > temperatures[stack[stack.length - 1]]) {
            const prevIndex = stack.pop();
            answer[prevIndex] = i - prevIndex; // Calculate offset days gap
        }
        stack.push(i); // Push current index to stack
    }
    return answer;
}
```
* **Complexity:** Time: **O(n)**, Space: **O(n)**.

---

### Problem 2 (Medium): Stock Span Problem (LeetCode 901)
*Stock rate inputs daily streams mein mil rahe hain. Span nikalna hai ki pichle consecutive kitne dino tak stock price current day price se choti ya equal thi.*

#### 🧠 Step 1: Let the Learner Identify!
*   *Observation:* Yeh basically *"Previous Greater Element Index"* dhoondhne ka check pattern hai! PGN (Previous Greater Node) ke milte hi current stock price boundary distance span range resolve ho jati hai.
*   *Stack Store:* Pair values store karenge: `[price, span]`.

```javascript
class StockSpanner {
    constructor() {
        this.stack = []; // Store pair elements: [price, spanValue]
    }

    next(price) {
        let span = 1;
        // Pop and accumulate spans of all continuous elements smaller than or equal to current price
        while (this.stack.length > 0 && this.stack[this.stack.length - 1] <= price) {
            span += this.stack.pop(); // Accumulate past spans
        }
        this.stack.push([price, span]);
        return span;
    }
}
```
* **Complexity:** Time: **O(1)** amortized per query, Space: **O(n)**.

---

## 6. SDE TRAPS & COMMON MISTAKES ⚠️

Technical interviews ke emotional stress mein in 4 bugs se hamesha bacho bacho:

1.  ** TERRTerrible dequeue using Array `shift()`:**
    Large scale inputs range par standard array unshift ya shift never use karein. Always use either custom objects with index pointers or pointer-link lists.
2.  **Incorrect Monotonic Direction Updates:**
    Next greater query par smaller logic run kar dena, ya loop bounds direction checks opposite sequence trace updates miss out kar dena.
3.  **Forgetting Stack/Queue Underflow boundaries Checks:**
    Stack pop or peek calculations trigger karne se pehle empty validations size evaluations skip kar dena, jo program par standard `null reference errors` crash create karte hain.
4.  **Reference Aliasing Stack snapshots copies:**
    Array coordinates modifications mutations elements restore operations backtracking states par avoid gaps prevent.

---

## CHAPTER END SUMMARY

### Completed Topics:
* Stack LIFO wedding plates structures and dynamic operations.
* Sequential V8 engines call stack recursion boundaries tracing.
* Array dynamic shift indexes latency bottlenecks.
* MODULO logical circular arrays queues boundaries updates.
* Monotonic increasing vs decreasing stacks amortized runs.

### Mastered Patterns:
* **LIFO parenthesis checks stack evaluation** balanced strings validating par.
* **Two stacks logical reversed mappings** to construct Queue standard limits.
* **Backwards decreasing stack elements checks** for Next Greater elements configurations.

---

### SDE Practice Roadmap:
1.  Solve *Valid Parentheses* on LeetCode 20.
2.  Implement *Queue using Stacks* on LeetCode 232.
3.  Complete *Stock Span* and *Next Greater Element* LeetCode challenges.

---


