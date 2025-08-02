# 🔄 **BACKTRACKING: DETAILED GUIDE (JAVA & GO)**

**Master the Art of Exhaustive Search with Smart Pruning**

*Prerequisites: Complete recursion, trees, and basic algorithms first*

---

## 🤔 **WHAT IS BACKTRACKING?**

### **Simple Explanation:**
Backtracking is like **navigating a maze with breadcrumbs**:
- **Try a path** → Place a breadcrumb
- **Hit a dead end?** → **Backtrack** by following breadcrumbs
- **Remove the breadcrumb** → Try a different path
- **Repeat** until you find the exit or exhaust all paths

### **Real-World Example - Sudoku Solving:**

```
Try placing 1 in cell (0,0)
├── Try placing 2 in cell (0,1)
│   ├── Try placing 3 in cell (0,2) ✗ (violates rules)
│   ├── Try placing 4 in cell (0,2) ✗ (violates rules)
│   └── Try placing 5 in cell (0,2) ✓ (continue...)
├── Try placing 3 in cell (0,1)
│   └── ...
└── Backtrack if no valid numbers work
```

### **Backtracking vs Other Approaches:**

**🔄 Backtracking:**
- **Systematic exploration** of all possibilities
- **Prune invalid paths** early (smart optimization)
- **Undo choices** when they lead nowhere
- **Guarantee finding solution** if one exists

**🌊 Brute Force:**
- Generate ALL possible combinations
- Check each one individually
- No early pruning or optimization

**🎯 Greedy:**
- Make locally optimal choices
- Never reconsider previous decisions
- May miss the global optimum

### **When to Use Backtracking:**
1. **Constraint satisfaction** problems
2. **Generate all solutions** to a problem
3. **Find if solution exists** in decision trees
4. **Optimization** with multiple constraints
5. **Combinatorial** problems (permutations, combinations)

---

## 🎯 **BACKTRACKING TEMPLATE**

### **Universal Backtracking Pattern:**
```java
public void backtrack(State currentState, List<Solution> allSolutions) {
    // BASE CASE: Found a complete solution
    if (isComplete(currentState)) {
        allSolutions.add(new Solution(currentState));
        return;
    }
    
    // TRY all possible choices from current state
    for (Choice choice : getAllValidChoices(currentState)) {
        // MAKE the choice
        makeChoice(currentState, choice);
        
        // RECURSE with new state
        backtrack(currentState, allSolutions);
        
        // UNMAKE the choice (BACKTRACK)
        unmakeChoice(currentState, choice);
    }
}
```

### **Three Essential Steps:**
1. **MAKE** the choice (move forward)
2. **RECURSE** (explore deeper)
3. **UNMAKE** the choice (backtrack)

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Classic Backtracking Problems**

