**Arey bacho! Jaldi se apni-apni seats par baith jao aur register nikal kar whiteboard par dhyan seedhe focus karo.** 

Pichle chapter mein humne **Interview Problem Solving (Chapter 26)** ke under us ultimate 14-step framework ko master kiya jisse hum kisi bhi anjaan problem ko interviewer ke saamne systematically breakdown karte hain [cite: 10, 15, 756]. Humne seekha ki kaise clarify karna hai, kaise think-aloud approach build karni hai, aur kaise constraints ko dekh kar approach choose karni hai [cite: 11, 51, 52, 520].

Lekin beta, aaj hum jis stage par khade hain, use competitive programming aur product-based interviews ki duniya mein **"The SDE Battleground"** kehte hain! Aaj humara samna kisi pre-defined topic se nahi hoga. 

*"Sir, kya aaj mixed round hai?"*

Haan beta! Whiteboard bilkul clean hai. Aaj hum **Chapter 27: Mixed Interview Problems** mein enter kar rahe hain. Main problem ka title ya header dekar pehle se pattern leak nahi karunga. Tumhe problem ko padhna hai, uske constraints ko parse karna hai, aur dimaag ke pattern-recognition engine ko active karke sahi data structure aur algorithm tak pahunchna hai [cite: 13, 276, 351]!

Chalo dosto, dimaag ki batti jalao aur focus whiteboard par lao! 🚀

---

## THE SDE BATTLE ARENA: SPECIAL PARADIGM COMPARISONS ⚔️

Bacho, mixed problems par jump karne se pehle, in classic pairings ke differences ko dimaag mein permanently lock kar lo, kyunki interviewers aksar yahin par trap set karte hain:

### 1. Two Pointers vs. Sliding Window 🪟
*   **Two Pointers:** Hum dono pointers ko linear array ke extreme ends (`left = 0`, `right = n-1`) par rakhte hain aur unhe aapas mein converge ya diverge karate hain, generally pairs dhoondhne ke liye [cite: 288, 306, 523].
*   **Sliding Window:** Hum do pointers se ek continuous boundary (subarray/substring) define karte hain, jise condition ke base par expand (`right++`) ya shrink (`left++`) kiya jata hai contiguous components ko track karne ke liye [cite: 276, 524].

### 2. Prefix Sum vs. Sliding Window 📈
*   **Prefix Sum:** Tab use hota hai jab array static ho aur humse multiple subarray range queries `[L, R]` ke results constant time \\(O(1)\\) mein mange gaye hon [cite: 330, 380].
*   **Sliding Window:** Tab lagta hai jab humein dynamic window generate karke contiguous sum ya unique characters constraints validate karne hon array ke single pass mein [cite: 276, 524].

### 3. Hashing vs. Sliding Window (with negative numbers 🚨)
*   **The Trap:** Kai baar array mein subarray sum \\(K\\) nikalna hota hai. Agar array mein saare numbers strictly positive hain, toh **Sliding Window** perfectly chalega (window right move karne se sum badhega, left shrink se ghatega) [cite: 524].
*   **The SDE Reality:** Agar array mein **negative numbers** bhi exist karte hain, toh window ka sum monotonic nahi rehta (right move karne se sum ghat bhi sakta hai!). Aise cases mein sliding window completely fail ho jata hai! Humein **Hashing + Prefix Sum** ka use karna hi padega [cite: 330, 524].

### 4. Heap vs. Sorting 🌾
*   **Sorting:** Pure array ko ordered sequence mein convert karta hai jiske liye \\(O(N \log N)\\) time lagta hai [cite: 54, 290, 308].
*   **Heap:** Pure data set ko sort nahi karta. Agar humein dynamic stream mein se sirf top \\(K\\) minimum ya maximum elements chahiye, toh Min/Max-Heap humein sirf \\(O(\log K)\\) insertion aur lookup cost dekar optimize kar deta hai [cite: 211, 299, 317, 332].

