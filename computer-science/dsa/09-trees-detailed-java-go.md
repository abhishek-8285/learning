# 🌳 **TREES: DETAILED GUIDE (JAVA & GO)**

**Master Hierarchical Data Structures - From Binary Trees to Advanced Variants**

*Prerequisites: Complete recursion, linked lists, and basic data structures first*

---

## 🤔 **WHAT ARE TREES?**

### **Simple Explanation:**
A tree is like a **family tree** or **organizational chart**:
- **Root** = CEO or family patriarch (top node)
- **Parent** = Manager or parent (node with children)
- **Children** = Employees or kids (nodes below parent)
- **Leaves** = Entry-level or youngest generation (nodes with no children)
- **Hierarchy** = Clear levels and relationships

### **Visual Tree Structure:**

```
        10 (Root)
       /  \
      5    15 (Internal nodes)
     / \   / \
    3   7 12  20 (Leaves)
   /
  1

Levels:
0: 10
1: 5, 15  
2: 3, 7, 12, 20
3: 1
```

### **Real-World Examples:**

**🏢 Company Organization:**
- CEO → VPs → Directors → Managers → Employees
- Clear hierarchy, each person has one boss (parent)
- Some people manage others (internal nodes)
- Some have no direct reports (leaves)

**💻 File System:**
- Root directory → Folders → Subfolders → Files
- Each folder can contain other folders and files
- Files are leaves (no children)
- Folders are internal nodes

**🧬 Decision Making:**
- "Should I go out?" → "Is it raining?" → Yes/No branches
- Each decision point branches into options
- Final decisions are leaves

### **Why Learn Trees:**
- **Hierarchical data modeling** - Natural for many problems
- **Efficient operations** - O(log n) search, insert, delete
- **Foundation for advanced structures** - Heaps, tries, graphs
- **Interview essential** - 35% of coding interviews
- **Real applications** - Databases, file systems, parsers

---

## 🎯 **TREE TERMINOLOGY**

### **Basic Terms:**
- **Node** - Each element in the tree
- **Root** - Top node (only one, no parent)
- **Parent** - Node with children
- **Child** - Node below another node
- **Leaf** - Node with no children
- **Edge** - Connection between nodes
- **Path** - Sequence of nodes connected by edges
- **Subtree** - Tree rooted at any node

### **Measurements:**
- **Height** - Longest path from root to leaf
- **Depth** - Distance from root to specific node
- **Level** - All nodes at same depth
- **Degree** - Number of children a node has
- **Size** - Total number of nodes

### **Tree Properties:**
- **N nodes** → **N-1 edges** (always)
- **Connected** - Path exists between any two nodes
- **Acyclic** - No circular paths
- **Hierarchical** - Clear parent-child relationships

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Binary Tree Node and Basic Operations**

