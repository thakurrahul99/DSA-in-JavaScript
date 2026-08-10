**Arey bacho! Jaldi se apni-apni seats par baith jao, register nikal lo, aur blackboard par apna dhyan focus karo.**

Abhi tak humne pure DSA roadmap ke conceptual frameworks aur individual patterns ko deeply decode kar liya hai. Lekin real-world interviews (jaise Google, Amazon, ya Microsoft) mein, sabse mushkil kaam hota hai kisi anjaan problem ko padh kar sahi pattern, correct data structure, aur optimal execution direction choose karna. 

Aaj hum shuru kar rahe hain humara sabse dhasu aur rigorous segment—**The SDE Brain: Frequently Asked Problems & How to Think (Part 1)**! Is phase mein hum pure syllabus ke **150+ high-value problems** ko progressively zero se optimal level tak breakdown karenge. Hum solutions ko ratein ge nahi, balki unhe constraints aur observations se live derive karenge!

Yeh is dynamic masterclass ka **Part 1** hai. Chalo bacho, pen aur register uthao, aur shuru karte hain! 🚀

---

## SECTION 1: ARRAYS & STRINGS (PROBLEMS 1 - 12)

Bacho, Arrays aur Strings humari foundation hain. Sabse zyada questions yahin se aate hain, aur inke linear memory layout ka use karke humein optimal traversals dhoondhne hotey hain.

---

### PROBLEM 1: TWO SUM

#### 📝 Problem Statement:
*Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.*

*   **Examples:**
    *   `nums = [2, 7, 11, 15]`, `target = 9` → Output: `[0, 1]`
*   **Constraints:**
    *   2 <= nums.length <= 10^4
    *   -10^9 <= nums[i] <= 10^9

#### 🚨 How to Think (Teacher's Thinking Aloud) 🎙️:
> *"Sawaal ko calmly padhte hain bacho. Humein target sum $K$ banana hai do numbers ko choose karke. Constraint mein array size $10^4$ hai. Brute force kya hoga? Hum do nested loops chala sakte hain aur har pair $(i, j)$ ko check kar sakte hain, par woh $O(N^2)$ time lega. 
> 
> Bottleneck kya hai? Jab hum kisi element $X$ par khade hain, toh hum child loops mein $Y = target - X$ ko linear-way mein dhoondh rahe hain. Agar hum $Y$ ko instant $O(1)$ time mein look-up kar sakein, toh dhasu optimization ho jayegi! $O(1)$ look-up ke liye humein **Hash Map** se dosti karni hogi."*

#### 💡 Progressively Revealed Hints:
*   **Hint 1 (Observation):** Target equation: X + Y = target => Y = target - X.
*   **Hint 2 (Direction):** Pichle processed numbers ko unke indices ke saath save karte chalo.
*   **Hint 3 (Pattern/DS):** Map lookup pattern. Use JavaScript `Map`.

#### 💻 JavaScript Code:
```javascript
function twoSum(nums, target) {
    const map = new Map(); // key: element, value: index
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i]; //
        if (map.has(complement)) {
            return [map.get(complement), i]; // Instantly found index
        }
        map.set(nums[i], i); // Store for future lookup
    }
    return [];
}
```

#### 🔍 Dry Run:
Input: `nums = [2, 7, 11, 15]`, `target = 9`
1.  `i = 0` (val `2`): `complement = 9 - 2 = 7`. Map is empty. Save `2` → `{2 => 0}`.
2.  `i = 1` (val `7`): `complement = 9 - 7 = 2`. Map has `2`! Return `[map.get(2), 1]` → `[0, 1]`.

*   **Complexity:** Time: **O(N)**, Space: **O(N)**.
*   **Edge Cases:** Elements negative ho sakte hain, mapping flawlessly handles it.
*   **Same pattern in other problems:** 4Sum, Subarray Sum Equals K.
*   **Is problem se kya seekhna hai?** Check complement existence using Map to reduce nested loops to O(N)!

---

### PROBLEM 2: REMOVE DUPLICATES FROM SORTED ARRAY

#### 📝 Problem Statement:
*Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. Return the number of unique elements.*

*   **Examples:**
    *   `nums =` → Output: `2`, `nums = [1, 2, _]`
*   **Constraints:**
    *   1 <= nums.length <= 3 × 10^4
    *   `nums` is sorted in non-decreasing order.

#### 🚨 How to Think 🎙️:
> *"Bacho, dhyan se input ko dekho. Array is **already sorted**! Yeh bohot bada clue hai. Iska matlab saare duplicates hamesha aapas mein adjacent honge. 
> 
> Brute force kya karega? Hum ek extra array/Set lekar unique elements copy karenge aur fir original mein dump karenge, par interviewers strictly **in-place** demand karte hain. Extra space block hai. 
> 
> Kaise karein? Hum do pointers maintain kar sakte hain: ek pointer `i` unique elements ko overwrite karne ke liye, aur dusra `j` pure array ko scan karne ke liye."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Any unique element will be strictly greater than the last unique element recorded.
*   **Hint 2 (Direction):** Keep a pointer `writeIdx` at index `1`. Iterate `readIdx` from `1` to `N-1`.
*   **Hint 3 (Pattern/DS):** Two Pointers pattern on sorted array.

#### 💻 JavaScript Code:
```javascript
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    let writeIdx = 1; // Position to insert next unique element
    for (let readIdx = 1; readIdx < nums.length; readIdx++) {
        if (nums[readIdx] !== nums[readIdx - 1]) {
            nums[writeIdx] = nums[readIdx]; // Overwrite in-place
            writeIdx++;
        }
    }
    return writeIdx;
}
```

#### 🔍 Dry Run:
Input: `nums =`
1.  `writeIdx = 1`. `readIdx = 1`: `nums === nums` (both are 1). Skip.
2.  `readIdx = 2`: `nums (2) !== nums (1)`. Overwrite: `nums[writeIdx] = nums` → `nums = 2`. Increment `writeIdx` to 2.
3.  Loop ends. Unique count is `2`. Array state: ``.

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.
*   **Edge Cases:** Array with all unique elements or single element.
*   **Same pattern in other problems:** Move Zeroes, Remove Element.
*   **Is problem se kya seekhna hai?** Sorted status allows duplicate grouping. Use in-place overwrite pointers to achieve O(1) space!

---

### PROBLEM 3: ROTATE ARRAY

#### 📝 Problem Statement:
*Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.*

*   **Examples:**
    *   `nums =`, `k = 3` → Output: ``
*   **Constraints:**
    *   1 <= nums.length <= 10^5
    *   0 <= k <= 10^5

#### 🚨 How to Think 🎙️:
> *"Array rotate karna hai. Agar hum $k$ times pure array ko ek-ek shift karenge, toh complexity $O(N × k)$ ho jayegi, jo ki $10^{10}$ steps le sakti hai. TLE aa jayega! 
> 
> Bottleneck: Repeated shifting. Extra memory allow karein toh $O(N)$ space mein direct temporary array mein modulo indices copy ho sakti hai, par humein $O(1)$ space optimization chahiye. 
> 
> Let's make an observation: Agar hum pooray array ko reverse kar dein, toh elements cyclic-way mein correct slots ke paas toh aa jate hain, bas unka mutual ordering kharab ho jata hai. Unhe individual reverse karke recover kiya ja sakta hai!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** k values rotating actually cycles elements. First modify k = k % N to avoid redundant complete cycles.
*   **Hint 2 (Direction):** Reversing the whole array brings right elements to left. But they are backward. Reverse first k elements, then reverse the remaining N - k elements.
*   **Hint 3 (Pattern/DS):** Boundary-based Array Reversal optimization.

#### 💻 JavaScript Code:
```javascript
function rotate(nums, k) {
    const n = nums.length;
    k = k % n; // Clean up redundant rotations
    
    // Reverse Helper
    const reverse = (arr, start, end) => {
        while (start < end) {
            const temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    };

    reverse(nums, 0, n - 1);     // Step 1: Reverse whole array
    reverse(nums, 0, k - 1);     // Step 2: Reverse first k elements
    reverse(nums, k, n - 1);     // Step 3: Reverse remaining n-k elements
}
```

#### 🔍 Dry Run:
Input: `nums =`, `k = 2`
1.  Whole reverse → ``
2.  Reverse first $2$ (k=2) → ``
3.  Reverse remaining $3$ → ``. Done!

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.
*   **Edge Cases:** k >= n (handled by modulo operator), k = 0.
*   **Is problem se kya seekhna hai?** Spatial rotations can be translated into symmetric matrix/array reversals to preserve in-place memory!

---

### PROBLEM 4: MAXIMUM SUBARRAY (KADANE'S ALGORITHM)

#### 📝 Problem Statement:
*Given an integer array `nums`, find the subarray with the largest sum and return its sum.*

*   **Examples:**
    *   `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]` → Output: `6` (Subarray: `[4, -1, 2, 1]`)
*   **Constraints:**
    *   1 <= nums.length <= 10^5
    *   -10^4 <= nums[i] <= 10^4

#### 🚨 How to Think 🎙️:
> *"Subarray maximum sum! Brute force do nested loops chala kar har subarray $(i, j)$ ka sum nikalega, taking $O(N^2)$. Lekin $N = 10^5$ constraints standard par nested loop crash ho jayega! 
> 
> Bottleneck: Sum recalculations. 
> 
> Let's think: Jab main kisi index `i` par khada hoon, toh mere paas do choices hain: kya main pichle running sum ko aage carry karoon ya fir pichle sum ko discard karke *yahan se ek naya subarray shuru karoon*? Agar pichla sum negative ho chuka hai, toh use carry karne se mera potential sum sirf ghat hi sakta hai bacho! Isliye discard it!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** A negative prefix sum is always worse than starting a fresh prefix.
*   **Hint 2 (Direction):** Maintain `currSum` and `maxSum`. For each element, `currSum = Math.max(num, currSum + num)`.
*   **Hint 3 (Pattern/DS):** Kadane's Greedy / Dynamic Programming array sweep.

#### 💻 JavaScript Code:
```javascript
function maxSubArray(nums) {
    let currSum = nums;
    let maxSum = nums;
    for (let i = 1; i < nums.length; i++) {
        // Either join current elements chain or start fresh
        currSum = Math.max(nums[i], currSum + nums[i]);
        maxSum = Math.max(maxSum, currSum);
    }
    return maxSum;
}
```

#### 🔍 Dry Run:
Input: `nums = [-2, 1, -3]`
1.  `currSum = -2`, `maxSum = -2`
2.  `i = 1` (val `1`): `currSum = max(1, -2 + 1) = 1`. `maxSum = max(-2, 1) = 1`.
3.  `i = 2` (val `-3`): `currSum = max(-3, 1 + -3) = -2`. `maxSum = max(1, -2) = 1`.
4.  Returns `1`.

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.
*   **Edge Cases:** All negative numbers (handled perfectly as we take the least negative value).
*   **Is problem se kya seekhna hai?** Local state decisions (joining vs starting fresh) simplify global range problems to linear time!

---

### PROBLEM 5: MERGE SORTED ARRAY

