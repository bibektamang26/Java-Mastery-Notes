# 08. Passing Data to Methods in Java

## 1. Introduction

- **What is Parameter Passing?** Parameter passing is the mechanism by which values are transferred from the code that calls a method into that method's own parameter variables, allowing the method to work with data supplied by the caller.
- **Why is Data Passed to Methods?** Methods are designed to be reusable blocks of logic — the same method (e.g., calculating the area of a rectangle) needs to work with different inputs each time it's called. Parameter passing is what makes this flexibility possible, rather than hardcoding fixed values inside every method.
- **Importance of Parameter Passing:** Understanding exactly _how_ Java passes data — especially the distinction between primitives and objects — is one of the most important and most commonly misunderstood topics in Java. It directly affects whether changes made inside a method are visible to the caller after the method returns, a subject frequently tested in interviews and exams.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand parameter passing.
- Pass primitive values to methods.
- Pass objects to methods.
- Pass arrays to methods.
- Return values from methods.
- Explain why Java is Pass-by-Value.

---

## 3. What is Parameter Passing?

**Definition:** Parameter passing refers to the process of supplying values (called **arguments**) to a method's declared **parameters** when the method is invoked, so the method can use those values during its execution.

**Purpose:** It allows a single method definition to operate on different data each time it is called, making code reusable, modular, and far more flexible than duplicating logic for every possible input.

**Working:** When a method is called, Java evaluates each argument expression, and copies the resulting value into the corresponding parameter variable inside the method. The method then executes using these parameter variables exactly as if they were its own local variables.

**Examples:**

```java
static int add(int a, int b) {      // a and b are parameters
    return a + b;
}

public static void main(String[] args) {
    int result = add(5, 10);         // 5 and 10 are arguments
    System.out.println(result);      // 15
}
```

---

## 4. Method Parameters vs Method Arguments

**Definition:**

- A **parameter** is the variable listed in a method's declaration/signature — it acts as a placeholder for a value the method expects to receive.
- An **argument** is the actual value supplied when the method is **called**, which gets copied into the corresponding parameter.

**Difference:** Parameters exist in the method's _definition_; arguments exist at the method's _call site_. Every argument passed must correspond to a parameter of a compatible type, in the same order.

**Examples:**

```java
static void greet(String name, int age) {   // name, age = parameters
    System.out.println("Hello " + name + ", age " + age);
}

public static void main(String[] args) {
    greet("Asha", 22);   // "Asha", 22 = arguments
}
```

**Comparison Table:**

| Aspect        | Parameter                                | Argument                    |
| ------------- | ---------------------------------------- | --------------------------- |
| Defined where | In the method's declaration/signature    | At the method call site     |
| Represents    | A placeholder variable expecting a value | The actual value supplied   |
| Example       | `int a` in `add(int a, int b)`           | `5` in `add(5, 10)`         |
| Scope         | Local to the method                      | Belongs to the calling code |
| Also known as | Formal parameter                         | Actual parameter            |

---

## 5. Passing Primitive Data Types

Primitive types (`int`, `double`, `char`, `boolean`, etc.) are passed to methods by copying their **actual value** into the method's parameter.

**Passing `int`:**

```java
static void increment(int num) {
    num = num + 1;
    System.out.println("Inside method: " + num);
}

public static void main(String[] args) {
    int x = 10;
    increment(x);
    System.out.println("After method call: " + x);
}
```

**Output:**

```
Inside method: 11
After method call: 10
```

**Passing `double`:**

```java
static void applyDiscount(double price) {
    price = price * 0.9;
    System.out.println("Discounted (inside method): " + price);
}
```

**Passing `char`:**

```java
static void printNext(char ch) {
    ch = (char) (ch + 1);
    System.out.println("Next char: " + ch);
}
```

**Passing `boolean`:**

```java
static void toggle(boolean flag) {
    flag = !flag;
    System.out.println("Toggled (inside method): " + flag);
}
```

**Memory Diagram:**

```
main() Stack Frame:        increment() Stack Frame:
  x = 10   ──copy──▶          num = 10  (separate memory)

Inside increment(): num becomes 11
                              num = 11

Back in main(): x is untouched
  x = 10   (still 10, completely unaffected by num's change)
```

In every case above, the change made _inside_ the method has **no effect** on the original variable in the caller — because only a **copy** of the value was passed in.

---

