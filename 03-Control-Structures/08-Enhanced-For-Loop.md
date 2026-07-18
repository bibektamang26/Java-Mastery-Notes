# Enhanced for Loop (For-Each Loop) — Java

---

## 1. Introduction

### Introduction to Enhanced for Loop

The **Enhanced for Loop**, also known as the **for-each loop**, is a simplified looping construct in Java designed specifically for **traversing** arrays and collections. It eliminates the need for manual index management, making code shorter, cleaner, and less error-prone.

```java
int[] numbers = {10, 20, 30, 40};
for (int num : numbers) {
    System.out.println(num);
}
```

### History (Introduced in Java 5)

The Enhanced for Loop was introduced in **Java 5 (J2SE 5.0, released in 2004)** as part of a set of major language features, alongside **Generics**, **Autoboxing/Unboxing**, **Varargs**, and **Enums**. It was designed to simplify the common pattern of iterating over every element of an array or a `Collection` (via the `Iterable` interface).

### Why Java Introduced the Enhanced for Loop

Before Java 5, traversing an array or a collection required boilerplate index or iterator management:

```java
// Old style with Iterator
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
    String s = it.next();
    System.out.println(s);
}
```

This was repetitive and error-prone (off-by-one errors, forgetting `hasNext()`, etc). The Enhanced for Loop abstracts this away, letting developers focus on **what** to do with each element rather than **how** to iterate.

### Importance of Enhanced for Loop

- Makes traversal code **shorter and more readable**.
- Reduces **index-related bugs** like `ArrayIndexOutOfBoundsException`.
- Works uniformly across **arrays** and any class implementing `Iterable` (like `ArrayList`, `HashSet`, `LinkedList`).
- Widely used in real-world Java code — a foundational construct for every Java developer.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand the Enhanced for Loop and its purpose.
- Explain its syntax and how it works internally.
- Traverse arrays (primitive and object types) using the Enhanced for Loop.
- Iterate through collections using the Enhanced for Loop.
- Compare it with the traditional for loop across various dimensions.
- Identify situations where the Enhanced for Loop should and should not be used.

---

## 3. What is an Enhanced for Loop?

### Definition

> The Enhanced for Loop is a control flow statement that iterates over every element of an array or an object implementing the `Iterable` interface, without requiring explicit index tracking.

### Real-life Analogy

Imagine a **postman delivering letters** to every house on a street, one after another, in order — he doesn't need to know the house _number_ (index); he simply moves to "the next house" until there are no houses left. That's exactly how the Enhanced for Loop works: it visits every element, one at a time, without you tracking a counter.

### Why it is called "Enhanced"

It is called "enhanced" because it is an **improved, simplified version** of the traditional `for` loop — specifically tailored for **traversal-only** scenarios, removing the need for loop counters, boundary checks, and increment/decrement expressions.

### Key Characteristics

- Automatically iterates over **every element**, from first to last.
- No explicit index variable is used or accessible.
- Works with **arrays** and any class implementing `java.lang.Iterable` (most Collections).
- The loop variable holds a **copy** of each element's value during each iteration (for primitives) or a **reference** to each object (for reference types).
- Cannot be used to modify the original array/collection structure during iteration (adding/removing elements from a collection mid-loop throws `ConcurrentModificationException`).

---

## 4. Why Do We Need an Enhanced for Loop?

### Problems with the Traditional for Loop

**Manual Index Handling**

```java
int[] arr = {1, 2, 3, 4, 5};
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);  // must manually manage 'i'
}
```

You must correctly initialize, check, and increment the index every time.

**Index Out-of-Bounds Errors**
A simple mistake like `i <= arr.length` instead of `i < arr.length` throws `ArrayIndexOutOfBoundsException`. This is a common source of bugs in beginner (and even experienced) code.

**Less Readable Code**
The traditional loop mixes **iteration logic** (`int i = 0; i < arr.length; i++`) with **business logic** (what you actually want to do with each element), making the code more cluttered, especially for simple read-only traversal.

### Advantages of Enhanced for Loop

- Eliminates manual index management entirely.
- Removes the risk of off-by-one errors during traversal.
- Produces cleaner, more declarative code — expressing **"for each element, do X"** directly.
- Works consistently across arrays and all standard collection types.

---

## 5. Syntax

