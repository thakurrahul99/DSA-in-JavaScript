**Arey bacho! Jaldi se class mein aa jao aur dhyan seedhe whiteboard par lagao.**

Pichle chapters mein humne Arrays, Matrices, aur Strings ko poore depth mein samjha aur dekha ki kaise dynamic window tracking ya opposite-direction traversal se hum complex loops ko transform karte hain. 

Lekin beta, ek bohot bada bottleneck abhi bhi bacha hua hai. Agar humein kisi array mein koi element dhoondhna ho, toh sorted array mein Binary Search lagakar \\(O(\log n)\\) time lagta hai, aur unsorted mein toh linear scan karke pure \\(O(n)\\) operations karne padte hain. 

*"Sir, kya koi aisa magic structure hai jahan hum lakhon elements ke beech mein se apna target bina kisi loop ke, instantly **\\(O(1)\\) (Constant Time)** mein dhoondh sakein?"*

**Haan bacho, bilkul hai!** Aur usi superpower ka naam hai **Hashing**! Aaj hum Hashing ke concepts ko bilkul zero se advanced Google/Amazon interview level tak breakdown karenge. Apne dimaag ke saare window compartments open kar lo aur register par likhna shuru karo!

---

## 1. THE Locker AnalogY: WHAT IS HASHING?

Chalo ek dam mast real-life example se shuru karte hain.

Manlo tum ek bade amusement park ya swimming pool mein gaye. Wahan ek **Locker Room** hai jahan 100 lockers hain, aur unke numbers `0` se `99` tak hain.

```
                  ┌───────┬───────┬───────┬───────┐
                  │ Slot 0│ Slot 1│ Slot 2│ Slot 3│ ... up to Slot 99
                  ├───────┼───────┼───────┼───────┤
                  │ Empty │ Bag 🎒│ Empty │ Shoes │ 
                  └───────┴───────┴───────┴───────┘
```

Agar tum apna bag locker room mein rakhna chahte ho, toh wahan ka manager ek simple mathematical formula use karta hai. Woh tumhara naam poochta hai, tumne bataya: **"Amit"**.

Manager ne ek magic machine (Hash Function) mein **"Amit"** dala:
1. Machine ne Amit ke characters ka sum nikala: \\(A(65) + m(109) + i(105) + t(116) = 395\\).
2. Locker total 100 hain, toh usne modulo kiya: \\(395 \% 100 = 95\\).
3. Manager ne bola: *"Amit babu, apna bag **Locker Number 95** mein rakh do!"*

Shaam ko jab tum wapas aaye aur bole: *"Mera bag de do"*, toh manager ne phir se wahi math calculate kiya: Amit \\(\rightarrow\\) 95. Usne bina kisi search ke, seedhe Locker 95 khola aur tumhara bag nikal kar de diya. 

**Isi ko Hashing kehte hain!** Bina poore room mein dhoondhe (no loops, no scans), seedhe address calculate karke element par jump karna.

### Key Concepts to Remember:
1. **Key:** Jo unique identifier tum pass karte ho (jaise tumhara Name ya Roll Number).
2. **Value:** Jo actual data tum save kar rahe ho (jaise tumhara Phone Number ya Bag).
3. **Hash Function:** Woh magic machine jo Key ko lekar ek specific integer index return karti hai.
4. **Hash Table:** Woh array/bucket structure jahan data actually un calculated indices par store hota hai.

---

## 2. UNDER THE HOOD: HASH FUNCTIONS & COLLISIONS

Tumhare dimaag mein ek sawaal aa raha hoga: *"Sir, agar do alag-alag naam ka calculated hash index same aa gaya toh kya hoga?"*

Chalo check karte hain:
* **Key 1: "Amit"** \\(\rightarrow\\) ASCII Sum = 395. Modulo 100 = **`95`**.
* **Key 2: "Mita"** \\(\rightarrow\\) ASCII Sum is also 395 (same characters rearrange ho gaye!). Modulo 100 = **`95`**.

**Oh Teri!** Dono ko same locker mila. Is condition ko DSA mein **Collision (Takraav)** kehte hain! Collision tab hota hai jab do different keys ke liye Hash Function same index output kar de.

```
                             COLLISION ENCOUNTERED!
                             
                                 Key: "Amit" ──┐
                                               ▼
                                         Hash Function
                                               ▲
                                 Key: "Mita" ──┘
                                               │
                                               ▼
                                          Index: 95 (Collision! 💥)
```