#### 📝 Problem Statement:
*You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`. Merge `nums2` into `nums1` as one sorted array in-place.*

*   **Examples:**
    *   `nums1 =`, `m = 3`, `nums2 =`, `n = 3` → Output: ``
*   **Constraints:**
    *   `nums1.length === m + n`
    *   `nums2.length === n`

#### 🚨 How to Think 🎙️:
> *"Sawaal kehta hai in-place merge karna hai `nums1` mein. `nums1` ke peeche humein extra space zero entries ke sath empty di hui hai. 
> 
> Agar hum traditional pointer traversal left to right lagayenge, toh `nums1` ke elements overwrite ho jayenge aur calculations distorted ho jayegi. 
> 
> Let's make an observation: Kyunki `nums1` ke rightmost slots khali (zero) hain, kyun na hum pointers ko right end par set karein aur **sabse bade elements ko pehle peeche se fill karna shuru karein**! Isse dynamic element overwriting ka risk zero ho jayega bacho."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Merging from left forces elements shift. Merging from right utilizes empty slots.
*   **Hint 2 (Direction):** Set three pointers: `p1 = m - 1` (end of active `nums1`), `p2 = n - 1` (end of `nums2`), and `p = m + n - 1` (absolute end of `nums1`).
*   **Hint 3 (Pattern/DS):** Backwards Two-Pointer placement.

#### 💻 JavaScript Code:
```javascript
function merge(nums1, m, nums2, n) {
    let p1 = m - 1;
    let p2 = n - 1;
    let p = m + n - 1;

    while (p2 >= 0) {
        if (p1 >= 0 && nums1[p1] > nums2[p2]) {
            nums1[p] = nums1[p1]; // Place larger element at the back
            p1--;
        } else {
            nums1[p] = nums2[p2];
            p2--;
        }
        p--;
    }
}
```

#### 🔍 Dry Run:
Input: `nums1 =`, `m = 3`, `nums2 =`, `n = 2`
1.  `p1 = 2` (val `6`), `p2 = 1` (val `2`), `p = 4`.
2.  `nums1 (6) > nums2 (2)`. `nums1 = 6`. `p1` becomes `1`, `p` becomes `3`.
3.  Next run: `nums1 (5) > nums2 (2)`. `nums1 = 5`. `p1` becomes `0`, `p` becomes `2`.
4.  Next: `nums1 (4) > nums2 (2)`. `nums1 = 4`. `p1` becomes `-1`, `p` becomes `1`.
5.  Now `p1 < 0`. Move remaining elements from `nums2`.
6.  `nums1 = nums2 (2)`. `p2` becomes `0`, `p` becomes `0`.
7.  `nums1 = nums2 (1)`. Array sorted: ``.

*   **Complexity:** Time: **O(M + N)**, Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** When working with structured arrays, backward traversal often completely eliminates element shifting costs!

---

### PROBLEM 6: REVERSE A STRING (IN-PLACE)

#### 📝 Problem Statement:
*Write a function that reverses a string. The input string is given as an array of characters `s`. You must do this by modifying the input array in-place with $O(1)$ extra memory.*

*   **Examples:**
    *   `s = ["h","e","l","l","o"]` → Output: `["o","l","l","e","h"]`
*   **Constraints:**
    *   1 <= s.length <= 10^5

#### 🚨 How to Think 🎙️:
> *"String reverse in-place. Extra space strict $O(1)$ limit hai. Hum aam aur direct recursion recursion stacking use nahi karenge. 
> 
> Rasta bohot clear hai bacho: Ek swap loop lagao, ek pointer left end par rakho aur dusra right end par, aur aapas mein swap karte huye centre ki taraf converge karo."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Reversing swap boundaries: Element at index `i` is swapped with element at index `N - 1 - i`.
*   **Hint 2 (Direction):** Start `left = 0`, `right = s.length - 1` and loop until they cross.
*   **Hint 3 (Pattern/DS):** Converging Two Pointers.

#### 💻 JavaScript Code:
```javascript
function reverseString(s) {
    let left = 0;
    let right = s.length - 1;
    while (left < right) {
        // Swap indices in-place
        const temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
}
```

#### 🔍 Dry Run:
Input: `s = ["a", "b", "c", "d"]`
1.  `left = 0` ("a"), `right = 3` ("d"). Swap → `["d", "b", "c", "a"]`. `left = 1`, `right = 2`.
2.  `left = 1` ("b"), `right = 2` ("c"). Swap → `["d", "c", "b", "a"]`. `left = 2`, `right = 1`.
3.  Loop terminates as `left >= right`. Correct!

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Converging pointers offer the cleanest O(1) in-place reverse logic.

---

### PROBLEM 7: VALID PALINDROME

#### 📝 Problem Statement:
*Given a string `s`, return `true` if it is a palindrome after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters.*

*   **Examples:**
    *   `s = "A man, a plan, a canal: Panama"` → Output: `true` (String cleans to: "amanaplanacanalpanama")
*   **Constraints:**
    *   1 <= s.length <= 2 × 10^5

#### 🚨 How to Think 🎙️:
> *"Pahle pure string ko clean karne ke liye copy create karein? Agar string size $2 × 10^5$ hai, toh duplicate copy memory limit scale badhayegi. 
> 
> Kya hum dynamic pointer checks lagate waqt alphanumeric values scan kar sakte hain bina memory backup ke? Bilkul! 
> 
> Hum do pointers use karenge: left aur right. Agar raaste mein koi non-alphanumeric character mile, toh use skipped out kardo pointers move karke!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Regular expressions are handy for character cleaning, but we can do character code boundary checks.
*   **Hint 2 (Direction):** Create a helper function `isAlphanumeric`. Scan with `left` and `right`. If `left` is not alphanumeric, `left++`. If `right` is not alphanumeric, `right--`.
*   **Hint 3 (Pattern/DS):** Two Pointer boundary convergence.

#### 💻 JavaScript Code:
```javascript
function isPalindrome(s) {
    let left = 0;
    let right = s.length - 1;

    const isAlphanumeric = (char) => {
        const code = char.charCodeAt(0); //
        return (code >= 48 && code <= 57) ||  // 0-9
               (code >= 65 && code <= 90) ||  // A-Z
               (code >= 97 && code <= 122);   // a-z
    };

    while (left < right) {
        while (left < right && !isAlphanumeric(s[left])) {
            left++; // Skip invalid characters on left
        }
        while (left < right && !isAlphanumeric(s[right])) {
            right--; // Skip invalid characters on right
        }
        if (s[left].toLowerCase() !== s[right].toLowerCase()) {
            return false; // Mismatch!
        }
        left++;
        right--;
    }
    return true;
}
```

#### 🔍 Dry Run:
Input: `s = "a, b"`
1.  `left = 0` ("a"), `right = 3` ("b").
2.  Compare "a" and "b" (both alphanumeric). Mismatch! Return `false`.

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.
*   **Is problem se kya seekhna hai?** Pointer skipping filters inputs dynamically in a single pass without any copy overhead!

---

### PROBLEM 8: REVERSE WORDS IN A STRING

#### 📝 Problem Statement:
*Given an input string `s`, reverse the order of the words.*

*   **Examples:**
    *   `s = "the sky is blue"` → Output: `"blue is sky the"`
*   **Constraints:**
    *   1 <= s.length <= 10^4
    *   `s` contains English letters, digits, and spaces.

#### 🚨 How to Think 🎙️:
> *"Words reverse karne hain, lekin string mein single ya multiple spaces adjacent ho sakte hain, jise humein filter karna hoga. 
> 
> Brute force kya hoga? JS built-ins call karo: `s.trim().split(/\s+/).reverse().join(' ')`. Ye acceptable hai, par system mechanics samajhne ke liye hum manual algorithm derive karenge bacho. 
> 
> Kaise reverse karein? First, pure string characters ko reverse kar do. Isse words apne positions ke relative right order mein toh aa jate hain par unke characters backward ho jate hain. Then, loop lagakar har ek word ke characters ko independently re-reverse kar do!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Pure reverse brings last words to front, but jumbles internal ordering of words.
*   **Hint 2 (Direction):** Clean up extra spaces first, reverse the whole array of characters, then reverse each individual word.
*   **Hint 3 (Pattern/DS):** Three-step Reversal Technique.

#### 💻 JavaScript Code:
```javascript
function reverseWords(s) {
    // Phase 1: Clean spaces and build char array
    const sArr = [];
    let i = 0;
    while (i < s.length) {
        if (s[i] !== ' ') {
            if (sArr.length > 0 && sArr[sArr.length - 1] !== ' ') {
                // Ensure single space separation
            }
            sArr.push(s[i]);
        } else if (sArr.length > 0 && sArr[sArr.length - 1] !== ' ') {
            sArr.push(' ');
        }
        i++;
    }
    // Remove trailing space if exists
    if (sArr[sArr.length - 1] === ' ') sArr.pop();

    const reverse = (arr, start, end) => {
        while (start < end) {
            const temp = arr[start];
            arr[start] = arr[end];
            arr[end] = temp;
            start++;
            end--;
        }
    };

    // Phase 2: Reverse whole char array
    reverse(sArr, 0, sArr.length - 1);

    // Phase 3: Reverse each individual word
    let start = 0;
    for (let end = 0; end <= sArr.length; end++) {
        if (end === sArr.length || sArr[end] === ' ') {
            reverse(sArr, start, end - 1);
            start = end + 1;
        }
    }

    return sArr.join('');
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)** for character representation space.
*   **Is problem se kya seekhna hai?** Multi-step transformations (Whole reverse → Local segment reverse) can bypass structural constraints cleanly.

---

### PROBLEM 9: STRING COMPRESSION

#### 📝 Problem Statement:
*Given an array of characters `chars`, compress it using the following algorithm: Begin with an empty string `s`. For each group of consecutive repeating characters in `chars`: if length of group is 1, append char to `s`; else append char followed by group's length. Modify the input array in-place and return its new length.*

*   **Examples:**
    *   `chars = ["a","a","b","b","c","c","c"]` → Output: Return `6`, first 6 chars modify to `["a","2","b","2","c","3"]`.
*   **Constraints:**
    *   1 <= chars.length <= 2000

#### 🚨 How to Think 🎙️:
> *"String compression in-place matrix. Input size limit only 2000. We must use Two Pointers to overwrite the input array dynamically while we scan consecutive occurrences groups. 
> 
> Keep a `write` pointer and a `read` pointer. Loop through the array to find the starting index and ending index of each repeating group of characters."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Group tracking: Use a pointer `anchor` at the start of a repeating group, and `read` to find its end.
*   **Hint 2 (Direction):** Group length is `read - anchor + 1`. If length > 1, convert it to a string and write its digits one by one to `chars[write++]`.
*   **Hint 3 (Pattern/DS):** Two Pointers interval overwrite.

#### 💻 JavaScript Code:
```javascript
function compress(chars) {
    let write = 0;
    let anchor = 0;

    for (let read = 0; read < chars.length; read++) {
        // If we reach the end of array or see a different character
        if (read + 1 === chars.length || chars[read] !== chars[read + 1]) {
            chars[write] = chars[anchor]; // Write the character
            write++;

            const count = read - anchor + 1;
            if (count > 1) {
                const countStr = count.toString();
                for (let c of countStr) {
                    chars[write] = c; // Write count digits
                    write++;
                }
            }
            anchor = read + 1; // Move anchor to next group start
        }
    }
    return write;
}
```

#### 🔍 Dry Run:
Input: `chars = ["a","a","b"]`
1.  `read = 0`: `read+1` is same.
2.  `read = 1`: `chars !== chars`. Write char: `chars = "a"`. `count = 1 - 0 + 1 = 2`. Write count: `chars = "2"`. `anchor` becomes 2. `write` is 2.
3.  `read = 2`: End of array. Write char: `chars = "b"`. `count = 1`. No count written. `write` becomes 3.
4.  Returns `3`. Input modified to `["a", "2", "b"]`.

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Scanning consecutive ranges and overwriting behind the read frontier lets us execute operations in O(1) extra space!

---

### PROBLEM 10: LONGEST COMMON PREFIX

#### 📝 Problem Statement:
*Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string.*

*   **Examples:**
    *   `strs = ["flower","flow","flight"]` → Output: `"fl"`
*   **Constraints:**
    *   1 <= strs.length <= 200
    *   0 <= strs[i].length <= 200

#### 🚨 How to Think 🎙️:
> *"Longest common prefix! Humein saare strings ke initial character columns matching coordinates dhoondhne hain. 
> 
> Brute force kya hoga? Pahle do strings ka common prefix nikalo, fir us result ko third string ke prefix se match karo, and so on (Horizontal Scanning). 
> 
> Isse badiya hum **Vertical Scanning** kar sakte hain! Isme hum first string ke characters par character-by-character scan chalate hain, aur har level par parallel strings ke same index character ko check karte hain."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Prefix cannot be longer than the shortest string in the array.
*   **Hint 2 (Direction):** Iterate through characters of the first string at index `i`. For each char, check if `i` is out of bounds for other strings or if they mismatch.
*   **Hint 3 (Pattern/DS):** Vertical Scanning algorithm.

#### 💻 JavaScript Code:
```javascript
function longestCommonPrefix(strs) {
    if (!strs || strs.length === 0) return "";
    
    // Scan characters of the first string vertically
    for (let i = 0; i < strs.length; i++) {
        const char = strs[i]; //
        for (let j = 1; j < strs.length; j++) {
            // If current string index is out of bounds or character mismatches
            if (i === strs[j].length || strs[j][i] !== char) {
                return strs.substring(0, i); //
            }
        }
    }
    return strs;
}
```

#### 🔍 Dry Run:
Input: `strs = ["flow", "flight"]`
1.  `i = 0` ('f'): Mapped with "flight" $→$ match.
2.  `i = 1` ('l'): Mapped with "flight" $→$ match.
3.  `i = 2` ('o'): Mapped with "flight" ('i') $→$ mismatch! Returns substring `0` to `2` of "flow" → `"fl"`.

*   **Complexity:** Time: **O(S)** (where S is sum of all characters in strs), Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Vertical alignment scanning detects failure boundaries at the earliest possible instant.

---

### PROBLEM 11: IMPLEMENT strStr() (FIRST OCCURRENCE INDEX)

#### 📝 Problem Statement:
*Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.*

*   **Examples:**
    *   `haystack = "sadbutsad"`, `needle = "sad"` → Output: `0`
*   **Constraints:**
    *   1 <= haystack.length, needle.length <= 10^4

#### 🚨 How to Think 🎙️:
> *"Haystack mein needle dhoondhna hai. JavaScript built-in `haystack.indexOf(needle)` runs instantly, par interviewers strict manual logic implementation demand karte hain. 
> 
> Brute force kya hoga? Haystack ke har index $i$ se needle matching windows match karo, complexity $O(N × M)$. 
> 
> SDE level par hum standard linear algorithm KMP (Knuth-Morris-Pratt) implement kar sakte hain bacho, par brute-force double loop string indexing transitions ko standard bounds checks ke sath safety check lagana seekhte hain."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** We only need to scan haystack up to `haystack.length - needle.length` index. Anything further cannot fit the needle.
*   **Hint 2 (Direction):** Loop `i` from `0` to `H - N`. For each index, check if slice matches the needle.
*   **Hint 3 (Pattern/DS):** Sliding Window / Substring matching.

#### 💻 JavaScript Code (Clean Manual Matcher):
```javascript
function strStr(haystack, needle) {
    const hLen = haystack.length;
    const nLen = needle.length;
    if (nLen === 0) return 0;

    // We only need to check up to hLen - nLen boundary
    for (let i = 0; i <= hLen - nLen; i++) {
        let j = 0;
        while (j < nLen && haystack[i + j] === needle[j]) {
            j++; // Match
        }
        if (j === nLen) {
            return i; // Found entire substring!
        }
    }
    return -1;
}
```

*   **Complexity:** Time: Worst-case **O(H × N)**, Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Restricting the boundary of outer loops (up to H - N) prevents useless checks.

---

### PROBLEM 12: PRODUCT OF ARRAY EXCEPT SELF

#### 📝 Problem Statement:
*Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`. Solve it in $O(N)$ time and without using division.*

*   **Examples:**
    *   `nums =` → Output: ``
*   **Constraints:**
    *   2 <= nums.length <= 10^5
    *   The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

#### 🚨 How to Think 🎙️:
> *"Division strictly forbidden hai bacho! Brute force do nested loops chala kar dynamic products calculate karega ($O(N^2)$). Lekin arrays size $10^5$ is dynamic calculation rate par timeout ho jayegi! 
> 
> Let's think: `answer[i]` is actually (Product of all elements to the left of `i`) $×$ (Product of all elements to the right of `i`). 
> 
> Hum left elements product prefix array aur right elements product suffix array pre-calculate kar sakte hain! Aur space optimize karne ke liye, hum target array ko hi left prefixes hold karne ke liye use karenge, aur right suffix ko constant variable pointer se linearly track karenge in-place."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** An index partition splits prefix and suffix components.
*   **Hint 2 (Direction):** Build running prefix products inside `result`. `result[i] = result[i-1] * nums[i-1]`.
*   **Hint 3 (Pattern/DS):** Prefix-Suffix Product array sweeps.

#### 💻 JavaScript Code:
```javascript
function productExceptSelf(nums) {
    const n = nums.length;
    const result = new Array(n);

    // Step 1: Compute left prefixes product
    result = 1; // Left of index 0 is empty
    for (let i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1]; //
    }

    // Step 2: Compute running right suffixes product on the fly
    let rightProduct = 1;
    for (let i = n - 1; i >= 0; i--) {
        result[i] = result[i] * rightProduct; // Multiply left and right product
        rightProduct *= nums[i]; // Update rightProduct
    }

    return result;
}
```

#### 🔍 Dry Run:
Input: `nums =`
1.  Compute Prefixes: `result = 1`, `result = 1 * 1 = 1`, `result = 1 * 2 = 2`. `result` is ``.
2.  Compute Suffixes from right end:
    *   `i = 2`: `result = result * rightProduct (1) = 2 * 1 = 2`. `rightProduct` becomes `1 * 3 = 3`.
    *   `i = 1`: `result = result * rightProduct (3) = 1 * 3 = 3`. `rightProduct` becomes `3 * 2 = 6`.
    *   `i = 0`: `result = result * rightProduct (6) = 1 * 6 = 6`.
3.  Returns ``. Exactly correct!

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary (ignoring output representation array).
*   **Is problem se kya seekhna hai?** Array partitioning (Prefix/Suffix separation) resolves complex products/divisions cleanly in linear space!

---

## SECTION 2: HASHING (PROBLEMS 13 - 20)

Bacho, Hashing computer science ki ultimate performance hack hai. Jab humein elements values ki frequency ya existence $O(1)$ constant time mein check karni ho, Hash Table humesha humara go-to weapon banega.

---

### PROBLEM 13: GROUP ANAGRAMS

#### 📝 Problem Statement:
*Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.*

*   **Examples:**
    *   `strs = ["eat","tea","tan","ate","nat","bat"]` → Output: `[["bat"],["nat","tan"],["ate","eat","tea"]]`
*   **Constraints:**
    *   1 <= strs.length <= 10^4
    *   0 <= strs[i].length <= 100

#### 🚨 How to Think 🎙️:
> *"Anagram strings kya hoti hain? Woh strings jinke character counts strictly same hon, bas permutation sequence different ho. 
> 
> Observation: Agar hum kisi string ke characters ko sorted order mein align kar dein, toh saari anagrams ek exact unique base key generate karengi bacho! 
> (E.g. "eat", "tea", "ate" ka sorted form "aet" hoga). 
> 
> SDE Pattern: Sorting mapping + Hash Map grouping. Map key sorted string hogi, aur value un authentic words ki lists ho sakti hai!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Anagrams sorted together form identical keys.
*   **Hint 2 (Direction):** For each string, `sortedKey = str.split('').sort().join('')`.
*   **Hint 3 (Pattern/DS):** HashMap bucket mapping.

#### 💻 JavaScript Code:
```javascript
function groupAnagrams(strs) {
    const map = new Map(); // key: sorted_word, value: array_of_anagrams
    for (let str of strs) {
        // Generate sorted unique key
        const sortedKey = str.split('').sort().join(''); //
        if (!map.has(sortedKey)) {
            map.set(sortedKey, []); //
        }
        map.get(sortedKey).push(str); // Group anagram
    }
    return Array.from(map.values()); // Convert buckets to final arrays list
}
```

#### 🔍 Dry Run:
Input: `strs = ["eat", "tea"]`
1.  `str = "eat"`: sortedKey = `"aet"`. Create map entry: `{"aet" => ["eat"]}`.
2.  `str = "tea"`: sortedKey = `"aet"`. Appended: `{"aet" => ["eat", "tea"]}`.
3.  Returns: `[["eat", "tea"]]`.

*   **Complexity:** Time: **O(N · K log K)** (where K is max word length as we sort characters), Space: **O(N · K)**.
*   **Is problem se kya seekhna hai?** Key normalization (e.g. sorting characters) creates perfect canonical keys for multi-bucket mapping inside a Map!

---

### PROBLEM 14: LONGEST CONSECUTIVE SEQUENCE

#### 📝 Problem Statement:
*Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence. Solve it in $O(N)$ time.*

*   **Examples:**
    *   `nums =` → Output: `4` (Sequence: ``)
*   **Constraints:**
    *   0 <= nums.length <= 10^5
    *   -10^9 <= nums[i] <= 10^9

#### 🚨 How to Think 🎙️:
> *"Sawaal linear time $O(N)$ optimization demand karta hai! Agar hum array ko sort karenge, toh sorting complexity $O(N log N)$ standard limit ko push karegi. Sorting forbidden hai. 
> 
> Humein bina sort kiye sequences boundaries identify karni hain. 
> 
> Let's make an observation: Koi element $X$ kisi sequence ka starting point kab hoga? Jab $X-1$ array mein exist nahi karta ho! Agar $X-1$ array mein already hai, toh $X$ toh us sequence ke beech ka element hai, use search range shuru karne ki zaroorati nahi hai bacho. 
> 
> Map/Set constant lookup use karke hum starting points identify karenge, aur wahan se sequence length explore karenge!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Search lookup in Set takes O(1).
*   **Hint 2 (Direction):** Insert all items into a `Set`. Iterate elements. If `set.has(num - 1)` is false, it's a start of sequence. Traverse forwards `num + 1` to compute length.
*   **Hint 3 (Pattern/DS):** HashSet boundary-checks pattern.

#### 💻 JavaScript Code:
```javascript
function longestConsecutive(nums) {
    const numSet = new Set(nums); // Remove duplicates and make O(1) lookups
    let longestStreak = 0;

    for (let num of nums) {
        // Optimize: Check if current number is starting point of a streak
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentStreak = 1;

            // Explore streak length
            while (numSet.has(currentNum + 1)) {
                currentNum += 1;
                currentStreak += 1;
            }

            longestStreak = Math.max(longestStreak, currentStreak);
        }
    }

    return longestStreak;
}
```

#### 🔍 Dry Run:
Input: `nums =`
1.  `numSet = Set {100, 4, 200, 1, 3, 2}`.
2.  `num = 100`: `100 - 1 = 99` not in Set. Start streak! `101` not in Set. Streak = 1.
3.  `num = 4`: `3` is in Set. Mismatch (not starting point). Skip.
4.  `num = 1`: `0` not in Set. Start streak! Explore: `2` (Yes), `3` (Yes), `4` (Yes). Streak = 4.
5.  Returns `4`.

*   **Complexity:** Time: **O(N)** because each element is visited at most 2 times (once in loop, once inside while exploration), Space: **O(N)**.
*   **Is problem se kya seekhna hai?** Starting point detection (`!set.has(num - 1)`) prevents redundant overlapping traversals, keeping the overall complexity strictly linear!

---

### PROBLEM 15: SUBARRAY SUM EQUALS K (NEGATIVE VALUES ALLOWED)

#### 📝 Problem Statement:
*Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals to `k`.*

*   **Examples:**
    *   `nums =`, `k = 2` → Output: `2`
*   **Constraints:**
    *   1 <= nums.length <= 2 × 10^4
    *   -1000 <= nums[i] <= 1000

#### 🚨 How to Think 🎙️:
> *"Sawaal contiguous subarray sum equals $K$ pucho hai bacho, par yahan negative values allowed hain, isiliye dynamic Sliding Window pointer constraints fail ho jayengi! 
> 
> Let's derive Prefix Sum relationship. Maan lo current index `i` par running prefix sum `currSum` hai. Humein target subarray ka sum $K$ chahiye:
> $$currSum - prevSum = K => prevSum = currSum - K$$
> Iska matlab, agar pichle cumulative sums frequency records mein humare paas `currSum - K` value exist karti hai, toh woh valid subarrays count provide karegi instantly! 
> 
> Map/Set frequency counting logic are exceptionally helpful here."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Target difference matching matches prefix subtraction logic.
*   **Hint 2 (Direction):** Maintain `currSum` running frequency inside JavaScript `Map`. Map initialization: `map.set(0, 1)` to handle self-matching elements.
*   **Hint 3 (Pattern/DS):** Prefix Sum + Hashing frequency tracker.

#### 💻 JavaScript Code:
```javascript
function subarraySum(nums, k) {
    const map = new Map();
    map.set(0, 1); // Base sum 0 occurs 1 time initially
    let currSum = 0;
    let count = 0;

    for (let num of nums) {
        currSum += num; // running prefix sum
        const targetComplement = currSum - k; //
        
        if (map.has(targetComplement)) {
            count += map.get(targetComplement); // Add occurrences frequency
        }
        
        map.set(currSum, (map.get(currSum) || 0) + 1); // Update frequency map
    }

    return count;
}
```

*   **Complexity:** Time: **O(N)** average, Space: **O(N)**.
*   **Is problem se kya seekhna hai?** Prefix dynamic subtraction mapping turns quadratic ranges calculation to O(N) space-time optimizations!

---

### PROBLEM 16: INTERSECTION OF TWO ARRAYS

#### 📝 Problem Statement:
*Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must be unique and you may return the result in any order.*

*   **Examples:**
    *   `nums1 =`, `nums2 =` → Output: ``
*   **Constraints:**
    *   1 <= nums1.length, nums2.length <= 1000

#### 🚨 How to Think 🎙️:
> *"Intersection element lookup check linear-way nested searches run $O(N^2)$ checks. Extra duplicates reject criteria rules are mandatory. 
> 
> Let's make an observation: Set handles uniqueness naturally. If we can insert `nums1` into a Set, then we can check `nums2` elements existence in $O(1)$."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Intersections must only have unique items in output result.
*   **Hint 2 (Direction):** Build Set from `nums1`. Filter unique elements of `nums2` matching entries inside Set.
*   **Hint 3 (Pattern/DS):** Set lookup filtering.

#### 💻 JavaScript Code:
```javascript
function intersection(nums1, nums2) {
    const set1 = new Set(nums1); //
    const resultSet = new Set(); // Stores intersection unique items

    for (let num of nums2) {
        if (set1.has(num)) {
            resultSet.add(num); // Found intersection!
        }
    }

    return Array.from(resultSet); // Convert back to array
}
```

*   **Complexity:** Time: **O(N + M)**, Space: **O(N + M)**.
*   **Is problem se kya seekhna hai?** Lookups can be instantly simplified via unique Sets!

---

### PROBLEM 17: FIRST REPEATING ELEMENT

#### 📝 Problem Statement:
*Given an array of integers `nums`, find the first repeating element. We need to find the element that occurs more than once and whose index of first occurrence is smallest.*

*   **Examples:**
    *   `nums =` → Output: `5` (Index is smallest among duplicates elements: 5 occurs at index 1 and repeats, 3 occurs at index 2 and repeats).
*   **Constraints:**
    *   1 <= nums.length <= 10^5

#### 🚨 How to Think 🎙️:
> *"Humein woh duplicate element return karna hai jo array mein **sabse pahle** dikha ho. 
> 
> Brute force kya karega? Do loops lagakar check target run karega. 
> 
> Observation: Agar hum elements traverse right end se shuru karein (backwards), aur running elements ko ek Set mein track karein, toh hum minimum index tracking update variables constant speed preserve track safely maintain kar sakte hain!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Traversing backwards preserves first duplicate candidate index efficiently.
*   **Hint 2 (Direction):** Initialize `minRepeatingIndex = -1`. Loop `i` from `N-1` down to `0`. If set contains element, update candidate. Else, insert element inside Set.
*   **Hint 3 (Pattern/DS):** Reverse traverse Hashing tracker.

#### 💻 JavaScript Code:
```javascript
function firstRepeatingElement(nums) {
    const set = new Set(); //
    let firstRepeated = -1;

    // Scan backwards to naturally preserve leftmost repeating element index
    for (let i = nums.length - 1; i >= 0; i--) {
        if (set.has(nums[i])) {
            firstRepeated = nums[i]; // Update earliest seen repeating element
        } else {
            set.add(nums[i]); //
        }
    }

    return firstRepeated;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)**.
*   **Is problem se kya seekhna hai?** Backwards traversing structures naturally solve leftmost/minimum boundary index queries effortlessly!

---

### PROBLEM 18: VALID SUDOKU

#### 📝 Problem Statement:
*Determine if a $9 × 9$ Sudoku board is valid. Only the filled cells need to be validated according to the following rules: Each row must contain digits 1-9 without repetition. Each column must contain digits 1-9 without repetition. Each of the nine $3 × 3$ sub-boxes of the grid must contain digits 1-9 without repetition.*

*   **Constraints:**
    *   board is 9 × 9 matrix grid.

#### 🚨 How to Think 🎙️:
> *"Sudoku board validation matrix checks! Humein teen parameters check karne hain parallel: Row repeats, Column repeats, and local $3 × 3$ box repeats. 
> 
> Kaise karein? Hum character string keys track hash maps ya Sets ke andar save kar sakte hain! 
> 
> Maan lo hum coordinates index `(r, c)` ke element `val` par hain.
> - Row path key: `"row" + r + val`
> - Column path key: `"col" + c + val`
> - Local Box key: `"box" + Math.floor(r/3) + "-" + Math.floor(c/3) + val`
> 
> Agar inme se koi bhi key humare Set mein pehle se hai, toh board invalid hai bacho!"*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Standardizing sub-box coordinates: `Math.floor(row / 3)` and `Math.floor(col / 3)` yields 3 × 3 indices spanning `0` to `2`.
*   **Hint 2 (Direction):** Build unique key tags. Add to a master Set. If duplicate exists, set violates rules.
*   **Hint 3 (Pattern/DS):** Multi-dimensional Hashing tracking.

#### 💻 JavaScript Code:
```javascript
function isValidSudoku(board) {
    const seen = new Set(); // Master tracker

    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            const val = board[r][c];
            if (val !== '.') {
                const rowKey = `row-${r}-${val}`;
                const colKey = `col-${c}-${val}`;
                const boxKey = `box-${Math.floor(r / 3)}-${Math.floor(c / 3)}-${val}`; //

                // If any of the keys already exist, duplicate detected!
                if (seen.has(rowKey) || seen.has(colKey) || seen.has(boxKey)) {
                    return false;
                }

                seen.add(rowKey);
                seen.add(colKey);
                seen.add(boxKey);
            }
        }
    }
    return true;
}
```

*   **Complexity:** Time: **O(1)** (Since board dimensions are constant 9 × 9 = 81 cells), Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Composite String keys represent complex multi-dimensional constraint grids parameters flawlessly inside a simple Set!

---

### PROBLEM 19: ISOMORPHIC STRINGS

#### 📝 Problem Statement:
*Given two strings `s` and `t`, determine if they are isomorphic. Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.*

*   **Examples:**
    *   `s = "egg"`, `t = "add"` → Output: `true`
    *   `s = "foo"`, `t = "bar"` → Output: `false`

#### 🚨 How to Think 🎙️:
> *"Isomorphic characters check bacho! characters mapping strictly one-to-one honi chahiye. 
> 
> E.g. agar `'e' -> 'a'`, toh aur koi character `'a'` par map nahi ho sakta, aur na hi `'e'` kisi aur element par map ho sakta hai. 
> 
> SDE Pattern: Bi-directional HashMap mapping constraints. Hum s-to-t aur t-to-s dono directions ki mapping map records maintain karenge safety ke liye."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** We need to preserve character alignments. Single direction check is incomplete (foo → bar fails, but ab → aa passes single map).
*   **Hint 2 (Direction):** Build Map1: `sChar -> tChar`, Map2: `tChar -> sChar`. At mismatch, return `false`.
*   **Hint 3 (Pattern/DS):** Double Map Isomorphism tracking.

#### 💻 JavaScript Code:
```javascript
function isIsomorphic(s, t) {
    if (s.length !== t.length) return false;
    const sToT = new Map(); //
    const tToS = new Map(); //

    for (let i = 0; i < s.length; i++) {
        const charS = s[i];
        const charT = t[i];

        // Check s -> t mapping consistency
        if (sToT.has(charS) && sToT.get(charS) !== charT) return false;
        // Check t -> s mapping consistency
        if (tToS.has(charT) && tToS.get(charT) !== charS) return false;

        sToT.set(charS, charT); //
        tToS.set(charT, charS); //
    }
    return true;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(Σ)** where Σ is character set alphabet count.
*   **Is problem se kya seekhna hai?** One-to-One bijective relationships require mutual bi-directional Map verifications!

---

### PROBLEM 20: FIND ALL ANAGRAMS IN A STRING

#### 📝 Problem Statement:
*Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`.*

*   **Examples:**
    *   `s = "cbaebabacd"`, `p = "abc"` → Output: ``
*   **Constraints:**
    *   1 <= s.length, p.length <= 3 × 10^4

#### 🚨 How to Think 🎙️:
> *"Sawaal substring index range dhoondh raha hai jo `p` ke character counts se match kare. 
> 
> Brute force: Har index $i$ se $i + p.length$ window split karo aur sort karke compare karo ($O(N × P log P)$). Timeout aa jayega! 
> 
> Bottleneck: Redundant sorting. 
> 
> Let's make an observation: Anagram character verification can be managed by maintaining character frequencies arrays! Hum ek **Sliding Window of size P** maintain karenge. Jab window right-shift ho, toh enter element index value increment karke left boundary element value decrement kar do in constant $O(1)$! compare loops array parameters strictly constant frequency length (26 alphabets limits) match karenge bacho."*

#### 💡 Progressive Hints:
*   **Hint 1 (Observation):** Array of size 26 is enough to represent character count frequencies maps.
*   **Hint 2 (Direction):** Build `pCount` frequency array. Maintain running `sCount` frequency array for window size `p.length`.
*   **Hint 3 (Pattern/DS):** Sliding Window with flat array comparisons.

#### 💻 JavaScript Code:
```javascript
function findAnagrams(s, p) {
    const result = [];
    if (s.length < p.length) return result;

    const pCount = new Array(26).fill(0); //
    const sCount = new Array(26).fill(0); //

    const getIdx = (char) => char.charCodeAt(0) - 97; //

    // Initialize counts for first window size
    for (let i = 0; i < p.length; i++) {
        pCount[getIdx(p[i])]++;
        sCount[getIdx(s[i])]++;
    }

    const matches = (arr1, arr2) => {
        for (let i = 0; i < 26; i++) {
            if (arr1[i] !== arr2[i]) return false;
        }
        return true;
    };

    if (matches(pCount, sCount)) result.push(0);

    // Slide window
    for (let i = p.length; i < s.length; i++) {
        sCount[getIdx(s[i])]++; // Add new element to window
        sCount[getIdx(s[i - p.length])]--; // Remove element leaving window

        if (matches(pCount, sCount)) {
            result.push(i - p.length + 1); // Record matching start index
        }
    }

    return result;
}
```

*   **Complexity:** Time: **O(N)** (Since frequency compare takes constant 26 steps loops), Space: **O(1)** auxiliary.
*   **Is problem se kya seekhna hai?** Fixed sliding window with dynamic array boundary shift checks avoids redundant computations completely!

---

## MIXED UNSEEN SET 1 (PROBLEMS 21 - 25)

**Arey bacho! Ab is section mein koi pre-defined header ya pattern leakage nahi hoga. Sawaal ko dekho, constraints analyze karo aur khud se dhoondho sahi algorithm!**

---

### PROBLEM 21: 3SUM

#### 📝 Problem Statement:
*Given an integer array `nums`, return all the unique triplets `[nums[i], nums[j], nums[k]]` such that $nums[i] + nums[j] + nums[k] === 0$ and $i \ne j, i \ne k, j \ne k$.*

*   **Examples:**
    *   `nums = [-1, 0, 1, 2, -1, -4]` → Output: `[[-1, -1, 2], [-1, 0, 1]]`
*   **Constraints:**
    *   3 <= nums.length <= 3000

#### 🚨 How to Think 🎙️:
> *"Humein unique triplets dhoondhne hain jinka sum zero ho. Constraint mein array size 3000 hai, iska matlab we cannot afford cubic $O(N^3)$ brute force nested loops. 
> 
> Optimal target maximum $O(N^2)$ ho sakta hai. 
> 
> Sawaal mein unique duplicate configurations prevent karni hain bacho. Sorting is highly useful here because elements sorted ho toh hum unique boundaries skip kar sakte hain duplicate loops mein! 
> 
> Let's make an observation: Agar hum array ko sort kar dein, aur ek outer variable `i` fixed karke bache huye range par **Two Pointers (left, right)** lagayein, toh hum $O(N^2)$ complexity mein linear zero pairs map dhoondh sakte hain!"*

#### 💻 JavaScript Code:
```javascript
function threeSum(nums) {
    const result = [];
    nums.sort((a, b) => a - b); // Step 1: Sort the array

    for (let i = 0; i < nums.length - 2; i++) {
        // Skip duplicate values for outer pointer
        if (i > 0 && nums[i] === nums[i - 1]) continue;

        let left = i + 1;
        let right = nums.length - 1;

        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                
                // Skip duplicates on left and right
                while (left < right && nums[left] === nums[left + 1]) left++;
                while (left < right && nums[right] === nums[right - 1]) right--;
                
                left++;
                right--;
            } else if (sum < 0) {
                left++; // sum too small, move left pointer rightwards
            } else {
                right--; // sum too large, move right pointer leftwards
            }
        }
    }
    return result;
}
```

*   **Complexity:** Time: **O(N^2)**, Space: **O(1)** auxiliary (ignoring sort recursion stack).
*   **Is problem se kya seekhna hai?** Fix a pivot and reduce the remaining problem to the standard Two-Pointer target search!

---

### PROBLEM 22: SUBARRAY SUMS DIVISIBLE BY K

#### 📝 Problem Statement:
*Given an integer array `nums` and an integer `k`, return the number of non-empty subarrays that have a sum divisible by `k`.*

*   **Examples:**
    *   `nums = [4, 5, 0, -2, -3, 1]`, `k = 5` → Output: `7` (Subarrays: ``, ``, `[5, 0, -2, -3]`, ``, `[0, -2, -3]`, `[-2, -3]`, `[4, 5, 0, -2, -3, 1]`)
*   **Constraints:**
    *   1 <= nums.length <= 3 × 10^4

#### 🚨 How to Think 🎙️:
> *"Subarray sum divisible by $K$! Negative integers allow hain. Brute force will timeout. 
> 
> Let's analyze modular arithmetic bacho:
> Maan lo humare paas do prefix sums hain, aur unke remainder modulo $K$ identical hain:
> $$Prefix_1 % K === Prefix_2 % K => (Prefix_2 - Prefix_1) % K === 0$$
> Iska matlab, un dono prefixes ke beech ka subarray sum strictly divisible by $K$ hoga! 
> 
> SDE Pattern: Hashing remainder counts. Hum running cumulative remainder frequencies map maintain karenge bacho. 
> JavaScript Negative Modulo adjustment rule:* `rem = ((sum % k) + k) % k` to handle safe positive integers.

