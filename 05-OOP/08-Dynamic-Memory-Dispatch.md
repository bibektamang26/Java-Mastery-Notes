# Dynamic Method Dispatch in Java

---

## 1. Introduction

### What is Dynamic Method Dispatch?

**Dynamic Method Dispatch** is the mechanism by which Java decides, **at runtime**, which overridden method implementation to actually execute — based on the **actual object** a reference variable points to, rather than the reference variable's **declared type**. It is the underlying process that makes runtime polymorphism possible in Java.

```java
Animal a = new Dog();
a.makeSound(); // Java decides at RUNTIME to run Dog's version, not Animal's
```

### Why is it Important?

Dynamic Method Dispatch is the **core engine** behind runtime polymorphism. Without it, method calls on overridden methods would always be resolved based on the reference type alone (like static methods are), making features like polymorphic collections, extensible class hierarchies, and flexible object-oriented designs impossible. Understanding this mechanism is essential to understanding _why_ polymorphism behaves the way it does in Java.

### Relationship with Runtime Polymorphism

**Runtime polymorphism** is the _behavior_ — the same method call producing different results depending on the object involved. **Dynamic Method Dispatch** is the _mechanism_ — the specific process the JVM follows to achieve that behavior. In other words: dynamic method dispatch is _how_ Java implements runtime polymorphism.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define Dynamic Method Dispatch and explain its purpose.
- Explain how Java selects which method implementation to execute at runtime.
- Understand dynamic binding (late binding) and how it differs from static binding.
- Differentiate compile-time binding from runtime binding with clear examples.
- Trace method execution in memory, from reference variable to actual object to method execution.

---

## 3. Prerequisites

### Quick Revision

**Classes**
A blueprint that defines the structure (fields) and behavior (methods) of the objects created from it.

**Objects**
An instance of a class, created using `new`, holding its own copy of instance data in heap memory.

**Inheritance**
A mechanism where a subclass acquires fields and methods from a superclass using `extends`, forming an "is-a" relationship — the structural foundation required for dynamic method dispatch to occur.

**Method Overriding**
A subclass providing its own specific implementation of a method already defined in its superclass, using the exact same method signature.

```java
class Animal {
    void sound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override
    void sound() { System.out.println("Bark"); }
}
```

**Polymorphism**
The broader OOP concept where a single method call or interface can produce different behavior depending on the actual object — dynamic method dispatch is the specific mechanism Java uses to achieve the _runtime_ form of this behavior.

---

## 4. What is Dynamic Method Dispatch?

### Definition

> Dynamic Method Dispatch is the process by which a call to an overridden method is resolved at **runtime**, based on the type of the **object** being referred to, rather than the type of the **reference variable**.

### Meaning of "Dynamic"

"Dynamic" refers to something determined **while the program is running**, as opposed to "static," which refers to something fixed and determined **before execution** (at compile time). Here, "dynamic" means the method resolution happens during program execution.

### Meaning of "Dispatch"

"Dispatch" means **sending** or **directing** a request to be handled — in this context, directing a method call to the appropriate method implementation. So "dynamic dispatch" literally means: **the method call is directed to the correct implementation while the program is running.**

### Purpose

The purpose of dynamic method dispatch is to allow a single reference type (e.g., a superclass or interface) to correctly invoke the **most specific, appropriate implementation** of a method — the one belonging to the actual object — even though the compiler only knows the reference's declared type.

### Examples

```java
class Animal {
    void sound() { System.out.println("Some generic sound"); }
}

class Dog extends Animal {
    @Override
    void sound() { System.out.println("Bark"); }
}

class Cat extends Animal {
    @Override
    void sound() { System.out.println("Meow"); }
}

public class DispatchDemo {
    public static void main(String[] args) {
        Animal a1 = new Dog();
        Animal a2 = new Cat();

        a1.sound(); // "Bark" — dispatched to Dog's version
        a2.sound(); // "Meow" — dispatched to Cat's version
    }
}
```

Both `a1` and `a2` are declared as type `Animal`, but the JVM correctly "dispatches" each call to the version of `sound()` belonging to the object actually referenced — `Dog` or `Cat` respectively.

---

## 5. Why Do We Need Dynamic Method Dispatch?

### Flexibility

Code can be written to operate on a general reference type (like a superclass), while automatically executing the correct, specific behavior for whatever actual object is provided — without needing to know that object's exact type in advance.

### Extensibility

New subclasses can be added to a system at any time, each with their own overridden method behavior, **without modifying** any existing code that already works with the superclass reference — dynamic dispatch will automatically route calls correctly to the new subclass's methods too.

### Runtime Decision Making

