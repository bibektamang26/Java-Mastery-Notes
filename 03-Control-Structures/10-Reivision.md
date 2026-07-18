# Chapter 3 Revision — Control Structures & Arrays (Java)

---

## 1. Chapter Overview

### What This Chapter Covers

Chapter 3 covers the core building blocks that control **how a Java program executes** and **how data is organized**:

- **Decision-making statements** (`if`, `if-else`, `else-if` ladder, `switch`) — allowing a program to choose between different paths of execution.
- **Loops** (`while`, `do-while`, `for`, enhanced `for`) — allowing repetitive tasks to be automated instead of writing repeated code.
- **Branching statements** (`break`, `continue`, `return`) — allowing fine control over loop and method execution flow.
- **Arrays** (one-dimensional and multidimensional) — allowing multiple values of the same type to be stored, accessed, and processed efficiently.

### Importance of Control Structures and Arrays

Without control structures, every Java program would execute strictly top-to-bottom with no ability to make decisions or repeat actions — extremely limiting for real-world problems. Without arrays, storing and processing collections of related data (marks, prices, inventory, matrices) would require unmanageable amounts of repetitive code. Together, these concepts form the **foundation** for every subsequent topic in Java — including Classes and Objects, Collections, and Algorithms — making them essential to master before progressing further.

---

## 2. Learning Outcomes

After revising this chapter, you should be able to:

- Use decision-making statements to control program flow based on conditions.
- Write loops to repeat blocks of code efficiently.
- Control loop execution using `break`, `continue`, and `return`.
- Work with one-dimensional and multidimensional arrays.
- Traverse arrays using various loop constructs, including the enhanced `for` loop.
- Solve programming problems that combine decision-making, loops, and arrays together.

---

## 3. Important Definitions

| Term                      | Definition                                                                                                                    |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **Control Structure**     | A programming construct that determines the order in which statements are executed — sequential, decision-making, or looping. |
| **Sequential Execution**  | The default flow of a program, where statements execute one after another, in the order they are written.                     |
| **Decision Making**       | A control structure (`if`, `switch`) that selects one execution path among multiple, based on a condition.                    |
| **Loop**                  | A control structure that repeats a block of code as long as a condition holds true.                                           |
| **Branching**             | Statements (`break`, `continue`, `return`) that alter the normal sequential flow of loops or methods.                         |
| **Array**                 | A fixed-size, homogeneous, indexable collection of elements stored in contiguous memory.                                      |
| **One-Dimensional Array** | An array where each element is accessed using a single index — represents a simple linear list.                               |
| **Two-Dimensional Array** | An array of arrays, accessed using two indices (row and column) — represents tabular/matrix data.                             |
| **Enhanced for Loop**     | A simplified loop (introduced in Java 5) for traversing arrays/collections without manual index management.                   |
| **Index**                 | The numeric position of an element within an array, starting at `0`.                                                          |
| **Element**               | An individual value stored within an array.                                                                                   |
| **Iteration**             | One complete execution of a loop's body.                                                                                      |

---

## 4. Syntax Cheat Sheet

### if

```java
if (condition) {
    // executes only if condition is true
}
```

### if-else

```java
if (condition) {
    // executes if condition is true
} else {
    // executes if condition is false
}
```

### else-if Ladder

```java
if (condition1) {
    // executes if condition1 is true
} else if (condition2) {
    // executes if condition1 is false and condition2 is true
} else {
    // executes if none of the above are true
}
```

### switch

```java
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // code if no case matches
}
```

### while

```java
while (condition) {
    // executes repeatedly while condition is true
}
```

### do-while

```java
do {
    // executes at least once, then repeats while condition is true
} while (condition);
```

### for

```java
for (initialization; condition; update) {
    // executes repeatedly while condition is true
}
```

### Enhanced for

```java
for (dataType variable : array) {
    // executes once per element in array/collection
}
```

### break

```java
break; // exits the nearest enclosing loop or switch immediately
```

### continue

```java
continue; // skips the rest of the current iteration, moves to the next one
```

### return

```java
return;        // exits the current method (void)
return value;  // exits the current method, returning a value
```

---

## 5. Comparison Tables

### if vs switch

| Aspect               | `if`                                       | `switch`                                                     |
| -------------------- | ------------------------------------------ | ------------------------------------------------------------ |
| Condition Type       | Boolean expressions, ranges, complex logic | Single expression matched against discrete constant values   |
| Data Types Supported | Any boolean-evaluable expression           | `byte`, `short`, `char`, `int`, `String`, `enum`             |
| Readability          | Better for few or complex conditions       | Better for many discrete, fixed-value choices                |
| Fall-through         | Not applicable                             | Occurs if `break` is omitted                                 |
| Range Checks         | Supported (`x > 10 && x < 20`)             | Not directly supported                                       |
| Performance          | Sequential evaluation                      | Can be optimized internally (jump table) for large case sets |

