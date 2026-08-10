**Arey bacho! Jaldi se apni seats par baith jao aur dhyan seedhe whiteboard par lagao.**

Pichle chapter mein humne **Backtracking (Chapter 9)** ke complex state-space trees aur recursion-undo mechanics ko poore dhasu tarike se samajh liya tha. Humne dekha ki kaise hum saare combinations ya permutations generate karte hain. 

Lekin beta, jab hum real-world systems build karte hain, toh humara sabse core task hota hai: **"Data ko dhoondhna" (Searching)**. Imagine karo, Google par tumne kuch search kiya, ya Instagram par kisi dost ki profile dhoondhi—agar computer har bar lakhon-karodon records par line-by-line loop chalane laga, toh tumhara application wahi freeze ho jayega! 

Aaj hum is chapter mein **Searching Algorithms** ke un powerful concepts ko zero se shuru karenge jo system efficiency ko drastically improve karte hain. Hum basic Linear Search se lekar, high-performance Binary Search, aur uske baad pure DSA ka sabse dhasu pattern—**Binary Search on Answer**—tak sab kuch visual dry runs aur solid line-by-line JavaScript implementations ke sath breakdown karenge.

Apni notebooks nikal lo, aur chalo dimaag ki bati jalate hain! 💡

---

## 1. COMPLEXITY MATRIX: LINEAR SEARCH vs. BINARY SEARCH

Bacho, searching ko bilkul simple tarike se samajhte hain. Maan lo tumhare paas ek kitabon ka dher (unsorted pile of books) hai aur main tumse kahun ki *"Hinglish Coding Manual"* dhoondh ke nikalo. Tum kya karoge? Tum ek-ek karke har kitab ko check karoge jab tak woh kitab mil nahi jaati. Isi sequential check ko hum **Linear Search** kehte hain.

Lekin agar wahi kitabein library ki shelf par alphabetically perfectly sorted arranged hon, toh tum direct beech wale segment par jaoge. Agar tumhara alphabet aage hai, toh tum aadhi shelf ko instantly ignore kar doge aur right-half par focus karoge. Isi divide-and-conquer approach ko hum **Binary Search** kehte hain.

Dhyan se is comparative breakdown table ko dekho:

| Feature | Linear Search 🔍 | Binary Search 🎯 |
| :--- | :--- | :--- |
| **Data Requirement** | Kisi bhi random, unsorted array par chal jata hai. | Array ka **sorted ya searchable (monotonic)** hona compulsory hai. |
| **How it Works?** | Index `0` se lekar `N-1` tak sequence mein ek-element ko check karta hai. | Search space ko har step par **aadha (half)** split karta hai. |
| **Best Case Time** | **O(1)** — Agar target pehle hi index par mil jaye. | **O(1)** — Agar target bilkul middle element (`mid`) hi ho. |
| **Worst/Average Time**| **O(n)** — Agar target last mein ho ya array mein ho hi na. | **O(log n)** — Har step par options half hone ke karan. |
| **Space Complexity** | **O(1)** — Koi extra helper array nahi chahiye. | **O(1)** for iterative; **O(log n)** stack space for recursive. |
| **JS Engine Behavior**| `Array.prototype.includes()` ya `indexOf()` internally ise hi run karte hain. | Custom implement karna padta hai, modern engines par minimal instructions leta hai. |

---

## 2. THE FOUNDATION: LINEAR SEARCH

### 🧠 Step-by-Step Diagnostic Block

#### 1. Problem:
Ek unsorted array `arr` aur ek `target` value di hai. Target ka index return karo, agar target absent hai toh `-1` return karo.

#### 2. Understand:
Humein array ke index `0` se aakhri element tak linearly traverse karna hai. Jaise hi element match hoga, search halt karke index return karenge.

#### 3. Brute Force / Optimal (Linear Search is itself the only path for unsorted data):
Unsorted data par sorting ka overhead **O(n log n)** hota hai, isiliye unsorted arrays par target search karne ke liye single linear pass of **O(n)** hi best possible solution hai.

#### 4. JavaScript Code:
```javascript
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) {
            return i; // Target found at index i
        }
    }
    return -1; // Target not present in the array
}
```

