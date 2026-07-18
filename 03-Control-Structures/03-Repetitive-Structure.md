# 3.3 Repetitive Structures

## 1. Introduction

- **What are Repetitive Structures?** Repetitive structures, commonly known as **loops**, are control structures that allow a block of code to be executed **repeatedly** — either a fixed number of times, or until a certain condition is no longer true — without the programmer having to write that block of code multiple times.
- **Why are Repetitive Structures Needed?** Many real-world tasks involve repetition: printing every item in a bill, validating input until it's correct, processing every record in a database, or checking every element in a large collection. Without loops, each repetition would need to be hardcoded as a separate statement, making programs impractically long and rigid.
- **Importance of Loops in Programming:** Loops are one of the most fundamental building blocks in programming — nearly every non-trivial program involves some form of repetition. Mastering loops is essential for writing efficient, concise, and scalable code.
- **Real-life Examples:** Washing dishes one by one until none remain; an alarm clock ringing every day at the same time; a teacher calling out student names one by one from an attendance register; an ATM repeatedly asking for a PIN until it's entered correctly (or a maximum number of attempts is reached).

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand repetitive structures.
- Explain different types of loops.
- Choose the appropriate loop for a problem.
- Write programs using loops.
- Understand nested loops.
- Avoid common looping mistakes.

---

## 3. What is a Loop?

**Definition:** A loop is a control structure that repeatedly executes a block of code, called the **loop body**, as long as a specified condition remains `true`.

**Flow of Execution:** A loop generally follows this cycle: check the condition → if true, execute the loop body → update the loop control variable → check the condition again → repeat until the condition becomes `false`, at which point the loop terminates and execution continues with the statement after the loop.

**Basic Terminology:**

- **Iteration:** One complete execution of the loop body.
- **Loop Control Variable (LCV):** The variable whose value is checked in the condition and changed on each iteration (commonly named `i`, `j`, `k`, or `count`).
- **Termination:** The point at which the loop's condition becomes `false` and the loop stops running.

**Loop Components:**

- **Initialization:** Setting up the loop control variable with a starting value before the loop begins.
- **Condition:** A boolean expression checked before each iteration; the loop continues only while this is `true`.
- **Update:** A statement that changes the loop control variable after each iteration (increment or decrement), eventually causing the condition to become `false`.
- **Loop Body:** The block of statements that gets executed repeatedly for as long as the condition holds.

**Diagram:**

<p align="center">
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSeMIZal9Z2YL75XXvfIlaXeVBCLpyPpZw-yWd6FGo6AEumfIIj-Upm6tA&s=10"
         alt="loop"
         width="70%">
    <br>
    <b>Figure:Loop </b>
</p>

---

## 4. Why Do We Need Loops?

**Problems without loops:** Without a loop, printing numbers 1 through 100 would require writing 100 separate `System.out.println()` statements — tedious, error-prone, and completely impractical for larger or dynamically-sized tasks (e.g., "print every element of an array of unknown size").

**Advantages of loops:**

- **Code reusability:** The same block of logic is written once and reused automatically for every repetition, rather than duplicated manually.
- **Efficiency:** Loops allow programs to handle large or variable amounts of repetition (even millions of iterations) with just a few lines of code.
- **Readability:** A loop clearly communicates the _intent_ of repetition, making code easier to understand than a long sequence of near-identical statements.

**Examples:**

```java
// Without a loop (impractical for large n)
System.out.println(1);
System.out.println(2);
System.out.println(3);
// ... would need 100 lines for numbers 1 to 100

// With a loop
for (int i = 1; i <= 100; i++) {
    System.out.println(i);
}
```

---

## 5. Types of Loops in Java

```
Repetitive Structures
│
├── while Loop
├── do-while Loop
├── for Loop
└── Enhanced for Loop (Introduction)
```

---

## 6. while Loop

**Definition:** The `while` loop is an **entry-controlled** (pre-tested) loop that checks its condition **before** each iteration — if the condition is `false` from the very start, the loop body never executes even once.

**Syntax:**

```java
while (condition) {
    // loop body
}
```

**Flowchart:**

<p align="center">
    <img src="https://www.geeksforgeeks.org/c/c-while-loop/"
         alt="while loop"
         width="70%">
    <br>
    <b>Figure: while Loop </b>
</p>

