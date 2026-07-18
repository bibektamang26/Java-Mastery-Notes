# Methods in Java

---

## 1. Introduction

### What is a Method?

A **method** is a named block of code inside a class that performs a specific task, and can be executed ("called" or "invoked") whenever that task needs to happen — potentially many times, from many places in a program.

### Why Methods are Needed?

Without methods, every piece of logic would have to be written out in full, every single time it's needed — leading to long, repetitive, error-prone code. Methods let us define a task **once** and reuse it wherever needed, simply by calling its name.

### Importance of Methods

Methods are central to writing clean, organized, object-oriented Java code. They let objects expose **behavior** (as introduced in the Objects chapter), break large problems into smaller, manageable pieces, and form the basis for more advanced concepts like overloading, overriding, and polymorphism.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define what a method is and identify its components.
- Declare and call methods in Java.
- Understand different types of methods based on parameters and return values.
- Understand how parameters are passed to methods.
- Understand variable scope and the call stack during method execution.

---

## 3. What is a Method?

### Definition

A method is a **reusable, named block of code** that performs a specific operation, optionally accepts input (parameters), and optionally produces output (a return value).

### Real-life Analogy

Think of a method like a **vending machine**: you provide some input (money + a selection code — the "parameters"), the machine performs an internal process, and it gives you back an output (the snack — the "return value"). You don't need to know _how_ the machine works internally; you just need to know what to give it and what you'll get back.

### Characteristics

- Has a **name** used to invoke it.
- Can accept **input** via parameters.
- Can produce **output** via a return value.
- Encapsulates a specific, well-defined task.
- Can be called (invoked) multiple times from different places.

---

## 4. Why Do We Need Methods?

### Code Reusability

Once a method is written, it can be called as many times as needed, from anywhere it's accessible — avoiding the need to rewrite the same logic repeatedly.

### Modularity

Methods let you break a large, complex program into smaller, independent, manageable pieces — each method handling one specific task.

### Readability

Well-named methods (e.g., `calculateTotal()`, `isEligible()`) make code self-documenting — a reader can understand _what_ a program does at a glance, without needing to trace through every line of logic immediately.

### Maintainability

When logic lives in one method, fixing a bug or improving that logic only requires changing it in **one place**, rather than hunting down every duplicated copy scattered throughout the code.

### Reduced Code Duplication

Methods directly eliminate repeated blocks of identical or near-identical code, following the "Don't Repeat Yourself" (DRY) principle.

---

## 5. Anatomy of a Method

```java
public void display() {
}
```

### Explain each component

- **Access Modifier** (`public`) — controls which parts of the program can call this method (e.g., `public`, `private`, `protected`, default).
- **Return Type** (`void`) — specifies the data type of the value the method sends back to its caller; `void` means it returns nothing.
- **Method Name** (`display`) — the identifier used to call/invoke the method.
- **Parameters** (`()` — empty here) — the list of inputs the method accepts, each with a type and a name; empty parentheses mean no input is required.
- **Method Body** (`{ }`) — the block of code containing the actual statements/logic executed when the method is called.

```java
public int add(int a, int b) {   // access modifier | return type | name | parameters
    return a + b;                 // method body
}
```

---

## 6. Method Declaration

### Syntax

```java
accessModifier returnType methodName(parameterList) {
    // method body
    // (optional) return statement
}
```

### Examples

```java
public void greet() {
    System.out.println("Hello!");
}

public int square(int n) {
    return n * n;
}

private String getGrade(double marks) {
    if (marks >= 90) return "A";
    return "B";
}
```

### Naming Rules

- Use **camelCase**: `calculateTotal`, `getName`, `isValid`.
- Prefer **verbs or verb phrases**, since a method represents an action: `displayInfo()`, `calculateSalary()`.
- Boolean-returning methods often start with `is`/`has`/`can`: `isEligible()`, `hasPermission()`.
- Names should be descriptive — avoid vague names like `doStuff()` or `process()` without context.

