**Arey bacho! Jaldi se apni seats par baith jao aur whiteboard par apna dhyan seedhe focus karo.**  

Pichle chapter mein humne **Heaps & Priority Queues (Chapter 16)** ko poore dhasu tarike se seekha aur dekha ki kaise dynamic elements ke stream mein se absolute minimum ya maximum ko instantly **\\(\mathcal{O}(1)\\)** mein fetch aur **\\(\mathcal{O}(\log n)\\)** mein delete kiya jata hai [cite: 516, 563, 631]. 

Lekin beta, aaj hum computer science ke ek behad dilchasp, creative aur dynamic design paradigm ko kholne ja rahe hain. Ise hum kehte hain—**Greedy Algorithms** [cite: 34, 270, 419]!

*"Sir, humne pichle chapters mein searching aur sorting to seekh li [cite: 283, 285]. Lekin jab real-world optimization problems aati hain, jahan humein maximum profit ya minimum cost nikalna ho, toh hum kaise sochte hain?"*

Bacho, jab life mein ya programming mein humare paas bohot saare choices hote hain, toh sabse pehli natural human tendency kya hoti hai? **"Jo abhi is moment par sabse best lag raha hai, use turant lapak lo (grab it)!"** 

Isi "greedy" nature ko jab hum code mein convert karte hain, toh use Greedy Paradigm kehte hain [cite: 34, 419]. Aaj hum greedy algorithms ko bilkul zero se shuru karke interview level tak le kar jayenge, aur ye seekhenge ki iske decisions ke peeche ka maths aur logic kaam kaise karta hai.

Apna pen aur register nikal lo, aur dhyan whiteboard par lagao! 🚀

---

## 1. THE FOUNDATIONS OF GREEDY: LOCAL VS. GLOBAL OPTIMUM

### Greedy Algorithm Kya Hai? 🧠
Greedy algorithm ek aisa algorithmic design pattern hai jo kisi problem ko solve karne ke liye **har ek step par sabse best local choice (local optimum)** banata hai, bina aage ki future consequences ki parwah kiye [cite: 34, 419]. 

Iska operational philosophy behad simple hai:
> **"Har point par jo locally optimal choice hai, use select karte jao, aur umeed karo ki aakhir mein humein globally optimal (absolute best) solution mil jayega!"** [cite: 34, 419]

```
                              THE GREEDY CHOICE PATH
                                       │
                 ┌─────────────────────┴─────────────────────┐
         Local Optimum                               Global Optimum
     Best decision *right now*                   The absolute best solution
     at the current step.                        for the entire problem.
```

### Local vs. Global Optimum (The Hill Climbing Analogy 🏔️)
Maan lo tum ek pahad par khade ho aur raat ka andhera hai. Tumhara goal hai pahad ke **highest peak (Global Maximum)** par pahunchna.
* **Greedy/Local Approach:** Tum apne aas-paas dekhte ho aur jis direction mein rasta upar ki taraf ja raha hota hai, tum wahan chalna shuru kar dete ho. Har step par tum "locally" height badha rahe ho.
* **The Trap:** Agar tum kisi chote pahad (local peak) par pahunch gaye, toh greedy approach wahan ruk jayegi, kyunki us point se har direction niche ki taraf ja rahi hogi. Lekin actual sabse uncha peak (Global Peak) kahin aur ho sakta hai!

```
                                  Global Peak (Global Optimum)
                                      /\
             Local Peak              /  \
               /\                   /    \
              /  \                 /      \
             /____\_______________/________\
```
Isi tarah, **har problem greedy se solve nahi hoti!** Greedy choice tabhi kaam karti hai jab local choices future decisions ko blocks na karein [cite: 34, 419].

---

### The Two Pillars of Greedy Correctness 🏛️
Kisi bhi problem par greedy algorithm lagane se pehle, usme do properties ka hona mandatory hai [cite: 270]:

1. **Greedy Choice Property:** Hum local decisions ke basis par choice bana sakte hain bina future subproblems ke results ka wait kiye [cite: 34, 419]. Yaani, humara current step decision future choices ke options ko damage nahi karega.
2. **Optimal Substructure:** Agar ek bade problem ka globally optimal solution, uske chote-chote subproblems ke optimal solutions se milkar bana ho, toh use optimal substructure kehte hain [cite: 270, 295].

---

## 2. GREEDY VS. BRUTE FORCE VS. DYNAMIC PROGRAMMING (DP) ⚔️

Bacho, dhyan se is comparative matrix ko dekho, ye interviews mein interviewer ka sabse favorite conceptual query hota hai:

| Parameter | Brute Force 🌋 | Greedy Algorithms 🏔️ | Dynamic Programming (DP) 🎯 |
| :--- | :--- | :--- | :--- |
| **Operational Philosophy** | Saare possible options ko check karo aur best select karo [cite: 34, 388]. | Har step par bina future soche best local choice uthao [cite: 34, 419]. | Subproblems ko solve karke unka result memoize karo aur combine karo [cite: 34, 295]. |
| **Decision Making** | Exhaustive (Explores all paths) [cite: 34, 388]. | One-shot (No backtracking) [cite: 34, 419]. | Explores overlapping subproblems and chooses globally [cite: 34, 295]. |
| **Time Complexity** | Highly expensive, usually **Exponential** (e.g., \\(\mathcal{O}(2^n)\\)). | Super fast, usually **\\(\mathcal{O}(n \log n)\\)** (sorting cost) ya **\\(\mathcal{O}(n)\\)** [cite: 370]. | Balanced, usually **Polynomial** (e.g., \\(\mathcal{O}(n \cdot W)\\)) [cite: 639]. |
| **Guarantee of Global Optimum** | Always guaranteed (since it checks everything) [cite: 34, 388]. | **Not always!** Sirf tabhi jab greedy choice prove ho sake [cite: 4, 326]. | Always guaranteed (if optimal substructure exists) [cite: 295]. |

---

### The Coin Change Counterexample (Kab Greedy Fail Hoti Hai? 🚨)
*Maan lo tum ek shopkeeper ho aur tumhein ek customer ko **change (refund)** dena hai minimum number of coins ke form mein.*

#### Case 1 (Greedy Works Perfectly):
* Denominations available: `` Rs coins.
* Target change required: **Rs 18**.
* **Greedy Choice:** Humesha sabse bada coin select karo jo target se chota ya barabar ho [cite: 270].
  1. Pick `10` Rs coin \\(\rightarrow\\) Remaining = `8` Rs.
  2. Pick `5` Rs coin \\(\rightarrow\\) Remaining = `3` Rs.
  3. Pick `2` Rs coin \\(\rightarrow\\) Remaining = `1` Rs.
  4. Pick `1` Rs coin \\(\rightarrow\\) Remaining = `0` Rs.
  * Total Coins Used: `4` coins (``). This is the absolute minimum!

#### Case 2 (Greedy Fails Horribly ❌):
* Denominations available: `` Rs coins.
* Target change required: **Rs 11**.
* **Greedy Choice Run:**
  1. Pick largest coin `9` Rs \\(\rightarrow\\) Remaining = `2` Rs.
  2. Pick `1` Rs coin \\(\rightarrow\\) Remaining = `1` Rs.
  3. Pick `1` Rs coin \\(\rightarrow\\) Remaining = `0` Rs.
  * Total Coins Used by Greedy: `3` coins (``).
* **The Global Optimum (DP / Brute Force):**
  * We could have selected `` coins!
  * Total Coins Used: **`2` coins**!
  * *Why did Greedy fail?* Kyunki `9` Rs coin uthane ki locally optimal choice ne future subproblem (`11 - 9 = 2`) ko damage kar diya aur use limited coins mein resolve nahi hone diya. Yahan humein **Dynamic Programming** lagana padta [cite: 34].

---

## 3. PROGRESSIVE MASTERCLASS: ALGORITHMS & IMPLEMENTATIONS

🚀 **Arey bacho! Board completely clean hai. Chalo ab simple matching se lekar advanced scheduling problems ko code aur dry run ke sath master karte hain.**

---

### PROBLEM 1 (Easy): Assign Cookies (LeetCode 455)

#### 1. Understand:
Humein do arrays diye hain: `g` (Greed factors of children) aur `s` (Size of cookies). Har bache \\(i\\) ko kam se kam `g[i]` size ki cookie chahiye. Agar cookie size `s[j] >= g[i]`, toh wo bacha khush ho jayega. Humari goal hai **maximum number of children** ko satisfy karna.

#### 2. Brute Force Idea:
Har ek bache ke greed factor ko array `s` mein search karo, aur pehli suitable cookie jo mile, use pick kar lo.
* **Why expensive?** Har choice par humein scan karna padega, jisse duplicates manage karna complex ho jayega aur time complexity \\(\mathcal{O}(n^2)\\) ho jayegi.

#### 3. SDE Observation:
* Agar hum chote greed wale bache ko choti cookie se satisfy kar dein, toh badi cookies bade greed wale bacho ke liye safe bachengi!
* Iske liye hum dono arrays (`g` aur `s`) ko **Sort** kar denge [cite: 270]. Sorting ke baad hamara rasta bilkul linear ho jata hai.

#### 4. Greedy Choice:
Sort both arrays ascending. Assign the smallest possible cookie that can satisfy the current child.

---

