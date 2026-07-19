# Abstract Classes in Java

---

## 1. Introduction

### What is an Abstract Class?

An **abstract class** in Java is a class declared using the `abstract` keyword that **cannot be instantiated directly** — meaning you can never write `new AbstractClassName()`. It exists specifically to serve as a **partial blueprint**, defining some behavior concretely while leaving other behavior (via **abstract methods**) to be implemented by its subclasses.

```java
abstract class Shape {
    abstract double area(); // no implementation — subclasses must provide it
}
```

### Why Do We Need Abstract Classes?

Abstract classes let us capture **common structure and behavior** shared across a group of related classes, while still forcing each individual subclass to define the parts of behavior that genuinely differ between them. This avoids duplicating shared code across every subclass, while still guaranteeing that certain essential methods are implemented consistently.

### Relationship with Abstraction

Abstract classes are one of Java's two primary tools (alongside interfaces) for implementing **abstraction** — the OOP principle of hiding implementation complexity while exposing only essential features. An abstract class defines the **essential operations** (via abstract methods) that every subclass must provide, without dictating exactly _how_ each subclass should implement them.

### Real-world Analogy

Think of an **architectural blueprint template** for houses. It might specify that every house built from it _must_ have a "front door" and a "roof" (structural requirements — like abstract methods), while also providing some pre-built shared components, like a standard foundation design (concrete, shared behavior — like concrete methods). No house is ever built directly _from the blueprint template itself_ (you can't live inside a blueprint) — it only becomes an actual, usable house once a specific design (a subclass) fills in the required details.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define an abstract class and explain its purpose.
- Create abstract classes and abstract methods using correct Java syntax.
- Differentiate between abstract classes and concrete (regular) classes.
- Explain when abstract classes are the appropriate design choice.
- Implement abstraction in Java programs using abstract classes.

---

## 3. Prerequisites

### Quick Revision

**Classes** — Blueprints defining the structure and behavior of objects.
**Objects** — Instances of classes, created using `new`.
**Inheritance** — A subclass acquiring fields/methods from a superclass using `extends`.
**Method Overriding** — A subclass providing its own implementation of an inherited method.
**Polymorphism** — The same method call producing different behavior depending on the actual object.
**Abstraction** — Hiding implementation complexity, exposing only essential features — the broader concept that abstract classes help implement.

---

## 4. What is an Abstract Class?

### Definition

> An abstract class is a class that cannot be instantiated on its own, and may contain a mix of fully-implemented (concrete) methods and unimplemented (abstract) methods, intended to be completed by its subclasses.

### Meaning of the `abstract` Keyword

The `abstract` keyword marks a class (or method) as **incomplete by design** — signaling that it represents a general concept or template rather than something that can exist as a standalone, fully-functional object on its own.

```java
abstract class Animal {
    abstract void makeSound(); // incomplete — no body
}
```

### Purpose

The purpose of an abstract class is to:

- Provide a **common base** with shared fields and concrete (already-implemented) methods.
- **Enforce a contract** — requiring every concrete subclass to implement specific abstract methods.
- Prevent the creation of "meaningless" standalone objects of overly general concepts (e.g., you shouldn't be able to create a generic `new Animal()` if "Animal" alone doesn't represent any specific, real animal).

### Examples

```java
abstract class Shape {
    String color;

    Shape(String color) {
        this.color = color;
    }

    abstract double area(); // abstract method — must be implemented by subclasses

    void displayColor() {   // concrete method — shared, ready-to-use behavior
        System.out.println("Color: " + color);
    }
}
```

---

## 5. Why Do We Need Abstract Classes?

### Code Reusability

Common fields and fully-implemented methods can be written **once** in the abstract class and automatically inherited by every subclass — avoiding duplicated code across related classes.

### Partial Abstraction

Unlike interfaces (traditionally fully abstract), abstract classes support **partial abstraction** — some methods can be fully implemented (shared behavior) while others remain abstract (subclass-specific behavior), giving a flexible middle ground.

### Common Base Class

Abstract classes provide a natural, meaningful **base type** for a group of related subclasses, enabling polymorphic collections and general-purpose code that operates on the shared abstract type.

### Enforcing Method Implementation

By declaring a method `abstract`, the abstract class **forces** every concrete subclass to provide its own implementation — the compiler will produce an error if a concrete subclass fails to implement all inherited abstract methods.

### Real-world Examples

| Domain    | Abstract Class | Shared (Concrete)          | Subclass-Specific (Abstract) |
| --------- | -------------- | -------------------------- | ---------------------------- |
| Vehicles  | `Vehicle`      | `honk()`, `displayModel()` | `startEngine()`              |
| Employees | `Employee`     | `displayDetails()`         | `calculateSalary()`          |
| Shapes    | `Shape`        | `displayColor()`           | `area()`                     |
| Payments  | `Payment`      | `logTransaction()`         | `pay()`                      |

---

## 6. Declaring an Abstract Class

### Syntax

```java
abstract class ClassName {
    // fields
    // constructors
    // concrete methods
    // abstract methods (no body, ends with semicolon)
}
```

### Explanation

- The `abstract` keyword is placed before `class`.
- The class may declare any number of **abstract methods** — each ending with a semicolon instead of a method body (`{...}`).
- The class may also contain regular (concrete) methods, fields, and constructors, just like a normal class.
- An abstract class **cannot be instantiated** — attempting `new ClassName()` on an abstract class results in a compile-time error.

### Examples

```java
abstract class Employee {
    String name;

    Employee(String name) {
        this.name = name;
    }

    abstract double calculateSalary(); // abstract method

    void displayDetails() {            // concrete method
        System.out.println("Employee: " + name);
    }
}

// Employee e = new Employee("Amit"); // COMPILE ERROR: cannot instantiate abstract class
```

---

## 7. Abstract Methods

### Definition

> An abstract method is a method declared without any implementation (no method body) — it consists only of a signature, ending in a semicolon, and must be implemented by any concrete subclass.

### Syntax

```java
abstract returnType methodName(parameters);
```

### Characteristics

- Declared using the `abstract` keyword.
- Has **no body** — no curly braces `{}`, just a semicolon after the signature.
- Can only exist **inside an abstract class** (or an interface).
- **Must be overridden** (implemented) by the first concrete subclass in the inheritance chain — a class cannot leave an inherited abstract method unimplemented unless it is itself declared `abstract`.
- Cannot be declared `private`, `static`, or `final` (each of these would conflict with the requirement that subclasses must be able to override it — see Section 10).

### Examples

```java
abstract class Shape {
    abstract double area();       // abstract method
    abstract double perimeter();  // another abstract method
}

class Circle extends Shape {
    double radius;
    Circle(double radius) { this.radius = radius; }

    @Override
    double area() { return Math.PI * radius * radius; }

    @Override
    double perimeter() { return 2 * Math.PI * radius; }
}
```

---

## 8. Concrete Methods

### Definition

> A concrete method is a regular method that has a **complete implementation** (a method body) — the standard kind of method you'd write in any class.

### Difference from Abstract Methods

| Aspect              | Abstract Method                     | Concrete Method                                     |
| ------------------- | ----------------------------------- | --------------------------------------------------- |
| Body                | No body (ends with `;`)             | Has a full implementation (`{...}`)                 |
| Where Allowed       | Abstract classes, interfaces        | Any class                                           |
| Must Be Overridden? | Yes, by the first concrete subclass | No, optional (can be inherited as-is or overridden) |
| Purpose             | Enforces subclass-specific behavior | Provides shared, ready-to-use behavior              |

### Examples

```java
abstract class Animal {
    abstract void makeSound();      // abstract — subclass-specific

    void breathe() {                // concrete — shared by all animals
        System.out.println("Breathing...");
    }
}
```

Every subclass of `Animal` automatically inherits the fully-working `breathe()` method without needing to rewrite it, while still being required to provide its own `makeSound()`.

---

## 9. Mixing Abstract and Concrete Methods

### Why Java Allows It

Java allows abstract classes to freely mix abstract and concrete methods because this is exactly what enables **partial abstraction** — some behavior is genuinely common across all subclasses (and should be written once, in the abstract class), while other behavior is genuinely subclass-specific (and should be left abstract, forcing each subclass to define it appropriately).

### Examples

```java
abstract class Employee {
    String name;
    Employee(String name) { this.name = name; }

    abstract double calculateSalary();  // subclass-specific

    void displayDetails() {             // shared/common
        System.out.println("Name: " + name);
        System.out.println("Salary: " + calculateSalary());
    }
}

class Manager extends Employee {
    Manager(String name) { super(name); }

    @Override
    double calculateSalary() { return 80000; }
}

public class MixedMethodsDemo {
    public static void main(String[] args) {
        Employee e = new Manager("Riya");
        e.displayDetails();
        // Name: Riya
        // Salary: 80000.0
    }
}
```

Notice that `displayDetails()` (concrete, shared) internally calls `calculateSalary()` (abstract) — and thanks to dynamic method dispatch, it correctly uses `Manager`'s specific implementation, even though `displayDetails()` itself is defined generically in `Employee`.

### Advantages

- Reduces code duplication for genuinely shared behavior.
- Still enforces subclass-specific implementation where genuinely needed.
- Enables powerful patterns like the **Template Method pattern**, where a concrete method defines a general algorithm's structure, while calling out to abstract methods for the steps that vary by subclass.

---

## 10. Rules of Abstract Classes

- **Cannot be instantiated** — `new AbstractClassName()` is always a compile-time error.
- **May contain abstract methods** — methods with no implementation.
- **May contain concrete methods** — fully-implemented, regular methods.
- **May contain constructors** — used to initialize fields, invoked automatically when a subclass object is created (see Section 11).
- **May contain static methods** — static methods belong to the class itself, not instances, so they don't conflict with the "incomplete" nature of the abstract class.
- **May contain final methods** (but final methods themselves **cannot be declared abstract**) — a method cannot simultaneously be `abstract` (must be overridden) and `final` (cannot be overridden), as these are contradictory requirements.
- **A subclass must implement all inherited abstract methods**, unless that subclass is _itself_ also declared `abstract` (in which case it can defer the responsibility further down the inheritance chain).

```java
abstract class Example {
    abstract void method1();       // OK — abstract method
    void method2() { }             // OK — concrete method
    static void method3() { }      // OK — static method
    final void method4() { }       // OK — final concrete method

    // abstract final void method5(); // COMPILE ERROR — cannot be both abstract and final
}
```

---

## 11. Constructors in Abstract Classes

### Why Constructors Exist

Even though an abstract class can never be instantiated directly, it **can still have constructors** — these constructors exist to initialize the **common fields** that every subclass will inherit. When a subclass object is created, its constructor implicitly (or explicitly, via `super(...)`) calls the abstract superclass's constructor first, ensuring shared fields are properly initialized.

### Execution Order

When a subclass object is created:

1. The **abstract superclass's constructor** executes first (either implicitly, or explicitly via a `super(...)` call).
2. Then the **subclass's own constructor body** executes.

### Examples

```java
abstract class Vehicle {
    String brand;

    Vehicle(String brand) {
        this.brand = brand;
        System.out.println("Vehicle constructor called");
    }

    abstract void start();
}

class Car extends Vehicle {
    Car(String brand) {
        super(brand); // explicitly calls Vehicle's constructor
        System.out.println("Car constructor called");
    }

    @Override
    void start() {
        System.out.println(brand + " car starting");
    }
}

public class ConstructorDemo {
    public static void main(String[] args) {
        Car c = new Car("Toyota");
        c.start();
        // Output:
        // Vehicle constructor called
        // Car constructor called
        // Toyota car starting
    }
}
```

---

## 12. Fields in Abstract Classes

### Instance Variables

Abstract classes can declare regular **instance variables**, just like any other class — each subclass object will have its own copy of these fields, inherited from the abstract class.

```java
abstract class Employee {
    String name;   // instance variable
    double baseSalary;
}
```

### Static Variables

Abstract classes can also declare **static variables**, shared across all instances of all subclasses (since `static` fields belong to the class itself, not any particular object).

```java
abstract class Employee {
    static int employeeCount = 0; // shared across all Employee subclasses
}
```

### Constants

Abstract classes commonly declare **constants** using `static final`, representing fixed values relevant to the entire class hierarchy.

```java
abstract class Shape {
    static final double PI_APPROX = 3.14159;
}
```

### Examples

```java
abstract class BankAccount {
    String accountHolder;
    double balance;
    static int totalAccounts = 0;
    static final double MIN_BALANCE = 500.0;

    BankAccount(String accountHolder, double balance) {
        this.accountHolder = accountHolder;
        this.balance = balance;
        totalAccounts++;
    }

    abstract void applyInterest();
}
```

---

## 13. Abstract Class Inheritance

### Conceptual Flow

```
Parent Abstract Class
        |
        v
   Child Class
        |
        v
Method Implementation
```

### Examples

```java
abstract class Shape {
    abstract double area();
}

class Rectangle extends Shape {
    double length, width;
    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() {
        return length * width;
    }
}

public class InheritanceDemo {
    public static void main(String[] args) {
        Shape s = new Rectangle(5, 4); // upcasting
        System.out.println("Area: " + s.area()); // Area: 20.0
    }
}
```

`Rectangle` inherits from the abstract `Shape` class and is **required** to implement `area()`, since it is a concrete (non-abstract) class. Once implemented, the object can be used polymorphically through a `Shape` reference, just like with regular inheritance.

---

## 14. Multi-level Abstract Inheritance

Abstract classes can participate in **multi-level inheritance hierarchies**, where an abstract class extends another abstract class, deferring the implementation of abstract methods further down the chain.

### Examples

```java
abstract class Animal {
    abstract void makeSound();
}

abstract class Pet extends Animal {
    abstract void play(); // Pet adds another abstract method, still doesn't implement makeSound()
}

class Dog extends Pet {
    @Override
    void makeSound() { System.out.println("Bark"); }

    @Override
    void play() { System.out.println("Fetching the ball"); }
}

public class MultilevelAbstractDemo {
    public static void main(String[] args) {
        Pet p = new Dog();
        p.makeSound(); // Bark
        p.play();      // Fetching the ball
    }
}
```

### Execution Flow

```
Animal (abstract)
   makeSound() -- abstract, undefined
        |
        v
Pet (abstract, extends Animal)
   play() -- abstract, undefined
   makeSound() -- still undefined (Pet is also abstract, so it's allowed to skip it)
        |
        v
Dog (concrete, extends Pet)
   makeSound() -- IMPLEMENTED
   play() -- IMPLEMENTED
   (Dog must implement BOTH inherited abstract methods, since it's the
    first concrete class in the chain)
```

Only when a **concrete** (non-abstract) class appears in the hierarchy does Java require **all** inherited abstract methods (from every ancestor in the chain) to be implemented.

---

## 15. Abstract Class vs Concrete Class

### Comparison Table

| Aspect                         | Abstract Class                                   | Concrete Class                            |
| ------------------------------ | ------------------------------------------------ | ----------------------------------------- |
| Instantiable?                  | No — cannot create objects directly              | Yes — can be instantiated with `new`      |
| Abstract Methods               | Can contain abstract methods                     | Cannot contain any abstract methods       |
| Purpose                        | Serves as a partial template/base for subclasses | Represents a fully complete, usable class |
| Declaration Keyword            | `abstract class`                                 | `class` (no `abstract` keyword)           |
| Must be Extended to be Useful? | Yes — typically must be subclassed to be used    | No — can be used directly as-is           |
| Example                        | `abstract class Shape`                           | `class Rectangle extends Shape`           |

---

## 16. Abstract Class vs Interface

### Comparison Table (Simple Overview)

| Aspect                      | Abstract Class                                                  | Interface                                                                      |
| --------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Keyword                     | `abstract class`                                                | `interface`                                                                    |
| Method Implementation       | Can mix abstract and concrete methods                           | Traditionally all abstract (modern Java allows `default`/`static` methods too) |
| Fields                      | Can have instance variables, static variables, constants        | Fields are implicitly `public static final` (constants only)                   |
| Constructors                | Can have constructors                                           | Cannot have constructors                                                       |
| Inheritance                 | A class can extend only ONE abstract class (single inheritance) | A class can implement MULTIPLE interfaces                                      |
| Access Modifiers on Methods | Can use any access modifier                                     | Methods are implicitly `public` (traditionally)                                |
| Best Used For               | Sharing common state/behavior among closely related classes     | Defining a capability/contract that unrelated classes can adopt                |

> _(This is only a simple overview — a full, dedicated comparison and deep dive into Interfaces will follow in the next chapter.)_

---

## 17. Advantages of Abstract Classes

- **Code Reuse** — shared fields and concrete methods are written once and inherited by all subclasses.
- **Common Functionality** — provides a natural home for behavior genuinely shared across a family of related classes.
- **Better Design** — enforces a clear contract (abstract methods) while still allowing flexibility (concrete methods) — a balanced middle ground between full abstraction and full implementation.
- **Flexibility** — supports the Template Method pattern and other designs where a general algorithm structure is defined once, with specific steps delegated to subclasses.
- **Maintainability** — changes to shared (concrete) logic only need to be made in one place (the abstract class), automatically propagating to every subclass.

---

## 18. Disadvantages

### Single Inheritance Limitation

Since Java only supports **single inheritance** for classes, a class can extend only **one** abstract class — if a class needs to inherit behavior from multiple unrelated sources, abstract classes alone cannot achieve this (interfaces, which support multiple implementation, are often used to complement this limitation).

### More Complex Hierarchy

Introducing abstract classes adds additional layers to a class hierarchy, which can make the overall structure harder to navigate and understand, especially in deep or poorly-organized hierarchies.

### Less Flexible Than Interfaces

Because a class can only extend one abstract class (but can implement many interfaces), abstract classes are generally considered less flexible for scenarios where a class needs to adopt multiple, unrelated capabilities simultaneously.

---

## 19. Real-world Examples

### Vehicle

```java
abstract class Vehicle {
    String brand;
    Vehicle(String brand) { this.brand = brand; }
    abstract void startEngine();
    void honk() { System.out.println("Beep beep!"); }
}
```

### Employee

```java
abstract class Employee {
    String name;
    Employee(String name) { this.name = name; }
    abstract double calculateSalary();
    void displayDetails() { System.out.println("Name: " + name); }
}
```

### Shape

```java
abstract class Shape {
    abstract double area();
    void printArea() { System.out.println("Area: " + area()); }
}
```

### Animal

```java
abstract class Animal {
    abstract void makeSound();
    void breathe() { System.out.println("Breathing..."); }
}
```

### BankAccount

```java
abstract class BankAccount {
    double balance;
    BankAccount(double balance) { this.balance = balance; }
    abstract void applyInterest();
    void checkBalance() { System.out.println("Balance: " + balance); }
}
```

### Game Character

```java
abstract class GameCharacter {
    String name;
    int health = 100;
    GameCharacter(String name) { this.name = name; }
    abstract void attack();
    void displayHealth() { System.out.println(name + " Health: " + health); }
}
```

---

## 20. Best Practices

1. **Use abstract classes when classes share common state and behavior** — if subclasses genuinely have shared fields and shared logic (not just a shared method signature), an abstract class is usually the right choice over a plain interface.
2. **Keep abstract methods meaningful** — only mark a method abstract if it genuinely needs subclass-specific behavior; don't leave methods abstract just for the sake of it.
3. **Avoid unnecessary abstract classes** — if a class hierarchy doesn't actually need shared state or partial implementation, a plain interface (or even no abstraction at all) may be simpler and more appropriate.
4. **Design for extension** — write abstract classes with the expectation that many different subclasses will extend them over time; keep the abstract contract stable and meaningful.

---

## 21. Common Mistakes

### Instantiating an Abstract Class

```java
abstract class Shape { abstract double area(); }
Shape s = new Shape(); // COMPILE ERROR: Shape is abstract; cannot be instantiated
```

### Forgetting to Implement Abstract Methods

```java
abstract class Shape { abstract double area(); }
class Circle extends Shape { } // COMPILE ERROR: Circle must implement area() (or be abstract itself)
```

### Declaring Abstract and Final Together

```java
abstract class Shape {
    abstract final double area(); // COMPILE ERROR: cannot be both abstract and final
}
```

This is contradictory: `abstract` means "must be overridden," while `final` means "cannot be overridden" — Java disallows combining them.

### Confusing Abstract Classes with Interfaces

**Mistake:** Assuming abstract classes and interfaces are interchangeable in every scenario.
**Clarification:** They serve different design purposes — abstract classes are best for sharing common state/behavior among closely related classes (single inheritance only), while interfaces are best for defining a capability/contract that can be adopted by many unrelated classes (multiple implementation allowed). See Section 16 for the comparison.

---

## 22. Interview Corner

**Q1. What is an abstract class?**
A class declared with the `abstract` keyword that cannot be instantiated directly, and may contain a mix of abstract (unimplemented) and concrete (fully-implemented) methods, serving as a partial template for its subclasses.

**Q2. Can we create an object of an abstract class?**
No — attempting `new AbstractClassName()` results in a compile-time error. However, you can create an object of a **concrete subclass** and reference it via an abstract class reference (upcasting).

**Q3. Can an abstract class have constructors?**
Yes — abstract classes can have constructors, used to initialize shared fields. These constructors are invoked automatically (via an implicit or explicit `super()` call) whenever a subclass object is created.

**Q4. Can it contain concrete methods?**
Yes — abstract classes can freely mix abstract methods (no implementation) with concrete methods (full implementation), enabling partial abstraction.

**Q5. Can it contain static methods?**
Yes — abstract classes can contain static methods, since static methods belong to the class itself rather than any particular instance, and don't conflict with the class's "incomplete" nature.

**Q6. Difference between abstract class and interface?**
Abstract classes support single inheritance, can hold instance state and constructors, and allow mixing abstract/concrete methods. Interfaces support multiple implementation, traditionally hold only constants (no instance state) and no constructors, and (traditionally) contain only abstract methods — though modern Java interfaces can also include `default`/`static` methods.

**Q7. Why use abstract classes?**
To share common state and behavior among closely related classes while still enforcing that certain subclass-specific behavior must be implemented — striking a balance between code reuse and enforced customization.

---

## 23. MCQs

**1. Can an abstract class be instantiated directly?**
a) Yes, always b) No, never c) Only with a constructor d) Only if it has no abstract methods

