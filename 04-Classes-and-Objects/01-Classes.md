# Classes in Java

---

## 1. Introduction

### What is a Class?

A **class** is a user-defined blueprint or template in Java that defines the structure (data) and behavior (methods) that its objects will have. It doesn't hold actual data itself — it describes _what kind of data_ and _what actions_ an object created from it will possess.

### Why are Classes Needed?

Without classes, we'd be forced to manage related data using loose, disconnected variables (e.g., separate arrays for student names, ages, and grades) with no natural way to group them or tie behavior to them. Classes let us bundle data and the operations on that data into a single, cohesive unit.

### Importance of Classes in OOP

Classes are the foundation of **Object-Oriented Programming (OOP)**. Every core OOP pillar — encapsulation, inheritance, polymorphism, and abstraction — is built on top of the class construct. Without classes, there are no objects, and without objects, there's no OOP.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define a class.
- Explain the purpose of a class.
- Design classes for real-world entities.
- Declare and create classes in Java.
- Understand the components of a class.

---

## 3. What is a Class?

### Definition

A class is a **user-defined data type** that groups together **data (fields)** and **behavior (methods)** related to a real-world or conceptual entity, serving as a template from which individual **objects** are created.

### Real-life analogy

Think of a class like a **cookie cutter**, and objects like the **cookies** made from it. The cutter defines the _shape_ (structure), but each cookie made from it is a separate, individual cookie — you can decorate one differently from another, but they all share the same basic shape.

Another common analogy: a class is like an **architectural blueprint** for a house. The blueprint itself isn't a house you can live in — it just describes rooms, dimensions, and layout. Actual houses (objects) get built _from_ that blueprint, and each house is a distinct, physical structure.

### Blueprint concept

A class specifies:

- What **attributes** (data) each object will have (but not the actual values).
- What **behaviors** (methods) each object can perform.

### Template concept

Just like a document template defines the structure (headings, sections) but not the actual content, a class defines the _structure_ every object built from it will follow — each object then fills in its own specific values.

### User-defined data type

Just as Java has built-in data types like `int`, `boolean`, and `String`, a class lets _you_ define your own custom data type — e.g., `Student`, `Car`, `Employee` — with fields and methods relevant to your program.

### Examples

```java
class Student {
    String name;
    int age;
}
```

Here, `Student` is a new data type. You can now declare variables of type `Student`, just like you'd declare a variable of type `int`.

---

## 4. Why Do We Need Classes?

### Problems without classes

Imagine tracking 100 students using separate arrays:

```java
String[] names = new String[100];
int[] ages = new int[100];
double[] grades = new double[100];
```

This works, but it's fragile and disorganized:

- No guarantee that `names[5]`, `ages[5]`, and `grades[5]` actually belong to the same student.
- No natural place to put student-related behavior (e.g., "calculate GPA").
- Difficult to extend, maintain, or reuse.

### Code organization

Classes group related data and behavior into one unit, making code easier to read, navigate, and reason about — everything about a `Student` lives inside the `Student` class.

### Reusability

Once a class is written, it can be reused to create as many objects as needed, and even reused across different programs/projects (e.g., importing a `Student` class into a new application).

### Maintainability

Changes to how a `Student` is represented (e.g., adding an `email` field) only need to happen in **one place** — the class definition — rather than scattered across the codebase.

### Encapsulation (Introduction)

Classes allow you to **bundle data with the methods that operate on it**, and control access to that data (e.g., via `private` fields and public methods). This is the foundation of **encapsulation**, a core OOP principle, covered in more depth in later chapters.

### Examples

```java
class Student {
    String name;
    int age;
    double gpa;

    void displayInfo() {
        System.out.println(name + " - " + age + " - " + gpa);
    }
}
```

All student-related data and behavior now live together in one organized unit.

---

## 5. Real-Life Examples of Classes

### Student

- **Class Name:** `Student`
- **Attributes:** `name`, `rollNumber`, `age`, `grade`
- **Behaviors:** `study()`, `attendClass()`, `submitAssignment()`

### Car

- **Class Name:** `Car`
- **Attributes:** `brand`, `model`, `speed`, `fuelLevel`
- **Behaviors:** `start()`, `accelerate()`, `brake()`, `stop()`

