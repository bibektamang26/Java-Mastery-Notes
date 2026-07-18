# Introduction to Arrays (Java)

---

## 1. Introduction

An **array** is one of the most fundamental data structures in computer science and forms the building block for many advanced data structures like stacks, queues, matrices, and hash tables. In Java, an array is an **object** that stores multiple values of the **same data type** in a single, contiguous block of memory.

Arrays allow us to group related data together and access it efficiently using a numeric index, instead of creating separate variables for every single value. This chapter introduces arrays in Java from the ground up — from theory to memory layout to practical coding.

---

## 2. Learning Objectives

By the end of this chapter, you should be able to:

- Define what an array is and explain why it is needed.
- Identify the problems faced without using arrays.
- List the characteristics, advantages, and disadvantages of arrays.
- Understand array terminology: element, index, length, and data type.
- Explain how arrays are represented in memory.
- Declare, create, and initialize arrays in Java using different methods.
- Access, update, and traverse array elements.
- Understand default values assigned to array elements.
- Identify and handle common array-related errors/exceptions.
- Apply best practices while working with arrays.
- Solve interview-level MCQs, viva questions, and programming exercises based on arrays.

---

## 3. What is an Array?

An **array** is a collection of a fixed number of elements of the **same data type**, stored in **contiguous memory locations**, and accessed using an **index**.

### Formal Definition

> An array is a linear data structure that stores a fixed-size sequential collection of elements of the same type.

### Simple Example

```java
int[] marks = {90, 85, 76, 88, 95};
```

Here, `marks` is an array that stores 5 integer values. Each value can be accessed using its index, starting from `0`.

### Real-Life Analogy

Think of an array as a **row of lockers** in a school corridor:

- Each locker has a **unique number** (index).
- Each locker can hold **one item** of a specific kind (data type).
- All lockers are **physically next to each other** (contiguous memory).

---

## 4. Why Do We Need Arrays?

Without arrays, handling multiple related values becomes cumbersome. Consider storing marks of 100 students — declaring 100 separate variables (`marks1, marks2, ..., marks100`) is impractical.

Arrays solve this by:

- Allowing **grouped storage** of related data under a single name.
- Enabling **efficient access** via index (O(1) time complexity).
- Making **iteration** over data simple using loops.
- Supporting **bulk operations** like sorting, searching, and filtering.
- Serving as the **foundation** for advanced data structures (stacks, queues, hash tables, matrices, heaps).

---

## 5. Problems Without Arrays

Imagine storing marks of 5 students without using an array:

```java
int marks1 = 90;
int marks2 = 85;
int marks3 = 76;
int marks4 = 88;
int marks5 = 95;

int total = marks1 + marks2 + marks3 + marks4 + marks5;
```

**Issues with this approach:**

| Problem           | Explanation                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------------- |
| Poor Scalability  | Doesn't scale for 1000+ students; you'd need 1000+ variables.                                 |
| No Looping        | You cannot use a `for` loop to process `marks1, marks2, ...` — each must be handled manually. |
| Code Duplication  | Repetitive code increases the chance of bugs.                                                 |
| Hard to Maintain  | Adding/removing a student means changing variable names throughout the code.                  |
| Memory Scattered  | Variables aren't guaranteed to be contiguous, making bulk operations inefficient.             |
| No Dynamic Access | You can't access "the 47th student's marks" using a computed index.                           |

Arrays solve **all** of these problems by allowing indexed, loopable, and scalable storage.

---

## 6. Characteristics of Arrays

1. **Fixed Size** — Once created, the size of an array cannot change.
2. **Homogeneous Elements** — All elements must be of the same data type.
3. **Contiguous Memory Allocation** — Elements are stored in adjacent memory locations.
4. **Zero-Based Indexing** — The first element is at index `0`, not `1`.
5. **Random Access** — Any element can be accessed directly in O(1) time using its index.
6. **Reference Type** — In Java, arrays are objects and are stored on the **heap**, even for primitive types.
7. **Default Initialization** — Elements get default values automatically upon creation.
8. **`length` Property** — Every array has a built-in `length` field indicating the number of elements.

---

## 7. Advantages of Arrays

