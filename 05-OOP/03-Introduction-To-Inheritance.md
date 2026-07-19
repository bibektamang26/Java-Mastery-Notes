# 03. Inheritance in Java

---

## 1. Introduction

- **What is Inheritance?** Inheritance is an OOP mechanism that allows one class to acquire the fields and methods of another class, enabling a new class to be built **on top of** an existing one, rather than written entirely from scratch.
- **Why is Inheritance Needed?** Many classes naturally share common characteristics — for example, both `Car` and `Bike` are kinds of `Vehicle`, sharing attributes like `speed` and behaviors like `accelerate()`. Inheritance lets this shared structure be defined **once**, in a common parent class, and reused across multiple related child classes.
- **Importance of Inheritance in OOP:** Inheritance is one of the four foundational pillars of OOP. It directly enables code reuse, supports the natural modeling of real-world "is-a" relationships, and forms the conceptual foundation for polymorphism, which is explored in a later chapter.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand the concept of inheritance.
- Create parent and child classes.
- Explain the IS-A relationship.
- Reuse code using inheritance.
- Identify different inheritance hierarchies.

---

## 3. What is Inheritance?

**Definition:** Inheritance is the mechanism by which a new class (called the **child class**) acquires the fields and methods of an existing class (called the **parent class**), allowing the child class to reuse, extend, or override that inherited functionality.

**Real-world analogy:** Think of inheritance like a family business passed down from parent to child. The child inherits the business's existing assets, customer relationships, and operating procedures (the "inherited members") automatically — but is also free to introduce new products or improve existing processes (adding new members, or overriding inherited behavior) on top of what was inherited.

**Biological inheritance:** The term itself is borrowed from biology, where children inherit traits (like eye color or height) from their parents. Similarly, in programming, a child class inherits characteristics (fields and methods) from its parent class.

**Parent-Child relationship:** The parent class defines the general, shared structure and behavior; the child class builds on that foundation, inheriting what already exists while being free to add new capabilities or customize existing ones.

---

## 4. Why Do We Need Inheritance?

- **Code Reusability:** Common fields and methods shared by multiple related classes can be written **once** in a parent class, rather than being duplicated in every related child class.
- **Reduce Code Duplication:** Without inheritance, classes like `Car`, `Bike`, and `Truck` would each need to separately redefine shared attributes like `speed` and shared behaviors like `accelerate()` — inheritance eliminates this repetition entirely.
- **Easier Maintenance:** If a shared behavior needs to change, it only needs to be updated in **one place** — the parent class — and that change automatically applies to every child class that inherits from it.
- **Extensibility:** New child classes can be added later to extend an existing parent class's functionality, without needing to modify the parent class itself, making a codebase easier to grow over time.

**Examples:**

```java
// Without inheritance — duplicated code
class Car {
    String brand;
    void start() { System.out.println("Car starting..."); }
}
class Bike {
    String brand;
    void start() { System.out.println("Bike starting..."); }  // duplicated logic pattern
}

// With inheritance — shared logic written once
class Vehicle {
    String brand;
    void start() { System.out.println("Vehicle starting..."); }
}
class Car extends Vehicle { }
class Bike extends Vehicle { }
```

---

## 5. Terminology

- **Parent Class:** The existing class whose members are inherited by another class.
- **Child Class:** The new class that inherits members from the parent class.
- **Superclass:** Another term for the parent class — emphasizes that it is "above" the child in the inheritance hierarchy.
- **Subclass:** Another term for the child class — emphasizes that it is "below" the parent in the inheritance hierarchy.
- **Base Class:** Another common term for the parent/superclass, often used interchangeably.
- **Derived Class:** Another common term for the child/subclass, often used interchangeably.

**Comparison Table:**

| Term Pair     | Meaning                           | Used Interchangeably With  |
| ------------- | --------------------------------- | -------------------------- |
| Parent Class  | The class being inherited from    | Superclass, Base Class     |
| Child Class   | The class that inherits           | Subclass, Derived Class    |
| Superclass    | Formal Java term for parent class | Parent Class, Base Class   |
| Subclass      | Formal Java term for child class  | Child Class, Derived Class |
| Base Class    | General OOP term for parent class | Parent Class, Superclass   |
| Derived Class | General OOP term for child class  | Child Class, Subclass      |

