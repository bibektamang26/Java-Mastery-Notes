# Introduction to Control Structures

## Introduction

By default, a Java program executes its statements **one after another, in the exact order they are written** — from top to bottom. This default behavior works fine for simple, linear tasks, but real-world programs rarely follow such a straightforward path. Programs need to make decisions ("if the user is eligible, allow access"), repeat actions ("keep asking until the input is valid"), and sometimes jump out of a normal sequence entirely ("stop the loop as soon as the item is found").

**Control structures** are the language features that let a program deviate from simple top-to-bottom execution — allowing it to make decisions, repeat blocks of code, and control exactly which statements run and in what order. They are what transform a static list of instructions into a genuinely intelligent, responsive program.

This chapter introduces the concept of control structures as a foundation for the more detailed chapters ahead on decision-making statements (`if`, `switch`), loops (`for`, `while`, `do-while`), and branching statements (`break`, `continue`, `return`).

---

## What is a Control Structure?

**Definition:** A control structure is a block of programming logic that determines the **order** in which individual statements, instructions, or function calls are executed within a program.

In other words, a control structure controls the **flow of execution** — deciding whether a block of code runs at all, how many times it runs, or which of several possible blocks runs, based on conditions evaluated while the program is running.

**Example — without any control structure (pure sequential code):**

```java
System.out.println("Step 1");
System.out.println("Step 2");
System.out.println("Step 3");
```

Every line here executes exactly once, in exactly this order — there is no decision-making or repetition involved.

**Example — with a control structure:**

```java
int marks = 75;

if (marks >= 40) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}
```

Here, the `if-else` control structure decides **which one** of the two `System.out.println()` statements actually executes, based on the value of `marks` at runtime.

---

## Why Control Structures are Needed

Without control structures, a program would be limited to executing a fixed sequence of instructions with no ability to adapt to different inputs, conditions, or situations. Control structures are essential because they allow a program to:

1. **Make decisions:** Choose between different courses of action based on a condition — for example, checking if a user's password is correct before granting access.
2. **Repeat actions:** Perform a task multiple times without duplicating code — for example, printing a receipt line for every item in a shopping cart, regardless of how many items there are.
3. **Respond to dynamic input:** Handle unpredictable, runtime-determined data (like user input or sensor readings) rather than only fixed, hardcoded values.
4. **Avoid code duplication:** Instead of writing the same block of code many times for repeated actions, a loop lets that block run as many times as needed with a single piece of code.
5. **Model real-world logic:** Nearly all real-world processes involve conditions and repetition (e.g., "keep withdrawing money from an ATM until the balance is zero, or the user cancels") — control structures let programs faithfully represent this kind of logic.
6. **Control complexity:** Branching statements like `break`, `continue`, and `return` give fine-grained control over exactly when to exit a loop or function early, keeping programs efficient and avoiding unnecessary work.

---

## Types of Control Structures

Java's control structures are broadly grouped into four categories, based on how they affect the flow of execution:

```
Control Structures
│
├── Sequential
├── Decision Making
├── Repetition (Looping)
└── Branching
```

### Sequential

**Definition:** Sequential control is the _default_ flow of execution — statements run one after another, strictly in the order they are written, with no conditions or repetition involved.

**Characteristics:**

- Each statement executes exactly once.
- Execution proceeds from top to bottom.
- No decisions or repetitions occur.

**Example:**

```java
int a = 5;
int b = 10;
int sum = a + b;
System.out.println("Sum: " + sum);
```

Every line here runs once, in the exact order written — this is sequential execution, and it forms the "default" baseline that the other three control structures modify or override.

### Decision Making

**Definition:** Decision-making (also called **selection**) control structures allow a program to choose between two or more possible paths of execution, based on whether a condition evaluates to `true` or `false`.

**Common statements:** `if`, `if-else`, `if-else-if` ladder, `switch`.

**Example:**

```java
int age = 20;

if (age >= 18) {
    System.out.println("Eligible to vote");
} else {
    System.out.println("Not eligible to vote");
}
```

Here, only **one** of the two branches executes, depending on the value of `age` — the program has effectively "chosen" its path at runtime.

### Repetition (Looping)

**Definition:** Repetition (or **looping**) control structures allow a block of code to be executed repeatedly, either a fixed number of times or until a certain condition is no longer true.

**Common statements:** `for`, `while`, `do-while`.

**Example:**

```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
```

This block runs the same `System.out.println()` statement five times without the programmer having to physically write it five separate times — the loop handles the repetition automatically.

### Branching

**Definition:** Branching control structures allow a program to **jump** out of, or skip parts of, the normal flow of execution — either exiting a loop early, skipping the current iteration, or exiting a method altogether.

**Common statements:** `break`, `continue`, `return`.

**Example:**

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;   // exits the loop entirely once i reaches 5
    }
    System.out.println(i);
}
```

Here, `break` causes the loop to terminate early, once a specific condition (`i == 5`) is met, rather than running through all ten iterations.

---

## Flowcharts

A **flowchart** is a diagram that visually represents the step-by-step flow of logic in a program, using standardized shapes to represent different kinds of operations.

**Common flowchart symbols:**

```
   ┌─────────┐
   │  Start  │        →  Oval: Start / End
   └─────────┘

   ┌─────────┐
   │ Process │        →  Rectangle: A processing step / statement
   └─────────┘

    ◇───────◇
    │Decision│         →  Diamond: A decision point (yes/no, true/false)
    ◇───────◇

   ┌─────────┐
   │  Input/ │        →  Parallelogram: Input or Output operation
   │ Output  │
   └─────────┘

      │
      ▼                →  Arrow: Direction of flow
