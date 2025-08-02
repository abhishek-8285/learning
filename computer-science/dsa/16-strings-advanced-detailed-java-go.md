# 🔤 **ADVANCED STRING ALGORITHMS: DETAILED GUIDE (JAVA & GO)**

**Master Pattern Matching, Text Processing, and String Optimization**

*Prerequisites: Complete basic strings, arrays, and algorithms first*

---

## 🤔 **WHAT ARE ADVANCED STRING ALGORITHMS?**

### **Simple Explanation:**
Advanced string algorithms are like **sophisticated text processing tools**:
- **Pattern Matching** = Finding needles in haystacks efficiently
- **Text Processing** = Analyzing, transforming, and manipulating text
- **String Optimization** = Solving complex text-based problems elegantly

### **Real-World Applications:**

**🔍 Search Engines:**
- **Google Search** - Find web pages containing query patterns
- **Autocomplete** - Suggest completions using tries and suffix structures
- **Spell Check** - Find similar words using edit distance

**🧬 Bioinformatics:**
- **DNA Sequencing** - Find patterns in genetic code
- **Protein Analysis** - Match amino acid sequences
- **Genome Assembly** - Overlap detection in DNA fragments

**📝 Text Editors & IDEs:**
- **Find & Replace** - Efficient pattern searching
- **Syntax Highlighting** - Pattern recognition for keywords
- **Code Completion** - Intelligent text suggestions

**💻 Cybersecurity:**
- **Virus Detection** - Pattern matching in file signatures
- **Intrusion Detection** - Analyze network traffic patterns
- **Data Leakage Prevention** - Identify sensitive patterns

### **Why Master String Algorithms:**
- **Interview favorites** - 20% of coding interviews
- **Real-world impact** - Core of many applications
- **Algorithm beauty** - Elegant solutions to complex problems
- **Performance critical** - Difference between O(n²) and O(n)

---

## 🎯 **STRING ALGORITHM CATEGORIES**

### **Pattern Matching:**
- **Naive** - Simple but slow O(nm)
- **KMP** - Linear time with preprocessing
- **Rabin-Karp** - Rolling hash approach
- **Boyer-Moore** - Skip characters smartly

### **String Processing:**
- **Longest Palindromes** - Manacher's algorithm
- **Suffix Arrays** - Efficient substring operations
- **Z-Algorithm** - Pattern matching with Z-values
- **Aho-Corasick** - Multiple pattern matching

### **String Structures:**
- **Trie** - Prefix tree for word storage
- **Suffix Tree** - All suffixes in tree form
- **Suffix Array** - Sorted suffixes array
- **LCP Array** - Longest common prefix

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Pattern Matching Algorithms**

