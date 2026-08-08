**Arey bacho, bade hi dhyan se suno! Aaj hum pure DSA ka sabse dhasu aur interview ka sabse favourite topic start karne ja rahe hain—Array Problem-Solving Patterns.** 

Dekho, bohot se bache mujhse kehte hain, *"Sir, array toh samajh aa gaya, par jab LeetCode par naya question dekhte hain, toh dimaag blank ho jata hai. Samajh hi nahi aata ki shuru kahan se karein!"* 

Tumhara yeh darr aaj khatam hone wala hai. Aaj hum sirf codes nahi ratenge; hum seekhenge **Pattern Recognition**—yaani question ko padh kar uske andar ke *clues* ko kaise decode karna hai, aur sahi weapon (pattern) kaise choose karna hai. Whiteboard saaf hai, marker taiyar hai. Shuru karein? Chalo!

---

## 1. BASIC ARRAY PROBLEMS (FOUNDATION BUILDERS)

Pehle hum kuch aisi problems dekhenge jo tumhare dimaag ke basic logic loops ko open karengi. Har ek problem mein hum brute force se optimal tak ka safar taiyar karenge.

### A. Second Largest Element in Array

* **Problem:** Ek array diya hai, usme se second largest element nikalna hai (bina array ko sort kiye).
* **Understand:** Agar array `` hai, toh output `34` hona chahiye. Agar array `` hai, toh koi second largest nahi hai, toh `-1` return karenge.
* **Brute Force:** Array ko descending order mein sort kar do (`arr.sort((a,b) => b-a)`) aur phir `arr` return kar do.
  * **Bottleneck:** Sorting ki complexity \\(O(n \log n)\\) hoti hai. Humein ise ek single scan (linear scan) mein solve karna hai.
* **Optimal Approach (Single-Pass Scan):**
  Hum do pointers/variables banayenge: `largest` aur `secondLargest`. Dono ko initially `-Infinity` set karenge. Pure array par ek baar loop chalayenge:
  1. Agar `arr[i] > largest` hai, toh jo pehle `largest` tha, wo ab `secondLargest` ban jayega, aur naya element `largest` ban jayega.
  2. Agar `arr[i]` humare `largest` se chota hai lekin `secondLargest` se bada hai, toh hum sirf `secondLargest` ko update karenge. Duplicates ko handle karne ke liye check karenge ki `arr[i] !== largest`.

```javascript
function findSecondLargest(arr) {
    if (arr.length < 2) return -1;
    
    let largest = -Infinity;
    let secondLargest = -Infinity;
    
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] > largest) {
            secondLargest = largest;
            largest = arr[i];
        } else if (arr[i] < largest && arr[i] > secondLargest) {
            secondLargest = arr[i];
        }
    }
    return secondLargest === -Infinity ? -1 : secondLargest;
}
```

* **Dry Run with `arr =`:**
  * Shuruat mein: `largest = -Infinity`, `secondLargest = -Infinity`.
  * `i = 0 (12)`: `12 > -Infinity`? Yes. `secondLargest = -Infinity`, `largest = 12`.
  * `i = 1 (35)`: `35 > 12`? Yes. `secondLargest = 12`, `largest = 35`.
  * `i = 2 (1)`: `1 > 35`? No. Else check: `1 < 35 && 1 > 12`? No.
  * `i = 3 (35)`: `35 > 35`? No. Else check: `35 < 35`? No (duplicate duplicate check prevents updating).
  * `i = 4 (34)`: `34 > 35`? No. Else check: `34 < 35 && 34 > 12`? Yes! `secondLargest = 34`.
  * Return `34`. Correct!
* **Complexity:** Time Complexity: **\\(O(n)\\)** (one scan). Space Complexity: **\\(O(1)\\)**.

---

### B. Move Zeroes to End

