# 02. Encapsulation in Java

---

## 1. Introduction

- **What is Encapsulation?** Encapsulation is the OOP principle of bundling an object's data (fields) and the methods that operate on that data together within a single class, while restricting direct external access to that data — typically by making fields `private` and providing controlled access through public methods.
- **Why is Encapsulation Important?** Without encapsulation, any part of a program could directly read or modify an object's internal data, with no way to ensure that data remains valid or consistent. Encapsulation gives a class full control over how its own data can be accessed and changed, protecting it from invalid or unintended modification.
- **Encapsulation as a Pillar of OOP:** Encapsulation is one of the four foundational pillars of Object-Oriented Programming (alongside inheritance, polymorphism, and abstraction), and is often considered the most fundamental — since it's what makes a class a genuinely self-contained, well-protected unit in the first place.
- **Real-world Analogy:** Think of a medicine capsule — the active ingredients (data) are sealed safely inside an outer shell (the class), and you interact with it only through its intended, safe interface (swallowing it), never by directly handling the raw ingredients inside. Similarly, a capsule dispenser at a pharmacy doesn't let a customer reach in and grab ingredients directly — everything goes through a controlled process, just as an encapsulated class only allows controlled access to its internal state.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define encapsulation.
- Explain the importance of data hiding.
- Create encapsulated classes.
- Use getters and setters effectively.
- Validate data using setter methods.
- Differentiate encapsulation from abstraction.

---

## 3. What is Encapsulation?

**Definition:** Encapsulation is the practice of combining an object's data and the methods that act on that data into a single unit (a class), while restricting direct outside access to that data — allowing it to be accessed or modified only through well-defined, controlled methods.

**Origin of the word "Encapsulation":** The term comes from "capsule" — something enclosed or sealed within a protective covering — reflecting how a class wraps its internal data safely away from direct outside interference.

**Binding data and methods together:** A class in Java naturally already bundles fields and methods together — encapsulation goes a step further by also **restricting** direct access to those fields, ensuring they can only be interacted with through the class's own methods.

**Data Hiding:** A core aspect of encapsulation is **hiding** an object's internal data from direct outside access (typically by declaring fields `private`), forcing all interaction to go through public methods that can enforce rules, validation, and consistency.

**Examples:**

```java
// Without encapsulation — direct, unrestricted access
class Student {
    public int marks;   // any code, anywhere, can set this to any value — even invalid ones
}

// With encapsulation — controlled, validated access
class Student {
    private int marks;   // hidden from direct access

    public void setMarks(int marks) {
        if (marks >= 0 && marks <= 100) {   // validation
            this.marks = marks;
        } else {
            System.out.println("Invalid marks!");
        }
    }

    public int getMarks() {
        return marks;
    }
}
```

---

## 4. Why Do We Need Encapsulation?

**Problems without encapsulation:**

- **Direct access to data:** Any part of a program can freely read or modify a class's fields, with no oversight or control from the class itself.
- **Invalid object state:** Without validation, fields can be set to nonsensical or impossible values (e.g., negative age, marks above 100), leaving an object in an invalid, inconsistent state.
- **Security issues:** Sensitive data (like account balances or passwords) becomes freely readable and writable by any code that has a reference to the object, with no way to restrict or audit that access.
- **Difficult maintenance:** If a field's internal representation ever needs to change (e.g., splitting a single `name` field into `firstName` and `lastName`), every piece of code directly accessing that field elsewhere in the program would break — encapsulated access through methods insulates external code from such internal changes.

**Benefits:**

- Full control over how data is accessed and modified.
- Ability to enforce validation rules consistently, in one place.
- Internal implementation details can change freely without affecting external code, as long as the public method interface stays the same.
- Improved security, since sensitive data isn't directly exposed.

**Examples:**

```java
class BankAccount {
    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;   // validated, controlled modification
        }
    }

    public double getBalance() {
        return balance;   // read-only access, no direct modification possible from outside
    }
}
```

Without encapsulation, `balance` could be set directly to any value (including negative numbers), bypassing all business logic entirely.

---

## 5. How Encapsulation Works