### Collision Resolution Strategies (Collision se kaise bachein?)
Real engines collisions ko handle karne ke liye do bade methods use karte hain:

#### A. Separate Chaining (The Linked List Chain)
Hum table ke har slot par direct data rakhne ke bajay ek **Linked List ka Head pointer** betha dete hain.
* Agar Locker 95 par pehle se Amit ka data hai, aur ab Mita bhi 95 par aayi, toh hum Amit ke node ke aage Mita ka node append kar denge.
* **Lookup:** Index 95 par jao, aur us slot ki Linked List ko traverse karke target key match karo.

```
           Index 95 ──► [ Amit | Value 1 ] ───► [ Mita | Value 2 ] ───► null
```

#### B. Linear Probing (Open Addressing)
Agar calculated slot pehle se full hai, toh aage badho aur agla jo bhi khali (empty) slot mile, wahan apna data rakh do!

---

### Complexity Caveat: The Worst Case Scenario ⚠️
* **Average Case Complexity:** Agar hash function bohot balanced hai aur keys perfectly distributed hain, toh insertion, deletion, aur search hamesha **\\(\mathcal{O}(1)\\)** hoga.
* **Worst Case Caveat:** Agar hash function ganda hai aur saari keys same index par crash ho gayi, toh separate chaining mein ek hi slot par \\(N\\) elements ki lambi Linked List ban jayegi. Tab search complexity degrade hokar **\\(\mathcal{O}(n)\\)** ho jayegi!

---

## 3. KEYED COLLECTIONS IN JAVASCRIPT: Object vs Map & Set vs Array

JavaScript mein Hashing patterns ko implement karne ke liye humare paas main teen weapons hote hain: **Object**, **Map**, aur **Set**. Inke differences ko bilkul dhyan se samajh lo, yeh interviewers ka hot-topic hai!

### A. Object vs. Map ⚔️

```
             OBJECT                                   ES6 MAP
      ┌──────────────────┐                     ┌──────────────────┐
      │ "key" ──► Value  │                     │  Key  ──► Value  │
      └──────────────────┘                     └──────────────────┘
      * Keys are ALWAYS coerced to Strings.    * Keys can be ANY data type
                                                 (Objects, Functions, etc.)
```

| Feature | Standard Object | ES6 Map |
| :--- | :--- | :--- |
| **Allowed Key Types** | Sirf **String** ya **Symbol**. Agar tum number ya array ko key banaoge, toh JS engine use internally string mein coerce (convert) kar dega. | **Any Type!** Tum pure arrays, objects, functions ya primitives ko directly key bana sakte ho. |
| **Key Ordering** | Elements sorted order mein iterate hote hain (integer keys pehle, baki insertion order mein). | Hamesha strictly **Insertion Order** guarantee karta hai. |
| **Size Lookup** | **\\(\mathcal{O}(n)\\)** time. Humein `Object.keys(obj).length` lagana padta hai jo pure object ko scan karta hai. | **\\(\mathcal{O}(1)\\)** time. Isme direct `.size` property hoti hai. |
| **Performance** | Basic cases ke liye theek hai, par frequent updates par slow ho jata hai. | High-frequency additions aur removals ke liye heavily optimized hai. |

---

### B. Set vs. Array ⚔️
* **Array:** Ordered collection hai jahan elements duplicates ho sakte hain aur search karne ke liye \\(\mathcal{O}(n)\\) scan lagta hai.
* **Set:** Unique values ka collection hai. Agar tum Set mein same element 10 baar bhi insert karoge, toh woh sirf ek hi baar save hoga. Set internally hash table use karta hai, isiliye check karna ki element present hai ya nahi (`set.has(val)`) instantly **\\(\mathcal{O}(1)\\)** mein hota hai!

---

### C. Map and Set Operations in Action (Hands-on) 💻

Chalo engine par inke behaviors ko practically trace karte hain:

```javascript
// 1. Map Operations
const userRoles = new Map();

// set(key, value)
userRoles.set("Amit", "SDE-1");
userRoles.set(45, "Locker Number"); // Numeric key allowed!

// get(key)
console.log(userRoles.get("Amit")); // Output: "SDE-1"
console.log(userRoles.get("Rahul")); // Output: undefined

// has(key) -> Returns boolean
console.log(userRoles.has(45)); // Output: true

// delete(key)
userRoles.delete(45);
console.log(userRoles.has(45)); // Output: false
console.log(userRoles.size); // Output: 1 (Instant O(1) size check)
```

