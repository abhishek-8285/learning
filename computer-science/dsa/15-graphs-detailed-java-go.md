# 🌐 **GRAPH ALGORITHMS: DETAILED GUIDE (JAVA & GO)**

**Master Networks, Relationships, and Complex Data Structures**

*Prerequisites: Complete trees, recursion, queues, and basic algorithms first*

---

## 🤔 **WHAT ARE GRAPHS?**

### **Simple Explanation:**
A graph is like a **social network map**:
- **People** = Nodes/Vertices (the entities)
- **Friendships** = Edges (the relationships)
- **Network** = Graph (the complete structure)
- **Connections** can be one-way (directed) or two-way (undirected)

### **Real-World Examples:**

**🌐 Social Networks:**
- **Facebook:** People connected by friendships (undirected)
- **Twitter:** People connected by follows (directed)
- **LinkedIn:** Professional connections (undirected)

**🗺️ Maps & Navigation:**
- **Cities** = Nodes, **Roads** = Edges
- **GPS Navigation:** Find shortest path between locations
- **Traffic:** Edge weights represent travel time

**💻 Computer Networks:**
- **Computers** = Nodes, **Connections** = Edges
- **Internet routing:** Find path for data packets
- **Network topology:** Design efficient connections

**🧬 Biology & Chemistry:**
- **Atoms** = Nodes, **Bonds** = Edges
- **Neural networks:** Neurons and synapses
- **Food chains:** Species and relationships

---

## 🎯 **GRAPH TERMINOLOGY**

### **Basic Components:**
- **Vertex/Node** - Individual entity in the graph
- **Edge** - Connection between two vertices
- **Degree** - Number of edges connected to a vertex
- **Path** - Sequence of vertices connected by edges
- **Cycle** - Path that starts and ends at same vertex

### **Graph Types:**

**📍 By Direction:**
- **Undirected:** Edges work both ways (friendship)
- **Directed:** Edges have direction (follows, roads)

**🏷️ By Weights:**
- **Unweighted:** All edges equal (simple connections)
- **Weighted:** Edges have values (distance, cost, time)

**🔗 By Connectivity:**
- **Connected:** Path exists between any two vertices
- **Disconnected:** Some vertices unreachable from others

**🌀 Special Properties:**
- **Acyclic:** No cycles (trees are acyclic graphs)
- **Complete:** Every vertex connected to every other vertex
- **Bipartite:** Vertices can be divided into two independent sets

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Graph Representations and Basic Operations**

```java
import java.util.*;

/**
 * Graph representation using Adjacency List
 * Most common and efficient for sparse graphs
 */
public class Graph {
    private int vertices;
    private List<List<Integer>> adjList;
    private boolean isDirected;
    
    public Graph(int vertices, boolean isDirected) {
        this.vertices = vertices;
        this.isDirected = isDirected;
        this.adjList = new ArrayList<>();
        
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }
    }
    
    /**
     * Add edge between two vertices
     * Time: O(1), Space: O(1)
     */
    public void addEdge(int src, int dest) {
        adjList.get(src).add(dest);
        
        // For undirected graph, add edge in both directions
        if (!isDirected) {
            adjList.get(dest).add(src);
        }
    }
    
    /**
     * Remove edge between two vertices
     * Time: O(degree), Space: O(1)
     */
    public void removeEdge(int src, int dest) {
        adjList.get(src).remove(Integer.valueOf(dest));
        
        if (!isDirected) {
            adjList.get(dest).remove(Integer.valueOf(src));
        }
    }
    
    /**
     * Check if edge exists
     * Time: O(degree), Space: O(1)
     */
    public boolean hasEdge(int src, int dest) {
        return adjList.get(src).contains(dest);
    }
    
    /**
     * Get all neighbors of a vertex
     * Time: O(1), Space: O(1)
     */
    public List<Integer> getNeighbors(int vertex) {
        return adjList.get(vertex);
    }
    
    /**
     * Get degree of a vertex
     * Time: O(1), Space: O(1)
     */
    public int getDegree(int vertex) {
        return adjList.get(vertex).size();
    }
    
    /**
     * Display the graph
     */
    public void display() {
        for (int i = 0; i < vertices; i++) {
            System.out.print("Vertex " + i + ": ");
            for (int neighbor : adjList.get(i)) {
                System.out.print(neighbor + " ");
            }
            System.out.println();
        }
    }
    
    public int getVertices() { return vertices; }
    public List<List<Integer>> getAdjList() { return adjList; }
}

/**
 * Weighted Graph representation
 */
public class WeightedGraph {
    public static class Edge {
        int dest, weight;
        
        Edge(int dest, int weight) {
            this.dest = dest;
            this.weight = weight;
        }
        
        @Override
        public String toString() {
            return "(" + dest + ", " + weight + ")";
        }
    }
    
    private int vertices;
    private List<List<Edge>> adjList;
    private boolean isDirected;
    
    public WeightedGraph(int vertices, boolean isDirected) {
        this.vertices = vertices;
        this.isDirected = isDirected;
        this.adjList = new ArrayList<>();
        
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }
    }
    
    public void addEdge(int src, int dest, int weight) {
        adjList.get(src).add(new Edge(dest, weight));
        
        if (!isDirected) {
            adjList.get(dest).add(new Edge(src, weight));
        }
    }
    
    public List<Edge> getNeighbors(int vertex) {
        return adjList.get(vertex);
    }
    
    public int getVertices() { return vertices; }
}
```

