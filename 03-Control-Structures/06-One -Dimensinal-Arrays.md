# One-Dimensional Arrays (Java)

---

## 1. Introduction

### What is a One-Dimensional Array?

A **one-dimensional array (1-D array)** is the simplest form of an array — a single row of elements of the same data type, stored in contiguous memory, and accessed using a single index.

```java
int[] marks = {90, 85, 76, 88, 95};
```

It is called "one-dimensional" because only **one index** is needed to identify any element (as opposed to 2-D arrays, which need a row and column index).

### Why One-Dimensional Arrays are Used

- To store a **list** of related values (e.g., marks, prices, temperatures) under a single variable name.
- To enable **looping** over data instead of writing repetitive code.
- To act as the **base building block** for more complex structures (2-D arrays, stacks, queues).
- To allow **efficient indexed access** and **bulk processing** (sorting, searching, filtering).

### Real-life Examples

| Real-World Scenario            | Array Representation     |
| ------------------------------ | ------------------------ |
| Marks of students in a class   | `int[] marks`            |
| Daily temperatures for a month | `double[] temperatures`  |
| Names of employees             | `String[] employeeNames` |
| Stock prices for a week        | `double[] stockPrices`   |
| Votes received by candidates   | `int[] votes`            |

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Declare one-dimensional arrays correctly.
- Create arrays using the `new` keyword and understand memory allocation.
- Initialize arrays using different techniques.
- Access and update array elements safely.
- Traverse arrays using various loop constructs.
- Perform common operations: sum, average, max, min, searching, copying, comparing.
- Pass arrays to methods and return arrays from methods.
- Identify and avoid common array-related errors.

---

## 3. Declaring an Array

### Syntax

```java
dataType[] arrayName;     // preferred (Java style)
dataType arrayName[];     // valid (C style), less preferred
```

### Rules

- No memory is allocated at declaration — only a reference variable is created (initialized to `null` by default).
- The data type must be specified and is fixed for the entire array's lifetime.
- Array name should follow standard Java variable naming conventions (camelCase).
- The size is **never** specified at declaration time in Java (unlike some other languages).

### Examples

```java
int[] marks;          // declares a reference to an int array
double[] prices;
char[] grades;
String[] names;
boolean flags[];       // valid but not recommended style
```

---

## 4. Creating an Array

### `new` Keyword

Creating an array means allocating actual memory on the **heap** using the `new` keyword, specifying the size.

```java
arrayName = new dataType[size];
```

### Memory Allocation

- When `new int[5]` is executed, Java allocates space for **5 contiguous integers** on the heap.
- Each element is automatically assigned a **default value** (`0` for `int`, `null` for objects, etc.).
- The reference variable on the stack now points to the base address of this block.

```
marks (stack) ---> [0][0][0][0][0]   (heap, 5 ints, default value 0)
                    idx:0 1 2 3 4
```

### Examples

```java
int[] marks;
marks = new int[5];        // creation after declaration

int[] scores = new int[10]; // declaration + creation combined

int n = 7;
double[] readings = new double[n]; // size decided at runtime
```

---

## 5. Initializing an Array

### Individual Initialization

Assigning values to each index one at a time.

```java
int[] arr = new int[3];
arr[0] = 5;
arr[1] = 10;
arr[2] = 15;
```

### Inline Initialization

Using array literal syntax at the time of declaration.

```java
int[] arr = {5, 10, 15};
```

> This shorthand is **only allowed during declaration**. `arr = {5, 10, 15};` (as a standalone statement later) is a compile-time error.

### Using Loops

Useful when values come from computation or user input.

```java
int[] squares = new int[5];
for (int i = 0; i < squares.length; i++) {
    squares[i] = i * i;
}
// squares = [0, 1, 4, 9, 16]
```

---

## 6. Different Ways to Create Arrays

**Method 1 — Declare and create, fill later**

```java
int[] numbers = new int[5];
```

**Method 2 — C-style declaration**

```java
int numbers[] = new int[5];
```

