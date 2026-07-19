# Interfaces in Java

---

## 1. Introduction

### What is an Interface?

An **interface** in Java is a completely abstract type that defines a **contract** of behavior — a set of methods that any implementing class _must_ provide — without specifying how that behavior is actually implemented (aside from certain modern method types, covered later).

### Why Do We Need Interfaces?

Interfaces let us define _what_ a class can do, completely separate from _how_ it does it. This separation allows different, unrelated classes to be used **interchangeably** by any code that only cares about the contract they fulfill — a foundational idea behind flexible, loosely coupled software design.

### Importance in Modern Java Development

Interfaces are everywhere in real-world Java — from the Collections Framework (`List`, `Map`, `Set`) to enterprise frameworks, to functional programming support (`Runnable`, `Comparator`, lambda expressions). Understanding interfaces is essential to reading and writing idiomatic, professional Java code.

### Real-world Analogy

Think of an interface like an **electrical wall socket standard**. Any device that follows the plug standard (the "contract") can be plugged in and used, regardless of what's inside the device — a lamp, a charger, a fan. The socket doesn't care _how_ the device works internally; it only cares that the device honors the shared connection contract.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define an interface.
- Create and implement interfaces.
- Differentiate interfaces from abstract classes.
- Explain multiple inheritance using interfaces.
- Understand modern interface features introduced in Java 8 and Java 9.

---

## 3. Prerequisites

### Quick Revision

- **Classes** — blueprints defining fields and methods (Chapter 2).
- **Objects** — instances created from classes (Chapter 3).
- **Inheritance** — a mechanism where one class acquires fields/methods from another (`extends`).
- **Method Overriding** — a subclass providing its own implementation of a method inherited from a parent.
- **Polymorphism** — the ability for objects of different types to be treated through a common type, executing type-specific behavior.
- **Abstraction** — hiding implementation details and exposing only essential behavior.
- **Abstract Classes** — classes that cannot be instantiated directly and may contain a mix of implemented and unimplemented (`abstract`) methods.

_(This chapter builds directly on Inheritance, Polymorphism, and Abstract Classes — concepts covered in their own dedicated chapters. A brief recap is given here only where directly relevant to interfaces.)_

---

## 4. What is an Interface?

### Definition

An interface is a reference type in Java, similar to a class, that can contain only **constants**, **method signatures** (abstract methods), **default methods**, **static methods**, and **private methods** (as of Java 9) — but, critically, **no instance fields** and **no constructors**, and (traditionally) no instance-level implementation.

### Meaning of an Interface

An interface specifies **what** a class must be able to do, without dictating **how**. Any class that "signs" this contract (via `implements`) commits to providing real, working implementations of the interface's abstract methods.

### Contract Concept

Think of an interface as a **legal contract**: it lists obligations (methods that must exist), and any class that agrees to the contract (`implements` it) must fulfill every obligation, or the code won't compile.

### Purpose

Interfaces exist to define **behavior contracts** that can be shared across completely unrelated classes, enabling polymorphism without requiring those classes to be related through inheritance.

### Examples

```java
interface Drawable {
    void draw();   // abstract method -- no body, just a signature
}

class Circle implements Drawable {
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Square implements Drawable {
    public void draw() {
        System.out.println("Drawing a square");
    }
}
```

`Circle` and `Square` are unrelated classes, but both honor the `Drawable` contract, so both can be treated as `Drawable` wherever that type is expected.

---

## 5. Why Do We Need Interfaces?

### Complete abstraction

Interfaces (in their traditional form) provide 100% abstraction — pure behavior specification with zero implementation detail, forcing implementing classes to define exactly how each method works.

### Loose coupling

Code that depends on an **interface type** rather than a specific class doesn't need to know or care which concrete implementation it's actually working with — reducing dependencies between different parts of a system.

### Standardization

Interfaces establish a common, standardized set of operations that multiple classes must support, ensuring consistency across different implementations (e.g., every `List` implementation supports `add()`, `remove()`, `get()`).

### Multiple inheritance

Since a Java class can only `extend` one other class, but can `implement` **many** interfaces, interfaces provide Java's mechanism for a class to inherit multiple behavior contracts at once.

### Flexibility

New implementations of an interface can be introduced at any time without changing the code that uses the interface type, as long as the new class honors the contract.

### Scalability

Large systems can be built with many interchangeable components, all conforming to shared interfaces, making it easier to add new functionality or swap implementations as a system grows.