```

**Example flowchart — checking if a number is even or odd:**

```
        ┌───────┐
        │ Start │
        └───┬───┘
            ▼
   ┌───────────────────┐
   │ Input number (n)  │
   └───────┬────────────┘
           ▼
      ◇───────────◇
      │ n % 2 == 0 │
      ◇─────┬──────◇
      Yes │      │ No
          ▼      ▼
   ┌──────────┐ ┌─────────┐
   │  "Even"  │ │  "Odd"  │
   └────┬─────┘ └────┬────┘
        └──────┬──────┘
               ▼
           ┌───────┐
           │  End  │
           └───────┘
```

Flowcharts are especially useful for visualizing decision-making and looping logic before writing actual code, since they make the flow of control immediately visible.

---

## Control Flow Diagram

A **control flow diagram** represents how execution moves between different control structures within a program as a whole — showing the overall path a program takes through sequential blocks, decisions, and loops.

**Example — control flow for a simple grading program:**

```
Sequential Block
      │
      ▼
Decision (marks >= 40 ?)
      │
   ┌──┴──┐
  Yes    No
   │      │
   ▼      ▼
"Pass"  "Fail"
   │      │
   └──┬───┘
      ▼
Sequential Block (print final message)
      │
      ▼
    End
```

**Example — control flow involving a loop with branching:**

```
Start
  │
  ▼
Initialize loop counter
  │
  ▼
┌─────────────────────┐
│ Loop condition true?│──No──▶ Exit loop ──▶ End
└──────────┬───────────┘
          Yes
           │
           ▼
     Execute loop body
           │
           ▼
   Branching condition met?
      │           │
     Yes          No
      │           │
   break/continue │
      │           │
      └─────┬─────┘
            ▼
    Update loop counter
            │
            └────────────► (back to loop condition)
```

This kind of diagram highlights how sequential execution, decision-making, and looping/branching structures don't operate in isolation — they combine and nest within each other to form a program's complete logic.

---

## Real-life Examples

Control structures mirror everyday decision-making and repetitive processes:

- **Sequential:** Following a recipe step by step — boil water, add pasta, wait, drain — each step happens once, in order.
- **Decision Making:** Deciding whether to carry an umbrella — "if it's raining, take an umbrella; otherwise, don't."
- **Repetition (Looping):** Doing laundry — "keep washing clothes until the laundry basket is empty," repeating the same action for each load.
- **Branching:** A fire alarm — "if smoke is detected, stop whatever you're doing immediately and evacuate," interrupting the normal flow of activity entirely.
- **Combined example — an ATM withdrawal:** Insert card (sequential) → check PIN validity (decision) → keep prompting for correct PIN up to 3 times (loop) → cancel transaction immediately if the card is invalid (branching/early exit).

---

## Summary

- **Control structures** determine the order in which a program's statements execute, allowing programs to move beyond simple, fixed, top-to-bottom execution.
- Without control structures, programs would only be able to run a fixed sequence of statements, unable to adapt to different conditions or repeat tasks efficiently.
- Java's control structures fall into four categories: **Sequential** (default top-to-bottom flow), **Decision Making** (`if`, `switch`), **Repetition/Looping** (`for`, `while`, `do-while`), and **Branching** (`break`, `continue`, `return`).
- **Flowcharts** visually represent a program's logic using standardized symbols (oval for start/end, rectangle for process, diamond for decision, parallelogram for input/output).
- **Control flow diagrams** show how execution moves through a program as a whole, illustrating how sequential blocks, decisions, and loops combine and nest together.
- Nearly all real-world processes — from following a recipe to withdrawing cash from an ATM — naturally map onto these same four categories of control structures.

---

## Interview Questions

**Q1. What is a control structure in programming?**
A control structure is a programming construct that determines the order in which statements execute, allowing a program to make decisions, repeat actions, or jump between different parts of its logic rather than always running strictly top to bottom.

**Q2. What are the four main types of control structures?**
Sequential, Decision Making (Selection), Repetition (Looping), and Branching.

**Q3. What is the default flow of execution in a program?**
Sequential execution — statements run one after another, in the exact order they appear in the code, unless a control structure changes that flow.

**Q4. Why are control structures necessary in programming?**
They allow programs to adapt to different conditions and inputs, avoid duplicating code for repeated actions, and model real-world logic that inherently involves decisions and repetition.

**Q5. What is the difference between decision-making and branching control structures?**
Decision-making structures (`if`, `switch`) choose _which_ block of code to execute based on a condition; branching structures (`break`, `continue`, `return`) alter the normal flow by jumping out of or skipping parts of a loop or method entirely.

**Q6. What is the purpose of a flowchart?**
A flowchart visually represents a program's logic and flow of control using standardized symbols, making it easier to plan, communicate, and understand a program's structure before or after writing the actual code.

**Q7. Can control structures be nested within each other?**
Yes — for example, a decision-making structure can exist inside a loop, and a loop can exist inside another loop; combining and nesting control structures is how complex program logic is built from these simple building blocks.

**Q8. Give a real-life analogy for a looping control structure.**
Doing laundry until the basket is empty — the same washing action repeats for each load, continuing only as long as the "basket not empty" condition remains true.

---

## Exam Questions

1. Define control structure and explain why it is needed in programming, with a suitable example.
2. Explain the four types of control structures in Java, giving one example of each.
3. Differentiate between sequential execution and decision-making control structures.
4. Draw and explain a flowchart for a program that checks whether a given number is even or odd.
5. Explain the difference between looping and branching control structures with examples.
6. What is a control flow diagram? Explain its importance with a suitable example involving a loop and a decision.
7. Give three real-life examples that map to sequential, decision-making, and looping control structures respectively.
8. Explain, with a diagram, how sequential, decision-making, and looping control structures can combine within a single program.
