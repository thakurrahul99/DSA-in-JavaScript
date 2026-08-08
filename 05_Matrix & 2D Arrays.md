**Namaste bacho! Aa jao class mein, aur marker par dhyan do.**

Pichle chapters mein humne linear arrays aur unke patterns ko dhang se seekh liya. Lekin real life mein, aur software engineering ke complex problems mein, data hamesha ek single straight line mein nahi hota.

Jaise chess board ka grid, computer screen ke pixels, ya Google Sheets ki spreadsheet—ye sab elements horizontal aur vertical coordinates dono mein spread hote hain. Aaj hum isi 2D space ko master karenge aur dhyan se samjhengay **Matrix & 2D Arrays in JavaScript**!

---

## 1. 2D ARRAY BASICS (GRID KA MENTAL MODEL)

### What is a 2D Array?
**Sabse pehle dimaag mein ek simple picture banao.** Ek standard 1D array linear sequence hota hai. Lekin ek **2D Array** aur kuch nahi, balki **an array of arrays** hai! Yani ek bada dibba, jiske andar har element apne aap mein ek aur array hai.

```
                 MATRIX REPRESENTATION (3 x 4 Grid)
                 
                 Column 0   Column 1   Column 2   Column 3
               ┌──────────┬──────────┬──────────┬──────────┐
      Row 0 ──►│    │   │   │   │
               ├──────────┼──────────┼──────────┼──────────┤
      Row 1 ──►│   │   │   │   │
               ├──────────┼──────────┼──────────┼──────────┤
      Row 2 ──►│   │   │   │   │
               └──────────┴──────────┴──────────┴──────────┘
```

* **Rows (Sote hue/Horizontal lines):** Isme indices upar se neeche (`0` se `rows - 1`) badhte hain.
* **Columns (Khade hue/Vertical lines):** Isme indices left se right (`0` se `cols - 1`) badhte hain.

### Creating a Matrix in JavaScript
JavaScript mein lower-level contiguous 2D memory blocks native nahi hote. Hum sub-arrays nesting use karte hain.

#### 1. Direct Initialization (Manual):
```javascript
const matrix = [
   ,
   ,
   
];
```

#### 2. Dynamic Initialization (V8 Engine-Friendly):
Agar humein dynamic sizes (`rows` aur `cols`) ka empty matrix banana ho, toh kabhi bhi `new Array(rows).fill(new Array(cols))` mat karna! 
* *Why?* Kyunki `fill()` same sub-array ka reference har row mein copy kar deta hai. Agar tum ek element change karoge, toh poori column change ho jayegi!
* *Correct Way:*
```javascript
const rows = 3;
const cols = 4;
// Create an array of size 'rows', then map each slot to a brand new array of size 'cols'
const dynamicMatrix = Array.from({ length: rows }, () => new Array(cols).fill(0));
```

### Accessing & Updating `matrix[row][column]`
Access karne ke liye, pehle coordinate row index ka select hota hai aur dusra col index ka:
* `matrix[r]` se poori row (jo ki ek array hai) fetch hoti hai.
* `matrix[r][c]` se us row ke andar ka specific element fetch hota hai.

```javascript
let val = matrix; // Row index 1, Column index 2 element
matrix = 99;      // Row index 2, Column index 0 ko update kiya
```

### Dimensions: Rectangular vs Square Matrix
* **Square Matrix:** Jiske rows aur columns barabar hon (`rows === cols`). Jaise \\(3 \times 3\\) matrix.
* **Rectangular Matrix:** Jiske rows aur columns barabar nahi hote (`rows !== cols`). Jaise \\(3 \times 4\\) matrix.
  * **Rows count:** `matrix.length`
  * **Columns count:** `matrix.length` (Safe check: `matrix.length > 0 ? matrix.length : 0`)

---

## 2. MATRIX TRAVERSAL (CLASSROOM WHITEBOARD)

Traversing ka matlab hai grid ke har coordinate par traverse karna. Chalo dono major traversal strategies ko dhang se dry-run karte hain.

```
      Row-Wise Traversal (Classic)            Column-Wise Traversal
      ┌─────────────────────────┐             ┌─────────────────────────┐
      │  ──►   ──►   ──►   ──►  │             │  │   │   │   │  ▲   │   │ │
      │  ──►   ──►   ──►   ──►  │             │  ▼   ▼   ▼   ▼  │   ▼   ▼ │
      └─────────────────────────┘             └─────────────────────────┘
```

