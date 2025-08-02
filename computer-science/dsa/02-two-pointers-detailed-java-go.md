# 🎯 **TWO POINTERS: DETAILED GUIDE (JAVA & GO)**

**The Elegant Technique That Solves 50+ Interview Problems**

*Prerequisites: Complete arrays and strings first*

---

## 🤔 **WHAT IS THE TWO POINTER TECHNIQUE?**

### **Simple Explanation:**
Imagine you have two people walking through a line of people:
- **Person A** starts from the **beginning** 
- **Person B** starts from the **end**
- They walk toward each other or in the same direction
- They work together to find what you're looking for

### **Real-World Examples:**
- **Library books:** One person checks from A-M, another from N-Z, meet in middle
- **Restaurant seating:** Check tables from front and back to find empty seats
- **Traffic jam:** Police check from both ends to find the accident

### **Why Two Pointers Work:**
- **Faster than nested loops** - O(n) instead of O(n²)
- **Uses no extra memory** - O(1) space complexity
- **Elegant and readable** - Clean, logical code
- **Handles many patterns** - Palindromes, pairs, subarrays

---

## 🎯 **WHEN TO USE TWO POINTERS**

### **🔍 Problem Patterns That Scream "Two Pointers":**

1. **"Find a pair that..."** 
   - Two Sum (with sorted array)
   - Pair with given sum/difference
   - Closest pair to target

2. **"Check if palindrome"**
   - Valid palindrome
   - Longest palindromic substring
   - Check if array reads same forwards/backwards

3. **"Remove/find duplicates"**
   - Remove duplicates from sorted array
   - Move zeros to end
   - Partition array

4. **"Reverse something"**
   - Reverse array/string
   - Reverse words in string
   - Reverse only letters

### **🚨 Keywords That Hint Two Pointers:**
- "sorted array" + "find pair"
- "palindrome"
- "two sum" + "sorted"
- "remove duplicates"
- "move elements"
- "partition"

---

## 📊 **TWO POINTER PATTERNS**

### **Pattern 1: Opposite Direction (Meet in Middle)**
```
Start:  [👈]              [👉]
        left              right

Step 1: [👈]        [👉]
        left        right

Step 2:     [👈][👉]
            left right

End:    When left >= right
```

**Use for:** Palindromes, Two Sum (sorted), reverse operations

### **Pattern 2: Same Direction (Slow & Fast)**
```
Start:  [👈][👉]
        slow fast

Step 1: [👈]  [👉]
        slow  fast

Step 2:   [👈]   [👉]
          slow   fast
```

**Use for:** Remove duplicates, move elements, cycle detection

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Problem 1: Valid Palindrome**