### **Graph Traversal Algorithms**

```java
import java.util.*;

public class GraphTraversal {
    
    /**
     * Depth-First Search (DFS) - Recursive
     * Pattern: Go deep before exploring siblings
     * Time: O(V + E), Space: O(V)
     */
    public static void dfsRecursive(Graph graph, int startVertex) {
        boolean[] visited = new boolean[graph.getVertices()];
        System.out.print("DFS (Recursive): ");
        dfsRecursiveHelper(graph, startVertex, visited);
        System.out.println();
    }
    
    private static void dfsRecursiveHelper(Graph graph, int vertex, boolean[] visited) {
        visited[vertex] = true;
        System.out.print(vertex + " ");
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfsRecursiveHelper(graph, neighbor, visited);
            }
        }
    }
    
    /**
     * Depth-First Search (DFS) - Iterative using Stack
     * Pattern: Use stack to simulate recursion
     * Time: O(V + E), Space: O(V)
     */
    public static void dfsIterative(Graph graph, int startVertex) {
        boolean[] visited = new boolean[graph.getVertices()];
        Stack<Integer> stack = new Stack<>();
        
        stack.push(startVertex);
        System.out.print("DFS (Iterative): ");
        
        while (!stack.isEmpty()) {
            int vertex = stack.pop();
            
            if (!visited[vertex]) {
                visited[vertex] = true;
                System.out.print(vertex + " ");
                
                // Add neighbors to stack (reverse order for consistent traversal)
                List<Integer> neighbors = graph.getNeighbors(vertex);
                for (int i = neighbors.size() - 1; i >= 0; i--) {
                    if (!visited[neighbors.get(i)]) {
                        stack.push(neighbors.get(i));
                    }
                }
            }
        }
        System.out.println();
    }
    
    /**
     * Breadth-First Search (BFS) - Level by level traversal
     * Pattern: Use queue to process level by level
     * Time: O(V + E), Space: O(V)
     */
    public static void bfs(Graph graph, int startVertex) {
        boolean[] visited = new boolean[graph.getVertices()];
        Queue<Integer> queue = new LinkedList<>();
        
        visited[startVertex] = true;
        queue.offer(startVertex);
        System.out.print("BFS: ");
        
        while (!queue.isEmpty()) {
            int vertex = queue.poll();
            System.out.print(vertex + " ");
            
            for (int neighbor : graph.getNeighbors(vertex)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
        System.out.println();
    }
    
    /**
     * Find all connected components
     * Pattern: DFS from each unvisited vertex
     * Time: O(V + E), Space: O(V)
     */
    public static List<List<Integer>> findConnectedComponents(Graph graph) {
        boolean[] visited = new boolean[graph.getVertices()];
        List<List<Integer>> components = new ArrayList<>();
        
        for (int i = 0; i < graph.getVertices(); i++) {
            if (!visited[i]) {
                List<Integer> component = new ArrayList<>();
                dfsForComponent(graph, i, visited, component);
                components.add(component);
            }
        }
        
        return components;
    }
    
    private static void dfsForComponent(Graph graph, int vertex, 
                                     boolean[] visited, List<Integer> component) {
        visited[vertex] = true;
        component.add(vertex);
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfsForComponent(graph, neighbor, visited, component);
            }
        }
    }
    
    /**
     * Check if graph has a cycle (undirected)
     * Pattern: DFS with parent tracking
     * Time: O(V + E), Space: O(V)
     */
    public static boolean hasCycleUndirected(Graph graph) {
        boolean[] visited = new boolean[graph.getVertices()];
        
        for (int i = 0; i < graph.getVertices(); i++) {
            if (!visited[i]) {
                if (dfsHasCycle(graph, i, visited, -1)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
    private static boolean dfsHasCycle(Graph graph, int vertex, 
                                     boolean[] visited, int parent) {
        visited[vertex] = true;
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                if (dfsHasCycle(graph, neighbor, visited, vertex)) {
                    return true;
                }
            } else if (neighbor != parent) {
                return true; // Back edge found (cycle)
            }
        }
        
        return false;
    }
    
    /**
     * Check if directed graph has a cycle
     * Pattern: DFS with recursion stack tracking
     * Time: O(V + E), Space: O(V)
     */
    public static boolean hasCycleDirected(Graph graph) {
        boolean[] visited = new boolean[graph.getVertices()];
        boolean[] recStack = new boolean[graph.getVertices()];
        
        for (int i = 0; i < graph.getVertices(); i++) {
            if (dfsHasCycleDirected(graph, i, visited, recStack)) {
                return true;
            }
        }
        
        return false;
    }
    
    private static boolean dfsHasCycleDirected(Graph graph, int vertex,
                                             boolean[] visited, boolean[] recStack) {
        if (recStack[vertex]) {
            return true; // Back edge in recursion stack
        }
        
        if (visited[vertex]) {
            return false; // Already processed
        }
        
        visited[vertex] = true;
        recStack[vertex] = true;
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (dfsHasCycleDirected(graph, neighbor, visited, recStack)) {
                return true;
            }
        }
        
        recStack[vertex] = false; // Remove from recursion stack
        return false;
    }
    
    public static void main(String[] args) {
        // Create sample graph
        Graph graph = new Graph(6, false);
        graph.addEdge(0, 1);
        graph.addEdge(0, 2);
        graph.addEdge(1, 3);
        graph.addEdge(2, 4);
        graph.addEdge(3, 5);
        graph.addEdge(4, 5);
        
        System.out.println("Graph representation:");
        graph.display();
        System.out.println();
        
        // Test traversals
        dfsRecursive(graph, 0);
        dfsIterative(graph, 0);
        bfs(graph, 0);
        
        // Test connected components
        List<List<Integer>> components = findConnectedComponents(graph);
        System.out.println("Connected components: " + components);
        
        // Test cycle detection
        System.out.println("Has cycle: " + hasCycleUndirected(graph));
    }
}
```

