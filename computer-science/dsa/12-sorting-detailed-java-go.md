# 🔢 **SORTING ALGORITHMS: DETAILED GUIDE (JAVA & GO)**

**Master All Sorting Techniques - From Basic to Advanced**

*Prerequisites: Complete arrays, recursion, and basic algorithms first*

---

## 🤔 **WHAT IS SORTING?**

### **Simple Explanation:**
Sorting is like **organizing your bookshelf**:
- **Input:** Books scattered randomly
- **Process:** Arrange by some criteria (alphabetical, height, genre)
- **Output:** Books in organized order
- **Goal:** Make finding and accessing books easier

### **Real-World Examples:**

**📱 Contact List:**
- **Before:** John, Alice, Bob, Zoe
- **After:** Alice, Bob, John, Zoe
- **Benefit:** Quick lookup, easier navigation

**🏪 E-commerce:**
- **Sort by:** Price (low to high), Rating (high to low), Date (newest first)
- **Benefit:** Customers find what they want faster

**📊 Data Analysis:**
- **Sort by:** Revenue, Date, Performance metrics
- **Benefit:** Identify patterns, trends, outliers

### **Why Learn Sorting:**
- **Foundation algorithm** - Appears everywhere
- **Interview essential** - 25% of coding interviews
- **Performance critical** - O(n²) vs O(n log n) matters
- **Real applications** - Databases, search, graphics
- **Algorithm analysis** - Great for understanding complexity

---

## 🎯 **SORTING ALGORITHM CATEGORIES**

### **By Time Complexity:**
- **O(n²)** - Bubble, Selection, Insertion (simple but slow)
- **O(n log n)** - Merge, Heap, Quick (efficient for large data)
- **O(n)** - Counting, Radix, Bucket (special cases)

### **By Memory Usage:**
- **In-place** - Uses O(1) extra space
- **Out-of-place** - Uses O(n) extra space

### **By Stability:**
- **Stable** - Equal elements maintain relative order
- **Unstable** - Equal elements may change relative order

### **By Approach:**
- **Comparison-based** - Compare elements to determine order
- **Non-comparison** - Use element properties (counting, radix)

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Simple Sorting Algorithms (O(n²))**

```java
import java.util.*;

public class SimpleSortingAlgorithms {
    
    /**
     * Bubble Sort - Repeatedly swap adjacent elements if in wrong order
     * Pattern: Largest element "bubbles" to the end in each pass
     * Time: O(n²), Space: O(1), Stable: Yes
     */
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            
            // Last i elements are already sorted
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap adjacent elements
                    swap(arr, j, j + 1);
                    swapped = true;
                }
            }
            
            // If no swapping occurred, array is sorted
            if (!swapped) {
                break;
            }
        }
    }
    
    /**
     * Selection Sort - Find minimum and place it at beginning
     * Pattern: Select the smallest, then second smallest, etc.
     * Time: O(n²), Space: O(1), Stable: No
     */
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;
            
            // Find minimum element in remaining array
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }
            
            // Swap minimum with first element
            swap(arr, i, minIndex);
        }
    }
    
    /**
     * Insertion Sort - Insert each element into its correct position
     * Pattern: Like sorting cards in your hand
     * Time: O(n²), Space: O(1), Stable: Yes
     */
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            
            // Move elements greater than key one position ahead
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            
            // Insert key at correct position
            arr[j + 1] = key;
        }
    }
    
    /**
     * Shell Sort - Improved insertion sort with gap sequence
     * Pattern: Sort elements at gap distance, reduce gap
     * Time: O(n log n) to O(n²), Space: O(1), Stable: No
     */
    public static void shellSort(int[] arr) {
        int n = arr.length;
        
        // Start with large gap, reduce it
        for (int gap = n / 2; gap > 0; gap /= 2) {
            // Perform insertion sort for this gap size
            for (int i = gap; i < n; i++) {
                int temp = arr[i];
                int j;
                
                for (j = i; j >= gap && arr[j - gap] > temp; j -= gap) {
                    arr[j] = arr[j - gap];
                }
                
                arr[j] = temp;
            }
        }
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    public static void main(String[] args) {
        int[] test1 = {64, 34, 25, 12, 22, 11, 90};
        int[] test2 = test1.clone();
        int[] test3 = test1.clone();
        int[] test4 = test1.clone();
        
        System.out.println("Original: " + Arrays.toString(test1));
        
        bubbleSort(test1);
        System.out.println("Bubble Sort: " + Arrays.toString(test1));
        
        selectionSort(test2);
        System.out.println("Selection Sort: " + Arrays.toString(test2));
        
        insertionSort(test3);
        System.out.println("Insertion Sort: " + Arrays.toString(test3));
        
        shellSort(test4);
        System.out.println("Shell Sort: " + Arrays.toString(test4));
    }
}
```

