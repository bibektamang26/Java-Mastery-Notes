# 2.7 Variables and Constants

## 1. Introduction

Every program needs a way to store, label, and work with data as it runs. In Java, this is done through **variables** and **constants**.

- A **variable** is a named piece of memory whose value can change during program execution.
- A **constant** is a named piece of memory whose value is fixed once assigned and cannot be changed afterward.

Variables allow programs to be dynamic and flexible — the same code can work with different inputs each time it runs. Constants, on the other hand, protect certain values from accidental modification, making programs safer and more predictable.

Both concepts are fundamental to programming — without them, every value would have to be hardcoded directly into expressions, making programs rigid, repetitive, and error-prone. Understanding how to declare, initialize, name, and scope variables — and when to use constants instead — is one of the very first and most essential skills in learning Java.

---

## 2. Learning Objectives

After completing this chapter, you should be able to:

- Define variables and constants.
- Declare and initialize variables.
- Understand different types of variables.
- Explain variable scope.
- Create constants using the `final` keyword.
- Follow Java naming conventions.

---

# VARIABLES

## 3. What is a Variable?

**Definition:** A variable is a named location in memory that holds a value which can be changed during the execution of a program. Every variable in Java has a **type**, a **name**, and a **value**.

**Real-life analogy:** Think of a variable like a labeled box or a storage locker. The label (variable name) stays the same, but what you put inside the box (the value) can be swapped out at any time. For example, a locker labeled "wallet" might hold ₹500 today and ₹1200 tomorrow — the label doesn't change, but the contents do.

**Memory representation:** When a variable is declared, the Java Virtual Machine (JVM) reserves a block of memory sized according to its data type, and associates that memory location with the variable's name so it can be accessed and modified using that name.

**Example:**

```java
int age = 20;
age = 21;  // value changed, name stays the same
```

---

## 4. Why Do We Need Variables?

Without variables, every value used in a program would need to be written directly (hardcoded) wherever it's needed, making the code repetitive, hard to update, and difficult to understand.

**Without variables:**

```java
System.out.println(10 + 20);
```

This works, but the numbers `10` and `20` have no meaning attached to them, and if they need to be reused or changed, every occurrence must be edited manually.

**With variables:**

```java
int a = 10;
int b = 20;

System.out.println(a + b);
```

Now `a` and `b` clearly represent meaningful quantities, can be reused throughout the program, and can be updated in a single place.

**Advantages:**