### Real-world Examples

- A `PaymentMethod` interface implemented by `CreditCard`, `PayPal`, and `UPI` classes — a checkout system can process any payment method without knowing the specifics of each.
- A `Shape` interface implemented by `Circle`, `Rectangle`, and `Triangle` — a drawing application can render any shape uniformly.

---

## 6. Declaring an Interface

### Syntax

```java
interface InterfaceName {
    // constants
    // abstract method signatures
    // default methods (Java 8+)
    // static methods (Java 8+)
    // private methods (Java 9+)
}
```

### Explanation

- The `interface` keyword replaces `class` in the declaration.
- Methods without a body are implicitly `public abstract` (you don't need to write these modifiers explicitly for basic abstract methods).
- Fields are implicitly `public static final` (i.e., constants).

### Examples

```java
interface Vehicle {
    int MAX_SPEED = 200;    // implicitly public static final

    void start();             // implicitly public abstract
    void stop();               // implicitly public abstract
}
```

---

## 7. Implementing an Interface

### `implements` Keyword

A class uses the `implements` keyword to declare that it fulfills a particular interface's contract, and must then provide concrete implementations for all of that interface's abstract methods (unless the class itself is declared `abstract`).

### Syntax

```java
class ClassName implements InterfaceName {
    // must implement all abstract methods from InterfaceName
}
```

### Examples

```java
interface Vehicle {
    void start();
    void stop();
}

class Car implements Vehicle {
    public void start() {
        System.out.println("Car starting...");
    }

    public void stop() {
        System.out.println("Car stopping...");
    }
}

Vehicle v = new Car();   // Car object, referenced via the Vehicle interface type
v.start();
v.stop();
```

> **Note:** When implementing an interface's abstract methods, the method must be declared `public` in the implementing class — interface methods are implicitly `public`, and Java doesn't allow reducing visibility when overriding/implementing.

---

## 8. Interface Methods

### Abstract Methods

Method signatures with **no body**, which every implementing class must provide a concrete implementation for.

```java
interface Shape {
    double area();   // abstract method
}
```

### Default Methods (Java 8)

Methods **with a body**, declared using the `default` keyword, providing a **default implementation** that implementing classes automatically inherit — but can optionally override.

```java
interface Shape {
    double area();

    default void describe() {
        System.out.println("This is a shape with area: " + area());
    }
}
```

### Static Methods (Java 8)

Methods **with a body**, declared `static`, belonging to the interface itself — called via the interface name, not through an implementing object.

```java
interface MathOps {
    static int square(int n) {
        return n * n;
    }
}

int result = MathOps.square(5);   // called via interface name
```

### Private Methods (Java 9)

Methods **with a body**, declared `private`, used internally within the interface to share common logic between its own default/static methods — not accessible to implementing classes.

```java
interface Shape {
    double area();

    private String formatArea() {   // private helper, Java 9+
        return "Area: " + area();
    }

    default void describe() {
        System.out.println(formatArea());   // used internally
    }
}
```

### Syntax

```java
interface Example {
    void abstractMethod();                       // abstract

    default void defaultMethod() { }               // default (Java 8)

    static void staticMethod() { }                 // static (Java 8)

    private void privateMethod() { }               // private (Java 9)
}
```

### Rules

- Abstract methods have no body and must be implemented by any non-abstract implementing class.
- Default and static methods must have a body.
- Private methods must have a body and can only be used **within the interface itself**.
- Static methods cannot be overridden by implementing classes (they belong to the interface, not the implementing class).

---

## 9. Interface Variables

### `public`

All interface fields are implicitly `public` — accessible from anywhere the interface itself is accessible.

### `static`

All interface fields are implicitly `static` — belonging to the interface itself, not to any implementing instance.

### `final`

All interface fields are implicitly `final` — meaning they must be initialized at declaration and can never be reassigned.

### Constants

Because interface fields are always `public static final`, they function exactly as **constants**.

### Examples

```java
interface Constants {
    int MAX_USERS = 500;         // implicitly: public static final int MAX_USERS = 500;
    String APP_NAME = "MyApp";   // implicitly: public static final String APP_NAME = "MyApp";
}

class Server implements Constants {
    void showLimit() {
        System.out.println(MAX_USERS);   // accessible directly, inherited as a constant
    }
}

System.out.println(Constants.MAX_USERS);  // also accessible via the interface name directly
```

---

## 10. Multiple Interface Implementation

### Definition

A single class in Java can implement **multiple interfaces** simultaneously, fulfilling the contracts of all of them at once — something a class cannot do with multiple _classes_ via `extends`.

### Syntax

```java
class ClassName implements InterfaceOne, InterfaceTwo, InterfaceThree {
    // must implement all abstract methods from every interface
}
```

### Examples

```java
interface Printable {
    void print();
}

interface Scannable {
    void scan();
}

class AllInOnePrinter implements Printable, Scannable {
    public void print() {
        System.out.println("Printing document...");
    }

    public void scan() {
        System.out.println("Scanning document...");
    }
}
```

### Advantages

- Models real-world entities that naturally fulfill more than one behavior contract (e.g., a printer that both prints and scans).
- Avoids the rigidity of single-class inheritance, while still allowing multiple "is-capable-of" relationships.
- Encourages small, focused interfaces (Interface Segregation) that are combined as needed, rather than one large, monolithic interface.

---

## 11. Multiple Inheritance Using Interfaces

### Definition

**Multiple inheritance** refers to a class inheriting behavior from more than one source. Java does not allow multiple inheritance of _classes_ (`extends` allows only one parent), but it **does** allow multiple inheritance of _interface types and contracts_ via `implements`.

### Why Java Allows It

Multiple inheritance of _classes_ is disallowed in Java specifically to avoid the **"Diamond Problem"** — ambiguity that arises when two parent classes provide conflicting implementations of the same method, and it's unclear which one a subclass should inherit. Since (traditionally) interfaces provided no implementation at all, there was no such conflict — a class could safely "inherit" multiple sets of method _signatures_ without ambiguity. (Java 8's default methods reintroduced a limited version of this conflict, resolved by rules covered in Section 15.)

