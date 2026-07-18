# 2.5 Keywords in Java

## Introduction

A programming language consists of many words that have predefined meanings. These words are known as **keywords** or **reserved words**. Java keywords are reserved by the Java language and cannot be used as identifiers such as variable names, method names, class names, or package names.

Keywords help define the structure and behavior of a Java program. They instruct the compiler to perform specific tasks, such as declaring variables, creating classes, controlling program flow, handling exceptions, or implementing object-oriented programming concepts.

For example, words like `class`, `public`, `if`, `for`, and `return` are Java keywords because they have special meanings that the compiler understands.

Understanding Java keywords is essential because almost every Java program uses them.

---

# What are Keywords?

A **keyword** is a reserved word that has a predefined meaning in the Java programming language.

The Java compiler recognizes these words and treats them specially.

Example:

```java
public class Student {

    public static void main(String[] args) {

        int age = 20;

        if(age >= 18){

            System.out.println("Adult");

        }

    }

}
```

In the above program, the following are Java keywords:

- public
- class
- static
- void
- int
- if

Words like `Student` and `age` are **identifiers**, not keywords.

---

# Characteristics of Java Keywords

Java keywords have the following characteristics:

- They have predefined meanings.
- They cannot be used as identifiers.
- They are written in lowercase (except `true`, `false`, and `null`, which are literals, not keywords).
- Their meaning cannot be changed.
- They are recognized directly by the Java compiler.

---

# Java Keywords

Modern Java contains **50 reserved keywords** (depending on the Java version, some are restricted or unused). Below are the most commonly used keywords, grouped by purpose.

---

# 1. Access Modifier Keywords

These keywords control the accessibility of classes, methods, and variables.

| Keyword   | Purpose                                      |
| --------- | -------------------------------------------- |
| public    | Accessible from anywhere                     |
| private   | Accessible only within the same class        |
| protected | Accessible within the package and subclasses |

Example:

```java
public class Student {

    private int age;

}
```

---

# 2. Class and Object Keywords

These keywords are used in Object-Oriented Programming.

| Keyword    | Purpose                      |
| ---------- | ---------------------------- |
| class      | Declares a class             |
| new        | Creates an object            |
| this       | Refers to the current object |
| super      | Refers to the parent class   |
| extends    | Creates inheritance          |
| implements | Implements an interface      |
| interface  | Declares an interface        |
| instanceof | Checks object type           |

Example:

```java
Student s = new Student();
```

---

# 3. Primitive Data Type Keywords

These keywords represent Java's built-in data types.

| Keyword | Data Type       |
| ------- | --------------- |
| byte    | Integer         |
| short   | Integer         |
| int     | Integer         |
| long    | Integer         |
| float   | Decimal         |
| double  | Decimal         |
| char    | Character       |
| boolean | True/False      |
| void    | No return value |

Example:

```java
int age = 20;

double salary = 25000.50;

char grade = 'A';
```

---

# 4. Decision-Making Keywords

Used to make decisions in a program.

| Keyword | Purpose                            |
| ------- | ---------------------------------- |
| if      | Executes code based on a condition |
| else    | Executes alternative code          |
| switch  | Selects one option                 |
| case    | Represents a switch option         |
| default | Default switch option              |

Example:

```java
if(age >= 18){

    System.out.println("Eligible");

}else{

    System.out.println("Not Eligible");

}
```

---

# 5. Loop Keywords

Used for repetition.

| Keyword  | Purpose                     |
| -------- | --------------------------- |
| for      | Loop                        |
| while    | Loop                        |
| do       | Executes loop at least once |
| break    | Terminates a loop           |
| continue | Skips the current iteration |

Example:

```java
for(int i = 1; i <= 5; i++){

    System.out.println(i);

}
```

---

# 6. Exception Handling Keywords

Used to detect and handle runtime errors.

| Keyword | Purpose                      |
| ------- | ---------------------------- |
| try     | Defines code to monitor      |
| catch   | Handles exceptions           |
| finally | Executes after try/catch     |
| throw   | Throws an exception          |
| throws  | Declares possible exceptions |

Example:

```java
try{

    int a = 10 / 0;

}catch(Exception e){

    System.out.println(e);

}
```

---

# 7. Package Keywords