```java
import java.util.*;

public class PatternMatching {
    
    /**
     * Naive Pattern Matching - Check every position
     * Pattern: Brute force comparison
     * Time: O(nm), Space: O(1) where n=text length, m=pattern length
     */
    public static List<Integer> naiveSearch(String text, String pattern) {
        List<Integer> matches = new ArrayList<>();
        int n = text.length();
        int m = pattern.length();
        
        for (int i = 0; i <= n - m; i++) {
            int j;
            for (j = 0; j < m; j++) {
                if (text.charAt(i + j) != pattern.charAt(j)) {
                    break;
                }
            }
            if (j == m) {
                matches.add(i); // Pattern found at index i
            }
        }
        
        return matches;
    }
    
    /**
     * KMP (Knuth-Morris-Pratt) Algorithm - Linear pattern matching
     * Pattern: Use failure function to skip characters
     * Time: O(n + m), Space: O(m)
     */
    public static List<Integer> kmpSearch(String text, String pattern) {
        List<Integer> matches = new ArrayList<>();
        int n = text.length();
        int m = pattern.length();
        
        // Build failure function (LPS array)
        int[] lps = buildLPS(pattern);
        
        int i = 0; // Index for text
        int j = 0; // Index for pattern
        
        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;
            }
            
            if (j == m) {
                matches.add(i - j); // Pattern found
                j = lps[j - 1];
            } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
        
        return matches;
    }
    
    /**
     * Build LPS (Longest Proper Prefix which is also Suffix) array
     */
    private static int[] buildLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        int length = 0; // Length of previous longest prefix suffix
        int i = 1;
        
        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(length)) {
                length++;
                lps[i] = length;
                i++;
            } else {
                if (length != 0) {
                    length = lps[length - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        
        return lps;
    }
    
    /**
     * Rabin-Karp Algorithm - Rolling hash pattern matching
     * Pattern: Use hash values to quick comparison
     * Time: O(n + m) average, O(nm) worst, Space: O(1)
     */
    public static List<Integer> rabinKarpSearch(String text, String pattern) {
        List<Integer> matches = new ArrayList<>();
        int n = text.length();
        int m = pattern.length();
        
        if (m > n) return matches;
        
        final int PRIME = 101; // Prime number for hashing
        final int BASE = 256;  // Number of characters in alphabet
        
        long patternHash = 0;
        long textHash = 0;
        long h = 1;
        
        // Calculate h = BASE^(m-1) % PRIME
        for (int i = 0; i < m - 1; i++) {
            h = (h * BASE) % PRIME;
        }
        
        // Calculate hash of pattern and first window of text
        for (int i = 0; i < m; i++) {
            patternHash = (BASE * patternHash + pattern.charAt(i)) % PRIME;
            textHash = (BASE * textHash + text.charAt(i)) % PRIME;
        }
        
        // Slide pattern over text
        for (int i = 0; i <= n - m; i++) {
            // Check if hash values match
            if (patternHash == textHash) {
                // Double-check character by character
                boolean match = true;
                for (int j = 0; j < m; j++) {
                    if (text.charAt(i + j) != pattern.charAt(j)) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    matches.add(i);
                }
            }
            
            // Calculate hash for next window
            if (i < n - m) {
                textHash = (BASE * (textHash - text.charAt(i) * h) + text.charAt(i + m)) % PRIME;
                
                // Handle negative hash
                if (textHash < 0) {
                    textHash += PRIME;
                }
            }
        }
        
        return matches;
    }
    
    /**
     * Z-Algorithm - Linear pattern matching using Z-array
     * Pattern: Z[i] = length of longest substring starting at i which is also prefix
     * Time: O(n + m), Space: O(n + m)
     */
    public static List<Integer> zAlgorithmSearch(String text, String pattern) {
        String combined = pattern + "$" + text; // $ is separator
        int[] z = buildZArray(combined);
        
        List<Integer> matches = new ArrayList<>();
        int patternLength = pattern.length();
        
        for (int i = patternLength + 1; i < combined.length(); i++) {
            if (z[i] == patternLength) {
                matches.add(i - patternLength - 1); // Adjust for separator
            }
        }
        
        return matches;
    }
    
    private static int[] buildZArray(String s) {
        int n = s.length();
        int[] z = new int[n];
        int left = 0, right = 0;
        
        for (int i = 1; i < n; i++) {
            if (i <= right) {
                z[i] = Math.min(right - i + 1, z[i - left]);
            }
            
            while (i + z[i] < n && s.charAt(z[i]) == s.charAt(i + z[i])) {
                z[i]++;
            }
            
            if (i + z[i] - 1 > right) {
                left = i;
                right = i + z[i] - 1;
            }
        }
        
        return z;
    }
    
    public static void main(String[] args) {
        String text = "ABABDABACDABABCABCABCABCABC";
        String pattern = "ABABCAB";
        
        System.out.println("Text: " + text);
        System.out.println("Pattern: " + pattern);
        System.out.println();
        
        System.out.println("Naive Search: " + naiveSearch(text, pattern));
        System.out.println("KMP Search: " + kmpSearch(text, pattern));
        System.out.println("Rabin-Karp Search: " + rabinKarpSearch(text, pattern));
        System.out.println("Z-Algorithm Search: " + zAlgorithmSearch(text, pattern));
    }
}
```

### **Advanced String Data Structures**

