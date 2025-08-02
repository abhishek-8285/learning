# 📚 **STACKS & QUEUES: DETAILED GUIDE (JAVA & GO)**

**Master the LIFO and FIFO Data Structures**

*Prerequisites: Complete arrays and basic algorithms first*

---

## 🤔 **WHAT ARE STACKS & QUEUES?**

### **Stack = Pile of Plates 🍽️**
- **LIFO** = Last In, First Out
- **Add** plates on top (push)
- **Remove** plates from top (pop)
- **Can only access** the top plate
- **Real-world:** Browser back button, function calls, undo operations

### **Queue = Line at Coffee Shop ☕**
- **FIFO** = First In, First Out  
- **Add** people at the back (enqueue)
- **Remove** people from the front (dequeue)
- **Can only access** the front person
- **Real-world:** Print queue, task scheduling, customer service

### **Visual Comparison:**

```
STACK (LIFO):
    ↓ push
   [3] ← top
   [2]
   [1] ← bottom
    ↑ pop

QUEUE (FIFO):
enqueue → [1][2][3] → dequeue
         rear   front
```

### **Why Learn Stacks & Queues:**
- **Foundation** for more complex data structures
- **Interview favorites** - appear in 30% of coding problems
- **System design** - Essential for understanding algorithms
- **Real applications** - Parsers, compilers, operating systems
- **Problem-solving patterns** - DFS uses stack, BFS uses queue

---

## 🎯 **WHEN TO USE STACKS & QUEUES**

### **🥞 Stack Use Cases:**

1. **"Undo" functionality**
   - Text editors (Ctrl+Z)
   - Browser back button
   - Game state management

2. **Expression evaluation**
   - Parentheses matching
   - Infix to postfix conversion
   - Calculator operations

3. **Function calls**
   - Call stack management
   - Recursion simulation
   - Memory management

4. **Parsing & syntax**
   - JSON/XML parsing
   - Compiler design
   - Balanced brackets

5. **Depth-First Search (DFS)**
   - Tree/graph traversal
   - Path finding
   - Maze solving

### **🎟️ Queue Use Cases:**

1. **Task scheduling**
   - CPU task scheduling
   - Print job management
   - Background processes

2. **Breadth-First Search (BFS)**
   - Level-order tree traversal
   - Shortest path finding
   - Social network analysis

3. **Data streaming**
   - Buffer for data processing
   - Real-time systems
   - Producer-consumer problems

4. **System communication**
   - Message queues
   - Event handling
   - Request processing

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Stack Implementation and Operations**

