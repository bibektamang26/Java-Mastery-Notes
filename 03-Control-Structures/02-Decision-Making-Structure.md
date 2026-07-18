# 02. Decision Making Structures

## Introduction

Decision-making structures allow a Java program to choose between different courses of action based on whether a condition is `true` or `false`. Instead of always executing the same fixed sequence of statements, a program can now respond intelligently to different situations — checking a user's age before allowing access, validating login credentials, calculating a grade based on marks, or picking one option out of many possibilities.

Java provides several decision-making (selection) statements, each suited to different kinds of situations:

- **`if`** — executes a block only when a condition is true.
- **`if-else`** — chooses between two alternative blocks.
- **Nested `if`** — an `if` statement placed inside another `if` for multi-level conditions.
- **`else-if` ladder** — chooses among multiple alternatives in sequence.
- **`switch`** — selects one of many blocks based on the value of a single variable/expression.

This chapter covers all of these in depth, along with flowcharts, comparisons, common mistakes, and practice material.

---

## `if` Statement

**Definition:** The `if` statement executes a block of code **only if** its condition evaluates to `true`. If the condition is `false`, the block is simply skipped, and execution continues with the statement immediately after the `if` block.

**Syntax:**

```java
if (condition) {
    // executes only if condition is true
}
```

**Example:**

```java
int age = 20;

if (age >= 18) {
    System.out.println("You are eligible to vote.");
}
System.out.println("Program continues...");
```

**Output:**

```
You are eligible to vote.
Program continues...
```

If `age` were `15` instead, the first `println` would simply be skipped, and only "Program continues..." would print.

**Flowchart:**

```
        ┌───────┐
        │ Start │
        └───┬───┘
            ▼
      ◇───────────◇
      │ condition? │
      ◇─────┬──────◇
      True  │  False
         ▼         │
   ┌──────────┐    │
   │ Execute  │    │
   │  block   │    │
   └────┬─────┘    │
        └─────┬─────┘
              ▼
            ┌───────┐
            │  End  │
            └───────┘
```

---

## if-else

**Definition:** The `if-else` statement extends the `if` statement by providing an alternative block of code that executes when the condition is `false`. Exactly one of the two blocks always runs.

**Syntax:**

```java
if (condition) {
    // executes if condition is true
} else {
    // executes if condition is false
}
```

**Example:**

```java
int marks = 35;

if (marks >= 40) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}
```

**Output:**

```
Fail
```

**Flowchart:**

```
        ┌───────┐
        │ Start │
        └───┬───┘
            ▼
      ◇───────────◇
      │ condition? │
      ◇─────┬──────◇
      True  │  False
        ▼         ▼
  ┌─────────┐ ┌─────────┐
  │  if     │ │  else   │
  │  block  │ │  block  │
  └────┬────┘ └────┬────┘
       └─────┬──────┘
             ▼
           ┌───────┐
           │  End  │
           └───────┘
```

---

## Nested if

**Definition:** A nested `if` is an `if` (or `if-else`) statement placed inside the body of another `if` (or `else`) statement. It is used when a decision depends on **more than one condition**, evaluated in stages, where the second condition is only relevant if the first is already true.

**Syntax:**

```java
if (condition1) {
    if (condition2) {
        // executes only if BOTH condition1 and condition2 are true
    }
}
```

**Example:**

```java
int age = 20;
boolean hasID = true;

if (age >= 18) {
    if (hasID) {
        System.out.println("Entry allowed.");
    } else {
        System.out.println("ID required for entry.");
    }
} else {
    System.out.println("Must be 18 or older.");
}
```

**Output:**

```
Entry allowed.
```

**Note:** Nested `if` is functionally similar to combining conditions with the `&&` operator (`if (age >= 18 && hasID)`), but nesting is useful when different actions need to happen at each stage, or when the inner condition genuinely only makes sense to check after the outer one passes.

---

## else-if Ladder

**Definition:** The `else-if` ladder (also called an `if-else-if` chain) allows a program to check a **series of conditions in sequence**, executing the block associated with the **first** condition that evaluates to `true`, and skipping all the rest. An optional final `else` catches the case where none of the conditions match.

**Syntax:**

```java
if (condition1) {
    // executes if condition1 is true
} else if (condition2) {
    // executes if condition1 is false AND condition2 is true
} else if (condition3) {
    // executes if condition1 and condition2 are false AND condition3 is true
} else {
    // executes if none of the above conditions are true
}
```