---

## MIXED UNSEEN PROBLEMS (CLASSROOM SIMULATION ROUND)

### PROBLEM 1 (Easy-Medium): "Maximum Profit from Magic Crystals"

#### 1. Problem Statement:
Humein ek array `prices` diya hai jahan `prices[i]` kisi magic crystal ki \\(i\\)-th day par price ko represent karta hai [cite: 433, 446, 775, 789, 800]. Hum kisi ek day par crystal buy kar sakte hain aur future ke kisi different day par use sell kar sakte hain [cite: 433, 446, 775, 789, 800]. Humein batana hai ki hum maximum kitna profit kama sakte hain [cite: 433, 446, 775, 789, 800]. Agar koi profit kamaana impossible hai, toh return `0` [cite: 433, 446, 775, 789, 800].

#### 2. Examples:
*   **Example 1:** `prices = ` \\(\rightarrow\\) Output: `5` (Buy on Day 1 (price=1) and sell on Day 4 (price=6), profit = \\(6-1 = 5\\)) [cite: 433, 446, 775, 789, 800].
*   **Example 2:** `prices = ` \\(\rightarrow\\) Output: `0` (Price lagataar ghat rahi hai, so no transaction can yield profit).

#### 3. Constraints:
*   \\(N = \text{prices.length} \le 10^5\\)
*   \\(\text{prices}[i] \ge 0\\)

---

#### 4. Diagnostic Checks & Approach Identification:
Bacho, dhyan se socho! Is problem mein humein maximum profit chahiye, aur sell day humesha buy day ke baad hona chahiye [cite: 433, 446, 775, 789, 800].
*   *Sawaal 1:* Kya hum array ko sort kar sakte hain? **Nahi!** Agar sort kiya, toh days ka chronological order (time sequence) completely destroy ho jayega [cite: 290, 308]!
*   *Sawaal 2:* Kya hum dynamic windows use kar sakte hain? Yes, as price moves forward, can we track the minimum baseline? [cite: 433, 446, 775, 789, 800]

---

#### 5. Brute Force:
Do nested loops lagakar har pair `(i, j)` where `j > i` ka price difference `prices[j] - prices[i]` calculate karo, aur maximum track karo [cite: 273, 524].
*   **Time Complexity:** \\(O(N^2)\\) [cite: 273, 524].
*   **Auxiliary Space:** \\(O(1)\\) [cite: 125].

#### 6. Bottleneck of Brute Force:
Har element ke liye hum pooray future space ko dubara-dubara scan kar rahe hain, jo redundant calculations badhata hai [cite: 524].

#### 7. Optimal Observation (One-Pass Greedy 💡):
Agar main array ko left to right scan karoon, aur abhi tak ki dekhi gayi **minimum price (`minPrice`)** ko maintain karoon [cite: 433, 446, 775, 789, 800], toh current day `i` par max profit kya ho sakta hai?
\\[\text{Potential Profit} = \text{prices}[i] - \text{minPrice}\\]
Humein bas ise pure array ke liye update karte chalna hai in a single pass [cite: 433, 446, 775, 789, 800]!

#### 8. Selected Pattern & Data Structure:
*   **Pattern:** One-Pass Greedy / Sliding Window (Tracking running minimum) [cite: 304, 433, 446, 524, 775, 789, 800].
*   **Data Structure:** Primitive tracking variables (`minPrice`, `maxProfit`).

---

#### 9. JavaScript Solution:
```javascript
function maxProfitMagicCrystals(prices) {
    // Edge Case handling: Minimum 2 days required [cite: 28, 244]
    if (!prices || prices.length < 2) {
        return 0;
    }

    let minPrice = Infinity;
    let maxProfit = 0;

    for (let i = 0; i < prices.length; i++) {
        const currentPrice = prices[i];
        
        if (currentPrice < minPrice) {
            minPrice = currentPrice; // Update running minimum [cite: 433, 446, 775, 789, 800]
        } else {
            const potentialProfit = currentPrice - minPrice;
            if (potentialProfit > maxProfit) {
                maxProfit = potentialProfit; // Update max profit [cite: 433, 446, 775, 789, 800]
            }
        }
    }

    return maxProfit;
}
```