---

### D. The JS Reference Trap in Hashing! 🚨

Bachon, yahan 99% developers fail hote hain. Is code ka output guess karo:

```javascript
const myMap = new Map();
myMap.set(, "Gold Medalist");

console.log(myMap.get()); // Kya output aayega?
```
Tum bologe: *"Sir, simple hai, 'Gold Medalist'!"* 

**Nahi beta! Iska output `undefined` aayega!** 

#### Why? (Samajho memory logic):
JavaScript mein arrays aur objects **Reference Types** hote hain. 
* Jab tumne `.set(, ...)` kiya, toh memory address `Ref_A` par ek array bana.
* Jab tumne `.get()` kiya, toh tumne ek naya array bracket khola, jiska memory address `Ref_B` hai.
* Map check karega: `Ref_A === Ref_B`? Dono references different heap blocks ko point kar rahe hain, isiliye use key nahi milegi!

> **Golden Rule:** Agar key Object ya Array hai, toh hamesha **same reference variable** pass hona chahiye:
> ```javascript
> const arrKey =;
> myMap.set(arrKey, "Success");
> console.log(myMap.get(arrKey)); // Output: "Success"! Correct!
> ```

---

## 4. THE SDE BLUEPRINT: PATTERN RECOGNITION

Interview room mein baithe ho, aur array/string ka question saamne aaya. Hashing kab aur kaise apply karni hai? Is strategy table ko dimaag mein lock karo:

```
                            HASHING SELECTION TREE
                                       │
         ┌─────────────────────────────┴─────────────────────────────┐
  Are you checking UNIQUE items?                              Do you need to store counts/indices?
         │                                                           │
         ▼ (Yes)                                                     ▼ (Yes)
      USE SET!                                                    USE MAP / OBJECT!
  (Instant O(1) duplicate filter)                             (Frequency array/Complement lookup)
```

1. **Kab SET use karein?** 
   Jab humein elements ki count ya positions se koi matlab nahi hai, bas unki **uniqueness/presence** check karni ho (e.g., duplicate detection, intersection checks).
2. **Kab MAP/OBJECT use karein?**
   * Jab humein elements ke sath unki **Frequencies (Count)** ya unka **Index map** save rakhna ho.
   * Jab **Complement Lookup** karna ho (jaise Two Sum).
3. **Kab Static Count Array (size 26/256) use karein?**
   Agar keys strictly lowercase English letters (`'a'-'z'`) ya ASCII characters tak limited hon, toh dynamic Map allocation ke overhead se bachne ke liye direct constant size integer array use karo. Isse performance bohot optimize ho jati hai.
4. **Kab Hashing USE NAHI karni chahiye?**
   * Jab memory ki bohot strict boundary ho (kyunki hashing extra auxiliary space leti hai).
   * Jab elements ka relative order ya sorting sequence barkarar rakhna ho.
   * Jab continuous prefix range boundaries par dynamic minimum/maximum ranges query karni ho.

---

## 5. MASTERCLASS PROBLEMS (PROGRESSIVE CLASSROOM)

🚀 **Aao dosto, whiteboard bilkul saaf hai. Ab hum progressive interview problems ko crack karenge!**

---

### Problem A (Easy): First Unique Character in a String (LeetCode 387)

#### 1. Understand:
Humein ek string `s` di gayi hai. Humein isme sabse pehla non-repeating character dhoondhna hai aur uska index return karna hai. Agar saare characters repeat ho rahe hain, toh `-1` return karo.

#### 2. Example:
Input: `s = "leetcode"`  
Output: `0` (Kyunki `'l'` pure string mein sirf 1 baar aaya hai aur woh index 0 par hai).

Input: `s = "loveleetcode"`  
Output: `2` (First unique character is `'v'`).

---

#### 3. Brute Force Approach:
Har character `s[i]` ke liye, poori string par ek aur nested loop chalakar check karo ki kya woh character aage ya peeche dobara aaya hai.
* **Complexity:** **Time: \\(\mathcal{O}(n^2)\\)** | **Space: \\(\mathcal{O}(1)\\)**.
* **Bottleneck:** Same character ka lookup baar-baar pure linear scan ke roop mein chal raha hai.