### Examples

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() {
        System.out.println("Duck flying");
    }

    public void swim() {
        System.out.println("Duck swimming");
    }
}
```

`Duck` effectively "inherits" behavior contracts from both `Flyable` and `Swimmable` — something impossible with class-based `extends` alone.

### Diagram

```
      Flyable            Swimmable
         \                   /
          \                 /
           \               /
              class Duck
        (implements both interfaces)
```

Unlike the "Diamond Problem" diagram with conflicting class implementations, this is a clean, unambiguous combination of separate behavior contracts.

---

## 12. Interface Inheritance

### Interface `extends` Interface

Unlike classes, **interfaces use `extends`** (not `implements`) to inherit from other interfaces — and an interface can extend **multiple** other interfaces at once (since there's no implementation to create ambiguity).

```java
interface Animal {
    void eat();
}

interface Pet extends Animal {
    void play();
}

class Dog implements Pet {
    public void eat() {           // must implement, inherited from Animal
        System.out.println("Dog eating");
    }

    public void play() {          // must implement, declared directly in Pet
        System.out.println("Dog playing");
    }
}
```

### Multiple Interface Inheritance

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

interface Amphibious extends Flyable, Swimmable {   // extends BOTH
    void walk();
}

class FlyingFish implements Amphibious {
    public void fly() { System.out.println("Flying"); }
    public void swim() { System.out.println("Swimming"); }
    public void walk() { System.out.println("Walking"); }
}
```

### Examples

An interface hierarchy like this lets you build increasingly specific behavior contracts, layer by layer, and any class implementing the most specific interface (`Amphibious`) must fulfill **every** method from the entire chain.

---

## 13. Functional Interfaces

### Definition

A **functional interface** is an interface that contains **exactly one abstract method** (though it may contain any number of default, static, or private methods). This single-abstract-method property is what allows functional interfaces to be implemented concisely using **lambda expressions**.

### `@FunctionalInterface`

An optional (but recommended) annotation placed above a functional interface's declaration, which tells the compiler to **enforce** the single-abstract-method rule — causing a compile-time error if a second abstract method is accidentally added.

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}
```

### Examples

```java
@FunctionalInterface
interface Calculator {
    int operate(int a, int b);
}

class Add implements Calculator {
    public int operate(int a, int b) {
        return a + b;
    }
}

