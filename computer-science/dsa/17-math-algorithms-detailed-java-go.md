# 🧮 **MATHEMATICAL ALGORITHMS: DETAILED GUIDE (JAVA & GO)**

**Master Number Theory, Computational Mathematics, and Algorithmic Problem Solving**

*Prerequisites: Complete basic algorithms, recursion, and number operations first*

---

## 🤔 **WHAT ARE MATHEMATICAL ALGORITHMS?**

### **Simple Explanation:**
Mathematical algorithms are like **smart calculators for complex problems**:
- **Number Theory** = Understanding properties of integers
- **Computational Math** = Efficient calculation techniques  
- **Problem Solving** = Converting math problems into code
- **Optimization** = Finding best solutions using mathematical insights

### **Real-World Applications:**

**🔐 Cryptography & Security:**
- **RSA Encryption** - Uses prime numbers and modular arithmetic
- **Hash Functions** - Mathematical operations for data integrity
- **Digital Signatures** - Number theory for authentication

**💰 Finance & Economics:**
- **Compound Interest** - Exponential growth calculations
- **Risk Analysis** - Statistical and probabilistic models
- **Trading Algorithms** - Mathematical pattern recognition

**🎮 Game Development:**
- **Physics Engines** - Mathematical simulations
- **Procedural Generation** - Random number algorithms
- **AI Behavior** - Mathematical decision making

**📊 Data Science & ML:**
- **Statistical Analysis** - Mathematical data processing
- **Optimization** - Finding best model parameters
- **Probability** - Uncertain event modeling

### **Why Master Mathematical Algorithms:**
- **Interview essentials** - 15% of coding interviews
- **Foundation knowledge** - Basis for advanced algorithms
- **Problem-solving skills** - Mathematical thinking helps everywhere
- **Performance optimization** - Often O(1) solutions to complex problems

---

## 🎯 **MATHEMATICAL ALGORITHM CATEGORIES**

### **Number Theory:**
- **Prime Numbers** - Generation, testing, factorization
- **GCD/LCM** - Greatest common divisor algorithms
- **Modular Arithmetic** - Operations with remainders
- **Chinese Remainder** - System of congruences

### **Combinatorics:**
- **Permutations** - Arrangements of objects
- **Combinations** - Selections of objects
- **Pascal's Triangle** - Binomial coefficients
- **Catalan Numbers** - Special counting sequences

### **Computational Geometry:**
- **Distance Calculations** - Points, lines, shapes
- **Convex Hull** - Smallest enclosing polygon
- **Line Intersections** - Geometric problem solving
- **Area Calculations** - Complex shape areas

### **Optimization:**
- **Linear Programming** - Constraint optimization
- **Binary Exponentiation** - Fast power calculations
- **Matrix Operations** - Efficient linear algebra
- **Numerical Methods** - Approximation algorithms

---

## ☕ **JAVA IMPLEMENTATIONS**

### **Prime Numbers and Number Theory**

