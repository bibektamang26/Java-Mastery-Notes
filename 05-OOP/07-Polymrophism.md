# Polymorphism in Java

---

## 1. Introduction

### What is Polymorphism?

**Polymorphism** is one of the four core pillars of Object-Oriented Programming (alongside Encapsulation, Inheritance, and Abstraction). It refers to the ability of a single interface, method name, or reference type to take on **many forms** of behavior depending on the context in which it's used.

```java
Animal a = new Dog();
a.makeSound(); // "Bark" — even though the reference type is Animal
```

### Meaning of "Poly" and "Morph"

The word **polymorphism** comes from Greek:

- **"Poly"** = many
- **"Morph"** = forms

Together, they literally mean **"many forms"** — the idea that the same action, method call, or interface can behave differently depending on the actual object involved.

### Why is Polymorphism Important?

- It allows code to be written in a **general, flexible** way — working with a base type or interface, while the actual behavior is determined by the specific object at runtime.
- It reduces the need for long `if-else` or `switch` chains that check an object's type before deciding what to do.
- It makes systems **extensible** — new types can be added without modifying existing code that uses the common interface.
- It's foundational to frameworks, libraries, and design patterns throughout real-world Java development.

### Real-world Analogy

Think of a **remote control's "power" button**. Pressing it triggers different actions depending on the device it's controlling — a TV turns on/off, an AC starts cooling, a music system starts playing. The **action requested is the same** ("power"), but the **actual behavior** depends on the object receiving the command. This is exactly how polymorphism works in code: the same method call produces different results depending on the actual object.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define polymorphism and explain its purpose in object-oriented programming.
- Explain compile-time polymorphism (method/constructor overloading) and runtime polymorphism (method overriding).
- Differentiate clearly between overloading and overriding.
- Understand how Java achieves runtime polymorphism using inheritance, upcasting, and dynamic method dispatch.
- Apply polymorphism to design flexible, real-world Java programs.

---

## 3. Prerequisites

### Quick Revision

**Classes**
A class is a blueprint defining the structure (fields) and behavior (methods) that its objects will have.

**Objects**
An object is an instance of a class — a concrete entity created in memory using the `new` keyword, possessing its own copy of instance data.

**Inheritance**
Inheritance allows one class (subclass/child) to acquire the fields and methods of another class (superclass/parent), using the `extends` keyword — forming an "is-a" relationship.

```java
class Animal { }
class Dog extends Animal { } // Dog is-a Animal
```

**Method Overriding**
Method overriding occurs when a subclass provides its **own specific implementation** of a method that is already defined in its superclass, using the same method signature.

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Bark"); }
}
```

> These four concepts are essential prerequisites — polymorphism, especially runtime polymorphism, is built directly on top of inheritance and method overriding.

---

## 4. What is Polymorphism?

### Definition

> Polymorphism is the ability of an object, method, or reference to take on multiple forms — allowing the same method call or interface to produce different behavior depending on the actual object it operates on.

### Origin of the Word

Derived from Greek: _"polys"_ (many) + _"morphe"_ (form) = **"many forms."**

### One Interface → Many Forms

```
        One Interface (e.g., "makeSound()")
                    |
        +-----------+-----------+
        |           |           |
        v           v           v
      Dog         Cat         Cow
    "Bark"      "Meow"       "Moo"
```

The **same method name** (`makeSound()`), called through a common reference/interface, results in **different behavior** depending on which actual object is invoked.

### Examples

```java
class Shape {
    double area() { return 0; }
}

class Circle extends Shape {
    double radius;
    Circle(double r) { radius = r; }
    @Override
    double area() { return Math.PI * radius * radius; }
}

class Rectangle extends Shape {
    double length, width;
    Rectangle(double l, double w) { length = l; width = w; }
    @Override
    double area() { return length * width; }
}