**Working:** The condition is evaluated first; if `true`, the loop body runs once, and then the condition is checked again — this repeats until the condition evaluates to `false`.

**Execution Process:** Initialize the loop control variable **before** the loop → check condition → execute body → update the variable **inside** the body → repeat.

**Examples:**

```java
int i = 1;
while (i <= 5) {
    System.out.println("Count: " + i);
    i++;
}
```

**Output:**

```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

**Real-life Applications:** Repeatedly prompting a user for input until a valid value is entered (when the number of attempts isn't known in advance); processing items from a queue until it's empty.

**Advantages:**

- Ideal when the number of iterations is **not known in advance** and depends on a runtime condition.
- Simple and flexible structure for condition-based repetition.

**Disadvantages:**

- Easy to accidentally create an infinite loop if the update statement is forgotten or incorrect.
- Since the condition is checked first, the body might never execute, which isn't always the desired behavior.

---

## 7. do-while Loop

**Definition:** The `do-while` loop is an **exit-controlled** (post-tested) loop that checks its condition **after** executing the loop body — guaranteeing the body runs **at least once**, regardless of whether the condition is true or false.

**Syntax:**

```java
do {
    // loop body
} while (condition);
```

> Note the semicolon `;` after the `while(condition)` — it is required and easy to forget.

**Flowchart:**

<p align="center">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20221006152307/dowhileloopinc.png"
             alt="do-while loop"
             width="70%">
        <br>
        <b>Figure: do-while Loop </b>
</p>

**Working:** The loop body executes first, and only afterward is the condition checked — if `true`, the loop repeats; if `false`, it terminates.

**Execution Process:** Execute body → update the loop control variable → check condition → repeat if true, exit if false.

**Examples:**

```java
int i = 1;
do {
    System.out.println("Count: " + i);
    i++;
} while (i <= 5);
```

**Output:**

```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

**Guaranteed first execution example (condition false from the start):**

```java
int x = 10;
do {
    System.out.println("This runs at least once, x = " + x);
} while (x < 5);
```

**Output:**

```
This runs at least once, x = 10
```

Even though `x < 5` is `false` from the start, the body still executes exactly once — this is the defining characteristic of `do-while`.

**Advantages:**

- Guarantees at least one execution — useful for menu-driven programs or input validation where the body must run at least once before checking the condition.

**Disadvantages:**

- Slightly less intuitive/readable than `while` for beginners, since the condition appears after the body.
- Can accidentally execute unwanted logic once even when the condition was never meant to be true, if not used carefully.

**Difference between `while` and `do-while`:**

| Aspect             | while                                                                                       | do-while                                                     |
| ------------------ | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| Condition check    | Before the loop body                                                                        | After the loop body                                          |
| Minimum executions | 0 (may never execute)                                                                       | 1 (always executes at least once)                            |
| Control type       | Entry-controlled                                                                            | Exit-controlled                                              |
| Syntax ending      | No semicolon after condition                                                                | Semicolon required after `while(condition);`                 |
| Best use case      | When the number of iterations depends entirely on a condition that might be false initially | When the body must run at least once, such as a menu display |

---

## 8. for Loop

**Definition:** The `for` loop is an **entry-controlled** loop that combines initialization, condition checking, and updating into a single, compact line — making it the most common choice when the number of iterations is known or countable in advance.

**Syntax:**

```java
for (initialization; condition; update) {
    // loop body
}
```

**Initialization:** Executed **once**, before the loop begins — typically declares and sets the starting value of the loop control variable.

**Condition:** Checked before every iteration; the loop continues only while this evaluates to `true`.

**Update Expression:** Executed after each iteration of the loop body, typically incrementing or decrementing the loop control variable.

**Flowchart:**

<p align="center">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQr85NEBsQ7LaV_6BfnCGJdL7FNbJ-SkkZ6kr0K52cml2lX1NXaHIgaS4Q&s=10"
             alt="do-while loop"
             width="70%">
        <br>
        <b>Figure: for loop </b>
</p>

**Execution Process:** Initialization runs once → condition checked → if true, body executes → update runs → condition checked again → repeat until condition is false.

**Examples:**

```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Iteration: " + i);
}
```

**Output:**

```
Iteration: 1
Iteration: 2
Iteration: 3
Iteration: 4
Iteration: 5
```

**Counting backward:**

```java
for (int i = 10; i >= 1; i--) {
    System.out.println(i);
}
```