### General Syntax

```java
for (dataType variable : collection) {
    // statements
}
```

### Example

```java
for (int num : numbers) {
    System.out.println(num);
}
```

### Explanation of Each Component

| Component          | Description                                                                                                                               |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `for` keyword      | Signals the start of a loop, same keyword as the traditional for loop.                                                                    |
| `dataType`         | The type of each element in the array/collection (e.g., `int`, `String`, `Student`). Must match (or be compatible with) the element type. |
| `variable`         | A new local variable created fresh for each iteration, holding the current element's value/reference.                                     |
| `colon (:)`        | Read as "in" — e.g., `for (int num : numbers)` means "for each `num` **in** `numbers`".                                                   |
| `array/collection` | The array or `Iterable` object being traversed. Must not be `null`, or a `NullPointerException` occurs.                                   |

---

## 6. Working of Enhanced for Loop

### Step-by-Step Execution

1. The loop takes the array/collection and identifies its **length** (for arrays) or obtains an internal **iterator** (for collections, via `Iterable.iterator()`).
2. It checks whether there is a **next element** to process.
3. If yes, it assigns the next element's value to the **loop variable**.
4. The loop body executes using this variable.
5. The loop automatically advances to the next element.
6. Steps 2–5 repeat until all elements have been visited, after which the loop terminates.

### Execution Flow

```
Start
  |
  v
Is there a next element? ----No----> Exit Loop
  |
 Yes
  |
  v
Assign element to loop variable
  |
  v
Execute loop body
  |
  v
Move to next element ---------> (back to "Is there a next element?")
```

### Flowchart (Text Diagram)

```
        +-------------------------+
        |     Start Enhanced      |
        |        for Loop         |
        +------------+------------+
                     |
                     v
        +-------------------------+
        | Get next element from   |
        | array / collection      |
        +------------+------------+
                     |
                     v
        +-------------------------+
   No   |  Elements remaining?    |
   +----+  (hasNext / index<len)  |
   |    +------------+------------+
   |                 | Yes
   |                 v
   |    +-------------------------+
   |    |  Assign to loop         |
   |    |  variable                |
   |    +------------+------------+
   |                 |
   |                 v
   |    +-------------------------+
   |    |  Execute loop body      |
   |    +------------+------------+
   |                 |
   |                 +----------------+
   |                                  |
   v                                  |
+-------------------------+           |
|     Exit Loop            |<---------+
+-------------------------+
```

_Figure: Execution flow of Enhanced for Loop_

Under the hood, for arrays, the compiler translates the Enhanced for Loop into a traditional index-based loop; for collections, it translates into an `Iterator`-based loop using `hasNext()` and `next()`.

```java
// This Enhanced for Loop:
for (int num : arr) {
    System.out.println(num);
}

// Compiles to (roughly) the equivalent of:
for (int i = 0; i < arr.length; i++) {
    int num = arr[i];
    System.out.println(num);
}
```

---

## 7. Enhanced for Loop with Arrays

### Integer Array

```java
int[] numbers = {5, 10, 15, 20};
for (int num : numbers) {
    System.out.println(num);
}
// 5
// 10
// 15
// 20
```

### String Array

```java
String[] names = {"Amit", "Riya", "Sam"};
for (String name : names) {
    System.out.println(name);
}
```

### Character Array

```java
char[] letters = {'J', 'A', 'V', 'A'};
for (char ch : letters) {
    System.out.print(ch + " ");
}
// J A V A
```

### Double Array

```java
double[] prices = {19.99, 5.49, 100.0};
for (double price : prices) {
    System.out.println("Price: $" + price);
}
```

### Boolean Array

```java
boolean[] flags = {true, false, true};
for (boolean flag : flags) {
    System.out.println(flag);
}
```

---

## 8. Enhanced for Loop with Objects

### Example Using Student Objects

```java
class Student {
    String name;
    int marks;

    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }
}

public class StudentDemo {
    public static void main(String[] args) {
        Student[] students = {
            new Student("Amit", 85),
            new Student("Riya", 92),
            new Student("Sam", 78)
        };

        for (Student s : students) {
            System.out.println(s.name + " scored " + s.marks);
        }
    }
}
```

### Accessing Object Members