#### 5. Dry Run on `arr =`, `target = 8`:
*   `i = 0`: `arr = 5 !== 8` → Continue.
*   `i = 1`: `arr = 3 !== 8` → Continue.
*   `i = 2`: `arr = 8 === 8` → Match found! Return index `2`.
*   **Complexity:** Time: **O(n)** worst case, Space: **O(1)** auxiliary.

---

## 3. THE MAGIC OF BINARY SEARCH (DIVIDE THE SEARCH SPACE)

### Intuition: "Aadha Kaat Do!" Pattern
Bacho, Binary Search ka core mantra hai: **Search space ko reduce karna**. Agar humein pata hai ki humari range sorted hai, toh hum har baar center element (`mid`) ko check karte hain. 
*   Agar `arr[mid] === target`, toh game over! Target mil gaya.
*   Agar `arr[mid] < target`, toh iska matlab target right side par hi hoga (kyunki left side wale toh aur bhi chote honge). Hum left half ko drop karke `low = mid + 1` kar dete hain.
*   Agar `arr[mid] > target`, toh hum right half ko drop karke `high = mid - 1` kar dete hain.

```
                     Search Space Partitioning:
                     
                     Sorted Array: [ 2,  4,  6,  8, 10, 12, 14 ]
                                     ▲           ▲           ▲
                                    low         mid         high
                                     
                     If target = 12: Since 12 > arr[mid] (8):
                     New Search Space:           [ 10, 12, 14 ]
                                                   ▲   ▲   ▲
                                                  low mid high
```

---

### Safe Mid Calculation Concept: Avoiding Integer Overflow 🚨
Bohot se bache `mid` nikalne ke liye aankh band karke likhte hain:
```javascript
let mid = Math.floor((low + high) / 2);
```
**Yeh bad habit hai bacho!** Agar `low` aur `high` dono bohot bade integers hain (jaise 2 × 10^9 ke kareeb), toh unka sum `low + high` JavaScript ke absolute safe integer limit (ya standard languages mein 32-bit signed integer limits) ko exceed kar jayega, jisse integer overflow bug ho jayega.

#### Safe Formula:
\\[mid = low + ⌊ high - low/2 ⌋\\]
Yeh mathematical representation overflow ko bilkul prevent karti hai kyunki hum `high - low` (jo ki humesha range se chota hoga) ko evaluate karke dynamic adjust kar rahe hain.

---

### Standard Iterative Implementation

```javascript
function binarySearchIterative(arr, target) {
    let low = 0;
    let high = arr.length - 1; //

    while (low <= high) { //
        // Safe mid calculation to prevent overflow
        const mid = low + Math.floor((high - low) / 2); //

        if (arr[mid] === target) {
            return mid; // Element found
        } else if (arr[mid] < target) {
            low = mid + 1; // Discard left half
        } else {
            high = mid - 1; // Discard right half
        }
    }
    return -1; // Target not found
}
```

---

### Standard Recursive Implementation

```javascript
function binarySearchRecursiveHelper(arr, target, low, high) {
    // Base Case: Range exhausted, element is not present
    if (low > high) {
        return -1; //
    }

    const mid = low + Math.floor((high - low) / 2);

    if (arr[mid] === target) {
        return mid; // Target found
    } else if (arr[mid] < target) {
        // Search right half
        return binarySearchRecursiveHelper(arr, target, mid + 1, high); //
    } else {
        // Search left half
        return binarySearchRecursiveHelper(arr, target, low, mid - 1); //
    }
}

function binarySearchRecursive(arr, target) {
    return binarySearchRecursiveHelper(arr, target, 0, arr.length - 1); //
}
```

---

### Step-by-Step Dry Run of Recursive Binary Search

#### Input:
`arr =`, `target = 12`

#### Tracing Call Stack:
1.  **First Call:** `binarySearchRecursiveHelper(arr, 12, 0, 6)`
    *   `mid = 0 + Math.floor((6 - 0) / 2) = 3`
    *   `arr` is `8`.
    *   Since `8 < 12`, recursive trigger: `mid + 1` → `binarySearchRecursiveHelper(arr, 12, 4, 6)`
