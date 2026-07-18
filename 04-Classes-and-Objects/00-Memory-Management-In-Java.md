# Memory Management in Java

---

## 1. Introduction

Memory management is one of the most important — and most misunderstood — topics for anyone learning Java. Java abstracts away a lot of low-level memory handling (unlike C/C++, where you manually allocate and free memory), but that doesn't mean memory management disappears. It just moves _underneath_ the language, into the JVM (Java Virtual Machine). Understanding how the JVM stores and manages your variables, objects, and method calls is the difference between writing code that "just works" and truly understanding _why_ it works — and why bugs like `NullPointerException`, `StackOverflowError`, and memory leaks happen.

This document walks through Java's memory model from the ground up: how data is stored, the difference between stack and heap, how references and objects behave, how garbage collection works, and how to avoid common mistakes.

---

## 2. Learning Objectives

By the end of these notes, you should be able to:

- Explain how Java allocates memory for variables, objects, and method calls.
- Differentiate clearly between **Stack** and **Heap** memory.
- Understand how **primitive types** and **reference types** are stored differently.
- Trace how objects are created and referenced in memory.
- Understand **pass-by-value** semantics in Java (even for objects).
- Understand when an object becomes eligible for **Garbage Collection**.
- Recognize and avoid common memory-related mistakes and leaks.
- Answer interview, viva, and exam-style questions confidently.

---

## 3. Why Should We Learn Memory Management?

### Why beginners get confused

- Java is often taught as a language where "you don't need to worry about memory" — which is _partially_ true (no manual `malloc`/`free`), but this leads beginners to skip understanding what's happening internally.
- Concepts like references vs. objects, `==` vs `.equals()`, pass-by-value vs. pass-by-reference, and `NullPointerException` all trace back to memory concepts. Without understanding memory, these feel like arbitrary rules to memorize.
- Errors like `StackOverflowError` and `OutOfMemoryError` are meaningless without knowing what the stack and heap actually are.

### Why understanding memory makes Java easier

- Once you know that variables either hold **primitive values** or **references to objects**, a huge number of "weird" behaviors in Java suddenly make sense.
- Debugging becomes easier — you can reason about _why_ two variables affect the same object, or _why_ changes inside a method don't reflect outside it.
- It builds the foundation for understanding performance, garbage collection tuning, and writing memory-efficient code.

---

## 4. How Java Stores Data

### Memory Overview

When a Java program runs, the JVM requests memory from the underlying operating system and organizes it into distinct regions, each with a specific purpose. This organized structure allows Java to manage memory automatically and safely.

### Runtime Memory

"Runtime" memory refers to memory that is allocated **while the program is executing** (as opposed to compile-time). Every time a method is called or an object is created, the JVM allocates memory at runtime for that specific execution.

### JVM Memory Model (Basic)

At a basic level, JVM memory is divided into:

| Region                      | Purpose                                                                          |
| --------------------------- | -------------------------------------------------------------------------------- |
| **Stack**                   | Stores method calls, local variables, and partial results. One stack per thread. |
| **Heap**                    | Stores all objects and arrays. Shared across all threads.                        |
| **Method Area / Metaspace** | Stores class-level data: class structure, static variables, method bytecode.     |
| **PC Registers**            | Tracks the address of the currently executing instruction per thread.            |
| **Native Method Stack**     | Supports native (non-Java) method calls.                                         |

For beginner-level understanding, the **Stack** and **Heap** are the two most important regions to master.

---

## 5. Types of Memory in Java

### Stack Memory

Used for storing method call frames and local (primitive and reference) variables. Fast, temporary, and automatically cleaned up.

### Heap Memory

Used for storing all objects and arrays created with `new`. Larger, slower to access than stack, and managed by the Garbage Collector.

### Comparison Table (Quick Preview)

| Feature      | Stack                         | Heap                            |
| ------------ | ----------------------------- | ------------------------------- |
| Stores       | Method calls, local variables | Objects, arrays                 |
| Access Speed | Faster                        | Slower                          |
| Lifetime     | Ends when method returns      | Until no longer referenced (GC) |
| Managed By   | Automatic (LIFO)              | Garbage Collector               |