> All three pairs (Parent/Child, Superclass/Subclass, Base/Derived) refer to exactly the same relationship — they are simply different naming conventions used across various programming contexts and textbooks.

---

## 6. The `extends` Keyword

**Syntax:**

```java
class Child extends Parent {
}
```

**Explain each part:**

- **`class Child`** — declares the new class that will inherit from another class.
- **`extends`** — the keyword that establishes the inheritance relationship, indicating that `Child` is building upon `Parent`.
- **`Parent`** — the existing class whose members `Child` will inherit.

**Complete example:**

```java
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println(name + " is barking.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.name = "Rex";     // inherited field
        d.eat();             // inherited method
        d.bark();            // Dog's own method
    }
}
```

**Output:**

```
Rex is eating.
Rex is barking.
```

> Java supports only **single inheritance** for classes — a class can `extend` only **one** parent class directly (unlike some languages that allow multiple class inheritance). Java achieves the benefits of multiple inheritance-like behavior through **interfaces** instead, covered in a later chapter.

---

## 7. How Inheritance Works

```
Parent Class
      ↓
Child Class
      ↓
Inherited Members
```

When a child class extends a parent class, the child class automatically gains access to the parent's accessible (non-private) fields and methods, as if they had been defined directly within the child class itself — **in addition to** whatever new members the child class defines on its own.

**Examples:**

```java
class Person {
    String name;
    int age;

    void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

class Student extends Person {
    String course;

    void displayCourse() {
        System.out.println(name + " is enrolled in " + course);   // 'name' is inherited
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "Meera";       // inherited field
        s.age = 20;              // inherited field
        s.course = "B.Sc CS";    // Student's own field

        s.displayInfo();          // inherited method
        s.displayCourse();        // Student's own method
    }
}
```

**Output:**

```
Name: Meera, Age: 20
Meera is enrolled in B.Sc CS
```

---

## 8. IS-A Relationship

**Definition:** Inheritance models what's known as an **"IS-A" relationship** — the child class **is a** more specific kind of the parent class. This relationship should feel natural and grammatically correct when stated as a sentence; if it doesn't, inheritance is likely the wrong design choice.

**Examples:**

- **Dog IS-A Animal** → `class Dog extends Animal { }`
- **Car IS-A Vehicle** → `class Car extends Vehicle { }`
- **Student IS-A Person** → `class Student extends Person { }`

```java
class Animal { }
class Dog extends Animal { }   // "A Dog IS-A(n) Animal" — makes sense ✔

class Vehicle { }
class Car extends Vehicle { }  // "A Car IS-A Vehicle" — makes sense ✔

class Person { }
class Student extends Person { }  // "A Student IS-A Person" — makes sense ✔
```

If a relationship doesn't naturally fit an "IS-A" sentence (for example, "A Car IS-A Engine" sounds wrong — a car **has** an engine, it isn't one), a **"HAS-A"** relationship (composition) is usually the more appropriate design, rather than inheritance.

---

## 9. What Gets Inherited?

**✔ Instance Variables** — Non-static fields defined in the parent class are inherited by the child class (subject to access modifier visibility).

**✔ Methods** — Non-private methods defined in the parent class are inherited and can be called directly on child class objects.

**✔ Protected Members** — Fields/methods marked `protected` are inherited and accessible within the child class, even across different packages.

**✔ Public Members** — Fields/methods marked `public` are always inherited and fully accessible.

**What does NOT get inherited?**

**✘ Constructors** — Constructors are **not** inherited by child classes; however, a child class's constructor can (and implicitly does) call a parent class constructor using `super()`, which is introduced in Section 11.

**✘ Private Members (directly)** — Fields/methods marked `private` in the parent class exist within an inherited object's memory, but are **not directly accessible** by name in the child class — they can only be accessed indirectly, through public/protected methods the parent class provides (e.g., getters/setters).

