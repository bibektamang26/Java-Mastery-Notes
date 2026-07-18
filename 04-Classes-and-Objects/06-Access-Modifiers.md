# Access Modifiers in Java

---

## 1. Introduction

### What are Access Modifiers?

**Access modifiers** are keywords in Java (`public`, `private`, `protected`, and the unwritten "default") that control **who can access** a class, field, method, or constructor — from where in the codebase it can be seen and used.

### Why are they needed?

Not every part of a class should be freely accessible to every other part of a program. Access modifiers let a class expose only what it wants to expose, while hiding internal details it doesn't want other code depending on or tampering with directly.

### Importance in Object-Oriented Programming

Access modifiers are the primary mechanism behind **encapsulation** — one of the four pillars of OOP. They allow a class to protect its internal state, expose a controlled interface, and enforce rules about how its data can be read or modified, all of which are essential to writing robust, maintainable object-oriented code.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define access modifiers and explain their purpose.
- Differentiate between `public`, `private`, `protected`, and default access.
- Apply the correct access modifier to classes, fields, methods, and constructors.
- Understand how access modifiers interact with packages and subclasses.
- Use `private` fields with public getters/setters to implement basic encapsulation.

---

## 3. What are Access Modifiers?

### Definition

Access modifiers are special keywords applied to a class, field, method, or constructor that determine the **scope of visibility** — i.e., from which parts of a program that member can be accessed.

### Purpose

Their purpose is to enforce **boundaries** around a class's internals: deciding what's exposed as a public "interface" for other code to use, and what's kept hidden as an internal implementation detail that only the class itself should touch.

### Access Control

Access control is the broader concept of restricting who can read or modify data and invoke behavior. Access modifiers are Java's concrete tool for implementing access control at the language level.

```java
class BankAccount {
    private double balance;     // hidden -- only accessible within this class

    public double getBalance() {  // exposed -- accessible from outside
        return balance;
    }
}
```

---

## 4. Why Do We Need Access Modifiers?

### Data Security

By restricting direct access to sensitive fields (like a bank balance), access modifiers prevent external code from reading or modifying that data in uncontrolled, potentially invalid ways.

### Encapsulation

Access modifiers are what make encapsulation possible — bundling data with the methods that operate on it, while hiding the data itself behind a controlled, well-defined interface.

### Controlled Access

Instead of allowing any code anywhere to freely change a field's value, access modifiers let a class dictate exactly which conditions and validations must be satisfied before a change is allowed (typically enforced inside a public method).

```java
class BankAccount {
    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {          // controlled access -- validation enforced
            balance += amount;
        }
    }
}
```

### Maintainability

Because external code depends only on a class's public interface (not its private internals), the class's internal implementation can be changed freely without breaking other code — as long as the public interface stays the same.

---

## 5. Types of Access Modifiers

Java provides four levels of access control:

- **`public`** — accessible from anywhere.
- **`private`** — accessible only within the declaring class.
- **`protected`** — accessible within the same package, and by subclasses (even in different packages).
- **default (package-private)** — no keyword written; accessible only within the same package.

### Overview Table

| Modifier    | Keyword Used     | Visibility Level          |
| ----------- | ---------------- | ------------------------- |
| `public`    | `public`         | Everywhere                |
| `protected` | `protected`      | Same package + subclasses |
| default     | _(none written)_ | Same package only         |
| `private`   | `private`        | Same class only           |

---

## 6. `public` Access Modifier

### Definition

A member (or class) marked `public` is accessible from **any other class**, in any package, anywhere in the program.

### Characteristics

- Widest possible visibility — no restrictions.
- Commonly used for methods that form a class's intended external interface (e.g., getters, setters, action methods meant to be called from outside).
- Overuse can weaken encapsulation, since anything marked `public` can be freely accessed and depended upon by other code.

### Examples

```java
public class Student {
    public String name;

    public void displayInfo() {
        System.out.println("Name: " + name);
    }
}

// From any other class, in any package:
Student s1 = new Student();
s1.name = "Aditi";          // accessible
s1.displayInfo();            // accessible
```

---

