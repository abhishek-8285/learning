# 🚀 **PROGRAMMING FUNDAMENTALS FOR DSA**

**Java and Go Basics for Data Structures & Algorithms**

*Perfect for complete beginners who need to understand the tools before solving problems*

---

## 🎯 **WHY JAVA AND GO FOR DSA?**

### **☕ Java - The Interview Standard**
- **90% of tech interviews** use Java or accept it
- **Excellent tooling** - IntelliJ, Eclipse, VS Code
- **Clear syntax** - Easy to read and understand
- **Rich libraries** - Built-in data structures
- **Memory management** - Automatic garbage collection

### **🐹 Go - The Modern Choice**
- **Simple and clean** - Minimal syntax
- **Fast execution** - Great for competitive programming
- **Growing popularity** - Many companies use Go
- **Great concurrency** - Built for modern systems
- **Easy deployment** - Single binary files

---

## 🛠️ **SETTING UP YOUR ENVIRONMENT**

### **☕ Java Setup**

#### **Step 1: Install Java JDK**
```bash
# Download from: https://openjdk.org/
# Or use package manager:

# macOS
brew install openjdk@17

# Windows
winget install Microsoft.OpenJDK.17

# Ubuntu/Debian
sudo apt install openjdk-17-jdk
```

#### **Step 2: Install VS Code + Extensions**
- Download VS Code: https://code.visualstudio.com/
- Install "Extension Pack for Java"
- Install "Code Runner" for quick execution

#### **Step 3: Test Your Setup**
```java
// Create: HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, DSA World!");
    }
}
```

### **🐹 Go Setup**

#### **Step 1: Install Go**
```bash
# Download from: https://golang.org/
# Or use package manager:

# macOS
brew install go

# Windows
winget install GoLang.Go

# Ubuntu/Debian
sudo apt install golang-go
```

#### **Step 2: Test Your Setup**
```go
// Create: hello.go
package main

import "fmt"

func main() {
    fmt.Println("Hello, DSA World!")
}
```

---

## 📊 **DATA TYPES COMPARISON**

### **Basic Types**

| Type | Java | Go | Use Case |
|------|------|----|---------| 
| **Integer** | `int` | `int` | Counting, indexing |
| **String** | `String` | `string` | Text processing |
| **Boolean** | `boolean` | `bool` | True/false logic |
| **Array** | `int[]` | `[]int` | Fixed-size collections |
| **Dynamic Array** | `ArrayList<Integer>` | `[]int` | Growing collections |

### **☕ Java Examples**
```java
// Basic variables
int count = 5;
String name = "Alice";
boolean isActive = true;

// Arrays
int[] numbers = {1, 2, 3, 4, 5};
String[] names = {"Alice", "Bob", "Charlie"};

// Dynamic arrays (ArrayList)
import java.util.ArrayList;
ArrayList<Integer> dynamicNumbers = new ArrayList<>();
dynamicNumbers.add(1);
dynamicNumbers.add(2);
```

### **🐹 Go Examples**
```go
// Basic variables
count := 5
name := "Alice"
isActive := true

// Arrays (fixed size)
numbers := [5]int{1, 2, 3, 4, 5}
names := [3]string{"Alice", "Bob", "Charlie"}

// Slices (dynamic arrays)
dynamicNumbers := []int{}
dynamicNumbers = append(dynamicNumbers, 1)
dynamicNumbers = append(dynamicNumbers, 2)
```

---

## 🔄 **CONTROL STRUCTURES**

### **Loops - The Heart of DSA**

#### **☕ Java Loops**
```java
// For loop - most common in DSA
for (int i = 0; i < 5; i++) {
    System.out.println("Number: " + i);
}

// Enhanced for loop - for arrays
int[] arr = {1, 2, 3, 4, 5};
for (int num : arr) {
    System.out.println("Value: " + num);
}

// While loop - for unknown iterations
int i = 0;
while (i < 5) {
    System.out.println("Count: " + i);
    i++;
}
```

#### **🐹 Go Loops**
```go
// For loop (Go only has for, no while)
for i := 0; i < 5; i++ {
    fmt.Printf("Number: %d\n", i)
}

// Range loop - for slices/arrays
arr := []int{1, 2, 3, 4, 5}
for index, value := range arr {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}

// While-style loop
i := 0
for i < 5 {
    fmt.Printf("Count: %d\n", i)
    i++
}
```

### **Conditionals**

