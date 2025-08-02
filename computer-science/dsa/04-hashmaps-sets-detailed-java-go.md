# 🗺️ **HASHMAPS & SETS: DETAILED GUIDE (JAVA & GO)**

**The Key to O(1) Lookups and Frequency Problems**

*Prerequisites: Complete arrays/strings and basic algorithms first*

---

## 🤔 **WHAT ARE HASHMAPS & SETS?**

### **Simple Explanation:**
Think of hashmaps like a **super-smart address book**:
- **Key** = Person's name (unique identifier)
- **Value** = Person's phone number (data you want to store)
- **Magic** = Instantly find anyone's number without flipping through pages

Think of sets like a **VIP guest list**:
- **Members** = People allowed in (unique elements only)
- **Magic** = Instantly check if someone is on the list
- **No duplicates** = Each person appears only once

### **Real-World Examples:**

**HashMap Examples:**
- **Dictionary** - Word (key) → Definition (value)
- **Phone book** - Name (key) → Phone number (value)
- **Student records** - Student ID (key) → Grade (value)
- **Cache** - URL (key) → Webpage content (value)

**Set Examples:**
- **Unique visitors** - Set of user IDs who visited today
- **Permissions** - Set of actions a user can perform
- **Seen movies** - Set of movie IDs you've watched
- **Available seats** - Set of seat numbers not taken

### **Why Hash Structures are Game-Changers:**
- **O(1) average lookup time** - Instantly find any element
- **No need to sort** - Works with any comparable data
- **Memory efficient** - Only store what you need
- **Perfect for counting** - Track frequencies effortlessly

---

## 🎯 **WHEN TO USE HASHMAPS & SETS**

### **🔍 HashMap Patterns:**

1. **"Count frequency of..."**
   - Characters in a string
   - Elements in an array
   - Words in a text

2. **"Check if exists..."**
   - Two Sum problem
   - Find duplicates
   - Complement exists

3. **"Group/categorize by..."**
   - Anagrams by sorted characters
   - Numbers by their sum of digits
   - Words by length

4. **"Cache/memoize..."**
   - Previously computed results
   - Expensive function calls
   - Database query results

### **🔍 Set Patterns:**

1. **"Find unique elements"**
   - Remove duplicates
   - Union/intersection of arrays
   - Unique characters

2. **"Check membership quickly"**
   - Allowed values
   - Blacklisted items
   - Valid options

3. **"Track what we've seen"**
   - Visited nodes in graph
   - Processed elements
   - Used resources

### **🚨 Keywords That Hint Hash Structures:**
- "frequency" or "count"
- "unique" or "distinct"
- "exists" or "contains"
- "duplicate" or "repeated"
- "group" or "categorize"
- "fast lookup" or "O(1)"

---

## ☕ **JAVA IMPLEMENTATIONS**

### **HashMap Mastery**

