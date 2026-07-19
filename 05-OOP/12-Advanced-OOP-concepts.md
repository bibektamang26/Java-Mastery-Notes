# Advanced Object-Oriented Programming Concepts

---

## 1. Introduction

### Why learn these concepts?

The topics covered so far — classes, objects, methods, constructors, `this`, access modifiers, static members, interfaces — form the _core_ of Java's object-oriented model. This chapter goes a level deeper, into concepts that every object in Java relies on behind the scenes (the `Object` class), design decisions that shape how classes relate to one another (composition vs. inheritance, object relationships), and more specialized class structures (nested classes, anonymous classes, sealed classes) that appear frequently in real, professional Java codebases.

### Learning Objectives

After completing this chapter, you will be able to:

- Explain the role of the `Object` class and its key methods.
- Correctly override `toString()`, `equals()`, and `hashCode()`, understanding the contract between the latter two.
- Distinguish composition from inheritance, and choose the appropriate one for a given design.
- Differentiate association, aggregation, and composition as object relationships.
- Understand and use the four kinds of nested classes.
- Understand anonymous classes and how they differ from lambda expressions.
- Understand the purpose and basic syntax of sealed classes.

### Prerequisites

- Classes, Objects, Methods, Constructors (earlier chapters).
- Inheritance and Method Overriding (concepts assumed from prior chapters).
- Interfaces (previous chapter) — useful context for sealed classes and object relationships.

---

## 2. The Object Class

### What is `Object`?

`Object` is a special class in Java's `java.lang` package that sits at the very **top of every class hierarchy**. Every class in Java, whether you realize it or not, is ultimately a subclass of `Object`.

### Why every class extends `Object`

If a class doesn't explicitly `extend` any other class, Java **implicitly** makes it extend `Object`. This guarantees that **every object**, of every type, in every Java program, automatically has a baseline set of common behaviors available to it — without the programmer needing to define them.

```java
class Student {   // implicitly: class Student extends Object
    String name;
}
```

### Important methods

`Object` provides several methods inherited by every class in Java:

- **`toString()`** — returns a string representation of the object.
- **`equals()`** — compares this object to another for logical equality.
- **`hashCode()`** — returns an integer "hash" used in hashing-based collections.
- **`clone()`** — creates and returns a copy of the object (requires implementing `Cloneable`).
- **`getClass()`** — returns runtime information about the object's actual class.
- **`wait()`, `notify()`, `notifyAll()`** — used for thread synchronization and communication (a topic belonging to Java's concurrency/multithreading material).

### Example

```java
class Student { String name; }

Student s1 = new Student();
System.out.println(s1.toString());        // inherited from Object, default form
System.out.println(s1.getClass().getName()); // "Student"
```

`getClass()` is especially useful for inspecting an object's actual runtime type — it returns a `Class` object describing `Student`, regardless of what reference type was used to access `s1`.

---

## 3. Understanding `toString()`

### Purpose

`toString()` is meant to provide a **human-readable string representation** of an object — used automatically whenever an object is printed (e.g., `System.out.println(obj)`) or concatenated with a string.

### Default implementation

`Object`'s default `toString()` returns a string in the form `ClassName@hexHashCode` — technically valid, but not very useful for understanding an object's actual data.

```java
Student s1 = new Student();
System.out.println(s1);   // e.g., Student@1b6d3586 (default, unhelpful)
```

### Overriding `toString()`

By **overriding** `toString()` in your own class, you control exactly what gets printed — typically showing the object's key field values.

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}

Student s1 = new Student("Aditi", 21);
System.out.println(s1);   // Student{name='Aditi', age=21}
```

### Best Practices

- Always override `toString()` for classes whose objects will be printed, logged, or debugged.
- Keep the format concise but informative — include the most important fields.
- Use the `@Override` annotation to catch mistakes (e.g., misspelling the method name) at compile time.

### Example

```java
class Book {
    String title;
    String author;

    Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    @Override
    public String toString() {
        return title + " by " + author;
    }
}