#### 10. Line-by-Line Explanation:
1.  `if (!prices || prices.length < 2) return 0;` \\(\rightarrow\\) Agar array empty hai ya ek hi element hai, transaction possible nahi hai, profit is 0 [cite: 28, 244].
2.  `let minPrice = Infinity;` \\(\rightarrow\\) Initial baseline ko Infinity rakha taaki pehla element aate hi override ho jaye.
3.  `for (let i = 0; i < prices.length; i++)` \\(\rightarrow\\) Pure prices list par linear scan chalaya.
4.  `if (currentPrice < minPrice) minPrice = currentPrice;` \\(\rightarrow\\) Agar aaj price sasti hai, toh aaj hi kharidenge.
5.  `else { const potentialProfit = currentPrice - minPrice; ... }` \\(\rightarrow\\) Warna hum aaj sell karke profit check karenge aur maximum update karenge [cite: 433, 446, 775, 789, 800].

---

#### 11. Complete Dry Run:
Input: `prices = `
*   **i = 0 (val 7):** `7 < Infinity` \\(\rightarrow\\) `minPrice = 7`.
*   **i = 1 (val 1):** `1 < 7` \\(\rightarrow\\) `minPrice = 1`.
*   **i = 2 (val 5):** `5 > 1`. `potentialProfit = 5 - 1 = 4`. `maxProfit = max(0, 4) = 4` [cite: 433, 446, 775, 789, 800].
*   **i = 3 (val 3):** `3 > 1`. `potentialProfit = 3 - 1 = 2`. `maxProfit = max(4, 2) = 4` [cite: 433, 446, 775, 789, 800].
*   **i = 4 (val 6):** `6 > 1`. `potentialProfit = 6 - 1 = 5`. `maxProfit = max(4, 5) = 5` [cite: 433, 446, 775, 789, 800].
*   **i = 5 (val 4):** `4 > 1`. `potentialProfit = 4 - 1 = 3`. `maxProfit = max(5, 3) = 5` [cite: 433, 446, 775, 789, 800].
*   **Returns:** `5`.

#### 12. Complexity:
*   **Time Complexity:** **\\(O(N)\\)** because we process each element exactly once.
*   **Auxiliary Space Complexity:** **\\(O(1)\\)** auxiliary.

#### 13. Edge Cases:
*   `prices.length < 2` \\(\rightarrow\\) Handled cleanly, returns `0` [cite: 28, 244].
*   Prices strictly decreasing \\(\rightarrow\\) `minPrice` updates continuously, `maxProfit` remains `0`. Handled perfectly!

#### 14. Alternative Approach:
Hum is problem ko **Kadane's Algorithm (Max Subarray Sum)** se bhi solve kar sakte hain agar hum consecutive prices ka difference array `diff[i] = prices[i] - prices[i-1]` bana kar max sum subarray find out karein [cite: 434, 486, 500]. Dono methods ki efficiency equal hai, par running minimum approach implementation-wise bohot simpler hai!

#### 15. Interview Follow-up:
*   **Interviewer:** *"What if you are allowed to buy and sell multiple times?"* (LeetCode 122 - Best Time to Buy and Sell Stock II)
*   **Candidate:** *"Sir, in that case, we can greedily collect every consecutive profit. Whenever prices increase from day `i-1` to day `i`, we buy on `i-1` and sell on `i` and add `prices[i] - prices[i-1]` to our total profit. It still runs in \\(O(N)\\) time and \\(O(1)\\) space."*

---

### PROBLEM 2 (Medium - Interview Simulation): "The Delivery Network Prerequisites"