```java
import java.util.*;

public class NumberTheory {
    
    /**
     * Check if number is prime - Optimized trial division
     * Pattern: Check divisors up to sqrt(n)
     * Time: O(√n), Space: O(1)
     */
    public static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        
        // Check for divisors of form 6k ± 1
        for (int i = 5; i * i <= n; i += 6) {
            if (n % i == 0 || n % (i + 2) == 0) {
                return false;
            }
        }
        
        return true;
    }
    
    /**
     * Sieve of Eratosthenes - Generate all primes up to n
     * Pattern: Mark multiples as composite
     * Time: O(n log log n), Space: O(n)
     */
    public static List<Integer> sieveOfEratosthenes(int n) {
        boolean[] isPrime = new boolean[n + 1];
        Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;
        
        for (int i = 2; i * i <= n; i++) {
            if (isPrime[i]) {
                // Mark multiples of i as composite
                for (int j = i * i; j <= n; j += i) {
                    isPrime[j] = false;
                }
            }
        }
        
        List<Integer> primes = new ArrayList<>();
        for (int i = 2; i <= n; i++) {
            if (isPrime[i]) {
                primes.add(i);
            }
        }
        
        return primes;
    }
    
    /**
     * Prime Factorization - Find all prime factors
     * Pattern: Divide by primes, collect factors
     * Time: O(√n), Space: O(log n)
     */
    public static List<Integer> primeFactors(int n) {
        List<Integer> factors = new ArrayList<>();
        
        // Handle factor 2
        while (n % 2 == 0) {
            factors.add(2);
            n /= 2;
        }
        
        // Handle odd factors
        for (int i = 3; i * i <= n; i += 2) {
            while (n % i == 0) {
                factors.add(i);
                n /= i;
            }
        }
        
        // If n is still > 2, it's a prime factor
        if (n > 2) {
            factors.add(n);
        }
        
        return factors;
    }
    
    /**
     * Greatest Common Divisor - Euclidean Algorithm
     * Pattern: Recursive remainder reduction
     * Time: O(log min(a,b)), Space: O(log min(a,b))
     */
    public static int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
    
    /**
     * Extended Euclidean Algorithm - Find GCD and coefficients
     * Pattern: Track coefficients through recursion
     * Time: O(log min(a,b)), Space: O(log min(a,b))
     */
    public static class ExtendedGCDResult {
        int gcd, x, y;
        
        ExtendedGCDResult(int gcd, int x, int y) {
            this.gcd = gcd;
            this.x = x;
            this.y = y;
        }
    }
    
    public static ExtendedGCDResult extendedGCD(int a, int b) {
        if (b == 0) {
            return new ExtendedGCDResult(a, 1, 0);
        }
        
        ExtendedGCDResult result = extendedGCD(b, a % b);
        int x = result.y;
        int y = result.x - (a / b) * result.y;
        
        return new ExtendedGCDResult(result.gcd, x, y);
    }
    
    /**
     * Least Common Multiple
     * Pattern: Use GCD relationship
     * Time: O(log min(a,b)), Space: O(log min(a,b))
     */
    public static long lcm(int a, int b) {
        return Math.abs((long) a * b) / gcd(a, b);
    }
    
    /**
     * Modular Exponentiation - Fast power with modulo
     * Pattern: Binary exponentiation with modular arithmetic
     * Time: O(log n), Space: O(1)
     */
    public static long modPow(long base, long exp, long mod) {
        long result = 1;
        base %= mod;
        
        while (exp > 0) {
            if (exp % 2 == 1) {
                result = (result * base) % mod;
            }
            exp >>= 1;
            base = (base * base) % mod;
        }
        
        return result;
    }
    
    /**
     * Modular Multiplicative Inverse
     * Pattern: Use extended Euclidean algorithm
     * Time: O(log m), Space: O(log m)
     */
    public static int modInverse(int a, int m) {
        ExtendedGCDResult result = extendedGCD(a, m);
        
        if (result.gcd != 1) {
            return -1; // Inverse doesn't exist
        }
        
        return (result.x % m + m) % m;
    }
    
    /**
     * Check if number is perfect power
     * Pattern: Check all possible exponents
     * Time: O(log³ n), Space: O(1)
     */
    public static boolean isPerfectPower(int n) {
        if (n <= 1) return true;
        
        for (int exp = 2; exp <= Math.log(n) / Math.log(2); exp++) {
            int root = (int) Math.round(Math.pow(n, 1.0 / exp));
            
            if (Math.pow(root, exp) == n) {
                return true;
            }
        }
        
        return false;
    }
    
    public static void main(String[] args) {
        // Test prime checking
        System.out.println("=== PRIME NUMBERS ===");
        System.out.println("Is 17 prime: " + isPrime(17));
        System.out.println("Is 25 prime: " + isPrime(25));
        
        // Test sieve
        List<Integer> primes = sieveOfEratosthenes(30);
        System.out.println("Primes up to 30: " + primes);
        
        // Test prime factorization
        System.out.println("Prime factors of 60: " + primeFactors(60));
        
        // Test GCD and LCM
        System.out.println("GCD(48, 18): " + gcd(48, 18));
        System.out.println("LCM(48, 18): " + lcm(48, 18));
        
        // Test modular exponentiation
        System.out.println("2^10 mod 1000: " + modPow(2, 10, 1000));
        
        // Test modular inverse
        System.out.println("Inverse of 3 mod 11: " + modInverse(3, 11));
        
        // Test perfect power
        System.out.println("Is 8 perfect power: " + isPerfectPower(8));
        System.out.println("Is 10 perfect power: " + isPerfectPower(10));
    }
}
```

### **Combinatorics and Counting**

