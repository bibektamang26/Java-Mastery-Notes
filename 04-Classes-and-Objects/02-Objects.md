# Objects in Java

---

## 1. Introduction

### What is an Object?

An **object** is a concrete, real instance created from a class — it's the actual "thing" that exists in memory, with its own specific data, built according to the structure the class defines.

### Why are Objects Needed?

A class alone is just a description — it doesn't represent any real entity until you create an object from it. Objects are what actually let your program _work with_ real data: a specific student, a specific car, a specific bank account — each with its own values and its own identity in memory.

### Relationship between Class and Object

A class and an object have a **one-to-many** relationship: one class can be used to create **many** objects, each independent of the others. The class defines _what_ an object will look like; the object is an actual, individual instance of that definition, holding real values.

```java
class Student { String name; int age; }

Student s1 = new Student(); // one object
Student s2 = new Student(); // another, independent object
```

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define an object.
- Create objects in Java.
- Access object members.
- Understand object references.
- Explain the object creation process.

---

## 3. What is an Object?

### Definition

An object is an **instance of a class** — a self-contained unit that combines the fields (data) and methods (behavior) defined by its class, allocated in memory with its own actual values.

### Real-life analogy

If a class is the blueprint for a house, an object is an **actual house built** from that blueprint — with a real address, real paint color, and real furniture inside. Multiple houses (objects) can be built from the same blueprint (class), each one distinct and independently existing.

### Instance of a class

"Instance" and "object" are used interchangeably in Java. When we say `s1` is "an instance of `Student`," we mean `s1` is an object created using the `Student` class as its template.

### Characteristics

Every object has three defining characteristics:

#### Identity

A unique existence in memory that distinguishes it from every other object — even two objects with _identical_ field values are still two separate, distinct objects (they live at different memory addresses).

#### State

The actual values currently held in the object's fields at any given moment (e.g., `name = "Aditi"`, `age = 21`). State can change over the object's lifetime.

#### Behavior

The actions the object can perform, defined by its class's methods (e.g., `student.study()`), often operating on or using the object's own state.

```java
Student s1 = new Student();
s1.name = "Aditi"; // state
s1.age = 21;        // state
s1.study();          // behavior
// s1 itself, as a distinct memory entity, represents its identity
```

---

## 4. Why Do We Need Objects?

### Real-world modeling

Objects let us represent real-world entities (a student, a car, an account) directly in code, with their own data and behavior — making programs naturally mirror the problems they're solving.

### Memory allocation

Objects are what actually trigger memory allocation on the heap. A class by itself uses only a small, fixed amount of memory (its loaded definition in the Method Area); it's objects that consume heap memory proportional to how many you create.

### Multiple instances

Objects allow the same class definition to represent many different, independent real-world entities simultaneously — e.g., a school with 500 students, all created from one `Student` class, each with their own name, age, and grades.

### Data encapsulation

By bundling data (fields) and behavior (methods) inside an object, and controlling access with access modifiers, objects support **encapsulation** — protecting an object's internal state from being changed in uncontrolled ways from outside the class.

---

## 5. Class vs Object

| Aspect         | Class                                                  | Object                                        |
| -------------- | ------------------------------------------------------ | --------------------------------------------- |
| **Definition** | A blueprint/template                                   | An actual instance created from the blueprint |
| **Existence**  | Logical concept; no memory for data until instantiated | Physical entity; occupies heap memory         |
| **Creation**   | Declared once (in a `.java` file)                      | Can be created many times using `new`         |
| **Data**       | Defines _what_ fields exist, not actual values         | Holds actual, concrete field values           |
| **Memory**     | Loaded into Method Area/Metaspace                      | Allocated on the Heap                         |
| **Quantity**   | Exists once per program (per class definition)         | Many objects can exist from a single class    |
| **Example**    | `class Student { ... }`                                | `Student s1 = new Student();`                 |

---

## 6. Object Creation Process

### Step 1 → Class Declaration

The class must already exist, defining the structure the object will follow:

```java
class Student {
    String name;
    int age;
}
```

### Step 2 → Object Declaration

Declaring a **reference variable** of the class type — at this point, no object exists yet, and the reference is `null` (or uninitialized, if local).

```java
Student s1;
```

### Step 3 → Object Instantiation

Using the `new` keyword allocates memory on the heap for a new object.

```java
new Student();
```

### Step 4 → Object Initialization

The class's **constructor** runs automatically as part of `new`, setting up the object's initial field values (default or explicitly provided).

### Step 5 → Object Usage

Once created, the reference can be used to access the object's fields and methods.

```java
Student s1 = new Student();
s1.name = "Aditi";
s1.age = 21;
```

### Memory Diagram

<p align="center">
    <img src="https://www.expertsmind.com/CMSImages/1766_Untitled.png"
         alt="if-esle ladder"
         width="70%">
    <br>