**Advantages:**

- Compact — initialization, condition, and update are all visible together in one line, making the loop's boundaries very clear.
- Ideal when the exact number of iterations is known or easily countable in advance.

**Disadvantages:**

- Less suited to situations where the number of iterations depends on a complex or unpredictable runtime condition (a `while` loop is often clearer there).
- All three components (init/condition/update) being on one line can become cluttered for very complex loop logic.

---

## 9. Enhanced for Loop (Introduction)

**Why it was introduced:** The enhanced `for` loop (also called the **for-each** loop) was introduced to simplify iterating over arrays and collections, removing the need to manually manage an index variable, reducing the risk of off-by-one errors.

**Syntax:**

```java
for (dataType element : collectionOrArray) {
    // use element
}
```

**Array Traversal:**

```java
int[] numbers = {10, 20, 30, 40, 50};

for (int num : numbers) {
    System.out.println(num);
}
```

**Output:**

```
10
20
30
40
50
```

**Advantages:**

- Cleaner, more readable syntax — no explicit index variable or manual bounds checking needed.
- Eliminates common indexing mistakes like off-by-one errors.

**Limitations:**

- Cannot easily access or modify the index of the current element.
- Cannot be used to iterate in reverse, skip elements, or modify the underlying array/collection structure during iteration.
- Not suitable when the loop logic needs the index itself (e.g., comparing an element to its neighbors).

_(Note: A detailed explanation of the enhanced for loop, including its use with collections like `ArrayList`, is provided in a separate chapter dedicated to arrays and collections.)_

---

## 10. Nested Loops

**Definition:** A nested loop is a loop placed inside the body of another loop. The **inner loop** completes all of its iterations for **each single iteration** of the **outer loop**.

**Working:** For every one iteration of the outer loop, the entire inner loop runs to completion before the outer loop proceeds to its next iteration.

**Syntax:**

```java
for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= columns; j++) {
        // inner loop body
    }
}
```

**Examples — Nested loop basic trace:**

```java
for (int i = 1; i <= 2; i++) {
    for (int j = 1; j <= 3; j++) {
        System.out.println("i=" + i + ", j=" + j);
    }
}
```

**Output:**

```
i=1, j=1
i=1, j=2
i=1, j=3
i=2, j=1
i=2, j=2
i=2, j=3
```

**Pattern Printing:**

```java
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("*");
    }
    System.out.println();
}
```

**Output:**

```
*
**
***
****
*****
```

**Multiplication Table (using nested loops for a full grid, 1–5):**

```java
for (int i = 1; i <= 5; i++) {
    for (int j = 1; j <= 5; j++) {
        System.out.print((i * j) + "\t");
    }
    System.out.println();
}
```

**Output:**

```
1   2   3   4   5
2   4   6   8   10
3   6   9   12  15
4   8   12  16  20
5   10  15  20  25
```

---

## 11. Infinite Loops

**Definition:** An infinite loop is a loop whose condition **never becomes false**, causing it to run forever (or until the program is forcibly terminated, or the system runs out of resources).

**Causes:**

- Forgetting to include an update statement that changes the loop control variable.
- Writing a condition that can never become false due to a logic error.
- Deliberately writing `while (true)` without a `break` statement to exit.

**Examples:**

```java
// Unintentional infinite loop — missing update statement
int i = 1;
while (i <= 5) {
    System.out.println(i);
    // i++ forgotten — i never changes, condition is always true
}
```

```java
// Intentional infinite loop, exited manually with break
while (true) {
    System.out.println("Running...");
    break;  // exits immediately in this example
}
```

**How to Avoid:**

- Always ensure the loop control variable is updated correctly within the loop body.
- Double-check that the condition can, in fact, eventually become `false` given how the variable changes.
- When intentionally using `while (true)`, always include a clear `break` condition somewhere inside the loop.

**Practical Uses:** Intentional infinite loops are commonly used for server programs that must keep running continuously, or menu-driven programs that keep prompting the user until they choose to exit (using `break` when the exit option is selected).

---

## 12. Loop Control Variables

**Initialization:** The starting value assigned to the loop control variable before the loop begins (e.g., `int i = 0;`).

**Condition:** The boolean expression checked against the loop control variable before each iteration (e.g., `i < 10`).

**Increment:** Increasing the loop control variable's value after each iteration (e.g., `i++`, `i += 2`).