#### 4. SDE Observation:
*"Sir, agar hum ek hi pass mein saare characters ka frequency count pehle se rakh lein, toh second pass mein hum bas check kar sakte hain ki kis character ka count exactly 1 hai!"*
* **What to remember?** Character frequencies (Counts).
* **Which Structure?** Map (ya 26-size ka count array, kyunki input lowercase English letters hai).

---

#### 5. JavaScript Implementation:
```javascript
function firstUniqChar(s) {
    const freqMap = new Map();

    // Pass 1: Build Frequency Map
    for (let char of s) {
        freqMap.set(char, (freqMap.get(char) || 0) + 1);
    }

    // Pass 2: Linearly find the first element with count === 1
    for (let i = 0; i < s.length; i++) {
        if (freqMap.get(s[i]) === 1) {
            return i;
        }
    }

    return -1;
}
```

#### 6. Dry Run on `s = "loveleetcode"`:
* **State of Map after Pass 1:**
  ```
  Map(7) {
    'l' => 2,
    'o' => 2,
    'v' => 1,
    'e' => 4,
    't' => 1,
    'c' => 1,
    'd' => 1
  }
  ```
* **Pass 2 Trace:**
  * `i = 0`: `s = 'l'`. `freqMap.get('l')` is `2` (Not 1).
  * `i = 1`: `s = 'o'`. `freqMap.get('o')` is `2` (Not 1).
  * `i = 2`: `s = 'v'`. `freqMap.get('v')` is `1` (Match Found! Return `2`). Correct!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n)\\)** (Two sequential linear passes).
* **Space Complexity:** **\\(\mathcal{O}(1)\\)** (Auxiliary space is limited to maximum 26 lowercase English characters).

#### 8. Edge Cases:
* Empty string `""` \\(\rightarrow\\) Return `-1`.
* All identical characters `"eeee"` \\(\rightarrow\\) Return `-1`.

---

### Problem B (Easy): Two Sum (LeetCode 1)

#### 1. Understand:
Humein ek integers ka array `nums` aur ek `target` sum diya gaya hai. Humein un do elements ke indices return karne hain jinka sum target ke barabar ho.

#### 2. Example:
Input: `nums =, target = 9`  
Output: `` (Kyunki `nums + nums === 9`).

---

#### 3. Brute Force Approach:
Nested loops chalakar saare possible pairs ka sum check karo.
```javascript
for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
        if (nums[i] + nums[j] === target) return [i, j];
    }
}
```
* **Complexity:** **Time: \\(\mathcal{O}(n^2)\\)** | **Space: \\(\mathcal{O}(1)\\)**.
* **Bottleneck:** Complement value `target - nums[i]` ko array mein dhoondhne ke liye har baar inner loop scan karna padta hai.

#### 4. SDE Observation (The Complement Lookup Pattern 💡):
*"Sir, agar hum value `nums[i]` par khade hain, toh humein bas yeh dhoondhna hai ki kya `target - nums[i]` pehle kabhi array mein aa chuka hai? Agar haan, toh hume pair mil gaya!"*
* **What to remember?** Past seen values aur unke indices.
* **Which Structure?** `Map` (Key = Value, Value = Index).

---

#### 5. JavaScript Implementation:
```javascript
function twoSum(nums, target) {
    const seen = new Map(); // Store: Value => Index

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];

        // Check if complement has already been processed
        if (seen.has(complement)) {
            return [seen.get(complement), i];
        }

        // Record current value index
        seen.set(nums[i], i);
    }

    return [];
}
```

#### 6. Dry Run on `nums =, target = 9`:
* `i = 0` (`nums = 2`): `complement = 9 - 2 = 7`.
  * Is `7` in Map? No.
  * Map State: `Map { 2 => 0 }`.
* `i = 1` (`nums = 11`): `complement = 9 - 11 = -2`.
  * Is `-2` in Map? No.
  * Map State: `Map { 2 => 0, 11 => 1 }`.
* `i = 2` (`nums = 7`): `complement = 9 - 7 = 2`.
  * Is `2` in Map? **Yes!**
  * `seen.get(2)` returns `0`.
  * Return ``. Correct!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n)\\)** because of single-pass map checks.
* **Space Complexity:** **\\(\mathcal{O}(n)\\)** auxiliary space for map storage.