### A. Row-Wise Traversal
Isme hum pehle row select karte hain, aur phir us row ke saare columns ko print karte hain, aur phir agli row par jump karte hain.

```javascript
function traverseRowWise(matrix) {
    const rows = matrix.length;
    const cols = matrix.length;
    
    for (let i = 0; i < rows; i++) { // Outer loop: Row controller
        for (let j = 0; j < cols; j++) { // Inner loop: Column controller
            console.log(matrix[i][j]);
        }
    }
}
```

#### Dry Run on `matrix = [,]`:
* `rows = 2`, `cols = 2`
* **`i = 0` (Row 0 selected):**
  * `j = 0`: prints `matrix` = `10`
  * `j = 1`: prints `matrix` = `20`
* **`i = 1` (Row 1 selected):**
  * `j = 0`: prints `matrix` = `30`
  * `j = 1`: prints `matrix` = `40`
* **Complexity:** Time: **\\(O(rows \times cols)\\)**, Space: **\\(O(1)\\)**.

---

### B. Column-Wise Traversal
Isme outer index column control karega aur inner index row ko control karega!

```javascript
function traverseColWise(matrix) {
    const rows = matrix.length;
    const cols = matrix.length;
    
    for (let j = 0; j < cols; j++) { // Outer loop: Column controller
        for (let i = 0; i < rows; i++) { // Inner loop: Row controller
            console.log(matrix[i][j]);
        }
    }
}
```

#### Dry Run on same `matrix = [,]`:
* **`j = 0` (Col 0 selected):**
  * `i = 0`: prints `matrix` = `10`
  * `i = 1`: prints `matrix` = `30`
* **`j = 1` (Col 1 selected):**
  * `i = 0`: prints `matrix` = `20`
  * `i = 1`: prints `matrix` = `40`
* **Complexity:** Time: **\\(O(rows \times cols)\\)**, Space: **\\(O(1)\\)**.

---

## 3. BASIC MATRIX OPERATIONS

### A. Matrix Sum, Row-Sums & Col-Sums
```javascript
function matrixOperations(matrix) {
    const rows = matrix.length;
    const cols = matrix.length;
    let totalSum = 0;
    
    const rowSums = new Array(rows).fill(0);
    const colSums = new Array(cols).fill(0);
    
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            const val = matrix[i][j];
            totalSum += val;
            rowSums[i] += val;
            colSums[j] += val;
        }
    }
    return { totalSum, rowSums, colSums };
}
```

---

### B. Diagonal Traversals (Only for Square Matrices)

Square matrix mein do major diagonals hote hain:
1. **Main Diagonal (Primary):** Top-left se bottom-right.
2. **Secondary Diagonal:** Top-right se bottom-left.

```
                   PRIMARY & SECONDARY DIAGONALS
                   
                   j=0      j=1      j=2
                 ┌────────┬────────┬────────┐
            i=0  │  Main  │        │  Sec   │   Index conditions:
                 ├────────┼────────┼────────┤
            i=1  │        │ Both ! │        │   Main: i === j
                 ├────────┼────────┼────────┤
            i=2  │  Sec   │        │  Main  │   Sec:  i + j === n - 1
                 └────────┴────────┴────────┘
```

#### Primary Diagonal Condition:
Har element diagonal line par `row index === column index` hota hai (`i === j`).
* Naive approach: double loop laga kar check karo `if (i === j)`. Complexity: \\(O(n^2)\\).
* **Optimal approach (Single loop):**
```javascript
function printPrimaryDiagonal(matrix) {
    const n = matrix.length;
    for (let i = 0; i < n; i++) {
        console.log(matrix[i][i]); // O(n) time! No nested loop!
    }
}
```

#### Secondary Diagonal Condition:
Is line par indices ka sum hamesha boundary range size minus one hota hai: `i + j === n - 1` yaani **`j === n - 1 - i`**.
* **Optimal approach (Single loop):**
```javascript
function printSecondaryDiagonal(matrix) {
    const n = matrix.length;
    for (let i = 0; i < n; i++) {
        console.log(matrix[i][n - 1 - i]); // O(n) time!
    }
}
```

---

## 4. TRANSPOSE OF A MATRIX

### What is Transpose? (Paltana)
**Rows ko Columns bana do, aur Columns ko Rows bana do!**
Yani coordinate `[i][j]` transpose hone ke baad coordinate `[j][i]` ban jata hai.