**Example — Grade Calculator:**

```java
int marks = 82;

if (marks >= 90) {
    System.out.println("Grade: A+");
} else if (marks >= 80) {
    System.out.println("Grade: A");
} else if (marks >= 70) {
    System.out.println("Grade: B");
} else if (marks >= 40) {
    System.out.println("Grade: C");
} else {
    System.out.println("Grade: F");
}
```

**Output:**

```
Grade: A
```

**Important note:** Once a matching condition is found, **all remaining conditions are skipped entirely** — even if they would also evaluate to `true`. This is why the conditions in a grading ladder must be arranged from most specific/highest to least specific/lowest (or vice versa, depending on the logic), or the wrong branch may execute.

---

## switch Statement

**Definition:** The `switch` statement evaluates a single expression once and compares its value against a list of possible constant `case` values, executing the block that matches. It's typically used as a cleaner alternative to a long `else-if` ladder when checking a single variable against many discrete, known values.

**Syntax:**

```java
switch (expression) {
    case value1:
        // code
        break;
    case value2:
        // code
        break;
    default:
        // code executed if no case matches
}
```

**Example:**

```java
int day = 3;
String dayName;

switch (day) {
    case 1:
        dayName = "Monday";
        break;
    case 2:
        dayName = "Tuesday";
        break;
    case 3:
        dayName = "Wednesday";
        break;
    case 4:
        dayName = "Thursday";
        break;
    case 5:
        dayName = "Friday";
        break;
    default:
        dayName = "Weekend";
}

System.out.println(dayName);
```

**Output:**

```
Wednesday
```

**Key rules of `switch`:**

- The `switch` expression can be of type `byte`, `short`, `char`, `int`, their wrapper classes, `String` (since Java 7), or an `enum`.
- Each `case` value must be a **compile-time constant** and must be unique.
- **`break`** is required at the end of each case to prevent **fall-through** (execution continuing into the next case).
- **`default`** is optional, and executes when no `case` matches; it can be placed anywhere in the `switch` block, though it's conventionally placed last.

**Fall-through example (without `break`):**

```java
int day = 2;

switch (day) {
    case 1:
        System.out.println("Monday");
    case 2:
        System.out.println("Tuesday");
    case 3:
        System.out.println("Wednesday");
        break;
    default:
        System.out.println("Other day");
}
```

**Output:**

```
Tuesday
Wednesday
```

Because there's no `break` after `case 2`, execution "falls through" into `case 3` as well, printing both — this is a very common source of bugs.

**Flowchart:**

```
        ┌───────┐
        │ Start │
        └───┬───┘
            ▼
   ┌────────────────────┐
   │ Evaluate expression │
   └──────────┬───────────┘
              ▼
      ◇─────────────◇
      │ Matches case?│──No──▶ default block
      ◇──────┬────────◇
            Yes
              ▼
       Matching case block
              ▼
             break
              ▼
            ┌───────┐
            │  End  │
            └───────┘
```

---

## switch vs if-else

| Aspect                                      | `switch`                                                            | `if-else` (ladder)                                          |
| ------------------------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------- |
| Best suited for                             | Checking one variable against many discrete constant values         | Checking complex or range-based conditions                  |
| Condition type                              | Equality comparison only (`==` against constants)                   | Any boolean expression (ranges, logical combinations, etc.) |
| Readability                                 | Cleaner and more readable for many fixed values                     | Can become long and harder to read with many conditions     |
| Data types supported                        | `byte`, `short`, `char`, `int`, wrapper types, `String`, `enum`     | Any type that can form a boolean expression                 |
| Fall-through behavior                       | Possible if `break` is omitted                                      | Not applicable — each block is independent                  |
| Performance                                 | Can be more efficient for many cases (compiler may use jump tables) | Evaluated sequentially, condition by condition              |
| Range checks (e.g., `marks >= 40`)          | Not directly supported                                              | Naturally supported                                         |
| Multiple conditions combined (`&&`, `\|\|`) | Not supported directly                                              | Fully supported                                             |
| Example use case                            | Menu selection, day-of-week lookup                                  | Grade calculation based on mark ranges                      |

---

## Modern switch expression (optional)

Since Java 14, `switch` can also be used as an **expression** that directly produces a value, using the arrow (`->`) syntax. This modern form removes the need for `break` statements entirely and eliminates the fall-through problem.

**Syntax:**