```java
import java.util.*;

// Binary Tree Node
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode() {}
    TreeNode(int val) { this.val = val; }
    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

public class BinaryTreeOperations {
    
    /**
     * Calculate height/depth of binary tree
     * Pattern: Recursive height calculation
     * Time: O(n), Space: O(h) where h is height
     */
    public static int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int leftHeight = maxDepth(root.left);
        int rightHeight = maxDepth(root.right);
        
        return 1 + Math.max(leftHeight, rightHeight);
    }
    
    /**
     * Check if tree is balanced (heights differ by at most 1)
     * Pattern: Bottom-up height checking
     * Time: O(n), Space: O(h)
     */
    public static boolean isBalanced(TreeNode root) {
        return checkBalance(root) != -1;
    }
    
    private static int checkBalance(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int leftHeight = checkBalance(root.left);
        if (leftHeight == -1) return -1; // Left subtree unbalanced
        
        int rightHeight = checkBalance(root.right);
        if (rightHeight == -1) return -1; // Right subtree unbalanced
        
        // Check if current node is balanced
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1; // Unbalanced
        }
        
        return 1 + Math.max(leftHeight, rightHeight);
    }
    
    /**
     * Count total nodes in tree
     * Pattern: Recursive counting
     * Time: O(n), Space: O(h)
     */
    public static int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
    
    /**
     * Sum all values in tree
     * Pattern: Recursive summation
     * Time: O(n), Space: O(h)
     */
    public static int sumTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        return root.val + sumTree(root.left) + sumTree(root.right);
    }
    
    /**
     * Check if two trees are identical
     * Pattern: Recursive structure and value comparison
     * Time: O(min(n,m)), Space: O(min(h1,h2))
     */
    public static boolean isSameTree(TreeNode p, TreeNode q) {
        // Both null
        if (p == null && q == null) {
            return true;
        }
        
        // One null, one not
        if (p == null || q == null) {
            return false;
        }
        
        // Compare values and recursively check subtrees
        return p.val == q.val && 
               isSameTree(p.left, q.left) && 
               isSameTree(p.right, q.right);
    }
    
    /**
     * Check if tree is symmetric (mirror of itself)
     * Pattern: Recursive mirror checking
     * Time: O(n), Space: O(h)
     */
    public static boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return isMirror(root.left, root.right);
    }
    
    private static boolean isMirror(TreeNode left, TreeNode right) {
        if (left == null && right == null) return true;
        if (left == null || right == null) return false;
        
        return left.val == right.val && 
               isMirror(left.left, right.right) && 
               isMirror(left.right, right.left);
    }
    
    /**
     * Find maximum value in tree
     * Pattern: Recursive max finding
     * Time: O(n), Space: O(h)
     */
    public static int findMax(TreeNode root) {
        if (root == null) {
            return Integer.MIN_VALUE;
        }
        
        int leftMax = findMax(root.left);
        int rightMax = findMax(root.right);
        
        return Math.max(root.val, Math.max(leftMax, rightMax));
    }
    
    /**
     * Search for value in tree
     * Pattern: Recursive search
     * Time: O(n), Space: O(h)
     */
    public static boolean search(TreeNode root, int target) {
        if (root == null) {
            return false;
        }
        
        if (root.val == target) {
            return true;
        }
        
        return search(root.left, target) || search(root.right, target);
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
        
        System.out.println("Tree height: " + maxDepth(root));        // 3
        System.out.println("Is balanced: " + isBalanced(root));      // true
        System.out.println("Node count: " + countNodes(root));       // 5
        System.out.println("Sum of tree: " + sumTree(root));         // 54
        System.out.println("Max value: " + findMax(root));           // 20
        System.out.println("Search 15: " + search(root, 15));        // true
        System.out.println("Search 100: " + search(root, 100));      // false
        System.out.println("Is symmetric: " + isSymmetric(root));    // false
    }
}
```

### **Tree Traversal Methods**

