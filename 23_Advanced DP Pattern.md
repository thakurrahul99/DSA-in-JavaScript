**Arey bacho! Jaldi se apni seats par baith jao, aaj hum computer science ke sabse dimag kholne wale topic ke aakhri padav par hain.**

Pichle chapter mein humne **Core DP Patterns (Chapter 22)** ko seekha, jahan humne 1D DP, Grid DP, aur Knapsack algorithms ke basic structures ko dhang se decode kiya [cite: 431, 733, 768, 785, 796].

Lekin bacho, real-world SDE interviews mein jab Google, Amazon, ya Microsoft ke interviewers aate hain, toh woh direct standard Knapsack nahi puchte [cite: 706]. Woh dynamic programming ko dynamic string alignments, interval splits, prefix trees ya bitwise configuration masks ke sath mix karke push karte hain [cite: 403, 414, 417, 731]. 

Aaj hum dynamic programming ke absolute **advanced patterns** ko dimaag mein fit karenge. Hum koi formula nahi ratenge! Whiteboard par bane ek-ek recursion tree aur index matrix transition ko dhyan se samajhna.

Chalo, shuru karte hain hamara dhasu—**Chapter 23: Advanced DP Patterns**! 🚀

---

## 1. STATE DIMENSION ENGINE: HOW TO DECIDE STATE SIZE? 📐

Bacho, sabse bada darr jo learner ko hota hai woh ye hai: *"Sir, mujhe kaise pata chalega ki state 1D hogi, 2D hogi ya 3D? Main dimension kaise decide karoon?"*

Iske liye humein khud se ek sawaal puchna hota hai:
> **"What information from the past is strictly necessary to make a valid choice right now without violating any constraints?"** [cite: 301, 325, 434]

Let's understand with this comparative matrix:

| PAST INFORMATION NEEDED 🧠 | THE NATURAL STATE SHAPE | GRAPHICAL BLUEPRINT | COMMON PATTERN |
| :--- | :--- | :--- | :--- |
| **Only the current index `i` is enough** to define the subproblem. | **1D State:** `dp[i]` [cite: 418, 430, 733, 767, 784, 795, 796] | `[ i ]` | Fibonacci, Climbing Stairs [cite: 326, 418, 430, 480, 733, 767, 784, 795, 796] |
| We need to track our position in **two independent sequences** or grid dimensions. | **2D State:** `dp[i][j]` [cite: 414, 418, 426, 733, 764, 767, 780, 784, 791, 795, 796] | `[ i ] ──► [ j ]` | LCS, Grid Paths, Edit Distance [cite: 403, 414, 418, 426, 733, 764, 767, 780, 784, 791, 795, 796] |
| We are taking/skipping items, and **how much capacity** `w` remains is critical [cite: 414, 764, 780, 791, 796]. | **2D State:** `dp[i][w]` [cite: 414, 764, 780, 791, 796] | `[ index ][ capacity ]` | 0/1 Knapsack, Subset Sum [cite: 414, 764, 780, 791, 796] |
| We are splitting/merging a range, so we need **both left `l` and right `r` boundaries** of the interval. | **2D Interval State:** `dp[l][r]` | `[ l <───► r ]` | Matrix Chain, Palindromic Partitioning |
| We need to track **which specific items have been selected** from a small pool. | **Bitmask DP:** `dp[i][mask]` [cite: 403, 413, 731, 762, 763, 777, 778, 788, 789] | `[ i ][ 01101 ]` | Travelling Salesman, Job Assignment |

### Constraint-to-State Estimator Table ⏱️
SDE coding tests mein constraints dekhkar hi tum complexity aur DP state dimensions predict kar sakte ho:

*   **\\(N \le 20\\):** Space limits bohot small hain, matlab \\(\mathcal{O}(2^N \cdot N)\\) **Bitmask DP** ya backtracking possible hai [cite: 413, 485, 486].
*   **\\(N \le 400\\):** Cubic solutions run ho sakte hain \\(\rightarrow\\) \\(\mathcal{O}(N^3)\\) **Partition DP / MCM** is highly likely.
*   **\\(N \le 2000\\):** Quadratic bounds \\(\rightarrow\\) \\(\mathcal{O}(N^2)\\) **2D DP (LCS, Edit Distance)** works safely [cite: 414, 733].
*   **\\(N \le 10^5\\):** Strictly linear ya log-linear limit \\(\rightarrow\\) \\(\mathcal{O}(N)\\) or \\(\mathcal{O}(N \log N)\\) **1D DP / LIS Optimization** is mandatory [cite: 414, 506].