```java
import java.util.*;

// Custom Stack Implementation using Array
class ArrayStack<T> {
    private T[] stack;
    private int top;
    private int capacity;
    
    @SuppressWarnings("unchecked")
    public ArrayStack(int capacity) {
        this.capacity = capacity;
        this.stack = (T[]) new Object[capacity];
        this.top = -1;
    }
    
    /**
     * Add element to top of stack
     * Time: O(1), Space: O(1)
     */
    public void push(T item) {
        if (isFull()) {
            throw new StackOverflowError("Stack is full");
        }
        stack[++top] = item;
    }
    
    /**
     * Remove and return top element
     * Time: O(1), Space: O(1)
     */
    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        T item = stack[top];
        stack[top--] = null; // Help GC
        return item;
    }
    
    /**
     * Return top element without removing
     * Time: O(1), Space: O(1)
     */
    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return stack[top];
    }
    
    public boolean isEmpty() {
        return top == -1;
    }
    
    public boolean isFull() {
        return top == capacity - 1;
    }
    
    public int size() {
        return top + 1;
    }
}

// Custom Stack Implementation using Linked List
class ListStack<T> {
    private Node<T> top;
    private int size;
    
    private static class Node<T> {
        T data;
        Node<T> next;
        
        Node(T data) {
            this.data = data;
        }
    }
    
    /**
     * Add element to top of stack
     * Time: O(1), Space: O(1)
     */
    public void push(T item) {
        Node<T> newNode = new Node<>(item);
        newNode.next = top;
        top = newNode;
        size++;
    }
    
    /**
     * Remove and return top element
     * Time: O(1), Space: O(1)
     */
    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        T item = top.data;
        top = top.next;
        size--;
        return item;
    }
    
    /**
     * Return top element without removing
     * Time: O(1), Space: O(1)
     */
    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return top.data;
    }
    
    public boolean isEmpty() {
        return top == null;
    }
    
    public int size() {
        return size;
    }
}

public class StackProblems {
    
    /**
     * Check if parentheses are balanced
     * Pattern: Stack for matching pairs
     * Time: O(n), Space: O(n)
     */
    public static boolean isBalanced(String s) {
        Stack<Character> stack = new Stack<>();
        Map<Character, Character> pairs = Map.of(')', '(', '}', '{', ']', '[');
        
        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else if (c == ')' || c == '}' || c == ']') {
                if (stack.isEmpty() || stack.pop() != pairs.get(c)) {
                    return false;
                }
            }
        }
        
        return stack.isEmpty();
    }
    
    /**
     * Evaluate postfix expression
     * Pattern: Stack for operand storage
     * Time: O(n), Space: O(n)
     */
    public static int evaluatePostfix(String expression) {
        Stack<Integer> stack = new Stack<>();
        String[] tokens = expression.split(" ");
        
        for (String token : tokens) {
            if (isOperator(token)) {
                int b = stack.pop();
                int a = stack.pop();
                int result = applyOperator(a, b, token);
                stack.push(result);
            } else {
                stack.push(Integer.parseInt(token));
            }
        }
        
        return stack.pop();
    }
    
    private static boolean isOperator(String token) {
        return token.equals("+") || token.equals("-") || token.equals("*") || token.equals("/");
    }
    
    private static int applyOperator(int a, int b, String operator) {
        switch (operator) {
            case "+": return a + b;
            case "-": return a - b;
            case "*": return a * b;
            case "/": return a / b;
            default: throw new IllegalArgumentException("Invalid operator: " + operator);
        }
    }
    
    /**
     * Find next greater element for each element in array
     * Pattern: Stack for pending elements
     * Time: O(n), Space: O(n)
     */
    public static int[] nextGreaterElement(int[] nums) {
        int[] result = new int[nums.length];
        Stack<Integer> stack = new Stack<>(); // Stack of indices
        
        // Initialize result with -1 (no greater element found)
        Arrays.fill(result, -1);
        
        for (int i = 0; i < nums.length; i++) {
            // Process all elements that have found their next greater element
            while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
                int index = stack.pop();
                result[index] = nums[i];
            }
            stack.push(i);
        }
        
        return result;
    }
    
    /**
     * Largest rectangle area in histogram
     * Pattern: Stack for maintaining increasing heights
     * Time: O(n), Space: O(n)
     */
    public static int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;
        
        for (int i = 0; i <= heights.length; i++) {
            int currentHeight = (i == heights.length) ? 0 : heights[i];
            
            while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()];
                int width = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            
            stack.push(i);
        }
        
        return maxArea;
    }
    
    public static void main(String[] args) {
        // Test balanced parentheses
        System.out.println("Balanced '({[]})': " + isBalanced("({[]})")); // true
        System.out.println("Balanced '({[})': " + isBalanced("({[})")); // false
        
        // Test postfix evaluation
        System.out.println("Postfix '3 4 + 2 *': " + evaluatePostfix("3 4 + 2 *")); // 14
        
        // Test next greater element
        int[] nums = {2, 1, 2, 4, 3, 1};
        int[] nextGreater = nextGreaterElement(nums);
        System.out.println("Next greater elements: " + Arrays.toString(nextGreater));
        
        // Test largest rectangle
        int[] histogram = {2, 1, 5, 6, 2, 3};
        System.out.println("Largest rectangle area: " + largestRectangleArea(histogram)); // 10
    }
}
```

### **Queue Implementation and Operations**

