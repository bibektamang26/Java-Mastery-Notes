# 08. Type Casting

## 1. Introduction

In Java, every variable has a specific data type, and there are many situations where a value of one type needs to be converted into another — for example, using an `int` value where a `double` is expected, or fitting a large calculated value into a smaller variable. This process of converting a value from one data type to another is called **type casting**.

- **What is Type Casting?** Type casting is the process of converting a variable from one data type into another, either automatically by the compiler or manually by the programmer.
- **Why is Type Casting Needed?** Programs frequently mix data types in expressions, method calls, and assignments. Without type casting, Java would not know how to reconcile a `double` result with an `int` variable, or how to pass a `byte` where an `int` is expected — casting bridges that gap in a controlled, predictable way.
- **Importance of Type Casting in Java:** Since Java is strongly and statically typed, it does not allow arbitrary mixing of types. Type casting gives programmers explicit, well-defined rules for converting between types, helping avoid unpredictable behavior while still allowing flexibility when different types genuinely need to interact.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Understand type casting.
- Differentiate between widening and narrowing conversion.
- Perform implicit and explicit casting.
- Identify data loss during conversion.
- Understand type promotion in expressions.

---

## 3. What is Type Casting?

**Definition:** Type casting is the process of converting a value of one data type into another data type. In Java, this can happen **implicitly** (automatically, by the compiler) or **explicitly** (manually, by the programmer using a cast operator).

**Real-life analogy:** Think of pouring water between containers of different sizes. Pouring water from a small cup into a large jug is always safe — the jug can hold everything the cup had, with room to spare (this is like widening casting). But pouring water from a large jug into a small cup risks spilling — some water simply won't fit (this is like narrowing casting, where data can be lost).

**Memory representation:** When a value is cast from one type to another, the JVM either extends the bit pattern (adding extra bits, typically for widening) or truncates it (cutting off extra bits, for narrowing) to fit the target type's memory size.

**Basic example:**

```java
int num = 100;
double d = num;       // implicit widening: int → double

double price = 99.99;
int rounded = (int) price;  // explicit narrowing: double → int (rounded = 99)
```

---

## 4. Why Do We Need Type Casting?

- **Different data types:** Programs often work with multiple data types together (e.g., an `int` counter and a `double` average), and casting allows these types to interact correctly in expressions and assignments.
- **Mathematical calculations:** Calculations often need to change precision temporarily — for instance, dividing two `int` values as `double` to get a decimal result rather than a truncated integer.
- **Method parameters:** A method expecting a `double` parameter can still accept an `int` argument thanks to implicit widening, avoiding the need for multiple overloaded methods for every numeric type.
- **Data conversion:** Real-world data (like user input or values read from files) often needs to be converted between types — such as converting a calculated `double` result into a whole-number `int` for display.
- **API compatibility:** Many built-in Java methods and libraries expect specific types; casting allows values to be adapted to match those expected types without rewriting the underlying data.

**Examples:**

```java
int total = 100;
int count = 3;
double average = (double) total / count;  // casting needed to avoid integer division truncation

System.out.println(average);  // 33.333333333333336
```

---

## 5. Types of Type Casting

```
Type Casting
│
├── Implicit Type Casting
│      (Widening)
└── Explicit Type Casting
       (Narrowing)
```

Java supports two broad categories of type casting: **implicit (widening)** casting, which the compiler performs automatically and safely, and **explicit (narrowing)** casting, which the programmer must perform manually because it carries a risk of data loss.

---

## 6. Implicit Type Casting (Widening)

**Definition:** Implicit type casting, also called **widening conversion**, happens automatically when a value of a smaller data type is assigned to a variable of a larger data type. No special syntax is required — the compiler handles it on its own.

**Why Java performs it automatically:** Because converting from a smaller type to a larger type never causes data loss — the destination type can always represent every possible value the source type could hold — Java considers this conversion completely safe and performs it without requiring the programmer to write anything extra.

**Memory diagram:**

```
byte (1 byte)  →  int (4 bytes)

[ 25 ]  →  [ 0 0 0 25 ]   (extra bytes filled with sign-extended zeros)
```

**Rules:**

- Conversion only happens from a smaller type to a larger, compatible type.
- The conversion happens automatically at compile time — no cast operator needed.
- No data is lost during widening (though very large `long` to `float`/`double` conversions may lose some _precision_, not magnitude — see Section 9).

**Order of widening:**

```
byte
↓
short
↓
int
↓
long
↓
float
↓
double
```

