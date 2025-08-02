# 🔍 **BINARY SEARCH: DETAILED GUIDE (JAVA & GO)**

**The Most Efficient Search Algorithm - Master the Art of Divide and Conquer**

*Prerequisites: Complete arrays, recursion, and basic algorithms first*

---

## 🤔 **WHAT IS BINARY SEARCH?**

### **Simple Explanation:**
Imagine you're playing a number guessing game (1-100):
- **Friend thinks of 50** - You guess: "Is it 50?" → "Too high!"
- **You guess 25** - "Too low!"
- **You guess 37** - "Too high!" 
- **You guess 31** - "Too low!"
- **You guess 34** - "Correct!"

**That's binary search!** Each guess eliminates **half** of the remaining possibilities.

### **Real-World Examples:**

**📖 Dictionary Lookup:**
- Looking for "PYTHON" in a dictionary
- Open to middle → "MIDDLE" → Too early, go to right half
- Open to middle of right half → "SYSTEM" → Too late, go to left
- Continue until you find "PYTHON"

**📚 Library Book Search:**
- Looking for book with call number "515.3"
- Start in middle of library
- If current section is "400s", go right
- If current section is "600s", go left
- Narrow down until you find "515.3"

**🎯 Why Binary Search is Revolutionary:**
- **O(log n) time complexity** - Super fast even for huge datasets
- **Elegantly simple** - Easy to understand and implement
- **Foundation for advanced algorithms** - Used in many complex problems
- **Interview favorite** - Appears in 25% of coding interviews

---

## 🎯 **WHEN TO USE BINARY SEARCH**

### **🔍 Perfect Binary Search Scenarios:**

1. **Array is SORTED** (most important requirement)
   - Numerical values in ascending/descending order
   - Strings in alphabetical order
   - Custom objects with defined ordering

2. **Search for specific value**
   - Find exact match
   - Find insert position
   - Find first/last occurrence

3. **Search for boundary/condition**
   - First element ≥ target
   - Last element ≤ target
   - Peak element in mountain array

4. **Optimization problems**
   - Minimize/maximize some value
   - Find smallest value that satisfies condition
   - Search in answer space

### **🚨 Keywords That Hint Binary Search:**
- "sorted array"
- "find target" 
- "O(log n) time"
- "first/last occurrence"
- "insert position"
- "search range"
- "minimize/maximize"

