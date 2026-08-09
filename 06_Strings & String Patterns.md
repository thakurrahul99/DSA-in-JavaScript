**Arey bacho! Sab log fatafat apni seats par baith jao aur whiteboard par apna dhyan lagao!** 

Pichle chapter mein humne **Matrix & 2D Arrays (Chapter 5)** ke complex traversals aur in-place rotations ko bohot hi dhasu tarike se seekha tha. 

Aaj hum DSA ke ek aise data structure ko shuru karne ja rahe hain jiske bina duniya ka koi bhi technical interview complete hi nahi hota—**Strings & String Patterns**! 

Chaho tum Amazon, Microsoft, ya kisi startup ke interview mein baithe ho, string manipulation aur pattern matching par sawaal guaranteed poocha jata hai. Lekin bacho, maximum log strings ke linear array rules ko blindly strings par apply karne ki galti karte hain, aur wahin par unka program fail ho jata hai. 

Aaj hum bilkul zero se shuru karenge aur strings ke un structural secrets ko kholenge jo aapko ek normal coder se pro software engineer banayenge. Copy aur pen taiyar rakho, shuru karte hain **Chapter 6: Strings & String Patterns**! 🚀

---

## 1. STRING BASICS: THE DSA PERSPECTIVE

### String Kya Hai?
Asaan bhasha mein bolo toh **String aur kuch nahi, balki characters ka ek sequence ya collection hai** jo kisi text ko represent karne ke kaam aata hai.

```
                     String: "HELLO" (Length = 5)
                     
                     Character:   'H'   'E'   'L'   'L'   'O'
                     Index:        0     1     2     3     4
```

*   **Characters:** String ke har ek single letter, number, space, ya special symbol ko character kehte hain. JavaScript mein alag se koi `character` data type nahi hota; ek single-character string hi character maani jaati hai.
*   **Indexing:** Arrays ki tarah strings mein bhi **0-based indexing** hoti hai. Pehla character index `0` par hota hai, aur aakhri character index `length - 1` par.
*   **Length:** String mein total kitne characters hain, use string ki `length` property se dhoondha jata hai.

---

### JavaScript Strings Ki Immutability (Sabse Bada Interview Concept! ⚠️)
Bacho, is line ko dimaag mein stone-carve kar lo: **JavaScript mein strings strictly IMMUTABLE hoti hain!**

**Immutability** ka matlab hai: *"Ek baar memory mein string create ho gayi, toh tum use kabhi bhi in-place change (modify) nahi kar sakte!"*

#### ❌ The silent failure trap:
Agar tumne ek string banayi aur socha ki main iska pehla character badal doon:
```javascript
let str = "hello";
str = "y"; // Tumne socha str ab "yello" ban gaya!
console.log(str); // Output: "hello" !!!
```
JavaScript mein index assignment (`str = 'y'`) bina kisi error ke silently fail ho jata hai (aur strict mode `'use strict'` mein direct error dega). Kyun? Kyunki string immutable hai. Agar tumhein string mein thoda sa bhi change karna hai, toh tumhein hamesha ek brand new string hi allocate karni padegi.

---

### String Traversal and Character Access
String ke characters ko hum do tarike se access kar sakte hain:
1.  **Square Bracket Notation:** `str[i]` (Modern and clean).
2.  **`charAt(i)` Method:** `str.charAt(i)` (Traditional).

```javascript
let str = "SDE";
console.log(str);       // Output: "D"
console.log(str.charAt(1)); // Output: "D"
```

#### Traversal using loops:
```javascript
let name = "Amit";
// Classic Index-Based Traversal
for (let i = 0; i < name.length; i++) {
    console.log(name[i]); // We need index 'i' for coordinate operations
}

// Modern Value-Based Traversal
for (const char of name) {
    console.log(char); // Simple and highly readable!
}
```

---

### String Comparison (Lexicographical Order)
JavaScript mein jab hum do strings ko compare karte hain (jaise `str1 < str2`), toh comparison unke characters ke **ASCII / Unicode values** ke basis par character-by-character hota hai. Ise **Lexicographical (dictionary) order** kehte hain.

*   `"apple" < "banana"` returns `true` kyunki `'a'` ka ASCII code (`97`) `'b'` ke ASCII code (`98`) se chota hai.
*   `"Apple" < "apple"` returns `true` kyunki uppercase letters ke ASCII codes (`65` se `90`) hamesha lowercase letters (`97` se `122`) se chote hote hain.

