**Arey bacho! Jaldi se apni seats par baith jao aur blackboard par apna dhyan seedhe focus karo.**  

Pichle chapters mein humne **Dynamic Programming (Chapter 21, 22, 23)** ke bade-bade and dhasu matrices transitions ko visually solve kiya. Humne seekha ki kaise complex overlapping states ko break kiya jata hai.

Lekin beta, aaj hum computer science ke sabse micro-level, high-performance aur optimized area mein enter karne ja rahe hain. Ise hum kehte hain—**Bit Manipulation**!

*"Sir, real world mein iska kya use hai? Hum toh simple operators use karte hain."*

Bacho, computer ko na JavaScript samajh aati hai, na dynamic programming. Computer ek simple electronic machine hai jo sirf ek hi cheez samajhti hai—**Electric Current**! 
*   **Current High (On):** represented by `1`
*   **Current Low (Off):** represented by `0`

Jab tum kisi company ke SDE (Software Development Engineer) interview mein baithoge, toh interviewer tumhari optimization skills check karne ke liye bit-manipulation ke tricks prapose karega. Kyunki bitwise operations directly computer hardware (CPU registers) par run hote hain, isiliye ye arithmetic operations (`+`, `-`, `*`, `/`) se **10x to 100x fast** hote hain!

Chalo aaj bilkul zero (0) se shuru karke binary digits ke logic ko apne dimaag mein hamesha ke liye fit karte hain. Pen aur register nikal lo, aur whiteboard par focus karo! 🚀

---

## 1. BINARY REPRESENTATION & CONVERSION FOUNDATIONS

Bacho, sabse pehle samajhte hain ki computer numbers ko dekhta kaise hai.

### Decimal vs. Binary System 📏
*   **Decimal System (Base 10):** Humari normal life ka system. Isme total 10 digits hote hain: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9`. Har position 10^x ka multiple hoti hai.
*   **Binary System (Base 2):** Computers ka system. Isme sirf 2 digits hote hain: `0` aur `1`. Har position 2^x ka multiple hoti hai.

```
                      BINARY POSITION VALUES WEIGHT:
                      
       ...   2^5    2^4    2^3    2^2    2^1    2^0
       ...   32     16      8      4      2      1
```

### Decimal → Binary (Kaise Banta hai Number? ⚙️)
Maan lo humein decimal number **`10`** ko binary mein convert karna hai. 
*Rule:* Number ko continuous `2` se divide karo aur bache huyen **remainders** ko reverse order mein note down karo:

1.  `10 / 2 = 5` | Remainder = **`0`** (Least Significant Bit - LSB)
2.  `5 / 2 = 2`  | Remainder = **`1`**
3.  `2 / 2 = 1`  | Remainder = **`0`**
4.  `1 / 2 = 0`  | Remainder = **`1`** (Most Significant Bit - MSB)

Ab in remainders ko neeche se upar (reverse order) read karo bacho: **`1010`**!
So, 10_{10} = 1010_2.

---

### Binary → Decimal (Wapas Kaise Aayein? 🔄)
Maan lo humare paas binary number **`1011`** hai. Ise wapas decimal mein convert karne ke liye position values ko multiply karke add karo:

\\[(1 × 2^3) + (0 × 2^2) + (1 × 2^1) + (1 × 2^0)\\]
\\[= 8 + 0 + 2 + 1 = 11_{10}\\]

### Bits and Positions (The Indexing)
Binary array system ke index coordinates humesha **Right to Left** chalte hain aur `0-based` hote hain:
```
                         Bit Positions:
                         
                         1   0   1   1   0   1
                         ▲                   ▲
                       Pos 5               Pos 0
                  (MSB - Most)        (LSB - Least)