Let's simulate a live SDE round. Watch how the candidate interacts with the interviewer [cite: 13, 15, 756]!

#### 🎙️ Live Mock Simulation:
*   **Interviewer:** *"Welcome! You are building a logistics delivery planner. There are `numTasks` delivery tasks, labeled from `0` to `numTasks - 1`. Some tasks have prerequisite constraints. For example, to complete task `0`, you must first complete task `1`, represented as a pair ``. Given the total number of tasks and a list of prerequisite pairs, return `true` if it is possible to complete all tasks, otherwise return `false`."* [cite: 301, 319, 383, 386]
*   **Candidate (Think Aloud):** [cite: 314]
    *   *Clarification:* *"Sir, let me clarify. This means we are given a dependency constraint list. If there is a cycle of prerequisites—for example, task `A` depends on `B`, and task `B` depends on `A`—then it's impossible to complete them, so we should return `false`. Am I correct?"* [cite: 301, 319, 383, 386]
*   **Interviewer:** *"Exactly! Your cycle detection rule is spot on. What are the constraints?"*
*   **Candidate:**
    *   *Constraints:* *"Let's assume the number of tasks \\(V \le 2000\\) and prerequisites list size \\(E \le 5000\\). This is small enough that \\(O(V + E)\\) or even \\(O(V^2)\\) will run comfortably [cite: 53]. Since we are dealing with prerequisite dependencies, we can model this as a **Directed Graph** where each task is a node, and prerequisites represent directed edges!"* [cite: 350, 383]
*   **Interviewer:** *"Great. How will you check if all tasks can be scheduled?"*
*   **Candidate:**
    *   *Optimal Observation:* *"Sir, a graph where we can successfully find a valid ordering of vertices based on directed prerequisites is called a **Directed Acyclic Graph (DAG)** [cite: 301, 319, 383, 386]. If there is a cycle, we can't sort it topological-wise [cite: 301, 319]. So we can apply **Topological Sort using Kahn's Algorithm (BFS)**! We will compute the **indegree** of all nodes. Nodes with `indegree === 0` have no prerequisites, so they can be processed immediately."*
*   **Interviewer:** *"Excellent. Explain Kahn's algorithm and then code it."* [cite: 11, 37, 520]
*   **Candidate:**
    *   *"Sure, sir. I will build an adjacency list to represent the graph, and an array `indegree` of size `numTasks`. I will push all nodes with `indegree === 0` into a Queue. Then, I will pop a node, decrement the indegree of all its neighbors, and if any neighbor's indegree hits `0`, I will push it into the Queue. If we can process all `numTasks` nodes, return `true`; else `false`."*

---

#### 💻 JavaScript Code:
```javascript
function canFinishTasks(numTasks, prerequisites) {
    // Adjacency List representing DAG [cite: 350, 383]
    const adjList = Array.from({ length: numTasks }, () => []);
    const indegree = new Array(numTasks).fill(0);

    // Step 1: Build Graph & Indegree matrix
    for (let [task, prereq] of prerequisites) {
        adjList[prereq].push(task); // prereq -> task directed link [cite: 350, 383]
        indegree[task]++;
    }

    // Step 2: Push all nodes with indegree 0 to Queue
    const queue = [];
    for (let i = 0; i < numTasks; i++) {
        if (indegree[i] === 0) {
            queue.push(i);
        }
    }

    let processedCount = 0;

    // Step 3: Run Kahn's BFS queue extraction [cite: 352]
    while (queue.length > 0) {
        const current = queue.shift(); // Dequeue [cite: 314]
        processedCount++;

        // Decrement neighbors indegrees
        for (let neighbor of adjList[current]) {
            indegree[neighbor]--;
            if (indegree[neighbor] === 0) {
                queue.push(neighbor); // Prerequisite met! Push to Queue
            }
        }
    }

    // If we successfully processed all tasks, DAG has no cycle!
    return processedCount === numTasks;
}
```

