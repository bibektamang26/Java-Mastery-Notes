# 09. Operators in Java

## 1. Introduction

- **What are Operators?** Operators are special symbols in Java that instruct the compiler to perform specific mathematical, logical, relational, or bitwise operations on one or more values (called operands), producing a result.
- **Why are Operators Needed?** Nearly every meaningful program needs to perform calculations, make comparisons, combine conditions, or manipulate bits — operators are the fundamental tools that make all of this possible directly within expressions.
- **Importance of Operators in Programming:** Operators form the backbone of nearly every line of logic in a program — from simple arithmetic like calculating a total, to complex decision-making involving multiple combined conditions. A solid understanding of operators, their precedence, and their behavior is essential to writing correct and predictable Java code.

---

## 2. Learning Objectives

After completing this chapter, you will be able to:

- Define operators.
- Identify different types of operators.
- Perform arithmetic and logical operations.
- Understand operator precedence and associativity.
- Use operators in real Java programs.

---

## 3. What is an Operator?

**Definition:** An operator is a symbol that tells the compiler to perform a specific operation — such as addition, comparison, or logical combination — on one, two, or three operands.

**Real-life analogy:** Think of an operator like a verb in a sentence — it describes the _action_ being performed. "Ram **adds** 5 and 10" — here "adds" is the action (operator), while "5" and "10" are the things being acted upon (operands), just as `+` acts on `5` and `10` in `5 + 10`.

**Components of an operation:**

- **Operand:** The value(s) an operator acts upon.
- **Operator:** The symbol representing the action to be performed.
- **Expression:** The combination of operators and operands that, together, evaluates to a single result.

**Example:**

```java
int result = 5 + 10;
// 5 and 10 are operands
// + is the operator
// 5 + 10 is the expression
// result stores the evaluated value: 15
```

---

## 4. What is an Operand?

**Definition:** An operand is a value or variable on which an operator performs its operation. Operands can be literals, variables, or even the results of other expressions.

**Examples:**

```java
int a = 10;
int b = 20;

int sum = a + b;
```

- **Operator** → `+`
- **Operands** → `a`, `b`

---

## 5. What is an Expression?

**Definition:** An expression is a combination of operators, operands, and (optionally) method calls that the compiler evaluates to produce a single value.

**Examples:**

```java
a + b

a * b + c

(x + y) / 2
```

Each of these combines variables and operators into a unit that resolves to one final computed value when evaluated.

---

## 6. Classification of Java Operators

```
Java Operators
│
├── Arithmetic Operators
├── Unary Operators
├── Assignment Operators
├── Relational (Comparison) Operators
├── Logical Operators
├── Bitwise Operators
├── Shift Operators
├── Conditional (Ternary) Operator
├── instanceof Operator
└── Type Cast Operator
```

---

## 7. Arithmetic Operators

**Introduction:** Arithmetic operators perform basic mathematical calculations on numeric operands — addition, subtraction, multiplication, division, and remainder (modulus).

```
+   -   *   /   %
```

### `+` (Addition)

- **Definition:** Adds two operands together.
- **Syntax:** `operand1 + operand2`
- **Example:** `int sum = 10 + 5;`
- **Output:** `15`
- **Real-life Example:** Adding the prices of two items in a shopping cart.
- **Common Mistakes:** Using `+` between a `String` and a number concatenates instead of adding — `"Total: " + 5 + 3` results in `"Total: 53"`, not `"Total: 8"`, since evaluation happens left to right.

### `-` (Subtraction)

- **Definition:** Subtracts the second operand from the first.
- **Syntax:** `operand1 - operand2`
- **Example:** `int diff = 20 - 8;`
- **Output:** `12`
- **Real-life Example:** Calculating remaining balance after a withdrawal.
- **Common Mistakes:** Confusing subtraction with the unary negative sign, e.g., `10 - -5` (which correctly evaluates to `15`) can look confusing without spacing.

### `*` (Multiplication)

- **Definition:** Multiplies two operands.
- **Syntax:** `operand1 * operand2`
- **Example:** `int product = 6 * 7;`
- **Output:** `42`
- **Real-life Example:** Calculating total cost as price × quantity.
- **Common Mistakes:** Multiplying two large `int` values can silently overflow — see Section 21.

### `/` (Division)

- **Definition:** Divides the first operand by the second.
- **Syntax:** `operand1 / operand2`
- **Example (integer division):** `int result = 10 / 3;` → `3` (fractional part truncated)
- **Example (floating-point division):** `double result = 10.0 / 3;` → `3.3333333333333335`
- **Real-life Example:** Splitting a total bill equally among a group of friends.
- **Common Mistakes:** Dividing two `int` values always produces a truncated `int` result, even when a decimal answer is expected — see Section 22.

### `%` (Modulus/Remainder)

- **Definition:** Returns the remainder of dividing the first operand by the second.
- **Syntax:** `operand1 % operand2`
- **Example:** `int rem = 10 % 3;`
- **Output:** `1`
- **Real-life Example:** Determining whether a number is even or odd (`num % 2 == 0`).
- **Common Mistakes:** Assuming modulus only works with positive numbers — Java allows negative operands too, but the sign of the result follows the sign of the dividend (see Section 22).