### **Shortest Path Algorithms**

```java
import java.util.*;

public class ShortestPathAlgorithms {
    
    /**
     * Dijkstra's Algorithm - Single source shortest path (non-negative weights)
     * Pattern: Greedy approach using priority queue
     * Time: O((V + E) log V), Space: O(V)
     */
    public static class DijkstraResult {
        int[] distances;
        int[] previous;
        
        DijkstraResult(int[] distances, int[] previous) {
            this.distances = distances;
            this.previous = previous;
        }
    }
    
    public static DijkstraResult dijkstra(WeightedGraph graph, int source) {
        int vertices = graph.getVertices();
        int[] distances = new int[vertices];
        int[] previous = new int[vertices];
        boolean[] visited = new boolean[vertices];
        
        Arrays.fill(distances, Integer.MAX_VALUE);
        Arrays.fill(previous, -1);
        distances[source] = 0;
        
        // Priority queue to store (distance, vertex) pairs
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[]{0, source});
        
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            int currentDist = current[0];
            int currentVertex = current[1];
            
            if (visited[currentVertex]) continue;
            visited[currentVertex] = true;
            
            for (WeightedGraph.Edge edge : graph.getNeighbors(currentVertex)) {
                int neighbor = edge.dest;
                int weight = edge.weight;
                
                if (!visited[neighbor]) {
                    int newDistance = distances[currentVertex] + weight;
                    
                    if (newDistance < distances[neighbor]) {
                        distances[neighbor] = newDistance;
                        previous[neighbor] = currentVertex;
                        pq.offer(new int[]{newDistance, neighbor});
                    }
                }
            }
        }
        
        return new DijkstraResult(distances, previous);
    }
    
    /**
     * Bellman-Ford Algorithm - Single source shortest path (handles negative weights)
     * Pattern: Dynamic programming with edge relaxation
     * Time: O(VE), Space: O(V)
     */
    public static class BellmanFordResult {
        int[] distances;
        boolean hasNegativeCycle;
        
        BellmanFordResult(int[] distances, boolean hasNegativeCycle) {
            this.distances = distances;
            this.hasNegativeCycle = hasNegativeCycle;
        }
    }
    
    public static BellmanFordResult bellmanFord(WeightedGraph graph, int source) {
        int vertices = graph.getVertices();
        int[] distances = new int[vertices];
        
        Arrays.fill(distances, Integer.MAX_VALUE);
        distances[source] = 0;
        
        // Relax all edges V-1 times
        for (int i = 0; i < vertices - 1; i++) {
            for (int u = 0; u < vertices; u++) {
                if (distances[u] != Integer.MAX_VALUE) {
                    for (WeightedGraph.Edge edge : graph.getNeighbors(u)) {
                        int v = edge.dest;
                        int weight = edge.weight;
                        
                        if (distances[u] + weight < distances[v]) {
                            distances[v] = distances[u] + weight;
                        }
                    }
                }
            }
        }
        
        // Check for negative cycles
        boolean hasNegativeCycle = false;
        for (int u = 0; u < vertices; u++) {
            if (distances[u] != Integer.MAX_VALUE) {
                for (WeightedGraph.Edge edge : graph.getNeighbors(u)) {
                    int v = edge.dest;
                    int weight = edge.weight;
                    
                    if (distances[u] + weight < distances[v]) {
                        hasNegativeCycle = true;
                        break;
                    }
                }
            }
            if (hasNegativeCycle) break;
        }
        
        return new BellmanFordResult(distances, hasNegativeCycle);
    }
    
    /**
     * Floyd-Warshall Algorithm - All pairs shortest path
     * Pattern: Dynamic programming with intermediate vertices
     * Time: O(V³), Space: O(V²)
     */
    public static int[][] floydWarshall(int[][] graph) {
        int vertices = graph.length;
        int[][] distances = new int[vertices][vertices];
        
        // Initialize distances matrix
        for (int i = 0; i < vertices; i++) {
            for (int j = 0; j < vertices; j++) {
                distances[i][j] = graph[i][j];
            }
        }
        
        // Try each vertex as intermediate point
        for (int k = 0; k < vertices; k++) {
            for (int i = 0; i < vertices; i++) {
                for (int j = 0; j < vertices; j++) {
                    if (distances[i][k] != Integer.MAX_VALUE && 
                        distances[k][j] != Integer.MAX_VALUE &&
                        distances[i][k] + distances[k][j] < distances[i][j]) {
                        distances[i][j] = distances[i][k] + distances[k][j];
                    }
                }
            }
        }
        
        return distances;
    }
    
    /**
     * Reconstruct shortest path from Dijkstra result
     */
    public static List<Integer> getPath(DijkstraResult result, int source, int target) {
        List<Integer> path = new ArrayList<>();
        
        if (result.distances[target] == Integer.MAX_VALUE) {
            return path; // No path exists
        }
        
        int current = target;
        while (current != -1) {
            path.add(0, current);
            current = result.previous[current];
        }
        
        return path;
    }
    
    public static void main(String[] args) {
        // Create weighted graph
        WeightedGraph graph = new WeightedGraph(5, true);
        graph.addEdge(0, 1, 10);
        graph.addEdge(0, 4, 5);
        graph.addEdge(1, 2, 1);
        graph.addEdge(1, 4, 2);
        graph.addEdge(2, 3, 4);
        graph.addEdge(3, 2, 6);
        graph.addEdge(4, 1, 3);
        graph.addEdge(4, 2, 9);
        graph.addEdge(4, 3, 2);
        
        // Test Dijkstra
        DijkstraResult dijkstraResult = dijkstra(graph, 0);
        System.out.println("Dijkstra distances from vertex 0: " + 
                          Arrays.toString(dijkstraResult.distances));
        
        List<Integer> path = getPath(dijkstraResult, 0, 3);
        System.out.println("Shortest path from 0 to 3: " + path);
        
        // Test Bellman-Ford
        BellmanFordResult bellmanResult = bellmanFord(graph, 0);
        System.out.println("Bellman-Ford distances: " + 
                          Arrays.toString(bellmanResult.distances));
        System.out.println("Has negative cycle: " + bellmanResult.hasNegativeCycle);
    }
}
```