### **Efficient Sorting Algorithms (O(n log n))**

```java
import java.util.*;

public class EfficientSortingAlgorithms {
    
    /**
     * Merge Sort - Divide array, sort halves, merge them
     * Pattern: Divide and conquer approach
     * Time: O(n log n), Space: O(n), Stable: Yes
     */
    public static void mergeSort(int[] arr) {
        if (arr.length <= 1) return;
        
        mergeSortHelper(arr, 0, arr.length - 1);
    }
    
    private static void mergeSortHelper(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;
            
            // Sort left and right halves
            mergeSortHelper(arr, left, mid);
            mergeSortHelper(arr, mid + 1, right);
            
            // Merge sorted halves
            merge(arr, left, mid, right);
        }
    }
    
    private static void merge(int[] arr, int left, int mid, int right) {
        // Create temporary arrays
        int[] leftArr = Arrays.copyOfRange(arr, left, mid + 1);
        int[] rightArr = Arrays.copyOfRange(arr, mid + 1, right + 1);
        
        int i = 0, j = 0, k = left;
        
        // Merge the two arrays
        while (i < leftArr.length && j < rightArr.length) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k++] = leftArr[i++];
            } else {
                arr[k++] = rightArr[j++];
            }
        }
        
        // Copy remaining elements
        while (i < leftArr.length) {
            arr[k++] = leftArr[i++];
        }
        while (j < rightArr.length) {
            arr[k++] = rightArr[j++];
        }
    }
    
    /**
     * Quick Sort - Choose pivot, partition, recursively sort
     * Pattern: Divide and conquer with in-place partitioning
     * Time: O(n log n) average, O(n²) worst, Space: O(log n), Stable: No
     */
    public static void quickSort(int[] arr) {
        quickSortHelper(arr, 0, arr.length - 1);
    }
    
    private static void quickSortHelper(int[] arr, int low, int high) {
        if (low < high) {
            // Partition array and get pivot index
            int pivotIndex = partition(arr, low, high);
            
            // Recursively sort elements before and after partition
            quickSortHelper(arr, low, pivotIndex - 1);
            quickSortHelper(arr, pivotIndex + 1, high);
        }
    }
    
    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // Choose last element as pivot
        int i = low - 1; // Index of smaller element
        
        for (int j = low; j < high; j++) {
            if (arr[j] <= pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        
        swap(arr, i + 1, high);
        return i + 1;
    }
    
    /**
     * Heap Sort - Build max heap, repeatedly extract maximum
     * Pattern: Use heap data structure for sorting
     * Time: O(n log n), Space: O(1), Stable: No
     */
    public static void heapSort(int[] arr) {
        int n = arr.length;
        
        // Build max heap
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }
        
        // Extract elements from heap one by one
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            swap(arr, 0, i);
            
            // Call heapify on reduced heap
            heapify(arr, i, 0);
        }
    }
    
    private static void heapify(int[] arr, int n, int i) {
        int largest = i; // Initialize largest as root
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        // If left child is larger than root
        if (left < n && arr[left] > arr[largest]) {
            largest = left;
        }
        
        // If right child is larger than largest so far
        if (right < n && arr[right] > arr[largest]) {
            largest = right;
        }
        
        // If largest is not root
        if (largest != i) {
            swap(arr, i, largest);
            
            // Recursively heapify the affected sub-tree
            heapify(arr, n, largest);
        }
    }
    
    /**
     * Randomized Quick Sort - Use random pivot for better average case
     * Pattern: Randomization to avoid worst-case scenarios
     * Time: O(n log n) expected, Space: O(log n), Stable: No
     */
    public static void randomizedQuickSort(int[] arr) {
        randomizedQuickSortHelper(arr, 0, arr.length - 1);
    }
    
    private static void randomizedQuickSortHelper(int[] arr, int low, int high) {
        if (low < high) {
            // Randomly choose pivot
            Random rand = new Random();
            int randomIndex = low + rand.nextInt(high - low + 1);
            swap(arr, randomIndex, high);
            
            int pivotIndex = partition(arr, low, high);
            
            randomizedQuickSortHelper(arr, low, pivotIndex - 1);
            randomizedQuickSortHelper(arr, pivotIndex + 1, high);
        }
    }
    
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    public static void main(String[] args) {
        int[] test1 = {64, 34, 25, 12, 22, 11, 90};
        int[] test2 = test1.clone();
        int[] test3 = test1.clone();
        int[] test4 = test1.clone();
        
        System.out.println("Original: " + Arrays.toString(test1));
        
        mergeSort(test1);
        System.out.println("Merge Sort: " + Arrays.toString(test1));
        
        quickSort(test2);
        System.out.println("Quick Sort: " + Arrays.toString(test2));
        
        heapSort(test3);
        System.out.println("Heap Sort: " + Arrays.toString(test3));
        
        randomizedQuickSort(test4);
        System.out.println("Randomized Quick Sort: " + Arrays.toString(test4));
    }
}
```