```
         Original (3x2 Rectangular)               Transposed (2x3 Matrix)
              ┌────────┬────────┐                     ┌───┬───┬───┐
              │   1    │   2    │                     │ 1 │ 3 │ 5 │
              ├────────┼────────┤                     ├───┼───┼───┤
              │   3    │   4    │   ─────────────►    │ 2 │ 4 │ 6 │
              ├────────┼────────┤                     └───┴───┴───┘
              │   5    │   6    │
              └────────┴────────┘
```

### A. Square Matrix In-Place Transpose (No extra space)
Square matrix ko transpose karte waqt, hum elements ko directly swap kar sakte hain, par dhyan rahe: **Humein sirf half diagonal swap karna hai (`j > i` area).** Agar pure loops check karenge, toh element do baar swap hokar vapas original position par aa jayega!

```javascript
function transposeSquare(matrix) {
    const n = matrix.length;
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) { // j starts from i + 1 (swapping above diagonal)
            let temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    return matrix;
}
```
* **Complexity:** Time Complexity: **\\(O(n^2)\\)**, Space Complexity: **\\(O(1)\\)** auxiliary space.

### B. Rectangular Matrix Transpose (New Matrix Allocation)
Rectangular matrix mein hum directly swap nahi kar sakte kyunki rows and cols size different hote hain. Humein ek naya matrix of size `cols x rows` allocate karna padega.

```javascript
function transposeRectangular(matrix) {
    const rows = matrix.length;
    const cols = matrix.length;
    
    // Create col x rows empty matrix
    const result = Array.from({ length: cols }, () => new Array(rows));
    
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            result[j][i] = matrix[i][j];
        }
    }
    return result;
}
```
* **Complexity:** Time: **\\(O(rows \times cols)\\)**, Space: **\\(O(rows \times cols)\\)** for result matrix.

---

## 5. MATRIX TRANSFORMATIONS: ROTATE 90 DEGREES

### Rotations (SDE Interview Favourite)
**Problem:** Ek square matrix ko 90 degrees clockwise direction mein rotate karna hai (in-place).

```
                      Original               Rotated 90°
                     ┌───┬───┬───┐          ┌───┬───┬───┐
                     │ 1 │ 2 │ 3 │          │ 7 │ 4 │ 1 │
                     ├───┼───┼───┤   ──►    ├───┼───┼───┤
                     │ 4 │ 5 │ 6 │          │ 8 │ 5 │ 2 │
                     ├───┼───┼───┤          ├───┼───┼───┤
                     │ 7 │ 8 │ 9 │          │ 9 │ 6 │ 3 │
                     └───┴───┴───┘          └───┴───┴───┘
```

#### Brute Force Strategy:
Naya matrix allocate karo aur direct indices map karo: `result[j][n - 1 - i] = matrix[i][j]`. Space takes \\(O(n^2)\\).

#### The Optimal Matrix Trick: (Transpose + Reverse)
*Dekho dosto, isme ek gajab ki symmetry hai!*
1. **Pehle matrix ko Transpose karo (In-place swap):**
   `` \\(\longrightarrow\\) `` (Isse humare rows diagonal symmetric reflect ho gaye).
2. **Ab Matrix ki har row ko reverse karo:**
   Row 0: `` becomes ``.
   Row 1: `` becomes ``.
   Row 2: `` becomes ``.
*Boom! Matrix bina kisi extra matrix space ke 90° clockwise rotate ho gaya!*

```javascript
function rotate90Clockwise(matrix) {
    const n = matrix.length;
    
    // Step 1: Transpose in-place
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            let temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    
    // Step 2: Reverse each row
    for (let i = 0; i < n; i++) {
        matrix[i].reverse(); // manual row reversal in-place
    }
    
    return matrix;
}
```
* **Complexity:** Time Complexity: **\\(O(n^2)\\)**, Space Complexity: **\\(O(1)\\)** auxiliary space.

---

## 6. SPIRAL TRAVERSAL (THE BOUNDARY SHRINKING PATTERN)

**Spiral Traversal matrix coding questions ka sabse popular pattern hai.**

```
                      1  ──►  2  ──►  3
                                      │
                      8  ──►  9       4
                      ▲               │
                      7  ◄──  6  ◄──  5
```

### The Boundary Approach (Pointers Strategy)
Hum char variables ke zariye boundary areas lock karte hain aur spiral movement ke mutabik boundary range shrink karte hain:
1. `top = 0`
2. `bottom = rows - 1`
3. `left = 0`
4. `right = cols - 1`