For localized, safe comparisons, we use **`str1.localeCompare(str2)`**.

---

## 2. STRING BUILDING AND MODIFICATION PATTERN

Bacho, dhyan se suno. Kyunki strings immutable hain, agar tum ek bade loop mein baar-baar string concatenation (`str += char`) karte ho, toh pata hai kya hota hai?

```javascript
let str = "";
for (let i = 0; i < n; i++) {
    str += "a"; // HAR STEP PAR BRAND NEW STRING ALLOCATE HOTI HAI!
}
```
Har step par purani string copy hoti hai naye block mein. Is loop ki overall complexity **\\(\mathcal{O}(n^2)\\)** ho jati hai! 😱

### The Optimized "String-Building" Pattern:
SDE interviews mein complexity ko linear rakhne ke liye hum humesha **Array Conversion Pattern** use karte hain:

```
                  ┌──────────────────────────────┐
                  │      Immutable String        │
                  └──────────────┬───────────────┘
                                 │  .split('') (O(n) Time & Space)
                                 ▼
                  ┌──────────────────────────────┐
                  │      Mutable Char Array      │
                  └──────────────┬───────────────┘
                                 │  Modify elements in-place (O(1))
                                 ▼
                  ┌──────────────────────────────┐
                  │      Modified Char Array     │
                  └──────────────┬───────────────┘
                                 │  .join('') (O(n) Time)
                                 ▼
                  ┌──────────────────────────────┐
                  │      Brand New String        │
                  └──────────────────────────────┘
```

#### Code example of Array-Based Builder:
```javascript
function replaceFirstChar(str, newChar) {
    // Step 1: Convert to Array
    let charArray = str.split(""); // O(n)
    
    // Step 2: Mutate in-place
    charArray = newChar; // O(1)
    
    // Step 3: Join back to String
    return charArray.join(""); // O(n)
}
```
*   **Time Complexity:** **\\(\mathcal{O}(n)\\)**.
*   **Space Complexity:** **\\(\mathcal{O}(n)\\)** auxiliary space.

---

## 3. IMPORTANT JAVASCRIPT STRING METHODS DISSECTED

DSA aur development ke liye, in built-in methods ke complexities aur behaviors bilkul crystal clear hone chahiye:

| Method | Syntax | Complexity (Time / Space) | Does it mutate? | Mechanism / Under-the-hood behavior |
| :--- | :--- | :--- | :--- | :--- |
| **`charAt`** | `str.charAt(i)` | **\\(\mathcal{O}(1)\\) / \\(\mathcal{O}(1)\\)** | No | Return character at specified index offset. |
| **`includes`** | `str.includes(sub)` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(1)\\)** | No | Linear scan to check membership of substring. |
| **`indexOf`** | `str.indexOf(sub)` | **\\(\mathcal{O}(n \cdot m)\\) / \\(\mathcal{O}(1)\\)** | No | Returns starting index of substring, else `-1`. |
| **`slice`** | `str.slice(start, end)` | **\\(\mathcal{O}(k)\\) / \\(\mathcal{O}(k)\\)** | No | Returns substring from `start` to `end` index (non-inclusive). Supports negative indexes. |
| **`substring`** | `str.substring(s, e)` | **\\(\mathcal{O}(k)\\) / \\(\mathcal{O}(k)\\)** | No | Similar to slice, but swaps arguments if `start > end`. |
| **`split`** | `str.split(delimiter)` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(n)\\)** | No | Splits string into an array of substrings. |
| **`join`** | `arr.join(delimiter)` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(n)\\)** | No | Array method. Joins array elements into a string. |
| **`trim`** | `str.trim()` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(n)\\)** | No | Removes leading & trailing whitespaces. |
| **`toLowerCase`** | `str.toLowerCase()` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(n)\\)** | No | Converts all characters to lowercase. |
| **`toUpperCase`** | `str.toUpperCase()` | **\\(\mathcal{O}(n)\\) / \\(\mathcal{O}(n)\\)** | No | Converts all characters to uppercase. |
| **`concat`** | `str1.concat(str2)` | **\\(\mathcal{O}(n + m)\\) / \\(\mathcal{O}(n + m)\\)** | No | Appends strings together. Same as `+` operator. |