```java
import java.util.*;

// Custom Queue Implementation using Array (Circular)
class ArrayQueue<T> {
    private T[] queue;
    private int front;
    private int rear;
    private int size;
    private int capacity;
    
    @SuppressWarnings("unchecked")
    public ArrayQueue(int capacity) {
        this.capacity = capacity;
        this.queue = (T[]) new Object[capacity];
        this.front = 0;
        this.rear = -1;
        this.size = 0;
    }
    
    /**
     * Add element to rear of queue
     * Time: O(1), Space: O(1)
     */
    public void enqueue(T item) {
        if (isFull()) {
            throw new IllegalStateException("Queue is full");
        }
        rear = (rear + 1) % capacity; // Circular increment
        queue[rear] = item;
        size++;
    }
    
    /**
     * Remove and return front element
     * Time: O(1), Space: O(1)
     */
    public T dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        T item = queue[front];
        queue[front] = null; // Help GC
        front = (front + 1) % capacity; // Circular increment
        size--;
        return item;
    }
    
    /**
     * Return front element without removing
     * Time: O(1), Space: O(1)
     */
    public T peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return queue[front];
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
    
    public boolean isFull() {
        return size == capacity;
    }
    
    public int size() {
        return size;
    }
}

// Custom Queue Implementation using Linked List
class ListQueue<T> {
    private Node<T> front;
    private Node<T> rear;
    private int size;
    
    private static class Node<T> {
        T data;
        Node<T> next;
        
        Node(T data) {
            this.data = data;
        }
    }
    
    /**
     * Add element to rear of queue
     * Time: O(1), Space: O(1)
     */
    public void enqueue(T item) {
        Node<T> newNode = new Node<>(item);
        
        if (isEmpty()) {
            front = rear = newNode;
        } else {
            rear.next = newNode;
            rear = newNode;
        }
        size++;
    }
    
    /**
     * Remove and return front element
     * Time: O(1), Space: O(1)
     */
    public T dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        
        T item = front.data;
        front = front.next;
        
        if (front == null) { // Queue became empty
            rear = null;
        }
        
        size--;
        return item;
    }
    
    /**
     * Return front element without removing
     * Time: O(1), Space: O(1)
     */
    public T peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return front.data;
    }
    
    public boolean isEmpty() {
        return front == null;
    }
    
    public int size() {
        return size;
    }
}

public class QueueProblems {
    
    /**
     * Generate binary numbers from 1 to n
     * Pattern: Queue for generating sequences
     * Time: O(n), Space: O(n)
     */
    public static List<String> generateBinaryNumbers(int n) {
        List<String> result = new ArrayList<>();
        Queue<String> queue = new LinkedList<>();
        
        queue.offer("1");
        
        for (int i = 0; i < n; i++) {
            String current = queue.poll();
            result.add(current);
            
            // Generate next binary numbers by appending 0 and 1
            queue.offer(current + "0");
            queue.offer(current + "1");
        }
        
        return result;
    }
    
    /**
     * First non-repeating character in stream
     * Pattern: Queue for maintaining order + frequency map
     * Time: O(n) per character, Space: O(k) where k is unique characters
     */
    static class FirstNonRepeating {
        private Queue<Character> queue;
        private Map<Character, Integer> frequency;
        
        public FirstNonRepeating() {
            queue = new LinkedList<>();
            frequency = new HashMap<>();
        }
        
        public char addCharacter(char c) {
            frequency.put(c, frequency.getOrDefault(c, 0) + 1);
            queue.offer(c);
            
            // Remove characters that are no longer non-repeating
            while (!queue.isEmpty() && frequency.get(queue.peek()) > 1) {
                queue.poll();
            }
            
            return queue.isEmpty() ? '#' : queue.peek();
        }
    }
    
    /**
     * Sliding window maximum
     * Pattern: Deque for maintaining maximum in window
     * Time: O(n), Space: O(k) where k is window size
     */
    public static int[] slidingWindowMaximum(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];
        Deque<Integer> deque = new ArrayDeque<>(); // Store indices
        
        for (int i = 0; i < n; i++) {
            // Remove elements outside current window
            while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }
            
            // Remove elements smaller than current element
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            
            deque.offerLast(i);
            
            // Add maximum to result when window is complete
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.peekFirst()];
            }
        }
        
        return result;
    }
    
    /**
     * Implement stack using two queues
     * Pattern: Queue manipulation to simulate stack
     */
    static class StackUsingQueues<T> {
        private Queue<T> queue1;
        private Queue<T> queue2;
        
        public StackUsingQueues() {
            queue1 = new LinkedList<>();
            queue2 = new LinkedList<>();
        }
        
        /**
         * Push element to stack (simulate with queues)
         * Time: O(1), Space: O(1)
         */
        public void push(T item) {
            queue1.offer(item);
        }
        
        /**
         * Pop element from stack (simulate with queues)
         * Time: O(n), Space: O(1)
         */
        public T pop() {
            if (queue1.isEmpty()) {
                throw new EmptyStackException();
            }
            
            // Move all elements except last to queue2
            while (queue1.size() > 1) {
                queue2.offer(queue1.poll());
            }
            
            // Get the last element (top of stack)
            T item = queue1.poll();
            
            // Swap queues
            Queue<T> temp = queue1;
            queue1 = queue2;
            queue2 = temp;
            
            return item;
        }
        
        public T peek() {
            if (queue1.isEmpty()) {
                throw new EmptyStackException();
            }
            
            // Move all elements except last to queue2
            while (queue1.size() > 1) {
                queue2.offer(queue1.poll());
            }
            
            // Peek at the last element
            T item = queue1.peek();
            queue2.offer(queue1.poll());
            
            // Swap queues
            Queue<T> temp = queue1;
            queue1 = queue2;
            queue2 = temp;
            
            return item;
        }
        
        public boolean isEmpty() {
            return queue1.isEmpty();
        }
    }
    
    public static void main(String[] args) {
        // Test binary number generation
        List<String> binaryNumbers = generateBinaryNumbers(8);
        System.out.println("Binary numbers 1-8: " + binaryNumbers);
        
        // Test first non-repeating character
        FirstNonRepeating fnr = new FirstNonRepeating();
        String stream = "aabccba";
        System.out.print("First non-repeating in stream: ");
        for (char c : stream.toCharArray()) {
            System.out.print(fnr.addCharacter(c) + " ");
        }
        System.out.println();
        
        // Test sliding window maximum
        int[] nums = {1, 3, -1, -3, 5, 3, 6, 7};
        int[] windowMax = slidingWindowMaximum(nums, 3);
        System.out.println("Sliding window maximum: " + Arrays.toString(windowMax));
        
        // Test stack using queues
        StackUsingQueues<Integer> stack = new StackUsingQueues<>();
        stack.push(1);
        stack.push(2);
        stack.push(3);
        System.out.println("Stack using queues - pop: " + stack.pop()); // 3
        System.out.println("Stack using queues - peek: " + stack.peek()); // 2
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Stack Implementation in Go**

```go
package main