#### 🔍 Complete Dry Run:
Input: `numTasks = 3, prerequisites = `
1.  `adjList = [ , [], ]` (Task 1 is prerequisite for 0, Task 2 is prereq for 1).
2.  `indegree = [ 1, 1, 0 ]` (Node 2 has indegree 0).
3.  Initialize Queue: `queue =`, `processedCount = 0`.
4.  **Loop 1:** Pop `current = 2`. `processedCount = 1`.
    *   Neighbors of 2: ``.
    *   `indegree--` \\(\rightarrow\\) `0`. Push `1` to queue. `queue =`.
5.  **Loop 2:** Pop `current = 1`. `processedCount = 2`.
    *   Neighbors of 1: ``.
    *   `indegree--` \\(\rightarrow\\) `0`. Push `0` to queue. `queue = `.
6.  **Loop 3:** Pop `current = 0`. `processedCount = 3`.
    *   Neighbors of 0: `[]`.
7.  Queue is empty. `processedCount === numTasks` \\(\rightarrow\\) `3 === 3` \\(\rightarrow\\) Returns `true`.

#### ⏱️ Complexity:
*   **Time Complexity:** **\\(O(V + E)\\)** where \\(V\\) is `numTasks` and \\(E\\) is the number of prerequisites [cite: 520].
*   **Auxiliary Space Complexity:** **\\(O(V + E)\\)** for graph adjacency list [cite: 512].

#### 🎙️ Live Mock Follow-up:
*   **Interviewer:** *"Brilliant. Now, instead of just returning true/false, how can you return the actual sequence of tasks that should be performed?"* (LeetCode 210 - Course Schedule II)
*   **Candidate:** *"Sir, Kahn's algorithm is extremely modular! We can simply maintain an array `order = []`. Every time we pop a node from the Queue (`queue.shift()`), we push it into `order`. At the end, if `order.length === numTasks`, we return `order`; else return an empty array `[]`."*

---

### PROBLEM 3 (Medium-Hard): "Continuous Subarray with Exact Score Sum"

#### 1. Problem Statement:
Humein ek unsorted integer array `nums` diya hai (jisne **negative integers** bhi exist ho sakte hain!) aur ek target value `K` di hai [cite: 331, 380]. Humein batana hai ki array mein total kitne unique **continuous subarrays** hain jinka elements sum strictly `K` ke barabar ho [cite: 330, 362, 380].

#### 2. Examples:
*   **Example 1:** `nums =, K = 2` \\(\rightarrow\\) Output: `2` (Subarrays: `` and ``).
*   **Example 2:** `nums =, K = 3` \\(\rightarrow\\) Output: `2` (Subarrays: `` and ``).

#### 3. Constraints:
*   \\(N = \text{nums.length} \le 10^5\\)
*   \\(-1000 \le \text{nums}[i] \le 1000\\)

---

#### 4. Approach & Pattern Selection Challenge (The Trap! 🚨):
Bacho, dhyan se socho. Kuch log isme bina soche-samjhe **Sliding Window** apply karne ka sochte hain.
*   *Why Sliding Window fails here:* Sliding window tabhi kaam karta hai jab elements strictly positive hon [cite: 524]. Agar saare elements positive hain, toh window right expand karne par sum strictly badhta hai, aur left shrink par strictly ghat ta hai. Lekin yahan **negative values** hain! Negative values ke aane se sum badhne ke bajay ghat sakta hai, jisse window pointer convergence rules completely break ho jate hain!
*   *SDE Logic:* Humein **Prefix Sum + Hashing** use karna padega [cite: 330, 362, 380].

---

#### 5. Brute Force:
Do nested loops `i` aur `j` lagakar saare ranges check karo: \\(O(N^2)\\) [cite: 273].

#### 6. Bottleneck of Brute Force:
Range sum calculations require quadratic lookups, exceeding acceptable interview running bounds [cite: 53, 524].