```java
resultType result = switch (expression) {
    case value1 -> resultExpression1;
    case value2 -> resultExpression2;
    default -> defaultResultExpression;
};
```

**Example:**

```java
int day = 3;

String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    case 4 -> "Thursday";
    case 5 -> "Friday";
    default -> "Weekend";
};

System.out.println(dayName);  // Wednesday
```

**Multiple values per case:**

```java
int day = 6;

String type = switch (day) {
    case 1, 2, 3, 4, 5 -> "Weekday";
    case 6, 7 -> "Weekend";
    default -> "Invalid day";
};

System.out.println(type);  // Weekend
```

**Advantages over traditional `switch`:**

- No `break` needed — each case doesn't fall through into the next.
- Can be used directly as an expression, assigned straight to a variable.
- Supports combining multiple values in a single `case` label using commas.
- Generally more concise and less error-prone than the traditional statement form.

---

## Flowcharts

**Combined decision-making flow (else-if ladder style):**

<figure style="text-align: center;">
  <img
    src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRxOH0mfQyAMvdm3fgTHh6wE03jiVIZlkLybnsTcE5R9BgjNBUh_JN8dQc&s=10"
    alt="Switch vs If-Else Comparison"
    style="max-width: 100%; height: auto; border-radius: 8px;"
  />
  <figcaption><em>If else Ladder</em></figcaption>
</figure>

**`switch` flow:**

<figure style="text-align: center;">
  <img
    src="https://media.geeksforgeeks.org/wp-content/uploads/20230224161406/switch-case-in-c.png"
    alt="Switch vs If-Else Comparison"
    style="max-width: 100%; height: auto; border-radius: 8px;"
  />
  <figcaption><em>If else Ladder</em></figcaption>
</figure>

---

## Examples

**1. Simple `if` — Discount eligibility:**

```java
double billAmount = 1200;

if (billAmount > 1000) {
    System.out.println("You get a 10% discount!");
}
```

**2. `if-else` — Odd or Even:**

```java
int num = 7;

if (num % 2 == 0) {
    System.out.println("Even");
} else {
    System.out.println("Odd");
}
```

**3. Nested `if` — Loan Eligibility:**

```java
int age = 25;
double income = 45000;

if (age >= 21) {
    if (income >= 30000) {
        System.out.println("Loan Approved");
    } else {
        System.out.println("Insufficient Income");
    }
} else {
    System.out.println("Not Eligible: Age Requirement Not Met");
}
```

**4. `else-if` Ladder — Traffic Light Actions:**

```java
String signal = "Yellow";

if (signal.equals("Red")) {
    System.out.println("Stop");
} else if (signal.equals("Yellow")) {
    System.out.println("Get Ready");
} else if (signal.equals("Green")) {
    System.out.println("Go");
} else {
    System.out.println("Invalid Signal");
}
```

**5. `switch` — Calculator using operator selection:**

```java
double a = 10, b = 5;
char operator = '*';
double result;

switch (operator) {
    case '+':
        result = a + b;
        break;
    case '-':
        result = a - b;
        break;
    case '*':
        result = a * b;
        break;
    case '/':
        result = a / b;
        break;
    default:
        throw new IllegalArgumentException("Invalid operator");
}

System.out.println("Result: " + result);  // Result: 50.0
```

**6. Modern `switch` expression — Season lookup:**

```java
int month = 12;

String season = switch (month) {
    case 12, 1, 2 -> "Winter";
    case 3, 4, 5 -> "Spring";
    case 6, 7, 8 -> "Summer";
    case 9, 10, 11 -> "Autumn";
    default -> "Invalid month";
};

System.out.println(season);  // Winter
```

---

## Common Mistakes

1. **Using `=` instead of `==` in conditions:**

   ```java
   // if (marks = 40) { }  // Compile error: incompatible types (int cannot be used as boolean)
   if (marks == 40) { }     // Correct
   ```

2. **Forgetting `break` in a traditional `switch`, causing unintended fall-through:**

   ```java
   switch (grade) {
       case 'A':
           System.out.println("Excellent");
           // missing break — falls through to case 'B' as well
       case 'B':
           System.out.println("Good");
           break;
   }
   ```

3. **Wrong order in an `else-if` ladder**, causing an earlier, broader condition to always match first and hide later, more specific ones:

   ```java
   if (marks >= 40) {          // this will always be true first for high marks too
       System.out.println("Pass");
   } else if (marks >= 90) {   // unreachable! marks >= 90 already satisfied marks >= 40
       System.out.println("Excellent");
   }
   ```