---

### while vs do-while vs for

| Aspect                   | `while`                                        | `do-while`                                      | `for`                                     |
| ------------------------ | ---------------------------------------------- | ----------------------------------------------- | ----------------------------------------- |
| Condition Check          | Before the loop body                           | After the loop body                             | Before the loop body                      |
| Minimum Executions       | 0 (if condition false initially)               | 1 (always runs at least once)                   | 0                                         |
| Best Use Case            | Unknown number of iterations, condition-driven | At least one guaranteed execution (e.g., menus) | Known number of iterations, counter-based |
| Initialization/Update    | Managed separately, outside the loop header    | Managed separately, outside the loop header     | Combined in the loop header               |
| Readability for Counting | Less compact                                   | Less compact                                    | Most compact for counter-based loops      |

---

### break vs continue vs return

| Aspect               | `break`                                               | `continue`                                                              | `return`                                                        |
| -------------------- | ----------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------- |
| Effect               | Exits the nearest enclosing loop or `switch` entirely | Skips the remaining code in the current iteration, proceeds to the next | Exits the current method entirely, optionally returning a value |
| Scope                | Loop / switch                                         | Loop                                                                    | Method                                                          |
| Remaining Iterations | Terminated                                            | Continue (except current one)                                           | Terminated (method ends)                                        |
| Common Use           | Exiting a search loop early upon finding a match      | Skipping invalid/unwanted values while still processing the rest        | Returning a computed result or exiting early on a condition     |

---

### Traditional for vs Enhanced for

| Aspect            | Traditional `for`                                       | Enhanced `for`                                |
| ----------------- | ------------------------------------------------------- | --------------------------------------------- |
| Index Access      | Yes                                                     | No                                            |
| Modify Elements   | Yes (via index)                                         | No (primitives); object fields only (objects) |
| Reverse Traversal | Yes                                                     | No                                            |
| Readability       | Moderate (more boilerplate)                             | High (concise, declarative)                   |
| Works With        | Arrays, indexable structures                            | Arrays, `Iterable` collections                |
| Best Use Case     | Index-based logic, modification, custom iteration order | Simple read-only traversal                    |

---

### One-Dimensional vs Two-Dimensional Arrays

| Aspect                | 1D Array                | 2D Array                                         |
| --------------------- | ----------------------- | ------------------------------------------------ |
| Structure             | Linear list of elements | Grid/table of rows and columns                   |
| Indices Needed        | One (`arr[i]`)          | Two (`arr[i][j]`)                                |
| Memory Model          | Single contiguous block | Array of row-array references (arrays of arrays) |
| Declaration           | `int[] arr;`            | `int[][] arr;`                                   |
| Common Use Case       | Lists (marks, prices)   | Matrices, tables, grids                          |
| Row Length Uniformity | N/A                     | Rows can differ in length (jagged arrays)        |

---

## 7. Memory Diagrams

### Array Representation

```text
Index:      0    1    2    3
          +----+----+----+----+
Value:    | 10 | 20 | 30 | 40 |
          +----+----+----+----+
Address:   100  104  108  112   (assuming int = 4 bytes)
```

The array reference (e.g., `arr`) is stored on the **stack**, pointing to the base address of the block on the **heap**. Any element's address is computed as `base_address + (index * element_size)`, giving O(1) access time.

### 2D Array

```
matrix ---> [ ref0 | ref1 | ref2 ]     (outer array on heap)
               |      |      |
               v      v      v
            [1,2,3] [4,5,6] [7,8,9]    (independent row arrays on heap)
```

Java implements 2D arrays as **arrays of arrays** — the outer array stores references to independently-allocated row arrays, which need not be contiguous with one another. `matrix.length` gives the row count; `matrix[i].length` gives the column count of row `i`.

### Enhanced for Loop (Internal Behavior)

```
For Arrays:
for (int val : arr) { ... }
   compiles roughly to:
for (int i = 0; i < arr.length; i++) {
    int val = arr[i];
    ...
}

For Collections:
for (String s : list) { ... }
   compiles roughly to:
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    String s = it.next();
    ...
}
```

---

## 8. Frequently Used Methods and Properties

### Array Property

