# 📚 **ARRAYS & STRINGS: DETAILED GUIDE (JAVA & GO)**

**Your Foundation for 70% of Coding Interview Problems**

*Prerequisites: Complete `00-programming-fundamentals-java-go.md` first*

---

## 🎯 **WHAT YOU'LL LEARN TODAY**

By the end of this guide, you'll be able to:
- ✅ Solve array manipulation problems in Java and Go
- ✅ Handle string processing efficiently  
- ✅ Recognize common patterns used in 100+ interview problems
- ✅ Understand time and space complexity basics
- ✅ Feel confident with your first real DSA problems

**Time needed:** 4-6 hours (spread over 2-3 days)

---

## 🔍 **WHY ARRAYS & STRINGS FIRST?**

### **📊 Interview Statistics:**
- **70% of easy problems** use arrays or strings
- **50% of medium problems** combine arrays with other concepts
- **Arrays appear in:** Two Sum, Best Time to Buy Stock, Maximum Subarray
- **Strings appear in:** Valid Palindrome, Longest Substring, Anagrams

### **🧠 Learning Benefits:**
- **Foundation for everything** - Lists, stacks, queues are all based on arrays
- **Pattern recognition** - Many problems use similar techniques
- **Confidence building** - Start with familiar concepts
- **Real-world relevance** - Used in every application you'll ever build

---

## 📊 **ARRAY FUNDAMENTALS DEEP DIVE**

### **🤔 What is an Array? (Real Examples)**

Think of arrays like:
- **Parking garage** - Numbered parking spots (index), each holds one car (data)
- **Apartment building** - Apartment numbers (index), each has residents (data)
- **Playlist** - Song positions (index), each has a song (data)

```
Real Playlist:      ["Song 1", "Song 2", "Song 3", "Song 4"]
Array Index:        [   0   ,    1   ,    2   ,    3   ]
Memory Address:     [ 1000  ,  1004  ,  1008  ,  1012  ]
```

### **⚡ Why Arrays are Fast**

**Memory Layout:**
```
Normal Variables:    [X]     [Y]        [Z]
Memory:             1000    5432       8765
                  (scattered everywhere)

Array:              [A] [B] [C] [D]
Memory:            1000 1004 1008 1012
                  (right next to each other)
```

**This makes arrays:**
- **Fast to access** - Computer knows exactly where each element is
- **Memory efficient** - No wasted space between elements
- **Cache friendly** - Loading one element loads nearby elements too

---

## ☕ **JAVA ARRAY MASTERY**

### **Creating and Using Arrays**

```java
public class ArrayMastery {
    public static void main(String[] args) {
        // Different ways to create arrays
        
        // Method 1: Declare size first
        int[] numbers = new int[5];  // [0, 0, 0, 0, 0]
        numbers[0] = 10;  // [10, 0, 0, 0, 0]
        numbers[1] = 20;  // [10, 20, 0, 0, 0]
        
        // Method 2: Initialize with values
        int[] scores = {95, 87, 92, 78, 88};
        
        // Method 3: Using new keyword with values
        String[] names = new String[]{"Alice", "Bob", "Charlie"};
        
        // Array properties
        System.out.println("Scores length: " + scores.length);
        System.out.println("First score: " + scores[0]);
        System.out.println("Last score: " + scores[scores.length - 1]);
        
        // Common mistake - accessing beyond array bounds
        // System.out.println(scores[10]); // This would crash!
    }
}
```

### **Essential Array Operations**

```java
import java.util.Arrays;

public class ArrayOperations {
    
    // 1. Finding elements
    public static int findElement(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;  // Return index where found
            }
        }
        return -1;  // Not found
    }
    
    // 2. Finding maximum element
    public static int findMax(int[] arr) {
        if (arr.length == 0) return Integer.MIN_VALUE;
        
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
    
    // 3. Reversing array (in-place)
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
    
    // 4. Removing duplicates (returns new size)
    public static int removeDuplicates(int[] arr) {
        if (arr.length <= 1) return arr.length;
        
        int writeIndex = 1;  // Where to write next unique element
        
        for (int readIndex = 1; readIndex < arr.length; readIndex++) {
            if (arr[readIndex] != arr[readIndex - 1]) {
                arr[writeIndex] = arr[readIndex];
                writeIndex++;
            }
        }
        
        return writeIndex;  // New length
    }
    
    public static void main(String[] args) {
        int[] numbers = {1, 3, 5, 3, 7, 5, 9};
        
        // Test finding
        int index = findElement(numbers, 5);
        System.out.println("Found 5 at index: " + index);
        
        // Test max
        int max = findMax(numbers);
        System.out.println("Maximum value: " + max);
        
        // Test reverse
        System.out.println("Original: " + Arrays.toString(numbers));
        reverseArray(numbers);
        System.out.println("Reversed: " + Arrays.toString(numbers));
        
        // Test remove duplicates (need sorted array)
        int[] sorted = {1, 1, 2, 2, 3, 4, 4, 5};
        int newLength = removeDuplicates(sorted);
        System.out.println("After removing duplicates: " + 
                          Arrays.toString(Arrays.copyOf(sorted, newLength)));
    }
}
```