**Method 3 — Array literal**

```java
int[] numbers = {10, 20, 30, 40};
```

### Comparison

| Method          | Size Known Upfront?  | Values Set Immediately? | Style Preference                      |
| --------------- | -------------------- | ----------------------- | ------------------------------------- |
| `new int[5]`    | Yes                  | No (default values)     | Common, used with loops/input         |
| `int numbers[]` | Yes                  | No                      | Legal but discouraged (C-style)       |
| `{10,20,30,40}` | Inferred from values | Yes                     | Best when values are known in advance |

---

## 7. Accessing Array Elements

### Indexing

Elements are accessed via `arrayName[index]`, where index ranges from `0` to `length - 1`.

### Examples

```java
int[] arr = {10, 20, 30, 40, 50};
System.out.println(arr[0]);              // 10
System.out.println(arr[3]);              // 40
System.out.println(arr[arr.length - 1]); // 50 (safe way to get the last element)
```

### Memory Diagram

```
Index:    0    1    2    3    4
Value:  [10] [20] [30] [40] [50]
Address: 100  104  108  112  116   (int = 4 bytes each)
```

The address of `arr[i]` is computed as:

```
base_address + (i * size_of_int)
```

This is why array access is **O(1)** — a direct calculation, not a search.

---

## 8. Updating Array Elements

Updating means overwriting the value at a specific index.

### Examples

```java
int[] arr = {10, 20, 30};
arr[1] = 99;                       // arr becomes {10, 99, 30}

// Updating in a loop — e.g., add 5 to every element
for (int i = 0; i < arr.length; i++) {
    arr[i] += 5;
}
// arr becomes {15, 104, 35}
```

---

## 9. Traversing Arrays

### `for` Loop

```java
int[] arr = {10, 20, 30};
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

### `while` Loop

```java
int i = 0;
while (i < arr.length) {
    System.out.println(arr[i]);
    i++;
}
```

### `do-while` Loop

```java
int i = 0;
do {
    System.out.println(arr[i]);
    i++;
} while (i < arr.length);
```

> Use `do-while` cautiously — it always executes at least once, so ensure the array is non-empty before using it.

### Enhanced `for` Loop (Brief)

```java
for (int val : arr) {
    System.out.println(val);
}
```

Simplifies traversal when the index isn't needed, but doesn't allow direct index-based modification.

### Comparison

| Loop Type      | Index Access? | Can Modify Array?   | Best Use Case                             |
| -------------- | ------------- | ------------------- | ----------------------------------------- |
| `for`          | Yes           | Yes                 | General-purpose, most flexible            |
| `while`        | Yes           | Yes                 | When loop count is condition-driven       |
| `do-while`     | Yes           | Yes                 | When at least one iteration is guaranteed |
| Enhanced `for` | No            | No (read-only copy) | Simple read-only traversal                |

---

## 10. Array Length Property

### `length`

- A **final field**, not a method, so no parentheses: `arr.length`.
- Represents the total number of elements the array was created with (its capacity).

### Examples

```java
int[] arr = new int[8];
System.out.println(arr.length); // 8

int[] literalArr = {1, 2, 3};
System.out.println(literalArr.length); // 3
```

> Common mistake: `arr.length()` — this causes a **compile-time error** since `length` is a field, not a method (unlike `String.length()`).

---

## 11. User Input in Arrays

### Scanner

The `Scanner` class is commonly used to read array values from the user at runtime.

### Examples

```java
import java.util.Scanner;

public class UserInputArray {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter array size: ");
        int n = sc.nextInt();