4. **Using non-constant values in `switch` cases**, which is not allowed:

   ```java
   int x = 5;
   // case x:   // Compile error: case label must be a constant expression
   ```

5. **Confusing `if` with `switch` for range-based conditions** — `switch` cannot directly check ranges like `marks >= 40 && marks < 60`; only exact matches against constants are supported.

6. **Forgetting the `default` case in `switch`**, silently doing nothing when no case matches, which can hide bugs.

7. **Unnecessary nested `if` when a simple `&&` would do**, making code harder to read than it needs to be:
   ```java
   if (age >= 18) {
       if (hasID) { ... }   // could simply be: if (age >= 18 && hasID) { ... }
   }
   ```

---

## MCQs

**1. What is the output of the following code?**

```java
int x = 10;
if (x > 5)
    System.out.println("A");
else if (x > 8)
    System.out.println("B");
else
    System.out.println("C");
```

a) A
b) B
c) C
d) A and B
**Answer: a) A** (once the first true condition is found, remaining ones are skipped)

**2. Which of these data types is NOT allowed in a traditional `switch` expression?**
a) int
b) String
c) double
d) char
**Answer: c) double**

**3. What happens if `break` is omitted in a `switch` case?**
a) Compile error
b) Only that case executes
c) Execution falls through to subsequent cases
d) The switch statement is skipped entirely
**Answer: c) Execution falls through to subsequent cases**

**4. What is the correct syntax for a modern switch expression case with multiple values?**
a) `case 1: case 2: -> "text";`
b) `case 1, 2 -> "text";`
c) `case (1, 2) -> "text";`
d) `case 1 || 2 -> "text";`
**Answer: b) `case 1, 2 -> "text";`**

**5. Which control structure is best suited for checking a range of values, such as `marks >= 40 && marks < 60`?**
a) switch
b) if-else / else-if ladder
c) Both equally well
d) Neither
**Answer: b) if-else / else-if ladder**

**6. What does the `default` case in a switch statement do?**
a) Executes always, regardless of other matches
b) Executes only if no other case matches
c) Causes a compile error if omitted
d) Must always be the first case
**Answer: b) Executes only if no other case matches**

**7. What is a nested `if` statement?**
a) An `if` statement with multiple conditions using `&&`
b) An `if` statement placed inside another `if` or `else` block
c) A `switch` statement inside an `if`
d) Multiple `if` statements written sequentially
**Answer: b) An `if` statement placed inside another `if` or `else` block**

**8. In a traditional switch statement, must `case` values be unique?**
a) No, duplicates are allowed
b) Yes, duplicate case values cause a compile error
c) Only for String switches
d) Only if `default` is present
**Answer: b) Yes, duplicate case values cause a compile error**

---

## Viva Questions

1. What is the difference between `if` and `if-else`?
2. When would you use a nested `if` instead of combining conditions with `&&`?
3. Why must `else-if` ladder conditions be ordered carefully?
4. What is fall-through in a `switch` statement, and how is it avoided?
5. Which data types can be used in a traditional Java `switch` statement?
6. What is the purpose of the `default` case in a `switch` statement?
7. What is the key difference between the traditional `switch` statement and the modern `switch` expression?
8. Why can't `switch` be used for range-based conditions like `age >= 18 && age < 60`?
9. Is it mandatory to include a `default` case in a `switch` statement?
10. Can a `switch` expression's case values be non-constant variables? Why or why not?

---

## Practice Programs

**Easy**

1. Write a program that checks whether a given number is positive, negative, or zero using `if-else`.
2. Write a program that uses `switch` to print the name of the day of the week for a given number (1–7).

**Medium**

3. Write a program that uses a nested `if` to check if a person is eligible for a driving license (age >= 18 and has a valid ID).
4. Write a program that calculates a student's grade using an `else-if` ladder based on marks ranges (A, B, C, D, F).
5. Write a program that uses `switch` to build a simple calculator supporting `+`, `-`, `*`, and `/`.

**Hard**

6. Write a program that determines the number of days in a given month (accounting for leap years) using `switch` combined with `if-else` for the February leap-year check.
7. Rewrite the grading program from Exercise 4 using a modern `switch` expression, and compare readability with the `else-if` version.
8. Write a menu-driven program using `switch` that lets the user choose between multiple operations (e.g., area of circle, area of rectangle, area of triangle) and computes the result based on the selection.