```java
import java.util.*;

public class BacktrackingProblems {
    
    /**
     * Generate All Permutations
     * Pattern: Choose/unchoose from remaining elements
     * Time: O(n! * n), Space: O(n)
     */
    public static List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> currentPermutation = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        
        permuteHelper(nums, currentPermutation, used, result);
        return result;
    }
    
    private static void permuteHelper(int[] nums, List<Integer> current, 
                                    boolean[] used, List<List<Integer>> result) {
        // BASE CASE: Permutation complete
        if (current.size() == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // TRY each unused number
        for (int i = 0; i < nums.length; i++) {
            if (!used[i]) {
                // MAKE choice
                current.add(nums[i]);
                used[i] = true;
                
                // RECURSE
                permuteHelper(nums, current, used, result);
                
                // UNMAKE choice (BACKTRACK)
                current.remove(current.size() - 1);
                used[i] = false;
            }
        }
    }
    
    /**
     * Generate All Subsets (Power Set)
     * Pattern: Include/exclude each element
     * Time: O(2^n * n), Space: O(n)
     */
    public static List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> currentSubset = new ArrayList<>();
        
        subsetsHelper(nums, 0, currentSubset, result);
        return result;
    }
    
    private static void subsetsHelper(int[] nums, int index, 
                                    List<Integer> current, List<List<Integer>> result) {
        // BASE CASE: Processed all elements
        if (index == nums.length) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // CHOICE 1: Exclude current element
        subsetsHelper(nums, index + 1, current, result);
        
        // CHOICE 2: Include current element
        current.add(nums[index]);
        subsetsHelper(nums, index + 1, current, result);
        current.remove(current.size() - 1); // BACKTRACK
    }
    
    /**
     * Combination Sum - Find all combinations that sum to target
     * Pattern: Try each number, allow repetition
     * Time: O(2^target), Space: O(target)
     */
    public static List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> currentCombination = new ArrayList<>();
        
        combinationSumHelper(candidates, target, 0, currentCombination, result);
        return result;
    }
    
    private static void combinationSumHelper(int[] candidates, int target, int start,
                                           List<Integer> current, List<List<Integer>> result) {
        // BASE CASE: Found valid combination
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // BASE CASE: Exceeded target
        if (target < 0) {
            return;
        }
        
        // TRY each candidate starting from 'start'
        for (int i = start; i < candidates.length; i++) {
            // MAKE choice
            current.add(candidates[i]);
            
            // RECURSE (can use same element again, so pass 'i')
            combinationSumHelper(candidates, target - candidates[i], i, current, result);
            
            // UNMAKE choice (BACKTRACK)
            current.remove(current.size() - 1);
        }
    }
    
    /**
     * Word Search - Find word in 2D grid
     * Pattern: DFS with visited tracking
     * Time: O(m*n*4^L), Space: O(L) where L is word length
     */
    public static boolean exist(char[][] board, String word) {
        int m = board.length;
        int n = board[0].length;
        
        // Try starting from each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (existHelper(board, word, i, j, 0)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private static boolean existHelper(char[][] board, String word, int row, int col, int index) {
        // BASE CASE: Found complete word
        if (index == word.length()) {
            return true;
        }
        
        // BASE CASE: Out of bounds or character doesn't match
        if (row < 0 || row >= board.length || col < 0 || col >= board[0].length ||
            board[row][col] != word.charAt(index)) {
            return false;
        }
        
        // MAKE choice (mark as visited)
        char originalChar = board[row][col];
        board[row][col] = '#'; // Mark as visited
        
        // RECURSE in all 4 directions
        boolean found = existHelper(board, word, row + 1, col, index + 1) ||
                       existHelper(board, word, row - 1, col, index + 1) ||
                       existHelper(board, word, row, col + 1, index + 1) ||
                       existHelper(board, word, row, col - 1, index + 1);
        
        // UNMAKE choice (BACKTRACK)
        board[row][col] = originalChar;
        
        return found;
    }
    
    /**
     * N-Queens Problem - Place N queens on N×N chessboard
     * Pattern: Row by row placement with constraint checking
     * Time: O(N!), Space: O(N)
     */
    public static List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];
        
        // Initialize board
        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }
        
        nQueensHelper(board, 0, result);
        return result;
    }
    
    private static void nQueensHelper(char[][] board, int row, List<List<String>> result) {
        // BASE CASE: Placed all queens
        if (row == board.length) {
            result.add(constructSolution(board));
            return;
        }
        
        // TRY placing queen in each column of current row
        for (int col = 0; col < board.length; col++) {
            if (isSafe(board, row, col)) {
                // MAKE choice
                board[row][col] = 'Q';
                
                // RECURSE to next row
                nQueensHelper(board, row + 1, result);
                
                // UNMAKE choice (BACKTRACK)
                board[row][col] = '.';
            }
        }
    }
    
    private static boolean isSafe(char[][] board, int row, int col) {
        int n = board.length;
        
        // Check column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }
        
        // Check diagonal (top-left)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        
        // Check diagonal (top-right)
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }
        
        return true;
    }
    
    private static List<String> constructSolution(char[][] board) {
        List<String> solution = new ArrayList<>();
        for (char[] row : board) {
            solution.add(new String(row));
        }
        return solution;
    }
    
    public static void main(String[] args) {
        // Test permutations
        int[] nums = {1, 2, 3};
        System.out.println("Permutations of [1,2,3]: " + permute(nums));
        
        // Test subsets
        System.out.println("Subsets of [1,2,3]: " + subsets(nums));
        
        // Test combination sum
        int[] candidates = {2, 3, 6, 7};
        System.out.println("Combination sum to 7: " + combinationSum(candidates, 7));
        
        // Test word search
        char[][] board = {
            {'A','B','C','E'},
            {'S','F','C','S'},
            {'A','D','E','E'}
        };
        System.out.println("Word 'ABCCED' exists: " + exist(board, "ABCCED"));
        
        // Test N-Queens
        System.out.println("4-Queens solutions: " + solveNQueens(4).size());
    }
}
```

