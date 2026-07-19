# 05. The `super` Keyword in Java

---

## 1. Introduction

- **What is the `super` Keyword?** `super` is a special reference in Java used within a child class to refer to its **immediate parent class** — allowing access to the parent's variables, methods, and constructors from within the child class.
- **Why is `super` Needed?** When a child class overrides a method or redeclares a variable with the same name as one in its parent, the child's own version normally takes precedence. `super` provides an explicit way to reach past that and access the parent class's original version when it's genuinely needed.
- **Importance of `super` in Inheritance:** `super` is essential for properly reusing and extending inherited functionality — it enables calling a parent's constructor to ensure proper initialization, invoking a parent's overridden method as part of a child's own implementation, and resolving ambiguity when a child class hides a parent's variable.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand the purpose of the `super` keyword.
- Access superclass variables using `super`.
- Call superclass methods using `super`.
- Invoke superclass constructors using `super()`.
- Differentiate between `this` and `super`.

---

## 3. What is the `super` Keyword?

**Definition:** `super` is a reference keyword in Java that a child class uses to explicitly refer to a member (variable, method, or constructor) belonging to its **immediate parent class**, rather than to the child class's own version of that member.

**Current Parent Object Reference:** Conceptually, `super` behaves similarly to `this`, except that while `this` refers to the current object as itself, `super` refers to the current object **viewed as an instance of its parent class** — giving access specifically to the parent's portion of the object.

**Purpose:** `super` exists to resolve situations where a child class's own members (fields or methods) would otherwise **hide or override** the parent's version, giving the programmer explicit, controlled access to the parent's original implementation when needed.

**Where it can be used:**

- `super.variableName` — to access a parent class's field.
- `super.methodName()` — to call a parent class's method.
- `super(arguments)` — to call a parent class's constructor (only valid as the first statement inside a constructor).

---

## 4. Why Do We Need `super`?

**Variable Hiding:** When a child class declares a field with the **same name** as a field in its parent class, the child's field "hides" the parent's field within the child class's own scope — `super.fieldName` is needed to access the parent's version explicitly.

**Method Overriding:** When a child class **overrides** a parent's method, calling that method normally invokes the child's version — `super.methodName()` allows the child to still call the parent's original implementation, often as part of extending (rather than completely replacing) that behavior.

**Constructor Chaining:** Since constructors are not inherited, `super(...)` is used to explicitly invoke a parent class's constructor from within a child class's constructor, ensuring the parent portion of the object is properly initialized before the child's own initialization runs.

**Examples:**

```java
class Vehicle {
    String type = "Generic Vehicle";

    void start() {
        System.out.println("Vehicle starting...");
    }
}

class Car extends Vehicle {
    String type = "Car";   // hides Vehicle's 'type' field

    void start() {
        super.start();                 // calls Vehicle's start() first
        System.out.println("Car engine roaring to life!");
    }

    void showTypes() {
        System.out.println("Child type: " + type);         // Car
        System.out.println("Parent type: " + super.type);   // Generic Vehicle
    }
}
```

---

## 5. Using `super` to Access Parent Variables

**Definition:** `super.variableName` is used to explicitly access a field defined in the parent class, specifically when the child class has declared a field of the **same name**, which would otherwise hide the parent's field.

**Syntax:**

```java
super.variableName
```

**Examples:**

```java
class Animal {
    String name = "Generic Animal";
}

class Dog extends Animal {
    String name = "Dog";

    void printNames() {
        System.out.println("Child's name: " + name);          // Dog
        System.out.println("Parent's name: " + super.name);   // Generic Animal
    }
}
```

**Output:**

```
Child's name: Dog
Parent's name: Generic Animal
```

**Memory Diagram:**

```
Heap:
┌───────────────────────────────┐
│           Dog Object              │
│  ┌───────────────────────────┐  │
│  │  Parent's field (Animal):    │  │
│  │   name = "Generic Animal"    │  │  ← accessed via super.name
│  └───────────────────────────┘  │
│  ┌───────────────────────────┐  │
│  │  Child's own field (Dog):    │  │
│  │   name = "Dog"                │  │  ← accessed via plain 'name' (or this.name)
│  └───────────────────────────┘  │
└───────────────────────────────┘
```