public class PolymorphismDemo {
    public static void main(String[] args) {
        Shape s1 = new Circle(5);
        Shape s2 = new Rectangle(4, 6);

        System.out.println("Circle area: " + s1.area());
        System.out.println("Rectangle area: " + s2.area());
    }
}
```

Both `s1.area()` and `s2.area()` call the **same method name** through the **same reference type** (`Shape`), but each executes the specific implementation relevant to its actual object.

---

## 5. Why Do We Need Polymorphism?

### Flexibility

Code can be written to operate on a general type (e.g., `Shape`, `Employee`, `Animal`) without needing to know the exact subtype in advance — new subtypes can be introduced later without altering the code that uses them.

### Code Reusability

Common logic (like iterating through a list of `Shape` objects and calling `area()` on each) can be written **once**, and it automatically works correctly for any current or future subclass.

### Extensibility

New behavior can be added to a system by creating new subclasses that override existing methods — without touching or breaking any existing code that relies on the parent type.

### Maintainability

Reduces the need for scattered `if (obj instanceof X)` type-checking logic throughout a codebase — behavior is instead defined locally within each class, making the system easier to understand and modify.

### Cleaner Code

Polymorphism often eliminates large `if-else` or `switch` chains, replacing them with a single polymorphic method call that automatically "does the right thing" based on the object's actual type.

### Real-world Examples

| Scenario            | Polymorphic Behavior                                                             |
| ------------------- | -------------------------------------------------------------------------------- |
| Payment System      | `pay()` behaves differently for `CreditCardPayment`, `UPIPayment`, `CashPayment` |
| Employee Management | `calculateSalary()` differs for `Manager`, `Developer`, `Intern`                 |
| Vehicle System      | `startEngine()` differs for `Car`, `Bike`, `ElectricVehicle`                     |
| Notification System | `send()` differs for `EmailNotification`, `SMSNotification`, `PushNotification`  |

---

## 6. Types of Polymorphism

Java supports two types of polymorphism:

### Compile-Time Polymorphism

Also called **static polymorphism** or **early binding** — resolved by the compiler at **compile time**, based on method signatures. Achieved via **method overloading** and **constructor overloading**.

### Runtime Polymorphism

Also called **dynamic polymorphism** or **late binding** — resolved by the JVM at **runtime**, based on the actual object being referenced. Achieved via **method overriding**, combined with **inheritance** and **upcasting**.

### Overview

```
                Polymorphism
                     |
       +-------------+-------------+
       |                           |
Compile-Time                  Runtime
Polymorphism                  Polymorphism
       |                           |
Method Overloading          Method Overriding
Constructor Overloading     (via Inheritance +
                              Upcasting)
```

---

## 7. Compile-Time Polymorphism

### Definition

Compile-time polymorphism occurs when the **method to be executed is determined at compile time**, based on the method's **signature** (number, type, and order of parameters) — the compiler decides which version of an overloaded method to call before the program even runs.

### Method Overloading

Defining **multiple methods with the same name** but **different parameter lists** within the same class.

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

### Constructor Overloading

Similarly, a class can have **multiple constructors** with different parameter lists, allowing objects to be created in different ways.

```java
class Student {
    String name;
    int age;

    Student() {
        this.name = "Unknown";
        this.age = 0;
    }

    Student(String name) {
        this.name = name;
        this.age = 0;
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### Examples

```java
public class OverloadingDemo {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        System.out.println(calc.add(2, 3));           // calls int version -> 5
        System.out.println(calc.add(2.5, 3.5));        // calls double version -> 6.0
        System.out.println(calc.add(1, 2, 3));         // calls 3-arg version -> 6

        Student s1 = new Student();
        Student s2 = new Student("Amit");
        Student s3 = new Student("Riya", 20);
    }
}
```

### Advantages

- Improves code readability — a single, intuitive method name (`add`) can handle multiple input variations.
- Reduces the need for awkwardly-named methods like `addTwoInts()`, `addTwoDoubles()`, `addThreeInts()`.
- Provides flexible object construction through multiple constructors, catering to different initialization needs.

---

## 8. Runtime Polymorphism

### Definition

Runtime polymorphism occurs when the **method to be executed is determined at runtime**, based on the **actual object** the reference variable points to — not the reference variable's declared type. This is achieved through **method overriding**.

### Method Overriding

A subclass provides a specific implementation of a method already defined in its superclass, using the **exact same method signature**.

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks");
    }
}
```

### Reference Variable

The **declared type** of the variable — determines what methods/fields are visible at compile time.

```java
Animal a; // reference type is Animal
```

### Object

The **actual type** of the object created — determines which overridden method actually executes at runtime.

```java
a = new Dog(); // actual object is Dog
```

### Examples (Simple Introduction)

```java
public class RuntimePolymorphismDemo {
    public static void main(String[] args) {
        Animal a = new Dog(); // reference type Animal, actual object Dog
        a.makeSound();         // "Dog barks" — Dog's overridden version runs
    }
}
```

Even though `a` is declared as type `Animal`, the JVM looks at the **actual object** (`Dog`) at runtime to decide which `makeSound()` implementation to execute — this mechanism is called **dynamic method dispatch**.

> _(A detailed, in-depth discussion of Method Overriding — including rules, access modifiers, and the `super` keyword — is covered in the next chapter.)_

---

## 9. How Polymorphism Works

### Conceptual Flow

```
Reference Variable
        |
        v
   Actual Object
        |
        v
   Method Call
        |
        v
