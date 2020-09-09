---
title: Java Enum Types
tags:
  - Java
  - Enum
categories:
  - - Java
    - Enum
date: 2020-09-09 21:39:58
---


Excerpts from [Enum Types](https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html)

> An *enum type* is a special data type that enables for a variable to  be a set of predefined constants. The variable must be equal to one of the values that have been predefined for it.

Common examples: Compass directions(NORTH, SOUTH, EAST, and WEST) and the days of the week.

Because they are constants, the name of an enum type's field are in uppercase letters.

Java programming languague enum types are much more powerful than theirs conunterparts in other languages. The enum declaration defines a *class* (called and *enum type* which implicitly extends `java.lang.Enum`). The enum class body can include method and other fields.

The constructor for an enum type must be package-private or private access. It automatically creates the constants that are defined at the beginning of the enum body. You cannot invoke an enum constructor yourself.