Both fields **actually exist simultaneously** within the same `Dog` object — the child's field doesn't overwrite or delete the parent's field, it merely "hides" it within the child class's own naming scope. `super` is what lets the program reach the hidden, parent-level copy.

---

## 6. Using `super` to Call Parent Methods

**Definition:** `super.methodName()` is used to explicitly call a method defined in the parent class, specifically when the child class has **overridden** that method with its own implementation.

**Syntax:**

```java
super.methodName(arguments);
```

**Examples:**

```java
class Employee {
    void work() {
        System.out.println("Employee is working.");
    }
}

class Manager extends Employee {
    void work() {
        super.work();   // calls Employee's version first
        System.out.println("Manager is also supervising the team.");
    }
}
```

**Output (when `work()` is called on a `Manager` object):**

```
Employee is working.
Manager is also supervising the team.
```

**Method Overriding Example — extending rather than completely replacing behavior:**

```java
class Shape {
    void draw() {
        System.out.println("Drawing a generic shape.");
    }
}

class Circle extends Shape {
    @Override
    void draw() {
        super.draw();   // still perform the generic drawing setup
        System.out.println("Drawing a circle specifically.");
    }
}
```

**Output:**

```
Drawing a generic shape.
Drawing a circle specifically.
```

This pattern — calling `super.methodName()` as the first action inside an overriding method — is a very common and useful technique for **extending** a parent's behavior rather than fully replacing it.

---

## 7. Using `super()` to Call Parent Constructors

**Definition:** `super()` (with parentheses) is used inside a child class's constructor to explicitly call a constructor of its parent class, ensuring the parent portion of the object is properly initialized.

**Syntax:**

```java
super();                  // calls the parent's no-argument constructor
super(argument1, argument2, ...);  // calls a specific parameterized parent constructor
```

**Execution Order:** If a child class constructor doesn't explicitly call `super(...)`, Java **automatically** inserts an implicit call to the parent's no-argument constructor `super()` as the very first statement. If the parent class doesn't have a no-argument constructor available, the child class **must** explicitly call one of the parent's available parameterized constructors using `super(...)`, or a compile-time error occurs.

**Examples — implicit `super()`:**

```java
class Animal {
    Animal() {
        System.out.println("Animal constructor called.");
    }
}

class Dog extends Animal {
    Dog() {
        // super(); is implicitly inserted here automatically
        System.out.println("Dog constructor called.");
    }
}
```

**Output:**

```
Animal constructor called.
Dog constructor called.
```

**Examples — explicit `super(...)` with arguments:**

```java
class Animal {
    String name;

    Animal(String name) {
        this.name = name;
        System.out.println("Animal constructor called for: " + name);
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);   // explicitly calls Animal's parameterized constructor
        System.out.println("Dog constructor called for: " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog("Rex");
    }
}
```

**Output:**

```
Animal constructor called for: Rex
Dog constructor called for: Rex
```

> Note: Since `Animal` here has **only** a parameterized constructor (no no-argument constructor), `Dog`'s constructor **must** explicitly call `super(name)` — omitting it would cause a compile-time error, since Java would otherwise try (and fail) to insert an implicit `super()` call to a non-existent no-argument `Animal()` constructor.

---

## 8. Constructor Chaining

```
Parent Constructor
        ↓
Child Constructor
```

**Rules:**

- Every child class constructor, either implicitly or explicitly, calls a parent class constructor as its **very first action**.
- This chaining continues all the way up the inheritance hierarchy — if `C extends B extends A`, creating a `C` object triggers `A`'s constructor first, then `B`'s, then finally `C`'s own constructor body.

**Examples — multilevel constructor chaining:**

```java
class A {
    A() {
        System.out.println("A's constructor");
    }
}

class B extends A {
    B() {
        System.out.println("B's constructor");
    }
}

class C extends B {
    C() {
        System.out.println("C's constructor");
    }
}

public class Main {
    public static void main(String[] args) {
        C obj = new C();
    }
}
```

**Output:**

```
A's constructor
B's constructor
C's constructor
```

**Memory Diagram:**

