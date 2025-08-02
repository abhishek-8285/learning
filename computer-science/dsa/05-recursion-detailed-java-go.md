# 🔄 **RECURSION: DETAILED GUIDE (JAVA & GO)**

**The Art of Solving Problems by Breaking Them into Smaller Versions**

*Prerequisites: Complete basic data structures first*

---

## 🤔 **WHAT IS RECURSION?**

### **Simple Explanation:**
Recursion is like **Russian nesting dolls** (Matryoshka):
- **Big doll** contains a **smaller doll**
- **Smaller doll** contains an **even smaller doll** 
- Continue until you reach the **smallest doll** (can't be opened further)
- **Solution** = Put all dolls back together in reverse order

In programming:
- **Big problem** → Break into **smaller problem**
- **Smaller problem** → Break into **even smaller problem**
- Continue until **base case** (simplest problem you can solve directly)
- **Solution** = Combine all small solutions back up

### **Real-World Examples:**

**🏢 Company Hierarchy:**
- CEO asks VP: "How many employees do we have?"
- VP asks Directors: "How many employees in your departments?"
- Directors ask Managers: "How many employees in your teams?"
- Managers count their direct reports (base case)
- Numbers bubble back up: Manager → Director → VP → CEO

**📁 File System:**
- Count all files in a folder
- If item is a file: count = 1 (base case)
- If item is a folder: count = sum of all items inside
- Recursively count each subfolder

**🧮 Mathematical:**
- Factorial: 5! = 5 × 4! = 5 × 4 × 3! = 5 × 4 × 3 × 2 × 1
- Tree traversal: Process root, then recursively process left and right subtrees

### **Why Learn Recursion:**
- **Elegant solutions** to complex problems
- **Tree and graph algorithms** rely heavily on recursion
- **Divide and conquer** approach to problem-solving
- **Interview favorite** - appears in 40% of coding interviews
- **Foundation** for dynamic programming and backtracking

---

## 🎯 **ANATOMY OF RECURSION**

### **Every Recursive Function Has 3 Parts:**

1. **🛑 Base Case** - When to stop recursing
2. **🔄 Recursive Case** - How to break down the problem
3. **🔧 Work** - What to do at each level

```java
public int recursiveFunction(parameters) {
    // 1. BASE CASE - Stop condition
    if (simplest_problem) {
        return simple_solution;
    }
    
    // 2. RECURSIVE CASE - Break down problem
    smaller_problem = modify(parameters);
    
    // 3. WORK - Combine solutions
    return combine(
        recursiveFunction(smaller_problem),
        current_level_work
    );
}
```

### **Example: Factorial**

```java
public int factorial(int n) {
    // BASE CASE: factorial of 0 or 1 is 1
    if (n <= 1) {
        return 1;
    }
    
    // RECURSIVE CASE: n! = n × (n-1)!
    return n * factorial(n - 1);
}
```

**Call Stack Visualization:**
```
factorial(5)
├── 5 * factorial(4)
    ├── 4 * factorial(3)
        ├── 3 * factorial(2)
            ├── 2 * factorial(1)
                └── 1 (base case)
            └── 2 * 1 = 2
        └── 3 * 2 = 6
    └── 4 * 6 = 24
└── 5 * 24 = 120
```

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Basic Recursion Patterns**

```java
import java.util.*;

public class RecursionBasics {
    
    /**
     * Calculate factorial using recursion
     * Pattern: Single recursive call, mathematical reduction
     * Time: O(n), Space: O(n) - call stack
     */
    public static long factorial(int n) {
        // Base case
        if (n <= 1) {
            return 1;
        }
        
        // Recursive case
        return n * factorial(n - 1);
    }
    
    /**
     * Calculate Fibonacci number
     * Pattern: Two recursive calls, overlapping subproblems
     * Time: O(2^n), Space: O(n) - call stack depth
     */
    public static int fibonacci(int n) {
        // Base cases
        if (n <= 1) {
            return n;
        }
        
        // Recursive case
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
    
    /**
     * Optimized Fibonacci with memoization
     * Pattern: Recursion + caching to avoid recomputation
     * Time: O(n), Space: O(n)
     */
    public static int fibonacciMemo(int n) {
        return fibonacciMemoHelper(n, new HashMap<>());
    }
    
    private static int fibonacciMemoHelper(int n, Map<Integer, Integer> memo) {
        // Check if already computed
        if (memo.containsKey(n)) {
            return memo.get(n);
        }
        
        // Base cases
        if (n <= 1) {
            return n;
        }
        
        // Compute and store result
        int result = fibonacciMemoHelper(n - 1, memo) + fibonacciMemoHelper(n - 2, memo);
        memo.put(n, result);
        return result;
    }
    
    /**
     * Calculate power using recursion
     * Pattern: Divide and conquer for optimization
     * Time: O(log n), Space: O(log n)
     */
    public static double power(double base, int exponent) {
        // Base case
        if (exponent == 0) {
            return 1;
        }
        
        // Handle negative exponents
        if (exponent < 0) {
            return 1 / power(base, -exponent);
        }
        
        // Divide and conquer optimization
        if (exponent % 2 == 0) {
            double half = power(base, exponent / 2);
            return half * half;
        } else {
            return base * power(base, exponent - 1);
        }
    }
    
    /**
     * Sum all digits of a number
     * Pattern: Recursive digit extraction
     * Time: O(log n), Space: O(log n)
     */
    public static int sumDigits(int number) {
        // Handle negative numbers
        number = Math.abs(number);
        
        // Base case
        if (number < 10) {
            return number;
        }
        
        // Recursive case: last digit + sum of remaining digits
        return (number % 10) + sumDigits(number / 10);
    }
    
    /**
     * Reverse a string using recursion
     * Pattern: Process one character, recurse on rest
     * Time: O(n), Space: O(n)
     */
    public static String reverseString(String str) {
        // Base case
        if (str.length() <= 1) {
            return str;
        }
        
        // Recursive case: last char + reverse of rest
        return str.charAt(str.length() - 1) + reverseString(str.substring(0, str.length() - 1));
    }
    
    /**
     * Check if string is palindrome
     * Pattern: Compare ends, recurse on middle
     * Time: O(n), Space: O(n)
     */
    public static boolean isPalindrome(String str) {
        return isPalindromeHelper(str, 0, str.length() - 1);
    }
    
    private static boolean isPalindromeHelper(String str, int left, int right) {
        // Base case: single char or empty
        if (left >= right) {
            return true;
        }
        
        // Check current characters and recurse on inner substring
        return str.charAt(left) == str.charAt(right) && 
               isPalindromeHelper(str, left + 1, right - 1);
    }
    
    public static void main(String[] args) {
        System.out.println("Factorial(5): " + factorial(5)); // 120
        System.out.println("Fibonacci(10): " + fibonacci(10)); // 55
        System.out.println("Fibonacci Memo(10): " + fibonacciMemo(10)); // 55
        System.out.println("Power(2, 10): " + power(2, 10)); // 1024
        System.out.println("Sum digits(1234): " + sumDigits(1234)); // 10
        System.out.println("Reverse 'hello': " + reverseString("hello")); // "olleh"
        System.out.println("Is 'racecar' palindrome: " + isPalindrome("racecar")); // true
    }
}
```

### **Array and List Recursion**

```java
import java.util.*;

public class ArrayRecursion {
    
    /**
     * Find maximum element in array
     * Pattern: Compare current with max of rest
     * Time: O(n), Space: O(n)
     */
    public static int findMax(int[] arr, int index) {
        // Base case: last element
        if (index == arr.length - 1) {
            return arr[index];
        }
        
        // Recursive case: max of current and max of rest
        return Math.max(arr[index], findMax(arr, index + 1));
    }
    
    /**
     * Sum all elements in array
     * Pattern: Current element + sum of rest
     * Time: O(n), Space: O(n)
     */
    public static int sumArray(int[] arr, int index) {
        // Base case: past end of array
        if (index >= arr.length) {
            return 0;
        }
        
        // Recursive case: current + sum of rest
        return arr[index] + sumArray(arr, index + 1);
    }
    
    /**
     * Binary search using recursion
     * Pattern: Divide and conquer search
     * Time: O(log n), Space: O(log n)
     */
    public static int binarySearch(int[] arr, int target, int left, int right) {
        // Base case: element not found
        if (left > right) {
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        // Base case: found target
        if (arr[mid] == target) {
            return mid;
        }
        
        // Recursive cases: search left or right half
        if (target < arr[mid]) {
            return binarySearch(arr, target, left, mid - 1);
        } else {
            return binarySearch(arr, target, mid + 1, right);
        }
    }
    
    /**
     * Generate all subsets (power set)
     * Pattern: Include/exclude each element
     * Time: O(2^n), Space: O(2^n)
     */
    public static List<List<Integer>> generateSubsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        generateSubsetsHelper(nums, 0, new ArrayList<>(), result);
        return result;
    }
    
    private static void generateSubsetsHelper(int[] nums, int index, 
                                           List<Integer> current, List<List<Integer>> result) {
        // Base case: processed all elements
        if (index == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // Recursive case 1: exclude current element
        generateSubsetsHelper(nums, index + 1, current, result);
        
        // Recursive case 2: include current element
        current.add(nums[index]);
        generateSubsetsHelper(nums, index + 1, current, result);
        current.remove(current.size() - 1); // backtrack
    }
    
    /**
     * Generate all permutations
     * Pattern: Try each unused element at each position
     * Time: O(n!), Space: O(n!)
     */
    public static List<List<Integer>> generatePermutations(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        generatePermutationsHelper(nums, new ArrayList<>(), new boolean[nums.length], result);
        return result;
    }
    
    private static void generatePermutationsHelper(int[] nums, List<Integer> current, 
                                                 boolean[] used, List<List<Integer>> result) {
        // Base case: permutation complete
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // Try each unused element
        for (int i = 0; i < nums.length; i++) {
            if (!used[i]) {
                current.add(nums[i]);
                used[i] = true;
                generatePermutationsHelper(nums, current, used, result);
                current.remove(current.size() - 1); // backtrack
                used[i] = false; // backtrack
            }
        }
    }
    
    public static void main(String[] args) {
        int[] arr = {3, 1, 4, 1, 5, 9, 2, 6};
        
        System.out.println("Max element: " + findMax(arr, 0)); // 9
        System.out.println("Sum of array: " + sumArray(arr, 0)); // 31
        
        int[] sortedArr = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        System.out.println("Binary search for 5: " + binarySearch(sortedArr, 5, 0, sortedArr.length - 1)); // 4
        
        int[] nums = {1, 2, 3};
        System.out.println("Subsets: " + generateSubsets(nums));
        System.out.println("Permutations: " + generatePermutations(nums));
    }
}
```

### **Tree Recursion**

```java
import java.util.*;

// Binary Tree Node
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode(int val) {
        this.val = val;
    }
}

public class TreeRecursion {
    
    /**
     * Calculate height/depth of binary tree
     * Pattern: Max of left and right subtree heights + 1
     * Time: O(n), Space: O(h) where h is height
     */
    public static int treeHeight(TreeNode root) {
        // Base case: empty tree
        if (root == null) {
            return 0;
        }
        
        // Recursive case: 1 + max height of subtrees
        return 1 + Math.max(treeHeight(root.left), treeHeight(root.right));
    }
    
    /**
     * Count total nodes in binary tree
     * Pattern: 1 + count of left + count of right
     * Time: O(n), Space: O(h)
     */
    public static int countNodes(TreeNode root) {
        // Base case: empty tree
        if (root == null) {
            return 0;
        }
        
        // Recursive case: current node + left subtree + right subtree
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
    
    /**
     * Sum all values in binary tree
     * Pattern: Current value + sum of subtrees
     * Time: O(n), Space: O(h)
     */
    public static int sumTree(TreeNode root) {
        // Base case: empty tree
        if (root == null) {
            return 0;
        }
        
        // Recursive case: current + left sum + right sum
        return root.val + sumTree(root.left) + sumTree(root.right);
    }
    
    /**
     * Check if two trees are identical
     * Pattern: Compare roots and recursively compare subtrees
     * Time: O(min(n,m)), Space: O(min(h1,h2))
     */
    public static boolean isSameTree(TreeNode p, TreeNode q) {
        // Base case: both null
        if (p == null && q == null) {
            return true;
        }
        
        // Base case: one null, one not
        if (p == null || q == null) {
            return false;
        }
        
        // Recursive case: compare values and subtrees
        return p.val == q.val && 
               isSameTree(p.left, q.left) && 
               isSameTree(p.right, q.right);
    }
    
    /**
     * Inorder traversal (Left, Root, Right)
     * Pattern: Process left, current, right
     * Time: O(n), Space: O(h)
     */
    public static List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }
    
    private static void inorderHelper(TreeNode root, List<Integer> result) {
        if (root == null) {
            return;
        }
        
        inorderHelper(root.left, result);   // Left
        result.add(root.val);               // Root
        inorderHelper(root.right, result);  // Right
    }
    
    /**
     * Find maximum path sum (any node to any node)
     * Pattern: For each node, consider paths through it
     * Time: O(n), Space: O(h)
     */
    private static int maxPathSum = Integer.MIN_VALUE;
    
    public static int maxPathSum(TreeNode root) {
        maxPathSum = Integer.MIN_VALUE;
        maxPathSumHelper(root);
        return maxPathSum;
    }
    
    private static int maxPathSumHelper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        // Get max path sum from left and right (ignore negative paths)
        int leftMax = Math.max(0, maxPathSumHelper(root.left));
        int rightMax = Math.max(0, maxPathSumHelper(root.right));
        
        // Update global maximum (path through current node)
        int currentMax = root.val + leftMax + rightMax;
        maxPathSum = Math.max(maxPathSum, currentMax);
        
        // Return max path sum ending at current node
        return root.val + Math.max(leftMax, rightMax);
    }
    
    public static void main(String[] args) {
        // Create sample tree:     3
        //                        / \
        //                       9   20
        //                          /  \
        //                         15   7
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);
        
        System.out.println("Tree height: " + treeHeight(root)); // 3
        System.out.println("Node count: " + countNodes(root)); // 5
        System.out.println("Sum of tree: " + sumTree(root)); // 54
        System.out.println("Inorder traversal: " + inorderTraversal(root)); // [9, 3, 15, 20, 7]
        System.out.println("Max path sum: " + maxPathSum(root)); // 42 (15->20->7)
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Recursion in Go**

```go
package main

import "fmt"

/**
 * Calculate factorial using recursion
 * Pattern: Single recursive call, mathematical reduction
 * Time: O(n), Space: O(n) - call stack
 */
func factorial(n int) int64 {
    // Base case
    if n <= 1 {
        return 1
    }
    
    // Recursive case
    return int64(n) * factorial(n-1)
}

/**
 * Calculate Fibonacci number
 * Pattern: Two recursive calls, overlapping subproblems
 * Time: O(2^n), Space: O(n) - call stack depth
 */
func fibonacci(n int) int {
    // Base cases
    if n <= 1 {
        return n
    }
    
    // Recursive case
    return fibonacci(n-1) + fibonacci(n-2)
}

/**
 * Optimized Fibonacci with memoization
 * Pattern: Recursion + caching to avoid recomputation
 * Time: O(n), Space: O(n)
 */
func fibonacciMemo(n int) int {
    memo := make(map[int]int)
    return fibonacciMemoHelper(n, memo)
}

func fibonacciMemoHelper(n int, memo map[int]int) int {
    // Check if already computed
    if result, exists := memo[n]; exists {
        return result
    }
    
    // Base cases
    if n <= 1 {
        return n
    }
    
    // Compute and store result
    result := fibonacciMemoHelper(n-1, memo) + fibonacciMemoHelper(n-2, memo)
    memo[n] = result
    return result
}

/**
 * Calculate power using recursion
 * Pattern: Divide and conquer for optimization
 * Time: O(log n), Space: O(log n)
 */
func power(base float64, exponent int) float64 {
    // Base case
    if exponent == 0 {
        return 1
    }
    
    // Handle negative exponents
    if exponent < 0 {
        return 1 / power(base, -exponent)
    }
    
    // Divide and conquer optimization
    if exponent%2 == 0 {
        half := power(base, exponent/2)
        return half * half
    } else {
        return base * power(base, exponent-1)
    }
}

/**
 * Sum all digits of a number
 * Pattern: Recursive digit extraction
 * Time: O(log n), Space: O(log n)
 */
func sumDigits(number int) int {
    // Handle negative numbers
    if number < 0 {
        number = -number
    }
    
    // Base case
    if number < 10 {
        return number
    }
    
    // Recursive case: last digit + sum of remaining digits
    return (number % 10) + sumDigits(number/10)
}

/**
 * Reverse a string using recursion
 * Pattern: Process one character, recurse on rest
 * Time: O(n), Space: O(n)
 */
func reverseString(str string) string {
    // Base case
    if len(str) <= 1 {
        return str
    }
    
    // Recursive case: last char + reverse of rest
    return string(str[len(str)-1]) + reverseString(str[:len(str)-1])
}

/**
 * Check if string is palindrome
 * Pattern: Compare ends, recurse on middle
 * Time: O(n), Space: O(n)
 */
func isPalindrome(str string) bool {
    return isPalindromeHelper(str, 0, len(str)-1)
}

func isPalindromeHelper(str string, left, right int) bool {
    // Base case: single char or empty
    if left >= right {
        return true
    }
    
    // Check current characters and recurse on inner substring
    return str[left] == str[right] && isPalindromeHelper(str, left+1, right-1)
}

func main() {
    fmt.Printf("Factorial(5): %d\n", factorial(5))                    // 120
    fmt.Printf("Fibonacci(10): %d\n", fibonacci(10))                  // 55
    fmt.Printf("Fibonacci Memo(10): %d\n", fibonacciMemo(10))         // 55
    fmt.Printf("Power(2, 10): %.0f\n", power(2, 10))                  // 1024
    fmt.Printf("Sum digits(1234): %d\n", sumDigits(1234))             // 10
    fmt.Printf("Reverse 'hello': %s\n", reverseString("hello"))       // "olleh"
    fmt.Printf("Is 'racecar' palindrome: %t\n", isPalindrome("racecar")) // true
}
```

### **Array and Slice Recursion in Go**

```go
package main

import "fmt"

/**
 * Find maximum element in slice
 * Pattern: Compare current with max of rest
 * Time: O(n), Space: O(n)
 */
func findMax(arr []int, index int) int {
    // Base case: last element
    if index == len(arr)-1 {
        return arr[index]
    }
    
    // Recursive case: max of current and max of rest
    maxRest := findMax(arr, index+1)
    if arr[index] > maxRest {
        return arr[index]
    }
    return maxRest
}

/**
 * Sum all elements in slice
 * Pattern: Current element + sum of rest
 * Time: O(n), Space: O(n)
 */
func sumSlice(arr []int, index int) int {
    // Base case: past end of slice
    if index >= len(arr) {
        return 0
    }
    
    // Recursive case: current + sum of rest
    return arr[index] + sumSlice(arr, index+1)
}

/**
 * Binary search using recursion
 * Pattern: Divide and conquer search
 * Time: O(log n), Space: O(log n)
 */
func binarySearch(arr []int, target, left, right int) int {
    // Base case: element not found
    if left > right {
        return -1
    }
    
    mid := left + (right-left)/2
    
    // Base case: found target
    if arr[mid] == target {
        return mid
    }
    
    // Recursive cases: search left or right half
    if target < arr[mid] {
        return binarySearch(arr, target, left, mid-1)
    } else {
        return binarySearch(arr, target, mid+1, right)
    }
}

/**
 * Generate all subsets (power set)
 * Pattern: Include/exclude each element
 * Time: O(2^n), Space: O(2^n)
 */
func generateSubsets(nums []int) [][]int {
    var result [][]int
    var current []int
    generateSubsetsHelper(nums, 0, current, &result)
    return result
}

func generateSubsetsHelper(nums []int, index int, current []int, result *[][]int) {
    // Base case: processed all elements
    if index == len(nums) {
        // Create copy of current subset
        subset := make([]int, len(current))
        copy(subset, current)
        *result = append(*result, subset)
        return
    }
    
    // Recursive case 1: exclude current element
    generateSubsetsHelper(nums, index+1, current, result)
    
    // Recursive case 2: include current element
    current = append(current, nums[index])
    generateSubsetsHelper(nums, index+1, current, result)
    current = current[:len(current)-1] // backtrack
}

/**
 * Generate all permutations
 * Pattern: Try each unused element at each position
 * Time: O(n!), Space: O(n!)
 */
func generatePermutations(nums []int) [][]int {
    var result [][]int
    var current []int
    used := make([]bool, len(nums))
    generatePermutationsHelper(nums, current, used, &result)
    return result
}

func generatePermutationsHelper(nums []int, current []int, used []bool, result *[][]int) {
    // Base case: permutation complete
    if len(current) == len(nums) {
        // Create copy of current permutation
        perm := make([]int, len(current))
        copy(perm, current)
        *result = append(*result, perm)
        return
    }
    
    // Try each unused element
    for i := 0; i < len(nums); i++ {
        if !used[i] {
            current = append(current, nums[i])
            used[i] = true
            generatePermutationsHelper(nums, current, used, result)
            current = current[:len(current)-1] // backtrack
            used[i] = false                    // backtrack
        }
    }
}

func main() {
    arr := []int{3, 1, 4, 1, 5, 9, 2, 6}
    
    fmt.Printf("Max element: %d\n", findMax(arr, 0))         // 9
    fmt.Printf("Sum of slice: %d\n", sumSlice(arr, 0))       // 31
    
    sortedArr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
    fmt.Printf("Binary search for 5: %d\n", binarySearch(sortedArr, 5, 0, len(sortedArr)-1)) // 4
    
    nums := []int{1, 2, 3}
    fmt.Printf("Subsets: %v\n", generateSubsets(nums))
    fmt.Printf("Permutations: %v\n", generatePermutations(nums))
}
```

### **Tree Recursion in Go**

```go
package main

import (
    "fmt"
    "math"
)

// Binary Tree Node
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

/**
 * Calculate height/depth of binary tree
 * Pattern: Max of left and right subtree heights + 1
 * Time: O(n), Space: O(h) where h is height
 */
func treeHeight(root *TreeNode) int {
    // Base case: empty tree
    if root == nil {
        return 0
    }
    
    // Recursive case: 1 + max height of subtrees
    leftHeight := treeHeight(root.Left)
    rightHeight := treeHeight(root.Right)
    
    if leftHeight > rightHeight {
        return 1 + leftHeight
    }
    return 1 + rightHeight
}

/**
 * Count total nodes in binary tree
 * Pattern: 1 + count of left + count of right
 * Time: O(n), Space: O(h)
 */
func countNodes(root *TreeNode) int {
    // Base case: empty tree
    if root == nil {
        return 0
    }
    
    // Recursive case: current node + left subtree + right subtree
    return 1 + countNodes(root.Left) + countNodes(root.Right)
}

/**
 * Sum all values in binary tree
 * Pattern: Current value + sum of subtrees
 * Time: O(n), Space: O(h)
 */
func sumTree(root *TreeNode) int {
    // Base case: empty tree
    if root == nil {
        return 0
    }
    
    // Recursive case: current + left sum + right sum
    return root.Val + sumTree(root.Left) + sumTree(root.Right)
}

/**
 * Check if two trees are identical
 * Pattern: Compare roots and recursively compare subtrees
 * Time: O(min(n,m)), Space: O(min(h1,h2))
 */
func isSameTree(p, q *TreeNode) bool {
    // Base case: both nil
    if p == nil && q == nil {
        return true
    }
    
    // Base case: one nil, one not
    if p == nil || q == nil {
        return false
    }
    
    // Recursive case: compare values and subtrees
    return p.Val == q.Val && 
           isSameTree(p.Left, q.Left) && 
           isSameTree(p.Right, q.Right)
}

/**
 * Inorder traversal (Left, Root, Right)
 * Pattern: Process left, current, right
 * Time: O(n), Space: O(h)
 */
func inorderTraversal(root *TreeNode) []int {
    var result []int
    inorderHelper(root, &result)
    return result
}

func inorderHelper(root *TreeNode, result *[]int) {
    if root == nil {
        return
    }
    
    inorderHelper(root.Left, result)    // Left
    *result = append(*result, root.Val) // Root
    inorderHelper(root.Right, result)   // Right
}

/**
 * Find maximum path sum (any node to any node)
 * Pattern: For each node, consider paths through it
 * Time: O(n), Space: O(h)
 */
func maxPathSum(root *TreeNode) int {
    maxSum := math.MinInt32
    maxPathSumHelper(root, &maxSum)
    return maxSum
}

func maxPathSumHelper(root *TreeNode, maxSum *int) int {
    if root == nil {
        return 0
    }
    
    // Get max path sum from left and right (ignore negative paths)
    leftMax := max(0, maxPathSumHelper(root.Left, maxSum))
    rightMax := max(0, maxPathSumHelper(root.Right, maxSum))
    
    // Update global maximum (path through current node)
    currentMax := root.Val + leftMax + rightMax
    if currentMax > *maxSum {
        *maxSum = currentMax
    }
    
    // Return max path sum ending at current node
    return root.Val + max(leftMax, rightMax)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    // Create sample tree:     3
    //                        / \
    //                       9   20
    //                          /  \
    //                         15   7
    root := &TreeNode{Val: 3}
    root.Left = &TreeNode{Val: 9}
    root.Right = &TreeNode{Val: 20}
    root.Right.Left = &TreeNode{Val: 15}
    root.Right.Right = &TreeNode{Val: 7}
    
    fmt.Printf("Tree height: %d\n", treeHeight(root))            // 3
    fmt.Printf("Node count: %d\n", countNodes(root))             // 5
    fmt.Printf("Sum of tree: %d\n", sumTree(root))               // 54
    fmt.Printf("Inorder traversal: %v\n", inorderTraversal(root)) // [9, 3, 15, 20, 7]
    fmt.Printf("Max path sum: %d\n", maxPathSum(root))           // 42 (15->20->7)
}
```

---

## 🧠 **RECURSION PATTERNS & TEMPLATES**

### **Pattern 1: Linear Recursion**
```java
public ReturnType linearRecursion(parameters) {
    // Base case
    if (simplest_condition) {
        return base_result;
    }
    
    // Recursive case - single recursive call
    return combine(current_work, linearRecursion(modified_parameters));
}
```
**Examples:** Factorial, sum of array, tree height

### **Pattern 2: Binary Recursion**
```java
public ReturnType binaryRecursion(parameters) {
    // Base case
    if (simplest_condition) {
        return base_result;
    }
    
    // Recursive case - two recursive calls
    ReturnType left = binaryRecursion(left_parameters);
    ReturnType right = binaryRecursion(right_parameters);
    return combine(current_work, left, right);
}
```
**Examples:** Fibonacci, tree traversal, binary search

### **Pattern 3: Multiple Recursion**
```java
public void multipleRecursion(parameters) {
    // Base case
    if (simplest_condition) {
        process_base_case();
        return;
    }
    
    // Try all possibilities
    for (option in all_options) {
        make_choice(option);
        multipleRecursion(modified_parameters);
        unmake_choice(option); // backtrack
    }
}
```
**Examples:** Generate permutations, subsets, N-Queens

### **Pattern 4: Tail Recursion**
```java
public ReturnType tailRecursion(parameters, accumulator) {
    // Base case
    if (simplest_condition) {
        return accumulator;
    }
    
    // Recursive case - recursive call is last operation
    return tailRecursion(modified_parameters, updated_accumulator);
}
```
**Examples:** Factorial with accumulator, sum with accumulator

---

## 🎯 **RECURSION VS ITERATION**

### **When to Use Recursion:**
✅ **Tree/Graph traversal** - Natural recursive structure  
✅ **Divide and conquer** - Problem breaks into similar subproblems  
✅ **Backtracking** - Need to explore all possibilities  
✅ **Mathematical sequences** - Defined recursively (Fibonacci, factorial)  
✅ **Nested structures** - JSON parsing, directory traversal  

### **When to Use Iteration:**
✅ **Simple repetition** - Counting, basic loops  
✅ **Memory constraints** - Avoid call stack overhead  
✅ **Performance critical** - Avoid function call overhead  
✅ **Linear processing** - Array scanning, string building  

### **Conversion Example: Factorial**

**Recursive:**
```java
public int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

**Iterative:**
```java
public int factorial(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Factorial** ✅ (Implemented above)
2. **Sum of Array** ✅ (Implemented above)
3. **Reverse String** ✅ (Implemented above)
4. **Check Palindrome** ✅ (Implemented above)
5. **Tree Height** ✅ (Implemented above)

### **Medium Problems:**

1. **Generate Subsets** ✅ (Implemented above)
2. **Generate Permutations** ✅ (Implemented above)
3. **Binary Search** ✅ (Implemented above)
4. **Tree Traversals** ✅ (Implemented above)
5. **Fibonacci with Memoization** ✅ (Implemented above)

### **Hard Problems:**

1. **N-Queens Problem**
2. **Sudoku Solver**
3. **Maximum Path Sum in Binary Tree** ✅ (Implemented above)
4. **Word Search in Grid**
5. **Expression Evaluation**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master the Mindset**
- [ ] Understand the 3 parts of recursion (base, recursive, work)
- [ ] Implement factorial and Fibonacci from scratch
- [ ] Practice tracing through recursive calls manually
- [ ] Focus on identifying base cases correctly

### **Day 2: Linear Recursion**
- [ ] Implement array sum, max finding, string reversal
- [ ] Practice with tree height and node counting
- [ ] Convert 2-3 recursive solutions to iterative
- [ ] Understand call stack visualization

### **Day 3: Binary & Multiple Recursion**
- [ ] Implement binary search recursively
- [ ] Generate all subsets and permutations
- [ ] Practice tree traversals (inorder, preorder, postorder)
- [ ] Understand exponential time complexity

### **Day 4: Advanced Applications**
- [ ] Implement path sum problems in trees
- [ ] Practice backtracking with constraint satisfaction
- [ ] Add memoization to optimize recursive solutions
- [ ] Solve 3-5 recursive problems on LeetCode

### **✅ You're Ready for Next Topic When:**
- [ ] You can identify recursive patterns in problems
- [ ] You can write base cases correctly
- [ ] You understand time/space complexity of recursive solutions
- [ ] You can convert between recursive and iterative solutions

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Stack & Queue - Detailed](./06-stack-queue-detailed-java-go.md)** - Master linear data structures
2. **[Binary Search - Detailed](./07-binary-search-detailed-java-go.md)** - Learn search algorithms
3. **[Dynamic Programming](./10-dynamic-programming-basics-detailed-java-go.md)** - Optimize recursive solutions

### **🎯 Advanced Recursion Topics:**
- **Tail Recursion Optimization** - Compiler optimizations
- **Memoization vs Tabulation** - Top-down vs bottom-up approaches
- **Mutual Recursion** - Functions calling each other recursively
- **Tree Recursion with State** - Passing context through recursive calls

### **💡 Pro Tips:**
- **Start with base case** - Always think about when to stop first
- **Trust the recursion** - Assume recursive calls work correctly
- **Draw it out** - Visualize the call stack for complex problems
- **Add memoization** - Cache results to avoid recomputation
- **Consider iteration** - Sometimes iterative solutions are cleaner

**🎉 You now master the art of recursion! This fundamental technique will unlock powerful solutions to tree, graph, and backtracking problems.**