Correct Method Executes
```

### Flow Diagram (Detailed)

```
        +------------------------+
        |  Animal a = new Dog(); |
        +-----------+------------+
                     |
                     v
        +------------------------+
        | a.makeSound() is called |
        +-----------+------------+
                     |
                     v
        +------------------------------+
        | JVM checks the ACTUAL OBJECT  |
        | (Dog), not the reference type |
        | (Animal), at RUNTIME           |
        +-----------+--------------------+
                     |
                     v
        +------------------------+
        | Dog's makeSound() runs  |
        |  -> prints "Dog barks"   |
        +------------------------+
```

This process — where the JVM decides _which_ overridden method to call based on the actual object rather than the reference type — is called **dynamic method dispatch**, and it's the mechanism underlying all of Java's runtime polymorphism.

---

## 10. Polymorphism Using Inheritance

Runtime polymorphism is only possible when classes are related through **inheritance** — a subclass overriding a superclass method.

```
            Animal
           (makeSound)
           /        \
        Dog          Cat
     (Bark)         (Meow)
```

### Examples

```java
class Animal {
    void makeSound() {
        System.out.println("Some generic animal sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}

public class AnimalDemo {
    public static void main(String[] args) {
        Animal[] animals = { new Dog(), new Cat(), new Animal() };

        for (Animal a : animals) {
            a.makeSound(); // calls the correct version for each actual object
        }
        // Bark
        // Meow
        // Some generic animal sound
    }
}
```

This demonstrates the true power of polymorphism: a **single array of type `Animal`** can hold objects of different subclasses, and calling `makeSound()` on each element automatically invokes the correct, type-specific behavior.

---

## 11. Upcasting

### Definition

**Upcasting** is the process of treating a subclass object as an instance of its superclass (or an implemented interface) — assigning it to a reference variable of the parent type. It's called "up" because it moves up the inheritance hierarchy (toward more general types).

### Syntax

```java
ParentClass ref = new ChildClass();
```

### Examples

```java
Animal a = new Dog(); // upcasting — Dog treated as an Animal
```

Upcasting happens **implicitly** in Java — no explicit cast is needed, since a `Dog` **is-an** `Animal` (a valid, always-safe relationship, guaranteed by inheritance).

### Advantages

- Enables **runtime polymorphism** — the foundation for dynamic method dispatch.
- Allows writing **general-purpose code** that works with the parent type, while supporting any current or future subclass.
- Useful for storing **heterogeneous collections** (e.g., an array or list of `Animal` that holds `Dog`, `Cat`, and other subclasses together).
- Supports the design principle of **"program to an interface, not an implementation."**

```java
public class UpcastingDemo {
    public static void main(String[] args) {
        List<Animal> animals = new ArrayList<>();
        animals.add(new Dog());  // upcasting: Dog -> Animal
        animals.add(new Cat());  // upcasting: Cat -> Animal

        for (Animal a : animals) {
            a.makeSound(); // works correctly for each, thanks to polymorphism
        }
    }
}
```

---

## 12. Downcasting (Introduction)

### Definition

**Downcasting** is the reverse of upcasting — converting a reference of a superclass/parent type **back** into a specific subclass type, in order to access members that exist only in that subclass (and not in the parent).

### Syntax

```java
ChildClass ref = (ChildClass) parentRef;
```

### `instanceof`

Because downcasting can fail at runtime if the object isn't actually of the target subclass, it's essential to check the object's actual type first using the `instanceof` operator.

```java
if (parentRef instanceof ChildClass) {
    ChildClass ref = (ChildClass) parentRef;
    // safe to use ref
}
```

### Examples

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Bark"); }
    void fetch() { System.out.println("Dog fetches the ball"); } // Dog-specific method
}

public class DowncastingDemo {
    public static void main(String[] args) {
        Animal a = new Dog(); // upcasting

        // a.fetch(); // COMPILE ERROR: fetch() is not visible via Animal reference

        if (a instanceof Dog) {
            Dog d = (Dog) a; // downcasting
            d.fetch();       // now accessible: "Dog fetches the ball"
        }
    }
}
```

### Safe Casting

Attempting to downcast to an **incompatible type** (one the object isn't actually an instance of) throws a `ClassCastException` at runtime:

```java
Animal a = new Cat();
Dog d = (Dog) a; // ClassCastException — 'a' is actually a Cat, not a Dog
```

**Always check with `instanceof`** before downcasting to avoid this exception, ensuring the cast is safe before performing it.

---

## 13. Benefits of Polymorphism

- **Loose Coupling** — code that depends on a general type (or interface) is decoupled from specific implementations, making the system easier to modify and extend.
- **Flexibility** — the same code can work with any current or future subclass without modification.
- **Code Reusability** — common logic written once (operating on the parent type) automatically applies to all subclasses.
- **Maintainability** — behavior specific to each subtype lives within that subtype's own class, rather than scattered across conditional checks elsewhere.
- **Scalability** — new subclasses/behaviors can be added to a growing system with minimal changes to existing code.

---

## 14. Limitations

### Slight Runtime Overhead

Dynamic method dispatch requires the JVM to determine the correct method to call at runtime (via a lookup mechanism), which introduces a small performance cost compared to statically-resolved (compile-time) method calls — though this is negligible for the vast majority of applications.

### Difficult Debugging

Because the actual method executed depends on the runtime object (not the reference type visible in the code), tracing program flow — especially in deep inheritance hierarchies — can be harder to follow than straightforward, non-polymorphic code.

### Can Increase Complexity

Overuse of polymorphism, especially with deep or unclear inheritance hierarchies, can make a codebase harder to understand for newcomers, since behavior is distributed across many classes rather than being visible in one place.

---

## 15. Real-world Examples

### Animal Sounds

```java
class Animal { void makeSound() { System.out.println("..."); } }
class Dog extends Animal { void makeSound() { System.out.println("Bark"); } }
class Cat extends Animal { void makeSound() { System.out.println("Meow"); } }
```

### Payment System

```java
abstract class Payment {
    abstract void pay(double amount);
}
class CreditCardPayment extends Payment {
    void pay(double amount) { System.out.println("Paid $" + amount + " via Credit Card"); }
}
class UpiPayment extends Payment {
    void pay(double amount) { System.out.println("Paid $" + amount + " via UPI"); }
}
```

### Employee Management

```java
class Employee {
    double calculateSalary() { return 0; }
}
class Manager extends Employee {
    double calculateSalary() { return 80000; }
}
class Developer extends Employee {
    double calculateSalary() { return 65000; }
}
```

### Vehicle System

```java
class Vehicle {
    void startEngine() { System.out.println("Generic engine starting"); }
}
class Car extends Vehicle {
    void startEngine() { System.out.println("Car engine roars to life"); }
}
class ElectricScooter extends Vehicle {
    void startEngine() { System.out.println("Electric scooter powers on silently"); }
}
```

### Shape Calculator

```java
abstract class Shape {
    abstract double area();
}
class Circle extends Shape {
    double radius;
    Circle(double r) { radius = r; }
    double area() { return Math.PI * radius * radius; }
}
class Square extends Shape {
    double side;
    Square(double s) { side = s; }
    double area() { return side * side; }
}
```

### Hospital Management

```java
class Staff {
    void performDuty() { System.out.println("Performing general duty"); }
}
class Doctor extends Staff {
    void performDuty() { System.out.println("Diagnosing and treating patients"); }
}
class Nurse extends Staff {
    void performDuty() { System.out.println("Assisting patients and administering care"); }
}
```

---

## 16. Best Practices

1. **Program to interfaces** — declare reference variables using the most general applicable type (interface or abstract class) rather than a specific concrete class, to maximize flexibility.
2. **Avoid unnecessary casting** — excessive downcasting is often a sign that polymorphism isn't being leveraged effectively; reconsider the design if it's happening frequently.
3. **Use polymorphism instead of long if-else chains** — replace `if (obj instanceof TypeA) { ... } else if (obj instanceof TypeB) { ... }` patterns with proper method overriding wherever possible.
4. **Prefer overriding over type checking** — let each subclass define its own behavior via overridden methods, rather than checking types externally and branching manually.
5. Always use `@Override` annotation when overriding methods — it helps the compiler catch signature mismatches early.
6. Keep inheritance hierarchies shallow and well-organized to avoid excessive complexity from deep polymorphic chains.

---

## 17. Common Mistakes

### Confusing Overriding with Polymorphism

**Mistake:** Treating "overriding" and "polymorphism" as identical concepts.
**Clarification:** Overriding is the **mechanism** (providing a subclass-specific implementation); polymorphism is the **broader behavior/outcome** (the same method call producing different results based on the actual object) — overriding is one of the primary ways Java achieves runtime polymorphism, but polymorphism also includes compile-time forms (overloading).

### Assuming All Methods Are Polymorphic

**Mistake:** Believing every method call in Java is resolved dynamically.
**Clarification:** Only **overridden instance methods** exhibit runtime polymorphism. `static` methods, `private` methods, and fields are resolved based on the **reference type** at compile time (not the actual object) — this is sometimes called "no polymorphism for fields/static methods."

```java
class Animal {
    static void staticMethod() { System.out.println("Animal static"); }
}
class Dog extends Animal {
    static void staticMethod() { System.out.println("Dog static"); }
}

Animal a = new Dog();
a.staticMethod(); // "Animal static" — resolved by REFERENCE TYPE, not actual object!
```

### Incorrect Casting

**Mistake:** Downcasting without checking `instanceof` first, leading to a `ClassCastException` at runtime.

```java
Animal a = new Cat();
Dog d = (Dog) a; // ClassCastException
```

### Forgetting Inheritance Requirement

**Mistake:** Attempting to achieve runtime polymorphism between unrelated classes (no inheritance relationship). Runtime polymorphism via overriding **requires** the classes to be connected through an inheritance (or interface implementation) hierarchy — it cannot occur between two entirely unrelated classes.

---

## 18. Polymorphism vs Method Overriding

### Comparison Table

| Aspect                | Polymorphism                                                                      | Method Overriding                                                                      |
| --------------------- | --------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Definition            | A broad OOP concept — "many forms" of behavior for the same interface/method call | A specific technique — subclass redefining a superclass method with the same signature |
| Scope                 | Includes both compile-time (overloading) and runtime (overriding) forms           | Only applies to runtime behavior                                                       |
| Relationship          | Overriding is one mechanism used to achieve polymorphism                          | Overriding is a means to an end (polymorphism is the end result)                       |
| Requires Inheritance? | Not always (overloading doesn't require inheritance)                              | Yes, always requires inheritance/interface implementation                              |
| Resolution Time       | Can be compile-time or runtime, depending on type                                 | Always resolved at runtime                                                             |

---

## 19. Compile-Time vs Runtime Polymorphism

### Comparison Table

| Aspect                | Compile-Time Polymorphism                   | Runtime Polymorphism                                             |
| --------------------- | ------------------------------------------- | ---------------------------------------------------------------- |
| Also Known As         | Static polymorphism, early binding          | Dynamic polymorphism, late binding                               |
| Achieved Via          | Method overloading, constructor overloading | Method overriding                                                |
| Resolved By           | Compiler, based on method signature         | JVM, based on actual object at runtime                           |
| Requires Inheritance? | No                                          | Yes                                                              |
| Performance           | Slightly faster (resolved before execution) | Slightly slower (resolved during execution via dynamic dispatch) |
| Flexibility           | Limited to variations within the same class | Highly flexible — supports extensible, hierarchy-based behavior  |
| Example               | `add(int, int)` vs `add(double, double)`    | `Animal a = new Dog(); a.makeSound();`                           |

---

## 20. Interview Corner

**Q1. What is polymorphism?**
Polymorphism is the ability of a single method name, interface, or reference to exhibit different behaviors depending on the actual object involved — literally meaning "many forms."

**Q2. What are the types of polymorphism in Java?**
**Compile-time polymorphism** (achieved via method/constructor overloading, resolved by the compiler) and **runtime polymorphism** (achieved via method overriding, resolved by the JVM at runtime using dynamic method dispatch).

**Q3. What is the difference between compile-time and runtime polymorphism?**
Compile-time polymorphism is resolved by the compiler based on method signatures (overloading) and doesn't require inheritance. Runtime polymorphism is resolved by the JVM based on the actual object at runtime (overriding), and always requires an inheritance/interface relationship.

**Q4. How does Java achieve runtime polymorphism?**
Through **dynamic method dispatch** — when an overridden method is called via a superclass reference pointing to a subclass object, the JVM looks up and executes the method implementation belonging to the actual (runtime) object, not the declared reference type.

**Q5. What is upcasting?**
Upcasting is assigning a subclass object to a reference variable of its superclass (or implemented interface) type — done implicitly in Java, since a subclass object is always a valid instance of its superclass.

**Q6. What is downcasting?**
Downcasting is converting a superclass reference back to a specific subclass type, typically to access subclass-specific members — it must be done explicitly and should be guarded with an `instanceof` check to avoid a `ClassCastException`.

**Q7. Difference between overriding and polymorphism?**
Overriding is the specific language mechanism (a subclass providing its own implementation of an inherited method); polymorphism is the broader resulting behavior (the same method call producing different, context-appropriate results) — overriding is one of the tools used to achieve polymorphism, but polymorphism itself is a wider concept that also includes overloading.

---

## 21. MCQs

**1. What does "polymorphism" literally mean?**
a) Single form b) Many forms c) No form d) Fixed form

> **Answer: b)**