* **Problem:** Ek array diya hai, saare zeroes ko aakhir mein shift karna hai, aur non-zero elements ka relative order maintain rehna chahiye.
* **Understand:** `arr =` \\(\longrightarrow\\) Output: ``.
* **Brute Force:** Ek naya temporary array banao. Purane array mein scan lagao, saare non-zero elements ko temp array mein dalo. Baaki bachi positions ko zero se fill kar do.
  * **Bottleneck:** Isme \\(O(n)\\) extra space lag raha hai. Humein isko **in-place** (bina extra space) solve karna hai.
* **Optimal Approach (Two-Pointer / Same Direction):**
  Hum ek pointer `insertPos = 0` rakhenge. Pure array par scan lagayenge. Jab bhi humein non-zero element milega, hum use `arr[insertPos]` ke sath swap karenge aur `insertPos++` kar denge.

```javascript
function moveZeroes(arr) {
    let insertPos = 0;
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] !== 0) {
            // Swapping arr[i] with arr[insertPos]
            let temp = arr[i];
            arr[i] = arr[insertPos];
            arr[insertPos] = temp;
            
            insertPos++;
        }
    }
    return arr;
}
```

* **Dry Run with `arr =`:**
  * `insertPos = 0`.
  * `i = 0`: `arr = 0`. Kuch nahi karenge.
  * `i = 1`: `arr = 1` (non-zero). Swap `arr` with `arr[insertPos]` (which is `arr`). Array becomes ``. `insertPos` becomes `1`.
  * `i = 2`: `arr = 0`. No action.
  * `i = 3`: `arr = 3` (non-zero). Swap `arr` with `arr[insertPos]` (which is `arr`). Array becomes ``. `insertPos` becomes `2`.
  * Loop ends. Output: ``. Perfect!
* **Complexity:** Time: **\\(O(n)\\)**, Space: **\\(O(1)\\)**.

---

### C. Rotate Array by \\(K\\) steps (Right/Left)

* **Problem:** Array ko \\(K\\) steps right ki taraf rotate karna hai.
* **Understand:** `arr =, K = 2` \\(\longrightarrow\\) Output: ``.
* **Brute Force:** Ek step rotate karne ke liye aakhir ke element ko uthakar pehle rakho, baki ko shift karo. Aise \\(K\\) baar karo.
  * **Bottleneck:** Time Complexity \\(O(n \times K)\\) ho jayegi, jo bohot slow hai agar \\(K\\) bada ho.
* **Optimal Approach (The Reversal Algorithm):**
  Dekho ek gajab ki mathematical property hai! Agar hum array ko do parts mein split karein aur unhe reverse karein, toh magic hota hai:
  1. Pehle \\(K\\) ko handle karo: `K = K % arr.length` (agar \\(K\\) array size se bada ho).
  2. Pure array ko reverse karo: `` \\(\longrightarrow\\) ``.
  3. Pehle \\(K\\) elements ko reverse karo (from index `0` to `K-1`): `` \\(\longrightarrow\\) ``. (Array: ``).
  4. Baki bache elements ko reverse karo (from index `K` to `n-1`): `` \\(\longrightarrow\\) ``. (Array: ``).
  *Boom! Sorted and rotated right instantly!*