</p>

## 7. Creating Objects

### Syntax

```java
Student s1 = new Student();
```

### Explain every part

- **`Student`** — the **type** of the reference variable; specifies that `s1` can only refer to `Student` objects.
- **`s1`** — the **name of the reference variable** being declared, which will store the address of the created object.
- **`=`** — the **assignment operator**, storing the result of the right-hand side (the object's address) into `s1`.
- **`new`** — the keyword that triggers **heap memory allocation** for a new object.
- **`Student()`** — a call to the **constructor** of the `Student` class, which initializes the newly allocated object.
- **`;`** — terminates the statement.

So, reading right to left: `new Student()` creates the object and returns its address → that address is assigned (`=`) to → the reference variable `s1`, declared as type `Student`.

---

## 8. Object Reference

### What is a reference?

A **reference** is a value that holds the **memory address** of an object on the heap — it's how Java lets you "access" and work with an object, without the object itself being stored directly in your variable.

### Reference vs Object

- The **object** is the actual data structure living on the heap (fields + their values).
- The **reference** is a small pointer/address, typically stored on the stack (for local variables), that "points to" that object.

They are not the same thing — a reference can be reassigned, copied, or set to `null`, all without affecting the actual object it once pointed to (unless it was the only reference, in which case the object becomes eligible for garbage collection).

### Reference Variable

```java
Student s1 = new Student();
```

Here `s1` is a **reference variable** — its value is the heap address of the `Student` object, not the object's data itself.

### Memory Diagram

```
Stack                 Heap
+--------+            +------------------------+
| s1     |----------> | Student object         |
+--------+            |   name = "Aditi"       |
                      |   age  = 21            |
                      +------------------------+
```

---

## 9. Accessing Object Members

### Fields

Object fields are accessed using the reference variable and the **dot operator**:

```java
s1.name = "Aditi";
System.out.println(s1.age);
```

### Methods

Object methods are called the same way:

```java
s1.study();
```

### Dot (.) Operator

The dot (`.`) operator means "go to the object this reference points to, and access the specified member (field or method) on it."

### Examples

```java
class Student {
    String name;
    int age;

    void displayInfo() {
        System.out.println(name + " is " + age + " years old.");
    }
}

Student s1 = new Student();
s1.name = "Aditi";      // accessing a field
s1.age = 21;             // accessing a field
s1.displayInfo();        // accessing/calling a method
```

---

## 10. Multiple Objects

### Example

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();

s1.name = "Aditi";
s2.name = "Rohan";
s3.name = "Meera";
```

Each of `s1`, `s2`, and `s3` is a **completely separate object**, with its own independent copy of `name` and `age`. Changing `s1.name` has no effect on `s2` or `s3`.

### Memory Diagram

```
Stack               Heap
+------+               +----------------------+
| s1   |-------------> | Student: "Aditi"     |
+------+               +----------------------+
| s2   |-------------> | Student: "Rohan"     |
+------+               +----------------------+
| s3   |-------------> | Student: "Meera"     |
+------+               +----------------------+
```

Three separate reference variables on the stack, each pointing to a distinct object on the heap.

---

## 11. Anonymous Objects

### Definition

An **anonymous object** is an object created **without** assigning it to a reference variable. It has no name/handle you can use to refer to it again later.

### Syntax

```java
new Student().displayInfo();
```

### Use Cases

- When an object is needed **only once**, immediately, and doesn't need to be reused (e.g., passing a temporary object as a method argument, or calling a single method right after creation).

```java
System.out.println(new Student("Aditi", 21).age);
```

### Limitations

- Since there's no reference to it, an anonymous object **cannot be reused or accessed again** after the statement in which it was created finishes executing.
- It becomes **immediately eligible for garbage collection** right after that statement completes, since nothing references it anymore.

---

## 12. Object Life Cycle

### (Simple introduction)

An object in Java generally goes through these stages:

1. **Created** — memory is allocated on the heap via `new`, and the constructor initializes it.
2. **Used** — the program accesses the object's fields and calls its methods through one or more references.
3. **Unreferenced** — all references to the object are removed (set to `null`, reassigned, or go out of scope), so the object is no longer reachable.
4. **Garbage Collection** — once unreferenced, the object becomes eligible for garbage collection; the JVM's Garbage Collector reclaims its heap memory at some later, unspecified point.

```java
Student s1 = new Student(); // Created
s1.displayInfo();            // Used
s1 = null;                    // Unreferenced -> eligible for GC
```

_(Garbage Collection is covered in full detail in the Memory Management chapter.)_

---

## 13. Objects in Heap Memory

### Detailed memory representation

```
Stack
  |
  |  (reference variable, e.g., s1, holds an address)
  v
Reference (the address value itself)
  |
  |  (points to)
  v
Heap
  |
  |  (actual object: fields + their current values)
  v
Student
  name = "Aditi"
  age  = 21
```

The **stack** holds the reference (a small, fixed-size address). That reference **points to** the actual object's location on the **heap**, where all of the object's real data lives. Accessing `s1.name` means: go to the stack, get the address stored in `s1`, follow it to the heap, and read the `name` field from the object found there.

---

## 14. Passing Objects

### Basic Introduction

When you pass an object to a method, Java copies the **reference (address)** — not the object itself — into the method's parameter. This means the method receives its own reference variable, but it points to the **same object** on the heap as the caller's reference.

```java
void updateAge(Student s) {
    s.age = 22;   // modifies the SAME object the caller's reference points to
}

Student s1 = new Student();
s1.age = 21;
updateAge(s1);
System.out.println(s1.age); // prints 22
```

_(Detailed coverage of parameter passing, including the distinction between mutating an object vs. reassigning the parameter, is provided in the Memory Management chapter.)_

---

## 15. Comparing Objects

### `==`

The `==` operator, when used on objects, compares **references** — i.e., whether both variables point to the exact same object in memory (the same address), not whether their field values are equal.

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = s1;

System.out.println(s1 == s2); // false (different objects)
System.out.println(s1 == s3); // true (same object, same reference)
```

### `equals()`

The `.equals()` method is meant to compare the **content/state** of two objects for logical equality. By default (inherited from `Object`), `equals()` behaves exactly like `==` unless the class **overrides** it to define what "equal" means for that specific type.

```java
System.out.println(s1.equals(s2)); // false by default, unless Student overrides equals()
```

_(Overriding `equals()` — along with `hashCode()` — is covered in a later chapter.)_

---

## 16. Common Mistakes

- **Null references** — trying to access fields/methods on a reference that was declared but never assigned an actual object (`Student s1; s1.name = "X";` → `NullPointerException`, since `s1` isn't pointing to any object).
- **Using object before initialization** — attempting to use fields before the object (or its fields) have been properly initialized, leading to unexpected default values (`null`, `0`, `false`).
- **Confusing object and reference** — assuming that copying a reference variable (`s2 = s1`) creates a **new object**, when in fact both variables now point to the **same** object.

```java
Student s2 = s1; // s2 does NOT create a new Student -- it's the same object as s1
```

---

## 17. Best Practices

- Use **meaningful object names** that reflect what the object represents (`student1`, `savingsAccount`) rather than vague names (`obj`, `temp`).
- **Avoid unnecessary objects** — don't create new objects repeatedly (e.g., inside a loop) when one reusable object would do, since this wastes memory and increases GC overhead.
- **Initialize properly** — always ensure an object's fields are set to valid, meaningful values (ideally through a constructor) before the object is used elsewhere in the program.

---

## 18. Interview Corner

1. **What is an object?**
   An object is a concrete instance of a class — an actual entity in memory with its own state (field values) and access to the behavior (methods) defined by its class.

2. **What is an instance?**
   "Instance" is another term for "object" — specifically, it emphasizes that the object was created _from_ (is an instance _of_) a particular class.

3. **Difference between object and reference?**
   The object is the actual data structure on the heap; the reference is a variable (usually on the stack) holding the object's memory address, used to access it.

4. **Where are objects stored?**
   Objects are stored on the **heap**; the reference variables pointing to them may live on the stack (if local) or inside other objects/static fields.

5. **What does `new` do?**
   `new` allocates memory on the heap for a new object, invokes the class's constructor to initialize it, and returns a reference (address) to that object.

---

## 19. MCQs

1. An object is:
   a) A blueprint b) An instance of a class c) A method d) A package