import (
    "errors"
    "fmt"
    "strconv"
    "strings"
)

// Array-based Stack
type ArrayStack struct {
    items    []interface{}
    capacity int
}

func NewArrayStack(capacity int) *ArrayStack {
    return &ArrayStack{
        items:    make([]interface{}, 0, capacity),
        capacity: capacity,
    }
}

/**
 * Add element to top of stack
 * Time: O(1), Space: O(1)
 */
func (s *ArrayStack) Push(item interface{}) error {
    if len(s.items) >= s.capacity {
        return errors.New("stack overflow")
    }
    s.items = append(s.items, item)
    return nil
}

/**
 * Remove and return top element
 * Time: O(1), Space: O(1)
 */
func (s *ArrayStack) Pop() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("stack underflow")
    }
    
    index := len(s.items) - 1
    item := s.items[index]
    s.items = s.items[:index]
    return item, nil
}

/**
 * Return top element without removing
 * Time: O(1), Space: O(1)
 */
func (s *ArrayStack) Peek() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("stack is empty")
    }
    return s.items[len(s.items)-1], nil
}

func (s *ArrayStack) IsEmpty() bool {
    return len(s.items) == 0
}

func (s *ArrayStack) Size() int {
    return len(s.items)
}

// Linked List-based Stack
type StackNode struct {
    data interface{}
    next *StackNode
}

