# 2.2 Java Program Structure

## Introduction

Every Java program follows a specific structure or format. Regardless of whether the program is a simple "Hello World" application or a large enterprise system containing thousands of classes, the basic structure remains the same.

Understanding the structure of a Java program is one of the most important steps in learning Java because every keyword, symbol, and statement has a specific meaning and purpose.

In this chapter, we will examine each component of a Java program in detail.

---

# Basic Structure of a Java Program

```java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello, World!");

    }

}
```

---

# Components of a Java Program

The basic Java program consists of the following components:

1. Comments
2. Package Declaration (Optional)
3. Import Statements (Optional)
4. Class Declaration
5. Main Method
6. Statements
7. Blocks
8. Output Statements

Each component plays an important role in the execution of a Java program.

---

# Structure Diagram

```
Java Program

│

├── Comments

├── Package

├── Import Statements

├── Class

│      └── Methods

│              └── Statements

└── Output
```

---

# 1. Comments

## Definition

Comments are lines of text that are ignored by the Java compiler.

They are used to explain the code, improve readability, and help programmers understand the purpose of different sections of a program.

Comments do not affect program execution.

---

## Types of Comments

### Single-Line Comment

Used for writing a single line of explanation.

```java
// This is a single-line comment
```

---

### Multi-Line Comment

Used when writing explanations spanning multiple lines.

```java
/*
This is
a multi-line
comment.
*/
```

---

### Documentation Comment

Used to generate API documentation using the **Javadoc** tool.

```java
/**
 * This is a documentation comment.
 */
```

---

## Why Comments are Important

- Explain code.
- Improve readability.
- Help teamwork.
- Simplify debugging.
- Generate documentation.

---

# 2. Package Declaration

## Definition

A package is used to organize Java classes into groups.

It is similar to folders on a computer.

Packages help avoid naming conflicts and improve project organization.

Example:

```java
package com.company.project;
```

---

## Rules

- Package declaration is optional.
- It must be the first statement in the program.
- Only one package declaration is allowed in a Java source file.

---

# 3. Import Statement

## Definition

The `import` statement allows a Java program to use classes from other packages.

Instead of writing the complete package name every time, we import the required class.

Example:

```java
import java.util.Scanner;
```

Now we can simply write:

```java
Scanner input = new Scanner(System.in);
```

instead of

```java
java.util.Scanner input = new java.util.Scanner(System.in);
```

---

## Why Import?

Without import statements, programmers would need to write long package names repeatedly.

Importing improves readability and simplifies coding.

---

# 4. Class Declaration

A Java program is built using classes.

Example:

```java
public class Student
```

Here,

- `public` → Access modifier
- `class` → Keyword
- `Student` → Class Name

A class acts as a blueprint for creating objects.

---

# Class Naming Rules

- Begins with a letter.
- Cannot contain spaces.
- Cannot use Java keywords.
- Should follow PascalCase.
- Public class name must match the filename.

Correct

```java
Student
```

Incorrect

```java
student details
```

---

# 5. Main Method

The `main()` method is the starting point of every Java application.

```java
public static void main(String[] args)
```

Whenever a Java program runs, the JVM first searches for this method.

Execution begins from this method.

If the JVM cannot find it, the program cannot start.

---

# Components of the Main Method

```
public

static

void

main

(String[] args)
```

We will study each of these keywords in detail in the following sections.

---

# 6. Statements

A statement is an instruction given to the computer.

Example:

```java
System.out.println("Hello");
```

Every statement performs a specific task.

---

# Statement Termination

Every Java statement ends with a semicolon (`;`).

Example:

```java
int age = 20;
```

The semicolon tells the compiler that the statement has ended.

---

# 7. Blocks

A block is a group of statements enclosed within curly braces.

Example:

```java
{

}
```

Blocks define the scope of classes, methods, loops, and conditional statements.

---

# Nested Blocks

Java allows blocks inside other blocks.

Example:

```java
public class Test {

    public static void main(String[] args) {

        if(true){

            System.out.println("Hello");

        }

    }

}
```

---

# 8. Output Statements

Java provides several methods for displaying output.

## print()

Prints text without moving to the next line.

```java
System.out.print("Hello");
System.out.print("World");
```

Output

```
HelloWorld
```

---

## println()

Prints text and moves the cursor to the next line.

```java
System.out.println("Hello");
System.out.println("World");
```

Output

```
Hello
World
```

---

## printf()

Prints formatted output.

Example:

```java
System.out.printf("Age = %d",20);
```

Output

```
Age = 20
```

---

# Complete Structure of a Java Program

```
Comments

↓

Package

↓

Import Statements

↓

Class Declaration

↓

Main Method

↓

Statements

↓

Output
```

---

# Common Mistakes

- Forgetting the semicolon.
- Using an incorrect class name.
- Saving the file with a different name.
- Writing package declaration after imports.
- Forgetting curly braces.
- Misspelling the main method.

---

# Try It Yourself

### Program 1

Write a Java program that prints your name.

---

### Program 2

Write a Java program using both `print()` and `println()`.

---

### Program 3

Write a Java program containing all three types of comments.

---

### Program 4

Import the `Scanner` class and create an object.

---

# Important Exam Questions

## Short Questions

1. What is the structure of a Java program?
2. What is a package?
3. What is an import statement?
4. What are comments?
5. What is the purpose of the main() method?

---

## Long Questions

1. Explain the structure of a Java program with a neat diagram.
2. Explain the different types of comments in Java.
3. Differentiate between `print()`, `println()`, and `printf()`.
4. Explain the purpose of packages and import statements.

---

# Viva Questions

- Why do we use packages?
- What happens if we remove the main() method?
- Why is import required?
- Can a Java program run without comments?
- Why do statements end with a semicolon?

---

# Key Points

- Every Java program follows a standard structure.
- Comments improve readability but are ignored by the compiler.
- Packages organize related classes.
- Import statements allow the use of classes from other packages.
- Execution begins from the main() method.
- Statements end with a semicolon.
- Blocks are enclosed within curly braces.

---

# Summary

A Java program consists of several well-defined components, including comments, packages, imports, classes, methods, statements, and output instructions. Understanding the role of each component is essential for writing correct, organized, and maintainable Java programs. This structure forms the foundation upon which all Java applications are built.