---

## 2. PATTERN 1: LIS (LONGEST INCREASING SUBSEQUENCE) & SEQUENCE DP [cite: 414, 764, 780, 791, 796]

Bacho, LIS computer science ka ek behad classic pattern hai. Iska objective hota hai array mein se sabse uncha strictly badhta hua (increasing) subsequence dhoondhna [cite: 414, 764, 780, 791, 796].

```
                     LIS Array Alignment:
                     nums =
                     LIS is or ==> Length = 4!
```

---

### A. The \\(\mathcal{O}(N^2)\\) Naive DP Approach [cite: 405, 414, 764, 780, 791, 796]

#### ✏️ State & Transition Derivation:
*   **State:** Let `dp[i]` be the **length of the LIS ending strictly at index `i`** [cite: 733].
*   **Choice:** Index `i` par khade hokar hum pichle kisi bhi index `j < i` ko scan karenge. Agar `nums[i] > nums[j]` hai, toh hum current element ko `j` par khatam hone wali sequence mein append kar sakte hain [cite: 412].
*   **Transition:**
    \\[dp[i] = \max(dp[i], dp[j] + 1) \quad \forall j < i \text{ where } nums[i] > nums[j]\\]
*   **Base Case:** Har element apne aap mein size `1` ka subsequence hota hai, so initialize `dp` array with `1` [cite: 414].

#### 💻 JavaScript Code (with Path Reconstruction 💎):
```javascript
function lengthOfLISNaive(nums) {
    if (nums.length === 0) return 0;
    const n = nums.length;
    const dp = new Array(n).fill(1);
    
    // parent indices array to reconstruct path
    const parent = new Array(n).fill(-1); 
    let maxLen = 1;
    let bestEndIndex = 0;

    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                parent[i] = j; // Trace back link
            }
        }
        if (dp[i] > maxLen) {
            maxLen = dp[i];
            bestEndIndex = i;
        }
    }

    // Path Reconstruction Walkway 💎
    const path = [];
    let curr = bestEndIndex;
    while (curr !== -1) {
        path.push(nums[curr]);
        curr = parent[curr];
    }
    console.log("Reconstructed LIS Path:", path.reverse());

    return maxLen;
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(N^2)\\)**, Space: **\\(\mathcal{O}(N)\\)** auxiliary.

---

### B. The \\(\mathcal{O}(N \log N)\\) Binary Search (Patience Sorting) Optimization 🫧 [cite: 506]

Bacho, dhyan se whiteboard par dekho, ye optimizer kaise kaam karta hai:
Imagine hum cards (playing cards) ka ek game khel rahe hain jise kehte hain **Patience Sorting**.

1. Jab bhi koi naya card `x` aaye, hum use pehle pile (deck) par rakhte hain jiska top card `x` se bada ya barabar (\\(\ge\\)) ho.
2. Agar aisa koi pile nahi hai, toh hum right side par ek **naya pile** start kar dete hain.
3. Kyunki piles ke top cards hamesha left-to-right sorted order mein hote hain, hum correct pile dhoondhne ke liye **Binary Search** lagate hain [cite: 306, 462, 500]!
4. **The Magic:** Total active piles ka count hi hamare LIS ki final length banta hai!

```
                  Patience Sorting Deck Run:
                  nums =
                  
                  Pile 1      Pile 2      Pile 3
                  
              ->   [ 9] (Replaces 10)
              ->   [ 2] (Replaces 9)
                                [ 5]
              ->                [ 3] (Replaces 5)
                                            [ 7]
                  
                  Active Piles Top Elements: ==> Length is 3!