> **Answer: b)**

**2. What happens if a concrete subclass fails to implement an inherited abstract method?**
a) Runtime exception b) Compile-time error c) The method is silently ignored d) Nothing happens

> **Answer: b)**

**3. Which keyword declares an abstract class?**
a) `final` b) `abstract` c) `static` d) `interface`

> **Answer: b)**

**4. Can an abstract class have a constructor?**
a) No, never b) Yes, to initialize shared fields c) Only if it has no abstract methods d) Only static constructors

> **Answer: b)**

**5. Can a method be both `abstract` and `final`?**
a) Yes b) No, this is a compile-time error c) Only in interfaces d) Only in static context

> **Answer: b)**

**6. What must a concrete subclass of an abstract class do?**
a) Nothing special b) Implement all inherited abstract methods c) Redeclare all fields d) Avoid using constructors

> **Answer: b)**

**7. Can an abstract class contain static methods?**
a) No b) Yes c) Only final static methods d) Only in interfaces

> **Answer: b)**

**8. What is "partial abstraction"?**
a) A class with only abstract methods b) A class mixing both abstract and concrete methods c) A fully implemented class d) An interface with default methods only

> **Answer: b)**

**9. In a multi-level abstract inheritance chain, when must all abstract methods finally be implemented?**
a) In the first subclass, always b) Only when a concrete (non-abstract) class appears in the chain c) Never d) Only in interfaces