```
Private Fields
      ↓
Public Methods
      ↓
Controlled Access
      ↓
Validation
      ↓
Safe Object
```

**Diagram:**

```
        ┌───────────────────┐
        │   Private Fields     │   ← hidden from direct outside access
        └──────────┬────────────┘
                   ▼
        ┌───────────────────┐
        │   Public Methods     │   ← the only way to interact with the fields
        │  (getters/setters)    │
        └──────────┬────────────┘
                   ▼
        ┌───────────────────┐
        │  Controlled Access    │   ← external code must go through these methods
        └──────────┬────────────┘
                   ▼
        ┌───────────────────┐
        │     Validation         │   ← setters can reject invalid values
        └──────────┬────────────┘
                   ▼
        ┌───────────────────┐
        │     Safe Object        │   ← guaranteed to remain in a valid, consistent state
        └───────────────────┘
```

---

## 6. Access Modifiers Review

_(Quick Revision)_

- **`private`** — Accessible only within the same class. This is the access level used for encapsulated fields.
- **`public`** — Accessible from anywhere in the program. Typically used for getter/setter methods that provide the controlled access point.
- **`protected`** — Accessible within the same package, and by subclasses in other packages (relevant for inheritance).
- **default (no modifier)** — Accessible only within the same package.

**Role of `private` in encapsulation:** `private` is the access modifier that makes true encapsulation possible — by preventing any code outside the class from directly reading or writing a field, it forces all interaction to go through the class's own controlled, validated methods.

---

## 7. Creating an Encapsulated Class

**Step-by-step:**

1. **Declare fields `private`** — hide the internal data from direct outside access.
2. **Create getter methods** — provide controlled read access to each field, as needed.
3. **Create setter methods** — provide controlled write access to each field, as needed.
4. **Validate data** — within setters, check that incoming values are valid before actually updating the field.

**Complete example:**

```java
class Student {
    private String name;
    private int marks;

    // Getter
    public String getName() {
        return name;
    }

    // Setter
    public void setName(String name) {
        this.name = name;
    }

    // Getter
    public int getMarks() {
        return marks;
    }

    // Setter with validation
    public void setMarks(int marks) {
        if (marks >= 0 && marks <= 100) {
            this.marks = marks;
        } else {
            System.out.println("Invalid marks! Must be between 0 and 100.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        s.setName("Priya");
        s.setMarks(85);

        System.out.println(s.getName() + ": " + s.getMarks());  // Priya: 85

        s.setMarks(150);   // rejected by validation
        System.out.println(s.getMarks());  // still 85, unchanged
    }
}
```

---

## 8. Getter Methods

**Definition:** A getter method is a public method that returns the current value of a private field, providing controlled **read** access to it.

**Purpose:** To allow external code to retrieve a field's value without being able to directly access (or accidentally modify) the field itself.

**Syntax:**

```java
public returnType getFieldName() {
    return fieldName;
}
```

**Naming Convention:** Getters conventionally start with `get`, followed by the capitalized field name (e.g., `getName()` for a field named `name`). For `boolean` fields, the convention is often `isFieldName()` instead (e.g., `isActive()` for a field named `active`).

**Examples:**

```java
class Employee {
    private String name;
    private boolean active;

    public String getName() {
        return name;
    }

    public boolean isActive() {   // boolean getter convention
        return active;
    }
}
```

---

## 9. Setter Methods

**Definition:** A setter method is a public method that assigns a new value to a private field, providing controlled **write** access to it — typically including validation logic to ensure only valid values are accepted.

**Purpose:** To allow external code to modify a field's value in a controlled way, while still allowing the class to enforce rules about what values are acceptable.

**Syntax:**

```java
public void setFieldName(dataType value) {
    // optional validation
    this.fieldName = value;
}
```

**Naming Convention:** Setters conventionally start with `set`, followed by the capitalized field name (e.g., `setName(String name)` for a field named `name`).

**Examples:**

```java
class Employee {
    private double salary;

    public void setSalary(double salary) {
        if (salary >= 0) {
            this.salary = salary;
        } else {
            System.out.println("Salary cannot be negative.");
        }
    }
}
```