Since the actual object may not be known until the program is actually running (e.g., it could depend on user input, a database query, or a factory method's decision), dynamic method dispatch allows the correct behavior to be selected precisely when that information becomes available — at runtime.

### Loose Coupling

Code that calls methods via a superclass or interface reference is **decoupled** from the specific subclasses it might encounter — it doesn't need `if-else` chains checking types; dynamic dispatch handles the routing automatically and transparently.

### Real-world Analogy

Imagine a **customer support hotline** that routes your call based on which department you actually need (billing, technical support, sales) — but you always dial the **same phone number**. The "dispatcher" (like a switchboard operator or automated system) listens to your request and, **at the moment of your call** (runtime), decides where to actually route it. You don't need to know the internal extension numbers in advance — the system dynamically dispatches your call to the right handler. This mirrors exactly how the JVM handles a method call on an overridden method: the same call, dynamically routed to the correct implementation based on the actual object.

---

## 6. How Dynamic Method Dispatch Works

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
   JVM Searches
        |
        v
Correct Overridden Method Executes
```

### Flow Diagram (Detailed)

```
        +--------------------------+
        |  Animal a = new Dog();    |
        +-------------+------------+
                       |
                       v
        +--------------------------+
        |  a.sound() is called      |
        +-------------+------------+
                       |
                       v
        +--------------------------------+
        |  Compiler checks: does Animal    |
        |  have a sound() method?          |
        |  (compile-time check only)       |
        +-------------+--------------------+
                       |
                       v
        +--------------------------------+
        |  At RUNTIME, JVM looks at the    |
        |  ACTUAL OBJECT (Dog), not the     |
        |  reference type (Animal)          |
        +-------------+--------------------+
                       |
                       v
        +--------------------------+
        |  Dog's sound() executes    |
        |  -> prints "Bark"           |
        +--------------------------+
```

This two-stage process — a **compile-time check** (does the method exist on the reference type?) followed by a **runtime decision** (which actual implementation should run?) — is the essence of dynamic method dispatch.

---

## 7. Dynamic Binding (Late Binding)

### Definition

**Dynamic binding**, also called **late binding**, is the process of connecting a method call to its actual method implementation **at runtime**, rather than at compile time. This is the binding mechanism that powers dynamic method dispatch.

### Why It Is Called Late Binding

It's called "late" because the decision about which method to execute is deliberately **delayed** — postponed from compile time all the way until the program is actually running and the real object is known. This is in contrast to "early binding" (static binding), where the decision is made immediately, at compile time.

### Examples

```java
class Shape {
    void draw() { System.out.println("Drawing a generic shape"); }
}

class Circle extends Shape {
    @Override
    void draw() { System.out.println("Drawing a circle"); }
}

public class LateBindingDemo {
    public static void main(String[] args) {
        Shape s = new Circle();
        s.draw(); // "Drawing a circle" — determined at RUNTIME (late binding)
    }
}
```

Even though `s` is declared as `Shape`, Java delays the decision of _which_ `draw()` to call until the program actually runs and discovers that `s` refers to a `Circle` object.

---

## 8. Static Binding vs Dynamic Binding

### Definition

**Static Binding (Early Binding)**
The method (or variable) to be used is resolved **at compile time**, based purely on the **reference type** — applies to `static` methods, `private` methods, `final` methods, and all variable/field access.

**Dynamic Binding (Late Binding)**
The method to be used is resolved **at runtime**, based on the **actual object type** — applies specifically to **overridden instance methods**.

### Examples

```java
class Animal {
    static void staticMethod() { System.out.println("Animal's static method"); }
    void instanceMethod() { System.out.println("Animal's instance method"); }
}

class Dog extends Animal {
    static void staticMethod() { System.out.println("Dog's static method"); }
    @Override
    void instanceMethod() { System.out.println("Dog's instance method"); }
}

public class BindingDemo {
    public static void main(String[] args) {
        Animal a = new Dog();

        a.staticMethod();    // "Animal's static method" — STATIC binding (reference type wins)
        a.instanceMethod();  // "Dog's instance method"   — DYNAMIC binding (object type wins)
    }
}
```

### Comparison Table

| Aspect                           | Static Binding                                         | Dynamic Binding                         |
| -------------------------------- | ------------------------------------------------------ | --------------------------------------- |
| Also Known As                    | Early binding                                          | Late binding                            |
| Resolved At                      | Compile time                                           | Runtime                                 |
| Applies To                       | `static`, `private`, `final` methods, variables/fields | Overridden instance methods             |
| Determined By                    | Reference (declared) type                              | Actual (runtime) object type            |
| Requires Inheritance/Overriding? | No                                                     | Yes                                     |
| Performance                      | Slightly faster                                        | Slightly slower (due to runtime lookup) |
| Example                          | `a.staticMethod()`                                     | `a.instanceMethod()` (overridden)       |

---

## 9. Step-by-Step Execution

### `Animal a = new Dog();`

Let's trace exactly what happens for the statement:

```java
Animal a = new Dog();
a.sound();
```

### Step 1: Compiler Check

When the compiler processes `a.sound()`, it checks the **declared reference type** (`Animal`) to verify that a method named `sound()` (with a matching signature) actually exists there. If `Animal` didn't have a `sound()` method (and no subtype-specific knowledge is available at compile time), this would be a **compile-time error** — regardless of what `Dog` defines.

### Step 2: JVM Runtime Check

At runtime, when the line `a.sound()` actually executes, the JVM looks at the **actual object** stored in `a` — which is a `Dog` object — and searches for the most specific overridden version of `sound()` starting from `Dog`'s class definition (and moving up the hierarchy only if `Dog` itself doesn't override it).

### Step 3: `Dog.sound()`

Since `Dog` provides its own overridden `sound()` method, that specific implementation is the one executed — **not** `Animal`'s version, even though the reference variable `a` is declared as type `Animal`.

### Complete Explanation

```
Compile Time:                          Runtime:
+---------------------------+          +---------------------------+
| Compiler verifies sound()  |          | JVM checks actual object   |
| exists on Animal (the       |   -->    | type: Dog                    |
| DECLARED/reference type)     |          |                              |
+---------------------------+          +-------------+---------------+
                                                      |
                                                      v
                                        +---------------------------+
                                        | Dog's overridden sound()   |
                                        | method is executed          |
                                        +---------------------------+
```

This two-phase process — **compile-time existence check** on the reference type, followed by **runtime resolution** based on the actual object — is precisely what dynamic method dispatch entails.

---

## 10. Reference Type vs Object Type

### Definition

**Reference Type**
The **declared type** of a variable — determines which methods/fields are visible and callable at compile time.

**Object Type**
The **actual class** of the object that was created with `new` — determines which overridden method implementation actually executes at runtime.

### Examples

```java
Animal a = new Dog();
// Reference Type: Animal (what the compiler sees)
// Object Type: Dog (what actually exists in memory)

a.sound();       // Object type (Dog) wins for overridden instance methods
a.staticMethod(); // Reference type (Animal) wins for static methods
```

### Why Object Type Wins

For **overridden instance methods**, Java is specifically designed to let the **object type win** — this is precisely what enables runtime polymorphism to work meaningfully. If the reference type always won (as it does for static/private/final members), overriding would be pointless, since you'd always get the same behavior regardless of what object was actually assigned. By letting the object type determine the outcome, Java allows a single reference type (like `Animal`) to correctly represent and interact with many different actual behaviors (`Dog`, `Cat`, `Cow`, etc.) — the essence of polymorphism.

---

## 11. Upcasting and Dynamic Dispatch

### Definition

**Upcasting** — assigning a subclass object to a superclass (or interface) reference variable — is the setup step that **enables** dynamic method dispatch to become meaningful. Without upcasting, there would be no distinction between "reference type" and "object type" to exploit.

```java
Animal a = new Dog(); // upcasting: Dog treated as Animal
```

### Examples

```java
class Vehicle {
    void start() { System.out.println("Vehicle starting"); }
}
class Car extends Vehicle {
    @Override
    void start() { System.out.println("Car engine starting"); }
}
class Bike extends Vehicle {
    @Override
    void start() { System.out.println("Bike engine starting"); }
}

public class UpcastDispatchDemo {
    public static void main(String[] args) {
        Vehicle[] vehicles = { new Car(), new Bike() }; // both upcast to Vehicle

        for (Vehicle v : vehicles) {
            v.start(); // dynamic dispatch selects the correct version for each
        }
        // Car engine starting
        // Bike engine starting
    }
}
```

### Benefits

- Enables **heterogeneous collections** — a single array/list of the parent type can hold many different subclass objects.
- Supports writing **general-purpose methods** that accept a parent type parameter but correctly behave differently for each actual subclass passed in.
- Forms the foundation for **polymorphic design patterns** widely used throughout Java (Strategy, Factory, Template Method, and more).

---

## 12. Memory Representation

### Conceptual Flow

```
Stack
   |
   v
Reference Variable
   |
   v
Heap Object
   |
   v
Virtual Method Lookup
```

### Memory Diagram

```java
class Animal {
    void sound() { System.out.println("Some sound"); }
}
class Dog extends Animal {
    @Override
    void sound() { System.out.println("Bark"); }
}

public class MemoryDemo {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();
    }
}
```

```
Stack                    Heap
+----------------+       +--------------------------------+
| main() frame     |       | Dog object                       |
|   a ------------->|------->| (contains a hidden reference to   |
+----------------+       |  Dog's method table/class info)   |
                          +--------------------------------+
                                        |
                                        v
                          +--------------------------------+
                          | Dog's Method Table:               |
                          |   sound() -> Dog.sound()          |
                          +--------------------------------+
```

The reference variable `a` lives on the **stack**, pointing to the actual `Dog` object on the **heap**. Every object internally carries information about its actual class (conceptually, a reference to its class's method table). When `a.sound()` is called, the JVM uses this internal class information — not the compile-time reference type — to look up and execute the correct (`Dog`'s) version of `sound()`. This lookup process is often referred to as a **virtual method table (vtable)** lookup in general OOP theory, though Java's actual internal implementation details are hidden from the programmer.

---

## 13. JVM Method Selection Process

### Conceptual Flow

```
Compile Time
     |
     v
Verify Method Exists
     |
     v
Runtime
     |
     v
Locate Actual Object
     |
     v
Execute Overridden Method
```

### Diagram

```
+-------------------------------+
|          COMPILE TIME           |
|-------------------------------|
| 1. Check reference type (e.g.,  |
|    Animal) for a method named   |
|    sound() with matching        |
|    signature.                    |
| 2. If found -> compiles          |
|    successfully.                  |
| 3. If NOT found -> compile        |
|    error, REGARDLESS of what      |
|    the actual object might be.    |
+---------------+-----------------+
                |
                v
+-------------------------------+
|            RUNTIME              |
|-------------------------------|
| 4. JVM examines the ACTUAL       |
|    object referred to (e.g.,     |
|    Dog).                          |
| 5. JVM looks for the MOST         |
|    SPECIFIC override of           |
|    sound() starting from the      |
|    object's actual class.         |
| 6. Executes that implementation.  |
+-------------------------------+
```

This two-stage process ensures **type safety** at compile time (you can't call methods that don't exist on the declared type) while still allowing **flexible, polymorphic behavior** at runtime (the actual, most specific implementation is what actually runs).

---

## 14. Virtual Method Invocation (Concept)

### What Are Virtual Methods?

In general object-oriented terminology, a **virtual method** is a method that supports being overridden and resolved dynamically at runtime. In Java, **all non-static, non-private, non-final instance methods are virtual by default** — meaning they automatically participate in dynamic method dispatch whenever they are overridden in a subclass.

### How Java Treats Overridden Methods

- Any regular instance method that a subclass overrides becomes a candidate for dynamic dispatch.
- Java does **not** require any special keyword (like `virtual` in C++) to enable this behavior — it's the default for ordinary instance methods.
- Methods marked `static`, `private`, or `final` are explicitly **excluded** from dynamic dispatch, since they cannot be overridden (only `static` methods can be _hidden_, which behaves differently — see Section 19).

### Simple Explanation (No JVM Internals)

Think of it this way: whenever you override a method in Java, you're essentially telling the JVM, _"whenever someone calls this method through any reference — no matter its declared type — check what the object actually is, and run **my** version if it's more specific."_ This is exactly what "virtual method invocation" captures, without needing to dive into the precise internal bytecode mechanisms (like `invokevirtual`) that actually implement this behavior under the hood.

---

## 15. Advantages

- **Runtime Flexibility** — the correct method implementation is chosen precisely when needed, based on real, live object information rather than fixed, compile-time assumptions.
- **Loose Coupling** — code that calls methods via a parent reference doesn't need to know (or check) the exact subclass involved; dynamic dispatch handles the routing transparently.
- **Extensibility** — new subclasses with their own overridden behavior can be introduced later without modifying any existing code that already relies on the parent type.
- **Better Software Design** — enables clean, maintainable, and scalable object-oriented designs, avoiding large `if-else`/`switch` chains that check object types manually.

---

## 16. Limitations

### Slight Runtime Overhead

Because the JVM must perform a lookup at runtime to determine the correct method implementation (rather than resolving it once at compile time), dynamic dispatch introduces a small performance cost compared to statically-bound calls — though modern JVMs optimize this heavily (e.g., via inline caching), making the overhead negligible in almost all practical scenarios.

### Harder Debugging

Since the method actually executed depends on the runtime object rather than what's visible in the source code at the call site, tracing execution flow — especially across deep or complex inheritance hierarchies — can be more challenging than following straightforward, non-polymorphic code.

### Cannot Dispatch Static Methods

Dynamic method dispatch **only applies to overridden instance methods**. `static` methods are resolved using **static binding**, based on the reference type — they cannot be dynamically dispatched, and attempting to rely on this behavior is a common source of confusion (see Section 19).

---

## 17. Real-world Examples

### Animal System

```java
class Animal { void sound() { System.out.println("Generic sound"); } }
class Dog extends Animal { void sound() { System.out.println("Bark"); } }
class Cat extends Animal { void sound() { System.out.println("Meow"); } }
```

### Payment Gateway

```java
abstract class PaymentGateway {
    abstract void processPayment(double amount);
}
class StripeGateway extends PaymentGateway {
    void processPayment(double amount) { System.out.println("Processing $" + amount + " via Stripe"); }
}
class PaypalGateway extends PaymentGateway {
    void processPayment(double amount) { System.out.println("Processing $" + amount + " via PayPal"); }
}
```

### Vehicle System

```java
class Vehicle { void start() { System.out.println("Generic vehicle starting"); } }
class Car extends Vehicle { void start() { System.out.println("Car starting"); } }
class Motorcycle extends Vehicle { void start() { System.out.println("Motorcycle starting"); } }
```

### Shape Calculator

```java
abstract class Shape { abstract double area(); }
class Circle extends Shape { double r; Circle(double r){this.r=r;} double area(){return Math.PI*r*r;} }
class Square extends Shape { double s; Square(double s){this.s=s;} double area(){return s*s;} }
```

### Employee Management

```java
class Employee { double bonus() { return 1000; } }
class Manager extends Employee { double bonus() { return 5000; } }
class Intern extends Employee { double bonus() { return 500; } }
```

### Media Player

```java
class MediaFile { void play() { System.out.println("Playing generic media"); } }
class AudioFile extends MediaFile { void play() { System.out.println("Playing audio"); } }
class VideoFile extends MediaFile { void play() { System.out.println("Playing video"); } }
```

Each of these systems demonstrates the same underlying pattern: a common parent reference type, multiple overriding subclasses, and dynamic method dispatch automatically routing each call to the correct, specific implementation at runtime.

---

## 18. Best Practices

1. **Program to parent types or interfaces** — declare variables, parameters, and collections using the most general applicable type, letting dynamic dispatch handle the specifics.
2. **Avoid unnecessary type casting** — frequent downcasting to check/access subclass-specific behavior often indicates that the design isn't fully leveraging polymorphism and dynamic dispatch.
3. **Use overriding effectively** — ensure each subclass's overridden method genuinely represents meaningful, distinct behavior appropriate to that subclass, rather than overriding just for the sake of it.
4. **Keep overridden methods meaningful** — an overridden method should honor the general "contract" implied by the superclass method (e.g., a `draw()` method should actually draw something sensible for every subclass), to avoid surprising behavior when relying on dynamic dispatch.

---

## 19. Common Mistakes

### Confusing Reference Type with Object Type

**Mistake:** Assuming that the reference type always determines which method version runs.
**Reality:** For **overridden instance methods**, the **object type** determines execution — the reference type only matters for what's visible/callable at compile time.

### Expecting Static Methods to Be Dynamically Dispatched

**Mistake:** Believing `static` methods participate in dynamic dispatch like instance methods do.

```java
class Animal {
    static void identify() { System.out.println("I am an Animal (static)"); }
}
class Dog extends Animal {
    static void identify() { System.out.println("I am a Dog (static)"); }
}

Animal a = new Dog();
a.identify(); // "I am an Animal (static)" — NOT dynamically dispatched!
```

**Reality:** Static methods are resolved via **static binding**, based on the **reference type** — this is technically called **method hiding**, not overriding, and it does not participate in dynamic method dispatch at all.

### Assuming Constructors Participate in Dynamic Dispatch

**Mistake:** Believing constructors can be "overridden" and dynamically dispatched like instance methods.
**Reality:** Constructors are **not inherited** and cannot be overridden at all — each class defines its own constructors independently. There is no dynamic dispatch mechanism involved with constructor calls; the constructor executed is always exactly the one matching the class being instantiated (plus any implicit/explicit `super()` chain).

### Forgetting Inheritance is Required

**Mistake:** Expecting dynamic method dispatch to occur between two classes that have no inheritance (or interface implementation) relationship whatsoever.
**Reality:** Dynamic method dispatch is only possible when a method is genuinely **overridden** — which requires the classes to be connected via `extends` or `implements`. Two unrelated classes with a similarly-named method are simply unrelated methods, not a case of overriding or dynamic dispatch.

---

## 20. Interview Corner

**Q1. What is Dynamic Method Dispatch?**
It's the mechanism by which Java resolves a call to an overridden instance method at **runtime**, based on the actual object a reference variable points to, rather than the reference's declared (compile-time) type.

**Q2. Difference between Dynamic Method Dispatch and Polymorphism?**
Polymorphism is the broader _behavior/concept_ — the same method call producing different results for different objects. Dynamic Method Dispatch is the specific _mechanism_ Java uses to achieve the runtime form of that behavior.

**Q3. What is Late Binding?**
Another name for dynamic binding — the process of connecting a method call to its actual implementation at runtime (as opposed to "early"/static binding, which happens at compile time).

**Q4. Difference between Static Binding and Dynamic Binding?**
Static binding resolves method calls at compile time based on the reference type (used for `static`, `private`, `final` methods); dynamic binding resolves overridden instance method calls at runtime based on the actual object type.

**Q5. Why is Java called dynamically dispatched?**
Because for overridden instance methods, Java doesn't fix the method implementation at compile time — it deliberately delays (dispatches) that decision until runtime, examining the actual object to determine the correct method to execute.

**Q6. Can constructors participate in dynamic dispatch?**
No — constructors are not inherited and cannot be overridden, so there is no concept of "dynamic dispatch" for constructor calls; the constructor matching the exact class being instantiated always runs.

**Q7. Can static methods participate in dynamic dispatch?**
No — static methods are resolved via static binding, based on the reference type at compile time. If a subclass defines a static method with the same signature as one in its superclass, this is called **method hiding**, not overriding, and it does not use dynamic dispatch.

**Q8. Why does object type determine execution (for overridden methods)?**
Because the entire purpose of overriding is to let a subclass provide specialized behavior — if the reference type always determined execution instead, overriding would have no practical effect, and true polymorphism (many forms of behavior through one common interface) would be impossible to achieve.

---

## 21. MCQs

**1. What is Dynamic Method Dispatch?**
a) Resolving method calls at compile time b) Resolving overridden method calls at runtime based on the actual object c) A way to create objects dynamically d) A type of exception handling

> **Answer: b)**

**2. What determines which overridden method executes at runtime?**
a) Reference type b) Actual object type c) Compiler's guess d) Import statements

> **Answer: b)**

**3. Dynamic Method Dispatch is also known as:**
a) Early binding b) Static binding c) Late binding d) Compile-time binding

> **Answer: c)**

**4. Which of the following participates in dynamic method dispatch?**
a) Static methods b) Private methods c) Overridden instance methods d) Constructors

> **Answer: c)**

**5. What happens when a subclass defines a static method with the same signature as its superclass?**
a) Overriding occurs b) Method hiding occurs c) Compile error d) Dynamic dispatch occurs

> **Answer: b)**

**6. At compile time, the compiler checks method existence based on:**
a) The actual object type b) The reference (declared) type c) Both equally d) Neither

> **Answer: b)**

**7. What is required for dynamic method dispatch to occur?**
a) No relationship between classes b) Inheritance and method overriding c) Only interfaces d) Static methods only

> **Answer: b)**

**8. In `Animal a = new Dog(); a.sound();`, which class's `sound()` runs (assuming Dog overrides it)?**
a) Animal's b) Dog's c) Depends on JVM version d) Compile error

> **Answer: b)**

**9. Which of the following does NOT participate in dynamic dispatch?**
a) Overridden instance methods b) Static methods c) Both a and b equally d) None of these

> **Answer: b)**

**10. What term describes methods that support dynamic dispatch by default in Java?**
a) Final methods b) Static methods c) Virtual methods (conceptually) d) Private methods

> **Answer: c)**

**11. What is "Reference Type" in the context of dynamic dispatch?**
a) The actual class of the object b) The declared type of the variable c) The method's return type d) The constructor's parameter type

> **Answer: b)**

**12. What is "Object Type" in the context of dynamic dispatch?**
a) The declared variable type b) The actual class used to create the object with `new` c) The interface implemented d) The return type of a method

> **Answer: b)**

**13. Which binding type is used for `final` methods?**
a) Dynamic binding b) Static binding c) Both d) Neither

> **Answer: b)**

**14. What enables dynamic method dispatch to become meaningful in a program?**
a) Downcasting b) Upcasting c) Static import d) Method overloading

> **Answer: b)**

**15. Which of these correctly completes: "Dynamic method dispatch resolves method calls..."**
a) ...at compile time using the reference type b) ...at runtime using the object type c) ...randomly d) ...using method names only, ignoring class hierarchy

> **Answer: b)**

**16. Do constructors participate in dynamic method dispatch?**
a) Yes, always b) No, constructors are not inherited/overridden c) Only for abstract classes d) Only with `super()`

> **Answer: b)**

**17. What is the main limitation of dynamic method dispatch?**
a) It cannot handle inheritance b) Slight runtime overhead and potentially harder debugging c) It only works with primitives d) It requires static methods

> **Answer: b)**

**18. Which keyword, when applied to a method, prevents it from participating in dynamic dispatch (since it cannot be overridden)?**
a) `static` b) `abstract` c) `final` d) `public`

> **Answer: c)**

**19. What does the JVM use to determine the correct overridden method at runtime?**
a) The variable's name b) The actual object's class information c) The package name d) The method's access modifier

> **Answer: b)**

**20. Which of these best summarizes the relationship between polymorphism and dynamic method dispatch?**
a) They are unrelated concepts b) Dynamic method dispatch is the mechanism that implements runtime polymorphism c) Polymorphism only applies to static methods d) Dynamic dispatch replaces the need for inheritance

> **Answer: b)**

---

## 22. University Questions

### Short Questions

1. What is Dynamic Method Dispatch?
2. What is the difference between reference type and object type?
3. What is late binding?
4. Can static methods be dynamically dispatched? Why or why not?
5. What role does upcasting play in dynamic method dispatch?

### Long Questions

1. Explain Dynamic Method Dispatch with a suitable example program and step-by-step execution trace. _(10 marks)_
2. Differentiate between static binding and dynamic binding with example programs for each. _(10 marks)_
3. Explain, with a memory diagram, how the JVM selects the correct overridden method to execute at runtime. _(10 marks)_
4. Discuss the relationship between upcasting, inheritance, and dynamic method dispatch, using a real-world example (e.g., Vehicle or Payment system). _(10 marks)_
5. Explain why static methods, private methods, and constructors do not participate in dynamic method dispatch. _(5 marks)_
6. Discuss the advantages and limitations of dynamic method dispatch in software design. _(5 marks)_

---

## 23. Viva Questions

1. What is Dynamic Method Dispatch, in one line?
2. What is another name for dynamic binding?
3. What determines which overridden method runs — reference type or object type?
4. What is the difference between early binding and late binding?
5. Does dynamic method dispatch require inheritance? Why?
6. Can static methods be dynamically dispatched?
7. What is "method hiding," and how does it differ from overriding?
8. Do constructors participate in dynamic dispatch?
9. What role does upcasting play in enabling dynamic dispatch?
10. What is a "virtual method" conceptually, in Java's context?
11. Why is dynamic method dispatch considered the mechanism behind runtime polymorphism?
12. What happens at compile time versus runtime when an overridden method is called?
13. Can `final` methods be dynamically dispatched? Why or why not?
14. Give a real-world analogy for dynamic method dispatch.
15. Name one limitation of relying heavily on dynamic method dispatch.

---

## 24. Programming Exercises

_(All exercises should use a superclass reference variable pointing to different subclass objects at runtime, explicitly demonstrating dynamic method dispatch.)_

### 1. Animal → Dog

Create an `Animal` class with a `sound()` method, and a `Dog` subclass overriding it. Use an `Animal` reference to point to a `Dog` object and call `sound()`, then explain in comments why Dog's version executes.

### 2. Vehicle → Car

Create a `Vehicle` class with a `start()` method, and `Car`/`Bike` subclasses overriding it. Store multiple subclass objects in a `Vehicle[]` array and call `start()` on each in a loop.

### 3. Employee → Manager

Create an `Employee` class with a `calculateBonus()` method, and a `Manager` subclass overriding it. Demonstrate dynamic dispatch by assigning a `Manager` object to an `Employee` reference and calling `calculateBonus()`.

### 4. Payment System

Create an abstract `Payment` class with an abstract `pay()` method, and multiple subclasses (`CardPayment`, `CashPayment`). Process a list of `Payment` references (each pointing to different subclass objects) and demonstrate that each call correctly dispatches to its specific implementation.

### 5. Shape Hierarchy

Create an abstract `Shape` class with an abstract `area()` method, and subclasses `Circle`, `Rectangle`, `Triangle`. Store them in a `Shape[]` array and compute the total area using dynamic method dispatch within a loop.

_(All using runtime references — i.e., always declare the reference variable as the superclass/abstract type, and assign different subclass objects to observe dynamic dispatch in action.)_

---

## 25. Challenge Problems

Design runtime polymorphic systems for the following domains, ensuring each explicitly relies on dynamic method dispatch (superclass/interface references pointing to varying subclass objects):

### Hospital

Design a `Staff` hierarchy (`Doctor`, `Nurse`, `Receptionist`) with an overridden `performDuty()` method. Build a `Staff[]` roster and call `performDuty()` on each element, observing dynamic dispatch route each call correctly.

### Banking

Design an `Account` hierarchy (`SavingsAccount`, `CurrentAccount`) with an overridden `calculateInterest()` method. Process a list of `Account` references (holding different subclass objects) to compute total interest via dynamic dispatch.

### Library

Design a `LibraryItem` hierarchy (`Book`, `DVD`, `Magazine`) with an overridden `getLoanPeriod()` method. Iterate over a `LibraryItem[]` collection, using dynamic dispatch to determine each item's correct loan period.

### Online Shopping

Design a `Product` hierarchy (`Electronics`, `Clothing`) with an overridden `calculateDiscount()` method. Process a shopping cart of `Product` references, relying on dynamic dispatch to apply the correct discount logic per item.

### Payroll

Design an `Employee` hierarchy (`FullTimeEmployee`, `Contractor`) with an overridden `generatePayslip()` method. Process a mixed workforce array of `Employee` references, demonstrating dynamic dispatch correctly generating each payslip type.

---

## 26. Summary

- **Dynamic Method Dispatch** is Java's mechanism for resolving overridden method calls at **runtime**, based on the actual object rather than the reference's declared type.
- It is the specific implementation mechanism behind **runtime polymorphism**.
- Method resolution happens in two stages: a **compile-time check** (does the method exist on the reference type?) followed by a **runtime decision** (which specific implementation should run, based on the actual object?).
- This runtime resolution process is called **dynamic binding** or **late binding**, in contrast to **static binding** (early binding), which applies to `static`, `private`, and `final` methods, resolved entirely at compile time.
- **Upcasting** — assigning a subclass object to a superclass reference — is what enables dynamic dispatch to be meaningful, since it creates the distinction between reference type and object type that dynamic dispatch exploits.
- **Constructors and static methods do not participate** in dynamic method dispatch — constructors aren't inherited/overridden at all, and static methods are resolved via static binding (a subclass "hiding" a static method is different from overriding).
- Dynamic method dispatch enables flexible, extensible, and loosely-coupled software designs, though it introduces a small runtime overhead and can make debugging more challenging in complex hierarchies.

---

## 27. Key Takeaways

- Dynamic Method Dispatch is Java's runtime method selection mechanism.
- It enables runtime polymorphism.
- The compiler checks the reference type.
- The JVM executes the method based on the actual object.
- Overridden methods participate in dynamic dispatch.
- Static methods and constructors do not.

---

## 28. Cheat Sheet

### Definition

> Dynamic Method Dispatch = resolving an overridden method call at **runtime**, based on the **actual object type**, not the reference type.

### Execution Steps

```
1. Compiler checks: does the reference type have this method? (compile-time)
2. Runtime: JVM examines the ACTUAL object.
3. JVM executes the MOST SPECIFIC overridden version for that object.
```

### Binding Types

| Binding         | Also Called   | Resolved At  | Applies To                                   |
| --------------- | ------------- | ------------ | -------------------------------------------- |
| Static Binding  | Early Binding | Compile time | `static`, `private`, `final` methods, fields |
| Dynamic Binding | Late Binding  | Runtime      | Overridden instance methods                  |

### Comparison Tables

| Reference Type                            | Object Type                             |
| ----------------------------------------- | --------------------------------------- |
| Declared type of the variable             | Actual class used with `new`            |
| Determines compile-time method visibility | Determines which overridden method runs |
| Fixed at compile time                     | Known only at runtime                   |

| Participates in Dynamic Dispatch? |                                      |
| --------------------------------- | ------------------------------------ |
| Overridden instance methods       | ✔ Yes                                |
| Static methods                    | ✘ No (method hiding, static binding) |
| Private methods                   | ✘ No (not inherited/overridden)      |
| Final methods                     | ✘ No (cannot be overridden)          |
| Constructors                      | ✘ No (not inherited)                 |

### Quick Example

```java
class Animal { void sound() { System.out.println("Some sound"); } }
class Dog extends Animal {
    @Override void sound() { System.out.println("Bark"); }
}

Animal a = new Dog(); // upcasting
a.sound(); // "Bark" — dynamic method dispatch in action
```

### Interview Notes

- Dynamic Method Dispatch = the _mechanism_; Polymorphism = the _behavior/result_.
- Object type wins for overridden instance methods; reference type wins for static/private/final.
- Requires inheritance + method overriding.
- Constructors and static methods never participate.

---

_End of Chapter — Dynamic Method Dispatch in Java_