```

---

#### 💻 JavaScript Code (The \\(\mathcal{O}(N \log N)\\) Optimizer):
```javascript
function lengthOfLISOptimized(nums) {
    if (nums.length === 0) return 0;
    
    const tails = []; // Stores the active piles' top elements

    for (let x of nums) {
        let start = 0;
        let end = tails.length - 1;
        let indexToReplace = tails.length;

        // Binary Search (Lower Bound) [cite: 306, 462, 500]
        while (start <= end) {
            const mid = Math.floor((start + end) / 2); // [cite: 306, 462]
            if (tails[mid] >= x) {
                indexToReplace = mid;
                end = mid - 1; // [cite: 306, 462]
            } else {
                start = mid + 1; // [cite: 306, 462]
            }
        }

        // If no element >= x found, start a new pile
        if (indexToReplace === tails.length) {
            tails.push(x);
        } else {
            tails[indexToReplace] = x; // Replace top element of pile
        }
    }

    return tails.length;
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(N \log N)\\)** [cite: 506], Space: **\\(\mathcal{O}(N)\\)**.
*   **The SDE Comparison Check:**
    *   `O(N^2)` is simpler to reconstruct and customize (e.g. non-numeric relations).
    *   `O(N log N)` is exceptionally fast but harder to reconstruct the sequence (requires tracking indices parent pointers during replacement updates).

---

### C. LIS Variation: Longest Bitonic Subsequence

#### 🧠 Understand:
Bitonic subsequence woh sequence hoti hai jo pehle strictly badhti hai (increasing) aur phir strictly ghatti hai (decreasing).

```
                     Bitonic Sequence Curve:
                                  ● (Peak)
                                 / \
                                ●   ●
                               /     \
```

#### 💡 Optimal Approach:
1. Pure array par left-to-right LIS calculate karo: `left_LIS[i]`.
2. Pure array par right-to-left decreasing subsequence (LDS) calculate karo: `right_LDS[i]`.
3. Peak element `i` par max output merge karo:
   \\[\text{Bitonic Length} = \max(left\_LIS[i] + right\_LDS[i] - 1)\\]

---

## 3. PATTERN 2: LCS (LONGEST COMMON SUBSEQUENCE) & STRING DP [cite: 414, 764, 780, 791, 796]

Bacho, string DP mein hum character arrays par alignment check karte hain [cite: 414, 764, 780, 791, 796].

---

### A. Longest Palindromic Subsequence (LPS - LeetCode 516)

#### 🧠 Understand:
String `S` di gayi hai, iska sabse uncha palindromic subsequence nikalna hai (jo aage aur peeche dono se same read ho, non-contiguous allowed) [cite: 690].

```
                     LPS of "BBABCBCAB" is "BABCBAB" ==> Length is 7!
```

#### 💡 The LCS Connection Masterstroke:
*Observation:* Kisi string `S` ka palindrome sequence, strictly us string `S` aur uske **reversed representation `reverse(S)`** ke beech ka common part hi toh hai!
> **LPS(S) is mathematically identical to LCS(S, Reverse(S)) [cite: 414, 764, 780, 791, 796]!**

```javascript
function longestPalindromeSubseq(s) {
    const reversedS = s.split('').reverse().join('');
    return getLCS(s, reversedS); // Reuse standard LCS algorithm [cite: 414]
}

function getLCS(text1, text2) {
    const m = text1.length;
    const n = text2.length;
    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1]; // Diagonal merge [cite: 119]
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]); // Skip checks [cite: 119]
            }
        }
    }
    return dp[m][n];
}
```
*   **Minimum Insertions to make Palindrome:**
    *   *SDE Trick:* Total Insertions = `S.length - LPS(S)`. Jo characters already palindrome match ho rahe hain unhe chodo, baaki matching opposites insert kar do!

---

### B. LCS vs. Longest Common Substring (Contiguous Constraint 🚨)

Bacho, interviews mein dono strings patterns ko dhyan se samajhna:

```
          [ Subsequence DP (LCS) ]                   [ Substring DP ]
     If mismatch, carry forward max:            If mismatch, reset immediately to 0:
     dp[i][j] = max(dp[i-1][j], dp[i][j-1])     dp[i][j] = 0 (Contiguity broken!)
```

```javascript
// Longest Common Substring Implementation [cite: 415, 739]
function longestCommonSubstring(s1, s2) {
    const m = s1.length;
    const n = s2.length;
    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));
    let maxSubstringLength = 0;

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s1[i - 1] === s2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1]; // Match! Extend chain length
                maxSubstringLength = Math.max(maxSubstringLength, dp[i][j]);
            } else {
                dp[i][j] = 0; // Mismatch resets contiguous string tracker to 0! [cite: 739]
            }
        }
    }
    return maxSubstringLength;
}
```