2.  **Second Call:** `binarySearchRecursiveHelper(arr, 12, 4, 6)`
    *   `mid = 4 + Math.floor((6 - 4) / 2) = 5`
    *   `arr` is `12`.
    *   `12 === 12` → Target found! Returns index `5`.

#### Complexity:
*   **Time Complexity:** **O(log n)** in all cases.
*   **Space Complexity:** **O(1)** for Iterative. **O(log n)** for Recursive due to call stack frames.

---

## 4. CORE BOUNDARY PATTERNS: FIRST & LAST OCCURRENCE

SDE interviews mein direct search bohot kam poochte hain, bacho. Interviewers ka favourite pattern hota hai: *"Duplicates wale sorted array mein element ki **First/Last Occurrence** nikaalo"*.

### Boundary Logic Comparison:
*   **First Occurrence (Lower Boundary):** Jab hume target milta hai (`arr[mid] === target`), toh hum wahi par stop nahi karte! Ho sakta hai uske left mein aur bhi same elements hon. Hum index ko `result` mein save karte hain aur range ko left side shift karte hain: `high = mid - 1`.
*   **Last Occurrence (Upper Boundary):** Jab target milta hai, hum use `result` mein save karke right side shift karte hain: `low = mid + 1`, taaki agar aage aur same elements hon toh unhe dhoondh sakein.

```
                      First Occurrence Search Space Shift:
                      
                      Array: [ 2,  4,  4,  4,  8, 10 ]
                                   ▲   ▲   ▲
                                       mid
                      Match found! Save mid, shift high to mid - 1.
                      New Search Space: [ 2, 4 ] -> Keep checking left!
```

---

### Key Problem: First & Last Occurrence of Target in Sorted Array

#### 1. Understand:
Humein sorted array mein target ki first aur last position dhoondhni hai. Agar target absent hai, toh return `[-1, -1]`.

#### 2. Optimal Approach:
Do independent binary searches run karenge—ek first occurrence ke liye, dusri last occurrence ke liye.

#### 3. JavaScript Code:
```javascript
function findFirstOccurrence(arr, target) {
    let low = 0;
    let high = arr.length - 1;
    let result = -1;

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);
        if (arr[mid] === target) {
            result = mid;       // Potential answer found
            high = mid - 1;     // Keep searching left for earlier occurrence!
        } else if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return result;
}

function findLastOccurrence(arr, target) {
    let low = 0;
    let high = arr.length - 1;
    let result = -1;

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);
        if (arr[mid] === target) {
            result = mid;       // Potential answer found
            low = mid + 1;      // Keep searching right for later occurrence!
        } else if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return result;
}

function searchRange(arr, target) {
    return [findFirstOccurrence(arr, target), findLastOccurrence(arr, target)];
}
```
*   **Complexity:** Time: **O(log n)**, Space: **O(1)**.

---

### Related Applications: Count Occurrences, Lower/Upper Bound, Floor/Ceil
Yeh boundary logic teen aur standard problems ko instantly solve kar deti hai:

1.  **Count Occurrences of Target:**
    Sorted array mein target total kitni baar aaya? Simple formula:
    \\[Total Count = LastOccurrence - FirstOccurrence + 1\\]
    *(Agar element present hai, i.e., FirstOccurrence !== -1)*.
2.  **Lower Bound (Search Insert Position):**
    Humein woh pehla index dhoondhna hai jahan element ki value `target` ke barabar ya usse badi ho (`arr[mid] >= target`).
    ```javascript
    function lowerBound(arr, target) {
        let low = 0, high = arr.length - 1, ans = arr.length;
        while (low <= high) {
            const mid = low + Math.floor((high - low) / 2);
            if (arr[mid] >= target) {
                ans = mid;       // Candidate index
                high = mid - 1;  // Try to find smaller index
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }
    ```
3.  **Floor & Ceil in Sorted Array:**
    *   **Floor:** Target se chota ya uske barabar sabse bada element (`arr[mid] <= target`).
    *   **Ceil:** Target se bada ya uske barabar sabse chota element (`arr[mid] >= target`). Ceil is equivalent to Lower Bound!

---

## 5. REVOLUTIONARY JUMPS: ROTATED SORTED ARRAY SEARCH