System.out.println(new Book("1984", "George Orwell"));  // 1984 by George Orwell
```

---

## 4. Understanding `equals()`

### Purpose

`equals()` is meant to define what it means for two objects to be considered **logically equal** — based on their content/state, rather than their identity (memory address).

### `==` vs `equals()`

- **`==`**, for objects, compares **references** — whether both variables point to the exact same object in memory.
- **`equals()`**, by default (inherited from `Object`), behaves **identically to `==`** — unless a class explicitly overrides it to define content-based equality.

```java
Student s1 = new Student("Aditi", 21);
Student s2 = new Student("Aditi", 21);

System.out.println(s1 == s2);        // false -- different objects in memory
System.out.println(s1.equals(s2));   // false, UNLESS Student overrides equals()
```

### Overriding `equals()`

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;                 // same reference -> definitely equal
        if (obj == null || getClass() != obj.getClass()) return false;  // type check
        Student other = (Student) obj;                 // safe cast after type check
        return this.age == other.age && this.name.equals(other.name);  // content comparison
    }
}
```

### Example

```java
Student s1 = new Student("Aditi", 21);
Student s2 = new Student("Aditi", 21);

System.out.println(s1.equals(s2));   // true -- now compares content, not just reference
```

---

## 5. Understanding `hashCode()`

### Purpose

`hashCode()` returns an `int` value representing the object, used internally by **hash-based collections** (like `HashMap`, `HashSet`) to efficiently organize and locate objects by grouping them into "buckets."

### Relationship with `equals()`