### **❌ When NOT to Use Binary Search:**
- **Unsorted data** - Binary search requires sorted input
- **Linked lists** - No random access (can't jump to middle efficiently)
- **Small datasets** - Linear search might be simpler
- **Complex search criteria** - When simple comparison isn't enough

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Basic Binary Search Patterns**

```java
import java.util.*;

public class BinarySearchBasics {
    
    /**
     * Standard binary search - find exact match
     * Pattern: Classic divide and conquer
     * Time: O(log n), Space: O(1)
     */
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2; // Avoid overflow
            
            if (arr[mid] == target) {
                return mid; // Found target
            } else if (arr[mid] < target) {
                left = mid + 1; // Search right half
            } else {
                right = mid - 1; // Search left half
            }
        }
        
        return -1; // Target not found
    }
    
    /**
     * Recursive binary search
     * Pattern: Recursive divide and conquer
     * Time: O(log n), Space: O(log n) due to call stack
     */
    public static int binarySearchRecursive(int[] arr, int target, int left, int right) {
        // Base case: search space exhausted
        if (left > right) {
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            return binarySearchRecursive(arr, target, mid + 1, right);
        } else {
            return binarySearchRecursive(arr, target, left, mid - 1);
        }
    }
    
    /**
     * Find insert position for target (where target should be inserted)
     * Pattern: Lower bound search
     * Time: O(log n), Space: O(1)
     */
    public static int findInsertPosition(int[] arr, int target) {
        int left = 0;
        int right = arr.length;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        return left;
    }
    
    /**
     * Find first occurrence of target
     * Pattern: Lower bound with exact match check
     * Time: O(log n), Space: O(1)
     */
    public static int findFirstOccurrence(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        int result = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                result = mid;
                right = mid - 1; // Continue searching left for first occurrence
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    /**
     * Find last occurrence of target
     * Pattern: Upper bound with exact match check
     * Time: O(log n), Space: O(1)
     */
    public static int findLastOccurrence(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        int result = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                result = mid;
                left = mid + 1; // Continue searching right for last occurrence
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    /**
     * Find range (first and last occurrence) of target
     * Pattern: Combine first and last occurrence searches
     * Time: O(log n), Space: O(1)
     */
    public static int[] findRange(int[] arr, int target) {
        int first = findFirstOccurrence(arr, target);
        if (first == -1) {
            return new int[]{-1, -1}; // Target not found
        }
        
        int last = findLastOccurrence(arr, target);
        return new int[]{first, last};
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 4, 4, 4, 6, 8, 10};
        
        System.out.println("Binary search for 4: " + binarySearch(arr, 4)); // 2 (any occurrence)
        System.out.println("Recursive search for 6: " + binarySearchRecursive(arr, 6, 0, arr.length - 1)); // 5
        System.out.println("Insert position for 5: " + findInsertPosition(arr, 5)); // 5
        System.out.println("First occurrence of 4: " + findFirstOccurrence(arr, 4)); // 2
        System.out.println("Last occurrence of 4: " + findLastOccurrence(arr, 4)); // 4
        System.out.println("Range of 4: " + Arrays.toString(findRange(arr, 4))); // [2, 4]
        
        // Test with target not in array
        System.out.println("Range of 5: " + Arrays.toString(findRange(arr, 5))); // [-1, -1]
    }
}
```

### **Advanced Binary Search Problems**

```java
import java.util.*;

public class AdvancedBinarySearch {
    
    /**
     * Search in rotated sorted array
     * Pattern: Binary search with pivot detection
     * Time: O(log n), Space: O(1)
     */
    public static int searchRotated(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] == target) {
                return mid;
            }
            
            // Determine which half is sorted
            if (nums[left] <= nums[mid]) {
                // Left half is sorted
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1; // Target is in left half
                } else {
                    left = mid + 1; // Target is in right half
                }
            } else {
                // Right half is sorted
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1; // Target is in right half
                } else {
                    right = mid - 1; // Target is in left half
                }
            }
        }
        
        return -1;
    }
    
    /**
     * Find peak element (element greater than neighbors)
     * Pattern: Binary search on unsorted array using peak property
     * Time: O(log n), Space: O(1)
     */
    public static int findPeakElement(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[mid + 1]) {
                // Peak is in left half (including mid)
                right = mid;
            } else {
                // Peak is in right half
                left = mid + 1;
            }
        }
        
        return left;
    }
    
    /**
     * Find minimum in rotated sorted array
     * Pattern: Binary search to find rotation point
     * Time: O(log n), Space: O(1)
     */
    public static int findMin(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] > nums[right]) {
                // Minimum is in right half
                left = mid + 1;
            } else {
                // Minimum is in left half (including mid)
                right = mid;
            }
        }
        
        return nums[left];
    }
    
    /**
     * Search for target in 2D matrix (sorted rows and columns)
     * Pattern: Binary search in 2D space
     * Time: O(log(m*n)), Space: O(1)
     */
    public static boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        int left = 0;
        int right = rows * cols - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int midValue = matrix[mid / cols][mid % cols];
            
            if (midValue == target) {
                return true;
            } else if (midValue < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return false;
    }
    
    /**
     * Find square root using binary search
     * Pattern: Binary search on answer space
     * Time: O(log n), Space: O(1)
     */
    public static int sqrt(int x) {
        if (x < 2) return x;
        
        long left = 2;
        long right = x / 2;
        
        while (left <= right) {
            long mid = left + (right - left) / 2;
            long square = mid * mid;
            
            if (square == x) {
                return (int) mid;
            } else if (square < x) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return (int) right;
    }
    
    /**
     * Capacity to ship packages within D days
     * Pattern: Binary search on capacity (answer space)
     * Time: O(n * log(sum)), Space: O(1)
     */
    public static int shipWithinDays(int[] weights, int days) {
        int left = Arrays.stream(weights).max().getAsInt(); // Min capacity (heaviest package)
        int right = Arrays.stream(weights).sum(); // Max capacity (all packages in one day)
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            if (canShipWithinDays(weights, days, mid)) {
                right = mid; // Try smaller capacity
            } else {
                left = mid + 1; // Need larger capacity
            }
        }
        
        return left;
    }
    
    private static boolean canShipWithinDays(int[] weights, int days, int capacity) {
        int daysNeeded = 1;
        int currentWeight = 0;
        
        for (int weight : weights) {
            if (currentWeight + weight > capacity) {
                daysNeeded++;
                currentWeight = weight;
            } else {
                currentWeight += weight;
            }
        }
        
        return daysNeeded <= days;
    }
    
    /**
     * Find K closest elements to target
     * Pattern: Binary search + sliding window
     * Time: O(log n + k), Space: O(1)
     */
    public static List<Integer> findClosestElements(int[] arr, int k, int x) {
        int left = 0;
        int right = arr.length - k;
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            
            // Compare distances to decide which side to choose
            if (x - arr[mid] > arr[mid + k] - x) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        List<Integer> result = new ArrayList<>();
        for (int i = left; i < left + k; i++) {
            result.add(arr[i]);
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        // Test rotated array search
        int[] rotated = {4, 5, 6, 7, 0, 1, 2};
        System.out.println("Search 0 in rotated: " + searchRotated(rotated, 0)); // 4
        
        // Test peak element
        int[] peaks = {1, 2, 3, 1};
        System.out.println("Peak element index: " + findPeakElement(peaks)); // 2
        
        // Test find minimum
        int[] rotatedMin = {3, 4, 5, 1, 2};
        System.out.println("Minimum in rotated: " + findMin(rotatedMin)); // 1
        
        // Test 2D matrix search
        int[][] matrix = {{1, 4, 7, 11}, {2, 5, 8, 12}, {3, 6, 9, 16}};
        System.out.println("Search 5 in matrix: " + searchMatrix(matrix, 5)); // true
        
        // Test square root
        System.out.println("Square root of 8: " + sqrt(8)); // 2
        
        // Test ship capacity
        int[] packages = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println("Ship capacity for 5 days: " + shipWithinDays(packages, 5)); // 15
        
        // Test closest elements
        int[] arr = {1, 2, 3, 4, 5};
        System.out.println("3 closest to 3: " + findClosestElements(arr, 3, 3)); // [2, 3, 4]
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Binary Search in Go**

```go
package main

import "fmt"

/**
 * Standard binary search - find exact match
 * Pattern: Classic divide and conquer
 * Time: O(log n), Space: O(1)
 */
func binarySearch(arr []int, target int) int {
    left := 0
    right := len(arr) - 1
    
    for left <= right {
        mid := left + (right-left)/2 // Avoid overflow
        
        if arr[mid] == target {
            return mid // Found target
        } else if arr[mid] < target {
            left = mid + 1 // Search right half
        } else {
            right = mid - 1 // Search left half
        }
    }
    
    return -1 // Target not found
}

/**
 * Recursive binary search
 * Pattern: Recursive divide and conquer
 * Time: O(log n), Space: O(log n) due to call stack
 */
func binarySearchRecursive(arr []int, target, left, right int) int {
    // Base case: search space exhausted
    if left > right {
        return -1
    }
    
    mid := left + (right-left)/2
    
    if arr[mid] == target {
        return mid
    } else if arr[mid] < target {
        return binarySearchRecursive(arr, target, mid+1, right)
    } else {
        return binarySearchRecursive(arr, target, left, mid-1)
    }
}

/**
 * Find insert position for target (where target should be inserted)
 * Pattern: Lower bound search
 * Time: O(log n), Space: O(1)
 */
func findInsertPosition(arr []int, target int) int {
    left := 0
    right := len(arr)
    
    for left < right {
        mid := left + (right-left)/2
        
        if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    return left
}

/**
 * Find first occurrence of target
 * Pattern: Lower bound with exact match check
 * Time: O(log n), Space: O(1)
 */
func findFirstOccurrence(arr []int, target int) int {
    left := 0
    right := len(arr) - 1
    result := -1
    
    for left <= right {
        mid := left + (right-left)/2
        
        if arr[mid] == target {
            result = mid
            right = mid - 1 // Continue searching left for first occurrence
        } else if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return result
}

/**
 * Find last occurrence of target
 * Pattern: Upper bound with exact match check
 * Time: O(log n), Space: O(1)
 */
func findLastOccurrence(arr []int, target int) int {
    left := 0
    right := len(arr) - 1
    result := -1
    
    for left <= right {
        mid := left + (right-left)/2
        
        if arr[mid] == target {
            result = mid
            left = mid + 1 // Continue searching right for last occurrence
        } else if arr[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return result
}

/**
 * Find range (first and last occurrence) of target
 * Pattern: Combine first and last occurrence searches
 * Time: O(log n), Space: O(1)
 */
func findRange(arr []int, target int) []int {
    first := findFirstOccurrence(arr, target)
    if first == -1 {
        return []int{-1, -1} // Target not found
    }
    
    last := findLastOccurrence(arr, target)
    return []int{first, last}
}

func main() {
    arr := []int{1, 2, 4, 4, 4, 6, 8, 10}
    
    fmt.Printf("Binary search for 4: %d\n", binarySearch(arr, 4))                                  // 2 (any occurrence)
    fmt.Printf("Recursive search for 6: %d\n", binarySearchRecursive(arr, 6, 0, len(arr)-1))       // 5
    fmt.Printf("Insert position for 5: %d\n", findInsertPosition(arr, 5))                          // 5
    fmt.Printf("First occurrence of 4: %d\n", findFirstOccurrence(arr, 4))                         // 2
    fmt.Printf("Last occurrence of 4: %d\n", findLastOccurrence(arr, 4))                           // 4
    fmt.Printf("Range of 4: %v\n", findRange(arr, 4))                                             // [2, 4]
    
    // Test with target not in array
    fmt.Printf("Range of 5: %v\n", findRange(arr, 5)) // [-1, -1]
}
```

### **Advanced Binary Search in Go**

```go
package main

import "fmt"

/**
 * Search in rotated sorted array
 * Pattern: Binary search with pivot detection
 * Time: O(log n), Space: O(1)
 */
func searchRotated(nums []int, target int) int {
    left := 0
    right := len(nums) - 1
    
    for left <= right {
        mid := left + (right-left)/2
        
        if nums[mid] == target {
            return mid
        }
        
        // Determine which half is sorted
        if nums[left] <= nums[mid] {
            // Left half is sorted
            if target >= nums[left] && target < nums[mid] {
                right = mid - 1 // Target is in left half
            } else {
                left = mid + 1 // Target is in right half
            }
        } else {
            // Right half is sorted
            if target > nums[mid] && target <= nums[right] {
                left = mid + 1 // Target is in right half
            } else {
                right = mid - 1 // Target is in left half
            }
        }
    }
    
    return -1
}

/**
 * Find peak element (element greater than neighbors)
 * Pattern: Binary search on unsorted array using peak property
 * Time: O(log n), Space: O(1)
 */
func findPeakElement(nums []int) int {
    left := 0
    right := len(nums) - 1
    
    for left < right {
        mid := left + (right-left)/2
        
        if nums[mid] > nums[mid+1] {
            // Peak is in left half (including mid)
            right = mid
        } else {
            // Peak is in right half
            left = mid + 1
        }
    }
    
    return left
}

/**
 * Find minimum in rotated sorted array
 * Pattern: Binary search to find rotation point
 * Time: O(log n), Space: O(1)
 */
func findMin(nums []int) int {
    left := 0
    right := len(nums) - 1
    
    for left < right {
        mid := left + (right-left)/2
        
        if nums[mid] > nums[right] {
            // Minimum is in right half
            left = mid + 1
        } else {
            // Minimum is in left half (including mid)
            right = mid
        }
    }
    
    return nums[left]
}

/**
 * Search for target in 2D matrix (sorted rows and columns)
 * Pattern: Binary search in 2D space
 * Time: O(log(m*n)), Space: O(1)
 */
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    
    rows := len(matrix)
    cols := len(matrix[0])
    left := 0
    right := rows*cols - 1
    
    for left <= right {
        mid := left + (right-left)/2
        midValue := matrix[mid/cols][mid%cols]
        
        if midValue == target {
            return true
        } else if midValue < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return false
}

/**
 * Find square root using binary search
 * Pattern: Binary search on answer space
 * Time: O(log n), Space: O(1)
 */
func sqrt(x int) int {
    if x < 2 {
        return x
    }
    
    left := 2
    right := x / 2
    
    for left <= right {
        mid := left + (right-left)/2
        square := mid * mid
        
        if square == x {
            return mid
        } else if square < x {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return right
}

/**
 * Capacity to ship packages within D days
 * Pattern: Binary search on capacity (answer space)
 * Time: O(n * log(sum)), Space: O(1)
 */
func shipWithinDays(weights []int, days int) int {
    left := maxInt(weights)  // Min capacity (heaviest package)
    right := sumInt(weights) // Max capacity (all packages in one day)
    
    for left < right {
        mid := left + (right-left)/2
        
        if canShipWithinDays(weights, days, mid) {
            right = mid // Try smaller capacity
        } else {
            left = mid + 1 // Need larger capacity
        }
    }
    
    return left
}

func canShipWithinDays(weights []int, days, capacity int) bool {
    daysNeeded := 1
    currentWeight := 0
    
    for _, weight := range weights {
        if currentWeight+weight > capacity {
            daysNeeded++
            currentWeight = weight
        } else {
            currentWeight += weight
        }
    }
    
    return daysNeeded <= days
}

func maxInt(arr []int) int {
    max := arr[0]
    for _, v := range arr[1:] {
        if v > max {
            max = v
        }
    }
    return max
}

func sumInt(arr []int) int {
    sum := 0
    for _, v := range arr {
        sum += v
    }
    return sum
}

/**
 * Find K closest elements to target
 * Pattern: Binary search + sliding window
 * Time: O(log n + k), Space: O(1)
 */
func findClosestElements(arr []int, k, x int) []int {
    left := 0
    right := len(arr) - k
    
    for left < right {
        mid := left + (right-left)/2
        
        // Compare distances to decide which side to choose
        if x-arr[mid] > arr[mid+k]-x {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    result := make([]int, k)
    for i := 0; i < k; i++ {
        result[i] = arr[left+i]
    }
    
    return result
}

func main() {
    // Test rotated array search
    rotated := []int{4, 5, 6, 7, 0, 1, 2}
    fmt.Printf("Search 0 in rotated: %d\n", searchRotated(rotated, 0)) // 4
    
    // Test peak element
    peaks := []int{1, 2, 3, 1}
    fmt.Printf("Peak element index: %d\n", findPeakElement(peaks)) // 2
    
    // Test find minimum
    rotatedMin := []int{3, 4, 5, 1, 2}
    fmt.Printf("Minimum in rotated: %d\n", findMin(rotatedMin)) // 1
    
    // Test 2D matrix search
    matrix := [][]int{{1, 4, 7, 11}, {2, 5, 8, 12}, {3, 6, 9, 16}}
    fmt.Printf("Search 5 in matrix: %t\n", searchMatrix(matrix, 5)) // true
    
    // Test square root
    fmt.Printf("Square root of 8: %d\n", sqrt(8)) // 2
    
    // Test ship capacity
    packages := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Printf("Ship capacity for 5 days: %d\n", shipWithinDays(packages, 5)) // 15
    
    // Test closest elements
    arr := []int{1, 2, 3, 4, 5}
    fmt.Printf("3 closest to 3: %v\n", findClosestElements(arr, 3, 3)) // [2, 3, 4]
}
```

---

## 🧠 **UNDERSTANDING BINARY SEARCH PATTERNS**

### **Pattern 1: Exact Search (Standard Binary Search)**
```java
int left = 0, right = arr.length - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
return -1;
```
**Use for:** Finding exact value in sorted array

### **Pattern 2: Lower Bound (First occurrence)**
```java
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target) left = mid + 1;
    else right = mid;
}
return left;
```
**Use for:** Insert position, first occurrence, smallest element ≥ target

### **Pattern 3: Upper Bound (After last occurrence)**
```java
int left = 0, right = arr.length;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] <= target) left = mid + 1;
    else right = mid;
}
return left;
```
**Use for:** Position after last occurrence, smallest element > target

### **Pattern 4: Search in Answer Space**
```java
int left = minPossibleAnswer, right = maxPossibleAnswer;
while (left < right) {
    int mid = left + (right - left) / 2;
    if (isPossible(mid)) right = mid;  // Try smaller
    else left = mid + 1;               // Need larger
}
return left;
```
**Use for:** Optimization problems, capacity problems, square root

---

## 🎯 **COMMON PITFALLS & SOLUTIONS**

### **🚨 Pitfall 1: Integer Overflow**
```java
// WRONG - can overflow for large values
int mid = (left + right) / 2;

// CORRECT - prevents overflow
int mid = left + (right - left) / 2;
```

### **🚨 Pitfall 2: Infinite Loop**
```java
// WRONG - can cause infinite loop
while (left < right) {
    int mid = left + (right - left) / 2;
    if (condition) left = mid;  // BUG: left might not advance
    else right = mid - 1;
}

// CORRECT - ensure progress
while (left < right) {
    int mid = left + (right - left) / 2;
    if (condition) right = mid;
    else left = mid + 1;
}
```

### **🚨 Pitfall 3: Off-by-One Errors**
```java
// For finding exact match
while (left <= right) // Use <=

// For finding boundaries  
while (left < right)  // Use <
```

### **🚨 Pitfall 4: Array Bounds**
```java
// Always check bounds when accessing neighbors
if (mid > 0 && mid < arr.length - 1) {
    // Safe to access arr[mid-1] and arr[mid+1]
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Binary Search** ✅ (Implemented above)
2. **Find Insert Position** ✅ (Implemented above)
3. **First Bad Version** (Similar to lower bound)
4. **Square Root** ✅ (Implemented above)
5. **Perfect Square** (Binary search on answer)

### **Medium Problems:**

1. **Search in Rotated Sorted Array** ✅ (Implemented above)
2. **Find Peak Element** ✅ (Implemented above)
3. **Search in 2D Matrix** ✅ (Implemented above)
4. **Find Minimum in Rotated Array** ✅ (Implemented above)
5. **Capacity to Ship Packages** ✅ (Implemented above)

### **Hard Problems:**

1. **Median of Two Sorted Arrays**
2. **Find K-th Smallest Element in Sorted Matrix**
3. **Split Array Largest Sum**
4. **Aggressive Cows** (Classic competitive programming)
5. **Allocate Minimum Number of Pages**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Basic Binary Search**
- [ ] Implement standard binary search from scratch
- [ ] Practice first/last occurrence problems
- [ ] Understand the three main patterns (exact, lower, upper bound)
- [ ] Avoid common pitfalls (overflow, infinite loops)

### **Day 2: Rotated Arrays & 2D Search**
- [ ] Solve search in rotated sorted array
- [ ] Practice finding minimum in rotated array
- [ ] Implement 2D matrix search
- [ ] Understand how to identify sorted halves

### **Day 3: Answer Space Binary Search**
- [ ] Solve square root problem
- [ ] Practice capacity/optimization problems
- [ ] Understand search in solution space concept
- [ ] Solve ship packages and similar problems

### **Day 4: Advanced Applications**
- [ ] Solve peak element finding
- [ ] Practice K closest elements
- [ ] Combine binary search with other techniques
- [ ] Solve 5 medium-level problems on LeetCode

### **✅ You're Ready for Next Topic When:**
- [ ] You can identify when to use binary search instantly
- [ ] You know which of the 4 patterns to apply
- [ ] You can implement solutions without looking up syntax
- [ ] You understand search space vs element space

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Linked Lists - Detailed](./08-linked-lists-detailed-java-go.md)** - Master dynamic data structures
2. **[Trees - Detailed](./09-trees-detailed-java-go.md)** - Learn hierarchical structures
3. **[Dynamic Programming - Basics](./10-dynamic-programming-basics-detailed-java-go.md)** - Optimization algorithms

### **🎯 Advanced Binary Search Topics:**
- **Ternary Search** for unimodal functions
- **Binary Search on Floating Point** numbers
- **Parallel Binary Search** for multiple queries
- **Binary Search Trees** implementation and operations

### **💡 Pro Tips:**
- **Visualize the search space** - Draw it on paper
- **Check boundary conditions** - Empty arrays, single elements
- **Test with simple examples** - Small arrays first
- **Master the patterns** - Don't memorize, understand the logic
- **Practice edge cases** - Duplicates, rotated arrays, 2D matrices

**🎉 You now master one of the most fundamental and powerful algorithms in computer science! Binary search will help you solve optimization problems efficiently and is essential for many advanced algorithms.**