---

## 10. Data Validation

Setter methods are the natural place to enforce business rules and data validity checks before actually updating a field.

**Age Validation:**

```java
public void setAge(int age) {
    if (age > 0 && age < 150) {
        this.age = age;
    } else {
        System.out.println("Invalid age.");
    }
}
```

**Salary Validation:**

```java
public void setSalary(double salary) {
    if (salary >= 0) {
        this.salary = salary;
    } else {
        System.out.println("Salary cannot be negative.");
    }
}
```

**Password Validation:**

```java
public void setPassword(String password) {
    if (password != null && password.length() >= 8) {
        this.password = password;
    } else {
        System.out.println("Password must be at least 8 characters.");
    }
}
```

**Marks Validation:**

```java
public void setMarks(int marks) {
    if (marks >= 0 && marks <= 100) {
        this.marks = marks;
    } else {
        System.out.println("Marks must be between 0 and 100.");
    }
}
```

**Email Validation (simple check):**

```java
public void setEmail(String email) {
    if (email != null && email.contains("@") && email.contains(".")) {
        this.email = email;
    } else {
        System.out.println("Invalid email format.");
    }
}
```

---

## 11. Read-only Classes

**Getter Only:** A class can be made **read-only** for a particular field by providing only a getter for it, with no corresponding setter — meaning the field can be read from outside the class, but never modified after it's initially set (typically via the constructor).

**Examples:**

```java
class Employee {
    private final String employeeId;

    public Employee(String employeeId) {
        this.employeeId = employeeId;
    }

    public String getEmployeeId() {   // getter only — no setter provided
        return employeeId;
    }
}
```

**When to use:** Read-only fields are appropriate for values that should be set once (typically at object creation) and never change afterward — such as a unique ID, a creation timestamp, or other fundamentally fixed attributes of an object.

---

## 12. Write-only Classes

**Setter Only:** A class can be made **write-only** for a particular field by providing only a setter for it, with no corresponding getter — meaning the field can be updated from outside the class, but its current value can never be directly read back out.

**Examples:**

```java
class UserAccount {
    private String password;

    public void setPassword(String password) {   // setter only — no getter provided
        if (password != null && password.length() >= 8) {
            this.password = password;
        }
    }
    // no getPassword() — password should never be readable directly once set
}
```

**When to use:** Write-only fields are appropriate for sensitive data, like passwords, where allowing the value to be set (e.g., during registration or a password change) is necessary, but exposing it for direct reading afterward would be a security risk.

---

## 13. Immutable Classes (Introduction)

**Definition:** An immutable class is a class whose objects, once created, **cannot have their state changed** in any way — every field is set once (typically via the constructor) and never modified afterward.

**Characteristics:**

- All fields are declared `private` and `final`.
- No setter methods are provided at all.
- All fields are initialized through the constructor, exactly once.
- Any method that appears to "modify" the object actually returns a **new** object instead, leaving the original unchanged.

