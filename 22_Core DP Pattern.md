**Arey bacho! Jaldi se apni-apni seats par baith jao aur blackboard par dhyan seedhe focus karo.** 

Pichle chapter mein humne **Dynamic Programming (Chapter 21)** ke basic mental model ko unlock kiya tha. Humne seekha ki kaise normal recursion jab andhon ki tarah baar-baar same calculations karta hai (Overlapping Subproblems), toh hum ek **Memory Shield (Cache)** lagakar use protect karte hain. Humne seekha ki kaise hum State, Choice, Transition, aur Base Case ke framework se bade-bade recursive problems ko chote parts mein break karte hain.

Lekin bacho, interview room mein jab tum baithoge, toh interviewer tumse direct standard Fibonacci nahi puchega. Woh tumhe ek complex problem statement dega, aur tumhe khud pehchanna padega ki isme kaunsa **DP Pattern** fit hota hai!

Aaj hum dimaag ke saare kapde kholkar, top 5 most common interview DP patterns ko basic se lekar advanced (SDE-level) tak deeply breakdown karenge. Hum har pattern ko decode karenge, transition formulas ko mathematically derive karenge, aur un common JavaScript reference traps se bachenge jo aksar code ko crash kar dete hain.

Chalo bacho, shuru karte hain hamara dhasu—**Chapter 22: Core DP Patterns**! 🚀

---

## PATTERN 1: 1D DP (DECISION MAKING AT LINEAR STEP)

Bacho, 1D DP tab use hota hai jab hum ek single linear array ya sequence mein aage badh rahe hote hain, aur har index `i` par humara decision sirf **pichle kuch discrete states** (`i-1`, `i-2`, etc.) par depend karta hai.

```
                      1D DP State Dependency:
                      [ dp[i-2] ] ──┐
                                    ├───► [ dp[i] ] (Derives from previous steps)
                      [ dp[i-1] ] ──┘
```

Chalo direct pichle chapter ke **House Robber (LeetCode 198)** ke concept ko ek level upar lekar chalte hain aur iske dhasu variant ko decode karte hain.

---

### House Robber II (LeetCode 213) — Cyclic 1D DP 🏠🔄

#### 🧠 Understand:
Pichle House Robber problem mein ghar ek seedhi line mein the. Lekin is baar **saare ghar circular arrangement** mein hain! Iska matlab, pehla ghar (`House 0`) aur aakhri ghar (`House n-1`) aapas mein padosi (neighbors) hain! Tum dono mein ek sath chori nahi kar sakte.

```
                         Circular Houses Grid:
                             [House 0] ─── [House 1]
                                 │             │
                             [House 3] ─── [House 2]
                     (If you rob House 0, you CANNOT rob House 3!)
```

#### ❌ Why basic 1D DP fails here?
Standard 1D DP ko ye nahi pata hota ki humne `House 0` ko rob kiya hai ya nahi jab hum `House n-1` par pahunchte hain. Agar hum simple transition chalayenge, toh ho sakta hai hum galti se pehle aur aakhri ghar dono ko loot lein, jo rule ke khilaaf hai!

#### 💡 SDE Observation & Strategy:
Chalo is cyclic dependency ko simple linear subproblems mein break karte hain!
*   **Case A:** Agar hum pehle ghar (`House 0`) ko rob karte hain, toh hum aakhri ghar (`House n-1`) ko touch bhi nahi kar sakte. Yaani humara search space ho gaya: `[0 to n-2]`.
*   **Case B:** Agar hum aakhri ghar (`House n-1`) ko rob karte hain, toh hum pehle ghar (`House 0`) ko nahi rob kar sakte. Yaani humara search space ho gaya: `[1 to n-1]`.

> **The Master Trick:** Hum do independent linear House Robber runs chalayenge—ek `0 to n-2` tak aur dusra `1 to n-1` tak. Dono mein se jo maximum output dega, wahi hamara global optimal answer hoga!

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **State kya hai?** Let `dp[i]` be the maximum money robbed from index `start` to `i`.
2.  **Choice kya hai?** Index `i` par khade hokar humare paas 2 choices hain:
    *   **Rob current house `i`:** Pichle padosi ko skip karo, so total = `nums[i] + dp[i-2]`.
    *   **Skip current house `i`:** Pichle ghar tak jo max loot mili wahi carry forward karo, so total = `dp[i-1]`.