## 7. `private` Access Modifier

### Definition

A member marked `private` is accessible **only within the class in which it is declared** — no other class, not even a subclass or a class in the same package, can access it directly.

### Characteristics

- Most restrictive access level.
- Commonly used for internal fields that should not be directly exposed (supporting encapsulation).
- Cannot be applied to top-level classes (only to members: fields, methods, constructors) — though it can be applied to nested classes.

### Examples

```java
class BankAccount {
    private double balance;   // only BankAccount itself can access 'balance' directly

    private void logTransaction(String message) {  // internal helper, hidden from outside
        System.out.println("LOG: " + message);
    }

    public void deposit(double amount) {
        balance += amount;          // OK -- accessed from within the same class
        logTransaction("Deposited " + amount);  // OK -- called from within the same class
    }
}

BankAccount acc = new BankAccount();
// acc.balance = 1000;         // COMPILE ERROR -- 'balance' is private
// acc.logTransaction("x");    // COMPILE ERROR -- 'logTransaction' is private
```

---

## 8. `protected` Access Modifier

### Definition

A member marked `protected` is accessible within the **same package**, and additionally by **subclasses**, even if those subclasses are in a **different package**.

### Characteristics

- Wider than default, narrower than `public`.
- Primarily designed to support **inheritance** — letting subclasses access and build upon inherited fields/methods, without opening that access up to completely unrelated classes.
- Within the same package, `protected` behaves just like default access; the distinction matters specifically for subclasses in _other_ packages.

### Examples

```java
class Employee {
    protected double salary;   // accessible to subclasses, even in other packages

    protected void calculateBonus() {
        System.out.println("Calculating bonus...");
    }
}

class Manager extends Employee {   // subclass, possibly in a different package
    void giveRaise() {
        salary += 5000;          // OK -- 'salary' is protected, accessible via inheritance
        calculateBonus();          // OK -- inherited protected method
    }
}
```

_(Inheritance and `extends` are explored in full depth in a later, dedicated chapter — this example previews how `protected` supports it.)_

---

## 9. `default` Access Modifier

### Definition

**Default access** (also called **package-private**) applies when **no access modifier keyword is written at all**. A member with default access is accessible only from within **classes in the same package**.

### Characteristics

- Not an explicit keyword — simply the absence of `public`, `private`, or `protected`.
- Narrower than `protected` (excludes subclasses in other packages) but wider than `private` (allows any class in the same package).
- Often used for classes and members intended only for internal use within a specific module/package.

### Examples

```java
class Student {           // default access class
    String name;           // default access field
    int age;                // default access field

    void displayInfo() {   // default access method
        System.out.println(name + " - " + age);
    }
}

// From another class in the SAME package:
Student s1 = new Student();
s1.name = "Aditi";        // OK -- same package
s1.displayInfo();          // OK -- same package

// From a class in a DIFFERENT package:
// s1.name = "Aditi";      // COMPILE ERROR -- default access, different package
```

---

## 10. Accessibility Comparison Table

| Modifier              | Same Class | Same Package | Subclass (different package) | Other Package (non-subclass) |
| --------------------- | :--------: | :----------: | :--------------------------: | :--------------------------: |
| `public`              |     ✅     |      ✅      |              ✅              |              ✅              |
| `protected`           |     ✅     |      ✅      |              ✅              |              ❌              |
| default (no modifier) |     ✅     |      ✅      |              ❌              |              ❌              |
| `private`             |     ✅     |      ❌      |              ❌              |              ❌              |

Reading this table by rows shows how access **narrows** as you move from `public` down to `private` — each level is a strict subset of the visibility of the one above it.

---

## 11. Access Modifiers with

### Classes

- **Top-level classes** can only be declared `public` or default (package-private) — `private` and `protected` are not allowed on top-level classes (they _are_ allowed on nested/inner classes, a more advanced topic).

```java
public class Student { }     // valid -- accessible everywhere
class Teacher { }              // valid -- default access, same package only
// private class Course { }   // INVALID for a top-level class
```

### Variables

- Fields can use **any** of the four access modifiers, and are most commonly made `private` to support encapsulation.