_(A full detailed comparison appears in Section 8.)_

---

## 6. Stack Memory

### Definition

The **Stack** is a region of memory that stores method call information: each time a method is invoked, a new block called a **stack frame** is pushed onto the stack. When the method finishes, its frame is popped off.

### Characteristics

- Follows **LIFO** (Last-In-First-Out) order.
- Each **thread** has its own separate stack.
- Memory allocation and deallocation is automatic and fast (just moving a pointer).
- Fixed/limited size — can overflow with deep or infinite recursion.

### What is stored?

- Method call frames
- Local variables (primitives and references)
- Partial computation results
- Return addresses

### Method Calls

Every time you call a method, a new **stack frame** is created for that invocation, holding its own local variables and parameters.

### Local Variables

Variables declared _inside_ a method exist only within that method's stack frame.

### Primitive Variables

Primitive types (`int`, `char`, `boolean`, `double`, etc.) declared as local variables are stored **directly** in the stack frame — their actual value lives on the stack.

### Stack Frames

A stack frame typically contains:

- Local variable array
- Operand stack (used for intermediate computation)
- Reference to the runtime constant pool
- Return value handling info

### LIFO Principle

```
callA() calls callB() calls callC()

Stack grows like this:
| callC frame |  <- top (executes first, popped first)
| callB frame |
| callA frame |
| main frame  |  <- bottom
```

`callC` finishes first and is removed first — Last In, First Out.

### Advantages

- Very fast allocation/deallocation.
- Automatic memory cleanup (no manual work).
- Naturally supports recursion.

### Limitations

- Limited size — deep recursion causes `StackOverflowError`.
- Cannot store data that needs to outlive the method call.
- Not shared between threads (each thread needs its own stack).

### Memory Diagram

```
Thread Stack
+--------------------+
| main()             |
|  int x = 10        |
+--------------------+
| calculate()        |
|  int a = 5         |
|  int b = 15        |
+--------------------+   <- top of stack (current method)
```

---

## 7. Heap Memory

### Definition

The **Heap** is a shared memory region where all **objects** and **arrays** are stored when created using the `new` keyword (or similar object-creation mechanisms).

### Characteristics

- Shared across **all threads** in the application.
- Larger than the stack, but slower to access.
- Managed by the **Garbage Collector** — not manually freed.
- Objects persist as long as they are reachable (referenced).

### Object Storage

When you write `new ClassName()`, the actual object (its instance variables and data) is created in heap memory.

### Arrays

Arrays — whether of primitives or objects — are themselves objects in Java, so **arrays always live on the heap**, even an `int[]` array.

### Instance Variables

Instance (non-static) fields of an object are stored **inside the object itself** on the heap — not on the stack.

### Shared Memory

Because heap memory is shared, multiple references (even across different methods or threads) can point to and modify the _same_ object.

### Advantages

- Objects can outlive the method that created them.
- Enables dynamic memory allocation (size not fixed like the stack).
- Supports complex data structures (linked objects, trees, graphs).

### Limitations

- Slower access compared to stack.
- Requires garbage collection, which has performance overhead.
- Improper reference handling can cause memory leaks.

### Memory Diagram

```
Heap
+---------------------------+
| Object (Student)          |
|   name = "Aditi"          |
|   age  = 21                |
+---------------------------+
| Object (Student)          |
|   name = "Rohan"           |
|   age  = 22                |
+---------------------------+
```

---

## 8. Stack vs Heap — Comparison Table