---

## 4. CORE ALGORITHMIC STRING CONCEPTS

Bacho, array patterns ki tarah strings mein bhi teen definitions ko dhang se samajhna zaroori hai:

1.  **Substring (Contiguous segment):** String ka ek continuous slice. Elements ke beech mein gaps allowed nahi hain.
    *   *Example:* For `"ABCD"`, `"BC"` is a valid substring, but `"AD"` is not!
2.  **Subsequence (Ordered non-contiguous):** Characters ka aisa subset jo original string ke index sequence order ko preserve karta hai, par contiguous hona zaroori nahi hai (gaps allowed hain).
    *   *Example:* For `"ABCD"`, `"ACD"` is a valid subsequence.
3.  **Prefix & Suffix Basics:**
    *   **Prefix (Shuruat):** Substring jo index `0` se start ho. `"ABC"` ka prefix: `""`, `"A"`, `"AB"`, `"ABC"`.
    *   **Suffix (Aakhir):** Substring jo final index par end ho. `"ABC"` ka suffix: `""`, `"C"`, `"BC"`, `"ABC"`.

---

## 5. SDE STRATEGY: PATTERN RECOGNITION

Interview room mein baithe ho aur string ka question saamne aaya. Kaunsa brahmastra nikaloge? Is criteria tree ko dekho:

```
                            STRING PROBLEM CRITERIA
                                       │
           ┌───────────────────────────┴───────────────────────────┐
     Lookup / Counts                                        Spans / Boundaries
           │                                                       │
           ▼                                                       ▼
    Order Matters?                                        Contiguous Segment?
     ├─ Yes ==> Two Pointers (Start/End)                   ├─ Yes ==> Sliding Window
     └─ No  ==> Frequency Counter (Array/Map)              └─ No  ==> Subsequence (DP)
```

1.  **Frequency Counting Pattern \\(\rightarrow\\) Think Hashing / Static ASCII Array:**
    *   *Clues:* "Anagrams", "character counts", "rearrangements", "frequencies comparisons".
2.  **Two Pointers Pattern \\(\rightarrow\\) Think Opposite Direction Convergence:**
    *   *Clues:* "Palindrome checking", "reversing characters", "checking boundary symmetries".
3.  **Sliding Window Pattern \\(\rightarrow\\) Think Dynamic Boundaries:**
    *   *Clues:* "Longest substring with...", "minimum window substring containing...", "longest continuous unique character span".
4.  **Hashing Pattern (Rolling Hash) \\(\rightarrow\\) Think Rabin-Karp / Pattern Matching:**
    *   *Clues:* "Find substring pattern in a huge text stream in constant time".

---

## 6. PROGRESSIVE CLASSROOM PRACTICE

🚀 **Chalo dosto! Ab in teen progressive problems ko dhyan se trace karo. Pehle approach khud dimaag mein dhoondho, phir solution padhna!**

---

### Problem 1 (Easy): Valid Palindrome (LeetCode 125)
*Given a string `s`, return `true` if it is a palindrome, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters.*

#### 🧠 Step-by-Step Logic Building:
*   **Understand:** Palindrome ka matlab hai ki string ko aage se padho ya peeche se, wo bilkul same honi chahiye.
*   **Example:** `"A man, a plan, a canal: Panama"` \\(\rightarrow\\) Alphanumeric conversion ke baad: `"amanaplanacanalpanama"`. Peeche se reverse karo toh bhi `"amanaplanacanalpanama"`. Output: `true`.
*   **Brute Force:** Pure alphanumeric string ko filter karo, use reverse karke ek naye string variable mein store karo (`filtered.split('').reverse().join('')`) aur compare karo.
    *   *Bottleneck:* Reversing process humesha \\(\mathcal{O}(n)\\) extra space aur copy computations leta hai.
*   **Optimal Approach (Two Pointers - Opposite Ends):**
    Filter karne ke baad, do pointers betha do: `left = 0` aur `right = len - 1`. Dono directions se elements ko compare karte huye inward move karo. Agar kisi bhi step par match fail ho, toh instantly return `false`.