        int[] arr = new int[n];
        System.out.println("Enter " + n + " elements:");
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        System.out.println("You entered:");
        for (int val : arr) {
            System.out.print(val + " ");
        }
    }
}
```

---

## 12. Passing Arrays to Methods

Arrays in Java are **objects**, so when passed to a method, the **reference** is passed (not a copy of the data). This means changes made inside the method **affect the original array**.

### Examples

```java
public class PassArrayExample {
    public static void doubleElements(int[] arr) {
        for (int i = 0; i < arr.length; i++) {
            arr[i] *= 2;
        }
    }

    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4};
        doubleElements(numbers);
        // numbers is now {2, 4, 6, 8} — original array modified
        System.out.println(java.util.Arrays.toString(numbers));
    }
}
```

> Since arrays are passed **by reference value** (the reference itself is copied, but it points to the same array object), any in-place modification is visible to the caller. However, if you reassign the parameter to a _new_ array inside the method, it does **not** affect the caller's original reference.

---

## 13. Returning Arrays from Methods

A method can return an array using the array type as its return type.

### Examples

```java
public class ReturnArrayExample {
    public static int[] createSquares(int n) {
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = i * i;
        }
        return result;
    }

    public static void main(String[] args) {
        int[] squares = createSquares(5);
        System.out.println(java.util.Arrays.toString(squares));
        // [0, 1, 4, 9, 16]
    }
}
```

---

## 14. Common Array Operations

```java
int[] arr = {12, 45, 3, 67, 21, 8};
```

### Sum

```java
int sum = 0;
for (int val : arr) sum += val;
System.out.println("Sum = " + sum);
```

### Average

```java
double average = (double) sum / arr.length;
System.out.println("Average = " + average);
```

### Maximum

```java
int max = arr[0];
for (int val : arr) {
    if (val > max) max = val;
}
System.out.println("Max = " + max);
```

### Minimum

```java
int min = arr[0];
for (int val : arr) {
    if (val < min) min = val;
}
System.out.println("Min = " + min);
```

### Count Even Numbers

```java
int evenCount = 0;
for (int val : arr) {
    if (val % 2 == 0) evenCount++;
}
System.out.println("Even count = " + evenCount);
```

### Count Odd Numbers

```java
int oddCount = arr.length - evenCount;
System.out.println("Odd count = " + oddCount);
```

### Reverse Array

```java
int left = 0, right = arr.length - 1;
while (left < right) {
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;
    left++;
    right--;
}
System.out.println(java.util.Arrays.toString(arr));
```

---

## 15. Searching in Arrays

### Linear Search

**Introduction**
Linear search checks each element one by one until the target is found or the array ends. It works on both **sorted** and **unsorted** arrays, with time complexity **O(n)**.

**Examples**

```java
public class LinearSearchExample {
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i; // return index if found
            }
        }
        return -1; // not found
    }

    public static void main(String[] args) {
        int[] arr = {5, 3, 8, 1, 9};
        int index = linearSearch(arr, 8);
        if (index != -1) {
            System.out.println("Found at index: " + index);
        } else {
            System.out.println("Not found");
        }
    }
}
```

---

## 16. Copying Arrays

### Manual Copy

```java
int[] source = {1, 2, 3, 4};
int[] destination = new int[source.length];
for (int i = 0; i < source.length; i++) {
    destination[i] = source[i];
}
```

### `System.arraycopy()`

A fast, native method for copying array data.

```java
int[] source = {1, 2, 3, 4};
int[] destination = new int[4];
System.arraycopy(source, 0, destination, 0, source.length);
// arraycopy(src, srcPos, dest, destPos, length)
```

### `Arrays.copyOf()`

Creates a new array of a specified length, copying elements from the original (padding with default values if larger).

```java
import java.util.Arrays;

int[] source = {1, 2, 3};
int[] copy = Arrays.copyOf(source, 5);
// copy = {1, 2, 3, 0, 0}
```

> **Important:** `int[] b = a;` does **NOT** copy the array — it just copies the reference. Both `a` and `b` point to the **same** array in memory. Use one of the above methods for a true copy.

---

## 17. Comparing Arrays

### `==`

Compares **references**, not contents — checks if both variables point to the exact same array object in memory.

```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
System.out.println(a == b); // false — different objects, even though values are equal

int[] c = a;
System.out.println(a == c); // true — same reference
```

### `Arrays.equals()`

Compares **contents** element-by-element.

```java
import java.util.Arrays;