---

## 7. Calling a Method

### Method Invocation

To execute a method, you **call** it by writing its name followed by parentheses (with any required arguments), typically using the dot operator on an object (for instance methods).

### Examples

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }
}

Calculator calc = new Calculator();
int result = calc.add(5, 3);   // method invocation
System.out.println(result);     // 8
```

```java
class Greeter {
    void sayHello() {
        System.out.println("Hello!");
    }
}

Greeter g = new Greeter();
g.sayHello();   // method invocation, no return value used
```

---

## 8. Types of Methods

### Predefined Methods

Methods that are already written and provided by Java itself (in the standard library), ready to use without needing to define them yourself.

```java
Math.sqrt(25);          // predefined method from the Math class
"hello".length();        // predefined method from the String class
System.out.println();  // predefined method from PrintStream
```

### User-Defined Methods

Methods that the programmer writes themselves, specific to their program's needs.

```java
class Student {
    void displayInfo() {   // user-defined method
        System.out.println("Student info...");
    }
}
```

---

## 9. Method Parameters

Parameters are the **inputs** a method accepts, listed inside its parentheses, each with a data type and a name.

### No Parameters

```java
void sayHello() {
    System.out.println("Hello!");
}
// called as: sayHello();
```

### Single Parameter

```java
void greet(String name) {
    System.out.println("Hello, " + name);
}
// called as: greet("Aditi");
```

### Multiple Parameters

```java
int add(int a, int b) {
    return a + b;
}
// called as: add(5, 3);
```

### Examples

```java
void printDetails(String name, int age, double gpa) {
    System.out.println(name + " | " + age + " | " + gpa);
}
// called as: printDetails("Rohan", 22, 8.5);
```

---

## 10. Return Type

The **return type** specifies what kind of value (if any) a method sends back to whoever called it, using the `return` keyword.

### `void`

Indicates the method returns **no value** at all.

```java
void printMessage() {
    System.out.println("Just printing, nothing returned.");
}
```

### `int`

Returns a whole number.

```java
int getAge() {
    return 21;
}
```

### `double`

Returns a decimal (floating-point) number.

```java
double getGpa() {
    return 8.7;
}
```

### `String`

Returns text data.

```java
String getName() {
    return "Aditi";
}
```

### `Object`

Returns a reference to any object type (including custom classes).

```java
Student getStudent() {
    return new Student();
}
```

### Examples

```java
class Circle {
    double radius = 5.0;

    double calculateArea() {          // returns double
        return Math.PI * radius * radius;
    }

    String describe() {                 // returns String
        return "A circle with radius " + radius;
    }
}
```

---

## 11. Four Types of Methods

Based on whether a method takes parameters and/or returns a value, every method falls into one of four categories:

### 1. No arguments, No return value

```java
void printWelcome() {
    System.out.println("Welcome!");
}
// Usage: printWelcome();
```

### 2. Arguments, No return value

```java
void printSum(int a, int b) {
    System.out.println("Sum: " + (a + b));
}
// Usage: printSum(4, 6);
```

### 3. No arguments, Return value

```java
int getDefaultAge() {
    return 18;
}
// Usage: int age = getDefaultAge();
```

### 4. Arguments, Return value

```java
int add(int a, int b) {
    return a + b;
}
// Usage: int result = add(5, 3);
```

---

## 12. Parameter Passing

### Primitive Types

When a primitive value (e.g., `int`, `double`, `boolean`) is passed as an argument, a **copy of the value** is passed. Changes made to the parameter inside the method do **not** affect the original variable in the caller.

```java
void increment(int x) {
    x = x + 1;
}