```java
import java.util.*;

public class TreeTraversal {
    
    /**
     * Inorder traversal (Left, Root, Right)
     * Pattern: Recursive DFS
     * Time: O(n), Space: O(h)
     */
    public static List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }
    
    private static void inorderHelper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        inorderHelper(root.left, result);   // Left
        result.add(root.val);               // Root
        inorderHelper(root.right, result);  // Right
    }
    
    /**
     * Inorder traversal iterative using stack
     * Pattern: Iterative DFS with stack
     * Time: O(n), Space: O(h)
     */
    public static List<Integer> inorderIterative(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode current = root;
        
        while (current != null || !stack.isEmpty()) {
            // Go to leftmost node
            while (current != null) {
                stack.push(current);
                current = current.left;
            }
            
            // Process current node
            current = stack.pop();
            result.add(current.val);
            
            // Move to right subtree
            current = current.right;
        }
        
        return result;
    }
    
    /**
     * Preorder traversal (Root, Left, Right)
     * Pattern: Recursive DFS
     * Time: O(n), Space: O(h)
     */
    public static List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderHelper(root, result);
        return result;
    }
    
    private static void preorderHelper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        result.add(root.val);               // Root
        preorderHelper(root.left, result);  // Left
        preorderHelper(root.right, result); // Right
    }
    
    /**
     * Postorder traversal (Left, Right, Root)
     * Pattern: Recursive DFS
     * Time: O(n), Space: O(h)
     */
    public static List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorderHelper(root, result);
        return result;
    }
    
    private static void postorderHelper(TreeNode root, List<Integer> result) {
        if (root == null) return;
        
        postorderHelper(root.left, result);  // Left
        postorderHelper(root.right, result); // Right
        result.add(root.val);                // Root
    }
    
    /**
     * Level order traversal (BFS)
     * Pattern: BFS with queue
     * Time: O(n), Space: O(w) where w is maximum width
     */
    public static List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                currentLevel.add(node.val);
                
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            
            result.add(currentLevel);
        }
        
        return result;
    }
    
    /**
     * Zigzag level order traversal
     * Pattern: BFS with alternating direction
     * Time: O(n), Space: O(w)
     */
    public static List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        boolean leftToRight = true;
        
        while (!queue.isEmpty()) {
            int levelSize = queue.size();
            List<Integer> currentLevel = new ArrayList<>();
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode node = queue.poll();
                
                if (leftToRight) {
                    currentLevel.add(node.val);
                } else {
                    currentLevel.add(0, node.val); // Add at beginning
                }
                
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            
            result.add(currentLevel);
            leftToRight = !leftToRight; // Flip direction
        }
        
        return result;
    }
    
    /**
     * Vertical order traversal
     * Pattern: BFS with column tracking
     * Time: O(n log n), Space: O(n)
     */
    public static List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        
        Map<Integer, List<Integer>> columnMap = new TreeMap<>();
        Queue<TreeNode> nodeQueue = new LinkedList<>();
        Queue<Integer> columnQueue = new LinkedList<>();
        
        nodeQueue.offer(root);
        columnQueue.offer(0);
        
        while (!nodeQueue.isEmpty()) {
            TreeNode node = nodeQueue.poll();
            int column = columnQueue.poll();
            
            columnMap.computeIfAbsent(column, k -> new ArrayList<>()).add(node.val);
            
            if (node.left != null) {
                nodeQueue.offer(node.left);
                columnQueue.offer(column - 1);
            }
            
            if (node.right != null) {
                nodeQueue.offer(node.right);
                columnQueue.offer(column + 1);
            }
        }
        
        for (List<Integer> column : columnMap.values()) {
            result.add(column);
        }
        
        return result;
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
        
        System.out.println("Inorder: " + inorderTraversal(root));     // [9, 3, 15, 20, 7]
        System.out.println("Preorder: " + preorderTraversal(root));   // [3, 9, 20, 15, 7]
        System.out.println("Postorder: " + postorderTraversal(root)); // [9, 15, 7, 20, 3]
        System.out.println("Level order: " + levelOrder(root));       // [[3], [9, 20], [15, 7]]
        System.out.println("Zigzag: " + zigzagLevelOrder(root));      // [[3], [20, 9], [15, 7]]
    }
}
```

### **Advanced Tree Problems**