### Bank Account

- **Class Name:** `BankAccount`
- **Attributes:** `accountNumber`, `balance`, `accountHolderName`
- **Behaviors:** `deposit()`, `withdraw()`, `checkBalance()`

### Book

- **Class Name:** `Book`
- **Attributes:** `title`, `author`, `ISBN`, `price`
- **Behaviors:** `borrow()`, `returnBook()`, `getDetails()`

### Employee

- **Class Name:** `Employee`
- **Attributes:** `employeeId`, `name`, `salary`, `department`
- **Behaviors:** `work()`, `applyLeave()`, `calculateSalary()`

### Mobile Phone

- **Class Name:** `MobilePhone`
- **Attributes:** `brand`, `batteryLevel`, `storageCapacity`
- **Behaviors:** `call()`, `sendMessage()`, `installApp()`

### Hospital

- **Class Name:** `Hospital`
- **Attributes:** `name`, `address`, `numberOfBeds`, `doctorList`
- **Behaviors:** `admitPatient()`, `dischargePatient()`, `scheduleAppointment()`

---

## 6. Class Declaration

### General Syntax

```java
class ClassName {
    // fields
    // methods
    // constructors
}
```

### Explain each keyword

- `class` — a keyword that tells the compiler you're defining a new class/type.
- `ClassName` — the identifier (name) you're giving to this new type.
- `{ }` — curly braces enclosing the **class body**, where all fields, methods, and constructors are defined.

### Naming conventions

- Class names should use **PascalCase** (first letter of each word capitalized): `Student`, `BankAccount`, `MobilePhone`.
- Class names should be **nouns**, since a class represents a "thing" or entity.
- Avoid abbreviations unless they're widely understood.

### Examples

```java
class Car {
    String brand;
    int speed;
}

class BankAccount {
    String accountNumber;
    double balance;
}
```

---

## 7. Components of a Class

A class may contain:

- **Fields (Instance Variables)** — data belonging to each object.
- **Methods** — behavior/actions the object can perform.
- **Constructors** — special methods used to initialize new objects.
- **Static Members (Brief)** — members that belong to the class itself, shared by all objects, rather than to any one instance.
- **Initialization Blocks (Brief)** — blocks of code that run during object creation, before the constructor body, used for common setup logic.
- **Nested Classes (Brief)** — classes defined inside another class, used to logically group classes that are only used in one place.

_(Methods, constructors, static members, initialization blocks, and nested classes are each covered in detail in later chapters — this chapter focuses on the class itself and its fields.)_

---

## 8. Fields (Instance Variables)

### Definition

**Fields**, also called **instance variables**, are variables declared directly inside a class (but outside any method) that represent the data/state each object of that class will hold.

### Purpose

Fields define _what data_ each object carries. Every object created from the class gets its **own copy** of these fields (except `static` fields, which are shared).

### Declaration

```java
class Student {
    String name;   // field
    int age;       // field
    double gpa;    // field
}
```

### Examples

```java
class Car {
    String brand;
    String model;
    int speed;
}
```

### Memory Representation

Fields are **instance variables** — they don't occupy memory until an object is actually created. When you write `new Student()`, memory is allocated on the heap for that specific object, and it gets its own copies of `name`, `age`, and `gpa`.

```
Student s1 = new Student();     Student s2 = new Student();

Heap:
+----------------------+       +----------------------+
| s1's Student object   |       | s2's Student object   |
|  name = null          |       |  name = null          |
|  age  = 0              |       |  age  = 0              |
|  gpa  = 0.0            |       |  gpa  = 0.0            |
+----------------------+       +----------------------+
```

Each object maintains its own independent set of field values.

---

## 9. Methods (Introduction)

### Definition

A **method** is a block of code inside a class that defines a specific behavior or action that objects of that class can perform.

### Purpose

Methods let objects _do_ something — process data, return information, or change their own state (fields).

### Examples