| Aspect           | Stack                                         | Heap                                                |
| ---------------- | --------------------------------------------- | --------------------------------------------------- |
| **Size**         | Small, fixed at thread creation               | Large, can grow (up to JVM limit)                   |
| **Speed**        | Very fast (pointer arithmetic)                | Slower (dynamic allocation, GC overhead)            |
| **Lifetime**     | Ends when the method returns                  | Lives until no references point to it (GC eligible) |
| **Stores**       | Method frames, local primitives, references   | Objects, arrays, instance variables                 |
| **Visibility**   | Private to the owning thread                  | Shared across all threads                           |
| **Allocation**   | Automatic (compile-time size known per frame) | Dynamic, at runtime (`new`)                         |
| **Deallocation** | Automatic when frame pops                     | Automatic via Garbage Collector                     |

---

## 9. Variables in Memory

### Primitive Variables Example

```java
void method() {
    int x = 10;   // stored directly on the stack
    double y = 5.5; // stored directly on the stack
}
```

### Memory Diagram

```
Stack (method frame)
+----------------+
| x = 10         |
| y = 5.5        |
+----------------+
```

No heap involvement at all — primitives are self-contained values.

---

## 10. Reference Variables

### What is a Reference?

A **reference variable** does not store the object itself — it stores the **memory address (handle)** pointing to where the actual object lives on the heap.

### Why References Exist

Objects can be large and variable in size, so it wouldn't be efficient (or even feasible) to store them directly inside a stack frame. Instead, Java stores a small, fixed-size _reference_ on the stack, which "points to" the full object on the heap.

### Reference vs Object

```java
Student s = new Student("Aditi");
```

- `s` → stored on the **stack** (a reference/address)
- `Student("Aditi")` object → stored on the **heap**

### Memory Diagram

```
Stack                     Heap
+-----------+            +----------------------+
| s ---------------------> | Student Object      |
+-----------+            |   name = "Aditi"     |
                          +----------------------+
```

---

## 11. Objects in Memory

### Creating Objects

```java
Student s1 = new Student("Aditi");
```

### `new` Keyword

The `new` keyword tells the JVM: "allocate memory on the heap for this object, run the constructor, and return the reference (address)."

### Memory Allocation

1. JVM allocates space on the heap for the new `Student` object.
2. The constructor runs, initializing instance variables.
3. The address of that memory is returned.

### Reference Assignment

```java
Student s1 = new Student("Aditi");
```

- Step 1: Heap space created for `Student` object.
- Step 2: The address of that object is assigned to `s1` (on the stack).

### Memory Diagram

```
Stack               Heap
+--------+          +--------------------+
| s1 -----------------> | Student           |
+--------+          |   name = "Aditi"   |
                     +--------------------+
```

---

## 12. Arrays in Memory

### Array Allocation

Arrays are objects in Java — always heap-allocated, regardless of element type.

### Primitive Array

```java
int[] nums = {1, 2, 3};
```

```
Stack                Heap
+-----------+        +----------------+
| nums --------------> | [1, 2, 3]     |
+-----------+        +----------------+
```

### Object Array

```java
Student[] students = new Student[2];
students[0] = new Student("Aditi");
students[1] = new Student("Rohan");
```

### Memory Diagram

```
Stack                 Heap
+--------------+       +------------------------+
| students ------------> | [ ref0, ref1 ]        |
+--------------+       |    |       |            |
                       |    v       v            |
                       | Student   Student       |
                       | "Aditi"   "Rohan"       |
                       +------------------------+
```

The array itself holds **references** to the `Student` objects, which are separately allocated on the heap.

---

## 13. Method Calls

### Method Stack

Each method invocation gets a fresh **stack frame**, isolated from other calls.

### Stack Frames

```java
void main() {
    int a = add(2, 3);
}
int add(int x, int y) {
    return x + y;
}
```

```
Stack
+------------------+
| add(): x=2, y=3  |  <- pushed when add() is called
+------------------+
| main(): a        |
+------------------+
```

### Method Return

When `add()` returns, its frame is **popped off** the stack, and control (with the return value) goes back to `main()`.

### Memory Diagram

```
Before return:            After return:
+----------------+        +----------------+
| add() frame    |        | main(): a = 5  |
+----------------+        +----------------+
| main() frame   |
+----------------+
```

---