```
Object creation for 'new C()':

Step 1: A's constructor runs   →  initializes A's portion of the object
Step 2: B's constructor runs   →  initializes B's portion of the object
Step 3: C's constructor runs   →  initializes C's portion of the object

Final result: ONE fully-initialized C object, built up in layers,
              from the top of the hierarchy down to the bottom.
```

---

## 9. Rules of `super`

- **Must be the first statement inside a constructor** — `super(...)` can only appear as the very first line of a constructor; it cannot be preceded by any other statement.
- **Can call only one constructor** — a constructor can contain at most one call to `super(...)` (or `this(...)`, but never both in the same constructor).
- **Cannot be used inside static methods** — since `super` refers to the _current object's_ view of its parent, and static methods don't operate on any particular object instance, `super` has no meaning inside a static context.
- **Can access inherited members** — `super` can be used to access any non-private field or method inherited from the immediate parent class.
- **Refers only to the immediate parent class** — even in a multilevel hierarchy (`A → B → C`), `super` used inside `C` refers only to `B` (its direct parent), not to `A`.

```java
class Demo extends Something {
    Demo() {
        // super();   // OK — must be the FIRST statement if used explicitly
    }

    static void staticMethod() {
        // super.someMethod();   // Compile Error: cannot use 'super' in a static context
    }
}
```

---

## 10. `super` vs `this`

| Aspect                                   | `super`                                                                   | `this`                                                                              |
| ---------------------------------------- | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Refers to                                | The immediate parent class's members                                      | The current object itself                                                           |
| Used for variables                       | `super.variable` — accesses parent's hidden field                         | `this.variable` — accesses current object's own field                               |
| Used for methods                         | `super.method()` — calls parent's version (even if overridden)            | `this.method()` — calls the current object's own (possibly overridden) version      |
| Used for constructors                    | `super(...)` — calls a parent class constructor                           | `this(...)` — calls another constructor in the SAME class (constructor overloading) |
| Position rule                            | Must be the first statement in a constructor, if used                     | Must also be the first statement in a constructor, if used                          |
| Can both appear in the same constructor? | No — a constructor can use either `super(...)` or `this(...)`, never both | No — same restriction applies                                                       |
| Context restriction                      | Cannot be used in static methods                                          | Cannot be used in static methods                                                    |

---

## 11. Constructor Execution Flow

```
Object Creation
      ↓
Parent Constructor
      ↓
Child Constructor
      ↓
Object Ready
```

**Flow Diagram:**

```
        ┌────────────────────┐
        │  new Child() called  │
        └──────────┬────────────┘
                   ▼
        ┌────────────────────┐
        │ super() invoked       │  (implicit or explicit — always first)
        └──────────┬────────────┘
                   ▼
        ┌────────────────────┐
        │ Parent Constructor    │
        │ body executes          │
        └──────────┬────────────┘
                   ▼
        ┌────────────────────┐
        │ Child Constructor      │
        │ body executes (rest)   │
        └──────────┬────────────┘
                   ▼
        ┌────────────────────┐
        │ Object fully ready     │
        └────────────────────┘
```

---

## 12. Memory Representation

```
Stack
   ↓
Reference
   ↓
Child Object
   ↓
Inherited Parent Members
```

**Memory Diagram:**

```
Stack:                          Heap:
  d ─────────────────────▶  ┌───────────────────────── ─┐
                              │        Child Object      │
                              │  ┌────────────────────  ┐│
                              │  │Inherited from Parent:││
                              │  │  (initialized via    ││
                              │  │   super() first)     ││
                              │  └───────────────────── ┘│
                              │  ┌────────────────────┐  │
                              │  │ Child's own members:  │
                              │  │ (initialized after)│  │
                              │  └────────────────────┘  │
                              └──────────────────────────┘
```

The reference variable on the stack points to a single object on the heap, but that object's memory is conceptually laid out in layers — the parent's portion (initialized first, via `super()`) and the child's own portion (initialized afterward), together forming one complete object.

---

## 13. Real-world Examples

**Employee → Manager:**

```java
class Employee {
    String name;
    double baseSalary;

    Employee(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }

    double calculateSalary() {
        return baseSalary;
    }
}

class Manager extends Employee {
    double bonus;

    Manager(String name, double baseSalary, double bonus) {
        super(name, baseSalary);   // initializes Employee's part
        this.bonus = bonus;
    }

    @Override
    double calculateSalary() {
        return super.calculateSalary() + bonus;   // extends parent's calculation
    }
}
```