2. Where are objects stored in memory?
   a) Stack b) Heap c) Method Area d) Register

3. What does `new Student()` return?
   a) Nothing b) A copy of the class c) A reference to the newly created object d) A boolean

4. Which operator is used to access an object's members?
   a) `->` b) `::` c) `.` d) `#`

5. What happens when you assign `Student s2 = s1;`?
   a) A new object is created b) `s2` points to the same object as `s1` c) `s1` is destroyed d) Compilation error

6. An anonymous object is:
   a) An object with a private name b) An object created without a reference variable c) A static object d) An invalid object

7. `==` on two object references checks:
   a) Field equality b) Whether both point to the same object c) Method equality d) Class name equality

8. What is the default behavior of `.equals()` before overriding?
   a) Compares field values b) Same as `==` (reference comparison) c) Always returns true d) Throws an exception

9. When does an object become eligible for garbage collection?
   a) Immediately after creation b) When no reference points to it c) When declared d) Never

10. Accessing a field on a `null` reference causes:
    a) A default value b) `NullPointerException` c) `0` to be returned d) Nothing happens

11. Which of the following creates an object?
    a) `Student s1;` b) `class Student {}` c) `Student s1 = new Student();` d) `import Student;`

12. A reference variable stores:
    a) The full object b) The object's memory address c) A copy of all fields d) Nothing