Inside the loop, the loop variable (`s` above) is a **reference** to the actual object in the array — so you can access its fields and call its methods directly using the dot operator (`s.name`, `s.marks`, `s.someMethod()`).

### Examples

```java
for (Student s : students) {
    if (s.marks >= 90) {
        System.out.println(s.name + " is a topper!");
    }
}
```

> **Note:** Since `s` is a reference to the actual `Student` object (not a copy), you **can** modify the object's fields (`s.marks = 100;`) — this change will persist in the original array. What you **cannot** do is reassign `s` to point to a different object and expect the array to update (see Section 12).

---

## 9. Enhanced for Loop with Collections (Introduction)

The Enhanced for Loop works with any class that implements the `java.lang.Iterable` interface — this includes essentially all standard Java collections.

### ArrayList

```java
import java.util.ArrayList;

ArrayList<String> fruits = new ArrayList<>();
fruits.add("Apple");
fruits.add("Banana");
fruits.add("Mango");

for (String fruit : fruits) {
    System.out.println(fruit);
}
```

### HashSet

```java
import java.util.HashSet;

HashSet<Integer> uniqueNumbers = new HashSet<>();
uniqueNumbers.add(10);
uniqueNumbers.add(20);
uniqueNumbers.add(10); // duplicate, ignored

for (int num : uniqueNumbers) {
    System.out.println(num);
}
```

### LinkedList

```java
import java.util.LinkedList;

LinkedList<String> queue = new LinkedList<>();
queue.add("Task1");
queue.add("Task2");

for (String task : queue) {
    System.out.println(task);
}
```

> _(This is only a basic introduction — collections such as `ArrayList`, `HashSet`, and `LinkedList` are covered in full detail in the dedicated Collections chapter.)_

---

## 10. Traditional for Loop vs Enhanced for Loop

### Comparison Table

| Aspect       | Traditional `for`                                   | Enhanced `for`                                             |
| ------------ | --------------------------------------------------- | ---------------------------------------------------------- |
| Syntax       | `for(int i=0; i<arr.length; i++)`                   | `for(dataType var : collection)`                           |
| Readability  | More verbose, less readable for simple traversal    | Cleaner and more concise                                   |
| Index Access | Index (`i`) is available                            | No direct index access                                     |
| Modification | Can modify elements via index (`arr[i] = x`)        | Cannot modify array elements via loop variable             |
| Performance  | Same underlying performance for arrays              | Same underlying performance (compiles to similar bytecode) |
| Ease of Use  | Requires manual bounds management                   | Very easy — no bounds management needed                    |
| Use Cases    | Reverse traversal, index-based logic, modification  | Simple read-only traversal, display, summation             |
| Direction    | Can traverse forward, backward, or with custom step | Always traverses forward, element by element               |
| Works With   | Arrays, any indexable structure                     | Arrays and `Iterable` objects (Collections)                |

---

## 11. Advantages

- **Cleaner syntax** — expresses intent directly ("for each element, do X").
- **Easy to read** — especially beneficial for beginners and for quick code reviews.
- **Less code** — no index declaration, boundary check, or increment needed.
- **No index errors** — completely eliminates `ArrayIndexOutOfBoundsException` caused by incorrect loop bounds.
- **Better for traversal** — ideal for iterating over every element without any special logic.
- **Uniform syntax** across arrays and all Collection types.

---

## 12. Limitations

### Cannot Access Index Directly

```java
String[] names = {"A", "B", "C"};
for (String name : names) {
    // No way to know if this is index 0, 1, or 2 directly
    System.out.println(name);
}
```

If the index is needed (e.g., for numbering output), you must maintain a separate counter manually, defeating some of the loop's simplicity.

### Cannot Traverse Backwards

The Enhanced for Loop always moves **forward**, from the first element to the last. There is no built-in way to iterate in reverse.

```java
// NOT possible directly with enhanced for loop:
// for (int num : arr) // in reverse order
```

### Cannot Skip Arbitrary Elements Using an Index

You cannot easily process, say, "every second element" or "elements from index 2 to 5" — this requires index-based control that the enhanced for loop doesn't provide.

### Cannot Easily Modify Array Elements

```java
int[] arr = {1, 2, 3};
for (int num : arr) {
    num = num * 2;  // modifies only the LOCAL copy, not arr itself
}
System.out.println(java.util.Arrays.toString(arr)); // still [1, 2, 3]
```

