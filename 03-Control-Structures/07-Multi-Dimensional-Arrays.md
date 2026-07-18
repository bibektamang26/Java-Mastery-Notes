# Multidimensional Arrays (Java)

---

## 1. Introduction

A **multidimensional array** is an array of arrays — a data structure that stores data in more than one dimension, such as rows and columns. While a one-dimensional array represents a simple list, a multidimensional array represents structures like **tables, grids, and matrices**.

In Java, multidimensional arrays are implemented as **"arrays of arrays"** rather than a single contiguous block (unlike languages like C). This gives Java multidimensional arrays a unique flexibility — including the ability to create **jagged arrays**, where each row can have a different length.

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand what multidimensional arrays are and why they're needed.
- Declare, create, and initialize 2D arrays using various methods.
- Understand how Java represents multidimensional arrays in memory (arrays of arrays).
- Access, update, and traverse 2D arrays using nested loops.
- Read 2D array data from user input.
- Perform matrix operations: addition, subtraction, multiplication, and transpose.
- Understand and use jagged arrays.
- Identify common errors specific to multidimensional arrays.
- Apply best practices for working with 2D/multidimensional arrays.
- Solve interview questions, MCQs, and programming exercises involving multidimensional arrays.

---

## 3. What is a Multidimensional Array?

A **multidimensional array** is an array whose elements are themselves arrays. The most common form is the **two-dimensional (2D) array**, which can be visualized as a table with **rows** and **columns**.

```java
int[][] arr2D = new int[3][4]; // 3 rows, 4 columns
```

Java also supports **three-dimensional arrays** and beyond:

```java
int[][][] arr3D = new int[2][3][4]; // 2 blocks, each with 3 rows and 4 columns
```

### Formal Definition

> A multidimensional array is a collection of elements organized into multiple indices, where each additional dimension requires an additional index to access an element.

---

## 4. Why Do We Need Multidimensional Arrays?

- To represent **tabular data** — spreadsheets, grids, and tables (rows × columns).
- To represent **matrices** for mathematical computations (addition, multiplication, transformations).
- To model **real-world grids** — chess boards, tic-tac-toe boards, image pixels (2D grid of color values).
- To store **grouped data** — e.g., marks of multiple students across multiple subjects.
- To represent **3D data** — such as RGB image data (`height × width × color channels`) or 3D simulations.

### Example Use Case

```java
// Marks of 3 students in 4 subjects
int[][] marks = {
    {85, 90, 78, 92},  // Student 1
    {70, 88, 95, 60},  // Student 2
    {90, 85, 80, 75}   // Student 3
};
```

---

## 5. Two-Dimensional Arrays

A **2D array** is the most commonly used multidimensional array — essentially, an array where each element is itself a one-dimensional array (a "row").

```java
dataType[][] arrayName;
```

Conceptually:

```
        col0  col1  col2
row0 [   1     2     3  ]
row1 [   4     5     6  ]
row2 [   7     8     9  ]
```

Each element is accessed using **two indices**: `arr[row][col]`.

---

## 6. Memory Representation

Unlike C/C++ (where a 2D array is one contiguous memory block), Java implements a 2D array as an **array of array references** ("array of arrays").

```java
int[][] arr = {
    {1, 2, 3},
    {4, 5}
};
```

### Conceptual Memory Layout

```
Stack             Heap (outer array)          Heap (inner arrays)
arr  --------->   [ ref0 | ref1 ]
                     |       |
                     v       v
                  [1,2,3]  [4,5]
```

**Key Points:**

- `arr` is a reference to an **outer array**, where each element is itself a **reference** to an inner (row) array.
- Each row (inner array) can be located **anywhere** on the heap — they are not guaranteed to be contiguous with each other.
- Each row can have a **different length**, since each row is an independent array object (this enables **jagged arrays** — see Section 20).
- `arr.length` gives the number of **rows**; `arr[i].length` gives the number of **columns in row `i`**.

---

## 7. Declaring 2D Arrays

### Syntax