---

## 🐹 **GO SLICE MASTERY**

### **Creating and Using Slices**

```go
package main

import "fmt"

func main() {
    // Different ways to create slices
    
    // Method 1: Using slice literal
    numbers := []int{1, 2, 3, 4, 5}
    
    // Method 2: Using make (creates slice with specific length)
    scores := make([]int, 5)  // [0, 0, 0, 0, 0]
    scores[0] = 95
    scores[1] = 87
    
    // Method 3: Using make with capacity
    names := make([]string, 0, 10)  // length 0, capacity 10
    names = append(names, "Alice")
    names = append(names, "Bob")
    
    // Slice properties
    fmt.Printf("Numbers length: %d\n", len(numbers))
    fmt.Printf("Names capacity: %d\n", cap(names))
    fmt.Printf("First number: %d\n", numbers[0])
    fmt.Printf("Last number: %d\n", numbers[len(numbers)-1])
    
    // Slice operations
    subSlice := numbers[1:4]  // [2, 3, 4] (index 1 to 3)
    fmt.Printf("Sub-slice: %v\n", subSlice)
}
```

### **Essential Slice Operations**

```go
package main

import "fmt"

// 1. Finding elements
func findElement(arr []int, target int) int {
    for i, value := range arr {
        if value == target {
            return i  // Return index where found
        }
    }
    return -1  // Not found
}

// 2. Finding maximum element
func findMax(arr []int) int {
    if len(arr) == 0 {
        return 0  // or return error
    }
    
    max := arr[0]
    for _, value := range arr[1:] {
        if value > max {
            max = value
        }
    }
    return max
}

// 3. Reversing slice (in-place)
func reverseSlice(arr []int) {
    left := 0
    right := len(arr) - 1
    
    for left < right {
        // Swap elements (Go's multiple assignment)
        arr[left], arr[right] = arr[right], arr[left]
        left++
        right--
    }
}

// 4. Removing duplicates (returns new slice)
func removeDuplicates(arr []int) []int {
    if len(arr) <= 1 {
        return arr
    }
    
    result := []int{arr[0]}  // Start with first element
    
    for i := 1; i < len(arr); i++ {
        if arr[i] != arr[i-1] {
            result = append(result, arr[i])
        }
    }
    
    return result
}

func main() {
    numbers := []int{1, 3, 5, 3, 7, 5, 9}
    
    // Test finding
    index := findElement(numbers, 5)
    fmt.Printf("Found 5 at index: %d\n", index)
    
    // Test max
    max := findMax(numbers)
    fmt.Printf("Maximum value: %d\n", max)
    
    // Test reverse
    fmt.Printf("Original: %v\n", numbers)
    reverseSlice(numbers)
    fmt.Printf("Reversed: %v\n", numbers)
    
    // Test remove duplicates (need sorted slice)
    sorted := []int{1, 1, 2, 2, 3, 4, 4, 5}
    unique := removeDuplicates(sorted)
    fmt.Printf("After removing duplicates: %v\n", unique)
}
```

---

## 📝 **STRING PROCESSING MASTERY**

### **☕ Java String Operations**

