# 🎯 **GREEDY ALGORITHMS: DETAILED GUIDE (JAVA & GO)**

**Master Optimization Through Locally Optimal Choices**

*Prerequisites: Complete sorting, trees, and basic algorithms first*

---

## 🤔 **WHAT ARE GREEDY ALGORITHMS?**

### **Simple Explanation:**
Greedy algorithms are like a **hungry person at a buffet**:
- **Look at immediate options** (local choices)
- **Pick the best looking dish** right now (locally optimal)
- **Never go back** to change your mind (no backtracking)
- **Hope your strategy** leads to the best overall meal (globally optimal)

### **Real-World Example - Making Change:**

**Problem:** Give change for $0.67 using US coins (quarters, dimes, nickels, pennies)

**Greedy Strategy:**
1. Use **largest coin** that doesn't exceed remaining amount
2. **Repeat** until no money left

```
$0.67 → Use 2 quarters ($0.50) → $0.17 remaining
$0.17 → Use 1 dime ($0.10) → $0.07 remaining  
$0.07 → Use 1 nickel ($0.05) → $0.02 remaining
$0.02 → Use 2 pennies ($0.02) → $0.00 remaining

Result: 2 quarters + 1 dime + 1 nickel + 2 pennies = 6 coins (optimal!)
```

### **Greedy vs Other Approaches:**

**🎯 Greedy Algorithm:**
- **Make best local choice** at each step
- **Never reconsider** previous decisions
- **Fast and simple** - Often O(n log n) or O(n)
- **May not always** find global optimum

**💎 Dynamic Programming:**
- **Consider all possibilities** systematically
- **Build optimal solution** from subproblems
- **Guaranteed optimal** but slower
- **Higher complexity** - Often O(n²) or O(n³)

**🔄 Backtracking:**
- **Try all possible paths**
- **Backtrack when** hitting dead ends
- **Exhaustive search** with pruning
- **Very slow** - Often exponential time

---

## 🎯 **WHEN GREEDY WORKS**

### **Greedy Choice Property:**
A **globally optimal solution** can be reached by making **locally optimal choices**.

### **Optimal Substructure:**
An optimal solution contains **optimal solutions to subproblems**.

### **Classic Examples Where Greedy Works:**
1. **Activity Selection** - Choose non-overlapping activities
2. **Huffman Coding** - Build optimal compression tree
3. **Minimum Spanning Tree** - Connect all nodes with minimum cost
4. **Dijkstra's Algorithm** - Find shortest paths
5. **Fractional Knapsack** - Take items by value-to-weight ratio

### **When Greedy Fails:**
1. **0/1 Knapsack** - Cannot take fractions of items
2. **Longest Path** - Local choices may block global optimum
3. **Coin Change** - For some coin systems (e.g., coins: 1, 3, 4 for amount 6)

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Classic Greedy Problems**