```java
class Student {
    private String name;
    protected int age;
    String grade;          // default
    public String school;
}
```

### Methods

- Methods can use **any** of the four access modifiers, typically `public` for the class's external interface, and `private` for internal helper logic.

```java
class Calculator {
    public int add(int a, int b) { return a + b; }
    private void logCalculation() { /* internal use only */ }
}
```

### Constructors

- Constructors can also use any of the four access modifiers. A `private` constructor, for instance, can be used to prevent a class from being instantiated directly from outside (a more advanced design pattern, not needed for basic use here).

```java
class Student {
    public Student(String name) { }   // typical, accessible constructor
}
```

---

## 12. Real-World Examples

### Bank Account

```java
public class BankAccount {
    private double balance;              // hidden -- protects sensitive financial data
    public String accountHolderName;    // exposed -- generally fine to view

    public void deposit(double amount) {  // controlled, validated access to balance
        if (amount > 0) balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

### Student

```java
public class Student {
    private String name;
    private double gpa;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

### Employee

```java
public class Employee {
    private double salary;       // sensitive -- kept private
    protected String department; // shared with subclasses, e.g. Manager

    public double getSalary() { return salary; }
}
```

### Hospital

```java
public class Hospital {
    private String[] patientRecords;   // sensitive medical data -- private
    public String hospitalName;         // general info -- public is acceptable

    public void admitPatient(String record) {
        // validated, controlled way to add to patientRecords
    }
}
```

Across all four examples, the pattern is consistent: **sensitive or internal data is kept `private`**, while a small, deliberate set of methods (and occasionally fields) is exposed as `public` to form the class's intended interface.

---

## 13. Encapsulation using `private`

### Examples

**Encapsulation** means bundling an object's data with the methods that operate on it, while restricting direct access to that data from outside the class — achieved in Java primarily through `private` fields.

```java
class Student {
    private String name;   // hidden from direct outside access
    private int age;
}
```

### Getters

A **getter** is a public method that returns the value of a private field, providing controlled **read** access.

```java
class Student {
    private String name;

    public String getName() {
        return name;
    }
}
```

### Setters

A **setter** is a public method that assigns a value to a private field, providing controlled **write** access — often including validation logic.

```java
class Student {
    private int age;

    public void setAge(int age) {
        if (age > 0) {         // validation -- prevents invalid data
            this.age = age;
        }
    }

    public int getAge() {
        return age;
    }
}

Student s1 = new Student();
s1.setAge(21);        // controlled write, passes validation
// s1.age = -5;        // COMPILE ERROR -- 'age' is private, cannot be accessed directly
```

### (Simple introduction)

Getters and setters are the most common pattern for exposing controlled access to private fields, forming the basic mechanism of encapsulation. _(A deeper treatment of encapsulation as a formal OOP pillar belongs in its own dedicated chapter.)_

---

## 14. Packages and Access Modifiers

### Examples

Packages group related classes together, and several access modifiers (`protected` and default) derive their meaning specifically from package boundaries.

```java
package com.school.model;

class Student {          // default access -- visible only within com.school.model
    String name;           // default access
}
```

```java
package com.school.admin;

import com.school.model.Student;

class Registrar {
    void register() {
        Student s1 = new Student();   // Compile Error if Student class itself is default
                                        // and Registrar is in a DIFFERENT package
    }
}
```

### (Simple explanation)

- If a class itself has **default access**, it can only be used by other classes in the **same package** — even if you `import` it from elsewhere, you won't be able to access it.
- `public` classes and members are the ones intended to cross package boundaries freely.
- This is why library/API classes meant for external use are typically declared `public`, while internal helper classes often use default access.

_(Packages themselves — declaration, structure, and `import` — are covered more fully in a dedicated chapter; this section focuses only on their interaction with access modifiers.)_

---

## 15. Best Practices

- Default to making fields **`private`**, and expose access only through public getters/setters when necessary — this is the standard encapsulation pattern.
- Use `public` deliberately for methods and classes that are genuinely meant to form the class's external interface.
- Use `protected` specifically when designing for inheritance, where subclasses need direct access to inherited members.
- Avoid using default access carelessly — be intentional about whether a class/member truly should be limited to its package.
- Add **validation logic** inside setters, rather than allowing fields to be set to arbitrary, unchecked values.

---

## 16. Common Mistakes

- **Making all fields `public`** "for convenience," which breaks encapsulation and removes any control over how data is modified.
- **Forgetting that top-level classes can't be `private` or `protected`** — only `public` or default is valid for a top-level class.
- **Assuming `protected` behaves like `public` across all packages** — it only extends access to subclasses in other packages, not to unrelated classes.
- **Confusing default access with `private`** — default still allows access from other classes _in the same package_, unlike `private`.
- **Not validating input inside setters**, defeating the purpose of encapsulating the field in the first place.

---

## 17. Interview Corner

1. **What are access modifiers, and why are they important?**
   Keywords that control the visibility/accessibility of classes and their members, essential for enforcing encapsulation and controlled access to data.

2. **What is the difference between `protected` and default access?**
   Both allow access within the same package; `protected` additionally allows access from subclasses even in different packages, while default does not.

3. **Can a top-level class be `private`?**
   No — top-level classes can only be `public` or default (package-private).

4. **Why is `private` preferred for fields in encapsulation?**
   It prevents direct, uncontrolled access to a class's internal data, forcing all access to go through validated public methods (getters/setters).

5. **What is the most restrictive access modifier, and what is the least restrictive?**
   `private` is the most restrictive (class-only); `public` is the least restrictive (accessible everywhere).

---

## 18. MCQs

1. Which access modifier allows access from anywhere?
   a) private b) default c) public d) protected