- **Reusability** – The same variable can be used multiple times across a program without rewriting its value.
- **Readability** – Meaningful variable names (like `totalMarks` instead of `95`) make code easier to understand.
- **Maintainability** – Changing a value in one place (the variable's declaration) automatically reflects everywhere it is used.
- **Flexibility** – Variables allow programs to work with dynamic, user-provided, or calculated data instead of fixed values.

---

## 5. Memory Representation of Variables

When variables are declared and assigned values, the JVM stores them in memory (typically stack memory for local primitive variables), each in its own labeled slot.

```
RAM

+-----------+
| age = 20  |
+-----------+

+------------------+
| salary = 50000   |
+------------------+

+--------------+
| grade = 'A'  |
+--------------+
```

**Explanation:** Each variable occupies a distinct memory address. The variable name (`age`, `salary`, `grade`) is simply a human-readable label the compiler uses internally to refer to that memory address — the computer itself works with addresses, not names. The amount of memory reserved for each box depends on the variable's data type (e.g., `int` takes 4 bytes, `char` takes 2 bytes).

---

## 6. Variable Declaration

**Definition:** Declaration is the process of telling the compiler a variable's **name** and **data type**, without necessarily giving it a value yet.

**Syntax:**

```java
dataType variableName;
```

**Examples:**

```java
int age;
double salary;
char grade;
```

At this stage, memory is reserved for the variable, but for local variables, no usable value exists yet — the variable must be initialized before it can be read.

---

## 7. Variable Initialization

**Definition:** Initialization is the process of assigning an initial value to a variable at the time it is declared.

**Syntax:**

```java
dataType variableName = value;
```

**Examples:**

```java
int age = 20;
double salary = 45000.50;
```

Declaration and initialization can happen together (as shown above) or separately — declare first, then assign a value later in the code.

---

## 8. Declaration vs Initialization

| Aspect                  | Declaration                                | Initialization                                                 |
| ----------------------- | ------------------------------------------ | -------------------------------------------------------------- |
| Meaning                 | Specifies the variable's type and name     | Assigns an actual value to the variable                        |
| Memory                  | Memory is reserved                         | Memory slot is filled with a value                             |
| Syntax                  | `int age;`                                 | `age = 20;`                                                    |
| Can happen alone?       | Yes, without a value                       | No — initialization requires prior or simultaneous declaration |
| Usage before completion | Cannot use an uninitialized local variable | Variable becomes usable after initialization                   |

**Examples:**

```java
int marks;      // Declaration only
marks = 90;     // Initialization (separate statement)

int score = 85; // Declaration + Initialization together
```

---

## 9. Variable Assignment

**Explanation:** Assignment refers to giving a **new value** to a variable that has already been declared (and usually already initialized). Unlike initialization, assignment can happen any number of times during a program's execution.

```java
int marks = 50;

marks = 80;

marks = 95;
```

**How values change:** Each assignment statement overwrites the previous value stored in the variable's memory location. The variable name and its memory address remain the same throughout — only the contents change.

**Memory diagram:**

```
Step 1: marks = 50   →  [ marks | 50 ]
Step 2: marks = 80   →  [ marks | 80 ]   (50 is overwritten)
Step 3: marks = 95   →  [ marks | 95 ]   (80 is overwritten)
```

---

## 10. Rules for Naming Variables

Java enforces strict rules for what constitutes a valid variable name (identifier):

✔ **Must start with a letter** (`a`-`z`, `A`-`Z`) — e.g., `name`, `Age`

✔ **Can start with an underscore** `_` — e.g., `_count`

✔ **Can start with a dollar sign** `$` — e.g., `$price` (though rarely used in practice)

✔ **Cannot start with a digit** — `1name` is invalid; `name1` is valid

✔ **Cannot contain spaces** — `student name` is invalid; use `studentName` instead

✔ **Cannot contain special characters** like `@`, `#`, `%`, `-`, etc. — only letters, digits, `_`, and `$` are allowed

✔ **Cannot use reserved keywords** — words like `int`, `class`, `public`, `static`, `final` cannot be used as variable names

✔ **Case-sensitive** — `age`, `Age`, and `AGE` are three completely different variables in Java

---

## 11. Variable Naming Conventions

Java's official convention for naming variables is **camelCase** — the first word is lowercase, and each subsequent word starts with an uppercase letter, with no underscores or spaces between words.

**Good (follows convention):**

```java
studentName
totalMarks
monthlySalary
```

**Bad (violates convention, even if technically valid Java):**

```java
student_name   // uses snake_case instead of camelCase
StudentName    // starts with uppercase — looks like a class name
TOTALMARKS     // all uppercase — reserved style for constants, not variables
```

Following naming conventions doesn't change how the code runs, but it makes code far more readable and consistent with standard Java style, which matters greatly in team projects and academic evaluation.

---

## 12. Types of Variables

Java classifies variables into three types based on **where they are declared** and **how long they exist** in memory.

```
Variables

├── Local Variable

├── Instance Variable

└── Static Variable
```

---

## 13. Local Variables

**Definition:** A local variable is declared **inside a method, constructor, or block**, and is only accessible within that method, constructor, or block.

**Characteristics:**

- Must be initialized before use — Java does not provide default values for local variables.
- Not accessible outside the method/block in which they are declared.
- Stored in **stack memory**.

**Lifetime:** Exists only while the method or block in which it's declared is executing; destroyed once that method/block finishes.

**Scope:** Limited strictly to the enclosing method or block (including nested blocks within it).

**Memory:** Allocated on the stack when the method is called, and deallocated when the method returns.

**Example:**

```java
void calculateArea() {
    int length = 10;   // local variable
    int width = 5;     // local variable
    int area = length * width;
    System.out.println(area);
}
```

**Diagram:**

```
calculateArea() called
  → Stack Frame Created
      [ length = 10 ]
      [ width  = 5  ]
      [ area   = 50 ]
  → calculateArea() returns
      Stack Frame Destroyed (all local variables gone)
```

**Interview Notes:** A very common interview question is "why don't local variables have default values?" — the answer is that Java forces explicit initialization for local variables to avoid the JVM guessing at unintended values, catching potential bugs at compile time instead of at runtime.

---

## 14. Instance Variables

**Definition:** An instance variable is declared **inside a class but outside any method**, and belongs to individual **objects** (instances) of that class — each object gets its own separate copy.

**Characteristics:**

- Declared at the class level, without the `static` keyword.
- Each object of the class has its own independent copy of the instance variable.
- Automatically assigned a **default value** if not explicitly initialized.

**Lifetime:** Created when an object is instantiated (using `new`), and destroyed when that object becomes eligible for garbage collection.

**Default Values:** Instance variables of numeric types default to `0`/`0.0`, `boolean` defaults to `false`, `char` defaults to `'\u0000'`, and object references default to `null`.

**Memory:** Stored as part of the object itself, on the **heap**.

**Example:**

```java
class Student {
    String name;   // instance variable
    int age;       // instance variable

    void display() {
        System.out.println(name + " is " + age + " years old.");
    }
}
```

**Diagram:**

```
Student s1 = new Student();   →  Heap: [ name=null, age=0 ]  (s1's own copy)
Student s2 = new Student();   →  Heap: [ name=null, age=0 ]  (s2's own copy, independent of s1)
```

**Interview Notes:** Instance variables are tied to the **object**, not the class — changing `s1.age` has no effect on `s2.age`, since each object maintains its own separate memory for instance variables.

---

## 15. Static Variables

**Definition:** A static variable is declared inside a class using the `static` keyword, and belongs to the **class itself** rather than to any individual object — it is shared across all instances of the class.

**Characteristics:**

- Declared with the `static` keyword at the class level.
- Only **one copy** exists in memory, regardless of how many objects are created.
- Can be accessed directly using the class name, without creating an object.

**Memory:** Stored in a special area of memory associated with the class (method area/metaspace), created once when the class is loaded.

**Shared Variables:** Because there is only one copy, changes made through one object are visible to all other objects and to the class itself.

**Example:**

```java
class Counter {
    static int count = 0;  // static variable, shared by all objects

    Counter() {
        count++;
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();
System.out.println(Counter.count);  // Output: 3
```

**Diagram:**

```
Class Area:  [ Counter.count = 3 ]   ← shared by c1, c2, and c3
```

**Interview Notes:** A frequent viva question is "why is `count` accurate across all three objects?" — because `static` variables are not duplicated per object; every object modifies the **same single memory location**.

---

## 16. Scope of Variables

**Scope** refers to the region of a program within which a variable is accessible and can be used.

**Block Scope:** A variable declared inside a pair of curly braces `{ }` (such as inside an `if` or `for` block) is only accessible within that block.

**Method Scope:** A variable declared inside a method (but not inside any inner block) is accessible throughout that entire method.

**Class Scope:** Instance and static variables declared at the class level are accessible throughout the class (and, depending on access modifiers, potentially outside it too).

**Examples:**

```java
class ScopeDemo {
    int classVar = 100;  // class scope (instance variable)

    void method() {
        int methodVar = 50;  // method scope

        if (true) {
            int blockVar = 25;  // block scope
            System.out.println(blockVar);  // accessible here
        }
        // System.out.println(blockVar);  // Error: blockVar out of scope here
    }
}
```

**Nested blocks:** Variables declared in an outer block are visible inside inner (nested) blocks, but variables declared inside an inner block are **not** visible outside it once that block ends.

---

## 17. Lifetime of Variables

| Variable Type | Created When                  | Destroyed When                   |
| ------------- | ----------------------------- | -------------------------------- |
| Local         | Method/block execution begins | Method/block execution ends      |
| Instance      | Object is created (`new`)     | Object is garbage collected      |
| Static        | Class is loaded by the JVM    | Class is unloaded / program ends |

---

# CONSTANTS

## 18. What is a Constant?

**Definition:** A constant is a variable whose value is set once and **cannot be changed** afterward during program execution. In Java, constants are typically created by combining the `final` keyword with a variable declaration.

**Need:** Certain values in a program are meant to remain fixed throughout execution — such as mathematical constants (π), configuration limits, or fixed identifiers. Representing these as constants prevents accidental modification and clearly signals to anyone reading the code that the value should never change.

**Examples:**

```java
final double PI = 3.14159;
final int MAX_USERS = 100;
```

---

## 19. `final` Keyword

**Definition:** The `final` keyword in Java is used to declare a variable whose value, once assigned, cannot be reassigned.

**Syntax:**

```java
final dataType CONSTANT_NAME = value;
```

**Examples:**

```java
final double PI = 3.14159;
```

**Cannot modify:** Once a `final` variable has been assigned a value, any attempt to assign it a new value will result in a compile-time error.

**Compiler Error example:**

```java
final int MAX_SIZE = 50;
MAX_SIZE = 100;  // Compile Error: cannot assign a value to final variable MAX_SIZE
```

---

## 20. Why Use Constants?

**Advantages:**

- **Prevents accidental changes** to values that should remain fixed throughout the program.
- **Improves readability** — a name like `MAX_USERS` is far clearer than a bare number like `100` scattered throughout code.
- **Centralizes updates** — if a fixed value needs to change (e.g., a tax rate), it only needs to be updated in one place.
- **Communicates intent** — using `final` tells other programmers this value is intentionally fixed, not an oversight.

**Examples:**

```java
final double TAX_RATE = 0.18;
final int MAX_ATTEMPTS = 3;
```

**Real-life uses:** Mathematical constants (`PI`, `E`), configuration values (maximum login attempts, default port numbers), and fixed business rules (tax rates, interest rates) are all commonly represented as constants.

---

## 21. Variables vs Constants

| Aspect                | Variable                                    | Constant                                                                                  |
| --------------------- | ------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Value                 | Can change during execution                 | Fixed once assigned                                                                       |
| Keyword               | No special keyword required                 | Declared using `final`                                                                    |
| Naming convention     | camelCase                                   | UPPER_CASE with underscores                                                               |
| Reassignment          | Allowed any number of times                 | Not allowed — causes compile error                                                        |
| Purpose               | Store data that changes over time           | Store data that must remain fixed                                                         |
| Memory                | Value in memory can be overwritten          | Value in memory is set once and locked                                                    |
| Examples              | `int score = 50;`                           | `final int MAX_SCORE = 100;`                                                              |
| Initialization timing | Can be initialized later, after declaration | Must be initialized either at declaration or in a constructor (for instance-level finals) |
| Risk if misused       | May be accidentally overwritten             | Attempting to overwrite causes a clear compiler error                                     |
| Typical use case      | Counters, user input, calculated results    | Mathematical constants, configuration limits, fixed labels                                |

---

## 22. Java Naming Conventions for Constants

Constants in Java are conventionally written in **UPPER_CASE**, with underscores separating words.

**Examples:**

```java
MAX_SIZE
PI
DEFAULT_PORT
```

This convention makes it immediately clear, just by looking at a name in code, whether it refers to a constant or a regular variable.

---

## 23. Complete Example Program

```java
public class StudentRecord {

    // Constant
    static final double PASSING_PERCENTAGE = 40.0;

    public static void main(String[] args) {
        // Variables
        String studentName = "Aarav";
        int marksObtained = 78;
        int totalMarks = 100;

        double percentage = ((double) marksObtained / totalMarks) * 100;

        System.out.println("Student Name: " + studentName);
        System.out.println("Marks Obtained: " + marksObtained + "/" + totalMarks);
        System.out.println("Percentage: " + percentage + "%");

        if (percentage >= PASSING_PERCENTAGE) {
            System.out.println("Result: PASS");
        } else {
            System.out.println("Result: FAIL");
        }
    }
}
```

This program demonstrates variables (`studentName`, `marksObtained`, `totalMarks`, `percentage`) that change or are calculated during execution, alongside a constant (`PASSING_PERCENTAGE`) that remains fixed and defines a business rule used in the calculation.

---

## 24. Best Practices

✔ **Use meaningful names** — `totalMarks` is far clearer than `tm` or `x`.

✔ **Initialize variables** before using them, especially local variables, to avoid compile errors and undefined behavior.

✔ **Minimize scope** — declare variables in the smallest scope where they're needed, rather than making everything a class-level variable.

✔ **Use constants** for any value that should never change during execution, instead of relying on repeated hardcoded literals ("magic numbers").

✔ **Follow naming conventions** — camelCase for variables, UPPER_CASE for constants — to keep code consistent and readable.

---

## 25. Common Mistakes

- **Using uninitialized variables** — attempting to read a local variable before assigning it a value causes a compile-time error.
- **Changing final variables** — trying to reassign a `final` variable after its initial assignment causes a compiler error.
- **Meaningless names** — using names like `x`, `temp1`, `data2` that don't describe what the variable represents, making code hard to understand.
- **Wrong scope** — declaring variables at a broader scope than necessary (e.g., making everything an instance variable when a local variable would suffice), which increases the risk of unintended side effects.
- **Using a keyword as a variable name** — attempting to name a variable `class`, `int`, `public`, etc., results in a compile-time syntax error since these are reserved words.

---

## 26. Frequently Asked Interview Questions

**Q. Difference between declaration and initialization?**
Declaration specifies a variable's type and name and reserves memory for it; initialization assigns an actual starting value to that already-declared variable.

**Q. Difference between initialization and assignment?**
Initialization is the _first_ value given to a variable, typically at the point of declaration; assignment refers to giving a variable a value at any point, including overwriting a previously initialized value.

**Q. Difference between instance and static variables?**
Instance variables belong to individual objects, with each object holding its own separate copy; static variables belong to the class itself, with a single shared copy used by all objects.

**Q. Difference between local and instance variables?**
Local variables are declared inside methods/blocks and exist only during that method's execution; instance variables are declared at the class level and exist for as long as the object they belong to exists.

**Q. Can local variables have default values?**
No. Unlike instance and static variables, local variables in Java are not automatically initialized and must be explicitly assigned a value before use.

**Q. Why should constants be final?**
Marking a constant `final` ensures the compiler enforces immutability — any accidental attempt to modify its value is caught at compile time rather than causing subtle bugs at runtime.

---

## 27. MCQs

**1. Which keyword is used to declare a constant in Java?**
a) const
b) final
c) static
d) constant
**Answer: b) final**