### **Advanced Graph Algorithms**

```java
import java.util.*;

public class AdvancedGraphAlgorithms {
    
    /**
     * Topological Sort - Linear ordering of vertices in DAG
     * Pattern: DFS-based approach
     * Time: O(V + E), Space: O(V)
     */
    public static List<Integer> topologicalSort(Graph graph) {
        boolean[] visited = new boolean[graph.getVertices()];
        Stack<Integer> stack = new Stack<>();
        
        for (int i = 0; i < graph.getVertices(); i++) {
            if (!visited[i]) {
                topologicalSortDFS(graph, i, visited, stack);
            }
        }
        
        List<Integer> result = new ArrayList<>();
        while (!stack.isEmpty()) {
            result.add(stack.pop());
        }
        
        return result;
    }
    
    private static void topologicalSortDFS(Graph graph, int vertex,
                                         boolean[] visited, Stack<Integer> stack) {
        visited[vertex] = true;
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                topologicalSortDFS(graph, neighbor, visited, stack);
            }
        }
        
        stack.push(vertex);
    }
    
    /**
     * Kahn's Algorithm for Topological Sort
     * Pattern: BFS with in-degree tracking
     * Time: O(V + E), Space: O(V)
     */
    public static List<Integer> topologicalSortKahn(Graph graph) {
        int vertices = graph.getVertices();
        int[] inDegree = new int[vertices];
        
        // Calculate in-degree for each vertex
        for (int i = 0; i < vertices; i++) {
            for (int neighbor : graph.getNeighbors(i)) {
                inDegree[neighbor]++;
            }
        }
        
        Queue<Integer> queue = new LinkedList<>();
        List<Integer> result = new ArrayList<>();
        
        // Add all vertices with 0 in-degree
        for (int i = 0; i < vertices; i++) {
            if (inDegree[i] == 0) {
                queue.offer(i);
            }
        }
        
        while (!queue.isEmpty()) {
            int vertex = queue.poll();
            result.add(vertex);
            
            // Reduce in-degree of neighbors
            for (int neighbor : graph.getNeighbors(vertex)) {
                inDegree[neighbor]--;
                if (inDegree[neighbor] == 0) {
                    queue.offer(neighbor);
                }
            }
        }
        
        // Check if all vertices are included (no cycle)
        return result.size() == vertices ? result : new ArrayList<>();
    }
    
    /**
     * Strongly Connected Components - Kosaraju's Algorithm
     * Pattern: Two-pass DFS with transpose graph
     * Time: O(V + E), Space: O(V)
     */
    public static List<List<Integer>> stronglyConnectedComponents(Graph graph) {
        int vertices = graph.getVertices();
        boolean[] visited = new boolean[vertices];
        Stack<Integer> finishOrder = new Stack<>();
        
        // First DFS to get finish order
        for (int i = 0; i < vertices; i++) {
            if (!visited[i]) {
                dfsFinishOrder(graph, i, visited, finishOrder);
            }
        }
        
        // Create transpose graph
        Graph transpose = createTranspose(graph);
        
        // Second DFS on transpose in reverse finish order
        Arrays.fill(visited, false);
        List<List<Integer>> sccs = new ArrayList<>();
        
        while (!finishOrder.isEmpty()) {
            int vertex = finishOrder.pop();
            if (!visited[vertex]) {
                List<Integer> scc = new ArrayList<>();
                dfsCollectSCC(transpose, vertex, visited, scc);
                sccs.add(scc);
            }
        }
        
        return sccs;
    }
    
    private static void dfsFinishOrder(Graph graph, int vertex,
                                     boolean[] visited, Stack<Integer> finishOrder) {
        visited[vertex] = true;
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfsFinishOrder(graph, neighbor, visited, finishOrder);
            }
        }
        
        finishOrder.push(vertex);
    }
    
    private static Graph createTranspose(Graph graph) {
        Graph transpose = new Graph(graph.getVertices(), true);
        
        for (int i = 0; i < graph.getVertices(); i++) {
            for (int neighbor : graph.getNeighbors(i)) {
                transpose.addEdge(neighbor, i);
            }
        }
        
        return transpose;
    }
    
    private static void dfsCollectSCC(Graph graph, int vertex,
                                    boolean[] visited, List<Integer> scc) {
        visited[vertex] = true;
        scc.add(vertex);
        
        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited[neighbor]) {
                dfsCollectSCC(graph, neighbor, visited, scc);
            }
        }
    }
    
    /**
     * Bridge Detection - Find critical edges
     * Pattern: DFS with low-link values
     * Time: O(V + E), Space: O(V)
     */
    public static List<int[]> findBridges(Graph graph) {
        int vertices = graph.getVertices();
        boolean[] visited = new boolean[vertices];
        int[] disc = new int[vertices];
        int[] low = new int[vertices];
        int[] parent = new int[vertices];
        List<int[]> bridges = new ArrayList<>();
        
        Arrays.fill(parent, -1);
        
        for (int i = 0; i < vertices; i++) {
            if (!visited[i]) {
                bridgeDFS(graph, i, visited, disc, low, parent, bridges, new int[]{0});
            }
        }
        
        return bridges;
    }
    
    private static void bridgeDFS(Graph graph, int u, boolean[] visited,
                                int[] disc, int[] low, int[] parent,
                                List<int[]> bridges, int[] time) {
        visited[u] = true;
        disc[u] = low[u] = time[0]++;
        
        for (int v : graph.getNeighbors(u)) {
            if (!visited[v]) {
                parent[v] = u;
                bridgeDFS(graph, v, visited, disc, low, parent, bridges, time);
                
                low[u] = Math.min(low[u], low[v]);
                
                // If low[v] > disc[u], then u-v is a bridge
                if (low[v] > disc[u]) {
                    bridges.add(new int[]{u, v});
                }
            } else if (v != parent[u]) {
                low[u] = Math.min(low[u], disc[v]);
            }
        }
    }
    
    public static void main(String[] args) {
        // Test Topological Sort
        Graph dag = new Graph(6, true);
        dag.addEdge(5, 2);
        dag.addEdge(5, 0);
        dag.addEdge(4, 0);
        dag.addEdge(4, 1);
        dag.addEdge(2, 3);
        dag.addEdge(3, 1);
        
        List<Integer> topoSort = topologicalSort(dag);
        System.out.println("Topological Sort (DFS): " + topoSort);
        
        List<Integer> topoSortKahn = topologicalSortKahn(dag);
        System.out.println("Topological Sort (Kahn): " + topoSortKahn);
        
        // Test Strongly Connected Components
        Graph directed = new Graph(5, true);
        directed.addEdge(1, 0);
        directed.addEdge(0, 2);
        directed.addEdge(2, 1);
        directed.addEdge(0, 3);
        directed.addEdge(3, 4);
        
        List<List<Integer>> sccs = stronglyConnectedComponents(directed);
        System.out.println("Strongly Connected Components: " + sccs);
        
        // Test Bridge Detection
        Graph undirected = new Graph(5, false);
        undirected.addEdge(1, 0);
        undirected.addEdge(0, 2);
        undirected.addEdge(2, 1);
        undirected.addEdge(0, 3);
        undirected.addEdge(3, 4);
        
        List<int[]> bridges = findBridges(undirected);
        System.out.print("Bridges: ");
        for (int[] bridge : bridges) {
            System.out.print("(" + bridge[0] + "," + bridge[1] + ") ");
        }
        System.out.println();
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Basic Graph Structure in Go**

```go
package main