3.  **Transition kya hai?** 
    \\[dp[i] = max(dp[i-1], nums[i] + dp[i-2])\\]
4.  **Base Case kya hai?**
    *   `dp[start] = nums[start]`
    *   `dp[start+1] = max(nums[start], nums[start+1])`
5.  **Answer kahan milega?** `max(robLinear(0, n-2), robLinear(1, n-1))`

---

#### 💻 JavaScript Code (Highly Optimized):
```javascript
function robCircular(nums) {
    const n = nums.length;
    if (n === 0) return 0;
    if (n === 1) return nums; // Standard edge case handling

    // Run standard linear house robber on both sliced options
    return Math.max(
        robLinear(nums, 0, n - 2),
        robLinear(nums, 1, n - 1)
    );
}

function robLinear(nums, start, end) {
    if (start === end) return nums[start];
    
    // Space Optimization: We only need two variables instead of a full DP array!
    let prev2 = nums[start];
    let prev1 = Math.max(nums[start], nums[start + 1]);

    for (let i = start + 2; i <= end; i++) {
        const curr = Math.max(prev1, nums[i] + prev2);
        prev2 = prev1;
        prev1 = curr;
    }

    return prev1;
}
```

#### 🔍 Complete Dry Run:
Maan lo `nums =`, `n = 3`.
*   **Run 1 (0 to 1):** `nums` subset ``
    *   `prev2 = 2`
    *   `prev1 = max(2, 3) = 3`
    *   Loop `i = 2` doesn't run because `start + 2 = 2 > end (1)`.
    *   Returns `3`.
*   **Run 2 (1 to 2):** `nums` subset ``
    *   `prev2 = 3`
    *   `prev1 = max(3, 2) = 3`
    *   Returns `3`.
*   **Global Max:** `max(3, 3) = 3`.
*   **Complexity:** Time: **O(n)**, Space: **O(1)** auxiliary. Beautiful!

---

## PATTERN 2: GRID DP (2D STATE SPACE NAVIGATION)

Bacho, Grid DP ke problems mein humare paas ek 2D matrix/grid hoti hai, aur humein top-left corner se bottom-right corner tak move karna hota hai, ya fir cells par choices banakar min/max parameters evaluate karne hote hain.

---

### Unique Paths II (LeetCode 63) — Grid with Obstacles 🚧

#### 🧠 Understand:
Humein ek `m x n` size ki matrix grid di gayi hai. Hum sirf **Down (Niche)** aur **Right (Aage)** move kar sakte hain. Grid mein kuch cells par obstacles (`1`) hain jahan se jana completely blocked hai. Humein start `(0,0)` se end `(m-1, n-1)` tak ke total unique paths count karne hain.

```
                         Grid Navigation Matrix:
                            [ S ] ───► [   ] ───► [   ]
                              │          │          │
                              ▼          ▼          ▼
                            [   ] ───► [ X ] ───► [   ]  ◄── Obstacle at (1, 1)!
                              │          │          │
                              ▼          ▼          ▼
                            [   ] ───► [   ] ───► [ E ]
```

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **`dp[i][j]` ka matlab kya hai?**  
    `dp[i][j]` represent karta hai **total unique paths** starting from source `(0,0)` to reach coordinate `(i, j)`.
2.  **Choice kya hai?**  
    Coordinate `(i, j)` par hum sirf do pichle cell points se aa sakte hain:
    *   Pichle Row se niche aakar: `(i-1, j)`
    *   Pichle Column se right aakar: `(i, j-1)`
3.  **Transition kya hai?**
    *   Agar current cell `grid[i][j] === 1` (obstacle), toh `dp[i][j] = 0` (unreachable).
    *   Else:
        \\[dp[i][j] = dp[i-1][j] + dp[i][j-1]\\]
