# 09. Garbage Collection in Java

---

## 1. Introduction

- **What is Garbage Collection?** Garbage Collection (GC) is the process by which the Java Virtual Machine (JVM) automatically identifies objects that are no longer being used by a program and reclaims the heap memory they occupy, without the programmer having to manually free that memory.
- **Why is Garbage Collection Needed?** Every object created in Java lives on the heap, and heap memory is finite. If unused objects were never removed, a running program would eventually consume all available memory and crash — garbage collection prevents this by continuously cleaning up memory that is no longer reachable or needed.
- **Importance of Automatic Memory Management:** In languages like C or C++, programmers must manually allocate and free memory, which is a common source of serious bugs — memory leaks (forgetting to free memory) and dangling pointers (using memory after it's freed). Java's automatic garbage collection removes this burden entirely from the programmer, making Java programs significantly safer and easier to write, at the cost of some control over exactly when memory is freed.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand Garbage Collection.
- Explain object eligibility.
- Understand unreachable objects.
- Explain the role of the JVM Garbage Collector.
- Avoid common misconceptions.

---

## 3. What is Garbage Collection?

**Definition:** Garbage Collection is an automatic memory management process performed by the JVM, which identifies objects on the heap that are no longer reachable by any active part of the program, and reclaims the memory they occupy for future use.

**Automatic Memory Management:** Unlike languages that require explicit calls to free memory (like `free()` in C), Java handles memory deallocation entirely automatically — the programmer only needs to create objects; the JVM takes care of destroying them once they're no longer needed.

**JVM Responsibility:** Garbage collection is not a language feature controlled directly by the programmer — it is entirely managed internally by the JVM, running as a background process that decides when and how to reclaim memory.

**Why Java uses Garbage Collection:** Java was designed with safety, simplicity, and reliability as core goals. Automatic garbage collection eliminates an entire category of bugs related to manual memory management (memory leaks, use-after-free errors, double-free errors), allowing programmers to focus on program logic rather than low-level memory bookkeeping.

---

## 4. Why Do We Need Garbage Collection?

**Memory is Limited:** Every running program has access to only a finite amount of heap memory. Without a mechanism to reclaim memory from objects that are no longer needed, a long-running program would continuously consume more and more memory until it runs out entirely.

**Prevent Memory Leaks:** In manually-managed languages, forgetting to free an object leads to a memory leak — memory that's no longer used but never released. Garbage collection largely (though not entirely — see Section 12) protects against this by automatically identifying and cleaning up unreachable objects.

**Free Unused Objects:** As a program runs, many objects are created temporarily and then no longer needed (e.g., a `Scanner` used briefly, or a local object inside a method). Garbage collection frees this memory automatically so it can be reused.

**Improve Performance:** By efficiently reclaiming unused memory in the background, garbage collection helps a program continue running smoothly for long periods without progressively slowing down or crashing due to memory exhaustion.

**Real-world analogy:** Think of garbage collection like a city's waste management system. Households (the program) generate garbage (unused objects) continuously. Instead of requiring each household to personally haul their own trash to a dump (manual memory management), the city runs an automatic collection service (the garbage collector) that periodically identifies and removes trash, freeing up space — without residents needing to think about it at all.

---

## 5. Memory Review

**Quick revision:**

- **Stack Memory:** Stores method call frames, local variables, and reference variables. Memory here is automatically reclaimed the moment a method finishes executing — it's fast, but limited and short-lived.
- **Heap Memory:** Stores all objects created using `new`, including arrays. Objects here persist beyond the method that created them, and their memory must be explicitly reclaimed once no longer needed — this is exactly what garbage collection manages.
- **References:** A reference variable (stored on the stack, or as a field within another object) holds the _address_ of an object on the heap — it does not contain the object's data directly.
- **Objects:** The actual data and state of a class instance, always stored on the heap, accessed indirectly through one or more reference variables.

```
Stack:                        Heap:
  student ───────────────▶  [ Student Object: name="Asha", marks=85 ]
```

---

## 6. Object Life Cycle

```
Object Creation
      ↓
Object Usage
      ↓
Object Becomes Unreachable
      ↓
Eligible for Garbage Collection
      ↓
Memory Reclaimed
```

**Flow Diagram:**

```
        ┌──────────────────┐
        │  new Student()   │   ←--- Object created on heap, reference assigned
        └─────────┬────────┘
                  ▼
        ┌──────────────────┐
        │  Object in Use   │ ←-- Accessed/modified through its reference(s)
        └─────────┬────────┘
                  ▼
        ┌────────────────────────┐
        │ Reference removed/null │←---- No active reference points to it anymore
        │ or reassigned          │
        └─────────┬──────────────┘
                  ▼
        ┌──────────────────────────┐
        │Object becomes Unreachable│
        └─────────┬────────────────┘
                  ▼
        ┌──────────────────────────┐
        │ Eligible for Garbage     │  ← JVM may collect it, but timing is not guaranteed
        │ Collection               │
        └─────────┬────────────────┘
                  ▼
        ┌──────────────────┐
        │ Memory Reclaimed │  ← Heap space freed for future use
        └──────────────────┘
```

---

## 7. Object Eligibility

**When does an object become eligible?** An object becomes eligible for garbage collection the moment it is no longer **reachable** — meaning there is no live reference chain leading from any active part of the program (stack variables, static fields, or other reachable objects) to that object.

**Common situations that make an object eligible:**

- **Reference becomes `null`:** Explicitly setting a reference variable to `null` removes the only path to the object it previously pointed to.
- **Reference reassigned:** Pointing a reference variable to a different object leaves the original object without that particular reference.
- **Anonymous object:** An object created with `new` but never assigned to any reference variable at all (e.g., `new Student().displayInfo();`) is eligible immediately after that statement finishes.
- **Local object after method ends:** An object referenced only by a local variable inside a method becomes unreachable the moment that method returns, since the local variable's scope (and its stack frame) ends.
- **Isolated objects:** A group of objects that reference each other but have no reference from any reachable part of the program (see "Island of Isolation" in Section 8).

**Examples:**

```java
Student s = new Student();   // object created, reachable via 's'
s = null;                     // object now unreachable — eligible for GC

Student s1 = new Student();
Student s2 = new Student();
s1 = s2;                      // s1's original object is now unreachable (reference reassigned)

new Student();                // anonymous object — eligible immediately, no reference ever held it
```

---

## 8. Ways Objects Become Unreachable

**Case 1 — Reference = null:**

```java
Student s = new Student();
s = null;   // object is now unreachable
```

```
Before:   s ───▶ [Student Object]
After:    s ───▶ null        [Student Object] (now unreachable, GC-eligible)
```

**Case 2 — Reference reassigned:**

```java
Student s = new Student();      // Object A
s = new Student();               // s now points to Object B; Object A is unreachable
```

```
Before:   s ───▶ [Object A]
After:    s ───▶ [Object B]      [Object A] (now unreachable, GC-eligible)
```

**Case 3 — Anonymous Objects:**

```java
new Student();   // created, used (if at all) in this single statement, then immediately unreachable
```

```
No reference ever created  →  [Student Object] (unreachable from the moment of creation)
```

**Case 4 — Objects created inside methods:**

```java
static void createStudent() {
    Student s = new Student();   // local reference
}   // method ends — 's' goes out of scope, object becomes unreachable
```

```
During method:  s (local, on stack) ───▶ [Student Object]
After method returns: stack frame destroyed → no reference remains → object unreachable
```

**Case 5 — Island of Isolation:**

Two or more objects can reference **each other**, but if nothing from the reachable part of the program (stack, static fields) points to _any_ of them, the entire group is unreachable — even though they still reference each other internally.

```java
class Node {
    Node partner;
}

Node a = new Node();
Node b = new Node();
a.partner = b;   // a references b
b.partner = a;   // b references a

a = null;
b = null;   // both external references removed — a and b still reference EACH OTHER,
            // but neither is reachable from any live part of the program anymore
```

```
Before nulling:   a ───▶ [Node A] ⇄ [Node B] ◀─── b

After nulling:    a ───▶ null        b ───▶ null

                  [Node A] ⇄ [Node B]   (still reference each other, but BOTH unreachable — GC-eligible)
```

Java's garbage collector is specifically designed to detect and reclaim such "islands of isolation" — this is one of the key advantages over simpler reference-counting garbage collection schemes, which can fail to detect these mutually-referencing but externally-unreachable groups.

---

## 9. How Garbage Collector Works

_(Simple explanation)_

1. **Detects unreachable objects:** The JVM's garbage collector periodically scans the heap, starting from a set of known **root references** (local variables on the stack, static fields, active thread references), and traces every object reachable from those roots.
2. **Marks objects:** Any object that _is_ reached during this trace is marked as "live" (still in use); any object that is **not** reached is considered unreachable garbage.
3. **Reclaims heap memory:** The memory occupied by all unmarked (unreachable) objects is freed and made available for future object allocations, and in many collector implementations, the remaining live objects may also be compacted (moved together) to reduce memory fragmentation.

This general strategy is commonly known as **"mark and sweep"** (with many modern JVM garbage collectors using more advanced variations, such as generational collection, for improved performance).

---

## 10. Can We Force Garbage Collection?

**`System.gc()`** and **`Runtime.getRuntime().gc()`** are methods that a program can call to **suggest** that the JVM perform garbage collection.

```java
System.gc();                        // requests garbage collection
Runtime.getRuntime().gc();          // equivalent alternative call
```

**Why it is only a request:** Neither of these methods **guarantees** that garbage collection will actually run immediately, or at all, when called. The JVM is free to ignore the request entirely if it determines that garbage collection isn't currently necessary or beneficial. Java deliberately does not expose direct, guaranteed control over garbage collection timing to the programmer — the JVM's internal garbage collector algorithms decide when collection actually happens, based on heap usage, memory pressure, and other non-external(internal) factors.

---

## 11. `finalize()` Method

**What was `finalize()`?** `finalize()` was a special method that could be overridden in a class, which the garbage collector would call on an object **just before** reclaiming its memory — intended to give objects a chance to perform cleanup actions (like closing a file or releasing a resource) before being destroyed.

```java
@Override
protected void finalize() throws Throwable {
    // cleanup code (legacy pattern — no longer recommended)
}
```

**Why was it used?** It was originally intended as a safety net for releasing non-memory resources (file handles, network connections, etc.) that an object might be holding, in case the programmer forgot to release them explicitly.

**Why it is deprecated:** `finalize()` has several serious problems: its execution timing is completely unpredictable (since it depends on when garbage collection happens, which itself isn't guaranteed), it can significantly slow down garbage collection, it can resurrect objects in ways that cause unpredictable behavior, and exceptions thrown inside it are silently ignored. Due to these issues, `finalize()` was formally deprecated starting in Java 9, and is planned for eventual removal.

**Modern recommendation:** Use **try-with-resources** along with the `AutoCloseable`/`Closeable` interface for deterministic, explicit resource cleanup, rather than relying on `finalize()`.

```java
try (FileReader reader = new FileReader("data.txt")) {
    // use reader
}   // reader.close() is called automatically and deterministically, regardless of exceptions
```

---

## 12. Memory Leaks in Java

**Can Java have memory leaks?** Yes — despite having automatic garbage collection, Java programs **can** still suffer from memory leaks. This happens whenever an object is still **reachable** (so the GC won't touch it) but is no longer actually needed by the program — the object is technically alive, but effectively "leaked" since it will never be used again.

**Holding unnecessary references:** Keeping a reference to an object around long after it's actually needed prevents the garbage collector from reclaiming it, even though the program has no further use for it.

**Collections:** A very common source of Java memory leaks — adding objects to a `List`, `Map`, or other collection and then forgetting to remove them once they're no longer needed keeps them permanently reachable (and therefore alive) for as long as the collection itself exists.

```java
List<Student> cache = new ArrayList<>();

void processStudent(Student s) {
    cache.add(s);   // if never removed, s remains reachable forever, even after processing is done
}
```

**Static references:** Since static fields belong to the class itself and exist for the entire lifetime of the program (as long as the class is loaded), any object referenced by a static field remains reachable — and therefore never garbage collected — for the program's entire run, unless explicitly cleared.

```java
class Cache {
    static List<Object> data = new ArrayList<>();  // grows forever if items are never removed
}
```

**Examples of leak-prone patterns:**

```java
// Listener/callback registration that's never unregistered
someComponent.addListener(myListener);
// If someComponent lives much longer than expected, myListener (and whatever it references) leaks too.
```

---

## 13. Best Practices

- **Remove unnecessary references** — explicitly set references to `null` (or remove them from collections) once an object is genuinely no longer needed, especially for long-lived objects like caches.
- **Close resources** — always use try-with-resources (or explicit `close()` calls in a `finally` block) for file streams, database connections, and other resources, rather than relying on garbage collection to clean them up.
- **Avoid excessive object creation** — creating large numbers of short-lived objects unnecessarily increases the frequency and overhead of garbage collection cycles.
- **Use local variables wisely** — prefer local variables (which naturally become unreachable when a method ends) over long-lived static or instance fields, unless persistence is genuinely required.

---

## 14. Common Misconceptions

- **"Garbage Collector runs immediately"** ❌ — Becoming eligible for garbage collection does not mean an object is destroyed right away; the JVM decides when (and whether, within the program's lifetime) to actually run collection.
- **"`System.gc()` guarantees collection"** ❌ — It is only a _request/suggestion_ to the JVM; the JVM is free to ignore it entirely.
- **"Java has no memory leaks"** ❌ — While garbage collection prevents the _classic_ manual-memory-management leaks, Java programs can still leak memory through unnecessary reachable references (e.g., forgotten collection entries, static fields).
- **"A `null`-ed object means it is destroyed immediately"** ❌ — Setting a reference to `null` only makes the object _eligible_ for garbage collection; the actual memory reclamation happens later, at a time determined by the JVM.

---

## 15. Interview Corner

**Q1. What is garbage collection in Java?**
It's the JVM's automatic process of identifying objects on the heap that are no longer reachable by the program and reclaiming the memory they occupy, without requiring explicit deallocation from the programmer.

**Q2. Can you force garbage collection to run immediately?**
No — calling `System.gc()` only _suggests_ to the JVM that garbage collection should run; it does not guarantee immediate (or even eventual) collection.

**Q3. What makes an object eligible for garbage collection?**
An object becomes eligible when it is no longer reachable from any live reference chain — such as when its reference is set to `null`, reassigned, goes out of scope, or is part of an isolated group of mutually-referencing but externally-unreachable objects.

**Q4. What is an "island of isolation"?**
A group of two or more objects that reference each other internally but have no reachable reference from any active part of the program — Java's garbage collector is capable of detecting and reclaiming such groups, unlike simple reference-counting schemes.

**Q5. Why was `finalize()` deprecated?**
Because its execution timing is unpredictable, it can degrade garbage collection performance, exceptions inside it are silently swallowed, and it can lead to unpredictable object resurrection — try-with-resources and `AutoCloseable` are the modern, deterministic replacement.

**Q6. Can a Java program still have memory leaks despite garbage collection?**
Yes — if an object remains reachable (e.g., through a forgotten reference in a collection or a static field) even though it's no longer actually needed, garbage collection cannot reclaim it, resulting in a memory leak.

**Q7. What is the difference between an object being "unreachable" and being "garbage collected"?**
"Unreachable" means the object is eligible for collection because nothing live references it anymore; "garbage collected" means the JVM has actually reclaimed its memory — these two events are not simultaneous, and there can be an unpredictable delay between them.

---

## 16. MCQs

**1. What is the primary purpose of garbage collection?**
a) To create new objects
b) To automatically reclaim memory of unreachable objects
c) To speed up compilation
d) To manage stack memory only
**Answer: b) To automatically reclaim memory of unreachable objects**