For primitive arrays, `num` is a **copy** of the value — reassigning it has no effect on the original array.

### Cannot Resize Arrays

The Enhanced for Loop is purely for traversal — it has no facility (and arrays in general have no facility) for growing or shrinking during iteration.

### Examples

```java
// Trying to double every element — this does NOT work as expected:
int[] arr = {1, 2, 3};
for (int val : arr) {
    val *= 2;  // has no effect on arr
}
// Correct way: use a traditional for loop
for (int i = 0; i < arr.length; i++) {
    arr[i] *= 2;
}
```

---

## 13. When to Use Enhanced for Loop

- **Reading data** — simply going through each element without needing its position.
- **Displaying elements** — printing all values in an array or collection.
- **Summation** — adding up all elements (sum, count, average calculations).
- **Searching** — checking if a specific value exists (though a "found" flag is often needed since there's no early index reference).
- **Printing** — formatting and displaying object fields (like in the Student example).

```java
// Summation example
int[] marks = {80, 90, 70};
int total = 0;
for (int m : marks) {
    total += m;
}
System.out.println("Total = " + total);
```

---

## 14. When NOT to Use Enhanced for Loop

### Need Index

```java
// Printing with position numbers requires the index
String[] items = {"Pen", "Book", "Bag"};
for (int i = 0; i < items.length; i++) {
    System.out.println((i + 1) + ". " + items[i]);
}
```

### Need Reverse Traversal

```java
int[] arr = {1, 2, 3, 4, 5};
for (int i = arr.length - 1; i >= 0; i--) {
    System.out.println(arr[i]);
}
```

### Need Element Replacement

```java
int[] arr = {1, 2, 3};
for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[i] * 10;  // requires index-based access
}
```

### Need Complex Iteration

Situations requiring skipping elements, iterating two arrays in parallel using a shared index, or stopping early based on position all require the traditional `for` loop (or explicit `Iterator` usage).

```java
// Iterating two arrays together using a shared index
int[] a = {1, 2, 3};
int[] b = {10, 20, 30};
for (int i = 0; i < a.length; i++) {
    System.out.println(a[i] + " + " + b[i] + " = " + (a[i] + b[i]));
}
```

---

## 15. Common Mistakes

### Trying to Modify Elements

```java
int[] arr = {1, 2, 3};
for (int val : arr) {
    val = val + 100;  // MISTAKE: only changes the local copy 'val'
}
// arr is unchanged: [1, 2, 3]
```

### Confusing Loop Variable with Array Element

Beginners sometimes assume changing the loop variable changes the underlying array — this is only true for **object references** where you modify the object's fields (not when reassigning the reference itself).

```java
Student[] students = {new Student("Amit", 80)};
for (Student s : students) {
    s.marks = 100;      // WORKS — modifies the actual object's field
    s = new Student("New", 50); // does NOT replace the array element
}
```

### Expecting Index Value

```java
for (String name : names) {
    System.out.println(i);  // COMPILE ERROR: 'i' does not exist here
}
```

### Null Arrays

```java
int[] arr = null;
for (int val : arr) {   // NullPointerException
    System.out.println(val);
}
```

Always ensure the array/collection is not `null` before using it in an Enhanced for Loop.

### Examples — Correct vs Incorrect

```java
// INCORRECT — expecting modification
for (int num : arr) { num++; }

// CORRECT — use traditional for loop to modify
for (int i = 0; i < arr.length; i++) { arr[i]++; }
```

---

## 16. Best Practices

1. **Use meaningful variable names** — e.g., `for (Student student : students)` instead of `for (Student s : students)` when clarity matters, especially in larger codebases.
2. **Prefer Enhanced for Loop for read-only traversal** — when you simply need to read/display/sum/search values without needing the index.
3. **Use traditional for loop when index is required** — for numbering, reverse traversal, parallel array iteration, or in-place modification.
4. Always check for `null` before iterating over an array/collection that might not be initialized.
5. Avoid relying on the Enhanced for Loop when you need to remove elements from a collection during iteration — use an explicit `Iterator` with `remove()` instead, to avoid `ConcurrentModificationException`.
6. Keep the loop body concise and focused; if complex index logic creeps in, that's a signal to switch to a traditional `for` loop.

---

## 17. Real-World Applications

### Student Management

```java
for (Student s : classRoom) {
    System.out.println(s.name + ": " + s.grade);
}
```

### Employee Records

```java
for (Employee emp : employees) {
    System.out.println(emp.name + " - $" + emp.salary);
}
```

### Book Store

```java
for (Book book : inventory) {
    System.out.println(book.title + " by " + book.author);
}
```

### Inventory System

```java
int totalStock = 0;
for (Product product : products) {
    totalStock += product.quantity;
}
System.out.println("Total stock: " + totalStock);
```

### Library System

```java
for (Book book : library.getAllBooks()) {
    if (!book.isAvailable) {
        System.out.println(book.title + " is currently issued.");
    }
}
```

---

## 18. Interview Corner

**Q1. What is the Enhanced for Loop?**
It is a simplified loop construct, introduced in Java 5, used to traverse arrays and `Iterable` collections without manual index management, using the syntax `for (dataType var : collection)`.

**Q2. Difference between `for` and Enhanced `for`?**
The traditional `for` loop provides full control (index access, custom step, direction), while the Enhanced for Loop is a read-only, forward-only traversal construct with simpler syntax but no index access.

**Q3. Can we modify array elements using Enhanced for Loop?**
For primitive-type arrays, no — the loop variable holds a copy of the value, so reassigning it doesn't affect the array. For object arrays, you **can** modify an object's internal fields through the reference, but you cannot replace the array slot itself.

**Q4. Can we get the index in an Enhanced for Loop?**
No, not directly. You would need to maintain a separate counter variable manually, or use a traditional `for` loop instead.

**Q5. Can it iterate over collections?**
Yes — it works with any class implementing `java.lang.Iterable`, which includes `ArrayList`, `LinkedList`, `HashSet`, `TreeSet`, and more.

**Q6. Is it faster than a normal for loop?**
For arrays, the Enhanced for Loop is compiled by the Java compiler into essentially the same bytecode as an index-based loop, so there is **no meaningful performance difference**. For collections, it uses an `Iterator` internally, which has similar performance to manual iterator usage.

---

## 19. MCQs

**1. In which Java version was the Enhanced for Loop introduced?**
a) Java 1.4 b) Java 5 c) Java 8 d) Java 11