#### 5. JavaScript Implementation:
```javascript
function findContentChildren(g, s) {
    // Step 1: Sort children's greed factors and cookie sizes ascending [cite: 59, 285]
    g.sort((a, b) => a - b);
    s.sort((a, b) => a - b);

    let childPtr = 0; // Pointer for children [cite: 518]
    let cookiePtr = 0; // Pointer for cookies [cite: 518]

    // Step 2: Traverse linearly
    while (childPtr < g.length && cookiePtr < s.length) {
        // If current cookie satisfies the current child
        if (s[cookiePtr] >= g[childPtr]) {
            childPtr++; // Move to next child (satisfied!)
        }
        cookiePtr++; // Move to next cookie regardless (used or too small)
    }

    return childPtr; // Total satisfied children
}
```

#### 6. Line-by-Line Explanation:
* `g.sort((a, b) => a - b)`: Greedy sorting step jo elements ko unke scale ke hisab se align karta hai [cite: 59, 285].
* `if (s[cookiePtr] >= g[childPtr])`: Agar cookie ka size greed se bada ya barabar hai, iska matlab child content ho gaya. Hum child pointer ko aage badha dete hain.
* `cookiePtr++`: Agar cookie choti thi ya allocate ho gayi, use hum aage badha kar agle choice par jate hain.

#### 7. Complete Dry Run:
* Input: `g =`, `s =`
* After Sorting: `g =`, `s =`
* Pointers: `childPtr = 0`, `cookiePtr = 0`
  * **Step 1:** `s (1) >= g (1)`? Yes. `childPtr` becomes `1`, `cookiePtr` becomes `1`.
  * **Step 2:** `s (1) >= g (2)`? No (cookie size is too small). `childPtr` stays `1`, `cookiePtr` becomes `2`.
* Loop terminates because `cookiePtr === s.length`.
* Returns `childPtr = 1`. Absolutely correct!

#### 8. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n \log n + m \log m)\\)** dominated by the sorting steps of both arrays [cite: 59, 285].
* **Space Complexity:** **\\(\mathcal{O}(1)\\)** auxiliary (in-place sorting).

---

### PROBLEM 2 (Medium): Fractional Knapsack Problem

#### 1. Understand:
Humein \\(N\\) items diye hain, jinka `weight` aur `value` diya hai. Humein ek Knapsack (bag) diya hai jiski capacity `W` hai. Hum items ko **fractionally (tukdo mein)** bhi cut karke bag mein daal sakte hain. Goal hai bag mein maximum value generate karna.

```
                     Fractional Knapsack Strategy:
                     
       Item 1: Value 60, Weight 10   ==> Value/Weight Ratio = 6
       Item 2: Value 100, Weight 20  ==> Value/Weight Ratio = 5
       Item 3: Value 120, Weight 30  ==> Value/Weight Ratio = 4
```

#### 2. Brute Force:
Har item ke fractions ke combinations check karna mathematically infinite possibilities generate karega, jo computation limits ko break kar dega.

#### 3. SDE Observation:
Kyunki hum items ko fractionally divide kar sakte hain, toh humein kis cheez ko priority deni chahiye?
*"Sir, us item ko pehle uthao jiska per kg rate (value per unit weight) sabse high ho!"*
Exactly bacho! Isi unit metric ko hum kehte hain **Value/Weight Ratio**!

#### 4. Greedy Choice:
Sort all items descending based on their `Value/Weight` ratio. Bag mein pehle high ratio wale items ko poora bharo. Jab space khatam hone lage, toh bache huye item ka fractional portion bag mein fill kar do.

---

#### 5. JavaScript Implementation:
```javascript
class Item {
    constructor(value, weight) {
        this.value = value;
        this.weight = weight;
    }
}

function getFractionalKnapsack(W, items) {
    // Step 1: Calculate ratios and sort items descending based on value/weight ratio
    items.sort((a, b) => {
        const ratioA = a.value / a.weight;
        const ratioB = b.value / b.weight;
        return ratioB - ratioA; // Descending Sort [cite: 59, 285]
    });

    let currentWeight = 0; // Current weight inside bag
    let totalValue = 0.0;  // Total profit value generated

    // Step 2: Iterate over sorted items
    for (let item of items) {
        // If current item can be completely taken
        if (currentWeight + item.weight <= W) {
            currentWeight += item.weight;
            totalValue += item.value;
        } else {
            // Take the fractional part of this item to fill the remaining capacity
            const remainingCapacity = W - currentWeight;
            totalValue += item.value * (remainingCapacity / item.weight);
            break; // Bag is full!
        }
    }

    return totalValue;
}
```

#### 6. Complete Dry Run:
* Capacity `W = 50`, `items = [new Item(60, 10), new Item(100, 20), new Item(120, 30)]`
* Ratios calculated:
  * `Item 1: 60/10 = 6.0`
  * `Item 2: 100/20 = 5.0`
  * `Item 3: 120/30 = 4.0`