## 14. Parameter Passing

Java is **strictly pass-by-value** — always. What differs is _what value_ gets passed.

### Primitive Types

A **copy of the value** is passed. Changes inside the method do not affect the original variable.

```java
void change(int x) { x = 100; }

int a = 5;
change(a);
// a is still 5
```

### Reference Types

A **copy of the reference (address)** is passed — not the object itself. So the method _can_ modify the object's internal state (since it points to the same heap object), but **cannot** make the original variable point to a different object.

```java
void changeName(Student s) { s.name = "Changed"; }  // affects the original object

void reassign(Student s) { s = new Student("New"); } // does NOT affect the caller's reference
```

### Why Java is Pass-by-Value

Even for objects, what's copied is the **reference value** (the address), not the object. This is why:

- Mutating an object's fields _through_ a passed reference **is visible** to the caller.
- Reassigning the parameter to a new object **is not visible** to the caller — the local copy of the reference now points elsewhere, but the caller's original reference is untouched.

### Memory Illustration

```
Caller:  s1 ------> [Student: "Aditi"]

changeName(s1) called:
   param s ------> [Student: "Aditi"]   (same object, s.name changed -> visible)

reassign(s1) called:
   param s ------> [Student: "Aditi"]
   s = new Student("New")
   param s ------> [Student: "New"]     (s1 in caller still points to "Aditi")
```

---

## 15. Multiple References

Two (or more) variables can point to the **same object** on the heap. Any change made through one reference is visible through the other.

```java
Student s1 = new Student("Aditi");
Student s2 = s1;      // s2 now points to the SAME object
s2.name = "Rohan";
System.out.println(s1.name); // prints "Rohan"
```

### Memory Diagram

```
Stack               Heap
+--------+          +----------------------+
| s1 --------\        | Student              |
+--------+    \------> |   name = "Rohan"   |
| s2 --------/        +----------------------+
+--------+
```

Both `s1` and `s2` are separate variables (separate stack slots), but they hold the **same address**, so they refer to one shared object.

---

## 16. Null References

### Definition

A reference variable that points to **no object** is said to hold `null`. It's a special literal meaning "this reference is not pointing to any memory address."

```java
Student s = null;
```

### NullPointerException

Occurs when you try to **use** (call a method/access a field on) a reference that is `null` — because there's no actual object to operate on.

```java
Student s = null;
System.out.println(s.name); // throws NullPointerException
```

### Examples

```java
String str = null;
str.length();          // NullPointerException

int[] arr = null;
System.out.println(arr[0]); // NullPointerException

Map<String, String> map = null;
map.get("key");         // NullPointerException
```

---

## 17. Object Eligibility (for Garbage Collection)

### When does an object become eligible for GC?

An object becomes **eligible for garbage collection** when it is no longer **reachable** from any live thread or static reference — i.e., no active reference path leads to it.

Common scenarios:

1. **Reference set to null**
   ```java
   Student s = new Student("Aditi");
   s = null; // object now unreachable -> eligible for GC
   ```
2. **Reference reassigned to another object**
   ```java
   Student s = new Student("A");
   s = new Student("B"); // "A" object is now unreachable
   ```
3. **Object created inside a method, method ends**
   ```java
   void create() {
       Student s = new Student("Temp");
   } // once create() returns, "Temp" object is eligible for GC
   ```
4. **Island of isolation** — two or more objects reference each other but nothing outside references either of them.

### Examples

```java
class Node {
    Node next;
}

Node a = new Node();
Node b = new Node();
a.next = b;
b.next = a;

a = null;
b = null;
// a and b objects reference each other, but nothing external references them
// -> both eligible for GC (modern GCs handle cyclic references)
```

---

## 18. Garbage Collection (Introduction)

### What is Garbage Collection?

Garbage Collection (GC) is the JVM's automatic process of identifying and reclaiming heap memory occupied by objects that are no longer reachable/used by the program.

### Why Garbage Collection?