> **Answer: b) Java 5**

**2. What is the correct syntax of the Enhanced for Loop?**
a) `for(i=0; i<arr.length; i++)` b) `for(dataType var : collection)` c) `foreach(var in collection)` d) `for each(var : collection)`

> **Answer: b)**

**3. Which interface must a class implement to be used in an Enhanced for Loop?**
a) `Comparable` b) `Iterable` c) `Serializable` d) `Runnable`

> **Answer: b)**

**4. What happens when you try to modify a primitive array element via the Enhanced for Loop variable?**
a) The array is updated b) Compile error c) Only the local copy changes, array remains unchanged d) Runtime exception

> **Answer: c)**

**5. Which of these CANNOT be done using an Enhanced for Loop?**
a) Print all elements b) Sum all elements c) Access index directly d) Traverse a collection

> **Answer: c)**

**6. What exception occurs if you use Enhanced for Loop on a `null` array?**
a) ArrayIndexOutOfBoundsException b) NullPointerException c) IllegalArgumentException d) No exception

> **Answer: b)**

**7. Can Enhanced for Loop traverse in reverse order?**
a) Yes, by default b) No, only forward traversal is supported c) Only for arrays d) Only for collections

> **Answer: b)**

**8. What does the colon `:` mean in `for(int num : arr)`?**
a) Assignment operator b) "in" c) Ternary operator d) Range operator

> **Answer: b) "in"**

**9. Which loop is generally preferred for simple, read-only traversal?**
a) Traditional for loop b) Enhanced for loop c) do-while loop d) Nested for loop

> **Answer: b)**

**10. What does the Enhanced for Loop use internally to traverse a Collection?**
a) Index counter b) Iterator c) Recursion d) Stack

> **Answer: b) Iterator**

**11. Which statement about modifying object array elements in Enhanced for Loop is TRUE?**
a) You can modify the object's fields through the reference b) You cannot access object fields at all c) The object is always copied d) It throws an exception

> **Answer: a)**