`hashCode()` and `equals()` are deeply connected: hash-based collections use `hashCode()` to quickly narrow down _which bucket_ an object belongs in, and then use `equals()` to confirm actual equality among objects within that bucket. If these two methods are inconsistent with each other, hash-based collections can behave incorrectly (e.g., failing to find an object that's actually present).

### Contract between `equals()` and `hashCode()`

Java's formal contract states:

1. **If two objects are equal according to `equals()`, they MUST have the same `hashCode()`.**
2. If two objects have the same `hashCode()`, they are **not required** to be equal (a "hash collision" is allowed).
3. `hashCode()` must consistently return the same value for the same object, as long as its relevant fields don't change.

**Rule of thumb: whenever you override `equals()`, you must also override `hashCode()`**, using the same fields, to keep them consistent.

### Example

```java
class Student {
    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Student other = (Student) obj;
        return this.age == other.age && this.name.equals(other.name);
    }

    @Override
    public int hashCode() {
        return java.util.Objects.hash(name, age);   // combines both fields consistently
    }
}
```

Using `java.util.Objects.hash(...)` is a common, reliable way to generate a well-distributed hash code from multiple fields, matching the fields used in `equals()`.

---

## 6. Composition vs Inheritance

### HAS-A Relationship

**Composition** models a **"HAS-A"** relationship — one class contains a reference to another class as a field, representing a "made up of" or "has a part" relationship.

```java
class Engine {
    void start() { System.out.println("Engine starting..."); }
}

class Car {
    private Engine engine = new Engine();   // Car HAS-A Engine

    void start() {
        engine.start();
    }
}
```

### IS-A Relationship

**Inheritance** models an **"IS-A"** relationship — a subclass IS a more specific kind of its parent class.

```java
class Vehicle {
    void move() { System.out.println("Vehicle moving..."); }
}

class Car extends Vehicle {   // Car IS-A Vehicle
}
```

### Advantages (of Composition)

- More flexible — the contained object can be swapped out at runtime (e.g., replacing `Engine` with a different implementation).
- Avoids the rigidity and tight coupling that comes with deep inheritance hierarchies.
- Doesn't expose the container's internal parts through inheritance — the containing class fully controls how the contained object is used.

### Disadvantages (of Composition)

- Requires more boilerplate — the containing class must explicitly delegate to the contained object's methods when needed.
- Doesn't automatically inherit behavior the way subclassing does — every needed behavior must be explicitly exposed.

### When to use Composition

Use composition when the relationship is naturally "has a part" (a `Car` has an `Engine`), when you need flexibility to change the internal implementation at runtime, or when inheritance would create an inappropriate IS-A relationship (e.g., a `Car` is _not_ an `Engine`).

### When to use Inheritance

Use inheritance when there's a genuine, stable IS-A relationship (a `Car` really is a kind of `Vehicle`), and when you want subclasses to automatically inherit and potentially override shared behavior.

### Comparison Table

| Aspect              | Composition (HAS-A)                      | Inheritance (IS-A)                            |
| ------------------- | ---------------------------------------- | --------------------------------------------- |
| **Relationship**    | One class contains another as a field    | One class extends another                     |
| **Coupling**        | Looser — contained object can be swapped | Tighter — subclass tied to parent's structure |
| **Flexibility**     | High — can change behavior at runtime    | Lower — relationship fixed at compile time    |
| **Code reuse**      | Via delegation (explicit method calls)   | Automatic (inherited methods)                 |
| **Common guidance** | "Favor composition over inheritance"     | Use only for genuine IS-A relationships       |

### Example

```java
// Composition example
class Battery {
    void supplyPower() { System.out.println("Supplying power..."); }
}

class Laptop {
    private Battery battery = new Battery();  // Laptop HAS-A Battery

    void turnOn() {
        battery.supplyPower();
        System.out.println("Laptop turning on...");
    }
}
```

---

## 7. Object Relationships

### Association

**Association** is the most general relationship — it simply means two classes are connected/interact with each other in some way, without implying ownership. Objects can exist independently of each other.

### Aggregation

**Aggregation** is a specialized, weaker form of "HAS-A" relationship where one object _contains_ references to others, but those contained objects can exist **independently** — they are not owned exclusively, and their lifecycle is not tied to the container.

### Composition

**Composition** (as a relationship type, distinct from the "composition vs inheritance" design choice in Section 6) is a **stronger** form of "HAS-A" — the contained object's lifecycle is tightly bound to the container's; if the container is destroyed, the contained object typically has no independent purpose or existence.

### Definitions

| Relationship    | Meaning                                                                                                              |
| --------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Association** | A general connection/interaction between two classes; no ownership implied                                           |
| **Aggregation** | A "HAS-A" relationship where the contained object can exist independently ("weak ownership")                         |
| **Composition** | A "HAS-A" relationship where the contained object's lifecycle depends entirely on the container ("strong ownership") |

### Examples

**Association** — a `Teacher` and a `Student` are associated (a teacher teaches students), but neither owns the other; both exist independently.

```java
class Teacher {
    void teach(Student s) {
        System.out.println("Teaching " + s.name);
    }
}
```

**Aggregation** — a `Department` has `Professor` objects, but if the `Department` is dissolved, the `Professor` objects still exist (they could move to another department).

```java
class Professor { String name; }

class Department {
    List<Professor> professors;   // aggregation -- professors exist independently of the department
}
```

**Composition** — a `House` has `Room` objects; if the `House` is destroyed, the `Room`s cease to have any independent existence.

```java
class Room { String type; }

class House {
    private List<Room> rooms = new ArrayList<>();  // composition -- rooms don't exist without the house
}
```

### UML Representation

```
Association:   ClassA -------- ClassB               (plain line)

Aggregation:   ClassA <>------ ClassB               (hollow diamond at the "owner" end)

Composition:   ClassA <#>------ ClassB               (filled diamond at the "owner" end)
```

_(This is a simplified text depiction; actual UML tools render hollow vs. filled diamonds graphically.)_

### Comparison Table

| Aspect                   | Association     | Aggregation          | Composition |
| ------------------------ | --------------- | -------------------- | ----------- |
| **Ownership**            | None            | Weak                 | Strong      |
| **Lifecycle dependency** | Independent     | Independent          | Dependent   |
| **Example**              | Teacher–Student | Department–Professor | House–Room  |
| **"HAS-A"?**             | Not necessarily | Yes                  | Yes         |

---

## 8. Nested Classes

A **nested class** is any class defined **inside** another class. Java provides four kinds:

### Static Nested Class

A class declared `static` inside another class — does **not** require an instance of the outer class to exist, and cannot directly access the outer class's instance members.

```java
class Outer {
    static class Nested {
        void show() { System.out.println("Static nested class"); }
    }
}

Outer.Nested obj = new Outer.Nested();   // no Outer instance needed
```

### Inner Class

A **non-static** class declared inside another class — requires an instance of the outer class to exist, and _can_ directly access the outer class's instance members.

```java
class Outer {
    int value = 10;

    class Inner {
        void show() {
            System.out.println("Outer's value: " + value);   // direct access to outer's field
        }
    }
}

Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();   // requires an Outer instance
inner.show();
```

### Local Inner Class

A class defined **inside a method body** — scoped only to that method, and typically used for a narrowly-focused helper class needed only within that method's logic.

```java
class Outer {
    void process() {
        class LocalHelper {   // defined INSIDE the method
            void assist() { System.out.println("Helping..."); }
        }
        LocalHelper helper = new LocalHelper();
        helper.assist();
    }
}
```

### Anonymous Inner Class

A class with **no name**, defined and instantiated in a **single expression**, typically used to quickly provide a one-off implementation of an interface or abstract class. _(Covered in full detail in Section 9.)_

### Examples

_(See each subsection above for a focused example of each kind.)_

### Advantages

- Logically groups helper classes tightly with the class/method they support, improving code organization.
- Static nested classes avoid unnecessary coupling to an outer instance when that coupling isn't needed.
- Inner classes are useful when a nested class genuinely needs ongoing access to the outer object's state.
- Local and anonymous inner classes keep narrowly-scoped, single-use logic close to where it's actually needed, rather than cluttering the top level of a file.

---

## 9. Anonymous Classes

### Definition

An **anonymous class** is a class with no name, defined and instantiated all at once, typically used to provide an on-the-spot implementation of an interface or an abstract/concrete class — useful when you need a one-off object and don't want to create a separate, fully named class just for it.

### Syntax

```java
InterfaceOrClass ref = new InterfaceOrClass() {
    // method implementations here
};
```

### Example

```java
interface Greeting {
    void sayHello();
}

Greeting g = new Greeting() {         // anonymous class implementing Greeting
    public void sayHello() {
        System.out.println("Hello from an anonymous class!");
    }
};

g.sayHello();
```

```java
// Anonymous class extending a concrete class
class Animal {
    void sound() { System.out.println("Some generic sound"); }
}

Animal cat = new Animal() {           // anonymous subclass of Animal
    void sound() { System.out.println("Meow"); }
};

cat.sound();
```

### Advantages

- Avoids the overhead of writing a fully separate, named class for a one-time-use implementation.
- Keeps small, focused implementation logic close to where it's actually used.
- Commonly used historically for event listeners and callback-style code.

### Limitations

- Cannot have a named constructor (since the class itself has no name).
- Cannot implement multiple interfaces at once, or extend a class while also implementing an interface.
- Can make code harder to read if overused for anything beyond small, simple implementations.
- Cannot be reused elsewhere, since there's no name to refer to it by.

### Difference from Lambda Expressions

Anonymous classes and lambda expressions can look similar in purpose (providing a quick, inline implementation), but they differ meaningfully:

| Aspect                       | Anonymous Class                                              | Lambda Expression                                                                      |
| ---------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| **Applicable to**            | Any interface or abstract/concrete class                     | Only functional interfaces (single abstract method)                                    |
| **Syntax**                   | Verbose — full class body with `new InterfaceName() { ... }` | Concise — `(params) -> expression/body`                                                |
| **`this` reference**         | Refers to the anonymous class instance itself                | Refers to the **enclosing** class instance (lambdas don't create a new `this` context) |
| **Can hold state (fields)?** | Yes                                                          | No (no fields of its own)                                                              |

```java
// Anonymous class
Runnable r1 = new Runnable() {
    public void run() { System.out.println("Anonymous class running"); }
};

// Equivalent lambda (since Runnable is a functional interface)
Runnable r2 = () -> System.out.println("Lambda running");
```

---

## 10. Sealed Classes (Introduction)

### Why introduced

**Sealed classes**, introduced as a preview feature in Java 15 and finalized in Java 17, let a class or interface **explicitly restrict** which other classes are allowed to extend or implement it — giving the author precise control over a type's hierarchy, which regular `public`/unrestricted inheritance does not allow.

### `sealed`

The keyword used to declare a class or interface as sealed, restricting its permitted subclasses.

### `permits`

Used alongside `sealed` to explicitly **list** the specific classes that are allowed to extend the sealed class/interface — any other class attempting to extend it will fail to compile.

### `non-sealed`

A permitted subclass of a sealed class can be declared `non-sealed`, which **reopens** that specific branch of the hierarchy — allowing _any_ class to extend it further, undoing the restriction from that point onward.

### `final`

A permitted subclass can instead be declared `final`, which **closes off** the hierarchy completely at that point — no further subclassing is allowed at all.

### Simple Example

```java
sealed interface Shape permits Circle, Square, Triangle { }

final class Circle implements Shape { }         // closed -- no further subclassing

non-sealed class Square implements Shape { }    // reopened -- can be extended by anyone

sealed class Triangle implements Shape permits RightTriangle { }  // still restricted further

final class RightTriangle extends Triangle { }
```

### Comparison with `final` and abstract classes

| Aspect                     | `final` class                    | `abstract` class                                 | `sealed` class                                                                 |
| -------------------------- | -------------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------ |
| **Subclassing**            | Not allowed at all               | Required (cannot instantiate directly)           | Allowed, but only for explicitly permitted classes                             |
| **Control over hierarchy** | Total restriction (none allowed) | No restriction on who can extend                 | Precise, explicit control over exactly who can extend                          |
| **Use case**               | Prevent any extension            | Force a shared base with mandatory customization | Model a fixed, known set of possible subclasses (e.g., specific `Shape` types) |

Sealed classes are particularly useful when combined with modern pattern-matching features, since the compiler can know the **complete, closed set** of possible subclasses and verify that all cases are handled — a benefit regular open inheritance cannot provide.

_(Sealed classes are a more advanced, modern Java feature — a full treatment, including interaction with pattern matching and records, belongs in a dedicated later topic.)_

---

## 11. Best Practices

- Always override `toString()` for classes that will be printed, logged, or debugged.
- Whenever you override `equals()`, always override `hashCode()` too, using the same fields, to honor the contract.
- Favor **composition over inheritance** by default, reserving inheritance for genuine, stable IS-A relationships.
- Use aggregation vs. composition intentionally, based on whether the contained object should genuinely outlive the container.
- Prefer **static nested classes** unless the nested class truly needs access to the outer instance's state.
- Use anonymous classes only for small, simple, one-off implementations — prefer named classes or lambdas for anything more complex.
- Consider sealed classes/interfaces when modeling a fixed, known set of subclasses (e.g., specific shapes, specific payment types).

---

## 12. Common Mistakes

- **Overriding `equals()` without overriding `hashCode()`**, breaking the contract and causing subtle bugs in hash-based collections.
- **Using `==` instead of `equals()`** when logical content equality (not reference equality) is intended.
- **Choosing inheritance for convenience** rather than a genuine IS-A relationship, leading to fragile, tightly-coupled designs.
- **Confusing aggregation and composition** — forgetting that the key difference is whether the contained object's lifecycle depends on the container.
- **Overusing inner classes when a static nested class would do**, unnecessarily coupling the nested class to an outer instance.
- **Overusing anonymous classes for complex logic**, making code hard to read and impossible to reuse or unit test independently.

---

## 13. Interview Corner

1. **Why does every class in Java implicitly extend `Object`?**
   To guarantee a common baseline of behavior (like `toString()`, `equals()`, `hashCode()`) is available on every object, regardless of its specific type.

2. **What is the default behavior of `equals()` before overriding it?**
   It behaves exactly like `==`, comparing object references rather than content.

3. **What is the contract between `equals()` and `hashCode()`?**
   If two objects are equal according to `equals()`, they must return the same `hashCode()`; the reverse is not required.

4. **What happens if you override `equals()` but not `hashCode()`?**
   The class violates the equals/hashCode contract, which can cause incorrect behavior in hash-based collections like `HashMap` and `HashSet`.

5. **What's the difference between composition and inheritance?**
   Composition models a "HAS-A" relationship via containment; inheritance models an "IS-A" relationship via `extends`.

6. **Why is "favor composition over inheritance" common advice?**
   Composition offers looser coupling and more runtime flexibility, avoiding the rigidity and fragile hierarchies that deep inheritance can create.

7. **What's the difference between aggregation and composition (as object relationships)?**
   In aggregation, the contained object can exist independently of the container; in composition, its lifecycle is bound to the container's.

8. **What's the difference between a static nested class and an inner class?**
   A static nested class doesn't require an outer instance and can't directly access outer instance members; an inner class requires an outer instance and can access its members directly.

9. **What is an anonymous class used for?**
   Providing a quick, one-off implementation of an interface or class without creating a separate named class.

10. **How does an anonymous class differ from a lambda expression?**
    Anonymous classes work with any interface/class and can hold their own state; lambdas only work with functional interfaces, are more concise, and their `this` refers to the enclosing context, not the lambda itself.

11. **What is the purpose of sealed classes?**
    To let a class/interface explicitly restrict exactly which classes are permitted to extend/implement it, giving precise control over its hierarchy.

12. **What does the `permits` clause do?**
    Lists the specific classes allowed to extend a sealed class or implement a sealed interface.

---

## 14. MCQs

1. Every class in Java implicitly extends:
   a) `String` b) `Object` c) `Class` d) `System`