* Sorted descending sequence: `Item 1 -> Item 2 -> Item 3`
* Execution:
  * **Step 1 (Item 1):** Weight = `10 <= 50`. Take completely. `currentWeight = 10`, `totalValue = 60`.
  * **Step 2 (Item 2):** Weight = `10 + 20 = 30 <= 50`. Take completely. `currentWeight = 30`, `totalValue = 160`.
  * **Step 3 (Item 3):** Weight = `30 + 30 = 60 > 50`. Remaining space = `50 - 30 = 20`.
    * Take fraction: `120 * (20 / 30) = 80`.
    * `currentWeight` becomes `50`, `totalValue` becomes `160 + 80 = 240`.
* Returns `totalValue = 240`. Absolutely perfect!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n \log n)\\)** dominated by ratio sorting [cite: 59, 285].
* **Space Complexity:** **\\(\mathcal{O}(1)\\)** auxiliary if sorting occurs in-place.

---

### PROBLEM 3 (Medium): Activity Selection / Interval Scheduling (Earliest Finishing Activity)

Bacho, dhyan se suno. Interval based problems (Activity selection, Non-overlapping intervals, ya meeting scheduling) SDE interviews ke top-tier greedy patterns hain. Inhe solve karne ki hum ek standard technique seekhenge [cite: 270, 326].

#### 1. Understand:
Humein \\(N\\) activities di gayi hain unke `start` aur `end` timings ke sath. Ek computer par ek waqt mein sirf ek hi activity run ho sakti hai. Humari goal hai **maximum number of activities** ko perform karna.

```
                  Activities:
                  A1:  A2:  A3:  A4:  A5:
```

#### 2. Brute Force:
Saare possible combinations subsets generate karo aur validation checks chalao. Size \\(N\\) ke array ke liye combinations complexity **\\(\mathcal{O}(2^n)\\)** ho jayegi [cite: 511].

#### 3. Greedy Choice Observation:
*"Sir, hum activities ko kis index key par sort karein?"*
Chalo, teen possibilities par sote hain:
1. **Sort by Start Time:** Jo activity jaldi shuru ho rahi hai use pick karo.
   * *Counterexample:* Ek activity subah 5 baje shuru hoti hai par raat 11 baje khatam hoti hai. Agar use pick kar liya, toh pure din baaki koi activity run hi nahi ho payegi!
2. **Sort by Duration (Shortest first):** Jo sabse choti hai use pick karo.
   * *Counterexample:* Maan lo do meetings hain `` aur ``. Ek choti meeting banti hai ``. Agar humne `` ko pick kar liya, toh hum pehli aur teesri dono meetings ko blocks kar denge!
3. **Sort by End Time (Earliest Finishing Activity 💡):** Jo activity sabse pehle khatam ho rahi hai, use pehle pick karo! Isse dynamic computer resource ke paas future activities run karne ke liye sabse maximum space aur time bachega!

#### 4. Correctness Argument (Why local finishing choice doesn't damage future?):
Finishing time par sort karne se hum guaranteed optimal subproblem limit coordinate ko touch karte hain. Agar koi optimal solution hai jo humari first chosen activity se start nahi hota, toh hum bina loss of generality ke uski pehli activity ko apni chosen (earliest finishing) activity se swap kar sakte hain, aur solution tab bhi valid rahega.

---

#### 5. JavaScript Implementation:
```javascript
class Activity {
    constructor(start, end) {
        this.start = start;
        this.end = end;
    }
}

function selectMaxActivities(activities) {
    if (activities.length === 0) return 0;

    // Step 1: Sort activities based on their end (finishing) times ascending
    activities.sort((a, b) => a.end - b.end); //

    let count = 1; // First activity is always selected
    let lastSelectedEnd = activities.end; // Tracks end time of last selected activity

    // Step 2: Iterate over the remaining activities
    for (let i = 1; i < activities.length; i++) {
        const current = activities[i];
        
        // If current activity starts AFTER or AT the end of the last selected activity
        if (current.start >= lastSelectedEnd) {
            count++;
            lastSelectedEnd = current.end; // Update ending boundary [cite: 518]
        }
    }

    return count;
}
```

#### 6. Complete Dry Run:
* Input Activities: `A1:`, `A2:`, `A3:`, `A4:`, `A5:`
* After sorting by end time ascending:
  * `A1:`, `A2:`, `A3:`, `A4:`, `A5:` (already sorted!)
* Execution:
  * Select `A1` \\(\rightarrow\\) `count = 1`, `lastSelectedEnd = 4`.
  * **i = 1 (`A2:`):** `start (3) < lastSelectedEnd (4)` \\(\rightarrow\\) Overlap! Skip.
  * **i = 2 (`A3:`):** `start (0) < lastSelectedEnd (4)` \\(\rightarrow\\) Overlap! Skip.
  * **i = 3 (`A4:`):** `start (5) >= lastSelectedEnd (4)` \\(\rightarrow\\) Select! `count = 2`, `lastSelectedEnd = 7`.
  * **i = 4 (`A5:`):** `start (8) >= lastSelectedEnd (7)` \\(\rightarrow\\) Select! `count = 3`, `lastSelectedEnd = 9`.