```java
import java.util.*;

/**
 * Trie (Prefix Tree) - Efficient string storage and retrieval
 */
public class Trie {
    private TrieNode root;
    
    private static class TrieNode {
        TrieNode[] children;
        boolean isEndOfWord;
        
        TrieNode() {
            children = new TrieNode[26]; // For lowercase letters
            isEndOfWord = false;
        }
    }
    
    public Trie() {
        root = new TrieNode();
    }
    
    /**
     * Insert word into trie
     * Time: O(m), Space: O(m) where m is word length
     */
    public void insert(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            current = current.children[index];
        }
        
        current.isEndOfWord = true;
    }
    
    /**
     * Search for word in trie
     * Time: O(m), Space: O(1)
     */
    public boolean search(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        
        return current.isEndOfWord;
    }
    
    /**
     * Check if any word starts with prefix
     * Time: O(m), Space: O(1)
     */
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                return false;
            }
            current = current.children[index];
        }
        
        return true;
    }
    
    /**
     * Get all words with given prefix
     * Time: O(p + n) where p is prefix length, n is number of words
     */
    public List<String> getWordsWithPrefix(String prefix) {
        List<String> result = new ArrayList<>();
        TrieNode current = root;
        
        // Navigate to prefix end
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            if (current.children[index] == null) {
                return result; // No words with this prefix
            }
            current = current.children[index];
        }
        
        // DFS to collect all words
        dfsCollectWords(current, prefix, result);
        return result;
    }
    
    private void dfsCollectWords(TrieNode node, String current, List<String> result) {
        if (node.isEndOfWord) {
            result.add(current);
        }
        
        for (int i = 0; i < 26; i++) {
            if (node.children[i] != null) {
                dfsCollectWords(node.children[i], current + (char)('a' + i), result);
            }
        }
    }
}

/**
 * Suffix Array - Efficient substring operations
 */
public class SuffixArray {
    private String text;
    private int[] suffixArray;
    private int[] lcpArray;
    
    public SuffixArray(String text) {
        this.text = text + "$"; // Add terminator
        buildSuffixArray();
        buildLCPArray();
    }
    
    /**
     * Build suffix array using simple sorting approach
     * Time: O(n² log n), Space: O(n)
     * Note: More efficient algorithms exist (O(n) time)
     */
    private void buildSuffixArray() {
        int n = text.length();
        Integer[] indices = new Integer[n];
        
        for (int i = 0; i < n; i++) {
            indices[i] = i;
        }
        
        // Sort suffixes lexicographically
        Arrays.sort(indices, (i, j) -> text.substring(i).compareTo(text.substring(j)));
        
        suffixArray = new int[n];
        for (int i = 0; i < n; i++) {
            suffixArray[i] = indices[i];
        }
    }
    
    /**
     * Build LCP (Longest Common Prefix) array
     * Time: O(n), Space: O(n)
     */
    private void buildLCPArray() {
        int n = text.length();
        lcpArray = new int[n - 1];
        int[] rank = new int[n];
        
        // Build rank array
        for (int i = 0; i < n; i++) {
            rank[suffixArray[i]] = i;
        }
        
        int h = 0;
        for (int i = 0; i < n; i++) {
            if (rank[i] > 0) {
                int j = suffixArray[rank[i] - 1];
                while (i + h < n && j + h < n && text.charAt(i + h) == text.charAt(j + h)) {
                    h++;
                }
                lcpArray[rank[i] - 1] = h;
                if (h > 0) {
                    h--;
                }
            }
        }
    }
    
    /**
     * Search pattern in text using binary search on suffix array
     * Time: O(m log n), Space: O(1)
     */
    public List<Integer> search(String pattern) {
        List<Integer> matches = new ArrayList<>();
        int left = 0, right = suffixArray.length - 1;
        
        // Find leftmost position
        while (left <= right) {
            int mid = left + (right - left) / 2;
            String suffix = text.substring(suffixArray[mid]);
            
            if (suffix.startsWith(pattern)) {
                // Found a match, search for leftmost
                right = mid - 1;
                matches.add(suffixArray[mid]);
            } else if (suffix.compareTo(pattern) < 0) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        // Find all matches (simplified version)
        for (int i = 0; i < suffixArray.length; i++) {
            if (text.substring(suffixArray[i]).startsWith(pattern)) {
                if (!matches.contains(suffixArray[i])) {
                    matches.add(suffixArray[i]);
                }
            }
        }
        
        Collections.sort(matches);
        return matches;
    }
    
    /**
     * Find longest repeated substring
     * Time: O(n), Space: O(1)
     */
    public String longestRepeatedSubstring() {
        int maxLength = 0;
        int index = 0;
        
        for (int i = 0; i < lcpArray.length; i++) {
            if (lcpArray[i] > maxLength) {
                maxLength = lcpArray[i];
                index = suffixArray[i];
            }
        }
        
        return maxLength > 0 ? text.substring(index, index + maxLength) : "";
    }
}

public class AdvancedStringStructuresDemo {
    public static void main(String[] args) {
        // Test Trie
        System.out.println("=== TRIE DEMO ===");
        Trie trie = new Trie();
        String[] words = {"apple", "app", "application", "apply", "banana", "band"};
        
        for (String word : words) {
            trie.insert(word);
        }
        
        System.out.println("Search 'app': " + trie.search("app"));
        System.out.println("Search 'appl': " + trie.search("appl"));
        System.out.println("Starts with 'app': " + trie.startsWith("app"));
        System.out.println("Words with prefix 'app': " + trie.getWordsWithPrefix("app"));
        
        // Test Suffix Array
        System.out.println("\n=== SUFFIX ARRAY DEMO ===");
        SuffixArray sa = new SuffixArray("banana");
        System.out.println("Search 'ana': " + sa.search("ana"));
        System.out.println("Longest repeated substring: '" + sa.longestRepeatedSubstring() + "'");
    }
}
```