```java
dataType[][] arrayName;      // preferred
dataType arrayName[][];      // valid, C-style
dataType[] arrayName[];      // valid, mixed style (avoid)
```

### Examples

```java
int[][] matrix;
double[][] grid;
String[][] table;
```

> No memory is allocated at declaration time — `matrix` is simply a `null` reference until created.

---

## 8. Creating 2D Arrays

### Using `new` with fixed dimensions

```java
int[][] matrix = new int[3][4]; // 3 rows, 4 columns — all elements default to 0
```

### Creating rows separately (for jagged arrays)

```java
int[][] jagged = new int[3][];   // 3 rows, but column sizes undecided
jagged[0] = new int[2];
jagged[1] = new int[4];
jagged[2] = new int[3];
```

### Memory Allocation Diagram

```java
int[][] matrix = new int[2][3];
```

```
matrix ---> [ ref0, ref1 ]
              |      |
              v      v
           [0,0,0] [0,0,0]
```

Each row is independently allocated on the heap, and all elements default to `0` (or the type's default value).

---

## 9. Initializing 2D Arrays

### Element-by-Element

```java
int[][] matrix = new int[2][2];
matrix[0][0] = 1;
matrix[0][1] = 2;
matrix[1][0] = 3;
matrix[1][1] = 4;
```

### Array Literal (Declaration + Initialization)

```java
int[][] matrix = {
    {1, 2},
    {3, 4}
};
```

### Using `new` with Explicit Values

```java
int[][] matrix = new int[][]{
    {1, 2},
    {3, 4}
};
```

---

## 10. Different Ways to Initialize

```java
// 1. Static initialization (literal)
int[][] a = {{1,2},{3,4}};

// 2. Dynamic initialization using new + loops
int[][] b = new int[3][3];
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        b[i][j] = i + j;
    }
}

// 3. Using new with explicit values
int[][] c = new int[][]{{1,2,3},{4,5,6}};

// 4. Row-by-row (jagged) initialization
int[][] d = new int[3][];
d[0] = new int[]{1};
d[1] = new int[]{1, 2};
d[2] = new int[]{1, 2, 3};

// 5. Using Arrays.fill() per row (java.util.Arrays)
int[][] e = new int[3][4];
for (int[] row : e) {
    java.util.Arrays.fill(row, 9);
}

// 6. Using nested loops with user input
Scanner sc = new Scanner(System.in);
int[][] f = new int[2][2];
for (int i = 0; i < 2; i++)
    for (int j = 0; j < 2; j++)
        f[i][j] = sc.nextInt();
```

---

## 11. Accessing Elements

Elements are accessed using **two indices**: `arr[row][col]`.

```java
int[][] matrix = {
    {10, 20, 30},
    {40, 50, 60}
};

System.out.println(matrix[0][0]); // 10
System.out.println(matrix[1][2]); // 60
System.out.println(matrix[0][2]); // 30
```

**Getting dimensions:**

```java
int rows = matrix.length;         // 2
int cols = matrix[0].length;      // 3 (columns in row 0)
```

> **Caution:** For jagged arrays, `matrix[i].length` may differ per row — always check each row's length individually rather than assuming uniform columns.

---

## 12. Traversing 2D Arrays

### Basic Nested `for` Loop

```java
int[][] matrix = {{1,2,3},{4,5,6}};

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

### Enhanced `for` Loop

```java
for (int[] row : matrix) {
    for (int val : row) {
        System.out.print(val + " ");
    }
    System.out.println();
}
```

### Using `Arrays.deepToString()`

```java
System.out.println(java.util.Arrays.deepToString(matrix));
// [[1, 2, 3], [4, 5, 6]]
```

---

## 13. Nested Loops

Nested loops are essential for working with 2D arrays — the **outer loop** iterates over rows, and the **inner loop** iterates over columns within that row.

```java
for (int i = 0; i < matrix.length; i++) {        // rows
    for (int j = 0; j < matrix[i].length; j++) {  // columns
        // process matrix[i][j]
    }
}
```

**Common patterns:**

- Row-major traversal (default above): process row by row.
- Column-major traversal: swap loop order — outer loop over columns, inner over rows (requires uniform column count).
- Diagonal traversal: use a single loop where `row == col` (main diagonal) or `row + col == n - 1` (anti-diagonal).

```java
// Main diagonal
for (int i = 0; i < matrix.length; i++) {
    System.out.print(matrix[i][i] + " ");
}
```

---

## 14. User Input

```java
import java.util.Scanner;

public class Input2DArray {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter rows: ");
        int rows = sc.nextInt();
        System.out.print("Enter columns: ");
        int cols = sc.nextInt();

        int[][] matrix = new int[rows][cols];

        System.out.println("Enter elements:");
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                matrix[i][j] = sc.nextInt();
            }
        }

        System.out.println("You entered:");
        for (int[] row : matrix) {
            System.out.println(java.util.Arrays.toString(row));
        }
    }
}
```

---

## 15. Matrix Representation

A **matrix** in mathematics maps directly onto a 2D array in Java, where `matrix[i][j]` represents the element in row `i`, column `j` (both 0-indexed in Java, though mathematically matrices are usually 1-indexed).

```java
// A 3x3 matrix
int[][] A = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

