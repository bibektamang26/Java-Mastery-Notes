# 04. Types of Inheritance in Java

---

## 1. Introduction

- **What are Types of Inheritance?** Inheritance can be organized into several different **structural patterns**, based on how classes relate to one another within an inheritance hierarchy — a single parent with one child, a chain of successive parent-child relationships, one parent with multiple children, or a class attempting to inherit from multiple parents at once.
- **Why Study Different Types?** Recognizing these patterns helps a programmer choose the right class hierarchy for a given problem, anticipate potential design pitfalls (like the diamond problem), and understand precisely which patterns Java supports directly through classes versus which require interfaces instead.
- **Importance in Software Design:** Poorly structured inheritance hierarchies — too deep, too tangled, or attempting unsupported patterns — lead to fragile, hard-to-maintain code. Understanding the different types of inheritance equips a programmer to design cleaner, more predictable class relationships.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Identify different types of inheritance.
- Explain each inheritance type with examples.
- Understand which inheritance types Java supports.
- Explain why Java does not support multiple inheritance with classes.

---

## 3. Quick Revision

_(Brief review only)_

- **What is Inheritance?** A mechanism allowing a child class to acquire the fields and methods of a parent class.
- **Parent Class:** The existing class being inherited from.
- **Child Class:** The new class that inherits from the parent.
- **`extends` Keyword:** The keyword used in Java to establish a class inheritance relationship.
- **IS-A Relationship:** The relationship inheritance models — the child class is a more specific kind of the parent class (e.g., a Dog IS-A(n) Animal).

---

## 4. What are Types of Inheritance?

**Definition:** The "types of inheritance" refer to the different possible **structural arrangements** of parent-child relationships between classes — describing not _what_ inheritance is, but _how_ multiple classes can be connected to one another through it.

**Need for different inheritance structures:** Real-world relationships between entities aren't always a simple one-parent-one-child pair — sometimes many child classes share a single parent (like several vehicle types sharing a `Vehicle` parent), sometimes inheritance forms a chain across several levels (like `Animal → Mammal → Dog`), and understanding these different structures helps model such relationships accurately.

**Relationship hierarchy:** Each type of inheritance describes a distinct **shape** of the class hierarchy diagram — a straight line, a branching tree, or (in the case of multiple/hybrid inheritance) a converging or combined structure.

```
Types of Inheritance
│
├── Single Inheritance
├── Multilevel Inheritance
├── Hierarchical Inheritance
├── Multiple Inheritance
└── Hybrid Inheritance
```

---

## 5. Single Inheritance

**Definition:** Single inheritance involves exactly **one** child class inheriting from exactly **one** parent class — the simplest and most straightforward inheritance structure.

**Characteristics:**

- Exactly one parent, exactly one child.
- The most basic and most commonly used inheritance pattern.
- Fully and directly supported by Java.

**Syntax:**

```java
class Parent {
}

class Child extends Parent {
}
```

**Example:**

```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("The dog barks.");
    }
}
```

**Diagram:**

```
   Animal
      │
      ▼
    Dog
```

**Advantages:**

- Simple and easy to understand.
- Minimizes complexity — there's only one clear line of inheritance to trace.
- Well-suited for straightforward IS-A relationships.

**Memory Diagram:**

```
Heap:
┌───────────────────────┐
│      Dog Object         │
│  ┌───────────────────┐  │
│  │ Inherited: (Animal │  │
│  │  fields/methods)   │  │
│  └───────────────────┘  │
│  ┌───────────────────┐  │
│  │ Own: bark()         │  │
│  └───────────────────┘  │
└───────────────────────┘
```

---

## 6. Multilevel Inheritance

**Definition:** Multilevel inheritance involves a **chain** of inheritance, where a class inherits from a parent class, which itself inherits from another parent class, forming multiple successive levels.

**Characteristics:**

- Forms a straight-line chain: `A → B → C`.
- Each class in the chain inherits everything from all classes above it in the hierarchy.
- Fully and directly supported by Java.

**Syntax:**

```java
class A {
}

class B extends A {
}

class C extends B {
}
```

**Example:**

```java
class Animal {
    void eat() {
        System.out.println("This animal eats.");
    }
}

class Mammal extends Animal {
    void walk() {
        System.out.println("This mammal walks.");
    }
}

class Dog extends Mammal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.eat();    // inherited from Animal
        d.walk();   // inherited from Mammal
        d.bark();   // Dog's own method
    }
}
```

**Diagram:**