- Frees developers from manually deallocating memory (unlike C/C++'s `free()`/`delete`).
- Prevents (most) memory leaks caused by forgetting to release memory.
- Reduces bugs like dangling pointers / use-after-free errors.

### Automatic Memory Management

The JVM periodically runs the GC in the background, pausing or working alongside the application (depending on the GC algorithm) to reclaim heap space from unreachable objects.

### Garbage Collector

The GC generally works by:

1. Identifying "live" (reachable) objects, starting from **GC roots** (local variables on stacks, static variables, active threads, JNI references).
2. Marking everything unreachable as garbage.
3. Reclaiming that memory (and often compacting the heap).

_(Common algorithms: Mark-and-Sweep, Generational GC (Young/Old Gen), G1, ZGC — these are typically covered in advanced/JVM-tuning topics beyond this basic introduction.)_

### `finalize()` (Historical Note)

- Previously, objects could override `finalize()` to run cleanup code just before GC reclaimed them.
- **Deprecated** since Java 9 and scheduled for removal — it was unreliable (no guaranteed timing), and could hurt performance.
- Modern alternative: `try-with-resources` and the `AutoCloseable` interface for deterministic cleanup.

### `System.gc()`

- A method that **requests** the JVM run garbage collection.
- It is only a **hint** — the JVM is free to ignore it.
- Generally discouraged in production code; the JVM's own heuristics are usually better at deciding when to collect.

---

## 19. Memory Leaks in Java (Basic)

Even with automatic GC, memory leaks _can_ still happen in Java — they occur when objects are **no longer needed but are still reachable**, so the GC can't reclaim them.

### Holding unnecessary references

```java
class Cache {
    static List<Object> data = new ArrayList<>();

    void add(Object o) {
        data.add(o); // objects added here are never removed
    }
}
```

Since `data` is `static`, it lives for the program's lifetime, and anything added to it stays reachable forever unless explicitly removed.

### Collections

- Common leak source: adding objects to a `List`, `Map`, or `Set` and forgetting to remove them once they're no longer needed.
- Long-lived collections (especially static ones, or caches) are the most frequent culprits.
- Fix strategies: remove entries when done, use bounded caches, or use `WeakHashMap` / weak references where appropriate.

---

## 20. Common Misconceptions

### Object vs Reference

- **Misconception:** "The variable _is_ the object."
- **Reality:** The variable is a _reference_ (a pointer/address) to the object. The object itself lives separately on the heap.

### Stack vs Heap

- **Misconception:** "All variables are stored in the same place."
- **Reality:** Primitives and references live on the stack (per-method); actual objects and arrays live on the heap.

### Reference is NOT an object

- A reference variable and the object it points to are two different memory entities. Assigning references (`s2 = s1`) copies the _address_, not the object.

### `new` keyword

- **Misconception:** "`new` just creates a variable."
- **Reality:** `new` allocates memory **on the heap**, runs the constructor, and returns a reference — the "variable" is just where that reference gets stored (typically on the stack).

---

## 21. Best Practices

- Nullify references to large objects when they're no longer needed, especially in long-lived scopes (e.g., static fields, caches).
- Prefer local variables over static/instance variables when the data doesn't need to persist — locals are automatically cleaned up.
- Be cautious with static collections — they live for the program's lifetime.
- Close resources (streams, connections) using `try-with-resources` instead of relying on `finalize()`.
- Avoid unnecessary object creation inside loops — reuse objects where sensible.
- Use appropriate collection types (`WeakHashMap`, etc.) for caches that shouldn't prevent GC.
- Avoid deep/unbounded recursion to prevent `StackOverflowError`.

---

## 22. Common Mistakes

- Assuming Java is pass-by-reference for objects (it is **pass-by-value of the reference**).
- Forgetting that arrays are heap objects, even for primitive arrays.
- Comparing objects with `==` instead of `.equals()` (comparing references, not content).
- Letting static collections grow indefinitely.
- Ignoring `NullPointerException` root causes instead of understanding _why_ a reference was null.
- Relying on `System.gc()` or `finalize()` for critical cleanup logic.
- Confusing "object eligible for GC" with "object immediately destroyed" — GC timing isn't guaranteed.

---

## 23. Interview Corner — 20 Questions

1. What is the difference between Stack and Heap memory in Java?
2. Where are local variables stored?
3. Where are instance variables stored?
4. Is Java pass-by-value or pass-by-reference?
5. What happens in memory when you write `Student s = new Student();`?
6. Why are arrays always stored on the heap?
7. What causes a `NullPointerException`?
8. When does an object become eligible for garbage collection?
9. What is a stack frame?
10. What causes a `StackOverflowError`?
11. What is the role of the Garbage Collector?
12. Is `finalize()` still recommended? Why or why not?
13. What are GC roots?
14. Can two references point to the same object? What happens if you modify one?
15. Explain "island of isolation" in the context of GC.
16. What is a memory leak in Java, given that GC exists?
17. What's the difference between `==` and `.equals()` in terms of memory?
18. Does calling `System.gc()` guarantee garbage collection?
19. Why doesn't reassigning a method parameter affect the caller's variable?
20. What is the Method Area / Metaspace used for?

---

## 24. MCQs — 20 Questions

1. Which memory area stores method call frames?  
   a) Heap b) Stack c) Metaspace d) Cache