**✘ Static Members (in the same way as instance members)** — Static members belong to the class itself, not to individual objects; while a child class _can_ access a parent's static members (through the class or object reference), they are not "inherited" per-object in the same sense as instance members, and static methods cannot be overridden the way instance methods can (they can only be _hidden_ by a same-signature static method in the child class).

**Examples:**

```java
class Parent {
    public int publicVar = 10;
    protected int protectedVar = 20;
    private int privateVar = 30;

    public int getPrivateVar() {
        return privateVar;   // controlled access to the private field
    }
}

class Child extends Parent {
    void display() {
        System.out.println(publicVar);       // accessible directly
        System.out.println(protectedVar);    // accessible directly
        // System.out.println(privateVar);   // Compile Error: not directly accessible
        System.out.println(getPrivateVar());  // accessible indirectly, via inherited method
    }
}
```

---

## 10. Memory Representation

```
Parent Object
      ↓
Child Object
```

When a `Child` object is created, it conceptually contains **both** the fields inherited from `Parent` **and** its own additional fields, all as part of a single object on the heap.

**Memory Diagram:**

```
Heap:
┌─────────────────────────────┐
│        Child Object           │
│  ┌─────────────────────────┐  │
│  │  Inherited from Parent:  │  │
│  │   name, age               │  │
│  └─────────────────────────┘  │
│  ┌─────────────────────────┐  │
│  │  Defined in Child:        │  │
│  │   course                  │  │
│  └─────────────────────────┘  │
└─────────────────────────────┘
        ▲
        │
Stack:  s ───────────────────────┘
```

There is only **one** object in memory — the child object simply has a larger memory footprint than a plain parent object would, since it includes all the inherited fields plus its own additional ones.

---

## 11. Constructor Execution During Inheritance

_(Simple introduction)_

```
Parent Constructor
      ↓
Child Constructor
```

Even though constructors are not inherited, Java guarantees that whenever a child class object is created, the **parent class's constructor always runs first**, followed by the child class's own constructor. This happens automatically via an implicit call to `super()` (the no-argument parent constructor) as the very first line of the child's constructor, if the child doesn't explicitly call a parent constructor itself.

**Example:**

```java
class Animal {
    Animal() {
        System.out.println("Animal constructor called.");
    }
}

class Dog extends Animal {
    Dog() {
        System.out.println("Dog constructor called.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
    }
}
```

**Output:**

```
Animal constructor called.
Dog constructor called.
```

This confirms that the parent constructor always executes **before** the child constructor's own body runs — ensuring the parent's part of the object is fully initialized before the child adds its own initialization on top.

_(A full, detailed treatment of constructor chaining and the `super()` keyword is provided in a later, dedicated chapter.)_

---

## 12. Advantages of Inheritance

- **Promotes code reuse**, avoiding duplication of shared fields and methods across related classes.
- **Simplifies maintenance**, since shared logic only needs to be updated in one place (the parent class).
- **Models real-world relationships naturally**, through the IS-A relationship.
- **Supports extensibility**, allowing new child classes to be added later without modifying existing parent classes.
- **Lays the foundation for polymorphism**, enabling child class objects to be treated as instances of their parent class in a uniform way (explored in the next chapter).

---

## 13. Disadvantages of Inheritance

- **Tight coupling:** Child classes become dependent on the internal design of their parent class — changes to a parent class can unintentionally break child classes.
- **Inappropriate use:** Using inheritance where no genuine IS-A relationship exists leads to confusing, fragile designs (this is why "favor composition over inheritance" is a common OOP guideline).
- **Increased complexity in deep hierarchies:** Long chains of inheritance (e.g., `A → B → C → D`) can make it difficult to trace exactly where a particular field or method actually comes from.
- **Fragile base class problem:** Seemingly safe changes to a parent class can have unexpected, hard-to-predict effects on all of its child classes.
- **No multiple class inheritance:** Java classes can only extend one parent class directly, which can sometimes make certain designs (needing to inherit behavior from two unrelated sources) more awkward, though interfaces help address this limitation.