- **Fast Access:** Random access to any element in constant time O(1).
- **Memory Efficiency:** No extra overhead per element (unlike linked lists which need pointers).
- **Easy to Traverse:** Simple to iterate using loops.
- **Foundation for Other DS:** Used to implement stacks, queues, heaps, hash tables, matrices, and more.
- **Cache Friendly:** Contiguous memory improves CPU cache performance.
- **Supports Multi-Dimensional Data:** Can represent matrices, grids, tables, etc.

---

## 8. Disadvantages of Arrays

- **Fixed Size:** Size must be known in advance; cannot grow or shrink dynamically.
- **Costly Insertion/Deletion:** Inserting/removing an element (except at the end) requires shifting elements — O(n) time.
- **Memory Wastage:** If declared larger than needed, unused space is wasted.
- **Homogeneous Only:** Cannot store elements of different data types (unless using `Object[]`).
- **No Built-in Methods:** Unlike `ArrayList`, arrays don't have methods like `add()`, `remove()` — you must use utility classes like `java.util.Arrays`.

---

## 9. Array Terminology

### Element

Each individual value stored in the array.

```java
int[] arr = {10, 20, 30};
// 10, 20, and 30 are elements
```

### Index

The position of an element in the array, starting from `0` and going up to `length - 1`.

```java
arr[0]; // 10 → index 0
arr[2]; // 30 → index 2 (last index = length - 1 = 2)
```

### Length

The total number of elements the array can hold — accessed via the `length` field (not a method).

```java
System.out.println(arr.length); // 3
```

### Data Type

The type of elements the array can store — can be primitive (`int`, `char`, `double`, etc.) or reference types (`String`, custom objects, etc.).

```java
int[] intArr;         // primitive type array
String[] strArr;      // reference type array
```

---

## 10. Memory Representation of Arrays

In Java, arrays are **objects** and are always stored on the **heap** memory, regardless of whether they hold primitive or reference types. The reference variable (pointing to the array) is stored on the **stack**.

### Example

```java
int[] arr = {10, 20, 30, 40};
```

### Memory Layout (Conceptual)

```
Stack                Heap
------                --------------------------
arr  ---------->      [ 10 | 20 | 30 | 40 ]
                       index: 0    1    2    3
                       address: 100  104  108  112  (assuming int = 4 bytes)
```

**Key Points:**

- All elements are stored in **contiguous memory addresses**.
- The **base address** (address of index 0) is stored internally, and other elements are located using the formula:
  ```
  address(arr[i]) = base_address + (i * size_of_data_type)
  ```
- For **arrays of objects** (e.g., `String[]`), the array stores **references** to objects, and the actual objects live elsewhere on the heap.

```
String[] names = {"Amit", "Riya"};

Stack        Heap (array)             Heap (String objects)
names ---->  [ ref1 | ref2 ] ------->  "Amit"
                        \------------> "Riya"
```

---

## 11. Array Declaration

Declaring an array means specifying its **type** and **name** — no memory is allocated yet.

### Syntax

```java
dataType[] arrayName;      // preferred style
// OR
dataType arrayName[];      // C-style, also valid in Java
```

### Examples

```java
int[] numbers;
double[] prices;
String[] names;
char cGrades[];   // valid but not recommended style
```

> **Note:** At this stage, `numbers` is just a reference variable pointing to `null` — no array object exists in memory yet.

---

## 12. Array Creation

Creating an array means allocating memory for it using the `new` keyword.

### Syntax

```java
arrayName = new dataType[size];
```

### Example

```java
int[] numbers;          // declaration
numbers = new int[5];   // creation (allocates memory for 5 integers)
```

### Combined Declaration + Creation

```java
int[] numbers = new int[5];
```

### Important Rules