```java
import java.util.*;

public class GreedyAlgorithms {
    
    /**
     * Activity Selection - Select maximum number of non-overlapping activities
     * Pattern: Sort by end time, greedily select earliest ending
     * Time: O(n log n), Space: O(1)
     */
    public static class Activity {
        int start, end;
        
        Activity(int start, int end) {
            this.start = start;
            this.end = end;
        }
        
        @Override
        public String toString() {
            return "(" + start + "," + end + ")";
        }
    }
    
    public static List<Activity> selectActivities(Activity[] activities) {
        // Sort by end time (greedy choice: earliest ending first)
        Arrays.sort(activities, (a, b) -> a.end - b.end);
        
        List<Activity> selected = new ArrayList<>();
        selected.add(activities[0]); // Always select first activity
        
        int lastEndTime = activities[0].end;
        
        for (int i = 1; i < activities.length; i++) {
            // Select activity if it starts after last selected activity ends
            if (activities[i].start >= lastEndTime) {
                selected.add(activities[i]);
                lastEndTime = activities[i].end;
            }
        }
        
        return selected;
    }
    
    /**
     * Fractional Knapsack - Take items to maximize value within weight limit
     * Pattern: Sort by value-to-weight ratio, take greedily
     * Time: O(n log n), Space: O(1)
     */
    public static class Item {
        int value, weight;
        double ratio;
        
        Item(int value, int weight) {
            this.value = value;
            this.weight = weight;
            this.ratio = (double) value / weight;
        }
        
        @Override
        public String toString() {
            return String.format("V:%d W:%d R:%.2f", value, weight, ratio);
        }
    }
    
    public static double fractionalKnapsack(Item[] items, int capacity) {
        // Sort by value-to-weight ratio (greedy choice: highest ratio first)
        Arrays.sort(items, (a, b) -> Double.compare(b.ratio, a.ratio));
        
        double totalValue = 0.0;
        int remainingCapacity = capacity;
        
        for (Item item : items) {
            if (remainingCapacity >= item.weight) {
                // Take entire item
                totalValue += item.value;
                remainingCapacity -= item.weight;
            } else {
                // Take fraction of item
                double fraction = (double) remainingCapacity / item.weight;
                totalValue += item.value * fraction;
                break; // Knapsack is full
            }
        }
        
        return totalValue;
    }
    
    /**
     * Job Scheduling - Schedule jobs to minimize total completion time
     * Pattern: Sort by duration, process shortest first
     * Time: O(n log n), Space: O(1)
     */
    public static class Job {
        String name;
        int duration;
        
        Job(String name, int duration) {
            this.name = name;
            this.duration = duration;
        }
        
        @Override
        public String toString() {
            return name + "(" + duration + ")";
        }
    }
    
    public static List<Job> scheduleJobs(Job[] jobs) {
        // Sort by duration (greedy choice: shortest job first)
        Arrays.sort(jobs, (a, b) -> a.duration - b.duration);
        
        return Arrays.asList(jobs);
    }
    
    /**
     * Minimum Platforms - Find minimum platforms needed for train schedule
     * Pattern: Sort events, count overlapping intervals
     * Time: O(n log n), Space: O(n)
     */
    public static class Train {
        int arrival, departure;
        
        Train(int arrival, int departure) {
            this.arrival = arrival;
            this.departure = departure;
        }
    }
    
    public static int minPlatforms(Train[] trains) {
        int n = trains.length;
        int[] arrivals = new int[n];
        int[] departures = new int[n];
        
        for (int i = 0; i < n; i++) {
            arrivals[i] = trains[i].arrival;
            departures[i] = trains[i].departure;
        }
        
        // Sort arrival and departure times
        Arrays.sort(arrivals);
        Arrays.sort(departures);
        
        int platforms = 0, maxPlatforms = 0;
        int i = 0, j = 0;
        
        while (i < n && j < n) {
            if (arrivals[i] <= departures[j]) {
                platforms++; // Train arrives, need platform
                maxPlatforms = Math.max(maxPlatforms, platforms);
                i++;
            } else {
                platforms--; // Train departs, free platform
                j++;
            }
        }
        
        return maxPlatforms;
    }
    
    /**
     * Gas Station - Find starting station to complete circular route
     * Pattern: Track running balance, greedy starting point
     * Time: O(n), Space: O(1)
     */
    public static int canCompleteCircuit(int[] gas, int[] cost) {
        int totalGas = 0, totalCost = 0;
        int currentGas = 0, startStation = 0;
        
        for (int i = 0; i < gas.length; i++) {
            totalGas += gas[i];
            totalCost += cost[i];
            currentGas += gas[i] - cost[i];
            
            // If we can't reach next station, start from next station
            if (currentGas < 0) {
                startStation = i + 1;
                currentGas = 0;
            }
        }
        
        // Check if total gas is sufficient
        return totalGas >= totalCost ? startStation : -1;
    }
    
    public static void main(String[] args) {
        // Test Activity Selection
        Activity[] activities = {
            new Activity(1, 4), new Activity(3, 5), new Activity(0, 6),
            new Activity(5, 7), new Activity(3, 9), new Activity(5, 9),
            new Activity(6, 10), new Activity(8, 11), new Activity(8, 12),
            new Activity(2, 14), new Activity(12, 16)
        };
        
        List<Activity> selected = selectActivities(activities);
        System.out.println("Selected activities: " + selected);
        
        // Test Fractional Knapsack
        Item[] items = {
            new Item(60, 10), new Item(100, 20), new Item(120, 30)
        };
        double maxValue = fractionalKnapsack(items, 50);
        System.out.println("Maximum value: " + maxValue);
        
        // Test Job Scheduling
        Job[] jobs = {
            new Job("A", 3), new Job("B", 1), new Job("C", 4), new Job("D", 2)
        };
        List<Job> schedule = scheduleJobs(jobs);
        System.out.println("Job schedule: " + schedule);
        
        // Test Minimum Platforms
        Train[] trains = {
            new Train(900, 910), new Train(940, 1200), new Train(950, 1120),
            new Train(1100, 1130), new Train(1500, 1900), new Train(1800, 2000)
        };
        int platforms = minPlatforms(trains);
        System.out.println("Minimum platforms needed: " + platforms);
        
        // Test Gas Station
        int[] gas = {1, 2, 3, 4, 5};
        int[] cost = {3, 4, 5, 1, 2};
        int startStation = canCompleteCircuit(gas, cost);
        System.out.println("Starting gas station: " + startStation);
    }
}
```