#### 💻 JavaScript Code:
```javascript
function subarraysDivByK(nums, k) {
    const remainderFreq = new Map();
    remainderFreq.set(0, 1); // Base remainder 0 seen once

    let currSum = 0;
    let count = 0;

    for (let num of nums) {
        currSum += num; // Calculate running prefix sum
        let rem = currSum % k;
        
        // Handle JS negative remainder adjustment
        if (rem < 0) rem += k;

        if (remainderFreq.has(rem)) {
            count += remainderFreq.get(rem); // Add occurrences to output
        }

        remainderFreq.set(rem, (remainderFreq.get(rem) || 0) + 1); // Update frequency map
    }

    return count;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(K)** auxiliary capacity to store remainders frequency.
*   **Is problem se kya seekhna hai?** Remainders congruence property converts dynamic division intervals queries to a simple linear Hashing loop!

---

### PROBLEM 23: INTEGER TO ROMAN

#### 📝 Problem Statement:
*Convert a given integer `num` to a Roman numeral representation.*

*   **Examples:**
    *   `num = 374` → Output: `"CCCLXXIV"`
*   **Constraints:**
    *   1 <= num <= 3999

#### 🚨 How to Think 🎙️:
> *"Sawaal Roman representations compile karne ke liye bol raha hai bacho. 
> 
> Brute-force decision chains bohot complicated ho jayenge. 
> 
> Let's make an observation: Greedy matching performs exceptionally fast here! Hum Roman scale coordinates descending order sorted lists compile karenge, aur highest matching baseline ko dynamic subtract karte chalenge jab tak value match na ho!"*

#### 💻 JavaScript Code:
```javascript
function intToRoman(num) {
    // Greedy base lists in descending order
    const romanMappings = [
        { val: 1000, symbol: "M" },
        { val: 900, symbol: "CM" },
        { val: 500, symbol: "D" },
        { val: 400, symbol: "CD" },
        { val: 100, symbol: "C" },
        { val: 90, symbol: "XC" },
        { val: 50, symbol: "L" },
        { val: 40, symbol: "XL" },
        { val: 10, symbol: "X" },
        { val: 9, symbol: "IX" },
        { val: 5, symbol: "V" },
        { val: 4, symbol: "IV" },
        { val: 1, symbol: "I" }
    ];

    let result = "";
    for (let mapping of romanMappings) {
        // Greedily consume the largest possible roman base value
        while (num >= mapping.val) {
            result += mapping.symbol; // Append Roman symbol
            num -= mapping.val; // Subtract consumed amount
        }
    }
    return result;
}
```

*   **Complexity:** Time: **O(1)** (Since value has fixed upper bound 3999), Space: **O(1)**.
*   **Is problem se kya seekhna hai?** Downwards greedy scale consuming solves structural translations problems optimally!

---

### PROBLEM 24: TOP K FREQUENT ELEMENTS

#### 📝 Problem Statement:
*Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.*

*   **Examples:**
    *   `nums =`, `k = 2` → Output: ``
*   **Constraints:**
    *   1 <= nums.length <= 10^5

#### 🚨 How to Think 🎙️:
> *"Top K frequent elements dhoondhne hain bacho. 
> 
> Step 1 strictly obvious hai: Elements frequency count karo Hash Map use karke ($O(N)$). 
> 
> Step 2: Sort candidates based on frequency ($O(N log N)$). 
> Can we do it in linear $O(N)$? 
> Yes! Maan lo frequency count max $N$ hai. Hum ek array of buckets bana sakte hain jahan index represents the **frequency count**. Is technique ko kehte hain **Bucket Sort**. Linear scale sweep easily runs in $O(N)$ time!"*

#### 💻 JavaScript Code:
```javascript
function topKFrequent(nums, k) {
    const freqMap = new Map(); //
    for (let num of nums) {
        freqMap.set(num, (freqMap.get(num) || 0) + 1); // Step 1: Count frequency
    }

    const n = nums.length;
    // Buckets index represents frequency
    const buckets = Array.from({ length: n + 1 }, () => []);

    for (let [num, freq] of freqMap.entries()) {
        buckets[freq].push(num); // Place elements in corresponding bucket
    }

    const result = [];
    // Traverse backwards from high frequency to low frequency
    for (let i = n; i >= 0 && result.length < k; i--) {
        if (buckets[i].length > 0) {
            result.push(...buckets[i]); //
        }
    }

    return result.slice(0, k); // Return top K
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)** auxiliary memory.
*   **Is problem se kya seekhna hai?** Frequency arrays (Bucket sort) can completely eliminate standard O(N log N) sorting bottlenecks!

---

### PROBLEM 25: PARTITION LABELS

#### 📝 Problem Statement:
*You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part. Return a list of integers representing the size of these parts.*

*   **Examples:**
    *   `s = "ababcbacadefegdehijhklij"` → Output: `` (Subsegments: `"ababcbaca"`, `"defegde"`, `"hijhklij"`)
*   **Constraints:**
    *   1 <= s.length <= 500

#### 🚨 How to Think 🎙️:
> *"Sawaal linear segment cuts define karne ke liye bol raha hai bacho. 
> 
> Let's analyze condition: Kisi character `C` ko start karne ke baad, hum tabhi partition cut laga sakte hain jab us segment ke saare characters ka absolute **last occurrence index** currently checked window boundaries ke andar finalize ho chuka ho! 
> 
> SDE Pattern: Greedy interval expansion. first pass chalakar har character ka last occurrence index Map/Array mein save karo. Second pass mein window boundary dynamically expand karo jab tak elements scan range matching end coordinate tak na pahunche bacho!"*

#### 💻 JavaScript Code:
```javascript
function partitionLabels(s) {
    const lastIndex = new Array(26).fill(0); //
    for (let i = 0; i < s.length; i++) {
        lastIndex[s.charCodeAt(i) - 97] = i; // Record last occurrence
    }

    const result = [];
    let start = 0;
    let end = 0;

    for (let i = 0; i < s.length; i++) {
        // Expand window end to the last occurrence of the current character
        end = Math.max(end, lastIndex[s.charCodeAt(i) - 97]); //

        // If we reach the boundary, record size and slice out!
        if (i === end) {
            result.push(end - start + 1);
            start = i + 1; // Move start to the next segment
        }
    }

    return result;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary (as only size 26 array used).
*   **Is problem se kya seekhna hai?** Tracking worst-case boundaries (last indices) helps safely partition contiguous streams greedily in linear passes!

---

## SECTION SUMMARY & SDE BLUEPRINTS 📊

Bacho, is summary block ko register par dhyan se update karo:

| Problem Patterns | Core Structural Philosophy | Candidate Bug Traps ⚠️ |
| :--- | :--- | :--- |
| **Two Pointers** | Converge/Diverge linearly to solve sorting bounds pairs. | Applying on unsorted dynamic arrays. |
| **Prefix Sums** | Subtracting cumulative prefixes yields O(1) range queries. | Floating point precision bugs, integer limits. |
| **Hashing Bucket** | Standardizing keys maps multi-bucket elements instantly. | Not handling string normalization correctly. |

---

## SDE MASTERCLASS PROGRESS TARGETS 🗺️

*   **Current Progress:** 25 / 150+ Problems completed!
*   **Concepts Covered:** Arrays operations, String reversals, converging pointers, Hashing frequency maps, Congruence prefix mods, Boundary expansions.

---

## SECTION 3: SLIDING WINDOW & TWO POINTERS (ADVANCED) (PROBLEMS 26 - 29)

---

### PROBLEM 26: SLIDING WINDOW MAXIMUM (LEETCODE 239)

#### 1. Problem Statement:
*You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Return the max sliding window.*

#### 2. Examples:
*   `nums =`, `k = 3` → Output: ``
    *   *Explanation:* 
        *   Window `` → Max: 3
        *   Window `` → Max: 3
        *   Window `` → Max: 5, and so on.

#### 3. Constraints:
*   1 <= nums.length <= 10^5
*   -10^4 <= nums[i] <= 10^4
*   1 <= k <= nums.length

#### 4. Think (Candidate Thinking Aloud) 🎙️:
> *"Sawaal ko dhyan se samajhte hain bacho. Ek array hai aur K size ki window right side shift ho rahi hai, humein har instance par window ka maximum element return karna hai. Constraint dekho: N = 10^5. Agar main har shift par window ke saare K elements ko linear scan karoon, toh complexity O(N × K) ho jayegi. Agar K bhi 10^5 ke close hua, toh code instantly crash (TLE) ho jayega! Humein O(N) solution chahiye.
>
> Bottleneck: Har window transition par pichle elements ka max nikalne ke liye poora scan dobara karna pad raha hai.
>
> Mujhe koi aisa structure chahiye jo elements ko monotonically sorted order mein rakh sake aur invalid elements (jo window se bahar ja chuke hain) unhe efficiently remove kar sake.
>
> Hum ek **Deque (Double Ended Queue)** use kar sakte hain! Deque mein hum indices store karenge aur use **strictly decreasing order** mein maintain karenge. Yaani deque ke front par humesha current window ka absolute maximum element ka index hoga!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Agar window mein koi naya bada element `X` aata hai, toh pichle chote elements kabhi bhi future windows mein maximum nahi ban sakte, so unhe discard kar do.
*   **Hint 2 (Direction):** Maintain indices in Deque. Front of Deque should always hold the index of the maximum element.
*   **Hint 3 (Pattern/DS):** Monotonic Deque / Sliding Window Maximum pattern.

#### 6. Brute Force:
Har window position `i` se lekar `i + k - 1` tak nested loop chala kar maximum find out karna. Time: O(N × K), Space: O(1).

#### 7. Bottleneck of Brute Force:
Same elements ka maximum status check har slide par recalculate hota hai.

#### 8. Optimal Observation:
Agar hum left pointer aur right pointer ka use karein, aur double-ended structure (Deque) maintain karein jahan:
1. Naya element `nums[i]` aane par, deque ke back se saare elements chote than `nums[i]` ko pop kar dein.
2. Deque ke front par khade index ko check karein, agar woh `i - k` se chota ya barabar hai, toh window se bahar ho chuka hai, use front se shift/pop kar dein.

#### 9. Selected Pattern & Data Structure:
*   **Pattern:** Monotonic Decreasing Deque.
*   **Data Structure:** Array simulating Deque (using `push`, `pop`, `shift` optimization).

#### 10. Optimal Approach:
Iterate right pointer from 0 to N-1. Clean smaller elements from back of Deque, add current index, remove out-of-bound indexes from front, and record front element to result array once i >= k - 1.

#### 11. JavaScript Code:
```javascript
function maxSlidingWindow(nums, k) {
    const result = [];
    const deque = []; // Stores indices

    for (let i = 0; i < nums.length; i++) {
        // Step 1: Remove out of bound indices from front of deque
        if (deque.length > 0 && deque < i - k + 1) {
            deque.shift(); // Remove oldest index
        }

        // Step 2: Maintain monotonic decreasing order (remove smaller elements from back)
        while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
            deque.pop(); // Discard smaller elements as they can't be max
        }

        // Step 3: Add current index to deque
        deque.push(i);

        // Step 4: Once window size k is reached, record the front element as max
        if (i >= k - 1) {
            result.push(nums[deque]); // Front always has maximum element
        }
    }

    return result;
}
```

#### 12. Line-by-Line Explanation:
1. `if (deque < i - k + 1) deque.shift();` → Agar deque ka sabse purana index current window range `[i-k+1, i]` se bahar hai, toh use remove kar do.
2. `while (...) deque.pop();` → Jab tak deque ke peeche wale elements current element se chote hain, unhe pop karo kyunki woh kabhi maximum nahi ban sakte.
3. `deque.push(i);` → Current index ko deque mein push karo.
4. `if (i >= k - 1)` → Jab window pehli baar size K hit kare, tabhi se result record karna shuru karo.

#### 13. Complete Dry Run:
Input: `nums =`, `k = 3`
*   `i = 0` (val `1`): Deque is empty. `deque =`.
*   `i = 1` (val `3`): `3 > nums (1)`. Pop `0`. `deque =`. Window not reached.
*   `i = 2` (val `-1`): `deque =`. `i >= k-1` (2 >= 2). `result.push(nums[deque])` → `result =`.
*   `i = 3` (val `-3`): `deque =`. `result.push(nums[deque])` → `result =`.
*   `i = 4` (val `5`): `5 > -3` (pop 3), `5 > -1` (pop 2), `5 > 3` (pop 1). `deque =`. `result.push(5)` → `result =`.

#### 14. Complexity:
*   **Time Complexity:** **O(N)** because each index is pushed once and popped at most once.
*   **Auxiliary Space Complexity:** **O(K)** worst-case to store indices in Deque.

#### 15. Edge Cases:
*   `k === 1` → Handled correctly, returns the array itself.

#### 16. Interview Follow-up:
*   **Interviewer:** *"Can you do this without using `shift()` since array shift is O(N) in JavaScript?"*
*   **Candidate:** *"Yes, sir! We can optimize this by keeping a `headPointer` variable inside Deque instead of physically shifting elements, or we can use a custom Doubly Linked List-based Deque to make all push/pop/shift operations strictly O(1)!"*

#### 17. Is problem se kya seekhna hai?
Monotonic deque structure range maximums queries ko O(1) amortized time mein solve karne ka best pattern hai!

---

### PROBLEM 27: MINIMUM WINDOW SUBSTRING (LEETCODE 76)

#### 1. Problem Statement:
*Given two strings `s` and `t` of lengths `m` and `n` respectively, return the minimum window substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.*

#### 2. Examples:
*   `s = "ADOBECODEBANC"`, `t = "ABC"` → Output: `"BANC"`

#### 3. Constraints:
*   1 <= s.length, t.length <= 10^5

#### 4. Think 🎙️:
> *"Sawaal kehta hai string `s` ke andar sabse choti contiguous substring dhoondhni hai jisme `t` ke saare characters ho. Constraint bahut large hai (10^5), isliye nested loops se all substrings generate karna (O(N^2)) block hai!
>
> Bottleneck: Unnecessary substrings scanning.
>
> Let's think: Jaise hi humein ek valid window milti hai jisme `t` ke saare characters hain, kya hum use chota (shrink) karne ki koshish kar sakte hain left boundary ko aage badha kar? Bilkul! 
>
> Yeh strictly ek **Sliding Window with Two Pointers (Expand & Shrink)** model hai. Hum frequency hash maps maintain karenge to validate matching constraints."*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Target frequencies match karne ke liye character frequency mapping bana lo.
*   **Hint 2 (Direction):** Expand `right` to find a valid window. Once valid, increment `left` to minimize the window until it becomes invalid.
*   **Hint 3 (Pattern/DS):** Sliding Window with character count frequency tracking.

#### 6. Brute Force:
Generate all substrings, check if each contains all chars of `t`. Complexity: O(N^3).

#### 7. Bottleneck of Brute Force:
Quadratic substring generation.

#### 8. Optimal Observation:
We can maintain a count of "required unique characters" (`requiredCount`).
*   When we expand the window and find a required character, we decrement its needed count. If its frequency meets the requirement, we increment our `formed` count.
*   Once `formed === requiredCount`, we record the minimum window and shrink from left.

#### 9. Selected Pattern & Data Structure:
*   **Pattern:** Sliding Window (Dynamic Expansion & Shrink).
*   **Data Structure:** JavaScript `Map` or frequency array.

#### 10. Optimal Approach:
Use sliding window with dual map tracking.

#### 11. JavaScript Code:
```javascript
function minWindow(s, t) {
    if (s.length < t.length) return "";

    const tMap = new Map();
    for (let char of t) {
        tMap.set(char, (tMap.get(char) || 0) + 1); // Target character counts
    }

    const windowMap = new Map();
    let left = 0;
    let requiredCount = tMap.size;
    let formed = 0;

    let minLen = Infinity;
    let minLeft = 0;

    for (let right = 0; right < s.length; right++) {
        const char = s[right];
        windowMap.set(char, (windowMap.get(char) || 0) + 1); // Expand window

        if (tMap.has(char) && windowMap.get(char) === tMap.get(char)) {
            formed++; // A character frequency constraint is met
        }

        // Try to shrink window from left once valid
        while (formed === requiredCount && left <= right) {
            const currentWindowLen = right - left + 1;
            if (currentWindowLen < minLen) {
                minLen = currentWindowLen;
                minLeft = left; // Record start index of best window
            }

            const leftChar = s[left];
            windowMap.set(leftChar, windowMap.get(leftChar) - 1);
            if (tMap.has(leftChar) && windowMap.get(leftChar) < tMap.get(leftChar)) {
                formed--; // Window became invalid
            }
            left++; // Shrink window
        }
    }

    return minLen === Infinity ? "" : s.substring(minLeft, minLeft + minLen); //
}
```

#### 12. Line-by-Line Explanation:
1. `tMap.set(...)` → Target characters ki desired count frequency Map mein populate karo.
2. `for (let right = 0; ...)` → Window right end dynamically expand karo.
3. `while (formed === requiredCount)` → Jab tak current window valid hai, shrink left boundary to find the minimum possible length.
4. `s.substring(minLeft, minLeft + minLen)` → Substring slice output return karo.

#### 13. Complete Dry Run:
Input: `s = "ADOBECODEBANC"`, `t = "ABC"`
*   `tMap = {A:1, B:1, C:1}`, `requiredCount = 3`.
*   Expand until `right = 5` (substring `"ADOBEC"`), `windowMap` contains A, B, C. `formed = 3`.
*   `minLen` becomes 6 (`"ADOBEC"`).
*   Shrink left: `left` moves past 'A'. `formed` becomes 2. Window invalid.
*   Next valid window at `right = 10` (`"BECODEBA"`), shrink from left to get `"CODEBA"`, then `"BANC"` at index 9 to 12.
*   **Returns:** `"BANC"`.

#### 14. Complexity:
*   **Time Complexity:** **O(N)** since pointers visit each index at most twice.
*   **Auxiliary Space Complexity:** **O(Σ)** where Σ is alphabet size for maps.

#### 15. Edge Cases:
*   `s` doesn't contain all characters of `t` → Returns `""`.

#### 16. Alternative Approach:
Instead of tracking all characters, filter `s` into a list of indices containing only characters present in `t`. This is highly optimized when `s` is huge and `t` is very small!

#### 17. Is problem se kya seekhna hai?
Expansion and shrinking pointers balance dynamically is the gold standard for continuous constraint optimizations!

---

## SECTION 4: BINARY SEARCH (ADVANCED BOUNDARIES) (PROBLEMS 28 - 29)

---

### PROBLEM 28: SEARCH IN ROTATED SORTED ARRAY (LEETCODE 33)

#### 1. Problem Statement:
*There is an integer array `nums` sorted in ascending order (with distinct values). Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index. Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`. Solve it in O(log N) time.*

#### 2. Examples:
*   `nums =`, `target = 0` → Output: `4`

#### 3. Constraints:
*   1 <= nums.length <= 5000
*   All values of `nums` are unique.

#### 4. Think 🎙️:
> *"Sawaal sorted array search ka hai par array pivot point par rotated hai. Time complexity target is O(log N). Linear search strictly forbidden.
>
> Bottleneck: Pure array standard binary sorted order mein nahi hai.
>
> SDE Observation: Agar hum rotated sorted array ko centre se divide karein (at index `mid`), toh **kam se kam ek half humesha strictly sorted hoga**!
>
> Jaise, `mid = 7` in ``. Left half `` strictly sorted hai.
> Hum check karenge ki sorted half kaunsa hai, aur kya target us sorted half ke range boundary ke andar lie karta hai!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Identify if left half `[start, mid]` is sorted or right half `[mid, end]` is sorted.
*   **Hint 2 (Direction):** Check if `nums[start] <= nums[mid]`. If yes, left is sorted. Else, right is sorted.
*   **Hint 3 (Pattern/DS):** Modified Binary Search with partition sorting check.

#### 6. Brute Force:
Linear search through array to find target. Complexity: O(N).

#### 7. Bottleneck of Brute Force:
Ignores rotational sorted properties.

#### 8. Optimal Observation:
*   If Left half is sorted: Check if `target >= nums[start] && target < nums[mid]`. If yes, search left (`end = mid - 1`); else search right (`start = mid + 1`).
*   If Right half is sorted: Check if `target > nums[mid] && target <= nums[end]`. If yes, search right (`start = mid + 1`); else search left (`end = mid - 1`).

#### 9. Selected Pattern & Data Structure:
*   **Pattern:** Binary Search on Rotated Partitions.
*   **Data Structure:** Array bounds pointers (`start`, `end`).

#### 10. Optimal Approach:
Standard binary loop, branch left/right based on range validations.

#### 11. JavaScript Code:
```javascript
function search(nums, target) {
    let start = 0;
    let end = nums.length - 1;

    while (start <= end) {
        const mid = Math.floor((start + end) / 2); //
        if (nums[mid] === target) return mid;

        // Check if Left half is strictly sorted
        if (nums[start] <= nums[mid]) {
            // Is target within left sorted boundaries?
            if (target >= nums[start] && target < nums[mid]) {
                end = mid - 1; // Search Left
            } else {
                start = mid + 1; // Search Right
            }
        } 
        // Right half must be sorted
        else {
            // Is target within right sorted boundaries?
            if (target > nums[mid] && target <= nums[end]) {
                start = mid + 1; // Search Right
            } else {
                end = mid - 1; // Search Left
            }
        }
    }

    return -1; // Target not found
}
```

#### 12. Line-by-Line Explanation:
1. `if (nums[start] <= nums[mid])` → Left segment checks to verify sorting status.
2. `target >= nums[start] && target < nums[mid]` → Target boundaries checks on the sorted partition.
3. `end = mid - 1` or `start = mid + 1` → Binary bounds half update safely.

#### 13. Complete Dry Run:
Input: `nums =`, `target = 0`
*   `start = 0` (val 4), `end = 6` (val 2). `mid = 3` (val 7).
*   `nums[mid] !== 0`. Left check: `nums (4) <= nums (7)` → Left is sorted.
*   Is target `0` inside `[4, 7)`? No! Move `start = mid + 1 = 4`.
*   Next run: `start = 4` (val 0), `end = 6` (val 2). `mid = 5` (val 1).
*   Is target `0` inside ``? Right check: `nums (0) <= nums (1)`. Yes, left sorted. Target matches `nums`. Returns `4`. Correct!

#### 14. Complexity:
*   **Time Complexity:** **O(log N)**.
*   **Auxiliary Space Complexity:** **O(1)**.

#### 15. Edge Cases:
*   Single element array → Handled correctly.

#### 16. Same pattern in other problems:
Search in Rotated Sorted Array II (duplicates allowed).

#### 17. Is problem se kya seekhna hai?
Even in rotated conditions, half of the array is guaranteed to remain sorted, allowing us to drop half of the search space at each iteration!

---

### PROBLEM 29: FIND MINIMUM IN ROTATED SORTED ARRAY

#### 1. Problem Statement:
*Given the sorted rotated array `nums` of unique elements, return the minimum element of this array. Solve it in O(log N) time.*

#### 2. Examples:
*   `nums =` → Output: `1`

#### 3. Constraints:
*   1 <= n <= 5000

#### 4. Think 🎙️:
> *"Rotated array mein minimum dhoondhna hai. O(log N) constraints force binary search.
>
> SDE Observation: Kisi bhi index `mid` par khade hokar, agar `nums[mid] > nums[end]` hai, toh iska matlab rotation point aur minimum element hamesha right half mein exist karenge!
>
> Agar `nums[mid] < nums[end]`, toh right half sorted hai aur minimum element `mid` ya usse pehle left half mein hi hoga bacho!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Compare `mid` value strictly with the `end` pointer value.
*   **Hint 2 (Direction):** If `nums[mid] > nums[end]`, search right half (`start = mid + 1`). Else, search left half including mid (`end = mid`).
*   **Hint 3 (Pattern/DS):** Modified Binary Search.

#### 6. JavaScript Code:
```javascript
function findMin(nums) {
    let start = 0;
    let end = nums.length - 1;

    while (start < end) {
        const mid = Math.floor((start + end) / 2); //

        if (nums[mid] > nums[end]) {
            start = mid + 1; // Minimum must be in the right un-sorted half
        } else {
            end = mid; // Minimum can be mid itself or to the left
        }
    }

    return nums[start];
}
```

*   **Complexity:** Time: **O(log N)**, Space: **O(1)**.
*   **Is problem se kya seekhna hai?** Modifying search bounds based on boundary constraints comparison is the key to customized Binary Search algorithms.

---

## SECTION 5: LINKED LISTS (POINTER DYNAMICS) (PROBLEMS 30 - 32)

---

### PROBLEM 30: REVERSE LINKED LIST (LEETCODE 206)

#### 1. Problem Statement:
*Given the head of a singly linked list, reverse the list, and return the reversed list.*

#### 2. Examples:
*   `head =` → Output: ``

#### 3. Constraints:
*   The number of nodes in the list is in the range ``.