2. The default `equals()` implementation compares:
   a) Field values b) References (like `==`) c) Hash codes only d) Nothing

3. If two objects are equal via `equals()`, their `hashCode()` must be:
   a) Different b) The same c) Unrelated d) Zero

4. Composition models a:
   a) IS-A relationship b) HAS-A relationship c) Uses-A relationship only d) No relationship

5. Inheritance models a:
   a) HAS-A relationship b) IS-A relationship c) Weak ownership d) No relationship

6. Which relationship implies the contained object cannot exist without the container?
   a) Association b) Aggregation c) Composition d) Inheritance

7. A static nested class:
   a) Requires an outer instance b) Does not require an outer instance c) Cannot have methods d) Must be abstract

8. An inner (non-static) class:
   a) Can access outer instance members directly b) Cannot access outer members at all c) Requires no outer instance d) Must be static

9. An anonymous class is:
   a) A named, reusable class b) A class with no name, defined and instantiated together c) Always abstract d) Only usable with lambdas

10. Which of these can a lambda expression NOT do that an anonymous class can?
    a) Implement a functional interface b) Hold its own fields/state c) Be assigned to a variable d) Call methods

11. `sealed` classes use which keyword to specify allowed subclasses?
    a) `allows` b) `permits` c) `extends` d) `restricts`