Bacho, LeetCode ka ek aur standard problem jo interviews mein kafi tezi se pucha jata hai: **Rotated Sorted Array**.

```
              Sorted Array:         [ 1, 2, 4, 5, 6, 7 ]
              Rotated at index 3:   [ 5, 6, 7, 1, 2, 4 ]
```

### Key Observation: One Half is ALWAYS Sorted 💡
Pure rotated array sorted nahi hai, par dhyan se dekho! Agar hum is array ko kisi bhi point se break karein, toh **kam se kam ek side ka segment hamesha perfectly sorted hoga**.
*   Agar `arr[low] <= arr[mid]`, toh left side sorted hai.
*   Agar `arr[mid] <= arr[high]`, toh right side sorted hai.

Hum pehle sorted half ko identify karenge, phir check karenge ki kya humara target us sorted range ke limits mein lie karta hai. Agar haan, toh us side binary search lagayenge; nahi toh search segment ko toggle kar denge!

---

### Key Problem: Search in Rotated Sorted Array (LeetCode 33)

#### 1. Understand:
Sorted rotated array mein `target` element ka index dhoondhna hai. **Constraint:** Time complexity strictly **O(log n)** honi chahiye.

#### 2. JavaScript Code:
```javascript
function searchRotated(arr, target) {
    let low = 0;
    let high = arr.length - 1;

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);

        if (arr[mid] === target) return mid;

        // Check if the left half is sorted
        if (arr[low] <= arr[mid]) {
            // Check if target lies within the sorted left boundary
            if (target >= arr[low] && target < arr[mid]) {
                high = mid - 1; // Search left
            } else {
                low = mid + 1;  // Search right
            }
        } 
        // Otherwise, the right half must be sorted
        else {
            // Check if target lies within the sorted right boundary
            if (target > arr[mid] && target <= arr[high]) {
                low = mid + 1;  // Search right
            } else {
                high = mid - 1; // Search left
            }
        }
    }
    return -1;
}
```

#### 3. Complete Dry Run on `arr =`, `target = 1`:
*   `low = 0`, `high = 5`.
*   **Step 1:** `mid = 0 + Math.floor((5 - 0) / 2) = 2`.
    *   `arr = 7`. Since `7 !== 1` → Check sorting.
    *   `arr[low] = arr = 5 <= arr[mid] = 7`. Left half is indeed sorted!
    *   Is target `1` in range `[5, 7)`? No (`1 < 5`).
    *   So search right: `low = mid + 1 = 3`.
*   **Step 2:** `mid = 3 + Math.floor((5 - 3) / 2) = 4`.
    *   `arr = 2`.
    *   Is `arr[low] = arr = 1 <= arr[mid] = 2`? Left half is sorted.
    *   Is target `1` in range `[1, 2)`? Yes (`1 === 1`).
    *   Search left: `high = mid - 1 = 3`.
*   **Step 3:** `mid = 3 + Math.floor((3 - 3) / 2) = 3`.
    *   `arr = 1`. Match found! Returns index `3` (Correct!).

#### 4. Complexity:
Time: **O(log n)**, Space: **O(1)**.

---

### Rotated Sorted Array with Duplicates Variant (LeetCode 81)
*   **The Trap ⚠️:** Agar duplicates present hon (jaise `arr =`, `target = 0`), toh starting element, middle element, aur ending element teeno barabar ho sakte hain (`arr[low] === arr[mid] === arr[high]`). Is specific corner edge case par hum decide hi nahi kar sakte ki kaunsa half sorted hai!
*   **The Fix:** Agar yeh condition hit ho, toh low aur high pointers ko shrink karke range choti karo, jab tak inequality break na ho jaye:
    ```javascript
    if (arr[low] === arr[mid] && arr[mid] === arr[high]) {
        low++;
        high--;
        continue;
    }
    ```
    *   **Worst Case Complexity:** If all elements are duplicates, this shrinks down to **O(n)** linear checks.

---

## 6. EXPANDING HORIZONS: PEAK ELEMENT & MOUNTAIN ARRAY SEARCH

Bacho, is pattern ko dhyan se samajhna kyunki isme array fully sorted nahi hota, fir bhi hum Binary Search apply kar sakte hain!