> **Answer: b)**

**10. Can a class extend more than one abstract class in Java?**
a) Yes b) No, Java supports single inheritance for classes c) Only with interfaces d) Only with static classes

> **Answer: b)**

**11. What is the primary purpose of an abstract method?**
a) To provide default behavior b) To enforce that subclasses provide their own implementation c) To prevent inheritance d) To improve performance

> **Answer: b)**

**12. Which of the following is TRUE about abstract class fields?**
a) They cannot have instance variables b) They can have instance variables, static variables, and constants c) They can only have constants d) They cannot have static variables

> **Answer: b)**

**13. What happens when you try to instantiate an abstract class directly?**
a) Runtime exception b) Compile-time error c) It works fine d) A warning only

> **Answer: b)**

**14. Which statement about abstract classes and interfaces is TRUE?**
a) A class can implement multiple interfaces but extend only one abstract class b) A class can extend multiple abstract classes c) Interfaces support constructors d) Abstract classes cannot have fields

> **Answer: a)**

**15. What is the execution order when a subclass object of an abstract class is created?**
a) Subclass constructor first, then abstract class constructor b) Abstract class constructor first, then subclass constructor c) Only the subclass constructor runs d) Neither runs automatically

> **Answer: b)**

**16. Can an abstract class have zero abstract methods?**
a) No, it must have at least one b) Yes, a class can be declared abstract even with no abstract methods c) Only interfaces allow this d) This causes a compile error