> Note: `char` can also widen directly to `int`, `long`, `float`, or `double`, but does not participate in the byte→short chain since `char` and `byte`/`short` are not directly compatible without an explicit cast.

**Examples:**

```java
byte b = 10;
int i = b;        // byte → int (widening)

int num = 500;
long l = num;      // int → long (widening)

long bigNum = 100000L;
float f = bigNum;  // long → float (widening)

float fVal = 5.75f;
double d = fVal;   // float → double (widening)
```

**Real-life examples:** Storing a small measured quantity (like a `byte` representing someone's age) into a more general-purpose `int` variable used throughout a larger calculation; passing an `int` score into a method that calculates a `double` percentage.

**Advantages:**

- Fully automatic — reduces code clutter since no cast operator is needed.
- **No data loss** in terms of magnitude — the destination type is always large enough to represent the source value exactly (with the minor precision caveat noted above for very large integral values converted to floating-point types).

---

## 7. Explicit Type Casting (Narrowing)

**Definition:** Explicit type casting, also called **narrowing conversion**, is when a value of a larger data type is manually converted into a smaller data type using a **cast operator**. Unlike widening, this must be done explicitly by the programmer because it carries a risk of data loss.

**Why manual casting is required:** Since a smaller data type may not be able to hold the full range or precision of a larger type's value, Java forces the programmer to explicitly acknowledge this risk by writing a cast — this prevents accidental, unnoticed data loss.

**Syntax:**

```java
targetType variableName = (targetType) sourceValue;
```

**Examples:**

```java
double price = 99.99;
int rounded = (int) price;     // 99 (decimal part truncated, not rounded)

int largeNum = 300;
byte b = (byte) largeNum;      // overflow: result is 44, not 300

long bigValue = 123456789L;
int i = (int) bigValue;        // fits fine since it's within int range
```

**Memory diagram:**

```
int (4 bytes)  →  byte (1 byte)

[ 00000001 00101100 ]  →  [ 00101100 ]   (higher-order bytes are simply discarded)
```

**Possible data loss:**

- **Truncation:** Converting a floating-point value to an integer type discards the fractional part entirely (it does _not_ round).
- **Overflow/wraparound:** Converting a large value into a smaller integer type can cause the value to wrap around unpredictably if it doesn't fit.

**Overflow examples:**

```java
int x = 130;
byte b = (byte) x;   // -126 (130 exceeds byte's max of 127, wraps around)

double d = 9.99;
int i = (int) d;     // 9 (fractional part .99 is simply dropped, not rounded to 10)
```

**Advantages:**

- Gives the programmer full control over conversions, even ones that might otherwise be unsafe.
- Enables necessary conversions, such as fitting calculated results into smaller storage types when the value range is known to be safe.

**Disadvantages:**

- Risk of silent data loss (truncation or overflow) if not handled carefully.
- Can introduce subtle bugs that are hard to trace, since Java does not throw a runtime error for overflow during narrowing — it just produces an unexpected value.

---

## 8. Widening vs Narrowing

| Aspect                   | Widening (Implicit)                                                      | Narrowing (Explicit)                                                                        |
| ------------------------ | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Direction                | Smaller type → Larger type                                               | Larger type → Smaller type                                                                  |
| Cast operator required?  | No                                                                       | Yes, e.g. `(int)`                                                                           |
| Performed by             | Compiler, automatically                                                  | Programmer, manually                                                                        |
| Risk of data loss        | None (magnitude always preserved)                                        | Yes — truncation or overflow possible                                                       |
| Safety                   | Always safe                                                              | Potentially unsafe                                                                          |
| Compile-time check       | No error even without a cast                                             | Compile error if attempted without a cast                                                   |
| Example                  | `int → double`                                                           | `double → int`                                                                              |
| Fractional part handling | N/A (going to a type with equal/greater precision)                       | Fractional part is truncated, not rounded                                                   |
| Typical use case         | Passing a smaller type into a calculation/method expecting a larger type | Fitting a large calculated value into a smaller variable when the range is known to be safe |
| Common pitfall           | Minor precision loss for very large `long`→`float`/`double` conversions  | Silent overflow/wraparound or unexpected truncation                                         |

---

## 9. Data Loss During Casting

Data loss most commonly happens during narrowing conversions, and can take a few different forms:

```
double
↓
int
↓
byte
```

**Truncation:** When converting from a floating-point type (`float`/`double`) to an integer type (`int`, `long`, etc.), Java discards the entire fractional part — it does **not** round to the nearest whole number.

```java
double d = 9.9999;
int i = (int) d;   // 9, not 10
```

**Overflow:** When converting a value that is too large to fit into a smaller integer type, the value wraps around using two's complement rules, producing an unexpected (often negative) result.

```java
int x = 200;
byte b = (byte) x;   // -56 (200 doesn't fit in byte's -128 to 127 range)
```

**Precision loss:** When converting very large `long` values into `float` (or in rare cases `double`), the value's _magnitude_ is preserved, but some of the least-significant digits may be lost because `float`/`double` cannot represent every possible large integer exactly.

```java
long bigLong = 123456789123456789L;
float f = bigLong;   // may not represent the exact same value due to limited precision
```

---

## 10. Primitive Type Conversion Chart

| From ↓ / To → | byte          | short         | int           | long          | float         | double        | char          |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| byte          | —             | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✖ Manual only |
| short         | ✖ Manual      | —             | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✖ Manual only |
| int           | ✖ Manual      | ✖ Manual      | —             | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✖ Manual only |
| long          | ✖ Manual      | ✖ Manual      | ✖ Manual      | —             | ✔ Automatic   | ✔ Automatic   | ✖ Manual only |
| float         | ✖ Manual      | ✖ Manual      | ✖ Manual      | ✖ Manual      | —             | ✔ Automatic   | ✖ Manual only |
| double        | ✖ Manual      | ✖ Manual      | ✖ Manual      | ✖ Manual      | ✖ Manual      | —             | ✖ Manual only |
| char          | ✖ Manual      | ✖ Manual      | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | ✔ Automatic   | —             |
| boolean       | ✖ Not Allowed | ✖ Not Allowed | ✖ Not Allowed | ✖ Not Allowed | ✖ Not Allowed | ✖ Not Allowed | ✖ Not Allowed |

> ✔ Automatic = implicit widening. ✖ Manual = requires explicit cast (narrowing). ✖ Not Allowed = no conversion exists at all (this applies to `boolean` with every other type).

---

## 11. Type Promotion

**Definition:** Type promotion refers to Java automatically converting smaller integer types to `int` (or, in mixed expressions, to the "widest" type present) when they are used in arithmetic expressions — even if neither the operands nor the result are explicitly declared as `int`.

**Automatic promotion in expressions:**

```
byte + byte
↓
int
```

```java
byte a = 10;
byte b = 20;
// byte c = a + b;      // Compile Error: result of a+b is promoted to int
int c = a + b;           // Correct: result must be stored as int (or explicitly cast back to byte)
```

**Explain why:** Java's arithmetic operators are defined to always operate on at least `int`-sized values. This is partly for historical/performance reasons at the JVM instruction level, and partly to reduce the risk of overflow during intermediate calculations — performing arithmetic at a minimum of `int` precision gives more headroom before results wrap around.

Similarly, in mixed-type expressions, the result is promoted to the **widest** type among the operands:

```java
int i = 10;
double d = 5.5;
double result = i + d;   // int is promoted to double for the addition; result: 15.5
```

---

## 12. Character Type Casting

**ASCII:** A legacy 7-bit character encoding covering the basic English alphabet, digits, and symbols (values 0–127).

**Unicode:** Java's `char` type is based on the 16-bit Unicode standard, allowing it to represent a vastly larger set of characters from many world languages, not just ASCII.

Internally, a `char` is stored as an unsigned 16-bit integer, which is why it can be freely converted to and from numeric types.

**Examples:**

```java
// char → int
char ch = 'A';
int code = ch;          // 65 (implicit widening)

// int → char
int num = 66;
char letter = (char) num;  // 'B' (explicit narrowing required)
```

**Character arithmetic:**

```java
char c = 'A';
char next = (char) (c + 1);  // 'B'

System.out.println('A' + 1);        // 66 (int result, since char + int promotes to int)
System.out.println((char) ('A' + 1)); // 'B' (explicit cast back to char)
```

---

## 13. Boolean Conversion

**Explain why:** Unlike C/C++, Java strictly separates `boolean` from all numeric types. A `boolean` can only ever hold `true` or `false`, and Java's type system does not define any conversion path between `boolean` and numeric types, in either direction.

```
boolean
×
Cannot be converted to numeric types
```

```java
boolean flag = true;
// int x = flag;         // Compile Error: incompatible types
// boolean b = 1;         // Compile Error: incompatible types
```

**Comparison with C/C++:** In C/C++, integers can be implicitly treated as booleans (`0` is false, any non-zero value is true), and booleans can be used in arithmetic. Java deliberately removes this flexibility to prevent a common class of bugs — such as accidentally writing `if (x = 1)` (assignment) instead of `if (x == 1)` (comparison) — which would silently compile in C but is a compile-time error in Java, since the assignment's result (`1`, an `int`) cannot be implicitly used as a `boolean`.

---

## 14. Wrapper Class Conversion (Brief Introduction)

Java provides **wrapper classes** (`Integer`, `Double`, `Character`, `Boolean`, etc.) that allow primitive values to be treated as objects — necessary for use in collections like `ArrayList`, which cannot store primitives directly.

**Autoboxing:** The automatic conversion of a primitive value into its corresponding wrapper object.

```java
int num = 10;
Integer obj = num;   // autoboxing: int → Integer
```

**Unboxing:** The automatic conversion of a wrapper object back into its corresponding primitive value.

```java
Integer obj = 25;
int num = obj;        // unboxing: Integer → int
```

This is just a brief introduction — autoboxing, unboxing, and the wrapper classes are covered in full detail in a later chapter dedicated to them.

---

## 15. Practical Examples

**Student Marks (average calculation requiring casting):**

```java
int totalMarks = 450;
int numberOfSubjects = 5;
double average = (double) totalMarks / numberOfSubjects;
System.out.println("Average: " + average);  // 90.0
```

**Temperature Converter (Celsius to Fahrenheit):**

```java
int celsius = 30;
double fahrenheit = (celsius * 9.0 / 5) + 32;  // int promoted to double via 9.0
System.out.println("Fahrenheit: " + fahrenheit);  // 86.0
```

**Currency Converter:**

```java
double amountInDollars = 100.75;
int roundedRupees = (int) (amountInDollars * 83);  // explicit narrowing after calculation
System.out.println("Rupees (rounded down): " + roundedRupees);
```

**Grade Calculator:**

```java
int marks = 78;
char grade = marks >= 90 ? 'A' : marks >= 75 ? 'B' : marks >= 60 ? 'C' : 'F';
System.out.println("Grade: " + grade);  // B
```

**Character Converter:**

```java
char lower = 'a';
char upper = (char) (lower - 32);  // 'A' — using ASCII/Unicode offset between cases
System.out.println("Uppercase: " + upper);
```

---

## 16. Common Errors

- **Loss of precision:** Casting a `double` with a fractional part into an `int` silently drops the decimal portion instead of rounding it.
  ```java
  int x = (int) 9.9;  // 9, not 10
  ```
- **Overflow:** Narrowing a value that's too large for the target type causes it to wrap around unexpectedly.
  ```java
  byte b = (byte) 200;  // -56, not 200
  ```
- **Wrong casting:** Forgetting to cast when narrowing is required results in a compile-time error.
  ```java
  int i = 100;
  // byte b = i;   // Compile Error: possible lossy conversion
  byte b = (byte) i;  // Correct
  ```
- **Invalid conversions:** Attempting to cast between fundamentally incompatible types, such as `boolean` and `int`, is not allowed under any circumstances.
  ```java
  // int x = (int) true;  // Compile Error: incompatible types
  ```

---

## 17. Best Practices

- **Use widening whenever possible** — it's automatic, safe, and requires no special handling.
- **Avoid unnecessary casting** — only cast when a genuine type mismatch requires it; excessive casting can make code harder to read and hide potential bugs.
- **Use explicit casting carefully** — always be aware of the target type's range and whether truncation or overflow could occur before casting.
- **Check data range** before narrowing a value, especially when the source value is calculated at runtime and its exact size isn't known in advance.

---

## 18. Interview Corner

**Q1. Difference between widening and narrowing?**
Widening converts a smaller type to a larger one automatically and safely, with no data loss; narrowing converts a larger type to a smaller one, requires an explicit cast, and can cause data loss through truncation or overflow.

**Q2. Why is `byte + byte = int`?**
Java's arithmetic operators always promote operands smaller than `int` to `int` before performing the operation, both for JVM-level consistency and to reduce the risk of overflow during the calculation.

**Q3. Can boolean be cast?**
No. Java does not allow any conversion between `boolean` and numeric types in either direction — this is a deliberate design choice to avoid ambiguity between assignment and comparison.

**Q4. What happens when casting double to int?**
The fractional part is truncated (discarded), not rounded — `(int) 9.9` results in `9`, not `10`.

**Q5. Does widening ever cause data loss?**
Not in terms of magnitude, but converting very large `long` values to `float` or `double` can lose some least-significant precision, since floating-point types can't represent every large integer exactly.

**Q6. Why does Java require explicit casts for narrowing but not widening?**
Because narrowing risks losing data, Java forces the programmer to consciously acknowledge that risk by writing a cast, whereas widening is always safe and doesn't need that safeguard.

**Q7. What is the difference between casting and type promotion?**
Casting is an explicit or implicit conversion applied to a single value/variable; type promotion is the automatic elevation of operand types (typically to `int` or the widest type present) specifically within an arithmetic expression.

**Q8. Can you cast an `Object` to a `boolean`?**
No — `boolean` cannot be cast to or from any other type, primitive or reference, except its own wrapper class `Boolean` (via autoboxing/unboxing).

---

## 19. MCQs

**1. Which of these is an example of widening conversion?**
a) double to int
b) int to byte
c) byte to int
d) float to int
**Answer: c) byte to int**