```java
array.length     // field (no parentheses) — returns number of elements/rows
```

### Scanner (for reading user input)

```java
Scanner sc = new Scanner(System.in);

sc.nextInt();      // reads an int
sc.nextDouble();   // reads a double
sc.next();         // reads a single token (String, no spaces)
sc.nextLine();     // reads an entire line (including spaces)
```

| Method         | Reads             | Notes                                                                            |
| -------------- | ----------------- | -------------------------------------------------------------------------------- |
| `nextInt()`    | Integer value     | Throws `InputMismatchException` on invalid input                                 |
| `nextDouble()` | Decimal value     | Same caution as above                                                            |
| `next()`       | Single word/token | Stops at whitespace                                                              |
| `nextLine()`   | Full line of text | Often needs a buffer-clearing `nextLine()` call after `nextInt()`/`nextDouble()` |

---

## 9. Common Errors

- **Missing `break`** — causes unintended **fall-through** in `switch` statements, executing subsequent cases unintentionally.
- **Infinite loops** — occur when the loop's condition never becomes false (e.g., forgetting to update the loop variable).
- **Wrong loop condition** — using `<=` instead of `<` (or vice versa), causing off-by-one behavior or skipped iterations.
- **`ArrayIndexOutOfBoundsException`** — accessing an index outside the valid range `[0, length-1]`.
- **`NullPointerException`** — accessing an array (or its object elements) that is `null`, often from an uninitialized jagged array row.
- **Off-by-one error** — a specific subset of loop condition mistakes, frequently seen when translating mathematical ranges into 0-indexed loops.

```java
// Example: Missing break (fall-through)
switch (day) {
    case 1:
        System.out.println("Monday");
    case 2:  // executes too, since no break above!
        System.out.println("Tuesday");
}

// Example: Infinite loop
int i = 0;
while (i < 5) {
    System.out.println(i);
    // forgot i++ — infinite loop!
}

// Example: Off-by-one
int[] arr = {1, 2, 3};
for (int i = 0; i <= arr.length; i++) {  // BUG: should be i < arr.length
    System.out.println(arr[i]);           // throws exception at i = 3
}
```

---

## 10. Best Practices

1. **Use meaningful variable names** — e.g., `studentMarks` instead of `arr`, `isEligible` instead of `flag`.
2. **Keep loops simple** — avoid deeply nested logic inside loop bodies; extract complex logic into helper methods.
3. **Use enhanced for loop for read-only traversal** — reserve the traditional `for` loop for cases needing the index, reverse order, or modification.
4. **Validate array indices** — always check bounds before accessing an index, especially when derived from user input or calculations.
5. **Avoid unnecessary nested loops** — nested loops multiply time complexity (O(n²), O(n³)); use them only when genuinely required (e.g., matrix operations).
6. Always include a `break` in `switch` cases unless intentional fall-through is required (and comment it clearly if so).
7. Double-check loop conditions (`<` vs `<=`) whenever translating between mathematical ranges and 0-indexed array logic.
8. Prefer `Arrays.toString()` / `Arrays.deepToString()` for debugging array contents instead of manual print loops.

---

## 11. Frequently Asked University Questions

### Short Questions

1. What is a loop?
2. What is an array?
3. Difference between `while` and `do-while`.
4. Difference between `break` and `continue`.
5. Advantages of arrays.

### Long Questions

1. Explain all looping statements (`while`, `do-while`, `for`) with syntax and examples. _(10 marks)_
2. Explain one-dimensional arrays — declaration, creation, initialization, and traversal — with examples. _(10 marks)_
3. Explain two-dimensional arrays with memory representation and a matrix example program. _(10 marks)_
4. Compare `while`, `do-while`, and `for` loops in terms of syntax, use cases, and execution behavior. _(10 marks)_
5. Explain branching statements (`break`, `continue`, `return`) with example programs demonstrating each. _(10 marks)_

---

## 12. Viva Questions

1. What is a control structure?
2. Why do we need loops?
3. What is an infinite loop?
4. What is array indexing?
5. Can array size be changed?
6. What is the enhanced for loop?
7. Difference between `break` and `return`?
8. Difference between `continue` and `break`?
9. What happens if an index exceeds the array length?
10. What is `ArrayIndexOutOfBoundsException`?

---

## 13. Interview Corner