> **Answer: b) (a class can be marked abstract purely to prevent instantiation, even without any abstract methods)**

**17. Which access modifiers are disallowed for abstract methods?**
a) `public` b) `protected` c) `private` d) Both b and a are allowed; c is disallowed

> **Answer: d) (private would prevent subclasses from overriding it, which contradicts the purpose of an abstract method)**

**18. What is a common real-world use case for an abstract class?**
a) Defining unrelated, standalone utility functions b) Modeling a family of related classes sharing common state and partially common behavior (e.g., Shape, Employee) c) Storing only constant values d) Replacing all interfaces

> **Answer: b)**

**19. Which of these is a valid abstract class declaration?**
a) `abstract class Shape { abstract double area(); }` b) `class abstract Shape { }` c) `interface abstract Shape { }` d) `final abstract class Shape { }`

> **Answer: a) (option d is invalid because `final` and `abstract` on a class are contradictory)**

**20. Why can't abstract classes have both `abstract` and `final` on the same method?**
a) Syntax doesn't allow multiple modifiers b) `abstract` requires overriding while `final` prevents overriding — a direct contradiction c) Java doesn't support final methods d) There is no such restriction

> **Answer: b)**

---

## 24. University Questions

### Short Questions

1. What is an abstract class? Can it be instantiated?
2. What is an abstract method? State its syntax rules.
3. Can an abstract class have constructors? Why?
4. State two differences between abstract classes and concrete classes.
5. Can a method be both `abstract` and `final`? Justify your answer.