```java
import java.util.*;

public class Combinatorics {
    
    /**
     * Calculate factorial
     * Pattern: Iterative multiplication
     * Time: O(n), Space: O(1)
     */
    public static long factorial(int n) {
        if (n < 0) return 0;
        if (n <= 1) return 1;
        
        long result = 1;
        for (int i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }
    
    /**
     * Calculate factorial with modulo to prevent overflow
     * Pattern: Modular arithmetic in iteration
     * Time: O(n), Space: O(1)
     */
    public static long factorialMod(int n, long mod) {
        long result = 1;
        for (int i = 2; i <= n; i++) {
            result = (result * i) % mod;
        }
        return result;
    }
    
    /**
     * Calculate nCr (combinations) - Choose r items from n
     * Pattern: Use factorial relationship with optimization
     * Time: O(min(r, n-r)), Space: O(1)
     */
    public static long combinations(int n, int r) {
        if (r > n || r < 0) return 0;
        if (r == 0 || r == n) return 1;
        
        // Optimize: C(n,r) = C(n,n-r)
        r = Math.min(r, n - r);
        
        long result = 1;
        for (int i = 0; i < r; i++) {
            result = result * (n - i) / (i + 1);
        }
        
        return result;
    }
    
    /**
     * Calculate nPr (permutations) - Arrange r items from n
     * Pattern: Factorial division
     * Time: O(r), Space: O(1)
     */
    public static long permutations(int n, int r) {
        if (r > n || r < 0) return 0;
        if (r == 0) return 1;
        
        long result = 1;
        for (int i = n; i > n - r; i--) {
            result *= i;
        }
        
        return result;
    }
    
    /**
     * Generate Pascal's Triangle
     * Pattern: Each element is sum of two above elements
     * Time: O(n²), Space: O(n²)
     */
    public static List<List<Integer>> generatePascalTriangle(int numRows) {
        List<List<Integer>> triangle = new ArrayList<>();
        
        for (int i = 0; i < numRows; i++) {
            List<Integer> row = new ArrayList<>();
            
            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i) {
                    row.add(1);
                } else {
                    List<Integer> prevRow = triangle.get(i - 1);
                    row.add(prevRow.get(j - 1) + prevRow.get(j));
                }
            }
            
            triangle.add(row);
        }
        
        return triangle;
    }
    
    /**
     * Calculate nth Catalan number
     * Pattern: Catalan formula or dynamic programming
     * Time: O(n), Space: O(1)
     */
    public static long catalanNumber(int n) {
        if (n <= 1) return 1;
        
        // Using formula: C(n) = (2n)! / ((n+1)! * n!)
        return combinations(2 * n, n) / (n + 1);
    }
    
    /**
     * Generate all Catalan numbers up to n
     * Pattern: Dynamic programming approach
     * Time: O(n²), Space: O(n)
     */
    public static List<Long> generateCatalanNumbers(int n) {
        List<Long> catalan = new ArrayList<>();
        if (n >= 0) catalan.add(1L);
        if (n >= 1) catalan.add(1L);
        
        for (int i = 2; i <= n; i++) {
            long sum = 0;
            for (int j = 0; j < i; j++) {
                sum += catalan.get(j) * catalan.get(i - 1 - j);
            }
            catalan.add(sum);
        }
        
        return catalan;
    }
    
    /**
     * Count derangements - Permutations where no element is in original position
     * Pattern: Inclusion-exclusion principle
     * Time: O(n), Space: O(1)
     */
    public static long derangements(int n) {
        if (n == 0) return 1;
        if (n == 1) return 0;
        if (n == 2) return 1;
        
        long prev2 = 1; // D(0)
        long prev1 = 0; // D(1)
        
        for (int i = 2; i <= n; i++) {
            long current = (i - 1) * (prev1 + prev2);
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    /**
     * Stirling numbers of second kind - Number of ways to partition n objects into k groups
     * Pattern: Dynamic programming with recurrence relation
     * Time: O(nk), Space: O(nk)
     */
    public static long stirlingSecond(int n, int k) {
        if (n == 0 && k == 0) return 1;
        if (n == 0 || k == 0) return 0;
        if (k > n) return 0;
        
        long[][] dp = new long[n + 1][k + 1];
        dp[0][0] = 1;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= Math.min(i, k); j++) {
                dp[i][j] = j * dp[i - 1][j] + dp[i - 1][j - 1];
            }
        }
        
        return dp[n][k];
    }
    
    /**
     * Count ways to make change - Classic dynamic programming
     * Pattern: DP with coin denominations
     * Time: O(amount * coins), Space: O(amount)
     */
    public static int countWaysToMakeChange(int amount, int[] coins) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }
        
        return dp[amount];
    }
    
    public static void main(String[] args) {
        // Test factorial
        System.out.println("=== FACTORIAL ===");
        System.out.println("5! = " + factorial(5));
        System.out.println("10! mod 1000 = " + factorialMod(10, 1000));
        
        // Test combinations and permutations
        System.out.println("\n=== COMBINATIONS & PERMUTATIONS ===");
        System.out.println("C(5,2) = " + combinations(5, 2));
        System.out.println("P(5,2) = " + permutations(5, 2));
        
        // Test Pascal's triangle
        System.out.println("\n=== PASCAL'S TRIANGLE ===");
        List<List<Integer>> pascal = generatePascalTriangle(5);
        for (List<Integer> row : pascal) {
            System.out.println(row);
        }
        
        // Test Catalan numbers
        System.out.println("\n=== CATALAN NUMBERS ===");
        System.out.println("Catalan(4) = " + catalanNumber(4));
        System.out.println("First 6 Catalan numbers: " + generateCatalanNumbers(5));
        
        // Test derangements
        System.out.println("\n=== DERANGEMENTS ===");
        System.out.println("Derangements of 4 items: " + derangements(4));
        
        // Test Stirling numbers
        System.out.println("\n=== STIRLING NUMBERS ===");
        System.out.println("S(4,2) = " + stirlingSecond(4, 2));
        
        // Test coin change
        System.out.println("\n=== COIN CHANGE ===");
        int[] coins = {1, 2, 5};
        System.out.println("Ways to make 5: " + countWaysToMakeChange(5, coins));
    }
}
```

### **Computational Geometry**