```java
import java.util.*;

public class AdvancedTreeProblems {
    
    /**
     * Find lowest common ancestor (LCA)
     * Pattern: Recursive LCA finding
     * Time: O(n), Space: O(h)
     */
    public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        
        if (left != null && right != null) {
            return root; // Both found in different subtrees
        }
        
        return left != null ? left : right; // Return the non-null one
    }
    
    /**
     * Maximum path sum (any node to any node)
     * Pattern: Recursive path sum with global maximum
     * Time: O(n), Space: O(h)
     */
    private static int maxPathSum = Integer.MIN_VALUE;
    
    public static int maxPathSum(TreeNode root) {
        maxPathSum = Integer.MIN_VALUE;
        maxPathSumHelper(root);
        return maxPathSum;
    }
    
    private static int maxPathSumHelper(TreeNode root) {
        if (root == null) return 0;
        
        // Get max path sum from left and right (ignore negative paths)
        int leftMax = Math.max(0, maxPathSumHelper(root.left));
        int rightMax = Math.max(0, maxPathSumHelper(root.right));
        
        // Update global maximum (path through current node)
        int currentMax = root.val + leftMax + rightMax;
        maxPathSum = Math.max(maxPathSum, currentMax);
        
        // Return max path sum ending at current node
        return root.val + Math.max(leftMax, rightMax);
    }
    
    /**
     * Diameter of binary tree (longest path between any two nodes)
     * Pattern: Height calculation with diameter tracking
     * Time: O(n), Space: O(h)
     */
    private static int diameter = 0;
    
    public static int diameterOfBinaryTree(TreeNode root) {
        diameter = 0;
        height(root);
        return diameter;
    }
    
    private static int height(TreeNode root) {
        if (root == null) return 0;
        
        int leftHeight = height(root.left);
        int rightHeight = height(root.right);
        
        // Update diameter (path through current node)
        diameter = Math.max(diameter, leftHeight + rightHeight);
        
        return 1 + Math.max(leftHeight, rightHeight);
    }
    
    /**
     * Construct binary tree from inorder and preorder traversal
     * Pattern: Recursive tree construction
     * Time: O(n), Space: O(n)
     */
    private static int preorderIndex = 0;
    
    public static TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderIndex = 0;
        Map<Integer, Integer> inorderIndexMap = new HashMap<>();
        
        for (int i = 0; i < inorder.length; i++) {
            inorderIndexMap.put(inorder[i], i);
        }
        
        return buildTreeHelper(preorder, inorderIndexMap, 0, inorder.length - 1);
    }
    
    private static TreeNode buildTreeHelper(int[] preorder, Map<Integer, Integer> inorderIndexMap, 
                                          int inorderStart, int inorderEnd) {
        if (inorderStart > inorderEnd) {
            return null;
        }
        
        int rootVal = preorder[preorderIndex++];
        TreeNode root = new TreeNode(rootVal);
        
        int rootIndex = inorderIndexMap.get(rootVal);
        
        // Build left subtree first (preorder: root, left, right)
        root.left = buildTreeHelper(preorder, inorderIndexMap, inorderStart, rootIndex - 1);
        root.right = buildTreeHelper(preorder, inorderIndexMap, rootIndex + 1, inorderEnd);
        
        return root;
    }
    
    /**
     * Serialize and deserialize binary tree
     * Pattern: Preorder traversal with null markers
     */
    public static String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        serializeHelper(root, sb);
        return sb.toString();
    }
    
    private static void serializeHelper(TreeNode root, StringBuilder sb) {
        if (root == null) {
            sb.append("null,");
            return;
        }
        
        sb.append(root.val).append(",");
        serializeHelper(root.left, sb);
        serializeHelper(root.right, sb);
    }
    
    public static TreeNode deserialize(String data) {
        Queue<String> nodes = new LinkedList<>(Arrays.asList(data.split(",")));
        return deserializeHelper(nodes);
    }
    
    private static TreeNode deserializeHelper(Queue<String> nodes) {
        String val = nodes.poll();
        if ("null".equals(val)) {
            return null;
        }
        
        TreeNode root = new TreeNode(Integer.parseInt(val));
        root.left = deserializeHelper(nodes);
        root.right = deserializeHelper(nodes);
        
        return root;
    }
    
    /**
     * Convert sorted array to balanced BST
     * Pattern: Recursive middle element selection
     * Time: O(n), Space: O(log n)
     */
    public static TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBSTHelper(nums, 0, nums.length - 1);
    }
    
    private static TreeNode sortedArrayToBSTHelper(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }
        
        int mid = left + (right - left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        
        root.left = sortedArrayToBSTHelper(nums, left, mid - 1);
        root.right = sortedArrayToBSTHelper(nums, mid + 1, right);
        
        return root;
    }
    
    /**
     * Validate binary search tree
     * Pattern: Recursive validation with bounds
     * Time: O(n), Space: O(h)
     */
    public static boolean isValidBST(TreeNode root) {
        return isValidBSTHelper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    private static boolean isValidBSTHelper(TreeNode root, long minVal, long maxVal) {
        if (root == null) {
            return true;
        }
        
        if (root.val <= minVal || root.val >= maxVal) {
            return false;
        }
        
        return isValidBSTHelper(root.left, minVal, root.val) && 
               isValidBSTHelper(root.right, root.val, maxVal);
    }
    
    public static void main(String[] args) {
        // Create sample tree for testing
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);
        
        System.out.println("Max path sum: " + maxPathSum(root));           // Test max path sum
        System.out.println("Diameter: " + diameterOfBinaryTree(root));     // Test diameter
        
        // Test serialization
        String serialized = serialize(root);
        System.out.println("Serialized: " + serialized);
        TreeNode deserialized = deserialize(serialized);
        System.out.println("Deserialized successfully: " + (deserialized != null));
        
        // Test sorted array to BST
        int[] sortedArray = {-10, -3, 0, 5, 9};
        TreeNode bst = sortedArrayToBST(sortedArray);
        System.out.println("BST created from sorted array: " + (bst != null));
        System.out.println("Is valid BST: " + isValidBST(bst));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Binary Tree Node and Basic Operations in Go**

```go
package main