## 6. Java is Pass-by-Value

**Definition:** Java is strictly a **pass-by-value** language. This means that when a method is called, Java always copies the value of each argument into the corresponding parameter — the method never receives direct access to the original variable itself, only a copy of what it held at the time of the call.

**Why?** This design keeps method calls predictable and self-contained: a method can never accidentally overwrite the caller's original primitive variable, since it's only ever working with its own private copy.

**Common Misconception:** Many beginners (and even some experienced programmers coming from other languages) believe Java is "pass-by-reference" for objects, because modifying an object's fields _inside_ a method **is** visible outside the method. This is misleading — Java is passing a **copy of the object's reference (address)**, not the object itself, and definitely not a reference to the _variable_ holding that reference. See Section 12 for a complete breakdown of this distinction.

**Examples:**

```java
static void modify(int num) {
    num = 100;   // only changes the local copy
}

public static void main(String[] args) {
    int value = 5;
    modify(value);
    System.out.println(value);   // still 5 — unaffected
}
```

**Memory Diagram:**

```
Before call:   value = 5     (in main's stack frame)

Call modify(value):
   → num = 5    (a brand-new, separate copy, in modify's own stack frame)

Inside modify(): num changed to 100
   → num = 100  (only this copy changes)

After modify() returns:
   → value = 5   (main's original variable, completely untouched)
```

---

## 7. Passing Objects