```
                       Mountain / Peak Pattern:
                       
                                  8 (Peak Element)
                                 / \
                                5   6
                               /     \
                              3       2
```

### Identification Rule: Monotonic Slopes ⛰️
Peak element woh hota hai jo apne left aur right dono padosi (neighbors) se bada ho. 
*   Agar `arr[mid] < arr[mid + 1]`, iska matlab hum mountain ke **ascending slope (upward incline)** par hain. Peak element hamesha right side hi hoga → `low = mid + 1`.
*   Agar `arr[mid] > arr[mid + 1]`, iska matlab hum mountain ke **descending slope (downward incline)** par hain. `mid` khud peak ho sakta hai ya peak left side par hoga → `high = mid`.

---

### Key Problem: Find Peak Element (LeetCode 162)

#### JavaScript Code:
```javascript
function findPeakElement(arr) {
    let low = 0;
    let high = arr.length - 1;

    while (low < high) { // Strictly < because we compare mid with mid+1
        const mid = low + Math.floor((high - low) / 2);
        
        if (arr[mid] < arr[mid + 1]) {
            low = mid + 1; // Climb up the slope to the right
        } else {
            high = mid;    // Peak is on the left or is mid itself
        }
    }
    return low; // or high, both converge to peak element index
}
```
*   **Complexity:** Time: **O(log n)**, Space: **O(1)**.

---

## 7. SEARCHING IN 2D SPACE: MATRIX SEARCH PATTERNS

Matrices aur 2D array coordinates (Chapter 5) ko search space ki tarah use karne ke do badhiya patterns hote hain:

### Pattern A: Perfectly Sorted Flat Matrix (Virtual 1D Array)
*   **Constraint:** Har row sorted hai, aur har row ka first element pichli row ke last element se bada hai.
*   **Strategy:** Pure matrix ko ek virtual continuous 1D sorted array ki tarah treat karo. 1D virtual index `mid` ko real coordinates mein transform karne ka formula:
    \\[row = ⌊ mid/cols ⌋\\]
    \\[col = mid \pmod{cols}\\]
*   **Complexity:** Time: **O(log(R × C))**, Space: **O(1)**.

---

### Pattern B: Row-Column Sorted Matrix (Staircase Search)
*   **Constraint:** Har row aur har column independently sorted hain, par contiguous sequence continuous nahi hai.
*   **Strategy:** Start from **Top-Right Corner** `(0, cols - 1)`.
    *   Agar current element target se bada hai, iska matlab target column left side par hi bachega (kyunki current column ke elements toh sorted hain, niche toh aur bade milenge). Hum column decrement karenge: `col--`.
    *   Agar current element target se chota hai, hum row increment karenge: `row++`.

```javascript
function searchMatrixII(matrix, target) {
    if (matrix.length === 0) return false;
    let R = matrix.length;
    let C = matrix.length;

    let row = 0;
    let col = C - 1; // Top-right corner

    while (row < R && col >= 0) {
        if (matrix[row][col] === target) {
            return true; // Target matched
        } else if (matrix[row][col] > target) {
            col--; // Move leftward
        } else {
            row++; // Move downward
        }
    }
    return false;
}
```
*   **Complexity:** Time: **O(R + C)** linear time, Space: **O(1)**.

---

## 8. MASTERCLASS: BINARY SEARCH ON ANSWER (THE ULTIMATE PATTERN)

Bacho, ab tak hum sorted array mein target index dhoondh rahe the. Lekin real-world optimization problems mein target diya nahi hota! 

Humein ek specific condition ke mutabik **"minimum possible value"** ya **"maximum possible value"** nikaalni hoti hai. Isi standard pattern ko hum **Binary Search on Answer** kehte hain.

```
                     BINARY SEARCH ON ANSWER BLUEPRINT
                                     │
         ┌───────────────────────────┴───────────────────────────┐
  Pillar 1: Define Answer Space                          Pillar 2: Feasibility Function
  Determine exact bounds:                                boolean isFeasible(candidate)
  low = min_possible, high = max_possible                 Checks if candidate satisfies constraints.
                                     │
                                     ▼
                        Monotonicity Condition:
                        Is there a clear break point?
                        Yes: [ Valid, Valid, Valid, Invalid, Invalid ]
                        Use Binary Search to converge on boundary!
```