```java
import java.util.*;

public class HashMapProblems {
    
    /**
     * Two Sum - Find indices of two numbers that add up to target
     * Pattern: HashMap for O(1) complement lookup
     * Time: O(n), Space: O(n)
     */
    public static int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> numToIndex = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            // Check if complement exists
            if (numToIndex.containsKey(complement)) {
                return new int[]{numToIndex.get(complement), i};
            }
            
            // Store current number and its index
            numToIndex.put(nums[i], i);
        }
        
        return new int[]{}; // No solution found
    }
    
    /**
     * Count frequency of each character in string
     * Pattern: HashMap for frequency counting
     * Time: O(n), Space: O(k) where k is unique characters
     */
    public static Map<Character, Integer> countCharacterFrequency(String s) {
        HashMap<Character, Integer> frequency = new HashMap<>();
        
        for (char c : s.toCharArray()) {
            frequency.put(c, frequency.getOrDefault(c, 0) + 1);
        }
        
        return frequency;
    }
    
    /**
     * Group anagrams together
     * Pattern: HashMap with sorted string as key
     * Time: O(n * m log m), Space: O(n * m) where n is array size, m is average string length
     */
    public static List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> anagramGroups = new HashMap<>();
        
        for (String str : strs) {
            // Sort characters to create key
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            String sortedKey = new String(chars);
            
            // Add to appropriate group
            anagramGroups.computeIfAbsent(sortedKey, k -> new ArrayList<>()).add(str);
        }
        
        return new ArrayList<>(anagramGroups.values());
    }
    
    /**
     * Find if array contains duplicate within k distance
     * Pattern: HashMap to track recent positions
     * Time: O(n), Space: O(min(n,k))
     */
    public static boolean containsNearbyDuplicate(int[] nums, int k) {
        HashMap<Integer, Integer> numToRecentIndex = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            if (numToRecentIndex.containsKey(nums[i])) {
                int prevIndex = numToRecentIndex.get(nums[i]);
                if (i - prevIndex <= k) {
                    return true;
                }
            }
            numToRecentIndex.put(nums[i], i);
        }
        
        return false;
    }
    
    /**
     * Find longest consecutive sequence length
     * Pattern: HashSet for O(1) membership testing
     * Time: O(n), Space: O(n)
     */
    public static int longestConsecutiveSequence(int[] nums) {
        HashSet<Integer> numSet = new HashSet<>();
        
        // Add all numbers to set
        for (int num : nums) {
            numSet.add(num);
        }
        
        int maxLength = 0;
        
        for (int num : numSet) {
            // Only start counting from the beginning of a sequence
            if (!numSet.contains(num - 1)) {
                int currentNum = num;
                int currentLength = 1;
                
                // Count consecutive numbers
                while (numSet.contains(currentNum + 1)) {
                    currentNum++;
                    currentLength++;
                }
                
                maxLength = Math.max(maxLength, currentLength);
            }
        }
        
        return maxLength;
    }
    
    /**
     * Find intersection of two arrays
     * Pattern: HashSet for fast membership testing
     * Time: O(n + m), Space: O(min(n,m))
     */
    public static int[] intersectionOfArrays(int[] nums1, int[] nums2) {
        HashSet<Integer> set1 = new HashSet<>();
        HashSet<Integer> intersection = new HashSet<>();
        
        // Add all elements from first array
        for (int num : nums1) {
            set1.add(num);
        }
        
        // Find common elements
        for (int num : nums2) {
            if (set1.contains(num)) {
                intersection.add(num);
            }
        }
        
        // Convert to array
        return intersection.stream().mapToInt(Integer::intValue).toArray();
    }
    
    /**
     * Check if two strings are isomorphic
     * Pattern: Two HashMaps for bidirectional mapping
     * Time: O(n), Space: O(1) - limited by alphabet size
     */
    public static boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) return false;
        
        HashMap<Character, Character> sToT = new HashMap<>();
        HashMap<Character, Character> tToS = new HashMap<>();
        
        for (int i = 0; i < s.length(); i++) {
            char sChar = s.charAt(i);
            char tChar = t.charAt(i);
            
            // Check s -> t mapping
            if (sToT.containsKey(sChar)) {
                if (sToT.get(sChar) != tChar) {
                    return false;
                }
            } else {
                sToT.put(sChar, tChar);
            }
            
            // Check t -> s mapping
            if (tToS.containsKey(tChar)) {
                if (tToS.get(tChar) != sChar) {
                    return false;
                }
            } else {
                tToS.put(tChar, sChar);
            }
        }
        
        return true;
    }
    
    /**
     * Top K frequent elements
     * Pattern: HashMap + Priority Queue
     * Time: O(n log k), Space: O(n)
     */
    public static int[] topKFrequent(int[] nums, int k) {
        // Count frequencies
        HashMap<Integer, Integer> frequency = new HashMap<>();
        for (int num : nums) {
            frequency.put(num, frequency.getOrDefault(num, 0) + 1);
        }
        
        // Use min heap to keep top K frequent
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(
            (a, b) -> frequency.get(a) - frequency.get(b)
        );
        
        for (int num : frequency.keySet()) {
            minHeap.offer(num);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        // Convert to array
        int[] result = new int[k];
        for (int i = k - 1; i >= 0; i--) {
            result[i] = minHeap.poll();
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        // Test Two Sum
        int[] nums1 = {2, 7, 11, 15};
        int[] twoSumResult = twoSum(nums1, 9);
        System.out.println("Two Sum indices: " + Arrays.toString(twoSumResult));
        
        // Test character frequency
        Map<Character, Integer> charFreq = countCharacterFrequency("programming");
        System.out.println("Character frequencies: " + charFreq);
        
        // Test group anagrams
        String[] words = {"eat", "tea", "tan", "ate", "nat", "bat"};
        List<List<String>> anagramGroups = groupAnagrams(words);
        System.out.println("Anagram groups: " + anagramGroups);
        
        // Test nearby duplicate
        int[] nums2 = {1, 2, 3, 1};
        boolean hasNearbyDup = containsNearbyDuplicate(nums2, 3);
        System.out.println("Has nearby duplicate: " + hasNearbyDup);
        
        // Test longest consecutive
        int[] nums3 = {100, 4, 200, 1, 3, 2};
        int longestSeq = longestConsecutiveSequence(nums3);
        System.out.println("Longest consecutive sequence: " + longestSeq);
        
        // Test intersection
        int[] arr1 = {1, 2, 2, 1};
        int[] arr2 = {2, 2};
        int[] intersection = intersectionOfArrays(arr1, arr2);
        System.out.println("Intersection: " + Arrays.toString(intersection));
        
        // Test isomorphic
        boolean isomorph = isIsomorphic("egg", "add");
        System.out.println("Is isomorphic: " + isomorph);
        
        // Test top K frequent
        int[] nums4 = {1, 1, 1, 2, 2, 3};
        int[] topK = topKFrequent(nums4, 2);
        System.out.println("Top 2 frequent: " + Arrays.toString(topK));
    }
}
```