### **String Processing Algorithms**

```java
import java.util.*;

public class StringProcessing {
    
    /**
     * Manacher's Algorithm - Find longest palindromes in linear time
     * Pattern: Expand around centers with preprocessing
     * Time: O(n), Space: O(n)
     */
    public static String longestPalindrome(String s) {
        if (s == null || s.length() == 0) return "";
        
        // Preprocess string: "abc" -> "^#a#b#c#$"
        StringBuilder processed = new StringBuilder();
        processed.append("^#");
        for (char c : s.toCharArray()) {
            processed.append(c).append("#");
        }
        processed.append("$");
        
        String processedStr = processed.toString();
        int n = processedStr.length();
        int[] P = new int[n]; // P[i] = radius of palindrome centered at i
        int center = 0, right = 0;
        
        for (int i = 1; i < n - 1; i++) {
            int mirror = 2 * center - i;
            
            if (i < right) {
                P[i] = Math.min(right - i, P[mirror]);
            }
            
            // Try to expand palindrome centered at i
            while (processedStr.charAt(i + P[i] + 1) == processedStr.charAt(i - P[i] - 1)) {
                P[i]++;
            }
            
            // If palindrome centered at i extends past right, adjust center and right
            if (i + P[i] > right) {
                center = i;
                right = i + P[i];
            }
        }
        
        // Find longest palindrome
        int maxLength = 0;
        int centerIndex = 0;
        for (int i = 1; i < n - 1; i++) {
            if (P[i] > maxLength) {
                maxLength = P[i];
                centerIndex = i;
            }
        }
        
        int start = (centerIndex - maxLength) / 2;
        return s.substring(start, start + maxLength);
    }
    
    /**
     * Edit Distance (Levenshtein Distance) - Minimum operations to transform
     * Pattern: Dynamic programming on strings
     * Time: O(mn), Space: O(mn)
     */
    public static int editDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        
        int[][] dp = new int[m + 1][n + 1];
        
        // Initialize base cases
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = 1 + Math.min(
                        Math.min(dp[i - 1][j], dp[i][j - 1]),  // Delete or Insert
                        dp[i - 1][j - 1]                        // Replace
                    );
                }
            }
        }
        
        return dp[m][n];
    }
    
    /**
     * Longest Common Subsequence - Find LCS of two strings
     * Pattern: Dynamic programming with sequence matching
     * Time: O(mn), Space: O(mn)
     */
    public static String longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        // Reconstruct LCS
        StringBuilder lcs = new StringBuilder();
        int i = m, j = n;
        
        while (i > 0 && j > 0) {
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                lcs.insert(0, text1.charAt(i - 1));
                i--;
                j--;
            } else if (dp[i - 1][j] > dp[i][j - 1]) {
                i--;
            } else {
                j--;
            }
        }
        
        return lcs.toString();
    }
    
    /**
     * String Compression - Compress string using character counts
     * Pattern: Two pointers with counting
     * Time: O(n), Space: O(1)
     */
    public static String compress(String s) {
        if (s == null || s.length() <= 1) return s;
        
        StringBuilder compressed = new StringBuilder();
        int count = 1;
        
        for (int i = 1; i < s.length(); i++) {
            if (s.charAt(i) == s.charAt(i - 1)) {
                count++;
            } else {
                compressed.append(s.charAt(i - 1));
                if (count > 1) {
                    compressed.append(count);
                }
                count = 1;
            }
        }
        
        // Add last character
        compressed.append(s.charAt(s.length() - 1));
        if (count > 1) {
            compressed.append(count);
        }
        
        return compressed.length() < s.length() ? compressed.toString() : s;
    }
    
    /**
     * Anagram Groups - Group anagrams together
     * Pattern: Use sorted string as key
     * Time: O(n * m log m), Space: O(n * m)
     */
    public static List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> anagramGroups = new HashMap<>();
        
        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);
            String sortedKey = new String(chars);
            
            anagramGroups.computeIfAbsent(sortedKey, k -> new ArrayList<>()).add(str);
        }
        
        return new ArrayList<>(anagramGroups.values());
    }
    
    /**
     * Valid Palindrome - Check if string can form palindrome after removing at most one character
     * Pattern: Two pointers with one skip allowed
     * Time: O(n), Space: O(1)
     */
    public static boolean validPalindrome(String s) {
        return isPalindromeRange(s, 0, s.length() - 1, false);
    }
    
    private static boolean isPalindromeRange(String s, int left, int right, boolean deleted) {
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                if (deleted) {
                    return false; // Already deleted one character
                }
                // Try deleting left or right character
                return isPalindromeRange(s, left + 1, right, true) ||
                       isPalindromeRange(s, left, right - 1, true);
            }
            left++;
            right--;
        }
        return true;
    }
    
    public static void main(String[] args) {
        // Test Manacher's Algorithm
        System.out.println("=== LONGEST PALINDROME ===");
        System.out.println("Longest palindrome in 'babad': '" + longestPalindrome("babad") + "'");
        System.out.println("Longest palindrome in 'cbbd': '" + longestPalindrome("cbbd") + "'");
        
        // Test Edit Distance
        System.out.println("\n=== EDIT DISTANCE ===");
        System.out.println("Edit distance 'horse' -> 'ros': " + editDistance("horse", "ros"));
        System.out.println("Edit distance 'intention' -> 'execution': " + editDistance("intention", "execution"));
        
        // Test LCS
        System.out.println("\n=== LONGEST COMMON SUBSEQUENCE ===");
        System.out.println("LCS of 'abcde' and 'ace': '" + longestCommonSubsequence("abcde", "ace") + "'");
        
        // Test String Compression
        System.out.println("\n=== STRING COMPRESSION ===");
        System.out.println("Compress 'aabcccccaaa': '" + compress("aabcccccaaa") + "'");
        
        // Test Anagram Groups
        System.out.println("\n=== ANAGRAM GROUPS ===");
        String[] strs = {"eat", "tea", "tan", "ate", "nat", "bat"};
        System.out.println("Anagram groups: " + groupAnagrams(strs));
        
        // Test Valid Palindrome
        System.out.println("\n=== VALID PALINDROME ===");
        System.out.println("'aba' valid palindrome: " + validPalindrome("aba"));
        System.out.println("'abca' valid palindrome: " + validPalindrome("abca"));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Pattern Matching in Go**

```go
package main

