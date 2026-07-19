# 06. Method Overriding in Java

---

## 1. Introduction

- **What is Method Overriding?** Method overriding occurs when a child class provides its **own specific implementation** of a method that is already defined in its parent class, using the exact same method signature — effectively replacing the parent's version when called on a child class object.
- **Why is Method Overriding Needed?** Inheritance allows a child class to reuse a parent's methods as-is, but sometimes a child class needs to **behave differently** for a particular inherited method. Overriding allows exactly this — customizing or completely redefining specific inherited behavior while keeping the same method name and calling convention.
- **Importance in Object-Oriented Programming:** Method overriding is the mechanism that makes **runtime polymorphism** possible — one of the most powerful features of OOP, allowing different objects to respond to the exact same method call in their own distinct ways.
- **Real-world Analogy:** Think of a general "make payment" instruction at a store. A `CashPayment` handles it by counting physical currency, a `CardPayment` handles it by processing a card transaction, and a `UPIPayment` handles it via a mobile app — the _instruction_ ("make payment") stays the same, but each specific payment method **overrides** exactly _how_ that instruction is carried out.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand the concept of method overriding.
- Differentiate overriding from overloading.
- Explain runtime polymorphism.
- Use the `@Override` annotation correctly.
- Understand Java's overriding rules.

---

## 3. Prerequisites

_(Quick Revision)_

- **Inheritance:** The mechanism allowing a child class to acquire the fields and methods of a parent class.
- **`extends` Keyword:** Establishes the inheritance relationship between a child class and its parent.
- **Parent Class:** The existing class being inherited from.
- **Child Class:** The new class that inherits from, and can extend or customize, the parent class.
- **`super` Keyword:** Used within a child class to explicitly access the immediate parent class's members, including calling the parent's version of an overridden method.

---

## 4. What is Method Overriding?

**Definition:** Method overriding is the process by which a child class redefines a method that it has inherited from its parent class, using the **exact same method name, parameter list, and (compatible) return type** — the child's version replaces the parent's version specifically when that method is called on a child class object.

**Concept:** Overriding lets a child class say, "I inherit this behavior from my parent, but I need to do it _differently_ for my own specific case," without changing the method's name or how it's called from the outside.

**How it Works:** When an overridden method is called on a child class object, Java determines **at runtime** which version of the method to actually execute — the parent's or the child's — based on the object's **actual runtime type**, not the type of the reference variable used to call it.

**Purpose:** Overriding enables **runtime polymorphism**, allowing different classes in an inheritance hierarchy to respond to the same method call in ways specific to each class, while still being treated uniformly through their shared parent type.

**Examples:**

```java
class Animal {
    void makeSound() {
        System.out.println("Animal makes a generic sound.");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();   // reference type: Animal, actual object: Dog
        a.makeSound();           // "Dog barks." — the CHILD's version runs
    }
}
```

**Output:**

```
Dog barks.
```

---

## 5. Why Do We Need Method Overriding?

- **Customizing inherited behavior:** A child class can adapt a general behavior it inherited from its parent to better suit its own specific characteristics, without needing to write an entirely new, differently-named method.
- **Runtime polymorphism:** Overriding is what allows a single method call to produce different, context-appropriate behavior depending on the actual object type — the foundation of writing flexible, general-purpose code that works correctly across many related classes.
- **Flexibility:** New child classes can be introduced later, each providing their own version of a shared method, without needing to modify any existing code that calls that method.
- **Extensibility:** Systems built around overriding can grow to support new object types simply by adding new child classes with their own overridden behavior, rather than modifying existing logic.

**Real-world examples:**

- A `Shape` class defines a general `calculateArea()` method; `Circle`, `Rectangle`, and `Triangle` each override it with their own specific area formula.
- A `PaymentMethod` class defines `processPayment()`; `CreditCard`, `PayPal`, and `BankTransfer` each override it with their own payment-processing logic.

---

## 6. How Method Overriding Works

```
Parent Method
      ↓
Child Overrides
      ↓
Method Called
      ↓
Child Version Executes
```

**Memory Diagram:**

