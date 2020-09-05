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
  - A Java complier applies string type ckecking to generic code and issues errors if the code violates type safety. Fixing complie-time erros is easier than fixing runtime errors, which can be difficult to find.

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

- A generic class cana have multiple type parameters. For example, the generic `OrderedPair` class, which implements the generic `Pair` interface:

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

TBD

### Bounded Type Parameters

TBD

### Wildcards

TBD

#### Upperbounded Wildcards

TBD

#### LowerBounded Wildcards

TBD

#### Multiple Bounds

TBD

### Restrictions on Generics

TBD

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