```java
class Student {
    String name;
    int age;

    void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

_(Methods — including parameters, return types, and overloading — are covered in full detail in a later chapter.)_

---

## 10. Constructors (Introduction)

### Definition

A **constructor** is a special block of code, resembling a method, that is automatically called when an object is created using `new`. It is used to **initialize** the object's fields.

### Purpose

Constructors ensure that an object starts its life in a valid, properly initialized state, rather than relying on default values.

### Examples

```java
class Student {
    String name;
    int age;

    Student(String n, int a) {
        name = n;
        age = a;
    }
}

Student s1 = new Student("Aditi", 21);
```

_(Constructors — including default constructors, overloading, and `this` — are covered in full detail in a later chapter.)_

---

## 11. Access Modifiers (Introduction)

Access modifiers control **who can access** a class, field, or method.

- **`public`** — accessible from anywhere.
- **`private`** — accessible only within the same class.
- **`protected`** — accessible within the same package and by subclasses.
- **default** (no modifier) — accessible only within the same package.

```java
class Student {
    private String name;   // only accessible within Student
    public int age;         // accessible from anywhere
}
```

_(A full treatment of access modifiers, along with encapsulation, is provided in a later chapter — this is only a brief introduction.)_

---

## 12. Naming Conventions

### Class Names

- **PascalCase**, nouns: `Student`, `Employee`, `BankAccount`.

### Field Names

- **camelCase**, descriptive nouns: `firstName`, `accountBalance`, `phoneNumber`.

### Method Names

- **camelCase**, usually verbs or verb phrases describing an action: `calculateSalary()`, `displayInfo()`, `getBalance()`.

### Examples

```java
class BankAccount {
    private double accountBalance;   // field: camelCase

    public double getAccountBalance() {  // method: camelCase, verb-based
        return accountBalance;
    }
}
```

### Java Coding Standards

- One public class per file, and the file name must match the public class name (e.g., `Student.java` for `class Student`).
- Use meaningful, descriptive names — avoid single letters except for loop counters.
- Be consistent with casing across the whole codebase.

---

## 13. Class Diagram (UML Introduction)

UML (Unified Modeling Language) class diagrams provide a simple visual way to represent a class's structure.

### Simple UML Diagram

```
+----------------------+
|      Student         |
+----------------------+
| - name : String      |
| - age : int          |
+----------------------+
| + display() : void   |
+----------------------+
```

### Explain each part

- **Top compartment** — the class name (`Student`).
- **Middle compartment** — the fields, in the format `visibility name : type`.
  - `-` means `private`
  - `+` means `public`
  - `#` means `protected`
- **Bottom compartment** — the methods, in the format `visibility methodName(parameters) : returnType`.

This diagram tells us: `Student` has two private fields (`name` of type `String`, `age` of type `int`) and one public method (`display()`, which returns nothing — `void`).

---

## 14. Class Memory Representation

Understanding what happens to your class _before_ it ever creates an object:

```
Source Code (.java file)
        |
        v
   Compilation (javac)
        |
        v
   Class File (.class) — bytecode
        |
        v
   Loaded into JVM (by the Class Loader, into the Method Area/Metaspace)
```

### Simple explanation

1. You write your class definition in a `.java` file.
2. The Java **compiler** (`javac`) translates it into **bytecode**, stored in a `.class` file.
3. When the program runs, the JVM's **Class Loader** loads this `.class` file into the **Method Area (Metaspace)** — this is where the class's structure (fields, methods, static data) lives.
4. **Only when you use `new ClassName()`** does the JVM allocate memory on the **heap** for an actual object based on that loaded class definition.

This is an important distinction: **the class itself is loaded once**, but you can create **many objects** from it, each with its own heap memory.

---

## 15. Characteristics of a Good Class

### Single Responsibility

A well-designed class should represent **one clear concept** and have one primary reason to change. A `Student` class should manage student-related data — not also handle, say, database connections or file I/O.

### Meaningful Name

The class name should clearly communicate what it represents. `Student` is meaningful; `Data1` or `Manager` (without context) is not.

### High Cohesion

All the fields and methods inside a class should be closely related and work together toward a single purpose. A `Student` class should only contain student-related fields/methods.

### Low Coupling (Basic)

A class should depend on other classes as little as possible. Minimizing dependencies between classes makes the system easier to modify and test, since a change in one class is less likely to break others.

### Readable Design

