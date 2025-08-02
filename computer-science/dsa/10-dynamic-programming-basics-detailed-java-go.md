# 🎯 **DYNAMIC PROGRAMMING: DETAILED GUIDE (JAVA & GO)**

**Master the Art of Optimization - From Recursion to Efficient Solutions**

*Prerequisites: Complete recursion, arrays, and basic algorithms first*

---

## 🤔 **WHAT IS DYNAMIC PROGRAMMING?**

### **Simple Explanation:**
Dynamic Programming is like **smart homework copying**:
- **Problem:** Calculate something complex repeatedly
- **Naive approach:** Redo the same calculations over and over
- **DP approach:** Write down answers as you solve them, look them up later
- **Result:** Solve in fraction of the time

### **Real-World Example - Climbing Stairs:**

**Problem:** How many ways to climb 5 stairs if you can take 1 or 2 steps at a time?

**Naive Recursion (slow):**
```
ways(5) = ways(4) + ways(3)
ways(4) = ways(3) + ways(2)  
ways(3) = ways(2) + ways(1)  [calculated multiple times!]
ways(2) = ways(1) + ways(0)  [calculated multiple times!]
...
```

**Dynamic Programming (fast):**
```
ways(0) = 1 ✓ (memo[0] = 1)
ways(1) = 1 ✓ (memo[1] = 1) 
ways(2) = 2 ✓ (memo[2] = 2)
ways(3) = 3 ✓ (memo[3] = 3) [looked up memo[2] + memo[1]]
ways(4) = 5 ✓ (memo[4] = 5) [looked up memo[3] + memo[2]]
ways(5) = 8 ✓ (memo[5] = 8) [looked up memo[4] + memo[3]]
```

### **When to Use Dynamic Programming:**
1. **Overlapping subproblems** - Same calculations repeated
2. **Optimal substructure** - Optimal solution contains optimal subsolutions
3. **Optimization problems** - Find minimum/maximum/count
4. **Decision problems** - Can this be done? What's the best way?

---

## 🎯 **DYNAMIC PROGRAMMING PATTERNS**

### **Pattern 1: Memoization (Top-Down)**
```
Start from main problem → Break into subproblems → Store results → Reuse
```
- **Recursive approach** with caching
- **Easy to implement** from recursive solution
- **Natural thinking** process

### **Pattern 2: Tabulation (Bottom-Up)**
```
Start from base cases → Build up to main problem → Use previous results
```
- **Iterative approach** with table
- **Space efficient** (often)
- **Better performance** (no recursion overhead)

### **Pattern 3: Space Optimization**
```
Only keep what you need → Reduce space from O(n) to O(1)
```
- **Identify dependencies** in the recurrence
- **Keep only necessary** previous values
- **Rolling array** technique

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Classic DP Problems - Fundamentals**