**Definition:** When an object is "passed" to a method, what is actually copied into the parameter is the **value of the reference** (essentially, the object's memory address on the heap) — not the object itself, and not the original reference variable.

**Reference Variables:** A reference variable (like `Student s = new Student();`) doesn't store the object directly; it stores the **address** of the object, which actually lives on the heap.

**Reference Copy:** When an object is passed to a method, the method's parameter receives a **copy of that address** — both the original variable and the method's parameter now point to the **exact same object** in heap memory.

**Examples:**

```java
class Student {
    String name;
}

static void updateName(Student s) {
    s.name = "Updated Name";   // modifies the SAME object the caller's variable points to
}

public static void main(String[] args) {
    Student student = new Student();
    student.name = "Original Name";

    updateName(student);
    System.out.println(student.name);  // "Updated Name" — the caller SEES this change
}
```

**Output:**

```
Updated Name
```

This is exactly the behavior that leads to the "Java is pass-by-reference" misconception — but it is still pass-by-value; what's being passed by value is the **reference itself** (the address).

**Reassignment does NOT affect the caller (proving it's still pass-by-value):**

```java
static void reassign(Student s) {
    s = new Student();          // s now points to a brand-new object
    s.name = "Brand New Student";
}

public static void main(String[] args) {
    Student student = new Student();
    student.name = "Original Name";

    reassign(student);
    System.out.println(student.name);  // still "Original Name" — unaffected!
}
```

**Output:**

```
Original Name
```

Reassigning the parameter `s` inside the method only changes what the **local copy** of the reference points to — it has no effect on the original `student` variable back in `main()`, which still points to the original object. This definitively proves Java is pass-by-value, even for objects.

**Memory Diagram:**

```
main() Stack:                    Heap:
  student ──────────────────▶  [ Student Object A: name = "Original Name" ]
                                        ▲
updateName() Stack:                    │
  s ──────────copy of address──────────┘
  (s and student point to the SAME object)

  s.name = "Updated Name"  →  modifies Object A directly (visible to both)


---- After reassign() example ----

main() Stack:                    Heap:
  student ──────────────────▶  [ Student Object A: name = "Original Name" ]

reassign() Stack:
  s ─────(originally pointed to A, then reassigned)────▶ [ Student Object B: name = "Brand New Student" ]

  (student still points to A; only s's local copy was redirected to B)
```

---

## 8. Passing Arrays

**Definition:** Arrays in Java are **objects**, so they follow the same reference-passing behavior described above — a copy of the array's reference (address) is passed to the method, meaning both the caller and the method refer to the **same underlying array** in memory.

**Examples:**

```java
static void doubleValues(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        arr[i] = arr[i] * 2;   // modifies the SAME array the caller has
    }
}

public static void main(String[] args) {
    int[] numbers = {1, 2, 3, 4, 5};
    doubleValues(numbers);

    for (int num : numbers) {
        System.out.print(num + " ");   // 2 4 6 8 10 — changes ARE visible
    }
}
```

**Output:**

```
2 4 6 8 10
```

**Reassigning the array parameter does NOT affect the caller's array (same principle as objects):**

```java
static void replaceArray(int[] arr) {
    arr = new int[]{100, 200, 300};   // arr now points to a new array
}

public static void main(String[] args) {
    int[] numbers = {1, 2, 3};
    replaceArray(numbers);
    System.out.println(numbers[0]);   // still 1 — unaffected
}
```

**Memory Diagram:**

```
main() Stack:                    Heap:
  numbers ─────────────────▶  [ 1, 2, 3, 4, 5 ]
                                     ▲
doubleValues() Stack:                │
  arr ─────copy of address ──────────┘
  (arr and numbers point to the SAME array)

  arr[i] = arr[i] * 2  →  modifies the shared array directly, visible to both
```

---

## 9. Returning Data from Methods

**Returning Primitive Values:** A method with a primitive return type sends back a **copy** of a computed value, which the caller can store or use directly.

```java
static int square(int num) {
    return num * num;
}

int result = square(5);   // result = 25
```

**Returning Objects:** A method can return a reference to an object, which the caller can then store in its own reference variable — both then point to the same object.

```java
static Student createStudent(String name) {
    Student s = new Student();
    s.name = name;
    return s;
}

Student newStudent = createStudent("Ravi");
System.out.println(newStudent.name);  // Ravi
```

**Returning Arrays:** Similarly, a method can create and return an array, giving the caller a reference to it.

```java
static int[] generateSquares(int n) {
    int[] result = new int[n];
    for (int i = 0; i < n; i++) {
        result[i] = (i + 1) * (i + 1);
    }
    return result;
}

int[] squares = generateSquares(5);
// squares = {1, 4, 9, 16, 25}
```

**Examples — combining passing and returning:**

```java
static int[] appendValue(int[] arr, int value) {
    int[] newArr = new int[arr.length + 1];
    System.arraycopy(arr, 0, newArr, 0, arr.length);
    newArr[arr.length] = value;
    return newArr;
}

int[] original = {1, 2, 3};
int[] updated = appendValue(original, 4);
// original = {1, 2, 3}  (unchanged, since a NEW array was created and returned)
// updated  = {1, 2, 3, 4}
```

---

## 10. What Actually Happens in Memory?

```
Primitive
   ↓
 Stack

Reference (for objects/arrays)
   ↓
 Heap
```

**Illustrations:**

- **Primitive variables** live entirely within **stack** memory. When passed to a method, the actual bits representing the value are copied into a new stack slot for the parameter.

```
Stack:
  main(): x = 10
  method(): num = 10   (independent copy, own slot)
```

- **Object/array variables** store a **reference (address)** in stack memory, while the actual object/array data lives in **heap** memory. When passed to a method, only the reference (the address) is copied — both the original variable and the parameter end up pointing to the **same heap location**.

```
Stack:                        Heap:
  main(): obj ───────────▶  [ Actual Object Data ]
  method(): param ────┘    (both point to the same heap object)
```

This is the core reason why modifying an object's _fields_ inside a method is visible to the caller, but _reassigning_ the reference variable itself inside the method is not — the stack-level reference copy is independent, even though it initially points to the same heap object.

---

## 11. Primitive Passing vs Object Passing

| Aspect                                         | Primitive Passing                  | Object Passing (including Arrays)                               |
| ---------------------------------------------- | ---------------------------------- | --------------------------------------------------------------- |
| What is copied                                 | The actual value                   | The reference (address), not the object itself                  |
| Storage location                               | Stack                              | Reference in stack, actual data in heap                         |
| Modifying value/fields inside method           | Not visible to caller              | Visible to caller (same underlying object is modified)          |
| Reassigning the parameter itself inside method | Not visible to caller (expected)   | Also NOT visible to caller (common misconception aside)         |
| Underlying mechanism                           | Pass-by-value                      | Still pass-by-value — but the value being copied is a reference |
| Example                                        | `int`, `double`, `char`, `boolean` | `Student`, `int[]`, `String`, any class type                    |

---

## 12. Call by Value vs Call by Reference

**Definition:**

- **Call by Value:** A copy of the argument's value is passed to the method; changes made to the parameter inside the method do not affect the original argument.
- **Call by Reference:** The method receives direct access to the original variable itself (not a copy); changes made to the parameter inside the method **do** affect the original argument, including reassignment.

**Java's Behaviour:** Java is **exclusively call by value** — for both primitives and objects. For objects, what gets copied "by value" is the **reference** (the address) itself, not the object's data, and definitely not the original reference variable.

**Why Java is NOT Pass-by-Reference:** If Java were truly pass-by-reference, then **reassigning** a parameter inside a method (`s = new Student();`) would cause the caller's original variable to also point to the new object. As demonstrated in Section 7, this does **not** happen in Java — the caller's variable remains completely unaffected by any reassignment inside the method. This is the definitive proof that Java is pass-by-value, even when working with object references.

**Examples (summary of the definitive test):**

```java
static void trueReferenceTest(Student s) {
    s = new Student();       // if Java were pass-by-reference, this WOULD affect the caller
    s.name = "Changed";
}
// Result: caller's original object is untouched — proving pass-by-value
```

**Interview Explanation:** The cleanest way to explain this in an interview is: _"Java always passes a copy of the value. For primitives, that value is the data itself. For objects, that value is a reference (memory address) to the object. Because the reference itself is copied, changes to the object's internal state are visible to the caller, but reassigning the parameter to point elsewhere is not — which is exactly what call-by-value predicts, and call-by-reference would not."_

---

## 13. Real-world Examples

**Student Object:**

```java
class Student {
    String name;
    int marks;
}

static void updateMarks(Student s, int newMarks) {
    s.marks = newMarks;   // visible to caller — same object modified
}
```

**Bank Account:**

```java
class BankAccount {
    double balance;
}

static void deposit(BankAccount account, double amount) {
    account.balance += amount;   // caller's account balance is updated
}
```

**Shopping Cart:**

```java
class Cart {
    java.util.List<String> items = new java.util.ArrayList<>();
}

static void addItem(Cart cart, String item) {
    cart.items.add(item);   // caller's cart now includes the new item
}
```

**Employee:**

```java
class Employee {
    double salary;
}

static void giveRaise(Employee emp, double percentage) {
    emp.salary += emp.salary * (percentage / 100);   // caller's employee salary updated
}
```

**Library (Book collection):**

```java
class Library {
    java.util.List<String> books = new java.util.ArrayList<>();
}

static void addBook(Library library, String bookTitle) {
    library.books.add(bookTitle);   // reflected in the caller's library instance
}
```

In every one of these examples, the method modifies the object's **internal state**, which is why the change is visible to the caller — but if any of these methods instead did `account = new BankAccount();` or similar, that reassignment would **not** be visible outside the method.

---

## 14. Best Practices

- Understand that Java is **always** pass-by-value — this mental model prevents a huge class of bugs and confusion, especially around objects and arrays.
- When a method needs to return **multiple** modified values, prefer returning an object or array rather than relying on "side effects" through reassigned parameters (which won't work anyway).
- Be cautious when passing mutable objects (like arrays or custom objects) into methods — unintentional modification of the caller's data is a common source of bugs if not handled carefully.
- If a method should **not** modify the caller's object, consider passing a copy (a defensive/deep copy) of the object or array instead of the original reference.
- Clearly document whether a method modifies its object/array parameters as a side effect, since this isn't always obvious just from the method signature.

---

## 15. Common Mistakes

1. **Expecting a primitive change inside a method to persist outside it:**

   ```java
   static void addFive(int x) {
       x += 5;
   }
   int num = 10;
   addFive(num);
   System.out.println(num);  // still 10, NOT 15 — common beginner mistake
   ```

2. **Believing Java is pass-by-reference because object field changes are visible:**

   ```java
   // Object field mutation IS visible — but this does NOT mean Java is pass-by-reference.
   // The definitive test is reassignment (see Section 12), which is NOT visible.
   ```

3. **Expecting reassignment of an object parameter to affect the caller:**

   ```java
   static void reset(Student s) {
       s = null;   // only the LOCAL copy of the reference becomes null
   }
   Student student = new Student();
   reset(student);
   System.out.println(student == null);  // false — student is still a valid reference
   ```

4. **Accidentally modifying a caller's array/object when only a "read-only" operation was intended:**

   ```java
   static void printAndAccidentallyModify(int[] arr) {
       arr[0] = 0;   // unintentionally changes the caller's array too
       System.out.println(arr[0]);
   }
   ```

5. **Assuming returning `void` means a method can't affect the caller's data at all** — it can, if it modifies a passed-in mutable object's internal state, even without returning anything explicitly.

---

## 16. Interview Corner

**Q1. Is Java pass-by-value or pass-by-reference?**
Java is strictly pass-by-value, for both primitives and objects. For objects, the "value" being passed is the reference (memory address) itself — not the object's data, and not the original reference variable.

**Q2. If Java is pass-by-value, why do changes to an object's fields inside a method appear outside the method too?**
Because the copied reference still points to the exact same object on the heap — modifying the object's internal state through that reference affects the one shared object, which both the caller and the method can see.

**Q3. What is the definitive proof that Java is not pass-by-reference?**
Reassigning an object parameter inside a method to point to a brand-new object has no effect on the caller's original variable — if Java were truly pass-by-reference, this reassignment would be visible to the caller, but it isn't.

**Q4. Are arrays passed by value or reference in Java?**
Arrays are objects in Java, so they follow the same rule as any other object — a copy of the array's reference is passed, meaning in-place modifications are visible to the caller, but reassigning the array parameter is not.

**Q5. Does returning a value from a method count as "passing data"?**
It's a related but distinct mechanism — parameter passing sends data _into_ a method, while a `return` statement sends data _back out_ to the caller; both work by copying values (or references) in the same pass-by-value manner.

**Q6. How would you make a method that truly does not modify the caller's object at all?**
By creating and modifying a defensive copy of the object/array inside the method, rather than modifying the original object's fields directly — ensuring the caller's original data remains completely untouched.

---

## 17. MCQs

**1. What does Java copy when passing a primitive to a method?**
a) The variable's memory address
b) The actual value
c) Nothing, primitives can't be passed
d) A reference to the variable
**Answer: b) The actual value**