**2. What is the result of `(int) 7.8`?**
a) 8
b) 7
c) 7.8
d) Compile error
**Answer: b) 7**

**3. Which conversion requires an explicit cast?**
a) int to long
b) float to double
c) double to int
d) byte to short (widening chain not direct but is automatic)
**Answer: c) double to int**

**4. What is the output of `byte b = (byte) 130;`?**
a) 130
b) -126
c) Compile error
d) 2
**Answer: b) -126**

**5. In `byte a = 5, b = 10; int c = a + b;`, what type is the result of `a + b` before assignment?**
a) byte
b) short
c) int
d) long
**Answer: c) int**

**6. Which of these conversions is NOT allowed in Java?**
a) char to int
b) boolean to int
c) int to double
d) byte to int
**Answer: b) boolean to int**

**7. What does `(char) 66` evaluate to?**
a) 66
b) 'B'
c) 'A'
d) Compile error
**Answer: b) 'B'**

**8. Which type is at the "widest" end of the widening chain?**
a) float
b) long
c) double
d) int
**Answer: c) double**

**9. What is autoboxing?**
a) Converting a wrapper object to a primitive
b) Converting a primitive to a wrapper object automatically
c) Casting between two primitive types
d) None of the above
**Answer: b) Converting a primitive to a wrapper object automatically**