---

### normal Binary Search vs. Binary Search on Answer ⚔️

| Feature | Normal Binary Search | Binary Search on Answer |
| :--- | :--- | :--- |
| **Search Range** | Array ke low-high indices range `[0, N-1]`. | Target Answer ki values space range `[MinPossible, MaxPossible]`. |
| **Target Check** | `arr[mid] === target` match check. | `isFeasible(mid)` helper function check. |
| **Data Depend** | Array values directly index sequential check. | Physical constraints of real-world variables. |

---

### Step-by-Step Diagnostic Check: BS on Answer kab lagti hai?
1.  **Multiple choices with monotonic boundaries:** Problem mein ek range of possible values exist karti hai. Agar value `X` feasible hai, toh kya `X + 1` ya `X - 1` bhi monotonic pattern follow karegi?
2.  **Minimize Maximum / Maximize Minimum clue:** SDE interviews ka sabse bada sign! Agar question mein *"minimize the maximum cost"* ya *"maximize the minimum speed"* likha ho, toh 99% Binary Search on Answer hi lagta hai.
3.  **Feasibility Validation O(n) check helper function:** Humein ek standard checker function `isFeasible(val)` design karna padega jo sequential array loop scan karke linear time mein batata hai ki kya target constraint valid hai ya nahi.

---

### Key Problem: Koko Eating Bananas (LeetCode 875)

#### 1. Understand:
Koko ke paas `N` piles of bananas hain, jahan `piles[i]` pile `i` ka banana count hai. Guard `H` ghante (hours) ke baad aayenge. Koko ko banana khane ki ek constant hourly speed `K` select karni hai. Agar current speed `K` hai, aur pile ka size `P` hai, toh Koko us pile ko khane mein `Math.ceil(P / K)` hours legi. Koko ko **minimum eating speed K** dhoondhni hai taaki woh saare bananas guard ke aane se pehle (`<= H` hours mein) kha sake.

#### 2. Example:
`piles =`, `H = 8`
*   If `K = 4`: Hours needed = `Math.ceil(3/4) + Math.ceil(6/4) + Math.ceil(7/4) + Math.ceil(11/4) = 1 + 2 + 2 + 3 = 8` hours. This is valid!
*   If `K = 3`: Hours needed = `1 + 2 + 3 + 4 = 10` hours (which is `> 8`, invalid!).
*   **Result:** Minimum speed is `4`.

---

#### 3. Brute Force:
`K = 1` se start karke sequential iteration check chalao jab tak total hours `<= H` na ho jaye.
*   **Complexity:** Time: **O(max(pile) × N)**, leading to TLE.

#### 4. Bottleneck:
Max speed range bohot badi ho sakti hai (up to 10^9). Linear checks are too slow.

#### 5. Search Space Identification ⛰️:
*   *Minimum Possible Speed (`low`):* `1` (Koko kam se kam 1 banana per hour toh khayegi hi).
*   *Maximum Possible Speed (`high`):* `Math.max(...piles)` (Piles ke max size se zyada speed ka koi matlab nahi hai, kyunki ek hour mein ek hi pile kha sakti hai).
*   Search Space bounds: `` for our example.

---

#### 6. Feasibility Function (`isFeasible`):
Maan lo humari speed `speed` hai. Total hours calculate karne ka code linear pass mein run karenge:
```javascript
function canEatAll(piles, H, speed) {
    let hoursSpent = 0;
    for (let pile of piles) {
        hoursSpent += Math.ceil(pile / speed);
    }
    return hoursSpent <= H;
}
```

---

#### 7. JavaScript Code:
```javascript
function minEatingSpeed(piles, h) {
    let low = 1;
    let high = Math.max(...piles); // Max range element as upper bound
    let result = high;

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);

        // If Koko can eat all bananas with 'mid' speed, try to minimize speed
        if (canEatAll(piles, h, mid)) {
            result = mid;     // Save current valid speed
            high = mid - 1;   // Search left for even smaller speed!
        } else {
            low = mid + 1;    // Speed too slow, increase speed!
        }
    }
    return result;
}

function canEatAll(piles, h, speed) {
    let totalHours = 0;
    for (let pile of piles) {
        totalHours += Math.ceil(pile / speed);
    }
    return totalHours <= h;
}
```