### **Advanced Greedy Problems**

```java
import java.util.*;

public class AdvancedGreedyProblems {
    
    /**
     * Huffman Coding - Build optimal compression tree
     * Pattern: Build tree bottom-up, always merge two smallest
     * Time: O(n log n), Space: O(n)
     */
    public static class HuffmanNode {
        char character;
        int frequency;
        HuffmanNode left, right;
        
        HuffmanNode(char character, int frequency) {
            this.character = character;
            this.frequency = frequency;
        }
        
        HuffmanNode(int frequency, HuffmanNode left, HuffmanNode right) {
            this.frequency = frequency;
            this.left = left;
            this.right = right;
        }
        
        boolean isLeaf() {
            return left == null && right == null;
        }
    }
    
    public static HuffmanNode buildHuffmanTree(Map<Character, Integer> frequencies) {
        // Create priority queue (min heap) of nodes
        PriorityQueue<HuffmanNode> pq = new PriorityQueue<>(
            (a, b) -> a.frequency - b.frequency
        );
        
        // Add all characters as leaf nodes
        for (Map.Entry<Character, Integer> entry : frequencies.entrySet()) {
            pq.offer(new HuffmanNode(entry.getKey(), entry.getValue()));
        }
        
        // Build tree bottom-up
        while (pq.size() > 1) {
            HuffmanNode left = pq.poll();
            HuffmanNode right = pq.poll();
            
            HuffmanNode merged = new HuffmanNode(
                left.frequency + right.frequency, left, right
            );
            
            pq.offer(merged);
        }
        
        return pq.poll();
    }
    
    public static Map<Character, String> getHuffmanCodes(HuffmanNode root) {
        Map<Character, String> codes = new HashMap<>();
        if (root != null) {
            generateCodes(root, "", codes);
        }
        return codes;
    }
    
    private static void generateCodes(HuffmanNode node, String code, 
                                    Map<Character, String> codes) {
        if (node.isLeaf()) {
            codes.put(node.character, code.isEmpty() ? "0" : code);
        } else {
            if (node.left != null) {
                generateCodes(node.left, code + "0", codes);
            }
            if (node.right != null) {
                generateCodes(node.right, code + "1", codes);
            }
        }
    }
    
    /**
     * Minimum Spanning Tree - Kruskal's Algorithm
     * Pattern: Sort edges, greedily add if no cycle
     * Time: O(E log E), Space: O(V)
     */
    public static class Edge {
        int src, dest, weight;
        
        Edge(int src, int dest, int weight) {
            this.src = src;
            this.dest = dest;
            this.weight = weight;
        }
        
        @Override
        public String toString() {
            return String.format("(%d-%d: %d)", src, dest, weight);
        }
    }
    
    public static class UnionFind {
        int[] parent, rank;
        
        UnionFind(int n) {
            parent = new int[n];
            rank = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
        }
        
        int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]); // Path compression
            }
            return parent[x];
        }
        
        boolean union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            
            if (rootX == rootY) {
                return false; // Already connected (would create cycle)
            }
            
            // Union by rank
            if (rank[rootX] < rank[rootY]) {
                parent[rootX] = rootY;
            } else if (rank[rootX] > rank[rootY]) {
                parent[rootY] = rootX;
            } else {
                parent[rootY] = rootX;
                rank[rootX]++;
            }
            
            return true;
        }
    }
    
    public static List<Edge> kruskalMST(int vertices, Edge[] edges) {
        // Sort edges by weight (greedy choice: smallest weight first)
        Arrays.sort(edges, (a, b) -> a.weight - b.weight);
        
        List<Edge> mst = new ArrayList<>();
        UnionFind uf = new UnionFind(vertices);
        
        for (Edge edge : edges) {
            // Add edge if it doesn't create cycle
            if (uf.union(edge.src, edge.dest)) {
                mst.add(edge);
                
                // Stop when we have V-1 edges
                if (mst.size() == vertices - 1) {
                    break;
                }
            }
        }
        
        return mst;
    }
    
    /**
     * Jump Game - Check if can reach end of array
     * Pattern: Greedy farthest reachable position
     * Time: O(n), Space: O(1)
     */
    public static boolean canJump(int[] nums) {
        int farthest = 0;
        
        for (int i = 0; i < nums.length; i++) {
            // If current position is beyond farthest reachable, can't proceed
            if (i > farthest) {
                return false;
            }
            
            // Update farthest reachable position
            farthest = Math.max(farthest, i + nums[i]);
            
            // If we can reach or exceed the last index
            if (farthest >= nums.length - 1) {
                return true;
            }
        }
        
        return false;
    }
    
    /**
     * Jump Game II - Minimum jumps to reach end
     * Pattern: Greedy range expansion
     * Time: O(n), Space: O(1)
     */
    public static int minJumps(int[] nums) {
        if (nums.length <= 1) return 0;
        
        int jumps = 0;
        int currentEnd = 0;
        int farthest = 0;
        
        for (int i = 0; i < nums.length - 1; i++) {
            // Update farthest reachable position
            farthest = Math.max(farthest, i + nums[i]);
            
            // If we've reached the end of current range
            if (i == currentEnd) {
                jumps++;
                currentEnd = farthest;
                
                // If we can reach the end
                if (currentEnd >= nums.length - 1) {
                    break;
                }
            }
        }
        
        return jumps;
    }
    
    /**
     * Candy Distribution - Minimum candies satisfying neighbor constraints
     * Pattern: Two-pass greedy (left-to-right, right-to-left)
     * Time: O(n), Space: O(n)
     */
    public static int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
        Arrays.fill(candies, 1); // Everyone gets at least 1 candy
        
        // Left to right: ensure right neighbor gets more if higher rating
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }
        
        // Right to left: ensure left neighbor gets more if higher rating
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = Math.max(candies[i], candies[i + 1] + 1);
            }
        }
        
        return Arrays.stream(candies).sum();
    }
    
    public static void main(String[] args) {
        // Test Huffman Coding
        Map<Character, Integer> frequencies = new HashMap<>();
        frequencies.put('a', 5);
        frequencies.put('b', 9);
        frequencies.put('c', 12);
        frequencies.put('d', 13);
        frequencies.put('e', 16);
        frequencies.put('f', 45);
        
        HuffmanNode root = buildHuffmanTree(frequencies);
        Map<Character, String> codes = getHuffmanCodes(root);
        System.out.println("Huffman codes: " + codes);
        
        // Test Kruskal's MST
        Edge[] edges = {
            new Edge(0, 1, 10), new Edge(0, 2, 6), new Edge(0, 3, 5),
            new Edge(1, 3, 15), new Edge(2, 3, 4)
        };
        List<Edge> mst = kruskalMST(4, edges);
        System.out.println("MST edges: " + mst);
        
        // Test Jump Game
        int[] nums1 = {2, 3, 1, 1, 4};
        System.out.println("Can jump: " + canJump(nums1));
        
        int[] nums2 = {2, 3, 1, 1, 4};
        System.out.println("Min jumps: " + minJumps(nums2));
        
        // Test Candy Distribution
        int[] ratings = {1, 0, 2};
        System.out.println("Min candies: " + candy(ratings));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Greedy Algorithms in Go**

```go
package main