**2. Which type of polymorphism is resolved at compile time?**
a) Runtime polymorphism b) Compile-time polymorphism c) Both equally d) Neither

> **Answer: b)**

**3. Method overloading is an example of:**
a) Runtime polymorphism b) Compile-time polymorphism c) Inheritance d) Encapsulation

> **Answer: b)**

**4. Method overriding is an example of:**
a) Compile-time polymorphism b) Runtime polymorphism c) Abstraction only d) Constructor chaining

> **Answer: b)**

**5. Runtime polymorphism in Java requires:**
a) Overloading b) Inheritance c) Static methods d) Multiple classes with no relationship

> **Answer: b)**

**6. What determines which overridden method runs at runtime?**
a) Reference type b) Actual object type c) Compiler decision d) Method name only

> **Answer: b)**

**7. What is upcasting?**
a) Subclass object assigned to a subclass reference b) Subclass object assigned to a superclass reference c) Superclass object assigned to a subclass reference d) None of these

> **Answer: b)**

**8. What exception can occur during incorrect downcasting?**
a) NullPointerException b) ClassCastException c) ArrayIndexOutOfBoundsException d) ArithmeticException

> **Answer: b)**

**9. Which operator is used to safely check an object's type before downcasting?**
a) `typeof` b) `instanceof` c) `checktype` d) `is`