```java
import java.util.*;

public class ComputationalGeometry {
    
    /**
     * Point class for 2D geometry
     */
    public static class Point {
        double x, y;
        
        Point(double x, double y) {
            this.x = x;
            this.y = y;
        }
        
        @Override
        public String toString() {
            return String.format("(%.2f, %.2f)", x, y);
        }
    }
    
    /**
     * Calculate distance between two points
     * Pattern: Euclidean distance formula
     * Time: O(1), Space: O(1)
     */
    public static double distance(Point p1, Point p2) {
        double dx = p1.x - p2.x;
        double dy = p1.y - p2.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    /**
     * Calculate area of triangle given three points
     * Pattern: Cross product formula
     * Time: O(1), Space: O(1)
     */
    public static double triangleArea(Point p1, Point p2, Point p3) {
        return Math.abs((p1.x * (p2.y - p3.y) + p2.x * (p3.y - p1.y) + p3.x * (p1.y - p2.y)) / 2.0);
    }
    
    /**
     * Check orientation of three points
     * Pattern: Cross product sign
     * Time: O(1), Space: O(1)
     * Returns: 0 = collinear, 1 = clockwise, 2 = counterclockwise
     */
    public static int orientation(Point p, Point q, Point r) {
        double val = (q.y - p.y) * (r.x - q.x) - (q.x - p.x) * (r.y - q.y);
        
        if (Math.abs(val) < 1e-9) return 0; // Collinear
        return (val > 0) ? 1 : 2; // Clockwise or counterclockwise
    }
    
    /**
     * Check if point lies on line segment
     * Pattern: Collinearity and range check
     * Time: O(1), Space: O(1)
     */
    public static boolean onSegment(Point p, Point q, Point r) {
        return q.x <= Math.max(p.x, r.x) && q.x >= Math.min(p.x, r.x) &&
               q.y <= Math.max(p.y, r.y) && q.y >= Math.min(p.y, r.y);
    }
    
    /**
     * Check if two line segments intersect
     * Pattern: Orientation-based intersection test
     * Time: O(1), Space: O(1)
     */
    public static boolean doIntersect(Point p1, Point q1, Point p2, Point q2) {
        int o1 = orientation(p1, q1, p2);
        int o2 = orientation(p1, q1, q2);
        int o3 = orientation(p2, q2, p1);
        int o4 = orientation(p2, q2, q1);
        
        // General case
        if (o1 != o2 && o3 != o4) {
            return true;
        }
        
        // Special cases (collinear points)
        if (o1 == 0 && onSegment(p1, p2, q1)) return true;
        if (o2 == 0 && onSegment(p1, q2, q1)) return true;
        if (o3 == 0 && onSegment(p2, p1, q2)) return true;
        if (o4 == 0 && onSegment(p2, q1, q2)) return true;
        
        return false;
    }
    
    /**
     * Convex Hull using Graham Scan
     * Pattern: Sort points and use stack
     * Time: O(n log n), Space: O(n)
     */
    public static List<Point> convexHull(List<Point> points) {
        if (points.size() < 3) return new ArrayList<>();
        
        // Find bottom-most point (or leftmost in case of tie)
        Point start = points.get(0);
        for (Point p : points) {
            if (p.y < start.y || (p.y == start.y && p.x < start.x)) {
                start = p;
            }
        }
        
        final Point startPoint = start;
        
        // Sort points by polar angle with respect to start point
        List<Point> sortedPoints = new ArrayList<>(points);
        sortedPoints.remove(start);
        sortedPoints.sort((p1, p2) -> {
            int orient = orientation(startPoint, p1, p2);
            if (orient == 0) {
                // If collinear, sort by distance
                double dist1 = distance(startPoint, p1);
                double dist2 = distance(startPoint, p2);
                return Double.compare(dist1, dist2);
            }
            return (orient == 2) ? -1 : 1;
        });
        
        // Create convex hull
        Stack<Point> hull = new Stack<>();
        hull.push(start);
        
        for (Point p : sortedPoints) {
            // Remove points that make clockwise turn
            while (hull.size() > 1) {
                Point top = hull.pop();
                if (orientation(hull.peek(), top, p) != 1) {
                    hull.push(top);
                    break;
                }
            }
            hull.push(p);
        }
        
        return new ArrayList<>(hull);
    }
    
    /**
     * Check if point is inside polygon
     * Pattern: Ray casting algorithm
     * Time: O(n), Space: O(1)
     */
    public static boolean pointInPolygon(Point point, List<Point> polygon) {
        int n = polygon.size();
        if (n < 3) return false;
        
        // Create a point at infinity
        Point extreme = new Point(10000, point.y);
        
        int count = 0;
        int i = 0;
        
        do {
            int next = (i + 1) % n;
            
            // Check if ray intersects with polygon edge
            if (doIntersect(polygon.get(i), polygon.get(next), point, extreme)) {
                // Handle special case of collinear points
                if (orientation(polygon.get(i), point, polygon.get(next)) == 0) {
                    return onSegment(polygon.get(i), point, polygon.get(next));
                }
                count++;
            }
            i = next;
        } while (i != 0);
        
        return (count % 2 == 1);
    }
    
    /**
     * Calculate polygon area using shoelace formula
     * Pattern: Shoelace/surveyor's formula
     * Time: O(n), Space: O(1)
     */
    public static double polygonArea(List<Point> polygon) {
        if (polygon.size() < 3) return 0;
        
        double area = 0;
        int n = polygon.size();
        
        for (int i = 0; i < n; i++) {
            int j = (i + 1) % n;
            area += polygon.get(i).x * polygon.get(j).y;
            area -= polygon.get(j).x * polygon.get(i).y;
        }
        
        return Math.abs(area) / 2.0;
    }
    
    /**
     * Find closest pair of points
     * Pattern: Divide and conquer
     * Time: O(n log n), Space: O(n)
     */
    public static double closestPairDistance(List<Point> points) {
        if (points.size() < 2) return Double.MAX_VALUE;
        
        // Sort points by x-coordinate
        List<Point> sortedX = new ArrayList<>(points);
        sortedX.sort((p1, p2) -> Double.compare(p1.x, p2.x));
        
        return closestPairRec(sortedX);
    }
    
    private static double closestPairRec(List<Point> points) {
        int n = points.size();
        
        // Base case: brute force for small arrays
        if (n <= 3) {
            double minDist = Double.MAX_VALUE;
            for (int i = 0; i < n; i++) {
                for (int j = i + 1; j < n; j++) {
                    minDist = Math.min(minDist, distance(points.get(i), points.get(j)));
                }
            }
            return minDist;
        }
        
        // Divide
        int mid = n / 2;
        Point midPoint = points.get(mid);
        
        List<Point> left = points.subList(0, mid);
        List<Point> right = points.subList(mid, n);
        
        // Conquer
        double leftMin = closestPairRec(left);
        double rightMin = closestPairRec(right);
        
        double minDist = Math.min(leftMin, rightMin);
        
        // Find closest points across the divide
        List<Point> strip = new ArrayList<>();
        for (Point p : points) {
            if (Math.abs(p.x - midPoint.x) < minDist) {
                strip.add(p);
            }
        }
        
        return Math.min(minDist, stripClosest(strip, minDist));
    }
    
    private static double stripClosest(List<Point> strip, double minDist) {
        // Sort strip by y-coordinate
        strip.sort((p1, p2) -> Double.compare(p1.y, p2.y));
        
        for (int i = 0; i < strip.size(); i++) {
            for (int j = i + 1; j < strip.size() && (strip.get(j).y - strip.get(i).y) < minDist; j++) {
                minDist = Math.min(minDist, distance(strip.get(i), strip.get(j)));
            }
        }
        
        return minDist;
    }
    
    public static void main(String[] args) {
        // Test basic geometry
        System.out.println("=== BASIC GEOMETRY ===");
        Point p1 = new Point(0, 0);
        Point p2 = new Point(3, 4);
        System.out.println("Distance between " + p1 + " and " + p2 + ": " + distance(p1, p2));
        
        Point p3 = new Point(0, 3);
        System.out.println("Triangle area: " + triangleArea(p1, p2, p3));
        
        // Test line segment intersection
        System.out.println("\n=== LINE INTERSECTION ===");
        Point l1p1 = new Point(1, 1);
        Point l1p2 = new Point(10, 1);
        Point l2p1 = new Point(1, 2);
        Point l2p2 = new Point(10, 2);
        System.out.println("Lines intersect: " + doIntersect(l1p1, l1p2, l2p1, l2p2));
        
        // Test convex hull
        System.out.println("\n=== CONVEX HULL ===");
        List<Point> points = Arrays.asList(
            new Point(0, 3), new Point(1, 1), new Point(2, 2),
            new Point(4, 4), new Point(0, 0), new Point(1, 2),
            new Point(3, 1), new Point(3, 3)
        );
        List<Point> hull = convexHull(points);
        System.out.println("Convex hull: " + hull);
        
        // Test polygon area
        System.out.println("\n=== POLYGON AREA ===");
        List<Point> square = Arrays.asList(
            new Point(0, 0), new Point(2, 0),
            new Point(2, 2), new Point(0, 2)
        );
        System.out.println("Square area: " + polygonArea(square));
        
        // Test closest pair
        System.out.println("\n=== CLOSEST PAIR ===");
        List<Point> randomPoints = Arrays.asList(
            new Point(2, 3), new Point(12, 30), new Point(40, 50),
            new Point(5, 1), new Point(12, 10), new Point(3, 4)
        );
        System.out.println("Closest pair distance: " + closestPairDistance(randomPoints));
    }
}
```