import (
    "fmt"
    "sort"
)

// Activity represents a time interval activity
type Activity struct {
    Start, End int
}

/**
 * Activity Selection - Select maximum number of non-overlapping activities
 * Pattern: Sort by end time, greedily select earliest ending
 * Time: O(n log n), Space: O(1)
 */
func selectActivities(activities []Activity) []Activity {
    // Sort by end time (greedy choice: earliest ending first)
    sort.Slice(activities, func(i, j int) bool {
        return activities[i].End < activities[j].End
    })
    
    var selected []Activity
    selected = append(selected, activities[0]) // Always select first activity
    
    lastEndTime := activities[0].End
    
    for i := 1; i < len(activities); i++ {
        // Select activity if it starts after last selected activity ends
        if activities[i].Start >= lastEndTime {
            selected = append(selected, activities[i])
            lastEndTime = activities[i].End
        }
    }
    
    return selected
}

// Item represents an item with value and weight
type Item struct {
    Value, Weight int
    Ratio         float64
}

/**
 * Fractional Knapsack - Take items to maximize value within weight limit
 * Pattern: Sort by value-to-weight ratio, take greedily
 * Time: O(n log n), Space: O(1)
 */
func fractionalKnapsack(items []Item, capacity int) float64 {
    // Calculate ratios and sort by value-to-weight ratio
    for i := range items {
        items[i].Ratio = float64(items[i].Value) / float64(items[i].Weight)
    }
    
    sort.Slice(items, func(i, j int) bool {
        return items[i].Ratio > items[j].Ratio
    })
    
    totalValue := 0.0
    remainingCapacity := capacity
    
    for _, item := range items {
        if remainingCapacity >= item.Weight {
            // Take entire item
            totalValue += float64(item.Value)
            remainingCapacity -= item.Weight
        } else {
            // Take fraction of item
            fraction := float64(remainingCapacity) / float64(item.Weight)
            totalValue += float64(item.Value) * fraction
            break // Knapsack is full
        }
    }
    
    return totalValue
}