* Returns `count = 3` (`A1, A4, A5`). Correct output!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n \log n)\\)** dominated by end time sorting [cite: 59, 285].
* **Space Complexity:** **\\(\mathcal{O}(1)\\)** auxiliary.

---

### PROBLEM 4 (Medium): Non-Overlapping Intervals (LeetCode 435)

#### 1. Understand:
Humein intervals di gayi hain. Humein **minimum number of intervals** ko remove karna hai taaki bache huye intervals aapas mein overlap na karein.

#### 2. Visual Representation:
```
           Intervals:,,,
           If we remove, the rest do not overlap!
```

#### 3. SDE Observation:
*Yeh directly Activity Selection problem ka mirror image hai bacho!*  
Max non-overlapping intervals calculate kar lo (`selectMaxActivities`). Total intervals mein se use subtract kar do, toh humein humari minimum removals mil jayegi!
\\[\text{Min Removals} = \text{Total Intervals} - \text{Max Non-Overlapping Intervals}\\]

---

#### 4. JavaScript Code:
```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length <= 1) return 0;

    // Step 1: Sort by interval end times ascending [cite: 59, 285]
    intervals.sort((a, b) => a - b);

    let nonOverlappingCount = 1;
    let end = intervals;

    for (let i = 1; i < intervals.length; i++) {
        // If current interval starts at or after the previous non-overlapping end
        if (intervals[i] >= end) {
            nonOverlappingCount++;
            end = intervals[i]; // Move end pointer
        }
    }

    return intervals.length - nonOverlappingCount; // Removals needed
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n \log n)\\)**, Space: **\\(\mathcal{O}(1)\\)** auxiliary.

---

### PROBLEM 5 (Medium): Minimum Number of Meeting Rooms (LeetCode 253 equivalent)

#### 1. Understand:
Humein meeting intervals di gayi hain. Humein check karna hai ki in saari meetings ko conduct karwane ke liye kam se kam **kitne meeting rooms (resources)** ki requirement hogi.

#### 2. Visual Representation:
```
           Meetings: M1:, M2:, M3:
           M1 starts first. Room 1 occupied.
           M2 starts at 5. Since Room 1 is occupied till 30, we need Room 2!
```

#### 3. Optimal Approach (The Event-Line Sweeping Pointer Technique 💡):
* Hum meeting start times aur end times ko alag-alag arrays mein split kar lenge aur dono ko independently ascending order mein sort kar lenge [cite: 59, 285]!
* Hum start pointer `s` aur end pointer `e` chalakar check karenge:
  * Agar koi meeting shuru ho rahi hai isse pehle ki koi pichli meeting khatam ho (`start[s] < end[e]`), toh humein ek **naya meeting room allocate** karna padega (`roomsCount++`).
  * Agar koi meeting khatam ho jaye (`start[s] >= end[e]`), toh humein naya room nahi chahiye, hum purana room reuse kar sakte hain (`e++` shift, and we don't increase active rooms count).

---

#### 4. JavaScript Code:
```javascript
function minMeetingRooms(intervals) {
    if (intervals.length === 0) return 0;

    const starts = intervals.map(interval => interval).sort((a, b) => a - b);
    const ends = intervals.map(interval => interval).sort((a, b) => a - b);

    let startPtr = 0;
    let endPtr = 0;
    let activeRooms = 0;
    let maxRoomsNeeded = 0;

    while (startPtr < intervals.length) {
        // If next meeting starts before the earliest meeting ends
        if (starts[startPtr] < ends[endPtr]) {
            activeRooms++; // Allocate a new room
            startPtr++;
        } else {
            activeRooms--; // Release a room
            endPtr++;
        }
        maxRoomsNeeded = Math.max(maxRoomsNeeded, activeRooms);
    }

    return maxRoomsNeeded;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n \log n)\\)**, Space: **\\(\mathcal{O}(n)\\)** to hold extracted start/end coordinate arrays.

---

### PROBLEM 6 (Medium): Jump Game I (LeetCode 55)

#### 1. Understand:
Humein ek integer array `nums` diya hai. Humein index `0` se start karke aakhir (index `n-1`) tak pahunchna hai. `nums[i]` represent karta hai ki us index se hum maximum kitne kadam (jumps) aage chal sakte hain [cite: 387, 404].

#### 2. Brute Force Idea:
Recursion se saare possible step choices explore karo.
* **Complexity:** **\\(\mathcal{O}(3^n)\\)** or exponential backtracking paths [cite: 387, 404].