#### 7. Optimal Observation (Prefix Sum & Target Complement Map 💡):
Maan lo index `i` par humara cumulative prefix sum `currSum` hai [cite: 330, 362, 380]. Humein ek aisa contiguous subarray dhoondhna hai jiska sum `K` ho.
Mathematical derivation:
\\[\text{currSum} - \text{prevSum} = K \implies \text{prevSum} = \text{currSum} - K\\]
Iska matlab, agar pichle processed prefix sums mein se humare paas `currSum - K` value exist karti hai, toh un indices se lekar current index `i` tak ke subarray ka sum strictly `K` hoga [cite: 330, 362, 380]!
Hum ek HashMap ka use karenge to store the frequency of all previous prefix sums in \\(O(1)\\) average lookup time [cite: 3, 330, 351, 362, 380].

#### 8. Required Pattern & Data Structure:
*   **Pattern:** Hashing with running Prefix Sum [cite: 330, 362, 380].
*   **Data Structure:** JavaScript `Map()` [cite: 362, 380].

---

#### 9. JavaScript Solution:
```javascript
function subarraySumEqualsK(nums, K) {
    const prefixSumFreq = new Map();
    
    // Base Case initialization: Sum 0 has occurred 1 time (empty subarray) [cite: 330, 380]
    prefixSumFreq.set(0, 1);

    let currSum = 0;
    let count = 0;

    for (let num of nums) {
        currSum += num; // Calculate running prefix sum [cite: 330, 380]

        // Complement check [cite: 351]: Does currSum - K exist in past?
        const complement = currSum - K;
        if (prefixSumFreq.has(complement)) {
            count += prefixSumFreq.get(complement);
        }

        // Store current sum frequency in Hash Map
        prefixSumFreq.set(currSum, (prefixSumFreq.get(currSum) || 0) + 1);
    }

    return count;
}
```

#### 10. Line-by-Line Explanation:
1.  `const prefixSumFreq = new Map();` \\(\rightarrow\\) Key-value pairs store karne ke liye Map banaya, jahan keys prefix sums hain aur values unki frequency [cite: 362, 380].
2.  `prefixSumFreq.set(0, 1);` \\(\rightarrow\\) Baseline set kiya. Agar current prefix sum khud `K` ke barabar ho, toh `currSum - K = 0` valid state return kare [cite: 330, 380].
3.  `currSum += num;` \\(\rightarrow\\) Run-time addition.
4.  `const complement = currSum - K;` \\(\rightarrow\\) Target differences math checks [cite: 351].
5.  `count += prefixSumFreq.get(complement);` \\(\rightarrow\\) Complement frequency results mein sum up karein.

---

#### 11. Complete Dry Run:
Input: `nums = [1, -1, 1, 1, 1], K = 2`
*   Initial: `prefixSumFreq = { 0 => 1 }`, `currSum = 0`, `count = 0`.
*   **num = 1:** `currSum = 1`. `complement = 1 - 2 = -1`. Map doesn't have `-1`.
    *   Map becomes: `{ 0 => 1, 1 => 1 }`.
*   **num = -1:** `currSum = 0`. `complement = 0 - 2 = -2`. Map doesn't have `-2`.
    *   Map becomes: `{ 0 => 2, 1 => 1 }`.
*   **num = 1:** `currSum = 1`. `complement = 1 - 2 = -1`. Map doesn't have `-1`.
    *   Map becomes: `{ 0 => 2, 1 => 2 }`.
*   **num = 1:** `currSum = 2`. `complement = 2 - 2 = 0`. Map HAS `0` (frequency is 2!).
    *   `count += 2` \\(\rightarrow\\) `count = 2`.
    *   Map becomes: `{ 0 => 2, 1 => 2, 2 => 1 }`.
*   **num = 1:** `currSum = 3`. `complement = 3 - 2 = 1`. Map HAS `1` (frequency is 2!).
    *   `count += 2` \\(\rightarrow\\) `count = 4`.
    *   Map becomes: `{ 0 => 2, 1 => 2, 2 => 1, 3 => 1 }`.