---

## 🐹 **GO IMPLEMENTATIONS**

### **Number Theory in Go**

```go
package main

import (
    "fmt"
    "math"
)

/**
 * Check if number is prime
 * Time: O(√n), Space: O(1)
 */
func isPrime(n int) bool {
    if n <= 1 {
        return false
    }
    if n <= 3 {
        return true
    }
    if n%2 == 0 || n%3 == 0 {
        return false
    }
    
    for i := 5; i*i <= n; i += 6 {
        if n%i == 0 || n%(i+2) == 0 {
            return false
        }
    }
    
    return true
}

/**
 * Sieve of Eratosthenes
 * Time: O(n log log n), Space: O(n)
 */
func sieveOfEratosthenes(n int) []int {
    isPrime := make([]bool, n+1)
    for i := range isPrime {
        isPrime[i] = true
    }
    isPrime[0] = false
    isPrime[1] = false
    
    for i := 2; i*i <= n; i++ {
        if isPrime[i] {
            for j := i * i; j <= n; j += i {
                isPrime[j] = false
            }
        }
    }
    
    var primes []int
    for i := 2; i <= n; i++ {
        if isPrime[i] {
            primes = append(primes, i)
        }
    }
    
    return primes
}

/**
 * Prime factorization
 * Time: O(√n), Space: O(log n)
 */
func primeFactors(n int) []int {
    var factors []int
    
    // Handle factor 2
    for n%2 == 0 {
        factors = append(factors, 2)
        n /= 2
    }
    
    // Handle odd factors
    for i := 3; i*i <= n; i += 2 {
        for n%i == 0 {
            factors = append(factors, i)
            n /= i
        }
    }
    
    // If n is still > 2, it's a prime factor
    if n > 2 {
        factors = append(factors, n)
    }
    
    return factors
}

/**
 * Greatest Common Divisor - Euclidean Algorithm
 * Time: O(log min(a,b)), Space: O(log min(a,b))
 */
func gcd(a, b int) int {
    if b == 0 {
        return a
    }
    return gcd(b, a%b)
}

/**
 * Least Common Multiple
 * Time: O(log min(a,b)), Space: O(log min(a,b))
 */
func lcm(a, b int) int {
    return (a * b) / gcd(a, b)
}

/**
 * Modular Exponentiation
 * Time: O(log n), Space: O(1)
 */
func modPow(base, exp, mod int64) int64 {
    result := int64(1)
    base %= mod
    
    for exp > 0 {
        if exp%2 == 1 {
            result = (result * base) % mod
        }
        exp >>= 1
        base = (base * base) % mod
    }
    
    return result
}

/**
 * Extended Euclidean Algorithm
 * Returns gcd and coefficients x, y such that ax + by = gcd(a, b)
 */
func extendedGCD(a, b int) (int, int, int) {
    if b == 0 {
        return a, 1, 0
    }
    
    gcd, x1, y1 := extendedGCD(b, a%b)
    x := y1
    y := x1 - (a/b)*y1
    
    return gcd, x, y
}

/**
 * Modular Multiplicative Inverse
 * Time: O(log m), Space: O(log m)
 */
func modInverse(a, m int) int {
    gcd, x, _ := extendedGCD(a, m)
    
    if gcd != 1 {
        return -1 // Inverse doesn't exist
    }
    
    return (x%m + m) % m
}

func main() {
    // Test prime checking
    fmt.Println("=== PRIME NUMBERS ===")
    fmt.Printf("Is 17 prime: %t\n", isPrime(17))
    fmt.Printf("Is 25 prime: %t\n", isPrime(25))
    
    // Test sieve
    primes := sieveOfEratosthenes(30)
    fmt.Printf("Primes up to 30: %v\n", primes)
    
    // Test prime factorization
    fmt.Printf("Prime factors of 60: %v\n", primeFactors(60))
    
    // Test GCD and LCM
    fmt.Printf("GCD(48, 18): %d\n", gcd(48, 18))
    fmt.Printf("LCM(48, 18): %d\n", lcm(48, 18))
    
    // Test modular exponentiation
    fmt.Printf("2^10 mod 1000: %d\n", modPow(2, 10, 1000))
    
    // Test modular inverse
    fmt.Printf("Inverse of 3 mod 11: %d\n", modInverse(3, 11))
}
```