**Vehicle → Car:**

```java
class Vehicle {
    void start() {
        System.out.println("Vehicle starting...");
    }
}

class Car extends Vehicle {
    @Override
    void start() {
        super.start();
        System.out.println("Car's ignition system engaged.");
    }
}
```

**Animal → Dog:**

```java
class Animal {
    String sound = "Generic sound";

    void makeSound() {
        System.out.println("Animal makes: " + sound);
    }
}

class Dog extends Animal {
    String sound = "Bark";

    void compareSounds() {
        System.out.println("Dog's sound: " + sound);
        System.out.println("Animal's sound: " + super.sound);
    }
}
```

**BankAccount → SavingsAccount:**

```java
class BankAccount {
    double balance;

    BankAccount(double balance) {
        this.balance = balance;
    }

    void deposit(double amount) {
        balance += amount;
    }
}

class SavingsAccount extends BankAccount {
    double interestRate;

    SavingsAccount(double balance, double interestRate) {
        super(balance);   // initializes BankAccount's part
        this.interestRate = interestRate;
    }

    void addInterest() {
        double interest = balance * (interestRate / 100);
        super.deposit(interest);   // reuses parent's deposit logic
    }
}
```

---

## 14. Best Practices

- **Use `super` only when necessary** — reach for it specifically when a child class's own field/method would otherwise hide or override the parent's version and the parent's version is genuinely needed.
- **Call parent constructors to initialize inherited fields** — always ensure a parent class's fields are properly initialized via `super(...)`, rather than trying to reinitialize them redundantly within the child.
- **Avoid unnecessary use of `super`** — don't call `super.method()` or reference `super.field` when there's no actual naming conflict or need to reach past the child's own version; doing so needlessly clutters code.
- **Prefer readability** — when overriding a method and using `super.method()` to extend rather than replace behavior, make the intent clear through good naming and, where helpful, comments.

---

## 15. Common Mistakes

1. **Calling `super()` after another statement:**

   ```java
   class Dog extends Animal {
       Dog() {
           System.out.println("Dog created");
           // super();   // Compile Error: super() must be the FIRST statement
       }
   }
   ```

2. **Confusing `this` with `super`:**

   ```java
   class Dog extends Animal {
       String name = "Dog";
       void show() {
           System.out.println(super.name);  // refers to Animal's field, NOT Dog's — easy to misuse if confused with 'this.name'
       }
   }
   ```

3. **Assuming `super` creates a new object:**
   `super()` does **not** create a separate parent object — it initializes the **parent's portion of the same single object** that is being constructed; there is only ever one object in memory, not two.

4. **Trying to access private members with `super`:**

   ```java
   class Animal {
       private String secret = "hidden";
   }
   class Dog extends Animal {
       void show() {
           // System.out.println(super.secret);  // Compile Error: private members are not accessible, even via super
       }
   }
   ```

5. **Forgetting that `super()` is required explicitly when the parent has no no-argument constructor**, leading to a compile-time error if omitted.

6. **Using `super` inside a static method or static context**, which is never allowed since `super` is tied to a specific object instance.

---

## 16. Interview Corner

**Q1. What is the `super` keyword?**
`super` is a reference used within a child class to explicitly access members (variables, methods, or constructors) of its immediate parent class, especially when those members are hidden or overridden by the child's own versions.

**Q2. Why is `super` used?**
It's used to access a parent's hidden field, call a parent's overridden method (often to extend rather than replace its behavior), and to explicitly invoke a parent class's constructor for proper object initialization.

**Q3. Difference between `this` and `super`?**
`this` refers to the current object itself and is used to access the current class's own members or call another constructor in the same class; `super` refers to the immediate parent class and is used to access the parent's members or call a parent class constructor.

**Q4. Can `super` access private variables?**
No — private members are never directly accessible outside their own declaring class, even through `super` from a child class; they can only be accessed indirectly, through public/protected methods the parent class provides.

**Q5. Can `super()` be called multiple times?**
No — a constructor can contain at most **one** call to `super(...)`, and it must be the very first statement in that constructor.