int[] a = {1, 2, 3};
int[] b = {1, 2, 3};
System.out.println(Arrays.equals(a, b)); // true — same content
```

---

## 18. Common Errors

### ArrayIndexOutOfBoundsException

Occurs when accessing an index outside the valid range `[0, length-1]`.

```java
int[] arr = {1, 2, 3};
System.out.println(arr[3]); // ArrayIndexOutOfBoundsException
```

### NullPointerException

Occurs when trying to access an array (or its object elements) that is `null`.

```java
int[] arr = null;
System.out.println(arr[0]); // NullPointerException
```

### Off-by-One Error

A very common bug caused by using `<=` instead of `<` in loop conditions.

```java
int[] arr = {1, 2, 3};
for (int i = 0; i <= arr.length; i++) {  // BUG: should be i < arr.length
    System.out.println(arr[i]);          // throws exception when i = 3
}
```

---

## 19. Best Practices

1. Always use `arr.length` in loop conditions instead of hardcoding array size.
2. Prefer the enhanced `for` loop for simple, read-only traversal.
3. Use `Arrays.equals()` to compare array contents, never `==`.
4. Use `Arrays.copyOf()` or `System.arraycopy()` for copying — avoid manual loops unless necessary.
5. Validate array bounds before accessing indices, especially with user input.
6. Check for `null` before accessing an array received as a method parameter.
7. Use meaningful, descriptive array names (`studentMarks`, not `arr1`).
8. Avoid unnecessary array copying in performance-critical code.
9. Prefer returning empty arrays (`new int[0]`) instead of `null` from methods, to avoid `NullPointerException` at the call site.
10. Use `java.util.Arrays` utility methods rather than reinventing common operations.

---

## 20. Interview Corner

**Q1. Why does array access take O(1) time?**
Because the memory address of any element can be directly computed using `base_address + (index * element_size)` — no traversal is needed.

**Q2. What happens when you pass an array to a method in Java?**
The reference to the array is passed by value, meaning the method receives a copy of the reference, but both point to the same array object. In-place modifications inside the method affect the original array.

**Q3. What is the difference between `arr1 == arr2` and `Arrays.equals(arr1, arr2)`?**
`==` compares object references (memory addresses); `Arrays.equals()` compares actual content, element by element.

**Q4. How would you reverse an array in-place without using extra space?**
Use the two-pointer technique — swap elements from both ends moving toward the center (see Section 14).

**Q5. Why is Linear Search O(n) but array access O(1)?**
Array access uses direct index-based address calculation, while linear search must potentially inspect every element sequentially until a match is found.

**Q6. What's the difference between `System.arraycopy()` and `Arrays.copyOf()`?**
`System.arraycopy()` copies into an **existing** destination array; `Arrays.copyOf()` creates and returns a **brand-new** array of the specified length.

**Q7. Can a method return an array of a different length than what was passed in?**
Yes — since a new array can be created inside the method with any size, the returned array's length is entirely independent of any input array's length.

**Q8. Is it possible to modify the original array without returning it from a method?**
Yes, because arrays are passed by reference; in-place changes (e.g., `arr[i] = x`) inside a method persist after the method returns.

---

## 21. MCQs

**1. Which of these is a valid 1-D array declaration?**
a) `int arr[5];` b) `int[] arr;` c) `array int arr;` d) `int arr = new int();`

> **Answer: b)**

**2. What is the result of `arr.length` for `int[] arr = new int[6];`?**
a) 5 b) 6 c) 0 d) Compile Error

> **Answer: b) 6**

**3. Which statement about `==` and `Arrays.equals()` is TRUE?**
a) `==` compares content; `Arrays.equals()` compares references  
b) Both compare references  
c) `==` compares references; `Arrays.equals()` compares content  
d) Both compare content

> **Answer: c)**

**4. What is the time complexity of Linear Search in the worst case?**
a) O(1) b) O(log n) c) O(n) d) O(n²)

> **Answer: c) O(n)**

**5. Which method creates a completely new array while copying values?**
a) `System.arraycopy()` b) `Arrays.copyOf()` c) Direct assignment (`b = a`) d) None of these

> **Answer: b)**

**6. What happens if you access `arr[-1]`?**
a) Returns last element b) Returns 0 c) Throws ArrayIndexOutOfBoundsException d) Compile Error

> **Answer: c)**

**7. When an array is passed to a method and modified in-place, what happens to the original array?**
a) Remains unchanged b) Gets modified c) Throws exception d) Depends on JVM

> **Answer: b) Gets modified (since reference is shared)**

**8. Which loop does NOT allow direct index-based access to array elements?**
a) `for` loop b) `while` loop c) Enhanced `for` loop d) `do-while` loop

> **Answer: c)**

**9. What is the output of `int[] a = {1,2,3}; int[] b = a; System.out.println(a == b);`?**
a) false b) true c) Compile Error d) Runtime Error

> **Answer: b) true (same reference)**

**10. What is the default value of `arr[2]` for `String[] arr = new String[5];`?**
a) "" b) 0 c) null d) undefined

> **Answer: c) null**

---

## 22. University Questions

### Short Questions

1. Define a one-dimensional array with an example.
2. What is the difference between array declaration and array creation?
3. What is the purpose of the `length` property in arrays?
4. Differentiate between `==` and `Arrays.equals()` for arrays.
5. What is `ArrayIndexOutOfBoundsException`? Give an example.

### Long Questions

1. Explain different ways of declaring, creating, and initializing a one-dimensional array in Java with examples. _(10 marks)_
2. Write a Java program to find the sum, average, maximum, and minimum of elements in an array. _(10 marks)_
3. Explain how arrays are passed to and returned from methods in Java, with suitable examples. _(10 marks)_
4. Describe Linear Search with an algorithm and Java implementation. Discuss its time complexity. _(10 marks)_
5. Explain different methods of copying arrays in Java (`manual loop`, `System.arraycopy()`, `Arrays.copyOf()`) with examples. _(10 marks)_
6. Discuss common exceptions related to arrays with example programs demonstrating each. _(5 marks)_

---

## 23. Viva Questions

1. What is a one-dimensional array?
2. How is an array different from a normal variable?
3. Why does array indexing start from 0?
4. What is the output of printing an array directly using `System.out.println(arr)`?
5. What is the difference between `arr.length` and `arr.length()`?
6. How do you copy an array without sharing the same reference?
7. What is the difference between passing a primitive to a method vs. passing an array?
8. Can you change the size of an array after passing it to a method?
9. What does Linear Search return if the element is not found?
10. What is the difference between `==` and `.equals()` when comparing arrays?
11. What happens if you try to access a negative index?
12. Why is `do-while` risky to use for array traversal?
13. What is the default value stored in an uninitialized `int[]` array?
14. How would you check if two arrays contain the same elements?
15. What does `System.arraycopy()` do internally?

---

## 24. Programming Exercises

### Easy

1. Write a program to input `n` numbers into an array and print them in reverse order.
2. Write a program to find the sum of all elements in an array.
3. Write a program to count how many elements in an array are greater than a given number.
4. Write a program to find the largest and smallest elements in an array.

### Medium

1. Write a program to perform Linear Search on an array and return all indices where the target is found (not just the first).
2. Write a program to remove duplicate elements from an array and print unique elements.
3. Write a program to merge two arrays into a third array without using built-in methods.
4. Write a program to check whether an array is sorted in ascending order.
5. Write a program to find the second largest element in an array without sorting.

### Hard

1. Write a program to rotate an array left by `k` positions using O(1) extra space.
2. Write a program to find all pairs of elements in an array whose sum equals a given target value.
3. Write a program to find the maximum sum of any contiguous subarray (Kadane's Algorithm).
4. Write a program to rearrange an array so that all even numbers appear before all odd numbers, maintaining relative order.

---

## 25. Challenge Problems

### Student Marks

Given an array of student marks, compute the class average, identify students who scored above average, and assign grades based on ranges (A/B/C/D/F).

### Salary Management

Given an array of employee salaries, calculate total payroll, average salary, and find employees earning above/below the average. Apply a percentage raise to all salaries and print updated values.

### Temperature Analysis

Given an array of 30 daily temperatures for a month, compute the average temperature, find the hottest and coldest day, and count how many days exceeded a given threshold.

### Voting Results

Given an array of votes received by 5 candidates, determine the winner, compute total votes cast, and calculate each candidate's percentage of the total votes.

### Bank Transactions

Given an array representing daily transaction amounts (positive for deposits, negative for withdrawals), compute the final account balance, total deposits, total withdrawals, and flag any single transaction exceeding a suspicious threshold.

---

## 26. Summary

- A **one-dimensional array** stores a linear sequence of elements of the same type, accessed via a single index.
- Arrays must be **declared**, **created** (using `new`), and **initialized** before meaningful use.
- Elements are accessed and updated using zero-based indexing, in **O(1)** time.
- Arrays can be traversed using `for`, `while`, `do-while`, or enhanced `for` loops.
- Arrays are **objects** in Java — passed to methods by reference, allowing in-place modification.
- Common operations include sum, average, max, min, searching, copying, comparing, and reversing.
- **Linear Search** is the simplest search technique, with O(n) time complexity.
- `==` compares references, while `Arrays.equals()` compares actual content.
- Common pitfalls include `ArrayIndexOutOfBoundsException`, `NullPointerException`, and off-by-one loop errors.

---

## 27. Key Takeaways

- Always validate indices before accessing array elements.
- `arr.length` is a **field**, not a method.
- Arrays are passed by reference — in-place changes persist outside the method.
- Use `Arrays` utility class methods instead of manual implementations where possible.
- Never compare array contents using `==`; always use `Arrays.equals()`.
- Direct assignment (`b = a`) does not copy an array — it copies the reference.
- Linear Search works on both sorted and unsorted arrays but is O(n).

---

## 28. Cheat Sheet

```java
// Declaration
int[] arr;