type ListStack struct {
    top  *StackNode
    size int
}

func NewListStack() *ListStack {
    return &ListStack{}
}

/**
 * Add element to top of stack
 * Time: O(1), Space: O(1)
 */
func (s *ListStack) Push(item interface{}) {
    newNode := &StackNode{data: item, next: s.top}
    s.top = newNode
    s.size++
}

/**
 * Remove and return top element
 * Time: O(1), Space: O(1)
 */
func (s *ListStack) Pop() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("stack underflow")
    }
    
    item := s.top.data
    s.top = s.top.next
    s.size--
    return item, nil
}

/**
 * Return top element without removing
 * Time: O(1), Space: O(1)
 */
func (s *ListStack) Peek() (interface{}, error) {
    if s.IsEmpty() {
        return nil, errors.New("stack is empty")
    }
    return s.top.data, nil
}

func (s *ListStack) IsEmpty() bool {
    return s.top == nil
}

func (s *ListStack) Size() int {
    return s.size
}

/**
 * Check if parentheses are balanced
 * Pattern: Stack for matching pairs
 * Time: O(n), Space: O(n)
 */
func isBalanced(s string) bool {
    stack := NewListStack()
    pairs := map[rune]rune{')': '(', '}': '{', ']': '['}
    
    for _, char := range s {
        if char == '(' || char == '{' || char == '[' {
            stack.Push(char)
        } else if char == ')' || char == '}' || char == ']' {
            if stack.IsEmpty() {
                return false
            }
            
            top, _ := stack.Pop()
            if top != pairs[char] {
                return false
            }
        }
    }
    
    return stack.IsEmpty()
}

/**
 * Evaluate postfix expression
 * Pattern: Stack for operand storage
 * Time: O(n), Space: O(n)
 */
func evaluatePostfix(expression string) int {
    stack := NewListStack()
    tokens := strings.Fields(expression)
    
    for _, token := range tokens {
        if isOperator(token) {
            b, _ := stack.Pop()
            a, _ := stack.Pop()
            result := applyOperator(a.(int), b.(int), token)
            stack.Push(result)
        } else {
            num, _ := strconv.Atoi(token)
            stack.Push(num)
        }
    }
    
    result, _ := stack.Pop()
    return result.(int)
}

func isOperator(token string) bool {
    return token == "+" || token == "-" || token == "*" || token == "/"
}

func applyOperator(a, b int, operator string) int {
    switch operator {
    case "+":
        return a + b
    case "-":
        return a - b
    case "*":
        return a * b
    case "/":
        return a / b
    default:
        panic("Invalid operator")
    }
}

/**
 * Find next greater element for each element in array
 * Pattern: Stack for pending elements
 * Time: O(n), Space: O(n)
 */
func nextGreaterElement(nums []int) []int {
    result := make([]int, len(nums))
    stack := NewListStack()
    
    // Initialize result with -1 (no greater element found)
    for i := range result {
        result[i] = -1
    }
    
    for i, num := range nums {
        // Process all elements that have found their next greater element
        for !stack.IsEmpty() {
            top, _ := stack.Peek()
            index := top.(int)
            if nums[index] < num {
                stack.Pop()
                result[index] = num
            } else {
                break
            }
        }
        stack.Push(i)
    }
    
    return result
}

func main() {
    // Test balanced parentheses
    fmt.Printf("Balanced '({[]})': %t\n", isBalanced("({[]})")) // true
    fmt.Printf("Balanced '({[})': %t\n", isBalanced("({[})"))   // false
    
    // Test postfix evaluation
    fmt.Printf("Postfix '3 4 + 2 *': %d\n", evaluatePostfix("3 4 + 2 *")) // 14
    
    // Test next greater element
    nums := []int{2, 1, 2, 4, 3, 1}
    nextGreater := nextGreaterElement(nums)
    fmt.Printf("Next greater elements: %v\n", nextGreater)
}
```

### **Queue Implementation in Go**

```go
package main