```java
import java.util.*;

public class DynamicProgrammingBasics {
    
    /**
     * Fibonacci - Classic DP Example
     * Pattern: Memoization to avoid recalculation
     * Time: O(n), Space: O(n)
     */
    
    // Naive recursion - O(2^n) time
    public static int fibonacciNaive(int n) {
        if (n <= 1) return n;
        return fibonacciNaive(n - 1) + fibonacciNaive(n - 2);
    }
    
    // Memoization - O(n) time, O(n) space
    public static int fibonacciMemo(int n) {
        Map<Integer, Integer> memo = new HashMap<>();
        return fibonacciMemoHelper(n, memo);
    }
    
    private static int fibonacciMemoHelper(int n, Map<Integer, Integer> memo) {
        if (memo.containsKey(n)) {
            return memo.get(n);
        }
        
        if (n <= 1) {
            return n;
        }
        
        int result = fibonacciMemoHelper(n - 1, memo) + fibonacciMemoHelper(n - 2, memo);
        memo.put(n, result);
        return result;
    }
    
    // Tabulation - O(n) time, O(n) space
    public static int fibonacciTabulation(int n) {
        if (n <= 1) return n;
        
        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }
    
    // Space optimized - O(n) time, O(1) space
    public static int fibonacciOptimized(int n) {
        if (n <= 1) return n;
        
        int prev2 = 0;
        int prev1 = 1;
        
        for (int i = 2; i <= n; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    /**
     * Climbing Stairs - Count ways to reach top
     * Pattern: Each step depends on previous steps
     * Time: O(n), Space: O(1)
     */
    public static int climbStairs(int n) {
        if (n <= 2) return n;
        
        int oneStepBefore = 2;
        int twoStepsBefore = 1;
        
        for (int i = 3; i <= n; i++) {
            int current = oneStepBefore + twoStepsBefore;
            twoStepsBefore = oneStepBefore;
            oneStepBefore = current;
        }
        
        return oneStepBefore;
    }
    
    /**
     * House Robber - Maximum money without robbing adjacent houses
     * Pattern: Include/exclude decision with constraint
     * Time: O(n), Space: O(1)
     */
    public static int rob(int[] nums) {
        if (nums.length == 0) return 0;
        if (nums.length == 1) return nums[0];
        
        int prev2 = nums[0];
        int prev1 = Math.max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.length; i++) {
            int current = Math.max(prev1, prev2 + nums[i]);
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    /**
     * Coin Change - Minimum coins to make amount
     * Pattern: Try each coin, take minimum
     * Time: O(amount * coins), Space: O(amount)
     */
    public static int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1); // Initialize with impossible value
        dp[0] = 0;
        
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        return dp[amount] > amount ? -1 : dp[amount];
    }
    
    /**
     * Longest Increasing Subsequence
     * Pattern: For each element, find best previous element
     * Time: O(n²), Space: O(n)
     */
    public static int lengthOfLIS(int[] nums) {
        if (nums.length == 0) return 0;
        
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);
        int maxLength = 1;
        
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            maxLength = Math.max(maxLength, dp[i]);
        }
        
        return maxLength;
    }
    
    public static void main(String[] args) {
        // Test Fibonacci
        int n = 10;
        System.out.println("Fibonacci(" + n + "):");
        System.out.println("  Memoization: " + fibonacciMemo(n));           // 55
        System.out.println("  Tabulation: " + fibonacciTabulation(n));      // 55
        System.out.println("  Optimized: " + fibonacciOptimized(n));        // 55
        
        // Test climbing stairs
        System.out.println("Climb 5 stairs: " + climbStairs(5));            // 8
        
        // Test house robber
        int[] houses = {2, 7, 9, 3, 1};
        System.out.println("Max rob amount: " + rob(houses));               // 12
        
        // Test coin change
        int[] coins = {1, 3, 4};
        System.out.println("Min coins for 6: " + coinChange(coins, 6));     // 2
        
        // Test LIS
        int[] sequence = {10, 9, 2, 5, 3, 7, 101, 18};
        System.out.println("LIS length: " + lengthOfLIS(sequence));         // 4
    }
}
```

### **2D Dynamic Programming**