#### 3. Optimal Approach (The Forward Greedy Maximizer 💡):
* Hum hamesha track karenge ek value: **`maxReachable`** (hum max kis index tak chalaang laga sakte hain) [cite: 518]!
* Har index `i` par jaakar:
  * Agar hum kisi aise index par bhatak gaye jo `maxReachable` ke aage hai (`i > maxReachable`), toh aage jana impossible hai! Return `false` [cite: 513].
  * Har valid step par `maxReachable` ko optimize update karte raho:
    \\[\text{maxReachable} = \max(\text{maxReachable}, i + \text{nums}[i])\\] [cite: 513]
  * Agar `maxReachable` end boundary ko touch kar jaye, toh return `true` [cite: 513].

---

#### 4. JavaScript Code:
```javascript
function canJump(nums) {
    let maxReachable = 0; // Tracks the absolute farthest index we can reach [cite: 518]

    for (let i = 0; i < nums.length; i++) {
        // If current index is unreachable, we are trapped!
        if (i > maxReachable) {
            return false; // [cite: 513]
        }
        
        // Update our maximum reach limits globally
        maxReachable = Math.max(maxReachable, i + nums[i]); // [cite: 513]
        
        // Optimizing Early exit: if we can already reach the end of array
        if (maxReachable >= nums.length - 1) {
            return true; // [cite: 513]
        }
    }
    return true;
}
```

#### 5. Complete Dry Run on `nums =`:
* Initial: `maxReachable = 0`
* **i = 0:** `i <= maxReachable (0 <= 0)`. Update `maxReachable = Math.max(0, 0 + 2) = 2`.
* **i = 1:** `i <= maxReachable (1 <= 2)`. Update `maxReachable = Math.max(2, 1 + 3) = 4`.
  * Since `maxReachable (4) >= end (4)`, returns `true` instantly! Absolutely correct!

#### 6. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n)\\)** linear single-pass checks.
* **Space Complexity:** **\\(\mathcal{O}(1)\\)** auxiliary.

---

### PROBLEM 7 (Medium): Jump Game II (LeetCode 45)

#### 1. Understand:
Jump Game I ke continuous check ke sath, is baar humein **minimum number of jumps** nikalni hain jo end index tak pahunchne mein lagengi. (Assume solution always exists).

#### 2. DP vs. Greedy Solution (Why Greedy works? 💡):
* *DP Approach:* Hum har index par minimum jumps recursively compute karke memoize karte hain, which takes quadratic time **\\(\mathcal{O}(n^2)\\)** [cite: 34, 295].
* *Greedy Approach:* Hum jumps ko range-by-range evaluate karenge BFS logic ki tarah!
  * Hum current jump ki boundary `currentEnd` aur next potential jump ki limits `farthest` track karenge.
  * Jab hum chalte-chalte `currentEnd` (purani jumps boundary limit) par pahunchenge, toh hum jumps count ko barha denge (`jumps++`) aur boundary ko `farthest` par update kar denge!

---

#### 3. JavaScript Code:
```javascript
function jump(nums) {
    if (nums.length <= 1) return 0;

    let jumps = 0;
    let currentEnd = 0; // Current jump boundary limit
    let farthest = 0;   // Absolute farthest index reachable globally

    for (let i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);

        // When we reach the end of our current jump's reach
        if (i === currentEnd) {
            jumps++; // Take a jump!
            currentEnd = farthest; // Update current boundary limits
            
            // Early stop: if we can already step over the end of the array
            if (currentEnd >= nums.length - 1) {
                break;
            }
        }
    }

    return jumps;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)**, Space: **\\(\mathcal{O}(1)\\)** auxiliary.

---

### PROBLEM 8 (Medium): Gas Station (LeetCode 134)

#### 1. Understand:
Humein circular track par \\(N\\) gas stations diye hain. arrays `gas` (petrol available) aur `cost` (petrol needed to go to next station) diye hain. Humein starting station dhoondhna hai jahan se hum bina fuel khatam kiye poora circular tour complete kar sakein.

#### 2. SDE Observation:
1. **Total Fuel Validation:** Agar pooray journey mein `totalGas < totalCost` ho jaye, toh journey complete karna mathematically impossible hai (Return `-1`).
2. **Greedy Start Choice:** Agar hum kisi station `i` se shuru karke chalte hain aur kisi point `j` par humara fuel negative ho jata hai, iska matlab **`i` se lekar `j` ke beech ka koi bhi station starting point nahi ban sakta!** (Kyunki agar hum start se positive tank ke sath aaye the tab bhi fail ho gaye, toh direct start karne par toh aur pehle fail honge).
3. Isiliye, agla candidate start point hamesha `j + 1` banna chahiye!

---

#### 3. JavaScript Code:
```javascript
function canCompleteCircuit(gas, cost) {
    let totalGas = 0;
    let totalCost = 0;
    let currentTank = 0; // Fuel status for current segment run
    let startStation = 0; // Farthest valid candidate

    for (let i = 0; i < gas.length; i++) {
        totalGas += gas[i];
        totalCost += cost[i];
        currentTank += gas[i] - cost[i];

        // If tank runs out of gas during segment run
        if (currentTank < 0) {
            startStation = i + 1; // Restart search from next station!
            currentTank = 0;      // Reset tank
        }
    }

    // Mathematical global validation check
    return totalGas >= totalCost ? startStation : -1;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)** linear sweep, Space: **\\(\mathcal{O}(1)\\)** auxiliary.