// Job represents a job with duration
type Job struct {
    Name     string
    Duration int
}

/**
 * Job Scheduling - Schedule jobs to minimize total completion time
 * Pattern: Sort by duration, process shortest first
 * Time: O(n log n), Space: O(1)
 */
func scheduleJobs(jobs []Job) []Job {
    // Sort by duration (greedy choice: shortest job first)
    sort.Slice(jobs, func(i, j int) bool {
        return jobs[i].Duration < jobs[j].Duration
    })
    
    return jobs
}

// Train represents a train with arrival and departure times
type Train struct {
    Arrival, Departure int
}

/**
 * Minimum Platforms - Find minimum platforms needed for train schedule
 * Pattern: Sort events, count overlapping intervals
 * Time: O(n log n), Space: O(n)
 */
func minPlatforms(trains []Train) int {
    n := len(trains)
    arrivals := make([]int, n)
    departures := make([]int, n)
    
    for i, train := range trains {
        arrivals[i] = train.Arrival
        departures[i] = train.Departure
    }
    
    // Sort arrival and departure times
    sort.Ints(arrivals)
    sort.Ints(departures)
    
    platforms := 0
    maxPlatforms := 0
    i, j := 0, 0
    
    for i < n && j < n {
        if arrivals[i] <= departures[j] {
            platforms++ // Train arrives, need platform
            if platforms > maxPlatforms {
                maxPlatforms = platforms
            }
            i++
        } else {
            platforms-- // Train departs, free platform
            j++
        }
    }
    
    return maxPlatforms
}

/**
 * Gas Station - Find starting station to complete circular route
 * Pattern: Track running balance, greedy starting point
 * Time: O(n), Space: O(1)
 */
func canCompleteCircuit(gas, cost []int) int {
    totalGas, totalCost := 0, 0
    currentGas, startStation := 0, 0
    
    for i := 0; i < len(gas); i++ {
        totalGas += gas[i]
        totalCost += cost[i]
        currentGas += gas[i] - cost[i]
        
        // If we can't reach next station, start from next station
        if currentGas < 0 {
            startStation = i + 1
            currentGas = 0
        }
    }
    
    // Check if total gas is sufficient
    if totalGas >= totalCost {
        return startStation
    }
    return -1
}

func main() {
    // Test Activity Selection
    activities := []Activity{
        {1, 4}, {3, 5}, {0, 6}, {5, 7}, {3, 9}, {5, 9},
        {6, 10}, {8, 11}, {8, 12}, {2, 14}, {12, 16},
    }
    
    selected := selectActivities(activities)
    fmt.Printf("Selected activities: %v\n", selected)
    
    // Test Fractional Knapsack
    items := []Item{
        {60, 10, 0}, {100, 20, 0}, {120, 30, 0},
    }
    maxValue := fractionalKnapsack(items, 50)
    fmt.Printf("Maximum value: %.2f\n", maxValue)
    
    // Test Job Scheduling
    jobs := []Job{
        {"A", 3}, {"B", 1}, {"C", 4}, {"D", 2},
    }
    schedule := scheduleJobs(jobs)
    fmt.Printf("Job schedule: %v\n", schedule)
    
    // Test Minimum Platforms
    trains := []Train{
        {900, 910}, {940, 1200}, {950, 1120},
        {1100, 1130}, {1500, 1900}, {1800, 2000},
    }
    platforms := minPlatforms(trains)
    fmt.Printf("Minimum platforms needed: %d\n", platforms)
    
    // Test Gas Station
    gas := []int{1, 2, 3, 4, 5}
    cost := []int{3, 4, 5, 1, 2}
    startStation := canCompleteCircuit(gas, cost)
    fmt.Printf("Starting gas station: %d\n", startStation)
}
```

### **Advanced Greedy Algorithms in Go**

```go
package main