12. A `non-sealed` subclass of a sealed class:
    a) Cannot be extended further b) Reopens the hierarchy for further extension c) Must be final d) Is not allowed in Java

13. What must you do when overriding `equals()`?
    a) Nothing else is required b) Also override `hashCode()` for consistency c) Remove `toString()` d) Make the class final

14. `toString()`'s default implementation returns:
    a) The object's field values b) `ClassName@hashCode` in hex c) `null` d) An empty string

15. Which best describes "favor composition over inheritance"?
    a) Never use inheritance b) Prefer containment relationships over subclassing for flexibility, using inheritance only for genuine IS-A cases c) Always use inheritance instead d) Composition and inheritance are identical

---

## 15. University Questions

### Short Questions

1. What is the `Object` class, and why does every class extend it?
2. Differentiate between `==` and `equals()`.
3. State the contract between `equals()` and `hashCode()`.
4. Differentiate between composition and inheritance with examples.
5. What is an anonymous class?

### Long Questions

1. Explain the `Object` class and its important methods, with examples of `toString()`, `equals()`, and `hashCode()`.
2. Discuss composition versus inheritance, including a comparison table, and explain when each should be used.
3. Explain association, aggregation, and composition as object relationships, with real-world examples and UML representation.
4. Describe the four types of nested classes in Java, with an example of each.
5. Explain anonymous classes, including their syntax, advantages, limitations, and how they differ from lambda expressions.