### **Non-Comparison Sorting Algorithms**

```java
import java.util.*;

public class NonComparisonSorting {
    
    /**
     * Counting Sort - Count occurrences of each element
     * Pattern: Use element values as indices
     * Time: O(n + k), Space: O(k), Stable: Yes
     * Where k is the range of input
     */
    public static int[] countingSort(int[] arr, int maxValue) {
        int[] count = new int[maxValue + 1];
        int[] result = new int[arr.length];
        
        // Count occurrences of each element
        for (int num : arr) {
            count[num]++;
        }
        
        // Transform count array to store actual positions
        for (int i = 1; i <= maxValue; i++) {
            count[i] += count[i - 1];
        }
        
        // Build result array
        for (int i = arr.length - 1; i >= 0; i--) {
            result[count[arr[i]] - 1] = arr[i];
            count[arr[i]]--;
        }
        
        return result;
    }
    
    /**
     * Radix Sort - Sort by each digit position
     * Pattern: Use counting sort for each digit
     * Time: O(d * (n + k)), Space: O(n + k), Stable: Yes
     * Where d is number of digits
     */
    public static void radixSort(int[] arr) {
        // Find maximum number to know number of digits
        int max = Arrays.stream(arr).max().orElse(0);
        
        // Do counting sort for every digit
        for (int exp = 1; max / exp > 0; exp *= 10) {
            countingSortByDigit(arr, exp);
        }
    }
    
    private static void countingSortByDigit(int[] arr, int exp) {
        int n = arr.length;
        int[] result = new int[n];
        int[] count = new int[10]; // Digits 0-9
        
        // Count occurrences of each digit
        for (int i = 0; i < n; i++) {
            count[(arr[i] / exp) % 10]++;
        }
        
        // Transform count array
        for (int i = 1; i < 10; i++) {
            count[i] += count[i - 1];
        }
        
        // Build result array
        for (int i = n - 1; i >= 0; i--) {
            int digit = (arr[i] / exp) % 10;
            result[count[digit] - 1] = arr[i];
            count[digit]--;
        }
        
        // Copy result back to original array
        System.arraycopy(result, 0, arr, 0, n);
    }
    
    /**
     * Bucket Sort - Distribute elements into buckets, sort each bucket
     * Pattern: Divide range into buckets, sort individually
     * Time: O(n + k) average, Space: O(n), Stable: Yes
     */
    public static void bucketSort(float[] arr) {
        int n = arr.length;
        if (n <= 0) return;
        
        // Create empty buckets
        List<List<Float>> buckets = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            buckets.add(new ArrayList<>());
        }
        
        // Put array elements in different buckets
        for (int i = 0; i < n; i++) {
            int bucketIndex = (int) (arr[i] * n);
            buckets.get(bucketIndex).add(arr[i]);
        }
        
        // Sort individual buckets
        for (List<Float> bucket : buckets) {
            Collections.sort(bucket);
        }
        
        // Concatenate all buckets into arr[]
        int index = 0;
        for (List<Float> bucket : buckets) {
            for (float value : bucket) {
                arr[index++] = value;
            }
        }
    }
    
    public static void main(String[] args) {
        // Test counting sort
        int[] arr1 = {4, 2, 2, 8, 3, 3, 1};
        int[] sorted1 = countingSort(arr1, 8);
        System.out.println("Counting Sort: " + Arrays.toString(sorted1));
        
        // Test radix sort
        int[] arr2 = {170, 45, 75, 90, 2, 802, 24, 66};
        radixSort(arr2);
        System.out.println("Radix Sort: " + Arrays.toString(arr2));
        
        // Test bucket sort
        float[] arr3 = {0.897f, 0.565f, 0.656f, 0.1234f, 0.665f, 0.3434f};
        bucketSort(arr3);
        System.out.println("Bucket Sort: " + Arrays.toString(arr3));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Simple Sorting Algorithms in Go**

```go
package main