**Memory diagram (conceptual):**

```
a = 10, b = 3

a + b → 13
a - b → 7
a * b → 30
a / b → 3   (int division, truncated)
a % b → 1   (remainder)
```

---

## 8. Unary Operators

```
+   -   ++   --   !   ~
```

Unary operators act on a **single** operand.

- **`+` (Unary Plus):** Indicates a positive value (rarely used explicitly, since numbers are positive by default). Example: `int x = +5;`
- **`-` (Unary Minus):** Negates a value. Example: `int x = -5;`
- **`++` (Increment):** Increases a variable's value by 1.
- **`--` (Decrement):** Decreases a variable's value by 1.
- **`!` (Logical NOT):** Reverses a boolean value. Example: `!true` → `false`.
- **`~` (Bitwise Complement):** Inverts all bits of an integer value. Example: `~5` → `-6`.

**Pre Increment (`++x`):** Increments the value **first**, then uses the updated value in the expression.

**Post Increment (`x++`):** Uses the **current** value in the expression **first**, then increments it afterward.

**Pre Decrement (`--x`):** Decrements the value first, then uses it.

**Post Decrement (`x--`):** Uses the current value first, then decrements it.

**Difference:**

```java
int x = 5;
int a = ++x;   // x becomes 6 first, then a = 6
System.out.println(x + " " + a);  // 6 6

int y = 5;
int b = y++;   // b = 5 (current value used), then y becomes 6
System.out.println(y + " " + b);  // 6 5
```

**Memory Diagram:**

```
Pre-increment (++x):    [increment x] → [use x in expression]
Post-increment (x++):   [use x in expression] → [increment x]
```

**Common Interview Questions:**

- What is the output of `int x = 5; System.out.println(x++ + ++x);`? → `5 + 7 = 12` (x becomes 6 after `x++` is evaluated as 5, then `++x` makes it 7)
- Does `!` work on integers? → No, `!` only applies to `boolean` values in Java.
- What does `~5` evaluate to and why? → `-6`, because bitwise complement flips every bit, and for two's complement representation this is equivalent to `-(x + 1)`.

---

## 9. Assignment Operators

```
=   +=   -=   *=   /=   %=   &=   |=   ^=   <<=   >>=   >>>=
```

**Explain each:**

- **`=`** — Assigns the right-hand value to the left-hand variable. `int x = 10;`
- **`+=`** — Adds the right operand to the variable and assigns the result. `x += 5;` is equivalent to `x = x + 5;`
- **`-=`** — Subtracts and assigns. `x -= 5;` ≡ `x = x - 5;`
- **`*=`** — Multiplies and assigns. `x *= 5;` ≡ `x = x * 5;`
- **`/=`** — Divides and assigns. `x /= 5;` ≡ `x = x / 5;`
- **`%=`** — Takes modulus and assigns. `x %= 5;` ≡ `x = x % 5;`
- **`&=`** — Bitwise AND and assigns. `x &= 5;` ≡ `x = x & 5;`
- **`|=`** — Bitwise OR and assigns. `x |= 5;` ≡ `x = x | 5;`
- **`^=`** — Bitwise XOR and assigns. `x ^= 5;` ≡ `x = x ^ 5;`
- **`<<=`** — Left shift and assigns. `x <<= 2;` ≡ `x = x << 2;`
- **`>>=`** — Right shift and assigns. `x >>= 2;` ≡ `x = x >> 2;`
- **`>>>=`** — Unsigned right shift and assigns. `x >>>= 2;` ≡ `x = x >>> 2;`

**Examples:**

```java
int score = 50;
score += 10;   // 60
score -= 5;    // 55
score *= 2;    // 110
score /= 10;   // 11
score %= 4;    // 3
```

**Advantages:** Compound assignment operators make code shorter and more readable, and also implicitly perform a narrowing cast back to the variable's original type — for instance, `byte b = 10; b += 5;` compiles fine even though `b + 5` alone would be an `int`, because `+=` automatically casts the result back to `byte`.

---

## 10. Relational Operators

```
==   !=   >   <   >=   <=
```

**Examples:**

```java
int a = 10, b = 20;

System.out.println(a == b);  // false
System.out.println(a != b);  // true
System.out.println(a > b);   // false
System.out.println(a < b);   // true
System.out.println(a >= 10); // true
System.out.println(b <= 20); // true
```

**Return Type:** Every relational operator evaluates to a `boolean` value — either `true` or `false`.

**Boolean Result:** Because the result is always a `boolean`, relational expressions are most commonly used directly inside control-flow statements like `if`, `while`, and `for`.

**Real-life Examples:** Checking if a student's marks meet the passing threshold (`marks >= 40`), or if a user's age qualifies them for voting (`age >= 18`).

---

## 11. Logical Operators

```
&&   ||   !
```

**Truth Table:**

| A     | B     | A && B | A \|\| B | !A    |
| ----- | ----- | ------ | -------- | ----- |
| true  | true  | true   | true     | false |
| true  | false | false  | true     | false |
| false | true  | false  | true     | true  |
| false | false | false  | false    | true  |