### **Combinatorics in Go**

```go
package main

import "fmt"

/**
 * Calculate factorial
 * Time: O(n), Space: O(1)
 */
func factorial(n int) int64 {
    if n < 0 {
        return 0
    }
    if n <= 1 {
        return 1
    }
    
    result := int64(1)
    for i := 2; i <= n; i++ {
        result *= int64(i)
    }
    return result
}

/**
 * Calculate nCr (combinations)
 * Time: O(min(r, n-r)), Space: O(1)
 */
func combinations(n, r int) int64 {
    if r > n || r < 0 {
        return 0
    }
    if r == 0 || r == n {
        return 1
    }
    
    // Optimize: C(n,r) = C(n,n-r)
    if r > n-r {
        r = n - r
    }
    
    result := int64(1)
    for i := 0; i < r; i++ {
        result = result * int64(n-i) / int64(i+1)
    }
    
    return result
}

/**
 * Calculate nPr (permutations)
 * Time: O(r), Space: O(1)
 */
func permutations(n, r int) int64 {
    if r > n || r < 0 {
        return 0
    }
    if r == 0 {
        return 1
    }
    
    result := int64(1)
    for i := n; i > n-r; i-- {
        result *= int64(i)
    }
    
    return result
}

/**
 * Generate Pascal's Triangle
 * Time: O(n²), Space: O(n²)
 */
func generatePascalTriangle(numRows int) [][]int {
    triangle := make([][]int, numRows)
    
    for i := 0; i < numRows; i++ {
        triangle[i] = make([]int, i+1)
        
        for j := 0; j <= i; j++ {
            if j == 0 || j == i {
                triangle[i][j] = 1
            } else {
                triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]
            }
        }
    }
    
    return triangle
}

/**
 * Calculate nth Catalan number
 * Time: O(n), Space: O(1)
 */
func catalanNumber(n int) int64 {
    if n <= 1 {
        return 1
    }
    
    return combinations(2*n, n) / int64(n+1)
}

/**
 * Generate Catalan numbers up to n
 * Time: O(n²), Space: O(n)
 */
func generateCatalanNumbers(n int) []int64 {
    catalan := make([]int64, n+1)
    if n >= 0 {
        catalan[0] = 1
    }
    if n >= 1 {
        catalan[1] = 1
    }
    
    for i := 2; i <= n; i++ {
        catalan[i] = 0
        for j := 0; j < i; j++ {
            catalan[i] += catalan[j] * catalan[i-1-j]
        }
    }
    
    return catalan
}

/**
 * Count derangements
 * Time: O(n), Space: O(1)
 */
func derangements(n int) int64 {
    if n == 0 {
        return 1
    }
    if n == 1 {
        return 0
    }
    if n == 2 {
        return 1
    }
    
    prev2 := int64(1) // D(0)
    prev1 := int64(0) // D(1)
    
    for i := 2; i <= n; i++ {
        current := int64(i-1) * (prev1 + prev2)
        prev2 = prev1
        prev1 = current
    }
    
    return prev1
}

/**
 * Count ways to make change
 * Time: O(amount * coins), Space: O(amount)
 */
func countWaysToMakeChange(amount int, coins []int) int {
    dp := make([]int, amount+1)
    dp[0] = 1
    
    for _, coin := range coins {
        for i := coin; i <= amount; i++ {
            dp[i] += dp[i-coin]
        }
    }
    
    return dp[amount]
}

func main() {
    // Test factorial
    fmt.Println("=== FACTORIAL ===")
    fmt.Printf("5! = %d\n", factorial(5))
    
    // Test combinations and permutations
    fmt.Println("\n=== COMBINATIONS & PERMUTATIONS ===")
    fmt.Printf("C(5,2) = %d\n", combinations(5, 2))
    fmt.Printf("P(5,2) = %d\n", permutations(5, 2))
    
    // Test Pascal's triangle
    fmt.Println("\n=== PASCAL'S TRIANGLE ===")
    pascal := generatePascalTriangle(5)
    for _, row := range pascal {
        fmt.Println(row)
    }
    
    // Test Catalan numbers
    fmt.Println("\n=== CATALAN NUMBERS ===")
    fmt.Printf("Catalan(4) = %d\n", catalanNumber(4))
    fmt.Printf("First 6 Catalan numbers: %v\n", generateCatalanNumbers(5))
    
    // Test derangements
    fmt.Println("\n=== DERANGEMENTS ===")
    fmt.Printf("Derangements of 4 items: %d\n", derangements(4))
    
    // Test coin change
    fmt.Println("\n=== COIN CHANGE ===")
    coins := []int{1, 2, 5}
    fmt.Printf("Ways to make 5: %d\n", countWaysToMakeChange(5, coins))
}
```