```java
public class TwoPointerProblems {
    
    /**
     * Check if a string is a palindrome (ignoring case and non-alphanumeric)
     * Pattern: Opposite direction pointers
     * Time: O(n), Space: O(1)
     */
    public static boolean isPalindrome(String s) {
        // Clean the string
        String cleaned = s.toLowerCase().replaceAll("[^a-z0-9]", "");
        
        int left = 0;
        int right = cleaned.length() - 1;
        
        // Move pointers toward each other
        while (left < right) {
            if (cleaned.charAt(left) != cleaned.charAt(right)) {
                return false;  // Characters don't match
            }
            left++;   // Move left pointer forward
            right--;  // Move right pointer backward
        }
        
        return true;  // All characters matched
    }
    
    /**
     * Two Sum - Find indices of two numbers that add up to target (sorted array)
     * Pattern: Opposite direction pointers
     * Time: O(n), Space: O(1)
     */
    public static int[] twoSum(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                // Found the pair! Return 1-indexed positions
                return new int[]{left + 1, right + 1};
            } else if (sum < target) {
                // Sum too small, need larger number
                left++;  // Move left pointer to larger number
            } else {
                // Sum too large, need smaller number
                right--; // Move right pointer to smaller number
            }
        }
        
        return new int[]{};  // No solution found
    }
    
    /**
     * Remove duplicates from sorted array (in-place)
     * Pattern: Same direction pointers (slow & fast)
     * Time: O(n), Space: O(1)
     */
    public static int removeDuplicates(int[] nums) {
        if (nums.length <= 1) return nums.length;
        
        int slow = 0;  // Points to position for next unique element
        
        // Fast pointer scans through array
        for (int fast = 1; fast < nums.length; fast++) {
            // Found a new unique element
            if (nums[fast] != nums[slow]) {
                slow++;  // Move slow pointer
                nums[slow] = nums[fast];  // Place unique element
            }
        }
        
        return slow + 1;  // Length of array without duplicates
    }
    
    /**
     * Reverse array in-place
     * Pattern: Opposite direction pointers
     * Time: O(n), Space: O(1)
     */
    public static void reverseArray(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            // Swap elements
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            
            left++;
            right--;
        }
    }
    
    /**
     * Move all zeros to end of array
     * Pattern: Same direction pointers
     * Time: O(n), Space: O(1)
     */
    public static void moveZeros(int[] nums) {
        int writeIndex = 0;  // Where to write next non-zero
        
        // Move all non-zero elements to front
        for (int readIndex = 0; readIndex < nums.length; readIndex++) {
            if (nums[readIndex] != 0) {
                nums[writeIndex] = nums[readIndex];
                writeIndex++;
            }
        }
        
        // Fill remaining positions with zeros
        while (writeIndex < nums.length) {
            nums[writeIndex] = 0;
            writeIndex++;
        }
    }
    
    public static void main(String[] args) {
        // Test palindrome
        System.out.println("'racecar' is palindrome: " + isPalindrome("racecar"));
        System.out.println("'race a car' is palindrome: " + isPalindrome("race a car"));
        
        // Test two sum
        int[] sortedArray = {2, 7, 11, 15};
        int[] result = twoSum(sortedArray, 9);
        System.out.println("Two sum indices: [" + result[0] + ", " + result[1] + "]");
        
        // Test remove duplicates
        int[] withDuplicates = {1, 1, 2, 2, 3, 4, 4, 5};
        int newLength = removeDuplicates(withDuplicates);
        System.out.print("After removing duplicates: ");
        for (int i = 0; i < newLength; i++) {
            System.out.print(withDuplicates[i] + " ");
        }
        System.out.println();
        
        // Test reverse
        int[] toReverse = {1, 2, 3, 4, 5};
        System.out.print("Before reverse: ");
        for (int num : toReverse) System.out.print(num + " ");
        reverseArray(toReverse);
        System.out.print("\nAfter reverse: ");
        for (int num : toReverse) System.out.print(num + " ");
        System.out.println();
        
        // Test move zeros
        int[] withZeros = {0, 1, 0, 3, 12};
        System.out.print("Before moving zeros: ");
        for (int num : withZeros) System.out.print(num + " ");
        moveZeros(withZeros);
        System.out.print("\nAfter moving zeros: ");
        for (int num : withZeros) System.out.print(num + " ");
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

```go
package main

import (
    "fmt"
    "strings"
    "unicode"
)

/**
 * Check if a string is a palindrome (ignoring case and non-alphanumeric)
 * Pattern: Opposite direction pointers
 * Time: O(n), Space: O(1)
 */
func isPalindrome(s string) bool {
    // Convert to slice of runes for easier manipulation
    runes := []rune(strings.ToLower(s))
    
    left := 0
    right := len(runes) - 1
    
    for left < right {
        // Skip non-alphanumeric characters from left
        for left < right && !unicode.IsLetter(runes[left]) && !unicode.IsDigit(runes[left]) {
            left++
        }
        
        // Skip non-alphanumeric characters from right
        for left < right && !unicode.IsLetter(runes[right]) && !unicode.IsDigit(runes[right]) {
            right--
        }
        
        // Compare characters
        if runes[left] != runes[right] {
            return false
        }
        
        left++
        right--
    }
    
    return true
}

/**
 * Two Sum - Find indices of two numbers that add up to target (sorted array)
 * Pattern: Opposite direction pointers
 * Time: O(n), Space: O(1)
 */
func twoSum(numbers []int, target int) []int {
    left := 0
    right := len(numbers) - 1
    
    for left < right {
        sum := numbers[left] + numbers[right]
        
        if sum == target {
            // Found the pair! Return 1-indexed positions
            return []int{left + 1, right + 1}
        } else if sum < target {
            // Sum too small, need larger number
            left++ // Move left pointer to larger number
        } else {
            // Sum too large, need smaller number
            right-- // Move right pointer to smaller number
        }
    }
    
    return []int{} // No solution found
}

