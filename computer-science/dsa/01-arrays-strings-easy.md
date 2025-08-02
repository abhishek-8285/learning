# Arrays & Strings - Your Foundation 📚

**The Building Blocks of Everything You'll Build**

*If you're coming from the beginner guide, you're ready for this! If not, consider starting with `00-complete-beginner-start-here.md`*

---

## 🤔 **WHAT ARE ARRAYS & STRINGS? (Simple Explanation)**

### **Arrays = Your Digital Bookshelf**
Think of an array like a bookshelf with numbered slots:
```
Slot:  [0]    [1]    [2]    [3]    [4]
Book:  [📗]   [📘]   [📙]   [📕]   [📓]
```

**Real-world examples:**
- Your phone's contact list (each contact has a position)
- Items in your shopping cart (numbered 1, 2, 3...)
- Seats in a movie theater (Row A, Seat 1, 2, 3...)

### **Strings = Sequences of Letters**
A string is just a chain of characters:
```
"HELLO" = ['H', 'E', 'L', 'L', 'O']
Position:  0    1    2    3    4
```

**Real-world examples:**
- Your name, address, email
- Text messages, tweets, comments
- Any text you see on your screen

---

## 🧠 **WHY LEARN ARRAYS & STRINGS FIRST?**

### **🔥 They're EVERYWHERE:**
- **70% of coding interview problems** use arrays or strings
- **Every app you use** stores data in arrays (photos, messages, contacts)
- **Foundation for everything else** - trees, graphs, etc. all build on arrays

### **💪 They Teach Core Skills:**
- **Problem-solving patterns** that apply to harder topics
- **Thinking systematically** about data manipulation
- **Algorithm design** - breaking problems into steps

---

## 📚 **BEGINNER-FRIENDLY CONCEPTS**

### **Arrays in Simple Terms**

#### **What "Fast Access" Means:**
```python
contacts = ["Mom", "Dad", "Alice", "Bob"]
print(contacts[2])  # Gets "Alice" instantly
```
**Why it's fast:** Computer knows exactly where slot 2 is, like knowing your locker number

#### **What "Searching Takes Time" Means:**
```python
# To find "Alice", computer might check:
# contacts[0] = "Mom"     ❌ Not Alice
# contacts[1] = "Dad"     ❌ Not Alice  
# contacts[2] = "Alice"   ✅ Found her!
```
**Why it's slow:** Computer has to check each slot until it finds what you want

#### **What "Adding/Removing is Expensive" Means:**
```python
# To insert "Eve" at position 1:
# Before: ["Mom", "Dad", "Alice", "Bob"]
# After:  ["Mom", "Eve", "Dad", "Alice", "Bob"]
```
**Why it's expensive:** Everything after position 1 has to shift over (like making space in a crowded elevator)

### **Strings in Simple Terms**

#### **Characters vs Strings:**
```python
char = 'A'        # Single character
string = "Apple"  # Multiple characters together
```

#### **Accessing Parts of Strings:**
```python
word = "PROGRAMMING"
print(word[0])     # 'P' (first letter)
print(word[5])     # 'A' (6th letter, remember we start counting at 0)
print(word[0:4])   # 'PROG' (first 4 letters)
```

#### **Common String Operations (Like Text Editing):**
```python
message = "Hello World"
print(message.upper())          # "HELLO WORLD" (like caps lock)
print(message.lower())          # "hello world" (like turning off caps)
print(message.replace('o', '0')) # "Hell0 W0rld" (find and replace)
print(message.split())          # ["Hello", "World"] (split into words)
```

## 🔧 **Common Operations & Patterns**

### **1. Array Traversal**

```python
# Forward traversal
for i in range(len(arr)):
    print(arr[i])

# Backward traversal
for i in range(len(arr)-1, -1, -1):
    print(arr[i])

# Using indices and values
for index, value in enumerate(arr):
    print(f"Index: {index}, Value: {value}")
```

### **2. String Operations**

```python
# Common string operations
s = "hello world"
print(s.upper())        # HELLO WORLD
print(s.lower())        # hello world
print(s.split())        # ['hello', 'world']
print(s.replace('l', 'x'))  # hexxo worxd
print(s[:5])            # hello (substring)
```

## 🧩 **Essential Patterns**

### **Pattern 1: Frequency Counter**

```python
def character_frequency(s):
    """Count frequency of each character in string"""
    freq = {}
    for char in s:
        freq[char] = freq.get(char, 0) + 1
    return freq

# Example: "hello" -> {'h': 1, 'e': 1, 'l': 2, 'o': 1}
```

### **Pattern 2: Reverse Array/String**

```python
def reverse_array(arr):
    """Reverse array in-place using two pointers"""
    left, right = 0, len(arr) - 1

    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1

    return arr

def reverse_string(s):
    """Reverse string (convert to list first if immutable)"""
    chars = list(s)
    left, right = 0, len(chars) - 1

    while left < right:
        chars[left], chars[right] = chars[right], chars[left]
        left += 1
        right -= 1

    return ''.join(chars)
```

