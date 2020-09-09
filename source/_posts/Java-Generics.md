---
title: Java Generics
date: 2020-08-28 22:22:14
tags:
- Java
- Generics
category:
- [Java, Generics]
---

## Generics

> Generics add stability to your code by making more of your bugs detectable at complie time.

### Benefits of using generics

- Stronger type checks at complie time.
  - A Java complier applies strong type ckecking to generic code and issues errors if the code violates type safety. Fixing complie-time erros is easier than fixing runtime errors, which can be difficult to find.

- Elimination of casts.
  - The following code snippet without generics requires casting:

  ```java
  List list = new ArrayList();
  list.add("hello");
  String s = (String) list.get(0);
  ```

  - When re-written to use generics, the code does not require casting:

  ```java
  List<String> list = new ArrayList<String>();
  list.add("hello");
  String s = list.get(0);   // no cast
  ```

  - Enabling programmers to implement generic algorithms.
    - By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.

## Generic Types

- Definition: A *generic type* is generic `class` or `interface` that is parameterized over types.

### Generic Class

- A *generic* class is defined with the following format:

  ```java
  class name<T1, T2, ..., Tn> { /* ... */ }
  ```

### Type Prameter Naming Conventions

- E: Element(used extensively by the Java Collections Framework)
- K: Key
- N: Number
- T: Type
- V: Value
- S, U, V etc.: 2nd, 3rd, 4th types

### Invoking and Instantiating a Generic Type

- By performing *generic type invocation* which replaces `T` with some concrerate value, such as `Integer`:

```java
Box<Integer> intergerBox;
```

- To instantiate this class, us the `new` keyword, as usual, but place `Integer` between the class name and the parenthesis:

```java
Box<Integer> integerBox = new Box<Integer>();
```

- Or, In Java SE 7 and later, you can replace the type argument required to
  invoke the constructor of a gneeric class with an empty set of type argument (<>) as long as the complier can determin, or infer, the type argument from the context.

  ```java
  Box<Integer> integerBox = new Box<>();
  ```

### Multiple Type Parameters

- A generic class can have multiple type parameters. For example, the generic `OrderedPair` class, which implements the generic `Pair` interface:

```java
public interface Pair<K, V> {
  public K getKey();
  public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {

  private K key;
  private V value;

  public OrderedPair(K key, V value) {
    this.key = key;
    this.value = value;
  }

  public K getKey() { return key; }
  public V getValue() { return value; }
}
```

- The following statements create two instantiations of the `OrderdPair` class:

```java
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String>  p2 = new OrderedPair<String, String>("hello", "world");
```

### Parameterized Types

- You can also substitute a type paramter (i.e. K or V) with a parametrized(i.e. `List<String>`). For example, using the `OrderedPair<K, V>` example:

```java
  OrderedPair<String, Box<Integer>> p = new OrderedPair<>("primes", new Box<Integer>(...));
```

### Raw Types

- A *raw type* is the name of a generic `class` or interface without any type arguments. For example, given the generic `Box` classes:

```java
public class Box<T> {
  public void set(T t) { /* ... */ }
  // ...
}
```

### Generic Methods

*Generic methods* are methods that introduce their own type parameters.
This is similar to declaring a generic type, but the type parameter's scope is limited to the method where it is declared. Static and non-static generic methods are allowed, as well as class constructors.

The syntax for a generic method includes a list of type parameters, inside angle brackets, which appears before the method's return type. For static generic methods, the type parameter section must appear before the method's return type.

The `Util` class includes a generic method, `compare`, which compares two `pair` objects:

```java
public class Util {
  public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
    return p1.getKey().equals(p2.getKey()) && p1.getValue().equals(p2.getValue());
  }
}

public class Pair<K, V> {

  private K key;
  private V value;

  public Pair(K key, V value) {
    this.key = key;
    this.value = value;
  }

  public void setKey(K key) { this.key = key; }
  public void setValue(V value) { this.value = value; }
  public K getKey()   { return key; }
  public V getValue() { return value; }
}
```