### **Advanced Backtracking Problems**

```java
import java.util.*;

public class AdvancedBacktracking {
    
    /**
     * Sudoku Solver - Solve 9x9 Sudoku puzzle
     * Pattern: Try each number, check constraints
     * Time: O(9^(empty cells)), Space: O(1)
     */
    public static boolean solveSudoku(char[][] board) {
        return sudokuHelper(board);
    }
    
    private static boolean sudokuHelper(char[][] board) {
        // Find next empty cell
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                if (board[row][col] == '.') {
                    // TRY each digit 1-9
                    for (char digit = '1'; digit <= '9'; digit++) {
                        if (isValidSudoku(board, row, col, digit)) {
                            // MAKE choice
                            board[row][col] = digit;
                            
                            // RECURSE
                            if (sudokuHelper(board)) {
                                return true; // Found solution
                            }
                            
                            // UNMAKE choice (BACKTRACK)
                            board[row][col] = '.';
                        }
                    }
                    return false; // No valid digit found
                }
            }
        }
        return true; // All cells filled
    }
    
    private static boolean isValidSudoku(char[][] board, int row, int col, char digit) {
        // Check row
        for (int j = 0; j < 9; j++) {
            if (board[row][j] == digit) {
                return false;
            }
        }
        
        // Check column
        for (int i = 0; i < 9; i++) {
            if (board[i][col] == digit) {
                return false;
            }
        }
        
        // Check 3x3 box
        int boxRow = (row / 3) * 3;
        int boxCol = (col / 3) * 3;
        for (int i = boxRow; i < boxRow + 3; i++) {
            for (int j = boxCol; j < boxCol + 3; j++) {
                if (board[i][j] == digit) {
                    return false;
                }
            }
        }
        
        return true;
    }
    
    /**
     * Generate Parentheses - Generate all valid parentheses combinations
     * Pattern: Track open/close count with constraints
     * Time: O(4^n / sqrt(n)), Space: O(n)
     */
    public static List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        generateParenthesisHelper("", 0, 0, n, result);
        return result;
    }
    
    private static void generateParenthesisHelper(String current, int open, int close, 
                                                int n, List<String> result) {
        // BASE CASE: Complete valid combination
        if (current.length() == 2 * n) {
            result.add(current);
            return;
        }
        
        // CHOICE 1: Add opening parenthesis (if we can)
        if (open < n) {
            generateParenthesisHelper(current + "(", open + 1, close, n, result);
        }
        
        // CHOICE 2: Add closing parenthesis (if valid)
        if (close < open) {
            generateParenthesisHelper(current + ")", open, close + 1, n, result);
        }
    }
    
    /**
     * Palindrome Partitioning - Partition string into all palindromes
     * Pattern: Try all possible cuts
     * Time: O(2^n * n), Space: O(n)
     */
    public static List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();
        List<String> currentPartition = new ArrayList<>();
        
        partitionHelper(s, 0, currentPartition, result);
        return result;
    }
    
    private static void partitionHelper(String s, int start, 
                                      List<String> current, List<List<String>> result) {
        // BASE CASE: Processed entire string
        if (start == s.length()) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        // TRY all possible cuts starting from 'start'
        for (int end = start; end < s.length(); end++) {
            String substring = s.substring(start, end + 1);
            
            if (isPalindrome(substring)) {
                // MAKE choice
                current.add(substring);
                
                // RECURSE
                partitionHelper(s, end + 1, current, result);
                
                // UNMAKE choice (BACKTRACK)
                current.remove(current.size() - 1);
            }
        }
    }
    
    private static boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
    
    /**
     * Restore IP Addresses - Generate all valid IP addresses
     * Pattern: Try all valid segment lengths
     * Time: O(3^4), Space: O(1)
     */
    public static List<String> restoreIpAddresses(String s) {
        List<String> result = new ArrayList<>();
        if (s.length() < 4 || s.length() > 12) {
            return result;
        }
        
        restoreIpHelper(s, 0, new ArrayList<>(), result);
        return result;
    }
    
    private static void restoreIpHelper(String s, int index, 
                                      List<String> segments, List<String> result) {
        // BASE CASE: Found 4 segments and used all characters
        if (segments.size() == 4 && index == s.length()) {
            result.add(String.join(".", segments));
            return;
        }
        
        // BASE CASE: Invalid state
        if (segments.size() == 4 || index == s.length()) {
            return;
        }
        
        // TRY segments of length 1, 2, 3
        for (int len = 1; len <= 3 && index + len <= s.length(); len++) {
            String segment = s.substring(index, index + len);
            
            if (isValidIpSegment(segment)) {
                // MAKE choice
                segments.add(segment);
                
                // RECURSE
                restoreIpHelper(s, index + len, segments, result);
                
                // UNMAKE choice (BACKTRACK)
                segments.remove(segments.size() - 1);
            }
        }
    }
    
    private static boolean isValidIpSegment(String segment) {
        // No leading zeros (except "0" itself)
        if (segment.length() > 1 && segment.charAt(0) == '0') {
            return false;
        }
        
        int num = Integer.parseInt(segment);
        return num >= 0 && num <= 255;
    }
    
    public static void main(String[] args) {
        // Test generate parentheses
        System.out.println("Generate 3 parentheses: " + generateParenthesis(3));
        
        // Test palindrome partitioning
        System.out.println("Palindrome partitions of 'aab': " + partition("aab"));
        
        // Test restore IP addresses
        System.out.println("IP addresses from '25525511135': " + 
                          restoreIpAddresses("25525511135"));
        
        // Test Sudoku solver
        char[][] sudoku = {
            {'5','3','.','.','7','.','.','.','.'},
            {'6','.','.','1','9','5','.','.','.'},
            {'.','9','8','.','.','.','.','6','.'},
            {'8','.','.','.','6','.','.','.','3'},
            {'4','.','.','8','.','3','.','.','1'},
            {'7','.','.','.','2','.','.','.','6'},
            {'.','6','.','.','.','.','2','8','.'},
            {'.','.','.','4','1','9','.','.','5'},
            {'.','.','.','.','8','.','.','7','9'}
        };
        
        System.out.println("Sudoku solvable: " + solveSudoku(sudoku));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Backtracking in Go**

```go
package main