> **Answer: b)**

**10. Is upcasting done implicitly or explicitly in Java?**
a) Explicitly, always b) Implicitly c) Never allowed d) Only with casting operators

> **Answer: b)**

**11. Which of these is NOT resolved using dynamic method dispatch?**
a) Overridden instance methods b) Static methods c) Both a and b d) None of these

> **Answer: b) (static methods are resolved by reference type at compile time)**

**12. What is the mechanism Java uses to achieve runtime polymorphism called?**
a) Static binding b) Dynamic method dispatch c) Constructor chaining d) Type inference

> **Answer: b)**

**13. Which of the following is required for method overriding?**
a) Same method name and parameters, but can differ in class relationship b) Inheritance relationship between classes c) Only interfaces d) Static methods only

> **Answer: b)**

**14. Overloaded methods differ in:**
a) Method name b) Return type only c) Number, type, or order of parameters d) Access modifier only

> **Answer: c)**

**15. Which type of polymorphism provides better flexibility for extensible systems?**
a) Compile-time polymorphism b) Runtime polymorphism c) Both equally d) Neither

> **Answer: b)**

**16. What happens if you don't use `@Override` when overriding a method?**
a) Compile error always b) The method still overrides correctly if the signature matches, but you lose compiler safety checks c) The program crashes d) Overriding becomes impossible