Used to organize Java programs.

| Keyword | Purpose                              |
| ------- | ------------------------------------ |
| package | Declares a package                   |
| import  | Imports classes from another package |

Example:

```java
import java.util.Scanner;
```

---

# 8. Method Keywords

Used while declaring methods.

| Keyword      | Purpose                                |
| ------------ | -------------------------------------- |
| static       | Belongs to the class                   |
| final        | Cannot be changed or overridden        |
| abstract     | Declares an incomplete method or class |
| native       | Calls native code                      |
| synchronized | Thread synchronization                 |

---

# 9. Miscellaneous Keywords

| Keyword   | Purpose                                     |
| --------- | ------------------------------------------- |
| return    | Returns a value from a method               |
| enum      | Declares an enumeration                     |
| assert    | Used for debugging                          |
| transient | Prevents serialization                      |
| volatile  | Ensures variable visibility between threads |
| strictfp  | Controls floating-point calculations        |

---

# Keywords vs Identifiers

| Keywords                         | Identifiers                                    |
| -------------------------------- | ---------------------------------------------- |
| Reserved words                   | User-defined names                             |
| Have predefined meanings         | Programmer chooses them                        |
| Cannot be used as variable names | Used to name variables, methods, classes, etc. |
| Examples: `class`, `if`, `while` | Examples: `Student`, `age`, `salary`           |

Example:

```java
int rollNumber = 20;      // Correct

int while = 20;    // Incorrect
```

---

# Common Mistakes

- Using keywords as variable names.
- Confusing keywords with identifiers.
- Writing keywords in uppercase (`Public` instead of `public`).
- Assuming `true`, `false`, and `null` are keywords. (They are reserved literals.)

---

# Memory Tip

Instead of memorizing all keywords randomly, remember them by category:

```
Access
↓

public
private
protected

↓

Classes

class
new
this
super

↓

Data Types

int
double
char
boolean

↓

Decision

if
else
switch

↓

Loops

for
while
do

↓

Exceptions

try
catch
throw
finally
```

Learning keywords by category is much easier than memorizing a long list.

---

# Try It Yourself

Determine whether the following are keywords or identifiers.

| Word    | Keyword or Identifier? |
| ------- | ---------------------- |
| class   | ?                      |
| Student | ?                      |
| public  | ?                      |
| age     | ?                      |
| while   | ?                      |
| marks   | ?                      |
| static  | ?                      |
| salary  | ?                      |

---

# Interview Corner

### Q1. Can we use a Java keyword as a variable name?

No. Keywords are reserved by the Java language and cannot be used as identifiers.

---

### Q2. Are `true`, `false`, and `null` keywords?

No. They are **reserved literals**, not keywords.

---

### Q3. Why are keywords reserved?

Because the Java compiler uses them to recognize the syntax and structure of the language.

---

### Q4. Is `String` a keyword?

No.

`String` is a predefined class in the Java Standard Library.

---

### Q5. Can we create our own keyword?

No.

The set of Java keywords is fixed by the language specification.

---

# Important Exam Questions

## Short Questions

1. What is a keyword?
2. Why are keywords called reserved words?
3. Can keywords be used as identifiers? Explain.
4. Differentiate between keywords and identifiers.
5. What is the purpose of the `static` keyword?

---

## Long Questions

1. Explain Java keywords with suitable examples.
2. Differentiate between keywords and identifiers.
3. Explain different categories of Java keywords.

---

# Viva Questions

- How many keywords are there in Java?
- Is `String` a keyword?
- Is `main` a keyword?
- Is `System` a keyword?
- Can we create our own keyword?

---

# Key Points

- Keywords are reserved words with predefined meanings.
- Keywords cannot be used as identifiers.
- Java keywords define the structure and behavior of programs.
- Learning keywords by category is easier than memorizing them individually.
- `String`, `main`, `System`, `true`, `false`, and `null` are **not** Java keywords.

---

# Summary

Keywords are reserved words that form the foundation of Java programming. They provide instructions to the compiler for defining classes, variables, methods, control structures, exception handling, object-oriented programming concepts, and many other language features. Since keywords have predefined meanings, they cannot be used as identifiers. Understanding Java keywords is essential for writing correct, readable, and efficient Java programs.
