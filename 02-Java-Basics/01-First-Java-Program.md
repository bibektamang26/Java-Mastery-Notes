# 2.1 Writing Your First Java Program

## Introduction

Every programmer begins learning a programming language by writing a simple program known as the **Hello World Program**. Although the program is very small, it introduces the basic structure of a Java program and demonstrates how Java code is written, compiled, and executed.

The purpose of this program is not only to display the text **"Hello, World!"** on the screen but also to understand the syntax and structure of a Java program. Every Java application, whether it is a small calculator or a large banking system, follows the same basic structure.

In this chapter, we will write our first Java program and understand the purpose of every keyword, symbol, and statement used in it.

---

# Hello World Program

```java
public class HelloWorld {

    public static void main(String[] args) {

        System.out.println("Hello, World!");

    }

}
```

---

# Program Output

```
Hello, World!
```

---

# Understanding the Program

At first glance, the program may look confusing because it contains many keywords and symbols. However, every part of the program has a specific purpose.

We will now study each part of the program one by one.

---

# Step 1: The `public` Keyword

```java
public
```

The keyword **public** is an **access modifier** in Java.

It specifies the accessibility of a class, method, or variable.

When a class or method is declared as **public**, it means that it can be accessed from anywhere within the program and even from other packages.

In the Hello World program, the class is declared as public because the Java Virtual Machine (JVM) must be able to access it when executing the program.

### Syntax

```java
public class ClassName
```

### Key Points

- `public` is an access modifier.
- It allows access from anywhere.
- The JVM can access a public class or method.

---

# Step 2: The `class` Keyword

```java
class
```

The keyword **class** is used to define a class in Java.

A class is a blueprint or template for creating objects. It groups together related data (attributes) and behaviors (methods).

Every Java program must contain at least one class because Java is an object-oriented programming language.

### Syntax

```java
class ClassName {

}
```

### Key Points

- A class is a blueprint for objects.
- Every Java program contains one or more classes.
- Java code is written inside a class.

---

# Step 3: Class Name

```java
HelloWorld
```

**HelloWorld** is the name of the class.

In Java, class names should begin with a capital letter and follow the **PascalCase** naming convention.

If the class is declared as `public`, the filename **must exactly match** the class name.

For example,

```java
public class HelloWorld
```

must be saved as

```
HelloWorld.java
```

Otherwise, the compiler will generate an error.

### Naming Rules

- Begin with a letter.
- Use meaningful names.
- Follow PascalCase.
- Filename and public class name must be identical.

---

# Step 4: Curly Braces `{ }`

```java
{

}
```

Curly braces define the beginning and end of a block of code.

Everything inside the class braces belongs to that class.

Similarly, everything inside the method braces belongs to that method.

Blocks improve code organization and readability.

---

# Step 5: The `main()` Method

```java
public static void main(String[] args)
```

The **main()** method is the **entry point** of every Java application.

Whenever a Java program is executed, the JVM first looks for the `main()` method.

Execution always begins from this method.

If the JVM cannot find the `main()` method, the program cannot run.

In the following sections, we will study each keyword (`static`, `void`, `String[] args`) in detail.

---

# Step 6: Printing Output

```java
System.out.println("Hello, World!");
```

This statement displays text on the console.

It consists of three important parts:

- `System` → A predefined Java class.
- `out` → A standard output stream.
- `println()` → A method used to print text followed by a new line.

The text inside double quotation marks is called a **String Literal**.

---

# Step 7: Semicolon (`;`)

Every Java statement ends with a semicolon (`;`).

The semicolon tells the compiler that one statement has ended and the next statement can begin.

Forgetting a semicolon results in a **compile-time error**.

Example:

```java
System.out.println("Hello")
```

Output:

```
error: ';' expected
```

---

# How the Program Executes

The following steps occur when you run the program:

1. Write the source code in a file named `HelloWorld.java`.
2. Save the file.
3. Compile it using the Java Compiler (`javac`).
4. The compiler generates `HelloWorld.class`.
5. The JVM loads the class.
6. The JVM calls the `main()` method.
7. The statement `System.out.println()` executes.
8. The output appears on the screen.

---

# Common Mistakes

- Forgetting the semicolon (`;`).
- Writing `Main()` instead of `main()`.
- Saving the file with a different name than the public class.
- Misspelling `System`.
- Misspelling `println()`.
- Forgetting the opening or closing braces.

---

# Try It Yourself

## Program 1

Print your name.

```java
public class MyName {

    public static void main(String[] args) {

        System.out.println("Bibek");

    }

}
```

---

## Program 2

Print your college name.

---

## Program 3

Print your address.

---

## Program 4

Print three lines using `println()`.

---

## Program 5

Print two words on the same line using `print()`.

---

# Important Exam Questions

## Short Questions

1. What is the purpose of the `main()` method?
2. Why is Java called an object-oriented language?
3. What is the purpose of `System.out.println()`?
4. Why must the filename match the public class name?
5. What is a String literal?

---

## Long Questions

1. Write and explain the structure of a Java program.
2. Explain every component of the Hello World program.
3. Describe how a Java program is compiled and executed.

---

# Viva Questions

- Why is the `main()` method called the entry point of a Java program?
- Can a Java program contain more than one class?
- Can a Java file contain more than one `main()` method?
- What happens if the `main()` method is missing?
- What is the difference between `print()` and `println()`?

---

# Key Points

- Every Java application starts from the `main()` method.
- Java code is written inside a class.
- Statements end with a semicolon.
- `System.out.println()` is used to display output.
- The filename and the public class name must be identical.

---

# Summary

The Hello World program introduces the basic structure of a Java application. It demonstrates how Java code is organized into classes and methods and how the JVM begins execution from the `main()` method. Understanding each component of this program is essential because every Java application, regardless of its size or complexity, follows the same fundamental structure.