import "fmt"

/**
 * Bubble Sort - Repeatedly swap adjacent elements if in wrong order
 * Pattern: Largest element "bubbles" to the end in each pass
 * Time: O(n²), Space: O(1), Stable: Yes
 */
func bubbleSort(arr []int) {
    n := len(arr)
    
    for i := 0; i < n-1; i++ {
        swapped := false
        
        // Last i elements are already sorted
        for j := 0; j < n-i-1; j++ {
            if arr[j] > arr[j+1] {
                // Swap adjacent elements
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = true
            }
        }
        
        // If no swapping occurred, array is sorted
        if !swapped {
            break
        }
    }
}

/**
 * Selection Sort - Find minimum and place it at beginning
 * Pattern: Select the smallest, then second smallest, etc.
 * Time: O(n²), Space: O(1), Stable: No
 */
func selectionSort(arr []int) {
    n := len(arr)
    
    for i := 0; i < n-1; i++ {
        minIndex := i
        
        // Find minimum element in remaining array
        for j := i + 1; j < n; j++ {
            if arr[j] < arr[minIndex] {
                minIndex = j
            }
        }
        
        // Swap minimum with first element
        arr[i], arr[minIndex] = arr[minIndex], arr[i]
    }
}

/**
 * Insertion Sort - Insert each element into its correct position
 * Pattern: Like sorting cards in your hand
 * Time: O(n²), Space: O(1), Stable: Yes
 */
func insertionSort(arr []int) {
    n := len(arr)
    
    for i := 1; i < n; i++ {
        key := arr[i]
        j := i - 1
        
        // Move elements greater than key one position ahead
        for j >= 0 && arr[j] > key {
            arr[j+1] = arr[j]
            j--
        }
        
        // Insert key at correct position
        arr[j+1] = key
    }
}

/**
 * Shell Sort - Improved insertion sort with gap sequence
 * Pattern: Sort elements at gap distance, reduce gap
 * Time: O(n log n) to O(n²), Space: O(1), Stable: No
 */
func shellSort(arr []int) {
    n := len(arr)
    
    // Start with large gap, reduce it
    for gap := n / 2; gap > 0; gap /= 2 {
        // Perform insertion sort for this gap size
        for i := gap; i < n; i++ {
            temp := arr[i]
            j := i
            
            for j >= gap && arr[j-gap] > temp {
                arr[j] = arr[j-gap]
                j -= gap
            }
            
            arr[j] = temp
        }
    }
}