### **Set Operations in Java**

```java
import java.util.*;

public class SetProblems {
    
    /**
     * Remove duplicates from array while preserving order
     * Pattern: LinkedHashSet for order preservation
     * Time: O(n), Space: O(n)
     */
    public static int[] removeDuplicatesWithOrder(int[] nums) {
        LinkedHashSet<Integer> uniqueSet = new LinkedHashSet<>();
        
        for (int num : nums) {
            uniqueSet.add(num);
        }
        
        return uniqueSet.stream().mapToInt(Integer::intValue).toArray();
    }
    
    /**
     * Find union of multiple arrays
     * Pattern: HashSet for unique elements
     * Time: O(n), Space: O(n)
     */
    public static Set<Integer> findUnion(int[]... arrays) {
        HashSet<Integer> union = new HashSet<>();
        
        for (int[] array : arrays) {
            for (int num : array) {
                union.add(num);
            }
        }
        
        return union;
    }
    
    /**
     * Find elements that appear in all arrays
     * Pattern: Set intersection
     * Time: O(n), Space: O(n)
     */
    public static Set<Integer> findIntersectionAll(int[]... arrays) {
        if (arrays.length == 0) return new HashSet<>();
        
        HashSet<Integer> result = new HashSet<>();
        for (int num : arrays[0]) {
            result.add(num);
        }
        
        for (int i = 1; i < arrays.length; i++) {
            HashSet<Integer> currentSet = new HashSet<>();
            for (int num : arrays[i]) {
                currentSet.add(num);
            }
            result.retainAll(currentSet); // Keep only common elements
        }
        
        return result;
    }
    
    /**
     * Check if one array is subset of another
     * Pattern: Set containment
     * Time: O(n + m), Space: O(n)
     */
    public static boolean isSubset(int[] subset, int[] superset) {
        HashSet<Integer> supersetSet = new HashSet<>();
        for (int num : superset) {
            supersetSet.add(num);
        }
        
        for (int num : subset) {
            if (!supersetSet.contains(num)) {
                return false;
            }
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        // Test remove duplicates with order
        int[] withDups = {1, 2, 3, 2, 1, 4, 5, 4};
        int[] noDups = removeDuplicatesWithOrder(withDups);
        System.out.println("Remove duplicates: " + Arrays.toString(noDups));
        
        // Test union
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {3, 4, 5};
        int[] arr3 = {5, 6, 7};
        Set<Integer> union = findUnion(arr1, arr2, arr3);
        System.out.println("Union: " + union);
        
        // Test intersection of all
        int[] arr4 = {1, 2, 3, 4};
        int[] arr5 = {2, 3, 4, 5};
        int[] arr6 = {3, 4, 5, 6};
        Set<Integer> intersectionAll = findIntersectionAll(arr4, arr5, arr6);
        System.out.println("Intersection of all: " + intersectionAll);
        
        // Test subset
        int[] subset = {1, 2};
        int[] superset = {1, 2, 3, 4, 5};
        boolean isSubsetResult = isSubset(subset, superset);
        System.out.println("Is subset: " + isSubsetResult);
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Map Mastery in Go**

```go
package main