**Examples:**

```java
int age = 20;
boolean hasID = true;

System.out.println(age >= 18 && hasID);  // true
System.out.println(age < 18 || hasID);   // true
System.out.println(!hasID);              // false
```

**Short-Circuit Evaluation:** `&&` and `||` are "short-circuiting" — if the result can already be determined from the first operand, the second operand is **not evaluated at all**. For `&&`, if the first operand is `false`, the whole expression is `false` and the second operand is skipped. For `||`, if the first operand is `true`, the whole expression is `true` and the second operand is skipped.

```java
int x = 0;
if (x != 0 && (10 / x > 1)) {  // second condition never evaluated, avoids division by zero
    System.out.println("Safe");
}
```

**Difference between `&&` and `&`:** `&&` is short-circuiting (skips the second operand when possible); `&` always evaluates **both** operands, even when the result is already determined — `&` can also be used as a bitwise operator on integers, whereas `&&` cannot.

**Difference between `||` and `|`:** Same relationship — `||` short-circuits and skips the second operand when the first is already `true`; `|` always evaluates both operands and can also serve as a bitwise OR on integers.

---

## 12. Bitwise Operators

```
&   |   ^   ~
```

Bitwise operators work directly on the individual **bits** of integer operands.

**Binary Examples:**

```
a = 5   → 0101
b = 3   → 0011

a & b   → 0001  (1)   — AND: 1 only where both bits are 1
a | b   → 0111  (7)   — OR: 1 where at least one bit is 1
a ^ b   → 0110  (6)   — XOR: 1 where bits differ
~a      → 1010...  (-6 in two's complement) — flips every bit
```

**Bit Manipulation:** Bitwise operators are often used to set, clear, toggle, or check individual bits within a number — a common technique in low-level programming, flags, and permission systems.

**Applications:** Encoding multiple boolean flags into a single integer (permission systems), fast even/odd checks (`n & 1`), swapping values without a temporary variable using XOR, and cryptographic or compression algorithms.

---

## 13. Shift Operators

```
<<   >>   >>>
```

**Binary Representation:** Shift operators move the bits of a number left or right by a specified number of positions.

- **`<<` (Left Shift):** Shifts bits to the left, filling vacated positions with `0`. Equivalent to multiplying by 2 for each position shifted (for values within range).
- **`>>` (Signed Right Shift):** Shifts bits to the right, filling vacated positions with the **sign bit** (0 for positive numbers, 1 for negative numbers) — preserves the sign of the original number.
- **`>>>` (Unsigned Right Shift):** Shifts bits to the right, always filling vacated positions with `0`, regardless of sign — this can turn a negative number into a large positive one.

**Positive Numbers:**

```java
int a = 8;          // 0000...1000
System.out.println(a << 1);  // 16  (0000...10000)
System.out.println(a >> 1);  // 4   (0000...0100)
System.out.println(a >>> 1); // 4   (same as >> for positive numbers)
```

**Negative Numbers:**

```java
int b = -8;
System.out.println(b >> 1);   // -4  (sign bit preserved)
System.out.println(b >>> 1);  // a large positive number (sign bit not preserved, 0 filled in)
```

**Examples:**

```java
System.out.println(4 << 2);   // 16  (4 * 2^2)
System.out.println(16 >> 2);  // 4   (16 / 2^2)
```

---

## 14. Conditional (Ternary) Operator

```
condition ? expression1 : expression2
```

**Syntax:** If `condition` evaluates to `true`, the ternary expression evaluates to `expression1`; otherwise, it evaluates to `expression2`.

**Examples:**

```java
int marks = 75;
String result = (marks >= 40) ? "Pass" : "Fail";
System.out.println(result);  // Pass

int a = 10, b = 20;
int max = (a > b) ? a : b;
System.out.println(max);  // 20
```

**Comparison with if-else:**

```java
// Using if-else
String result;
if (marks >= 40) {
    result = "Pass";
} else {
    result = "Fail";
}

// Using ternary (equivalent, more concise)
String result2 = (marks >= 40) ? "Pass" : "Fail";
```

**Advantages:** The ternary operator condenses simple two-branch conditional logic into a single expression, which is especially useful for concise variable assignments, though it should be avoided for overly complex or nested conditions where an `if-else` would be clearer.

---

## 15. `instanceof` Operator

**Definition:** The `instanceof` operator checks whether an object is an instance of a specific class (or implements a specific interface), returning a `boolean` result.

**Purpose:** It is primarily used to safely verify an object's actual runtime type before performing a type-specific operation, such as casting, helping avoid `ClassCastException`.

**Examples:**

```java
String str = "Hello";
System.out.println(str instanceof String);  // true

Object obj = "Java";
if (obj instanceof String) {
    String s = (String) obj;   // safe cast, since instanceof confirmed the type
    System.out.println(s.length());
}
```

**Object Checking:** `instanceof` returns `false` (rather than throwing an error) if the object is `null`, or if it simply isn't an instance of the specified type.

