# 🪟 **SLIDING WINDOW: DETAILED GUIDE (JAVA & GO)**

**The Most Efficient Technique for Substring and Subarray Problems**

*Prerequisites: Complete arrays/strings and two pointers first*

---

## 🤔 **WHAT IS THE SLIDING WINDOW TECHNIQUE?**

### **Simple Explanation:**
Imagine looking through a window on a moving train:
- **Window** = A fixed or variable "frame" that views part of the data
- **Sliding** = Moving this frame through your data efficiently
- **Technique** = Instead of checking every possible substring, you expand/contract smartly

### **Real-World Examples:**
- **Netflix viewing** - Watching a 30-second preview (window) of different movies (sliding)
- **Security camera** - Monitoring a hallway section by section  
- **Restaurant orders** - Processing orders in batches of 5 (window size)
- **Stock analysis** - Looking at 7-day moving averages

### **Why Sliding Window Works:**
- **Eliminates redundancy** - Reuses previous calculations
- **O(n) time complexity** instead of O(n²) or O(n³)
- **Elegant solutions** to complex substring/subarray problems
- **Pattern recognition** for 50+ interview problems

---

## 🎯 **WHEN TO USE SLIDING WINDOW**

### **🔍 Problem Patterns That Scream "Sliding Window":**

1. **"Find substring/subarray with..."**
   - Maximum/minimum length
   - Specific sum or condition
   - All unique characters
   - Exactly K distinct elements

2. **"Longest/shortest substring..."**
   - Without repeating characters
   - With at most K distinct characters
   - That satisfies some condition

3. **"Find all subarrays..."**
   - With sum equal to K
   - With exactly K distinct elements
   - Meeting specific criteria

4. **"Maximum/minimum window..."**
   - Containing all characters of another string
   - With specific constraints

### **🚨 Keywords That Hint Sliding Window:**
- "substring" or "subarray"
- "window" or "range"
- "longest" or "shortest"
- "maximum" or "minimum"
- "contiguous"
- "K distinct" or "at most"

---

## 📊 **SLIDING WINDOW PATTERNS**

### **Pattern 1: Fixed Size Window**
```
Array:  [1, 2, 3, 4, 5, 6, 7, 8]
Window: [1, 2, 3] -> sum = 6
        [2, 3, 4] -> sum = 9  
        [3, 4, 5] -> sum = 12
        ...
```
**Use for:** Problems with fixed window size K

### **Pattern 2: Variable Size Window (Expand & Contract)**
```
Array:  [1, 2, 3, 4, 5]
Start:  [1] 
Expand: [1, 2]
Expand: [1, 2, 3]  <- condition met
Contract: [2, 3]
Expand: [2, 3, 4]
...
```
**Use for:** Find optimal window size that meets condition

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Problem 1: Maximum Sum of K Elements (Fixed Window)**

