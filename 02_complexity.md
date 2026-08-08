**Namaste bacho! Aa jao class mein, aur apna notebook aur pen nikal kar baith jao.** 

Pichle chapter mein humne seekha tha ki DSA kya hota hai aur iski kya importance hai. Aaj hum DSA ka sabse core, sabse basic aur sabse zyada practical chapter shuru karne ja rahe hain—**Complexity Analysis**. 

Bohot se bache mujhse kehte hain, *"Sir, humein standard algorithms ki Time Complexity toh yaad hai, par jab interviewer naya code dekar complexity poochta hai, toh humara dimaag blank ho jata hai!"*

**Mera waada hai:** Aaj ki is masterclass ke baad, tum kisi bhi JavaScript code ko dekhkar uski Time aur Space Complexity **khud apne dimaag se** nikaal paoge, bina kisi ratte ke! Ekdam basic se shuru karenge aur advanced interview level tak jayenge. Let's start!

---

## 1. FOUNDATION: WHY COMPLEXITY MATTERS

Manlo tumhare paas ek problem hai aur use solve karne ke liye do bacho—Amit aur Sumeet—ne alag-alag code likha. Dono ka code bilkul sahi chal raha hai aur exact correct output de raha hai.

* **Amit ka code:** 10 elements ke liye 0.01 milliseconds leta hai.
* **Sumeet ka code:** 10 elements ke liye 0.012 milliseconds leta hai.

Amit chillane laga, *"Mera code fast hai!"* Lekin kya Amit sach bol raha hai? 

```
┌────────────────────────────────────────────────────────┐
│               THE SCALABILITY SHOCK                    │
├────────────────────────────────────────────────────────┤
│                                                        │
│   Input Size (n)   │  Amit's Code   │  Sumeet's Code   │
│   ─────────────────┼────────────────┼────────────────  │
│   10               │   0.01 ms      │   0.012 ms       │
│   10,000           │   100 ms       │   0.15 ms        │
│   1,000,000        │   15 Hours     │   1.2 ms         │ (System Crash!)
│                                                        │
└────────────────────────────────────────────────────────┘
```

**Yeh hota hai real-world scalability shock.** Jab input size **\\(n\\)** lakhon-karodon mein jata hai, tab ganda code collapse ho jata hai aur achha code bina kisi dikkat ke milliseconds mein chalta hai.

### growth trend ko kyun analyze karte hain, exact seconds ko kyun nahi?
Hum kabhi bhi complexity ko seconds ya milliseconds mein nahi naapte, kyunki:
1. **Hardware Dependency:** Ek hi code mere powerful core-i9 laptop par 1 second mein chalega, par tumhare purane phone par 10 seconds lega.
2. **Operating System Factors:** background execution, RAM speed aur CPU core allocation ke mutabik run-time badal jata hai.

Isiliye computer science mein hum **Asymptotic Analysis** ka use karte hain. Iska matlab hai: **"Jaise-jaise input size \\(n\\) infinity ki taraf badhega, waise-waise operations ka count kis rate se grow karega?"**

---

## 2. ASYMPTOTIC NOTATIONS (THE BOUNDS)

Interviewers aksar poochte hain, *"Big-O, Big-Theta aur Big-Omega mein kya farq hai?"* Aur 90% bache galat answer dete hain ki *"Big-O worst case hai, Omega best case hai."* **Yeh bilkul galat definition hai!** 

Asymptotic notations mathematics hain jo functions ke growth bounds ko define karte hain:

```
  Operations
     ▲
     │          / Upper Bound: Big-O (O) - Maximum rate of growth
     │         /
     │        /─── Actual Algorithm Execution Rate
     │       /
     │      /───── Lower Bound: Big-Omega (Ω) - Minimum rate of growth
     │     /
     └─────┴─────────────────────────────────────► Input Size (n)
```

1. **Big-O (\\(O\\)) — Upper Bound (Roof/Ceiling):** Yeh batata hai ki tumhara code maximum kitne operations le sakta hai. Is bound ko rate of growth kabhi cross nahi karega.
2. **Big-Omega (\\(\Omega\\)) — Lower Bound (Floor):** Yeh batata hai ki tumhara code kam se kam kitne operations lega hi lega (minimum benchmark).
3. **Big-Theta (\\(\Theta\\)) — Tight Bound:** Jab upper bound aur lower bound bilkul same ho jayein, toh use tight bound kehte hain. Yeh exact rate of growth ko lock kar deta hai.