import (
    "fmt"
    "sort"
    "strings"
)

/**
 * Two Sum - Find indices of two numbers that add up to target
 * Pattern: Map for O(1) complement lookup
 * Time: O(n), Space: O(n)
 */
func twoSum(nums []int, target int) []int {
    numToIndex := make(map[int]int)
    
    for i, num := range nums {
        complement := target - num
        
        // Check if complement exists
        if index, exists := numToIndex[complement]; exists {
            return []int{index, i}
        }
        
        // Store current number and its index
        numToIndex[num] = i
    }
    
    return []int{} // No solution found
}

/**
 * Count frequency of each character in string
 * Pattern: Map for frequency counting
 * Time: O(n), Space: O(k) where k is unique characters
 */
func countCharacterFrequency(s string) map[rune]int {
    frequency := make(map[rune]int)
    
    for _, char := range s {
        frequency[char]++
    }
    
    return frequency
}

/**
 * Group anagrams together
 * Pattern: Map with sorted string as key
 * Time: O(n * m log m), Space: O(n * m)
 */
func groupAnagrams(strs []string) [][]string {
    anagramGroups := make(map[string][]string)
    
    for _, str := range strs {
        // Sort characters to create key
        chars := []rune(str)
        sort.Slice(chars, func(i, j int) bool {
            return chars[i] < chars[j]
        })
        sortedKey := string(chars)
        
        // Add to appropriate group
        anagramGroups[sortedKey] = append(anagramGroups[sortedKey], str)
    }
    
    // Convert map values to slice
    var result [][]string
    for _, group := range anagramGroups {
        result = append(result, group)
    }
    
    return result
}

/**
 * Find if array contains duplicate within k distance
 * Pattern: Map to track recent positions
 * Time: O(n), Space: O(min(n,k))
 */
func containsNearbyDuplicate(nums []int, k int) bool {
    numToRecentIndex := make(map[int]int)
    
    for i, num := range nums {
        if prevIndex, exists := numToRecentIndex[num]; exists {
            if i-prevIndex <= k {
                return true
            }
        }
        numToRecentIndex[num] = i
    }
    
    return false
}

/**
 * Find longest consecutive sequence length
 * Pattern: Set (map[int]bool) for O(1) membership testing
 * Time: O(n), Space: O(n)
 */
func longestConsecutiveSequence(nums []int) int {
    numSet := make(map[int]bool)
    
    // Add all numbers to set
    for _, num := range nums {
        numSet[num] = true
    }
    
    maxLength := 0
    
    for num := range numSet {
        // Only start counting from the beginning of a sequence
        if !numSet[num-1] {
            currentNum := num
            currentLength := 1
            
            // Count consecutive numbers
            for numSet[currentNum+1] {
                currentNum++
                currentLength++
            }
            
            if currentLength > maxLength {
                maxLength = currentLength
            }
        }
    }
    
    return maxLength
}

/**
 * Find intersection of two arrays
 * Pattern: Set for fast membership testing
 * Time: O(n + m), Space: O(min(n,m))
 */
func intersectionOfArrays(nums1, nums2 []int) []int {
    set1 := make(map[int]bool)
    intersection := make(map[int]bool)
    
    // Add all elements from first array
    for _, num := range nums1 {
        set1[num] = true
    }
    
    // Find common elements
    for _, num := range nums2 {
        if set1[num] {
            intersection[num] = true
        }
    }
    
    // Convert to slice
    var result []int
    for num := range intersection {
        result = append(result, num)
    }
    
    return result
}

/**
 * Check if two strings are isomorphic
 * Pattern: Two maps for bidirectional mapping
 * Time: O(n), Space: O(1) - limited by alphabet size
 */
func isIsomorphic(s, t string) bool {
    if len(s) != len(t) {
        return false
    }
    
    sToT := make(map[rune]rune)
    tToS := make(map[rune]rune)
    
    sRunes := []rune(s)
    tRunes := []rune(t)
    
    for i := 0; i < len(sRunes); i++ {
        sChar := sRunes[i]
        tChar := tRunes[i]
        
        // Check s -> t mapping
        if mappedChar, exists := sToT[sChar]; exists {
            if mappedChar != tChar {
                return false
            }
        } else {
            sToT[sChar] = tChar
        }
        
        // Check t -> s mapping
        if mappedChar, exists := tToS[tChar]; exists {
            if mappedChar != sChar {
                return false
            }
        } else {
            tToS[tChar] = sChar
        }
    }
    
    return true
}