```java
public class StringMastery {
    
    // 1. Check if string is palindrome
    public static boolean isPalindrome(String s) {
        // Convert to lowercase and remove non-alphanumeric
        String cleaned = s.toLowerCase().replaceAll("[^a-z0-9]", "");
        
        int left = 0;
        int right = cleaned.length() - 1;
        
        while (left < right) {
            if (cleaned.charAt(left) != cleaned.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
    
    // 2. Reverse words in a string
    public static String reverseWords(String s) {
        String[] words = s.trim().split("\\s+");
        StringBuilder result = new StringBuilder();
        
        for (int i = words.length - 1; i >= 0; i--) {
            result.append(words[i]);
            if (i > 0) {
                result.append(" ");
            }
        }
        
        return result.toString();
    }
    
    // 3. Find first unique character
    public static int firstUniqueChar(String s) {
        // Count frequency of each character
        int[] count = new int[26];  // for lowercase a-z
        
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        
        // Find first character with count 1
        for (int i = 0; i < s.length(); i++) {
            if (count[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }
        
        return -1;  // No unique character
    }
    
    // 4. Check if two strings are anagrams
    public static boolean areAnagrams(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        
        int[] count1 = new int[26];
        int[] count2 = new int[26];
        
        for (int i = 0; i < s1.length(); i++) {
            count1[s1.charAt(i) - 'a']++;
            count2[s2.charAt(i) - 'a']++;
        }
        
        for (int i = 0; i < 26; i++) {
            if (count1[i] != count2[i]) {
                return false;
            }
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        // Test palindrome
        System.out.println("'racecar' is palindrome: " + isPalindrome("racecar"));
        System.out.println("'A man a plan a canal Panama' is palindrome: " + 
                          isPalindrome("A man, a plan, a canal: Panama"));
        
        // Test reverse words
        System.out.println("Reversed: '" + reverseWords("Hello World Java") + "'");
        
        // Test first unique character
        System.out.println("First unique in 'leetcode': " + firstUniqueChar("leetcode"));
        System.out.println("First unique in 'loveleetcode': " + firstUniqueChar("loveleetcode"));
        
        // Test anagrams
        System.out.println("'listen' and 'silent' are anagrams: " + 
                          areAnagrams("listen", "silent"));
    }
}
```

### **🐹 Go String Operations**

```go
package main

import (
    "fmt"
    "strings"
    "unicode"
)

// 1. Check if string is palindrome
func isPalindrome(s string) bool {
    // Convert to lowercase and keep only alphanumeric
    cleaned := ""
    for _, r := range strings.ToLower(s) {
        if unicode.IsLetter(r) || unicode.IsDigit(r) {
            cleaned += string(r)
        }
    }
    
    left, right := 0, len(cleaned)-1
    
    for left < right {
        if cleaned[left] != cleaned[right] {
            return false
        }
        left++
        right--
    }
    
    return true
}

// 2. Reverse words in a string
func reverseWords(s string) string {
    words := strings.Fields(s)  // Split by whitespace
    
    // Reverse the slice of words
    for i, j := 0, len(words)-1; i < j; i, j = i+1, j-1 {
        words[i], words[j] = words[j], words[i]
    }
    
    return strings.Join(words, " ")
}

// 3. Find first unique character
func firstUniqueChar(s string) int {
    // Count frequency of each character
    count := make(map[rune]int)
    
    for _, r := range s {
        count[r]++
    }
    
    // Find first character with count 1
    for i, r := range s {
        if count[r] == 1 {
            return i
        }
    }
    
    return -1  // No unique character
}

// 4. Check if two strings are anagrams
func areAnagrams(s1, s2 string) bool {
    if len(s1) != len(s2) {
        return false
    }
    
    count1 := make(map[rune]int)
    count2 := make(map[rune]int)
    
    for _, r := range s1 {
        count1[r]++
    }
    
    for _, r := range s2 {
        count2[r]++
    }
    
    // Compare the maps
    for r, freq1 := range count1 {
        if freq2, exists := count2[r]; !exists || freq1 != freq2 {
            return false
        }
    }
    
    return true
}

func main() {
    // Test palindrome
    fmt.Printf("'racecar' is palindrome: %t\n", isPalindrome("racecar"))
    fmt.Printf("'A man a plan a canal Panama' is palindrome: %t\n", 
               isPalindrome("A man, a plan, a canal: Panama"))
    
    // Test reverse words
    fmt.Printf("Reversed: '%s'\n", reverseWords("Hello World Go"))
    
    // Test first unique character
    fmt.Printf("First unique in 'leetcode': %d\n", firstUniqueChar("leetcode"))
    fmt.Printf("First unique in 'loveleetcode': %d\n", firstUniqueChar("loveleetcode"))
    
    // Test anagrams
    fmt.Printf("'listen' and 'silent' are anagrams: %t\n", 
               areAnagrams("listen", "silent"))
}
```

---

## 🧠 **UNDERSTANDING TIME & SPACE COMPLEXITY**

### **⏰ Time Complexity (How Fast?)**

**Think of it like this:**
- **O(1)** = Instant coffee (same time no matter how many people)
- **O(n)** = Reading a book (time increases with book length)
- **O(n²)** = Comparing every student with every other student