Calculator calc = new Add();
System.out.println(calc.operate(3, 4));  // 7
```

### Lambda Preview

Because `Calculator` has exactly one abstract method, it can be implemented far more concisely using a **lambda expression**, instead of writing a full class:

```java
Calculator addLambda = (a, b) -> a + b;
System.out.println(addLambda.operate(3, 4));  // 7
```

_(Lambda expressions are a substantial topic on their own — this is only a brief preview to show why functional interfaces matter; a full treatment belongs in a dedicated Lambda Expressions chapter.)_

---

## 14. Marker Interfaces

### Definition

A **marker interface** is an interface with **no methods or fields at all** — an empty interface used purely to "tag" or "mark" a class as having a certain property, which other code (often the JVM or a framework) can check for using `instanceof`.

### Purpose

Marker interfaces let a class **opt into** special treatment or behavior enforced elsewhere (e.g., by the JVM), without needing to implement any actual methods.

### Examples

```java
interface Markable { }   // an empty, custom marker interface

class SpecialItem implements Markable { }

Object obj = new SpecialItem();
if (obj instanceof Markable) {
    System.out.println("This object is marked!");
}
```

### `Serializable`

A built-in Java marker interface. A class implementing `Serializable` signals to the JVM that its objects are allowed to be converted into a byte stream (serialized) — for example, to save to a file or send over a network.

```java
class Student implements java.io.Serializable {
    String name;
}
```

### `Cloneable`

Another built-in marker interface. A class implementing `Cloneable` signals that it's legal to call `Object`'s `clone()` method to create a field-for-field copy of its objects; without implementing this marker, calling `clone()` throws a `CloneNotSupportedException`.

```java
class Point implements Cloneable {
    int x, y;
}
```

---

## 15. Default Methods

### Definition

A **default method** is a method defined directly inside an interface, **with a body**, using the `default` keyword — giving implementing classes a ready-made implementation they can use as-is, or override if needed.

### Purpose

Default methods were introduced in Java 8 primarily to let existing interfaces (like those in the Collections Framework) **add new methods without breaking every class that already implements them** — since implementing classes automatically inherit the default behavior rather than being forced to implement the new method themselves.

### Conflict Resolution

If a class implements **two interfaces** that both provide a **default method with the same signature**, Java cannot automatically decide which one to inherit — this causes a **compile-time error**, and the implementing class **must explicitly override** the method to resolve the conflict.

```java
interface A {
    default void greet() { System.out.println("Hello from A"); }
}

interface B {
    default void greet() { System.out.println("Hello from B"); }
}

class C implements A, B {
    // Must override greet() to resolve the conflict
    public void greet() {
        A.super.greet();   // explicitly choosing A's version (or write custom logic)
    }
}
```

### Examples

```java
interface Vehicle {
    void start();

    default void honk() {
        System.out.println("Beep beep!");
    }
}

class Car implements Vehicle {
    public void start() {
        System.out.println("Car starting...");
    }
    // honk() is inherited automatically -- no need to implement it
}

Car c = new Car();
c.honk();   // "Beep beep!" -- uses the default implementation
```

---

## 16. Static Methods

### Definition

A **static method** in an interface is a method with a body, declared `static`, belonging to the **interface itself** — not inherited by implementing classes, and not callable through an implementing object.

### Syntax

```java
interface InterfaceName {
    static returnType methodName(parameters) {
        // implementation
    }
}
```

### Examples

```java
interface MathOps {
    static int square(int n) {
        return n * n;
    }

    static int cube(int n) {
        return n * n * n;
    }
}

int result = MathOps.square(5);   // called via interface name, like a utility method
```

Static interface methods are commonly used for utility/helper functionality directly related to the interface's purpose (similar in spirit to how `Collections` or `Math` provide static utility methods).

---

## 17. Private Methods (Java 9)

### Definition

A **private method** in an interface is a method with a body, declared `private`, that can only be called from **within the interface itself** — used to share common logic between the interface's own default and/or static methods, without exposing that logic to implementing classes.

### Purpose

Private interface methods help avoid **code duplication** between multiple default (or static) methods in the same interface, following the same "extract shared logic" principle used in regular classes — without leaking that internal helper method into the public contract.

### Examples

```java
interface Invoice {
    double getAmount();
    double getTaxRate();

    private double calculateTax() {          // private helper -- Java 9+
        return getAmount() * getTaxRate();
    }

    default void printInvoice() {
        System.out.println("Amount: " + getAmount());
        System.out.println("Tax: " + calculateTax());   // reused internally
    }