```
   Animal
      │
      ▼
   Mammal
      │
      ▼
    Dog
```

**Advantages:**

- Allows progressively more specialized classes to be built in stages.
- Promotes reuse across multiple levels, not just a single parent-child pair.

**Memory Diagram:**

```
Heap:
┌────────────────────────────┐
│         Dog Object            │
│  ┌──────────────────────┐    │
│  │ Inherited from Animal:│    │
│  │  eat()                 │    │
│  └──────────────────────┘    │
│  ┌──────────────────────┐    │
│  │ Inherited from Mammal:│    │
│  │  walk()                │    │
│  └──────────────────────┘    │
│  ┌──────────────────────┐    │
│  │ Own: bark()             │    │
│  └──────────────────────┘    │
└────────────────────────────┘
```

---

## 7. Hierarchical Inheritance

**Definition:** Hierarchical inheritance involves **multiple** child classes all inheriting from a **single common parent class**.

**Characteristics:**

- One parent, multiple independent children.
- Each child class inherits the same shared members from the parent, but the children are otherwise unrelated to each other.
- Fully and directly supported by Java.

**Syntax:**

```java
class Parent {
}

class ChildA extends Parent {
}

class ChildB extends Parent {
}
```

**Example:**

```java
class Vehicle {
    void start() {
        System.out.println("Vehicle starting...");
    }
}

class Car extends Vehicle {
    void openTrunk() {
        System.out.println("Trunk opened.");
    }
}

class Bike extends Vehicle {
    void kickStart() {
        System.out.println("Bike kick-started.");
    }
}
```

**Diagram:**

```
            Vehicle
           /        \
        Car          Bike
```

**Advantages:**

- Avoids duplicating shared logic across multiple, otherwise-unrelated child classes.
- Naturally models "categories" of related but distinct entities (e.g., multiple vehicle types, multiple employee types).

**Memory Diagram:**

```
Heap (separate objects, each independently inheriting from Vehicle):

┌─────────────────────┐        ┌─────────────────────┐
│      Car Object        │        │      Bike Object       │
│  Inherited: start()     │        │  Inherited: start()     │
│  Own: openTrunk()        │        │  Own: kickStart()        │
└─────────────────────┘        └─────────────────────┘
```

---

## 8. Multiple Inheritance

**Definition:** Multiple inheritance refers to a single class inheriting directly from **more than one** parent class simultaneously.

**Characteristics:**

- One child class, multiple parent classes.
- Combines fields and methods from all parent classes into a single child class.
- **Not supported by Java directly through classes** — only achievable through interfaces.

**Real-world Example:** A `SmartWatch` might conceptually need to inherit behavior from both a `Watch` class and a `Phone` class — displaying time like a watch, while also making calls like a phone. This is a natural candidate for multiple inheritance in concept, even though Java restricts how it can actually be implemented.

**Diamond Problem:** _(introduced here, explained in full detail in Section 12)_ If two parent classes both define a method with the same signature, and a child class inherits from both, it becomes ambiguous which version of the method the child should inherit — this ambiguity is known as the diamond problem.

**Why Java does NOT support it using classes:** Java deliberately disallows a class from extending more than one class specifically to avoid the diamond problem and the ambiguity it creates — if `ClassC extends ClassA, ClassB` were allowed, and both `ClassA` and `ClassB` defined a conflicting method, the compiler (and the programmer) would have no clear way to determine which version `ClassC` should use.

```java
// class SmartWatch extends Watch, Phone { }   // Compile Error: Java does not allow this
```

**How Java supports it using interfaces:** Since interfaces (prior to default methods) traditionally only declare method signatures without providing implementations, a class can safely `implement` multiple interfaces without the same ambiguity — any conflicting default method implementations must be explicitly resolved by the implementing class, removing the ambiguity that classes would otherwise create.

```java
interface Watch {
    void showTime();
}

interface Phone {
    void makeCall();
}

class SmartWatch implements Watch, Phone {
    public void showTime() {
        System.out.println("Displaying time...");
    }
    public void makeCall() {
        System.out.println("Making a call...");
    }
}
```

**Memory Diagram:**

```
Interfaces (contracts only, no shared state ambiguity):

  Watch (interface)      Phone (interface)
        \                    /
         \                  /
          SmartWatch (implements both)
              │
              ▼
      SmartWatch Object
      (must provide its own implementation for
       every method from both interfaces)
```

---

## 9. Hybrid Inheritance

**Definition:** Hybrid inheritance is any **combination** of two or more inheritance types (e.g., combining hierarchical and multiple inheritance) within a single class hierarchy.