---

### PROBLEM 9 (Medium): Job Sequencing with Deadlines

#### 1. Understand:
Humein \\(N\\) jobs di gayi hain. Har job ke paas ek `deadline` hai (jis time limits tak job complete ho jana chahiye) aur ek `profit` hai jo execute karne par milega. Har job ko complete hone mein exactly `1` unit time lagta hai. Maximum profit generate karne ka schedule return karo.

```
                  Jobs: J1(Dead:4, Profit:20), J2(Dead:1, Profit:10), J3(Dead:1, Profit:40)
```

#### 2. Greedy Choice Option:
Humesha highest profit wale jobs ko pick karo aur unhe unki **deadline ke absolute aakhri slot (farthest slot)** par schedule karo taaki aage aane wale slots choti deadlines ke liye safe rahein!

---

#### 3. JavaScript Code:
```javascript
function jobScheduling(jobs) {
    // Step 1: Sort jobs descending based on their profit values
    jobs.sort((a, b) => b.profit - a.profit); // [cite: 59, 285]

    // Find maximum deadline to define timeline range limits
    const maxDeadline = Math.max(...jobs.map(job => job.deadline));
    const schedule = new Array(maxDeadline + 1).fill(-1); // Timeline array

    let totalProfit = 0;
    let jobsScheduledCount = 0;

    // Step 2: Iterate over sorted jobs
    for (let job of jobs) {
        // Try scheduling job from its maximum deadline slot backward to start
        for (let slot = job.deadline; slot > 0; slot--) {
            if (schedule[slot] === -1) { // If slot is empty
                schedule[slot] = job.id; // Occupy slot [cite: 470]
                totalProfit += job.profit;
                jobsScheduledCount++;
                break;
            }
        }
    }

    return { totalProfit, jobsScheduledCount };
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n \log n + n \cdot \text{maxDeadline})\\)** which degrades to quadratic in worst cases, Space: **\\(\mathcal{O}(\text{maxDeadline})\\)** for timeline storage.

---

### PROBLEM 10 (Medium): Minimum Platforms / Resource Scheduling

#### 1. Understand:
Humein railway station par trains ke `arrival` aur `departure` times diye hain. Humein station par required **minimum platforms** ka count nikalna hai taaki koi train crash/wait na kare.

#### 2. Optimal Sweeping Pointer Approach 💡:
* arrival times aur departure times dono ko separately sort kar do [cite: 59, 285]!
* arrival ko traverse karte waqt pointer se compare karo:
  * Agar train arrives before departure (`arr[i] <= dep[depPtr]`), we need one more platform (`platformsNeeded++`).
  * Else, platform gets vacated (`platformsNeeded--`, `depPtr++`).

---

#### 3. JavaScript Code:
```javascript
function findPlatform(arr, dep) {
    // Sort arrival and departure times separately ascending [cite: 59, 285]
    arr.sort((a, b) => a - b);
    dep.sort((a, b) => a - b);

    let platformsNeeded = 0;
    let maxPlatforms = 0;

    let arrPtr = 0;
    let depPtr = 0;

    while (arrPtr < arr.length) {
        // If next train arrives before the previous train departs
        if (arr[arrPtr] <= dep[depPtr]) {
            platformsNeeded++; // Need another platform
            arrPtr++;
        } else {
            platformsNeeded--; // Platform cleared
            depPtr++;
        }
        maxPlatforms = Math.max(maxPlatforms, platformsNeeded);
    }

    return maxPlatforms;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n \log n)\\)**, Space: **\\(\mathcal{O}(1)\\)** auxiliary.

---

## 4. THE DECISION MAKER: MIXED PROBLEM TESTING ROUND

🚀 **Arey bacho! Ab dynamic systems interview round start ho raha hai. Tumhein pehle identify karna hai ki problem kis paradigm ki hai, tabhi implementation logic par aage badhna hai!**

---

### Problem A: Best Time to Buy and Sell Stock (LeetCode 121)
*Given stock rates jumbled on consecutive days, select single day to buy, and a future day to sell to maximize profit.*