    default void printTaxOnly() {
        System.out.println("Tax: " + calculateTax());   // reused again, avoiding duplication
    }
}
```

`calculateTax()` is not part of the public contract at all — implementing classes cannot call it directly; it exists purely to support `Invoice`'s own default methods.

---

## 18. Interface vs Abstract Class — Detailed Comparison Table

| Aspect                                | Interface                                                                                          | Abstract Class                                                  |
| ------------------------------------- | -------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **Keyword**                           | `interface`                                                                                        | `abstract class`                                                |
| **Methods**                           | Abstract by default; can have default, static, private methods (Java 8/9+)                         | Can have both abstract and fully implemented (concrete) methods |
| **Fields**                            | Only `public static final` (constants)                                                             | Can have instance variables of any access modifier              |
| **Constructors**                      | Not allowed                                                                                        | Allowed (used when subclassed)                                  |
| **Inheritance keyword (for classes)** | `implements`                                                                                       | `extends`                                                       |
| **Multiple inheritance**              | A class can implement multiple interfaces                                                          | A class can extend only one abstract class                      |
| **Access modifiers on methods**       | Implicitly `public` (abstract/default/static); `private` allowed since Java 9 for internal helpers | Any access modifier allowed                                     |
| **When to use**                       | To define a pure behavior contract, possibly across unrelated classes                              | To share common state/behavior among closely related classes    |
| **State (instance fields)**           | Not supported                                                                                      | Fully supported                                                 |

---

## 19. Interface vs Class — Comparison Table

| Aspect                           | Interface                                                    | Class                                                  |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| **Instantiable?**                | No — cannot create objects of an interface directly          | Yes (if not abstract)                                  |
| **Fields**                       | Only constants (`public static final`)                       | Instance and static variables of any kind              |
| **Constructors**                 | Not allowed                                                  | Allowed                                                |
| **Method bodies**                | Only default, static, private methods have bodies            | All methods typically have bodies                      |
| **Relationship keyword**         | `implements` (by a class) / `extends` (by another interface) | `extends` (single inheritance only)                    |
| **Multiple inheritance support** | Yes, a class can implement many interfaces                   | No, a class can extend only one other class            |
| **Purpose**                      | Define a contract of behavior                                | Define a blueprint for objects with state and behavior |

---

## 20. Advantages of Interfaces

### Loose Coupling

Code written against an interface type doesn't depend on any specific implementation, making it easy to swap implementations without changing the dependent code.

### Multiple Inheritance

A class can implement multiple interfaces, effectively inheriting multiple independent behavior contracts — something not possible with class-based inheritance alone.

### Flexibility

New classes can implement an existing interface at any time, immediately becoming compatible with all code that already works with that interface type.

### Extensibility

Default and static methods (Java 8+) let interfaces evolve over time — adding new functionality without breaking existing implementing classes.

### Better Design

Interfaces encourage designing systems around **behavior contracts** rather than concrete implementations, a core principle of good object-oriented and enterprise software design.

### Testability

Since code can depend on interface types, it's easy to substitute real implementations with mock/fake implementations during testing, without changing the code under test.

---

## 21. Disadvantages

### Extra Design Effort

Designing clean, well-thought-out interfaces requires more upfront planning than simply writing a concrete class — poorly designed interfaces can be harder to fix later, since many classes may already depend on them.

### Too Many Interfaces Can Increase Complexity

Overusing interfaces — creating one for every single class "just in case" — can lead to a codebase with excessive indirection, making it harder to trace what's actually happening and increasing cognitive overhead without real benefit.

---

## 22. Real-world Examples

### Payment Gateway

```java
interface PaymentGateway {
    void processPayment(double amount);
}

class CreditCardPayment implements PaymentGateway {
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment: " + amount);
    }
}
```

### Media Player

```java
interface MediaPlayer {
    void play();
    void pause();
    void stop();
}
```

### Database Driver

```java
interface DatabaseDriver {
    void connect();
    void disconnect();
    void executeQuery(String query);
}
```

### Printer

```java
interface Printer {
    void print(String document);
}
```

### Notification Service

```java
interface NotificationService {
    void sendNotification(String message);
}