4.  **Base Case kya hai?**  
    Starting cell `dp = grid === 1 ? 0 : 1`.
5.  **Answer kahan milega?**  
    Bottom-right corner index: `dp[m-1][n-1]`.

---

#### ❌ The JavaScript 2D Array Allocation Trap 🚨
Bacho, dhyan se whiteboard par dhyan do. JavaScript mein 2D array banate waqt ye galti kabhi mat karna:
```javascript
// ❌ CRITICAL BUG: Do NOT use fill with Array nested references!
let dp = Array(m).fill(Array(n).fill(0));
```
*Why?* Kyunki JavaScript `Array.fill` mein nested arrays ka **same memory reference** copy kar deta hai. Agar tum `dp = 5` karoge, toh poore column `0` ke saare elements dynamic copy reference ke karan automatic `5` ho jayenge!

**The Correct Way to Allocate 2D DP Table:**
```javascript
// ✅ CORRECT: Safe independent rows allocation
let dp = Array.from({ length: m }, () => new Array(n).fill(0));
```

---

#### 💻 JavaScript Code (Standard Tabulation + Space Optimization):
Bacho, dhyan se socho! `dp[i][j]` calculate karne ke liye humein sirf **pichli row** (`dp[i-1]`) aur **current row ka pichla element** (`dp[i][j-1]`) chahiye. Iska matlab hum pure 2D table ko replace karke sirf **1D array of size `n`** se space optimize kar sakte hain!

```javascript
function uniquePathsWithObstacles(obstacleGrid) {
    const m = obstacleGrid.length;
    const n = obstacleGrid.length;

    // If starting cell or ending cell has an obstacle, paths are impossible!
    if (obstacleGrid === 1 || obstacleGrid[m - 1][n - 1] === 1) return 0;

    // Space Optimized: Create a 1D DP table of size n
    const dp = new Array(n).fill(0);
    
    // Base Case initialization
    dp = 1; 

    for (let r = 0; r < m; r++) {
        for (let c = 0; c < n; c++) {
            if (obstacleGrid[r][c] === 1) {
                dp[c] = 0; // Blocked cell, paths become 0
            } else if (c > 0) {
                // dp[c] = dp[c] (value from previous row) + dp[c-1] (current row left cell value)
                dp[c] += dp[c - 1]; 
            }
        }
    }

    return dp[n - 1]; // Result state
}
```

#### 🔍 Complete 1D Table Dry Run:
Let's trace on Grid: `[ ,,  ]`, `m = 3, n = 3`.
*   **Initial:** `dp =`
*   **Row `r = 0`:**
    *   `c = 0`: `obstacleGrid === 0`, `c === 0` (no change). `dp = 1`.
    *   `c = 1`: `obstacleGrid === 0`. `dp += dp => 0 + 1 = 1`. `dp =`
    *   `c = 2`: `obstacleGrid === 0`. `dp += dp => 0 + 1 = 1`. `dp =`
*   **Row `r = 1`:**
    *   `c = 0`: `obstacleGrid === 0`. `dp = 1`.
    *   `c = 1`: `obstacleGrid === 1` (Obstacle!). `dp = 0`. `dp =`
    *   `c = 2`: `obstacleGrid === 0`. `dp += dp => 1 + 0 = 1`. `dp =`
*   **Row `r = 2`:**
    *   `c = 0`: `obstacleGrid === 0`. `dp = 1`.
    *   `c = 1`: `obstacleGrid === 0`. `dp += dp => 0 + 1 = 1`. `dp =`
    *   `c = 2`: `obstacleGrid === 0`. `dp += dp => 1 + 1 = 2`. `dp =`
*   **Returns:** `dp = 2`. Correct output!
*   **Complexity:** Time: **O(m × n)**, Space: **O(n)** auxiliary. Fully optimized!

---

## PATTERN 3: KNAPSACK PATTERN (0/1 KNAPSACK & SUBSET DECOMPOSITION)

Bacho, Knapsack DP ka core concept bada simple hai: "Humein kuch items diye hain, aur ek defined **Capacity** di hai. Har item ke liye humare paas binary decision choice hoti hai—ya toh us item ko **Le lo (Take)** ya fir **Chod do (Skip)**."