```java
import java.util.*;

public class SlidingWindowProblems {
    
    /**
     * Find maximum sum of any subarray of size K
     * Pattern: Fixed size sliding window
     * Time: O(n), Space: O(1)
     */
    public static int maxSumFixedWindow(int[] arr, int k) {
        if (arr.length < k) return 0;
        
        // Calculate sum of first window
        int windowSum = 0;
        for (int i = 0; i < k; i++) {
            windowSum += arr[i];
        }
        
        int maxSum = windowSum;
        
        // Slide the window: remove first element, add next element
        for (int i = k; i < arr.length; i++) {
            windowSum = windowSum - arr[i - k] + arr[i];
            maxSum = Math.max(maxSum, windowSum);
        }
        
        return maxSum;
    }
    
    /**
     * Longest substring without repeating characters
     * Pattern: Variable size window with HashMap
     * Time: O(n), Space: O(min(m,n)) where m is charset size
     */
    public static int longestSubstringWithoutRepeating(String s) {
        if (s == null || s.length() == 0) return 0;
        
        HashMap<Character, Integer> charIndex = new HashMap<>();
        int maxLength = 0;
        int left = 0;  // Left boundary of window
        
        for (int right = 0; right < s.length(); right++) {
            char currentChar = s.charAt(right);
            
            // If character is repeated and within current window
            if (charIndex.containsKey(currentChar) && charIndex.get(currentChar) >= left) {
                // Move left boundary to after the duplicate
                left = charIndex.get(currentChar) + 1;
            }
            
            // Update character's latest index
            charIndex.put(currentChar, right);
            
            // Update maximum length
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
    
    /**
     * Find all anagrams of pattern in text
     * Pattern: Fixed size window with frequency counting
     * Time: O(n), Space: O(1) - since alphabet size is constant
     */
    public static List<Integer> findAnagrams(String text, String pattern) {
        List<Integer> result = new ArrayList<>();
        if (text.length() < pattern.length()) return result;
        
        // Frequency arrays for pattern and current window
        int[] patternFreq = new int[26];
        int[] windowFreq = new int[26];
        
        // Count frequency of characters in pattern
        for (char c : pattern.toCharArray()) {
            patternFreq[c - 'a']++;
        }
        
        int windowSize = pattern.length();
        
        // Process first window
        for (int i = 0; i < windowSize; i++) {
            windowFreq[text.charAt(i) - 'a']++;
        }
        
        // Check if first window is an anagram
        if (Arrays.equals(patternFreq, windowFreq)) {
            result.add(0);
        }
        
        // Slide the window
        for (int i = windowSize; i < text.length(); i++) {
            // Remove leftmost character
            windowFreq[text.charAt(i - windowSize) - 'a']--;
            // Add rightmost character
            windowFreq[text.charAt(i) - 'a']++;
            
            // Check if current window is an anagram
            if (Arrays.equals(patternFreq, windowFreq)) {
                result.add(i - windowSize + 1);
            }
        }
        
        return result;
    }
    
    /**
     * Minimum window substring containing all characters of pattern
     * Pattern: Variable size window with frequency matching
     * Time: O(n), Space: O(1)
     */
    public static String minWindowSubstring(String s, String t) {
        if (s.length() < t.length()) return "";
        
        // Frequency map for target string
        HashMap<Character, Integer> targetFreq = new HashMap<>();
        for (char c : t.toCharArray()) {
            targetFreq.put(c, targetFreq.getOrDefault(c, 0) + 1);
        }
        
        int required = targetFreq.size();  // Number of unique chars in t
        int formed = 0;  // Number of unique chars in current window with desired frequency
        
        HashMap<Character, Integer> windowFreq = new HashMap<>();
        
        int left = 0, right = 0;
        int minLen = Integer.MAX_VALUE;
        int minStart = 0;
        
        while (right < s.length()) {
            // Expand window by including right character
            char rightChar = s.charAt(right);
            windowFreq.put(rightChar, windowFreq.getOrDefault(rightChar, 0) + 1);
            
            // Check if frequency of current character matches target frequency
            if (targetFreq.containsKey(rightChar) && 
                windowFreq.get(rightChar).intValue() == targetFreq.get(rightChar).intValue()) {
                formed++;
            }
            
            // Try to contract window from left
            while (left <= right && formed == required) {
                // Update minimum window if current is smaller
                if (right - left + 1 < minLen) {
                    minLen = right - left + 1;
                    minStart = left;
                }
                
                // Remove leftmost character
                char leftChar = s.charAt(left);
                windowFreq.put(leftChar, windowFreq.get(leftChar) - 1);
                
                if (targetFreq.containsKey(leftChar) && 
                    windowFreq.get(leftChar).intValue() < targetFreq.get(leftChar).intValue()) {
                    formed--;
                }
                
                left++;
            }
            
            right++;
        }
        
        return minLen == Integer.MAX_VALUE ? "" : s.substring(minStart, minStart + minLen);
    }
    
    /**
     * Longest substring with at most K distinct characters
     * Pattern: Variable size window with character counting
     * Time: O(n), Space: O(K)
     */
    public static int longestSubstringWithKDistinct(String s, int k) {
        if (s == null || s.length() == 0 || k == 0) return 0;
        
        HashMap<Character, Integer> charCount = new HashMap<>();
        int maxLength = 0;
        int left = 0;
        
        for (int right = 0; right < s.length(); right++) {
            char rightChar = s.charAt(right);
            charCount.put(rightChar, charCount.getOrDefault(rightChar, 0) + 1);
            
            // Contract window if we have more than K distinct characters
            while (charCount.size() > k) {
                char leftChar = s.charAt(left);
                charCount.put(leftChar, charCount.get(leftChar) - 1);
                
                if (charCount.get(leftChar) == 0) {
                    charCount.remove(leftChar);
                }
                
                left++;
            }
            
            maxLength = Math.max(maxLength, right - left + 1);
        }
        
        return maxLength;
    }
    
    public static void main(String[] args) {
        // Test maximum sum of K elements
        int[] arr1 = {2, 1, 5, 1, 3, 2};
        System.out.println("Max sum of 3 elements: " + maxSumFixedWindow(arr1, 3)); // 9
        
        // Test longest substring without repeating
        System.out.println("Longest unique substring 'abcabcbb': " + 
                          longestSubstringWithoutRepeating("abcabcbb")); // 3
        
        // Test find anagrams
        List<Integer> anagrams = findAnagrams("abab", "ab");
        System.out.println("Anagram positions: " + anagrams); // [0, 2]
        
        // Test minimum window substring
        System.out.println("Minimum window 'ADOBECODEBANC' containing 'ABC': " + 
                          minWindowSubstring("ADOBECODEBANC", "ABC")); // "BANC"
        
        // Test longest substring with K distinct
        System.out.println("Longest substring with 2 distinct 'eceba': " + 
                          longestSubstringWithKDistinct("eceba", 2)); // 3
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

```go
package main