**2. What is the default value of an uninitialized local variable in Java?**
a) 0
b) null
c) It has no default value and cannot be used
d) Depends on the data type
**Answer: c) It has no default value and cannot be used**

**3. Which naming convention is followed for variables in Java?**
a) UPPER_CASE
b) snake_case
c) camelCase
d) PascalCase
**Answer: c) camelCase**

**4. Which of these is an invalid variable name?**
a) \_count
b) $price
c) 2ndValue
d) totalMarks
**Answer: c) 2ndValue**

**5. How many copies of a static variable exist, regardless of the number of objects created?**
a) One per object
b) Exactly one, shared by all objects
c) Zero until an object is created
d) One per method call
**Answer: b) Exactly one, shared by all objects**

**6. What happens if you try to reassign a final variable after initialization?**
a) It silently updates the value
b) Compile-time error
c) Runtime exception only
d) Nothing happens
**Answer: b) Compile-time error**

**7. Where are instance variables stored in memory?**
a) Stack
b) Heap (as part of the object)
c) Method area only
d) CPU registers
**Answer: b) Heap (as part of the object)**

**8. Which of these is a valid constant declaration?**
a) final int max_size = 100;
b) final int MAX_SIZE = 100;
c) const int MAX_SIZE = 100;
d) static MAX_SIZE = 100;
**Answer: b) final int MAX_SIZE = 100;**