*   **Returns:** `4` (Subarrays: `[1, -1, 1, 1]`, `[-1, 1, 1, 1]`, `` at end, and middle ``).

#### 12. Complexity:
*   **Time Complexity:** **\\(O(N)\\)** average time because of Map fast lookups [cite: 3, 351, 362, 380].
*   **Auxiliary Space Complexity:** **\\(O(N)\\)** auxiliary.

---

### PROBLEM 4 (Hard): "The Timeline Overlap Optimization"

#### 1. Problem Statement:
Humein \\(N\\) conference meetings ke timelines start and end intervals `[start_i, end_i]` diyen hain [cite: 276]. Humein minimum kitne conference rooms allocate karne honge taaki koi bhi do meetings ek hi room mein overlap na karein [cite: 276]?

#### 2. Examples:
*   **Example 1:** `meetings = [,,]` \\(\rightarrow\\) Output: `2` (Meeting `` runs simultaneously with other two).
*   **Example 2:** `meetings = [,]` \\(\rightarrow\\) Output: `1` (Meetings are completely disjoint, so same room can be reused!).

#### 3. Constraints:
*   \\(N \le 10^5\\)
*   \\(0 \le start_i < end_i \le 10^6\\)

---

#### 4. Diagnostic Checks & Approach Identification:
Bacho, dhyan se whiteboard par bani is coordinate structure ko dekho.
*   *Why normal sort fails:* Agar hum pure interval subsets ko simply sort karenge, toh end timings tracking mismatch options overlap detect nahi karegi [cite: 276].
*   *SDE Logic:* Humein busy timelines ko track karna hai. Jab koi naya interval shuru ho, toh kya pichla room khali ho gaya hai? Iske liye humein **Min-Heap** ya **Chronological boundary sorting** use karni chahiye [cite: 276, 299, 317, 332]!

---

#### 5. Brute Force:
Har interval ke liye pichle saare allocated room schedules ke overlapping limits scan karna: \\(O(N^2)\\) [cite: 273].

#### 6. Bottleneck of Brute Force:
Scanning each allocated room sequentially causes massive latency on large datasets [cite: 524].

#### 7. Optimal Observation (Chronological Sorting 💡):
Humein pure timeline par do discrete events dikhte hain:
1.  **Start Event:** Ek naya room busy hota hai.
2.  **End Event:** Ek room khali (free) ho jata hai.
Agar hum saare **start times** ko alag array mein aur saare **end times** ko alag array mein sort kar dein [cite: 276], aur unhe do pointers (`startPtr`, `endPtr`) se track karein:
*   Jab current start time, current end time se chota hai, matlab pichli meeting abhi khatam nahi hui hai, so humein ek naya room chahiye (`rooms++`).
*   Jab current start time, current end time se bada ya barabar hai, matlab pichli meeting khatam ho chuki hai, hum same room reuse kar sakte hain (`endPtr++`, meaning room balance stays constant!).

#### 8. Required Pattern & Data Structure:
*   **Pattern:** Intervals with Two-Pointer Chronological boundary sweep [cite: 276, 523, 601].
*   **Data Structure:** Two sorted arrays.

---

#### 9. JavaScript Solution (Min Rooms Optimization):
```javascript
function minMeetingRooms(intervals) {
    if (!intervals || intervals.length === 0) return 0;

    const starts = intervals.map(i => i).sort((a, b) => a - b);
    const ends = intervals.map(i => i).sort((a, b) => a - b); // [cite: 358]

    let startPtr = 0;
    let endPtr = 0;
    let roomsAllocated = 0;

    while (startPtr < intervals.length) {
        // If a new meeting starts before the oldest active meeting ends
        if (starts[startPtr] < ends[endPtr]) {
            roomsAllocated++; // Need a new room!
        } else {
            endPtr++; // A room became free, move pointer [cite: 276]
        }
        startPtr++;
    }

    return roomsAllocated;
}
```