**2. Which of these makes an object eligible for garbage collection?**
a) Assigning it to a new variable
b) Setting its only reference to null
c) Declaring it as static
d) Adding it to a List
**Answer: b) Setting its only reference to null**

**3. Does `System.gc()` guarantee immediate garbage collection?**
a) Yes, always
b) No, it is only a request to the JVM
c) Only in single-threaded programs
d) Only for static objects
**Answer: b) No, it is only a request to the JVM**

**4. What is an "island of isolation"?**
a) A single unreferenced object
b) A group of objects referencing each other but unreachable from the program
c) A static field holding a large object
d) An object stored in the stack
**Answer: b) A group of objects referencing each other but unreachable from the program**

**5. Why is `finalize()` deprecated?**
a) It runs too quickly
b) Its execution timing is unpredictable and it can degrade performance
c) It cannot be overridden
d) It is only used for primitives
**Answer: b) Its execution timing is unpredictable and it can degrade performance**

**6. Can Java programs have memory leaks despite garbage collection?**
a) No, never
b) Yes, if objects remain reachable but are no longer needed
c) Only in multi-threaded programs
d) Only when using arrays
**Answer: b) Yes, if objects remain reachable but are no longer needed**

**7. What is the modern recommended alternative to `finalize()` for resource cleanup?**
a) System.gc()
b) try-with-resources / AutoCloseable
c) Runtime.exit()
d) Static initializer blocks
**Answer: b) try-with-resources / AutoCloseable**

