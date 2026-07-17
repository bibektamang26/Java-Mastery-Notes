# 1.3 Java Editions

As Java became popular, it was used to develop different types of applications. However, not all applications have the same requirements. A desktop application, a banking system, and a smartwatch application require different libraries and tools.

To meet these different needs, Java was divided into different **editions**. Each edition is designed for a specific type of application while sharing the same core Java language.

The three main Java editions are:

- Java SE (Standard Edition)
- Java EE (Enterprise Edition)
- Java ME (Micro Edition)

---

# 1. Java SE (Standard Edition)

**Java SE (Standard Edition)** is the core edition of Java and serves as the foundation for all other Java editions. It provides the essential libraries, tools, and APIs required for developing general-purpose Java applications.

If you are learning Java for the first time, you are learning **Java SE**.

## Features

- Core Java programming
- Object-Oriented Programming
- Collections Framework
- Exception Handling
- Multithreading
- File Handling
- JDBC
- Networking
- Swing and JavaFX for GUI development

## Applications

Java SE is commonly used for:

- Desktop Applications
- Console Applications
- Utility Software
- Educational Projects
- Small Business Applications

## Examples

- Calculator
- Student Management System
- Library Management System
- Inventory Management System

---

# 2. Java EE (Enterprise Edition)

**Java EE (Enterprise Edition)** is built on top of Java SE and is designed for developing **large-scale enterprise applications**.

Enterprise applications are software systems used by organizations, businesses, banks, hospitals, airlines, and government offices. These applications often need to support thousands or even millions of users simultaneously.

Java EE provides additional APIs and technologies to simplify enterprise application development.

> **Note:** Java EE is now known as **Jakarta EE** after being transferred from Oracle to the Eclipse Foundation.

## Features

- Web Application Development
- Enterprise Application Development
- Distributed Computing
- Web Services
- Security
- Transaction Management
- Dependency Injection

## Common Technologies

- Servlets
- JavaServer Pages (JSP)
- Enterprise JavaBeans (EJB)
- Java Persistence API (JPA)
- RESTful Web Services

## Applications

Java EE is widely used in:

- Banking Systems
- Hospital Management Systems
- Airline Reservation Systems
- Government Portals
- E-commerce Websites

## Examples

- Online Banking
- Amazon-like Shopping Systems
- Hospital Information Systems
- University Management Systems

---

# 3. Java ME (Micro Edition)

**Java ME (Micro Edition)** is a lightweight edition of Java designed for devices with limited memory, processing power, and storage.

It was mainly developed for embedded systems and mobile devices.

Although Android does not use Java ME, this edition is still used in many embedded devices.

## Features

- Lightweight Libraries
- Small Memory Footprint
- Optimized Performance
- Device-Specific APIs

## Applications

Java ME is commonly used in:

- Smart Cards
- Embedded Systems
- GPS Devices
- Industrial Controllers
- Home Automation Devices
- IoT (Internet of Things)

## Examples

- ATM Machines
- Smart Electricity Meters
- POS Machines
- Vehicle Tracking Systems

---

# Comparison of Java Editions

| Feature      | Java SE                     | Java EE (Jakarta EE)          | Java ME                   |
| ------------ | --------------------------- | ----------------------------- | ------------------------- |
| Full Name    | Standard Edition            | Enterprise Edition            | Micro Edition             |
| Purpose      | General-purpose programming | Enterprise & Web Applications | Embedded & Mobile Devices |
| Target Users | Individual Developers       | Large Organizations           | Device Manufacturers      |
| Applications | Desktop, Console            | Banking, Web, Enterprise      | Embedded Systems, IoT     |
| Based On     | Core Java                   | Java SE                       | Java SE (subset)          |
| Complexity   | Easy                        | Advanced                      | Lightweight               |

---

# Which Edition Should You Learn?

As a beginner, you should always start with **Java SE** because it teaches the core concepts of Java programming.

Once you have a strong understanding of Java SE, you can move on to:

- **Jakarta EE (Java EE)** for enterprise and web application development.
- **Spring Boot** for modern backend development.
- **Java ME** if you are interested in embedded systems and IoT.

---

# Key Points

- Java is divided into different editions to meet different application requirements.
- Java SE is the foundation of all Java editions.
- Java EE (Jakarta EE) is used for enterprise and web applications.
- Java ME is designed for embedded and resource-constrained devices.
- Beginners should first master Java SE before learning other Java technologies.

---

# Summary

## Java Editions allow developers to choose the right platform based on the type of application they want to build. Java SE provides the core language and libraries for general programming. Java EE (Jakarta EE) extends Java SE with technologies for enterprise and web applications, while Java ME offers a lightweight platform for embedded systems and IoT devices. Understanding these editions helps developers select the appropriate Java platform for their software projects.

                   Learn Java
                       │
                       ▼
                Java SE (Core Java)
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼

Jakarta EE / Spring Boot Java ME
(Web & Enterprise Apps) (Embedded & IoT)

---