**Combination of inheritance types:** For example, a hierarchy might have hierarchical inheritance (`Animal → Dog`, `Animal → Cat`) combined with an attempt at multiple inheritance (a class trying to inherit from both `Dog` and `Cat`) — this combined structure is what's referred to as hybrid inheritance.

**Why Java does not support it directly:** Since hybrid inheritance typically involves multiple inheritance as one of its components, and Java classes cannot perform multiple inheritance directly (for the diamond problem reasons discussed above), Java cannot support hybrid inheritance directly through classes either.

**Hybrid using interfaces:** Just as with pure multiple inheritance, hybrid inheritance patterns **can** be achieved safely in Java by using a combination of class-based single/multilevel/hierarchical inheritance **together with** interfaces for any part of the design that would otherwise require multiple inheritance.

```java
class Animal {
    void eat() { System.out.println("Eating..."); }
}

interface Swimmer {
    void swim();
}

interface Runner {
    void run();
}

// Hybrid: class-based inheritance (Animal) + multiple interface implementation
class Dog extends Animal implements Runner {
    public void run() { System.out.println("Dog runs."); }
}

class Duck extends Animal implements Swimmer, Runner {
    public void swim() { System.out.println("Duck swims."); }
    public void run() { System.out.println("Duck runs."); }
}
```

**Diagram:**

```
                Animal
               /      \
            Dog        Duck ── implements ── Swimmer
              │           │
       implements     implements
              │           │
            Runner      Runner

(Dog: hierarchical inheritance from Animal + one interface
 Duck: hierarchical inheritance from Animal + multiple interfaces — a hybrid structure)
```

---

## 10. Comparison of All Types

| Type         | Parents             | Children            | Java Support (Classes)     | Java Support (Interfaces)   |
| ------------ | ------------------- | ------------------- | -------------------------- | --------------------------- |
| Single       | 1                   | 1                   | ✔ Full support             | ✔ N/A (already single)      |
| Multilevel   | Chain (1 per level) | Chain (1 per level) | ✔ Full support             | ✔ Fully supported           |
| Hierarchical | 1                   | Many                | ✔ Full support             | ✔ Fully supported           |
| Multiple     | Many                | 1                   | ✘ Not supported            | ✔ Fully supported           |
| Hybrid       | Combination         | Combination         | ✘ Not supported (directly) | ✔ Achievable via interfaces |

---

## 11. Java Supported vs Unsupported Types

**Supported (directly through classes):**

- ✔ Single
- ✔ Multilevel
- ✔ Hierarchical

**Supported through Interfaces:**

- ✔ Multiple
- ✔ Hybrid

**Not Supported through Classes:**

- ✘ Multiple
- ✘ Hybrid

This distinction is one of the most frequently tested concepts in Java exams and interviews: Java classes support only single, multilevel, and hierarchical inheritance directly; multiple and hybrid inheritance are only achievable by using interfaces instead of (or alongside) class-based inheritance.

---

## 12. Diamond Problem

**Definition:** The diamond problem is a classic ambiguity that arises in multiple inheritance, when a class inherits from two parent classes that both define a method with the same signature — the compiler cannot determine which version of the method the child class should inherit.

**Why ambiguity occurs:** If both parent classes provide their own distinct implementation of the same method, and the child class doesn't specify which one to use, there is no unambiguous way to decide — inheriting both would create a naming conflict, and arbitrarily picking one would be unpredictable and error-prone.