import "fmt"

// Graph represents an adjacency list graph
type Graph struct {
    vertices  int
    adjList   [][]int
    directed  bool
}

// NewGraph creates a new graph
func NewGraph(vertices int, directed bool) *Graph {
    return &Graph{
        vertices: vertices,
        adjList:  make([][]int, vertices),
        directed: directed,
    }
}

// AddEdge adds an edge between two vertices
func (g *Graph) AddEdge(src, dest int) {
    g.adjList[src] = append(g.adjList[src], dest)
    
    if !g.directed {
        g.adjList[dest] = append(g.adjList[dest], src)
    }
}

// GetNeighbors returns neighbors of a vertex
func (g *Graph) GetNeighbors(vertex int) []int {
    return g.adjList[vertex]
}

// Display prints the graph
func (g *Graph) Display() {
    for i := 0; i < g.vertices; i++ {
        fmt.Printf("Vertex %d: ", i)
        for _, neighbor := range g.adjList[i] {
            fmt.Printf("%d ", neighbor)
        }
        fmt.Println()
    }
}

// DFS recursive implementation
func (g *Graph) DFS(startVertex int) {
    visited := make([]bool, g.vertices)
    fmt.Print("DFS: ")
    g.dfsHelper(startVertex, visited)
    fmt.Println()
}