```javascript
function isPalindrome(s) {
    // Step 1: Clean string using regex (O(n) time, O(n) space)
    const cleanStr = s.toLowerCase().replace(/[^a-z0-9]/g, "");
    
    let left = 0;
    let right = cleanStr.length - 1;
    
    // Step 2: Converging Bilateral Scan
    while (left < right) {
        if (cleanStr[left] !== cleanStr[right]) {
            return false; // Symmetry broken!
        }
        left++;
        right--;
    }
    return true;
}
```

#### Dry Run on `"No 'x' in 'Nixon'"`:
*   Alphanumeric Cleaning: `"noxinnixon"`
*   `left = 0` (`'n'`), `right = 9` (`'n'`) \\(\rightarrow\\) Same! `left++`, `right--`
*   `left = 1` (`'o'`), `right = 8` (`'o'`) \\(\rightarrow\\) Same! `left++`, `right--`
*   ... continues smoothly until center is met.
*   Returns `true`. Correct!

#### Complexity:
*   **Time Complexity:** **\\(\mathcal{O}(n)\\)** since we scan the cleaned string once.
*   **Space Complexity:** **\\(\mathcal{O}(n)\\)** auxiliary space for storing the cleaned alphanumeric representation.

---

### Problem 2 (Medium): Valid Anagram (LeetCode 242)
*Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.*

#### 🧠 Step-by-Step Logic Building:
*   **Understand:** Anagram ka matlab hai ki kya hum dono strings ke character frequencies ko rearrange karke ek doosre ke bilkul identical bana sakte hain.
*   **Example:** `s = "anagram"`, `t = "nagaram"` \\(\rightarrow\\) Dono mein `'a'` teen baar hai, `'n'` ek baar, `'g'` ek baar, `'r'` ek baar, `'m'` ek baar. Output: `true`.
*   **Brute Force (Sorting):** Dono strings ko split karke alphabetically sort karo aur fir match karo.
    ```javascript
    let sortedS = s.split("").sort().join(""); // O(n log n)
    ```
    *   *Bottleneck:* Sorting takes **\\(\mathcal{O}(n \log n)\\)** time complexity.