import (
    "errors"
    "fmt"
)

// Array-based Queue (Circular)
type ArrayQueue struct {
    items    []interface{}
    front    int
    rear     int
    size     int
    capacity int
}

func NewArrayQueue(capacity int) *ArrayQueue {
    return &ArrayQueue{
        items:    make([]interface{}, capacity),
        front:    0,
        rear:     -1,
        size:     0,
        capacity: capacity,
    }
}

/**
 * Add element to rear of queue
 * Time: O(1), Space: O(1)
 */
func (q *ArrayQueue) Enqueue(item interface{}) error {
    if q.IsFull() {
        return errors.New("queue is full")
    }
    
    q.rear = (q.rear + 1) % q.capacity
    q.items[q.rear] = item
    q.size++
    return nil
}

/**
 * Remove and return front element
 * Time: O(1), Space: O(1)
 */
func (q *ArrayQueue) Dequeue() (interface{}, error) {
    if q.IsEmpty() {
        return nil, errors.New("queue is empty")
    }
    
    item := q.items[q.front]
    q.items[q.front] = nil // Help GC
    q.front = (q.front + 1) % q.capacity
    q.size--
    return item, nil
}

/**
 * Return front element without removing
 * Time: O(1), Space: O(1)
 */
func (q *ArrayQueue) Peek() (interface{}, error) {
    if q.IsEmpty() {
        return nil, errors.New("queue is empty")
    }
    return q.items[q.front], nil
}

func (q *ArrayQueue) IsEmpty() bool {
    return q.size == 0
}

func (q *ArrayQueue) IsFull() bool {
    return q.size == q.capacity
}

func (q *ArrayQueue) Size() int {
    return q.size
}

// Linked List-based Queue
type QueueNode struct {
    data interface{}
    next *QueueNode
}

type ListQueue struct {
    front *QueueNode
    rear  *QueueNode
    size  int
}

func NewListQueue() *ListQueue {
    return &ListQueue{}
}

/**
 * Add element to rear of queue
 * Time: O(1), Space: O(1)
 */
func (q *ListQueue) Enqueue(item interface{}) {
    newNode := &QueueNode{data: item}
    
    if q.IsEmpty() {
        q.front = newNode
        q.rear = newNode
    } else {
        q.rear.next = newNode
        q.rear = newNode
    }
    q.size++
}

/**
 * Remove and return front element
 * Time: O(1), Space: O(1)
 */
func (q *ListQueue) Dequeue() (interface{}, error) {
    if q.IsEmpty() {
        return nil, errors.New("queue is empty")
    }
    
    item := q.front.data
    q.front = q.front.next
    
    if q.front == nil { // Queue became empty
        q.rear = nil
    }
    
    q.size--
    return item, nil
}

/**
 * Return front element without removing
 * Time: O(1), Space: O(1)
 */
func (q *ListQueue) Peek() (interface{}, error) {
    if q.IsEmpty() {
        return nil, errors.New("queue is empty")
    }
    return q.front.data, nil
}

func (q *ListQueue) IsEmpty() bool {
    return q.front == nil
}

func (q *ListQueue) Size() int {
    return q.size
}

/**
 * Generate binary numbers from 1 to n
 * Pattern: Queue for generating sequences
 * Time: O(n), Space: O(n)
 */
func generateBinaryNumbers(n int) []string {
    var result []string
    queue := NewListQueue()
    
    queue.Enqueue("1")
    
    for i := 0; i < n; i++ {
        current, _ := queue.Dequeue()
        currentStr := current.(string)
        result = append(result, currentStr)
        
        // Generate next binary numbers by appending 0 and 1
        queue.Enqueue(currentStr + "0")
        queue.Enqueue(currentStr + "1")
    }
    
    return result
}

/**
 * First non-repeating character in stream
 * Pattern: Queue for maintaining order + frequency map
 */
type FirstNonRepeating struct {
    queue     *ListQueue
    frequency map[rune]int
}

func NewFirstNonRepeating() *FirstNonRepeating {
    return &FirstNonRepeating{
        queue:     NewListQueue(),
        frequency: make(map[rune]int),
    }
}