import "fmt"

/**
 * Generate All Permutations
 * Pattern: Choose/unchoose from remaining elements
 * Time: O(n! * n), Space: O(n)
 */
func permute(nums []int) [][]int {
    var result [][]int
    var current []int
    used := make([]bool, len(nums))
    
    permuteHelper(nums, current, used, &result)
    return result
}

func permuteHelper(nums []int, current []int, used []bool, result *[][]int) {
    // BASE CASE: Permutation complete
    if len(current) == len(nums) {
        permutation := make([]int, len(current))
        copy(permutation, current)
        *result = append(*result, permutation)
        return
    }
    
    // TRY each unused number
    for i := 0; i < len(nums); i++ {
        if !used[i] {
            // MAKE choice
            current = append(current, nums[i])
            used[i] = true
            
            // RECURSE
            permuteHelper(nums, current, used, result)
            
            // UNMAKE choice (BACKTRACK)
            current = current[:len(current)-1]
            used[i] = false
        }
    }
}

/**
 * Generate All Subsets (Power Set)
 * Pattern: Include/exclude each element
 * Time: O(2^n * n), Space: O(n)
 */
func subsets(nums []int) [][]int {
    var result [][]int
    var current []int
    
    subsetsHelper(nums, 0, current, &result)
    return result
}