import (
    "fmt"
    "math"
    "strconv"
    "strings"
)

// TreeNode structure
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

/**
 * Calculate height/depth of binary tree
 * Pattern: Recursive height calculation
 * Time: O(n), Space: O(h) where h is height
 */
func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    
    leftHeight := maxDepth(root.Left)
    rightHeight := maxDepth(root.Right)
    
    return 1 + max(leftHeight, rightHeight)
}

/**
 * Check if tree is balanced
 * Pattern: Bottom-up height checking
 * Time: O(n), Space: O(h)
 */
func isBalanced(root *TreeNode) bool {
    _, balanced := checkBalance(root)
    return balanced
}

func checkBalance(root *TreeNode) (int, bool) {
    if root == nil {
        return 0, true
    }
    
    leftHeight, leftBalanced := checkBalance(root.Left)
    if !leftBalanced {
        return 0, false
    }
    
    rightHeight, rightBalanced := checkBalance(root.Right)
    if !rightBalanced {
        return 0, false
    }
    
    // Check if current node is balanced
    if abs(leftHeight-rightHeight) > 1 {
        return 0, false
    }
    
    return 1 + max(leftHeight, rightHeight), true
}

/**
 * Count total nodes in tree
 * Pattern: Recursive counting
 * Time: O(n), Space: O(h)
 */
func countNodes(root *TreeNode) int {
    if root == nil {
        return 0
    }
    
    return 1 + countNodes(root.Left) + countNodes(root.Right)
}

/**
 * Sum all values in tree
 * Pattern: Recursive summation
 * Time: O(n), Space: O(h)
 */
func sumTree(root *TreeNode) int {
    if root == nil {
        return 0
    }
    
    return root.Val + sumTree(root.Left) + sumTree(root.Right)
}

/**
 * Check if two trees are identical
 * Pattern: Recursive structure and value comparison
 * Time: O(min(n,m)), Space: O(min(h1,h2))
 */
func isSameTree(p, q *TreeNode) bool {
    // Both nil
    if p == nil && q == nil {
        return true
    }
    
    // One nil, one not
    if p == nil || q == nil {
        return false
    }
    
    // Compare values and recursively check subtrees
    return p.Val == q.Val && 
           isSameTree(p.Left, q.Left) && 
           isSameTree(p.Right, q.Right)
}

/**
 * Check if tree is symmetric
 * Pattern: Recursive mirror checking
 * Time: O(n), Space: O(h)
 */
func isSymmetric(root *TreeNode) bool {
    if root == nil {
        return true
    }
    return isMirror(root.Left, root.Right)
}

func isMirror(left, right *TreeNode) bool {
    if left == nil && right == nil {
        return true
    }
    if left == nil || right == nil {
        return false
    }
    
    return left.Val == right.Val && 
           isMirror(left.Left, right.Right) && 
           isMirror(left.Right, right.Left)
}

/**
 * Find maximum value in tree
 * Pattern: Recursive max finding
 * Time: O(n), Space: O(h)
 */
func findMax(root *TreeNode) int {
    if root == nil {
        return math.MinInt32
    }
    
    leftMax := findMax(root.Left)
    rightMax := findMax(root.Right)
    
    return max(root.Val, max(leftMax, rightMax))
}

/**
 * Search for value in tree
 * Pattern: Recursive search
 * Time: O(n), Space: O(h)
 */