#### Traversal Algorithm:
Hum loop tab tak chalate hain jab tak boundary pointers overlaps na karein (`top <= bottom && left <= right`):
1. **Left to Right (Top row constant):** Traverse `left` to `right` pointer. Increment `top++`.
2. **Top to Bottom (Right column constant):** Traverse `top` to `bottom`. Decrement `right--`.
3. **Right to Left (Bottom row constant):** Check `top <= bottom` first (safety check), traverse `right` to `left`. Decrement `bottom--`.
4. **Bottom to Top (Left column constant):** Check `left <= right` first (safety check), traverse `bottom` to `top`. Increment `left++`.

```javascript
function spiralOrder(matrix) {
    const result = [];
    if (matrix.length === 0) return result;
    
    let top = 0;
    let bottom = matrix.length - 1;
    let left = 0;
    let right = matrix.length - 1;
    
    while (top <= bottom && left <= right) {
        // 1. Left to Right
        for (let i = left; i <= right; i++) {
            result.push(matrix[top][i]);
        }
        top++;
        
        // 2. Top to Bottom
        for (let i = top; i <= bottom; i++) {
            result.push(matrix[i][right]);
        }
        right--;
        
        // 3. Right to Left (Safety check to avoid redundant prints)
        if (top <= bottom) {
            for (let i = right; i >= left; i--) {
                result.push(matrix[bottom][i]);
            }
            bottom--;
        }
        
        // 4. Bottom to Top (Safety check)
        if (left <= right) {
            for (let i = bottom; i >= top; i--) {
                result.push(matrix[i][left]);
            }
            left++;
        }
    }
    return result;
}
```
* **Complexity:** Time Complexity: **\\(O(rows \times cols)\\)** (every single index visited once), Space Complexity: **\\(O(1)\\)** auxiliary space (excluding final result list storage).

---

## 7. MATRIX SEARCH (LOOKUP OPTIMIZATIONS)

Matrix search queries ke algorithms is baat par depend karte hain ki matrix kaisa sorted hai.

### Case A: Search in a row-wise and column-wise Sorted Matrix (Search in 2D Matrix II)
Matrix values rows aur columns dono mein sorted hain ascending orders mein.
```
                  [ 10,  20,  30,  40 ]
                  [ 15,  25,  35,  45 ]
                  [ 27,  29,  37,  48 ]
```

#### The Staircase Search Logic:
*Hum isme binary search ki tarah range dynamic reduction lagayenge.*
* **Start Corner Selection:** Hum top-right corner `(row = 0, col = cols - 1)` se shuru karte hain.
* **Movement Rule:**
  * Agar `matrix[row][col] === target`, mil gaya!
  * Agar `matrix[row][col] > target`, iska matlab is column ki values target se badi hain. Decrement `col--` (move left).
  * Agar `matrix[row][col] < target`, iska matlab is row ki values target se choti hain. Increment `row++` (move down).

```javascript
function searchMatrixSorted(matrix, target) {
    if (matrix.length === 0) return false;
    
    let row = 0;
    let col = matrix.length - 1; // Start at Top-Right
    
    while (row < matrix.length && col >= 0) {
        if (matrix[row][col] === target) {
            return true;
        } else if (matrix[row][col] > target) {
            col--; // Move Left
        } else {
            row++; // Move Down
        }
    }
    return false;
}
```
* **Complexity Comparison:**
  * *Brute Force (Linear Scan):* Time \\(O(rows \times cols)\\).
  * *Staircase Search (Sorted):* Time Complexity **\\(O(rows + cols)\\)**. Space Complexity: **\\(O(1)\\)**.

---

## 8. 2D PREFIX SUM PATTERN (INTEGRAL IMAGING)

**1D prefix sum range queries ko fast karta hai. Par matrix grids mein sub-rectangle range queries ko \\(O(1)\\) kaise karein?**

### The Sub-Rectangle Sum Query Problem:
Manlo grid size bada hai aur interviewer tumse poochta hai: *"Index (r1, c1) se (r2, c2) rectangle boundaries ke bache sub-matrix ka sum kya hai?"*

```
                  (0,0) ──────────► Columns (j)
                    │   ┌───┬───┬───┬───┐
                    │   │   │   │   │   │
                    ▼   ├───┼───┼───┼───┤
                 Rows   │   │(r1,c1)│   │  ◄── sub-rectangle
                  (i)   ├───┼───┼───┼───┤
                        │   │   │(r2,c2)│
                        └───┴───┴───┴───┘
```