### Best, Average, aur Worst Case Analysis
Case analysis completely dependent hai **input ke behavior/distribution** par, jabki notations mathematical bounds hain.

Chalo ek simple **Linear Search** ka example lete hain:
`let arr =` aur humein ek `target` dhoondhna hai.

* **Best Case (Target is 10):** Pehle hi step par target mil gaya. Operations = 1. Tight bound is \\(\Theta(1)\\).
* **Worst Case (Target is 100):** Target pure array mein nahi mila. Pooray array ko traverse karna pada. Operations = \\(n\\). Tight bound is \\(\Theta(n)\\).
* **Average Case:** Target array ke beech mein kahin mila. Average steps = \\(\approx n/2\\). Constant factor hatane par tight bound is \\(\Theta(n)\\).

> **Interview Golden Rule:** Interviews mein hum hamesha **Worst Case ka Upper Bound** describe karte hain, jise hum **Big-O (\\(O\\))** se represent karte hain taaki hum safety margin ensure kar sakein.

---

## 3. COMMON COMPLEXITY SPECTRUM (GROWTH CLASSES)

Chalo standard growth classes ki intuitive understanding build karte hain JavaScript examples ke sath:

### A. \\(O(1)\\) — Constant Time (Instant)
Input size kitna bhi badhe, code hamesha ek fixed aur constant number of operations hi perform karega.
```javascript
function getFirstElement(arr) {
    return arr; // Direct address offset pointer access
}
```
* **Why:** Engine directly elements ke starting block address par offset calculation karke jump karta hai. Iska size se koi matlab nahi.

### B. \\(O(\log n)\\) — Logarithmic Time (Very Fast)
Har operation ke baad, problem ka size direct **aadhi (half)** reh jati hai.
* **Example:** Sorted array par **Binary Search** chalana.
```javascript
// Har iteration par search range aadhi (half) ho jati hai
```
* **Why:** Agar input size \\(n = 16\\) hai, toh \\(16 \rightarrow 8 \rightarrow 4 \rightarrow 2 \rightarrow 1\\) (sirf 4 steps lagte hain, jo ki \\(\log_2(16) = 4\\) hai).

### C. \\(O(n)\\) — Linear Time (Fair)
Operations, input size \\(n\\) ke directly proportional badhte hain.
```javascript
function findElement(arr, target) {
    for (let i = 0; i < arr.length; i++) { // Runs N times
        if (arr[i] === target) return true;
    }
    return false;
}
```
* **Why:** Worst-case mein array ke har single slot ko check karna padega.

### D. \\(O(n \log n)\\) — Linearithmic Time (Good)
Efficient sorting algorithms (jaise Merge Sort aur Quick Sort) is rate par chalte hain.
* **Why:** Divide and Conquer strategy mein, divide hone par levels \\(\log n\\) hote hain, aur har level par complete boundary range array merging ke liye \\(O(n)\\) loop chalta hai.

### E. \\(O(n^2)\\) — Quadratic Time (Slow)
Input size double hone par, operations **char guna (4x)** badh jate hain.
```javascript
function printAllPairs(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length; j++) {
            console.log(arr[i], arr[j]);
        }
    }
}
```
* **Why:** Outer loop \\(n\\) baar chalega, aur har ek outer loop iteration ke liye inner loop bhi \\(n\\) baar chalega, yani total \\(n \times n = n^2\\) operations.

### F. \\(O(2^n)\\) — Exponential Time (Dangerous)
Har ek additional input ke sath operations **double** ho jate hain.
* **Example:** Naive Recursive Fibonacci algorithm without caching.
```
                      fib(4)
                     /      \
                 fib(3)      fib(2)
                /     \     /     \
             fib(2)  fib(1)fib(1) fib(0)
```
* **Why:** Har ek step par call tree recursively split hota hai, jisse dynamic growth speed exponential ho jati hai.

### G. \\(O(n!)\\) — Factorial Time (Unusable)
Sabse dangerous aur worst growth rate. String ke saare possible permutations generate karna.
* **Computations Comparison:** Agar \\(n=10\\) hai, toh operations count is \\(3,628,800\\)! Agar \\(n=100\\) ho, toh complete collapse.