/**
 * Top K frequent elements
 * Pattern: Map + Sorting
 * Time: O(n log n), Space: O(n)
 */
func topKFrequent(nums []int, k int) []int {
    // Count frequencies
    frequency := make(map[int]int)
    for _, num := range nums {
        frequency[num]++
    }
    
    // Create slice of unique numbers
    var uniqueNums []int
    for num := range frequency {
        uniqueNums = append(uniqueNums, num)
    }
    
    // Sort by frequency (descending)
    sort.Slice(uniqueNums, func(i, j int) bool {
        return frequency[uniqueNums[i]] > frequency[uniqueNums[j]]
    })
    
    // Return top K
    return uniqueNums[:k]
}

/**
 * Check if string has all unique characters
 * Pattern: Set for tracking seen characters
 * Time: O(n), Space: O(1) - limited by alphabet size
 */
func hasAllUniqueChars(s string) bool {
    seen := make(map[rune]bool)
    
    for _, char := range s {
        if seen[char] {
            return false
        }
        seen[char] = true
    }
    
    return true
}

/**
 * Find first non-repeating character
 * Pattern: Map for frequency counting + linear scan
 * Time: O(n), Space: O(1) - limited by alphabet size
 */
func firstNonRepeatingChar(s string) rune {
    frequency := make(map[rune]int)
    
    // Count frequencies
    for _, char := range s {
        frequency[char]++
    }
    
    // Find first character with frequency 1
    for _, char := range s {
        if frequency[char] == 1 {
            return char
        }
    }
    
    return 0 // No non-repeating character found
}

func main() {
    // Test Two Sum
    nums1 := []int{2, 7, 11, 15}
    twoSumResult := twoSum(nums1, 9)
    fmt.Printf("Two Sum indices: %v\n", twoSumResult)
    
    // Test character frequency
    charFreq := countCharacterFrequency("programming")
    fmt.Printf("Character frequencies: %v\n", charFreq)
    
    // Test group anagrams
    words := []string{"eat", "tea", "tan", "ate", "nat", "bat"}
    anagramGroups := groupAnagrams(words)
    fmt.Printf("Anagram groups: %v\n", anagramGroups)
    
    // Test nearby duplicate
    nums2 := []int{1, 2, 3, 1}
    hasNearbyDup := containsNearbyDuplicate(nums2, 3)
    fmt.Printf("Has nearby duplicate: %t\n", hasNearbyDup)
    
    // Test longest consecutive
    nums3 := []int{100, 4, 200, 1, 3, 2}
    longestSeq := longestConsecutiveSequence(nums3)
    fmt.Printf("Longest consecutive sequence: %d\n", longestSeq)
    
    // Test intersection
    arr1 := []int{1, 2, 2, 1}
    arr2 := []int{2, 2}
    intersection := intersectionOfArrays(arr1, arr2)
    fmt.Printf("Intersection: %v\n", intersection)
    
    // Test isomorphic
    isomorph := isIsomorphic("egg", "add")
    fmt.Printf("Is isomorphic: %t\n", isomorph)
    
    // Test top K frequent
    nums4 := []int{1, 1, 1, 2, 2, 3}
    topK := topKFrequent(nums4, 2)
    fmt.Printf("Top 2 frequent: %v\n", topK)
    
    // Test unique characters
    hasUnique := hasAllUniqueChars("programming")
    fmt.Printf("Has all unique characters: %t\n", hasUnique)
    
    // Test first non-repeating
    firstNonRep := firstNonRepeatingChar("programming")
    fmt.Printf("First non-repeating character: %c\n", firstNonRep)
}
```

### **Set Operations in Go**

```go
package main

import "fmt"

/**
 * Remove duplicates from slice while preserving order
 * Pattern: Map for seen tracking
 * Time: O(n), Space: O(n)
 */