import (
    "fmt"
    "strings"
)

/**
 * Naive Pattern Matching
 * Time: O(nm), Space: O(1)
 */
func naiveSearch(text, pattern string) []int {
    var matches []int
    n, m := len(text), len(pattern)
    
    for i := 0; i <= n-m; i++ {
        j := 0
        for j < m && text[i+j] == pattern[j] {
            j++
        }
        if j == m {
            matches = append(matches, i)
        }
    }
    
    return matches
}

/**
 * KMP Algorithm
 * Time: O(n + m), Space: O(m)
 */
func kmpSearch(text, pattern string) []int {
    var matches []int
    n, m := len(text), len(pattern)
    
    lps := buildLPS(pattern)
    
    i, j := 0, 0
    for i < n {
        if text[i] == pattern[j] {
            i++
            j++
        }
        
        if j == m {
            matches = append(matches, i-j)
            j = lps[j-1]
        } else if i < n && text[i] != pattern[j] {
            if j != 0 {
                j = lps[j-1]
            } else {
                i++
            }
        }
    }
    
    return matches
}

func buildLPS(pattern string) []int {
    m := len(pattern)
    lps := make([]int, m)
    length := 0
    i := 1
    
    for i < m {
        if pattern[i] == pattern[length] {
            length++
            lps[i] = length
            i++
        } else {
            if length != 0 {
                length = lps[length-1]
            } else {
                lps[i] = 0
                i++
            }
        }
    }
    
    return lps
}