import (
    "fmt"
    "math"
    "strings"
)

/**
 * Find maximum sum of any subarray of size K
 * Pattern: Fixed size sliding window
 * Time: O(n), Space: O(1)
 */
func maxSumFixedWindow(arr []int, k int) int {
    if len(arr) < k {
        return 0
    }
    
    // Calculate sum of first window
    windowSum := 0
    for i := 0; i < k; i++ {
        windowSum += arr[i]
    }
    
    maxSum := windowSum
    
    // Slide the window: remove first element, add next element
    for i := k; i < len(arr); i++ {
        windowSum = windowSum - arr[i-k] + arr[i]
        if windowSum > maxSum {
            maxSum = windowSum
        }
    }
    
    return maxSum
}

/**
 * Longest substring without repeating characters
 * Pattern: Variable size window with map
 * Time: O(n), Space: O(min(m,n)) where m is charset size
 */
func longestSubstringWithoutRepeating(s string) int {
    if len(s) == 0 {
        return 0
    }
    
    charIndex := make(map[byte]int)
    maxLength := 0
    left := 0 // Left boundary of window
    
    for right := 0; right < len(s); right++ {
        currentChar := s[right]
        
        // If character is repeated and within current window
        if index, exists := charIndex[currentChar]; exists && index >= left {
            // Move left boundary to after the duplicate
            left = index + 1
        }
        
        // Update character's latest index
        charIndex[currentChar] = right
        
        // Update maximum length
        currentLength := right - left + 1
        if currentLength > maxLength {
            maxLength = currentLength
        }
    }
    
    return maxLength
}

/**
 * Find all anagrams of pattern in text
 * Pattern: Fixed size window with frequency counting
 * Time: O(n), Space: O(1) - since alphabet size is constant
 */
func findAnagrams(text, pattern string) []int {
    var result []int
    if len(text) < len(pattern) {
        return result
    }
    
    // Frequency arrays for pattern and current window
    patternFreq := make([]int, 26)
    windowFreq := make([]int, 26)
    
    // Count frequency of characters in pattern
    for _, c := range pattern {
        patternFreq[c-'a']++
    }
    
    windowSize := len(pattern)
    
    // Process first window
    for i := 0; i < windowSize; i++ {
        windowFreq[text[i]-'a']++
    }
    
    // Check if first window is an anagram
    if isEqual(patternFreq, windowFreq) {
        result = append(result, 0)
    }
    
    // Slide the window
    for i := windowSize; i < len(text); i++ {
        // Remove leftmost character
        windowFreq[text[i-windowSize]-'a']--
        // Add rightmost character
        windowFreq[text[i]-'a']++
        
        // Check if current window is an anagram
        if isEqual(patternFreq, windowFreq) {
            result = append(result, i-windowSize+1)
        }
    }
    
    return result
}

func isEqual(a, b []int) bool {
    if len(a) != len(b) {
        return false
    }
    for i := range a {
        if a[i] != b[i] {
            return false
        }
    }
    return true
}

/**
 * Minimum window substring containing all characters of pattern
 * Pattern: Variable size window with frequency matching
 * Time: O(n), Space: O(1)
 */
func minWindowSubstring(s, t string) string {
    if len(s) < len(t) {
        return ""
    }
    
    // Frequency map for target string
    targetFreq := make(map[byte]int)
    for i := 0; i < len(t); i++ {
        targetFreq[t[i]]++
    }
    
    required := len(targetFreq) // Number of unique chars in t
    formed := 0                 // Number of unique chars in current window with desired frequency
    
    windowFreq := make(map[byte]int)
    
    left, right := 0, 0
    minLen := math.MaxInt32
    minStart := 0
    
    for right < len(s) {
        // Expand window by including right character
        rightChar := s[right]
        windowFreq[rightChar]++
        
        // Check if frequency of current character matches target frequency
        if targetCount, exists := targetFreq[rightChar]; exists && windowFreq[rightChar] == targetCount {
            formed++
        }
        
        // Try to contract window from left
        for left <= right && formed == required {
            // Update minimum window if current is smaller
            if right-left+1 < minLen {
                minLen = right - left + 1
                minStart = left
            }
            
            // Remove leftmost character
            leftChar := s[left]
            windowFreq[leftChar]--
            
            if targetCount, exists := targetFreq[leftChar]; exists && windowFreq[leftChar] < targetCount {
                formed--
            }
            
            left++
        }
        
        right++
    }
    
    if minLen == math.MaxInt32 {
        return ""
    }
    return s[minStart : minStart+minLen]
}

