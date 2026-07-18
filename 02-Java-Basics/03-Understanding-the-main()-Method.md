# 2.3 Understanding the `main()` Method

## Introduction

Every Java application begins its execution from a special method called the **`main()` method**. It is known as the **entry point** of a Java program because the Java Virtual Machine (JVM) always starts execution from this method.

When you run a Java program, the JVM first searches for the `main()` method. If it finds the method with the correct signature, it starts executing the statements inside it. If it cannot find the method, the program cannot run.

The syntax of the `main()` method is:

```java
public static void main(String[] args) {

}
```

Although it appears to be a single statement, it is actually made up of several keywords and symbols. Each one has a specific purpose and meaning.

---

# Structure of the `main()` Method

```java
public static void main(String[] args) {

}
```

The components are:

1. `public`
2. `static`
3. `void`
4. `main`
5. `String[]`
6. `args`
7. Parentheses `()`
8. Curly Braces `{}`

We will study each one in detail.

---

# 1. `public`

```java
public
```

## Definition

`public` is an **access modifier**.

It specifies that the method can be accessed from anywhere in the program.

The JVM must be able to access the `main()` method to start program execution. Therefore, it is declared as `public`.

If the method is not public, the JVM cannot legally call it.

---

## What happens if `public` is removed?

Example

```java
static void main(String[] args)
```

The program compiles, but the JVM cannot recognize it as the application's entry point.

Depending on the Java version and launcher, you'll typically get an error indicating that the `main` method is not accessible or not found with the required signature.

---

## Key Points

- `public` is an access modifier.
- Allows the JVM to access the `main()` method.
- Required for the standard application entry point.

---

# 2. `static`

```java
static
```

## Definition

`static` means the method belongs to the **class itself**, not to an object of the class.

Normally, methods are called using objects.

Example:

```java
Student s = new Student();
s.display();
```

However, before creating any object, the JVM must start the program.

If `main()` were not static, the JVM would first have to create an object to call it—which would create a circular problem because the program hasn't started yet.

Making `main()` static solves this problem.

---

## Key Points

- Belongs to the class.
- No object is required.
- The JVM calls it directly.

---

# 3. `void`

```java
void
```

## Definition

`void` is the **return type** of the method.

It tells the compiler that the `main()` method does **not return any value**.

Some methods return values.

Example:

```java
int add() {

    return 10;

}
```

The `main()` method simply performs its work and finishes.

Therefore, its return type is `void`.

---

## Key Points

- Indicates no return value.
- Required in the standard `main()` method.

---

# 4. `main`

```java
main
```

## Definition

`main` is the **name of the method**.

The JVM specifically looks for a method named `main` with the correct signature.

If you rename it, the JVM will not treat it as the program's starting point.

Example

```java
public static void start(String[] args)
```

This method is valid Java, but it is **not** the entry point of the application.

---

## Key Points

- `main` is the predefined entry-point method name.
- It must be spelled exactly as `main`.
- Java is case-sensitive (`Main` and `main` are different).

---

# 5. `String[]`

```java
String[]
```

## Definition

`String[]` means **an array of String objects**.

It allows command-line arguments to be passed to the program when it starts.

For example:

```bash
java Hello Alice 20
```

Then:

- `args[0]` → `"Alice"`
- `args[1]` → `"20"`

If no command-line arguments are supplied, the array is simply empty.

---

## Why is `String` Capitalized?

`String` is **not a primitive data type**.

It is a predefined class in the Java Standard Library.

Class names in Java begin with a capital letter by convention.

---

## Key Points

- `String[]` is an array of strings.
- Stores command-line arguments.
- Can be empty if no arguments are provided.

---

# 6. `args`

```java
args
```

## Definition

`args` is the **parameter name**.

It stores the array of command-line arguments.

The name `args` is only a convention.

You may use any valid identifier.

Example

```java
public static void main(String[] names)
```

This works exactly the same because the JVM only cares about the method signature, not the parameter name.

---

## Key Points

- Holds command-line arguments.
- The name can be changed.
- `args` is the common convention.

---

# 7. Parentheses `()`

```java
()
```

Parentheses indicate that `main` is a **method**.

They also contain the method's parameters.

Every Java method uses parentheses.

---

# 8. Curly Braces `{}`

```java
{

}
```

Curly braces define the body of the method.

Every statement that belongs to the `main()` method is written inside these braces.

When execution starts, the JVM begins executing the statements inside this block from top to bottom.

---

# Complete Breakdown

```java
public static void main(String[] args)
│      │      │      │        │
│      │      │      │        └── Parameter Name
│      │      │      └────────── Array of Strings
│      │      └───────────────── Method Name
│      └──────────────────────── Static Keyword
└─────────────────────────────── Access Modifier
```

---

# How the JVM Uses the `main()` Method

```
Run Program

      │

      ▼

JVM Starts

      │

      ▼

Searches for

public static void main(String[] args)

      │

      ▼

Method Found?

   │          │

  Yes        No

   │          │

   ▼          ▼

Execute    Error
Program
```

---

# Common Mistakes

- Writing `Main()` instead of `main()`.
- Removing `static`.
- Using `Void` instead of `void`.
- Forgetting the parameter list.
- Thinking `args` is a keyword.
- Assuming `String` is a primitive data type.

---

# Try It Yourself

1. Change the parameter name from `args` to `numbers`.
2. Try writing `Main()` instead of `main()`.
3. Remove `static` and observe the error.
4. Replace `void` with `int` and see what happens.
5. Print the first command-line argument using `args[0]`.

---

# Important Exam Questions

## Short Questions

1. Why is the `main()` method called the entry point of a Java program?
2. What is the purpose of the `static` keyword?
3. Why is the return type of `main()` `void`?
4. What is the role of `String[] args`?
5. Can the name `args` be changed?

---

## Long Questions

1. Explain the structure of the `main()` method.
2. Describe the purpose of each keyword used in the `main()` method.
3. Explain how the JVM starts executing a Java program.

---

# Viva Questions

- Who calls the `main()` method?
- Why must `main()` be `public`?
- Can `main()` return a value?
- Is `String` a keyword?
- Can there be multiple `main()` methods?

---

# Key Points

- `main()` is the entry point of every Java application.
- The JVM starts execution from the `main()` method.
- `public` allows the JVM to access the method.
- `static` allows the JVM to call the method without creating an object.
- `void` indicates that the method returns no value.
- `String[] args` stores command-line arguments.
- The parameter name `args` can be changed.

---

# Summary

The `main()` method is the starting point of every Java application. It has a predefined structure that the JVM recognizes and uses to begin execution. Understanding each component of the method—`public`, `static`, `void`, `main`, and `String[] args`—helps build a strong foundation for learning Java programming and prepares you for writing more complex applications.