import (
    "container/heap"
    "fmt"
    "sort"
)

// HuffmanNode represents a node in Huffman tree
type HuffmanNode struct {
    Character rune
    Frequency int
    Left, Right *HuffmanNode
}

// PriorityQueue implements heap.Interface for HuffmanNode
type PriorityQueue []*HuffmanNode

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].Frequency < pq[j].Frequency
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
}

func (pq *PriorityQueue) Push(x interface{}) {
    *pq = append(*pq, x.(*HuffmanNode))
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    *pq = old[0 : n-1]
    return item
}

/**
 * Huffman Coding - Build optimal compression tree
 * Pattern: Build tree bottom-up, always merge two smallest
 * Time: O(n log n), Space: O(n)
 */
func buildHuffmanTree(frequencies map[rune]int) *HuffmanNode {
    // Create priority queue (min heap) of nodes
    pq := &PriorityQueue{}
    heap.Init(pq)
    
    // Add all characters as leaf nodes
    for char, freq := range frequencies {
        node := &HuffmanNode{Character: char, Frequency: freq}
        heap.Push(pq, node)
    }
    
    // Build tree bottom-up
    for pq.Len() > 1 {
        left := heap.Pop(pq).(*HuffmanNode)
        right := heap.Pop(pq).(*HuffmanNode)
        
        merged := &HuffmanNode{
            Frequency: left.Frequency + right.Frequency,
            Left:      left,
            Right:     right,
        }
        
        heap.Push(pq, merged)
    }
    
    return heap.Pop(pq).(*HuffmanNode)
}

func getHuffmanCodes(root *HuffmanNode) map[rune]string {
    codes := make(map[rune]string)
    if root != nil {
        generateCodes(root, "", codes)
    }
    return codes
}

func generateCodes(node *HuffmanNode, code string, codes map[rune]string) {
    if node.Left == nil && node.Right == nil {
        // Leaf node
        if code == "" {
            code = "0" // Special case for single character
        }
        codes[node.Character] = code
    } else {
        if node.Left != nil {
            generateCodes(node.Left, code+"0", codes)
        }
        if node.Right != nil {
            generateCodes(node.Right, code+"1", codes)
        }
    }
}

// Edge represents a weighted edge in a graph
type Edge struct {
    Src, Dest, Weight int
}

// UnionFind data structure for cycle detection
type UnionFind struct {
    Parent, Rank []int
}

func NewUnionFind(n int) *UnionFind {
    parent := make([]int, n)
    rank := make([]int, n)
    for i := range parent {
        parent[i] = i
    }
    return &UnionFind{Parent: parent, Rank: rank}
}

func (uf *UnionFind) Find(x int) int {
    if uf.Parent[x] != x {
        uf.Parent[x] = uf.Find(uf.Parent[x]) // Path compression
    }
    return uf.Parent[x]
}

func (uf *UnionFind) Union(x, y int) bool {
    rootX := uf.Find(x)
    rootY := uf.Find(y)
    
    if rootX == rootY {
        return false // Already connected (would create cycle)
    }
    
    // Union by rank
    if uf.Rank[rootX] < uf.Rank[rootY] {
        uf.Parent[rootX] = rootY
    } else if uf.Rank[rootX] > uf.Rank[rootY] {
        uf.Parent[rootY] = rootX
    } else {
        uf.Parent[rootY] = rootX
        uf.Rank[rootX]++
    }
    
    return true
}

/**
 * Minimum Spanning Tree - Kruskal's Algorithm
 * Pattern: Sort edges, greedily add if no cycle
 * Time: O(E log E), Space: O(V)
 */
func kruskalMST(vertices int, edges []Edge) []Edge {
    // Sort edges by weight (greedy choice: smallest weight first)
    sort.Slice(edges, func(i, j int) bool {
        return edges[i].Weight < edges[j].Weight
    })
    
    var mst []Edge
    uf := NewUnionFind(vertices)
    
    for _, edge := range edges {
        // Add edge if it doesn't create cycle
        if uf.Union(edge.Src, edge.Dest) {
            mst = append(mst, edge)
            
            // Stop when we have V-1 edges
            if len(mst) == vertices-1 {
                break
            }
        }
    }
    
    return mst
}

/**
 * Jump Game - Check if can reach end of array
 * Pattern: Greedy farthest reachable position
 * Time: O(n), Space: O(1)
 */