- Array size must be a **non-negative integer**.
- Once created, the size **cannot be changed**.
- Array size can be determined at **runtime** (unlike C, where it's often compile-time in the classic sense):

```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
int[] arr = new int[n];   // valid — size decided at runtime
```

---

## 13. Array Initialization

Initialization means assigning values to the array elements.

### Method 1: Element-by-Element

```java
int[] arr = new int[3];
arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
```

### Method 2: Array Literal (Declaration + Initialization together)

```java
int[] arr = {10, 20, 30};
```

### Method 3: Using `new` with Values

```java
int[] arr = new int[]{10, 20, 30};
```

> **Note:** The shorthand `{10, 20, 30}` syntax **can only be used at the time of declaration**. It cannot be used later like `arr = {10, 20, 30};` — that will cause a compile-time error. Use `new int[]{...}` instead in such cases.

---

## 14. Different Ways to Initialize Arrays

```java
// 1. Static Initialization
int[] a = {1, 2, 3, 4, 5};

// 2. Dynamic Initialization (via new + assignment)
int[] b = new int[5];
for (int i = 0; i < b.length; i++) {
    b[i] = i * 2;
}

// 3. Using new with explicit values
int[] c = new int[]{1, 2, 3};

// 4. Using a loop with user input
Scanner sc = new Scanner(System.in);
int[] d = new int[5];
for (int i = 0; i < d.length; i++) {
    d[i] = sc.nextInt();
}

// 5. Using Arrays.fill() to initialize with the same value
int[] e = new int[5];
Arrays.fill(e, 7);   // all elements become 7

// 6. Using streams (Java 8+)
int[] f = java.util.stream.IntStream.range(0, 5).toArray(); // {0,1,2,3,4}
```

---

## 15. Default Values of Array Elements

When an array is created using `new` but not explicitly initialized, Java automatically assigns **default values** based on the data type.

| Data Type                                 | Default Value               |
| ----------------------------------------- | --------------------------- |
| `byte`                                    | `0`                         |
| `short`                                   | `0`                         |
| `int`                                     | `0`                         |
| `long`                                    | `0L`                        |
| `float`                                   | `0.0f`                      |
| `double`                                  | `0.0d`                      |
| `char`                                    | `'\u0000'` (null character) |
| `boolean`                                 | `false`                     |
| Reference types (`String`, objects, etc.) | `null`                      |

### Example

```java
int[] arr = new int[3];
System.out.println(arr[0]); // 0
System.out.println(arr[1]); // 0

boolean[] flags = new boolean[2];
System.out.println(flags[0]); // false

String[] names = new String[2];
System.out.println(names[0]); // null
```

---

## 16. Accessing Array Elements

Elements are accessed using the **index operator `[]`**, with indices ranging from `0` to `length - 1`.

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr[0]);  // 10 (first element)
System.out.println(arr[4]);  // 50 (last element)
System.out.println(arr[arr.length - 1]); // 50 — safe way to get last element
```

**Attempting to access an invalid index throws an exception:**

```java
System.out.println(arr[5]); // ArrayIndexOutOfBoundsException
```

---

## 17. Updating Array Elements

You can modify the value at a specific index by directly assigning a new value to it.

```java
int[] arr = {10, 20, 30};
arr[1] = 99;   // updates index 1

System.out.println(Arrays.toString(arr)); // [10, 99, 30]
```

### Updating in a Loop (e.g., doubling all elements)

```java
for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[i] * 2;
}
```

---

## 18. Traversing an Array

Traversing means visiting each element of the array, typically to display, process, or compute something.

### Using a Standard `for` Loop

```java
int[] arr = {10, 20, 30, 40};
for (int i = 0; i < arr.length; i++) {
    System.out.println("Index " + i + ": " + arr[i]);
}
```

### Using an Enhanced `for-each` Loop

```java
for (int val : arr) {
    System.out.println(val);
}
```

> **Note:** The for-each loop is simpler but does **not** give access to the index, and you cannot modify the original array elements through it (since `val` is a copy).

### Using a `while` Loop

```java
int i = 0;
while (i < arr.length) {
    System.out.println(arr[i]);
    i++;
}
```

### Using Streams (Java 8+)

```java
Arrays.stream(arr).forEach(System.out::println);
```

---

## 19. Array Length Property

- `length` is a **final field**, not a method — accessed as `arrayName.length` (no parentheses).
- It represents the **total capacity** of the array, not the number of "filled" elements (arrays don't track that separately).
- Common mistake: confusing `array.length` (field, for arrays) with `string.length()` (method, for Strings) and `list.size()` (method, for Collections).

```java
int[] arr = new int[10];
System.out.println(arr.length); // 10