```javascript
function reverse(arr, start, end) {
    while (start < end) {
        let temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}

function rotateArray(arr, k) {
    const n = arr.length;
    k = k % n;
    
    reverse(arr, 0, n - 1); // 1. Reverse entire array
    reverse(arr, 0, k - 1); // 2. Reverse first k elements
    reverse(arr, k, n - 1); // 3. Reverse remaining elements
    return arr;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)** (kyunki har element maximum 2 baar swap ho raha hai). Space Complexity: **\\(O(1)\\)**.

---

### D. Missing Number

* **Problem:** Range `[0, n]` ke \\(n\\) distinct integers diye hain, lekin ek number missing hai. Use dhoondho.
* **Understand:** `nums =` (length = 3). Range hai `0` se `3`. Missing element is `2`.
* **Brute Force / Sorting:** Array ko sort karo, phir check karo ki kya `nums[i] === i` hai. Agar nahi, toh `i` missing hai.
  * **Bottleneck:** Sorting takes \\(O(n \log n)\\).
* **Optimal Approach (The Sum Formula):**
  Hum mathematically jante hain ki Sum of first \\(N\\) natural numbers kya hota hai: \\(\frac{n(n+1)}{2}\\).
  Hum mathematical total sum nikalenge aur usme se array ke saare elements ke sum ko subtract kar denge. Jo difference bachega, wahi humara missing number hai!

```javascript
function findMissingNumber(nums) {
    const n = nums.length;
    const expectedSum = (n * (n + 1)) / 2;
    
    let actualSum = 0;
    for (let i = 0; i < n; i++) {
        actualSum += nums[i];
    }
    return expectedSum - actualSum;
}
```
* **Complexity:** Time: **\\(O(n)\\)**, Space: **\\(O(1)\\)**.

---

### E. Majority Element (Boyer-Moore Voting Algorithm)

* **Problem:** Array mein wo element dhoondho jo \\(> \lfloor n/2 \rfloor\\) baar aata hai (assumed that majority element always exists).
* **Understand:** `arr =` \\(\longrightarrow\\) Output: `2`.
* **Brute Force:** Do nested loops lagakar har element ki frequency count karo. Complexity: \\(O(n^2)\\).
* **Better Approach:** Hash Map/Object banakar counts store karo. Time: \\(O(n)\\), Space: \\(O(n)\\) (for hash table storage).
* **Optimal Approach (Boyer-Moore Voting):**
  *Dekho, yeh algorithm ekdam dhasu hai!* Yeh ek simple concept par kaam karta hai: **"Agar Majority element dushmano se lade, tab bhi aakhir mein majority hi jeetegi kyunki uske paas n/2 se zyada log hain."**
  1. Hum ek `candidate` aur ek `count` variable select karte hain (initially `count = 0`).
  2. Hum array par traverse karte hain. 
  3. Agar `count === 0` hota hai, toh hum current element ko apna naya `candidate` maan lete hain aur `count = 1` set kar dete hain.
  4. Agar current element humare `candidate` ke barabar hai, toh `count++` karenge, warna `count--` karenge.
  5. Aakhir mein jo `candidate` bachega, wahi majority element hoga.

```javascript
function majorityElement(nums) {
    let candidate = null;
    let count = 0;
    
    for (let i = 0; i < nums.length; i++) {
        if (count === 0) {
            candidate = nums[i];
            count = 1;
        } else if (nums[i] === candidate) {
            count++;
        } else {
            count--;
        }
    }
    return candidate;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)**, Space Complexity: **\\(O(1)\\)**. *Dekha? Humne space ko completely optimize karke \\(O(1)\\) kar diya!*

---

## 2. PREFIX SUM PATTERN (QUERY OPTIMIZATION)

**Sabse pehle samjho ki Prefix Sum ki zaroorat kyun padti hai.**

Manlo tumhare paas ek array hai `arr =`. Interviewer tumse poochta hai:
* *"Batao, index 1 se 4 tak ke elements ka sum kya hai?"*
* *"Index 0 se 3 ka sum kya hai?"*
* *"Index 2 se 5 ka sum kya hai?"*

Agar tum har baar query aane par `for` loop chalakar sum nikaloge, toh har range sum query ko solve karne mein **\\(O(n)\\)** time lagega. Agar interviewer tumse \\(Q\\) queries poochta hai, toh total complexity **\\(O(Q \times n)\\)** ho jayegi. Agar \\(Q = 10^5\\) aur \\(n = 10^5\\) ho, toh code crash ho jayega!

### Prefix Array Construction (Precomputing)
Hum ek aisa array banate hain jahan har index `i` par array ke starting se lekar `i` tak ke elements ka cumulative sum store hota hai:
\\[\text{prefix}[i] = \text{arr} + \text{arr} + \dots + \text{arr}[i]\\]

```
arr:         [  3,   2,   8,   1,   5  ]
prefix:      [  3,   5,  13,  14,  19  ]
```
Formula: `prefix[i] = prefix[i-1] + arr[i]`