---

## 4. CODE ANALYSIS (HOW TO DECODE COMPLEXITY FROM SCRATCH)

**Dekho, code dekhkar complexity nikalne ke do sunhre rules hain:**
* **Addition (O(a + b)):** Jab operations ek ke baad ek sequential chalte hain.
* **Multiplication (O(a * b)):** Jab operations nested loop ke andar chalte hain.

### A. Statement-by-Statement Analysis
```javascript
let a = 10;           // O(1)
let b = 20;           // O(1)
let sum = a + b;      // O(1)
```
* **Derivation:** \\(O(1) + O(1) + O(1) = 3 \cdot O(1)\\). Constants drop karne par, total complexity **\\(O(1)\\)** bachti hai.

---

### B. Sequential Loops (Addition)
```javascript
function process(arr) {
    let n = arr.length;
    // Loop 1
    for (let i = 0; i < n; i++) {
        console.log(arr[i]); // Runs N times
    }
    // Loop 2
    for (let j = 0; j < n; j++) {
        console.log(arr[j]); // Runs N times
    }
}
```
* **Analysis:** Pehla loop \\(n\\) operations leta hai. Uske khatam hone par, dusra loop bhi \\(n\\) operations leta hai.
* **Calculation:** \\(O(n) + O(n) = O(2n)\\).
* **Simplification:** Drop Constants rule ke mutabik: **\\(O(n)\\)**.

---

### C. Standard Nested Loops (Multiplication)
```javascript
function printGrid(arr) {
    let n = arr.length;
    for (let i = 0; i < n; i++) {         // Outer: Runs N times
        for (let j = 0; j < n; j++) {     // Inner: Runs N times
            console.log(arr[i], arr[j]);  // O(1) work
        }
    }
}
```
* **Analysis:** Outer loop \\(i=0\\) par inner loop ko \\(n\\) baar chalata hai. Aise hi \\(i=1\\) par bhi \\(n\\) baar chalata hai.
* **Calculation:** \\(N \times N = N^2\\) operations.
* **Time Complexity:** **\\(O(n^2)\\)**.

---

### D. Dependent/Triangular Nested Loops
**Yeh interviewers ka sabse favourite trap hai! Dhyan se dekho.**
```javascript
function printTriangle(arr) {
    let n = arr.length;
    for (let i = 0; i < n; i++) {         // Outer: Runs N times
        for (let j = 0; j <= i; j++) {    // Inner: Runs dynamically up to 'i'
            console.log(arr[i], arr[j]);
        }
    }
}
```
* **Trace-table & Dry Run:**
  * Jab \\(i=0\\), inner loop chalta hai hamesha: **\\(1\\)** step.
  * Jab \\(i=1\\), inner loop chalta hai: **\\(2\\)** steps.
  * Jab \\(i=2\\), inner loop chalta hai: **\\(3\\)** steps.
  * ...
  * Jab \\(i=n-1\\), inner loop chalta hai: **\\(n\\)** steps.
* **Summation of AP (Arithmetic Progression):**
  \\[\text{Total Operations} = 1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2} = \frac{n^2 + n}{2}\\]
* **Dominant Term Extraction:** Hum high-order power (\\(n^2\\)) ko rakhte hain, aur low-order (\\(n\\)) aur division constant (\\(1/2\\)) ko discard kar dete hain.
* **Time Complexity:** **\\(O(n^2)\\)**.

---

### E. Variable updating with Multipliers (`i *= 2` vs `i /= 2`)
```javascript
function binaryJump(n) {
    for (let i = 1; i < n; i *= 2) { // Updates by multiplication
        console.log(i);
    }
}
```
* **Dry Run values of `i`:**
  * Step 1: \\(i = 1\\)
  * Step 2: \\(i = 2\\)
  * Step 3: \\(i = 4\\)
  * Step 4: \\(i = 8\\)
  * Step \\(k\\): \\(i = 2^k\\)
* **Solving for stopping point:** Loop tab rukega jab \\(2^k \ge n\\) ho jaye. Dono sides base-2 log apply karne par: \\(k = \log_2(n)\\).
* **Time Complexity:** **\\(O(\log n)\\)**.

---