---

## 16. Viva Questions

1. What is the `Object` class in Java?
2. Why is `toString()` usually overridden?
3. What is the difference between `==` and `.equals()`?
4. What happens if `equals()` and `hashCode()` are inconsistent?
5. What is the difference between a HAS-A and an IS-A relationship?
6. What is the difference between aggregation and composition?
7. What is a static nested class, and how does it differ from an inner class?
8. What is a local inner class?
9. What is an anonymous class, and when would you use one?
10. What is the purpose of sealed classes in modern Java?
11. What does the `permits` keyword do?
12. Can a sealed class's permitted subclass be reopened for further extension? How?

---

## 17. Programming Exercises

1. Create a `Book` class and override `toString()` to display its title and author meaningfully.
2. Create a `Point` class with `x` and `y` fields, and override both `equals()` and `hashCode()` correctly.
3. Design a `Car` and `Engine` class demonstrating composition (HAS-A), and a `Vehicle`/`Car` pair demonstrating inheritance (IS-A).
4. Design a `Library` and `Book` pair demonstrating aggregation, and a `House` and `Room` pair demonstrating composition (as a relationship).
5. Write a program using a static nested class and an inner class inside the same outer class, demonstrating the difference in how each accesses the outer instance.
6. Use an anonymous class to implement a simple interface (e.g., a `Greeting` interface), then rewrite the same logic using a lambda expression.