Fields and methods should be logically grouped and named clearly, so anyone reading the class can quickly understand its purpose and how to use it.

---

## 16. Creating Multiple Classes

A Java program is typically composed of multiple, cooperating classes rather than one giant class.

### Student

```java
class Student {
    String name;
    int rollNumber;
}
```

### Teacher

```java
class Teacher {
    String name;
    String subject;
}
```

### Course

```java
class Course {
    String courseName;
    int credits;
}
```

Each class handles a distinct concept, and together they can be combined (e.g., a `Course` might later reference a `Teacher` and a list of `Student` objects) to build a complete application.

---

## 17. Packages and Classes (Brief)

### Why packages exist

As programs grow to include many classes, **packages** provide a way to group related classes together (similar to folders), avoiding naming conflicts and improving organization.

### Relationship with classes

Every class in Java belongs to a package — either an explicitly declared one (`package com.school.model;`) or the unnamed **default package** if none is specified. Related classes (like `Student`, `Teacher`, `Course`) are often grouped into the same package.

```java
package com.school.model;

class Student {
    String name;
}
```

_(Packages are covered in full detail in a later chapter — this is only a brief introduction.)_

---

## 18. Common Mistakes

- **Using incorrect naming conventions** — e.g., naming a class `student` (lowercase) instead of `Student`.
- **Creating overly large classes** — cramming unrelated responsibilities into a single class ("God classes"), violating single responsibility.
- **Confusing class with object** — thinking the class itself holds data, rather than understanding it's only a template; the actual data lives in objects.
- **Forgetting braces** — mismatched or missing `{ }` around the class body causes compilation errors.
- **Declaring everything public** — exposing all fields as `public` breaks encapsulation and makes the class harder to maintain safely.

---

## 19. Best Practices

- Use **PascalCase** for class names.
- Keep classes **focused** on a single responsibility.
- Use **descriptive names** for classes, fields, and methods.
- **Group related members** together (fields, then constructors, then methods) for readability.
- Follow standard **Java coding conventions** consistently across your project.

---

## 20. Interview Corner

1. **What is a class?**
   A class is a user-defined blueprint/template that defines the fields and methods common to all objects of a particular type.

2. **Why is a class called a blueprint?**
   Because it defines the structure and behavior an object _will_ have, without itself being a usable object — similar to how an architectural blueprint defines a house's layout without being a livable house.

3. **Is a class an object?**
   No. A class is a template; an object is a concrete instance created from that template using `new`.

4. **Can a class exist without objects?**
   Yes. A class can be defined and compiled without ever creating an object from it — it simply won't do anything on its own until instantiated (though static members can be used without instantiation).

5. **Can we create multiple objects from one class?**
   Yes. A single class can be used to create any number of independent objects, each with its own field values.

6. **Is a class a data type?**
   Yes. A class defines a new, user-defined (reference) data type, just as Java has built-in types like `int` and `boolean`.

---

## 21. MCQs

1. A class in Java is best described as:
   a) An object b) A blueprint for objects c) A method d) A variable

2. Which keyword is used to define a class?
   a) `object` b) `new` c) `class` d) `define`

3. Fields in a class are also known as:
   a) Local variables b) Instance variables c) Static methods d) Constructors

4. Which naming convention is used for class names?
   a) camelCase b) snake_case c) PascalCase d) UPPERCASE

5. When are instance variables allocated memory?
   a) At class declaration b) At compile time c) When an object is created d) Never

6. Which of these is NOT typically a class component?
   a) Fields b) Methods c) Constructors d) Compilers

7. A constructor is used to:
   a) Delete objects b) Initialize objects c) Compile classes d) Import packages

8. Which access modifier restricts access to only within the same class?
   a) public b) protected c) private d) default

9. The `.class` file is generated by:
   a) The JVM b) The compiler (javac) c) The programmer manually d) The IDE only

10. Where is class-level (loaded) data stored in JVM memory?
    a) Stack b) Heap c) Method Area / Metaspace d) Cache

11. Which of the following is a valid class name following convention?
    a) `student` b) `Student` c) `STUDENT_class` d) `1Student`

12. A class with too many unrelated responsibilities violates:
    a) Encapsulation b) Single Responsibility c) Inheritance d) Polymorphism