### Bounded Type Parameters

There may be times when you want to restrict the types that can be used as type arguments in a parameterized type. For example, a method that operates on numbers might only want to accept instances of Number or its subclasses.
This is what *bounded type parameters are for*.

To declare a bounded type parmeter, list hte type parameter's name. followed by the extends keyword, followed by its *upper bound*, which in this example is Number. Note that, in this context, `extends` is used in a general sense to mean either "extends" (as in classed) or "implements" (as in interfaces)

#### Multiple Bounds

The preceding example illustrates the use of a type parameter with a single bound, but a type parameter can have *multiple bounds*:

```java
<T extends B1 & B2 & B3>
```

If one of the bounds is class, it must be specified first. For example:

```java
class A { /* ... */}
interface B { /* ... */}
interface C { /* ... */ }

class D <T extends A & B & C> { /* ... */ }
```

If bound `A` is not specified first, you get a compile-time error.

### Wildcards

In generic code, the question mark (?), called the *wildcard*, represents an unknown type. The wildcard can be used in a variety of situations: as the type of a parameter, field, or local variable; sometimes as a return type. The wildcard is never used as a type argument for a generic method invocation, a generic class instance creation, or a supertype.

#### Unbounded Wildcards

The unbounded wildcard type is specified using the wildcards character (`?`), for example, List<?>, This is called *list of unknown type*. There are two senarios where an unbounded wildcard is a useful appoach:

- If you are writing a method that can be implemented using functionality provided in the `object` class.
- When the code is using methods in the generic class that don't depend on the type parameter.

#### Upperbounded Wildcards

To declare an upper-bounded wildcard, use the wildcard character (`?`), followed by the `extends` keyword, followed by its `upper bound`. Note that, in this context, `extends` is used in general senese to mean either "extends" (as in classes) or "implements" (as in interfaces).

#### LowerBounded Wildcards

A lower bounded wildcard is expressed using the wildcard character (`?`), follwing by the `super` keyword, followed by its *lower bound*: `<? super A>`

Say you want to write a method that puts `Integer` objects into a list. To maximize flexibility, you would like the method to work on `List<Integer>`, `List<Number>`, and `List<Object>` -- anything that can hold `Integer` values.

### Restrictions on Generics

- Cannot instantiate generic types with primitypes

  ```java
  Pair<int, char> p = new Pair<>(8, 'a'); // compile-time error
  Pair<Integer, Character> p = new Pair<>(8, 'a');
  ```

- Cannot create instance of type parameters

  ```java
  public static <E> void append(List<E> list) {
    E elem = new E(); // complie-time error
    list.add(elem);
  }
  ```

- Cannot declare static fields whose types are type parameters

  ```java
  public class MobileDevice<T> {
    private static T os;
  }
  // ...
  ```

- Cananot use cast or `instanceof` with parameterized types

```java
public static <E> void rtti(List<E> list) {
  Object obj = new Object();
  if (obj instanceof E) { // not possible

  }

  if (list instanceof ArrayList<Integer>) { // compile-time error
    // ...
  }
}
```

- Cannot create arrays of parameterized types

```java
List<Integer>[] arrayOfLists = new List<Integer>[2]; // compile-time error
```

- Cannot create, catch, or throw object of parameterized types
  - A gneneric class connot extend the `Throwable` class directly or indirectly. For example, the following classes will not complie.

```java
// Extends Throwable indirectly
class MathException<T> extends Exception { /* ... */ } // compile-time error

// Extends Throwable directly
class QueueFullException<T> extends Throwable { /* ... */ } // compile-time error
```

- Cannot overload a method where the format parameter types of each overloads erase to the same raw type.
  - A class cannot have two overloaded methods that will have the same signature after type erasure.

  ```java
  public class Example {
      public void print(Set<String> strSet) { }
      public void print(Set<Integer> intSet) { }
  }
  ```

  - The overloads would all share the same classfile representation and will generate a compile-time error.