**Benefits:** Immutable objects are inherently thread-safe (since their state never changes, there's no risk of concurrent modification issues), simpler to reason about, and safe to freely share across different parts of a program without fear of unexpected changes.

**Simple Example:**

```java
final class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() { return x; }
    public int getY() { return y; }

    // No setters — once created, x and y can never change
}
```

_(A full, dedicated treatment of immutability, including designing complex immutable classes, is provided in a later chapter.)_

---

## 14. Encapsulation vs Data Hiding

**Definition:**

- **Encapsulation** is the broader concept of bundling data and behavior together within a class, along with controlling access to that data through methods.
- **Data Hiding** specifically refers to the practice of restricting direct access to a class's internal data (typically via `private` fields), which is one of the key **techniques** used to achieve encapsulation.

**Relationship:** Data hiding is a _part_ of encapsulation, not a separate, unrelated concept — encapsulation is the overall principle (bundling + controlling access), while data hiding is specifically the "hiding the data" aspect of that principle.

**Comparison Table:**

| Aspect           | Encapsulation                                                  | Data Hiding                                                       |
| ---------------- | -------------------------------------------------------------- | ----------------------------------------------------------------- |
| Scope            | Broader concept — bundling data + behavior + controlled access | Narrower concept — specifically restricting direct access to data |
| Achieved through | Classes, private fields, public methods (getters/setters)      | Primarily through `private` access modifiers                      |
| Focus            | Overall class design and controlled interaction                | Specifically protecting internal data from outside visibility     |
| Relationship     | The broader principle                                          | A specific technique used to achieve encapsulation                |

---

## 15. Encapsulation vs Abstraction

**Definition:**

- **Encapsulation** focuses on **how** data is protected — bundling data and behavior together, and controlling access to that data through methods.
- **Abstraction** focuses on **what** is shown to the outside world — exposing only essential features and hiding complex implementation details, regardless of how that data is internally protected.

**Comparison Table:**

| Aspect           | Encapsulation                                             | Abstraction                                                                              |
| ---------------- | --------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Focus            | HOW data is protected (implementation-level)              | WHAT is shown to the user (design-level)                                                 |
| Achieved through | Access modifiers (`private`), getters/setters             | Abstract classes, interfaces                                                             |
| Primary goal     | Data protection and controlled access                     | Hiding complexity, showing only essentials                                               |
| Relates to       | Data security and validity                                | Simplifying the user's interaction with an object                                        |
| Example          | Making a field `private` and providing a validated setter | Providing a `startCar()` method without exposing the engine's internal ignition sequence |

**Real-world Examples:**

- **Encapsulation:** A car's fuel tank is sealed and only accessible through a fuel cap — you can't reach in and directly touch the fuel; you interact with it through a controlled interface (the fuel cap and gauge).
- **Abstraction:** When driving a car, you use a steering wheel, pedals, and gear shift — you don't need to understand the internal combustion process or transmission mechanics to operate the vehicle; the complexity is hidden behind a simple set of controls.

---

## 16. Advantages of Encapsulation

- **Better Security:** Sensitive data is hidden from direct outside access, reducing the risk of unauthorized or accidental modification.
- **Data Protection:** Validation logic in setters ensures an object's data always remains in a valid, consistent state.
- **Flexibility:** A class's internal implementation can be changed freely (e.g., changing how a value is stored or calculated) without affecting external code, as long as the public method signatures remain the same.
- **Easy Maintenance:** Since all logic related to a particular field is centralized within the class's own methods, understanding, debugging, and updating that logic is more straightforward.
- **Reusability:** Well-encapsulated classes, with clear and controlled interfaces, are easier to reuse safely across different parts of a program.
- **Testing:** Encapsulated classes are easier to test in isolation, since their behavior is fully defined by their public interface, without unpredictable external interference with their internal state.
- **Loose Coupling:** Other parts of a program depend only on a class's public methods, not its internal field structure, reducing the ripple effect of internal changes.

---

## 17. Disadvantages

- **Slightly More Code:** Writing getter and setter methods for every field adds more lines of code compared to simply exposing public fields directly.
- **Extra Methods:** A class can end up with a large number of getter/setter methods, especially for classes with many fields, which can somewhat increase the class's apparent complexity.
- **Small Performance Overhead (Usually Negligible):** Calling a getter/setter method technically involves a small amount of overhead compared to direct field access, though in practice this is generally negligible and often optimized away by the JVM.

---

## 18. Real-world Examples

**Bank Account:**

```java
class BankAccount {
    private double balance;

    public double getBalance() { return balance; }
    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) balance -= amount;
    }
}
```

**Student:**

```java
class Student {
    private String name;
    private int marks;
    // getters and setters with validation, as shown earlier
}
```

**Hospital (Patient):**

```java
class Patient {
    private String diagnosis;   // sensitive — controlled access only

    public String getDiagnosis() { return diagnosis; }
    public void setDiagnosis(String diagnosis) { this.diagnosis = diagnosis; }
}
```

**Employee:**

```java
class Employee {
    private double salary;

    public double getSalary() { return salary; }
    public void setSalary(double salary) {
        if (salary >= 0) this.salary = salary;
    }
}
```

**Library Book:**

```java
class Book {
    private boolean isAvailable = true;

    public boolean isAvailable() { return isAvailable; }
    public void checkOut() {
        if (isAvailable) isAvailable = false;
    }
    public void returnBook() {
        isAvailable = true;
    }
}
```

**Shopping Cart:**

```java
class ShoppingCart {
    private double total;

    public double getTotal() { return total; }
    public void addItem(double price) {
        if (price > 0) total += price;
    }
}
```

**ATM Machine:**

```java
class ATM {
    private double accountBalance;

    public double checkBalance() { return accountBalance; }
    public void withdraw(double amount) {
        if (amount > 0 && amount <= accountBalance) {
            accountBalance -= amount;
        } else {
            System.out.println("Invalid or insufficient balance.");
        }
    }
}
```

---

## 19. Best Practices

- **Always keep fields private** — this is the foundation of encapsulation; avoid public fields except for `public static final` constants.
- **Validate data in setters** — enforce business rules and reject invalid values before they're ever assigned to a field.
- **Expose only necessary methods** — don't provide a getter or setter for a field simply out of habit; only expose what external code genuinely needs.
- **Don't create setters unnecessarily** — if a field should never change after object creation, provide only a getter (or make the field `final` with no setter at all).
- **Keep objects in a valid state** — design setters and constructors so that it's never possible to leave an object with inconsistent or nonsensical data.

---

## 20. Common Mistakes

1. **Public fields:**

   ```java
   class Student {
       public int marks;   // no protection, no validation — defeats the purpose of encapsulation
   }
   ```

2. **No validation:**

   ```java
   public void setMarks(int marks) {
       this.marks = marks;   // accepts ANY value, including invalid ones like -50 or 500
   }
   ```

3. **Returning mutable objects directly:**

   ```java
   class Team {
       private List<String> members = new ArrayList<>();

       public List<String> getMembers() {
           return members;   // returns direct reference — caller can modify the internal list!
       }
   }
   // Better: return a copy, e.g., new ArrayList<>(members), to preserve true encapsulation
   ```

4. **Unnecessary getters/setters:**
   Automatically generating a getter and setter for every single field, even ones that should never be exposed or modified from outside (like sensitive internal state), undermines the actual purpose of encapsulation.

5. **Breaking encapsulation:**
   Providing a setter for a field that logically should never change after construction (like a unique ID), or exposing internal implementation details through poorly designed public methods.

---

## 21. Interview Corner

**Q1. What is encapsulation?**
Encapsulation is the OOP principle of bundling an object's data and the methods that operate on it within a single class, while restricting direct outside access to that data through the use of private fields and controlled public methods.

**Q2. Why is encapsulation important?**
It protects an object's internal data from invalid or unauthorized modification, allows validation rules to be enforced consistently, and gives a class the flexibility to change its internal implementation without breaking external code that depends on it.

**Q3. Is encapsulation the same as data hiding?**
Not exactly — data hiding (restricting direct access to fields) is one specific technique used to achieve the broader principle of encapsulation, which also includes bundling data and behavior together and providing controlled access through methods.

**Q4. Difference between encapsulation and abstraction?**
Encapsulation is about _how_ data is protected (implementation-level, using private fields and access-controlled methods); abstraction is about _what_ is shown to the user (design-level, hiding complexity behind a simpler interface).

**Q5. Difference between encapsulation and information hiding?**
These terms are very closely related and often used interchangeably — information hiding is essentially the underlying goal that data hiding (and by extension, encapsulation) achieves: concealing internal implementation details from the parts of a program that don't need to know them.

**Q6. Why are variables usually private?**
To prevent uncontrolled, unvalidated direct access from outside the class, ensuring that all modifications go through methods that can enforce business rules and keep the object in a valid state.

**Q7. Why use getters and setters?**
They provide a controlled, validated interface for reading and modifying a class's private fields, allowing the class to enforce rules, and giving it the flexibility to change its internal implementation without affecting external code.

**Q8. Can encapsulation exist without inheritance?**
Yes — encapsulation is entirely independent of inheritance; a single, standalone class with private fields and public getter/setter methods is fully encapsulated, with no inheritance involved at all.

**Q9. Can immutable classes be encapsulated?**
Yes — in fact, immutable classes are a strong example of encapsulation done well: fields are private and final, access is tightly controlled (read-only, via getters), and the object's state can never be altered after construction.

---

## 22. MCQs

**1. What is encapsulation primarily achieved through in Java?**
a) Public fields
b) Private fields with public getter/setter methods
c) Static methods only
d) Interfaces only
**Answer: b) Private fields with public getter/setter methods**