### F. Multiple Independent Input Variables (\\(n\\) and \\(m\\))
```javascript
function processGrid(arr1, arr2) {
    for (let i = 0; i < arr1.length; i++) {       // Runs N times
        for (let j = 0; j < arr2.length; j++) {   // Runs M times
            console.log(arr1[i], arr2[j]);
        }
    }
}
```
* **Trap Alert:** Yahan tum ise \\(O(n^2)\\) nahi bol sakte! Kyunki `arr1` aur `arr2` complete different ranges ke ho sakte hain (e.g. \\(N=5\\), \\(M=1,000,000\\)).
* **Time Complexity:** **\\(O(n \times m)\\)**.

---

### G. Early Exit & Conditions
```javascript
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) return true; // Break/early return
    }
    return false;
}
```
* **Best Case:** Target index 0 par mil jaye \\(\rightarrow O(1)\\).
* **Worst Case:** Target index \\(n-1\\) par mile ya absent ho \\(\rightarrow O(n)\\).
* **Rule:** Interviews mein hamesha Worst Case hi represent kiya jata hai, isiliye iska safe bound **\\(O(n)\\)** hai.

---

## 5. THE SIMPLIFICATION RULES

Complexity ko hamesha clean aur asymptotic bounds mein rakhne ke liye yeh do rules yaad rakho:

### Rule 1: Ignore Constant Multipliers
Asymptotic analysis mein constant factors irrelevant ho jate hain jab input scale extreme high par pahunch jaye.
* \\(O(2n) \longrightarrow O(n)\\)
* \\(O(500n^2) \longrightarrow O(n^2)\\)
* \\(O(3n \log n + 100) \longrightarrow O(n \log n)\\)

### Rule 2: Keep only the Dominant Term (Drop Lower-Order Terms)
Agar ek code segment multiple components ka dynamic scale maintain kar raha hai, toh hum low-rate growth levels ko completely discard kar dete hain.
* \\(O(n^2 + n) \longrightarrow O(n^2)\\)
* \\(O(n^3 + n \log n + 50) \longrightarrow O(n^3)\\)
* \\(O(2^n + n^5) \longrightarrow O(2^n)\\) (Since exponential scales far exceed polynomial)

---

## 6. SPACE COMPLEXITY (MEMORY ALLOCATIONS)

**Space Complexity ka matlab hai: "Jaise-jaise input size \\(n\\) badhega, waise-waise computer extra memory kitni consume karega?"**

\\[\text{Total Space Complexity} = \text{Input Space} + \text{Auxiliary Space}\\]

* **Input Space:** original input values ko rakhne ke liye jo RAM reserve hoti hai.
* **Auxiliary Space:** Wo extra memory jo tumhare program ne problem ko solve karne ke liye khud dynamically allocate ki (e.g. temporary variables, dynamic arrays, sets). **Interviews mein hamesha focus Auxiliary Space par hota hai!**

### JavaScript data types ki memory footprint:
1. **Primitives (Variables):** Standard numbers, booleans, pointers hamesha steady space allocate karte hain \\(\rightarrow O(1)\\) space.
2. **Arrays (\\(O(n)\\) Space):** Agar tum naya array banakar dynamically push kar rahe ho:
   ```javascript
   let list = [];
   for (let i = 0; i < n; i++) list.push(i); // O(n) Extra Space
   ```
3. **In-place Algorithms:** Jo algorithms bina naya data structure create kiye original inputs ko overwrite karke process karte hain, unka auxiliary space hamesha **\\(O(1)\\)** constant hota hai.

---

## 7. RECURSION COMPLEXITY & THE CALL STACK

Recursion mein space complexity calculate karte waqt hamesha **Call Stack** memory par dhyan dena zaroori hai, jo humari sub-operations frames save rakhti hai.

### Example A: Factorial Recursion Depth
```javascript
export function factorial(number) {
    if (number === 0) return 1; // Base case
    return factorial(number - 1) * number; // Recursive call
}
```

* **Recursion Tree Analysis:** 
  factorial(4) \\(\rightarrow\\) factorial(3) \\(\rightarrow\\) factorial(2) \\(\rightarrow\\) factorial(1) \\(\rightarrow\\) factorial(0).