/**
 * Longest substring with at most K distinct characters
 * Pattern: Variable size window with character counting
 * Time: O(n), Space: O(K)
 */
func longestSubstringWithKDistinct(s string, k int) int {
    if len(s) == 0 || k == 0 {
        return 0
    }
    
    charCount := make(map[byte]int)
    maxLength := 0
    left := 0
    
    for right := 0; right < len(s); right++ {
        rightChar := s[right]
        charCount[rightChar]++
        
        // Contract window if we have more than K distinct characters
        for len(charCount) > k {
            leftChar := s[left]
            charCount[leftChar]--
            
            if charCount[leftChar] == 0 {
                delete(charCount, leftChar)
            }
            
            left++
        }
        
        currentLength := right - left + 1
        if currentLength > maxLength {
            maxLength = currentLength
        }
    }
    
    return maxLength
}

/**
 * Find maximum sum of subarray with sum <= target
 * Pattern: Variable size window with sum constraint
 * Time: O(n), Space: O(1)
 */
func maxSubarrayWithSumLessEqual(arr []int, target int) int {
    maxLength := 0
    left := 0
    currentSum := 0
    
    for right := 0; right < len(arr); right++ {
        currentSum += arr[right]
        
        // Contract window while sum exceeds target
        for currentSum > target && left <= right {
            currentSum -= arr[left]
            left++
        }
        
        // Update maximum length
        currentLength := right - left + 1
        if currentLength > maxLength {
            maxLength = currentLength
        }
    }
    
    return maxLength
}

/**
 * Count number of subarrays with exactly K distinct elements
 * Pattern: Two sliding windows (at most K) - (at most K-1)
 * Time: O(n), Space: O(K)
 */
func countSubarraysWithKDistinct(arr []int, k int) int {
    return atMostK(arr, k) - atMostK(arr, k-1)
}

func atMostK(arr []int, k int) int {
    if k == 0 {
        return 0
    }
    
    count := 0
    left := 0
    numCount := make(map[int]int)
    
    for right := 0; right < len(arr); right++ {
        numCount[arr[right]]++
        
        // Contract window if more than K distinct elements
        for len(numCount) > k {
            numCount[arr[left]]--
            if numCount[arr[left]] == 0 {
                delete(numCount, arr[left])
            }
            left++
        }
        
        // Add count of all subarrays ending at right
        count += right - left + 1
    }
    
    return count
}

func main() {
    // Test maximum sum of K elements
    arr1 := []int{2, 1, 5, 1, 3, 2}
    fmt.Printf("Max sum of 3 elements: %d\n", maxSumFixedWindow(arr1, 3)) // 9
    
    // Test longest substring without repeating
    fmt.Printf("Longest unique substring 'abcabcbb': %d\n", 
               longestSubstringWithoutRepeating("abcabcbb")) // 3
    
    // Test find anagrams
    anagrams := findAnagrams("abab", "ab")
    fmt.Printf("Anagram positions: %v\n", anagrams) // [0, 2]
    
    // Test minimum window substring
    fmt.Printf("Minimum window 'ADOBECODEBANC' containing 'ABC': %s\n", 
               minWindowSubstring("ADOBECODEBANC", "ABC")) // "BANC"
    
    // Test longest substring with K distinct
    fmt.Printf("Longest substring with 2 distinct 'eceba': %d\n", 
               longestSubstringWithKDistinct("eceba", 2)) // 3
    
    // Test max subarray with sum constraint
    arr2 := []int{1, 2, 3, 4, 5}
    fmt.Printf("Max subarray length with sum <= 8: %d\n", 
               maxSubarrayWithSumLessEqual(arr2, 8)) // 3
    
    // Test count subarrays with K distinct
    arr3 := []int{1, 2, 1, 2, 3}
    fmt.Printf("Subarrays with exactly 2 distinct: %d\n", 
               countSubarraysWithKDistinct(arr3, 2)) // 7
}
```

---

## 🧠 **UNDERSTANDING THE PATTERNS**

### **Pattern 1: Fixed Size Window**

**When to use:**
- Problem asks for window of **exact size K**
- Need to find **maximum/minimum/average** of all K-sized subarrays
- **Optimization:** Avoid recalculating entire sum each time

**Template:**
```java
int windowSum = 0;
// Calculate first window
for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}
int result = windowSum;