### Range Sum Formula
Agar humein kisi bhi subarray Range `[L, R]` ka sum nikalna hai, toh hum bina kisi loop ke constant time mein nikal sakte hain:
\\[\text{RangeSum}(L, R) = \text{prefix}[R] - \text{prefix}[L - 1]\\]
*(Agar \\(L = 0\\) hai, toh simply `prefix[R]`)*

```
Query: Range in arr =
Calculated RangeSum = prefix - prefix
                    = 14 - 3 = 11 (which is 2 + 8 + 1). O(1) Time!
```

### JavaScript Implementation of Prefix Sum:
```javascript
class NumArray {
    constructor(arr) {
        this.prefix = [];
        if (arr.length > 0) {
            this.prefix = arr;
            for (let i = 1; i < arr.length; i++) {
                this.prefix[i] = this.prefix[i - 1] + arr[i];
            }
        }
    }
    
    getRangeSum(L, R) {
        if (L === 0) return this.prefix[R];
        return this.prefix[R] - this.prefix[L - 1];
    }
}
```
* **Complexity Improvement:** Array preprocessing takes **\\(O(n)\\)** time and **\\(O(n)\\)** space, lekin uske baad **har Range Query constant \\(O(1)\\) time mein** solve ho jati hai!

> 💡 **Pattern Recognition Clue:** Jab bhi question mein **repeated sum/queries of subarrays or ranges** dikhe, toh bina soche sabse pehle **Prefix Sum** dimaag mein aana chahiye!

---

## 3. DIFFERENCE ARRAY PATTERN (RANGE UPDATE OPTIMIZATION)

Prefix Sum Range queries ke sum ko optimize karta hai. Lekin kya hoga agar humein **multiple range updates** karne hon?

* **Problem Statement:** Tumhe ek array `arr` diya hai aur multiple range update operations karne hain. Jaise: *"Index 1 se 3 tak ke elements mein +5 add kar do"*, *"Index 2 se 4 mein -2 add kar do"*. Pure updates complete hone ke baad final array kya hoga?
* **Brute Force:** Har operation ke liye range `[L, R]` ke elements par loop chalao aur value add karo. Agar \\(Q\\) operations hain, toh worst case time complexity: **\\(O(Q \times n)\\)**.

### The Smart Solution: Difference Array (\\(D\\))
Hum ek extra auxiliary difference array banate hain jiska size `n + 1` hota hai. 
Difference array ka logic yeh hai ki iska har element `D[i] = arr[i] - arr[i-1]` hota hai.

#### Range Update Strategy:
Agar humein range `[L, R]` mein value `X` add karni hai, toh hum poore elements ko change nahi karte! Hum sirf do positions ko change karte hain:
1. `D[L] += X` (Isse index L ke baad wale saare elements mein X add karne ka trigger milta hai).
2. `D[R + 1] -= X` (Isse range ke bahar wale elements par is change ke effect ko nullify kar diya jata hai).

#### Reconstruction (Prefix Sum of Difference Array):
Operations complete hone ke baad, jab hum difference array ka prefix sum nikalte hain, toh original array update hokar mil jata hai!

```
Initial arr:       [ 10,  20,  30,  40 ] (size 4)
Diff Array (D):    [ 10,  10,  10,  10,   0 ] (initially D[i] = arr[i] - arr[i-1])

Operation: Add +5 in Range
1. D += 5  ==>  D becomes 15
2. D -= 5  ==>  D becomes 5
D Array now:       [ 10,  15,  10,   5,   0 ]

Reconstruct: prefixSum(D)
Step 0: 10
Step 1: 10 + 15 = 25
Step 2: 25 + 10 = 35
Step 3: 35 + 5  = 40
Final Reconstructed updated array:
(Indeed original indexes 1 & 2 became 20+5=25 and 30+5=35! Magic!)
```