/**
 * Remove duplicates from sorted slice (in-place)
 * Pattern: Same direction pointers (slow & fast)
 * Time: O(n), Space: O(1)
 */
func removeDuplicates(nums []int) int {
    if len(nums) <= 1 {
        return len(nums)
    }
    
    slow := 0 // Points to position for next unique element
    
    // Fast pointer scans through slice
    for fast := 1; fast < len(nums); fast++ {
        // Found a new unique element
        if nums[fast] != nums[slow] {
            slow++              // Move slow pointer
            nums[slow] = nums[fast] // Place unique element
        }
    }
    
    return slow + 1 // Length of slice without duplicates
}

/**
 * Reverse slice in-place
 * Pattern: Opposite direction pointers
 * Time: O(n), Space: O(1)
 */
func reverseSlice(arr []int) {
    left := 0
    right := len(arr) - 1
    
    for left < right {
        // Swap elements using Go's multiple assignment
        arr[left], arr[right] = arr[right], arr[left]
        left++
        right--
    }
}

/**
 * Move all zeros to end of slice
 * Pattern: Same direction pointers
 * Time: O(n), Space: O(1)
 */
func moveZeros(nums []int) {
    writeIndex := 0 // Where to write next non-zero
    
    // Move all non-zero elements to front
    for readIndex := 0; readIndex < len(nums); readIndex++ {
        if nums[readIndex] != 0 {
            nums[writeIndex] = nums[readIndex]
            writeIndex++
        }
    }
    
    // Fill remaining positions with zeros
    for writeIndex < len(nums) {
        nums[writeIndex] = 0
        writeIndex++
    }
}

/**
 * Container with most water (bonus problem)
 * Pattern: Opposite direction pointers
 * Time: O(n), Space: O(1)
 */
func maxArea(height []int) int {
    left := 0
    right := len(height) - 1
    maxWater := 0
    
    for left < right {
        // Calculate area with current pointers
        width := right - left
        minHeight := min(height[left], height[right])
        area := width * minHeight
        
        if area > maxWater {
            maxWater = area
        }
        
        // Move the pointer with smaller height
        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }
    
    return maxWater
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    // Test palindrome
    fmt.Printf("'racecar' is palindrome: %t\n", isPalindrome("racecar"))
    fmt.Printf("'race a car' is palindrome: %t\n", isPalindrome("race a car"))
    
    // Test two sum
    sortedArray := []int{2, 7, 11, 15}
    result := twoSum(sortedArray, 9)
    fmt.Printf("Two sum indices: %v\n", result)
    
    // Test remove duplicates
    withDuplicates := []int{1, 1, 2, 2, 3, 4, 4, 5}
    newLength := removeDuplicates(withDuplicates)
    fmt.Printf("After removing duplicates: %v\n", withDuplicates[:newLength])
    
    // Test reverse
    toReverse := []int{1, 2, 3, 4, 5}
    fmt.Printf("Before reverse: %v\n", toReverse)
    reverseSlice(toReverse)
    fmt.Printf("After reverse: %v\n", toReverse)
    
    // Test move zeros
    withZeros := []int{0, 1, 0, 3, 12}
    fmt.Printf("Before moving zeros: %v\n", withZeros)
    moveZeros(withZeros)
    fmt.Printf("After moving zeros: %v\n", withZeros)
    
    // Test container with most water
    heights := []int{1, 8, 6, 2, 5, 4, 8, 3, 7}
    fmt.Printf("Max water area: %d\n", maxArea(heights))
}
```

---

## 🧠 **UNDERSTANDING THE PATTERNS**

### **Pattern 1: Opposite Direction (Meet in Middle)**

**When to use:**
- Array/string is **sorted** or you want to check **symmetry**
- Looking for **pairs** that satisfy a condition
- **Reversing** or checking **palindromes**

**Template:**
```java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    // Check condition with arr[left] and arr[right]
    if (condition_met) {
        // Found answer
        return result;
    } else if (need_larger_value) {
        left++;  // Move to larger value
    } else {
        right--; // Move to smaller value
    }
}
```

### **Pattern 2: Same Direction (Slow & Fast)**

**When to use:**
- **Removing/moving** elements in array
- **Finding cycles** in linked lists
- **Partitioning** array elements

**Template:**
```java
int slow = 0;  // Where to place/check elements