#### **☕ Java Conditionals**
```java
int score = 85;

if (score >= 90) {
    System.out.println("Excellent!");
} else if (score >= 80) {
    System.out.println("Good!");
} else {
    System.out.println("Keep trying!");
}

// Switch statement
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Other day");
}
```

#### **🐹 Go Conditionals**
```go
score := 85

if score >= 90 {
    fmt.Println("Excellent!")
} else if score >= 80 {
    fmt.Println("Good!")
} else {
    fmt.Println("Keep trying!")
}

// Switch statement
day := 3
switch day {
case 1:
    fmt.Println("Monday")
case 2:
    fmt.Println("Tuesday")
default:
    fmt.Println("Other day")
}
```

---

## 📊 **ESSENTIAL DATA STRUCTURES**

### **Arrays - Your Foundation**

#### **☕ Java Arrays**
```java
public class ArrayBasics {
    public static void main(String[] args) {
        // Creating arrays
        int[] numbers = new int[5];  // [0, 0, 0, 0, 0]
        numbers[0] = 10;  // [10, 0, 0, 0, 0]
        numbers[1] = 20;  // [10, 20, 0, 0, 0]
        
        // Array with initial values
        String[] fruits = {"apple", "banana", "orange"};
        
        // Accessing elements
        System.out.println("First fruit: " + fruits[0]);
        System.out.println("Array length: " + fruits.length);
        
        // Iterating through array
        for (int i = 0; i < numbers.length; i++) {
            System.out.println("Index " + i + ": " + numbers[i]);
        }
    }
}
```

#### **🐹 Go Arrays & Slices**
```go
package main

import "fmt"

func main() {
    // Arrays (fixed size)
    var numbers [5]int  // [0, 0, 0, 0, 0]
    numbers[0] = 10     // [10, 0, 0, 0, 0]
    numbers[1] = 20     // [10, 20, 0, 0, 0]
    
    // Array with initial values
    fruits := [3]string{"apple", "banana", "orange"}
    
    // Slices (dynamic arrays) - more common
    dynamicNumbers := []int{1, 2, 3}
    dynamicNumbers = append(dynamicNumbers, 4)  // [1, 2, 3, 4]
    
    // Accessing elements
    fmt.Printf("First fruit: %s\n", fruits[0])
    fmt.Printf("Slice length: %d\n", len(dynamicNumbers))
    
    // Iterating
    for i, value := range dynamicNumbers {
        fmt.Printf("Index %d: %d\n", i, value)
    }
}
```

### **Strings - Text Processing**

#### **☕ Java Strings**
```java
public class StringBasics {
    public static void main(String[] args) {
        String text = "Hello World";
        
        // String properties
        System.out.println("Length: " + text.length());
        System.out.println("Character at index 0: " + text.charAt(0));
        
        // String methods (important for DSA)
        System.out.println("Lowercase: " + text.toLowerCase());
        System.out.println("Substring: " + text.substring(0, 5));
        System.out.println("Contains 'World': " + text.contains("World"));
        
        // Converting to character array (useful for manipulation)
        char[] chars = text.toCharArray();
        for (char c : chars) {
            System.out.print(c + " ");
        }
    }
}
```

#### **🐹 Go Strings**
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    text := "Hello World"
    
    // String properties
    fmt.Printf("Length: %d\n", len(text))
    fmt.Printf("Character at index 0: %c\n", text[0])
    
    // String methods (from strings package)
    fmt.Printf("Lowercase: %s\n", strings.ToLower(text))
    fmt.Printf("Substring: %s\n", text[0:5])
    fmt.Printf("Contains 'World': %t\n", strings.Contains(text, "World"))
    
    // Converting to slice of bytes (useful for manipulation)
    bytes := []byte(text)
    for _, b := range bytes {
        fmt.Printf("%c ", b)
    }
    fmt.Println()
    
    // Converting to slice of runes (for Unicode)
    runes := []rune(text)
    for _, r := range runes {
        fmt.Printf("%c ", r)
    }
}
```

---

## 🧮 **FUNCTIONS - BUILDING BLOCKS**

### **☕ Java Functions**
```java
public class FunctionExamples {
    // Basic function
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Function with array parameter
    public static int findMax(int[] arr) {
        if (arr.length == 0) return 0;
        
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        return max;
    }
    