**2. What is data hiding?**
a) Deleting unused variables
b) Restricting direct access to a class's internal data
c) Hiding methods from subclasses
d) Making all fields static
**Answer: b) Restricting direct access to a class's internal data**

**3. Which access modifier is essential for achieving encapsulation?**
a) public
b) protected
c) private
d) default
**Answer: c) private**

**4. What is the primary difference between encapsulation and abstraction?**
a) They are the same concept
b) Encapsulation focuses on HOW data is protected; abstraction focuses on WHAT is shown
c) Abstraction requires inheritance; encapsulation does not
d) Encapsulation only applies to static classes
**Answer: b) Encapsulation focuses on HOW data is protected; abstraction focuses on WHAT is shown**

**5. What is a read-only class field characterized by?**
a) A setter but no getter
b) A getter but no setter
c) Neither getter nor setter
d) Both getter and setter
**Answer: b) A getter but no setter**

**6. Which of these is a common mistake that breaks encapsulation?**
a) Using private fields
b) Validating input in setters
c) Returning a direct reference to a mutable internal object
d) Providing only necessary getters
**Answer: c) Returning a direct reference to a mutable internal object**

**7. What characterizes an immutable class?**
a) Fields are public and mutable
b) Fields are private and final, with no setters
c) Fields have only setters
d) Fields are static
**Answer: b) Fields are private and final, with no setters**