**12. What is a key limitation of the Enhanced for Loop?**
a) Cannot iterate collections b) Cannot access index c) Slower than traditional for loop d) Cannot be used with arrays

> **Answer: b)**

**13. Which of the following will cause a compile-time error?**
a) `for(int n : intArray)` b) `for(String s : stringArray)` c) `for(int i : stringArray)` d) `for(Object o : anyArray)`

> **Answer: c) (type mismatch — cannot assign String to int)**

**14. Is the Enhanced for Loop significantly faster than a traditional for loop for arrays?**
a) Yes, always b) No, performance is essentially the same c) Only for large arrays d) Only for small arrays

> **Answer: b)**

**15. What happens if you try to remove an element from an `ArrayList` directly inside an Enhanced for Loop?**
a) Works fine b) Throws ConcurrentModificationException c) Silently ignored d) Compile error

> **Answer: b)**

---

## 20. University Questions

### Short Questions

1. What is the Enhanced for Loop? In which Java version was it introduced?
2. Write the syntax of the Enhanced for Loop.
3. State two limitations of the Enhanced for Loop.
4. Can the Enhanced for Loop access the index of an element? Justify your answer.

### Long Questions

1. Explain the Enhanced for Loop with syntax, working, and a suitable example program. _(10 marks)_
2. Compare the traditional `for` loop and the Enhanced `for` loop in terms of syntax, readability, index access, and use cases. _(10 marks)_
3. Write a Java program to traverse an array of `Student` objects using the Enhanced for Loop and display their names and marks. _(10 marks)_
4. Discuss the situations where the Enhanced for Loop should NOT be used, with suitable examples. _(5 marks)_
5. Explain, with an example, why modifying elements of a primitive array inside an Enhanced for Loop does not affect the original array. _(5 marks)_

---

## 21. Viva Questions

1. What is another name for the Enhanced for Loop?
2. Which Java version introduced the Enhanced for Loop?
3. What interface does a class need to implement to be used with the Enhanced for Loop?
4. Can you access the array index inside an Enhanced for Loop?
5. What happens if you try to modify a primitive array element inside the loop?
6. Can the Enhanced for Loop be used with `ArrayList`?
7. What exception occurs when using the Enhanced for Loop on a `null` array?
8. Can you traverse an array in reverse using the Enhanced for Loop?
9. What does the colon (`:`) represent in the loop syntax?
10. Is the Enhanced for Loop faster than the traditional `for` loop?
11. Can you break out of an Enhanced for Loop early using `break`?
12. What happens internally when the compiler processes an Enhanced for Loop over an array vs. over a collection?
13. Why can object fields be modified inside an Enhanced for Loop, but array elements (primitives) cannot?
14. What exception can occur if you try to add/remove elements from a `List` during Enhanced for Loop iteration?
15. Give one real-world scenario where the Enhanced for Loop is clearly preferable to a traditional for loop.

---

## 22. Programming Exercises

### Easy

1. Write a program to print all elements of an integer array using the Enhanced for Loop.
2. Write a program to print all names from a `String` array using the Enhanced for Loop.

### Medium

1. Write a program to calculate the sum of all elements in an integer array using the Enhanced for Loop.
2. Write a program to find the maximum element in an array using the Enhanced for Loop.

### Hard

1. Write a program to traverse an array of `Employee` objects and print the name and salary of each employee using the Enhanced for Loop.
2. Write a program to calculate the average marks of a class, given an array of `Student` objects, using the Enhanced for Loop.

### Sample Solution — Average Marks of Students

```java
class Student {
    String name;
    int marks;
    Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }
}

public class AverageMarks {
    public static void main(String[] args) {
        Student[] students = {
            new Student("Amit", 80),
            new Student("Riya", 90),
            new Student("Sam", 70)
        };

        int total = 0;
        for (Student s : students) {
            total += s.marks;
        }

        double average = (double) total / students.length;
        System.out.println("Class Average: " + average);
    }
}
```

---

## 23. Challenge Programs

### Student Result System

Given an array of `Student` objects (name, marks in 5 subjects), use the Enhanced for Loop to calculate each student's total, percentage, and grade, then print a formatted result sheet.

### Library Book List

Given an array of `Book` objects (title, author, availability status), use the Enhanced for Loop to print all currently available books and count how many are checked out.