func subsetsHelper(nums []int, index int, current []int, result *[][]int) {
    // BASE CASE: Processed all elements
    if index == len(nums) {
        subset := make([]int, len(current))
        copy(subset, current)
        *result = append(*result, subset)
        return
    }
    
    // CHOICE 1: Exclude current element
    subsetsHelper(nums, index+1, current, result)
    
    // CHOICE 2: Include current element
    current = append(current, nums[index])
    subsetsHelper(nums, index+1, current, result)
    current = current[:len(current)-1] // BACKTRACK
}

/**
 * Combination Sum - Find all combinations that sum to target
 * Pattern: Try each number, allow repetition
 * Time: O(2^target), Space: O(target)
 */
func combinationSum(candidates []int, target int) [][]int {
    var result [][]int
    var current []int
    
    combinationSumHelper(candidates, target, 0, current, &result)
    return result
}

func combinationSumHelper(candidates []int, target, start int, current []int, result *[][]int) {
    // BASE CASE: Found valid combination
    if target == 0 {
        combination := make([]int, len(current))
        copy(combination, current)
        *result = append(*result, combination)
        return
    }
    
    // BASE CASE: Exceeded target
    if target < 0 {
        return
    }
    
    // TRY each candidate starting from 'start'
    for i := start; i < len(candidates); i++ {
        // MAKE choice
        current = append(current, candidates[i])
        
        // RECURSE (can use same element again, so pass 'i')
        combinationSumHelper(candidates, target-candidates[i], i, current, result)
        
        // UNMAKE choice (BACKTRACK)
        current = current[:len(current)-1]
    }
}

/**
 * Word Search - Find word in 2D grid
 * Pattern: DFS with visited tracking
 * Time: O(m*n*4^L), Space: O(L) where L is word length
 */
func exist(board [][]byte, word string) bool {
    m := len(board)
    n := len(board[0])
    
    // Try starting from each cell
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            if existHelper(board, word, i, j, 0) {
                return true
            }
        }
    }
    
    return false
}

func existHelper(board [][]byte, word string, row, col, index int) bool {
    // BASE CASE: Found complete word
    if index == len(word) {
        return true
    }
    
    // BASE CASE: Out of bounds or character doesn't match
    if row < 0 || row >= len(board) || col < 0 || col >= len(board[0]) ||
        board[row][col] != word[index] {
        return false
    }
    
    // MAKE choice (mark as visited)
    originalChar := board[row][col]
    board[row][col] = '#' // Mark as visited
    
    // RECURSE in all 4 directions
    found := existHelper(board, word, row+1, col, index+1) ||
        existHelper(board, word, row-1, col, index+1) ||
        existHelper(board, word, row, col+1, index+1) ||
        existHelper(board, word, row, col-1, index+1)
    
    // UNMAKE choice (BACKTRACK)
    board[row][col] = originalChar
    
    return found
}

/**
 * N-Queens Problem - Place N queens on N×N chessboard
 * Pattern: Row by row placement with constraint checking
 * Time: O(N!), Space: O(N)
 */
func solveNQueens(n int) [][]string {
    var result [][]string
    board := make([][]rune, n)
    
    // Initialize board
    for i := range board {
        board[i] = make([]rune, n)
        for j := range board[i] {
            board[i][j] = '.'
        }
    }
    
    nQueensHelper(board, 0, &result)
    return result
}

func nQueensHelper(board [][]rune, row int, result *[][]string) {
    // BASE CASE: Placed all queens
    if row == len(board) {
        *result = append(*result, constructSolution(board))
        return
    }
    
    // TRY placing queen in each column of current row
    for col := 0; col < len(board); col++ {
        if isSafe(board, row, col) {
            // MAKE choice
            board[row][col] = 'Q'
            
            // RECURSE to next row
            nQueensHelper(board, row+1, result)
            
            // UNMAKE choice (BACKTRACK)
            board[row][col] = '.'
        }
    }
}