func main() {
    test1 := []int{64, 34, 25, 12, 22, 11, 90}
    test2 := make([]int, len(test1))
    test3 := make([]int, len(test1))
    test4 := make([]int, len(test1))
    
    copy(test2, test1)
    copy(test3, test1)
    copy(test4, test1)
    
    fmt.Printf("Original: %v\n", test1)
    
    bubbleSort(test1)
    fmt.Printf("Bubble Sort: %v\n", test1)
    
    selectionSort(test2)
    fmt.Printf("Selection Sort: %v\n", test2)
    
    insertionSort(test3)
    fmt.Printf("Insertion Sort: %v\n", test3)
    
    shellSort(test4)
    fmt.Printf("Shell Sort: %v\n", test4)
}
```

### **Efficient Sorting Algorithms in Go**

```go
package main

import (
    "fmt"
    "math/rand"
    "time"
)

/**
 * Merge Sort - Divide array, sort halves, merge them
 * Pattern: Divide and conquer approach
 * Time: O(n log n), Space: O(n), Stable: Yes
 */
func mergeSort(arr []int) {
    if len(arr) <= 1 {
        return
    }
    
    mergeSortHelper(arr, 0, len(arr)-1)
}

func mergeSortHelper(arr []int, left, right int) {
    if left < right {
        mid := left + (right-left)/2
        
        // Sort left and right halves
        mergeSortHelper(arr, left, mid)
        mergeSortHelper(arr, mid+1, right)
        
        // Merge sorted halves
        merge(arr, left, mid, right)
    }
}

func merge(arr []int, left, mid, right int) {
    // Create temporary arrays
    leftArr := make([]int, mid-left+1)
    rightArr := make([]int, right-mid)
    
    copy(leftArr, arr[left:mid+1])
    copy(rightArr, arr[mid+1:right+1])
    
    i, j, k := 0, 0, left
    
    // Merge the two arrays
    for i < len(leftArr) && j < len(rightArr) {
        if leftArr[i] <= rightArr[j] {
            arr[k] = leftArr[i]
            i++
        } else {
            arr[k] = rightArr[j]
            j++
        }
        k++
    }
    
    // Copy remaining elements
    for i < len(leftArr) {
        arr[k] = leftArr[i]
        i++
        k++
    }
    for j < len(rightArr) {
        arr[k] = rightArr[j]
        j++
        k++
    }
}

/**
 * Quick Sort - Choose pivot, partition, recursively sort
 * Pattern: Divide and conquer with in-place partitioning
 * Time: O(n log n) average, O(n²) worst, Space: O(log n), Stable: No
 */
func quickSort(arr []int) {
    quickSortHelper(arr, 0, len(arr)-1)
}

func quickSortHelper(arr []int, low, high int) {
    if low < high {
        // Partition array and get pivot index
        pivotIndex := partition(arr, low, high)
        
        // Recursively sort elements before and after partition
        quickSortHelper(arr, low, pivotIndex-1)
        quickSortHelper(arr, pivotIndex+1, high)
    }
}

func partition(arr []int, low, high int) int {
    pivot := arr[high] // Choose last element as pivot
    i := low - 1       // Index of smaller element
    
    for j := low; j < high; j++ {
        if arr[j] <= pivot {
            i++
            arr[i], arr[j] = arr[j], arr[i]
        }
    }
    
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1
}

/**
 * Heap Sort - Build max heap, repeatedly extract maximum
 * Pattern: Use heap data structure for sorting
 * Time: O(n log n), Space: O(1), Stable: No
 */
func heapSort(arr []int) {
    n := len(arr)
    
    // Build max heap
    for i := n/2 - 1; i >= 0; i-- {
        heapify(arr, n, i)
    }
    
    // Extract elements from heap one by one
    for i := n - 1; i > 0; i-- {
        // Move current root to end
        arr[0], arr[i] = arr[i], arr[0]
        
        // Call heapify on reduced heap
        heapify(arr, i, 0)
    }
}

func heapify(arr []int, n, i int) {
    largest := i // Initialize largest as root
    left := 2*i + 1
    right := 2*i + 2
    
    // If left child is larger than root
    if left < n && arr[left] > arr[largest] {
        largest = left
    }
    
    // If right child is larger than largest so far
    if right < n && arr[right] > arr[largest] {
        largest = right
    }
    
    // If largest is not root
    if largest != i {
        arr[i], arr[largest] = arr[largest], arr[i]
        
        // Recursively heapify the affected sub-tree
        heapify(arr, n, largest)
    }
}