func removeDuplicatesWithOrder(nums []int) []int {
    seen := make(map[int]bool)
    var result []int
    
    for _, num := range nums {
        if !seen[num] {
            seen[num] = true
            result = append(result, num)
        }
    }
    
    return result
}

/**
 * Find union of multiple slices
 * Pattern: Set for unique elements
 * Time: O(n), Space: O(n)
 */
func findUnion(arrays ...[]int) []int {
    union := make(map[int]bool)
    
    for _, array := range arrays {
        for _, num := range array {
            union[num] = true
        }
    }
    
    var result []int
    for num := range union {
        result = append(result, num)
    }
    
    return result
}

/**
 * Find elements that appear in all slices
 * Pattern: Set intersection
 * Time: O(n), Space: O(n)
 */
func findIntersectionAll(arrays ...[]int) []int {
    if len(arrays) == 0 {
        return []int{}
    }
    
    // Start with first array
    result := make(map[int]bool)
    for _, num := range arrays[0] {
        result[num] = true
    }
    
    // Intersect with each subsequent array
    for i := 1; i < len(arrays); i++ {
        currentSet := make(map[int]bool)
        for _, num := range arrays[i] {
            currentSet[num] = true
        }
        
        // Keep only common elements
        for num := range result {
            if !currentSet[num] {
                delete(result, num)
            }
        }
    }
    
    // Convert to slice
    var intersection []int
    for num := range result {
        intersection = append(intersection, num)
    }
    
    return intersection
}

/**
 * Check if one slice is subset of another
 * Pattern: Set containment
 * Time: O(n + m), Space: O(n)
 */
func isSubset(subset, superset []int) bool {
    supersetSet := make(map[int]bool)
    for _, num := range superset {
        supersetSet[num] = true
    }
    
    for _, num := range subset {
        if !supersetSet[num] {
            return false
        }
    }
    
    return true
}

/**
 * Find symmetric difference (elements in either set but not both)
 * Pattern: Set operations
 * Time: O(n + m), Space: O(n + m)
 */
func symmetricDifference(arr1, arr2 []int) []int {
    set1 := make(map[int]bool)
    set2 := make(map[int]bool)
    
    for _, num := range arr1 {
        set1[num] = true
    }
    
    for _, num := range arr2 {
        set2[num] = true
    }
    
    var result []int
    
    // Elements in set1 but not set2
    for num := range set1 {
        if !set2[num] {
            result = append(result, num)
        }
    }
    
    // Elements in set2 but not set1
    for num := range set2 {
        if !set1[num] {
            result = append(result, num)
        }
    }
    
    return result
}

func main() {
    // Test remove duplicates with order
    withDups := []int{1, 2, 3, 2, 1, 4, 5, 4}
    noDups := removeDuplicatesWithOrder(withDups)
    fmt.Printf("Remove duplicates: %v\n", noDups)
    
    // Test union
    arr1 := []int{1, 2, 3}
    arr2 := []int{3, 4, 5}
    arr3 := []int{5, 6, 7}
    union := findUnion(arr1, arr2, arr3)
    fmt.Printf("Union: %v\n", union)
    
    // Test intersection of all
    arr4 := []int{1, 2, 3, 4}
    arr5 := []int{2, 3, 4, 5}
    arr6 := []int{3, 4, 5, 6}
    intersectionAll := findIntersectionAll(arr4, arr5, arr6)
    fmt.Printf("Intersection of all: %v\n", intersectionAll)
    
    // Test subset
    subset := []int{1, 2}
    superset := []int{1, 2, 3, 4, 5}
    isSubsetResult := isSubset(subset, superset)
    fmt.Printf("Is subset: %t\n", isSubsetResult)
    
    // Test symmetric difference
    symDiff := symmetricDifference([]int{1, 2, 3}, []int{3, 4, 5})
    fmt.Printf("Symmetric difference: %v\n", symDiff)
}
```

---

## 🧠 **UNDERSTANDING HASH STRUCTURES**

### **How Hash Tables Work (Simplified)**

```
Hash Function Magic:
Input: "apple"
Hash Function: Converts to number
Output: 42 (index in internal array)

Internal Array:
[0] -> null
[1] -> null
...
[42] -> "apple" -> value
...
[99] -> null