---

### 0/1 Knapsack Problem (Standard SDE Blueprint 🎒)

#### 🧠 Understand:
Humein `N` items ke values aur weights diye hain. Humein unhe ek bag (knapsack) mein dalna hai jiski max capacity `W` hai. Goal hai value maximize karna bina weight limit cross kiye.

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **`dp[i][w]` ka matlab kya hai?**  
    `dp[i][w]` represent karta hai **maximum value** jo hum nikal sakte hain considering elements from index `0 to i` with a strictly remaining bag capacity `w`.
2.  **Choice kya hai?**  
    Har item `i` par humare paas 2 choices hain:
    *   **Skip item `i`:** Capacity same rahegi, value change nahi hogi → `dp[i-1][w]`.
    *   **Take item `i` (only if weight[i] <= w):** Capacity kam ho jayegi, value badh jayegi → `values[i] + dp[i-1][w - weights[i]]`.
3.  **Transition kya hai?**
    \\[dp[i][w] = max(dp[i-1][w], values[i] + dp[i-1][w - weights[i]])\\]
4.  **Base Case kya hai?**  
    If index `i < 0` ya remaining capacity `w === 0`, value generated is `0`.
5.  **Answer kahan milega?**  
    `dp[n-1][W]`.

---

#### 💻 JavaScript Code (Optimized to 1D Array):
Bacho, dhyan se socho! `dp[i][w]` ko calculate karne ke liye humein sirf pichli row (`i-1`) ke chote weights (`w - weights[i]`) ki zaroorat hoti hai. Agar hum **right-to-left (reverse loop)** chalayein, toh hum single row mein pichli state values ko overwrite hone se bacha sakte hain!

```javascript
function knapsack01(values, weights, W) {
    const n = values.length;
    // dp[w] stores max value generated for bag capacity w
    const dp = new Array(W + 1).fill(0);

    for (let i = 0; i < n; i++) {
        // Reverse loop is mandatory to prevent reuse of same item in 0/1 Knapsack!
        for (let w = W; w >= weights[i]; w--) {
            dp[w] = Math.max(dp[w], values[i] + dp[w - weights[i]]);
        }
    }

    return dp[W];
}
```
*   **Complexity:** Time: **O(N × W)**, Space: **O(W)** auxiliary.

---

### Partition Equal Subset Sum (LeetCode 416)

#### 🧠 Understand:
Humein ek positive integers ka array `nums` diya hai. Humein batana hai ki kya hum is array ko **do parts (subsets)** mein partition kar sakte hain jinka elements sum strictly equal ho.

```
                     Partition Equal Subset Sum:
                     nums = => Total Sum = 22.
                     Target Subset Sum = 22 / 2 = 11.
                     Can we find a subset with sum 11?
                     Yes, subset sums to 11! Return true.
```

#### 💡 SDE Observation:
*   Agar total array elements ka sum **odd** hai, toh use equal halves mein partition karna mathematically impossible hai. Instant `return false`!
*   Agar sum **even** hai, toh humari problem convert ho jati hai: **"Kya array mein koi aisa subset hai jiska sum target = TotalSum / 2 ke barabar ho?"**
*   *This is exactly Knapsack!* Har number ke liye binary choice hai: Take vs Skip.

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **`dp[i][j]` ka matlab kya hai?**  
    `dp[i][j]` is a **boolean state** (`true/false`) jo batata hai ki kya array index `0 to i` tak ke elements se target sum `j` banana possible hai.
2.  **Choice kya hai?**  
    Index `i` ke element `nums[i]` par khade hokar 2 choices hain:
    *   **Skip element `i`:** Target sum pichle elements se hi banana hoga → `dp[i-1][j]`.
    *   **Take element `i`:** Remaining target sum banayenge → `dp[i-1][j - nums[i]]`.
3.  **Transition kya hai?**
    \\[dp[i][j] = dp[i-1][j]  OR  dp[i-1][j - nums[i]]\\]