/**
 * Randomized Quick Sort - Use random pivot for better average case
 * Pattern: Randomization to avoid worst-case scenarios
 * Time: O(n log n) expected, Space: O(log n), Stable: No
 */
func randomizedQuickSort(arr []int) {
    rand.Seed(time.Now().UnixNano())
    randomizedQuickSortHelper(arr, 0, len(arr)-1)
}

func randomizedQuickSortHelper(arr []int, low, high int) {
    if low < high {
        // Randomly choose pivot
        randomIndex := low + rand.Intn(high-low+1)
        arr[randomIndex], arr[high] = arr[high], arr[randomIndex]
        
        pivotIndex := partition(arr, low, high)
        
        randomizedQuickSortHelper(arr, low, pivotIndex-1)
        randomizedQuickSortHelper(arr, pivotIndex+1, high)
    }
}

func main() {
    test1 := []int{64, 34, 25, 12, 22, 11, 90}
    test2 := make([]int, len(test1))
    test3 := make([]int, len(test1))
    test4 := make([]int, len(test1))
    
    copy(test2, test1)
    copy(test3, test1)
    copy(test4, test1)
    
    fmt.Printf("Original: %v\n", test1)
    
    mergeSort(test1)
    fmt.Printf("Merge Sort: %v\n", test1)
    
    quickSort(test2)
    fmt.Printf("Quick Sort: %v\n", test2)
    
    heapSort(test3)
    fmt.Printf("Heap Sort: %v\n", test3)
    
    randomizedQuickSort(test4)
    fmt.Printf("Randomized Quick Sort: %v\n", test4)
}
```

### **Non-Comparison Sorting in Go**

```go
package main

import (
    "fmt"
    "sort"
)

/**
 * Counting Sort - Count occurrences of each element
 * Pattern: Use element values as indices
 * Time: O(n + k), Space: O(k), Stable: Yes
 */
func countingSort(arr []int, maxValue int) []int {
    count := make([]int, maxValue+1)
    result := make([]int, len(arr))
    
    // Count occurrences of each element
    for _, num := range arr {
        count[num]++
    }
    
    // Transform count array to store actual positions
    for i := 1; i <= maxValue; i++ {
        count[i] += count[i-1]
    }
    
    // Build result array
    for i := len(arr) - 1; i >= 0; i-- {
        result[count[arr[i]]-1] = arr[i]
        count[arr[i]]--
    }
    
    return result
}

/**
 * Radix Sort - Sort by each digit position
 * Pattern: Use counting sort for each digit
 * Time: O(d * (n + k)), Space: O(n + k), Stable: Yes
 */
func radixSort(arr []int) {
    // Find maximum number to know number of digits
    max := findMax(arr)
    
    // Do counting sort for every digit
    for exp := 1; max/exp > 0; exp *= 10 {
        countingSortByDigit(arr, exp)
    }
}

func countingSortByDigit(arr []int, exp int) {
    n := len(arr)
    result := make([]int, n)
    count := make([]int, 10) // Digits 0-9
    
    // Count occurrences of each digit
    for i := 0; i < n; i++ {
        count[(arr[i]/exp)%10]++
    }
    
    // Transform count array
    for i := 1; i < 10; i++ {
        count[i] += count[i-1]
    }
    
    // Build result array
    for i := n - 1; i >= 0; i-- {
        digit := (arr[i] / exp) % 10
        result[count[digit]-1] = arr[i]
        count[digit]--
    }
    
    // Copy result back to original array
    copy(arr, result)
}

func findMax(arr []int) int {
    max := arr[0]
    for _, num := range arr[1:] {
        if num > max {
            max = num
        }
    }
    return max
}

/**
 * Bucket Sort - Distribute elements into buckets, sort each bucket
 * Pattern: Divide range into buckets, sort individually
 * Time: O(n + k) average, Space: O(n), Stable: Yes
 */