func (g *Graph) dfsHelper(vertex int, visited []bool) {
    visited[vertex] = true
    fmt.Printf("%d ", vertex)
    
    for _, neighbor := range g.adjList[vertex] {
        if !visited[neighbor] {
            g.dfsHelper(neighbor, visited)
        }
    }
}

// BFS implementation
func (g *Graph) BFS(startVertex int) {
    visited := make([]bool, g.vertices)
    queue := []int{startVertex}
    visited[startVertex] = true
    
    fmt.Print("BFS: ")
    
    for len(queue) > 0 {
        vertex := queue[0]
        queue = queue[1:]
        fmt.Printf("%d ", vertex)
        
        for _, neighbor := range g.adjList[vertex] {
            if !visited[neighbor] {
                visited[neighbor] = true
                queue = append(queue, neighbor)
            }
        }
    }
    fmt.Println()
}

// HasCycle checks if undirected graph has cycle
func (g *Graph) HasCycle() bool {
    visited := make([]bool, g.vertices)
    
    for i := 0; i < g.vertices; i++ {
        if !visited[i] {
            if g.dfsHasCycle(i, visited, -1) {
                return true
            }
        }
    }
    
    return false
}

func (g *Graph) dfsHasCycle(vertex int, visited []bool, parent int) bool {
    visited[vertex] = true
    
    for _, neighbor := range g.adjList[vertex] {
        if !visited[neighbor] {
            if g.dfsHasCycle(neighbor, visited, vertex) {
                return true
            }
        } else if neighbor != parent {
            return true
        }
    }
    
    return false
}