#### **Common Examples:**

```java
// O(1) - Constant time
int getFirst(int[] arr) {
    return arr[0];  // Always takes same time
}

// O(n) - Linear time  
int findMax(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++) {  // Loop through all elements
        if (arr[i] > max) max = arr[i];
    }
    return max;
}

// O(n²) - Quadratic time
void printAllPairs(int[] arr) {
    for (int i = 0; i < arr.length; i++) {        // n iterations
        for (int j = 0; j < arr.length; j++) {    // n iterations for each i
            System.out.println(arr[i] + ", " + arr[j]);
        }
    }
}
```

### **💾 Space Complexity (How Much Memory?)**

```java
// O(1) space - uses same memory regardless of input size
void printArray(int[] arr) {
    for (int num : arr) {
        System.out.println(num);  // Only stores 'num' variable
    }
}

// O(n) space - memory grows with input size
int[] copyArray(int[] arr) {
    int[] copy = new int[arr.length];  // Creates new array of same size
    for (int i = 0; i < arr.length; i++) {
        copy[i] = arr[i];
    }
    return copy;
}
```

---

## 🎯 **PRACTICE PROBLEMS**

### **Problem 1: Two Sum (EASY)**
**Given an array and target, find two numbers that add up to target.**

#### **☕ Java Solution**
```java
import java.util.HashMap;

public int[] twoSum(int[] nums, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        
        map.put(nums[i], i);
    }
    
    return new int[]{};
}
```

#### **🐹 Go Solution**
```go
func twoSum(nums []int, target int) []int {
    numMap := make(map[int]int)
    
    for i, num := range nums {
        complement := target - num
        
        if index, exists := numMap[complement]; exists {
            return []int{index, i}
        }
        
        numMap[num] = i
    }
    
    return []int{}
}
```

### **Problem 2: Valid Palindrome (EASY)**
**Check if a string is a palindrome, ignoring case and non-alphanumeric characters.**

*Try implementing this yourself using the palindrome examples above!*

### **Problem 3: Best Time to Buy and Buy Stock (EASY)**
**Find the maximum profit from buying and selling stock once.**

#### **☕ Java Solution**
```java
public int maxProfit(int[] prices) {
    int minPrice = Integer.MAX_VALUE;
    int maxProfit = 0;
    
    for (int price : prices) {
        if (price < minPrice) {
            minPrice = price;
        } else if (price - minPrice > maxProfit) {
            maxProfit = price - minPrice;
        }
    }
    
    return maxProfit;
}
```

#### **🐹 Go Solution**
```go
func maxProfit(prices []int) int {
    minPrice := prices[0]
    maxProfit := 0
    
    for _, price := range prices {
        if price < minPrice {
            minPrice = price
        } else if price-minPrice > maxProfit {
            maxProfit = price - minPrice
        }
    }
    
    return maxProfit
}
```

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1-2: Master the Basics**
- [ ] Set up Java and Go development environments
- [ ] Type out and run all the array operation examples
- [ ] Type out and run all the string operation examples
- [ ] Understand what each line does

### **Day 3-4: Solve Problems**
- [ ] Solve Two Sum in both Java and Go
- [ ] Solve Valid Palindrome in both languages
- [ ] Solve Best Time to Buy Stock in both languages
- [ ] Explain each solution out loud

### **Day 5-6: Pattern Recognition**
- [ ] Notice the HashMap/Map pattern in Two Sum
- [ ] Notice the two-pointer pattern in Palindrome
- [ ] Notice the single-pass pattern in Stock problem
- [ ] Practice explaining these patterns

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement basic array operations without looking
- [ ] You understand time/space complexity basics
- [ ] You've solved at least 3 problems in both languages
- [ ] You can explain your solutions to someone else

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Two Pointers - Easy](./02-two-pointers-easy.md)** - Learn the two-pointer technique
2. **[Sliding Window - Easy](./03-sliding-window-easy.md)** - Master window-based problems
3. **Practice more** - Try 5-10 easy array/string problems on LeetCode

### **🎯 Pro Tips:**
- **Practice both languages** - Different companies prefer different languages
- **Time yourself** - Aim for 15-20 minutes per easy problem
- **Explain out loud** - If you can teach it, you understand it
- **Don't memorize** - Understand the patterns and logic

**🎉 Congratulations! You now have a solid foundation in arrays and strings. These skills will help you in 70% of coding interviews!**