---

## 4. PATTERN 3: EDIT DISTANCE (LEVENSHTEIN DISTANCE - LeetCode 72) [cite: 403, 414, 733, 765, 780, 791, 796]

#### 🧠 Understand:
Do strings `word1` aur `word2` di hain [cite: 77]. Humein `word1` ko `word2` mein convert karna hai minimum moves mein. Valid moves are: **Insert, Delete, or Replace** [cite: 414].

```
                     word1 = "horse", word2 = "ros"
                     horse ──► rorse (replace 'h' with 'r')
                           ──► rose (delete 'r')
                           ──► ros (delete 'e') ==> Total 3 moves!
```

---

#### ✏️ State & Choices Derivation:
*   **`dp[i][j]` ka matlab kya hai?**  
    `dp[i][j]` represent karta hai **minimum edit moves** jo lagenge to convert substring `word1[0...i-1]` into `word2[0...j-1]`.
*   **Choice Analysis:**
    Pointers index `i` on `word1` and `j` on `word2` par khade hokar:
    *   **Case 1: Characters Match (`word1[i-1] === word2[j-1]`):**  
        Koi extra cost nahi lagegi, pointers aage move kar do \\(\rightarrow\\) `dp[i-1][j-1]`.
    *   **Case 2: Mismatch (Explore the 3 options):**
        1. **Insert:** character insert karne par `word2` ka target char pass ho jata hai \\(\rightarrow\\) `1 + dp[i][j-1]`.
        2. **Delete:** character delete karne par `word1` ka check bypass hota hai \\(\rightarrow\\) `1 + dp[i-1][j]`.
        3. **Replace:** character swap/replace karne par dono progress karte hain \\(\rightarrow\\) `1 + dp[i-1][j-1]`.

---

#### ✏️ Transition & Base Cases:
\\[dp[i][j] = \min(dp[i-1][j-1] \text{ (if match else Replace + 1)}, \text{Insert } dp[i][j-1] + 1, \text{Delete } dp[i-1][j] + 1)\\]

*   **Base Cases:**
    *   If `word1` is empty (`i === 0`), we need `j` insertions: `dp[j] = j`.
    *   If `word2` is empty (`j === 0`), we need `i` deletions: `dp[i] = i`.

---

#### 💻 JavaScript Code (with 1D Space Optimization):
```javascript
function minDistance(word1, word2) {
    const m = word1.length;
    const n = word2.length;

    // Space Optimized: We only need two rows to compute DP states [cite: 47, 48]
    let prev = Array.from({ length: n + 1 }, (_, j) => j); // Base Row
    let curr = new Array(n + 1).fill(0);

    for (let i = 1; i <= m; i++) {
        curr = i; // Column Base Case (i deletions)
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                curr[j] = prev[j - 1]; // Direct match, no edit cost
            } else {
                curr[j] = 1 + Math.min(
                    prev[j - 1], // Replace
                    prev[j],     // Delete
                    curr[j - 1]  // Insert
                );
            }
        }
        prev = [...curr]; // Shift current row state to previous [cite: 48]
    }

    return prev[n];
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(M \times N)\\)**, Space: **\\(\mathcal{O}(N)\\)** optimized.

---

## 5. PATTERN 4: WORD/STRING DP (WORD BREAK - LeetCode 139) [cite: 418, 430, 431, 733, 767, 784, 795, 796]

#### 🧠 Understand:
String `s` aur ek dictionary wordlist `wordDict` diya hai. Humein batana hai ki kya string `s` ko space-separated subwords mein slice kiya ja sakta hai jismein saare words dictionary se matched hon.

```
                     s = "leetcode", wordDict = ["leet", "code"]
                     Yes! "leet" + "code" ==> Return true.