**Applications:** Commonly used in polymorphic code where a collection may hold objects of different subclasses, and type-specific behavior needs to be safely determined before casting and using an object.

---

## 16. Type Cast Operator

**Brief Introduction:** The type cast operator, written as `(targetType)`, explicitly converts a value from one data type to another — most notably required for narrowing conversions, where data could otherwise be lost.

**Examples:**

```java
double price = 99.99;
int rounded = (int) price;   // 99

Object obj = "Hello";
String s = (String) obj;     // casting a reference type
```

**Reference to previous chapter:** Type casting was covered in complete depth — including implicit widening, explicit narrowing, and all associated data-loss scenarios — in Chapter 08: Type Casting.

---

## 17. Operator Precedence

**Complete Table (Highest → Lowest):**

| Precedence  | Operator(s)                    | Description                                                         |
| ----------- | ------------------------------ | ------------------------------------------------------------------- |
| 1 (Highest) | `()` `[]` `.`                  | Parentheses, array access, member access                            |
| 2           | `++` `--` `~` `!` (unary)      | Postfix/prefix increment-decrement, bitwise complement, logical NOT |
| 3           | `*` `/` `%`                    | Multiplicative                                                      |
| 4           | `+` `-`                        | Additive                                                            |
| 5           | `<<` `>>` `>>>`                | Shift                                                               |
| 6           | `<` `<=` `>` `>=` `instanceof` | Relational                                                          |
| 7           | `==` `!=`                      | Equality                                                            |
| 8           | `&`                            | Bitwise AND                                                         |
| 9           | `^`                            | Bitwise XOR                                                         |
| 10          | `\|`                           | Bitwise OR                                                          |
| 11          | `&&`                           | Logical AND                                                         |
| 12          | `\|\|`                         | Logical OR                                                          |
| 13          | `?:`                           | Ternary                                                             |
| 14 (Lowest) | `=` `+=` `-=` etc.             | Assignment                                                          |

**Examples:**

```java
int result = 5 + 3 * 2;    // 11, not 16 — * has higher precedence than +
int result2 = (5 + 3) * 2; // 16 — parentheses override default precedence
```

**Practice Questions:**

1. What is the result of `10 + 5 * 2 - 3`? (Answer: `17`)
2. What is the result of `(10 + 5) * (2 - 3)`? (Answer: `-15`)
3. What is the result of `20 / 4 + 3 * 2`? (Answer: `11`)

---

## 18. Operator Associativity

**Left to Right:** Most binary operators (arithmetic, relational, logical, bitwise) are evaluated left to right when they share the same precedence level.

```java
int result = 20 - 5 - 3;   // (20 - 5) - 3 = 12, evaluated left to right
```

**Right to Left:** Assignment operators and unary operators are evaluated right to left.

```java
int a, b, c;
a = b = c = 10;   // evaluated right to left: c=10, then b=c, then a=b
```

**Examples:**

```java
int x = 10 / 2 * 5;   // (10 / 2) * 5 = 25, left to right since / and * share precedence
int y;
int z = y = 5;         // right to left: y = 5 first, then z = y
```

---

## 19. Operator Precedence vs Associativity

| Aspect               | Precedence                                                               | Associativity                                                            |
| -------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| Definition           | Determines which operator is evaluated first among _different_ operators | Determines the evaluation order among operators of the _same_ precedence |
| Purpose              | Resolves ambiguity between different operator types                      | Resolves ambiguity between repeated operators of equal priority          |
| Example              | `+` vs `*` — `*` evaluated first                                         | `-` vs `-` — evaluated left to right                                     |
| Direction            | Not directional — it's about ranking operators                           | Directional — left-to-right or right-to-left                             |
| Typical case         | `5 + 3 * 2` → multiplication first                                       | `20 - 5 - 3` → left operation first                                      |
| Assignment operators | Lowest precedence among all operators                                    | Right-to-left associativity                                              |

**Examples:**

```java
int result = 10 + 2 * 3;     // precedence: * before +  → 16
int a = 5, b = 5, c = 5;
int sum = a - b - c;          // associativity: left to right → -5
```

---

## 20. Type Promotion in Expressions

```
byte + byte
↓
int
```

When operands smaller than `int` (such as `byte`, `short`, or `char`) are used in an arithmetic expression, Java automatically promotes them to `int` before performing the operation.

**Examples:**

```java
byte a = 10;
byte b = 20;
int sum = a + b;   // a and b promoted to int before addition; sum = 30

char c1 = 'A';
char c2 = 'B';
int total = c1 + c2;  // both promoted to int; total = 131
```

If the operands are of different sizes, the result is promoted to the size of the **larger** operand (e.g., `int + long` → `long`; `int + double` → `double`).

---

## 21. Overflow & Underflow

**Arithmetic Overflow:** Occurs when the result of a calculation exceeds the maximum value a data type can hold; Java does not raise an error, and the value silently wraps around.

**Examples:**

```java
int max = Integer.MAX_VALUE;      // 2147483647
System.out.println(max + 1);       // -2147483648 (overflow wraps around)
```

**Integer Overflow:**