### **Pattern 3: Palindrome Check**

```python
def is_palindrome(s):
    """Check if string is palindrome using two pointers"""
    # Clean string: remove non-alphanumeric, convert to lowercase
    clean_s = ''.join(char.lower() for char in s if char.isalnum())

    left, right = 0, len(clean_s) - 1

    while left < right:
        if clean_s[left] != clean_s[right]:
            return False
        left += 1
        right -= 1

    return True
```

### **Pattern 4: Array Rotation**

```python
def rotate_array_right(arr, k):
    """Rotate array to right by k positions"""
    n = len(arr)
    k = k % n  # Handle k > n

    # Method 1: Using extra space
    return arr[-k:] + arr[:-k]

def rotate_array_inplace(arr, k):
    """Rotate array in-place using reversal algorithm"""
    n = len(arr)
    k = k % n

    # Reverse entire array
    reverse_subarray(arr, 0, n-1)
    # Reverse first k elements
    reverse_subarray(arr, 0, k-1)
    # Reverse remaining elements
    reverse_subarray(arr, k, n-1)

    return arr

def reverse_subarray(arr, start, end):
    while start < end:
        arr[start], arr[end] = arr[end], arr[start]
        start += 1
        end -= 1
```

## 📊 **Time & Space Complexity Analysis**

| Operation       | Array | String | Space |
| --------------- | ----- | ------ | ----- |
| Access          | O(1)  | O(1)   | O(1)  |
| Search          | O(n)  | O(n)   | O(1)  |
| Insert          | O(n)  | O(n)   | O(1)  |
| Delete          | O(n)  | O(n)   | O(1)  |
| Reverse         | O(n)  | O(n)   | O(1)  |
| Frequency Count | O(n)  | O(n)   | O(k)  |

## 🎮 **Practice Problems**

### **Easy Level**

1. **Two Sum** - Find indices of two numbers that add up to target
2. **Valid Palindrome** - Check if string is palindrome
3. **Reverse String** - Reverse characters in string
4. **Remove Duplicates** - Remove duplicates from sorted array
5. **Rotate Array** - Rotate array by k steps

### **Problem Solutions**

#### **Two Sum Problem**

```python
def two_sum(nums, target):
    """Find indices of two numbers that add up to target"""
    # Using hash map for O(n) solution
    num_map = {}

    for i, num in enumerate(nums):
        complement = target - num
        if complement in num_map:
            return [num_map[complement], i]
        num_map[num] = i

    return []  # No solution found

# Time: O(n), Space: O(n)
```

#### **Remove Duplicates from Sorted Array**

```python
def remove_duplicates(nums):
    """Remove duplicates in-place, return new length"""
    if not nums:
        return 0

    write_index = 1  # Position to write next unique element

    for i in range(1, len(nums)):
        if nums[i] != nums[i-1]:  # Found unique element
            nums[write_index] = nums[i]
            write_index += 1

    return write_index

# Time: O(n), Space: O(1)
```

## 🎨 **Visual Examples**

### **Array Rotation Visualization**

```
Original: [1, 2, 3, 4, 5, 6, 7]
Rotate right by 3:

Step 1: Reverse entire array
[7, 6, 5, 4, 3, 2, 1]

Step 2: Reverse first 3 elements
[5, 6, 7, 4, 3, 2, 1]

Step 3: Reverse remaining elements
[5, 6, 7, 1, 2, 3, 4]

Result: [5, 6, 7, 1, 2, 3, 4]
```

### **Two Pointers Technique**

```
Palindrome Check: "racecar"
left = 0, right = 6

r a c e c a r
↑           ↑
left      right

Compare: r == r ✓
Move pointers inward...

r a c e c a r
  ↑       ↑
  left  right

Continue until pointers meet
```

## 💡 **Key Tips**

1. **Index Management**: Always check array bounds (0 to n-1)
2. **String Immutability**: Convert to list for modifications in Python
3. **Edge Cases**: Empty arrays/strings, single element, all same elements
4. **Two Pointers**: Powerful technique for many array/string problems
5. **Hash Maps**: Use for O(1) lookups and frequency counting

## 🔄 **Common Patterns Summary**

- **Frequency Counting**: Use hash maps to count occurrences
- **Two Pointers**: For palindromes, reversing, finding pairs
- **Sliding Window**: For subarray/substring problems (covered next)
- **Prefix Sums**: For range queries and subarray sums
- **Sorting**: Often simplifies many problems

## 📈 **Next Steps**

Once you master arrays and strings, move on to:

- [Two Pointers Technique](./02-two-pointers.md) - Building on array fundamentals
- [Sliding Window](./03-sliding-window.md) - Advanced array patterns
- [Hash Maps & Sets](./04-hashmaps-sets.md) - Efficient data lookup

---

_Practice these fundamentals until they become second nature before moving to more complex topics!_