**10. What is the output of `System.out.println('A' + 1);`?**
a) 'B'
b) 66
c) A1
d) Compile error
**Answer: b) 66**

**11. Which of the following causes truncation, not rounding?**
a) int to long
b) double to int
c) byte to int
d) float to double
**Answer: b) double to int**

**12. What is the default promoted type for a `short + short` expression?**
a) short
b) int
c) long
d) double
**Answer: b) int**

**13. Which cast is invalid in Java?**
a) (int) 5.5
b) (double) 5
c) (int) true
d) (char) 65
**Answer: c) (int) true**

**14. What happens when a `long` value is assigned to an `int` variable without a cast?**
a) It compiles fine
b) It causes a compile-time error
c) It's automatically truncated
d) It throws a runtime exception
**Answer: b) It causes a compile-time error**

**15. Which of these best describes narrowing conversion?**
a) Always safe, no cast required
b) Automatic, performed by the compiler
c) Manual, requires explicit cast, may lose data
d) Only applies to reference types
**Answer: c) Manual, requires explicit cast, may lose data**

---

## 20. Short Questions

1. What is type casting in Java?
2. What is the difference between implicit and explicit type casting?
3. Why does narrowing conversion require an explicit cast?
4. What happens when a `double` is cast to an `int`?
5. What is type promotion? Give an example.
6. Can a `char` be directly assigned to an `int` variable? Why?
7. Why can't `boolean` be cast to `int` in Java?
8. What is the order of types in the widening conversion chain?
9. What is overflow during narrowing conversion?
10. What is the difference between truncation and rounding?