```java
import java.util.*;

public class TwoDimensionalDP {
    
    /**
     * Unique Paths - Count paths from top-left to bottom-right
     * Pattern: Grid DP, paths from adjacent cells
     * Time: O(m*n), Space: O(m*n)
     */
    public static int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        
        // Initialize first row and column
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 0; j < n; j++) dp[0][j] = 1;
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];
    }
    
    /**
     * Unique Paths with Obstacles
     * Pattern: Grid DP with constraints
     * Time: O(m*n), Space: O(m*n)
     */
    public static int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        if (obstacleGrid[0][0] == 1) return 0;
        
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        
        // Initialize first column
        for (int i = 1; i < m; i++) {
            dp[i][0] = (obstacleGrid[i][0] == 0 && dp[i-1][0] == 1) ? 1 : 0;
        }
        
        // Initialize first row
        for (int j = 1; j < n; j++) {
            dp[0][j] = (obstacleGrid[0][j] == 0 && dp[0][j-1] == 1) ? 1 : 0;
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        
        return dp[m-1][n-1];
    }
    
    /**
     * Minimum Path Sum - Find path with minimum sum from top-left to bottom-right
     * Pattern: Grid DP with optimization
     * Time: O(m*n), Space: O(m*n)
     */
    public static int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        
        // Initialize first row
        for (int j = 1; j < n; j++) {
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }
        
        // Initialize first column
        for (int i = 1; i < m; i++) {
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        
        return dp[m-1][n-1];
    }
    
    /**
     * Longest Common Subsequence
     * Pattern: String DP, match or skip
     * Time: O(m*n), Space: O(m*n)
     */
    public static int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i-1) == text2.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        
        return dp[m][n];
    }
    
    /**
     * Edit Distance (Levenshtein Distance)
     * Pattern: String transformation DP
     * Time: O(m*n), Space: O(m*n)
     */
    public static int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        
        int[][] dp = new int[m + 1][n + 1];
        
        // Initialize base cases
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = 1 + Math.min(
                        Math.min(dp[i-1][j], dp[i][j-1]),  // Delete or Insert
                        dp[i-1][j-1]                        // Replace
                    );
                }
            }
        }
        
        return dp[m][n];
    }
    
    /**
     * 0/1 Knapsack Problem
     * Pattern: Decision DP, include or exclude
     * Time: O(n*W), Space: O(n*W)
     */
    public static int knapsack(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        int[][] dp = new int[n + 1][capacity + 1];
        
        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= capacity; w++) {
                if (weights[i-1] <= w) {
                    // Can include current item
                    dp[i][w] = Math.max(
                        dp[i-1][w],                           // Don't include
                        dp[i-1][w - weights[i-1]] + values[i-1]  // Include
                    );
                } else {
                    // Can't include current item
                    dp[i][w] = dp[i-1][w];
                }
            }
        }
        
        return dp[n][capacity];
    }
    
    public static void main(String[] args) {
        // Test unique paths
        System.out.println("Unique paths 3x7: " + uniquePaths(3, 7));       // 28
        
        // Test min path sum
        int[][] grid = {{1,3,1},{1,5,1},{4,2,1}};
        System.out.println("Min path sum: " + minPathSum(grid));             // 7
        
        // Test LCS
        System.out.println("LCS of 'abcde' and 'ace': " + 
                          longestCommonSubsequence("abcde", "ace"));         // 3
        
        // Test edit distance
        System.out.println("Edit distance 'horse' to 'ros': " + 
                          minDistance("horse", "ros"));                      // 3
        
        // Test knapsack
        int[] weights = {1, 3, 4, 5};
        int[] values = {1, 4, 5, 7};
        System.out.println("Knapsack max value: " + knapsack(weights, values, 7));  // 9
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Dynamic Programming in Go**

```go
package main

import "fmt"

/**
 * Fibonacci - Classic DP Example
 * Pattern: Memoization to avoid recalculation
 * Time: O(n), Space: O(n)
 */

// Naive recursion - O(2^n) time
func fibonacciNaive(n int) int {
    if n <= 1 {
        return n
    }
    return fibonacciNaive(n-1) + fibonacciNaive(n-2)
}

// Memoization - O(n) time, O(n) space
func fibonacciMemo(n int) int {
    memo := make(map[int]int)
    return fibonacciMemoHelper(n, memo)
}

func fibonacciMemoHelper(n int, memo map[int]int) int {
    if result, exists := memo[n]; exists {
        return result
    }
    
    if n <= 1 {
        return n
    }
    
    result := fibonacciMemoHelper(n-1, memo) + fibonacciMemoHelper(n-2, memo)
    memo[n] = result
    return result
}

// Tabulation - O(n) time, O(n) space
func fibonacciTabulation(n int) int {
    if n <= 1 {
        return n
    }
    
    dp := make([]int, n+1)
    dp[0] = 0
    dp[1] = 1
    
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }
    
    return dp[n]
}

// Space optimized - O(n) time, O(1) space
func fibonacciOptimized(n int) int {
    if n <= 1 {
        return n
    }
    
    prev2 := 0
    prev1 := 1
    
    for i := 2; i <= n; i++ {
        current := prev1 + prev2
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}

/**
 * Climbing Stairs - Count ways to reach top
 * Pattern: Each step depends on previous steps
 * Time: O(n), Space: O(1)
 */
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }
    
    oneStepBefore := 2
    twoStepsBefore := 1
    
    for i := 3; i <= n; i++ {
        current := oneStepBefore + twoStepsBefore
        twoStepsBefore = oneStepBefore
        oneStepBefore = current
    }
    
    return oneStepBefore
}

