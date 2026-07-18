# 2.4 Identifiers in Java

## Introduction

Every programming language provides a way to name different elements of a program. In Java, these names are called **identifiers**.

An **identifier** is the name given by the programmer to program elements such as variables, methods, classes, interfaces, packages, and objects. Identifiers make the code readable and meaningful by allowing programmers to refer to these elements using descriptive names instead of memory addresses or numbers.

For example, instead of storing a student's age in an unnamed memory location, we can create a variable named `age`. Similarly, we can create classes named `Student`, methods named `calculateSalary()`, or packages named `com.company.project`.

Choosing meaningful identifiers is an important programming practice because it improves code readability, maintainability, and collaboration among developers.

---

# What is an Identifier?

An **identifier** is a user-defined name used to identify a program element.

Examples:

```java
int age = 20;

String name = "Bibek";

class Student {

}

void display() {

}
```

Here,

- `age` → Variable Identifier
- `name` → Variable Identifier
- `Student` → Class Identifier
- `display` → Method Identifier

---

# Where are Identifiers Used?

Identifiers are used to name:

- Variables
- Constants
- Classes
- Objects
- Methods
- Interfaces
- Packages
- Labels
- Enum names

Example:

```java
package com.company;

public class Student {

    int age;

    void display() {

        System.out.println(age);

    }

}
```

---

# Rules for Naming Identifiers

Java has specific rules that every identifier must follow.

---

## Rule 1: Must Begin with a Letter, Dollar Sign (`$`), or Underscore (`_`)

Correct

```java
name

_age

$salary
```

Incorrect

```java
1name

9student
```

Identifiers cannot start with a digit.

---

## Rule 2: Remaining Characters May Include Letters, Digits, `_`, and `$`

Correct

```java
student1

rollNumber2

emp_01

price$1
```

Incorrect

```java
student-number

student#1
```

Special characters such as `-`, `#`, `@`, `%`, and `&` are not allowed.

---

## Rule 3: Cannot Use Java Keywords

Keywords are reserved words with predefined meanings.

Incorrect

```java
int class = 10;

int public = 20;

int while = 30;
```

These produce compilation errors.

---

## Rule 4: Cannot Contain Spaces

Correct

```java
studentName
```

Incorrect

```java
student name
```

Use **camelCase** instead of spaces.

---

## Rule 5: Java is Case Sensitive

Java treats uppercase and lowercase letters as different.

Example:

```java
age

Age

AGE
```

These are three different identifiers.

---

## Rule 6: No Length Limit

Identifiers can be very long.

Example:

```java
numberOfStudentsEnrolledInSecondSemester
```

However, excessively long names reduce readability.

---

# Valid Identifiers

```java
name

studentName

student_age

$price

_rollNo

marks2026

Employee
```

---

# Invalid Identifiers

```java
2marks

student-name

student name

public

while

total%
```

---

# Naming Conventions in Java

Although Java only enforces the rules above, programmers follow standard naming conventions to improve readability and maintain consistency.

---

## Variables

Use **camelCase**.

```java
studentName

totalMarks

monthlySalary
```

---

## Methods

Use **camelCase** beginning with a verb.

```java
calculateTotal()

displayResult()

printDetails()
```

---

## Classes

Use **PascalCase**.

```java
Student

Employee

BankAccount
```

---

## Packages

Use **lowercase letters**.

```java
com.company.project

java.util

oop.notes
```

---

## Constants

Use **UPPER_CASE** with underscores.

```java
PI

MAX_SIZE

DEFAULT_TIMEOUT
```

---

# Examples

## Example 1

```java
public class Student {

    public static void main(String[] args) {

        String studentName = "Bibek";

        int age = 20;

        System.out.println(studentName);

        System.out.println(age);

    }

}
```

Identifiers used:

- Student
- main
- args
- studentName
- age
- System
- out
- println

---

## Example 2

```java
double monthlySalary = 25000;

double yearlySalary = monthlySalary * 12;
```

Identifiers:

- monthlySalary
- yearlySalary

---

# Why Meaningful Identifiers are Important

Compare the following programs.

Poor Naming

```java
int a = 25;

int b = 10;

int c = a + b;
```

Good Naming

```java
int mathematicsMarks = 25;

int scienceMarks = 10;

int totalMarks = mathematicsMarks + scienceMarks;
```

The second program is much easier to understand.

Meaningful identifiers improve code quality.

---

# Common Mistakes

- Starting an identifier with a number.
- Using spaces.
- Using Java keywords.
- Using special characters.
- Ignoring Java naming conventions.
- Creating meaningless names such as `x1`, `abc`, or `temp123` without purpose.

---

# Try It Yourself

Determine whether the following identifiers are valid or invalid.

| Identifier   | Valid / Invalid |
| ------------ | --------------- |
| studentName  | ?               |
| 2marks       | ?               |
| total_marks  | ?               |
| while        | ?               |
| Employee     | ?               |
| student-name | ?               |
| MAX_SIZE     | ?               |
| \_count      | ?               |
| $salary      | ?               |
| student name | ?               |

---

# Important Exam Questions

## Short Questions

1. What is an identifier?
2. State any four rules for naming identifiers.
3. Why are Java identifiers case-sensitive?
4. Can an identifier begin with a digit? Explain.
5. What is the difference between an identifier and a keyword?

---

## Long Questions

1. Explain identifiers in Java with suitable examples.
2. Write the rules and naming conventions for identifiers.
3. Differentiate between valid and invalid identifiers.

---

# Viva Questions

- Can an identifier contain numbers?
- Can an identifier start with `_`?
- Why can't we use keywords as identifiers?
- Is `Student` the same as `student`?
- Which naming convention is used for class names?

---

# Key Points

- Identifiers are user-defined names.
- They are used to name variables, classes, methods, objects, packages, and other program elements.
- Identifiers cannot begin with digits.
- Keywords cannot be used as identifiers.
- Java is case-sensitive.
- Use meaningful names and follow Java naming conventions.

---

# Summary

Identifiers are user-defined names assigned to various elements of a Java program. They improve the readability and organization of code by giving meaningful names to variables, methods, classes, packages, and other program components. Java defines specific rules for creating valid identifiers, and programmers follow standard naming conventions to write clean, maintainable, and professional code.