2. Objects in Java are created in:  
   a) Stack b) Heap c) Register d) Cache

3. Java parameter passing is:  
   a) Pass-by-reference b) Pass-by-value c) Pass-by-pointer d) Depends on type

4. Which of these lives on the heap?  
   a) `int x = 5` b) `boolean flag` c) `new Student()` d) method return address

5. An array in Java is:  
   a) Always a primitive b) Always an object c) Stored on stack d) Never garbage collected

6. What causes `StackOverflowError`?  
   a) Too many objects b) Infinite/deep recursion c) Null reference d) Heap overflow

7. A reference variable stores:  
   a) The object's data b) The object's address c) A copy of the object d) Nothing

8. When is an object eligible for GC?  
   a) When declared b) When unreachable c) When null d) Never

9. `finalize()` is:  
   a) Mandatory b) Deprecated c) Faster than GC d) A stack method

10. `System.gc()`:  
    a) Guarantees GC b) Is a request/hint c) Deletes objects immediately d) Crashes JVM

11. Local primitive variables are stored:  
    a) Heap b) Stack c) Method Area d) Cache

12. Which causes NullPointerException?  
    a) `int x = 0` b) `String s = null; s.length();` c) `new Student()` d) `int[] a = new int[5]`

13. Instance variables are stored:  
    a) Inside the object on heap b) On stack c) In Metaspace d) In cache

14. Two references pointing to the same object:  
    a) Create two objects b) Share the same object c) Cause an error d) Are illegal

15. Which is shared across threads?  
    a) Stack b) Heap c) PC Register d) Native stack

16. Which region stores class-level static data?  
    a) Stack b) Heap c) Method Area/Metaspace d) Register

17. What happens when a method returns?  
    a) Heap is cleared b) Its stack frame is popped c) GC runs immediately d) Nothing

18. Static collections holding unused objects can cause:  
    a) Faster GC b) Memory leaks c) Stack overflow d) Compilation error

19. `==` for objects compares:  
    a) Field values b) References/addresses c) Hash only d) Nothing

20. GC roots include:  
    a) Local variables on stack b) Static variables c) Active thread references d) All of the above

---

## 25. University Questions

1. Explain the JVM memory model with a labeled diagram.
2. Differentiate between Stack and Heap memory with examples.
3. Describe how objects are created and stored in memory when using the `new` keyword.
4. Explain pass-by-value in the context of Java method calls, with an example involving objects.
5. What is Garbage Collection? Explain how an object becomes eligible for GC.
6. Explain the concept of memory leaks in Java despite having automatic garbage collection.
7. Draw and explain a memory diagram showing two references pointing to the same object.
8. Differentiate between reference variables and objects with a suitable example.