### Long Questions

1. Explain abstract classes in Java with syntax, rules, and a suitable example program. _(10 marks)_
2. Differentiate between abstract methods and concrete methods, and explain why Java allows mixing both within an abstract class. _(10 marks)_
3. Explain the rules governing abstract classes in Java, with example code demonstrating each rule. _(10 marks)_
4. Explain multi-level abstract class inheritance with a suitable example, including when abstract methods must finally be implemented. _(10 marks)_
5. Compare abstract classes with interfaces, and discuss scenarios where one would be preferred over the other. _(10 marks)_
6. Discuss the advantages and disadvantages of using abstract classes in Java program design. _(5 marks)_

---

## 25. Viva Questions

1. What is an abstract class, in one line?
2. Can you instantiate an abstract class?
3. Can an abstract class have a constructor?
4. What happens if a concrete subclass doesn't implement all abstract methods?
5. Can an abstract class contain concrete methods?
6. Can an abstract class contain static methods?
7. Can a method be declared both `abstract` and `final`? Why or why not?
8. What is the difference between an abstract class and a concrete class?
9. What is the difference between an abstract class and an interface (basic level)?
10. Can an abstract class exist with zero abstract methods?
11. How many abstract classes can a single class extend in Java?
12. What is the execution order of constructors in an abstract class hierarchy?
13. Can abstract classes have instance variables?
14. Why would you choose an abstract class over an interface?
15. What is the Template Method pattern, and how do abstract classes support it?