**Q1. Why are arrays fixed in size?**
Because arrays allocate a single, contiguous block of memory (or, in Java's case, contiguous references) at creation time, based on the specified size — resizing would require allocating an entirely new block, which is why dynamic structures like `ArrayList` exist for cases needing flexible sizing.

**Q2. Why is the enhanced for loop introduced?**
To simplify the common pattern of traversing every element of an array or collection, removing the need for manual index or iterator management and reducing bugs like off-by-one errors.

**Q3. Can we modify array elements using enhanced for?**
For primitive-type elements, no — the loop variable holds a copy of the value, so reassigning it doesn't affect the array. For object arrays, the object's fields can be modified through the reference, but the array slot itself can't be replaced via the loop variable.

**Q4. Which loop is fastest?**
There is no meaningful performance difference between `while`, `do-while`, and `for` loops in Java — they all compile to similar bytecode. The choice is a matter of readability and appropriateness for the task, not speed.

**Q5. Difference between `while` and `for`?**
Both check their condition before execution, but `for` combines initialization, condition, and update in a single, compact header — ideal when the number of iterations is known upfront. `while` is more suited to condition-driven loops where the iteration count isn't predetermined.

**Q6. What happens if `break` is omitted in `switch`?**
Execution "falls through" to the next `case` block(s), continuing to execute their code regardless of whether their value matches, until a `break` (or the end of the `switch`) is reached.

**Q7. Difference between `==` and `.equals()` (basic understanding)?**
`==` compares **references** (memory addresses) for objects, or raw values for primitives. `.equals()` (when properly overridden, as in `String`) compares the actual **content/value** of two objects. For arrays specifically, use `Arrays.equals()` rather than either of these for content comparison.

---

## 14. Important Programs to Practice

### Decision Making

- Even/Odd
- Largest of Three
- Grade Calculator
- Calculator using `switch`

### Loops

- Factorial
- Fibonacci
- Prime Number
- Armstrong Number
- Palindrome
- Reverse Number
- Multiplication Table

### Arrays

- Sum
- Average
- Maximum
- Minimum
- Reverse Array
- Matrix Addition
- Matrix Transpose

> _(Full solutions with problem statements, algorithms, code, sample I/O, explanations, and time complexity for all of these are available in `09-Chapter-3-Practice-Problems.md`.)_

---

## 15. MCQs for Revision

**1. Which statement correctly describes `if-else`?**
a) Executes both blocks always b) Executes one block based on a condition c) Only works with integers d) Requires a `break` statement

> **Answer: b)**

**2. What happens if a `switch` case has no `break`?**
a) Compile error b) Only that case executes c) Execution falls through to the next case(s) d) Program terminates

> **Answer: c)**

**3. Which loop guarantees at least one execution?**
a) `for` b) `while` c) `do-while` d) Enhanced `for`

> **Answer: c)**

**4. What is the index of the first element of an array?**
a) 1 b) -1 c) 0 d) Depends on the array

> **Answer: c)**

**5. What does `break` do inside a loop?**
a) Skips current iteration b) Exits the loop entirely c) Restarts the loop d) Pauses the loop

> **Answer: b)**

**6. What does `continue` do inside a loop?**
a) Exits the loop b) Skips remaining code in current iteration, moves to next c) Terminates the program d) Restarts from the beginning

> **Answer: b)**

**7. Which exception occurs when accessing an invalid array index?**
a) NullPointerException b) ArrayIndexOutOfBoundsException c) ArithmeticException d) ClassCastException

> **Answer: b)**

**8. Which of these is NOT a valid loop in Java?**
a) `for` b) `while` c) `repeat-until` d) `do-while`

> **Answer: c)**

**9. What is the default value of an `int` array element?**
a) null b) undefined c) 0 d) 1

> **Answer: c)**

**10. Which loop is best suited when the number of iterations is known in advance?**
a) `while` b) `do-while` c) `for` d) Infinite loop

> **Answer: c)**

**11. Where are arrays stored in Java?**
a) Stack b) Heap c) Register d) Static pool

> **Answer: b)**

**12. What is the correct syntax for the enhanced for loop?**
a) `for(i=0; i<arr.length; i++)` b) `for(dataType var : array)` c) `foreach(var in array)` d) `for each var in array`

> **Answer: b)**

**13. What does `array.length` represent for a 2D array?**
a) Total number of elements b) Number of columns c) Number of rows d) Number of dimensions

> **Answer: c)**

**14. Which statement about `return` is TRUE?**
a) It only works inside loops b) It exits the current method c) It exits only the innermost loop d) It skips the current iteration

> **Answer: b)**

**15. What is the time complexity of accessing an array element by index?**
a) O(1) b) O(n) c) O(log n) d) O(n²)

> **Answer: a)**