Matrices are widely used in graphics, scientific computing, machine learning, and solving systems of linear equations — making 2D arrays a critical data structure to master.

---

## 16. Matrix Addition

Two matrices of the **same dimensions** can be added by summing corresponding elements.

```java
public class MatrixAddition {
    public static void main(String[] args) {
        int[][] A = {{1, 2}, {3, 4}};
        int[][] B = {{5, 6}, {7, 8}};
        int rows = A.length, cols = A[0].length;

        int[][] result = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                result[i][j] = A[i][j] + B[i][j];
            }
        }

        for (int[] row : result) {
            System.out.println(java.util.Arrays.toString(row));
        }
        // [6, 8]
        // [10, 12]
    }
}
```

---

## 17. Matrix Subtraction

Similar to addition, but subtracting corresponding elements. Matrices must have the same dimensions.

```java
public class MatrixSubtraction {
    public static void main(String[] args) {
        int[][] A = {{5, 6}, {7, 8}};
        int[][] B = {{1, 2}, {3, 4}};
        int rows = A.length, cols = A[0].length;

        int[][] result = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                result[i][j] = A[i][j] - B[i][j];
            }
        }

        for (int[] row : result) {
            System.out.println(java.util.Arrays.toString(row));
        }
        // [4, 4]
        // [4, 4]
    }
}
```

---

## 18. Matrix Multiplication

For matrix multiplication `A x B`, the **number of columns in A** must equal the **number of rows in B**. The result has dimensions `(rows of A) x (columns of B)`.

```java
public class MatrixMultiplication {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2, 3},
            {4, 5, 6}
        }; // 2x3

        int[][] B = {
            {7, 8},
            {9, 10},
            {11, 12}
        }; // 3x2

        int rowsA = A.length, colsA = A[0].length;
        int colsB = B[0].length;

        int[][] result = new int[rowsA][colsB]; // 2x2

        for (int i = 0; i < rowsA; i++) {
            for (int j = 0; j < colsB; j++) {
                int sum = 0;
                for (int k = 0; k < colsA; k++) {
                    sum += A[i][k] * B[k][j];
                }
                result[i][j] = sum;
            }
        }

        for (int[] row : result) {
            System.out.println(java.util.Arrays.toString(row));
        }
        // [58, 64]
        // [139, 154]
    }
}
```

**Time Complexity:** O(n³) for square matrices of size `n x n` using the standard triple-loop algorithm.

---

## 19. Matrix Transpose

The **transpose** of a matrix flips it over its diagonal — rows become columns and vice versa. If `A` is `m x n`, its transpose `Aᵀ` is `n x m`.