func isSafe(board [][]rune, row, col int) bool {
    n := len(board)
    
    // Check column
    for i := 0; i < row; i++ {
        if board[i][col] == 'Q' {
            return false
        }
    }
    
    // Check diagonal (top-left)
    for i, j := row-1, col-1; i >= 0 && j >= 0; i, j = i-1, j-1 {
        if board[i][j] == 'Q' {
            return false
        }
    }
    
    // Check diagonal (top-right)
    for i, j := row-1, col+1; i >= 0 && j < n; i, j = i-1, j+1 {
        if board[i][j] == 'Q' {
            return false
        }
    }
    
    return true
}

func constructSolution(board [][]rune) []string {
    var solution []string
    for _, row := range board {
        solution = append(solution, string(row))
    }
    return solution
}

func main() {
    // Test permutations
    nums := []int{1, 2, 3}
    fmt.Printf("Permutations of [1,2,3]: %v\n", permute(nums))
    
    // Test subsets
    fmt.Printf("Subsets of [1,2,3]: %v\n", subsets(nums))
    
    // Test combination sum
    candidates := []int{2, 3, 6, 7}
    fmt.Printf("Combination sum to 7: %v\n", combinationSum(candidates, 7))
    
    // Test word search
    board := [][]byte{
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'},
    }
    fmt.Printf("Word 'ABCCED' exists: %t\n", exist(board, "ABCCED"))
    
    // Test N-Queens
    fmt.Printf("4-Queens solutions count: %d\n", len(solveNQueens(4)))
}
```

---

## 🧠 **UNDERSTANDING BACKTRACKING PATTERNS**

### **Pattern 1: Generate All Solutions**
```java
void backtrack(currentSolution, allSolutions) {
    if (isComplete(currentSolution)) {
        allSolutions.add(copy(currentSolution));
        return;
    }
    
    for (choice : getChoices()) {
        make(choice);
        backtrack(currentSolution, allSolutions);
        unmake(choice);
    }
}
```
**Examples:** Permutations, Subsets, Combinations

### **Pattern 2: Find Any Valid Solution**
```java
boolean backtrack(currentSolution) {
    if (isComplete(currentSolution)) {
        return true; // Found solution
    }
    
    for (choice : getChoices()) {
        if (isValid(choice)) {
            make(choice);
            if (backtrack(currentSolution)) {
                return true; // Solution found deeper
            }
            unmake(choice);
        }
    }
    return false; // No solution found
}
```
**Examples:** Sudoku, N-Queens, Word Search

### **Pattern 3: Optimization with Pruning**
```java
void backtrack(currentSolution, bestSolution) {
    if (canPrune(currentSolution, bestSolution)) {
        return; // Prune this branch
    }
    
    if (isComplete(currentSolution)) {
        if (isBetter(currentSolution, bestSolution)) {
            update(bestSolution, currentSolution);
        }
        return;
    }
    
    for (choice : getChoices()) {
        make(choice);
        backtrack(currentSolution, bestSolution);
        unmake(choice);
    }
}
```
**Examples:** Traveling Salesman, Optimal path finding

---

## ⚡ **OPTIMIZATION TECHNIQUES**

### **1. Early Pruning:**
```java
// Instead of checking validity at the end
if (isComplete(current) && isValid(current)) {
    addSolution(current);
}