**Example (conceptual — not valid Java, since classes can't do this):**

```java
// Conceptual illustration — NOT valid Java syntax
class A {
    void greet() { System.out.println("Hello from A"); }
}
class B {
    void greet() { System.out.println("Hello from B"); }
}
// class C extends A, B { }   // If this were allowed: which greet() does C inherit? Ambiguous!
```

**Diagram (the "diamond" shape that gives the problem its name):**

```
        A
       / \
      B   C
       \ /
        D

(D inherits from both B and C, which both inherit from A —
 if B and C both override a method from A differently,
 D faces ambiguity about which version to use)
```

**How interfaces solve it:** Java interfaces avoid this ambiguity by requiring the **implementing class** to explicitly provide its own implementation of any method — even if two interfaces both declare (or, since Java 8, both provide `default` implementations for) a method with the same signature, the implementing class **must** resolve any conflict itself by overriding the method explicitly, removing all ambiguity.

```java
interface X {
    default void greet() { System.out.println("Hello from X"); }
}
interface Y {
    default void greet() { System.out.println("Hello from Y"); }
}

class Z implements X, Y {
    // Compile Error if greet() is not overridden here — Java forces explicit resolution
    @Override
    public void greet() {
        X.super.greet();   // explicitly choosing X's version (or write custom logic)
    }
}
```

This explicit resolution requirement is precisely why Java can safely allow multiple interface implementation, while still disallowing multiple class inheritance.

---

## 13. Real-world Examples

**Animal (Hierarchical):**

```
        Animal
       /   |   \
     Dog  Cat  Bird
```

**Vehicle (Multilevel):**

```
Vehicle → MotorVehicle → Car
```

**Employee (Hierarchical):**

```
        Employee
       /        \
   Manager    Developer
```

**Bank (Multilevel + Hierarchical combined — hybrid via interfaces):**

```
Account → SavingsAccount → StudentSavingsAccount
Account → CurrentAccount
(with an interface like InterestBearing implemented where applicable)
```

**School (Hierarchical):**

```
         Person
       /        \
   Teacher     Student
```

**Hospital (Multiple via Interfaces):**

```
interface Payable { }
interface Schedulable { }
class Doctor implements Payable, Schedulable { }
```

---

## 14. Advantages of Different Inheritance Types

- **Single Inheritance:** Simplicity and clarity — easiest to understand and trace.
- **Multilevel Inheritance:** Enables progressive specialization across multiple stages, reusing logic at every level.
- **Hierarchical Inheritance:** Efficiently shares common logic across many related but distinct child classes.
- **Multiple Inheritance (via interfaces):** Allows a class to conform to multiple independent behavioral contracts at once, offering great design flexibility.
- **Hybrid Inheritance (via interfaces):** Combines the benefits of class-based reuse with interface-based flexible multiple-contract support, modeling complex real-world relationships accurately.

---

## 15. Disadvantages

- **Multilevel Inheritance:** Deep chains can make it hard to trace exactly where a given field or method originates.
- **Hierarchical Inheritance:** Changes to the shared parent class can unexpectedly affect _all_ child classes at once.
- **Multiple/Hybrid Inheritance (via interfaces):** Can require careful, explicit conflict resolution when multiple interfaces declare overlapping default methods, adding some design complexity.
- **General risk across all types:** Overusing or misusing any inheritance type — especially where no genuine IS-A relationship exists — leads to fragile, tightly-coupled designs (see Chapter 02: Inheritance for the broader discussion of inheritance's general disadvantages).

---

## 16. Best Practices

- Use inheritance only when there is a true **IS-A** relationship.
- Prefer **composition** when appropriate, rather than forcing an inheritance relationship that doesn't naturally fit.
- Avoid **deep** inheritance hierarchies — excessive multilevel chains reduce readability and maintainability.
- Keep inheritance **simple** — choose the least complex inheritance type that adequately models the relationship at hand.

---

## 17. Common Mistakes

- **Confusing multiple and multilevel inheritance:** Multilevel is a _chain_ (`A → B → C`), while multiple involves a single class inheriting from _more than one direct parent_ (`C` inheriting from both `A` and `B` simultaneously) — these are fundamentally different structures, frequently mixed up by students.
- **Using inheritance instead of composition:** Forcing an inheritance relationship where a HAS-A (composition) relationship would be more appropriate.
- **Assuming Java supports multiple inheritance with classes:** A very common misconception — Java explicitly disallows a class from extending more than one class, specifically to avoid the diamond problem.
- **Creating unnecessary inheritance chains:** Adding extra levels of inheritance "just in case," without a genuine need, which only adds unnecessary complexity to the class hierarchy.

---

## 18. Interview Corner

**Q1. What are the types of inheritance?**
Single, Multilevel, Hierarchical, Multiple, and Hybrid inheritance.

**Q2. Which inheritance types does Java support?**
Java directly supports Single, Multilevel, and Hierarchical inheritance through classes. Multiple and Hybrid inheritance are only achievable through interfaces, not directly through classes.

**Q3. Why doesn't Java support multiple inheritance?**
To avoid the diamond problem — the ambiguity that arises when a class inherits conflicting method implementations from two or more parent classes with no clear way to resolve which version should be used.

**Q4. What is the Diamond Problem?**
It's the ambiguity that occurs when a class would inherit the same method signature from two different parent classes (typically visualized as a diamond-shaped class hierarchy), leaving it unclear which parent's implementation should be used.

**Q5. Can Java achieve multiple inheritance?**
Yes — indirectly, through interfaces. A class can implement multiple interfaces, and Java requires the implementing class to explicitly resolve any conflicting default method implementations, which avoids the ambiguity that classes would otherwise create.

**Q6. Difference between multiple and multilevel inheritance?**
Multiple inheritance involves a single class inheriting from more than one direct parent class at the same level; multilevel inheritance involves a chain of single-parent relationships stacked across multiple successive levels (`A → B → C`).

---

## 19. MCQs

**1. Which inheritance type involves a single chain of parent-child relationships across multiple levels?**
a) Hierarchical
b) Multilevel
c) Multiple
d) Hybrid
**Answer: b) Multilevel**