#### 4. Think 🎙️:
> *"Linked list ko reverse karna hai. Extra memory use nahi karni, O(1) auxiliary space optimization chahiye.
>
> Bottleneck: Agar hum seedhe links change karenge, toh next node ka reference lose ho jayega!
>
> SDE Observation: Humein teen pointers maintain karne honge: `prev` (jo humesha pichle node ko track karega), `current` (jo current node par hoga), aur `next` (jo current change karne se pehle next node ka backup rakhega)!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Draw nodes on paper. At node `current`, we want to set `current.next = prev`.
*   **Hint 2 (Direction):** Before setting, store `nextNode = current.next`.
*   **Hint 3 (Pattern/DS):** Three-Pointer Iterative Reversal Pattern.

#### 6. JavaScript Code:
```javascript
function reverseList(head) {
    let prev = null;
    let current = head;

    while (current !== null) {
        const nextNode = current.next; // Step 1: Save next node reference
        current.next = prev;           // Step 2: Reverse the link
        prev = current;                // Step 3: Move prev forward
        current = nextNode;            // Step 4: Move current forward
    }

    return prev; // New head of reversed list
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.
*   **Is problem se kya seekhna hai?** References preserving is critical in Linked List manipulations. Use a temporary pointer for safe transition.

---

### PROBLEM 31: LINKED LIST CYCLE II (LEETCODE 142)

#### 1. Problem Statement:
*Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.*

#### 2. Examples:
*   `head = [3 -> 2 -> 0 -> -4 -> 2 (cycle back)]` → Output: `node with value 2`

#### 3. Constraints:
*   Number of nodes <= 10^4.

#### 4. Think 🎙️:
> *"Sawaal cycle detect karke starting point return karne ka hai.
>
> SDE Pattern: **Floyd's Tortoise and Hare (Fast & Slow Pointers)**.
>
> Mathematical Proof: Maan lo starting point se cycle start tak ka distance D hai, aur meeting point tak cycle ka distance M hai. Slow pointer covers D + M. Fast covers 2 × (D + M). 
>
> Mathematically, yeh tabhi possible hai jab hum slow ko wapas `head` par set karein aur dono slow aur fast ko same speed (1 step) se aage badhayein! Woh strictly **cycle starting node** par hi milenge bacho!"*

#### 5. JavaScript Code:
```javascript
function detectCycle(head) {
    if (!head || !head.next) return null;

    let slow = head;
    let fast = head;

    // Step 1: Detect cycle
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;

        if (slow === fast) {
            // Cycle detected!
            let entry = head;
            // Step 2: Find starting node of cycle
            while (entry !== slow) {
                entry = entry.next;
                slow = slow.next;
            }
            return entry; // Cycle origin node
        }
    }

    return null; // No cycle
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.
*   **Is problem se kya seekhna hai?** Tortoise and Hare pointers meet at a mathematically bounded offset, which solves cyclic tracking in constant space!

---

### PROBLEM 32: REMOVE NTH NODE FROM END OF LIST

#### 📝 Problem Statement:
*Given the head of a linked list, remove the n-th node from the end of the list and return its head.*

#### 💻 JavaScript Code:
```javascript
function removeNthFromEnd(head, n) {
    const dummy = new ListNode(0); //
    dummy.next = head;
    let fast = dummy;
    let slow = dummy;

    // Step 1: Advance fast pointer by n + 1 steps
    for (let i = 0; i <= n; i++) {
        fast = fast.next;
    }

    // Step 2: Move both together until fast reaches end
    while (fast !== null) {
        fast = fast.next;
        slow = slow.next; //
    }

    // Step 3: Remove target node
    slow.next = slow.next.next;

    return dummy.next;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.
*   **Is problem se kya seekhna hai?** Dummy node creation prevents edge cases like deleting the root/head node of the list!

---

## SECTION 6: STACK & QUEUE (LIFO / FIFO BOUNDARIES) (PROBLEMS 33 - 35)

---

### PROBLEM 33: VALID PARENTHESES (LEETCODE 20)

#### 1. Problem Statement:
*Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.*

#### 2. Examples:
*   `s = "()[]{}"` → Output: `true`

#### 3. Constraints:
*   1 <= s.length <= 10^4

#### 4. Think 🎙️:
> *"Valid brackets syntax matching! LIFO sequence follow karta hai (Last Opened Bracket must be First Closed).
>
> SDE Pattern: **Stack LIFO**. Match elements using Hashing."*

#### 5. JavaScript Code:
```javascript
function isValid(s) {
    if (s.length % 2 !== 0) return false; // Odd length can never be balanced

    const stack = []; //
    const mapping = {
        ')': '(',
        '}': '{',
        ']': '['
    };

    for (let char of s) {
        if (char === '(' || char === '{' || char === '[') {
            stack.push(char); // Push opening brackets
        } else {
            const topElement = stack.pop(); // Pop top
            if (topElement !== mapping[char]) {
                return false; // Mismatched brackets!
            }
        }
    }

    return stack.length === 0; // Balanced if stack is empty
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)**.

---

### PROBLEM 34: NEXT GREATER ELEMENT I (LEETCODE 496)

#### 1. Problem Statement:
*The next greater element of some element `x` in an array is the first greater element that is to the right of `x` in the same array. Find all next greater elements for subset array.*

#### 2. Examples:
*   `nums1 =`, `nums2 =` → Output: ``

#### 3. Constraints:
*   1 <= nums1.length <= nums2.length <= 1000

#### 4. Think 🎙️:
> *"Maan lo hum `nums2` mein khade hain aur right side par first greater element dhoondhna hai.
>
> SDE Pattern: **Monotonic Stack (decreasing order)**. 
>
> Hum `nums2` par left-to-right scan karenge aur stack mein elements maintain karenge. Jaise hi koi naya bada element milta hai, hum stack ke elements ko pop karke unki next greater mapping record karte chalenge!"*

#### 5. JavaScript Code:
```javascript
function nextGreaterElement(nums1, nums2) {
    const map = new Map();
    const stack = []; // Monotonic decreasing stack

    for (let num of nums2) {
        // While current number is greater than stack's top
        while (stack.length > 0 && num > stack[stack.length - 1]) {
            map.set(stack.pop(), num); // Resolve next greater relationship
        }
        stack.push(num);
    }

    // Remaining elements in stack have no next greater element
    while (stack.length > 0) {
        map.set(stack.pop(), -1);
    }

    return nums1.map(num => map.get(num)); // Map query subset
}
```

*   **Complexity:** Time: **O(N + M)**, Space: **O(N)**.
*   **Is problem se kya seekhna hai?** Monotonic stacks resolve outstanding rightward elements queries efficiently in linear time!

---

### PROBLEM 35: IMPLEMENT QUEUE USING STACKS (LEETCODE 232)

#### 1. Problem Statement:
*Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).*

#### 2. Think 🎙️:
> *"Stacks standard LIFO use karte hain, jabki Queue FIFO protocol par chalti hai. Two stacks `inStack` aur `outStack` ko use karke hum elements ordering ko double-reverse (LIFO + LIFO = FIFO) karke balance kar sakte hain!"*

#### 3. JavaScript Code (Amortized O(1)):
```javascript
class MyQueue {
    constructor() {
        this.inStack = [];
        this.outStack = [];
    }

    push(x) {
        this.inStack.push(x); // Standard push
    }

    pop() {
        this.peek(); // Ensure outStack is populated
        return this.outStack.pop();
    }

    peek() {
        // Transfer all from inStack to outStack only if outStack is empty
        if (this.outStack.length === 0) {
            while (this.inStack.length > 0) {
                this.outStack.push(this.inStack.pop()); //
            }
        }
        return this.outStack[this.outStack.length - 1]; // Peak top
    }

    empty() {
        return this.inStack.length === 0 && this.outStack.length === 0;
    }
}
```

*   **Complexity:** Time: **O(1) amortized** for pop and peek. Space: **O(N)**.
*   **Is problem se kya seekhna hai?** Lazy migration (transferring elements only when `outStack` is empty) yields perfect amortized constant time execution!

---

## MIXED UNSEEN SET 2 (PROBLEMS 36 - 40)

**Arey bacho! Ab is section mein koi pre-defined header ya pattern leakage nahi hoga. Sawaal ko dhyan se note karo aur decode karo!**

---

### PROBLEM 36: SUBARRAY PRODUCT LESS THAN K

#### 1. Problem Statement:
*Given an array of integers `nums` and an integer `k`, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than `k`.*

*   **Examples:**
    *   `nums =`, `k = 100` → Output: `8`

#### 2. Think 🎙️:
> *"Contiguous subarray product strictly less than K. Elements strictly positive hain. 
> 
> SDE Pattern: **Sliding Window**.
>
> Kyunki product strictly increasing hai positive values ke saath, right pointer move karne se product badhega, aur left boundary shrink karne se product ghat-ta hai. At any valid window, the number of new valid subarrays ending at `right` is strictly `right - left + 1`!"*

#### 3. JavaScript Code:
```javascript
function numSubarrayProductLessThanK(nums, k) {
    if (k <= 1) return 0; // Since elements are positive integers

    let left = 0;
    let currentProduct = 1;
    let count = 0;

    for (let right = 0; right < nums.length; right++) {
        currentProduct *= nums[right]; // Expand window

        // Shrink window if product exceeds target k
        while (currentProduct >= k && left <= right) {
            currentProduct /= nums[left];
            left++; // Shrink
        }

        // Count valid subarrays ending at 'right'
        count += (right - left + 1);
    }

    return count;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.

---

### PROBLEM 37: LONGEST SUBSTRING WITHOUT REPEATING CHARACTERS (LEETCODE 3)

#### 1. Problem Statement:
*Given a string `s`, find the length of the longest substring without repeating characters.*

#### 2. Think 🎙️:
> *"Longest contiguous substring with unique characters. 
>
> SDE Pattern: **Sliding Window with Hashing**.
>
> Map/Set maintain karke window expand karo. Agar duplicate character 'C' mile, toh left boundary ko `map.get(C) + 1` par direct index shift/shrink kar do!"*

#### 3. JavaScript Code:
```javascript
function lengthOfLongestSubstring(s) {
    const map = new Map(); // Key: char, Value: index
    let left = 0;
    let maxLen = 0;

    for (let right = 0; right < s.length; right++) {
        const char = s[right];

        if (map.has(char)) {
            // Shift left past the duplicate's last recorded position
            left = Math.max(left, map.get(char) + 1);
        }

        map.set(char, right); // Record current index
        maxLen = Math.max(maxLen, right - left + 1); // Record maximum
    }

    return maxLen;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(min(N, Σ))**.

---

### PROBLEM 38: ASTEROID COLLISION (LEETCODE 735)

#### 1. Problem Statement:
*We are given an array `asteroids` of integers representing asteroids in a row. For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Find the state of the asteroids after all collisions.*

*   **Examples:**
    *   `asteroids =` → Output: ``

#### 2. Think 🎙️:
> *"Asteroids order sequence check: positive asteroids move right, negative move left. Collision tabhi hoga jab right-moving element (`+`) ke baad left-moving element (`-`) aaye. LIFO order collision resolve karne ke liye **Stack** perfect structure hai!"*

#### 3. JavaScript Code:
```javascript
function asteroidCollision(asteroids) {
    const stack = []; //

    for (let ast of asteroids) {
        let alive = true;

        // Collision conditions: top of stack moving right (>0) and current moving left (<0)
        while (stack.length > 0 && stack[stack.length - 1] > 0 && ast < 0) {
            const top = stack[stack.length - 1];
            if (Math.abs(top) < Math.abs(ast)) {
                stack.pop(); // Top destroyed, current remains alive
                continue;
            } else if (Math.abs(top) === Math.abs(ast)) {
                stack.pop(); // Both destroyed
            }
            alive = false; // Current asteroid is destroyed
            break;
        }

        if (alive) stack.push(ast);
    }

    return stack;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)** worst-case.

---

### PROBLEM 39: SWAP NODES IN PAIRS (LEETCODE 24)

#### 1. Problem Statement:
*Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed).*

#### 2. Think 🎙️:
> *"Nodes pairs swap in-place. Links changes transitions manage karne ke liye dummy node setup aur three auxiliary reference links adjustments are mandatory!"*

#### 3. JavaScript Code:
```javascript
function swapPairs(head) {
    const dummy = new ListNode(0); //
    dummy.next = head;
    let current = dummy;

    while (current.next !== null && current.next.next !== null) {
        const first = current.next;
        const second = current.next.next;

        // Links swapping transitions
        first.next = second.next;
        second.next = first;
        current.next = second;

        current = first; // Jump forward two nodes
    }

    return dummy.next;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.

---

### PROBLEM 40: SIMULATE QUEUE CARDS (FIFO TRACKING)

#### 1. Problem Statement:
*You are given an integer card deck array. Play a cyclic card game: 
1. Remove topmost card from deck and put to result list.
2. If more cards remain, move the next topmost card to the very bottom of the deck.
Return the sequence of processed cards.*

#### 2. Think 🎙️:
> *"First node elements extract karke deck ke end par push cyclic shifting FIFO characteristics represent karti hai. **Queue** using standard JS shift/push operations handles this cleanly!"*

#### 3. JavaScript Code:
```javascript
function simulateCards(deck) {
    const queue = [...deck]; //
    const result = [];

    while (queue.length > 0) {
        result.push(queue.shift()); // Remove first card
        if (queue.length > 0) {
            queue.push(queue.shift()); // Shift next card to bottom of deck
        }
    }

    return result;
}
```

*   **Complexity:** Time: **O(N^2)** because of array `shift()` operations. (Can be optimized to **O(N)** using a Doubly Linked List Queue!)

---

## SECTION SUMMARY & SDE BLUEPRINTS 📊

Bacho, is summary block ko dhyan se register par note karo:

| SDE Patterns | Core Dynamic Philosophy | Candidate Bug Traps ⚠️ |
| :--- | :--- | :--- |
| **Monotonic Stack** | Keep running indices strictly sorted to resolve left/right lookup queries. | Forgetting to pop smaller values, index limits. |
| **Floyd Cycle Detection** | Fast/slow pointers overlap offsets cyclic graphs. | Missing null pointer fast step validation. |
| **Sliding Window (Shrink)** | Minimize valid boundaries incrementally in O(1) space. | Window invalidation conditions tracking. |

---

## SDE MASTERCLASS PROGRESS TARGETS 🗺️

*   **Current Progress:** 40 / 150+ Problems completed!
*   **Concepts Covered:** Sliding Window range counts, Monotonic queues/deque, search partition boundary checks, linked list cycle pointers, balanced bracket states.

---

## SECTION 7: BINARY TREES & BST (PROBLEMS 41 - 52)

Binary Trees mein data elements linear nahi, balki hierarchical structure mein save hote hain. Jab bhi tree ka question mile, hamesha yaad rakho: **90% Tree problems Recursion (DFS) ya Queue-based level order (BFS) se solve hote hain**!

---

### PROBLEM 41: MAXIMUM DEPTH OF BINARY TREE (LEETCODE 104)

#### 1. Problem Statement:
*Given the root of a binary tree, return its maximum depth. A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.*

#### 2. Examples:
*   `root = [3, 9, 20, null, null, 15, 7]` → Output: `3`

#### 3. Constraints:
*   The number of nodes in the tree is in the range [0, 10^4].
*   -100 <= Node.val <= 100

#### 4. Think (Candidate Thinking Aloud) 🎙️:
> *"Maximum depth nikalni hai bacho. Depth kya hoti hai? Root se lekar sabse door wale leaf node tak ka distance.
>
> **Brute force naturally kaise sochein?** Hum pooray tree ke saare paths recursively traverse karke unki lengths ko list mein store karein aur max nikalein, par yeh bohot redundant aur extra space-consuming hoga.
>
> **Optimal Observation:** Agar main kisi node `X` par khade hokar uske left child aur right child se unki respectively maximum depths maang loon, toh current node `X` ki maximum depth kya hogi?
> \\[ maxDepth(X) = 1 + max(maxDepth(X.left), maxDepth(X.right)) \\]
> Yeh toh perfect subproblem structure hai! Hum recursively bottom-up dhang se answer compile kar sakte hain."*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Base case kya hoga? Agar `root === null` hai, toh depth strictly `0` hogi.
*   **Hint 2 (Direction):** Find height of left subtree recursively, then height of right subtree, and take the maximum.
*   **Hint 3 (Pattern/DS):** Postorder DFS Tree Traversal.

#### 6. Brute Force:
Generate all paths from root to leaves, find maximum path length. Complexity: O(N log N) for balanced tree, O(N^2) for skewed tree.

#### 7. Bottleneck:
Scanning paths individually instead of calculating height bottom-up.

#### 8. Optimal Observation:
The depth of any node depends purely on the maximum of its subtrees' depths plus one.

#### 9. Selected Pattern & Data Structure:
*   **Pattern:** Bottom-Up DFS Recursion.
*   **Data Structure:** Recursive Call Stack.

#### 10. Optimal Approach:
Base case: `if (root === null) return 0`. Otherwise, return `1 + Math.max(maxDepth(root.left), maxDepth(root.right))`.

#### 11. JavaScript Code:
```javascript
function maxDepth(root) {
    if (root === null) return 0; // Base Case: empty tree has depth 0
    
    const leftHeight = maxDepth(root.left);   // Recursively fetch left subtree height
    const rightHeight = maxDepth(root.right); // Recursively fetch right subtree height
    
    return 1 + Math.max(leftHeight, rightHeight); // Add 1 for the current node
}
```

#### 12. Line-by-Line Explanation:
1. `if (root === null) return 0;` → Empty tree contains no nodes, hence depth is 0.
2. `maxDepth(root.left)` → Left child ko pivot banakar uski maximum subtree height calculate ki.
3. `1 + Math.max(leftHeight, rightHeight)` → Left aur right heights mein se max ko select karke current node ko consider karte huye `1` add kiya.

#### 13. Complete Dry Run:
Input: `root = [3, 9, 20, null, null, 15, 7]`
*   Call `maxDepth(3)`:
    *   Call `maxDepth(9)`:
        *   Left child `null` → `0`. Right child `null` → `0`.
        *   `maxDepth(9)` returns `1 + max(0, 0) = 1`.
    *   Call `maxDepth(20)`:
        *   `maxDepth(15)` returns `1`.
        *   `maxDepth(7)` returns `1`.
        *   `maxDepth(20)` returns `1 + max(1, 1) = 2`.
    *   `maxDepth(3)` returns `1 + max(1, 2) = 3`. Correct!

#### 14. Time & Space Complexity:
*   **Time Complexity:** **O(N)** because we visit every node exactly once.
*   **Auxiliary Space Complexity:** **O(H)** where H is the height of the tree, representing the recursive call stack space. In the worst case (skewed tree), this is O(N).

#### 15. Edge Cases:
*   Empty tree `root = null` → Handled cleanly, returns `0`.

#### 16. Interview Follow-up:
*   **Interviewer:** *"Can you solve this iteratively using BFS?"*
*   **Candidate:** *"Yes, sir! We can perform a Level Order Traversal using a Queue. We increment our depth counter at each level. This takes O(N) time and O(N) space for the queue!"*

#### 17. Same idea in other problems:
Minimum Depth of Binary Tree, Diameter of Binary Tree.

---

### PROBLEM 42: INVERT BINARY TREE (LEETCODE 226)

#### 1. Problem Statement:
*Given the root of a binary tree, invert the tree, and return its root. (Mirror image the tree).*

#### 2. Examples:
*   `root =` → Output: ``

#### 3. Constraints:
*   The number of nodes in the tree is in the range .

#### 4. Think 🎙️:
> *"Tree invert karna hai bacho. Mirror image banana hai.
>
> **Observation:** Agar hum tree ko dhyan se dekhein, toh har node ke left aur right child aapas mein swap ho rahe hain.
>
> Aur yeh swapping har single subtree par recursively perform ho rahi hai.
> Iska matlab, agar main current node par khada hokar uske left aur right pointers ko swap kar doon, aur fir yahi kaam recursively uske left aur right subtrees par repeat kar doon, toh poora tree mirror ho jayega!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Swapping references in JS is done via a temporary variable.
*   **Hint 2 (Direction):** Invert left subtree, invert right subtree, and then swap the left and right children of the root.
*   **Hint 3 (Pattern/DS):** Postorder DFS Recursion.

#### 6. JavaScript Code:
```javascript
function invertTree(root) {
    if (root === null) return null; // Base Case

    // Step 1: Recursively invert the subtrees
    const leftInverted = invertTree(root.left);
    const rightInverted = invertTree(root.right);

    // Step 2: Swap left and right child pointers
    root.left = rightInverted;
    root.right = leftInverted;

    return root;
}
```

#### 7. Complexity:
*   **Time Complexity:** **O(N)** since we process every node.
*   **Auxiliary Space Complexity:** **O(H)** for recursive call stack.

---

### PROBLEM 43: SAME TREE (LEETCODE 100)

#### 1. Problem Statement:
*Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not. Two binary trees are considered the same if they are structurally identical, and the nodes have the same values.*

#### 2. Think 🎙️:
> *"Do trees ko identical check karna hai bacho.
>
> Do trees kab same honge? Jab unki current nodes ki values identical hon, aur unke left subtrees bhi identical hon, aur unke right subtrees bhi identical hon!
>
> **Base Cases:**
> 1. Agar dono nodes `null` hain → identical (`true`).
> 2. Agar ek `null` hai aur dusra nahi, ya unki values different hain → different (`false`)."*

#### 3. JavaScript Code:
```javascript
function isSameTree(p, q) {
    // If both nodes are null, trees are structurally same
    if (p === null && q === null) return true;
    
    // If only one of them is null, they are structurally different
    if (p === null || q === null) return false;
    
    // Values must be equal, and left subtrees & right subtrees must match
    return (p.val === q.val) && 
           isSameTree(p.left, q.left) && 
           isSameTree(p.right, q.right);
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(H)**.
*   **What to notice next time:** Structural identical checks require recursive validation of all partitions simultaneously.

---

### PROBLEM 44: SUBTREE OF ANOTHER TREE (LEETCODE 572)

#### 1. Problem Statement:
*Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot`, and `false` otherwise.*

#### 2. Think 🎙️:
> *"Humein check karna hai ki kya `subRoot` tree, `root` ka koi hissa hai ya nahi.
>
> **Observation:** Kisi node `root` par, `subRoot` subtree kab banega?
> 1. Ya toh `root` aur `subRoot` khud identical trees hain! (We can use `isSameTree` helper).
> 2. Ya fir `subRoot`, `root.left` ka subtree hai.
> 3. Ya fir `subRoot`, `root.right` ka subtree hai."*

#### 3. JavaScript Code:
```javascript
function isSubtree(root, subRoot) {
    if (root === null) return false; // Main tree is empty, cannot contain subtree
    
    // Helper function from Problem 43
    if (isSameTree(root, subRoot)) return true;
    
    // Recursively check left and right subtrees
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}
```

*   **Complexity:** Time: **O(N × M)** where N is nodes in `root` and M is nodes in `subRoot` in the worst case, Space: **O(H_{root})**.

---

### PROBLEM 45: BINARY TREE LEVEL ORDER TRAVERSAL (LEETCODE 102)

#### 1. Problem Statement:
*Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).*

#### 2. Examples:
*   `root = [3, 9, 20, null, null, 15, 7]` → Output: `[,,]`

#### 3. Think 🎙️:
> *"Level-by-level traverse karna hai bacho. Jab bhi tree ko wide traverse karna ho, hamara go-to weapon hoga **BFS (Breadth-First Search) using a Queue**!
>
> **The Challenge:** Humein output mein har level ke elements ko separate arrays/sub-lists mein group karna hai.
>
> **Observation:** Jab hum kisi level ki processing shuru karte hain, toh queue ke andar strictly usi level ke saare nodes present hote hain bacho! Agar hum loop chalane se pehle current `queue.length` ko lock kar dein, toh hum exact number of elements ko extract karke ek level array mein push kar sakte hain!"*

#### 4. JavaScript Code:
```javascript
function levelOrder(root) {
    const result = [];
    if (root === null) return result; // Edge Case

    const queue = [root]; // Queue initialized with root

    while (queue.length > 0) {
        const levelSize = queue.length; // Lock level size
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift(); // Dequeue
            currentLevel.push(currentNode.val);

            // Enqueue left and right children for the next level
            if (currentNode.left !== null) queue.push(currentNode.left);
            if (currentNode.right !== null) queue.push(currentNode.right);
        }

        result.push(currentLevel); // Record current level
    }

    return result;
}
```

#### 5. Complexity:
*   **Time Complexity:** **O(N)** because we process each node exactly once.
*   **Auxiliary Space Complexity:** **O(N)** since the last level of a perfect binary tree can contain up to N/2 nodes in the queue.

---

### PROBLEM 46: LOWEST COMMON ANCESTOR OF A BST (LEETCODE 235)

#### 1. Problem Statement:
*Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.*

#### 2. Think 🎙️:
> *"LCA in a Binary Search Tree bacho! Remember, BST has a beautiful invariant property: **Left subtrees have smaller values, right subtrees have larger values**!
>
> Maan lo humein do target nodes `p` aur `q` diye hain.
> 1. Agar dono targets humare current `root` se bade hain (`p.val > root.val` and `q.val > root.val`), toh LCA strictly right subtree mein hoga bacho! We can pivot right (`root = root.right`).
> 2. Agar dono targets `root` se chote hain → LCA strictly left subtree mein hoga (`root = root.left`).
> 3. **The Split Point:** Agar ek target node current `root` se chota hai aur dusra bada, ya root khud ek target hai, toh current `root` hi humara absolute Lowest Common Ancestor (LCA) hai! Is split point ko return kar do bacho."*

#### 3. JavaScript Code:
```javascript
function lowestCommonAncestor(root, p, q) {
    let current = root;

    while (current !== null) {
        // Case 1: Both nodes are on the right side
        if (p.val > current.val && q.val > current.val) {
            current = current.right;
        } 
        // Case 2: Both nodes are on the left side
        else if (p.val < current.val && q.val < current.val) {
            current = current.left;
        } 
        // Case 3: Split point found! Current node is the LCA
        else {
            return current;
        }
    }
    return null;
}
```

*   **Complexity:** Time: **O(log N)** average for balanced tree, **O(H)** space/time in general.
*   **What to notice next time:** In BST, the LCA is the first node where the search paths for `p` and `q` split directions!

---

### PROBLEM 47: CONSTRUCT BINARY TREE FROM PREORDER AND INORDER TRAVERSAL (LEETCODE 105)

#### 1. Problem Statement:
*Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.*

#### 2. Think 🎙️:
> *"Preorder aur Inorder se tree rebuild karna hai bacho!
>
> **Key Observation:**
> 1. **Preorder traversal** hamesha Root node se start hota hai (`preorder` is the absolute root).
> 2. **Inorder traversal** mein, root node left and right subtrees ko perfectly divide karta hai!
>
> Agar hum preorder ke current element ko root banakar inorder mein uski position `mid` dhoondh lein, toh inorder ke `[0, mid - 1]` range wale elements strictly left subtree ke honge, aur `[mid + 1, end]` elements right subtree ke honge.
> Humein bas Map ka lookup use karke fast index matching karni hai bacho!"*

#### 3. JavaScript Code:
```javascript
function buildTree(preorder, inorder) {
    const inorderMap = new Map(); // For O(1) index lookup of root in inorder
    for (let i = 0; i < inorder.length; i++) {
        inorderMap.set(inorder[i], i);
    }

    let preorderIndex = 0;

    const helper = (left, right) => {
        if (left > right) return null; // Base case: subsegment is empty

        const rootVal = preorder[preorderIndex];
        const root = new TreeNode(rootVal);
        preorderIndex++;

        // Split inorder array at root's index
        const mid = inorderMap.get(rootVal);

        // Recursively build subtrees. Note: build left subtree FIRST in preorder
        root.left = helper(left, mid - 1);
        root.right = helper(mid + 1, right);

        return root;
    };

    return helper(0, inorder.length - 1);
}
```

*   **Complexity:** Time: **O(N)** average using Hash Map, Space: **O(N)**.

---

### PROBLEM 48: KTH SMALLEST ELEMENT IN A BST (LEETCODE 230)

#### 1. Problem Statement:
*Given the root of a binary search tree, and an integer `k`, return the `k`-th smallest value (1-indexed) of all the values of the nodes in the tree.*

#### 2. Think 🎙️:
> *"BST mein `k`-th smallest element nikalna hai.
>
> **The Blueprint:** BST property states that **Inorder Traversal hamesha values sorted order (ascending) mein yield karta hai**!
>
> Iska matlab, agar hum inorder traverse karein, aur elements count maintain rakhein, toh jaise hi count `k` hit karega, humein hamara `k`-th smallest element mil jayega bacho!"*

#### 3. JavaScript Code (Clean Iterative Stack version):
```javascript
function kthSmallest(root, k) {
    const stack = []; // Using stack to avoid recursion overhead
    let current = root;
    let count = 0;

    while (current !== null || stack.length > 0) {
        // Go as deep as possible to the left
        while (current !== null) {
            stack.push(current);
            current = current.left;
        }

        current = stack.pop(); // Process topmost node
        count++;

        if (count === k) {
            return current.val; // Found Kth smallest!
        }

        current = current.right; // Pivot to the right branch
    }

    return -1;
}
```

*   **Complexity:** Time: **O(H + K)** where H is height of tree, Space: **O(H)**.

---

### PROBLEM 49: VALIDATE BINARY SEARCH TREE (LEETCODE 98) (DEEP DIVE)

#### 1. Problem Statement:
*Given the root of a binary tree, determine if it is a valid binary search tree (BST).*

#### 2. JavaScript Code:
```javascript
function isValidBST(root) {
    return validate(root, -Infinity, Infinity);
}

function validate(node, min, max) {
    if (node === null) return true; // Empty node is valid

    // Current node value must strictly lie within (min, max) range
    if (node.val <= min || node.val >= max) {
        return false;
    }

    // Left subtrees must be smaller than parent, right must be larger
    return validate(node.left, min, node.val) && 
           validate(node.right, node.val, max);
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(H)**.

---

### PROBLEM 50: BINARY TREE RIGHT SIDE VIEW (LEETCODE 199)

#### 1. Problem Statement:
*Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.*

#### 2. Think 🎙️:
> *" खड़ा होकर right-side se tree dekhna hai bacho.
>
> **Observation:** Right side se har level ka strictly **rightmost element** hi visible hoga!
>
> Hum Level Order Traversal (BFS) chalakar har level ka last element result array mein store kar sakte hain, ya fir modified DFS (Right node ko pehle visit karte huye) kar sakte hain!"*

#### 3. JavaScript Code (BFS Level Order Method):
```javascript
function rightSideView(root) {
    const result = [];
    if (root === null) return result; //

    const queue = [root]; //

    while (queue.length > 0) {
        const levelSize = queue.length; //

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift(); // Dequeue

            // If it is the last element of the current level, record it
            if (i === levelSize - 1) {
                result.push(currentNode.val);
            }

            if (currentNode.left !== null) queue.push(currentNode.left);
            if (currentNode.right !== null) queue.push(currentNode.right);
        }
    }

    return result;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)**.