**2. What does Java copy when passing an object to a method?**
a) The entire object
b) A copy of the reference (address) to the object
c) Nothing
d) A deep clone of the object
**Answer: b) A copy of the reference (address) to the object**

**3. What is the output of this code?**

```java
static void change(int x) { x = 50; }
int num = 10;
change(num);
System.out.println(num);
```

a) 50
b) 10
c) 0
d) Compile error
**Answer: b) 10**

**4. What is the output of this code?**

```java
static void changeName(Student s) { s.name = "New"; }
Student student = new Student();
student.name = "Old";
changeName(student);
System.out.println(student.name);
```

a) Old
b) New
c) null
d) Compile error
**Answer: b) New**

**5. What is the output of this code?**

```java
static void reset(Student s) { s = new Student(); s.name = "Fresh"; }
Student student = new Student();
student.name = "Old";
reset(student);
System.out.println(student.name);
```

a) Old
b) Fresh
c) null
d) Compile error
**Answer: a) Old**

**6. Is Java pass-by-reference?**
a) Yes, always
b) No, it is always pass-by-value
c) Only for objects
d) Only for primitives
**Answer: b) No, it is always pass-by-value**

**7. What happens when an array is passed to a method and its elements are modified inside?**
a) Changes are not visible to the caller
b) Changes ARE visible to the caller, since it's the same underlying array
c) A compile error occurs
d) The array is automatically copied
**Answer: b) Changes ARE visible to the caller, since it's the same underlying array**