**16. Which of the following correctly checks if a year is a leap year?**
a) `year % 4 == 0`  
b) `(year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)`  
c) `year % 400 == 0`  
d) `year % 100 == 0`

> **Answer: b)**

**17. What happens when you use `arr[-1]` in Java?**
a) Returns the last element b) Returns 0 c) Throws ArrayIndexOutOfBoundsException d) Compile error

> **Answer: c)**

**18. Which construct is best for traversing a collection without needing the index?**
a) Traditional `for` b) Enhanced `for` c) `do-while` d) Nested loop

> **Answer: b)**

**19. What is the output of `5 % 2` in Java?**
a) 2 b) 2.5 c) 1 d) 0

> **Answer: c)**

**20. Which loop type is generally used for "unknown number of iterations, condition-driven" scenarios?**
a) `for` b) `while` c) Enhanced `for` d) None of the above

> **Answer: b)**

---

## 16. Formula & Logic Sheet

**Sum of first n numbers**

```
n × (n + 1) / 2
```

**Even Number Check**

```
number % 2 == 0
```

**Odd Number Check**

```
number % 2 != 0
```

**Leap Year**

```
(year % 400 == 0) ||
(year % 4 == 0 && year % 100 != 0)
```

**Factorial**

```
n! = n × (n-1) × (n-2) × ... × 1,   0! = 1
```

**GCD (Euclidean Algorithm)**

```
gcd(a, b) = gcd(b, a % b), until b == 0
```

**LCM**

```
lcm(a, b) = (a × b) / gcd(a, b)
```

**Array Traversal Range**

```
0 → length - 1
```

**Array Element Address (Conceptual)**

```
base_address + (index × size_of_data_type)
```

**Matrix Multiplication Validity**

```
A (m × n) × B (n × p) → valid only if A's columns == B's rows
Result dimensions: m × p
```

---

## 17. Quick Revision Sheet

Remember:

✔ `if` → decision making

✔ `switch` → multiple choices

✔ `while` → unknown iterations

✔ `do-while` → executes at least once

✔ `for` → known iterations

✔ `break` → exits loop

✔ `continue` → skips current iteration

✔ `return` → exits method

✔ Arrays store same type of data.

✔ Index starts from 0.

✔ Arrays have fixed size.

✔ Enhanced for loop is used for traversal.

✔ `arr.length` is a field, not a method.

✔ 2D arrays in Java are "arrays of arrays" — rows can be jagged.

✔ Linear Search: O(n); Binary Search (sorted only): O(log n).

✔ Matrix multiplication requires columns of A = rows of B.

---

## 18. Chapter Summary

Chapter 3 laid the foundation for **controlling program flow** and **organizing data** in Java. On the control-flow side, decision-making statements (`if`, `if-else`, `else-if`, `switch`) allow programs to choose between execution paths based on conditions, while loops (`while`, `do-while`, `for`, enhanced `for`) automate repetitive tasks — each suited to different scenarios depending on whether the iteration count is known, whether at least one execution is guaranteed, and whether index access is needed. Branching statements (`break`, `continue`, `return`) provide fine-grained control to exit loops early, skip iterations, or return from methods.

On the data side, **arrays** provide a fixed-size, indexed, homogeneous way to store and process multiple related values efficiently — from simple one-dimensional lists to two-dimensional matrices used for tabular data and mathematical operations. The **enhanced for loop** simplifies read-only array/collection traversal, while the traditional `for` loop remains essential whenever index access or in-place modification is required.

Together, these concepts — control structures and arrays — are used constantly throughout programming, forming a necessary foundation before moving into object-oriented programming concepts like Classes and Objects.

---

## 19. Key Takeaways

- Control structures determine the flow of execution.
- Decision-making statements select different execution paths.
- Loops automate repetitive tasks.
- Branching statements modify the normal flow of loops and methods.
- Arrays efficiently store multiple values of the same type.
- Enhanced for loops simplify array traversal.
- Understanding these concepts is essential before learning Classes and Objects.

---

## 20. Chapter Checklist

Before moving to Chapter 4, ensure you can:

☐ Explain all control structures.

☐ Write programs using `if` and `switch`.

☐ Use `while`, `do-while`, and `for` loops.

☐ Apply `break`, `continue`, and `return` correctly.

☐ Create and manipulate one-dimensional arrays.

☐ Work with two-dimensional arrays.

☐ Use the enhanced for loop.

☐ Solve basic array programming problems.

☐ Trace program execution manually.

☐ Identify and fix common programming errors.

---

_End of Chapter 3 Revision — Control Structures & Arrays (Java)_