---

### PROBLEM 51: PATH SUM (LEETCODE 112)

#### 1. Problem Statement:
*Given the root of a binary tree and an integer `targetSum`, return `true` if the tree has a root-to-leaf path such that adding up all the values along the path equals `targetSum`.*

#### 2. JavaScript Code:
```javascript
function hasPathSum(root, targetSum) {
    if (root === null) return false; // Base Case: empty tree

    // If we reach a leaf node, check if the remaining sum matches root value
    if (root.left === null && root.right === null) {
        return targetSum === root.val;
    }

    const remainingSum = targetSum - root.val;
    
    // Check if either left subtree or right subtree contains a matching path
    return hasPathSum(root.left, remainingSum) || 
           hasPathSum(root.right, remainingSum);
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(H)**.

---

### PROBLEM 52: DIAMETER OF BINARY TREE (LEETCODE 543)

#### 1. Problem Statement:
*The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.*

#### 2. Think 🎙️:
> *"Diameter nikalna hai bacho. Longest path between two nodes.
>
> **Observation:** Longest path hamesha kisi node `X` ke left tree height aur right tree height ka summation hoga!
> \\[ Diameter at  X = height(X.left) + height(X.right) \\]
> Hum standard height function use karenge, par parallelly ek global tracker variable mein `leftHeight + rightHeight` ko compare karke absolute max diameter track karte chalenge bottom-up!"*

#### 3. JavaScript Code:
```javascript
function diameterOfBinaryTree(root) {
    let maxDiameter = 0;

    const dfsHeight = (node) => {
        if (node === null) return 0; //

        const leftHeight = dfsHeight(node.left);
        const rightHeight = dfsHeight(node.right);

        // Update global max diameter
        maxDiameter = Math.max(maxDiameter, leftHeight + rightHeight);

        // Return height of current node bottom-up
        return 1 + Math.max(leftHeight, rightHeight);
    };

    dfsHeight(root);
    return maxDiameter;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(H)**.

---

## SECTION 8: HEAPS / PRIORITY QUEUES (PROBLEMS 53 - 57)

Heap ek non-linear complete binary tree structure hota hai. **Max-Heap** mein root par hamesha sabse bada element hota hai, aur **Min-Heap** mein root par hamesha sabse chota element hota hai bacho!

---

### PROBLEM 53: KTH LARGEST ELEMENT IN AN ARRAY (LEETCODE 215)

#### 1. Problem Statement:
*Given an integer array `nums` and an integer `k`, return the `k`-th largest element in the array.*

#### 2. Think 🎙️:
> *"Array ka `k`-th largest element chahiye.
>
> **Brute force:** Array ko descend-order mein sort kar do (O(N log N)) aur index `k-1` return kar do.
>
> **Optimal Observation (The Heap Space optimization):** Agar hum pure array ko sort na karein, balki ek **Min-Heap of size K** maintain karein bacho!
>
> Kyunki Min-Heap ke top par hamesha sabse chota element hota hai, agar heap ka size `K` se bade hone par hum top element ko pop kar dein, toh loop khatam hone ke baad heap ke andar strictly **K largest elements** hi bachenge! Aur top par un K elements mein se sabse chota (yaani K-th largest) element hoga!"*

#### 3. JavaScript Code (Using Minimalist Ad-hoc MinHeap):
```javascript
class MinHeapKth {
    constructor() {
        this.heap = [];
    }
    push(val) {
        this.heap.push(val);
        this.bubbleUp();
    }
    pop() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        const min = this.heap;
        this.heap = this.heap.pop();
        this.sinkDown();
        return min;
    }
    bubbleUp() {
        let idx = this.heap.length - 1;
        while (idx > 0) {
            let parentIdx = Math.floor((idx - 1) / 2);
            if (this.heap[idx] < this.heap[parentIdx]) {
                [this.heap[idx], this.heap[parentIdx]] = [this.heap[parentIdx], this.heap[idx]];
                idx = parentIdx;
            } else break;
        }
    }
    sinkDown() {
        let idx = 0;
        const length = this.heap.length;
        while (2 * idx + 1 < length) {
            let leftChildIdx = 2 * idx + 1;
            let rightChildIdx = 2 * idx + 2;
            let smallest = leftChildIdx;
            if (rightChildIdx < length && this.heap[rightChildIdx] < this.heap[leftChildIdx]) {
                smallest = rightChildIdx;
            }
            if (this.heap[idx] > this.heap[smallest]) {
                [this.heap[idx], this.heap[smallest]] = [this.heap[smallest], this.heap[idx]];
                idx = smallest;
            } else break;
        }
    }
    size() {
        return this.heap.length;
    }
    peek() {
        return this.heap;
    }
}

function findKthLargest(nums, k) {
    const minHeap = new MinHeapKth(); //

    for (let num of nums) {
        minHeap.push(num);
        if (minHeap.size() > k) {
            minHeap.pop(); // Keep heap size exactly K
        }
    }

    return minHeap.peek(); // Root of the K-size heap is Kth largest!
}
```

*   **Complexity:** Time: **O(N log K)**, Space: **O(K)** heap memory.

---

### PROBLEM 54: FIND MEDIAN FROM DATA STREAM (LEETCODE 295) (HARD)

#### 1. Problem Statement:
*The median is the middle value in an ordered integer list. Design a data structure that supports adding numbers from a data stream and finding the median of the stream.*

#### 2. Think 🎙️:
> *"Dynamic median calculations from a stream bacho! Highly frequent system design/coding problem.
>
> **The Two Heaps Hack:**
> Hum data stream ko do equal halves mein split karke do priority heaps maintain karenge:
> 1. Left Half → Ek **Max-Heap** (`maxHeapLeft`) jo left half ke sabse bade element ko top par rakhega.
> 2. Right Half → Ek **Min-Heap** (`minHeapRight`) jo right half ke sabse chote element ko top par rakhega bacho.
>
> **Median extraction:**
> * Agar elements count odd hai → larger heap's top.
> * Agar elements count even hai → `(maxHeapLeft.peek() + minHeapRight.peek()) / 2`."*

#### 3. JavaScript Code (Using two heaps model):
```javascript
// Minimalist implementation assuming standard MinHeap/MaxHeap classes
class MedianFinder {
    constructor() {
        this.maxHeapLeft = new MaxHeapCustom(); // High values of lower half
        this.minHeapRight = new MinHeapCustom(); // Low values of upper half
    }

    addNum(num) {
        // Enqueue to Max-Heap first, then balance
        this.maxHeapLeft.push(num);
        this.minHeapRight.push(this.maxHeapLeft.pop());

        // Balance heap sizes so maxHeapLeft can have at most 1 more element than minHeapRight
        if (this.maxHeapLeft.size() < this.minHeapRight.size()) {
            this.maxHeapLeft.push(this.minHeapRight.pop());
        }
    }

    findMedian() {
        if (this.maxHeapLeft.size() > this.minHeapRight.size()) {
            return this.maxHeapLeft.peek();
        }
        return (this.maxHeapLeft.peek() + this.minHeapRight.peek()) / 2;
    }
}
```

*   **Complexity:** Time: `addNum` is **O(log N)**, `findMedian` is **O(1)**, Space: **O(N)**.

---

### PROBLEM 55: TOP K FREQUENT WORDS (LEETCODE 692)

#### 1. Problem Statement:
*Given an array of strings `words` and an integer `k`, return the `k` most frequent strings. Sorted lexicographically in case of frequency ties.*

#### 2. JavaScript Code:
```javascript
function topKFrequentWords(words, k) {
    const freqMap = new Map();
    for (let word of words) {
        freqMap.set(word, (freqMap.get(word) || 0) + 1);
    }

    // Convert frequency entries to unique sorted array
    const candidates = Array.from(freqMap.keys());
    candidates.sort((a, b) => {
        const freqA = freqMap.get(a);
        const freqB = freqMap.get(b);
        if (freqA !== freqB) {
            return freqB - freqA; // Higher frequency first
        }
        return a.localeCompare(b); // Lexicographical ascending on tie
    });

    return candidates.slice(0, k); //
}
```

*   **Complexity:** Time: **O(N log N)**, Space: **O(N)**.

---

### PROBLEM 56: K CLOSEST POINTS TO ORIGIN (LEETCODE 973)

#### 1. Problem Statement:
*Given an array of `points` where `points[i] = [x, y]` represents a point on the X-Y plane and an integer `k`, return the `k` closest points to the origin `(0,0)`.*

#### 2. JavaScript Code:
```javascript
function kClosest(points, k) {
    const getDist = (p) => p * p + p * p; // Distance formula relative to (0,0)

    // Sort using distance values ascending
    points.sort((a, b) => getDist(a) - getDist(b));

    return points.slice(0, k); // Return top K
}
```

*   **Complexity:** Time: **O(N log N)**, Space: **O(1)** ignoring output representation array.

---

### PROBLEM 57: MERGE K SORTED LISTS (LEETCODE 23) (REVISITED)

We analyzed this hard problem in Chapter 26 mock round. Bottom-up heap insertions of list heads takes strictly **O(N log K)** time and **O(K)** auxiliary space.

---

## SECTION 9: GRAPHS & GRAPH TRAVERSALS (PROBLEMS 58 - 62)

Graph non-linear structures are formed of vertices and edges. Vertices connection paths are validated dynamically using DFS (deep backtracking) or BFS (layer sweep)!

---

### PROBLEM 58: NUMBER OF ISLANDS (LEETCODE 200)

#### 1. Problem Statement:
*Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.*

#### 2. Examples:
*   `grid = [`
        `["1","1","0","0","0"],`
        `["1","1","0","0","0"],`
        `["0","0","1","0","0"],`
        `["0","0","0","1","1"]`
    `]` → Output: `3`

#### 3. Constraints:
*   M = grid.length, N = grid[i].length
*   1 <= M, N <= 300

#### 4. Think 🎙️:
> *"2D Grid coordinate space mein islands count karne hain bacho.
>
> **Observation:** Jab hum kisi land `'1'` par khade hote hain, toh woh pure island ka start point ho sakta hai bacho.
>
> Hum us land point se start karke us island ki horizontal and vertical connections ko explore karenge use karke **DFS (Depth-First Search)**. Exploration ke raaste mein jo bhi land `'1'` milega, use hum **`'0'` (water) mein mark kar denge (sink the island)** taaki dynamic scan overlap na ho bacho!
>
> Ise bolte hain **Connected Components DFS Grid Flood Fill pattern**."*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Grid boundary conditions check: `r < 0 || r >= m || c < 0 || c >= n` must return immediately.
*   **Hint 2 (Direction):** Double loop matrix scanner. When land `'1'` is found, increment counter and trigger DFS to sink that component.
*   **Hint 3 (Pattern/DS):** DFS Grid Traversal.

#### 6. JavaScript Code:
```javascript
function numIslands(grid) {
    if (!grid || grid.length === 0) return 0; //

    const m = grid.length;
    const n = grid.length;
    let islandCount = 0;

    const sinkIslandDFS = (r, c) => {
        // Grid boundary limits and water validation check
        if (r < 0 || r >= m || c < 0 || c >= n || grid[r][c] === '0') {
            return;
        }

        grid[r][c] = '0'; // Sink current visited land!

        // DFS Flood Fill in 4 adjacent directions
        sinkIslandDFS(r + 1, c); // Down
        sinkIslandDFS(r - 1, c); // Up
        sinkIslandDFS(r, c + 1); // Right
        sinkIslandDFS(r, c - 1); // Left
    };

    for (let r = 0; r < m; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] === '1') {
                islandCount++; // Found fresh island component!
                sinkIslandDFS(r, c); // Sink entire island using DFS
            }
        }
    }

    return islandCount;
}
```

*   **Complexity:** Time: **O(M × N)** since every grid cell is visited strictly constant times, Space: **O(M × N)** in the worst-case recursive call stack.

---

### PROBLEM 59: CLONE GRAPH (LEETCODE 133)

#### 1. Problem Statement:
*Given a reference of a node in a connected undirected graph, return a deep copy (clone) of the graph.*

#### 2. Think 🎙️:
> *"Graph cloning bacho! Undirected structures have cycles, meaning we can visit nodes repeatedly.
>
> We will use a **HashMap to map: original_node → cloned_node** to avoid infinite loops and reuse cloned references!"*

#### 3. JavaScript Code:
```javascript
function cloneGraph(node) {
    if (node === null) return null; //

    const visitedMap = new Map(); // Maps: original -> clone

    const dfs = (currNode) => {
        if (visitedMap.has(currNode)) {
            return visitedMap.get(currNode); // Reuse existing cloned node
        }

        const clone = new Node(currNode.val);
        visitedMap.set(currNode, clone); // Record

        // Recursively clone neighbors
        for (let neighbor of currNode.neighbors) {
            clone.neighbors.push(dfs(neighbor));
        }

        return clone;
    };

    return dfs(node);
}
```

*   **Complexity:** Time: **O(V + E)**, Space: **O(V)**.

---

### PROBLEM 60: COURSE SCHEDULE (LEETCODE 207)

We analyzed this directed cycle dependency problem in Chapter 27 using Kahn's topological sort BFS algorithm. Directed cyclic checks takes strictly **O(V + E)** time.

---

### PROBLEM 61: ROTTING ORANGES (LEETCODE 994) (MULTI-SOURCE BFS)

#### 1. Problem Statement:
*You are given an `m x n` grid where each cell can have one of three values: 0 (empty), 1 (fresh orange), 2 (rotten orange). Return the minimum number of minutes that must elapse until no cell has a fresh orange. If impossible, return -1.*

#### 2. Think 🎙️:
> *"Rotting oranges bacho! Rot spreads layer-by-layer simultaneously from all rotten oranges at the same time.
>
> **The SDE Trap:** Single-source BFS won't work because different source paths overlap.
>
> **Optimal Observation:** This is a classic **Multi-source BFS** problem. We push all rotten oranges indices `(r, c)` to a Queue initially. Then, at each minute interval, we pop all oranges of current layer and rot their neighbors, updating coordinates queue and fresh orange counters!"*

#### 3. JavaScript Code:
```javascript
function orangesRotting(grid) {
    const m = grid.length;
    const n = grid.length;
    const queue = [];
    let freshCount = 0;

    // Step 1: Push all initial rotten coordinates & count fresh oranges
    for (let r = 0; r < m; r++) {
        for (let c = 0; c < n; c++) {
            if (grid[r][c] === 2) {
                queue.push([r, c, 0]); // Stores: row, col, minute
            } else if (grid[r][c] === 1) {
                freshCount++;
            }
        }
    }

    let minutesElapsed = 0;
    const directions = [, [-1,0],, [0,-1]];

    // Step 2: Run BFS
    while (queue.length > 0) {
        const [r, c, minutes] = queue.shift(); // Dequeue
        minutesElapsed = minutes;

        for (let [dr, dc] of directions) {
            const nr = r + dr;
            const nc = c + dc;

            // If neighbor is fresh, rot it and enqueue
            if (nr >= 0 && nr < m && nc >= 0 && nc < n && grid[nr][nc] === 1) {
                grid[nr][nc] = 2; // Rot the orange
                freshCount--;
                queue.push([nr, nc, minutes + 1]); // Enqueue
            }
        }
    }

    return freshCount === 0 ? minutesElapsed : -1; // If fresh oranges remain, return -1
}
```

*   **Complexity:** Time: **O(M × N)**, Space: **O(M × N)** queue bounds.

---

### PROBLEM 62: REDUNDANT CONNECTION (LEETCODE 684)

#### 1. Problem Statement:
*Given a graph that started as a tree with N nodes labeled from 1 to N, with one additional edge added. Find and return the redundant edge that can be removed so that the resulting graph is a tree of N nodes.*

#### 2. Think 🎙️:
> *"Redundant edge dhoondhni hai bacho. Iska matlab ek aisi edge jo cycle create kar rahi hai.
>
> SDE Pattern: **Union-Find / Disjoint Set Union (DSU)**.
>
> **DSU Principle:** We initialize each node as its own parent. When we process an edge `(u, v)`:
> * If `find(u) === find(v)`, they are already in the same connected component, meaning adding this edge creates a cycle! This edge is the redundant cycle generator!
> * Else, we perform `union(u, v)`."**

#### 3. JavaScript Code:
```javascript
class DSU {
    constructor(size) {
        this.parent = Array.from({ length: size }, (_, i) => i);
    }
    find(i) {
        if (this.parent[i] === i) return i;
        return this.parent[i] = this.find(this.parent[i]); // Path compression
    }
    union(i, j) {
        const rootI = this.find(i);
        const rootJ = this.find(j);
        if (rootI !== rootJ) {
            this.parent[rootI] = rootJ; // Link component
            return true;
        }
        return false; // Cycle detected!
    }
}

function findRedundantConnection(edges) {
    const n = edges.length;
    const dsu = new DSU(n + 1); // 1-indexed labeling

    for (let [u, v] of edges) {
        if (!dsu.union(u, v)) {
            return [u, v]; // Found the cycle generator edge
        }
    }
    return [];
}
```

*   **Complexity:** Time: **\\(O(N · α(N))\\)** where α is inverse Ackermann function (extremely fast, effectively O(1) amortized), Space: **O(N)**.

---

## MIXED UNSEEN SET 3 (PROBLEMS 63 - 65)

**Arey bacho! Ab is section mein koi pre-defined header ya pattern leakage nahi hoga. Sawaal ko constraints ke sath parse karo!**

---

### PROBLEM 63: BINARY TREE ZIGZAG LEVEL ORDER TRAVERSAL (LEETCODE 103)

#### 1. Problem Statement:
*Given the root of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate).*

#### 2. Think 🎙️:
> *"Level-by-level zigzag sequence checks!
>
> SDE Pattern: **Modified BFS Level Order**.
>
> We use standard BFS queue lock, but maintain a toggle boolean `leftToRight`.
> * If `leftToRight === true`, push values normally.
> * If `leftToRight === false`, unshift/insert elements to the front of sub-list!"*