**8. Where do all Java objects reside in memory?**
a) Stack
b) Heap
c) Method area only
d) CPU registers
**Answer: b) Heap**

**9. What algorithm broadly describes how most garbage collectors identify unreachable objects?**
a) Quick sort
b) Mark and sweep
c) Binary search
d) Depth-first traversal only, without marking
**Answer: b) Mark and sweep**

**10. What happens to an object referenced only by a local variable once its method returns?**
a) It remains reachable forever
b) It becomes unreachable and eligible for garbage collection
c) It is immediately destroyed before the method even returns
d) It is moved to the stack permanently
**Answer: b) It becomes unreachable and eligible for garbage collection**

---

## 17. University Questions

1. What is garbage collection? Explain why it is needed in Java with a real-world analogy.
2. Explain the object life cycle in Java, from creation to memory reclamation, with a diagram.
3. Describe the different scenarios in which an object becomes eligible for garbage collection, with examples for each.
4. What is an "island of isolation"? Explain with a program example and memory diagram.
5. Explain the working of the JVM's garbage collector using the mark-and-sweep approach.
6. Discuss `System.gc()` and explain why calling it does not guarantee immediate garbage collection.
7. What is the `finalize()` method? Explain why it has been deprecated and what the modern alternative is.
8. Can a Java program have memory leaks even with automatic garbage collection? Explain with examples involving collections and static references.