func search(root *TreeNode, target int) bool {
    if root == nil {
        return false
    }
    
    if root.Val == target {
        return true
    }
    
    return search(root.Left, target) || search(root.Right, target)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func abs(a int) int {
    if a < 0 {
        return -a
    }
    return a
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
    
    fmt.Printf("Tree height: %d\n", maxDepth(root))        // 3
    fmt.Printf("Is balanced: %t\n", isBalanced(root))      // true
    fmt.Printf("Node count: %d\n", countNodes(root))       // 5
    fmt.Printf("Sum of tree: %d\n", sumTree(root))         // 54
    fmt.Printf("Max value: %d\n", findMax(root))           // 20
    fmt.Printf("Search 15: %t\n", search(root, 15))        // true
    fmt.Printf("Search 100: %t\n", search(root, 100))      // false
    fmt.Printf("Is symmetric: %t\n", isSymmetric(root))    // false
}
```

### **Tree Traversal in Go**

```go
package main

import "fmt"

/**
 * Inorder traversal (Left, Root, Right)
 * Pattern: Recursive DFS
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
 * Preorder traversal (Root, Left, Right)
 * Pattern: Recursive DFS
 * Time: O(n), Space: O(h)
 */
func preorderTraversal(root *TreeNode) []int {
    var result []int
    preorderHelper(root, &result)
    return result
}

func preorderHelper(root *TreeNode, result *[]int) {
    if root == nil {
        return
    }
    
    *result = append(*result, root.Val) // Root
    preorderHelper(root.Left, result)   // Left
    preorderHelper(root.Right, result)  // Right
}

/**
 * Postorder traversal (Left, Right, Root)
 * Pattern: Recursive DFS
 * Time: O(n), Space: O(h)
 */
func postorderTraversal(root *TreeNode) []int {
    var result []int
    postorderHelper(root, &result)
    return result
}

func postorderHelper(root *TreeNode, result *[]int) {
    if root == nil {
        return
    }
    
    postorderHelper(root.Left, result)  // Left
    postorderHelper(root.Right, result) // Right
    *result = append(*result, root.Val) // Root
}

/**
 * Level order traversal (BFS)
 * Pattern: BFS with queue
 * Time: O(n), Space: O(w) where w is maximum width
 */
func levelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result
    }
    
    queue := []*TreeNode{root}
    
    for len(queue) > 0 {
        levelSize := len(queue)
        var currentLevel []int
        
        for i := 0; i < levelSize; i++ {
            node := queue[0]
            queue = queue[1:]
            currentLevel = append(currentLevel, node.Val)
            
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
        
        result = append(result, currentLevel)
    }
    
    return result
}

/**
 * Zigzag level order traversal
 * Pattern: BFS with alternating direction
 * Time: O(n), Space: O(w)
 */
func zigzagLevelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result
    }
    
    queue := []*TreeNode{root}
    leftToRight := true
    
    for len(queue) > 0 {
        levelSize := len(queue)
        currentLevel := make([]int, levelSize)
        
        for i := 0; i < levelSize; i++ {
            node := queue[0]
            queue = queue[1:]
            
            var index int
            if leftToRight {
                index = i
            } else {
                index = levelSize - 1 - i
            }
            currentLevel[index] = node.Val
            
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
        
        result = append(result, currentLevel)
        leftToRight = !leftToRight
    }
    
    return result
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
    
    fmt.Printf("Inorder: %v\n", inorderTraversal(root))     // [9 3 15 20 7]
    fmt.Printf("Preorder: %v\n", preorderTraversal(root))   // [3 9 20 15 7]
    fmt.Printf("Postorder: %v\n", postorderTraversal(root)) // [9 15 7 20 3]
    fmt.Printf("Level order: %v\n", levelOrder(root))       // [[3] [9 20] [15 7]]
    fmt.Printf("Zigzag: %v\n", zigzagLevelOrder(root))      // [[3] [20 9] [15 7]]
}
```

---

## 🧠 **UNDERSTANDING TREE PATTERNS**

### **Pattern 1: Recursive Tree Processing**
```java
public int processTree(TreeNode root) {
    // Base case
    if (root == null) {
        return baseValue;
    }
    
    // Process subtrees
    int leftResult = processTree(root.left);
    int rightResult = processTree(root.right);
    
    // Combine results
    return combineResults(root.val, leftResult, rightResult);
}
```
**Use for:** Height, sum, count, max/min, validation

### **Pattern 2: Level-by-Level Processing (BFS)**
```java
public void levelOrder(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            // Process current node
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
}
```
**Use for:** Level order traversal, zigzag, finding level information

### **Pattern 3: Path Tracking**
```java
public void findPaths(TreeNode root, List<Integer> currentPath, List<List<Integer>> allPaths) {
    if (root == null) return;
    
    currentPath.add(root.val);
    
    if (root.left == null && root.right == null) {
        // Leaf node - path complete
        allPaths.add(new ArrayList<>(currentPath));
    } else {
        findPaths(root.left, currentPath, allPaths);
        findPaths(root.right, currentPath, allPaths);
    }
    
    currentPath.remove(currentPath.size() - 1); // Backtrack
}
```
**Use for:** Root-to-leaf paths, path sum problems

### **Pattern 4: Tree Construction**
```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    // Use preorder for root selection
    // Use inorder for left/right subtree division
    // Recursively build subtrees
}
```
**Use for:** Building trees from traversals, serialization

---

## 📊 **TREE COMPLEXITY ANALYSIS**

| Operation | Binary Tree | Binary Search Tree | Balanced BST |
|-----------|-------------|-------------------|--------------|
| **Search** | O(n) | O(h) | O(log n) |
| **Insert** | O(1)* | O(h) | O(log n) |
| **Delete** | O(n) | O(h) | O(log n) |
| **Traversal** | O(n) | O(n) | O(n) |
| **Height** | O(n) | O(1)** | O(1)** |
| **Space** | O(n) | O(n) | O(n) |

*If you have reference to insertion point  
**If height is maintained

### **Height Impact:**
- **Balanced tree:** h = O(log n) → All operations are efficient
- **Skewed tree:** h = O(n) → Degrades to linear time
- **Average case:** h = O(log n) for random insertions

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Maximum Depth of Binary Tree** ✅ (Implemented above)
2. **Same Tree** ✅ (Implemented above)
3. **Symmetric Tree** ✅ (Implemented above)
4. **Invert Binary Tree**
5. **Path Sum** (Root to leaf)

### **Medium Problems:**

1. **Binary Tree Level Order Traversal** ✅ (Implemented above)
2. **Maximum Path Sum** ✅ (Implemented above)
3. **Lowest Common Ancestor** ✅ (Implemented above)
4. **Diameter of Binary Tree** ✅ (Implemented above)
5. **Construct Binary Tree from Traversals** ✅ (Implemented above)

### **Hard Problems:**

1. **Serialize and Deserialize Binary Tree** ✅ (Implemented above)
2. **Binary Tree Maximum Path Sum**
3. **Recover Binary Search Tree**
4. **Binary Tree Cameras**
5. **House Robber III**

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Tree Fundamentals**
- [ ] Understand tree terminology and structure
- [ ] Implement basic tree operations (height, count, sum)
- [ ] Practice recursive thinking with trees
- [ ] Visualize tree structures on paper

### **Day 2: Master Tree Traversals**
- [ ] Implement all four traversal methods
- [ ] Practice both recursive and iterative approaches
- [ ] Understand when to use each traversal type
- [ ] Solve level order and zigzag problems

### **Day 3: Advanced Tree Operations**
- [ ] Solve maximum path sum problem
- [ ] Implement tree construction from traversals
- [ ] Practice LCA and diameter problems
- [ ] Understand tree validation techniques

### **Day 4: Binary Search Trees**
- [ ] Understand BST properties and operations
- [ ] Practice BST validation and construction
- [ ] Solve search and insertion problems
- [ ] Compare BST with regular binary trees

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement tree operations recursively
- [ ] You understand all traversal methods
- [ ] You can solve tree problems using BFS and DFS
- [ ] You recognize tree patterns in problems

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Dynamic Programming - Basics](./10-dynamic-programming-basics-detailed-java-go.md)** - Learn optimization techniques
2. **[Sorting Algorithms - Detailed](./12-sorting-detailed-java-go.md)** - Master efficient sorting
3. **[Graph Algorithms - Detailed](./15-graphs-detailed-java-go.md)** - Extend to graph structures

### **🎯 Advanced Tree Topics:**
- **AVL Trees** and **Red-Black Trees** for self-balancing
- **Trie (Prefix Tree)** for string operations
- **Segment Trees** and **Fenwick Trees** for range queries
- **B-Trees** for database indexing

### **💡 Pro Tips:**
- **Visualize recursion** - Draw the call stack for complex problems
- **Practice both approaches** - Recursive and iterative solutions
- **Understand tree properties** - Use them to optimize solutions
- **Master traversals** - Foundation for most tree algorithms
- **Think in terms of subtrees** - Break problems into smaller tree problems

**🎉 You now master hierarchical data structures! Trees are fundamental to many advanced algorithms and real-world applications like databases, file systems, and compilers.**