**Decrement:** Decreasing the loop control variable's value after each iteration (e.g., `i--`, `i -= 2`).

**Examples:**

```java
// Increment by 1
for (int i = 0; i < 5; i++) {
    System.out.println(i);   // 0 1 2 3 4
}

// Decrement by 1
for (int i = 5; i > 0; i--) {
    System.out.println(i);   // 5 4 3 2 1
}

// Increment by 2 (skip counting)
for (int i = 0; i <= 10; i += 2) {
    System.out.println(i);   // 0 2 4 6 8 10
}
```

---

## 13. Loop Execution Flow

**Flowchart — `while`:**

<p align="center">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20240214162718/While-Loop-GeeksforGeeks.jpg"
             alt="loop"
             width="70%">
        <br>
        <b>Figure: while loop execution </b>
</p>

**Flowchart — `do-while`:**

<p align="center">
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR6B3FjheD3nFAsSFctfCQtuygFRaP5SlDicwB6XpGe3A&s=10"
             alt="loop"
             width="70%">
        <br>
        <b>Figure: do-while loop execution </b>
</p>

**Flowchart — `for`:**

<p align="center">
        <img src="https://media.geeksforgeeks.org/wp-content/uploads/20240214152414/For-Loop.jpg"
             alt="loop"
             width="70%">
        <br>
        <b>Figure: for loop execution </b>
</p>
---

## 14. Comparison of Loops

| Feature                 | while                                                     | do-while                                                             | for                                                               |
| ----------------------- | --------------------------------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Entry Controlled        | Yes                                                       | No                                                                   | Yes                                                               |
| Exit Controlled         | No                                                        | Yes                                                                  | No                                                                |
| Condition Check         | Before loop body                                          | After loop body                                                      | Before loop body                                                  |
| Minimum Executions      | 0                                                         | 1 (always at least once)                                             | 0                                                                 |
| Best Use Cases          | Unknown number of iterations, condition-driven repetition | When the body must run at least once (e.g., menus, input validation) | Known/countable number of iterations                              |
| Readability             | Good for simple condition-based loops                     | Slightly less intuitive due to condition placement                   | Very readable for counting loops — init/condition/update together |
| Initialization location | Before the loop                                           | Before the loop                                                      | Inside the loop's own syntax                                      |
| Update location         | Inside the loop body                                      | Inside the loop body                                                 | Inside the loop's own syntax                                      |

---

## 15. Choosing the Right Loop

- **`while`:** Use when the number of iterations is **unknown in advance** and depends on a condition evaluated at runtime — e.g., reading input until the user types "exit".
- **`do-while`:** Use when the loop body **must execute at least once**, regardless of the condition — e.g., displaying a menu at least once before asking whether to continue.
- **`for`:** Use when the number of iterations is **known or easily countable** in advance — e.g., iterating from 1 to 100, or processing a fixed-size array by index.
- **Enhanced for:** Use when you need to **simply access every element** of an array or collection, without needing the index or making structural changes during iteration — e.g., printing all elements of a list.

**Real-world examples:**

- **`while`:** An ATM repeatedly asking for a PIN until it's correct or the maximum attempts are used.
- **`do-while`:** A restaurant menu that is shown at least once, then asks "Would you like to order more?"
- **`for`:** Printing a fixed multiplication table from 1 to 10.
- **Enhanced for:** Displaying every product name in a shopping cart list.

---

## 16. Common Programming Patterns

**Counting:**

```java
for (int i = 1; i <= 10; i++) {
    System.out.println(i);
}
```

**Summation (sum of first N numbers):**

```java
int n = 10, sum = 0;
for (int i = 1; i <= n; i++) {
    sum += i;
}
System.out.println("Sum: " + sum);  // 55
```

**Factorial:**

```java
int n = 5, factorial = 1;
for (int i = 1; i <= n; i++) {
    factorial *= i;
}
System.out.println("Factorial: " + factorial);  // 120
```

**Multiplication Table:**

```java
int num = 7;
for (int i = 1; i <= 10; i++) {
    System.out.println(num + " x " + i + " = " + (num * i));
}
```

**Number Reversal:**

```java
int num = 1234, reversed = 0;
while (num != 0) {
    int digit = num % 10;
    reversed = reversed * 10 + digit;
    num /= 10;
}
System.out.println("Reversed: " + reversed);  // 4321
```