13. Passing an object to a method passes:
    a) A full copy of the object b) A copy of the reference (address) c) Nothing d) The class definition

14. How many objects can be created from a single class?
    a) Only one b) Exactly two c) As many as needed d) Zero

15. `Student s1 = new Student(); Student s2 = new Student();` — are `s1` and `s2` the same object?
    a) Yes b) No, they are separate objects c) Only if fields match d) Only in the same method

16. What triggers heap memory allocation?
    a) Class declaration b) The `new` keyword c) Method calls d) Variable declaration only

17. Object identity refers to:
    a) The object's field values b) A unique existence distinguishing it from other objects c) Its class name d) Its method count

18. Which statement about class vs. object is TRUE?
    a) A class can only create one object b) An object can exist without a class c) A class can be used to create many objects d) A class and object are the same thing

19. What is object "state"?
    a) The class name b) The current values of its fields c) Its memory address d) Its access modifier

20. Which of these best describes encapsulation's connection to objects?
    a) Objects have no data b) Objects bundle data and behavior together with controlled access c) Objects cannot have methods d) Objects are always static

---

## 20. University Questions

### Short Questions

1. Define an object with an example.
2. Differentiate between a class and an object.
3. What is an object reference?
4. What is an anonymous object?
5. List the characteristics of an object.

### Long Questions

1. Explain the step-by-step process of object creation in Java with a memory diagram.
2. Discuss the relationship between a class and its objects, with suitable examples.
3. Explain how object references work in Java, including how multiple references can point to the same object.
4. Describe the object life cycle from creation to garbage collection.
5. Explain the difference between `==` and `.equals()` when comparing objects, with examples.

---

## 21. Viva Questions

1. What is an object in Java?
2. How is an object different from a class?
3. What does the `new` keyword do?
4. What is stored in a reference variable?
5. Can two reference variables point to the same object? What happens if you modify the object through one of them?
6. What is a `NullPointerException` and when does it occur?
7. What is an anonymous object, and when would you use one?
8. Where does an object live in memory versus its reference?
9. What happens to an object when it has no more references?
10. What is the difference between `==` and `.equals()` for objects?

---

## 22. Programming Exercises

1. Create a `Student` object, assign values to its fields, and display them.
2. Create multiple `Car` objects with different brands and speeds, and print their details.
3. Create `BankAccount` objects and demonstrate depositing and checking balances.
4. Create `Employee` objects, assign salaries, and display each employee's information.

_(Focus on object creation, field assignment, and member access using the concepts covered in this chapter.)_

---

## 23. Summary

An object is a concrete, individually existing instance of a class, created using the `new` keyword, and stored in heap memory. While a class defines the structure (fields and methods) an object will have, the object itself holds actual data — its state — and can perform actions through its methods. Objects are accessed through **reference variables**, which store the object's memory address rather than the object itself; this distinction explains why copying a reference doesn't create a new object, and why passing an object to a method allows the method to modify its state but not redirect the caller's own reference. Multiple, independent objects can be created from a single class, each with its own identity and state, making objects the fundamental building blocks for representing real-world data in an object-oriented program.

---

## 24. Key Takeaways

- An object is an instance of a class, created using `new`, and stored on the heap.
- Every object has identity, state, and behavior.
- A reference variable holds an object's memory address, not the object itself.
- Multiple reference variables can point to the same object; changes through one are visible through all.
- Anonymous objects have no reference and are used-and-discarded in a single statement.
- `==` compares object references; `.equals()` compares logical content (once overridden).
- An object becomes eligible for garbage collection once it has no reachable references.

---

## 25. Cheat Sheet

| Concept                 | Quick Reference                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------- |
| **Object**              | An instance of a class, created via `new`, living on the heap                       |
| **Reference**           | A variable holding an object's memory address                                       |
| **Creation Syntax**     | `ClassName varName = new ClassName();`                                              |
| **Access Members**      | `objectRef.fieldName` / `objectRef.methodName()`                                    |
| **Multiple References** | Copying a reference (`s2 = s1`) points to the SAME object                           |
| **Anonymous Object**    | `new ClassName().method();` — no reference, used once                               |
| **Object Life Cycle**   | Created → Used → Unreferenced → Eligible for GC                                     |
| **`==` on objects**     | Compares references (addresses)                                                     |
| **`.equals()`**         | Compares content — must be overridden for meaningful use                            |
| **Passing Objects**     | Reference is passed by value; same object can be mutated, not reassigned externally |

---

_End of Notes — Objects in Java_