---

## 21. Long Questions

1. Explain implicit and explicit type casting in Java with suitable examples and memory diagrams.
2. Differentiate between widening and narrowing conversion with at least ten comparison points.
3. Explain type promotion in Java expressions with examples involving `byte`, `short`, and mixed-type arithmetic.
4. Discuss the different forms of data loss that can occur during type casting, with examples of truncation, overflow, and precision loss.
5. Write a program demonstrating character type casting (char to int and int to char) and explain the underlying Unicode-based logic.

---

## 22. Viva Questions

1. What is the difference between casting and promotion?
2. Why is widening conversion always considered safe?
3. What is the result of casting a negative `int` to a `byte`?
4. Can you cast a `String` to an `int` directly? Why or why not?
5. Why does `(int) 9.99` return `9` and not `10`?
6. What is the smallest and largest type in the widening chain?
7. Why is `boolean` excluded from all casting conversions?
8. What is autoboxing, and how does it relate to casting?
9. Does casting a `float` to a `double` ever cause data loss?
10. Why does Java promote `byte + byte` to `int` instead of keeping it as `byte`?

---

## 23. Programming Exercises

**Easy**

1. Write a program that declares an `int` and assigns it to a `double` variable, then prints both values.
2. Write a program that casts a `double` value to an `int` and observes the truncation.