---

## 14. Real-world Examples

**Animal:**

```java
class Animal {
    String name;
    void eat() { System.out.println(name + " eats."); }
}
class Cat extends Animal {
    void meow() { System.out.println(name + " meows."); }
}
```

**Vehicle:**

```java
class Vehicle {
    int speed;
    void accelerate() { System.out.println("Speeding up..."); }
}
class Truck extends Vehicle {
    int loadCapacity;
}
```

**Employee:**

```java
class Employee {
    String name;
    double baseSalary;
}
class Manager extends Employee {
    double bonus;
}
```

**Person:**

```java
class Person {
    String name;
    int age;
}
class Student extends Person {
    String course;
}
```

**Bank Account:**

```java
class BankAccount {
    double balance;
    void deposit(double amount) { balance += amount; }
}
class SavingsAccount extends BankAccount {
    double interestRate;
}
```

**Library:**

```java
class LibraryItem {
    String title;
    boolean isAvailable;
}
class Book extends LibraryItem {
    String author;
}
```

Each of these examples models a natural IS-A relationship (a Cat IS-A(n) Animal, a Truck IS-A Vehicle, a Manager IS-A(n) Employee, and so on), which is exactly the kind of situation where inheritance is the appropriate design choice.

---

## 15. Common Mistakes

1. **Using inheritance for a relationship that isn't truly IS-A:**

   ```java
   class Engine { }
   class Car extends Engine { }   // Wrong! A Car doesn't "IS-A" Engine — it HAS an Engine (composition)
   ```

2. **Assuming private fields are directly accessible in the child class:**

   ```java
   class Parent {
       private int secret = 42;
   }
   class Child extends Parent {
       void show() {
           // System.out.println(secret);  // Compile Error: secret is private to Parent
       }
   }
   ```

3. **Forgetting that Java doesn't support multiple class inheritance:**

   ```java
   // class Child extends Parent1, Parent2 { }   // Compile Error: not allowed in Java
   ```

4. **Overusing deep inheritance hierarchies** (`A → B → C → D → E`), making it difficult to trace where fields/methods actually originate, and creating fragile dependencies between many layers.

5. **Assuming constructors are inherited:**

   ```java
   class Parent {
       Parent(int x) { }
   }
   class Child extends Parent {
       // A no-arg Child() constructor is NOT automatically usable here unless
       // Parent also has a no-arg constructor, or Child explicitly calls super(x)
   }
   ```

6. **Modifying a parent class without considering the impact on all its child classes** — since child classes depend on the parent's structure, seemingly small changes can have wide-reaching, unexpected effects.

---

## 16. Best Practices

- Only use inheritance when a genuine **IS-A** relationship exists between the parent and child class.
- Prefer **composition over inheritance** ("HAS-A" relationships) when a class simply needs to _use_ another class's functionality, rather than _be_ a specialized version of it.
- Keep inheritance hierarchies **shallow** — avoid excessively long chains of parent-child relationships where possible.
- Use `protected` (rather than `public`) for fields/methods that child classes need direct access to, but that shouldn't be exposed to the outside world.
- Document the intended purpose of a parent class clearly, since changes to it can ripple out to every child class that depends on it.

---

## 17. Interview Corner

**Q1. What is inheritance in Java?**
Inheritance is an OOP mechanism that allows a child class to acquire the fields and methods of a parent class, enabling code reuse and modeling of "IS-A" relationships between classes.

**Q2. Does Java support multiple inheritance for classes?**
No — a Java class can `extend` only one parent class directly. Java achieves similar benefits to multiple inheritance through interfaces, which a class can implement in any number.

**Q3. Are constructors inherited in Java?**
No — constructors are never inherited. However, a child class's constructor implicitly (or explicitly, via `super()`) calls a parent class constructor as its very first action, ensuring the parent portion of the object is properly initialized.

**Q4. Can a child class access a parent class's private members?**
Not directly by name — private members are not accessible outside their own class, even to a child class. They can only be accessed indirectly, through public or protected methods the parent class provides.