### Precalculating 2D Prefix Matrix:
Hum ek `prefix[i][j]` matrix design karenge, jahan har index `(i,j)` top-left starting corner `(0,0)` se lekar coordinate `(i,j)` tak ke rectangular boundaries ke values ka cumulative sum save rakhta hai.

\\[\text{prefix}[i][j] = \text{matrix}[i][j] + \text{prefix}[i-1][j] + \text{prefix}[i][j-1] - \text{prefix}[i-1][j-1]\\]
* *Intuition:* Dono left and upper coordinates ka cumulative sums bounds add karte waqt diagonal overlap index segment `prefix[i-1][j-1]` do baar add ho jata hai, isiliye use subtract karna padta hai.

```javascript
class NumMatrix {
    constructor(matrix) {
        if (matrix.length === 0 || matrix.length === 0) return;
        const r = matrix.length;
        const c = matrix.length;
        
        // 1-padded prefix matrix to handle negative offset indices elegantly
        this.prefix = Array.from({ length: r + 1 }, () => new Array(c + 1).fill(0));
        
        for (let i = 1; i <= r; i++) {
            for (let j = 1; j <= c; j++) {
                this.prefix[i][j] = matrix[i - 1][j - 1] 
                                  + this.prefix[i - 1][j] 
                                  + this.prefix[i][j - 1] 
                                  - this.prefix[i - 1][j - 1];
            }
        }
    }
    
    // Constant time range sum query O(1) Time!
    sumRegion(r1, c1, r2, c2) {
        // Shift indexing according to 1-padding matrix allocations
        r1++; c1++; r2++; c2++;
        return this.prefix[r2][c2] 
             - this.prefix[r1 - 1][c2] 
             - this.prefix[r2][c1 - 1] 
             + this.prefix[r1 - 1][c1 - 1];
    }
}
```
* **Complexity:** Matrix setup preprocessing takes **\\(O(rows \times cols)\\)**, but once created, **any sub-rectangle sum region query runs in \\(O(1)\\) constant time**!

---

## 9. PRACTICE PROBLEMS (CORNER STUDY)

🚀 **Whiteboard bilkul ready hai bacho! Chalo ab teen behtareen interview matrix problems ko trace aur implement karte hain.**

### Problem 1 (Easy): Diagonal Sums Difference
*Given a square matrix, calculate the absolute difference between the sums of its diagonals.*

#### 🧠 Analysis:
* Square matrix condition means `primary` can be iterated via `i === j`, and `secondary` via `j === n - 1 - i`.
* We can compute both sums in a **single loop of \\(O(n)\\)** time complexity!

```javascript
function diagonalDifference(matrix) {
    const n = matrix.length;
    let primarySum = 0;
    let secondarySum = 0;
    
    for (let i = 0; i < n; i++) {
        primarySum += matrix[i][i];
        secondarySum += matrix[i][n - 1 - i];
    }
    return Math.abs(primarySum - secondarySum);
}
```
* **Complexity:** Time: **\\(O(n)\\)**, Space: **\\(O(1)\\)**.

---

### Problem 2 (Medium): Set Matrix Zeroes
*Given an \\(m \times n\\) integer matrix, if an element is 0, set its entire row and column to 0's in-place.*

#### 🧠 Analysis & Bottleneck:
* If we update elements to `0` immediately when scanned, we will accidentally propagate zeroes down the matrix, turning the entire matrix zero!
* *Brute force:* Use \\(O(rows + cols)\\) extra space using two arrays to record indices of zeroes seen.
* *Optimal In-place approach:* We can use the **first row and first column of the matrix itself as markers**!
  We just need two extra flags to check if the first row or first column itself contained zeroes originally.

```javascript
function setZeroes(matrix) {
    const rows = matrix.length;
    const cols = matrix.length;
    let firstRowHasZero = false;
    let firstColHasZero = false;
    
    // Check if first column has any zero
    for (let i = 0; i < rows; i++) {
        if (matrix[i] === 0) firstColHasZero = true;
    }
    
    // Check if first row has any zero
    for (let j = 0; j < cols; j++) {
        if (matrix[j] === 0) firstRowHasZero = true;
    }
    
    // Use first row and column as marker storage
    for (let i = 1; i < rows; i++) {
        for (let j = 1; j < cols; j++) {
            if (matrix[i][j] === 0) {
                matrix[i] = 0; // Row marker
                matrix[j] = 0; // Col marker
            }
        }
    }
    
    // Place zeroes based on markers
    for (let i = 1; i < rows; i++) {
        for (let j = 1; j < cols; j++) {
            if (matrix[i] === 0 || matrix[j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }
    
    // Zero out first col if needed
    if (firstColHasZero) {
        for (let i = 0; i < rows; i++) matrix[i] = 0;
    }
    
    // Zero out first row if needed
    if (firstRowHasZero) {
        for (let j = 0; j < cols; j++) matrix[j] = 0;
    }
}
```
* **Complexity:** Time Complexity: **\\(O(rows \times cols)\\)**, Space Complexity: **\\(O(1)\\)** auxiliary space. *Zero extra variables storage bacho!*