```
*   **Least Significant Bit (LSB):** Sabse rightmost bit (Pos 0). Ye decide karta hai ki number **Odd** hai ya **Even**!
*   **Most Significant Bit (MSB):** Sabse leftmost bit. Signed representations mein ye check karta hai ki number positive (`0`) hai ya negative (`1`)!

---

### The Powers of 2 Connection 🔌
*Observation:* Kisi bhi power of 2 wale number (jaise 1, 2, 4, 8, 16, 32...) ka binary form hamesha **ek single `1` aur baki saare `0`** se milkar banta hai!
*   2^0 = 1 → 000001_2
*   2^1 = 2 → 000010_2
*   2^2 = 4 → 000100_2
*   2^3 = 8 → 001000_2

---

## 2. JAVASCRIPT-SPECIFIC WARNING: THE 32-BIT SIGNED TRAP 🚨

Bacho, dhyan se whiteboard par bani is heading ko dhoondho. JavaScript mein bitwise operations perform karte waqt ek bohot bada **interview crash bug** hota hai.

### The Trap: Double Precision Float to 32-bit Signed Int
*   JavaScript mein by default saare numbers **64-bit Double Precision Floating-point** format mein save hote hain (jismein decimals hotey hain).
*   Lekin, jaise hi tum JavaScript mein koi bitwise operator (`&`, `|`, `^`, `<<`, `>>`) use karte ho, JS engine internally us number ko turant **32-bit signed integer** representation mein force convert kar deta hai!
*   Operation complete hone ke baad, result wapas 64-bit float banakar return kiya jata hai.

### Two's Complement & Negative Numbers 👿
Signed 32-bit binary representation mein, negative numbers ko save karne ke liye computer **Two's Complement** representation use karta hai.
*   *How to get Two's Complement?* Invert all bits (NOT operation) and then add 1!
*   *SDE Impact:* JavaScript mein if you write `~6` (NOT of 6), the engine returns `-7`. Why? Kyunki bits inversion ke baad sign bit dynamic change update binary complement sequence produce karta hai.

> **BigInt Solution:** Agar tumhe standard 32-bit integer ki range (-2^{31} to 2^{31}-1) ke bahar kisi bade number ke bits ko manipulate karna hai, toh mandatory **`BigInt` (e.g., `12n`)** format use karna padega, warna bit calculations jumbled overflow ho jayengi.

---

## 3. BITWISE OPERATORS BREAKDOWN (THE TRUTH TABLES)

Bacho, logical operators (`&&`, `||`, `!`) pure expression ke boolean conditions (true/false) par play karte hain. Lekin **Bitwise Operators** directly binary positions ke ek-ek bit par independently computation perform karte hain.

---

### A. Bitwise AND `&` (The Strict Gatekeeper 🔒)
*   **Logic:** Output strictly `1` tabhi banega jab **dono** corresponding bits `1` hon.

| Bit A | Bit B | Result (A & B) |
| :---: | :---: | :---: |
|   0   |   0   |       0       |
|   0   |   1   |       0       |
|   1   |   0   |       0       |
|   1   |   1   |   **1**       |

*   **Binary Example:** `5 & 6`
    *   5 → 0101_2
    *   6 → 0110_2
    *   `5 & 6` calculation:
        ```
          0101 (5)
        & 0110 (6)
        ----------
          0100 (4)
        ```
    *   *JS Syntax:* `console.log(5 & 6); // Output: 4`
*   **Practical Use:** Kisi specific bit ki status (set/unset) check karne ke liye masking run karna.

---

### B. Bitwise OR `|` (The Friendly Guard 🔓)
*   **Logic:** Output `1` banega agar **ek bhi** input bit `1` ho.

| Bit A | Bit B | Result (A \| B) |
| :---: | :---: | :---: |
|   0   |   0   |       0       |
|   0   |   1   |   **1**       |
|   1   |   0   |   **1**       |
|   1   |   1   |   **1**       |

*   **Binary Example:** `5 | 6`
    *   `5 | 6` calculation:
        ```
          0101 (5)
        | 0110 (6)
        ----------
          0111 (7)
        ```
    *   *JS Syntax:* `console.log(5 | 6); // Output: 7`