```java
int a = 2_000_000_000;
int b = 2_000_000_000;
System.out.println(a + b);   // -294967296 (overflowed, incorrect result)
```

**Floating Overflow:** When a floating-point calculation exceeds the maximum representable range, the result becomes `Infinity` or `-Infinity` rather than wrapping around.

```java
double big = Double.MAX_VALUE;
System.out.println(big * 2);   // Infinity
```

---

## 22. Special Cases

**Division by Zero:**

```java
int a = 10 / 0;    // throws ArithmeticException: / by zero (integer division)
double b = 10.0 / 0;  // Infinity (floating-point division does NOT throw an exception)
```

**Modulus with Negative Numbers:** In Java, the sign of the result of `%` follows the sign of the **dividend** (the first operand), not the divisor.

```java
System.out.println(10 % 3);    // 1
System.out.println(-10 % 3);   // -1
System.out.println(10 % -3);   // 1
System.out.println(-10 % -3);  // -1
```

**Floating Point Division:** Dividing floating-point numbers never throws an exception, even for division by zero — it instead produces special values like `Infinity`, `-Infinity`, or `NaN`.

**NaN (Not a Number):** Represents an undefined or unrepresentable floating-point result, such as `0.0 / 0.0`.

```java
System.out.println(0.0 / 0.0);   // NaN
System.out.println(Double.NaN == Double.NaN);  // false — NaN is never equal to anything, including itself
```

**Infinity:** Represents a value too large to be represented, resulting from operations like dividing a nonzero number by `0.0`.

```java
System.out.println(5.0 / 0.0);   // Infinity
System.out.println(-5.0 / 0.0);  // -Infinity
```

---

## 23. Practical Programs

**Calculator:**

```java
int a = 10, b = 3;
System.out.println("Sum: " + (a + b));
System.out.println("Difference: " + (a - b));
System.out.println("Product: " + (a * b));
System.out.println("Quotient: " + (a / b));
System.out.println("Remainder: " + (a % b));
```

**Percentage Calculator:**

```java
int obtained = 450, total = 500;
double percentage = (obtained / (double) total) * 100;
System.out.println("Percentage: " + percentage + "%");
```

**Simple Interest:**

```java
double principal = 10000, rate = 5, time = 2;
double simpleInterest = (principal * rate * time) / 100;
System.out.println("Simple Interest: " + simpleInterest);
```

**Area Calculator (Circle):**

```java
double radius = 7;
double area = Math.PI * radius * radius;
System.out.println("Area: " + area);
```

**Electricity Bill:**

```java
int units = 250;
double ratePerUnit = 6.5;
double bill = units * ratePerUnit;
System.out.println("Electricity Bill: " + bill);
```

**BMI Calculator:**

```java
double weight = 65, heightInMeters = 1.7;
double bmi = weight / (heightInMeters * heightInMeters);
System.out.println("BMI: " + bmi);
```

**Temperature Converter:**

```java
double celsius = 25;
double fahrenheit = (celsius * 9.0 / 5) + 32;
System.out.println("Fahrenheit: " + fahrenheit);
```

**Voting Eligibility:**

```java
int age = 19;
boolean canVote = age >= 18;
System.out.println("Eligible to vote: " + canVote);
```

**Largest Number (of three):**

```java
int a = 12, b = 45, c = 30;
int largest = (a > b) ? (a > c ? a : c) : (b > c ? b : c);
System.out.println("Largest: " + largest);
```

**Even/Odd:**

```java
int num = 17;
System.out.println((num % 2 == 0) ? "Even" : "Odd");
```

**Leap Year:**

```java
int year = 2024;
boolean isLeap = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
System.out.println("Leap Year: " + isLeap);
```

---

## 24. Best Practices

- **Use parentheses** to make the intended evaluation order explicit, even when default precedence would already produce the correct result — this greatly improves readability.
- **Avoid unnecessary increments** or overly compact increment/decrement expressions within larger statements, since they can make code harder to trace (e.g., avoid `a[i++] = i++;`-style expressions).
- **Write readable expressions** — break very long or deeply nested expressions into smaller, well-named intermediate variables.
- **Avoid magic numbers** — use named constants instead of unexplained literal values scattered through expressions.

---

## 25. Common Mistakes

- **Using `=` instead of `==`:** Writing `if (x = 5)` instead of `if (x == 5)` — in Java this is a compile-time error for `boolean` contexts (unlike C/C++), but remains a common source of confusion.
- **Confusing `++i` and `i++`:** Mixing up pre- and post-increment inside complex expressions can produce unexpected results.
- **Integer Division:** Forgetting that dividing two `int` values truncates the result, rather than producing a decimal answer.
- **Overflow:** Not accounting for the possibility that a calculation might exceed a type's maximum value, leading to silently wrong results.
- **Wrong precedence:** Assuming operators are evaluated strictly left to right without considering precedence rules, leading to incorrect results (e.g., forgetting that `*` binds tighter than `+`).

---

## 26. Interview Corner

**Q1. Difference between `++i` and `i++`?**
`++i` (pre-increment) increments the variable first and then uses the updated value in the expression; `i++` (post-increment) uses the current value in the expression first and increments the variable afterward.