> **Answer: b)**

**17. Which keyword allows a class to acquire another class's properties, forming the basis for runtime polymorphism?**
a) `implements` b) `extends` c) `import` d) `static`

> **Answer: b)**

**18. Can polymorphism occur without any inheritance at all?**
a) No, never b) Yes, via method/constructor overloading (compile-time polymorphism) c) Only with interfaces d) Only with abstract classes

> **Answer: b)**

**19. What is the relationship between a subclass and superclass called?**
a) "has-a" b) "is-a" c) "uses-a" d) "contains-a"

> **Answer: b)**

**20. Which of these best describes "loose coupling" as a benefit of polymorphism?**
a) Classes become tightly interdependent b) Code depends on general types/interfaces rather than specific implementations c) All classes must be in the same file d) Polymorphism eliminates the need for classes

> **Answer: b)**

---

## 22. University Questions

### Short Questions

1. Define polymorphism with an example.
2. What is the difference between compile-time and runtime polymorphism?
3. What is upcasting? Give an example.
4. What is downcasting, and why is `instanceof` used before it?
5. State two benefits of polymorphism.

### Long Questions

1. Explain polymorphism in Java. Discuss its types with suitable examples. _(10 marks)_
2. Differentiate between method overloading and method overriding with example programs. _(10 marks)_
3. Explain how Java achieves runtime polymorphism using dynamic method dispatch, with a program and flow diagram. _(10 marks)_
4. Explain upcasting and downcasting with syntax, examples, and the role of `instanceof` in safe casting. _(10 marks)_
5. Design a real-world example (e.g., Payment System or Shape Calculator) demonstrating polymorphism using inheritance and method overriding. _(10 marks)_
6. Discuss the benefits and limitations of polymorphism in software design. _(5 marks)_