---

## 26. Programming Exercises

_(All using abstract classes.)_

### 1. Animal

Create an abstract `Animal` class with an abstract `makeSound()` method and a concrete `breathe()` method. Create subclasses `Dog`, `Cat`, and `Cow`, each implementing `makeSound()`.

### 2. Vehicle

Create an abstract `Vehicle` class with an abstract `startEngine()` method and a concrete `honk()` method. Create subclasses `Car` and `Motorcycle`, each implementing `startEngine()`.

### 3. Shape

Create an abstract `Shape` class with an abstract `area()` method and a concrete `printArea()` method that calls `area()` internally. Create subclasses `Circle`, `Rectangle`, and `Triangle`.

### 4. Employee

Create an abstract `Employee` class with an abstract `calculateSalary()` method and a concrete `displayDetails()` method. Create subclasses `Manager`, `Developer`, and `Intern`.

### 5. Bank Account

Create an abstract `BankAccount` class with an abstract `applyInterest()` method and a concrete `checkBalance()` method. Create subclasses `SavingsAccount` and `FixedDepositAccount`.

### Sample Solution — Shape

```java
abstract class Shape {
    abstract double area();

    void printArea() {
        System.out.println("Area = " + area());
    }
}

class Circle extends Shape {
    double radius;
    Circle(double radius) { this.radius = radius; }

    @Override
    double area() { return Math.PI * radius * radius; }
}

class Rectangle extends Shape {
    double length, width;
    Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    double area() { return length * width; }
}

public class ShapeDemo {
    public static void main(String[] args) {
        Shape[] shapes = { new Circle(5), new Rectangle(4, 6) };
        for (Shape s : shapes) {
            s.printArea();
        }
    }
}
```