*   **Practical Use:** Kisi specific position ke bit ko `1` par **set** (force enable) karne ke liye.

---

### C. Bitwise XOR `^` (The Odd-One Out / Difference Detector 💥)
*   **Logic:** Output `1` sirf tabhi banega jab dono bits **different** hon (ek `0` ho aur ek `1`).

| Bit A | Bit B | Result (A ^ B) |
| :---: | :---: | :---: |
|   0   |   0   |       0       |
|   0   |   1   |   **1**       |
|   1   |   0   |   **1**       |
|   1   |   1   |       0       |

*   **Binary Example:** `5 ^ 6`
    *   `5 ^ 6` calculation:
        ```
          0101 (5)
        ^ 0110 (6)
        ----------
          0011 (3)
        ```
    *   *JS Syntax:* `console.log(5 ^ 6); // Output: 3`
*   **Practical Use:** Duplicates data values ko cancel out karne and differences count dhoondhne ke liye.

---

### D. Bitwise NOT `~` (The Flipping Mirror 🔄)
*   **Logic:** Har individual bit ko invert (flip) kar deta hai (`0` ban jata hai `1`, aur `1` ban jata hai `0`).

| Bit A | Result (~A) |
| :---: | :---: |
|   0   |   **1**  |
|   1   |   **0**  |