```
Heap:
┌───────────────────────────┐
│         Dog Object            │
│  ┌─────────────────────┐   │
│  │ makeSound() — OVERRIDDEN│   │   ← this version is what actually runs
│  │  (Dog's own version)     │   │
│  └─────────────────────┘   │
└───────────────────────────┘
        ▲
        │
Stack:  a (declared as Animal) ───┘
        (but points to a Dog object at runtime)

When a.makeSound() is called:
   JVM looks at the ACTUAL object type (Dog) — not the reference type (Animal) —
   and executes Dog's overridden version.
```

This runtime lookup process (deciding which version of an overridden method to actually run, based on the object's real type) is often referred to as **dynamic method dispatch**.

---

## 7. Syntax of Method Overriding

**General Syntax:**

```java
class Parent {
    returnType methodName(parameterList) {
        // parent implementation
    }
}

class Child extends Parent {
    @Override
    returnType methodName(parameterList) {
        // child's own implementation
    }
}
```

**Example:**

```java
class Shape {
    double calculateArea() {
        return 0;
    }
}

class Circle extends Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

**Explanation:** `Circle`'s `calculateArea()` has the **exact same method name and return type** as `Shape`'s version, but provides its own specific implementation, correctly calculating the area of a circle rather than returning the parent's generic (in this case, placeholder) value.

---

## 8. The `@Override` Annotation

**Definition:** `@Override` is an annotation placed directly above a method to explicitly indicate that it is intended to override a method from its parent class (or implement a method from an interface).

**Purpose:** It tells the Java compiler to **verify** that the annotated method actually does correctly override an existing parent method — catching a range of common mistakes at compile time rather than allowing them to fail silently.

**Advantages:**

- Provides immediate compile-time verification that the overriding method's signature genuinely matches the parent's method.
- Makes the programmer's _intent_ explicit and clear to anyone reading the code.
- Helps catch subtle bugs — like an accidental typo in the method name, or a slightly different parameter list — that would otherwise silently create a brand-new, unrelated method (accidental overloading) instead of a true override.

**Examples:**

```java
class Animal {
    void makeSound() {
        System.out.println("Generic sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {   // correctly overrides — compiles fine
        System.out.println("Bark");
    }
}
```

**Compiler Benefits:**

```java
class Dog extends Animal {
    @Override
    void makeSoundd() {   // typo in method name!
        System.out.println("Bark");
    }
}
// Compile Error: method does not override a method from its superclass
// Without @Override, this typo would silently create an unrelated new method,
// and Dog objects would still call Animal's original makeSound() unexpectedly.
```

---

## 9. Rules of Method Overriding

- **Same method name** — the overriding method in the child class must have exactly the same name as the method in the parent class.
- **Same parameter list** — the number, order, and types of parameters must match exactly; any difference results in overloading instead of overriding.
- **Same return type (or covariant return type)** — the return type must either be identical, or a subtype of the parent method's return type (see Section 10).
- **Cannot reduce access level** — the overriding method's access modifier must be the same as, or more accessible than, the parent method's (e.g., a `protected` method can be overridden as `protected` or `public`, but not as `private` or default).
- **Cannot override private methods** — private methods are not inherited at all, so they cannot be overridden by a child class; a same-named private method in the child is treated as an entirely separate, unrelated method.
- **Cannot override final methods** — a method marked `final` in the parent class is explicitly locked against being overridden by any child class.
- **Static methods are hidden, not overridden** — a static method with the same signature in a child class does not override the parent's version; it _hides_ it instead, with resolution happening based on the reference type at compile time, not the object's actual runtime type (see Section 12).
- **Constructors cannot be overridden** — constructors are not inherited at all, so the concept of overriding doesn't apply to them (see Section 14).

**Examples:**

```java
class Parent {
    protected void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {
    @Override
    public void display() {   // OK — access level widened from protected to public
        System.out.println("Child display");
    }
}

class InvalidChild extends Parent {
    // @Override
    // private void display() {   // Compile Error: cannot reduce visibility from protected to private
    // }
}
```

---

## 10. Covariant Return Types

**Definition:** A covariant return type allows an overriding method to return a **subtype** of the return type declared in the parent class's original method, rather than requiring an exact type match.

**Why Java Allows It:** Since any object of the subtype is also a valid object of the parent's declared return type (due to the IS-A relationship from inheritance), returning a more specific subtype is always safe and doesn't break the contract established by the parent method.

**Examples:**

```java
class Animal {
    Animal reproduce() {
        return new Animal();
    }
}

class Dog extends Animal {
    @Override
    Dog reproduce() {   // covariant return type — Dog is a subtype of Animal
        return new Dog();
    }
}
```

This is valid because `Dog` **IS-A(n)** `Animal` — anywhere an `Animal` is expected, a `Dog` object works perfectly well, so returning the more specific `Dog` type instead of the general `Animal` type is completely safe.

```java
class Shape {
    Shape getCopy() {
        return new Shape();
    }
}

class Circle extends Shape {
    @Override
    Circle getCopy() {   // covariant — Circle is a subtype of Shape
        return new Circle();
    }
}
```

---

## 11. Access Modifiers in Overriding

**`private`:** Private methods cannot be overridden at all, since they are not inherited by child classes in the first place.

**default (no modifier):** A default-access method can be overridden by a child class **within the same package**, but not from a subclass in a different package (since default access itself isn't visible across packages).

**`protected`:** A protected method can be overridden and its access level can be widened to `protected` or `public` in the child class, but not narrowed.

**`public`:** A public method, being the most accessible level, must remain `public` when overridden — it cannot be narrowed to `protected`, default, or `private`.

**Rules:** The overriding method's access modifier must be **equal to or more accessible than** the parent method's — never less accessible.

**Examples:**

```java
class Parent {
    public void show() { }
}

class Child extends Parent {
    @Override
    public void show() { }   // OK — same access level (public)

    // @Override
    // protected void show() { }   // Compile Error: cannot reduce visibility from public to protected
}
```

---

## 12. Static Methods vs Instance Methods

**Method Hiding:** When a child class declares a `static` method with the exact same signature as a `static` method in its parent class, the child's version does **not** override the parent's — it **hides** it instead. Unlike true overriding, which is resolved at runtime based on the object's actual type, method hiding is resolved at **compile time**, based on the reference variable's declared type.

**Difference:**

```java
class Parent {
    static void staticMethod() {
        System.out.println("Parent static method");
    }

    void instanceMethod() {
        System.out.println("Parent instance method");
    }
}

class Child extends Parent {
    static void staticMethod() {
        System.out.println("Child static method");
    }

    @Override
    void instanceMethod() {
        System.out.println("Child instance method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();

        p.staticMethod();     // "Parent static method" — resolved by REFERENCE TYPE (compile time)
        p.instanceMethod();   // "Child instance method" — resolved by ACTUAL OBJECT TYPE (runtime)
    }
}
```

**Output:**

```
Parent static method
Child instance method
```

**Examples (key distinction):** This difference is a very common source of confusion and a frequent interview/exam question — instance method calls use **dynamic (runtime) dispatch**, while static method calls are resolved **statically (at compile time)**, based purely on the reference variable's declared type, completely ignoring the object's actual runtime type.

---

## 13. Final Methods

**Definition:** A method declared with the `final` keyword in a parent class cannot be overridden by any child class.

**Why final methods cannot be overridden:** Marking a method `final` is a deliberate design decision indicating that its behavior must remain **exactly the same** across every subclass — Java enforces this at compile time, ensuring no child class can accidentally or intentionally alter that specific behavior.

**Examples:**

```java
class Vehicle {
    final void startEngine() {
        System.out.println("Engine starting in the standard way.");
    }
}

class Car extends Vehicle {
    // @Override
    // void startEngine() {   // Compile Error: cannot override the final method from Vehicle
    //     System.out.println("Custom engine start.");
    // }
}
```

---

## 14. Constructor vs Method Overriding

**Why constructors cannot be overridden:** Constructors are tied specifically to the class in which they're defined and are never inherited by child classes at all — since overriding fundamentally requires a method to first be **inherited** by the child class, and constructors aren't inherited, the concept of "overriding a constructor" doesn't apply in Java.

**Examples:**

```java
class Parent {
    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    Child() {
        // this is NOT "overriding" Parent's constructor —
        // it's simply Child's own, separate constructor,
        // which happens to implicitly call super() first
        System.out.println("Child constructor");
    }
}
```

What a child class constructor _can_ do is call a parent constructor explicitly via `super(...)` (constructor chaining, covered in the `super` keyword chapter) — but this is fundamentally different from overriding, since both constructors still run, in sequence, rather than the child's replacing the parent's entirely.

---

## 15. Runtime Method Selection

```
Compile Time
      ↓
Runtime
      ↓
Correct Method Executes
```

**Flow Diagram:**

```
        ┌─────────────────────────┐
        │  a.makeSound() written     │
        │  (a declared as Animal)     │
        └────────────┬────────────────┘
                     ▼
        ┌─────────────────────────┐
        │  COMPILE TIME:              │
        │  Compiler checks that        │
        │  Animal HAS a makeSound()    │
        │  method (signature check)    │
        └────────────┬────────────────┘
                     ▼
        ┌─────────────────────────┐
        │  RUNTIME:                    │
        │  JVM checks the ACTUAL        │
        │  object type (e.g., Dog)      │
        └────────────┬────────────────┘
                     ▼
        ┌─────────────────────────┐
        │  Dog's overridden           │
        │  makeSound() executes         │
        └─────────────────────────┘
```

This two-phase process — compile-time signature checking, followed by runtime dispatch to the actual overriding method — is what makes runtime (dynamic) polymorphism possible in Java.

---

## 16. Memory Representation

```
Stack
   ↓
Reference Variable
   ↓
Heap Object
   ↓
Overridden Method
```

**Memory Diagram:**

```
Stack:                          Heap:
  a (type: Animal) ─────────▶  ┌────────────────────────┐
                                 │       Dog Object           │
                                 │  ┌──────────────────┐   │
                                 │  │ makeSound()           │   │ ← Dog's OWN, overridden version
                                 │  │ (overridden)           │   │    is what's actually stored/used
                                 │  └──────────────────┘   │
                                 └────────────────────────┘

a.makeSound() → JVM follows the reference to the actual Dog object on the heap,
                and executes the method found THERE (Dog's version),
                completely regardless of 'a' being declared as type Animal.
```

---

## 17. Method Overriding vs Method Overloading

| Aspect                | Method Overriding                                               | Method Overloading                                                                                              |
| --------------------- | --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Definition            | Child class redefines a parent's method with the same signature | Multiple methods in the same class (or same inheritance level) with the same name but different parameter lists |
| Requires inheritance? | Yes                                                             | No                                                                                                              |
| Method signature      | Must be identical (same name, same parameters)                  | Must differ in parameter list (number, type, or order)                                                          |
| Return type           | Same or covariant                                               | Can be different (as long as parameter lists differ)                                                            |
| Resolved at           | Runtime (dynamic dispatch)                                      | Compile time (static dispatch)                                                                                  |
| Polymorphism type     | Runtime polymorphism                                            | Compile-time polymorphism                                                                                       |
| Access modifier rule  | Cannot reduce visibility                                        | No such restriction — access can differ freely                                                                  |
| Purpose               | Customize/redefine inherited behavior                           | Provide multiple ways to call a similarly-purposed method                                                       |

---

## 18. Advantages of Method Overriding

- **Runtime Polymorphism:** Enables a single method call to behave differently depending on the actual object type, which is foundational to flexible, extensible OOP design.
- **Flexibility:** Allows child classes to adapt inherited behavior to their own specific needs without changing the method's external interface.
- **Extensibility:** New child classes with their own overridden behavior can be added later without modifying existing code that relies on the parent type.
- **Reusability:** The general structure and method signature defined in the parent class is reused across all child classes, even as the specific implementation varies.
- **Better Design:** Encourages designing systems around shared, general interfaces (parent classes/methods) while allowing specialized behavior where needed — a hallmark of well-structured OOP code.

---

## 19. Disadvantages

- **Slight Runtime Overhead:** Dynamic method dispatch (determining which overridden version to call at runtime) introduces a small amount of overhead compared to a direct, statically-resolved method call — though this is generally negligible in practice.
- **Can Reduce Readability if Misused:** Excessive or unclear overriding — especially deep inheritance chains where a method is overridden multiple times at different levels — can make it harder to trace exactly which version of a method will actually execute for a given object.

---

## 20. Real-world Examples

**Animal → Dog:**

```java
class Animal {
    void makeSound() { System.out.println("Some generic sound"); }
}
class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Bark"); }
}
```

**Vehicle → Car:**

```java
class Vehicle {
    void start() { System.out.println("Vehicle starting..."); }
}
class Car extends Vehicle {
    @Override
    void start() { System.out.println("Car engine starting with ignition."); }
}
```

**Employee → Manager:**

```java
class Employee {
    double calculateBonus() { return 1000; }
}
class Manager extends Employee {
    @Override
    double calculateBonus() { return 5000; }
}
```

**BankAccount → SavingsAccount:**

```java
class BankAccount {
    double calculateInterest() { return 0; }
}
class SavingsAccount extends BankAccount {
    @Override
    double calculateInterest() { return 500; }  // e.g., based on balance * rate
}
```

**Shape → Circle:**

```java
class Shape {
    double calculateArea() { return 0; }
}
class Circle extends Shape {
    double radius = 5;
    @Override
    double calculateArea() { return Math.PI * radius * radius; }
}
```

---

## 21. Best Practices

- **Always use `@Override`** — it provides valuable compile-time verification and clearly communicates intent to other developers.
- **Override only when necessary** — don't override a method just because you can; only do so when the child class genuinely needs different behavior.
- **Call `super.method()` if parent behavior is needed** — when a child class's override should _extend_ rather than completely replace the parent's logic, explicitly call the parent's version first.
- **Keep overridden methods meaningful** — the overriding method should still fulfill the same general _purpose_ as the parent's version, just with specialized behavior.
- **Follow the Liskov Substitution Principle** — an overriding method should be usable anywhere the parent's method is expected, without breaking the correctness of the program (i.e., a child object should be safely substitutable for a parent object).

---

## 22. Common Mistakes

1. **Different parameter list (becomes overloading, not overriding):**

   ```java
   class Parent {
       void show(int x) { }
   }
   class Child extends Parent {
       void show(int x, int y) { }   // NOT overriding — this is a separate, overloaded method
   }
   ```

2. **Wrong access modifier (attempting to reduce visibility):**

   ```java
   class Parent {
       public void display() { }
   }
   class Child extends Parent {
       // protected void display() { }   // Compile Error: cannot reduce visibility
   }
   ```

3. **Overriding final methods:**

   ```java
   class Parent {
       final void lockedMethod() { }
   }
   class Child extends Parent {
       // void lockedMethod() { }   // Compile Error: cannot override a final method
   }
   ```

4. **Expecting constructors to override:**
   Constructors are never inherited, so there is no such thing as "overriding" a constructor — each class's constructor is entirely its own.

5. **Forgetting `@Override`:**
   Omitting `@Override` doesn't cause a compile error by itself, but it removes the valuable safety net that would catch typos or signature mismatches — a small typo in the method name would silently create an unrelated new method instead of properly overriding the intended one.

---

## 23. Interview Corner

**Q1. What is method overriding?**
Method overriding is when a child class provides its own implementation of a method already defined in its parent class, using the exact same method signature, and that implementation is used instead of the parent's whenever the method is called on a child class object.

**Q2. Why is overriding used?**
It allows a child class to customize or completely redefine inherited behavior, and is the mechanism that enables runtime polymorphism, allowing different object types to respond to the same method call in their own distinct ways.

**Q3. Difference between overriding and overloading?**
Overriding requires inheritance and an identical method signature, with resolution happening at runtime based on the object's actual type; overloading occurs within the same class (or level) with methods sharing a name but different parameter lists, resolved entirely at compile time.

**Q4. Can private methods be overridden?**
No — private methods are not inherited by child classes at all, so they cannot be overridden; a same-named private method in a child class is treated as a completely separate, unrelated method.

**Q5. Can constructors be overridden?**
No — constructors are never inherited, and overriding fundamentally requires inheritance of the method being overridden, so the concept doesn't apply to constructors at all.

**Q6. Can final methods be overridden?**
No — a method marked `final` in a parent class is explicitly locked against being overridden by any child class; attempting to do so results in a compile-time error.

**Q7. What is method hiding?**
Method hiding occurs when a child class declares a `static` method with the same signature as a static method in its parent class — unlike true overriding, this is resolved at compile time based on the reference variable's declared type, not the object's actual runtime type.

**Q8. What is the purpose of `@Override`?**
It instructs the compiler to verify that the annotated method genuinely overrides an existing parent method, catching signature mismatches or typos at compile time rather than silently creating an unrelated new method.

**Q9. What is a covariant return type?**
It's when an overriding method's return type is a subtype of the return type declared in the parent's original method — allowed because any object of that subtype is still a valid instance of the parent's declared return type.

---

## 24. MCQs

**1. What is required for method overriding to occur?**
a) Method overloading
b) Inheritance
c) Static methods only
d) Constructors
**Answer: b) Inheritance**

**2. What must remain identical between an overriding method and the method it overrides?**
a) Access modifier only
b) Method name and parameter list
c) Method body
d) Variable names inside the method
**Answer: b) Method name and parameter list**

**3. Can access visibility be reduced when overriding a method?**
a) Yes, always
b) No, it can only stay the same or be widened
c) Only for private methods
d) Only for static methods
**Answer: b) No, it can only stay the same or be widened**

**4. Can a `final` method be overridden?**
a) Yes
b) No
c) Only in the same package
d) Only if declared protected
**Answer: b) No**

**5. What happens when a static method with the same signature is declared in a child class?**
a) It overrides the parent's method
b) It hides the parent's method, resolved at compile time
c) It causes a compile error
d) It is ignored
**Answer: b) It hides the parent's method, resolved at compile time**

**6. What does the `@Override` annotation do?**
a) Changes the method's return type
b) Instructs the compiler to verify the method truly overrides a parent method
c) Makes the method final
d) Prevents inheritance
**Answer: b) Instructs the compiler to verify the method truly overrides a parent method**

**7. What is a covariant return type?**
a) A return type identical to the parameter type
b) A return type that is a subtype of the parent method's declared return type
c) A return type that must always be void
d) A return type only allowed in interfaces
**Answer: b) A return type that is a subtype of the parent method's declared return type**

**8. Can private methods be overridden?**
a) Yes, always
b) No, since they are not inherited
c) Only if declared final
d) Only in the same class
**Answer: b) No, since they are not inherited**

**9. What kind of polymorphism does method overriding enable?**
a) Compile-time polymorphism
b) Runtime polymorphism
c) Static polymorphism
d) No polymorphism
**Answer: b) Runtime polymorphism**

**10. What is the output of this code?**

```java
class Parent {
    static void show() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void show() { System.out.println("Child"); }
}
Parent p = new Child();
p.show();
```

a) Child
b) Parent
c) Compile error
d) Runtime exception
**Answer: b) Parent** (static method resolution is based on reference type, not object type)

---

## 25. University Questions

### Short Questions

1. What is method overriding? Give an example.
2. What is the difference between method overriding and method overloading?
3. Why can't static methods be truly overridden?
4. What is a covariant return type?
5. Why can't constructors be overridden?

### Long Questions

1. Explain method overriding in Java with a complete program example and its rules.
2. Discuss the difference between method overriding and method hiding, with a program example demonstrating both.
3. Explain the role of the `@Override` annotation, including the compile-time benefits it provides, with examples.
4. Discuss the rules governing access modifiers in method overriding, with examples of valid and invalid cases.
5. Explain runtime polymorphism and how method overriding enables it, using a suitable real-world example (e.g., a Shape hierarchy).

---

## 26. Viva Questions

1. What is dynamic method dispatch?
2. Why is `@Override` recommended even though it's optional?
3. Can the return type of an overriding method differ from the parent's? Under what condition?
4. What happens if you try to reduce the access level of an overridden method?
5. What is the difference between overriding and method hiding for static methods?
6. Why doesn't Java allow overriding of `final` methods?
7. Can an overriding method throw new or broader checked exceptions than the parent method? (Conceptually, why or why not?)
8. What is the relationship between method overriding and the Liskov Substitution Principle?
9. Does overriding require the child class to be in the same package as the parent?
10. What role does `super.method()` play when working with overridden methods?

---

## 27. Programming Exercises

**1. Animal → Dog**

Create an `Animal` class with a `makeSound()` method, and a `Dog` class that overrides it with its own implementation.

**2. Vehicle → Car**

Create a `Vehicle` class with a `start()` method, and a `Car` class that overrides it, also calling `super.start()` to extend the parent's behavior.

**3. Employee → Manager**

Create an `Employee` class with a `calculateBonus()` method, and a `Manager` class that overrides it with a higher bonus calculation.

**4. BankAccount → SavingsAccount**

Create a `BankAccount` class with a `calculateInterest()` method, and a `SavingsAccount` class that overrides it with an actual interest calculation based on balance and rate.

**5. Shape → Circle**

Create a `Shape` class with a `calculateArea()` method, and a `Circle` class that overrides it with the correct area formula.

_(All exercises should include a `Main` class demonstrating that the overridden method executes correctly when called through a parent-type reference.)_

---

## 28. Challenge Problems

**Build overriding examples for:**

- **Payment System:** Design a `PaymentMethod` class with a `processPayment()` method, and child classes `CreditCardPayment`, `UPIPayment`, and `CashPayment`, each overriding it with their own processing logic.
- **Online Shopping:** Design a `Discount` class with an `applyDiscount()` method, and child classes for different discount types (e.g., `PercentageDiscount`, `FlatDiscount`), each overriding it differently.
- **Hospital Management:** Design a `Staff` class with a `performDuty()` method, and child classes `Doctor` and `Nurse`, each overriding it with their own specific duties.
- **Library Management:** Design a `LibraryItem` class with a `calculateLateFee()` method, and child classes `Book` and `DVD`, each overriding it with different fee calculations.
- **Employee Payroll:** Design an `Employee` class with a `calculateSalary()` method, and child classes `FullTimeEmployee` and `PartTimeEmployee`, each overriding it with their own salary calculation logic.

---

## 29. Summary

- **Method overriding** allows a child class to provide its own implementation of a method it inherited from its parent, using the exact same method signature.
- Overriding requires **inheritance**, and the overriding method must match the parent's method name, parameter list, and return type (or a covariant subtype of it).
- The overriding method's access level cannot be **reduced** compared to the parent's version.
- **Private, final, and static** methods each interact differently with overriding: private methods can't be overridden at all (not inherited), final methods are explicitly locked against overriding, and static methods are _hidden_ rather than truly overridden.
- The **`@Override`** annotation provides valuable compile-time verification and should be used consistently for clarity and safety.
- Method overriding is the foundational mechanism behind **runtime polymorphism**, resolved via dynamic method dispatch based on an object's actual type at runtime.

---

## 30. Key Takeaways

- Method overriding allows a child class to provide its own implementation.
- It requires inheritance.
- The method signature must remain the same.
- Overriding enables runtime polymorphism.
- Use `@Override` to improve readability and compiler checking.

---

## 31. Cheat Sheet

**Definition:**

```
Overriding = child class redefines a parent's method,
             using the SAME signature, resolved at RUNTIME
```

**Syntax:**

```java
class Parent {
    returnType methodName(params) { }
}
class Child extends Parent {
    @Override
    returnType methodName(params) { }   // OR a covariant subtype return
}
```

**Rules (quick list):**

```
✔ Same method name
✔ Same parameter list
✔ Same OR covariant return type
✔ Access level: same or WIDER (never narrower)
✘ Cannot override private methods (not inherited)
✘ Cannot override final methods
✘ Static methods are HIDDEN, not overridden
✘ Constructors cannot be overridden
```

**`@Override`:**

```java
@Override
void methodName() { }
// → compiler VERIFIES this truly overrides a parent method
```

**Covariant Return Example:**

```java
class Animal { Animal reproduce() { return new Animal(); } }
class Dog extends Animal {
    @Override
    Dog reproduce() { return new Dog(); }   // Dog is a subtype of Animal — valid
}
```

**Comparison Table (Overriding vs Overloading):**

| Aspect               | Overriding | Overloading              |
| -------------------- | ---------- | ------------------------ |
| Requires inheritance | Yes        | No                       |
| Signature            | Identical  | Different parameter list |
| Resolved at          | Runtime    | Compile time             |
| Polymorphism type    | Runtime    | Compile-time             |

**Best Practices (quick list):**

```
✔ Always use @Override
✔ Override only when genuinely needed
✔ Use super.method() to extend, not just replace, parent behavior
✔ Follow the Liskov Substitution Principle
```