**9. What is the scope of a variable declared inside an if block?**
a) The entire class
b) The entire method
c) Only within that if block
d) The entire program
**Answer: c) Only within that if block**

**10. Which variable type is created when an object is instantiated and destroyed when it's garbage collected?**
a) Static variable
b) Local variable
c) Instance variable
d) Constant
**Answer: c) Instance variable**

---

## 28. Short Questions

1. What is a variable in Java?
2. What is the difference between declaration and initialization?
3. List any four rules for naming variables in Java.
4. What is a local variable? Give one example.
5. What is a static variable, and how is it different from an instance variable?
6. Define a constant with a suitable Java example.
7. Why is the `final` keyword used with constants?
8. What naming convention is used for constants in Java?
9. Can a local variable be used without initialization? Why or why not?
10. What is the scope of a variable declared inside a method?

---

## 29. Long Questions

1. Explain the different types of variables in Java with suitable examples and diagrams.
2. Differentiate between variables and constants with at least ten comparison points.
3. Explain variable scope in Java with examples of block scope, method scope, and class scope.
4. Describe the lifetime of local, instance, and static variables with a comparison table.
5. Write a Java program that demonstrates the use of local, instance, and static variables together, and explain the output.

---

## 30. Viva Questions

1. What is the difference between a variable and a constant?
2. Why don't local variables get default values in Java?
3. What memory area are local variables stored in?
4. Can you change the value of a `final` variable inside a constructor?
5. What is the naming convention difference between variables and constants?
6. What happens if two objects modify the same static variable?
7. What is meant by "scope" of a variable?
8. Give an example of a real-world use case for a constant.
9. Is `$` allowed at the start of a Java variable name?
10. Why is Java case-sensitive when it comes to variable names?