**Q5. What is the difference between IS-A and HAS-A relationships?**
IS-A describes an inheritance relationship, where a child class is a more specific type of its parent (e.g., a Dog IS-A(n) Animal); HAS-A describes a composition relationship, where a class contains or uses another class as one of its parts (e.g., a Car HAS-A(n) Engine).

**Q6. Which constructor runs first when a child class object is created — parent's or child's?**
The parent class's constructor always runs first, followed by the child class's own constructor — this is guaranteed by Java's implicit/explicit `super()` call as the first statement in every constructor.

**Q7. Why is "favor composition over inheritance" a common piece of OOP advice?**
Because inheritance creates tight coupling between parent and child classes, and using it where no genuine IS-A relationship exists leads to fragile, confusing designs; composition offers more flexibility by allowing classes to reuse functionality without being locked into a rigid parent-child hierarchy.

---

## 18. MCQs

**1. Which keyword is used to implement inheritance in Java?**
a) implements
b) extends
c) inherits
d) super
**Answer: b) extends**

**2. How many classes can a Java class directly extend?**
a) Unlimited
b) Exactly one
c) Exactly two
d) Zero
**Answer: b) Exactly one**

**3. Are constructors inherited by a child class?**
a) Yes, always
b) No, never
c) Only if declared public
d) Only if declared static
**Answer: b) No, never**

**4. Which of these best describes an IS-A relationship?**
a) A Car HAS-A Engine
b) A Dog IS-A(n) Animal
c) A Library HAS-A Book
d) A Bank HAS-A(n) Account
**Answer: b) A Dog IS-A(n) Animal**

**5. Can a child class directly access a private field of its parent class?**
a) Yes, always
b) No, not directly
c) Only if in the same package
d) Only if declared final
**Answer: b) No, not directly**

**6. Which constructor executes first when creating a child class object?**
a) Child's constructor
b) Parent's constructor
c) Both simultaneously
d) Neither, unless explicitly called
**Answer: b) Parent's constructor**

**7. What term is used interchangeably with "child class"?**
a) Superclass
b) Base class
c) Subclass
d) Interface
**Answer: c) Subclass**

**8. What is a disadvantage of inheritance?**
a) Increased code reusability
b) Tight coupling between parent and child classes
c) Easier maintenance
d) Better extensibility
**Answer: b) Tight coupling between parent and child classes**

**9. Which OOP principle does inheritance lay the foundation for?**
a) Encapsulation
b) Abstraction
c) Polymorphism
d) Cohesion
**Answer: c) Polymorphism**

**10. What should be used instead of inheritance when no true IS-A relationship exists?**
a) Interfaces only
b) Composition (HAS-A relationship)
c) Static methods
d) Constructors
**Answer: b) Composition (HAS-A relationship)**

---

## 19. University Questions

1. Define inheritance and explain why it is needed in object-oriented programming.
2. Explain the `extends` keyword with a suitable Java program demonstrating a parent and child class.
3. What is the IS-A relationship? Explain with at least three examples.
4. Discuss what is and is not inherited by a child class in Java, with examples for each category.
5. Explain the order of constructor execution when a child class object is created, with a program example.
6. Discuss the advantages and disadvantages of inheritance in software design.
7. Differentiate between IS-A and HAS-A relationships, explaining when each should be used.

---

## 20. Viva Questions

1. What keyword is used for inheritance in Java?
2. Why does Java not support multiple inheritance for classes?
3. Are static members inherited the same way as instance members?
4. Can a subclass access its superclass's protected members from a different package?
5. What happens if a parent class has no no-argument constructor and the child class doesn't explicitly call `super(...)`?
6. What is the difference between a superclass and a subclass?
7. Why are constructors not inherited?
8. What is the "fragile base class" problem?
9. Give one real-world example each of an IS-A and a HAS-A relationship.
10. Why is deep inheritance (many levels) generally discouraged?

---

## 21. Programming Exercises

**Easy**