### JavaScript Implementation:
```javascript
function applyRangeUpdates(arr, updates) {
    const n = arr.length;
    // 1. Initialize Difference Array with 0s
    let diff = new Array(n + 1).fill(0);
    
    // 2. Apply range update markers
    for (let u of updates) {
        let [L, R, val] = u;
        diff[L] += val;
        diff[R + 1] -= val;
    }
    
    // 3. Reconstruct the updates through prefix sum
    let currentSum = 0;
    for (let i = 0; i < n; i++) {
        currentSum += diff[i];
        arr[i] += currentSum;
    }
    return arr;
}
```
* **Complexity:** Time Complexity: **\\(O(n + Q)\\)** (where \\(Q\\) is the number of updates). *Double loop se linear scan par le aaye bacho!* Space Complexity: **\\(O(n)\\)**.

---

## 4. TWO POINTERS PATTERN (BILATERAL SCANNING)

**Two Pointers pattern nested loop ki \\(O(n^2)\\) complexity ko linear \\(O(n)\\) complexity mein badal deta hai.**

Hum do index pointers coordinate karte hain—ya toh hum unhe opposide sides se start karke center ki taraf meet karate hain (**Left/Right pointer**), ya fir same direction mein move karte hain (**Same-direction/Fast-Slow pointer**).

```
  Opposite Direction (Left/Right)          Same Direction (Fast/Slow)
  ┌───┐                       ┌───┐        ┌───┐ ┌───┐
  │ L │ ───►             ◄─── │ R │        │ S │ │ F │ ───► (Both move right)
  └───┘                       └───┘        └───┘ └───┘
```

### Opposite Direction: Pair Sum in Sorted Array (Two Sum II)
* **Problem:** Sorted array mein aise do indices dhoondho jinka sum equal to `target` ho.
* **Understand:** `arr =, target = 9` \\(\longrightarrow\\) Output: `` (since 4 + 5 = 9).
* **Pointer Movement Logic:**
  1. `left` pointer index `0` par set karo, aur `right` pointer index `n-1` par.
  2. Compute `currentSum = arr[left] + arr[right]`.
  3. Agar `currentSum === target` hai, toh indices mil gaye!
  4. Agar `currentSum < target` hai, toh sum badhane ke liye humein `left++` karna padega (sorted array property).
  5. Agar `currentSum > target` hai, toh sum ghatane ke liye humein `right--` karna padega.

```javascript
function hasPairWithTarget(arr, target) {
    let left = 0;
    let right = arr.length - 1;
    
    while (left < right) {
        let currentSum = arr[left] + arr[right];
        if (currentSum === target) {
            return [left, right];
        } else if (currentSum < target) {
            left++; // Need a larger value
        } else {
            right--; // Need a smaller value
        }
    }
    return [];
}
```
* **Complexity:** Time: **\\(O(n)\\)** (linear search in single scan). Space: **\\(O(1)\\)**.

> 💡 **Pattern Recognition Clue:** Jab bhi linear data structure **Sorted** form mein ho, aur range/pairs scan karke target output banana ho, toh **Two-Pointer bilateral scan** lagao!

---

## 5. SLIDING WINDOW PATTERN (CONTIGUOUS SUBARRAY SCANNING)

**Sliding window contiguous subarrays ya substrings ke subsets ko process karne ka best pattern hai.**

Hum range scan karne ke liye loop ke andar baar-baar start se loop nahi chalate. Hum ek fixed ya variable size ki window banate hain, jise hum aage badhate jate hain. Piche se element ko subtract karte hain, aur aage se element ko add karte hain.

```
Window Step 1:  [  1,  2,  3  ], 4,  5   ==> Sum = 6
Window Step 2:   1, [  2,  3,  4  ], 5   ==> New Sum = 6 - 1 + 4 = 9 (Fast sliding!)
```

### A. Fixed-Size Window: Max Sum Subarray of size \\(K\\)
* **Problem:** Array mein se contiguous subarray dhoondho jiska size exactly \\(K\\) ho aur sum maximum ho.
* **Understand:** `arr =, K = 3` \\(\longrightarrow\\) Output: `9` (since subarray `` has max sum = 9).
* **Sliding logic:**
  1. Pehle index `0` se `K-1` tak ke first window ka sum manually calculate karo.
  2. Phir window ko index `K` se end tak slide karo. Har step par window mein naya element add karo (`arr[i]`) aur window se pichla out-of-bound element subtract karo (`arr[i - K]`).