---

## 31. Programming Exercises

**Easy**

1. Declare and initialize variables for a person's name, age, and city, then print them.
2. Write a program that declares a constant for the value of PI and calculates the area of a circle.

**Medium**

3. Write a program using a static variable to count how many objects of a class have been created.
4. Write a program demonstrating the difference between local and instance variables using two methods in the same class.

**Hard**

5. Write a program that uses `final` constants to define tax rate and discount rate, and calculates the final price of a product after applying both.

---

## 32. Challenge Programs

**Student Record:** Create a `Student` class with instance variables for name, roll number, and marks, plus a static variable that tracks the total number of students created.

**Bank Account:** Create a `BankAccount` class with instance variables for account holder name and balance, and a `final` constant for the minimum balance requirement, enforcing it during withdrawals.

**Employee Details:** Create an `Employee` class with instance variables for name and salary, and a static variable for company name shared across all employees.

**Circle Area using Constant PI:** Write a program that declares `PI` as a `final` constant and uses it to calculate both the area and circumference of a circle for a user-provided radius.

---

## 33. Summary

- A **variable** is a named memory location whose value can change during program execution, while a **constant** holds a value that stays fixed once assigned.
- Variables must be **declared** (given a type and name) before they can be **initialized** (given a value) and later **assigned** new values.
- Java identifiers must follow strict naming rules, and by convention, variables use **camelCase** while constants use **UPPER_CASE**.
- Java has three types of variables: **local** (inside methods/blocks), **instance** (per-object, class-level), and **static** (shared across all objects of a class).
- **Scope** determines where a variable can be accessed, while **lifetime** determines how long it exists in memory.
- Constants are created using the **`final`** keyword, which prevents reassignment and enforces immutability at compile time.

---

## 34. Key Takeaways

- Variables store data that can change; constants store data that must not change.
- Local variables require explicit initialization — Java gives them no default value.
- Instance variables belong to objects; static variables belong to the class and are shared.
- Naming conventions (camelCase for variables, UPPER_CASE for constants) are a Java standard, not just a stylistic choice.
- The `final` keyword is what makes a variable behave as a true constant in Java.
- Scope and lifetime are related but distinct concepts — scope is about _where_ a variable is accessible, lifetime is about _how long_ it exists in memory.

---

## 35. Quick Revision Sheet

- **Variable** → Stores changing value
- **Constant** → Stores fixed value
- **Declaration** → Creates variable
- **Initialization** → Gives value
- **Assignment** → Changes value
- **Local Variable** → Inside method
- **Instance Variable** → Inside class
- **Static Variable** → Shared among objects
- **Scope** → Accessibility
- **Lifetime** → Duration in memory