**Q6. Why must `super()` be the first statement in a constructor?**
Because Java requires the parent portion of an object to be fully initialized **before** the child class's own initialization logic runs, ensuring the object is built up correctly, layer by layer, from the top of the hierarchy downward.

---

## 17. MCQs

**1. What does the `super` keyword refer to?**
a) The current class
b) The immediate parent class
c) Any class in the hierarchy
d) A static class
**Answer: b) The immediate parent class**

**2. Where must `super()` appear if used explicitly in a constructor?**
a) Anywhere in the constructor
b) As the last statement
c) As the first statement
d) Only inside loops
**Answer: c) As the first statement**

**3. Can `super` be used inside a static method?**
a) Yes, always
b) No, never
c) Only in the main method
d) Only with static fields
**Answer: b) No, never**

**4. What happens if a child class constructor doesn't explicitly call `super(...)`?**
a) Compile error, always
b) Java implicitly inserts a call to the parent's no-argument constructor
c) The parent class is never initialized
d) The child constructor runs twice
**Answer: b) Java implicitly inserts a call to the parent's no-argument constructor**

**5. Can `super` access a parent class's private members?**
a) Yes, always
b) No, never directly
c) Only if declared protected
d) Only inside the same package
**Answer: b) No, never directly**

**6. What is the output of this code?**

```java
class A {
    int x = 10;
}
class B extends A {
    int x = 20;
    void show() {
        System.out.println(x + " " + super.x);
    }
}
```

a) 10 20
b) 20 10
c) 20 20
d) 10 10
**Answer: b) 20 10**

**7. What does `super.methodName()` do when the method is overridden in the child class?**
a) Calls the child's version
b) Calls the parent's original version
c) Causes a compile error
d) Calls both versions
**Answer: b) Calls the parent's original version**

**8. Can a constructor contain both `super(...)` and `this(...)`?**
a) Yes, always
b) No, only one of them can be used, and only as the first statement
c) Only in static constructors
d) Only if both classes are abstract
**Answer: b) No, only one of them can be used, and only as the first statement**

**9. What happens if the parent class has no no-argument constructor and the child doesn't call `super(...)` explicitly?**
a) Nothing, it compiles fine
b) Compile-time error
c) Runtime exception
d) The child constructor is skipped
**Answer: b) Compile-time error**

**10. In a hierarchy A → B → C, what does `super` refer to when used inside C?**
a) A
b) B
c) Both A and B
d) C itself
**Answer: b) B**

---

## 18. University Questions

### Short Questions

1. What is the `super` keyword used for in Java?
2. How is `super` used to access a hidden parent class variable?
3. What is the difference between `super.method()` and a normal method call?
4. Why must `super()` be the first statement in a constructor?
5. Can `super` be used inside a static method? Justify your answer.

### Long Questions

1. Explain the `super` keyword with examples of accessing variables, calling methods, and invoking constructors.
2. Discuss constructor chaining in Java using `super()`, with a multilevel inheritance example and its execution flow.
3. Differentiate between `this` and `super` with a complete comparison table and suitable examples.
4. Explain, with an example, how `super.method()` can be used to extend rather than completely replace a parent class's behavior.
5. Discuss the rules governing the use of `super` in Java, with examples illustrating each rule.

---

## 19. Viva Questions

1. What is the difference between `super` and `super()`?
2. Can `super` be used to access a grandparent class's members directly in a multilevel hierarchy?
3. What happens internally when `super()` is called in a constructor?
4. Why can't `super` be used in a static context?
5. Can you explicitly call `super()` and `this()` in the same constructor?
6. What is variable hiding, and how does `super` help resolve it?
7. Does `super()` create a new object separate from the child object?
8. What error occurs if `super(...)` isn't the first statement in a constructor?
9. Why is `super` essential when a parent class has no no-argument constructor?
10. How does `super.method()` differ in effect from simply calling `method()` inside an overriding method?

---

## 20. Programming Exercises

**1. Access Parent Variable**

Create a parent class and a child class that both declare a field with the same name; use `super` to print both the parent's and child's versions of that field.

**2. Call Parent Method**