**8. Can encapsulation exist without inheritance?**
a) No, they are dependent
b) Yes, they are independent concepts
c) Only in abstract classes
d) Only with interfaces
**Answer: b) Yes, they are independent concepts**

**9. What is the purpose of validation inside a setter method?**
a) To slow down the program
b) To ensure only valid data is assigned to a field
c) To make the field public
d) To remove the need for a getter
**Answer: b) To ensure only valid data is assigned to a field**

**10. Which of these best describes "write-only" access in an encapsulated class?**
a) A getter with no setter
b) A setter with no getter
c) Both public getter and setter
d) Neither getter nor setter
**Answer: b) A setter with no getter**

---

## 23. University Questions

### Short Questions

1. What is encapsulation? Give an example.
2. What is data hiding, and how does it relate to encapsulation?
3. What is the purpose of getter and setter methods?
4. What is an immutable class?
5. Differentiate between a read-only and a write-only class field.

### Long Questions

1. Explain encapsulation in Java with a complete program example involving private fields, getters, setters, and validation.
2. Differentiate between encapsulation and abstraction with a comparison table and real-world examples.
3. Discuss the advantages and disadvantages of encapsulation in software design.
4. Explain how data validation is implemented in setter methods, with examples for at least three different types of validation.
5. Design and explain an encapsulated `BankAccount` class, including proper validation for deposit and withdrawal operations.

---

## 24. Viva Questions

1. Why is direct access to fields discouraged in encapsulated classes?
2. What is the naming convention for getter methods on boolean fields?
3. Can a class be encapsulated without having any setters at all?
4. What happens if a getter returns a direct reference to a mutable field (like a `List`)? Why is this a problem?
5. What is the relationship between `private` access modifiers and encapsulation?
6. Give one real-world example of a write-only field.
7. Why might a class avoid providing a setter for a particular field?
8. What makes a class immutable, and how does that relate to encapsulation?
9. Is data hiding the same thing as encapsulation? Explain the distinction.
10. Why is encapsulation considered important for testing and maintainability?

---

## 25. Programming Exercises

**Student Class**

Create an encapsulated `Student` class with private fields for `name` and `marks`, with a setter for `marks` that validates the value is between 0 and 100.

**Employee Class**

Create an encapsulated `Employee` class with a private `salary` field, ensuring the setter rejects negative values.

**Bank Account**