```java
public class MatrixTranspose {
    public static void main(String[] args) {
        int[][] A = {
            {1, 2, 3},
            {4, 5, 6}
        }; // 2x3

        int rows = A.length, cols = A[0].length;
        int[][] transpose = new int[cols][rows]; // 3x2

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                transpose[j][i] = A[i][j];
            }
        }

        for (int[] row : transpose) {
            System.out.println(java.util.Arrays.toString(row));
        }
        // [1, 4]
        // [2, 5]
        // [3, 6]
    }
}
```

---

## 20. Jagged Arrays

A **jagged array** is a multidimensional array in which each row can have a **different number of columns** — since Java implements 2D arrays as arrays of independent row-arrays, this is naturally supported.

```java
int[][] jagged = new int[3][];
jagged[0] = new int[]{1};
jagged[1] = new int[]{1, 2};
jagged[2] = new int[]{1, 2, 3};

for (int[] row : jagged) {
    System.out.println(java.util.Arrays.toString(row));
}
// [1]
// [1, 2]
// [1, 2, 3]
```

### When to Use Jagged Arrays

- Representing **triangular data** (e.g., Pascal's Triangle).
- Storing data where each entity has a variable number of attributes (e.g., students with a variable number of grades).
- Saving memory when a full rectangular grid would waste space.

### Pascal's Triangle Example

```java
public class PascalsTriangle {
    public static void main(String[] args) {
        int n = 5;
        int[][] triangle = new int[n][];

        for (int i = 0; i < n; i++) {
            triangle[i] = new int[i + 1];
            triangle[i][0] = triangle[i][i] = 1;
            for (int j = 1; j < i; j++) {
                triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j];
            }
        }

        for (int[] row : triangle) {
            System.out.println(java.util.Arrays.toString(row));
        }
    }
}
```

---

## 21. Advantages

- **Natural representation** of tabular, grid, and matrix data.
- **Flexible structure** — jagged arrays allow variable row lengths, unlike strict rectangular arrays in other languages.
- **Efficient random access** — any element accessible in O(1) via `arr[i][j]`.
- Useful in **image processing**, **game boards**, **scientific computing**, and **graph adjacency matrices**.
- Supports **any number of dimensions** (2D, 3D, and beyond) for complex data modeling.

---

## 22. Disadvantages

- **Memory overhead** — each row is a separate object, requiring extra reference storage compared to a truly contiguous block.
- **Cache performance** — since rows may not be contiguous in memory, traversal can be less cache-friendly than in languages with truly contiguous 2D arrays (like C).
- **Fixed size** — dimensions cannot change once created (same limitation as 1D arrays).
- **Complexity** — nested loops and multiple indices increase the chance of logical errors (e.g., mixing up row/column indices).
- **Risk of `NullPointerException`** — with jagged arrays, forgetting to initialize a row (`arr[i] = new int[...]`) leaves it `null`.

---

## 23. Common Errors

### ArrayIndexOutOfBoundsException (row or column)

```java
int[][] matrix = new int[2][3];
System.out.println(matrix[2][0]); // Exception — only rows 0 and 1 exist
System.out.println(matrix[0][3]); // Exception — only columns 0,1,2 exist
```

### NullPointerException (uninitialized rows)

```java
int[][] jagged = new int[3][];  // rows are all null initially
System.out.println(jagged[0][0]); // NullPointerException — jagged[0] is null
```

**Fix:**

```java
jagged[0] = new int[]{1, 2, 3}; // initialize row before accessing
```

### Mismatched Dimensions in Matrix Operations

```java
int[][] A = {{1,2},{3,4}};       // 2x2
int[][] B = {{1,2,3},{4,5,6}};   // 2x3
// A + B is invalid — dimensions must match for addition/subtraction
// A * B requires A's columns == B's rows for multiplication
```

### Confusing Rows and Columns

A very common bug is swapping `i` and `j`, e.g., writing `matrix[j][i]` instead of `matrix[i][j]`, especially during transpose or multiplication — always double-check index order.

---

## 24. Best Practices

1. Always use `matrix.length` (rows) and `matrix[i].length` (columns for row `i`) instead of hardcoding dimensions.
2. When working with jagged arrays, always check `matrix[i] != null` before accessing elements, or ensure every row is explicitly initialized.
3. Use `Arrays.deepToString()` for printing 2D arrays instead of manual nested loops when just debugging.
4. Validate matrix dimensions before performing addition, subtraction, or multiplication.
5. Use clear variable names like `rows`, `cols`, `i`, `j` consistently to avoid index confusion.
6. For performance-critical code with large matrices, consider cache-friendly traversal order (row-major, matching Java's storage layout).
7. Prefer helper methods (e.g., `printMatrix()`, `addMatrices()`) to keep matrix logic reusable and readable.
8. Use `Arrays.deepEquals()` to compare two 2D arrays for equality (not `==` or `Arrays.equals()`, which won't work correctly for nested arrays).

---

## 25. Interview Corner

**Q1. How does Java implement multidimensional arrays internally?**
As **arrays of arrays** — the outer array holds references to inner (row) arrays, which may be located anywhere on the heap, unlike a single contiguous block in C/C++.

**Q2. What is a jagged array, and why is it possible in Java?**
A jagged array is a 2D array where rows have different lengths. It's possible because each row is an independently created array object.

**Q3. What is the time complexity of matrix multiplication using the standard algorithm?**
O(n³) for two `n x n` matrices, due to the triple nested loop (row × column × shared dimension).

**Q4. How do you find the number of rows and columns of a 2D array?**
`matrix.length` gives the number of rows; `matrix[i].length` gives the number of columns in row `i` (important: check per-row for jagged arrays).

**Q5. What is the difference between `Arrays.toString()` and `Arrays.deepToString()`?**
`Arrays.toString()` works for 1D arrays and prints inner arrays as memory addresses/hashcodes when applied to 2D arrays. `Arrays.deepToString()` recursively formats nested arrays, correctly printing 2D (and deeper) array contents.

**Q6. Can you have a 3D array in Java? Give an example.**
Yes: `int[][][] cube = new int[2][3][4];` — represents 2 blocks, each with 3 rows and 4 columns.

**Q7. Why might row-major traversal be more efficient than column-major traversal in Java?**
Because rows are stored as separate contiguous blocks; accessing all elements of one row sequentially benefits from CPU cache locality, whereas jumping between rows for column-major traversal does not.

**Q8. How do you transpose a non-square matrix?**
Create a new matrix with dimensions swapped (`cols x rows` instead of `rows x cols`), then assign `transpose[j][i] = original[i][j]`.

---

## 26. MCQs

**1. How is a 2D array implemented internally in Java?**
a) Single contiguous block b) Array of arrays c) Linked list of arrays d) Hash map