**2. Which inheritance type involves multiple child classes inheriting from one common parent?**
a) Single
b) Multilevel
c) Hierarchical
d) Multiple
**Answer: c) Hierarchical**

**3. Does Java support multiple inheritance directly through classes?**
a) Yes, fully
b) No, only through interfaces
c) Only with abstract classes
d) Only in nested classes
**Answer: b) No, only through interfaces**

**4. What problem does Java avoid by disallowing multiple class inheritance?**
a) Stack overflow
b) Diamond problem
c) Memory leak
d) Infinite loop
**Answer: b) Diamond problem**

**5. Which of the following is an example of hybrid inheritance?**
a) A single class extending one parent
b) A chain of three classes, each extending the previous
c) A combination of hierarchical inheritance and multiple interface implementation
d) A class with no parent
**Answer: c) A combination of hierarchical inheritance and multiple interface implementation**

**6. In the diamond problem, why is ambiguity created?**
a) Too many child classes
b) Two parent classes provide conflicting implementations of the same method
c) The child class has no methods
d) The parent class is abstract
**Answer: b) Two parent classes provide conflicting implementations of the same method**

**7. Which inheritance type is the simplest and most basic?**
a) Hybrid
b) Multiple
c) Single
d) Hierarchical
**Answer: c) Single**

**8. How does Java resolve conflicting default methods from multiple interfaces?**
a) It picks one automatically
b) It requires the implementing class to explicitly override and resolve the conflict
c) It throws a runtime exception
d) It merges both implementations automatically
**Answer: b) It requires the implementing class to explicitly override and resolve the conflict**

**9. Which of these is NOT directly supported by Java through classes?**
a) Single inheritance
b) Multilevel inheritance
c) Multiple inheritance
d) Hierarchical inheritance
**Answer: c) Multiple inheritance**

**10. What shape is commonly used to describe the multiple inheritance ambiguity problem?**
a) Triangle
b) Diamond
c) Circle
d) Square
**Answer: b) Diamond**

---

## 20. University Questions

### Short Questions

1. What is single inheritance? Give an example.
2. What is the difference between multilevel and hierarchical inheritance?
3. Why doesn't Java support multiple inheritance through classes?
4. What is the diamond problem?
5. How can multiple inheritance be achieved in Java?

### Long Questions

1. Explain all five types of inheritance in Java with diagrams and examples for each.
2. Discuss the diamond problem in detail, explaining why it occurs and how Java's interface-based approach avoids it.
3. Compare single, multilevel, hierarchical, multiple, and hybrid inheritance with a complete comparison table.
4. Explain, with a program example, how Java achieves multiple inheritance-like behavior using interfaces despite not supporting it through classes.
5. Design and explain a hybrid inheritance hierarchy for a real-world system (e.g., a hospital or banking system), using a combination of class inheritance and interfaces.

---

## 21. Viva Questions

1. What is the key structural difference between multilevel and hierarchical inheritance?
2. Why is multiple inheritance risky if implemented through classes?
3. Can a Java interface itself extend multiple other interfaces? How does this differ from class inheritance rules?
4. What is a default method, and how does it relate to the diamond problem in interfaces?
5. Give a real-world example of hierarchical inheritance.
6. Why is hybrid inheritance considered a "combination" type rather than a distinct structure of its own?
7. What must an implementing class do if two interfaces it implements both declare a conflicting default method?
8. Is single inheritance a special case of multilevel inheritance? Explain.
9. What kind of relationship (IS-A or HAS-A) does multiple inheritance via interfaces typically represent?
10. Why is understanding inheritance types important for exam and interview preparation specifically?

---

## 22. Programming Exercises

**1. Single Inheritance**

Create a `Person` class and a `Student` class that extends it, adding a `course` field. Create a `Student` object and print all details.

**2. Multilevel Inheritance**