// Creation
arr = new int[5];

// Initialization
int[] arr = {1, 2, 3, 4, 5};

// Access
int x = arr[0];

// Update
arr[0] = 100;

// Length
int len = arr.length;

// Traversal
for (int i = 0; i < arr.length; i++) { System.out.println(arr[i]); }
for (int val : arr) { System.out.println(val); }

// Sum / Average
int sum = 0;
for (int v : arr) sum += v;
double avg = (double) sum / arr.length;

// Max / Min
int max = arr[0], min = arr[0];
for (int v : arr) {
    if (v > max) max = v;
    if (v < min) min = v;
}

// Linear Search
int target = 5, index = -1;
for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) { index = i; break; }
}

// Reverse (two-pointer)
int l = 0, r = arr.length - 1;
while (l < r) {
    int t = arr[l]; arr[l] = arr[r]; arr[r] = t;
    l++; r--;
}

// Copying
int[] copy1 = java.util.Arrays.copyOf(arr, arr.length);
int[] copy2 = new int[arr.length];
System.arraycopy(arr, 0, copy2, 0, arr.length);

// Comparing
boolean same = java.util.Arrays.equals(arr, copy1); // content comparison

// Passing to method
static void modify(int[] a) { a[0] = 999; }  // affects original array

// Returning from method
static int[] makeArray() { return new int[]{1, 2, 3}; }
```

| Concept    | Syntax                  |
| ---------- | ----------------------- |
| Declare    | `int[] arr;`            |
| Create     | `arr = new int[5];`     |
| Initialize | `int[] arr = {1,2,3};`  |
| Access     | `arr[i]`                |
| Update     | `arr[i] = value;`       |
| Length     | `arr.length`            |
| Copy       | `Arrays.copyOf(arr, n)` |
| Compare    | `Arrays.equals(a, b)`   |
| Search     | Linear Search — O(n)    |

---

_End of Chapter — One-Dimensional Arrays (Java)_