/**
 * House Robber - Maximum money without robbing adjacent houses
 * Pattern: Include/exclude decision with constraint
 * Time: O(n), Space: O(1)
 */
func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    
    prev2 := nums[0]
    prev1 := max(nums[0], nums[1])
    
    for i := 2; i < len(nums); i++ {
        current := max(prev1, prev2+nums[i])
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}

/**
 * Coin Change - Minimum coins to make amount
 * Pattern: Try each coin, take minimum
 * Time: O(amount * coins), Space: O(amount)
 */
func coinChange(coins []int, amount int) int {
    dp := make([]int, amount+1)
    for i := range dp {
        dp[i] = amount + 1 // Initialize with impossible value
    }
    dp[0] = 0
    
    for i := 1; i <= amount; i++ {
        for _, coin := range coins {
            if coin <= i {
                dp[i] = min(dp[i], dp[i-coin]+1)
            }
        }
    }
    
    if dp[amount] > amount {
        return -1
    }
    return dp[amount]
}

/**
 * Longest Increasing Subsequence
 * Pattern: For each element, find best previous element
 * Time: O(n²), Space: O(n)
 */
func lengthOfLIS(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    
    dp := make([]int, len(nums))
    for i := range dp {
        dp[i] = 1
    }
    
    maxLength := 1
    
    for i := 1; i < len(nums); i++ {
        for j := 0; j < i; j++ {
            if nums[j] < nums[i] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxLength = max(maxLength, dp[i])
    }
    
    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    // Test Fibonacci
    n := 10
    fmt.Printf("Fibonacci(%d):\n", n)
    fmt.Printf("  Memoization: %d\n", fibonacciMemo(n))           // 55
    fmt.Printf("  Tabulation: %d\n", fibonacciTabulation(n))      // 55
    fmt.Printf("  Optimized: %d\n", fibonacciOptimized(n))        // 55
    
    // Test climbing stairs
    fmt.Printf("Climb 5 stairs: %d\n", climbStairs(5))            // 8
    
    // Test house robber
    houses := []int{2, 7, 9, 3, 1}
    fmt.Printf("Max rob amount: %d\n", rob(houses))               // 12
    
    // Test coin change
    coins := []int{1, 3, 4}
    fmt.Printf("Min coins for 6: %d\n", coinChange(coins, 6))     // 2
    
    // Test LIS
    sequence := []int{10, 9, 2, 5, 3, 7, 101, 18}
    fmt.Printf("LIS length: %d\n", lengthOfLIS(sequence))         // 4
}
```

### **2D Dynamic Programming in Go**

```go
package main

import "fmt"

/**
 * Unique Paths - Count paths from top-left to bottom-right
 * Pattern: Grid DP, paths from adjacent cells
 * Time: O(m*n), Space: O(m*n)
 */
func uniquePaths(m, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    
    // Initialize first row and column
    for i := 0; i < m; i++ {
        dp[i][0] = 1
    }
    for j := 0; j < n; j++ {
        dp[0][j] = 1
    }
    
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }
    
    return dp[m-1][n-1]
}

/**
 * Minimum Path Sum - Find path with minimum sum
 * Pattern: Grid DP with optimization
 * Time: O(m*n), Space: O(m*n)
 */
func minPathSum(grid [][]int) int {
    m := len(grid)
    n := len(grid[0])
    
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    
    dp[0][0] = grid[0][0]
    
    // Initialize first row
    for j := 1; j < n; j++ {
        dp[0][j] = dp[0][j-1] + grid[0][j]
    }
    
    // Initialize first column
    for i := 1; i < m; i++ {
        dp[i][0] = dp[i-1][0] + grid[i][0]
    }
    
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
        }
    }
    
    return dp[m-1][n-1]
}

/**
 * Longest Common Subsequence
 * Pattern: String DP, match or skip
 * Time: O(m*n), Space: O(m*n)
 */