String str = "hello";
System.out.println(str.length()); // 5 — method call, note the ()
```

| Type      | Syntax         | Kind   |
| --------- | -------------- | ------ |
| Array     | `arr.length`   | Field  |
| String    | `str.length()` | Method |
| ArrayList | `list.size()`  | Method |

---

## 20. Common Errors

### ArrayIndexOutOfBoundsException

Thrown when accessing an index that is **negative** or **≥ array length**.

```java
int[] arr = {1, 2, 3};
System.out.println(arr[3]);
// Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException:
// Index 3 out of bounds for length 3
```

**Common causes:**

- Off-by-one errors in loops (`i <= arr.length` instead of `i < arr.length`).
- Using 1-based indexing by mistake.
- Accessing `arr[-1]` thinking it means "last element" (unlike Python).

**Fix:**

```java
for (int i = 0; i < arr.length; i++) { ... }   // correct
```

### NullPointerException

Thrown when trying to use an array reference (or its elements, in case of object arrays) that is `null`.

```java
int[] arr = null;
System.out.println(arr[0]);
// Exception in thread "main" java.lang.NullPointerException

String[] names = new String[3]; // elements default to null
System.out.println(names[0].length());
// NullPointerException — names[0] is null, calling .length() on it fails
```

**Fix:** Always ensure the array (and its elements, if objects) are properly initialized before use, and consider null-checks when working with object arrays.

---

## 21. Best Practices

1. Always use `array.length` in loop conditions instead of hardcoding the size.
2. Prefer the **enhanced for-loop** when the index is not needed — it's cleaner and less error-prone.
3. Initialize arrays with meaningful default/starting values when necessary, especially for object arrays.
4. Use `java.util.Arrays` utility methods (`toString()`, `sort()`, `fill()`, `copyOf()`, `equals()`) instead of manually reinventing them.
5. Validate user input/index before accessing array elements to avoid `ArrayIndexOutOfBoundsException`.
6. Use meaningful variable names (e.g., `studentMarks` instead of `arr`) for readability.
7. Prefer `ArrayList`/Collections when the size needs to change dynamically.
8. Avoid magic numbers — use constants for array sizes where applicable.
9. Be cautious with multi-dimensional arrays; always double-check row/column bounds.
10. Free large arrays (`arr = null;`) when no longer needed in memory-sensitive applications, allowing garbage collection.

---

## 22. Interview Corner

**Q1. Why are arrays index-based starting from 0 in Java?**
Because the index represents an **offset** from the base address in memory. Index `0` means "0 elements away from the start," which simplifies address calculation: `base_address + (index * element_size)`.

**Q2. Are arrays primitive types or objects in Java?**
Arrays are **objects** in Java, even when they store primitive types. They are created on the heap and have a `length` field, plus they inherit from `Object` (they even have a `clone()` method).

**Q3. Can array size be negative or a non-integer?**
No. Array size must be a non-negative `int`. A negative size throws `NegativeArraySizeException` at runtime.

**Q4. What's the difference between `length` and `length()`?**
`length` is a field used for arrays; `length()` is a method used for `String` objects.

**Q5. Can we change the size of an array after creation?**
No, arrays in Java are **fixed-size**. To simulate resizing, you must create a new array and copy elements (or use `Arrays.copyOf()`), or use a dynamic structure like `ArrayList`.

**Q6. What is the time complexity of accessing an array element?**
O(1) — constant time, due to direct address calculation via indexing.

**Q7. What is the time complexity of inserting an element in the middle of an array?**
O(n) — because all subsequent elements must be shifted to make room.

**Q8. How are multi-dimensional arrays stored in Java?**
As **arrays of arrays** (jagged array model), not as a single contiguous block like in C. Each row can even have a different length.

**Q9. What happens if you try to store a `double` in an `int[]` array?**
Compile-time error — Java is statically typed, and implicit narrowing conversions are not allowed without an explicit cast.

**Q10. Is array covariance allowed in Java?**
Yes — `Object[] objArr = new String[3];` is legal (array covariance), but it can lead to `ArrayStoreException` at runtime if you try to store an incompatible type, e.g., `objArr[0] = 10;` (an Integer into a String array).

---

## 23. MCQs

**1. What is the index of the first element in a Java array?**
a) 1 b) -1 c) 0 d) Depends on declaration

> **Answer: c) 0**

**2. Which of these correctly creates an array of 10 integers?**
a) `int arr[10];` b) `int[] arr = new int[10];` c) `int arr = new int[10];` d) `array int[10] arr;`

> **Answer: b)**

**3. What is the default value of a `boolean` array element?**
a) 0 b) true c) false d) null

> **Answer: c) false**

**4. What exception is thrown when accessing `arr[arr.length]`?**
a) NullPointerException b) ArrayIndexOutOfBoundsException c) ArithmeticException d) No exception

> **Answer: b)**

**5. In Java, arrays are stored in:**
a) Stack b) Heap c) Register d) Static pool

> **Answer: b) Heap**

**6. Which statement about array size is TRUE?**
a) Can be changed after creation b) Must be fixed at creation time c) Can be negative d) Is always 10

> **Answer: b)**

**7. What does `arr.length` return for `int[] arr = new int[7];`?**
a) 6 b) 7 c) 0 d) Compile Error

> **Answer: b) 7**

**8. What is the default value of an element in `String[] names = new String[3];`?**
a) "" b) null c) 0 d) undefined

> **Answer: b) null**

**9. Which loop CANNOT modify array elements directly?**
a) for loop b) while loop c) for-each (enhanced for) loop d) do-while loop

> **Answer: c)**

**10. Which of the following is a valid array declaration?**
a) `int arr[] = new int[5];` b) `int[] arr = new int[5];` c) Both a and b d) None

> **Answer: c) Both a and b**

---

## 24. University Questions

1. Define array. Explain its characteristics with a suitable example. _(5 marks)_
2. What are the advantages and disadvantages of using arrays over other data structures? _(5 marks)_
3. Explain the memory representation of a one-dimensional array with a diagram. _(10 marks)_
4. Differentiate between array declaration, creation, and initialization with examples. _(5 marks)_
5. What is `ArrayIndexOutOfBoundsException`? Explain with a program demonstrating the error and its fix. _(5 marks)_
6. Write a Java program to declare, initialize, and traverse an array of 10 integers, printing their sum and average. _(10 marks)_
7. Explain default values assigned to array elements for different data types with justification. _(5 marks)_
8. What is the difference between `array.length` and `String.length()`? Explain with examples. _(3 marks)_
9. Describe different ways of initializing an array in Java with code snippets for each. _(10 marks)_
10. Explain why arrays are considered objects in Java, unlike in languages such as C. _(5 marks)_

---

## 25. Viva Questions

1. What is an array, in one line?
2. Where are arrays stored in Java — stack or heap?
3. What is the index of the last element of an array of size `n`?
4. Can you resize an array once it is created?
5. What happens if you access an index beyond the array's bounds?
6. What is the default value of an `int` array element? What about a `String` array?
7. Is `length` a method or a field? What about `length()` in Strings?
8. What is the time complexity of accessing an array element by index?
9. Can an array store elements of different data types?
10. What is the difference between `new int[5]` and `{1,2,3,4,5}`?
11. What does `Arrays.toString()` do, and why is it needed?
12. Why do array indices start at 0 instead of 1?
13. What happens internally when you write `int[] arr = new int[5];`?
14. What is array covariance, and what risk does it introduce?
15. Name one real-world use case where arrays are preferred over `ArrayList`.

---

## 26. Programming Exercises

**1. Sum and Average**
Write a program to read `n` integers into an array and compute their sum and average.

**2. Find Maximum and Minimum**
Write a program to find the largest and smallest elements in an array.

**3. Reverse an Array**
Write a program to reverse the elements of an array without using an extra array.

**4. Linear Search**
Write a program to search for a given element in an array and print its index (or -1 if not found).

**5. Count Even and Odd Numbers**
Write a program to count how many even and odd numbers exist in an array.

**6. Second Largest Element**
Write a program to find the second largest element in an array without sorting.

**7. Array Sorting**
Write a program to sort an array in ascending order using Bubble Sort (without using `Arrays.sort()`).

**8. Remove Duplicates**
Write a program to remove duplicate elements from an array and print the unique elements.

**9. Merge Two Arrays**
Write a program to merge two arrays into a third array.

**10. Sample Solution — Sum and Average**

```java
import java.util.Scanner;