### Employee Salary Calculator

Given an array of `Employee` objects (name, base salary), use the Enhanced for Loop to apply a bonus percentage to each employee's salary and print the updated payroll along with the total company expenditure.

### Shopping Cart

Given an array of `Product` objects (name, price, quantity), use the Enhanced for Loop to calculate the total bill, apply a discount if the total exceeds a threshold, and print an itemized receipt.

---

## 24. Summary

- The **Enhanced for Loop** (for-each loop) was introduced in **Java 5** to simplify traversal of arrays and collections.
- Its syntax, `for (dataType var : collection)`, reads naturally as "for each `var` in `collection`."
- It works with **arrays** and any class implementing `Iterable` (like `ArrayList`, `HashSet`, `LinkedList`).
- It eliminates manual index handling, reducing the risk of index-related bugs.
- It **cannot** access the index, traverse backward, skip elements, or modify primitive array elements through the loop variable.
- For object arrays, the loop variable is a reference, so **object fields can be modified**, but the array slot itself cannot be reassigned through the loop variable.
- It is best used for **simple, read-only traversal** — display, summation, searching. The traditional `for` loop remains necessary when index access, reverse traversal, or element replacement is required.

---

## 25. Key Takeaways

- Enhanced for Loop = simpler syntax, no index, forward-only traversal.
- Works on arrays and `Iterable` collections.
- Cannot modify primitive array elements (loop variable is a copy).
- Can modify object fields (loop variable is a reference to the object).
- Cannot access index directly — use traditional `for` loop if index is needed.
- Compiles internally to an index-based loop (arrays) or `Iterator`-based loop (collections) — no real performance penalty.
- Avoid adding/removing collection elements during Enhanced for Loop iteration (`ConcurrentModificationException`).

---

## 26. Cheat Sheet

### Syntax

```java
for (dataType variable : arrayOrCollection) {
    // statements
}
```

### Advantages

- Cleaner, shorter, more readable code
- No manual index management
- No `ArrayIndexOutOfBoundsException` risk
- Works uniformly on arrays and collections

### Limitations

- No index access
- No reverse traversal
- No element modification (primitives)
- No structural modification of collections during iteration

### Comparison Table

| Aspect            | Traditional `for`            | Enhanced `for`                          |
| ----------------- | ---------------------------- | --------------------------------------- |
| Index access      | Yes                          | No                                      |
| Modify elements   | Yes                          | No (primitives) / Fields only (objects) |
| Reverse traversal | Yes                          | No                                      |
| Readability       | Moderate                     | High                                    |
| Works with        | Arrays, indexable structures | Arrays, `Iterable` collections          |

### Best Practices

- Use for **read-only** traversal.
- Use traditional `for` when index, modification, or reverse order is needed.
- Always check for `null` before iterating.
- Use meaningful loop variable names.

### Quick Examples

```java
// Array
for (int n : new int[]{1,2,3}) { System.out.println(n); }

// ArrayList
for (String s : new ArrayList<String>(List.of("A","B"))) { System.out.println(s); }

// Object array
for (Student s : students) { System.out.println(s.name); }
```

---

## 27. Common Errors

### Compilation Errors

```java
// Type mismatch
String[] names = {"A", "B"};
for (int n : names) { }   // COMPILE ERROR: incompatible types

// Trying to access loop variable outside the loop
for (int n : arr) { }
System.out.println(n);    // COMPILE ERROR: n is out of scope
```

### Runtime Errors

```java
int[] arr = null;
for (int n : arr) { }     // NullPointerException at runtime

List<String> list = new ArrayList<>(List.of("A", "B", "C"));
for (String s : list) {
    if (s.equals("B")) {
        list.remove(s);   // ConcurrentModificationException at runtime
    }
}
```

### Logic Errors

```java
int[] arr = {1, 2, 3};
for (int n : arr) {
    n = n * 2;             // LOGIC ERROR: does not update arr, easy to miss
}
System.out.println(java.util.Arrays.toString(arr)); // still [1, 2, 3]

// Assuming loop variable index exists
for (String name : names) {
    System.out.println(index + ": " + name); // LOGIC/COMPILE ERROR: no such variable
}
```

---

_End of Chapter — Enhanced for Loop (For-Each Loop) in Java_