---

## 27. Challenge Problems

Design abstract classes for the following domains, ensuring each includes both abstract methods (subclass-specific behavior) and concrete methods (shared behavior):

### Hospital Management

Design an abstract `Staff` class with an abstract `performDuty()` method and a concrete `clockIn()` method. Create subclasses `Doctor`, `Nurse`, and `Receptionist`.

### Library Management

Design an abstract `LibraryItem` class with an abstract `getLoanPeriod()` method and a concrete `displayInfo()` method. Create subclasses `Book`, `DVD`, and `Magazine`.

### Food Delivery

Design an abstract `Order` class with an abstract `calculateDeliveryFee()` method and a concrete `printReceipt()` method. Create subclasses `RestaurantOrder` and `GroceryOrder`.

### Smart Home

Design an abstract `SmartDevice` class with an abstract `turnOn()` method and a concrete `showStatus()` method. Create subclasses `SmartLight`, `SmartThermostat`, and `SmartCamera`.

### E-commerce

Design an abstract `Product` class with an abstract `calculateDiscount()` method and a concrete `displayPrice()` method. Create subclasses `Electronics`, `Clothing`, and `Groceries`.

---

## 28. Summary

- An **abstract class** is a class that cannot be instantiated directly, serving as a partial template for related subclasses.
- Abstract classes may freely mix **abstract methods** (no implementation, must be overridden) with **concrete methods** (fully implemented, shared behavior) — this is called **partial abstraction**.
- Abstract classes **can** have constructors, instance variables, static variables, and constants — they behave like regular classes in every way except that they cannot be instantiated and may contain abstract methods.
- A method cannot be both `abstract` and `final`, since these modifiers are contradictory (must vs. cannot be overridden).
- Any **concrete** subclass must implement **all** inherited abstract methods; abstract subclasses may defer this responsibility further down the inheritance chain (multi-level abstract inheritance).
- Java supports only **single inheritance** for classes, meaning a class can extend only one abstract class — a key limitation compared to interfaces, which support multiple implementation.
- Abstract classes are best used when a family of related classes shares **both common state/behavior AND some subclass-specific requirements** — for purely behavioral contracts without shared state, interfaces are often more appropriate (covered next chapter).