func canJump(nums []int) bool {
    farthest := 0
    
    for i := 0; i < len(nums); i++ {
        // If current position is beyond farthest reachable, can't proceed
        if i > farthest {
            return false
        }
        
        // Update farthest reachable position
        if i+nums[i] > farthest {
            farthest = i + nums[i]
        }
        
        // If we can reach or exceed the last index
        if farthest >= len(nums)-1 {
            return true
        }
    }
    
    return false
}

/**
 * Jump Game II - Minimum jumps to reach end
 * Pattern: Greedy range expansion
 * Time: O(n), Space: O(1)
 */
func minJumps(nums []int) int {
    if len(nums) <= 1 {
        return 0
    }
    
    jumps := 0
    currentEnd := 0
    farthest := 0
    
    for i := 0; i < len(nums)-1; i++ {
        // Update farthest reachable position
        if i+nums[i] > farthest {
            farthest = i + nums[i]
        }
        
        // If we've reached the end of current range
        if i == currentEnd {
            jumps++
            currentEnd = farthest
            
            // If we can reach the end
            if currentEnd >= len(nums)-1 {
                break
            }
        }
    }
    
    return jumps
}

/**
 * Candy Distribution - Minimum candies satisfying neighbor constraints
 * Pattern: Two-pass greedy (left-to-right, right-to-left)
 * Time: O(n), Space: O(n)
 */
func candy(ratings []int) int {
    n := len(ratings)
    candies := make([]int, n)
    
    // Everyone gets at least 1 candy
    for i := range candies {
        candies[i] = 1
    }
    
    // Left to right: ensure right neighbor gets more if higher rating
    for i := 1; i < n; i++ {
        if ratings[i] > ratings[i-1] {
            candies[i] = candies[i-1] + 1
        }
    }
    
    // Right to left: ensure left neighbor gets more if higher rating
    for i := n - 2; i >= 0; i-- {
        if ratings[i] > ratings[i+1] {
            if candies[i+1]+1 > candies[i] {
                candies[i] = candies[i+1] + 1
            }
        }
    }
    
    total := 0
    for _, c := range candies {
        total += c
    }
    return total
}

func main() {
    // Test Huffman Coding
    frequencies := map[rune]int{
        'a': 5, 'b': 9, 'c': 12, 'd': 13, 'e': 16, 'f': 45,
    }
    
    root := buildHuffmanTree(frequencies)
    codes := getHuffmanCodes(root)
    fmt.Printf("Huffman codes: %v\n", codes)
    
    // Test Kruskal's MST
    edges := []Edge{
        {0, 1, 10}, {0, 2, 6}, {0, 3, 5}, {1, 3, 15}, {2, 3, 4},
    }
    mst := kruskalMST(4, edges)
    fmt.Printf("MST edges: %v\n", mst)
    
    // Test Jump Game
    nums1 := []int{2, 3, 1, 1, 4}
    fmt.Printf("Can jump: %t\n", canJump(nums1))
    
    nums2 := []int{2, 3, 1, 1, 4}
    fmt.Printf("Min jumps: %d\n", minJumps(nums2))
    
    // Test Candy Distribution
    ratings := []int{1, 0, 2}
    fmt.Printf("Min candies: %d\n", candy(ratings))
}
```

---

## 🧠 **UNDERSTANDING GREEDY PATTERNS**

### **Pattern 1: Interval Scheduling**
```java
// Sort by end time, greedily select non-overlapping
sort(intervals, (a, b) -> a.end - b.end);

int lastEnd = Integer.MIN_VALUE;
for (Interval interval : intervals) {
    if (interval.start >= lastEnd) {
        select(interval);
        lastEnd = interval.end;
    }
}
```
**Examples:** Activity Selection, Meeting Rooms

### **Pattern 2: Optimization by Sorting**
```java
// Sort by key metric, process in greedy order
sort(items, (a, b) -> metric(b) - metric(a)); // Descending order

for (Item item : items) {
    if (canTake(item)) {
        take(item);
    }
}
```
**Examples:** Fractional Knapsack, Job Scheduling

### **Pattern 3: Two-Pass Greedy**
```java
// First pass: left to right
for (int i = 1; i < n; i++) {
    if (condition(i, i-1)) {
        update(i, i-1);
    }
}