class EmailNotification implements NotificationService {
    public void sendNotification(String message) {
        System.out.println("Email sent: " + message);
    }
}
```

### Authentication

```java
interface Authenticator {
    boolean authenticate(String username, String password);
}
```

### Cloud Storage

```java
interface CloudStorage {
    void upload(String fileName);
    void download(String fileName);
    void delete(String fileName);
}
```

Each of these represents a real-world scenario where **multiple, interchangeable implementations** naturally exist (different payment providers, different notification channels, different cloud storage vendors), making interfaces the ideal design tool.

---

## 23. Best Practices

- **Program to interfaces** — declare variables and method parameters using interface types (e.g., `List<String> list = new ArrayList<>();`) rather than concrete class types, to maximize flexibility.
- **Keep interfaces focused** (Interface Segregation Principle) — prefer several small, purpose-specific interfaces over one large interface that forces implementing classes to support methods they don't need.
- **Use meaningful method names** that clearly describe the behavior being contracted, since the interface _is_ the contract's documentation.
- **Prefer interfaces for behavior contracts**, and reserve abstract classes for cases where meaningful shared state or partial implementation genuinely needs to be shared among closely related classes.
- **Avoid "God Interfaces"** — huge interfaces that try to cover every possible responsibility, making implementing classes bloated and hard to maintain.

---

## 24. Common Mistakes

- **Confusing interfaces with abstract classes** — forgetting that interfaces cannot hold instance state or constructors, while abstract classes can.
- **Adding unrelated methods** to a single interface, violating Interface Segregation and forcing implementing classes to support behavior they don't logically need.
- **Misunderstanding interface variables** — forgetting that all interface fields are implicitly `public static final` (constants), not regular mutable instance data.
- **Forgetting to implement required methods** — a non-abstract class that implements an interface must provide implementations for every abstract method, or the code won't compile.
- **Assuming static interface methods are inherited** by implementing classes — they are not; they must be called via the interface name.

---

## 25. Interview Corner

1. **What is an interface?**
   A reference type that defines a contract of behavior — method signatures (and, since Java 8/9, default, static, and private methods) — that implementing classes must fulfill.

2. **Why do we use interfaces?**
   To achieve complete abstraction, loose coupling, and a form of multiple inheritance, enabling flexible, interchangeable implementations of a shared contract.

3. **Difference between an interface and an abstract class?**
   Interfaces cannot hold instance state or constructors and support multiple implementation via `implements`; abstract classes can hold state, have constructors, and support only single inheritance via `extends`.

4. **Can an interface have constructors?**
   No — interfaces cannot be instantiated, so constructors are not allowed.

5. **Can interfaces have variables?**
   Yes, but only as implicitly `public static final` constants — not regular mutable instance fields.

6. **Can interfaces contain implemented methods?**
   Yes — default methods and static methods (Java 8+) and private methods (Java 9+) all have bodies; only abstract methods remain unimplemented.

7. **What are default methods?**
   Methods with a body, declared with the `default` keyword, that implementing classes automatically inherit and can optionally override.

8. **What are marker interfaces?**
   Interfaces with no methods or fields at all, used to "tag" a class with a special property that other code can check via `instanceof` (e.g., `Serializable`, `Cloneable`).

9. **What are functional interfaces?**
   Interfaces with exactly one abstract method, which can be implemented concisely using lambda expressions.

10. **Why are interfaces preferred in enterprise applications?**
    They promote loose coupling, testability (via mock implementations), and flexibility to swap or extend implementations without disrupting dependent code — all critical in large, evolving systems.

---

## 26. MCQs

1. Which keyword is used by a class to implement an interface?
   a) `extends` b) `implements` c) `interface` d) `abstract`

2. Interface fields are implicitly:
   a) `private static final` b) `public static final` c) `protected final` d) `public` only

3. Can an interface have a constructor?
   a) Yes, always b) No c) Only default constructors d) Only if abstract

4. A class implementing an interface must:
   a) Implement all abstract methods, or be declared abstract b) Implement no methods c) Extend the interface d) Never override any methods

5. Which keyword do interfaces use to inherit from other interfaces?
   a) `implements` b) `extends` c) `interface` d) `super`

6. Java achieves multiple inheritance through:
   a) Multiple class extension b) Interfaces c) Constructors d) Static blocks

7. A default method is declared using:
   a) `static` b) `default` c) `final` d) `private`

8. Static methods in interfaces are called via:
   a) An implementing object b) The interface name c) `this` d) `super`

9. What happens if two implemented interfaces have conflicting default methods with the same signature?
   a) The first interface wins automatically b) Compile-time error unless explicitly resolved c) Both execute together d) A runtime exception occurs

10. A functional interface has:
    a) No methods b) Exactly one abstract method c) Only static methods d) Multiple abstract methods

11. Which annotation enforces the functional interface rule?
    a) `@Override` b) `@FunctionalInterface` c) `@Interface` d) `@Functional`

12. A marker interface is:
    a) An interface with many abstract methods b) An empty interface used to tag a class c) A class with no methods d) An interface with only static methods

13. Which of these is a built-in Java marker interface?
    a) `Runnable` b) `Serializable` c) `Comparator` d) `List`

14. Private interface methods (Java 9+) can be called:
    a) From implementing classes directly b) Only from within the interface itself c) From any class in the same package d) From static contexts only

15. Which of these can an interface NOT have?
    a) Abstract methods b) Instance fields c) Static methods d) Default methods

16. Can a class implement multiple interfaces?
    a) No, only one b) Yes, any number c) Only two d) Only if they share methods

17. What is the primary purpose of an interface?
    a) To hold instance data b) To define a behavior contract c) To create objects directly d) To replace all classes

18. Which of these correctly describes interface method visibility?
    a) Abstract/default/static methods are implicitly public b) All methods are private by default c) Methods require explicit `public` always d) Interfaces have no visibility rules

19. What was the primary reason Java introduced default methods?
    a) To allow constructors in interfaces b) To let interfaces evolve without breaking existing implementing classes c) To remove abstract methods entirely d) To support private fields

20. Which of these is TRUE about interface vs abstract class?
    a) Abstract classes support multiple inheritance of state; interfaces do not b) Interfaces can have constructors; abstract classes cannot c) Both can be instantiated directly d) Abstract classes cannot have any concrete methods

---

## 27. University Questions

### Short Questions

1. Define an interface with a suitable example.
2. What is the difference between `implements` and `extends` in the context of interfaces?
3. What is a functional interface? Give an example.
4. What is a marker interface? Name two built-in examples.
5. Why are interface fields implicitly `public static final`?

### Long Questions

1. Explain interfaces in Java with syntax, examples, and their role in achieving abstraction.
2. Discuss how Java achieves multiple inheritance through interfaces, with a diagram and example, and explain why this avoids the Diamond Problem.
3. Differentiate between interfaces and abstract classes with a detailed comparison table and code examples.
4. Explain default, static, and private methods in interfaces (Java 8/9 features), including conflict resolution for default methods.
5. Design a real-world system (e.g., a Payment Gateway) using interfaces, demonstrating multiple implementations of the same contract.

---

## 28. Viva Questions

1. What is an interface in Java?
2. Why can't interfaces have constructors?
3. What is the default access level of interface methods and fields?
4. How does Java achieve multiple inheritance using interfaces?
5. What is the Diamond Problem, and how do interfaces avoid it (traditionally)?
6. What is a default method, and why was it introduced?
7. What happens when two interfaces provide conflicting default methods?
8. What is a functional interface, and how does it relate to lambda expressions?
9. What is a marker interface? Give an example.
10. What is the key difference between an interface and an abstract class?
11. Can an interface extend multiple interfaces? Can a class extend multiple classes?
12. Why can't static interface methods be overridden by implementing classes?

---

## 29. Programming Exercises

1. **Payment System** — Design a `PaymentMethod` interface with a `pay(double amount)` method, implemented by `CreditCard`, `DebitCard`, and `UPI` classes.
2. **Media Player** — Design a `MediaPlayer` interface with `play()`, `pause()`, and `stop()` methods, implemented by an `AudioPlayer` class.
3. **Printer** — Design a `Printer` interface and a `Scanner` interface, then create an `AllInOnePrinter` class implementing both.
4. **Vehicle** — Design a `Vehicle` interface with default methods for common behavior (e.g., `honk()`), implemented by `Car` and `Bike` classes.
5. **Notification System** — Design a `NotificationService` interface implemented by `EmailNotification` and `SMSNotification` classes.
6. **Authentication** — Design an `Authenticator` interface with a static utility method for password strength checking, and a functional interface for a custom validation rule.
7. **Database Driver** — Design a `DatabaseDriver` interface implemented by `MySQLDriver` and `PostgreSQLDriver` classes, demonstrating polymorphic usage.

---

## 30. Challenge Problems

Design interfaces for:

1. **Hospital Management System** — Define interfaces for `Admittable`, `Dischargeable`, and `Billable`, and design a `Patient` class implementing relevant combinations.
2. **Banking System** — Define an `Account` interface with default methods for common operations, implemented by `SavingsAccount` and `CurrentAccount`.
3. **Food Delivery App** — Define interfaces for `Orderable`, `Trackable`, and `Payable`, and combine them meaningfully in a `FoodOrder` class.
4. **Smart Home** — Define a `SmartDevice` interface with default and static methods, implemented by `SmartLight`, `SmartThermostat`, and `SmartLock`.
5. **E-commerce Platform** — Define interfaces for `Searchable`, `Purchasable`, and `Reviewable`, and design a `Product` class implementing all three.
6. **Ride Sharing Application** — Define interfaces for `Bookable`, `Trackable`, and `Payable`, and design a `RideRequest` class combining them, discussing why interfaces (rather than a single class) suit this design.

---

## 31. Summary

An interface in Java defines a pure **behavior contract** — a set of method signatures that any implementing class must fulfill — enabling complete abstraction, loose coupling, and a form of multiple inheritance unavailable through class-based `extends`. While traditionally interfaces contained only abstract methods and constant fields, Java 8 introduced **default** and **static** methods (allowing interfaces to evolve without breaking existing implementations and to hold utility logic), and Java 9 added **private** methods (allowing internal code reuse within the interface itself). A class can implement multiple interfaces at once, and interfaces themselves can extend multiple other interfaces, giving Java a safe, unambiguous form of multiple inheritance. Interfaces differ fundamentally from abstract classes in that they cannot hold instance state or constructors, making them ideal for defining shared contracts across otherwise unrelated classes — a pattern seen throughout the Java standard library and essential to flexible, testable, enterprise-grade software design.

---

## 32. Key Takeaways

- Interfaces define behavior contracts. Classes implement interfaces using the `implements` keyword.
- Java supports multiple inheritance through interfaces.
- Interfaces improve flexibility and maintainability.
- Modern interfaces can include default, static, and private methods.
- All interface fields are implicitly `public static final` constants; no instance state is allowed.
- A class can implement multiple interfaces; an interface can extend multiple interfaces.
- Functional interfaces (exactly one abstract method) enable lambda expressions.
- Marker interfaces carry no methods, only signaling a special property to other code.

---

## 33. Cheat Sheet

### Definition

An interface is a contract of behavior — method signatures (plus optional default/static/private method bodies) that implementing classes must fulfill.

### Syntax

```java
interface Name {
    int CONSTANT = 10;          // public static final
    void abstractMethod();       // public abstract
    default void defMethod() {} // Java 8
    static void statMethod() {} // Java 8
    private void privMethod() {} // Java 9
}