---

## 18. Viva Questions

1. What is the difference between an object being unreachable and being garbage collected?
2. Does setting a reference to `null` immediately free the object's memory?
3. What are root references in the context of garbage collection?
4. Why can't reference counting alone handle islands of isolation?
5. What is the risk of holding unnecessary references in a long-lived collection?
6. Why is calling `System.gc()` discouraged in most production code?
7. What replaced `finalize()` for reliable resource cleanup?
8. Can a static field cause a memory leak? Explain how.
9. What is the difference between stack and heap memory in relation to garbage collection?
10. Does garbage collection guarantee a program will never run out of memory?

---

## 19. Programming Exercises

**Easy**

1. Write a program that creates an object, sets its reference to `null`, and explain (in comments) at what point it becomes eligible for garbage collection.
2. Write a program demonstrating an anonymous object that is eligible for garbage collection immediately after use.

**Medium**

3. Write a program that demonstrates reference reassignment making a previously created object unreachable.
4. Write a program showing an object created inside a method becoming unreachable once the method returns.

**Hard**

5. Write a program simulating an "island of isolation" using two custom classes that reference each other, then null out all external references and explain why both objects are still eligible for garbage collection.

---

## 20. Challenge Problems

**Leak Simulator:** Write a program that continuously adds objects to a static `List` without ever removing them, and explain (in comments) why this constitutes a memory leak despite Java's garbage collector being active.