#### 🧠 Diagnostics:
* *Choices Analysis:* Kya DP ki zaroorat hai? Kya humein saare combinations subsets recursion se check karne hain?
* *Observation:* Hum linearly single pointer sweep ke dauran hamesha **minimum price seen so far** track kar sakte hain [cite: 518]! Har din ka maximum profit us din ki price aur minimum price ka difference hoga.
* *Paradigm Choice:* **Greedy Single-Pass** [cite: 404, 418, 513]!

```javascript
function maxProfit(prices) {
    let minPrice = Infinity;
    let maxProfitValue = 0;

    for (let price of prices) {
        minPrice = Math.min(minPrice, price); // Greedy choice: smallest price so far [cite: 518]
        maxProfitValue = Math.max(maxProfitValue, price - minPrice); // Profit maximized
    }

    return maxProfitValue;
}
```
* **Complexity:** Time: **\\(\mathcal{O}(n)\\)**, Space: **\\(\mathcal{O}(1)\\)**.

---

### Problem B: 0/1 Knapsack Problem (No Fractions Allowed)
*Fractional Knapsack ki tarah hi, par is baar items ko split/cut nahi kiya ja sakta. Ya toh item ko poora lo (1), ya use chodo (0) [cite: 387, 400].*

#### 🧠 Diagnostics:
* *Choices Analysis:* Kya Value/Weight ratio sorting lag sakti hai?
* *Counterexample:* W = 4, Items: `I1(Val:30, Wt:3), I2(Val:20, Wt:2), I3(Val:20, Wt:2)`
  * Ratios: `I1 = 10, I2 = 10, I3 = 10`.
  * Sorting leaves them in any order. If we pick `I1`, remaining space = 1 (cannot take `I2` or `I3`). Total Value = **30**.
  * Optimal Solution: Pick `I2` and `I3`. Weight = `2+2=4`, Value = `20+20=40`.
  * *Why did Greedy fail?* Kyunki items split nahi ho sakte, toh humein binary choices check karni padegi overlapping subproblems ke limits par [cite: 270].
* *Paradigm Choice:* **Dynamic Programming (DP)** [cite: 34, 270, 295]! (Greedy is completely invalid here).

---

## 5. RECOGNITION GUIDE: HOW TO SPOT GREEDY PROBLEMS? 🗺️

Interview room mein dhyan se questions ko padhkar in check cues ko find out karna:

```
                            GREEDY RECOGNITION CHECKLIST
                                         │
        ┌────────────────────────────────┼────────────────────────────────┐
  1. Optimization Keywords         2. Single-Pass Sweep             3. Sortable Inputs
  "Minimize", "Maximize",          Decisions are linear,            Sorting inputs simplifies
  "Farthest", "Earliest".          cannot be reversed.              overlapping conditions.
```

1. **Optimization Keywords:** Jab question explicitly maximum profit ya minimum cost fetch karne ko bole.
2. **One-Way Decisions:** Agar tumne ek decision bana liya, toh use backtracking se repair karne ki zaroorat nahi honi chahiye.
3. **Sorting simplifies everything:** Agar data ko sort karne ke baad decisions automatic intuitive direct paths par fit ho rahe hain, toh 99% greedy approach lagti hai [cite: 270].

---

## 6. SDE TRAPS & COMMON MISTAKES TO AVOID ⚠️

Technical evaluation tests mein in classic bugs se hamesha bacho bacho:

1. **Greedy Guesswork without Edge Cases verification:**
   Bina logic aur proof ke directly greedy guess kar lena. Humesha do ya teen custom inputs arrays banyein and identify karein ki kya current choice future selections ko blocks toh nahi kar rahi.
2. **Incorrect Sorting Key selection:**
   Interval problems mein intervals ko start time par sort kar dena ya random attributes par sorting sequence implement karna [cite: 270, 326].
3. **Confusing Greedy with DP scenarios:**
   0/1 Knapsack ya exact coin changes par bina fractions check kiye greedily items scan check perform karna, jisse solutions completely fail ho jate hain [cite: 34].

---

## CHAPTER END SUMMARY

### Completed Concepts:
* Greedy paradigm choice properties and local-to-global optimal substructures [cite: 270, 419].
* Fractional division algorithms and per-unit scale value ratios [cite: 388, 419].
* Line events sweeping pointers strategies on resource intervals allocations [cite: 518].
* Jump Games linear maximizer logic to bypass backtrack constraints [cite: 387, 404].

### Mastered Greedy Patterns:
* **Sort-by-End-Time index keys** for scheduling and activity selections [cite: 270, 326].
* **Forward range boundary maximizers** on jump paths.
* **Bilateral sweeping lists checks** for platform allocations.

---

### SDE Practice Roadmap:
1. Complete *Assign Cookies* on LeetCode 455 [cite: 270].
2. Solve *Non-overlapping Intervals* (LeetCode 435) with end-sorting rules [cite: 326].
3. Build *Fractional Knapsack* manually and trace step outcomes.

---