**Fibonacci Series:**

```java
int n = 10, a = 0, b = 1;
for (int i = 1; i <= n; i++) {
    System.out.print(a + " ");
    int next = a + b;
    a = b;
    b = next;
}
// Output: 0 1 1 2 3 5 8 13 21 34
```

**Prime Numbers (checking a single number):**

```java
int num = 29;
boolean isPrime = true;

if (num < 2) {
    isPrime = false;
} else {
    for (int i = 2; i <= Math.sqrt(num); i++) {
        if (num % i == 0) {
            isPrime = false;
            break;
        }
    }
}
System.out.println(num + " is prime: " + isPrime);  // true
```

**Armstrong Numbers:**

```java
int num = 153, original = num, sum = 0;
while (num != 0) {
    int digit = num % 10;
    sum += digit * digit * digit;
    num /= 10;
}
System.out.println(original + " is Armstrong: " + (sum == original));  // true
```

---

## 17. Best Practices

- **Avoid unnecessary loops** — don't loop when a direct calculation or built-in method could achieve the same result more efficiently.
- **Use meaningful loop variables** — for simple counters `i`, `j`, `k` are conventional and fine, but for loops with real domain meaning, prefer descriptive names (e.g., `studentIndex`).
- **Prevent infinite loops** — always verify that the loop's update logic will eventually make the condition `false`.
- **Keep loops simple** — avoid overly complex conditions or update expressions that make the loop's behavior hard to trace at a glance.
- **Minimize nested loops** — deeply nested loops can hurt both readability and performance; consider breaking logic into separate methods if nesting becomes excessive.

---

## 18. Common Mistakes

- **Missing increment/decrement:** Forgetting to update the loop control variable, causing an infinite loop.
- **Wrong condition:** Using an incorrect comparison operator (e.g., `<=` instead of `<`), causing the loop to run one time too many or too few.
- **Infinite loop:** Writing a condition that never becomes false, whether due to a logic error or a forgotten update statement.
- **Off-by-one error:** Starting or ending a loop one iteration too early or too late — e.g., using `i < n` when `i <= n` was intended, or vice versa.
- **Incorrect nesting:** Mismatched braces or incorrectly scoped inner loops, causing unexpected behavior.
- **Using the wrong loop type:** Using a `for` loop when the number of iterations truly isn't known in advance, or a `while` loop for simple, fixed-count iteration where a `for` loop would be clearer.

---

## 19. Interview Corner

**Q1. Difference between `while` and `do-while`?**
`while` checks its condition before executing the loop body, so the body may run zero times; `do-while` checks its condition after executing the loop body, guaranteeing at least one execution regardless of the condition.

**Q2. Which loop is fastest?**
In practice, there is no meaningful performance difference between `while`, `do-while`, and `for` loops in Java — they all compile down to similar bytecode-level looping constructs; the choice between them should be based on readability and suitability for the problem, not speed.

**Q3. Can a `for` loop be written as a `while` loop?**
Yes — any `for` loop can be rewritten as an equivalent `while` loop by moving the initialization before the loop, keeping the same condition, and placing the update statement at the end of the loop body, and vice versa.