2. Which is the most restrictive access modifier?
   a) public b) protected c) default d) private

3. A field with no access modifier written has:
   a) public access b) private access c) default (package-private) access d) protected access

4. `protected` members are accessible:
   a) Only within the same class b) Same package and subclasses (even in other packages) c) Everywhere without restriction d) Nowhere outside the class

5. Which of these CANNOT be applied to a top-level class?
   a) public b) default (no modifier) c) private d) Both public and default are valid, but private/protected are not

6. What is the primary purpose of access modifiers?
   a) To speed up compilation b) To control visibility/access to classes and members c) To define data types d) To create loops

7. A `private` field can be accessed:
   a) From any subclass b) From any class in the same package c) Only within the declaring class d) From anywhere

8. Which access modifier is most associated with supporting inheritance across packages?
   a) private b) default c) protected d) public

9. A getter method is typically declared:
   a) private b) public c) protected d) default

10. What does encapsulation primarily rely on?
    a) public fields b) private fields with controlled public access c) static methods d) default classes only

11. Which statement is TRUE about default access?
    a) It's the same as private b) It allows access within the same package only c) It allows access from anywhere d) It requires a keyword to be written

12. A setter method is used to:
    a) Read a private field's value b) Provide controlled write access to a private field c) Delete a field d) Declare a new class

13. Which access level is a strict subset of `protected`'s visibility?
    a) public b) default c) private d) None

14. Can constructors have access modifiers?
    a) No, never b) Yes, any of the four c) Only public d) Only private

15. If class `A` has default access and is used from a class in a different package:
    a) It compiles fine b) It causes a compile error c) It runs but throws a runtime exception d) It becomes automatically public

16. Which of the following best supports data security in a class?
    a) Making all fields public b) Making sensitive fields private c) Removing all access modifiers d) Making all methods static

17. Which access modifiers can be applied to instance variables (fields)?
    a) Only public b) Only private c) Any of the four (public, private, protected, default) d) None

18. A `protected` field in package A, accessed by a subclass in package B:
    a) Is not accessible b) Is accessible due to the subclass relationship c) Causes a compile error always d) Requires the field to also be static

19. What is the risk of overusing `public` on fields?
    a) Slower compilation b) Breaks encapsulation, allows uncontrolled external modification c) Prevents inheritance d) Causes memory leaks

20. Which access modifier, when used in the same package, behaves identically to default?
    a) public b) private c) protected d) None of the above