**Resource Cleanup Comparison:** Write two versions of a file-reading program — one that (incorrectly) relies on `finalize()` for cleanup, and one that correctly uses try-with-resources — and explain the practical differences in reliability.

**Cache with Cleanup:** Design a simple caching class that stores objects in a `Map`, and implement a method to explicitly remove entries, demonstrating how proper reference removal helps avoid memory leaks in long-running programs.

**Reachability Tracer (conceptual):** Write a short program with a chain of objects referencing one another, then explain step by step, using comments, exactly which objects remain reachable and which become eligible for garbage collection after specific reference changes.

---

## 21. Summary

- **Garbage Collection** is the JVM's automatic process for reclaiming heap memory occupied by objects that are no longer reachable by the running program.
- Objects become **eligible** for garbage collection when no live reference chain leads to them — through nulled references, reassignment, going out of scope, or forming an unreachable "island of isolation."
- The garbage collector generally works using a **mark-and-sweep** approach: tracing reachable objects from root references, then reclaiming memory from everything left unmarked.
- **`System.gc()`** only _requests_ garbage collection — it never guarantees when, or even if, collection will actually occur.
- **`finalize()`** has been deprecated in favor of **try-with-resources** and `AutoCloseable` for reliable, deterministic resource cleanup.
- Java programs **can** still experience memory leaks — not through classic "forgotten deallocation," but through objects that remain unnecessarily reachable via collections, static fields, or forgotten references.