4.  **Base Case kya hai?**  
    *   If target sum `j === 0`, return `true` (empty subset can always form sum 0).
    *   If `index < 0` but target sum `j > 0`, return `false`.
5.  **Answer kahan milega?**  
    `dp[n-1][target]`.

---

#### 💻 JavaScript Code (Optimized to 1D Array):
```javascript
function canPartition(nums) {
    const totalSum = nums.reduce((acc, curr) => acc + curr, 0); //
    
    // Odd sum cannot be partitioned equally
    if (totalSum % 2 !== 0) return false;

    const target = totalSum / 2;
    // dp[j] is true if sum j is possible
    const dp = new Array(target + 1).fill(false);
    
    // Base Case: Sum 0 is always possible
    dp = true;

    for (let num of nums) {
        // Reverse loop to prevent using same number multiple times!
        for (let j = target; j >= num; j--) {
            if (dp[j - num] === true) {
                dp[j] = true;
            }
        }
    }

    return dp[target];
}
```
*   **Complexity:** Time: **O(N × target)**, Space: **O(target)** auxiliary. Super clean!

---

## PATTERN 4: UNBOUNDED KNAPSACK (REUSABLE CHOICE MATCH)

Bacho, Unbounded Knapsack aur 0/1 Knapsack mein strictly ek hi difference hai. 
*   **0/1 Knapsack:** Har item ko hum sirf **ek hi baar** use kar sakte hain. Isiliye looping hamesha **reverse index** par chalti hai!
*   **Unbounded Knapsack:** Humein har item ki **unlimited supply** di hoti hai! Hum ek hi item ko baar-baar reuse kar sakte hain. Isiliye loop ka direction dynamic forwarding (left-to-right) ho jata hai!

---

### Coin Change II (LeetCode 518) — Total Number of Ways

#### 🧠 Understand:
Humein ek array `coins` aur ek `amount` diya hai. Humein total ways (combinations) count karne hain jisse hum coins ko unlimted times use karke woh target amount bana sakein.

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **`dp[j]` ka matlab kya hai?**  
    `dp[j]` represent karta hai **total unique combinations** of coins jo target amount `j` bana sakte hain.
2.  **Choice kya hai?**  
    Kisi coin `c` par khade hokar humare paas options hain:
    *   Hum target `j` banane ke liye coin `c` ko infinite times include kar sakte hain. So if we choose coin `c`, the ways increase by the ways to form amount `j - c`.
3.  **Transition kya hai?**
    \\[dp[j] = dp[j] + dp[j - c]\\]
4.  **Base Case kya hai?**  
    `dp = 1` (Amount 0 banane ka strictly 1 hi tarika hai—kuch bhi mat select karo!).
5.  **Answer kahan milega?**  
    `dp[amount]`.

---

#### ❌ SDE Trap: The Loop Order Dilemma (Permutations vs. Combinations 🚨)
Bacho, dhyan se whiteboard par bani in do algorithms ko dekho, ye pure technical interviews ka absolute core point hai:

```
          [ Standard Combination Loop ]               [ Standard Permutation Loop ]
          for (let coin of coins) {                    for (let j = 1; j <= amount; j++) {
              for (let j = coin; j <= amount; j++) {       for (let coin of coins) {
                  dp[j] += dp[j - coin];                       if (j - coin >= 0) dp[j] += dp[j - coin];
              }                                            }
          }                                            }
```
*   **Coins Outside Loop (Combination):** Jab coin loop bahar hota hai, toh pehle coin `2` ke saare options explore ho kar block honge, fir coin `5` explore hoga. Isse hamesha sorted unique groups banenge jaise `` and never duplicate as ``. This returns **Combinations** (Coin Change II)!
*   **Amount Outside Loop (Permutations):** Jab amount loop bahar hota hai, toh har amount `j` ke liye saare coins dobara line se scan hote hain. Isse duplicate arrays combinations generate honge jaise `` and ``. This returns **Permutations / Total arrangements** (Combination Sum IV)!

---