// Check validity early to prune invalid branches
if (!isValid(currentChoice)) {
    continue; // Skip this choice
}
```

### **2. Constraint Propagation:**
```java
// Sudoku: After placing a number, remove it from row/col/box possibilities
void makeChoice(int row, int col, int digit) {
    board[row][col] = digit;
    removeFromRow(row, digit);
    removeFromCol(col, digit);
    removeFromBox(row/3, col/3, digit);
}
```

### **3. Ordering Heuristics:**
```java
// Try most constrained choices first (MRV - Minimum Remaining Values)
List<Cell> getEmptyCells() {
    return emptyCells.stream()
        .sorted((a, b) -> a.possibleValues.size() - b.possibleValues.size())
        .collect(toList());
}
```

### **4. Symmetry Breaking:**
```java
// N-Queens: Only try first row positions 0 to n/2
for (int col = 0; col <= n/2; col++) {
    // Place first queen
    // Generate solutions
    // Mirror solutions for symmetry
}
```

---

## 📊 **COMPLEXITY ANALYSIS**

| Problem | Time Complexity | Space Complexity | Explanation |
|---------|----------------|------------------|-------------|
| **Permutations** | O(n! × n) | O(n) | n! permutations, O(n) to copy each |
| **Subsets** | O(2^n × n) | O(n) | 2^n subsets, O(n) to copy each |
| **N-Queens** | O(N!) | O(N) | N! ways to place queens |
| **Sudoku** | O(9^(empty cells)) | O(1) | 9 choices per empty cell |
| **Word Search** | O(m×n×4^L) | O(L) | Start from each cell, 4^L paths |

### **Why Exponential?**
- **Complete search** of solution space
- **Branching factor** (choices per step) multiplied by **depth**
- **Pruning** can significantly reduce actual complexity

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Generate Parentheses** ✅ (Implemented above)
2. **Subsets** ✅ (Implemented above)
3. **Permutations** ✅ (Implemented above)
4. **Combination Sum** ✅ (Implemented above)
5. **Letter Case Permutation**

### **Medium Problems:**

1. **Word Search** ✅ (Implemented above)
2. **Palindrome Partitioning** ✅ (Implemented above)
3. **N-Queens** ✅ (Implemented above)
4. **Restore IP Addresses** ✅ (Implemented above)
5. **Combinations**

### **Hard Problems:**

1. **Sudoku Solver** ✅ (Implemented above)
2. **N-Queens II** (Count solutions)
3. **Word Search II** (Multiple words)
4. **Expression Add Operators**
5. **Remove Invalid Parentheses**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master the Template**
- [ ] Understand the three-step pattern (make, recurse, unmake)
- [ ] Implement basic permutations and subsets
- [ ] Practice identifying base cases
- [ ] Focus on correct backtracking (unmake step)

### **Day 2: Constraint Problems**
- [ ] Solve N-Queens problem step by step
- [ ] Implement word search in 2D grid
- [ ] Practice constraint checking and validation
- [ ] Understand pruning strategies

### **Day 3: Complex Applications**
- [ ] Implement Sudoku solver
- [ ] Solve palindrome partitioning
- [ ] Practice optimization techniques
- [ ] Focus on early pruning

### **Day 4: Problem Recognition**
- [ ] Solve 5+ backtracking problems on LeetCode
- [ ] Identify when to use backtracking vs other approaches
- [ ] Practice explaining backtracking solutions
- [ ] Build confidence with recursive thinking

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement the backtracking template from memory
- [ ] You recognize when problems need exhaustive search
- [ ] You can optimize with pruning strategies
- [ ] You understand space-time tradeoffs

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Sorting Algorithms - Detailed](./12-sorting-detailed-java-go.md)** - Master efficient sorting
2. **[Greedy Algorithms - Detailed](./13-greedy-detailed-java-go.md)** - Learn optimization strategies
3. **[Graph Algorithms - Detailed](./15-graphs-detailed-java-go.md)** - Extend to graph problems

### **🎯 Advanced Backtracking Topics:**
- **Branch and Bound** for optimization problems
- **Constraint Satisfaction Problems** (CSP)
- **Alpha-Beta Pruning** for game trees
- **Parallel Backtracking** for multi-core systems

### **💡 Pro Tips:**
- **Start with brute force** - Then add backtracking optimizations
- **Draw the decision tree** - Visualize the search space
- **Identify constraints early** - Use them for pruning
- **Practice state management** - Make/unmake operations
- **Consider iterative alternatives** - Sometimes stacks are cleaner

**🎉 You now master backtracking! This powerful technique will help you solve constraint satisfaction problems and generate all solutions efficiently.**