---

## 22. Key Takeaways

- GC automatically reclaims memory from unreachable objects — no manual `free()` needed.
- Eligibility ≠ immediate collection — there's no guaranteed timing.
- `System.gc()` is a request, not a command.
- `finalize()` is deprecated — use try-with-resources instead.
- Java can still leak memory through unnecessarily-held reachable references.
- Islands of isolation (mutually-referencing but externally-unreachable objects) ARE correctly detected and collected by Java's GC.

---

## 23. Cheat Sheet

```
OBJECT LIFECYCLE
Creation → Usage → Unreachable → Eligible for GC → Memory Reclaimed

WAYS AN OBJECT BECOMES UNREACHABLE
1. reference = null;
2. reference reassigned to a different object
3. anonymous object (never assigned to a reference)
4. local object, method has returned
5. island of isolation (mutually-referencing, externally unreachable)

REQUESTING (NOT FORCING) GC
System.gc();
Runtime.getRuntime().gc();
→ Both are merely SUGGESTIONS to the JVM — never guaranteed

FINALIZE (DEPRECATED)
protected void finalize() { }   // DO NOT USE — unpredictable, deprecated since Java 9

MODERN RESOURCE CLEANUP
try (Resource r = new Resource()) {
    // use r
}   // r.close() guaranteed, deterministic

MEMORY LEAK RISK AREAS
✔ Unremoved entries in long-lived collections
✔ Static fields holding references indefinitely
✔ Forgotten listener/callback registrations

COMMON MISCONCEPTIONS TO AVOID
✘ GC runs immediately when eligible          → FALSE
✘ System.gc() guarantees collection           → FALSE
✘ Java is 100% immune to memory leaks         → FALSE
✘ null-ing a reference destroys the object instantly → FALSE
```