*   **Binary Example & Two's Complement:** `~5`
    *   5 → 00000000000000000000000000000101_2
    *   `~5` flips to: 11111111111111111111111111111010_2 (Representing `-6` in Two's Complement).
    *   *JS Syntax:* `console.log(~5); // Output: -6`
*   **Mathematical Short-trick:** \\(\sim x = -(x + 1)\\)

---

### E. Left Shift `<<` (The Exponential Multiplier 🏎️)
*   **Logic:** Saare bits ko specified positions se **left** shift karta hai, aur right side se `0s` push karke empty spaces fill kar deta hai.

```
                     Left Shift Visualization (5 << 1):
                     Original:   0 1 0 1  (5)
                                ↙ ↙ ↙ ↙ 
                     Shifted:  1 0 1 0    (10)  ◄── Right end filled with 0
```
*   *JS Syntax:* `console.log(5 << 1); // Output: 10`
*   **Practical Use:** Fast multiplication by powers of 2: \\(x \ll k = x × 2^k\\).

---

### F. Right Shift `>>` (The Integer Divider 📉)
*   **Logic:** Saare bits ko specified positions se **right** shift karta hai, aur leftmost ends se MSB (sign bit) ke values replicate karke blank space fill karta hai.

```
                     Right Shift Visualization (5 >> 1):
                     Original:   0 1 0 1  (5)
                                  ↘ ↘ ↘ 
                     Shifted:    0 0 1 0  (2)  ◄── Left end filled with sign bit
```
*   *JS Syntax:* `console.log(5 >> 1); // Output: 2`
*   **Practical Use:** Fast integer division by powers of 2: \\(x \gg k = ⌊ x / 2^k ⌋\\).

---

## 4. THE BITWISE TOOLKIT: CORE CRUD OPERATIONS

Bacho, register mein in dhasu code lines ke syntax ko clean note down kar lo. Bitwise questions start karne se pehle in micro operations ka clear hona mandatory hai:

```
                            THE BITWISE TOOLKIT
                                     │
      ┌────────────────┬─────────────┴───┬───────────────┬────────────────┐
      ▼                ▼                 ▼               ▼                ▼
  Get Bit           Set Bit          Clear Bit       Toggle Bit       Check Odd/Even
(n >> i) & 1     n | (1 << i)      n & ~(1 << i)    n ^ (1 << i)         n & 1
```

### 1. Check/Get `i`-th Bit 🔍
*   **How?** Target bit position ko Right shift se `0-th` slot par lekar aao, fir `& 1` mask se check karo.
*   *Code:* `const getBit = (num, i) => (num >> i) & 1;`

### 2. Set `i`-th Bit to 1 🛠️
*   **How?** `1` ko `i` positions shift karo (`1 << i`) aur use OR `|` operation ke sath merge karo.
*   *Code:* `const setBit = (num, i) => num | (1 << i);`

### 3. Clear `i`-th Bit to 0 🧼
*   **How?** Bitmask compile karo jismein `i`-th position `0` ho aur baaki sab `1` (via `~(1 << i)`). Fir execute AND `&`.
*   *Code:* `const clearBit = (num, i) => num & ~(1 << i);`

### 4. Toggle `i`-th Bit (0 → 1, 1 → 0) 🔄
*   **How?** Target position bit to XOR `^` operation se process karo.
*   *Code:* `const toggleBit = (num, i) => num ^ (1 << i);`

### 5. Check Odd/Even using Bits 🥚
*   **How?** Binary numbers mein Even numbers ke Pos 0 (LSB) humesha `0` hoti hai, aur Odd numbers ke Pos 0 hamesha `1` hoti hai. So perform `& 1` check!
*   *Code:*
    ```javascript
    const isOdd = (num) => (num & 1) === 1; // If true, number is Odd!
    ```

### 6. Fast Multiply / Divide by Powers of 2 🏎️
*   `num << k` translates to num × 2^k.
*   `num >> k` translates to ⌊ num / 2^k ⌋.

### 7. Power of Two Verification Rule ⚡
*   **How?** Agar number `N` power of 2 hai, toh binary mein sirf ek hi `1` set hoga. Isliye, `N - 1` ke pass bache saare bits `1` honge.
    E.g., 8 = 1000_2 | 7 = 0111_2.
    Perform `N & (N - 1)`. If result is `0`, N is a Power of Two!
*   *Code:*
    ```javascript
    const isPowerOfTwo = (n) => n > 0 && (n & (n - 1)) === 0; //
    ```

---

## 5. THE MAGIC OF XOR: REMOVING DUPLICATES DEEPLY

Bacho, bitwise XOR (`^`) technical coding rounds ka absolute champion operator hai. Iski four primary properties iski capabilities ko exceptionally power-packed banati hain:

1.  **Identity:** x & 0 = x (XORing with 0 does not change status)
2.  **Self-Inverse:** x & x = 0 (Any number XORed with itself cancels out to 0!)
3.  **Commutative:** A & B = B & A (Order does not affect outcome)
4.  **Associative:** A & (B & C) = (A & B) & C

```
                     XOR Duplicates Cancellation Wave:
                     Let values be:
                     
                     XOR cumulatively: 4 ^ 1 ^ 2 ^ 1 ^ 2
                     Re-order variables: (1 ^ 1) ^ (2 ^ 2) ^ 4
                     Cancel duplicates:  0 ^ 0 ^ 4 ==> Result is 4!
```

---

## 6. CORE INTERVIEW PROBLEMS (PROGRESSIVE MASTERCLASS)

🚀 **Aao bacho! Ab bit manipulation ke in 6 major problems ko clear dry runs aur micro-binary transitions ke sath breakdown karte hain!**

---

### PROBLEM 1 (Easy): Single Number (LeetCode 136)

#### 1. Understand:
Humein ek non-empty array of integers `nums` diya hai. Har element exactly **do baar** aata hai, siwaye ek element ke jo unique hai. Humein woh unique element return karna hai.

#### 2. Brute Force Approach:
Ek frequency map maintain karo aur check count return karo, jismein Space Complexity O(N) barh jayegi.

#### 3. SDE Observation (XOR Cancellation 💡):
Kyunki saare elements twice repeat hote hain aur unique element single hai, agar hum pure array ke numbers ko aapas mein sequentially XOR `^` kar dein, toh duplicates properties ke hisab se pairs cancel out hokar `0` ban jayenge aur bacha hua unique number absolute balance return ho jayega!

---

#### 4. JavaScript Code:
```javascript
function singleNumber(nums) {
    let xorSum = 0; // x ^ 0 = x
    for (let num of nums) {
        xorSum ^= num; // Cumulatively perform XOR
    }
    return xorSum;
}
```

#### 5. Complete Binary Dry Run on `nums =`:
*   Initial: `xorSum = 0`
*   **Step 1:** `xorSum = 0 ^ 2 = 2` | Binary: `0000_2 ^ 0010_2 = 0010_2` (2)
*   **Step 2:** `xorSum = 2 ^ 2 = 0` | Binary: `0010_2 ^ 0010_2 = 0000_2` (0)
*   **Step 3:** `xorSum = 0 ^ 1 = 1` | Binary: `0000_2 ^ 0001_2 = 0001_2` (1)
*   Returns `1`. Perfectly matched!

#### 6. Complexity Analysis:
*   **Time Complexity:** **O(N)** single pass.
*   **Space Complexity:** **O(1)** auxiliary.

---

### PROBLEM 2 (Easy): Missing Number (LeetCode 268)

#### 1. Understand:
Humein ek array `nums` diya hai jismein `n` distinct numbers hain range `[0, n]` ke beech. range mein se jo ek single number missing hai, use find out karo.

#### 2. Arithmetic Approach vs. XOR Approach:
*   *Arithmetic sum formula:* Total expected sum S = n × (n+1)/2 calculate karo aur array numbers sum minus karo.
    *   *Bug in JS:* Large numbers hone par summation variables integers range overflow kar sakte hain.
*   *XOR Approach:* Range `[0, n]` ke saare indices aur array ke saare elements ko sequentially XOR kar do. Missing element directly capture ho jayega because indices and element matches cancel each other out!

---

#### 3. JavaScript Code:
```javascript
function missingNumber(nums) {
    let xorSum = nums.length; // Range limit index initialization
    for (let i = 0; i < nums.length; i++) {
        xorSum ^= i ^ nums[i]; // XOR indexes and numbers parallel
    }
    return xorSum;
}
```

#### 4. Complete Binary Dry Run on `nums =`:
*   `n = 3`. `xorSum` starts at `3`.
*   Loop iterations:
    *   `i = 0`: `xorSum = 3 ^ (0 ^ 3) = 0` (commutes to cancel duplicates)
    *   `i = 1`: `xorSum = 0 ^ (1 ^ 0) = 1`
    *   `i = 2`: `xorSum = 1 ^ (2 ^ 1) = 2`
*   Returns `2`. Correct!

#### 5. Complexity Analysis:
*   **Time Complexity:** **O(N)**, **Space Complexity:** **O(1)**.

---

### PROBLEM 3 (Medium): Two Non-Repeating Numbers (Single Number III)

#### 1. Understand:
Humein ek array diya hai jismein saare numbers do-do baar aate hain, par **do numbers** aise hain jo sirf ek hi baar aate hain. Humein un dono elements ko find out karna hai.

#### 2. SDE Observation:
1.  Agar hum poore array ko XOR karenge, toh duplicates cancel hokar humein end mein: **`tempXor = x ^ y`** milega (where `x` and `y` are the two unique elements).
2.  Kyunki `x` aur `y` unique hain, toh `tempXor !== 0` hoga, matlab iske binary configuration mein kam se kam **ek set bit (1)** zaroor hoga.
3.  Hum is rightmost set bit ki position dhoondhenge: `rightmostSetBit = tempXor & -tempXor`.
4.  Yeh set bit position divide-rule check karega: Jis group ke numbers ke pass ye position `1` hai, unka XOR group A mein cancel out hoga, aur jinki pos `0` hai, unka XOR group B mein cancel out hoga!

---

#### 3. JavaScript Code:
```javascript
function singleNumberIII(nums) {
    let tempXor = 0;
    for (let num of nums) tempXor ^= num;

    // Get the rightmost set bit
    const rightmostSetBit = tempXor & -tempXor;

    let x = 0;
    let y = 0;

    for (let num of nums) {
        if ((num & rightmostSetBit) !== 0) {
            x ^= num; // Group A (Target bit is 1)
        } else {
            y ^= num; // Group B (Target bit is 0)
        }
    }

    return [x, y];
}
```
*   **Complexity:** Time: **O(N)**, Space: **O(1)**. Outstanding SDE logic!

---

### PROBLEM 4 (Easy): Number of 1 Bits (LeetCode 191)

#### 1. Understand:
Humein ek positive integer `n` diya hai. Humein iske binary representation mein set bits (`1s`) ka count return karna hai (also known as Hamming Weight).

#### 2. The Algorithmic Clash:
*   **Linear Scan Approach:** Loop through 32 times, shifting right and checking odd bit count `num & 1`.
    *   *Complexity:* Always runs exactly **32 times** regardless of total set bits.
*   **Brian Kernighan’s Algorithm (The Set Bit Pruner 💡):**
    *   *Idea:* Jab hum **`n & (n - 1)`** execute karte hain, toh hum binary number ke **sabse lowest set bit (1) ko direct clear (0)** kar dete hain!
    *   *Benefit:* Loop strictly utni hi baar chalega jitne `n` ke pass active set bits (1) hain! Highly optimized for sparse configurations.

---

#### 3. JavaScript Code (Brian Kernighan’s Approach):
```javascript
function hammingWeight(n) {
    let count = 0;
    while (n !== 0) {
        n = n & (n - 1); // Clears the lowest set bit
        count++;
    }
    return count;
}
```

#### 4. Complete Binary Dry Run on `n = 11` (Binary `1011`):
*   Initial: `count = 0`
*   **Loop 1:** `n = 11 (1011_2) & 10 (1010_2) = 10 (1010_2)`. `count` becomes `1`.
*   **Loop 2:** `n = 10 (1010_2) & 9 (1001_2) = 8 (1000_2)`. `count` becomes `2`.
*   **Loop 3:** `n = 8 (1000_2) & 7 (0111_2) = 0 (0000_2)`. `count` becomes `3`.
*   Loop terminates because `n === 0`.
*   Returns `3`. (Binary 11 has three 1s: `1011`). Correct!

#### 5. Complexity Analysis:
*   **Time Complexity:** **O(Set Bits Count)** which is at most O(32).
*   **Space Complexity:** **O(1)**.

---

### PROBLEM 5 (Easy): Power of Two (LeetCode 231)

```javascript
function isPowerOfTwo(n) {
    // If n <= 0, it cannot be a power of 2
    return n > 0 && (n & (n - 1)) === 0; //
}
```
*   **Complexity:** Time: **O(1)**, Space: **O(1)**.

---

### PROBLEM 6 (Medium): Counting Bits (LeetCode 338)

#### 1. Understand:
Humein ek integer `n` diya hai. Humein array return karna hai jiske index `i` par `0` se `n` tak ke numbers ke active set bits count saved hon.

#### 2. The DP bit transition logic 🧠:
*   *Observation:* Kisi number `i` ke set bits, uske right shifted half `i >> 1` ke bits ke directly similar hote hain, bas current bit offset add karna hota hai if `i` is Odd (`i & 1`)!
    \\[SetBits(i) = SetBits(i / 2) + (i % 2)\\]

```javascript
function countBits(n) {
    const dp = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        // dp[i >> 1] is similar to diving by 2, (i & 1) gets 1 if odd else 0
        dp[i] = dp[i >> 1] + (i & 1); 
    }
    return dp;
}
```
*   **Complexity:** Time: **O(N)**, Space: **O(N)** auxiliary table.

---

## 7. BITMASKS AND SUBSETS: CONNECTING TO DP STATE SPACE

### What is a Bitmask? 🎭
Bitmask ek mathematical technique hai jahan hum ek single integer ki binary representation use karke kisi array ya set ke active selection elements state ko track karte hain.
*   If `i`-th bit is `1` → i-th element is **Selected**.
*   If `i`-th bit is `0` → i-th element is **Unselected**.

### Power Set (Subset Generation using Bitmask - LeetCode 78)
Maan lo humare paas set hai `{A, B, C}` (size N=3). Total subsets kitne honge? 2^3 = 8 subsets!
Hum loop chalayenge decimals `0` se `7` tak, aur unki bits matching positions elements select karenge:

```javascript
function subsets(nums) {
    const n = nums.length;
    const totalCombinations = 1 << n; // 2^n possibilities
    const result = [];

    for (let mask = 0; mask < totalCombinations; mask++) {
        const subset = [];
        for (let i = 0; i < n; i++) {
            // Check if i-th bit is enabled in the current configuration mask
            if ((mask & (1 << i)) !== 0) {
                subset.push(nums[i]);
            }
        }
        result.push(subset);
    }
    return result;
}
```
*   **When is Bitmask DP practical?**  
    Bitmask state space exponential scale par grow karta hai (size 2^N). Isliye, coding test cases mein bitmasking patterns tabhi apply karna jab constraints explicitly defined hon **N <= 20**.

---

## 8. SDE COMPARATIVE STUDY BOARDS ⚔️

Whiteboard ke is summary section ko dhyan se dimaag mein update karo:

### A. Bitwise AND vs. OR vs. XOR
*   **AND `&`:** Dono bits `1` hone par hi `1` deta hai. (Strict intersection).
*   **OR `|`:** Kisi ek ke `1` hone par hi `1` de deta hai. (Lenient union).
*   **XOR `^`:** Dono bits different hone par hi `1` deta hai. (Difference finder).

### B. Normal Counting vs. Brian Kernighan
*   **Normal Counting:** Loops strictly 32 times, evaluating each bit.
*   **Brian Kernighan:** Loops strictly equal to total set bits, pruning rightmost boundaries dynamically. Time: **O(Set Bits Count)**.

---

## 9. SDE TRAPS & COMMON MISTAKES TO AVOID ⚠️

1.  **Forgetting Parentheses (Operator Precedence Bug):**
    Bitwise operators ki precedence arithmetic comparisons operators se low hoti hai.
    *   `if (num & 1 === 1)` is evaluated as `num & (1 === 1)` by JS, which causes logical bugs!
    *   *The Fix:* Hamesha bitwise operations ko parentheses brackets mein wrap karein: **`if ((num & 1) === 1)`**!
2.  **Confusing XOR `^` with Exponentiation:**
    JS math variables systems mein `2 ^ 3` power exponentiation check nahi hai! `2 ^ 3` is bitwise XOR resulting `1`. Exponentiation ke liye **`2 ** 3`** ya `Math.pow(2, 3)` use karein.
3.  **Assuming arbitrary size integer shift safety:**
    JS signed integers bits shifts operations safely strictly **32-bits range bounds** par hi execute kar sakti hain. Large values coordinate setups par mandatory `BigInt` check setup apply karein.

---

## BIT TRICKS CHEAT SHEET 🗃️

*   **LSB Odd Check:** `(n & 1) !== 0`
*   **Clear Lowest Set Bit:** `n = n & (n - 1)`
*   **Get lowest set bit element:** `lowestSet = n & -n`
*   **Multiply by 2:** `n << 1`
*   **Divide by 2:** `n >> 1`

---

## PRACTICE ROADMAP 🗺️

1.  Solve LeetCode 136 *Single Number* using XOR cumulative summation.
2.  Practice LeetCode 191 *Number of 1 Bits* via Brian Kernighan's bit pruner.
3.  Solve LeetCode 78 *Subsets* using bitwise masking combinatorics.

---

⏮️ **Pichle Chapters ka Connection:** Chapter 23 ke Bitmask DP assignment matrices states par humne isi integers mask and shifts binary properties operator tracking variables ko base states optimize karne ke liye use kiya!