*   **Better Approach (Hash Map):** Count character frequencies using a Map. Space: \\(\mathcal{O}(k)\\).
*   **Optimal Approach (Static Frequency Array Optimization 💡):**
    Kyunki question mein characters lowercase English letters (`a-z`) tak hi bound hote hain, hum extra map overheads se bachne ke liye **26-size ka static integer array** use kar sakte hain! ASCII arithmetic se har character ka relative index instantly calculate ho jata hai:
    \\[\text{Index of char} = \text{char.charCodeAt}(0) - \text{`a'.charCodeAt}(0)\\]

```javascript
function isAnagram(s, t) {
    if (s.length !== t.length) return false;
    
    // 26 size static buckets initialized to 0
    const count = new Array(26).fill(0);
    const baseCode = 'a'.charCodeAt(0);
    
    // Single pass frequency tracking
    for (let i = 0; i < s.length; i++) {
        count[s.charCodeAt(i) - baseCode]++; // Increment for string s
        count[t.charCodeAt(i) - baseCode]--; // Decrement for string t
    }
    
    // If all buckets are exactly 0, they are anagrams!
    for (let val of count) {
        if (val !== 0) return false;
    }
    return true;
}
```

#### Dry Run on `s = "rat", t = "car"`:
*   Initial count array: `[0, 0, ..., 0]` (size 26)
*   `i = 0`: `'r'` incremented, `'c'` decremented \\(\rightarrow\\) `count = 1`, `count = -1`
*   `i = 1`: `'a'` incremented, `'a'` decremented \\(\rightarrow\\) `count = 0`
*   `i = 2`: `'t'` incremented, `'r'` decremented \\(\rightarrow\\) `count = 1`, `count = 0`
*   Count validation loop detects non-zero bucket `count = -1` \\(\rightarrow\\) Returns `false`. Correct!

#### Complexity:
*   **Time Complexity:** **\\(\mathcal{O}(n)\\)** linear time scan.
*   **Space Complexity:** **\\(\mathcal{O}(1)\\)** constant space auxiliary because bucket size is strictly fixed at 26!

---

### Problem 3 (Hard): Longest Substring Without Repeating Characters (LeetCode 3)
*Given a string `s`, find the length of the longest substring without repeating characters.*

#### 🧠 Step-by-Step Logic Building:
*   **Understand:** Humein ek aisi longest continuous window (substring) ki length dhoondhni hai jismein saare characters unique hon.
*   **Example:** `"abcabcbb"` \\(\rightarrow\\) Longest unique span `"abc"` hai (length = 3).
*   **Brute Force:** Nested loops chalakar saare possible substrings generate karo, aur har substring ke characters ki uniqueness ko check karo.
    *   *Bottleneck:* Substrings generation ki complexity **\\(\mathcal{O}(n^2)\\)** ya check lagane par **\\(\mathcal{O}(n^3)\\)** ho jati hai.
*   **Optimal Approach (Sliding Window + Set/Map):**
    Hum ek dynamic window maintain karenge jiske boundaries `left` aur `right` pointers se control honge.
    *   Hum `right` pointer ko aage badhakar window ko expand karenge aur characters ko `Set` mein track karenge.
    *   **Shrink Condition:** Agar humein koi aisa char mila jo set mein pehle se present hai (`set.has(char)`), toh hum window ke left character ko remove karenge (`set.delete(s[left])`) aur `left++` karenge jab tak duplicate element remove na ho jaye.
    *   Har step par max length update karenge: `maxLength = max(maxLength, right - left + 1)`.

```javascript
function lengthOfLongestSubstring(s) {
    let left = 0;
    let maxLength = 0;
    const charSet = new Set(); // To track unique character footprints
    
    for (let right = 0; right < s.length; right++) {
        // Shrink window from left until the duplicate element is removed
        while (charSet.has(s[right])) {
            charSet.delete(s[left]);
            left++;
        }
        
        // Add current character to set and record local maximum length
        charSet.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    return maxLength;
}
```

#### Dry Run on `"pwwkew"`:
*   `right = 0` (`'p'`): set = `{'p'}`, `maxLength = max(0, 0-0+1) = 1`
*   `right = 1` (`'w'`): set = `{'p', 'w'}`, `maxLength = max(1, 1-0+1) = 2`
*   `right = 2` (`'w'`): set has `'w'`! 
    *   Shrink loop starts: delete `s` (`'p'`), `left = 1`. set now `{'w'}`.
    *   Set still has `'w'`! delete `s` (`'w'`), `left = 2`. set now `{}`.
    *   Add `'w'`, set = `{'w'}`, `maxLength = max(2, 2-2+1) = 2`
*   `right = 3` (`'k'`): set = `{'w', 'k'}`, `maxLength = max(2, 3-2+1) = 2`
*   `right = 4` (`'e'`): set = `{'w', 'k', 'e'}`, `maxLength = max(2, 4-2+1) = 3`
*   `right = 5` (`'w'`): set has `'w'`! delete `s` (`'w'`), `left = 3`. set now `{'k', 'e'}`. Add `'w'`, set = `{'k', 'e', 'w'}`, `maxLength = max(3, 3) = 3`.
*   Returns `3`. Correct!

#### Complexity:
*   **Time Complexity:** **\\(\mathcal{O}(n)\\)**. (Dono `left` aur `right` pointers string ko maximum ek-ek baar hi traverse karenge).
*   **Space Complexity:** **\\(\mathcal{O}(\min(n, k))\\)** where \\(k\\) is the alphabet size (maximum size of the hash set representation).

---

### Problem 4 (SDE Special): First Unique Character in a String (LeetCode 387)
*Given a string `s`, find the first non-repeating character in it and return its index. If it does not exist, return -1.*

#### 🧠 Step-by-Step Logic Building:
*   **Understand:** Humein string ka wo sabse pehla character dhoondhna hai jiska overall frequency count exactly `1` ho.
*   **Example:** `"leetcode"` \\(\rightarrow\\) `'l'` is index 0, frequency is 1. Output: `0`.
*   **Optimal Approach (Two-Pass Frequency Mapping):**
    1.  *First Pass:* String ko scan karke frequencies count karo (array of size 26 or Map).
    2.  *Second Pass:* String ke indexes ko index `0` se end tak sequentially scan karo, aur frequency lookup karo. Jis character ki frequency pehle `1` milegi, uska index return kar do!

```javascript
function firstUniqChar(s) {
    const frequency = new Array(26).fill(0);
    const baseCode = 'a'.charCodeAt(0);
    
    // Pass 1: Build frequency map
    for (let i = 0; i < s.length; i++) {
        frequency[s.charCodeAt(i) - baseCode]++;
    }
    
    // Pass 2: Linearly find the first element with frequency 1
    for (let i = 0; i < s.length; i++) {
        if (frequency[s.charCodeAt(i) - baseCode] === 1) {
            return i;
        }
    }
    return -1;
}
```
*   **Complexity:** Time: **\\(\mathcal{O}(n)\\)** (two independent linear passes), Space: **\\(\mathcal{O}(1)\\)** auxiliary space.

---

## 7. STRING PATTERN MATCHING BASICS (SDE INTRADAY PREVIEW)

Bacho, jab hum bade string `text` ke andar kisi sub-pattern `pattern` ko dhoondhte hain, toh normal index search se behtar algorithms exist karte hain.

```
Text:     [ A B C D A B C E ]  (Length N)
Pattern:  [ A B C E ]          (Length M)
```

1.  **Naive String Search:**
    Hum text ke har possible index `i` par `pattern` ke characters ko compare karte hain. Agar mismatch ho, toh loop badhakar `i + 1` par alignment check karte hain. Worst case complexity turns to **\\(\mathcal{O}(N \times M)\\)**.
2.  **SDE Level Intuition: Advanced Matching Algorithms (Full detail later in advanced structures):**
    *   **KMP Algorithm (Knuth-Morris-Pratt):** Naive search mein jab shift mismatched hota hai, hum humesha starting characters ko discard kar dete hain. KMP **LPS (Longest Prefix Suffix) array** ke concept se backtracking ko avoid karta hai, aur skip logic se complexity ko linear **\\(\mathcal{O}(N + M)\\)** kar deta hai.
    *   **Rabin-Karp Algorithm:** Yeh **Rolling Hash** ka use karta hai. Har window segment ka mathematical hash generate hota hai. Match hone par hi deep character check hota hai, jisse dynamic lookups average linear time mein solve ho jate hain.

---

## 8. SDE TRAPS & COMMON MISTAKES ⚠️

Interviews mein galti se bhi in bugs ko code mein aane mat dena:

1.  **Direct String Mutations:**
    Writing code like `str[i] = 'X'` and expecting the string to modify in-place. Remember: *Strings are immutable!* Always use split/join or helper strings.
2.  **The Case Sensitivity Trap:**
    Comparing characters directly without normalising them to uppercase/lowercase. `"A" === "a"` is `false`. *Always convert string cases first if input checks allow case-insensitivity!*
3.  **The Unicode Surrogate Pair Oversight (Specialized SDE Bug! 💡):**
    JavaScript strings operate on **16-bit code units (UTF-16)**. Emojis or ancient languages symbols (like `𐌵` or `😂`) require 2 code units (surrogate pairs) to represent.
    If you slice or index them standardly, you will break the character apart!
    ```javascript
    let emojiStr = "😂";
    console.log(emojiStr.length); // Output: 2! (Not 1)
    ```
    To safely iterate over Unicode code points, use **`for...of`** loops or **`Array.from(str)`** which handle surrogate pairs automatically.
4.  **Substring vs Subsequence Confusion:**
    Treating subsequences as continuous segments. Substring is contiguous; Subsequence can have gaps.

---

## CHAPTER END SUMMARY

### Completed Topics:
*   String representations, zero-based indexing offsets, and UTF-16 surrogate pairs.
*   The computational logic behind String Immutability.
*   Optimized Array-Based Builder pattern to replace quadratic concatenation loops.
*   Time/Space Complexities of Slice, Split, Join, Trim, and indexOf methods.
*   Basic pattern matching mechanisms (Naive, Rabin-Karp, and KMP LPS arrays).

### Mastered Patterns:
*   **Opposite end bilateral pointers convergence** for palindrome verification.
*   **Static ASCII frequency array allocation** to skip hash map initialization cost.
*   **Contiguous sliding window set boundaries** for unique character span search.

---

### Masterclass Practice Roadmap:
1.  Try *Valid Palindrome* on LeetCode 125.
2.  Solve *Valid Anagram* using the static `new Array(26)` count technique.
3.  Complete *Longest Substring Without Repeating Characters* on LeetCode 3.

---