#### 8. Complete Dry Run on `piles =`, `h = 8`:
*   `low = 1`, `high = 11`.
*   **Step 1:** `mid = 1 + Math.floor((11 - 1) / 2) = 6`.
    *   `canEatAll(piles, 8, 6)`: Hours = `Math.ceil(3/6) + Math.ceil(6/6) + Math.ceil(7/6) + Math.ceil(11/6) = 1 + 1 + 2 + 2 = 6` hours.
    *   Since `6 <= 8`, speed `6` is valid!
    *   `result = 6`, search left: `high = 6 - 1 = 5`.
*   **Step 2:** `mid = 1 + Math.floor((5 - 1) / 2) = 3`.
    *   `canEatAll(piles, 8, 3)`: Hours = `1 + 2 + 3 + 4 = 10` hours.
    *   Since `10 > 8`, speed is too slow!
    *   Search right: `low = 3 + 1 = 4`.
*   **Step 3:** `mid = 4 + Math.floor((5 - 4) / 2) = 4`.
    *   `canEatAll(piles, 8, 4)`: Hours = `1 + 2 + 2 + 3 = 8` hours.
    *   Since `8 <= 8`, speed `4` is valid!
    *   `result = 4`, search left: `high = 4 - 1 = 3`.
*   **Converged!** Loop breaks as `low > high` (`4 > 3`). Returns `4` (Correct!).

#### 9. Complexity:
*   **Time Complexity:** **\\(O(N log(max(pile)))\\)**.
*   **Space Complexity:** **O(1)** auxiliary space.

---

## 9. PRACTICE CORNER (EASY → MEDIUM → HARD)

🚀 **Chalo bacho, marker whiteboard par hai! Ab hum progressive practice problems ko solve karenge. Pehle search space identify karo, fir answer dekhna!**

---

### Problem 1 (Easy): Search Insert Position (LeetCode 35)
*Sorted array aur target value di hai. Target ka index return karo agar target present hai. Agar target absent hai, toh return karo woh index jahan target ko sorted order preserve karte hue insert kiya ja sake.*

#### 🧠 Diagnostics:
*   *Search Space:* Indices range `[0, arr.length - 1]`.
*   *Intuition:* Beta, dhyan se dekho. Search insert position aur kuch nahi balki **Lower Bound (First element >= target)** hai! Agar target present hai, toh uski position lower bound hogi. Agar target absent hai, toh jahan check pointer break hota hai, wahi insertion space coordinate hota hai.

```javascript
function searchInsert(arr, target) {
    let low = 0;
    let high = arr.length - 1;
    let ans = arr.length; // Default to tail insertion

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);
        if (arr[mid] >= target) {
            ans = mid;
            high = mid - 1; // Look for earlier insertion indexes
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```
*   **Complexity:** Time: **O(log n)**, Space: **O(1)**.

---

### Problem 2 (Medium): Find Minimum in Rotated Sorted Array (LeetCode 153)
*Rotated sorted array diya hai, iska absolute minimum element dhoondh kar return karo.*

#### 🧠 Diagnostics:
*   *Search Space:* Indices range `[0, arr.length - 1]`.
*   *Observation:* Agar `arr[low] <= arr[high]`, iska matlab hum sorted segment par hain. Wahan range ka sabse pehla element `arr[low]` hi minimum hoga.
*   *Decision:* Agar current mid range sorted nahi hai, toh hum sorted half check karenge.
    *   If `arr[low] <= arr[mid]`, left half is sorted, hum left minimum record karenge aur right side move karenge: `low = mid + 1`.
    *   Else, right half is sorted, mid element can be the minimum, move leftward: `high = mid`.