```java
// for loop
for (int i = 0; i < 5; i++) { System.out.println(i); }

// equivalent while loop
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

**Q4. What is an infinite loop?**
A loop whose terminating condition never becomes `false`, causing it to run indefinitely unless manually interrupted (e.g., by a `break` statement or external termination).

**Q5. What is an enhanced for loop?**
Also called a for-each loop, it's a simplified loop syntax used specifically to iterate over every element of an array or collection without manually managing an index variable.

**Q6. Why is `do-while` useful for menu-driven programs?**
Because the menu needs to be displayed to the user at least once before any condition (like "continue?") can even be checked — `do-while`'s guaranteed first execution matches this requirement naturally.

**Q7. What is the difference between entry-controlled and exit-controlled loops?**
Entry-controlled loops (`while`, `for`) check their condition before running the loop body, so the body might never execute; exit-controlled loops (`do-while`) check their condition after running the body, guaranteeing at least one execution.

---

## 20. MCQs

**1. Which loop guarantees at least one execution of its body?**
a) while
b) for
c) do-while
d) enhanced for
**Answer: c) do-while**

**2. What is the output of the following code?**

```java
int i = 5;
while (i < 5) {
    System.out.println(i);
    i++;
}
```

a) 5
b) Nothing is printed
c) Infinite loop
d) Compile error
**Answer: b) Nothing is printed** (condition is false from the start)

**3. Which part of a `for` loop executes only once?**
a) Condition
b) Update
c) Initialization
d) Loop body
**Answer: c) Initialization**

**4. What causes an infinite loop?**
a) Correct update statement
b) A condition that always evaluates to true
c) Using a for loop
d) Using break inside the loop
**Answer: b) A condition that always evaluates to true**

**5. Which loop type is best for iterating over an array without needing the index?**
a) while
b) do-while
c) for
d) enhanced for
**Answer: d) enhanced for**

**6. What is the output of this nested loop?**

```java
for (int i = 1; i <= 2; i++) {
    for (int j = 1; j <= 2; j++) {
        System.out.print(i + "" + j + " ");
    }
}
```

a) 11 12 21 22
b) 12 21
c) 11 21 12 22
d) 1122
**Answer: a) 11 12 21 22**

**7. Which of these is an entry-controlled loop?**
a) do-while
b) for
c) Both while and for
d) None of these
**Answer: c) Both while and for**

**8. What is an off-by-one error?**
a) A syntax error in loop declaration
b) An error where the loop executes one time too many or too few
c) An infinite loop
d) A compile-time type mismatch
**Answer: b) An error where the loop executes one time too many or too few**

**9. What must you remember when writing a `do-while` loop?**
a) Semicolon after the while condition
b) No condition is needed
c) The body must be empty
d) It cannot contain a loop control variable
**Answer: a) Semicolon after the while condition**

**10. Which loop is most suitable when the exact number of iterations is known beforehand?**
a) while
b) do-while
c) for
d) None of these
**Answer: c) for**

**11. In a nested loop, how many times does the inner loop run in total if the outer loop runs 3 times and the inner loop runs 4 times per outer iteration?**
a) 3
b) 4
c) 7
d) 12
**Answer: d) 12**

**12. Which statement about the enhanced for loop is true?**
a) It allows modifying the loop index directly
b) It cannot be used with arrays
c) It simplifies iteration but doesn't expose the index
d) It only works with while loops
**Answer: c) It simplifies iteration but doesn't expose the index**

**13. What is the result of running `while(true) { break; }`?**
a) Infinite loop
b) Loop runs once, then exits
c) Compile error
d) Loop never executes
**Answer: b) Loop runs once, then exits**

**14. Which loop checks its condition after executing the body?**
a) for
b) while
c) do-while
d) enhanced for
**Answer: c) do-while**

**15. What will happen if the update statement is missing in a while loop meant to count from 1 to 5?**
a) Loop runs exactly 5 times
b) Loop doesn't execute at all
c) Infinite loop
d) Compile error
**Answer: c) Infinite loop**

---

## 21. University Exam Questions

### Short Questions

1. What is a loop? Name the different types of loops in Java.
2. What is the difference between entry-controlled and exit-controlled loops?
3. What is an infinite loop, and how can it be avoided?
4. What is a nested loop? Give one example.
5. What is the purpose of the enhanced for loop?
6. What is an off-by-one error?
7. Write the general syntax of a `for` loop and explain each part.
8. Why does `do-while` guarantee at least one execution?

### Long Questions

1. Explain the `while`, `do-while`, and `for` loops in Java with syntax, flowcharts, and examples for each.
2. Differentiate between `while`, `do-while`, and `for` loops with a complete comparison table.
3. Write a program to print a right-angled triangle pattern of stars using nested loops, and explain the logic.
4. Explain nested loops with an example of generating a multiplication table.
5. Discuss common mistakes made while writing loops in Java, with examples of each and how to fix them.

---

## 22. Viva Questions

1. What is the difference between `while` and `for` loops in terms of readability?
2. Can the loop control variable of a `for` loop be modified inside the loop body? What are the risks of doing so?
3. Why is `do-while` less commonly used than `while` and `for`?
4. What happens if you write `for(;;)` in Java?
5. Is it possible to have multiple variables initialized in a single `for` loop?
6. What is the scope of a variable declared in the initialization of a `for` loop?
7. Can a `while` loop be converted into a `do-while` loop directly? What must change?
8. What is the purpose of nested loops in pattern printing programs?
9. Why is `break` sometimes needed inside loops that check for prime numbers?
10. What is the time complexity consideration when using nested loops?

---

## 23. Programming Exercises

**Easy**

- Print numbers from 1 to 10.
- Print even numbers between 1 and 50.
- Find the sum of the first N natural numbers.

**Medium**

- Print the multiplication table of a given number.
- Calculate the factorial of a given number.
- Generate the first N terms of the Fibonacci series.
- Check whether a given number is prime.

**Hard**

- Print a pattern (e.g., right triangle, inverted triangle) using nested loops.
- Print a diamond pattern using nested loops.
- Print Pascal's Triangle up to N rows.
- Print a number pyramid pattern using nested loops.

---

## 24. Challenge Problems

**ATM Menu:** Build a menu-driven ATM simulation using a `do-while` loop that keeps showing options (balance check, withdraw, deposit, exit) until the user chooses to exit.

**Student Result System:** Write a program that repeatedly accepts marks for multiple students (using a loop) and calculates and displays each student's grade.

**Number Guessing Game:** Write a program that generates a random number and repeatedly asks the user to guess it, using a loop that continues until the correct number is guessed.

**Mini Calculator using Loops:** Build a menu-driven calculator that keeps performing operations in a loop until the user selects "exit".

**Password Validation:** Write a program that repeatedly asks the user to enter a password until it meets certain validation criteria (e.g., minimum length, contains a digit).

---

## 25. Summary

- **Loops (repetitive structures)** allow a block of code to execute repeatedly, avoiding manual duplication and enabling programs to handle large or variable amounts of repetition efficiently.
- Java provides four main looping constructs: **`while`** (entry-controlled, condition-driven), **`do-while`** (exit-controlled, guarantees at least one execution), **`for`** (entry-controlled, ideal for known iteration counts), and the **enhanced for loop** (simplified iteration over arrays/collections).
- **Nested loops** place one loop inside another, commonly used for tasks like pattern printing and multiplication tables, where the inner loop completes fully for every single iteration of the outer loop.
- **Infinite loops** occur when a loop's condition never becomes false, usually due to a missing or incorrect update statement — though they can also be used intentionally with an explicit `break` condition.
- Choosing the right loop depends on whether the number of iterations is known in advance, whether the body must run at least once, and whether index access is needed during iteration.

---

## 26. Key Takeaways

- Loops repeat code efficiently.
- Java provides `while`, `do-while`, `for`, and enhanced `for` loops.
- Choose the loop based on the problem.
- Avoid infinite loops.
- Nested loops are useful for matrices and patterns.

---

## 27. Cheat Sheet

```
WHILE LOOP (Entry-controlled)
while (condition) {
    // body
}