---

### Problem C (Medium): Group Anagrams (LeetCode 49)

#### 1. Understand:
Humein strings ka ek array diya gaya hai. Humein anagrams ko ek sath groups mein collect/arrange karna hai. Anagrams wo strings hoti hain jinke character counts bilkul identical hote hain (jaise `"eat"`, `"tea"`, `"ate"`).

#### 2. Example:
Input: `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`  
Output: `[["bat"], ["nat", "tan"], ["ate", "eat", "tea"]]`

---

#### 3. Brute Force Approach:
Har word ke liye poore array mein duplicate frequencies compare karo aur manually buckets mein match karke push karo.
* **Complexity:** **Time: \\(\mathcal{O}(n^2 \cdot k \log k)\\)** (where \\(k\\) is max string length).

#### 4. Better/Optimal Approach (The Sorted-Key Categorization Pattern 💡):
*"Sir, agar hum kisi anagram string ke characters ko alphabetically sort kar dein, toh uske saare variants (eat, tea, ate) ka sorted version exact same string `"aet"` ban jayega! Hum is sorted string ko hi **Map Key** ki tarah use kar sakte hain!"*

* **What to remember?** Sorted base-key and its list of anagram words.
* **Which Structure?** `Map` (Key = Sorted String, Value = Array of original strings).

---

#### 5. JavaScript Implementation:
```javascript
function groupAnagrams(strs) {
    const anagramGroups = new Map();

    for (let str of strs) {
        // Step 1: Create unique categorization key by sorting
        const sortedKey = str.split("").sort().join(""); // eat -> aet

        // Step 2: Initialize group list if absent
        if (!anagramGroups.has(sortedKey)) {
            anagramGroups.set(sortedKey, []);
        }

        // Step 3: Add original string to its mapped group
        anagramGroups.get(sortedKey).push(str);
    }

    // Convert map values directly to Array of Arrays
    return Array.from(anagramGroups.values());
}
```

#### 6. Dry Run on `["eat", "tea", "tan"]`:
* `str = "eat"`: `sortedKey = "aet"`.
  * Map State: `Map { "aet" => ["eat"] }`.
* `str = "tea"`: `sortedKey = "aet"`.
  * Map State: `Map { "aet" => ["eat", "tea"] }`.
* `str = "tan"`: `sortedKey = "ant"`.
  * Map State: `Map { "aet" => ["eat", "tea"], "ant" => ["tan"] }`.
* **Output values:** `[["eat", "tea"], ["tan"]]`. Correct!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n \cdot k \log k)\\)** where \\(n\\) is number of words and \\(k\\) is word length (due to sorting of characters).
* **Space Complexity:** **\\(\mathcal{O}(n \cdot k)\\)** to store words inside map.

---

### Problem D (Medium): Longest Consecutive Sequence (LeetCode 128)

#### 1. Understand:
Humein ek unsorted integers ka array `nums` diya gaya hai. Humein sabse lambi contiguous consecutive sequence (jaise ``) ki length return karni hai. **Constraint:** Code ki Time Complexity strictly **\\(\mathcal{O}(n)\\)** honi chahiye!

#### 2. Example:
Input: `nums =`  
Output: `4` (Kyunki consecutive sequence `` sabse badi hai, iski length 4 hai).

---

#### 3. Brute Force Approach:
Array ko sort kar do, aur fir linear check chalakar count karo ki kitne numbers consecutive order follow kar rahe hain.
* **Complexity:** **Time: \\(\mathcal{O}(n \log n)\\)** due to sorting.
* **Bottleneck:** Sorting rules humein \\(\mathcal{O}(n \log n)\\) se fast chalne nahi dete. We must achieve strictly \\(\mathcal{O}(n)\\).

#### 4. Optimal Approach (The Boundary Marker Pattern 💡):
*"Sir, consecutive sequence hamesha apne sabse chote element se shuru hoti hai! Agar hum Set ka use karein, toh hum \\(\mathcal{O}(1)\\) mein check kar sakte hain ki kya current element sequence ka start hai ya beech ka part."*

**The Algorithm:**
1. Saare elements ko ek **Set** mein daal do (uniqueness and fast lookup ensure karne ke liye).
2. Array ke elements ko check karo. Ek element `num` sequence ka starting point tabhi hoga jab `num - 1` Set mein present **na ho**!
3. Agar `num - 1` present nahi hai, toh loop chalakar check karo ki `num + 1`, `num + 2`... kab tak Set mein hain. Us path ki total length count karo aur max record karo.