#### 10. Complete Line-by-Line Dry Run:
Input: `intervals = [,,]`
1.  `starts =`
2.  `ends =`
3.  Pointers: `startPtr = 0`, `endPtr = 0`, `roomsAllocated = 0`.
4.  **Loop 1 (`startPtr = 0`, val 0):** `starts (0) < ends (10)`. True \\(\rightarrow\\) `roomsAllocated = 1`. `startPtr++`.
5.  **Loop 2 (`startPtr = 1`, val 5):** `starts (5) < ends (10)`. True \\(\rightarrow\\) `roomsAllocated = 2`. `startPtr++`.
6.  **Loop 3 (`startPtr = 2`, val 15):** `starts (15) >= ends (10)`. False \\(\rightarrow\\) `endPtr++` (now 1). `startPtr++`.
7.  Loop terminates because `startPtr === 3`.
8.  **Returns:** `2`. (At peak, we only need 2 rooms!).

#### 11. Complexity:
*   **Time Complexity:** **\\(O(N \log N)\\)** for sorting starts and ends arrays [cite: 358].
*   **Auxiliary Space Complexity:** **\\(O(N)\\)** to allocate memory for starts and ends arrays [cite: 512].

---

## SDE TRAPS & CLASSOMIC CODING ERRUGS ⚠️

Bacho, interviews mein jab pressure hota hai, toh in three classic bugs se humesha bacho:

1.  **Using `Array.sort()` without custom comparison callback:**
    JavaScript mein numbers sort karte waqt agar tumne custom callback nahi diya (`arr.sort((a,b) => a-b)`), toh JS use internally **lexicographically (string comparison)** sort karega [cite: 358, 175]! Yaani `10` comes before `2` because `'1' < '2'`.
2.  **Confusing Subsequence and Subarray in DP calculations:**
    *Subarray* strictly continuous elements contain karta hai, jabki *Subsequence* gaps handle kar sakta hai character position sequence maintain karte huye [cite: 81, 412]. Dono par check parameters alag lagte hain!
3.  **Applying Two-Pointer on Unsorted Arrays without sorting checks:**
    Converging pointers linear binary range checks strictly sorted elements sequences par hi run hote hain [cite: 288, 306, 523]!

---

## PERFORMANCE REVIEW CHECKLIST & WEAK-AREA DETECTOR 🗺️

Bacho, niche diye huye points ko review karo aur analyze karo tumhara kaunsa area abhi thin hai:

```
                            SDE SKILL PERFORMANCE TRACKER
                                          │
        ┌─────────────────────────────────┼─────────────────────────────────┐
  1D Array Sweeps?                 2D Grid Navigation?               Trees Traversals?
  Two Pointers & Windows           Graph Paths, BFS, DFS,            Level Order, Inorder,
  Optimal? [cite: 523, 524]                 Topological DAG [cite: 382, 383].          BST Invariant? [cite: 343, 366]
```

### Mixed Practice Roadmap:
1.  Solve LeetCode 11 *Container with Most Water* using Two Pointers.
2.  Practice LeetCode 207 *Course Schedule* Kahn's topological cycle check.
3.  Practice LeetCode 560 *Subarray Sum Equals K* using HashMap Prefix sum [cite: 330, 362].

---

⏮️ **Pichle Chapters ka Connection:** Chapter 21 se 26 tak jitne bhi complex binary data arrays, heaps, matrices, backtracking, and dynamic state transitions humne padhe, un sabhi ke dynamic algorithms ko humne modular interview-level configurations mein successfully connect kiya [cite: 10, 15, 301, 756]!

⏭️ **Agle Chapter ka Setup: Chapter 28 — Mock Interviews (Whiteboard simulation runs, edge-case checkpoints, debugging drills, and live coding performance trackers!) [cite: 13, 17].**