DO-WHILE LOOP (Exit-controlled, runs at least once)
do {
    // body
} while (condition);          // semicolon required!

FOR LOOP (Entry-controlled, best for known iteration counts)
for (initialization; condition; update) {
    // body
}

ENHANCED FOR LOOP (for-each, best for arrays/collections)
for (dataType element : array) {
    // use element
}

COMPARISON QUICK REFERENCE
while       → condition first, may run 0 times
do-while    → body first, always runs at least once
for         → best when iteration count is known
enhanced for → best for simple full traversal, no index needed

AVOIDING INFINITE LOOPS
✔ Always update the loop control variable
✔ Ensure the condition can eventually become false
✔ Use break with while(true) intentionally, never by accident
```

---

## 28. Common Errors

- **Infinite loop:** Caused by a missing or incorrect update statement, or a condition that can never become false.
- **Off-by-one error:** Using `<=` instead of `<` (or vice versa) in the loop condition, causing one extra or one missing iteration.
- **Missing braces:** Forgetting `{ }` around a multi-statement loop body, causing only the first statement to actually be part of the loop.
  ```java
  for (int i = 0; i < 5; i++)
      System.out.println(i);
      System.out.println("Done");  // NOT part of the loop — runs 5 times unintentionally otherwise assumed
  ```
- **Wrong update expression:** Using an update that moves the loop control variable in the wrong direction (e.g., `i--` in a loop meant to count upward), which can also cause an infinite loop.
- **Variable scope issues:** Attempting to access a `for` loop's initialization variable (e.g., `i`) outside the loop, which fails because that variable's scope is limited to the loop itself.
  ```java
  for (int i = 0; i < 5; i++) { }
  // System.out.println(i);  // Compile Error: i cannot be resolved outside the loop
  ```