```javascript
function maxSubarraySum(arr, k) {
    if (arr.length < k) return 0;
    
    let windowSum = 0;
    // 1. Calculate sum of first window
    for (let i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    let maxSum = windowSum;
    // 2. Slide the window
    for (let i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k]; // add new, subtract old
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)**, Space Complexity: **\\(O(1)\\)**. *No nested loops, linear execution!*

---

### B. Variable-Size Window: Minimum Size Subarray Sum
* **Problem:** Positive integers ke array mein se us smallest contiguous subarray ki length dhoondho jiska sum \\(\ge target\\) ho.
* **Understand:** `arr =, target = 7` \\(\longrightarrow\\) Output: `2` (since subarray `` sum is 7).
* **Logic of variable window (Expand & Shrink):**
  1. Hum window ke `start = 0` aur `end = 0` pointers rakhenge.
  2. Window ko right-side expand karenge (`end++`) aur sums ko accumulate karenge.
  3. Jaise hi `currentSum >= target` ho jaye, hum tab tak window ko left se shrink karenge (`start++`) jab tak humara sum valid rahe. Is shrink phase mein hum shortest valid window length ko dynamically record karenge.

```javascript
function minSubArrayLen(target, nums) {
    let start = 0;
    let currentSum = 0;
    let minLength = Infinity;
    
    for (let end = 0; end < nums.length; end++) {
        currentSum += nums[end]; // Expand window
        
        // Shrink window from left as long as sum >= target
        while (currentSum >= target) {
            minLength = Math.min(minLength, end - start + 1);
            currentSum -= nums[start];
            start++;
        }
    }
    return minLength === Infinity ? 0 : minLength;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)** (even though nested loop is there, both start and end visit each index at most once). Space Complexity: **\\(O(1)\\)**.

> 💡 **Pattern Recognition Clue:** Jab bhi question mein **contiguous subarray/substring** ki baat ho aur hume **length, sum, ya counts ko maximize/minimize** karna ho, toh **Sliding Window** use karo!

---

## 6. KADANE'S ALGORITHM (MAXIMUM SUBARRAY SUM)

**Kadane's Algorithm ek dynamic programming pattern hai jo standard quadratic maximum subarray sum ko linear time mein optimize kar deta hai.**

* **Problem Statement:** Unsorted integers (including negatives) ke array mein se us contiguous subarray ko dhoondho jiska sum sabse bada (maximum) ho.
* **Understand:** `arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]` \\(\longrightarrow\\) Output: `6` (subarray `[4, -1, 2, 1]` sum is 6).

### Brute Force (Double Loop Scan):
Hinglish approach: "Hum saare possible subarrays ke ranges `[i, j]` scan karenge aur unka sum nikal kar max update karenge."
```javascript
// O(n^2) Brute Force
function maxSubarrayBrute(arr) {
    let maxSum = -Infinity;
    for (let i = 0; i < arr.length; i++) {
        let currentSum = 0;
        for (let j = i; j < arr.length; j++) {
            currentSum += arr[j];
            maxSum = Math.max(maxSum, currentSum);
        }
    }
    return maxSum;
}
```
* **Bottleneck:** Quadratic complexity. Humein isse single linear scan mein solve karna hai.

### Kadane's Core Intuition (The "Chodho ya Sath Raho" Principle):
Dekho bacho, Kadane's ek simple logic par chalta hai:
*"Jab hum array par aage badh rahe hain, toh kya pichla chal raha subarray sum mere sath jud kar mere sum ko badhayega, ya pichla sum ganda (negative) hai aur mujhe wahan se picha chodhkar ek naye subarray ki shuruwat karni chahiye?"*

At each index `i`:
\\[\text{CurrentMax} = \max(\text{arr}[i], \text{CurrentMax} + \text{arr}[i])\\]

```
arr:           [ -2,   1,  -3,   4 ]
currentMax:    [ -2,   1,  -2,   4 ] (At -3, we do 1 + (-3) = -2. At 4, we prefer starting new subarray 4 rather than -2 + 4 = 2!)
```