13. In UML, a `-` before a field name indicates:
    a) Public b) Protected c) Private d) Static

14. High cohesion means:
    a) A class does many unrelated things b) A class's members are closely related and focused c) A class has no methods d) A class has many dependencies

15. Which of these is a real-world example best modeled as a class?
    a) `5` b) `true` c) `Car` d) `+`

16. What must match the public class name in a Java file?
    a) The package name b) The file name c) The method name d) The constructor name

17. A class without any object created:
    a) Cannot be compiled b) Causes a runtime error c) Can still be compiled and exist d) Is illegal in Java

18. Which of the following best describes low coupling?
    a) Classes heavily depend on each other b) Classes are minimally dependent on each other c) Classes have no fields d) Classes cannot have methods

19. Packages are primarily used to:
    a) Speed up compilation b) Group related classes and avoid naming conflicts c) Replace classes d) Create objects

20. What does `new ClassName()` primarily do?
    a) Loads the class file b) Allocates heap memory and creates an object c) Deletes the class d) Declares a class

---

## 22. University Questions

### Short Questions

1. Define a class with a suitable example.
2. What is the difference between a class and an object?
3. List the components of a class.
4. What are instance variables?
5. Write the general syntax for declaring a class in Java.

### Long Questions

1. Explain the concept of a class with a real-life analogy and a Java example.
2. Discuss why classes are essential in object-oriented programming, highlighting reusability and maintainability.
3. Design a `BankAccount` class with appropriate fields and methods, and explain each component.
4. Explain the memory representation of a class from source code to object creation.
5. Describe the characteristics of a well-designed class with examples.

---

## 23. Viva Questions

1. What is a class in Java?
2. How is a class different from an object?
3. What are the main components of a class?
4. Why do we use access modifiers in a class?
5. What is the purpose of a constructor?
6. Can a class have no fields or methods? Is that valid?
7. What naming convention is used for Java class names?
8. What does `new` do when creating an object?
9. What is meant by "a class is a blueprint"?
10. Why is single responsibility important in class design?

---

## 24. Programming Exercises

1. Design a `Student` class with fields for name, roll number, and grade (no object creation yet).
2. Design a `Car` class with fields for brand, model, and speed.
3. Design an `Employee` class with fields for employee ID, name, and salary.
4. Design a `BankAccount` class with fields for account number, holder name, and balance.
5. Design a `Book` class with fields for title, author, and ISBN.

_(These exercises focus purely on class design — declaring appropriate fields and method signatures. Object creation and usage will be covered in the next chapter.)_

---

## 25. Summary

A class in Java is a user-defined blueprint that groups related data (fields) and behavior (methods) into a single, reusable unit, forming the foundation of object-oriented programming. Rather than holding data itself, a class defines the _structure_ that its objects will follow — each object created from the class gets its own independent set of field values. Classes solve real problems in code organization, reusability, and maintainability that arise when data and behavior are scattered across disconnected variables. A well-designed class follows principles like single responsibility, high cohesion, and low coupling, and adheres to Java's naming and coding conventions. Understanding classes — their declaration, components, and memory representation — is the essential first step before learning how objects are created and used.

---

## 26. Key Takeaways

- A class is a blueprint for creating objects.
- A class defines the properties and behaviors of objects.
- Classes help organize code into reusable components.
- A class itself does not occupy memory for object data until objects are created.
- Good class design improves maintainability and readability.

---

## 27. Cheat Sheet

| Concept            | Quick Reference                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------- |
| **Definition**     | A user-defined blueprint/template describing fields and methods for objects                          |
| **Syntax**         | `class ClassName { fields; methods; constructors; }`                                                 |
| **Naming Rules**   | Class: PascalCase noun · Field/Method: camelCase                                                     |
| **Components**     | Fields, Methods, Constructors, Static Members, Initialization Blocks, Nested Classes                 |
| **UML Diagram**    | Class name (top) · Fields (middle, `- name : Type`) · Methods (bottom, `+ method() : ReturnType`)    |
| **Best Practices** | Single responsibility · high cohesion · low coupling · descriptive names · minimal `public` exposure |

---

_End of Notes — Classes in Java_