```javascript
function findMin(arr) {
    let low = 0;
    let high = arr.length - 1;

    while (low < high) {
        // If range is already sorted
        if (arr[low] < arr[high]) {
            return arr[low];
        }

        const mid = low + Math.floor((high - low) / 2);

        // If mid is greater than high element, minimum must lie in the right half
        if (arr[mid] > arr[high]) {
            low = mid + 1;
        } else {
            high = mid; // mid could be the minimum, shrink range leftward
        }
    }
    return arr[low];
}
```
*   **Complexity:** Time: **O(log n)**, Space: **O(1)**.

---

### Problem 3 (Hard): Split Array Largest Sum / Book Allocation (LeetCode 410)
*Non-negative integers ka array `nums` aur ek integer `m` diya hai. Humein array ko `m` non-empty contiguous subarrays mein split karna hai. Subarrays ke individual elements ke sums ka jo maximum value hoga, humein use **minimize** karna hai.*

#### 🧠 Diagnostics:
*   *Is it BS on Answer?* Yes! *"Minimize the maximum sum"* is the clear clue.
*   *Identify the Search Space on Answer:*
    *   *Min possible sum (`low`):* `Math.max(...nums)` (Kyunki kam se kam ek subarray pure split element ko capture karega, isiliye split sum is value se chota nahi ho sakta).
    *   *Max possible sum (`high`):* `nums.reduce((a, b) => a + b, 0)` (Pure array ka single split run sum).
*   *Feasibility Checker:* Check karega ki kya current maximum sum limit ke sath array ko `m` arrays mein split kiya ja sakta hai ya nahi:

```javascript
function isSplitFeasible(nums, m, maxSumLimit) {
    let currentSum = 0;
    let subarrayCount = 1;

    for (let num of nums) {
        if (currentSum + num > maxSumLimit) {
            subarrayCount++;
            currentSum = num;
            if (subarrayCount > m) return false; // Exceeds split budget
        } else {
            currentSum += num;
        }
    }
    return true;
}

function splitArray(nums, m) {
    let low = Math.max(...nums);
    let high = nums.reduce((acc, curr) => acc + curr, 0);
    let result = high;

    while (low <= high) {
        const mid = low + Math.floor((high - low) / 2);

        if (isSplitFeasible(nums, m, mid)) {
            result = mid;     // Save current valid split limit
            high = mid - 1;   // Try to minimize even further!
        } else {
            low = mid + 1;    // Max sum limit too tight, increase threshold!
        }
    }
    return result;
}
```
*   **Complexity:** Time: **O(N log(Sum - Max))**, Space: **O(1)**.

---

## 10. COMMON MISTAKES & INTERVIEW TRAPS ⚠️

Bacho, interviews mein in bugs ko door rakhna:

1.  **Running BS on Unsorted Data:**
    Always check: Kya array monotonic/sorted hai? Agar nahi, toh sorting bypass kiye bina traditional binary search apply mat karna!
2.  **Infinite Loops due to incorrect Pointer Updates:**
    Using `low = mid` or `high = mid` in a `low <= high` loop. This can trap the pointers if `mid` converges near bounds, causing an infinite loop. Always ensure either range shrinks (`mid + 1` or `mid - 1`) or use a strict `low < high` termination check.
3.  **Wrong `low/high` Initialization in BS on Answer:**
    Setting `low = 0` when the minimum possible feasibility starts from `Math.max(...arr)`. This wastes lookup steps and creates edge cases.
4.  **Reference Aliasing / Array Mutation:**
    Modifying the search parameters during helper operations which corrupts original variables state.

---

## CHAPTER END SUMMARY

### Completed Topics:
*   Horizontal Linear search bounds and average comparisons.
*   Monotonic sorted array search structures.
*   Safe arithmetic calculation of `mid`.
*   Boundary logic patterns of Lower/Upper boundaries.
*   Rotated sorted segment checks and duplication fixes.
*   Binary Search on Answer optimization framework.

### Mastered Patterns:
*   **Converging low-high pointers** to shrink index range by half each turn.
*   **Complement tracking Set comparisons** replaced with constant bounds lookup.
*   **isFeasible helper scanning** to search over dynamic answer spaces.

---

### Practice Roadmap:
1.  Solve *Search Insert Position* on LeetCode 35.
2.  Solve *Search in Rotated Sorted Array* (LeetCode 33).
3.  Complete *Koko Eating Bananas* (LeetCode 875) to master BS on Answer.

---