**8. Where does a primitive local variable live in memory?**
a) Heap
b) Stack
c) Method area
d) Register only
**Answer: b) Stack**

**9. Where does an object's actual data live in memory?**
a) Stack
b) Heap
c) Both equally
d) CPU cache
**Answer: b) Heap**

**10. What proves Java is pass-by-value even for objects?**
a) Field modifications being visible to the caller
b) Reassigning the parameter having no effect on the caller's variable
c) Objects being stored on the heap
d) Arrays being objects
**Answer: b) Reassigning the parameter having no effect on the caller's variable**

---

## 18. University Questions

1. Define parameter passing. Differentiate between parameters and arguments with examples.
2. Explain how primitive data types are passed to methods in Java, with a suitable example and memory diagram.
3. Explain why Java is considered a pass-by-value language, even when working with objects.
4. With a program example, prove that Java does not support pass-by-reference for objects.
5. Explain how arrays are passed to methods in Java, and describe what happens when array elements are modified versus when the array reference itself is reassigned inside a method.
6. Discuss the difference between call-by-value and call-by-reference, and explain which one Java implements, with supporting examples.
7. Explain how a method can return primitive values, objects, and arrays, with one example of each.

---

## 19. Viva Questions

1. What is the difference between a parameter and an argument?
2. Does modifying a primitive parameter inside a method affect the caller's variable?
3. Why does modifying an object's field inside a method affect the caller's object?
4. What happens if you reassign an object parameter inside a method?
5. Is an array passed by value or by reference in Java?
6. Where are primitive variables stored, and where are objects stored?
7. What is the single clearest way to prove Java is pass-by-value, not pass-by-reference?
8. Can a method return more than one value directly? How can multiple values be returned indirectly?
9. What is a defensive copy, and why might it be used when passing objects to methods?
10. Does returning an object from a method create a new object, or share the same object with the caller?