public class SumAverage {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter number of elements: ");
        int n = sc.nextInt();
        int[] arr = new int[n];
        int sum = 0;

        for (int i = 0; i < n; i++) {
            System.out.print("Enter element " + i + ": ");
            arr[i] = sc.nextInt();
            sum += arr[i];
        }

        double average = (double) sum / n;
        System.out.println("Sum = " + sum);
        System.out.println("Average = " + average);
    }
}
```

---

## 27. Challenge Problems

1. **Rotate an Array:** Rotate an array to the left (or right) by `k` positions, in-place, without using extra space (O(1) extra space).
2. **Kadane's Algorithm:** Find the maximum sum of a contiguous subarray.
3. **Rearrange Alternately:** Rearrange an array so positive and negative numbers alternate, maintaining relative order as much as possible.
4. **Dutch National Flag Problem:** Sort an array of 0s, 1s, and 2s in a single pass.
5. **Missing Number:** Given an array of `n-1` distinct integers from `1` to `n`, find the missing number in O(n) time and O(1) space.
6. **Trapping Rain Water:** Given an array representing elevation heights, calculate how much water can be trapped between the bars.
7. **Move Zeroes:** Move all zeroes in an array to the end while maintaining the relative order of non-zero elements, in-place.

---

## 28. Summary

- An **array** is a fixed-size, homogeneous, indexable collection of elements stored in contiguous memory.
- Arrays solve the problem of managing many related values without declaring separate variables for each.
- In Java, arrays are **objects** stored on the **heap**, accessed via a reference stored on the stack.
- Arrays support **declaration**, **creation** (via `new`), and **initialization** (via literals or loops).
- Elements are accessed and updated using **zero-based indexing**.
- Uninitialized array elements get **default values** based on data type.
- Common errors include `ArrayIndexOutOfBoundsException` and `NullPointerException`.
- Arrays offer **O(1) access** but **O(n) insertion/deletion**, and have a **fixed size**.
- The `java.util.Arrays` class provides many useful utility methods for working with arrays.

---

## 29. Key Takeaways

- Index starts at `0`, ends at `length - 1`.
- `length` is a field (no parentheses); `length()` is a method (used with String).
- Arrays are always objects in Java, even for primitives.
- Fixed size — cannot grow/shrink after creation.
- Default values: `0`/`0.0` for numeric types, `false` for boolean, `null` for objects.
- Accessing an invalid index → `ArrayIndexOutOfBoundsException`.
- Using a `null` array reference → `NullPointerException`.
- Prefer `ArrayList` when dynamic resizing is required.

---

## 30. Cheat Sheet

```java
// Declaration
int[] arr;