> **Answer: b)**

**2. What does `matrix.length` return for a 2D array?**
a) Total number of elements b) Number of columns c) Number of rows d) Number of dimensions

> **Answer: c) Number of rows**

**3. Which of these creates a valid jagged array?**
a) `int[][] arr = new int[3][3];`  
b) `int[][] arr = new int[3][];`  
c) `int[][] arr = new int[][3];`  
d) `int[][] arr = new int[];`

> **Answer: b)**

**4. For matrix multiplication A x B to be valid:**
a) A and B must have the same dimensions  
b) Number of columns in A must equal number of rows in B  
c) Number of rows in A must equal number of rows in B  
d) A and B must both be square matrices

> **Answer: b)**

**5. What method correctly prints the contents of a 2D array?**
a) `Arrays.toString(matrix)` b) `Arrays.deepToString(matrix)` c) `matrix.toString()` d) `System.out.println(matrix)`

> **Answer: b)**

**6. What happens when you access an uninitialized row in a jagged array?**
a) Returns 0 b) Returns null c) Throws NullPointerException d) Throws ArrayIndexOutOfBoundsException

> **Answer: c)**

**7. What is the time complexity of standard matrix multiplication for two n x n matrices?**
a) O(n) b) O(n²) c) O(n³) d) O(log n)

> **Answer: c)**