    // Function that modifies array
    public static void reverseArray(int[] arr) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left < right) {
            // Swap elements
            int temp = arr[left];
            arr[left] = arr[right];
            arr[right] = temp;
            
            left++;
            right--;
        }
    }
    
    public static void main(String[] args) {
        System.out.println(add(5, 3));  // 8
        
        int[] numbers = {3, 1, 4, 1, 5};
        System.out.println(findMax(numbers));  // 5
        
        reverseArray(numbers);
        for (int num : numbers) {
            System.out.print(num + " ");  // 5 1 4 1 3
        }
    }
}
```

### **🐹 Go Functions**
```go
package main

import "fmt"

// Basic function
func add(a, b int) int {
    return a + b
}

// Function with slice parameter
func findMax(arr []int) int {
    if len(arr) == 0 {
        return 0
    }
    
    max := arr[0]
    for i := 1; i < len(arr); i++ {
        if arr[i] > max {
            max = arr[i]
        }
    }
    return max
}

// Function that modifies slice
func reverseSlice(arr []int) {
    left := 0
    right := len(arr) - 1
    
    for left < right {
        // Swap elements
        arr[left], arr[right] = arr[right], arr[left]
        left++
        right--
    }
}

func main() {
    fmt.Println(add(5, 3))  // 8
    
    numbers := []int{3, 1, 4, 1, 5}
    fmt.Println(findMax(numbers))  // 5
    
    reverseSlice(numbers)
    for _, num := range numbers {
        fmt.Printf("%d ", num)  // 5 1 4 1 3
    }
    fmt.Println()
}
```

---

## 🎯 **YOUR FIRST DSA PROBLEM**

Let's solve the famous "Two Sum" problem in both languages!

**Problem:** Given an array of numbers and a target, find two numbers that add up to the target.

### **☕ Java Solution**
```java
import java.util.HashMap;
import java.util.Arrays;

public class TwoSum {
    public static int[] twoSum(int[] nums, int target) {
        // HashMap to store number and its index
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            // If complement exists in map, we found our answer
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }
            
            // Store current number and its index
            map.put(nums[i], i);
        }
        
        return new int[]{};  // No solution found
    }
    
    public static void main(String[] args) {
        int[] nums = {2, 7, 11, 15};
        int target = 9;
        
        int[] result = twoSum(nums, target);
        System.out.println("Indices: " + Arrays.toString(result));
        System.out.println("Numbers: " + nums[result[0]] + " + " + nums[result[1]] + " = " + target);
    }
}
```

### **🐹 Go Solution**
```go
package main

import "fmt"

func twoSum(nums []int, target int) []int {
    // Map to store number and its index
    numMap := make(map[int]int)
    
    for i, num := range nums {
        complement := target - num
        
        // If complement exists in map, we found our answer
        if index, exists := numMap[complement]; exists {
            return []int{index, i}
        }
        
        // Store current number and its index
        numMap[num] = i
    }
    
    return []int{}  // No solution found
}

func main() {
    nums := []int{2, 7, 11, 15}
    target := 9
    
    result := twoSum(nums, target)
    fmt.Printf("Indices: %v\n", result)
    fmt.Printf("Numbers: %d + %d = %d\n", nums[result[0]], nums[result[1]], target)
}
```

### **🧠 Understanding the Solution**

1. **The Problem:** Find two numbers that add up to 9 in [2, 7, 11, 15]
2. **The Insight:** If we need `a + b = target`, then `b = target - a`
3. **The Strategy:** 
   - For each number, calculate what its "partner" should be
   - Check if we've seen that partner before
   - If yes, we found our answer!
4. **The Result:** Numbers at indices 0 and 1 (values 2 and 7) add up to 9

---

## 🚀 **NEXT STEPS**

### **✅ You're Ready For:**
1. **[Arrays & Strings - Easy](./01-arrays-strings-easy.md)** - Start solving real problems
2. **Practice the Two Sum problem** until you understand it completely
3. **Set up both Java and Go** on your computer

### **🎯 Practice Goals:**
- [ ] Set up Java development environment
- [ ] Set up Go development environment  
- [ ] Solve Two Sum problem in both languages
- [ ] Understand arrays, strings, and basic functions
- [ ] Feel comfortable with loops and conditionals

### **📚 Study Tips:**
1. **Code every example** - Don't just read, type it out
2. **Experiment** - Change values and see what happens
3. **Explain out loud** - If you can explain it, you understand it
4. **Start small** - Master the basics before moving to complex problems

**🎉 Congratulations! You now have the programming foundation needed for Data Structures & Algorithms. Time to start solving real problems!**