1. Create a `Person` class with `name` and `age` fields, and a `Student` class that extends it with an additional `course` field. Create a `Student` object and print all its details.
2. Create an `Animal` class with an `eat()` method, and a `Dog` class that extends it with a `bark()` method.

**Medium**

3. Create a `Vehicle` class with `speed` and `accelerate()`, and two child classes `Car` and `Bike`, each adding their own unique field and method.
4. Write a program demonstrating that a child class cannot directly access a private field of its parent class, and show the correct way to access it using an inherited public method.

**Hard**

5. Create a three-level inheritance hierarchy (`Employee → Manager → SeniorManager`), each level adding new fields, and write a program that creates a `SeniorManager` object and prints all inherited and own fields.

---

## 22. Challenge Problems

**Library Management:** Design a `LibraryItem` parent class with common fields (`title`, `isAvailable`), and child classes `Book` and `Magazine`, each with their own additional fields — write a program that creates objects of both and demonstrates inherited behavior.

**Bank Account Hierarchy:** Design a `BankAccount` parent class with a `deposit()` method and a `balance` field, and child classes `SavingsAccount` (with an `interestRate` field) and `CurrentAccount` (with an `overdraftLimit` field).

**Employee Payroll System:** Design an `Employee` parent class with `name` and `baseSalary`, and child classes `Manager` (with a `bonus` field) and `Intern` (with a `stipend` field), demonstrating how each child class extends the shared base structure differently.

**Shape Hierarchy (conceptual foundation for later polymorphism):** Design a `Shape` parent class with a `name` field, and child classes `Circle` and `Rectangle`, each with their own dimension fields — this exercise sets up the foundation for method overriding, explored in the next chapter on polymorphism.

---

## 23. Summary

- **Inheritance** allows a child class to acquire the fields and methods of a parent class, enabling code reuse and reducing duplication across related classes.
- The **`extends`** keyword establishes an inheritance relationship in Java; a class can extend only **one** parent class directly.
- Inheritance models an **IS-A relationship** — the child class must genuinely be a more specific kind of the parent class for inheritance to be an appropriate design choice.
- Instance variables, methods, protected members, and public members are inherited; **constructors, private members (directly), and static members** are not inherited in the same way.
- Whenever a child class object is created, the **parent class's constructor always executes first**, followed by the child class's own constructor.
- Inheritance offers real advantages (reusability, maintainability, extensibility) but also real risks (tight coupling, fragile base class problems) if used where no genuine IS-A relationship exists — composition is often the better alternative in such cases.

---

## 24. Key Takeaways

- Inheritance = code reuse + IS-A relationship modeling.
- `extends` establishes inheritance; Java allows only single class inheritance.
- Constructors are never inherited, but the parent's constructor always runs before the child's.
- Private members aren't directly accessible in child classes — only indirectly, through inherited public/protected methods.
- Favor composition (HAS-A) over inheritance (IS-A) when no genuine specialization relationship exists.
- Inheritance sets up the foundation for polymorphism, covered next.

---

## 25. Cheat Sheet

```
SYNTAX
class Child extends Parent {
    // additional fields and methods
}

TERMINOLOGY (all interchangeable pairs)
Parent Class  = Superclass  = Base Class
Child Class   = Subclass    = Derived Class

WHAT IS INHERITED
✔ Instance variables (non-private)
✔ Methods (non-private)
✔ protected members
✔ public members

WHAT IS NOT INHERITED
✘ Constructors (never inherited, but parent's always runs first)
✘ private members (not DIRECTLY accessible; accessible only via inherited methods)
✘ static members (not per-object inherited; can be hidden, not overridden)

CONSTRUCTOR EXECUTION ORDER
Parent Constructor  →  runs FIRST
Child Constructor   →  runs SECOND

IS-A vs HAS-A
IS-A  → use INHERITANCE   (Dog IS-A Animal)
HAS-A → use COMPOSITION   (Car HAS-A(n) Engine)

JAVA RULE
A class can extend only ONE parent class directly (no multiple class inheritance).
Interfaces are used to achieve multiple-inheritance-like behavior (covered later).
```