func (fnr *FirstNonRepeating) AddCharacter(c rune) rune {
    fnr.frequency[c]++
    fnr.queue.Enqueue(c)
    
    // Remove characters that are no longer non-repeating
    for !fnr.queue.IsEmpty() {
        front, _ := fnr.queue.Peek()
        if fnr.frequency[front.(rune)] > 1 {
            fnr.queue.Dequeue()
        } else {
            break
        }
    }
    
    if fnr.queue.IsEmpty() {
        return '#'
    }
    
    front, _ := fnr.queue.Peek()
    return front.(rune)
}

/**
 * Implement stack using two queues
 * Pattern: Queue manipulation to simulate stack
 */
type StackUsingQueues struct {
    queue1 *ListQueue
    queue2 *ListQueue
}

func NewStackUsingQueues() *StackUsingQueues {
    return &StackUsingQueues{
        queue1: NewListQueue(),
        queue2: NewListQueue(),
    }
}

/**
 * Push element to stack (simulate with queues)
 * Time: O(1), Space: O(1)
 */
func (s *StackUsingQueues) Push(item interface{}) {
    s.queue1.Enqueue(item)
}

/**
 * Pop element from stack (simulate with queues)
 * Time: O(n), Space: O(1)
 */
func (s *StackUsingQueues) Pop() (interface{}, error) {
    if s.queue1.IsEmpty() {
        return nil, errors.New("stack is empty")
    }
    
    // Move all elements except last to queue2
    for s.queue1.Size() > 1 {
        item, _ := s.queue1.Dequeue()
        s.queue2.Enqueue(item)
    }
    
    // Get the last element (top of stack)
    item, _ := s.queue1.Dequeue()
    
    // Swap queues
    s.queue1, s.queue2 = s.queue2, s.queue1
    
    return item, nil
}

func (s *StackUsingQueues) IsEmpty() bool {
    return s.queue1.IsEmpty()
}

func main() {
    // Test binary number generation
    binaryNumbers := generateBinaryNumbers(8)
    fmt.Printf("Binary numbers 1-8: %v\n", binaryNumbers)
    
    // Test first non-repeating character
    fnr := NewFirstNonRepeating()
    stream := "aabccba"
    fmt.Print("First non-repeating in stream: ")
    for _, c := range stream {
        fmt.Printf("%c ", fnr.AddCharacter(c))
    }
    fmt.Println()
    
    // Test stack using queues
    stack := NewStackUsingQueues()
    stack.Push(1)
    stack.Push(2)
    stack.Push(3)
    item, _ := stack.Pop()
    fmt.Printf("Stack using queues - pop: %v\n", item) // 3
}
```

---

## 🧠 **UNDERSTANDING APPLICATIONS**

### **🔍 Stack Applications Deep Dive**

#### **1. Function Call Management**
```java
// How recursion actually works under the hood
public int factorial(int n) {
    if (n <= 1) return 1;    // Call Stack: [factorial(1)]
    return n * factorial(n-1); // Call Stack: [factorial(5), factorial(4), factorial(3), factorial(2), factorial(1)]
}
```

#### **2. Expression Parsing**
```java
// Convert infix to postfix using stack
// Infix: 3 + 4 * 2
// Postfix: 3 4 2 * +