---

#### 5. JavaScript Implementation:
```javascript
function longestConsecutive(nums) {
    const numSet = new Set(nums); // O(n) space and time setup
    let longestStreak = 0;

    for (let num of numSet) {
        // Step 1: Identify if current number is the START of a sequence
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentStreak = 1;

            // Step 2: Keep checking the next elements in O(1)
            while (numSet.has(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            // Step 3: Update global maximum
            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }

    return longestStreak;
}
```

#### 6. Dry Run on `nums =`:
* `numSet = Set { 100, 4, 200, 1, 3, 2 }`.
* `num = 100`: Is `99` in Set? No.
  * Start streak check from 100. Is 101 in set? No. `streak = 1`. `longest = max(0, 1) = 1`.
* `num = 4`: Is `3` in Set? **Yes!** (Skip, because 4 is not the starting boundary).
* `num = 200`: Is `199` in Set? No.
  * Streak from 200: `streak = 1`. `longest = max(1, 1) = 1`.
* `num = 1`: Is `0` in Set? No.
  * Streak from 1: Is 2 in set? Yes. Is 3 in set? Yes. Is 4 in set? Yes. Is 5 in set? No.
  * Streak length = 4. `longest = max(1, 4) = 4`.
* Returns `4`. Correct!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n)\\)**. (Dhyan se dekho bacho! Bhale hi `while` loop nested dikh raha hai, par any element maximum 2 baar hi check hota hai—ek baar starting point check hone par, aur ek baar streak expansion ke dauran. Isiliye amortized operations strictly linear **\\(\mathcal{O}(n)\\)** rehte hain).
* **Space Complexity:** **\\(\mathcal{O}(n)\\)** auxiliary space Set store karne ke liye.

---

### Problem E (Hard): Subarray Sum Equals K (LeetCode 560)

#### 1. Understand:
Humein ek array `nums` aur ek target sum `k` diya gaya hai. Humein un continuous subarrays (segments) ki total count dhoondhni hai jinka sum exactly `k` ho. Array mein negative numbers aur zeroes dono ho sakte hain.

#### 2. Example:
Input: `nums =, k = 2`  
Output: `2` (Subarrays are `` at index 0-1, and `` at index 1-2).

Input: `nums = [3, 4, 7, 2, -3, 1, 4, 2], k = 7`  
Output: `4` (Subarrays: ``, ``, `[7, 2, -3, 1]`, `[-3, 1, 4, 2]`).

---

#### 3. Brute Force Approach:
Do nested loops chalakar saare contiguous subarrays generate karo aur unka range sum check karo.
* **Complexity:** **Time: \\(\mathcal{O}(n^2)\\)** | **Space: \\(\mathcal{O}(1)\\)**.
* **Bottleneck:** Shifting windows check slow ho jati hai jab array mein negatives hon, jiski wajah se hum sliding window ko shrink ya expand karne ka optimal criteria lose kar dete hain.

#### 4. Optimal Approach (The Prefix Sum + Hash Map Lookup Pattern 💡):
*"Sir, Chapter 4 mein humne Prefix Sum seekha tha! Agar index `0` se `i` tak ka cumulative sum \\(P[i]\\) hai, aur humein beech ka segment sum `k` chahiye, toh equations ke mutabik:"*
\\[\text{prefixSum}[i] - \text{prefixSum}[j] = k \implies \text{prefixSum}[j] = \text{prefixSum}[i] - k\\]

Iska matlab: agar hum current prefix sum par khade hain, aur humein past history mein se koi aisa prefix sum dhoondhna hai jiske subtraction se target `k` mile, toh humein bas check karna hai ki **`currentPrefixSum - k` past mein kitni baar aa chuka hai!**

* **What to remember?** Previous prefix sums ke count/frequencies.
* **Which Structure?** `Map` (Key = Prefix Sum value, Value = Frequency).

---