// Second pass: right to left
for (int i = n-2; i >= 0; i--) {
    if (condition(i, i+1)) {
        update(i, i+1);
    }
}
```
**Examples:** Candy Distribution, Trapping Rain Water

### **Pattern 4: Greedy with State Tracking**
```java
int state = initialState;
for (int i = 0; i < n; i++) {
    // Make greedy choice based on current state
    if (shouldUpdate(state, data[i])) {
        state = updateState(state, data[i]);
    }
}
```
**Examples:** Gas Station, Jump Game

---

## 📊 **GREEDY VS OTHER APPROACHES**

| Problem | Greedy Solution | Optimal Alternative | When Greedy Works |
|---------|----------------|-------------------|------------------|
| **Activity Selection** | Sort by end time | DP on intervals | Always optimal |
| **Fractional Knapsack** | Sort by ratio | N/A | Always optimal |
| **0/1 Knapsack** | ❌ Wrong | Dynamic Programming | Never optimal |
| **Coin Change** | Largest first | DP | Only for some coin systems |
| **Huffman Coding** | Min frequency merge | N/A | Always optimal |
| **MST** | Kruskal/Prim | N/A | Always optimal |
| **Shortest Path** | Dijkstra | Bellman-Ford/Floyd | Non-negative weights |

### **Proving Greedy Correctness:**

**1. Exchange Argument:**
- Assume optimal solution differs from greedy
- Show you can "exchange" choices to make it match greedy
- Prove this doesn't worsen the solution

**2. Greedy Choice Property:**
- Show that greedy choice is always safe
- Prove optimal solution contains greedy choice

**3. Optimal Substructure:**
- After greedy choice, remaining problem has optimal substructure
- Combine with induction

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Assign Cookies** - Satisfy children with minimum cookies
2. **Lemonade Change** - Check if can provide correct change
3. **Largest Number** - Arrange numbers for largest result
4. **Queue Reconstruction** - Arrange people by height and position
5. **Minimum Arrows** - Burst balloons with minimum arrows

### **Medium Problems:**

1. **Gas Station** ✅ (Implemented above)
2. **Jump Game** ✅ (Implemented above)
3. **Candy Distribution** ✅ (Implemented above)
4. **Task Scheduler** - Schedule tasks with cooldown
5. **Non-overlapping Intervals** - Remove minimum intervals

### **Hard Problems:**

1. **Merge Intervals** - Merge overlapping intervals
2. **Meeting Rooms II** - Minimum meeting rooms needed
3. **Remove K Digits** - Remove digits for smallest number
4. **Smallest String Starting From Leaf** - Tree path optimization
5. **Video Stitching** - Minimum clips to cover time range

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Greedy Thinking**
- [ ] Understand greedy choice property and optimal substructure
- [ ] Practice activity selection problem step by step
- [ ] Learn to identify when greedy works vs fails
- [ ] Implement fractional knapsack and compare with 0/1

### **Day 2: Interval Problems**
- [ ] Solve multiple interval scheduling problems
- [ ] Practice meeting rooms and platform problems
- [ ] Master the "sort by end time" pattern
- [ ] Understand overlap detection techniques

### **Day 3: Advanced Greedy**
- [ ] Implement Huffman coding algorithm
- [ ] Practice minimum spanning tree (Kruskal's)
- [ ] Solve jump game variations
- [ ] Learn two-pass greedy techniques

### **Day 4: Problem Recognition**
- [ ] Solve 5+ greedy problems on LeetCode
- [ ] Identify greedy vs DP problems
- [ ] Practice proving greedy correctness
- [ ] Build pattern recognition skills

### **✅ You're Ready for Next Topic When:**
- [ ] You can identify greedy opportunities in problems
- [ ] You understand when greedy works vs fails
- [ ] You can implement classic greedy algorithms
- [ ] You can prove greedy correctness

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Advanced DP - Detailed](./14-dp-advanced-detailed-java-go.md)** - Complex optimization patterns
2. **[Graph Algorithms - Detailed](./15-graphs-detailed-java-go.md)** - Master graph traversal and algorithms
3. **[String Algorithms - Detailed](./16-strings-advanced-detailed-java-go.md)** - Advanced string processing

### **🎯 Advanced Greedy Topics:**
- **Matroids** and greedy algorithm theory
- **Approximation Algorithms** using greedy approaches
- **Online Algorithms** for streaming data
- **Competitive Programming** greedy techniques

### **💡 Pro Tips:**
- **Think locally** - What's the best immediate choice?
- **Prove correctness** - Use exchange argument or contradiction
- **Watch for counterexamples** - Test your greedy strategy
- **Compare with DP** - Understand when each approach works
- **Practice pattern recognition** - Many problems have similar structures

**🎉 You now master greedy algorithms! This powerful paradigm will help you solve optimization problems efficiently when local choices lead to global optimality.**