---

## 18. Summary

Every object in Java ultimately inherits from the `Object` class, gaining baseline methods like `toString()`, `equals()`, and `hashCode()` — methods that are frequently overridden to provide meaningful, content-based behavior instead of Java's default reference-based versions, with `equals()` and `hashCode()` bound together by a strict contract that must be respected to avoid subtle bugs in hash-based collections. Beyond these foundational methods, this chapter covered key design decisions in object-oriented Java: choosing **composition** (a flexible "HAS-A" relationship) over **inheritance** (a rigid "IS-A" relationship) where appropriate, and understanding the finer-grained distinctions between **association**, **aggregation**, and **composition** as object relationships based on ownership and lifecycle dependency. Finally, more specialized class structures — **nested classes** (static, inner, local, anonymous), **anonymous classes** as concise one-off implementations, and **sealed classes** as a modern tool for precisely controlling a type's hierarchy — round out a more complete, professional-level understanding of object-oriented design in Java.

---

## 19. Key Takeaways

- Every class implicitly extends `Object`, inheriting `toString()`, `equals()`, `hashCode()`, `getClass()`, `clone()`, and thread-related methods.
- `equals()` defaults to reference comparison (like `==`) unless overridden.
- Overriding `equals()` requires also overriding `hashCode()`, using the same fields, to satisfy their contract.
- Composition ("HAS-A") is generally preferred over inheritance ("IS-A") for flexibility, but inheritance suits genuine IS-A relationships.
- Aggregation implies independent lifecycles; composition (as a relationship) implies dependent lifecycles.
- Java has four kinds of nested classes: static nested, inner, local inner, and anonymous.
- Anonymous classes provide quick, one-off implementations but can't be reused and can hold state, unlike lambdas.
- Sealed classes (`sealed`, `permits`, `non-sealed`, `final`) give precise, explicit control over a type's permitted subclasses.

---

## 20. Cheat Sheet

| Concept                               | Quick Reference                                                    |
| ------------------------------------- | ------------------------------------------------------------------ |
| **`Object`**                          | Root of every Java class hierarchy                                 |
| **`toString()`**                      | Human-readable string; override for meaningful output              |
| **`equals()`**                        | Content equality; default is reference equality                    |
| **`hashCode()`**                      | Used by hash-based collections; must be consistent with `equals()` |
| **equals/hashCode contract**          | Equal objects → same hash code (reverse not required)              |
| **Composition**                       | "HAS-A"; flexible, loosely coupled                                 |
| **Inheritance**                       | "IS-A"; tightly coupled, use for genuine hierarchies               |
| **Association**                       | General connection; no ownership                                   |
| **Aggregation**                       | "HAS-A"; weak ownership, independent lifecycle                     |
| **Composition (relationship)**        | "HAS-A"; strong ownership, dependent lifecycle                     |
| **Static Nested Class**               | No outer instance needed                                           |
| **Inner Class**                       | Requires outer instance; accesses its members                      |
| **Local Inner Class**                 | Defined inside a method body                                       |
| **Anonymous Class**                   | No name; one-off implementation; can hold state                    |
| **Lambda**                            | Concise; only for functional interfaces; no own state              |
| **`sealed` / `permits`**              | Explicitly restrict which classes may extend/implement             |
| **`non-sealed`**                      | Reopens a sealed hierarchy at that subclass                        |
| **`final` (on a permitted subclass)** | Closes that branch of the hierarchy completely                     |

---

_End of Notes — Advanced Object-Oriented Programming Concepts_