---

## 23. Viva Questions

1. What is polymorphism, in one line?
2. What are the two types of polymorphism in Java?
3. Which type of polymorphism does method overloading represent?
4. Which type of polymorphism does method overriding represent?
5. What is dynamic method dispatch?
6. What is the difference between a reference variable's type and an object's actual type?
7. What is upcasting? Is it done implicitly or explicitly?
8. What is downcasting? Why is it potentially risky?
9. What exception occurs during an invalid downcast?
10. Does runtime polymorphism require inheritance? Why?
11. Are static methods polymorphic in Java? Why or why not?
12. What does "program to an interface" mean in the context of polymorphism?
13. Give a real-world analogy for polymorphism.
14. What is the relationship between overriding and polymorphism?
15. Name one limitation of using polymorphism extensively in a program.

---

## 24. Programming Exercises

### 1. Animal Example

Create an `Animal` superclass with a `makeSound()` method, and subclasses `Dog`, `Cat`, and `Cow`, each overriding `makeSound()`. Store them in an array of type `Animal` and call `makeSound()` on each using a loop.

### 2. Vehicle Example

Create a `Vehicle` superclass with a `startEngine()` method, and subclasses `Car`, `Bike`, and `Truck`, each providing a specific engine-start message. Demonstrate polymorphism by calling `startEngine()` through `Vehicle` references.

### 3. Employee Example

Create an `Employee` superclass with a `calculateSalary()` method, and subclasses `Manager`, `Developer`, and `Intern`, each returning a different salary calculation. Compute and print the total payroll for a list of mixed employee types.

### 4. Payment System

Create an abstract `Payment` class with an abstract `pay(double amount)` method, and subclasses `CreditCardPayment`, `UpiPayment`, and `CashPayment`. Process a list of different payment types using polymorphism.

### 5. Shape Calculator

Create an abstract `Shape` class with an abstract `area()` method, and subclasses `Circle`, `Rectangle`, and `Triangle`. Compute and print the total area of a mixed collection of shapes using a single polymorphic loop.

### Sample Solution — Animal Example

```java
class Animal {
    void makeSound() { System.out.println("Some generic sound"); }
}
class Dog extends Animal {
    @Override void makeSound() { System.out.println("Bark"); }
}
class Cat extends Animal {
    @Override void makeSound() { System.out.println("Meow"); }
}
class Cow extends Animal {
    @Override void makeSound() { System.out.println("Moo"); }
}

public class AnimalSoundDemo {
    public static void main(String[] args) {
        Animal[] animals = { new Dog(), new Cat(), new Cow() };
        for (Animal a : animals) {
            a.makeSound();
        }
    }
}
```