**Medium**

3. Write a program that demonstrates overflow by casting an `int` value greater than 127 into a `byte`.
4. Write a program that converts a lowercase character to uppercase using character arithmetic and casting.

**Hard**

5. Write a program that takes a floating-point value from the user, casts it to an `int`, and compares the original and casted values to explain the precision lost, along with the type of loss (truncation vs overflow).

---

## 24. Challenge Problems

**Calculator:** Build a simple calculator that performs addition, subtraction, multiplication, and division on two numbers, correctly casting operands to `double` before division to avoid integer truncation.

**Character Converter:** Write a program that converts an entire string between uppercase and lowercase manually using character-to-int casting and arithmetic, without using built-in string methods.

**Temperature Converter:** Build a program converting between Celsius, Fahrenheit, and Kelvin, carefully handling the casting needed to preserve decimal precision throughout the calculations.

**ASCII Table Generator:** Write a program that prints a table of characters and their corresponding ASCII/Unicode values for a given range, using `char`-to-`int` casting.

---

## 25. Summary

- **Type casting** is the process of converting a value from one data type to another, either automatically (implicit/widening) or manually (explicit/narrowing).
- **Widening conversion** moves from a smaller type to a larger one, is performed automatically by the compiler, and never loses data in terms of magnitude.
- **Narrowing conversion** moves from a larger type to a smaller one, requires an explicit cast, and can cause truncation, overflow, or precision loss.
- **Type promotion** automatically elevates smaller integer types to `int` (or the widest operand type) during arithmetic expressions.
- `char` can be freely cast to and from numeric types since it is internally an unsigned 16-bit integer, but `boolean` cannot be cast to or from any numeric type under any circumstances.
- **Wrapper classes** enable autoboxing and unboxing, bridging primitive values and their object equivalents — a topic covered in greater depth separately.

---

## 26. Key Takeaways

- Widening = automatic + safe; Narrowing = manual + risky.
- Casting a floating-point value to an integer type **truncates**, it does not round.
- `byte`/`short`/`char` are always promoted to `int` in arithmetic expressions.
- `boolean` participates in **no** type conversions with any other type.
- Overflow during narrowing wraps values around silently — Java does not throw an error.
- Always check the target type's range before performing a narrowing cast on runtime-calculated values.

---

## 27. Cheat Sheet

```
WIDENING (Implicit, Safe, Automatic)
byte → short → int → long → float → double
char → int → long → float → double

NARROWING (Explicit, Risky, Manual cast required)
double → float → long → int → short → byte
int → char (manual, even though same "level" conceptually)

TYPE PROMOTION IN EXPRESSIONS
byte/short/char (operand) → promoted to int
Mixed types → result promoted to the widest operand type

BOOLEAN
No conversion to/from any numeric type — ever.

CASTING SYNTAX
targetType variable = (targetType) value;

COMMON RESULTS TO REMEMBER
(int) 9.99        → 9   (truncation, not rounding)
(byte) 130        → -126 (overflow/wraparound)
(char) 65         → 'A'
'A' + 1            → 66  (int, due to promotion)
(char) ('A' + 1)  → 'B'
```

---

## 28. Common Mistakes

**Compilation errors:**

```java
byte b = 300;             // Error: constant too large for byte at compile time
int i = 10L;               // Error: possible lossy conversion from long to int
boolean flag = (boolean) 1; // Error: incompatible types, boolean cannot be cast
```

**Runtime mistakes (not actual exceptions, but unexpected silent results):**

```java
int x = 130;
byte b = (byte) x;    // Silently becomes -126, no error or warning at runtime
```

**Logic mistakes:**

```java
double avg = totalMarks / numberOfSubjects;         // Wrong: integer division happens first if both are int
double avgCorrect = (double) totalMarks / numberOfSubjects;  // Correct: cast before division

int rounded = (int) 9.9;   // Mistake if the intent was actually to round to 10 —
                            // (int) truncates; use Math.round() for true rounding
```