// Creation
arr = new int[5];

// Declaration + Creation
int[] arr = new int[5];

// Declaration + Initialization (literal)
int[] arr = {1, 2, 3, 4, 5};

// Declaration + Initialization (new keyword)
int[] arr = new int[]{1, 2, 3, 4, 5};

// Access
int x = arr[0];

// Update
arr[0] = 100;

// Length
int len = arr.length;

// Traverse (for loop)
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}

// Traverse (for-each)
for (int val : arr) {
    System.out.println(val);
}

// Common java.util.Arrays methods
import java.util.Arrays;

Arrays.toString(arr);        // convert to printable string
Arrays.sort(arr);            // sort ascending
Arrays.fill(arr, 0);         // fill all elements with 0
Arrays.equals(arr1, arr2);   // compare two arrays
Arrays.copyOf(arr, 10);      // copy/resize array
Arrays.copyOfRange(arr, 1, 4); // copy a sub-range
int idx = Arrays.binarySearch(arr, 5); // search (array must be sorted)

// Multi-dimensional array
int[][] matrix = new int[3][3];
int[][] matrix2 = {{1,2},{3,4}};
matrix[0][1] = 10;

// Exceptions to remember
// ArrayIndexOutOfBoundsException -> invalid index
// NullPointerException -> array or element is null
```

| Concept      | Syntax                 |
| ------------ | ---------------------- |
| Declare      | `int[] arr;`           |
| Create       | `arr = new int[5];`    |
| Literal init | `int[] arr = {1,2,3};` |
| Access       | `arr[i]`               |
| Update       | `arr[i] = value;`      |
| Length       | `arr.length`           |
| Sort         | `Arrays.sort(arr);`    |
| Print        | `Arrays.toString(arr)` |

---

_End of Chapter — Introduction to Arrays (Java)_