Result: O(1) access time!
```

### **Hash Function Properties:**
- **Deterministic** - Same input always gives same output
- **Uniform distribution** - Spreads keys evenly
- **Fast computation** - Quick to calculate
- **Avalanche effect** - Small input change = big output change

### **Collision Handling:**
- **Chaining** - Store multiple items at same index (linked list)
- **Open addressing** - Find next available slot
- **Load factor** - Keep table from getting too full

---

## 🎯 **COMMON PATTERNS & TEMPLATES**

### **Pattern 1: Frequency Counting**
```java
Map<T, Integer> frequency = new HashMap<>();
for (T item : collection) {
    frequency.put(item, frequency.getOrDefault(item, 0) + 1);
}
```

### **Pattern 2: Fast Lookup**
```java
Set<T> lookupSet = new HashSet<>(collection);
if (lookupSet.contains(target)) {
    // Found in O(1) time
}
```

### **Pattern 3: Grouping**
```java
Map<KeyType, List<ValueType>> groups = new HashMap<>();
for (ValueType value : values) {
    KeyType key = extractKey(value);
    groups.computeIfAbsent(key, k -> new ArrayList<>()).add(value);
}
```

### **Pattern 4: Complement Finding**
```java
Map<T, Integer> seen = new HashMap<>();
for (int i = 0; i < array.length; i++) {
    T complement = target - array[i];
    if (seen.containsKey(complement)) {
        // Found pair: (complement, array[i])
    }
    seen.put(array[i], i);
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Two Sum** ✅ (Implemented above)
2. **Contains Duplicate** (Check if array has duplicates)
3. **Valid Anagram** (Check if two strings are anagrams)
4. **Intersection of Two Arrays** ✅ (Implemented above)
5. **First Unique Character** (Find first non-repeating character)

### **Medium Problems:**

1. **Group Anagrams** ✅ (Implemented above)
2. **Top K Frequent Elements** ✅ (Implemented above)
3. **Longest Consecutive Sequence** ✅ (Implemented above)
4. **4Sum II** (Four arrays, count tuples with sum zero)
5. **Subarray Sum Equals K** (Count subarrays with sum K)

### **Hard Problems:**

1. **Substring with Concatenation of All Words**
2. **Longest Substring with At Most K Distinct Characters**
3. **Insert Delete GetRandom O(1)**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master HashMap Basics**
- [ ] Implement Two Sum from scratch in both languages
- [ ] Practice frequency counting problems
- [ ] Understand hash function concepts
- [ ] Time yourself - aim for 10-15 minutes per easy problem

### **Day 2: Advanced HashMap Patterns**
- [ ] Implement Group Anagrams problem
- [ ] Solve Top K Frequent Elements
- [ ] Practice bidirectional mapping (isomorphic strings)
- [ ] Focus on edge cases and error handling

### **Day 3: Set Operations Mastery**
- [ ] Implement set intersection and union
- [ ] Solve Longest Consecutive Sequence
- [ ] Practice membership testing patterns
- [ ] Understand when to use Set vs HashMap

### **Day 4: Problem Recognition & Speed**
- [ ] Solve 5 hash-based problems on LeetCode
- [ ] For each problem, identify the pattern used
- [ ] Practice explaining your approach out loud
- [ ] Build speed and confidence

### **✅ You're Ready for Next Topic When:**
- [ ] You can recognize hash-based problems instantly
- [ ] You know when to use HashMap vs HashSet
- [ ] You can implement solutions without looking up syntax
- [ ] You understand time/space complexity trade-offs

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Recursion - Detailed](./05-recursion-detailed-java-go.md)** - Master recursive thinking
2. **[Stack & Queue - Detailed](./06-stack-queue-detailed-java-go.md)** - Learn linear data structures
3. **Practice more** - Solve 20+ hash-based problems

### **🎯 Advanced Hash Topics:**
- **Custom Hash Functions** for specific use cases
- **Consistent Hashing** for distributed systems
- **Bloom Filters** for approximate membership testing
- **Count-Min Sketch** for frequency estimation

### **💡 Pro Tips:**
- **Choose the right structure** - HashMap for key-value, HashSet for membership
- **Handle collisions gracefully** - Don't assume perfect hashing
- **Consider memory usage** - Hash tables can be space-intensive
- **Use appropriate initial capacity** - Avoid frequent resizing

**🎉 You now master hash-based data structures! These are fundamental tools that will appear in countless interview problems and real-world applications.**