/**
 * Rabin-Karp Algorithm
 * Time: O(n + m) average, O(nm) worst, Space: O(1)
 */
func rabinKarpSearch(text, pattern string) []int {
    var matches []int
    n, m := len(text), len(pattern)
    
    if m > n {
        return matches
    }
    
    const PRIME = 101
    const BASE = 256
    
    // Calculate hash value for pattern and first window
    patternHash := int64(0)
    textHash := int64(0)
    h := int64(1)
    
    // h = BASE^(m-1) % PRIME
    for i := 0; i < m-1; i++ {
        h = (h * BASE) % PRIME
    }
    
    // Calculate hash of pattern and first window
    for i := 0; i < m; i++ {
        patternHash = (BASE*patternHash + int64(pattern[i])) % PRIME
        textHash = (BASE*textHash + int64(text[i])) % PRIME
    }
    
    // Slide pattern over text
    for i := 0; i <= n-m; i++ {
        if patternHash == textHash {
            // Check characters one by one
            match := true
            for j := 0; j < m; j++ {
                if text[i+j] != pattern[j] {
                    match = false
                    break
                }
            }
            if match {
                matches = append(matches, i)
            }
        }
        
        // Calculate hash for next window
        if i < n-m {
            textHash = (BASE*(textHash-int64(text[i])*h) + int64(text[i+m])) % PRIME
            if textHash < 0 {
                textHash += PRIME
            }
        }
    }
    
    return matches
}

func main() {
    text := "ABABDABACDABABCABCABCABCABC"
    pattern := "ABABCAB"
    
    fmt.Printf("Text: %s\n", text)
    fmt.Printf("Pattern: %s\n", pattern)
    fmt.Println()
    
    fmt.Printf("Naive Search: %v\n", naiveSearch(text, pattern))
    fmt.Printf("KMP Search: %v\n", kmpSearch(text, pattern))
    fmt.Printf("Rabin-Karp Search: %v\n", rabinKarpSearch(text, pattern))
}
```

### **Trie Implementation in Go**

```go
package main

import "fmt"

// TrieNode represents a node in trie
type TrieNode struct {
    children    [26]*TrieNode
    isEndOfWord bool
}

// Trie represents the trie data structure
type Trie struct {
    root *TrieNode
}

// NewTrie creates a new trie
func NewTrie() *Trie {
    return &Trie{root: &TrieNode{}}
}

// Insert adds a word to the trie
func (t *Trie) Insert(word string) {
    current := t.root
    
    for _, char := range word {
        index := char - 'a'
        if current.children[index] == nil {
            current.children[index] = &TrieNode{}
        }
        current = current.children[index]
    }
    
    current.isEndOfWord = true
}

// Search looks for a word in the trie
func (t *Trie) Search(word string) bool {
    current := t.root
    
    for _, char := range word {
        index := char - 'a'
        if current.children[index] == nil {
            return false
        }
        current = current.children[index]
    }
    
    return current.isEndOfWord
}

// StartsWith checks if any word starts with prefix
func (t *Trie) StartsWith(prefix string) bool {
    current := t.root
    
    for _, char := range prefix {
        index := char - 'a'
        if current.children[index] == nil {
            return false
        }
        current = current.children[index]
    }
    
    return true
}

// GetWordsWithPrefix returns all words with given prefix
func (t *Trie) GetWordsWithPrefix(prefix string) []string {
    var result []string
    current := t.root
    
    // Navigate to prefix end
    for _, char := range prefix {
        index := char - 'a'
        if current.children[index] == nil {
            return result
        }
        current = current.children[index]
    }
    
    // DFS to collect all words
    t.dfsCollectWords(current, prefix, &result)
    return result
}

func (t *Trie) dfsCollectWords(node *TrieNode, current string, result *[]string) {
    if node.isEndOfWord {
        *result = append(*result, current)
    }
    
    for i, child := range node.children {
        if child != nil {
            t.dfsCollectWords(child, current+string(rune('a'+i)), result)
        }
    }
}