---

## 19. University Questions

### Short Questions

1. Define access modifiers and list the four types available in Java.
2. What is the difference between `private` and `protected`?
3. What is default (package-private) access?
4. Why can't a top-level class be declared `private`?
5. What is the role of access modifiers in encapsulation?

### Long Questions

1. Explain all four access modifiers in Java with examples and their accessibility scope.
2. Discuss how access modifiers support encapsulation, using a `BankAccount` example with private fields and public getters/setters.
3. Explain the differences between `protected` and default access, particularly regarding subclasses in different packages.
4. Describe how access modifiers apply differently to classes, fields, methods, and constructors, with examples for each.
5. Using a real-world example (e.g., `Employee` or `Hospital`), explain how a combination of access modifiers would be chosen to design the class responsibly.

---

## 20. Viva Questions

1. What are access modifiers in Java?
2. List the four access modifiers from most to least restrictive.
3. What access level is given when no modifier is specified?
4. Can `protected` members be accessed outside the package? Under what condition?
5. Why is `private` commonly used with encapsulation?
6. What is a getter and a setter?
7. Can a top-level class be `protected`? Why or why not?
8. What is the difference between `public` and default access for a class?
9. Why should sensitive data (like a bank balance) not be `public`?
10. How do access modifiers improve maintainability?

---

## 21. Programming Exercises

1. Design a `Student` class with private fields and public getters/setters for `name` and `gpa`.
2. Design a `BankAccount` class with a private `balance` field, and public `deposit()`/`withdraw()` methods that validate input.
3. Design an `Employee` class with a private `salary` field and a protected `department` field, demonstrating the difference in access.
4. Create two classes in the same package using default access, and demonstrate that they can access each other's default members.
5. Write a `Book` class where all fields are private, with getters for all fields and setters only for fields that should be modifiable (e.g., not `ISBN`).

---

## 22. Summary

Access modifiers — `public`, `private`, `protected`, and default (package-private) — are Java's mechanism for controlling the visibility of classes, fields, methods, and constructors, ranging from fully open (`public`) to fully restricted (`private`, visible only within the declaring class). They form the foundation of **encapsulation**, allowing a class to hide its internal data (typically via `private` fields) while exposing a controlled, validated interface through `public` methods like getters and setters. `protected` extends access specifically to support inheritance across packages, while default access limits visibility to classes within the same package. Choosing the right access modifier for each class member is a key design decision that directly affects a program's data security, maintainability, and adherence to sound object-oriented principles.

---

## 23. Key Takeaways

- Access modifiers control visibility: `public` (everywhere), `protected` (package + subclasses), default (package only), `private` (class only).
- Access narrows as you move from `public` to `private` — each level is a subset of the one above it.
- Top-level classes can only be `public` or default — never `private` or `protected`.
- `private` fields with public getters/setters are the standard pattern for encapsulation.
- `protected` is designed primarily to support inheritance across packages.
- Fields, methods, and constructors can all use any of the four access modifiers.
- Overusing `public` breaks encapsulation and removes control over how data is accessed or modified.

---

## 24. Cheat Sheet

| Modifier    | Same Class | Same Package | Subclass (other package) | Other Package |
| ----------- | :--------: | :----------: | :----------------------: | :-----------: |
| `public`    |     ✅     |      ✅      |            ✅            |      ✅       |
| `protected` |     ✅     |      ✅      |            ✅            |      ❌       |
| default     |     ✅     |      ✅      |            ❌            |      ❌       |
| `private`   |     ✅     |      ❌      |            ❌            |      ❌       |

| Applies To      | Allowed Modifiers      |
| --------------- | ---------------------- |
| Top-level Class | `public`, default only |
| Fields          | All four               |
| Methods         | All four               |
| Constructors    | All four               |

| Pattern                                  | Purpose                                |
| ---------------------------------------- | -------------------------------------- |
| `private` field + `public` getter/setter | Standard encapsulation                 |
| `protected` field/method                 | Shared with subclasses across packages |
| default class/member                     | Internal use within a single package   |

---

_End of Notes — Access Modifiers in Java_