**Q2. Difference between `&&` and `&`?**
`&&` is short-circuiting and skips evaluating the second operand if the result is already determined by the first; `&` always evaluates both operands and can also be used as a bitwise operator on integers.

**Q3. Difference between `==` and `.equals()`?**
`==` compares primitive values directly, or for objects, compares whether two references point to the same object in memory; `.equals()` (defined on objects) compares the actual logical content of two objects, and can be overridden to define custom equality logic.

**Q4. Difference between `>>` and `>>>`?**
`>>` is a signed right shift that preserves the sign bit (fills with `0` for positive numbers, `1` for negative numbers); `>>>` is an unsigned right shift that always fills with `0`, regardless of the original sign.

**Q5. Why is `byte + byte = int`?**
Java's arithmetic operators always promote operands smaller than `int` to `int` before performing the operation, both for JVM-level consistency and to reduce overflow risk during intermediate calculations.

**Q6. What is short-circuit evaluation, and why does it matter?**
It's the behavior where `&&` and `||` skip evaluating their second operand once the result is already determined — this matters because it can prevent errors (like division by zero) and improve performance by avoiding unnecessary evaluation.

**Q7. Can the ternary operator replace all if-else statements?**
No — it works well for simple two-branch expressions that produce a value, but becomes unreadable for complex, multi-branch, or statement-heavy logic, where a proper `if-else` chain is clearer.

**Q8. What does `instanceof` return for a `null` object?**
`false` — `instanceof` never throws an exception for `null`; it simply evaluates to `false` since `null` is not an instance of any specific type.

---

## 27. MCQs

**1. What is the output of `10 % 3`?**
a) 3
b) 1
c) 0
d) 10
**Answer: b) 1**

**2. What is the output of `int x = 5; System.out.println(x++ + ++x);`?**
a) 10
b) 11
c) 12
d) 13
**Answer: c) 12**

**3. Which operator has the highest precedence?**
a) `+`
b) `*`
c) `()`
d) `=`
**Answer: c) `()`**

**4. What is the result of `5 & 3`?**
a) 7
b) 1
c) 8
d) 2
**Answer: b) 1**

**5. What does `10 / 0` throw in Java for integer division?**
a) Infinity
b) NaN
c) ArithmeticException
d) Compile error
**Answer: c) ArithmeticException**

**6. What is the result of `10.0 / 0`?**
a) ArithmeticException
b) Infinity
c) 0
d) NaN
**Answer: b) Infinity**

**7. Which operator is short-circuiting?**
a) `&`
b) `|`
c) `&&`
d) `^`
**Answer: c) `&&`**

**8. What is the result of `-10 % 3`?**
a) 1
b) -1
c) 2
d) -2
**Answer: b) -1**

**9. What is the associativity of the assignment operator?**
a) Left to right
b) Right to left
c) None
d) Depends on context
**Answer: b) Right to left**

**10. What does `instanceof` return?**
a) int
b) String
c) boolean
d) Object
**Answer: c) boolean**

**11. What is the result of `~5`?**
a) -5
b) 5
c) -6
d) 6
**Answer: c) -6**

**12. Which of these is NOT a valid assignment operator?**
a) `+=`
b) `**=`
c) `-=`
d) `%=`
**Answer: b) `**=`\*\*

**13. What is the output of `5 + 3 * 2`?**
a) 16
b) 11
c) 13
d) 10
**Answer: b) 11**

**14. What is the result of `0.0 / 0.0`?**
a) 0
b) Infinity
c) NaN
d) ArithmeticException
**Answer: c) NaN**

**15. Which shift operator always fills vacated bits with 0?**
a) `>>`
b) `<<`
c) `>>>`
d) Both b and c
**Answer: d) Both b and c**

**16. What is the output of `int a = 5, b = 5, c = 5; int sum = a - b - c;`?**
a) 5
b) -5
c) 0
d) 15
**Answer: b) -5**

**17. What does the ternary operator return?**
a) A statement
b) A value
c) A boolean only
d) Nothing\*\*
**Answer: b) A value**

**18. What is the output of `Double.NaN == Double.NaN`?**
a) true
b) false
c) NaN
d) Compile error
**Answer: b) false**

**19. Which operator is used for bitwise XOR?**
a) `&`
b) `|`
c) `^`
d) `~`
**Answer: c) `^`**

**20. What is the output of `char c1 = 'A', c2 = 'B'; int total = c1 + c2;`?**
a) AB
b) 131
c) Compile error
d) 65 66
**Answer: b) 131**

---

## 28. Short Questions

1. What is an operator in Java?
2. What is the difference between an operand and an expression?
3. What are the different categories of operators in Java?
4. What is the difference between `++i` and `i++`?
5. What is short-circuit evaluation?
6. What is the difference between `&` and `&&`?
7. What does the `instanceof` operator do?
8. What is the difference between operator precedence and associativity?
9. Why does integer division truncate the result?
10. What is the sign rule for modulus with negative operands in Java?
11. What is NaN, and when does it occur?
12. What is the difference between `>>` and `>>>`?
13. What is the syntax of the ternary operator?
14. What happens during integer overflow in Java?
15. Why is `byte + byte` promoted to `int`?