---

## 29. Key Takeaways

- Abstract classes cannot be instantiated.
- They can contain both abstract and concrete methods.
- They support partial abstraction.
- Constructors and fields are allowed.
- They promote code reuse through inheritance.

---

## 30. Cheat Sheet

### Definition

> An abstract class = a class that cannot be instantiated, may mix abstract (unimplemented) and concrete (implemented) methods, serving as a partial blueprint for its subclasses.

### Syntax

```java
abstract class ClassName {
    // fields (instance, static, constants)
    ClassName(/* params */) { /* constructor */ }

    abstract returnType abstractMethod(/* params */); // no body

    returnType concreteMethod(/* params */) {          // full body
        // implementation
    }
}
```

### Rules

- Cannot be instantiated (`new AbstractClass()` → compile error).
- May contain abstract AND concrete methods.
- May contain constructors, instance variables, static variables, constants.
- Abstract methods cannot be `private`, `static`, or `final`.
- A method cannot be both `abstract` and `final`.
- First concrete subclass must implement ALL inherited abstract methods.

### Comparison Tables

| Abstract Class            | Concrete Class               |
| ------------------------- | ---------------------------- |
| Cannot be instantiated    | Can be instantiated          |
| Can have abstract methods | Cannot have abstract methods |

| Abstract Class                        | Interface                                                  |
| ------------------------------------- | ---------------------------------------------------------- |
| Single inheritance (`extends` one)    | Multiple implementation (`implements` many)                |
| Can have constructors, instance state | No constructors, no instance state (traditionally)         |
| Mix of abstract + concrete methods    | Traditionally all abstract (now allows `default`/`static`) |

### Best Practices

- Use for classes sharing common state AND some subclass-specific behavior.
- Keep abstract methods meaningful and minimal.
- Don't overuse — plain interfaces may be simpler when there's no shared state.

### Interview Notes

- Abstract class ≠ Interface — different inheritance models, different use cases.
- Abstract classes CAN have constructors (called via `super()` from subclasses).
- A class can extend only ONE abstract class, but implement MANY interfaces.
- `abstract` + `final` on the same method = compile error (contradictory).

---

_End of Chapter — Abstract Classes in Java_