### Optimal Implementation:
```javascript
function maxSubArray(nums) {
    let currentSum = nums;
    let maxSum = nums;
    
    for (let i = 1; i < nums.length; i++) {
        // Option 1: Join previous subarray
        // Option 2: Start a brand new subarray from index i
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

### Track Subarray Indices (If Interviewer asks: "Subarray print karke dikhao")
Hum pointers update tracking logic add karenge:
```javascript
function maxSubArrayWithIndices(nums) {
    let currentSum = nums;
    let maxSum = nums;
    let start = 0, end = 0, tempStart = 0;
    
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] > currentSum + nums[i]) {
            currentSum = nums[i];
            tempStart = i; // Reset subarray starting position
        } else {
            currentSum += nums[i];
        }
        
        if (currentSum > maxSum) {
            maxSum = currentSum;
            start = tempStart;
            end = i;
        }
    }
    console.log(`Max Subarray Range is index [${start} to ${end}]:`, nums.slice(start, end + 1));
    return maxSum;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)**, Space Complexity: **\\(O(1)\\)**. *Dekha bacho? Boyer-Moore algorithm ki tarah Kadane's ne bhi memory footprint ko extremely lightweight kar diya!*

---

## 7. PATTERN RECOGNITION MATRIX

Interview room mein jab naya question aaye, toh darna mat. Apne dimaag se yeh tabular matrix match karna:

| Pattern | Clues in Problem Statement (Ishara) | Core Questions to Ask | Why use it? |
| :--- | :--- | :--- | :--- |
| **Prefix Sum** | Range sums, repeated queries of sub-elements, cumulative processing. | "Kya ranges change ho rahi hain?" / "Kya operations additive hain?" | \\(O(n)\\) update scan ko \\(O(1)\\) fast lookups par shift kar deta hai. |
| **Difference Array**| Range updates, multiple index updates, range addition operations. | "Kya updates constant values ke hain?" | Range iterations skip karke direct boundaries mark karta hai. |
| **Two Pointers** | Sorted arrays, finding pairs, bilateral targets. | "Kya hum linear order scan narrow down kar sakte hain?" | Inner nested redundant pairs loop remove karta hai. |
| **Sliding Window** | Contiguous subarray/substrings, optimization of length or window sum. | "Kya search window contiguous hai?" | Range scan pointers shifts linear scans maintain karte hain. |
| **Kadane's** | Maximum subarray sum, continuous dynamic addition optimization. | "Kya negative indices array sums drop kar rahe hain?" | Array dynamic segments instantly calculate karta hai. |

---

## 8. REAL INTERACTIVE PRACTICE PROBLEMS

🚀 **Whiteboard bilkul saaf hai. Ab main tumhein teen practice questions de raha hoon. Pehle clues ko dhyan se padho, khud se socho ki kaunsa pattern lagega, aur fir solution ko analyze karo!**

### Problem 1 (Easy): Contains Duplicate II
*Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that `nums[i] === nums[j]` and `Math.abs(i - j) <= k`.*

#### 🧠 Analysis & Clues:
* *Contiguous Window:* Humein ek window check karni hai jiska size at most `k` ho, jismein duplicates available hon.
* *What is it?* Sliding Window of size `k` with Hashing (Set) duplicate detection!

#### Optimal Implementation:
```javascript
function containsNearbyDuplicate(nums, k) {
    const windowSet = new Set(); // To store elements in the current window
    
    for (let i = 0; i < nums.length; i++) {
        // If window exceeds size k, remove the oldest element
        if (i > k) {
            windowSet.delete(nums[i - k - 1]);
        }
        
        // If element already in the window, we found a duplicate within distance k
        if (windowSet.has(nums[i])) {
            return true;
        }
        windowSet.add(nums[i]);
    }
    return false;
}
```
* **Complexity:** Time: **\\(O(n)\\)**, Space: **\\(O(\min(n, k))\\)**.

---