---

## 29. Long Questions

1. Explain the different categories of operators in Java with at least one example for each.
2. Differentiate between pre-increment and post-increment operators with detailed examples and memory diagrams.
3. Explain operator precedence and associativity in Java with a complete table and practice examples.
4. Discuss bitwise and shift operators in Java with binary representation examples for both positive and negative numbers.
5. Explain the concept of short-circuit evaluation with examples showing how it can prevent runtime errors.
6. Write a program using arithmetic, relational, and logical operators together to determine loan eligibility based on age and income, and explain the logic.
7. Explain the special floating-point cases in Java: division by zero, NaN, and Infinity, with examples.
8. Compare `==` and `.equals()` in Java with suitable examples involving both primitives and objects.
9. Explain type promotion in expressions with examples involving `byte`, `short`, `char`, and mixed-type arithmetic.
10. Discuss integer overflow and underflow in Java with examples, and explain why Java does not throw an error for it.

---

## 30. Viva Questions

1. What is the difference between a unary and a binary operator?
2. Why does Java not allow using an integer directly as a boolean condition?
3. What is the output of `5 / 2` versus `5.0 / 2`?
4. What does the `~` operator do to a number?
5. Why is `&&` preferred over `&` in conditional statements?
6. What is the associativity of the ternary operator?
7. Can `instanceof` throw a `NullPointerException`?
8. Why does `0.0 / 0.0` return `NaN` instead of throwing an exception?
9. What is the difference between `+=` and manually writing `x = x + value`?
10. What happens when you shift a number by more bits than its total bit-width?
11. Why is the assignment operator right-associative?
12. What is the result of combining a `boolean` with `&` — is it a bitwise or logical operation?
13. Why does modulus with a negative dividend produce a negative result in Java?
14. What is the difference between compile-time and run-time division errors?
15. Can the ternary operator be nested? Give an example.

---

## 31. Programming Exercises

**Easy**

1. Write a program that takes two numbers and prints the results of all five arithmetic operators.
2. Write a program that demonstrates the difference between pre-increment and post-increment using print statements.

**Medium**

3. Write a program that checks whether a given year is a leap year using logical operators.
4. Write a program that uses the ternary operator to determine the largest of three numbers.

**Hard**

5. Write a program that uses bitwise operators to check whether a number is a power of two.

---

## 32. Challenge Problems

**Scientific Calculator:** Build a calculator supporting addition, subtraction, multiplication, division, modulus, and power operations, using a menu-driven structure.

**Expression Evaluator:** Write a program that evaluates a simple arithmetic expression given as a string (e.g., `"5 + 3 * 2"`), correctly respecting operator precedence.

**Binary Calculator:** Write a program that performs bitwise AND, OR, XOR, and shift operations on two numbers and displays both the decimal and binary representation of the results.

**Grade Calculator:** Write a program that calculates a student's grade using nested ternary operators based on marks thresholds.

**Discount Calculator:** Write a program that calculates a final price after applying a discount percentage, using arithmetic and assignment operators.

**Tax Calculator:** Write a program that calculates payable tax based on income slabs, using relational and logical operators to determine the correct slab.

---

## 33. Summary

- **Operators** are symbols that perform operations on operands to produce a result, and Java classifies them into arithmetic, unary, assignment, relational, logical, bitwise, shift, ternary, `instanceof`, and cast operators.
- **Arithmetic operators** (`+ - * / %`) perform basic math, with integer division truncating results and modulus following the sign of the dividend.
- **Logical operators** (`&& || !`) support short-circuit evaluation, which can prevent unnecessary computation and runtime errors.
- **Bitwise and shift operators** manipulate the individual bits of integer values, useful for low-level operations and performance-sensitive code.
- **Operator precedence** determines which operator is evaluated first among different operators; **associativity** determines the order among operators of equal precedence.
- Java handles **overflow** silently (wrapping around) for integers, but produces `Infinity`/`NaN` for floating-point special cases instead of throwing exceptions.

---

## 34. Key Takeaways

- Integer division truncates; floating-point division can produce `Infinity` or `NaN`.
- `&&`/`||` short-circuit; `&`/`|` always evaluate both operands.
- Precedence decides _which_ operator goes first; associativity decides direction for _equal_ precedence.
- `byte`/`short`/`char` are always promoted to `int` during arithmetic.
- Integer overflow wraps silently — no exception is thrown.
- `instanceof` safely returns `false` for `null`, never throws an error.

---

## 35. Cheat Sheet