```

---

#### ✏️ State & Choices Derivation:
*   **`dp[i]` ka matlab kya hai?**  
    `dp[i]` is a **boolean value** (`true/false`) jo batata hai ki kya prefix string `s[0...i-1]` ko successfully dictionary words mein segment kiya ja sakta hai.
*   **Choice & Transition:**  
    Har index `i` par khade hokar hum backward direction mein partition cuts `j < i` par sochenge.
    *   Agar pichli prefix substring `s[0...j-1]` successfully partitioned ho chuki thi (`dp[j] === true`), aur bachi hui substring `s[j...i-1]` ek valid dictionary word hai, toh `dp[i]` true ho jayega!
    \\[dp[i] = dp[j] \text{ AND } (s[j...i-1] \in \text{wordSet}) \quad \forall j < i\\]

---

#### 💻 JavaScript Code:
```javascript
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict); // O(1) Set lookups [cite: 287]
    const n = s.length;
    const dp = new Array(n + 1).fill(false);
    
    dp = true; // Base Case: empty string is always valid

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            // If prefix matches and remainder substring is a dictionary word
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break; // One valid break path is enough!
            }
        }
    }

    return dp[n];
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(N^2)\\)** (ignoring substring slicing overhead), Space: **\\(\mathcal{O}(N + \text{DictSize})\\)**.

---

## 6. PATTERN 5: PALINDROME INTERVAL DP (LeetCode 1312/516) [cite: 414, 764, 780, 791, 796]

#### 🧠 State Formulation:
Bacho, substring palindrome problems mein interval-style dynamic states banti hain.
Let **`dp[i][j]`** be `true` if substring `s[i...j]` is a palindrome.

#### Choices & Transitions 💡:
`dp[i][j]` tabhi true hoga jab uske outer bounds elements match karein (`s[i] === s[j]`) aur unke andar bacha hua segment bhi valid palindrome ho (`dp[i+1][j-1]`):
\\[dp[i][j] = (s[i] == s[j]) \text{ AND } (j - i \le 2 \text{ OR } dp[i+1][j-1])\\]

```
                     Palindromic Boundary Check:
                     [a] ─── [b] ─── [c] ─── [b] ─── [a]
                      i       i+1     ...     j-1     j
                     (If s[i] === s[j] and internal is palindrome, then Outer is Palindrome!)
```

*   **SDE Choice Check (Expand-Around-Center alternative):**
    For Palindromic substrings, **Expand Around Center** runs in \\(\mathcal{O}(N^2)\\) time and strictly **\\(\mathcal{O}(1)\\)** space, which beats DP's \\(\mathcal{O}(N^2)\\) space overhead! However, interval DP states are highly useful for nested interval optimization structures like Matrix Chain Multiplications [cite: 733].

---

## 7. PATTERN 6: PARTITION DP & MCM (MATRIX CHAIN MULTIPLICATION) [cite: 733]

Bacho, Partition DP tab lagta hai jab humein kisi array ya sequence ko different partitions (cuts) mein break karke optimal min/max evaluations karne hote hain [cite: 733].

---

### Matrix Chain Multiplication (The Holy Grail 🏆)

#### 🧠 Understand:
Humein \\(N\\) matrices di hain, jinhe multiply karne ki total cost dimensions chain `p` par depend karti hai. Matrices multiplication is associative (e.g. `(A*B)*C` vs `A*(B*C)`). Humein parenthesization ka aisa order dhoondhna hai jisse **multiplications count absolute minimum** ho!

---

#### ✏️ State & Choices Derivation:
*   **`dp[i][j]` ka matlab kya hai?**  
    `dp[i][j]` represent karta hai **minimum multiplication operations cost** to multiply matrices from chain indices `i` to `j`.
*   **Choice of Cuts (Partition Point `k`):**  
    Hum matrices `i` se `j` ke beech kisi bhi point `k` par partition check cut laga sakte hain (jahan \\(i \le k < j\\)).
    Total operations = (cost to multiply left half `i to k`) + (cost to multiply right half `k+1 to j`) + (multiplication of final results matrices: \\(p[i-1] \times p[k] \times p[j]\\)).

---

#### ✏️ Transition Equation:
\\[dp[i][j] = \min_{i \le k < j} \left( dp[i][k] + dp[k+1][j] + p[i-1] \times p[k] \times p[j] \right)\\]

---