Create a three-level hierarchy: `Animal → Mammal → Dog`, each level adding one new method, and demonstrate that a `Dog` object can access methods from all three levels.

**3. Hierarchical Inheritance**

Create a `Shape` parent class with a `name` field, and two child classes `Circle` and `Square`, each with their own unique field, demonstrating that both inherit the shared `name` field independently.

**4. Multiple Inheritance using Interfaces**

Create two interfaces, `Flyable` and `Swimmable`, and a `Duck` class that implements both, providing its own implementation for each interface method.

**5. Hybrid Inheritance using Interfaces**

Create a class hierarchy combining hierarchical class inheritance (e.g., `Animal → Dog`, `Animal → Duck`) with multiple interface implementation (e.g., `Duck` also implementing `Swimmable` and `Flyable`), and demonstrate the combined structure with a working program.

---

## 23. Challenge Problems

**Design inheritance hierarchies for:**

- **Library System:** Design a hierarchy involving `LibraryItem`, with child classes `Book` and `DVD` (hierarchical), and an interface `Reservable` implemented where appropriate (hybrid).
- **Banking System:** Design `Account → SavingsAccount → StudentSavingsAccount` (multilevel), combined with an interface `InterestBearing` (hybrid).
- **Hospital Management:** Design a `Person → Staff → Doctor`/`Nurse` hierarchy (multilevel + hierarchical), combined with interfaces like `Schedulable` and `Payable` (multiple, via interfaces).
- **Vehicle Management:** Design a `Vehicle → LandVehicle → Car`/`Bike` hierarchy (multilevel + hierarchical), combined with an interface `FuelEfficient` where applicable.
- **Employee Management:** Design an `Employee → Manager`/`Developer` hierarchy (hierarchical), combined with an interface `Billable` for employees who track billable hours (hybrid).

---

## 24. Summary

- Inheritance can be structured in several distinct patterns: **Single**, **Multilevel**, **Hierarchical**, **Multiple**, and **Hybrid**.
- **Single inheritance** involves one parent and one child; **multilevel** forms a chain across several levels; **hierarchical** involves multiple children sharing one parent.
- **Multiple inheritance** — a class inheriting from more than one parent directly — is **not** supported by Java through classes, due to the ambiguity it would create (the **diamond problem**).
- **Hybrid inheritance**, which typically combines multiple inheritance with other patterns, is similarly not directly supported through classes for the same reason.
- Java achieves the benefits of multiple and hybrid inheritance safely through **interfaces**, which require the implementing class to explicitly resolve any conflicting method implementations.
- Choosing the right inheritance type — and avoiding unnecessary complexity — is an important part of good object-oriented software design.

---

## 25. Key Takeaways

- Inheritance can be organized in different structures.
- Java directly supports Single, Multilevel, and Hierarchical inheritance.
- Java does not support Multiple inheritance with classes.
- Interfaces provide a safe way to achieve multiple inheritance.
- Understanding inheritance types helps in designing better software.

---

## 26. Cheat Sheet

**Definitions:**

```
Single         → 1 parent, 1 child
Multilevel     → chain: A → B → C
Hierarchical   → 1 parent, many children
Multiple       → 1 child, many DIRECT parents
Hybrid         → combination of the above types
```

**Syntax:**

```java
// Single
class B extends A { }

// Multilevel
class B extends A { }
class C extends B { }

// Hierarchical
class B extends A { }
class C extends A { }

// Multiple (NOT via classes — via interfaces only)
interface X { }
interface Y { }
class Z implements X, Y { }

// Hybrid (class inheritance + multiple interfaces)
class B extends A implements X, Y { }
```

**Supported Types:**

```
Directly via classes:      Single ✔   Multilevel ✔   Hierarchical ✔
NOT via classes:            Multiple ✘   Hybrid ✘
Achievable via interfaces:  Multiple ✔   Hybrid ✔
```

**Comparison Table:**

| Type         | Structure   | Java Class Support  |
| ------------ | ----------- | ------------------- |
| Single       | 1 → 1       | ✔                   |
| Multilevel   | A → B → C   | ✔                   |
| Hierarchical | 1 → many    | ✔                   |
| Multiple     | many → 1    | ✘ (interfaces only) |
| Hybrid       | combination | ✘ (interfaces only) |

**Best Practices:**

```
✔ Use inheritance only for genuine IS-A relationships
✔ Prefer composition when appropriate
✔ Avoid deep, unnecessary inheritance chains
✔ Resolve interface method conflicts explicitly and clearly
✔ Keep hierarchies as simple as the problem allows
```