Create an encapsulated `BankAccount` class with `deposit()` and `withdraw()` methods that validate amounts and prevent overdrafts.

**Library Book**

Create an encapsulated `Book` class with a private `isAvailable` field, and `checkOut()`/`returnBook()` methods that properly manage its state.

**Product Class**

Create an encapsulated `Product` class with private `price` and `stockQuantity` fields, validating that neither can be set to a negative value.

**Hospital Patient**

Create an encapsulated `Patient` class with a private `diagnosis` field, providing controlled getter/setter access appropriate for sensitive medical information.

---

## 26. Challenge Problems

**Create an encapsulated:**

- **ATM:** Design a class with a private balance field, and methods for checking balance, depositing, and withdrawing, with proper validation to prevent overdrafts and negative transactions.
- **Shopping Cart:** Design a class with a private list of items and a private total, providing controlled methods to add/remove items, with the total automatically validated and recalculated.
- **Employee Management System:** Design a class hierarchy with encapsulated `Employee` objects, including validated salary and bonus fields, and methods to calculate total compensation.
- **Inventory Management System:** Design a class with encapsulated `Product` objects tracking stock levels, with validated methods for adding and removing stock that prevent negative inventory.

_(All of the above should be implemented with proper validation in setters, ensuring objects can never be left in an invalid state.)_

---

## 27. Summary

- **Encapsulation** is the OOP principle of bundling data and behavior together within a class, while restricting direct outside access to that data through private fields and controlled public methods.
- **Data hiding**, achieved primarily through the `private` access modifier, is the specific technique used to protect a class's internal data as part of the broader encapsulation principle.
- **Getters** provide controlled read access, and **setters** provide controlled write access — often including validation logic to ensure only valid data is ever assigned to a field.
- Classes can be designed as **read-only** (getter only), **write-only** (setter only), or fully **immutable** (no setters at all, fields set once via the constructor).
- Encapsulation is distinct from but complementary to **abstraction** — encapsulation focuses on _how_ data is protected, while abstraction focuses on _what_ is exposed to the user.
- Proper encapsulation leads to better security, easier maintenance, greater flexibility, and more robust, valid object states throughout a program's execution.

---

## 28. Key Takeaways

- Encapsulation = bundling data + behavior + restricting direct access.
- `private` fields + public getters/setters = the core mechanism of encapsulation in Java.
- Data hiding is a technique that _achieves_ encapsulation, not a separate, unrelated concept.
- Validation belongs in setters, ensuring objects remain in a valid state at all times.
- Encapsulation ≠ Abstraction: encapsulation is about protecting data (how); abstraction is about hiding complexity (what).
- Immutable classes represent a strong, deliberate form of encapsulation — no setters, all fields final.

---

## 29. Cheat Sheet

**Definition:**

```
Encapsulation = binding data + methods together, in one class,
                with controlled (validated) access to that data
```

**Getter:**

```java
public returnType getFieldName() {
    return fieldName;
}
```

**Setter:**

```java
public void setFieldName(dataType value) {
    if (/* valid condition */) {
        this.fieldName = value;
    }
}
```

**Validation Pattern:**

```java
public void setAge(int age) {
    if (age > 0 && age < 150) {
        this.age = age;
    } else {
        System.out.println("Invalid age.");
    }
}
```

**Advantages (quick list):**

```
✔ Better security
✔ Data protection / validity
✔ Flexibility to change internals
✔ Easier maintenance
✔ Reusability
✔ Easier testing
✔ Loose coupling
```

**Comparison Tables:**

| Encapsulation vs Data Hiding | Encapsulation                           | Data Hiding               |
| ---------------------------- | --------------------------------------- | ------------------------- |
| Scope                        | Broader principle                       | Specific technique        |
| Achieved via                 | Class design + private fields + methods | `private` access modifier |

| Encapsulation vs Abstraction | Encapsulation                   | Abstraction                  |
| ---------------------------- | ------------------------------- | ---------------------------- |
| Focus                        | HOW data is protected           | WHAT is shown to the user    |
| Tooling                      | private fields, getters/setters | abstract classes, interfaces |