func bucketSort(arr []float64) {
    n := len(arr)
    if n <= 0 {
        return
    }
    
    // Create empty buckets
    buckets := make([][]float64, n)
    for i := range buckets {
        buckets[i] = make([]float64, 0)
    }
    
    // Put array elements in different buckets
    for i := 0; i < n; i++ {
        bucketIndex := int(arr[i] * float64(n))
        if bucketIndex >= n {
            bucketIndex = n - 1
        }
        buckets[bucketIndex] = append(buckets[bucketIndex], arr[i])
    }
    
    // Sort individual buckets
    for i := range buckets {
        sort.Float64s(buckets[i])
    }
    
    // Concatenate all buckets into arr[]
    index := 0
    for _, bucket := range buckets {
        for _, value := range bucket {
            arr[index] = value
            index++
        }
    }
}

func main() {
    // Test counting sort
    arr1 := []int{4, 2, 2, 8, 3, 3, 1}
    sorted1 := countingSort(arr1, 8)
    fmt.Printf("Counting Sort: %v\n", sorted1)
    
    // Test radix sort
    arr2 := []int{170, 45, 75, 90, 2, 802, 24, 66}
    radixSort(arr2)
    fmt.Printf("Radix Sort: %v\n", arr2)
    
    // Test bucket sort
    arr3 := []float64{0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434}
    bucketSort(arr3)
    fmt.Printf("Bucket Sort: %v\n", arr3)
}
```

---

## 📊 **SORTING ALGORITHM COMPARISON**

| Algorithm | Time (Best) | Time (Average) | Time (Worst) | Space | Stable | In-Place |
|-----------|-------------|----------------|--------------|-------|--------|----------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | ❌ | ✅ |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| **Shell Sort** | O(n log n) | O(n^1.5) | O(n²) | O(1) | ❌ | ✅ |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | ✅ |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ | ✅ |
| **Counting Sort** | O(n + k) | O(n + k) | O(n + k) | O(k) | ✅ | ❌ |
| **Radix Sort** | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | ✅ | ❌ |
| **Bucket Sort** | O(n + k) | O(n + k) | O(n²) | O(n) | ✅ | ❌ |

### **When to Use Each Algorithm:**

**🔵 Small Arrays (n < 50):**
- **Insertion Sort** - Simple, efficient for small data, adaptive

**🟢 Large Arrays (General Purpose):**
- **Quick Sort** - Fast average case, in-place
- **Merge Sort** - Stable, predictable performance
- **Heap Sort** - Guaranteed O(n log n), in-place

**🟡 Special Cases:**
- **Counting Sort** - Small range of integers
- **Radix Sort** - Fixed-width integers/strings
- **Bucket Sort** - Uniformly distributed data

**🔴 Educational Only:**
- **Bubble Sort** - Demonstrates basic sorting
- **Selection Sort** - Simple but inefficient

---

## 🧠 **UNDERSTANDING SORTING PATTERNS**

### **Divide and Conquer Pattern:**
```java
void sort(array, start, end) {
    if (start >= end) return; // Base case
    
    int mid = divide(array, start, end);
    sort(array, start, mid);     // Conquer left
    sort(array, mid + 1, end);   // Conquer right
    combine(array, start, mid, end); // Combine results
}
```
**Examples:** Merge Sort, Quick Sort

### **Incremental Construction Pattern:**
```java
void sort(array) {
    for (int i = 1; i < array.length; i++) {
        // Insert array[i] into sorted portion array[0...i-1]
        insert(array, i);
    }
}
```
**Examples:** Insertion Sort, Selection Sort

### **Transform-and-Sort Pattern:**
```java
void sort(array) {
    // Transform elements to make sorting easier
    transform(array);
    
    // Use simple sorting on transformed data
    simpleSortMethod(array);
    
    // Transform back if needed
    reverseTransform(array);
}
```
**Examples:** Counting Sort, Radix Sort

---

## 🎯 **OPTIMIZATION TECHNIQUES**

### **1. Hybrid Algorithms:**
```java
void efficientSort(int[] arr, int start, int end) {
    if (end - start <= THRESHOLD) {
        insertionSort(arr, start, end); // Small arrays
    } else {
        quickSort(arr, start, end);     // Large arrays
    }
}
```

### **2. Three-Way Partitioning (Dutch Flag):**
```java
// For arrays with many duplicates
void quickSort3Way(int[] arr, int low, int high) {
    if (high <= low) return;
    
    int lt = low, gt = high;
    int pivot = arr[low];
    int i = low;
    
    while (i <= gt) {
        if (arr[i] < pivot) swap(arr, lt++, i++);
        else if (arr[i] > pivot) swap(arr, i, gt--);
        else i++;
    }
    
    quickSort3Way(arr, low, lt - 1);
    quickSort3Way(arr, gt + 1, high);
}
```

### **3. Tail Recursion Optimization:**
```java
void quickSortIterative(int[] arr, int low, int high) {
    Stack<Integer> stack = new Stack<>();
    stack.push(low);
    stack.push(high);
    
    while (!stack.isEmpty()) {
        high = stack.pop();
        low = stack.pop();
        
        if (low < high) {
            int pivot = partition(arr, low, high);
            stack.push(low);
            stack.push(pivot - 1);
            stack.push(pivot + 1);
            stack.push(high);
        }
    }
}
```

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Sort an Array** - Implement any sorting algorithm
2. **Sort Colors** - Dutch National Flag problem
3. **Merge Sorted Array** - Merge two sorted arrays
4. **Sort Array by Parity** - Even numbers first
5. **Largest Number** - Custom comparator sorting

### **Medium Problems:**

1. **Kth Largest Element** - Use Quick Select
2. **Top K Frequent Elements** - Bucket sort approach
3. **Sort Characters by Frequency** - Counting + sorting
4. **Wiggle Sort** - Sort with specific pattern
5. **Pancake Sorting** - Only flip operations allowed

### **Hard Problems:**

1. **Count of Smaller Numbers After Self** - Merge sort with inversions
2. **Reverse Pairs** - Modified merge sort
3. **Maximum Gap** - Bucket sort application
4. **Count of Range Sum** - Merge sort with prefix sums
5. **Russian Doll Envelopes** - LIS with sorting

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Simple Sorts**
- [ ] Implement bubble, selection, insertion sort from scratch
- [ ] Understand when each performs well
- [ ] Practice analyzing time/space complexity
- [ ] Test with different input sizes

### **Day 2: Divide and Conquer**
- [ ] Implement merge sort step by step
- [ ] Master the merge operation
- [ ] Implement quick sort with different pivot strategies
- [ ] Understand recursion tree analysis

### **Day 3: Heap Sort and Advanced**
- [ ] Understand heap data structure
- [ ] Implement heapify and heap sort
- [ ] Practice shell sort and its gap sequences
- [ ] Compare performance of different algorithms

### **Day 4: Non-Comparison Sorts**
- [ ] Implement counting sort for integer arrays
- [ ] Master radix sort for multi-digit numbers
- [ ] Practice bucket sort for floating-point numbers
- [ ] Understand when each approach is optimal

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement 5+ sorting algorithms from memory
- [ ] You understand time/space complexity of each
- [ ] You can choose appropriate algorithm for given constraints
- [ ] You recognize sorting patterns in complex problems

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **[Greedy Algorithms - Detailed](./13-greedy-detailed-java-go.md)** - Learn optimization strategies
2. **[Graph Algorithms - Detailed](./15-graphs-detailed-java-go.md)** - Master graph traversal and algorithms
3. **[Advanced DP - Detailed](./14-dp-advanced-detailed-java-go.md)** - Complex optimization patterns

### **🎯 Advanced Sorting Topics:**
- **External Sorting** for data larger than memory
- **Parallel Sorting** algorithms for multi-core systems
- **Stable Marriage** and matching algorithms
- **Topological Sorting** for directed graphs

### **💡 Pro Tips:**
- **Know your data** - Choose algorithm based on input characteristics
- **Hybrid approaches** - Combine algorithms for optimal performance
- **Stability matters** - Consider whether equal elements need to maintain order
- **Memory constraints** - Balance time vs space complexity
- **Real-world optimizations** - Libraries often use sophisticated implementations

**🎉 You now master sorting algorithms! These fundamental techniques are the building blocks for many advanced algorithms and system optimizations.**