class Impl implements Name {
    public void abstractMethod() { }
}
```

### Rules

| Rule                      | Detail                                        |
| ------------------------- | --------------------------------------------- |
| Fields                    | Implicitly `public static final`              |
| Abstract methods          | No body; implicitly `public abstract`         |
| Default methods           | Have a body; overridable                      |
| Static methods            | Have a body; called via interface name only   |
| Private methods (Java 9+) | Have a body; usable only within the interface |
| Constructors              | Not allowed                                   |
| Instantiation             | Not allowed directly                          |
| Class relationship        | `implements` (multiple allowed)               |
| Interface relationship    | `extends` (multiple allowed)                  |

### Method Types

| Type     | Since    | Has Body? | Overridable by Implementer? |
| -------- | -------- | --------- | --------------------------- |
| Abstract | Java 1.0 | No        | Must implement              |
| Default  | Java 8   | Yes       | Optional                    |
| Static   | Java 8   | Yes       | No (belongs to interface)   |
| Private  | Java 9   | Yes       | N/A (not inherited)         |

### Comparison Tables

See Section 18 (Interface vs Abstract Class) and Section 19 (Interface vs Class) for full detail.

### Best Practices

- Program to interfaces.
- Keep interfaces focused (Interface Segregation).
- Use meaningful, contract-describing method names.
- Prefer interfaces for pure behavior contracts; use abstract classes when shared state is genuinely needed.
- Avoid "God Interfaces."

### Interview Notes

- Multiple inheritance: allowed via interfaces, not via classes.
- Diamond Problem: avoided for abstract methods; must be manually resolved for conflicting default methods.
- Functional interfaces: exactly one abstract method → lambda-compatible.
- Marker interfaces: no members at all, used for tagging (`Serializable`, `Cloneable`).

---

_End of Notes — Interfaces in Java_