for (int fast = 0; fast < arr.length; fast++) {
    if (condition_met) {
        arr[slow] = arr[fast];  // Place element
        slow++;
    }
}
```

---

## 🎯 **STEP-BY-STEP PROBLEM SOLVING**

### **Problem: Container With Most Water**
*Given heights, find two lines that form container with most water.*

#### **Step 1: Understand the Problem**
```
Heights: [1, 8, 6, 2, 5, 4, 8, 3, 7]
Indices:  0  1  2  3  4  5  6  7  8

Container between index 1 and 8:
- Width = 8 - 1 = 7
- Height = min(8, 7) = 7
- Area = 7 * 7 = 49
```

#### **Step 2: Identify the Pattern**
- We need to check **pairs** of heights
- We want **maximum area**
- Brute force would be O(n²) - check all pairs
- Two pointers can do it in O(n)!

#### **Step 3: Apply Two Pointer Logic**
```
Start with widest container (left=0, right=end)
- Calculate area
- Move the pointer with smaller height (why?)
- Because moving the larger height can only decrease area
```

#### **Step 4: Implement**
*(See the maxArea function in Go code above)*

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems to Try:**

1. **Valid Palindrome** ✅ (Implemented above)
2. **Two Sum II - Input array is sorted** ✅ (Implemented above)
3. **Remove Duplicates from Sorted Array** ✅ (Implemented above)
4. **Move Zeroes** ✅ (Implemented above)
5. **Reverse String** ✅ (Similar to reverse array)

### **Medium Problems to Try Next:**

1. **Container With Most Water** ✅ (Implemented above)
2. **3Sum** (Find three numbers that sum to zero)
3. **Remove Nth Node From End of List**
4. **Longest Substring Without Repeating Characters**

### **Problem-Solving Strategy:**

1. **Read the problem** - Understand what's being asked
2. **Look for keywords** - "sorted", "palindrome", "pairs", "remove"
3. **Choose pattern** - Opposite direction vs Same direction
4. **Write template** - Start with basic two pointer structure
5. **Add logic** - Fill in the condition and movement logic
6. **Test with examples** - Walk through with sample input
7. **Handle edge cases** - Empty arrays, single elements

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master the Concepts**
- [ ] Read and understand both pointer patterns
- [ ] Type out and run the palindrome examples
- [ ] Type out and run the two sum examples
- [ ] Understand why each pointer moves when it does

### **Day 2: Practice Implementation**
- [ ] Implement remove duplicates from scratch
- [ ] Implement move zeros from scratch
- [ ] Implement container with most water
- [ ] Compare your solutions with the provided ones

### **Day 3: Problem Recognition**
- [ ] Solve 3 easy two-pointer problems on LeetCode
- [ ] For each problem, identify which pattern to use
- [ ] Practice explaining your approach out loud
- [ ] Time yourself - aim for 15-20 minutes per problem

### **✅ You're Ready for Next Topic When:**
- [ ] You can recognize two-pointer problems instantly
- [ ] You know when to use opposite vs same direction
- [ ] You can implement basic solutions without looking
- [ ] You understand why two pointers are efficient

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Sliding Window - Easy](./03-sliding-window-easy.md)** - Master the sliding window technique
2. **[HashMaps & Sets - Easy](./04-hashmaps-sets-easy.md)** - Learn hash-based solutions
3. **Practice more** - Solve 10+ two-pointer problems

### **🎯 Advanced Two Pointer Topics:**
- **Fast & Slow Pointers** for cycle detection
- **Three Pointers** for 3Sum problems  
- **Multiple Passes** for complex partitioning

### **💡 Pro Tips:**
- **Draw it out** - Visualize pointer movements on paper
- **Start simple** - Get basic version working first
- **Edge cases** - Always test with empty, single-element arrays
- **Efficiency** - Two pointers often turn O(n²) into O(n)

**🎉 You now master one of the most elegant and powerful techniques in DSA! Two pointers will help you solve dozens of interview problems efficiently.**