**8. What is the transpose of matrix `{{1,2},{3,4}}`?**
a) `{{1,2},{3,4}}` b) `{{4,3},{2,1}}` c) `{{1,3},{2,4}}` d) `{{2,1},{4,3}}`

> **Answer: c)**

**9. Which statement about jagged arrays is TRUE?**
a) All rows must have equal length b) Rows can have different lengths c) Only supported in 3D arrays d) Not supported in Java

> **Answer: b)**

**10. What does `matrix[i][j]` represent?**
a) Element at row j, column i b) Element at row i, column j c) Total elements d) Row length

> **Answer: b)**

---

## 27. University Questions

1. Define a multidimensional array. Explain how it differs from a one-dimensional array. _(5 marks)_
2. Explain how Java represents 2D arrays internally, with a memory diagram. _(10 marks)_
3. Write a Java program to add and subtract two matrices. _(10 marks)_
4. Write a Java program to multiply two matrices and explain the algorithm's time complexity. _(10 marks)_
5. Write a Java program to find the transpose of a matrix. _(5 marks)_
6. What is a jagged array? Explain with a suitable example and program. _(5 marks)_
7. Explain the difference between `Arrays.toString()` and `Arrays.deepToString()` with examples. _(3 marks)_
8. Discuss common exceptions that occur while working with multidimensional arrays, with examples. _(5 marks)_
9. Compare row-major and column-major traversal in terms of performance in Java. _(5 marks)_

---

## 28. Viva Questions

1. What is a multidimensional array?
2. How does Java store 2D arrays internally?
3. What is the difference between `matrix.length` and `matrix[0].length`?
4. What is a jagged array? Give a real-world example where it's useful.
5. Can two matrices of different dimensions be added? Why or why not?
6. What is the condition for two matrices to be multiplied?
7. How do you find the transpose of a matrix?
8. What exception occurs when accessing an uninitialized row of a jagged array?
9. What does `Arrays.deepToString()` do differently from `Arrays.toString()`?
10. Can Java arrays have more than 2 dimensions? Give an example.
11. Why is row-major traversal generally faster in Java?
12. How would you print a 2D array without using `Arrays.deepToString()`?
13. What's the output shape of multiplying an `m x n` matrix with an `n x p` matrix?
14. What happens if you try to access `matrix[row][col]` with an invalid row or column index?
15. How do you compare two 2D arrays for equality?

---

## 29. Programming Exercises

1. Write a program to input a matrix and display it in proper row-column format.
2. Write a program to find the sum of all elements in a 2D array.
3. Write a program to find the sum of each row and each column of a matrix separately.
4. Write a program to check if a given matrix is a **symmetric matrix** (`A == Aᵀ`).
5. Write a program to find the largest and smallest elements in a 2D array.
6. Write a program to add two matrices of the same dimensions.
7. Write a program to multiply two matrices with compatible dimensions.
8. Write a program to find the transpose of a matrix without using a new array (only possible for square matrices, in-place).
9. Write a program to create and print a jagged array representing Pascal's Triangle.
10. **Sample Solution — Sum of Each Row and Column**

```java
public class RowColumnSum {
    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        int rows = matrix.length;
        int cols = matrix[0].length;

        // Row sums
        for (int i = 0; i < rows; i++) {
            int rowSum = 0;
            for (int j = 0; j < cols; j++) {
                rowSum += matrix[i][j];
            }
            System.out.println("Sum of row " + i + " = " + rowSum);
        }

        // Column sums
        for (int j = 0; j < cols; j++) {
            int colSum = 0;
            for (int i = 0; i < rows; i++) {
                colSum += matrix[i][j];
            }
            System.out.println("Sum of column " + j + " = " + colSum);
        }
    }
}
```

---

## 30. Challenge Problems