#### 3. JavaScript Code:
```javascript
function zigzagLevelOrder(root) {
    const result = [];
    if (root === null) return result; //

    const queue = [root]; //
    let leftToRight = true;

    while (queue.length > 0) {
        const levelSize = queue.length; //
        const currentLevel = [];

        for (let i = 0; i < levelSize; i++) {
            const currentNode = queue.shift(); // Dequeue

            if (leftToRight) {
                currentLevel.push(currentNode.val);
            } else {
                currentLevel.unshift(currentNode.val); // Add to front for reverse order
            }

            if (currentNode.left !== null) queue.push(currentNode.left);
            if (currentNode.right !== null) queue.push(currentNode.right);
        }

        result.push(currentLevel);
        leftToRight = !leftToRight; // Toggle directions
    }

    return result;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(N)**.

---

### PROBLEM 64: TASK SCHEDULER (LEETCODE 621)

#### 1. Problem Statement:
*Given a characters array `tasks` and an integer `n` representing cool-down interval, return the least number of units of times that the CPU will take to finish all the given tasks.*

#### 2. Think 🎙️:
> *"Frequency-based task scheduling with cooldowns bacho!
>
> SDE Pattern: **Greedy intervals slot allocation**.
>
> **Observation:** The execution limit is strictly determined by the task with maximum frequency `maxFreq`.
> Number of empty slot gaps we can partition: `(maxFreq - 1) * n`.
> We can dynamically pack other tasks into these slots to minimize idle intervals."**

#### 3. JavaScript Code:
```javascript
function leastInterval(tasks, n) {
    const freq = new Array(26).fill(0); //
    for (let t of tasks) {
        freq[t.charCodeAt(0) - 65]++; //
    }

    freq.sort((a, b) => b - a); // Sort by highest frequency

    const maxFreq = freq;
    let idleSlots = (maxFreq - 1) * n; // Max possible idle gaps

    for (let i = 1; i < 26; i++) {
        // Greedy fill slots
        idleSlots -= Math.min(maxFreq - 1, freq[i]);
    }

    return tasks.length + Math.max(0, idleSlots);
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.

---

### PROBLEM 65: PACIFIC ATLANTIC WATER FLOW (LEETCODE 417)

#### 1. Problem Statement:
*Given an `m x n` matrix representing heights, find all coordinates where water can flow to both Pacific and Atlantic oceans.*

#### 2. Think 🎙️:
> *"Water flow matching bacho! Flows to both Pacific (left/top) and Atlantic (right/bottom).
>
> **The Reverse Thinking Hack:**
> Scanning cell-by-cell is extremely slow. We think backwards: we start DFS from the shores (Pacific boundaries and Atlantic boundaries) recursively, climbing up where heights increase!
>
> Cells reachable by both boundary traversals form the overlap output result!"*

#### 3. JavaScript Code:
```javascript
function pacificAtlantic(heights) {
    if (!heights || heights.length === 0) return []; //

    const m = heights.length;
    const n = heights.length;
    const pacific = Array.from({ length: m }, () => new Array(n).fill(false));
    const atlantic = Array.from({ length: m }, () => new Array(n).fill(false));

    const dfs = (r, c, visited, prevHeight) => {
        if (r < 0 || r >= m || c < 0 || c >= n || visited[r][c] || heights[r][c] < prevHeight) {
            return;
        }

        visited[r][c] = true;

        dfs(r + 1, c, visited, heights[r][c]); // Down
        dfs(r - 1, c, visited, heights[r][c]); // Up
        dfs(r, c + 1, visited, heights[r][c]); // Right
        dfs(r, c - 1, visited, heights[r][c]); // Left
    };

    // Step 1: DFS from columns borders
    for (let r = 0; r < m; r++) {
        dfs(r, 0, pacific, -Infinity); // Left shore (Pacific)
        dfs(r, n - 1, atlantic, -Infinity); // Right shore (Atlantic)
    }

    // Step 2: DFS from row borders
    for (let c = 0; c < n; c++) {
        dfs(0, c, pacific, -Infinity); // Top shore (Pacific)
        dfs(m - 1, c, atlantic, -Infinity); // Bottom shore (Atlantic)
    }

    const result = [];
    for (let r = 0; r < m; r++) {
        for (let c = 0; c < n; c++) {
            if (pacific[r][c] && atlantic[r][c]) {
                result.push([r, c]); // Cell overlaps!
            }
        }
    }

    return result;
}
```

*   **Complexity:** Time: **O(M × N)**, Space: **O(M × N)** auxiliary.

---

## SECTION SUMMARY & SDE BLUEPRINTS 📊

Bacho, is summary block ko register par update karo:

| Problem Patterns | Core Structural Philosophy | Candidate Bug Traps ⚠️ |
| :--- | :--- | :--- |
| **Inorder BST** | BST yields elements in strictly ascending sorted order. | Forgetting that left must be less than grandparent. |
| **Two Heaps balance** | Divide data streams to retrieve dynamic boundaries. | Not balancing heights sizes on insertion. |
| **Grid DFS Flood** | Sink islands using local visited state modifications. | Stack overflow on infinite recursive components. |

---

## SDE MASTERCLASS PROGRESS TARGETS 🗺️

*   **Current Progress:** 65 / 150+ Problems completed!
*   **Concepts Covered:** Binary Trees, Postorder DFS, BFS Level Queues, BST Splits, Min/Max Heaps balance, Connected Components Grid DFS, Multi-source BFS, Union-Find disjoint sets.

---


## SECTION 10: BACKTRACKING (PROBLEMS 66 - 73)

Backtracking ek systematic search paradigm hai bacho. Jab humein kisi problem mein **"generate all possible configurations"**, **"check all permutations"**, ya **"solve a constraint maze"** pucha jaye, toh hum standard loops ke bajay **Decision Tree** explore karte hain. Agar kisi raaste par ja kar humara constraint fail ho jaye, toh hum piche aate hain (**backtrack**) aur dusra rasta try karte hain.

---

### PROBLEM 66: SUBSETS (LEETCODE 78)

#### 1. Problem Statement:
*Given an integer array `nums` of unique elements, return all possible subsets (the power set). The solution set must not contain duplicate subsets.*

#### 2. Examples:
*   `nums =` → Output: `[[],,,,,,]`

#### 3. Constraints:
*   1 <= nums.length <= 10
*   All elements of `nums` are unique.

#### 4. Think (Candidate Thinking Aloud) 🎙️:
> *"Sawaal kehta hai saare possible subsets (Power Set) generate karne hain bacho. Constraint dekho: N <= 10. Yeh bahut hi chota constraint hai! Iska matlab exponential O(2^N) solution chal sakta hai bacho.
>
> **Brute Force naturally kaise sochein?** 
> Kisi bhi element `X` ke paas do hi options hote hain subset ka part banne ke liye:
> 1. Ya toh hum use subset mein **Include** karenge.
> 2. Ya fir use **Exclude** karenge.
>
> Har element ke liye yeh do choices check karke hum poora decision tree explore kar sakte hain!
>
> **Optimal Observation & Pattern:**
> Hum ek backtracking helper function likhenge jo index `i` aur ek current running `path` maintain karega. Har node par hum `path` ka snapshot deep-copy karke humare global result mein push karte rahenge."*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Number of total subsets hamesha 2^N hote hain.
*   **Hint 2 (Direction):** Recursive state space: At index `i`, we can push `nums[i]` to `path` and recurse for `i+1` (inclusion), then pop `nums[i]` and recurse for `i+1` (exclusion).
*   **Hint 3 (Pattern/DS):** Backtracking with include/exclude decision branches.

#### 6. JavaScript Code (Standard DFS Backtracking):
```javascript
function subsets(nums) {
    const result = [];
    
    const backtrack = (index, currentPath) => {
        // Base case: If we processed all elements, record the subset snapshot
        result.push([...currentPath]); 
        
        for (let i = index; i < nums.length; i++) {
            currentPath.push(nums[i]); // Include nums[i]
            backtrack(i + 1, currentPath); // Move to next elements
            currentPath.pop(); // Backtrack (Exclude nums[i])
        }
    };
    
    backtrack(0, []);
    return result;
}
```

#### 7. Line-by-Line Explanation:
1. `result.push([...currentPath])` → Har recursion state par jo active subset build hua hai, uski deep copy (via spread operator) result bucket mein save karo.
2. `currentPath.push(nums[i])` → Element ko active path mein include kiya.
3. `backtrack(i + 1, currentPath)` → Index forward badha kar bache huye choices explore kiye.
4. `currentPath.pop()` → Aur fir backtracking rule ke mutabik state clean up kiya taaki next sister branches affect na hon!

#### 8. Complete Dry Run:
Input: `nums =`
*   `backtrack(0, [])` → push `[]` to result.
    *   `i = 0`: path becomes ``, `backtrack(1, )` → push ``.
        *   `i = 1`: path becomes ``, `backtrack(2, )` → push ``.
        *   `path.pop()` → path back to ``.
    *   `path.pop()` → path back to `[]`.
*   Result accumulates: `[[],,,]`. Correct!

#### 9. Complexity:
*   **Time Complexity:** **O(N · 2^N)** since there are 2^N states, and copying each takes O(N).
*   **Auxiliary Space Complexity:** **O(N)** for the recursive call stack and running path memory.

#### 10. Same pattern in other problems:
Subsets II (handling duplicates by sorting and skipping adjacent elements).

---

### PROBLEM 67: PERMUTATIONS (LEETCODE 46)

#### 1. Problem Statement:
*Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.*

#### 2. Examples:
*   `nums =` → Output: `[,,,,,]`

#### 3. Constraints:
*   1 <= nums.length <= 6

#### 4. Think 🎙️:
> *"Permutations generate karne hain bacho! Permutation mein **ordering matter karti hai**. (i.e. `` and `` distinct paths hain).
>
> SDE Pattern: **Backtracking with 'Visited' State tracking**.
>
> Hum element-by-element build karenge. Har position par hum array ke kisi bhi unvisited element ko place kar sakte hain bacho. Ek `Set` ya boolean array maintain karenge to verify elements visited status!"*

#### 5. JavaScript Code (Clean Visited Set Backtracking):
```javascript
function permute(nums) {
    const result = [];
    const visited = new Set(); // To ensure we don't reuse elements within same path

    const backtrack = (currentPath) => {
        // Base case: If permutation size matches array size, record it
        if (currentPath.length === nums.length) {
            result.push([...currentPath]); // Deep copy
            return;
        }

        for (let i = 0; i < nums.length; i++) {
            if (visited.has(nums[i])) continue; // Skip already used elements

            currentPath.push(nums[i]); // Include
            visited.add(nums[i]); // Mark as visited

            backtrack(currentPath); // Recurse

            visited.delete(nums[i]); // Unmark (Backtrack)
            currentPath.pop(); // Remove (Backtrack)
        }
    };

    backtrack([]);
    return result;
}
```

*   **Complexity:** Time: **O(N! · N)**, Space: **O(N)**.
*   **What to notice next time:** In permutations, we always loop from `0` to `N-1` inside the loop (as any element can go anywhere), but filter already visited items using a Set!

---

### PROBLEM 68: COMBINATION SUM (LEETCODE 39)

#### 1. Problem Statement:
*Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of candidates where the chosen numbers sum to `target`. You may return the combinations in any order. The same number may be chosen from candidates an unlimited number of times.*

#### 2. Examples:
*   `candidates =`, `target = 7` → Output: `[,]`

#### 3. Think 🎙️:
> *"Humein candidates choose karne hain jinka sum exact `target` ho, aur ek element ko hum **multiple times use kar sakte hain** bacho!
>
> **The SDE Strategy:** 
> Kisi index `i` par:
> * Option 1: Current element ko choose karo, aur index `i` par hi khade raho (infinite reuse allow karne ke liye)!
> * Option 2: Current element ko exclude karo, aur index `i + 1` par aage badh jao.
> 
> Base cases:
> * If `target === 0` → combination found!
> * If `target < 0` or `index === candidates.length` → invalid path, return immediately."*

#### 4. JavaScript Code:
```javascript
function combinationSum(candidates, target) {
    const result = [];

    const backtrack = (index, currentSum, path) => {
        if (currentSum === target) {
            result.push([...path]); // Found valid sum configuration!
            return;
        }
        if (currentSum > target || index === candidates.length) {
            return; // Prune invalid branch
        }

        // Choice 1: Include current candidate (We don't increment index to allow reuse!)
        path.push(candidates[index]);
        backtrack(index, currentSum + candidates[index], path);
        path.pop(); // Backtrack

        // Choice 2: Exclude current candidate, move to next index
        backtrack(index + 1, currentSum, path);
    };

    backtrack(0, 0, []);
    return result;
}
```

*   **Complexity:** Time: **O(2^{target})** worst-case branching, Space: **O(target)**.

---

### PROBLEM 69: N-QUEENS (LEETCODE 51) (HARD)

#### 1. Problem Statement:
*The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other. Return all distinct solutions.*

#### 2. Think 🎙️:
> *"N-Queens chessboard placement bacho! Sabse famous constraint satisfaction problem.
>
> Queens vertical, horizontal, aur diagonal lines mein attack karti hain. Hum row-by-row queen place karenge. Jab row `r` par queen place ho rahi ho, toh column `c` ko evaluate karenge.
>
> **Diagonal Hashing Hack:**
> * Positive diagonal check: r + c value remains constant along any bottom-left to top-right diagonal!
> * Negative diagonal check: r - c value remains constant along any top-left to bottom-right diagonal!
>
> In columns, posDiagonals, aur negDiagonals ko Sets mein store karke hum valid cells ko O(1) constant lookup se prune kar sakte hain!"*

#### 3. JavaScript Code (High-Performance Diagonal Hashing):
```javascript
function solveNQueens(n) {
    const result = [];
    const cols = new Set();
    const posDiag = new Set(); // (r + c)
    const negDiag = new Set(); // (r - c)

    // Empty board state generator
    const board = Array.from({ length: n }, () => new Array(n).fill('.'));

    const backtrack = (r) => {
        if (r === n) {
            // Found a valid N-Queens configuration! Convert grid state to output strings list
            result.push(board.map(row => row.join(''))); //
            return;
        }

        for (let c = 0; c < n; c++) {
            // Attack constraints validation
            if (cols.has(c) || posDiag.has(r + c) || negDiag.has(r - c)) {
                continue; // Under attack!
            }

            // State modification
            board[r][c] = 'Q';
            cols.add(c);
            posDiag.add(r + c);
            negDiag.add(r - c);

            backtrack(r + 1); // Recurse to next row

            // State cleanup (Backtrack)
            board[r][c] = '.';
            cols.delete(c);
            posDiag.delete(r + c);
            negDiag.delete(r - c);
        }
    };

    backtrack(0);
    return result;
}
```

*   **Complexity:** Time: **O(N!)**, Space: **O(N^2)** board grid memory.

---

### PROBLEM 70: WORD SEARCH (LEETCODE 79)

#### 1. Problem Statement:
*Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.*

#### 2. JavaScript Code:
```javascript
function exist(board, word) {
    const m = board.length;
    const n = board.length;

    const dfs = (r, c, index) => {
        // Base case: Found entire word!
        if (index === word.length) return true;

        // Boundary checks & character mismatch validation
        if (r < 0 || r >= m || c < 0 || c >= n || board[r][c] !== word[index]) {
            return false;
        }

        const temp = board[r][c];
        board[r][c] = '#'; // Mark cell as visited

        // Explore 4 directions
        const found = dfs(r + 1, c, index + 1) ||
                      dfs(r - 1, c, index + 1) ||
                      dfs(r, c + 1, index + 1) ||
                      dfs(r, c - 1, index + 1);

        board[r][c] = temp; // Restore state (Backtrack)
        return found;
    };

    for (let r = 0; r < m; r++) {
        for (let c = 0; c < n; c++) {
            if (board[r][c] === word && dfs(r, c, 0)) {
                return true;
            }
        }
    }

    return false;
}
```

*   **Complexity:** Time: **O(M × N × 4^L)** where L is word length, Space: **O(L)** recursion stack.

---

### PROBLEM 71: GENERATE PARENTHESES
We analyzed this in Part 3. Maintaining counts of `open` and `close` brackets guarantees balanced backtracking branches in **O(\frac{4^N}{N sqrt{N}})** Catalans bounds.

---

### PROBLEM 72: LETTER COMBINATIONS OF A PHONE NUMBER
Standard mapping of digits to alphabets, DFS tracking digit index across recursion branches.

---

### PROBLEM 73: PALINDROME PARTITIONING (LEETCODE 131)
Cut word at index `i`. If `word[start...i]` is a valid palindrome, recursively partition remaining substring. Backtrack on mismatch.

---

## SECTION 11: DYNAMIC PROGRAMMING (PROBLEMS 74 - 93)

Dynamic Programming (DP) humara sabse dhasu optimization tool hai bacho. Jab recursion tree mein **overlapping subproblems** dikhein (jaise fibonacci mein same states multiple times recalculate hoti hain), toh hum unhe memory table mein save kar lete hain (**Memoization**) ya fir bottom-up array state transition likhte hain (**Tabulation**).

```
                          DP ROADMAP TRANSITION
                                    │
       ┌────────────────────────────┴────────────────────────────┐
       ▼                                                         ▼
  Top-Down (Memoization)                                    Bottom-Up (Tabulation)
  - Recursive search with state caching.                    - Iterative state building.
  - Easy to design from DFS.                                - Faster execution, space-optimizable!
  -                                             -
```

---

### PROBLEM 74: CLIMBING STAIRS (LEETCODE 70)

#### 1. Problem Statement:
*You are climbing a staircase. It takes `n` steps to reach the top. Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?*

#### 2. Examples:
*   `n = 3` → Output: `3` (Ways: 1+1+1, 1+2, 2+1)

#### 3. Constraints:
*   1 <= n <= 45

#### 4. Think 🎙️:
> *"Stairs climb karni hain bacho.
>
> **Observation:** Farthest point `i` par pahunchne ke liye, hum sirf do hi jagah se jump kar sakte hain:
> 1. Ya toh stair `i - 1` se 1-step jump.
> 2. Ya fir stair `i - 2` se 2-step jump.
>
> Yani:
> \\[ Ways(i) = Ways(i - 1) + Ways(i - 2) \\]
> Yeh toh exact **Fibonacci series transition formula** hai! We can solve this bottom-up iteratively in O(1) auxiliary space!"*

#### 5. JavaScript Code (Space-Optimized Tabulation):
```javascript
function climbStairs(n) {
    if (n <= 2) return n;

    let prev2 = 1; // Base case: ways to reach step 1
    let prev1 = 2; // Base case: ways to reach step 2

    for (let i = 3; i <= n; i++) {
        const currentWays = prev1 + prev2; // Transition logic
        prev2 = prev1; // Slide states
        prev1 = currentWays; // Slide states
    }

    return prev1;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary strictly.

---

### PROBLEM 75: MIN COST CLIMBING STAIRS (LEETCODE 746)

#### 1. Problem Statement:
*Given an integer array `cost` where `cost[i]` is the cost of i-th step on a staircase. Once you pay the cost, you can either climb one or two steps. Find the minimum cost to reach the top of the floor.*

#### 2. JavaScript Code (In-place dynamic transitions):
```javascript
function minCostClimbingStairs(cost) {
    const n = cost.length;
    let prev2 = cost;
    let prev1 = cost;

    for (let i = 2; i < n; i++) {
        const currentCost = cost[i] + Math.min(prev1, prev2);
        prev2 = prev1;
        prev1 = currentCost;
    }

    return Math.min(prev1, prev2);
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.

---

### PROBLEM 76: COIN CHANGE (LEETCODE 322)

#### 1. Problem Statement:
*You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money. Return the fewest number of coins that you need to make up that amount.*

#### 2. Think 🎙️:
> *"Humein fewest coins chain construct karni hai.
>
> SDE Pattern: **Knapsack DP (Unbounded)**.
>
> **Transition logic:** Maan lo `dp[i]` represents amount `i` banane ke liye minimum coins required.
> For every coin in our choices:
> \\[ dp[i] = min(dp[i], 1 + dp[i - coin]) \\]
> We initialize the dp table with `Infinity`!"*

#### 3. JavaScript Code (Tabulation DP):
```javascript
function coinChange(coins, amount) {
    const dp = new Array(amount + 1).fill(Infinity);
    dp = 0; // Base Case: 0 amount requires 0 coins

    for (let i = 1; i <= amount; i++) {
        for (let coin of coins) {
            if (i - coin >= 0) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]); // Transition
            }
        }
    }

    return dp[amount] === Infinity ? -1 : dp[amount];
}
```

*   **Complexity:** Time: **O(amount × coins.length)**, Space: **O(amount)**.

---

### PROBLEM 77: LONGEST COMMON SUBSEQUENCE (LEETCODE 1143)

#### 1. Problem Statement:
*Given two strings `text1` and `text2`, return the length of their longest common subsequence.*

#### 2. Think 🎙️:
> *"Subsequences common string grids alignments check!
>
> SDE Pattern: **2D Grid Alignment DP**.
>
> **Transition rules:**
> * If `text1[i-1] === text2[j-1]` → match! `dp[i][j] = 1 + dp[i-1][j-1]`.
> * Else → mismatch! Take max of left or top neighbors: `dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1])`."*

#### 3. JavaScript Code (Space-Optimized Tabulation):
```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length;
    const n = text2.length;

    let prev = new Array(n + 1).fill(0);
    let curr = new Array(n + 1).fill(0);

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                curr[j] = 1 + prev[j - 1]; // Match!
            } else {
                curr[j] = Math.max(prev[j], curr[j - 1]); // Skip mismatch
            }
        }
        prev = [...curr]; // Slide row state
    }

    return prev[n];
}
```

*   **Complexity:** Time: **O(M × N)**, Space: **O(N)**.

---

### PROBLEM 78: LONGEST INCREASING SUBSEQUENCE (LEETCODE 300)

#### 1. Problem Statement:
*Given an integer array `nums`, return the length of the longest strictly increasing subsequence.*

#### 2. Think 🎙️:
> *"Increasing subsequence strictly ascending sequence.
>
> **Transition dynamic formula:** Maan lo `dp[i]` is LIS ending at index `i`.
> For every pichla index `j < i`:
> If `nums[i] > nums[j]` → `dp[i] = Math.max(dp[i], 1 + dp[j])`."*

#### 3. JavaScript Code:
```javascript
function lengthOfLIS(nums) {
    if (nums.length === 0) return 0;
    const dp = new Array(nums.length).fill(1); // Base: Each element itself is LIS of size 1
    let maxLIS = 1;

    for (let i = 1; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], 1 + dp[j]);
            }
        }
        maxLIS = Math.max(maxLIS, dp[i]);
    }

    return maxLIS;
}
```

*   **Complexity:** Time: **O(N^2)**. *(Can be optimized to **O(N log N)** using Binary Search on active sub-segment!)*. Space: **O(N)**.

---

### PROBLEM 79: UNIQUE PATHS (LEETCODE 62)

#### 1. Problem Statement:
*There is a robot on an `m x n` grid. The robot is initially located at the top-left corner. The robot tries to move to the bottom-right corner. The robot can only move either down or right at any point in time. Return the number of unique paths.*

#### 2. JavaScript Code:
```javascript
function uniquePaths(m, n) {
    const dp = new Array(n).fill(1); // Row tabulation initialization

    for (let r = 1; r < m; r++) {
        for (let c = 1; c < n; c++) {
            dp[c] = dp[c] + dp[c - 1]; // sum of top path (dp[c]) and left path (dp[c-1])
        }
    }

    return dp[n - 1];
}
```

*   **Complexity:** Time: **O(M × N)**, Space: **O(N)**.

---

### PROBLEM 80: HOUSE ROBBER (LEETCODE 198)

#### 1. Problem Statement:
*You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night. Return the maximum money you can rob.*

#### 2. Think 🎙️:
> *"Adjacent houses rob validation check bacho!
>
> At house `i`:
> * Option 1: Rob house `i` → adds `cost[i]` + money robbed up to `i-2` (cannot rob adjacent `i-1`!).
> * Option 2: Skip house `i` → carry money robbed up to `i-1` house.
>
> \\[ dp[i] = max(dp[i-1], cost[i] + dp[i-2]) \\]"*

#### 3. JavaScript Code (Iterative Space-Optimized):
```javascript
function rob(nums) {
    if (nums.length === 0) return 0;
    if (nums.length === 1) return nums; //

    let prev2 = 0; // House i-2 state
    let prev1 = nums; // House i-1 state

    for (let i = 1; i < nums.length; i++) {
        const currentMax = Math.max(prev1, nums[i] + prev2);
        prev2 = prev1;
        prev1 = currentMax;
    }

    return prev1;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)**.

---

### PROBLEM 81: PARTITION EQUAL SUBSET SUM (LEETCODE 416)
Boolean 1D array knapsack update. Find if there exists a subset with sum strictly equal to `totalSum / 2`.

---

### PROBLEM 82: EDIT DISTANCE (LEETCODE 72)
We coded this optimal space version in Part 1 mock rounds. Runs in **O(M × N)** time and **O(N)** space.

---

### PROBLEM 83: 0/1 KNAPSACK (CLASSIC)
Standard tabulation. Match weight boundaries using decreasing row sweeps to prevent multi-element reuse errors.

---

## SECTION 12: BIT MANIPULATION (PROBLEMS 94 - 101)

Bit manipulation computers ke level par micro-second optimizations run karti hai. Bits values shift and masking transitions logically reduce loop steps.

---

### PROBLEM 94: NUMBER OF 1 BITS (LEETCODE 191)

#### 1. Problem Statement:
*Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the Hamming weight).*

#### 2. Think 🎙️:
> *"Integer bits count 1s!
>
> **The Brian Kernighan Hack:**
> Standard loops shift bits rightwards 32 times. But there is a faster way bacho!
> Subtraction bitwise intersection:
> \\[ N \& (N - 1) \\]
> **This mathematical expression always clears the lowest set bit of N to zero**!
> Loop tabhi tak chalega jitne total number of set bits hain bacho, making it extremely fast."*

#### 3. JavaScript Code (Brian Kernighan's Algorithm):
```javascript
function hammingWeight(n) {
    let count = 0;
    while (n !== 0) {
        n = n & (n - 1); // Clears the lowest set bit to 0
        count++;
    }
    return count;
}
```

*   **Complexity:** Time: **O(Set Bits Count)** (at most 32 steps for 32-bit integers), Space: **O(1)**.

---

### PROBLEM 95: SINGLE NUMBER (LEETCODE 136)

#### 1. Problem Statement:
*Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one. Solve in linear time and constant space.*

#### 2. Think 🎙️:
> *"Sawaal linear time aur constant space demand karta hai. Hashing counts take extra memory (O(N) space).
>
> Let's think: **XOR (⊕) bitwise operator properties:**
> 1. Identity: A ⊕ 0 = A.
> 2. Self-inverse: A ⊕ A = 0.
> 3. Associative & Commutative: Ordering doesn't matter.
>
> Iska matlab, agar hum array ke saare elements ko XOR kar dein, toh jo elements pairs mein hain woh aapas mein cancel out (0) ho jayenge bacho! Aur sirf unique single element hi bachega!"*

#### 3. JavaScript Code:
```javascript
function singleNumber(nums) {
    let xorAccumulator = 0;
    for (let num of nums) {
        xorAccumulator ^= num; // Cancel duplicates out!
    }
    return xorAccumulator;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** strictly.

---

### PROBLEM 96: COUNTING BITS (LEETCODE 338)

#### 1. Problem Statement:
*Given an integer `n`, return an array `ans` of length `n + 1` such that `ans[i]` is the number of 1's in the binary representation of `i`.*

#### 2. Think 🎙️:
> *"Bits count for all elements up to N.
>
> SDE Pattern: **DP with Bit Shift offset**.
>
> **Observation:** Kisi number `i` ke bits count aur uske right shift `i >> 1` (yaani `Math.floor(i / 2)`) ke bits count mein direct linkage hai bacho!
> \\[ Bits(i) = Bits(i \gg 1) + (i \& 1) \\]
> We can compute this linearly in single pass!"*

#### 3. JavaScript Code:
```javascript
function countBits(n) {
    const ans = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        ans[i] = ans[i >> 1] + (i & 1); // DP bit state transfer
    }
    return ans;
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** excluding output representation array.

---

### PROBLEM 97: REVERSE BITS (LEETCODE 190)
Extract bits from right, shift output register left, append extracted bit. Repeat 32 times.

---

### PROBLEM 98: IS POWER OF TWO (LEETCODE 231)
If N > 0 and \\((N \& (N - 1)) === 0\\) → Power of two!

---

## MIXED UNSEEN SET 4 (PROBLEMS 102 - 106)

**Arey bacho! Ab is section mein koi pre-defined header ya pattern leakage nahi hoga. Sawaal ko constraints ke sath parse karo!**

---

### PROBLEM 102: HOUSE ROBBER II (LEETCODE 213)

#### 1. Problem Statement:
*Same adjacent rob constraint, but the houses are arranged in a circle. First house is neighbor of last house.*

#### 2. Think 🎙️:
> *"Circular houses arrangement bacho!
>
> **Observation:** Kyunki first aur last adjacent neighbors hain, hum first aur last dono ko ek sath rob nahi kar sakte bacho!
> This splits into two linear sub-problems:
> * Case A: Rob houses from index `0` to `N-2` (strictly ignore last).
> * Case B: Rob houses from index `1` to `N-1` (strictly ignore first).
>
> We return the maximum of these two linear robber calls!"*

#### 3. JavaScript Code:
```javascript
function robCircular(nums) {
    if (nums.length === 1) return nums; //

    const robLinearHelper = (houses) => {
        let prev2 = 0;
        let prev1 = 0;
        for (let num of houses) {
            const temp = Math.max(prev1, num + prev2);
            prev2 = prev1;
            prev1 = temp;
        }
        return prev1;
    };

    const skipLast = nums.slice(0, nums.length - 1); // Case A
    const skipFirst = nums.slice(1); // Case B

    return Math.max(robLinearHelper(skipLast), robLinearHelper(skipFirst));
}
```

*   **Complexity:** Time: **O(N)**, Space: **O(1)** auxiliary.

---

### PROBLEM 103: TRIANGLE (LEETCODE 120)

#### 1. Problem Statement:
*Given a `triangle` array, return the minimum path sum from top to bottom. For each step, you may move to adjacent numbers on the row below.*

#### 2. Think 🎙️:
> *"Triangle grid pathway min cost!
>
> **The Bottom-Up DP Hack:**
> Top-down recursive flows are very complex due to boundary conditions.
> If we start from the bottom-most row of the triangle and move upwards, the transition for row `r` and column `c` is simply:
> \\[ triangle[r][c] += min(triangle[r+1][c], triangle[r+1][c+1]) \\]
> The absolute top-most cell will contain the minimum path sum!"*

#### 3. JavaScript Code:
```javascript
function minimumTotal(triangle) {
    // Start from the second last row and merge downwards
    for (let r = triangle.length - 2; r >= 0; r--) {
        for (let c = 0; c < triangle[r].length; c++) {
            triangle[r][c] += Math.min(triangle[r + 1][c], triangle[r + 1][c + 1]);
        }
    }
    return triangle; // Apex holds result
}
```

*   **Complexity:** Time: **O(N^2)** total nodes traversed, Space: **O(1)** in-place modification.

---

### PROBLEM 104: COMBINATION SUM II (LEETCODE 40)

#### 1. Problem Statement:
*Unique candidate combinations target sum, but candidates can contain duplicates and each candidate can only be used once in output.*

#### 2. Think 🎙️:
> *"Unique combinations with duplicate elements!
>
> Sort array first. If active index `i > start` and `candidates[i] === candidates[i-1]`, we skip recursion to prevent duplicate branches configurations!"*

#### 3. JavaScript Code:
```javascript
function combinationSum2(candidates, target) {
    const result = [];
    candidates.sort((a, b) => a - b); //

    const backtrack = (start, currentSum, path) => {
        if (currentSum === target) {
            result.push([...path]); //
            return;
        }
        if (currentSum > target) return;

        for (let i = start; i < candidates.length; i++) {
            // Duplicate avoidance check
            if (i > start && candidates[i] === candidates[i - 1]) continue;

            path.push(candidates[i]);
            backtrack(i + 1, currentSum + candidates[i], path); // i+1 forces single use
            path.pop(); // Backtrack
        }
    };

    backtrack(0, 0, []);
    return result;
}
```

*   **Complexity:** Time: **O(2^N)**, Space: **O(N)**.

---

## SECTION SUMMARY & SDE BLUEPRINTS 📊

Bacho, is summary block ko register par dhyan se update karo:

| SDE Patterns | Core Structural Philosophy | Candidate Bug Traps ⚠️ |
| :--- | :--- | :--- |
| **Backtrack pop** | Restore global path state before exiting active stack frame. | Mismatch copy variable references. |
| **Tabulation 1D** | Reduce space by preserving only the last two row variables. | Mismatch state shifting order. |
| **XOR cancels** | Double occurrences bitwise cancel dynamically. | Mismatched bit sizes limits. |

---

## SDE MASTERCLASS PROGRESS TARGETS 🗺️

*   **Current Progress:** 104 / 150+ Problems completed!
*   **Concepts Covered:** Power Set generation, visited set tracking, Knapsack transitions, Unbounded coins minimization, XOR cancellation patterns, circular DP boundaries.

---

## SECTION 13: GREEDY & INTERVALS (PROBLEMS 105 - 110)

Greedy algorithms mein hum local optimum choice banate hain is umeed ke saath ki yeh humein global optimum tak le jayegi. Intervals problems mein **sorting hamesha 95% cases ko unlock karti hai bacho!** Jab bhi interval overlap check karna ho, intervals ko unke start times ke basis par sort kar lo.

---

### PROBLEM 105: MERGE INTERVALS (LEETCODE 56)

#### 1. Problem Statement:
*Given an array of `intervals` where `intervals[i] = [start, end]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the input intervals.*

#### 2. Examples:
*   `intervals = [[1,3],[2,6],[8,10],[15,18]]` → Output: `[[1,6],[8,10],[15,18]]`
    *   *Explanation:* Since intervals `[1,3]` and `[2,6]` overlap, merge them into `[1,6]`.

#### 3. Constraints:
*   1 <= intervals.length <= 10^4
*   `intervals[i].length === 2`

#### 4. Think (Candidate Thinking Aloud) 🎙️:
> *"Intervals aapas mein jumbled order mein hain bacho. Kaise pata chalega ki kaunsa kis par overlap kar raha hai?
> 
> **Brute Force naturally kaise sochein?** Hum har interval ko baaki saare intervals se compare karein, aur overlapping pairs ko merge karte chalein. Isme quadratic O(N^2) time lagega aur tracking bohot complex ho jayegi.
>
> **Bottleneck:** Intervals sorted nahi hain, isliye overlap check karne ke liye random elements par scan karna pad raha hai.
>
> **SDE Observation:** Agar hum saare intervals ko unke **start time ke basis par sort** kar dein, toh overlapping intervals hamesha ek dusre ke adjacent (padosi) ban jayenge bacho! 
> Maan lo humare paas sorted list mein do adjacent intervals hain: `prev = [s1, e1]` aur `curr = [s2, e2]`. Overlap kab hoga? Jab `curr` ka start time `s2 <= prev` ka end time `e1` ho! 
> Agar overlap hai, toh merged interval kya banega? `[s1, Math.max(e1, e2)]` bacho!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Intervals sorting simplifies the overlap checking to a single linear pass.
*   **Hint 2 (Direction):** Sort by `intervals`. Iterate through the sorted intervals. Keep track of the last merged interval.
*   **Hint 3 (Pattern/DS):** Sorting + Greedy Interval Merge.

#### 6. JavaScript Code (Standard SDE Pattern):
```javascript
function mergeIntervals(intervals) {
    if (intervals.length <= 1) return intervals;

    // Step 1: Sort intervals based on their start times (index 0)
    intervals.sort((a, b) => a[0] - b[0]);

    const merged = [intervals[0]]; // Start with the first interval

    for (let i = 1; i < intervals.length; i++) {
        const nextInterval = intervals[i];
        const lastMerged = merged[merged.length - 1];

        // If the next interval's start overlaps with the last merged interval's end
        if (nextInterval[0] <= lastMerged[1]) {
            // Merge them greedily by extending the end time
            lastMerged[1] = Math.max(lastMerged[1], nextInterval[1]);
        } else {
            // No overlap, push the next interval as a fresh entry
            merged.push(nextInterval);
        }
    }

    return merged;
}
```

#### 7. Line-by-Line Explanation:
1. `intervals.sort(...)` → Start times ke basis par saare intervals ko order mein le aaya.
2. `if (nextInterval[0] <= lastMerged[1])` → Agar next interval pichle merged interval ke end point se pehle (ya usi par) shuru ho raha hai, toh overlap pakka hai bacho!
3. `lastMerged[1] = Math.max(...)` → Last merged interval ka end boundary ko stretch karke extend kar diya.

#### 8. Complete Dry Run:
Input: `[[1,3],[2,6],[8,10],[15,18]]`
*   `intervals` sorted (already sorted by start): `[[1,3],[2,6],[8,10],[15,18]]`.
*   `merged = [[1,3]]`.
*   `i = 1`: `nextInterval = [2,6]`. `nextInterval[0] (2) <= lastMerged[1] (3)`. Overlap! Merge: `lastMerged[1] = Math.max(3, 6) = 6`. `merged` is now `[[1,6]]`.
*   `i = 2`: `nextInterval = [8,10]`. `nextInterval[0] (8) > lastMerged[1] (6)`. No overlap. Push `[8,10]`. `merged` becomes `[[1,6], [8,10]]`.
*   `i = 3`: `nextInterval = [15,18]`. `nextInterval[0] (15) > lastMerged[1] (10)`. No overlap. Push `[15,18]`. `merged` becomes `[[1,6], [8,10], [15,18]]`.
*   **Returns:** `[[1,6], [8,10], [15,18]]`. Correct! ✅

#### 9. Complexity:
*   **Time Complexity:** **O(N log N)** strictly dominated by the sorting step.
*   **Auxiliary Space Complexity:** **O(1)** if we ignore the storage space used for sorting recursion stack.

---

### PROBLEM 106: INSERT INTERVAL (LEETCODE 57)

#### 1. Problem Statement:
*You are given an array of non-overlapping intervals `intervals` sorted by their start times. Insert a `newInterval` into the intervals such that the list is still sorted and has no overlapping intervals.*

#### 2. Examples:
*   `intervals = [[1,3],[6,9]]`, `newInterval = [2,5]` → Output: `[[1,5],[6,9]]`

#### 3. Think 🎙️:
> *"Maan lo list already sorted aur non-overlapping hai bacho. Humein ek naya interval insert karna hai aur merge clean rakhna hai.
> 
> **SDE Observation:** Hum pure array ko teen segments mein break kar sakte hain:
> 1. **Left Segment:** Jo intervals `newInterval` ke start hone se pehle hi complete khatam ho chuke hain (`end < newInterval.start`). Unhe direct copy kar lo bacho!
> 2. **Overlapping Segment:** Jo intervals `newInterval` ke sath collide ho rahe hain. Unhe merge karke ek master `newInterval` generate karenge:
>    `newInterval.start = Math.min(newInterval.start, curr.start)`
>    `newInterval.end = Math.max(newInterval.end, curr.end)`
> 3. **Right Segment:** Jo intervals `newInterval` ke complete hone ke baad shuru hote hain (`start > newInterval.end`). Unhe bhi direct copy kar lo!"*

#### 4. JavaScript Code:
```javascript
function insertInterval(intervals, newInterval) {
    const result = [];
    let i = 0;
    const n = intervals.length;

    // Step 1: Add all intervals that end before newInterval starts
    while (i < n && intervals[i][1] < newInterval[0]) {
        result.push(intervals[i]);
        i++;
    }

    // Step 2: Merge all overlapping intervals with newInterval
    while (i < n && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push(newInterval); // Push the merged master interval

    // Step 3: Add remaining intervals that start after newInterval ends
    while (i < n) {
        result.push(intervals[i]);
        i++;
    }

    return result;
}
```

*   **Complexity:** Time: **O(N)** single pass traversal, Space: **O(1)** auxiliary.
*   **What to notice next time:** Unnecessary sorting cost O(N log N) can be bypassed if the input is already sorted!

---

### PROBLEM 107: NON-OVERLAPPING INTERVALS (LEETCODE 435)

#### 1. Problem Statement:
*Given an array of intervals `intervals`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.*

#### 2. Think 🎙️:
> *"Overlaps remove karne hain bacho. Minimal removals chahiye.
> 
> **SDE Observation:** Minimum removals is exactly identical to finding the **Maximum Number of Non-overlapping Intervals** we can pack! This is the classic **Interval Scheduling/Activity Selection Problem** bacho!
> 
> Isme greedy rule kya hota hai? **Humesha us interval ko pehle pick karo jo sabse pehle khatam (end) ho raha ho!** Kyunki jo jaldi end hoga, woh aage aane wale intervals ke liye maximum space bachaega bacho! Sort by end times!"*

#### 3. JavaScript Code:
```javascript
function eraseOverlapIntervals(intervals) {
    if (intervals.length === 0) return 0;

    // Step 1: Sort intervals based on their END times (index 1)
    intervals.sort((a, b) => a[1] - b[1]);

    let nonOverlapCount = 1; // At least first interval is non-overlapping
    let lastEnd = intervals[0][1];

    for (let i = 1; i < intervals.length; i++) {
        // If current interval starts after or at the end of last valid interval
        if (intervals[i][0] >= lastEnd) {
            nonOverlapCount++;
            lastEnd = intervals[i][1]; // Update last ending time
        }
    }

    // Removals = Total Intervals - Max non-overlapping intervals possible
    return intervals.length - nonOverlapCount;
}
```

*   **Complexity:** Time: **O(N log N)**, Space: **O(1)**.

---

### PROBLEM 108: MINIMUM NUMBER OF ARROWS TO BURST BALLOONS (LEETCODE 452)
Same concept as Problem 107! Balloons represent intervals. Sorting by end times and taking the intersection boundary tells us where to shoot the common arrow greedily.

---

### PROBLEM 109: JUMP GAME (LEETCODE 55)

#### 1. Problem Statement:
*You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position. Return `true` if you can reach the last index, or `false` otherwise.*

#### 2. Think 🎙️:
> *"Array positions par jumps track karni hain bacho.
> 
> **Brute force:** Backtracking se saare possible jump coordinates explore karein (O(2^N)).
>
> **SDE Greedy Hack:** Hum backward-traverse kar sakte hain! 
> Maan lo humara target `goal` last index `N - 1` hai bacho. 
> Agar kisi index `i` par khade hokar hum `i + nums[i] >= goal` clear jump kar sakte hain, toh iska matlab ab humara naya dynamic `goal` shifted hokar `i` ban jana chahiye bacho! 
> Agar end mein humara `goal` index `0` tak pahunch jaye, toh iska matlab rasta completely valid hai!"*

#### 3. JavaScript Code:
```javascript
function canJump(nums) {
    let goal = nums.length - 1; // Start goal at the absolute end

    // Backtrack from right to left greedily
    for (let i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= goal) {
            goal = i; // If we can reach the current goal, pull the goal post closer!
        }
    }

    return goal === 0; // Did we reach the start?
}
```

*   **Complexity:** Time: **O(N)** single pass, Space: **O(1)** auxiliary.

---

### PROBLEM 110: GAS STATION (LEETCODE 134)
Greedy validation. Track `totalGas` and `currentGas`. If `currentGas < 0`, start index must shift to `i + 1` and `currentGas` is reset to 0 bacho!

---

## SECTION 14: TRIES (PREFIX TREES) (PROBLEMS 111 - 113)

Trie ek specialized multi-way tree structure hota hai bacho, jo string search queries aur prefixes matching ko highly performant banata hai. Google maps ke auto-complete bar aur dictionaries mein iska exclusive use hota hai!

---

### PROBLEM 111: IMPLEMENT TRIE (PREFIX TREE) (LEETCODE 208)

#### 1. Problem Statement:
*A Trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. Implement the Trie class with `insert`, `search`, and `startsWith` methods.*

#### 2. Think 🎙️:
> *"Trie build karna hai bacho.
> 
> **Observation:** Trie ke har node ke paas characters connections mapping (usually 26 alphabets limits) aur ek boolean variable `isEndOfWord` hona chahiye, jo bataye ki kya yahan koi complete word finish hota hai ya nahi!
> 
> JavaScript object-based mapping are extremely handy for Trie nodes."*

#### 3. JavaScript Code (Clean Object Model):
```javascript
class TrieNode {
    constructor() {
        this.children = {}; // Map of char -> TrieNode
        this.isEndOfWord = false; // True if node represents end of a valid word
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    // Inserts a word into the trie
    insert(word) {
        let current = this.root;
        for (let char of word) {
            if (!current.children[char]) {
                current.children[char] = new TrieNode(); // Create node if doesn't exist
            }
            current = current.children[char]; // Move pointer
        }
        current.isEndOfWord = true; // Mark end of word
    }

    // Returns true if the word is in the trie
    search(word) {
        let current = this.root;
        for (let char of word) {
            if (!current.children[char]) {
                return false; // Character link missing
            }
            current = current.children[char]; // Move pointer
        }
        return current.isEndOfWord; // Word exists only if end mark is true
    }

    // Returns true if there is any word in the trie that starts with prefix
    startsWith(prefix) {
        let current = this.root;
        for (let char of prefix) {
            if (!current.children[char]) {
                return false;
            }
            current = current.children[char]; // Move pointer
        }
        return true; // Reached end of prefix safely!
    }
}
```

*   **Complexity:** Time: **O(L)** for all operations where L is word length, Space: **O(N × L)** where N is total words inserted.
*   **What to notice next time:** Trie drops search complexity from O(N × L) to strictly word length O(L)!

---

### PROBLEM 112: DESIGN ADD AND SEARCH WORDS DATA STRUCTURE (LEETCODE 211)
Same Trie structure, but if character is `'.'`, it represents a wildcard bacho! We recursively perform DFS on all active child branches to find matching sub-words.

---

### PROBLEM 113: WORD SEARCH II (LEETCODE 212) (HARD)
Instead of matching individual strings repeatedly in LeetCode 79 (O(N × 4^L)), we insert all target words into a Trie! Then, we run DFS on the grid, exploring Trie branches concurrently. This massive optimization is a favorite of Google interviewers!

---

## SECTION 15: ADVANCED GRAPH Shortest Paths (PROBLEMS 114 - 118)

SDE interviews mein weighted graph problems, network delays, aur routing mechanisms advanced algorithms demand karte hain. In teen main shortest path strategies ko whiteboard par dhyan se samajh lo bacho:

```
                            GRAPH SHORTEST PATHS
                                     │
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
  Dijkstra's Algorithm        Bellman-Ford                Floyd-Warshall
  - Single Source SP.         - Single Source SP.         - All Pairs SP.
  - Greedy choice.            - Dynamic Programming.      - Matrix DP transitions.
  - O((V + E) log V).     - O(V * E).            - O(V^3).
  - No Negative Weights!      - Handles Negative Weights! - Handles Negative Weights!
```

---

### PROBLEM 114: DIJKSTRA'S SHORTEST PATH ALGORITHM (DEEP DIVE)

#### 1. Problem Statement:
*Given a weighted graph represented as an adjacency list, and a starting source vertex, find the shortest path distances from source to all other vertices. Return a map/array of shortest distances.*

#### 2. Think 🎙️:
> *"Weighted Graph single source shortest path bacho. Negative edges nahi hain.
> 
> **The Greedy Relaxation Rule:**
> 1. We set all initial distances to `Infinity` and starting source node to `0`.
> 2. We maintain a **Priority Queue** of vertices based on tentative distance.
> 3. At each step, we poll the closest vertex `current` from the queue.
> 4. For each unvisited neighbor of `current`, we calculate the alternative path cost:
>    `distanceThroughCurrent = distance[current] + edgeWeight`.
> 5. If this new cost is strictly smaller than pichli recorded distance, we relaxation update the distance map and push the neighbor back into queue!"*

#### 3. JavaScript Code (Clean Priority Queue Emulated Dijkstra):
```javascript
function dijkstra(graph, source, numVertices) {
    const distances = new Array(numVertices).fill(Infinity); //
    distances[source] = 0; //

    // Priority Queue emulated via simple array sorting (can be optimized to heap!)
    const pQueue = [];
    pQueue.push({ node: source, dist: 0 }); //

    while (pQueue.length > 0) {
        // Sort greedily to always extract the closest node
        pQueue.sort((a, b) => a.dist - b.dist); //
        const { node, dist } = pQueue.shift(); // Poll

        // Skip outdated path entries
        if (dist > distances[node]) continue;

        // Iterate through all neighbors from adjacency list
        const neighbors = graph[node] || [];
        for (let neighbor of neighbors) {
            const neighborNode = neighbor.to;
            const edgeWeight = neighbor.weight; //

            // Relaxation Step
            if (distances[node] + edgeWeight < distances[neighborNode]) {
                distances[neighborNode] = distances[node] + edgeWeight; //
                pQueue.push({ node: neighborNode, dist: distances[neighborNode] }); // Enqueue
            }
        }
    }

    return distances;
}
```

*   **Complexity:** Time: **O((V + E) log V)** when implemented with binary heap priority queues, Space: **O(V + E)**.

---

### PROBLEM 115: BELLMAN-FORD SHORTEST PATH ALGORITHM

#### 1. Problem Statement:
*Same single-source shortest path, but edges can contain negative weights. Detect negative cycles if exist.*

#### 2. Think 🎙️:
> *"Agar graph mein negative weights hain, toh Dijkstra fail ho jayega! Hum Bellman-Ford algorithm use karenge.
> 
> **Transition rules:** Maan lo graph has `V` vertices. Any shortest path can contain at most `V - 1` edges.
> Iska matlab, agar hum saari edges ko strictly `V - 1` times relax kar dein, toh humein accurate shortest path distances mil jayengi bacho!
> 
> Aur `V`th iteration par agar fir se koi distance value reduce ho jaye, toh iska matlab graph mein ek negative cycle exist karta hai!"*

#### 3. JavaScript Code:
```javascript
function bellmanFord(edges, numVertices, source) {
    const distances = new Array(numVertices).fill(Infinity);
    distances[source] = 0;

    // Relax all edges V - 1 times
    for (let i = 1; i <= numVertices - 1; i++) {
        for (let [u, v, weight] of edges) {
            if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
                distances[v] = distances[u] + weight; // Relax Edge
            }
        }
    }

    // V-th iteration to check for negative cycles
    for (let [u, v, weight] of edges) {
        if (distances[u] !== Infinity && distances[u] + weight < distances[v]) {
            throw new Error("Negative cycle detected!"); // Distance keeps decreasing infinitely!
        }
    }

    return distances;
}
```

*   **Complexity:** Time: **O(V · E)**, Space: **O(V)**.

---

### PROBLEM 116: FLOYD-WARSHALL ALL-PAIRS SHORTEST PATH ALGORITHM

#### 1. Problem Statement:
*Find the shortest paths between all pairs of vertices in a weighted graph.*

#### 2. Think 🎙️:
> *"All Pairs Shortest Paths bacho!
> 
> We construct a 2D matrix `dist`. The core dynamic formula states:
> For every vertex `k` chosen as an intermediate midpoint candidate, and for any pair `i -> j`:
> \\[ dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]) \\]
> This elegant triple nested loop runs in O(V^3)!"*

#### 3. JavaScript Code:
```javascript
function floydWarshall(matrix, numVertices) {
    const dist = Array.from({ length: numVertices }, (_, i) => [...matrix[i]]); //

    for (let k = 0; k < numVertices; k++) {
        for (let i = 0; i < numVertices; i++) {
            for (let j = 0; j < numVertices; j++) {
                if (dist[i][k] !== Infinity && dist[k][j] !== Infinity) {
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]); // Transition
                }
            }
        }
    }

    return dist;
}
```

*   **Complexity:** Time: **O(V^3)**, Space: **O(V^2)** matrix.

---

### PROBLEM 117: KRUSKAL'S MINIMUM SPANNING TREE ALGORITHM
We sort all edges based on weight. We greedily pick the smallest edge and add to MST only if it doesn't form a cycle, which we verify in O(1) using Union-Find (DSU)!

---

### PROBLEM 118: NETWORK DELAY TIME (LEETCODE 743)
Weighted directed network delays. Standard application of Dijkstra's algorithm starting from source node `K`. The answer is the maximum value in the final `distances` array bacho!

---

## MIXED UNSEEN SET 5 (PROBLEMS 119 - 123)

**Arey bacho! Ab is section mein koi pre-defined header ya pattern leakage nahi hoga. Sawaal ko constraints ke sath parse karo!**

---

### PROBLEM 119: RANGE SUM QUERY - MUTABLE (LEETCODE 307)

#### 1. Problem Statement:
*Given an integer array `nums`, handle multiple queries of two types: Update the value of an element at index `index`. Calculate the sum of elements of `nums` between indices `left` and `right` inclusive.*

#### 2. Think 🎙️:
> *"Dynamic values update aur prefix sums calculation! 
> 
> SDE Pattern: **Binary Indexed Tree (BIT) / Fenwick Tree**.
> 
> standard arrays take O(1) to update but O(N) to sum. Prefix arrays take O(1) to sum but O(N) to update. 
> 
> BIT balances both operations in strictly **O(log N)**! We use bitwise offset indices transitions: `idx += (idx & -idx)` to update, and `idx -= (idx & -idx)` to query prefix sums bacho!"*

#### 3. JavaScript Code (Clean High-Performance BIT):
```javascript
class NumArray {
    constructor(nums) {
        this.nums = nums;
        this.n = nums.length;
        this.tree = new Array(this.n + 1).fill(0); // 1-indexed BIT
        
        for (let i = 0; i < this.n; i++) {
            this.init(i + 1, nums[i]);
        }
    }

    init(i, val) {
        while (i <= this.n) {
            this.tree[i] += val;
            i += (i & -i); // Add least significant bit
        }
    }

    update(index, val) {
        const delta = val - this.nums[index];
        this.nums[index] = val;
        this.init(index + 1, delta); // Update BIT entries
    }

    query(i) {
        let sum = 0;
        while (i > 0) {
            sum += this.tree[i];
            i -= (i & -i); // Subtract least significant bit
        }
        return sum;
    }

    sumRange(left, right) {
        return this.query(right + 1) - this.query(left); // Prefix sum difference
    }
}
```

*   **Complexity:** Time: **O(log N)** for update/query, Space: **O(N)** tree memory.

---

### PROBLEM 120: COURSE SCHEDULE II (LEETCODE 210)
Directed topological sorting. We construct Kahn's BFS queue of in-degrees. If final sorted course list size !== total courses, cycle exists, return `[]`.

---

### PROBLEM 121: MEETING ROOMS II (LEETCODE 253)
Find minimum meeting rooms required. Extract start and end times into two separate arrays. Sort both. Use Two Pointers to increment count at overlap and slide pointers.

---

### PROBLEM 122: LONGEST PALINDROMIC SUBSTRING (LEETCODE 5)
Expand Around Center technique. Pivot at index `i` (odd check) and `i, i+1` (even check) to find the longest symmetric palindrome boundary in O(N^2) time.

---

### PROBLEM 123: CONTAINER WITH MOST WATER (LEETCODE 11)
Two Pointer convergence. `left` at start, `right` at end. At each step, calculate area `(right - left) * Math.min(h[left], h[right])` and shrink pointer of the smaller height bacho!

---

## SECTION SUMMARY & SDE BLUEPRINTS 📊

Bacho, is summary block ko register par dhyan se update karo:

| SDE Patterns | Core Structural Philosophy | Candidate Bug Traps ⚠️ |
| :--- | :--- | :--- |
| **BIT / Fenwick** | Update and Query prefix sum elements balanced in O(log N). | Forget 1-indexed conversion logic. |
| **Dijkstra SP** | Greedily relax paths distances using Min-Heaps priority. | Using on negative weights edge sets. |
| **Greedy Intervals** | Sort by end times to secure maximum non-overlapping bounds. | Sorting by start time when end is required. |

---

## SDE MASTERCLASS PROGRESS TARGETS 🗺️

*   **Current Progress:** 123 / 150+ Problems completed!
*   **Concepts Covered:** Greedy scheduling, Prefix tree searches, Relaxation updates, Negative weight cycles, All-pairs SP matrix, BIT prefix sum logs.

---

## SECTION 16: SEGMENT TREES & RANGE QUERIES (PROBLEMS 124 - 128)

Bacho, jab kisi array par range queries (jaise range sum, range minimum, range XOR) aur updates dono parallelly O(log N) mein karne hon, aur Segment updates arbitrary intervals par ho rahe hon, toh humara ultimate weapon hota hai **Segment Tree**.

---

### PROBLEM 124: RANGE MINIMUM QUERY (LEETCODE 307 VARIANT)

#### 1. Problem Statement:
*Given an integer array `nums`, implement a Segment Tree to handle Range Minimum Queries (RMQ) and point updates in O(log N) time.*

#### 2. Examples:
*   `nums =` → `query(1, 4)` (indices 1 to 4: ``) returns `1`. Update index 2 to `10` → `query(1, 4)` returns `4`.

#### 3. Constraints:
*   1 <= nums.length <= 10^5
*   -10^9 <= nums[i] <= 10^9

#### 4. Think (Candidate Thinking Aloud) 🎙️:
> *"Bacho, dhyan se socho. Point updates aur range queries dynamic hain. 
> - Brute force: Update O(1), Query linear scan O(N). 
> - Prefix arrays: Query O(1), par update linear shift O(N) lega. 
> - Binary Indexed Tree (BIT) range minimum par direct apply nahi hota bacho, kyunki minimum function associative toh hai par invertible nahi hai (unlike sum where we can do prefix subtraction)!
> 
> **Bottleneck:** Humein overlapping intervals ko divide-and-conquer strategy se query karna hoga.
>
> **Pattern Recognition:** Segment Tree! Hum pure array range `[0, N-1]` ko half-half split karke tree ke nodes mein range values segment divide karenge. Leaf nodes elements ko represent karengi, aur parent node left aur right child ka minimum hold karega bacho!"*

#### 5. Progressive Hints:
*   **Hint 1 (Observation):** Segment Tree ka size worst-case mein 4N hota hai complete binary tree properties ke chalte.
*   **Hint 2 (Direction):** Recursive functions define karo: `build(node, start, end)`, `update(node, start, end, idx, val)`, aur `query(node, start, end, l, r)`.
*   **Hint 3 (Pattern/DS):** Segment Tree segment range representation.

#### 6. Brute Force:
Simple point updates and linear array scanning for range minimum. Time: O(N) query, Space: O(1).

#### 7. Bottleneck of Brute Force:
Re-scanning unchanged array elements during queries.

#### 8. Optimal Observation:
Any interval query can be decomposed into at most 2 log N pre-calculated canonical intervals of the Segment Tree.

#### 9. Selected Pattern & Data Structure:
*   **Pattern:** Range Partitioning Tree.
*   **Data Structure:** Array representing Segment Tree of size 4N.

#### 10. Optimal Approach:
Implement Segment Tree with standard logarithmic updates and range splits.

#### 11. JavaScript Code:
```javascript
class SegmentTree {
    constructor(nums) {
        this.n = nums.length;
        this.tree = new Array(4 * this.n).fill(Infinity);
        if (this.n > 0) this.build(nums, 0, 0, this.n - 1);
    }

    build(nums, node, start, end) {
        if (start === end) {
            this.tree[node] = nums[start]; // Leaf Node
            return;
        }
        const mid = Math.floor((start + end) / 2); //
        this.build(nums, 2 * node + 1, start, mid); // Left Child
        this.build(nums, 2 * node + 2, mid + 1, end); // Right Child
        this.tree[node] = Math.min(this.tree[2 * node + 1], this.tree[2 * node + 2]);
    }

    update(node, start, end, idx, val) {
        if (start === end) {
            this.tree[node] = val; // Update Leaf Node
            return;
        }
        const mid = Math.floor((start + end) / 2); //
        if (idx <= mid) {
            this.update(2 * node + 1, start, mid, idx, val);
        } else {
            this.update(2 * node + 2, mid + 1, end, idx, val);
        }
        this.tree[node] = Math.min(this.tree[2 * node + 1], this.tree[2 * node + 2]);
    }

    query(node, start, end, l, r) {
        if (r < start || end < l) return Infinity; // No overlap
        if (l <= start && end <= r) return this.tree[node]; // Total overlap

        const mid = Math.floor((start + end) / 2); //
        const leftMin = this.query(2 * node + 1, start, mid, l, r);
        const rightMin = this.query(2 * node + 2, mid + 1, end, l, r);
        return Math.min(leftMin, rightMin);
    }
}
```

#### 12. Line-by-Line Explanation:
1. `this.tree = new Array(4 * this.n)` → Safe allocation boundary size of 4N to avoid out of bounds in recursion.
2. `mid = Math.floor((start + end) / 2)` → Segment divide point.
3. `if (r < start || end < l) return Infinity` → Intersection safety prune to avoid considering out-of-range elements.

#### 13. Complete Dry Run:
Input: `nums =`, Build the tree.
*   `start = 0, end = 3`, `mid = 1`. Left branch ``, Right branch ``.
*   Leaves built: `node 3 = 2` (index 0), `node 4 = 5` (index 1), parent `node 1 = min(2,5) = 2`.
*   Leaves built: `node 5 = 1` (index 2), `node 6 = 4` (index 3), parent `node 2 = min(1,4) = 1`.
*   Root `node 0 = min(node 1, node 2) = min(2, 1) = 1`. Correct!

#### 14. Complexity:
*   **Time Complexity:** Point Update: **O(log N)**, Range Query: **O(log N)**.
*   **Auxiliary Space Complexity:** **O(N)** to store tree representation.

#### 15. Edge Cases:
*   `l === r`: Single element query, safely returns `nums[l]`.

#### 16. Same idea in other problems:
Range Sum Query, Range Maximum Query.

#### 17. Is problem se kya seekhna hai?
Interval splits on associative range operations can be calculated lazily in logarithmic steps using static segment maps!

---

### PROBLEM 125: RANGE SUM QUERY - MUTABLE (LEETCODE 307)
Standard Segment Tree implementation. Instead of tracking the minimum with `Math.min`, parent nodes sum child results: `this.tree[node] = this.tree[left] + this.tree[right]`.

---

### PROBLEM 126: RANGE XOR QUERY
Segment Tree where parent nodes calculate bitwise XOR of child elements: `this.tree[node] = this.tree[left] ^ this.tree[right]`. XOR is associative, making this optimal.

---

### PROBLEM 127: MERGE SORT TREE (HARD)
Segment Tree where each node stores a **sorted array** of elements within its range. Built in O(N log N) time by merging sorted arrays. Enables queries like "find count of elements smaller than X in range `[L, R]`" in O(log^2 N) bacho!

---

### PROBLEM 128: LAZY PROPAGATION (ADVANCED SEGMENT TREE)
For Range Updates (e.g. add V to all elements in range `[L, R]`), standard segment trees take O(N log N). **Lazy Propagation** defers updates to descendant nodes until they are strictly queried, optimizing range updates to **O(log N)** bacho!

---

## SECTION 17: ADVANCED STRING MATCHING (PROBLEMS 129 - 134)

Advanced String algorithms solve pattern search and substring parsing in pure linear time, bypassing the standard quadratic string-matching checks.

---

### PROBLEM 129: KNUTH-MORRIS-PRATT (KMP) SUBSTRING SEARCH (DEEP DIVE)

#### 1. Problem Statement:
*Given two strings `text` and `pattern`, find the index of the first occurrence of `pattern` in `text` using Knuth-Morris-Pratt (KMP) linear-time algorithm.*

#### 2. Examples:
*   `text = "abxabcabcaby"`, `pattern = "abcaby"` → Output: `6`

#### 3. Think 🎙️:
> *"Sawaal linear-time string search ka hai bacho! Brute force do loops se O(H × N) leta hai.
> 
> **Bottleneck:** Agar mismatch occur ho jaye, toh brute force window shift karke index 0 se rematching shuru karta hai, which is very slow!
>
> **SDE Observation:** Hum pattern ko preprocess karke ek **LPS (Longest Prefix which is also Suffix) Array** build kar sakte hain! LPS array batata hai ki mismatch hone par humein pattern mein kis position par jump karna hai, bina `text` pointer ko piche laye. Linear scan strictly maintained bacho!"*

#### 4. JavaScript Code:
```javascript
function KMP(text, pattern) {
    if (pattern.length === 0) return 0;
    const lps = buildLPS(pattern);
    let i = 0; // Text pointer
    let j = 0; // Pattern pointer

    while (i < text.length) {
        if (text[i] === pattern[j]) {
            i++;
            j++;
        }
        if (j === pattern.length) {
            return i - j; // Match found!
        } else if (i < text.length && text[i] !== pattern[j]) {
            // Mismatch after j matches
            if (j !== 0) {
                j = lps[j - 1]; // Shift pattern index using LPS
            } else {
                i++;
            }
        }
    }
    return -1;
}

function buildLPS(pattern) {
    const lps = new Array(pattern.length).fill(0);
    let len = 0; // Length of previous longest prefix suffix
    let i = 1;

    while (i < pattern.length) {
        if (pattern[i] === pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len !== 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    return lps;
}
```

#### 5. Complexity:
*   **Time Complexity:** **O(H + N)** where H is text length and N is pattern length.
*   **Auxiliary Space Complexity:** **O(N)** to store the LPS array.

#### 6. Is problem se kya seekhna hai?
String matching checks can bypass redundant comparisons by analyzing self-similarity of the pattern beforehand!

---

### PROBLEM 130: RABIN-KARP STRING MATCHING
Uses **Rolling Hash** technique. We calculate a hash of size M pattern and slide the hash window over `text` in O(1) on each transition bacho! Resolves multiple matching queries in O(N + M) average case.

---

### PROBLEM 131: REPEATED SUBSTRING PATTERN (LEETCODE 459)
If a string `S` is made of repeated substrings, then concatenating `S + S` and removing the first and last characters will still contain `S` bacho! Verify via `(S + S).slice(1, -1).includes(S)`.

---

### PROBLEM 132: ROTATE STRING (LEETCODE 796)
Check if string `A` can become `B` after shifts. Simply check if `B` is a substring of `A + A` and `A.length === B.length` bacho!

---

### PROBLEM 133: LONGEST HAPPY PREFIX (LEETCODE 1392) (HARD)
Use the LPS array of KMP. The longest prefix which is also a suffix of the entire string is simply `S.slice(0, lps[S.length - 1])` bacho!

---

### PROBLEM 134: Z-ALGORITHM STRING SEARCH
The Z-array at index `i` stores the length of the longest common prefix of `S` and `S.slice(i)`. Helps find all occurrences of pattern in text in **O(N + M)** time bacho!

---

## SECTION 18: HARD DECISION SPACE & BACKTRACKING (PROBLEMS 135 - 140)

Backtracking on extremely constrained multi-dimensional spaces requires aggressive branch pruning to avoid TLE bacho.

---

### PROBLEM 135: SUDOKU SOLVER (LEETCODE 37) (DEEP DIVE)

#### 1. Problem Statement:
*Write a program to solve a Sudoku puzzle by filling the empty cells with digits 1-9. Solved in-place.*

#### 2. Think 🎙️:
> *"Sawaal board solver ka hai bacho! Sudoku board 9 × 9 size ka hai.
> 
> **Brute force:** Har khali cell par 1 se 9 tak saare digits explore karo recursively, taking exponential combinations.
>
> **SDE Pruning Observation:** Agar hum kisi cell par value check kar rahe hain, toh place karne se pehle verify karo ki kya woh value us row, us column, aur us local 3 × 3 subgrid mein already present toh nahi hai!
> Agar valid cell value mil jaye, toh place karke recursion forward badhao. Agar aage ki branches fail ho jayein, toh instantly path rollback (backtrack) kar do!"*

#### 3. JavaScript Code:
```javascript
function solveSudoku(board) {
    solve(board);
}

function solve(board) {
    for (let r = 0; r < 9; r++) {
        for (let c = 0; c < 9; c++) {
            if (board[r][c] === '.') {
                for (let num = 1; num <= 9; num++) {
                    const charNum = num.toString();
                    if (isValid(board, r, c, charNum)) {
                        board[r][c] = charNum; // Try placing number

                        if (solve(board)) return true; // Path found!

                        board[r][c] = '.'; // Backtrack
                    }
                }
                return false; // Triggers backtracking to pichla valid state
            }
        }
    }
    return true; // Puzzle completely solved!
}

function isValid(board, row, col, char) {
    for (let i = 0; i < 9; i++) {
        if (board[row][i] === char) return false; // Row repeat check
        if (board[i][col] === char) return false; // Column repeat check
        
        // 3x3 grid repeat check
        const boxRow = 3 * Math.floor(row / 3) + Math.floor(i / 3); //
        const boxCol = 3 * Math.floor(col / 3) + (i % 3); //
        if (board[boxRow][boxCol] === char) return false;
    }
    return true;
}
```

#### 4. Complexity:
*   **Time Complexity:** **O(9^{81})** worst-case theoretical upper bound, but pruned backtracking paths reduce actual operations drastically.
*   **Auxiliary Space Complexity:** **O(1)** auxiliary recursive call stack depths of at most 81 frames.

#### 5. Is problem se kya seekhna hai?
Validating constraints at the earliest step (pruning) saves exponential branches of computation bacho!

---

### PROBLEM 136: PALINDROME PARTITIONING (LEETCODE 131)
Split string into prefix and suffix recursively. If prefix is a valid palindrome, backtrack to check remaining substring combinations.

---

### PROBLEM 137: WORD LADDER II (HARD)
Finds all shortest transformation paths from `beginWord` to `endWord` bacho! Solve using BFS to find shortest path level-mapping, then use DFS Backtracking to trace all paths.

---

### PROBLEM 138: WORD BREAK II (LEETCODE 140)
Generate all valid sentences from word segmentation. Backtrack through dictionary searches. Optimize using prefix-tree (Trie) pruning!

---

### PROBLEM 139: N-QUEENS II (LEETCODE 52) (HARD)
Return total count of distinct solutions instead of board representations. Uses bitwise column, positive, and negative diagonal tracking sets.

---

### PROBLEM 140: COMBINATION SUM III (LEETCODE 216)
Find all combinations of K numbers that sum to N using numbers from 1 to 9 strictly once. Backtrack with depth limit K.

---

## SECTION 19: ADVANCED DP (BITMASK & MULTI-STATE) (PROBLEMS 141 - 146)

Advanced DP transitions solve state selection with multiple active tracking variables.

---

### PROBLEM 141: EDIT DISTANCE (LEETCODE 72) (DEEP DIVE)

#### 1. Problem Statement:
*Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`. Operations allowed: Insert, Delete, or Replace.*

#### 2. Think 🎙️:
> *"Sawaal string distance matching ka hai bacho.
> 
> **Transition rules:** Maan lo `dp[i][j]` is cost to match prefix `word1[0...i-1]` and `word2[0...j-1]`.
> * If character matches (`word1[i-1] === word2[j-1]`): No new operations! `dp[i][j] = dp[i-1][j-1]`.
> * Else mismatch! We have three options:
>   1. Insert: `dp[i][j-1]`
>   2. Delete: `dp[i-1][j]`
>   3. Replace: `dp[i-1][j-1]`
>   Take the minimum and add 1!"*

#### 3. JavaScript Code:
```javascript
function minDistance(word1, word2) {
    const m = word1.length;
    const n = word2.length;
    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

    // Base cases: matching empty strings
    for (let i = 0; i <= m; i++) dp[i] = i; // Delete all chars
    for (let j = 0; j <= n; j++) dp[j] = j; // Insert all chars

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1]; // Match!
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j],    // Delete
                    dp[i][j - 1],    // Insert
                    dp[i - 1][j - 1] // Replace
                );
            }
        }
    }
    return dp[m][n];
}
```

#### 4. Complexity:
*   **Time Complexity:** **O(M × N)**, Space Complexity: **O(M × N)**. *(Space can be optimized to **O(N)** by only storing the previous row state!)*.

#### 5. Is problem se kya seekhna hai?
String alignments matching models can be reduced to 2D state matrices cleanly.

---

### PROBLEM 142: REGULAR EXPRESSION MATCHING (LEETCODE 10) (HARD)
Multi-state DP. Handles wildcard `'.'` and star `'*'`. The star symbol allows matching zero or more occurrences of the preceding character, creating branch transitions.

---

### PROBLEM 143: CHERRY PICKUP (LEETCODE 741)
Two paths tracking DP. We simulate two paths traversing the grid concurrently, optimizing total cherry collections in O(N^3) time.

---

### PROBLEM 144: COIN CHANGE II (LEETCODE 518)
Total number of ways to build sum K. Dynamic 1D Knapsack update: `dp[i] += dp[i - coin]`.

---

### PROBLEM 145: ALIEN DICTIONARY (HARD Topological Sort)
Determine letter order in alien language. Create DAG based on adjacent sorted words mismatches, then apply Topological Sort (Kahn's BFS queue).

---

### PROBLEM 146: UNIQUE PATHS II (LEETCODE 63) (Grid Obstacles)
Unique paths on grid with obstacles. If cell has obstacle `grid[r][c] === 1`, set `dp[c] = 0` to block path bacho!

---

## MIXED UNSEEN SET 6 (PROBLEMS 147 - 152)

**Arey bacho! Yeh is grand masterclass ka absolute final set hai. Pure DSA concepts, pointers aur logic ko use karke questions ko instantly crack karo!**

---

### PROBLEM 147: LONGEST PALINDROMIC SUBSEQUENCE (LEETCODE 516)

#### 📝 Problem Statement:
*Given a string `S`, find the longest palindromic subsequence's length in `S`.*

#### 🚨 How to Think 🎙️:
> *"Longest Palindromic Subsequence bacho! 
> 
> **The SDE Invariant Hack:** 
> Palindrome kya hota hai jo forwards aur backwards identical padha jaye. Iska matlab, agar hum string `S` ko reverse karke ek naya string `R = reverse(S)` bana lein...
> Toh `S` and `R` ka **Longest Common Subsequence (LCS)** hi strictly humara Longest Palindromic Subsequence hoga!
> Same LCS DP function call instantly cracks the problem!"*

```javascript
function longestPalindromeSubseq(s) {
    const rev = s.split('').reverse().join(''); //
    return longestCommonSubsequence(s, rev); // Call standard LCS function
}
```

*   **Complexity:** Time: **O(N^2)**, Space Complexity: **O(N)**.

---

### PROBLEM 148: SUBSET SUM EQUALS K (DP Version)
Check if subset sum exists. Initialize boolean array `dp = true`. For each number, sweep array backwards to set `dp[i] = dp[i] || dp[i - num]`.

---

### PROBLEM 149: COMBINATION SUM IV (LEETCODE 377)
Ordering matters (permutations sum). Initialize 1D array. Transition: `dp[i] += dp[i - num]` for all elements on each step.

---

### PROBLEM 150: WORD LADDER (LEETCODE 127) (BFS Word transitions)
Find shortest mutation length. Perform BFS starting from `beginWord`. Use character loop wildcard checks `(a-z)` to locate adjacent dictionary words in O(26 × L × N) bacho!

---

### PROBLEM 151: REMOVE OVERLAPPING STRINGS (KMP LPS usage)
Check if string `A` is suffix/prefix of `B`. Apply KMP preprocessing on composite string `A + '#' + B` to locate overlapping length bacho!

---

### PROBLEM 152: THE COIN CAP BALANCE (GREEDY COINS MATCH)
If coins denominations are multiples of pichla coins (e.g. 1, 5, 10, 20), greedy strategy of picking largest coin first is mathematically guaranteed to remain optimal!

---

## 🏁 SDE GRAND FINALE SUMMARY & BLUEPRINTS 🏁

Bacho, is milestone summary sheet ko apne register ke first page par note kar lo:

| Algorithmic Paradigms | Core Problem-Solving Intuition | SDE Level Big-O Targets |
| :--- | :--- | :--- |
| **Segment Trees** | Split arrays into range nodes to compute queries in O(log N). | O(N) Build, O(log N) Query. |
| **LPS Array (KMP)** | Shift pattern matches based on pattern self-similarity LPS checks. | O(H + N) Matching time. |
| **Backtrack + Pruning** | Exit invalid state branches early via constraint validations. | O(N!) worst-case bounds. |

---

## 🎉 CONGRATULATIONS BACHO! 152+ PROBLEMS MASTERED 🎉

Humne zero level logic building se shuru karke, linear collections, monotonic stacks, BST traversals, heaps operations, disjoint sets, Dijkstra networks, 2D DP transformations, segment tree queries, aur advanced matching structures ke **152 high-value, product-based MNC level problems** ko unke deep SDE intuition ke sath solve kar liya hai!

Ab interviewers koi bhi unseen problem pooch le bacho, constraints se bottleneck analyze karo, brute force se start karo, aur data structure properties use karke answer derive kardo! 🚀

---