* **Operations Count (Time):** Total calls \\(N\\) times lagti hain, har call mein const math operation hota hai \\(\rightarrow\\) **Time: \\(O(n)\\)**.
* **Memory Call Stack (Space):** Jab tak `factorial(0)` execute nahi hota, computer ko purani saari calls (`factorial(1)` se `factorial(4)`) call stack par hold karke rakhni padti hain.
* **Space Complexity:** **\\(O(n)\\)** auxiliary space stack trace ke liye.

---

## 8. AMORTIZED COMPLEXITY (DYNAMIC ARRAY RESIZING)

Manlo tum stadium mein match dekhne gaye aur stadium mein seats limit complete ho gayi. Ab stadium ko double capacity ka banana hai, toh poore bache hue spectators ko nayi double sized building mein shift karna padega. Is specific migration step mein bohot cost lagegi.

JavaScript Arrays internally dynamic size arrays use karte hain.

```
Step 1: Size is 2. Elements: [ 10, 20 ]
Step 2: Want to insert 30. No space!
Step 3: Array creates a NEW block of size 2 * 2 = 4 in background.
Step 4: Copies old elements: [ 10, 20, 30, _ ] (Expensive operation!)
```

### Amortized Intuition:
* Jab size limit complete hoti hai, tab insertion costly high execution step leta hai (size scaling up \\(O(n)\\)).
* Lekin yeh resizing step hamesha nahi hota! Yeh tabhi hota hai jab space complete saturate ho (e.g. at size 2, 4, 8, 16).
* Baaki bache saare standard insertion steps hamesha **\\(O(1)\\)** constant fast hote hain.
* Agar hum in expensive shiftings aur standard inserts ko distribute karke average out karein, toh average cost hamesha **\\(O(1)\\)** constant hi aati hai. **Is average overall consistency cost ko Amortized Complexity kehte hain.**

---

## 9. JAVASCRIPT BUILD-IN ENGINE COMPLEXITIES

V8 engine ke dynamic structures execution parameters lower-level pointers se calculate hote hain. Inki internal DSA complexities ka table dekho:

| Built-In Method | Average / Worst Complexity | Engine Level Architectural Mechanism |
| :--- | :--- | :--- |
| **Array Access (`arr[i]`)**| **\\(O(1)\\)** | contiguous packed memory addresses base pointer offset jump. |
| **`push()` / `pop()`** | **\\(O(1)\\) Amortized** | End append. Shifting ki koi overhead nahi hoti. |
| **`unshift()` / `shift()`**| **\\(O(n)\\)** | Free spaces zero indexes setup karne ke liye elements shift rightward. |
| **`slice(s, e)`** | **\\(O(k)\\)** (where \\(k\\) is segment len) | Segment memory data extraction blocks copying. |
| **`splice(start, count)`**| **\\(O(n)\\)** | Dynamic updates, middle removals and right shiftings execution. |
| **`includes()` / `indexOf()`**| **\\(O(n)\\)** | Complete sequential linear loop iteration check from start to end. |
| **`Map.get()` / `Map.has()`**| **\\(O(1)\\)** | Key hash representations evaluators lookup. |
| **`Set.has()`** | **\\(O(1)\\)** | Hashing lookup mapping. |
| **`Array.sort()`** | **\\(O(n \log n)\\)** | V8 mergesort/quicksort hybrid optimizations sorting algorithms. |

---

## 10. PROGRESSIVE CLASSROOM PRACTICE

🚀 **Chalo dosto, whiteboard bilkul khali hai. Ab main tumhein teen code blocks dunga. Inhe dhang se analyze karo aur time aur space complexity calculate karo!**

### Problem 1 (Easy): Alternate Jump Scan
```javascript
function skipTraverse(arr) {
    let count = 0;
    for (let i = 0; i < arr.length; i += 2) { // Jump step is constant 2
        count += arr[i];
    }
    return count;
}
```

#### 🧠 Analysis & Solution:
* **Time Analysis:** Loop variable `i` hamesha index coordinate 2-size se jump karta hai (alternate sequence). Total iterations honge hamesha \\(\frac{n}{2}\\). Drop constant multiplier rule ke mutabik: \\(\frac{1}{2} \times n \longrightarrow O(n)\\) bounds.
* **Space Analysis:** Sirf ek integer variable `count` utilize ho raha hai memory par jo input size par scale nahi hota \\(\longrightarrow O(1)\\) Auxiliary.
* **Complexity:** **Time: \\(O(n)\\) | Space: \\(O(1)\\)**.