func main() {
    // Create sample graph
    graph := NewGraph(5, false)
    graph.AddEdge(0, 1)
    graph.AddEdge(0, 4)
    graph.AddEdge(1, 2)
    graph.AddEdge(1, 3)
    graph.AddEdge(1, 4)
    graph.AddEdge(2, 3)
    graph.AddEdge(3, 4)
    
    fmt.Println("Graph representation:")
    graph.Display()
    fmt.Println()
    
    // Test traversals
    graph.DFS(0)
    graph.BFS(0)
    
    // Test cycle detection
    fmt.Printf("Has cycle: %t\n", graph.HasCycle())
}
```

### **Shortest Path Algorithms in Go**

```go
package main

import (
    "container/heap"
    "fmt"
    "math"
)

// Edge represents a weighted edge
type Edge struct {
    Dest   int
    Weight int
}

// WeightedGraph represents a weighted graph
type WeightedGraph struct {
    vertices int
    adjList  [][]Edge
    directed bool
}

// NewWeightedGraph creates a new weighted graph
func NewWeightedGraph(vertices int, directed bool) *WeightedGraph {
    return &WeightedGraph{
        vertices: vertices,
        adjList:  make([][]Edge, vertices),
        directed: directed,
    }
}

// AddEdge adds a weighted edge
func (g *WeightedGraph) AddEdge(src, dest, weight int) {
    g.adjList[src] = append(g.adjList[src], Edge{dest, weight})
    
    if !g.directed {
        g.adjList[dest] = append(g.adjList[dest], Edge{src, weight})
    }
}

// PriorityQueue for Dijkstra's algorithm
type PQItem struct {
    vertex   int
    distance int
    index    int
}

type PriorityQueue []*PQItem

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].distance < pq[j].distance
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}

func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    item := x.(*PQItem)
    item.index = n
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    item.index = -1
    *pq = old[0 : n-1]
    return item
}

// Dijkstra's algorithm implementation
func (g *WeightedGraph) Dijkstra(source int) ([]int, []int) {
    distances := make([]int, g.vertices)
    previous := make([]int, g.vertices)
    visited := make([]bool, g.vertices)
    
    // Initialize distances
    for i := range distances {
        distances[i] = math.MaxInt32
        previous[i] = -1
    }
    distances[source] = 0
    
    // Priority queue
    pq := &PriorityQueue{}
    heap.Init(pq)
    heap.Push(pq, &PQItem{vertex: source, distance: 0})
    
    for pq.Len() > 0 {
        current := heap.Pop(pq).(*PQItem)
        vertex := current.vertex
        
        if visited[vertex] {
            continue
        }
        visited[vertex] = true
        
        for _, edge := range g.adjList[vertex] {
            neighbor := edge.Dest
            weight := edge.Weight
            
            if !visited[neighbor] {
                newDistance := distances[vertex] + weight
                
                if newDistance < distances[neighbor] {
                    distances[neighbor] = newDistance
                    previous[neighbor] = vertex
                    heap.Push(pq, &PQItem{vertex: neighbor, distance: newDistance})
                }
            }
        }
    }
    
    return distances, previous
}

// BellmanFord algorithm implementation
func (g *WeightedGraph) BellmanFord(source int) ([]int, bool) {
    distances := make([]int, g.vertices)
    
    // Initialize distances
    for i := range distances {
        distances[i] = math.MaxInt32
    }
    distances[source] = 0
    
    // Relax edges V-1 times
    for i := 0; i < g.vertices-1; i++ {
        for u := 0; u < g.vertices; u++ {
            if distances[u] != math.MaxInt32 {
                for _, edge := range g.adjList[u] {
                    v := edge.Dest
                    weight := edge.Weight
                    
                    if distances[u]+weight < distances[v] {
                        distances[v] = distances[u] + weight
                    }
                }
            }
        }
    }
    
    // Check for negative cycles
    hasNegativeCycle := false
    for u := 0; u < g.vertices; u++ {
        if distances[u] != math.MaxInt32 {
            for _, edge := range g.adjList[u] {
                v := edge.Dest
                weight := edge.Weight
                
                if distances[u]+weight < distances[v] {
                    hasNegativeCycle = true
                    break
                }
            }
        }
        if hasNegativeCycle {
            break
        }
    }
    
    return distances, hasNegativeCycle
}