```
ARITHMETIC        +  -  *  /  %
UNARY             +  -  ++  --  !  ~
ASSIGNMENT        =  +=  -=  *=  /=  %=  &=  |=  ^=  <<=  >>=  >>>=
RELATIONAL        ==  !=  >  <  >=  <=
LOGICAL           &&  ||  !
BITWISE           &  |  ^  ~
SHIFT             <<  >>  >>>
TERNARY           condition ? expr1 : expr2
INSTANCEOF        object instanceof ClassName
CAST              (targetType) value

PRECEDENCE (High → Low)
() [] .  →  ++ -- ~ ! (unary)  →  * / %  →  + -  →  << >> >>>
→  < <= > >= instanceof  →  == !=  →  &  →  ^  →  |  →  &&  →  ||  →  ?:  →  = += -= ...

ASSOCIATIVITY
Most binary operators   → Left to Right
Unary & Assignment       → Right to Left

SPECIAL RESULTS
10 / 0        → ArithmeticException
10.0 / 0      → Infinity
0.0 / 0.0     → NaN
-10 % 3       → -1  (sign follows dividend)
Integer.MAX_VALUE + 1 → wraps to Integer.MIN_VALUE
```

---

## 36. Operator Comparison Tables

**Arithmetic vs Assignment**

| Aspect         | Arithmetic                            | Assignment                             |
| -------------- | ------------------------------------- | -------------------------------------- |
| Purpose        | Performs a calculation                | Stores a value into a variable         |
| Result         | Produces a value                      | Modifies a variable's contents         |
| Example        | `a + b`                               | `a += b` (calculates and stores)       |
| Standalone use | Can be used within larger expressions | Always modifies the left-hand variable |

**Logical vs Bitwise**

| Aspect           | Logical (`&&`, `\|\|`, `!`) | Bitwise (`&`, `\|`, `^`, `~`)       |
| ---------------- | --------------------------- | ----------------------------------- |
| Operates on      | `boolean` values            | Integer bits                        |
| Short-circuiting | Yes (for `&&`, `\|\|`)      | No — always evaluates both operands |
| Result type      | `boolean`                   | Integer (same type as operands)     |
| Typical use      | Combining conditions        | Bit manipulation, flags             |

**Pre vs Post Increment**

| Aspect                   | Pre-increment (`++x`)                    | Post-increment (`x++`)                                               |
| ------------------------ | ---------------------------------------- | -------------------------------------------------------------------- |
| Order                    | Increments first, then uses value        | Uses value first, then increments                                    |
| Value used in expression | Updated value                            | Original (pre-update) value                                          |
| Common use               | When updated value is needed immediately | When original value is needed before updating (e.g., array indexing) |

**`==` vs `.equals()`**

| Aspect     | `==`                                                     | `.equals()`                                  |
| ---------- | -------------------------------------------------------- | -------------------------------------------- |
| Applies to | Primitives and object references                         | Objects only                                 |
| Compares   | Actual values (primitives) or memory reference (objects) | Logical/content equality (can be overridden) |
| Example    | `str1 == str2` (reference check)                         | `str1.equals(str2)` (content check)          |

**`&&` vs `&`**

| Aspect           | `&&`                            | `&`                             |
| ---------------- | ------------------------------- | ------------------------------- |
| Short-circuits   | Yes                             | No                              |
| Applicable types | `boolean` only                  | `boolean` and integer (bitwise) |
| Performance      | Can skip unnecessary evaluation | Always evaluates both sides     |

**`||` vs `|`**

| Aspect           | `\|\|`                            | `\|`                                            |
| ---------------- | --------------------------------- | ----------------------------------------------- |
| Short-circuits   | Yes                               | No                                              |
| Applicable types | `boolean` only                    | `boolean` and integer (bitwise)                 |
| Use case         | Conditional logic with early exit | Combining flags/bits or forcing full evaluation |

**`<<` vs `>>`**

| Aspect    | `<<` (Left Shift)                 | `>>` (Signed Right Shift)                       |
| --------- | --------------------------------- | ----------------------------------------------- |
| Direction | Shifts bits left                  | Shifts bits right                               |
| Fill bit  | Always `0`                        | Sign bit (0 for positive, 1 for negative)       |
| Effect    | Roughly multiplies by 2 per shift | Roughly divides by 2 per shift, preserving sign |

**`>>` vs `>>>`**

| Aspect                     | `>>` (Signed)                  | `>>>` (Unsigned)                                 |
| -------------------------- | ------------------------------ | ------------------------------------------------ |
| Fill bit                   | Sign bit preserved             | Always `0`                                       |
| Effect on negative numbers | Stays negative                 | Can become a large positive number               |
| Use case                   | General arithmetic right shift | Manipulating raw bit patterns regardless of sign |

---

## 37. Common Errors

**Compilation Errors:**

```java
if (x = 5) { }              // Error: incompatible types — int cannot be used as boolean condition
boolean b = 5 & true;       // Error: & cannot mix int and boolean
int result = 5 / ;           // Error: missing operand
```

**Runtime Errors:**

```java
int result = 10 / 0;         // ArithmeticException: / by zero
int[] arr = new int[5];
System.out.println(arr[10]); // ArrayIndexOutOfBoundsException (unrelated to operators directly, but often triggered by operator-driven index calculations)
```

**Logic Errors:**

```java
double avg = total / count;              // Wrong if total and count are both int — truncates before assignment
double avgCorrect = (double) total / count;  // Correct

int x = 5;
System.out.println(x++ == 5 && x++ == 6);  // Logic can be confusing due to short-circuit combined with side effects
```