### **Computational Geometry in Go**

```go
package main

import (
    "fmt"
    "math"
    "sort"
)

// Point represents a 2D point
type Point struct {
    X, Y float64
}

func (p Point) String() string {
    return fmt.Sprintf("(%.2f, %.2f)", p.X, p.Y)
}

/**
 * Calculate distance between two points
 * Time: O(1), Space: O(1)
 */
func distance(p1, p2 Point) float64 {
    dx := p1.X - p2.X
    dy := p1.Y - p2.Y
    return math.Sqrt(dx*dx + dy*dy)
}

/**
 * Calculate area of triangle
 * Time: O(1), Space: O(1)
 */
func triangleArea(p1, p2, p3 Point) float64 {
    return math.Abs((p1.X*(p2.Y-p3.Y) + p2.X*(p3.Y-p1.Y) + p3.X*(p1.Y-p2.Y)) / 2.0)
}

/**
 * Check orientation of three points
 * Returns: 0 = collinear, 1 = clockwise, 2 = counterclockwise
 * Time: O(1), Space: O(1)
 */
func orientation(p, q, r Point) int {
    val := (q.Y-p.Y)*(r.X-q.X) - (q.X-p.X)*(r.Y-q.Y)
    
    if math.Abs(val) < 1e-9 {
        return 0 // Collinear
    }
    if val > 0 {
        return 1 // Clockwise
    }
    return 2 // Counterclockwise
}

/**
 * Check if point lies on line segment
 * Time: O(1), Space: O(1)
 */
func onSegment(p, q, r Point) bool {
    return q.X <= math.Max(p.X, r.X) && q.X >= math.Min(p.X, r.X) &&
           q.Y <= math.Max(p.Y, r.Y) && q.Y >= math.Min(p.Y, r.Y)
}

/**
 * Check if two line segments intersect
 * Time: O(1), Space: O(1)
 */
func doIntersect(p1, q1, p2, q2 Point) bool {
    o1 := orientation(p1, q1, p2)
    o2 := orientation(p1, q1, q2)
    o3 := orientation(p2, q2, p1)
    o4 := orientation(p2, q2, q1)
    
    // General case
    if o1 != o2 && o3 != o4 {
        return true
    }
    
    // Special cases (collinear points)
    if o1 == 0 && onSegment(p1, p2, q1) ||
       o2 == 0 && onSegment(p1, q2, q1) ||
       o3 == 0 && onSegment(p2, p1, q2) ||
       o4 == 0 && onSegment(p2, q1, q2) {
        return true
    }
    
    return false
}

/**
 * Convex Hull using Graham Scan
 * Time: O(n log n), Space: O(n)
 */
func convexHull(points []Point) []Point {
    if len(points) < 3 {
        return []Point{}
    }
    
    // Find bottom-most point
    start := points[0]
    for _, p := range points {
        if p.Y < start.Y || (p.Y == start.Y && p.X < start.X) {
            start = p
        }
    }
    
    // Remove start point and sort others by polar angle
    var sortedPoints []Point
    for _, p := range points {
        if p != start {
            sortedPoints = append(sortedPoints, p)
        }
    }
    
    sort.Slice(sortedPoints, func(i, j int) bool {
        orient := orientation(start, sortedPoints[i], sortedPoints[j])
        if orient == 0 {
            // If collinear, sort by distance
            dist1 := distance(start, sortedPoints[i])
            dist2 := distance(start, sortedPoints[j])
            return dist1 < dist2
        }
        return orient == 2 // Counterclockwise comes first
    })
    
    // Create convex hull
    hull := []Point{start}
    
    for _, p := range sortedPoints {
        // Remove points that make clockwise turn
        for len(hull) > 1 && orientation(hull[len(hull)-2], hull[len(hull)-1], p) == 1 {
            hull = hull[:len(hull)-1]
        }
        hull = append(hull, p)
    }
    
    return hull
}

/**
 * Calculate polygon area using shoelace formula
 * Time: O(n), Space: O(1)
 */
func polygonArea(polygon []Point) float64 {
    if len(polygon) < 3 {
        return 0
    }
    
    area := 0.0
    n := len(polygon)
    
    for i := 0; i < n; i++ {
        j := (i + 1) % n
        area += polygon[i].X * polygon[j].Y
        area -= polygon[j].X * polygon[i].Y
    }
    
    return math.Abs(area) / 2.0
}

/**
 * Check if point is inside polygon using ray casting
 * Time: O(n), Space: O(1)
 */
func pointInPolygon(point Point, polygon []Point) bool {
    n := len(polygon)
    if n < 3 {
        return false
    }
    
    // Create a point at infinity
    extreme := Point{X: 10000, Y: point.Y}
    
    count := 0
    i := 0
    
    for {
        next := (i + 1) % n
        
        // Check if ray intersects with polygon edge
        if doIntersect(polygon[i], polygon[next], point, extreme) {
            // Handle special case of collinear points
            if orientation(polygon[i], point, polygon[next]) == 0 {
                return onSegment(polygon[i], point, polygon[next])
            }
            count++
        }
        i = next
        if i == 0 {
            break
        }
    }
    
    return count%2 == 1
}

func main() {
    // Test basic geometry
    fmt.Println("=== BASIC GEOMETRY ===")
    p1 := Point{0, 0}
    p2 := Point{3, 4}
    fmt.Printf("Distance between %v and %v: %.2f\n", p1, p2, distance(p1, p2))
    
    p3 := Point{0, 3}
    fmt.Printf("Triangle area: %.2f\n", triangleArea(p1, p2, p3))
    
    // Test line segment intersection
    fmt.Println("\n=== LINE INTERSECTION ===")
    l1p1 := Point{1, 1}
    l1p2 := Point{10, 1}
    l2p1 := Point{1, 2}
    l2p2 := Point{10, 2}
    fmt.Printf("Lines intersect: %t\n", doIntersect(l1p1, l1p2, l2p1, l2p2))
    
    // Test convex hull
    fmt.Println("\n=== CONVEX HULL ===")
    points := []Point{
        {0, 3}, {1, 1}, {2, 2}, {4, 4},
        {0, 0}, {1, 2}, {3, 1}, {3, 3},
    }
    hull := convexHull(points)
    fmt.Printf("Convex hull: %v\n", hull)
    
    // Test polygon area
    fmt.Println("\n=== POLYGON AREA ===")
    square := []Point{
        {0, 0}, {2, 0}, {2, 2}, {0, 2},
    }
    fmt.Printf("Square area: %.2f\n", polygonArea(square))
    
    // Test point in polygon
    fmt.Println("\n=== POINT IN POLYGON ===")
    testPoint := Point{1, 1}
    fmt.Printf("Point %v in square: %t\n", testPoint, pointInPolygon(testPoint, square))
}
```