public String infixToPostfix(String infix) {
    Stack<Character> operators = new Stack<>();
    StringBuilder postfix = new StringBuilder();
    
    for (char c : infix.toCharArray()) {
        if (Character.isDigit(c)) {
            postfix.append(c).append(' ');
        } else if (c == '(') {
            operators.push(c);
        } else if (c == ')') {
            while (!operators.isEmpty() && operators.peek() != '(') {
                postfix.append(operators.pop()).append(' ');
            }
            operators.pop(); // Remove '('
        } else if (isOperator(c)) {
            while (!operators.isEmpty() && precedence(c) <= precedence(operators.peek())) {
                postfix.append(operators.pop()).append(' ');
            }
            operators.push(c);
        }
    }
    
    while (!operators.isEmpty()) {
        postfix.append(operators.pop()).append(' ');
    }
    
    return postfix.toString().trim();
}
```

### **🎯 Queue Applications Deep Dive**

#### **1. BFS Implementation**
```java
// Level-order traversal using queue
public List<List<Integer>> levelOrder(TreeNode root) {
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
```

#### **2. Producer-Consumer Pattern**
```java
// Thread-safe queue for producer-consumer
class ProducerConsumer {
    private final Queue<Integer> queue = new LinkedList<>();
    private final int capacity;
    
    public ProducerConsumer(int capacity) {
        this.capacity = capacity;
    }
    
    public synchronized void produce(int item) throws InterruptedException {
        while (queue.size() == capacity) {
            wait(); // Queue is full, wait for consumer
        }
        queue.offer(item);
        notifyAll(); // Notify consumers
    }
    
    public synchronized int consume() throws InterruptedException {
        while (queue.isEmpty()) {
            wait(); // Queue is empty, wait for producer
        }
        int item = queue.poll();
        notifyAll(); // Notify producers
        return item;
    }
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Stack Problems:**

1. **Valid Parentheses** ✅ (Implemented above)
2. **Evaluate Postfix Expression** ✅ (Implemented above)
3. **Next Greater Element** ✅ (Implemented above)
4. **Largest Rectangle in Histogram** ✅ (Implemented above)
5. **Min Stack** (Stack that supports getMin in O(1))

### **Queue Problems:**

1. **Generate Binary Numbers** ✅ (Implemented above)
2. **First Non-Repeating Character** ✅ (Implemented above)
3. **Sliding Window Maximum** ✅ (Implemented above)
4. **Implement Stack using Queues** ✅ (Implemented above)
5. **Implement Queue using Stacks**

### **Advanced Problems:**

1. **Design Circular Queue**
2. **Design Circular Deque**
3. **Trapping Rain Water** (Stack-based solution)
4. **Basic Calculator** (Stack for expression evaluation)
5. **Shortest Path in Binary Matrix** (BFS with queue)

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Stack Fundamentals**
- [ ] Implement stack using both array and linked list
- [ ] Solve valid parentheses problem
- [ ] Practice expression evaluation (postfix)
- [ ] Understand call stack visualization

### **Day 2: Master Queue Fundamentals**
- [ ] Implement queue using both array and linked list
- [ ] Understand circular queue concept
- [ ] Solve binary number generation
- [ ] Practice BFS tree traversal

### **Day 3: Advanced Applications**
- [ ] Implement stack using queues and vice versa
- [ ] Solve next greater element problem
- [ ] Practice sliding window maximum
- [ ] Understand deque operations

### **Day 4: Problem Recognition & Speed**
- [ ] Solve 5 stack/queue problems on LeetCode
- [ ] For each problem, identify whether to use stack or queue
- [ ] Practice explaining LIFO vs FIFO concepts
- [ ] Build pattern recognition skills

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement stacks and queues from scratch
- [ ] You know when to use LIFO vs FIFO
- [ ] You can recognize stack/queue patterns in problems
- [ ] You understand time/space complexity of operations

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Binary Search - Detailed](./07-binary-search-detailed-java-go.md)** - Master search algorithms
2. **[Linked Lists - Detailed](./08-linked-lists-detailed-java-go.md)** - Learn dynamic data structures
3. **[Trees - Detailed](./09-trees-detailed-java-go.md)** - Master hierarchical structures

### **🎯 Advanced Stack & Queue Topics:**
- **Priority Queues** and heaps
- **Deque** (double-ended queue) operations
- **Thread-safe** implementations
- **Memory-efficient** circular implementations

### **💡 Pro Tips:**
- **Choose the right tool** - Stack for LIFO, Queue for FIFO
- **Consider space complexity** - Linked list vs array trade-offs
- **Understand real applications** - Function calls, BFS, DFS
- **Practice both implementations** - Array and linked list versions

**🎉 You now master the fundamental linear data structures! Stacks and queues are the building blocks for many advanced algorithms and system designs.**