---

## 25. Challenge Problems

Design polymorphic systems for the following domains — each should use an abstract class or interface as the common type, with multiple subclasses overriding shared behavior:

### Banking

Design an `Account` hierarchy (`SavingsAccount`, `CurrentAccount`, `FixedDepositAccount`) with a polymorphic `calculateInterest()` method, and process a mixed list of accounts to compute total interest paid across the bank.

### Hospital

Design a `Staff` hierarchy (`Doctor`, `Nurse`, `Receptionist`) with a polymorphic `performDuty()` method, and simulate a daily hospital roster calling each staff member's specific duty.

### Library

Design a `LibraryItem` hierarchy (`Book`, `Magazine`, `DVD`) with a polymorphic `getLoanPeriod()` method (returning different loan durations), and process checkouts for a mixed collection of items.

### Online Shopping

Design a `Product` hierarchy (`Electronics`, `Clothing`, `Groceries`) with a polymorphic `calculateDiscount()` method, and compute the total discounted price for a shopping cart containing mixed product types.

### Payroll

Design an `Employee` hierarchy (`FullTimeEmployee`, `PartTimeEmployee`, `Contractor`) with a polymorphic `generatePayslip()` method, and print payslips for an entire mixed workforce using a single polymorphic loop.

---

## 26. Summary

- **Polymorphism** means "many forms" — the ability of a single interface, method call, or reference to produce different behavior depending on the actual object involved.
- Java supports two types: **compile-time polymorphism** (method/constructor overloading, resolved by the compiler) and **runtime polymorphism** (method overriding, resolved by the JVM via dynamic method dispatch).
- Runtime polymorphism **requires inheritance** — a subclass must override a method inherited from its superclass.
- **Upcasting** (subclass → superclass reference) happens implicitly and enables polymorphic behavior; **downcasting** (superclass → subclass reference) must be done explicitly and safely, typically guarded with `instanceof`.
- Polymorphism improves **flexibility, code reusability, extensibility, and maintainability**, though it introduces a slight runtime overhead and can increase debugging complexity if overused or poorly structured.
- Real-world systems — payment processing, employee management, shape calculators, hospital staff management — commonly rely on polymorphism to write clean, extensible code.

---

## 27. Key Takeaways

- Polymorphism means "many forms."
- Java supports compile-time and runtime polymorphism.
- Method overloading provides compile-time polymorphism.
- Method overriding provides runtime polymorphism.
- Runtime polymorphism requires inheritance.
- Polymorphism improves flexibility and maintainability.

---

## 28. Cheat Sheet

### Definition

> Polymorphism = one interface/method call, many forms of behavior, depending on the actual object.

### Types

```java
// Compile-Time Polymorphism (Overloading)
int add(int a, int b) { return a + b; }
double add(double a, double b) { return a + b; }

// Runtime Polymorphism (Overriding)
class Animal { void makeSound() { System.out.println("..."); } }
class Dog extends Animal {
    @Override void makeSound() { System.out.println("Bark"); }
}
Animal a = new Dog();
a.makeSound(); // "Bark" — resolved at runtime
```

### Advantages

- Flexibility, code reusability, extensibility, maintainability, loose coupling.

### Comparison Tables

| Compile-Time Polymorphism      | Runtime Polymorphism       |
| ------------------------------ | -------------------------- |
| Method/constructor overloading | Method overriding          |
| Resolved by compiler           | Resolved by JVM at runtime |
| No inheritance required        | Inheritance required       |

| Upcasting                       | Downcasting                     |
| ------------------------------- | ------------------------------- |
| Subclass → Superclass reference | Superclass → Subclass reference |
| Implicit                        | Explicit (requires cast)        |
| Always safe                     | Risky — use `instanceof` first  |

### Best Practices

- Program to interfaces/abstract types.
- Avoid unnecessary/unsafe casting.
- Replace `instanceof` chains with proper overriding.
- Always use `@Override`.

### Common Interview Questions

1. What is polymorphism?
2. Types of polymorphism?
3. Difference between compile-time and runtime polymorphism?
4. How does Java achieve runtime polymorphism?
5. What is upcasting?
6. What is downcasting?
7. Difference between overriding and polymorphism?

---

_End of Chapter — Polymorphism in Java_