1. **Spiral Matrix Traversal:** Print the elements of a matrix in spiral (clockwise) order.
2. **Rotate Matrix by 90 Degrees:** Rotate a square matrix in-place by 90 degrees clockwise, without using extra space.
3. **Diagonal Sum:** Compute the sum of both the main diagonal and the anti-diagonal of a square matrix.
4. **Search in a Sorted 2D Matrix:** Given a matrix where each row and column is sorted, search for a target value in better than O(rows × cols) time.
5. **Identity Matrix Check:** Write a program to determine if a given square matrix is an identity matrix.
6. **Sparse Matrix Representation:** Convert a matrix with many zero values into a compact representation storing only non-zero elements (row, column, value).
7. **Saddle Point:** Find a "saddle point" in a matrix — an element that is the minimum in its row and the maximum in its column.
8. **Toeplitz Matrix Check:** Determine whether a matrix is a Toeplitz matrix (every diagonal from top-left to bottom-right has the same value).

---

## 31. Summary

- A **multidimensional array** stores data across more than one dimension, most commonly as a **2D array (rows × columns)**.
- Java implements multidimensional arrays as **arrays of arrays**, not as one contiguous memory block.
- 2D arrays are declared with `dataType[][]`, created with `new dataType[rows][cols]`, and accessed via `arr[row][col]`.
- **Jagged arrays** allow rows of different lengths, since each row is an independently allocated array.
- Matrix operations — addition, subtraction, multiplication, and transpose — map naturally onto 2D array operations using nested loops.
- Standard matrix multiplication has **O(n³)** time complexity for `n x n` matrices.
- Common errors include row/column index confusion, `ArrayIndexOutOfBoundsException`, and `NullPointerException` from uninitialized jagged rows.
- `Arrays.deepToString()` and `Arrays.deepEquals()` should be used (instead of their non-deep counterparts) for printing and comparing multidimensional arrays.

---

## 32. Key Takeaways

- `matrix.length` = number of rows; `matrix[i].length` = number of columns in row `i`.
- Matrix addition/subtraction require **identical dimensions**; multiplication requires **columns of A = rows of B**.
- Use nested loops for traversal — outer loop for rows, inner loop for columns.
- Jagged arrays are a powerful, memory-efficient feature unique to Java's array-of-arrays model.
- Always initialize each row of a jagged array before accessing it.
- Use `Arrays.deepToString()`/`Arrays.deepEquals()` for correct printing/comparison of nested arrays.

---

## 33. Cheat Sheet

```java
// Declaration
int[][] matrix;

// Creation
matrix = new int[3][4];         // 3 rows, 4 columns
int[][] jagged = new int[3][];  // jagged — rows sized individually

// Initialization
int[][] matrix = {{1,2},{3,4}};

// Access
int val = matrix[1][0];

// Update
matrix[1][0] = 99;

// Dimensions
int rows = matrix.length;
int cols = matrix[0].length;

// Traversal
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}

// Printing
System.out.println(java.util.Arrays.deepToString(matrix));

// Comparing
boolean same = java.util.Arrays.deepEquals(matrix1, matrix2);

// Matrix Addition
result[i][j] = A[i][j] + B[i][j];

// Matrix Subtraction
result[i][j] = A[i][j] - B[i][j];

// Matrix Multiplication (A: m×n, B: n×p, result: m×p)
for (int i = 0; i < m; i++)
    for (int j = 0; j < p; j++)
        for (int k = 0; k < n; k++)
            result[i][j] += A[i][k] * B[k][j];

// Matrix Transpose
transpose[j][i] = original[i][j];

// Jagged Row Initialization
jagged[0] = new int[]{1, 2, 3};
```

| Concept                  | Syntax                     |
| ------------------------ | -------------------------- |
| Declare                  | `int[][] arr;`             |
| Create                   | `arr = new int[r][c];`     |
| Rows                     | `arr.length`               |
| Columns (row i)          | `arr[i].length`            |
| Access                   | `arr[i][j]`                |
| Print (deep)             | `Arrays.deepToString(arr)` |
| Compare (deep)           | `Arrays.deepEquals(a, b)`  |
| Multiplication condition | `A.cols == B.rows`         |

---

_End of Chapter — Multidimensional Arrays (Java)_