Create a parent class with a method, override it in a child class, and use `super.methodName()` inside the override to also call the parent's original version.

**3. Call Parent Constructor**

Create a parent class with a parameterized constructor and a child class that explicitly calls it using `super(...)`.

**4. Constructor Chaining**

Create a three-level class hierarchy (`A → B → C`) and demonstrate, using print statements, that the constructors execute in order from the top of the hierarchy down.

**5. Variable Hiding Example**

Create a scenario where a child class field hides a parent class field of the same name, and write a method that prints both values using `super` and the plain field name.

---

## 21. Challenge Problems

**Design inheritance hierarchies using `super`:**

- **Vehicle → Car:** Use `super()` to initialize shared vehicle attributes, and `super.method()` to extend a shared `start()` behavior in `Car`.
- **Employee → Manager:** Use `super(...)` to initialize base employee details, and `super.calculateSalary()` to extend salary calculation logic with a bonus.
- **Animal → Dog:** Demonstrate variable hiding with a `sound` field in both classes, using `super` to access the parent's version alongside the child's own.
- **BankAccount → SavingsAccount:** Use `super(...)` for balance initialization and `super.deposit(...)` to reuse the parent's deposit logic when adding interest.

---

## 22. Summary

- The **`super`** keyword allows a child class to explicitly access its immediate parent class's variables, methods, and constructors.
- **`super.variable`** accesses a parent field that's been hidden by a same-named child field; **`super.method()`** calls a parent's method even if the child has overridden it.
- **`super(...)`** invokes a parent class constructor and must be the **first statement** in a constructor — used explicitly, or automatically inserted by Java if omitted (when a no-argument parent constructor exists).
- **Constructor chaining** ensures that, no matter how deep an inheritance hierarchy is, every parent class's constructor runs before its child's, from the top of the hierarchy down.
- `super` refers only to the **immediate** parent class, even in multilevel hierarchies, and cannot be used to access private members or used inside static contexts.
- Understanding the distinction between `super` and `this` is essential for correctly reusing and extending inherited functionality.

---

## 23. Key Takeaways

- `super` refers to the immediate parent class.
- `super.variable` accesses inherited variables.
- `super.method()` invokes parent methods.
- `super()` calls the parent constructor.
- `super` helps reuse parent functionality and supports constructor chaining.

---

## 24. Cheat Sheet

**Definition:**

```
super = reference to the immediate parent class's members
        (used to resolve hiding/overriding, or to chain constructors)
```

**Syntax:**

```java
super.variableName            // access parent's field
super.methodName(args);       // call parent's method
super(args);                   // call parent's constructor (constructor's FIRST statement)
```

**`super.variable`:**

```java
class Parent { int x = 10; }
class Child extends Parent {
    int x = 20;
    void show() {
        System.out.println(x);        // 20 (child's own)
        System.out.println(super.x);  // 10 (parent's, via super)
    }
}
```

**`super.method()`:**

```java
class Parent {
    void greet() { System.out.println("Parent greet"); }
}
class Child extends Parent {
    @Override
    void greet() {
        super.greet();    // calls Parent's greet() first
        System.out.println("Child greet");
    }
}
```

**`super()`:**

```java
class Parent {
    Parent(String name) { System.out.println("Parent: " + name); }
}
class Child extends Parent {
    Child(String name) {
        super(name);   // MUST be first statement
        System.out.println("Child: " + name);
    }
}
```

**Rules:**

```
✔ super()/super must relate to the IMMEDIATE parent only
✔ super(...) must be the FIRST statement in a constructor
✔ A constructor may call EITHER super(...) OR this(...), never both
✘ super cannot access private members of the parent
✘ super cannot be used inside static methods/contexts
```

**Comparison Table (`super` vs `this`):**

| Aspect           | `super`                             | `this`                                        |
| ---------------- | ----------------------------------- | --------------------------------------------- |
| Refers to        | Parent class                        | Current object                                |
| Field access     | `super.field` (hidden field)        | `this.field` (own field)                      |
| Method call      | `super.method()` (parent's version) | `this.method()` (own/overridden version)      |
| Constructor call | `super(...)` → parent constructor   | `this(...)` → another constructor, same class |
| Static context   | Not allowed                         | Not allowed                                   |