func main() {
    trie := NewTrie()
    words := []string{"apple", "app", "application", "apply", "banana", "band"}
    
    for _, word := range words {
        trie.Insert(word)
    }
    
    fmt.Printf("Search 'app': %t\n", trie.Search("app"))
    fmt.Printf("Search 'appl': %t\n", trie.Search("appl"))
    fmt.Printf("Starts with 'app': %t\n", trie.StartsWith("app"))
    fmt.Printf("Words with prefix 'app': %v\n", trie.GetWordsWithPrefix("app"))
}
```

### **String Processing in Go**

```go
package main

import (
    "fmt"
    "sort"
    "strings"
)

/**
 * Edit Distance (Levenshtein Distance)
 * Time: O(mn), Space: O(mn)
 */
func editDistance(word1, word2 string) int {
    m, n := len(word1), len(word2)
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
                dp[i][j] = 1 + min(min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1])
            }
        }
    }
    
    return dp[m][n]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

/**
 * Longest Common Subsequence
 * Time: O(mn), Space: O(mn)
 */
func longestCommonSubsequence(text1, text2 string) string {
    m, n := len(text1), len(text2)
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
    
    // Reconstruct LCS
    var lcs strings.Builder
    i, j := m, n
    
    for i > 0 && j > 0 {
        if text1[i-1] == text2[j-1] {
            lcs.WriteByte(text1[i-1])
            i--
            j--
        } else if dp[i-1][j] > dp[i][j-1] {
            i--
        } else {
            j--
        }
    }
    
    // Reverse the result
    result := lcs.String()
    runes := []rune(result)
    for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
        runes[i], runes[j] = runes[j], runes[i]
    }
    
    return string(runes)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

/**
 * Group Anagrams
 * Time: O(n * m log m), Space: O(n * m)
 */
func groupAnagrams(strs []string) [][]string {
    anagramGroups := make(map[string][]string)
    
    for _, str := range strs {
        // Sort characters to create key
        runes := []rune(str)
        sort.Slice(runes, func(i, j int) bool {
            return runes[i] < runes[j]
        })
        sortedKey := string(runes)
        
        anagramGroups[sortedKey] = append(anagramGroups[sortedKey], str)
    }
    
    var result [][]string
    for _, group := range anagramGroups {
        result = append(result, group)
    }
    
    return result
}

/**
 * String Compression
 * Time: O(n), Space: O(1)
 */
func compress(s string) string {
    if len(s) <= 1 {
        return s
    }
    
    var compressed strings.Builder
    count := 1
    
    for i := 1; i < len(s); i++ {
        if s[i] == s[i-1] {
            count++
        } else {
            compressed.WriteByte(s[i-1])
            if count > 1 {
                compressed.WriteString(fmt.Sprintf("%d", count))
            }
            count = 1
        }
    }
    
    // Add last character
    compressed.WriteByte(s[len(s)-1])
    if count > 1 {
        compressed.WriteString(fmt.Sprintf("%d", count))
    }
    
    result := compressed.String()
    if len(result) < len(s) {
        return result
    }
    return s
}

/**
 * Valid Palindrome (with one deletion allowed)
 * Time: O(n), Space: O(1)
 */
func validPalindrome(s string) bool {
    return isPalindromeRange(s, 0, len(s)-1, false)
}

func isPalindromeRange(s string, left, right int, deleted bool) bool {
    for left < right {
        if s[left] != s[right] {
            if deleted {
                return false
            }
            // Try deleting left or right character
            return isPalindromeRange(s, left+1, right, true) ||
                isPalindromeRange(s, left, right-1, true)
        }
        left++
        right--
    }
    return true
}