---

## 📊 **MATHEMATICAL ALGORITHM COMPLEXITY**

| Category | Algorithm | Time Complexity | Space Complexity | Use Case |
|----------|-----------|----------------|------------------|----------|
| **Prime Numbers** | Trial Division | O(√n) | O(1) | Single prime check |
| **Prime Numbers** | Sieve of Eratosthenes | O(n log log n) | O(n) | Multiple primes up to n |
| **Number Theory** | Euclidean GCD | O(log min(a,b)) | O(log min(a,b)) | Greatest common divisor |
| **Number Theory** | Modular Exponentiation | O(log n) | O(1) | Fast power with modulo |
| **Combinatorics** | Combinations nCr | O(min(r, n-r)) | O(1) | Binomial coefficients |
| **Combinatorics** | Pascal's Triangle | O(n²) | O(n²) | All binomial coefficients |
| **Geometry** | Convex Hull | O(n log n) | O(n) | Smallest enclosing polygon |
| **Geometry** | Closest Pair | O(n log n) | O(n) | Nearest two points |

---

## 📝 **PRACTICE PROBLEMS**

### **Easy Problems:**

1. **Fibonacci Number** - Calculate nth Fibonacci number
2. **Power of Two** - Check if number is power of 2
3. **Happy Number** - Check if number eventually reaches 1
4. **Perfect Number** - Check if number equals sum of proper divisors
5. **Count Primes** - Count primes less than n

### **Medium Problems:**

1. **Prime Factorization** ✅ (Implemented above)
2. **Pascal's Triangle** ✅ (Implemented above)
3. **Catalan Numbers** ✅ (Implemented above)
4. **Convex Hull** ✅ (Implemented above)
5. **Modular Exponentiation** ✅ (Implemented above)

### **Hard Problems:**

1. **Chinese Remainder Theorem** - Solve system of congruences
2. **Miller-Rabin Primality Test** - Probabilistic prime testing
3. **Discrete Logarithm** - Inverse of modular exponentiation
4. **Polynomial Multiplication** - Fast Fourier Transform
5. **Linear Programming** - Simplex algorithm

---

## 🎯 **YOUR PRACTICE PLAN**

### **Day 1: Master Number Theory**
- [ ] Implement prime checking and sieve algorithms
- [ ] Practice GCD and LCM problems
- [ ] Understand modular arithmetic
- [ ] Solve basic number theory problems

### **Day 2: Combinatorics and Counting**
- [ ] Master combinations and permutations
- [ ] Generate Pascal's triangle and understand properties
- [ ] Practice counting problems and dynamic programming
- [ ] Learn Catalan numbers and their applications

### **Day 3: Computational Geometry**
- [ ] Implement basic geometric operations
- [ ] Practice point, line, and polygon problems
- [ ] Master convex hull algorithm
- [ ] Understand coordinate geometry

### **Day 4: Advanced Applications**
- [ ] Solve 5+ mathematical problems on LeetCode
- [ ] Practice recognizing mathematical patterns
- [ ] Implement optimization algorithms
- [ ] Build confidence with mathematical thinking

### **✅ You're Ready for Next Topic When:**
- [ ] You can implement fundamental number theory algorithms
- [ ] You understand combinatorial counting principles
- [ ] You can solve basic geometric problems
- [ ] You recognize mathematical patterns in coding problems

---

## 🚀 **NEXT STEPS**

### **📚 Continue Your Journey:**
1. **System Design Algorithms** - Large-scale mathematical computations
2. **Machine Learning Mathematics** - Linear algebra and statistics
3. **Competitive Programming** - Advanced mathematical techniques

### **🎯 Advanced Mathematical Topics:**
- **Group Theory** and abstract algebra
- **Graph Theory** mathematics
- **Fourier Analysis** for signal processing
- **Linear Programming** and optimization

### **💡 Pro Tips:**
- **Understand the mathematics** - Don't just memorize algorithms
- **Practice pattern recognition** - Many problems have mathematical solutions
- **Learn mathematical proof techniques** - Helps with algorithm correctness
- **Study real applications** - Cryptography, graphics, finance, science
- **Build mathematical intuition** - Think about why algorithms work

**🎉 You now master mathematical algorithms! These fundamental techniques will help you solve complex computational problems efficiently and build a strong foundation for advanced computer science topics.**