---

## 26. Viva Questions

1. What is stored in a stack frame?
2. Why can't objects be stored on the stack directly?
3. What is the lifetime of a local variable vs. an instance variable?
4. Give a real example where `NullPointerException` occurs.
5. What is the difference between `s1 == s2` and `s1.equals(s2)`?
6. Why is understanding memory management important for writing efficient Java code?
7. What happens to an object's memory when all references to it are removed?

---

## 27. Practice Problems

1. **Trace the stack:** Given nested method calls `main() -> A() -> B() -> C()`, draw the stack at the moment `C()` is executing, and again after `C()` returns.
2. **Reference tracking:** Given:
   ```java
   Student s1 = new Student("A");
   Student s2 = s1;
   Student s3 = new Student("B");
   s1 = s3;
   ```
   Draw the final state of stack and heap. Which object (if any) is eligible for GC?
3. **Pass-by-value:** Predict the output:
   ```java
   void modify(int[] arr) {
       arr[0] = 99;
       arr = new int[]{1,2,3};
   }
   int[] a = {5, 6, 7};
   modify(a);
   System.out.println(a[0]);
   ```
4. **NPE hunting:** Identify which lines throw `NullPointerException`:
   ```java
   String a = null;
   String b = "hello";
   System.out.println(b.length());   // line 1
   System.out.println(a.length());   // line 2
   ```
5. **GC eligibility:** In the following, list all objects (by variable name at creation) that become eligible for GC, and at which line:
   ```java
   Student s1 = new Student("A"); // line 1
   Student s2 = new Student("B"); // line 2
   s1 = s2;                        // line 3
   s2 = null;                      // line 4
   ```

---

## 28. Summary

Java's memory model divides responsibilities cleanly between the **Stack** (fast, per-thread, stores method calls and primitive/reference variables) and the **Heap** (shared, stores all actual objects and arrays). Every variable you declare is either a primitive value living directly on the stack, or a reference — an address — pointing to an object that lives on the heap. Java is always pass-by-value, but because object variables hold references, passing an object _reference_ by value allows the called method to modify the object's internal state without being able to redirect the caller's variable to a different object. The Garbage Collector automatically reclaims heap memory occupied by objects that are no longer reachable, freeing developers from manual memory management — though careless reference-holding (e.g., static collections) can still cause memory leaks even with GC in place.

---

## 29. Key Takeaways

- **Stack** = method calls + local variables (primitives & references). Fast, automatic, per-thread.
- **Heap** = all objects & arrays. Shared, GC-managed, slower.
- A reference variable ≠ the object; it's an address pointing to the object.
- Java is strictly **pass-by-value** — for objects, the _reference_ is copied, not the object.
- An object becomes GC-eligible when it's no longer reachable from any GC root.
- `finalize()` is deprecated; `System.gc()` is only a request, never a guarantee.
- Memory leaks in Java happen when references are unintentionally kept alive (e.g., static collections).

---

## 30. Cheat Sheet

| Concept                  | One-Line Rule                                                  |
| ------------------------ | -------------------------------------------------------------- |
| Local primitive          | Lives on stack                                                 |
| Local reference variable | Address stored on stack                                        |
| Object (via `new`)       | Lives on heap                                                  |
| Array (any type)         | Lives on heap                                                  |
| Instance variable        | Stored inside the object on heap                               |
| Static variable          | Stored in Method Area / Metaspace                              |
| Method call              | Pushed as a new stack frame                                    |
| Method return            | Stack frame popped                                             |
| Parameter passing        | Always pass-by-value (reference's value is copied for objects) |
| GC eligibility           | No reachable reference from any GC root                        |
| `finalize()`             | Deprecated — avoid                                             |
| `System.gc()`            | Just a hint, not a guarantee                                   |
| `NullPointerException`   | Trying to use a `null` reference                               |
| Memory leak in Java      | Reachable but unused references (e.g. static collections)      |

---

_End of Notes — Memory Management in Java_