### Problem 2 (Medium): Subarray Sum Equals \\(K\\)
*Given an array of integers `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals to `k`.*

#### 🧠 Analysis & Clues:
* *Contiguous subarray sum queries:* Is subarray contiguous? Yes. Are we looking for sum? Yes. Can we use prefix sum?
* *The Twist:* Array elements can be negative, so standard sliding window sum won't work because window contraction (shrinking left pointer) requires monotonic increase.
* *The Solution:* **Prefix Sum + Hashing Pattern!**
  If cumulative sum up to index `i` is `prefixSum`, and we want to find a subarray ending at `i` with sum `k`, we need a previous index with prefix sum `prefixSum - k`.
  Mathematical check:
  \\[\text{prefixSum}[i] - \text{prefixSum}[j] = k \implies \text{prefixSum}[j] = \text{prefixSum}[i] - k\\]

#### Optimal Implementation:
```javascript
function subarraySum(nums, k) {
    let count = 0;
    let currentPrefixSum = 0;
    
    // Hash map to store frequencies of prefix sums seen so far
    const prefixSumCounts = new Map();
    // Base Case: empty prefix has sum 0 once
    prefixSumCounts.set(0, 1);
    
    for (let i = 0; i < nums.length; i++) {
        currentPrefixSum += nums[i];
        
        // Check if prefixSum - k exists in our map
        if (prefixSumCounts.has(currentPrefixSum - k)) {
            count += prefixSumCounts.get(currentPrefixSum - k);
        }
        
        // Update current prefix sum frequency
        prefixSumCounts.set(currentPrefixSum, (prefixSumCounts.get(currentPrefixSum) || 0) + 1);
    }
    return count;
}
```
* **Complexity:** Time: **\\(O(n)\\)**, Space: **\\(O(n)\\)**. *This is a very frequent Google/Facebook interview problem, bacho!*

---

### Problem 3 (Challenging): Trapping Rain Water

*Given `n` non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.*

#### 🧠 Analysis & Clues:
* *Is there a bilateral boundary limits logic?* Yes, water trapping relies on left-right tall walls.
* *Pattern:* **Two Pointers!**
  We place a left pointer at `0` and a right pointer at `n-1`. We keep track of `leftMax` and `rightMax`. At each step, whichever side has a smaller max boundary dictates the water level, and we process that pointer inward.

#### Optimal Implementation:
```javascript
function trap(height) {
    if (height.length === 0) return 0;
    
    let left = 0;
    let right = height.length - 1;
    let leftMax = 0;
    let rightMax = 0;
    let trappedWater = 0;
    
    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                trappedWater += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                trappedWater += rightMax - height[right];
            }
            right--;
        }
    }
    return trappedWater;
}
```
* **Complexity:** Time Complexity: **\\(O(n)\\)**, Space Complexity: **\\(O(1)\\)**. *No extra arrays created! SDE-3 level optimization achieved bacho!*

---

### ✅ Completed | Chapter 4 — Array Problem-Solving Patterns

🧠 **Patterns Learned:**
* **Prefix Sum** subarray queries ko preprocessing se fast banata hai.
* **Difference Array** constant range update changes ko linear speed dekar reconstruction control karta hai.
* **Two Pointers** narrow binary directions search linear loops design deta hai.
* **Sliding Window** contiguous subranges updates linear scan coordinates maintain karta hai.
* **Kadane's** maximum subarray sums single scan DP algorithms logic run karta hai.

🎯 **Pattern Recognition Clues:**
* Ranges/queries sum \\(\longrightarrow\\) Prefix Sum.
* Constant ranges update operations \\(\longrightarrow\\) Difference Array.
* Sorted pair sums targets \\(\longrightarrow\\) Two-Pointer bilateral scan.
* Continuous segments optimization sum/len \\(\longrightarrow\\) Sliding Window.
* Unsorted maximum contiguous subarray sum \\(\longrightarrow\\) Kadane's.

⚠️ **Common Mistakes:**
* Window margins boundary index `i - K` out of bounds updates (off-by-one).
* Prefix Sum + Hashing mapping logic edge state `(0, 1)` initial check bhul jana.
* Boyer-Moore candidate updates duplicates relation checking elements variables ignore kar dena.