---

### Problem 3 (Challenging): Diagonal Traversal (Snake diagonal pattern)
*Given an \\(m \times n\\) matrix, return all elements of the matrix in diagonal order (zigzag snake style).*

```
                     Original (3x3 Matrix)
                     ┌───┬───┬───┐
                     │ 1 │ 2 │ 3 │
                     ├───┼───┼───┤   Zigzag output diagonal scan:
                     │ 4 │ 5 │ 6 │  
                     ├───┼───┼───┤
                     │ 7 │ 8 │ 9 │
                     └───┴───┴───┘
```

#### 🧠 Analysis & Mathematical symmetry:
* Notice that all elements on any single diagonal line have the **same index sum: `i + j === d`** (where diagonal index `d` ranges from `0` to `rows + cols - 2`).
* Zigzag trace direction depends on whether diagonal step index is even or odd:
  * Even diagonals traverse **upwards** (row decreasing, col increasing).
  * Odd diagonals traverse **downwards** (row increasing, col decreasing).

```javascript
function findDiagonalOrder(matrix) {
    if (matrix.length === 0) return [];
    const rows = matrix.length;
    const cols = matrix.length;
    const result = [];
    
    // Total number of diagonal lanes is (rows + cols - 1)
    const numDiagonals = rows + cols - 1;
    
    for (let d = 0; d < numDiagonals; d++) {
        const intermediate = [];
        
        // Find starting row and col indexes for diagonal sum line 'd'
        let r = d < cols ? 0 : d - cols + 1;
        let c = d < cols ? d : cols - 1;
        
        while (r < rows && c >= 0) {
            intermediate.push(matrix[r][c]);
            r++;
            c--;
        }
        
        // If even diagonal index sum, we reverse direction values to go upwards
        if (d % 2 === 0) {
            result.push(...intermediate.reverse());
        } else {
            result.push(...intermediate);
        }
    }
    return result;
}
```
* **Complexity:** Time Complexity: **\\(O(rows \times cols)\\)** (one linear zigzag trace), Space Complexity: **\\(O(rows \times cols)\\)** for intermediate reversing storage lanes.

---

## 11. COMMON MISTAKES (THE RED FLAGS)

1. **Row/Column confusion:**
   `matrix[col][row]` likhna sabse aam galti hai. Coordinates are hamesha `matrix[row][col]`. Outer bounds size limit is row lengths, inner is columns.
2. **Dynamic initialization referencing pointers bug:**
   `let matrix = new Array(rows).fill(new Array(cols))` creates identical references. Avoid this and use `Array.from()`.
3. **Invalid Index access crash:**
   Scanning diagonal calculations without boundary checks can cause reading properties of **`undefined`** (`matrix[i][j]` where `matrix[i]` is undefined). Always safeguard `row < rows` and `col >= 0`.
4. **Incorrect rotational transpositions:**
   Swapping rectangular matrices elements in-place will cause array boundaries overlap and index corruption. *Transpose rectangular arrays only by allocating new sized coordinates!*

---

### ✅ Completed | Chapter 5 — Matrix & 2D Arrays

🧠 **Matrix Skills:**
* Representing dynamic coordinate matrices inside weakly-typed arrays of arrays.
* Linear-time diagonal operations through single loops arithmetic optimizations.
* In-place rotational shifts (Transpose + Reverse) using algebraic symmetric reflections.

🎯 **Patterns Learned:**
* **Boundary Shrinking:** tracking top-bottom-left-right boundary pointers sequentially (Spiral Traversal).
* **Staircase Elimination:** row-wise and col-wise matrix scanning to drop search spaces in sorted structures.
* **2D Prefix Sum:** Precomputing integral values to resolve regional queries in \\(O(1)\\) constant time.

⚠️ **Common Mistakes:** Reference replication errors during empty array definitions, and index crashes from row-col swapping logic.