---

### Problem 2 (Medium): Nested Binary Half-Step
```javascript
function nestedHalf(n) {
    let sum = 0;
    for (let i = 0; i < n; i++) {                 // Outer: Runs N times
        for (let j = n; j > 0; j = Math.floor(j / 2)) { // Inner: Divide step
            sum += i + j;
        }
    }
    return sum;
}
```

#### 🧠 Analysis & Solution:
* **Time Analysis:** Outer loop runs exactly \\(n\\) times linearly. Inner loop `j` hamesha har step par half scale divide hota hai, jo humein logarithmic operations bounds deta hai (i.e. \\(\log_2(n)\\)). Dono loops nested order multiplication rule follow karte hain: \\(N \times \log(N)\\).
* **Space Analysis:** Extra data structure map ya lists construct nahi ho rahi, only primitives modification: \\(\rightarrow O(1)\\) Auxiliary.
* **Complexity:** **Time: \\(O(n \log n)\\) | Space: \\(O(1)\\)**.

---

### Problem 3 (Challenging): Dynamic Window AP Builder
```javascript
function createPyramids(n) {
    let result = [];
    for (let i = 1; i <= n; i++) {
        let currentWindow = [];
        for (let j = 0; j < i; j++) {
            currentWindow.push(j); // dynamic push
        }
        result.push(currentWindow);
    }
    return result;
}
```

#### 🧠 Analysis & Solution:
* **Time Analysis:** Outer loop runs \\(n\\) times sequentially. Inner loop dynamically depends on value of `i`, forming standard triangular AP series steps: \\(1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2}\\). Keep only dominant term: **\\(O(n^2)\\)**.
* **Space Analysis (Auxiliary Space):** result array ke andar total subArrays allocate ho rahe hain. Row 1 mein 1 element, Row 2 mein 2 elements, up to Row \\(n\\) mein \\(n\\) elements. Total slots occupied in result structures: \\(1 + 2 + \dots + n \approx \frac{n(n+1)}{2}\\) cells.
* **Complexity:** **Time: \\(O(n^2)\\) | Space: \\(O(n^2)\\)**.

---

## 11. COMMON MISTAKES & INTERVIEW PREP

SDE interviews mein complexity analysis pe sabse bada difference tumhare bolne ke tarike se banta hai:

### ❌ Beginners Weak Answer:
*"Sir, this is O(N square) because there are double loops."* (Boring, rote learner look).

### ✅ SDE-1 Level Professional Answer:
> *"Sir, the worst-case Time Complexity of my solution is \\(O(n^2)\\). This is because the outer loop runs exactly \\(n\\) times, and for each iteration, the inner loop processes elements starting from the current index \\(i\\) up to the end of the array. This forms an Arithmetic Progression summing up to \\(\frac{n(n+1)}{2}\\) operations. When we drop constants and non-dominating terms, we arrive at a tight quadratic bound.*
>
> *For space, we are not allocating any extra dynamic structures like maps or sets, nor using recursion stack frames, making our Auxiliary Space complexity constant, i.e., \\(O(1)\\)."*

---

### ✅ Completed | Chapter 2 — Complexity Analysis

🧠 **Key Takeaways:**
* Complexity milliseconds nahi naapti, balki growth trends evaluate karti hai jaise-jaise \\(n\\) infinity badhega.
* Primitives hamesha constant auxiliary storage space consume karte hain.
* Recursive functions memory frames stack dynamically capture karte hain jo space overhead badhate hain.

⚠️ **Common Mistakes:**
* Har nested loops code ko blindly quadratic \\(O(n^2)\\) bol dena, bina updates checking variables jump scale kiye.
* Input space aur auxiliary dynamically generated variables storage space ko mismatch kar jana.

⏭️ **Next: Arrays Fundamentals (Creation, Traversal Mechanics & Operations Logic)**

---
📊 Mujhe batao bacho, kya ab time aur space complexity calculate karne ki core logic tumhare dimaag mein dhang se fit ho gayi? Agar koi bhi doubt hai, toh poocho, warna hum **Phase 2 - Chapter 3: Arrays Fundamentals** ki taraf badhein?