func longestCommonSubsequence(text1, text2 string) int {
    m := len(text1)
    n := len(text2)
    
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }
    
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                dp[i][j] = dp[i-1][j-1] + 1
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }
    
    return dp[m][n]
}

/**
 * Edit Distance (Levenshtein Distance)
 * Pattern: String transformation DP
 * Time: O(m*n), Space: O(m*n)
 */
func minDistance(word1, word2 string) int {
    m := len(word1)
    n := len(word2)
    
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }
    
    // Initialize base cases
    for i := 0; i <= m; i++ {
        dp[i][0] = i
    }
    for j := 0; j <= n; j++ {
        dp[0][j] = j
    }
    
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if word1[i-1] == word2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = 1 + min(
                    min(dp[i-1][j], dp[i][j-1]),  // Delete or Insert
                    dp[i-1][j-1])                 // Replace
            }
        }
    }
    
    return dp[m][n]
}

/**
 * 0/1 Knapsack Problem
 * Pattern: Decision DP, include or exclude
 * Time: O(n*W), Space: O(n*W)
 */
func knapsack(weights, values []int, capacity int) int {
    n := len(weights)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, capacity+1)
    }
    
    for i := 1; i <= n; i++ {
        for w := 1; w <= capacity; w++ {
            if weights[i-1] <= w {
                // Can include current item
                dp[i][w] = max(
                    dp[i-1][w],                                      // Don't include
                    dp[i-1][w-weights[i-1]]+values[i-1])           // Include
            } else {
                // Can't include current item
                dp[i][w] = dp[i-1][w]
            }
        }
    }
    
    return dp[n][capacity]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    // Test unique paths
    fmt.Printf("Unique paths 3x7: %d\n", uniquePaths(3, 7))       // 28
    
    // Test min path sum
    grid := [][]int{{1, 3, 1}, {1, 5, 1}, {4, 2, 1}}
    fmt.Printf("Min path sum: %d\n", minPathSum(grid))             // 7
    
    // Test LCS
    fmt.Printf("LCS of 'abcde' and 'ace': %d\n", 
               longestCommonSubsequence("abcde", "ace"))         // 3
    
    // Test edit distance
    fmt.Printf("Edit distance 'horse' to 'ros': %d\n", 
               minDistance("horse", "ros"))                      // 3
    
    // Test knapsack
    weights := []int{1, 3, 4, 5}
    values := []int{1, 4, 5, 7}
    fmt.Printf("Knapsack max value: %d\n", knapsack(weights, values, 7))  // 9
}
```

---

## 🧠 **UNDERSTANDING DP PATTERNS**

### **Pattern 1: Linear DP**
```java
// State: dp[i] represents optimal solution for first i elements
for (int i = 1; i <= n; i++) {
    dp[i] = optimalChoice(dp[i-1], dp[i-2], ..., current[i]);
}
```
**Examples:** Fibonacci, Climbing Stairs, House Robber

### **Pattern 2: Grid DP**
```java
// State: dp[i][j] represents optimal solution for subgrid (0,0) to (i,j)
for (int i = 0; i < m; i++) {
    for (int j = 0; j < n; j++) {
        dp[i][j] = optimalChoice(dp[i-1][j], dp[i][j-1], grid[i][j]);
    }
}
```
**Examples:** Unique Paths, Min Path Sum, Grid Traveler

### **Pattern 3: String DP**
```java
// State: dp[i][j] represents optimal solution for text1[0..i-1] and text2[0..j-1]
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        if (text1[i-1] == text2[j-1]) {
            dp[i][j] = dp[i-1][j-1] + operation;
        } else {
            dp[i][j] = optimalChoice(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]);
        }
    }
}
```
**Examples:** LCS, Edit Distance, Palindrome Subsequences

### **Pattern 4: Decision DP**
```java
// State: dp[i][j] represents optimal solution using first i items with constraint j
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= constraint; j++) {
        dp[i][j] = optimalChoice(
            dp[i-1][j],                    // Don't take item i
            dp[i-1][j-cost[i]] + value[i]  // Take item i
        );
    }
}
```
**Examples:** Knapsack, Coin Change, Partition Problems

---

## 📊 **DP OPTIMIZATION TECHNIQUES**

### **Space Optimization:**

**Before (O(n) space):**
```java
int[] dp = new int[n];
for (int i = 1; i < n; i++) {
    dp[i] = dp[i-1] + dp[i-2];
}
```

**After (O(1) space):**
```java
int prev2 = dp[0], prev1 = dp[1];
for (int i = 2; i < n; i++) {
    int current = prev1 + prev2;
    prev2 = prev1;
    prev1 = current;
}
```

### **Rolling Array (2D → 1D):**

**Before (O(m*n) space):**
```java
int[][] dp = new int[m][n];
for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[i][j] = dp[i-1][j] + dp[i][j-1];
    }
}
```

**After (O(n) space):**
```java
int[] dp = new int[n];
for (int i = 1; i < m; i++) {
    for (int j = 1; j < n; j++) {
        dp[j] = dp[j] + dp[j-1];  // dp[j] = previous row, dp[j-1] = current row
    }
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Climbing Stairs** ✅ (Implemented above)
2. **House Robber** ✅ (Implemented above)
3. **Maximum Subarray** (Kadane's Algorithm)
4. **Best Time to Buy and Sell Stock**
5. **Pascal's Triangle**

### **Medium Problems:**

1. **Coin Change** ✅ (Implemented above)
2. **Longest Common Subsequence** ✅ (Implemented above)
3. **Unique Paths** ✅ (Implemented above)
4. **Longest Increasing Subsequence** ✅ (Implemented above)
5. **Word Break**

### **Hard Problems:**

1. **Edit Distance** ✅ (Implemented above)
2. **0/1 Knapsack** ✅ (Implemented above)
3. **Regular Expression Matching**
4. **Burst Balloons**
5. **Palindrome Partitioning II**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master the Fundamentals**
- [ ] Understand overlapping subproblems concept
- [ ] Practice converting recursion to memoization
- [ ] Solve Fibonacci in all three ways (naive, memo, tabulation)
- [ ] Master space optimization techniques

### **Day 2: Linear DP Mastery**
- [ ] Solve climbing stairs and house robber
- [ ] Practice identifying state transitions
- [ ] Master the pattern: dp[i] = f(dp[i-1], dp[i-2], ...)
- [ ] Solve 3-5 linear DP problems

### **Day 3: 2D DP Introduction**
- [ ] Solve unique paths and min path sum
- [ ] Understand grid-based state representation
- [ ] Practice the pattern: dp[i][j] = f(dp[i-1][j], dp[i][j-1])
- [ ] Learn space optimization for 2D problems

### **Day 4: String DP & Advanced**
- [ ] Solve LCS and edit distance
- [ ] Understand string comparison patterns
- [ ] Practice decision-based DP (knapsack)
- [ ] Solve 5+ medium DP problems on LeetCode

### **✅ You're Ready for Next Topic When:**
- [ ] You can identify DP problems instantly
- [ ] You know when to use memoization vs tabulation
- [ ] You can optimize space complexity
- [ ] You recognize common DP patterns

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Backtracking - Detailed](./11-backtracking-detailed-java-go.md)** - Learn exhaustive search
2. **[Sorting Algorithms - Detailed](./12-sorting-detailed-java-go.md)** - Master efficient sorting
3. **[Advanced DP - Detailed](./14-dp-advanced-detailed-java-go.md)** - Complex DP patterns

### **🎯 Advanced DP Topics:**
- **Interval DP** for range-based problems
- **Tree DP** for tree-based optimization
- **Digit DP** for number-based constraints
- **Probability DP** for expected value problems

### **💡 Pro Tips:**
- **Start with recursion** - Always write the recursive solution first
- **Identify overlapping subproblems** - Look for repeated calculations
- **Define state clearly** - What does dp[i] represent?
- **Handle base cases** - Initialize DP table correctly
- **Optimize step by step** - Recursion → Memoization → Tabulation → Space optimization

**🎉 You now master dynamic programming! This powerful technique will help you solve optimization problems efficiently and is essential for advanced algorithms.**