---

## 20. Programming Exercises

**Easy**

1. Write a program that passes an `int` to a method, modifies it inside the method, and shows that the caller's variable remains unchanged.
2. Write a program that passes a `Student` object to a method, modifies one of its fields, and shows that the change is visible in the caller.

**Medium**

3. Write a program that passes an array to a method, doubles every element inside the method, and prints the array in `main()` to confirm the changes are visible.
4. Write a program demonstrating that reassigning an object parameter inside a method does not affect the caller's original object.

**Hard**

5. Write a program with a method that accepts an array and an integer, returns a **new** array with the integer appended, and demonstrates that the original array passed in remains unmodified.

---

## 21. Challenge Problems

**Bank Transfer Simulation:** Write a program with a `transfer` method that accepts two `BankAccount` objects and an amount, deducting from one and adding to the other, demonstrating how object field changes persist outside the method.

**Immutable Point Class:** Design a simple `Point` class and a method that attempts to "move" a point by reassigning the parameter to a new `Point` object — demonstrate that the caller's original point is unaffected, and then redesign the method to properly modify the point's fields instead.

**Array Swap Utility:** Write a method that attempts to swap two `int` array references by reassigning parameters, and explain (with output) why the swap does not affect the caller's variables — then write a correct version that actually swaps the _contents_ of the two arrays.

**Deep Copy vs Shallow Reference:** Write a program that demonstrates the difference between passing an object directly (shared reference) versus passing a manually created deep copy of that object into a method.

---

## 22. Summary

- **Parameter passing** is how arguments supplied at a method call are transferred into a method's own parameter variables.
- Java is **always pass-by-value** — for primitives, the actual value is copied; for objects and arrays, a copy of the **reference (address)** is copied, not the object itself.
- Because both the caller's variable and the method's parameter reference point to the **same object**, modifying that object's fields inside the method is visible to the caller.
- **Reassigning** an object or array parameter inside a method has **no effect** on the caller's original variable — this is the definitive proof that Java does not support true pass-by-reference.
- Primitives live in **stack** memory; objects and arrays store their reference in the stack, with actual data living in **heap** memory.
- Methods can return primitive values, object references, or array references back to the caller using `return`.

---

## 23. Key Takeaways

- Java is 100% pass-by-value — no exceptions, even for objects.
- For objects/arrays, it's the **reference** that's passed by value, not the object itself.
- Field/element modifications through a passed reference ARE visible to the caller.
- Reassigning the parameter itself is NEVER visible to the caller.
- Primitives → Stack. Objects/Arrays → reference in Stack, data in Heap.
- The reassignment test is the cleanest way to prove/explain Java's pass-by-value nature.

---

## 24. Cheat Sheet

```
PASSING PRIMITIVES
- Copies the actual value
- Changes inside method → NOT visible to caller

PASSING OBJECTS / ARRAYS
- Copies the reference (address), not the object
- Field/element changes inside method → VISIBLE to caller (same object)
- Reassigning the parameter itself → NOT visible to caller (proves pass-by-value)

MEMORY MODEL
Primitive        → stored directly in Stack
Object reference → stored in Stack, actual object data in Heap
Array reference   → stored in Stack, actual array data in Heap

DEFINITIVE PASS-BY-VALUE PROOF
void reset(Student s) {
    s = new Student();   // only redirects the LOCAL copy of the reference
}
// caller's original object reference is untouched after the call

RETURNING DATA
return value;      // primitive: copy of the value sent back
return object;     // object: reference copied back to caller
return array;      // array: reference copied back to caller

RULE OF THUMB
"Java copies the value of the argument — always.
 For objects, that value just happens to be an address."
```