#### 💻 JavaScript Code:
```javascript
function matrixChainOrder(p) {
    const n = p.length - 1; // Number of matrices
    
    // V8 Safe independent 2D Array allocation [cite: 308]
    const dp = Array.from({ length: n + 1 }, () => new Array(n + 1).fill(0));

    // l is chain length (matrices range count)
    for (let l = 2; l <= n; l++) {
        for (let i = 1; i <= n - l + 1; i++) {
            const j = i + l - 1;
            dp[i][j] = Infinity;

            for (let k = i; k < j; k++) {
                const cost = dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j];
                dp[i][j] = Math.min(dp[i][j], cost); // Transition update
            }
        }
    }

    return dp[n]; // Entire chain operations
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(N^3)\\)**, Space: **\\(\mathcal{O}(N^2)\\)** auxiliary.

---

## 8. PATTERN 7: STATE COMPRESSION / BITMASK DP BASICS [cite: 403, 413, 731, 762, 763, 777, 778, 788, 789]

Bacho, jab dynamic choices mein past options unique combinations ya tracking maps bante hain, toh general arrays variables expand ho jate hain. 
*   **The Problem:** Agar hum set tracking ke liye dynamic arrays/sets use karenge, toh states map unique lookups fail ho jayenge.
*   **The Optimizer (Bitmasking):** Hum elements selection state ko **integers ke binary bits** se represent karte hain [cite: 485, 486]!
    *   `1` at position `i` means \\(i\\)-th element has been selected/visited [cite: 487].
    *   `0` at position `i` means \\(i\\)-th element is free [cite: 487].

```
                     Bitmask Representation:
                     For a set of 5 items: {A, B, C, D, E}
                     Mask = 13 in decimal ==> Binary 01101
                     Meaning: Items A, C, and D are Selected. Items B and E are Unselected.
```

---

### Bitwise State Operators Cheat Sheet ⚙️ [cite: 411, 731, 762, 777, 788]

1.  **Check if `i`-th item is visited:** `(mask & (1 << i)) !== 0` [cite: 487]
2.  **Mark `i`-th item as visited (Set bit to 1):** `newMask = mask | (1 << i)`
3.  **Clear `i`-th item (Set bit to 0):** `newMask = mask & ~(1 << i)`

### Assignment Problem (The Bitmask Entry Problem)

#### 🧠 Understand:
\\(N\\) persons aur \\(N\\) jobs hain. Kisi person ko job assign karne ka cost index coordinate matrix `cost[i][j]` se visible hota hai. Har person ko exactly ek job milna chahiye. Minimum cost nikalni hai assignment complete karne ki. (Constraint: \\(N \le 16\\) \\(\rightarrow\\) Hints for Bitmask DP!)

```javascript
function minCostAssignment(costMatrix) {
    const n = costMatrix.length;
    const totalStates = 1 << n; // 2^n states
    const dp = new Array(totalStates).fill(Infinity);
    
    dp = 0; // Cost is 0 with no jobs assigned

    // Iterate through all bit states
    for (let mask = 0; mask < totalStates; mask++) {
        // Count total set bits to identify which person we are currently assigning [cite: 411, 731, 762, 777, 788]
        const personIndex = countSetBits(mask); 
        
        if (personIndex >= n) continue;

        for (let job = 0; j < n; j++) {
            // If the job is not yet assigned in the current mask
            if ((mask & (1 << job)) === 0) {
                const nextState = mask | (1 << job);
                dp[nextState] = Math.min(dp[nextState], dp[mask] + costMatrix[personIndex][job]);
            }
        }
    }

    return dp[totalStates - 1]; // All jobs assigned state
}

function countSetBits(num) {
    let count = 0;
    while (num > 0) {
        count += (num & 1); // [cite: 485, 487]
        num >>= 1;
    }
    return count;
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(2^N \cdot N)\\)**, Space: **\\(\mathcal{O}(2^N)\\)** tracking bounds.

---

## SDE COMPARATIVE PARADIGM BATTLES ⚔️

Bacho, is matrix cheat-sheet ko hamesha dimaag mein fit rakhna:

| Dynamic Battle | First Competitor | Second Competitor | Winning Rule (The Crux) |
| :--- | :--- | :--- | :--- |
| **LCS vs. Substring** [cite: 414, 415, 733, 764, 765, 780, 781, 791, 792, 796] | **LCS:** Characters order matches but non-contiguous allowed [cite: 81]. | **Substring:** Characters must be contiguous. | Subsequence matches carry forward previous results diagonally [cite: 119]; Substring mismatch resets state immediately to `0` [cite: 739]. |
| **Subsequence vs. Partition** [cite: 414, 733] | **Subsequence DP:** Focuses on comparing characters from left-to-right [cite: 414]. | **Partition DP:** Focuses on finding optimal split points inside an interval [cite: 733]. | Subsequence uses linear/2D grids; Partition uses 2D boundary elements looping over range slices [cite: 733]. |
| **Palindromic Subseq vs. Substring** | **Subseq Palindrome (LPS):** LCS with its reversed string [cite: 414, 764, 780, 791, 796]. | **Substring Palindrome:** Boundary matching [cite: 690]. | Substring must be fully contiguous; Subsequence can have gaps between matches [cite: 81, 690]. |
| **DP vs. Backtracking** [cite: 301, 325, 434] | **DP:** Optimizes overlapping calculations using memory cache [cite: 301, 325, 434]. | **Backtracking:** Exhaustive search, explores all paths [cite: 30, 404, 733]. | DP is faster but takes extra space; Backtracking is slow but uses less memory space. |

---

## 9. SDE CORNER: HARDCORE PRACTICAL INTERVIEW PROBLEMS

🚀 **Bacho, ab mixed round start ho chuka hai! Pehle algorithm select karo, tabhi code likhna!**

---

### Problem A (Medium): Word Break (LeetCode 139) [cite: 418, 430, 431, 733, 767, 784, 795, 796]
*Determine if string `s` can be partitioned into a space-separated sequence of dictionary words.*

#### 🧠 Diagnostics:
*   *Choices:* Har index `i` par choices hain ki kis partition cut point `j` se string segment complete ho raha hai.
*   *Pattern:* **Prefix Word Break DP Pattern** [cite: 431]!

```javascript
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const n = s.length;
    const dp = new Array(n + 1).fill(false);
    dp = true;

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < i; j++) {
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[n];
}
```

---

### Problem B (Hard): Longest Increasing Subsequence (LeetCode 300) [cite: 402, 414, 764, 780, 791, 796]
*Find the length of the longest strictly increasing subsequence in an unsorted array.*

#### 🧠 Diagnostics:
*   *Observation:* Array limits \\(N \le 10^5\\) quadratic \\(\mathcal{O}(N^2)\\) solutions ko instantly crash kar degi! Hum ya toh linear ya log-linear binary search checks maintain karenge [cite: 51, 52].
*   *Pattern:* **LIS Optimized patience sorting [cite: 506]**!

```javascript
function lengthOfLIS(nums) {
    return lengthOfLISOptimized(nums); // Runs in O(N log N) successfully [cite: 506]!
}
```

---

## 10. COMMON SDE TRAPS & BUGS TO AVOID ⚠️

1.  **Duplicate row allocations in 2D grids (Reference bug):**
    `Array(m).fill(Array(n))` likhna [cite: 308]. Isse pure row pointers internally duplicate ho jayenge [cite: 308]. Hamesha use karein:
    `Array.from({ length: m }, () => new Array(n).fill(0))` [cite: m].
2.  **Incorrect indexing on empty character strings:**
    Substring boundaries or length bounds updates ke coordinates me `i` or `i - 1` offsets mix up karna [cite: 41, 131, 284].
3.  **Forgetting BigInt limits for assignment matrices:**
    State compression DP calculations karte waqt constraints large hone par bitwise operations integers precision range break exceptions check missing rakhna [cite: 519].

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Optimized \\(\mathcal{O}(N \log N)\\) patience sorting for Longest Increasing Subsequence [cite: 506].
* Edit Distance (Levenshtein) choices and space-optimized tabulation states [cite: 414, 733].
* Longest Palindromic Subsequence and its mathematical connection to LCS [cite: 414, 733].
* Boundary partitions inside interval constraints and Matrix Chain Multiplications [cite: 733].
* State compression and bitmask mapping principles [cite: 403, 413, 731, 762, 763, 777, 778, 788, 789].

---

### SDE Practice Roadmap:
1.  Solve LeetCode 300 *Longest Increasing Subsequence* using Patience Sorting [cite: 402, 506].
2.  Solve LeetCode 72 *Edit Distance* with Space Tabulations [cite: 414, 431].
3.  Implement LeetCode 139 *Word Break* manually [cite: 430, 431].

---

⏮️ **Pichle Chapters ka Connection:** Chapter 21 ke recursive backtracking call stacks memory allocations ko humne dynamic parameters (strings alignments, interval slices, and binary bit masks) par translate kiya [cite: 290, 488, 501, 733].