#### 💻 JavaScript Code (Coin Change II - Combinations):
```javascript
function change(amount, coins) {
    // dp[j] holds total combinations to form amount j
    const dp = new Array(amount + 1).fill(0);
    
    // Base Case initialization
    dp = 1;

    // Outer Loop: Coins (Ensures combinations, prevents duplicates!)
    for (let coin of coins) {
        // Inner Loop: Left-to-right (Allows infinite reuse!)
        for (let j = coin; j <= amount; j++) {
            dp[j] += dp[j - coin]; // Add choices
        }
    }

    return dp[amount]; // Final state
}
```
*   **Complexity:** Time: **O(amount × N)**, Space: **O(amount)** auxiliary table.

---

## PATTERN 5: SUBSEQUENCE / SEQUENCE DP (TWO STRING ALIGNMENTS)

Bacho, Sequence DP ke problems mein humein do strings ya sequences diye hote hain, aur hum unke indices ko character-by-character align aur compare karke choices banate hain.

---

### Longest Common Subsequence (LeetCode 1143)

#### 🧠 Understand:
Humein do strings `text1` aur `text2` di gayi hain. Humein in dono ke beech ka **Longest Common Subsequence (LCS)** ka count return karna hai. Subsequence ka matlab hai characters ka wahi sequence jo original string mein bikhra ho par order strictly maintain kare (jaise `abc` is a subsequence of `axbxc`).

```
                     LCS Visualization Table:
                     text1 = "abcde", text2 = "ace"
                     
                               a   c   e
                           a [ 1,  1,  1 ]
                           b [ 1,  1,  1 ]
                           c [ 1,  2,  2 ]
                           d [ 1,  2,  2 ]
                           e [ 1,  2,  3 ]  ◄── Answer is 3 (Sequence: "ace")!
```

---

#### ✏️ Deriving the States (The 5 Questions):
1.  **`dp[i][j]` ka matlab kya hai?**  
    `dp[i][j]` represent karta hai **LCS length** considering substring `text1[0...i-1]` and `text2[0...j-1]`.
2.  **Choice kya hai?**  
    Pointers index `i` on `text1` and `j` on `text2` par khade hokar:
    *   **If characters match (`text1[i-1] === text2[j-1]`):** Dono pointers ko safe move karo, match size 1 se increment hoga → `1 + dp[i-1][j-1]`.
    *   **If characters mismatch:** Ek pointer ko skip karke check karo, and take maximum → `max(dp[i-1][j], dp[i][j-1])`.
3.  **Transition kya hai?**
    \\[dp[i][j] = \begin{cases} 1 + dp[i-1][j-1] & if  text1[i-1] == text2[j-1] \\ max(dp[i-1][j], dp[i][j-1]) & otherwise \end{cases}\\]
4.  **Base Case kya hai?**  
    If length of any string is `0` (`i === 0` or `j === 0`), LCS is `0`.
5.  **Answer kahan milega?**  
    `dp[m][n]` (where `m` is `text1.length` and `n` is `text2.length`).

---

#### 💻 JavaScript Code (Tabulation + Answer Reconstruction 💎):
SDE-level interviews mein interviewer code complete hone ke baad aksar kehta hai: *"Chalo, mujhe sirf count nahi, actual string value reconstruct karke return karo!"* 

Hum matrix table par back-trace pointer mapping ka use karke actual string nikal sakte hain!

```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length;
    const n = text2.length;

    // Step 1: Create safe independent rows for 2D DP Table
    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

    // Step 2: Tabulation step
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1]; // Elements match
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]); // Mismatch
            }
        }
    }

    const lcsLength = dp[m][n];

    // Step 3: Reconstruction of Actual LCS String Value 💎
    let lcsString = [];
    let r = m, c = n;
    while (r > 0 && c > 0) {
        if (text1[r - 1] === text2[c - 1]) {
            lcsString.push(text1[r - 1]); // Element was selected
            r--;
            c--;
        } else if (dp[r - 1][c] >= dp[r][c - 1]) {
            r--; // Shift upwards
        } else {
            c--; // Shift leftwards
        }
    }

    const reconstructedLCS = lcsString.reverse().join('');
    console.log(`Reconstructed LCS Sequence: "${reconstructedLCS}"`);

    return lcsLength;
}
```