// Slide window
for (int i = k; i < arr.length; i++) {
    windowSum = windowSum - arr[i-k] + arr[i];
    result = Math.max(result, windowSum);
}
```

### **Pattern 2: Variable Size Window**

**When to use:**
- Find **optimal window size** that meets condition
- **Longest/shortest** substring/subarray problems
- Window size changes based on **constraints**

**Template:**
```java
int left = 0;
for (int right = 0; right < arr.length; right++) {
    // Expand window by including arr[right]
    
    // Contract window while condition not met
    while (condition_violated) {
        // Remove arr[left] from window
        left++;
    }
    
    // Update result with current window [left, right]
}
```

---

## 🎯 **STEP-BY-STEP PROBLEM SOLVING**

### **Problem: Fruit Into Baskets**
*Pick maximum fruits using only 2 types of baskets.*

#### **Step 1: Understand the Problem**
```
Fruits: [1, 2, 1, 2, 3, 1, 1]
Baskets: Can hold only 2 different types

Valid windows:
[1, 2, 1, 2] -> length 4 (types: 1, 2)
[3, 1, 1] -> length 3 (types: 3, 1)

Answer: 4 (maximum length)
```

#### **Step 2: Identify the Pattern**
- This is **longest substring with at most K distinct** where K=2
- Use **variable size window** with character counting
- **Expand** when <= 2 types, **contract** when > 2 types

#### **Step 3: Apply Sliding Window Logic**
```
Use HashMap to count fruit types in current window
Expand window by adding fruits
Contract when more than 2 types
Track maximum window size
```

#### **Step 4: Implement**
*(Similar to longestSubstringWithKDistinct with K=2)*

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Maximum Sum Subarray of Size K** ✅ (Implemented above)
2. **Longest Substring Without Repeating Characters** ✅ (Implemented above)
3. **Find All Anagrams in a String** ✅ (Implemented above)
4. **Fruit Into Baskets** (Longest substring with 2 distinct)
5. **Average of Subarrays of Size K**

### **Medium Problems:**

1. **Minimum Window Substring** ✅ (Implemented above)
2. **Longest Substring with At Most K Distinct Characters** ✅ (Implemented above)
3. **Subarrays with K Different Integers** ✅ (Implemented above)
4. **Longest Repeating Character Replacement**
5. **Minimum Size Subarray Sum**

### **Hard Problems:**

1. **Sliding Window Maximum**
2. **Substring with Concatenation of All Words**
3. **Minimum Window Subsequence**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Fixed Window**
- [ ] Implement maximum sum of K elements from scratch
- [ ] Solve average of all subarrays of size K
- [ ] Practice explaining the sliding optimization
- [ ] Time yourself - aim for 10-15 minutes

### **Day 2: Master Variable Window**
- [ ] Implement longest substring without repeating
- [ ] Solve longest substring with 2 distinct characters
- [ ] Understand expand/contract logic deeply
- [ ] Practice drawing window movements

### **Day 3: Advanced Applications**
- [ ] Implement minimum window substring
- [ ] Solve find all anagrams problem
- [ ] Practice frequency counting techniques
- [ ] Focus on edge cases

### **Day 4: Problem Recognition**
- [ ] Solve 5 sliding window problems on LeetCode
- [ ] For each problem, identify fixed vs variable pattern
- [ ] Practice explaining your approach
- [ ] Build pattern recognition muscle

### **✅ You're Ready for Next Topic When:**
- [ ] You can recognize sliding window problems instantly
- [ ] You know when to use fixed vs variable window
- [ ] You can implement solutions without looking
- [ ] You understand the optimization benefits

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[HashMaps & Sets - Detailed](./04-hashmaps-sets-detailed-java-go.md)** - Master hash-based solutions
2. **[Recursion - Detailed](./05-recursion-detailed-java-go.md)** - Learn recursive thinking
3. **Practice more** - Solve 15+ sliding window problems

### **🎯 Advanced Sliding Window Topics:**
- **Sliding Window Maximum** with deque
- **Multiple Windows** for complex constraints
- **Sliding Window + Other Patterns** combinations

### **💡 Pro Tips:**
- **Visualize the window** - Draw it on paper
- **Track what's in the window** - Use appropriate data structures
- **Think about invariants** - What must always be true?
- **Practice both patterns** - Fixed and variable windows are different

**🎉 You now master the sliding window technique! This elegant approach will help you solve some of the most challenging substring and subarray problems efficiently.**