int num = 5;
increment(num);
System.out.println(num);  // still 5
```

### Object References (Introduction)

When an object is passed as an argument, a **copy of the reference (address)** is passed — not the object itself. This means the method _can_ modify the object's internal state (since it's pointing to the same heap object), but cannot make the caller's variable point to a different object.

```java
void changeName(Student s) {
    s.name = "Changed";   // affects the original object
}
```

_(Full detail on this distinction — including memory diagrams — is covered in the Memory Management chapter.)_

---

## 13. Method Overloading (Introduction)

### Definition

**Method overloading** means defining multiple methods in the same class with the **same name**, but with **different parameter lists** (different number, type, or order of parameters).

### Examples

```java
int add(int a, int b) {
    return a + b;
}

double add(double a, double b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}
```

Java determines _which_ `add()` to call based on the arguments provided at the call site.

_(Method overloading is explained in full depth, including rules and examples, in the Polymorphism chapter.)_

---

## 14. Recursion (Introduction)

### Definition

**Recursion** occurs when a method calls **itself**, directly or indirectly, in order to solve a problem by breaking it down into smaller sub-problems of the same type.

### Examples

```java
int factorial(int n) {
    if (n == 0) return 1;        // base case
    return n * factorial(n - 1); // recursive case
}
// factorial(5) -> 5 * 4 * 3 * 2 * 1 = 120
```

### Advantages

- Can express certain problems (tree traversal, factorial, Fibonacci) more naturally and concisely than iterative loops.
- Well-suited to problems with a naturally recursive structure (e.g., divide-and-conquer algorithms).

### Disadvantages

- Each recursive call adds a new **stack frame**, so deep or infinite recursion can cause a `StackOverflowError`.
- Often less memory-efficient and sometimes slower than an equivalent iterative (loop-based) solution.
- Can be harder to read/debug for those unfamiliar with the technique.

---

## 15. Scope of Variables

**Scope** determines _where_ in the code a variable can be accessed/used.

### Local Variables

Declared inside a method (or block); only accessible **within that method/block**, and destroyed once the method finishes executing.

```java
void calculate() {
    int result = 10;   // local variable, exists only within calculate()
}
```

### Parameters

Parameters behave like local variables — they exist only within the method they were declared in, initialized with the values passed at the call site.

```java
void greet(String name) {   // 'name' is scoped to greet()
    System.out.println(name);
}
```

### Instance Variables (Brief)

Fields declared at the class level (outside any method) are accessible throughout **all** non-static methods of that class, and persist for the lifetime of the object (not just one method call).

```java
class Student {
    String name;   // instance variable — accessible in every method of this class

    void display() {
        System.out.println(name);  // accessible here
    }
}
```

_(Instance variables were introduced in the Classes chapter and are covered further in later chapters.)_

---

## 16. Call Stack

### Method Execution

Every time a method is called, the JVM pushes a new **stack frame** for that call onto the **call stack**, holding its local variables, parameters, and execution state. When the method finishes, its frame is popped off, and control returns to the caller.

### Stack Frames

```java
void main() {
    int result = add(2, 3);
}
int add(int a, int b) {
    return a + b;
}
```

```
Call order: main() calls add()

Stack (while add() is executing):
+---------------------+
| add(): a=2, b=3     |  <- top (current, executing)
+---------------------+
| main(): result      |
+---------------------+
```

### Memory Diagram

```
Recursive call: factorial(3)