func main() {
    // Create weighted graph
    graph := NewWeightedGraph(5, true)
    graph.AddEdge(0, 1, 10)
    graph.AddEdge(0, 4, 5)
    graph.AddEdge(1, 2, 1)
    graph.AddEdge(1, 4, 2)
    graph.AddEdge(2, 3, 4)
    graph.AddEdge(3, 2, 6)
    graph.AddEdge(4, 1, 3)
    graph.AddEdge(4, 2, 9)
    graph.AddEdge(4, 3, 2)
    
    // Test Dijkstra
    distances, _ := graph.Dijkstra(0)
    fmt.Printf("Dijkstra distances from vertex 0: %v\n", distances)
    
    // Test Bellman-Ford
    bfDistances, hasNegCycle := graph.BellmanFord(0)
    fmt.Printf("Bellman-Ford distances: %v\n", bfDistances)
    fmt.Printf("Has negative cycle: %t\n", hasNegCycle)
}
```

---

## 📊 **GRAPH ALGORITHM COMPLEXITY**

| Algorithm | Time Complexity | Space Complexity | Use Case |
|-----------|----------------|------------------|----------|
| **DFS/BFS** | O(V + E) | O(V) | Traversal, connectivity |
| **Dijkstra** | O((V + E) log V) | O(V) | Shortest path (non-negative) |
| **Bellman-Ford** | O(VE) | O(V) | Shortest path (negative edges) |
| **Floyd-Warshall** | O(V³) | O(V²) | All pairs shortest path |
| **Topological Sort** | O(V + E) | O(V) | DAG ordering |
| **Kosaraju** | O(V + E) | O(V) | Strongly connected components |
| **Kruskal MST** | O(E log E) | O(V) | Minimum spanning tree |
| **Prim MST** | O((V + E) log V) | O(V) | Minimum spanning tree |

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Clone Graph** - Deep copy of graph
2. **Number of Islands** - Count connected components in grid
3. **Course Schedule** - Detect cycle in directed graph
4. **Pacific Atlantic Water Flow** - DFS/BFS on grid
5. **Shortest Path in Binary Matrix** - BFS shortest path

### **Medium Problems:**

1. **Word Ladder** - BFS shortest transformation
2. **Network Delay Time** - Dijkstra's algorithm
3. **Redundant Connection** - Union-Find cycle detection
4. **Accounts Merge** - Connected components with Union-Find
5. **Minimum Height Trees** - Find graph centers

### **Hard Problems:**

1. **Alien Dictionary** - Topological sort
2. **Critical Connections** - Bridge detection
3. **Cheapest Flights Within K Stops** - Modified Dijkstra
4. **Strongly Connected Components** - Kosaraju/Tarjan
5. **Reconstruct Itinerary** - Eulerian path

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Graph Basics**
- [ ] Understand graph representations (adjacency list vs matrix)
- [ ] Implement DFS and BFS from scratch
- [ ] Practice on simple connectivity problems
- [ ] Understand directed vs undirected graphs

### **Day 2: Shortest Path Algorithms**
- [ ] Implement Dijkstra's algorithm
- [ ] Understand when to use Bellman-Ford
- [ ] Practice shortest path problems
- [ ] Learn path reconstruction techniques

### **Day 3: Advanced Graph Algorithms**
- [ ] Implement topological sort (both DFS and Kahn's)
- [ ] Practice strongly connected components
- [ ] Understand minimum spanning trees
- [ ] Solve cycle detection problems

### **Day 4: Real-World Applications**
- [ ] Solve 5+ graph problems on LeetCode
- [ ] Practice recognizing graph patterns in disguise
- [ ] Implement Union-Find data structure
- [ ] Build confidence with complex graph problems

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement DFS and BFS confidently
- [ ] You understand shortest path algorithms
- [ ] You can recognize when problems require graph approaches
- [ ] You can choose appropriate algorithms for different scenarios

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Advanced String Algorithms](./16-strings-advanced-detailed-java-go.md)** - Pattern matching and text processing
2. **[Mathematical Algorithms](./17-math-algorithms-detailed-java-go.md)** - Number theory and computation
3. **[System Design Algorithms](./18-system-design-detailed-java-go.md)** - Large-scale system patterns

### **🎯 Advanced Graph Topics:**
- **Network Flow** algorithms (Max Flow, Min Cut)
- **Bipartite Matching** and assignment problems
- **Planar Graphs** and specialized algorithms
- **Graph Coloring** and scheduling applications

### **💡 Pro Tips:**
- **Visualize graphs** - Draw them to understand problems better
- **Choose right representation** - Adjacency list vs matrix vs edge list
- **Understand graph properties** - Connected, directed, weighted, cyclic
- **Practice pattern recognition** - Many problems are graphs in disguise
- **Master traversals first** - DFS and BFS are building blocks for everything

**🎉 You now master graph algorithms! These powerful techniques will help you solve complex networking, relationship, and optimization problems efficiently.**