#### 5. JavaScript Implementation:
```javascript
function subarraySum(nums, k) {
    let count = 0;
    let runningPrefixSum = 0;
    const prefixFrequencies = new Map();

    // Base Case Setup: Prefix sum 0 has occurred exactly 1 time initially
    prefixFrequencies.set(0, 1);

    for (let i = 0; i < nums.length; i++) {
        runningPrefixSum += nums[i]; // Precomputation in action

        const targetPrefix = runningPrefixSum - k;

        // Check if our target complement has occurred before
        if (prefixFrequencies.has(targetPrefix)) {
            count += prefixFrequencies.get(targetPrefix);
        }

        // Record current prefix sum count in Map
        prefixFrequencies.set(
            runningPrefixSum, 
            (prefixFrequencies.get(runningPrefixSum) || 0) + 1
        );
    }

    return count;
}
```

#### 6. Dry Run on `nums =, k = 7`:
* Initial Map: `Map { 0 => 1 }`. `count = 0`.
* `i = 0` (Value = 3): `runningPrefixSum = 3`.
  * `targetPrefix = 3 - 7 = -4`. Is `-4` in Map? No.
  * Map State: `Map { 0 => 1, 3 => 1 }`.
* `i = 1` (Value = 4): `runningPrefixSum = 3 + 4 = 7`.
  * `targetPrefix = 7 - 7 = 0`. Is `0` in Map? **Yes!**
  * `count += prefixFrequencies.get(0)` \\(\rightarrow\\) `count = 0 + 1 = 1`. (Subarray matches index 0-1: ``).
  * Map State: `Map { 0 => 1, 3 => 1, 7 => 1 }`.
* `i = 2` (Value = 7): `runningPrefixSum = 7 + 7 = 14`.
  * `targetPrefix = 14 - 7 = 7`. Is `7` in Map? **Yes!**
  * `count += prefixFrequencies.get(7)` \\(\rightarrow\\) `count = 1 + 1 = 2`. (Subarray matches index 2: ``).
  * Map State: `Map { 0 => 1, 3 => 1, 7 => 1, 14 => 1 }`.
* Returns `2`. Correct!

#### 7. Complexity Analysis:
* **Time Complexity:** **\\(\mathcal{O}(n)\\)** strictly linear single pass lookup.
* **Space Complexity:** **\\(\mathcal{O}(n)\\)** auxiliary space map storage ke liye.

---

## 6. COMMON MISTAKES & INTERVIEW TRAPS ⚠️

Interview hall mein badmashi mat karna, in 4 bugs ko hamesha dhyan mein rakhna:

1. **Forgetting the Base Case `{0 => 1}` in Prefix Sum Map:**
   Agar subarray index 0 se hi target sum `k` bana deta hai (jaise pehle example mein `3 + 4 = 7`), toh `runningPrefixSum - k` ki value `0` banegi. Agar map mein `0 => 1` added nahi hai, toh index 0 starting point skip ho jayega!
2. **Confusing `Map.has()` with `Map.get()`:**
   * `.has(key)` sirf check karta hai ki key present hai ya nahi (returns boolean).
   * `.get(key)` us key ki associated value return karta hai. 
   `if (map.get(key))` likhne ki galti mat karna! Kyunki agar value `0` hui, toh JS use falsy consider kar lega aur logic fail ho jayega. Always use `map.has(key)`.
3. **Overwriting Map Keys dynamically without incremental checks:**
   Frequency counter banate waqt `map.set(char, map.get(char) + 1)` direct mat likho agar map mein character absent hai. Missing checks `undefined + 1 = NaN` generate kar dete hain! Always use the default or operator setup: `(map.get(char) || 0) + 1`.
4. **Object and Map Interchanging Myths:**
   Dhyan rakhna, standard objects prototypes methods inherit karte hain (jaise `toString`), jo keys collisions generate kar sakte hain agar properties dynamically user input se overlap karein. Strict hashing ke liye hamesha **Map** hi use karein!

---

## CHAPTER END SUMMARY

### Completed Topics:
* Hashing technique locker analogies aur lookup logic.
* Collisions Separate Chaining aur average \\(O(1)\\) vs worst case \\(O(N)\\) behaviors.
* ES6 Map & Set operations and Javascript key references heap behavior.
* Dynamic frequency arrays optimization lowercase English alphabet letters par.

### Mastered Patterns:
* **Frequency Counting Map** character duplicates aur groupings trace karne ke liye.
* **Complement Lookup pattern** target sums pairwise convergence drop karne.
* **Boundary tracking Set pattern** sequences scale linearly compute karne.
* **Prefix Sum Hashing** subarray partitions tracking limits dhoondhne.

---