+----------------------+
| factorial(0): n=0    |  <- base case reached, top of stack
+----------------------+
| factorial(1): n=1    |
+----------------------+
| factorial(2): n=2    |
+----------------------+
| factorial(3): n=3    |
+----------------------+
| main()               |
+----------------------+
```

As each `factorial()` call returns, its frame is popped, and the multiplication is completed one level at a time, back down to `main()`.

---

## 17. Best Practices

- Keep methods **short and focused** on a single task (Single Responsibility applied to methods).
- Use **descriptive, verb-based names** that clearly convey what the method does.
- Prefer fewer parameters where possible — too many parameters can indicate a method is doing too much.
- Always handle **edge cases** (e.g., null inputs, boundary values) within the method body.
- Add a **base case** to every recursive method to prevent infinite recursion.
- Return early where it improves readability, rather than deeply nesting conditionals.

---

## 18. Common Mistakes

- **Forgetting a `return` statement** in a non-`void` method (won't compile).
- **Mismatched argument types/count** when calling a method (compilation error).
- **Infinite recursion** — forgetting or incorrectly defining the base case, leading to `StackOverflowError`.
- **Confusing parameters with arguments** — parameters are the method's declared inputs; arguments are the actual values supplied at the call site.
- **Modifying a parameter expecting it to affect the caller** — for primitives, this never happens; for objects, only field mutations are visible, not reassignment of the reference itself.
- **Overly long methods** that try to do too many unrelated things at once.

---

## 19. Interview Corner

1. **What is a method in Java?**
   A named, reusable block of code that performs a specific task, optionally taking input and producing output.

2. **What is the difference between a method and a function?**
   In Java, methods are always associated with a class (there are no standalone "functions" outside of classes); the term "function" is often used informally to mean the same thing.

3. **What are the four types of methods based on parameters and return values?**
   No-args/no-return, args/no-return, no-args/return, args/return.

4. **What happens if a non-void method has no return statement?**
   It will not compile — Java requires every code path in a non-void method to return a value.

5. **What is the difference between a parameter and an argument?**
   A parameter is the variable listed in the method's declaration; an argument is the actual value passed when the method is called.

6. **What is recursion, and what risk does it carry?**
   A method calling itself to solve smaller sub-problems; without a proper base case, it risks a `StackOverflowError`.

7. **What is method overloading?**
   Defining multiple methods with the same name but different parameter lists within the same class.

---

## 20. MCQs

1. What does the `void` return type mean?
   a) Returns an integer b) Returns nothing c) Returns null d) Returns a string

2. Which part of a method controls its accessibility?
   a) Return type b) Access modifier c) Method body d) Parameter list

3. What is a parameter?
   a) A value passed during a method call b) A variable declared in the method signature c) The method's return type d) The method's name

4. Method overloading means:
   a) Same method name, different parameter lists b) Same method name, same parameters c) Different method names, same body d) Deleting a method

5. Which of these is a predefined method?
   a) `displayInfo()` b) `Math.sqrt()` c) `calculateSalary()` d) A custom `add()` method

6. What happens when a primitive is passed to a method and modified inside it?
   a) The original variable changes b) The original variable is unaffected c) A compile error occurs d) The method fails at runtime

7. A method with no parameters and no return value is:
   a) `int calc()` b) `void display()` c) `String getName()` d) `double compute(int x)`

8. What triggers a `StackOverflowError` in recursion?
   a) Too few parameters b) Missing/incorrect base case causing infinite calls c) Using `void` return type d) Declaring too many local variables

9. Each method call creates a new:
   a) Class b) Object c) Stack frame d) Package

10. Which naming convention is used for method names?
    a) PascalCase b) camelCase c) snake_case d) UPPERCASE

11. A local variable's scope is limited to:
    a) The entire class b) The entire program c) The method/block it's declared in d) All subclasses

12. What is returned by a method declared as `int getAge()`?
    a) Nothing b) A String c) An integer value d) A boolean

13. Passing an object reference as an argument passes:
    a) A full copy of the object b) A copy of the reference (address) c) Nothing d) The class file

14. Which of the following reduces code duplication?
    a) Writing the same logic repeatedly b) Using methods c) Avoiding methods d) Declaring more classes

15. What is the method body?
    a) The method's name b) The block of code that executes when the method is called c) The return type d) The parameter list

16. Which statement about recursion is TRUE?
    a) It never uses the call stack b) It always outperforms loops c) It requires a base case to terminate d) It cannot return a value

17. `String greet(String name)` is an example of:
    a) No arguments, no return value b) Arguments, return value c) No arguments, return value d) Arguments, no return value

18. What happens if you call a method with the wrong number of arguments?
    a) Runtime warning only b) Compilation error c) It runs with defaults d) It's automatically overloaded

19. Which keyword is used to send a value back from a method?
    a) `return` b) `void` c) `new` d) `this`

20. Instance variables, compared to local variables, are:
    a) Accessible only within one method b) Accessible throughout the class's non-static methods c) Destroyed after each method call d) Not stored anywhere

---

## 21. University Questions

### Short Questions

1. Define a method and list its components.
2. Differentiate between predefined and user-defined methods.
3. What is the difference between a parameter and an argument?
4. What is recursion? Give an example.
5. What is the scope of a local variable?

### Long Questions

1. Explain the anatomy of a Java method with a labeled example.
2. Discuss the four types of methods based on parameters and return values, with an example of each.
3. Explain how the call stack works during method execution, including recursive calls, with a diagram.
4. Describe how parameters are passed in Java for primitive types versus object references.
5. Explain method overloading with examples, and state why it is useful.

---

## 22. Viva Questions

1. What is a method in Java?
2. What is the purpose of the `return` keyword?
3. What is the difference between `void` and a non-void return type?
4. Can a method have no parameters? Give an example.
5. What is method overloading?
6. What is recursion, and what is a base case?
7. What is a stack frame, and when is it created?
8. What is the difference between local and instance variables?
9. Does passing an object to a method let you change what the caller's reference points to? Why or why not?
10. What error occurs from uncontrolled recursion?

---

## 23. Programming Exercises

1. Write a method `isEven(int n)` that returns whether a number is even.
2. Write a method `calculateArea(double radius)` that returns the area of a circle.
3. Write an overloaded `add()` method that works for two integers, two doubles, and three integers.
4. Write a recursive method to calculate the factorial of a number.
5. Write a method `displayStudent(String name, int age)` that prints student details (no return value).

---

## 24. Summary

A method is a named, reusable block of code that encapsulates a specific task, optionally accepting input through parameters and optionally producing output through a return value. Methods are essential for writing modular, readable, and maintainable Java programs, letting logic be defined once and reused wherever needed. Every method can be classified by whether it takes parameters and/or returns a value, and every method call creates a new stack frame that is pushed and popped as part of the call stack — a mechanism that also underlies recursion, where a method calls itself to solve smaller sub-problems. Understanding parameter passing (especially the distinction between primitives and object references), variable scope, and the call stack builds directly on the memory concepts from earlier chapters and prepares the way for more advanced topics like overloading and polymorphism.

---

## 25. Key Takeaways

- A method is a reusable block of code with an access modifier, return type, name, parameters, and body.
- Methods reduce duplication and improve modularity, readability, and maintainability.
- Every method is one of four types: no-args/no-return, args/no-return, no-args/return, args/return.
- Primitives are passed by value (copy); object references are also passed by value, but the copy still points to the same object.
- Method overloading allows multiple methods with the same name but different parameter lists.
- Recursion requires a proper base case to avoid `StackOverflowError`.
- Local variables and parameters are scoped to their method; instance variables are scoped to the whole object.
- Every method call adds a stack frame to the call stack, removed when the method returns.

---

## 26. Cheat Sheet

| Concept                        | Quick Reference                                                          |
| ------------------------------ | ------------------------------------------------------------------------ |
| **Method Syntax**              | `accessModifier returnType methodName(parameters) { body }`              |
| **Four Method Types**          | No-args/no-return · Args/no-return · No-args/return · Args/return        |
| **Predefined vs User-Defined** | Built into Java (`Math.sqrt()`) vs. written by the programmer            |
| **Primitive Parameter**        | Passed by value — original unaffected                                    |
| **Object Parameter**           | Reference copied — same object can be mutated, not reassigned externally |
| **Overloading**                | Same name, different parameter list                                      |
| **Recursion**                  | Method calls itself; needs a base case                                   |
| **Local Variable Scope**       | Limited to the declaring method/block                                    |
| **Instance Variable Scope**    | Accessible across the object's non-static methods                        |
| **Call Stack**                 | New stack frame per method call; popped on return                        |

---

_End of Notes — Methods in Java_