func main() {
    // Test Edit Distance
    fmt.Println("=== EDIT DISTANCE ===")
    fmt.Printf("Edit distance 'horse' -> 'ros': %d\n", editDistance("horse", "ros"))
    
    // Test LCS
    fmt.Println("\n=== LONGEST COMMON SUBSEQUENCE ===")
    fmt.Printf("LCS of 'abcde' and 'ace': '%s'\n", longestCommonSubsequence("abcde", "ace"))
    
    // Test Group Anagrams
    fmt.Println("\n=== ANAGRAM GROUPS ===")
    strs := []string{"eat", "tea", "tan", "ate", "nat", "bat"}
    fmt.Printf("Anagram groups: %v\n", groupAnagrams(strs))
    
    // Test String Compression
    fmt.Println("\n=== STRING COMPRESSION ===")
    fmt.Printf("Compress 'aabcccccaaa': '%s'\n", compress("aabcccccaaa"))
    
    // Test Valid Palindrome
    fmt.Println("\n=== VALID PALINDROME ===")
    fmt.Printf("'aba' valid palindrome: %t\n", validPalindrome("aba"))
    fmt.Printf("'abca' valid palindrome: %t\n", validPalindrome("abca"))
}
```

---

## 📊 **STRING ALGORITHM COMPLEXITY**

| Algorithm | Time Complexity | Space Complexity | Best Use Case |
|-----------|----------------|------------------|---------------|
| **Naive Search** | O(nm) | O(1) | Small patterns, simple implementation |
| **KMP** | O(n + m) | O(m) | Single pattern, linear time required |
| **Rabin-Karp** | O(n + m) avg | O(1) | Multiple patterns, rolling hash |
| **Z-Algorithm** | O(n + m) | O(n + m) | Pattern matching with preprocessing |
| **Trie** | O(m) insert/search | O(ALPHABET_SIZE * N * M) | Prefix matching, autocomplete |
| **Suffix Array** | O(n² log n) build | O(n) | Multiple substring queries |
| **Manacher** | O(n) | O(n) | All palindromes in linear time |
| **Edit Distance** | O(mn) | O(mn) | String similarity, spell check |

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Valid Anagram** - Check if two strings are anagrams
2. **First Unique Character** - Find first non-repeating character
3. **Valid Palindrome** - Check palindrome ignoring non-alphanumeric
4. **Implement strStr()** - Find needle in haystack
5. **Longest Common Prefix** - Find common prefix of string array

### **Medium Problems:**

1. **Group Anagrams** ✅ (Implemented above)
2. **Longest Palindromic Substring** ✅ (Implemented above)
3. **Edit Distance** ✅ (Implemented above)
4. **Minimum Window Substring** - Find minimum window containing all characters
5. **Palindromic Substrings** - Count all palindromic substrings

### **Hard Problems:**

1. **Regular Expression Matching** - Pattern matching with . and *
2. **Wildcard Matching** - Pattern matching with ? and *
3. **Shortest Palindrome** - Add minimum characters to make palindrome
4. **Distinct Subsequences** - Count distinct subsequences
5. **Interleaving String** - Check if string is interleaving of two others

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Pattern Matching**
- [ ] Implement naive, KMP, and Rabin-Karp algorithms
- [ ] Understand when to use each algorithm
- [ ] Practice on simple string search problems
- [ ] Focus on understanding time complexity differences

### **Day 2: String Data Structures**
- [ ] Implement Trie from scratch
- [ ] Practice prefix-based problems
- [ ] Understand suffix arrays and their applications
- [ ] Build autocomplete functionality

### **Day 3: Advanced String Processing**
- [ ] Implement Manacher's algorithm for palindromes
- [ ] Master edit distance and LCS problems
- [ ] Practice string transformation problems
- [ ] Understand dynamic programming on strings

### **Day 4: Real-World Applications**
- [ ] Solve 5+ string problems on LeetCode
- [ ] Practice anagram and palindrome variants
- [ ] Implement string compression algorithms
- [ ] Build pattern recognition skills

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement multiple pattern matching algorithms
- [ ] You understand when to use tries vs other structures
- [ ] You can solve string DP problems confidently
- [ ] You recognize string patterns in complex problems

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Mathematical Algorithms](./17-math-algorithms-detailed-java-go.md)** - Number theory and computation
2. **System Design Algorithms** - Large-scale string processing
3. **Competitive Programming** - Advanced string techniques

### **🎯 Advanced String Topics:**
- **Suffix Trees** for linear time construction
- **Aho-Corasick** for multiple pattern matching
- **Burrows-Wheeler Transform** for compression
- **String Kernels** for machine learning applications

### **💡 Pro Tips:**
- **Choose the right algorithm** - Consider pattern size and text size
- **Preprocessing pays off** - Build structures for repeated queries
- **Practice pattern recognition** - Many problems are string problems in disguise
- **Understand real applications** - Text editors, search engines, bioinformatics
- **Master the fundamentals** - Good grasp of basic algorithms helps with advanced ones

**🎉 You now master advanced string algorithms! These sophisticated techniques will help you build efficient text processing systems and solve complex pattern matching problems.**