#### 🔍 Complete Tabulation Grid Dry Run:
Let's trace `text1 = "abc"`, `text2 = "ac"`. `m = 3, n = 2`.
*   **Grid (i=0, j=0 to 2) initialized with 0.**
*   **`i = 1 (a)`:**
    *   `j = 1 (a)`: Matches! `dp = 1 + dp = 1`.
    *   `j = 2 (c)`: Mismatches! `dp = max(dp, dp) = max(0, 1) = 1`.
*   **`i = 2 (b)`:**
    *   `j = 1 (a)`: Mismatches! `dp = max(dp, dp) = max(1, 0) = 1`.
    *   `j = 2 (c)`: Mismatches! `dp = max(dp, dp) = max(1, 1) = 1`.
*   **`i = 3 (c)`:**
    *   `j = 1 (a)`: Mismatches! `dp = max(dp, dp) = max(1, 0) = 1`.
    *   `j = 2 (c)`: Matches! `dp = 1 + dp = 1 + 1 = 2`.
*   **Reconstruction Walkway starting at (3, 2):**
    *   `text1 === text2` ('c' === 'c'). Push 'c'. Move to (2, 1).
    *   `text1 !== text2` ('b' !== 'a'). `dp (1) >= dp (0)`. Move to (1, 1).
    *   `text1 === text2` ('a' === 'a'). Push 'a'. Move to (0, 0).
    *   Loop terminates. Reverse array `['c', 'a']` → `['a', 'c']`.
    *   Outputs sequence: `"ac"`. Absolutely beautiful!
*   **Complexity:** Time: **O(m × n)**, Space: **O(m × n)**.

---

## SDE COMPARATIVE PARADIGM BATTLES ⚔️

Bacho, whiteboard par bani is comparative breakdown matrix sheets ko dhyan se dimaag mein lock kar lo:

| Dynamic Battle | First Competitor | Second Competitor | The Core Difference (Winning Rule) |
| :--- | :--- | :--- | :--- |
| **0/1 vs. Unbounded** | **0/1 Knapsack:** Items are strictly unique. | **Unbounded Knapsack:** Items have infinite supplies. | 0/1 uses reverse loops (`W to weights[i]`); Unbounded uses forward loops (`coin to amount`). |
| **Subsequence vs. Substring** | **Subsequence:** Characters order must match. | **Substring:** Characters must be strictly continuous. | Subsequence matches inherit diagonally (`1+dp[i-1][j-1]`); Substring mismatch resets state immediately to `0`. |
| **2D DP vs. Space Optimized** | **2D Array:** Full tabular matrices. | **1D Array:** Optimized running variables. | 2D preserves past states history; 1D only keeps what's necessary for the immediate step. |

---

## PATTERN-RECOGNITION FLOWCHART 🗺️

**Beta, interview room mein dhyan se questions ko padhkar in check constraints ko locate karna:**

```
                            DP PATTERN RECOGNITION CUES
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
  1. Comparing Sequences           2. Reuse / Infinite              3. Multi-Choices
  Two strings, LCS, edit-          "Unlimited supply",              "Should I take or
  distance -> LCS DP.          Unbounded Knapsack.     skip?" -> Knapsack.
```

---

## THE CHALLENGE ROOM: COMPELLED MIXED QUESTIONS 🧠

🚀 **Chalo bacho, Mixed round shuru ho chuka hai! Tumhein pehle pehchanna hai ki problem kis pattern ki hai, tabhi coding approach par jana!**

---

### Problem A (Medium): Longest Increasing Subsequence (LeetCode 300)
*Unsorted integer array ka strictly increasing subsequence ki maximum length return karo.*

#### 🧠 Diagnostics:
*   *Choices Analysis:* Kya hum do sequences compare kar rahe hain? Nahi, sirf ek sequence hai!
*   *Observation:* Har index `i` par, humara selection constraint kis pe depend karta hai? Pichle saare indices `j < i` par! Agar `nums[i] > nums[j]`, toh hum `dp[j] + 1` se length badha sakte hain.
*   *Pattern Choice:* **LIS DP Variant (quadratic O(n^2) baseline)**!

```javascript
function lengthOfLIS(nums) {
    if (nums.length === 0) return 0;
    
    const n = nums.length;
    // dp[i] holds length of LIS ending at index i
    const dp = new Array(n).fill(1); 
    let maxLIS = 1;

    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1); // Transition
            }
        }
        maxLIS = Math.max(maxLIS, dp[i]);
    }

    return maxLIS;
}
```
*   **Complexity:** Time: **O(n^2)**, Space: **O(n)** auxiliary.

---

### Problem B (Hard): Target Sum (LeetCode 494)

#### 🧠 Diagnostics:
*   *Choices Analysis:* Humein integers array aur ek `target` diya hai. Har number ke pehle hum `+` ya `-` sign lagakar target sum banana chahte hain.
*   *Mathematical Observation:*
    Maan lo humne kuch elements ko positive liya (Subset S_1) aur kuch ko negative (Subset S_2).
    \\[Sum(S_1) - Sum(S_2) = target\\]
    \\[Sum(S_1) + Sum(S_2) = TotalSum\\]
    Dono ko add karo:
    \\[2 · Sum(S_1) = TotalSum + target => Sum(S_1) = TotalSum + target/2\\]
*   *The Magic:* Yeh problem strictly convert ho gayi **Subset Sum Problem (Knapsack)** mein! Humein ek subset dhoondhna hai jiska target sum TotalSum + target/2 ho!
*   *Pattern Choice:* **0/1 Knapsack (Subset Sum Variant)**!

```javascript
function findTargetSumWays(nums, target) {
    const totalSum = nums.reduce((acc, curr) => acc + curr, 0); //
    
    // Boundary conditions
    if (Math.abs(target) > totalSum) return 0;
    if ((totalSum + target) % 2 !== 0) return 0;

    const subsetTarget = (totalSum + target) / 2;
    if (subsetTarget < 0) return 0;

    // dp[j] stores number of ways to make sum j
    const dp = new Array(subsetTarget + 1).fill(0);
    dp = 1; // Base case

    for (let num of nums) {
        // Reverse loop to prevent using same number multiple times!
        for (let j = subsetTarget; j >= num; j--) {
            dp[j] += dp[j - num]; // Add ways
        }
    }

    return dp[subsetTarget];
}
```
*   **Complexity:** Time: **O(N × subsetTarget)**, Space: **O(subsetTarget)** auxiliary.

---

## 10. COMMON SDE TRAPS & BUGS TO AVOID ⚠️

Technical evaluation tests mein in 3 classic bugs se hamesha bacho:

1.  **Shared nested array row references:**
    JavaScript array initialization par `Array(r).fill(Array(c))` likh dena. Hamesha `Array.from` pattern se separate independent rows construct karein.
2.  **Wrong Loop Direction in Knapsack Variants:**
    0/1 Knapsack ko forward directions (`0 to W`) mein loop out kar dena. Isse single element multiple times select hokar unbounded ban jata hai! 0/1 mein strictly **reverse direction loop** chalayein.
3.  **Accidental Floating Point Divisors:**
    Indices calculations par decimal math operations use karna. JavaScript standard floating division follow karta hai, so index float check bypass karne ke liye hamesha `Math.floor` or similar explicit truncation options apply karein.

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Standard 1D and cyclic subproblems boundary splits.
* Safe multi-dimensional array mapping blocks in V8 environments.
* 0/1 vs. Unbounded loops direction properties.
* Sequence alignment dynamic comparisons (LCS, LIS, Subset sums).

### Mastered DP Patterns:
* **The 1D Optimized previous states** shift pointers mechanics.
* **Combinations vs Permutations loop alignments**.
* **LCS Back-trace pointers reconstruction walkway**.

---

### SDE Practice Roadmap:
1.  Solve LeetCode 213 *House Robber II* circular arrays.
2.  Solve LeetCode 63 *Unique Paths II* with dynamic obstacles grids.
3.  Implement *Target Sum* LeetCode 494 manually and trace outputs.

---

