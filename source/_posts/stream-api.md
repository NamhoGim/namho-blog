---
title: Stream API
tags:
  - Java
  - Stream API
categories:
  - Stream API
date: 2020-10-05 08:32:52
---


Most of the contents are excerpt from "Modern Java in Action"

## What are streams?

_Streams_ are an update to the Java API that let you manipulate collections of data
in a declarative way (you express a query rather than code an ad hoc implementation
for it.) In addition, streams can be processed in parallel _transparently_, without you having to write any multithreaded code.

- Before (Java 7)

```java
List<Dish> lowCaloricDishes = new ArrayList<>();
for(Dish dish: menu) {
    if(dish.getCalories() < 400) {
        lowCaloricDishes.add(dish); // Filters the elements using an accumulator
    }
}

Collections.sort(lowCaloricDishes, new Comparator<Dish>() { // Sorts the dishes with an anonymous class
    public int compare(Dish dish1, Dish dish2) {
        return Integer.compare(dish1.getCalories(), dish2.getCalories());
    }
});

List<String> lowCaloricDishesName = new ArrayList<>();
for(Dish dish: lowCaloricDishes) {
    lowCaloricDishesName.add(dish.getName()); // Processes the sorted list to select the names of dishes
}
```

- After (Java 8)

```java
import static java.util.Comparator.comparing;
import static java.util.stream.Collectors.toList;
List<String> lowCaloricDishsesName =
                menu.stream()
                    .filter(d -> d.getCalories() < 400)
                    .sorted(comparing(Dish::getCalories))
                    .map(Dish::getName)
                    .collect(toList());
```

You can see that the new approach offers several immediate benefits from a software
engineering point of view:

- The code is written in a _declarative way_: you specify _what_ you what to achieve
as opposed to specifying _how_ to implement an operation (using control-flow blocks such as loops and `if` conditions)

- You chain together several building-block operations to express a complicated
data-processing pipeline while keeping your code readable and its intent clear.

The Stream API in Java 8 lets you write code that's

- _Declarative_ -- More concise and readable
- _Composable_ -- Greater flexibility
- _Parallelizable_ -- Better performance

## Stream operations

Stream operations that can be connected are called _intermediate operations_, and oerations that close a stream are called _terminal operations_.

### Intermediate operations

Intermediate operations such as `filer` or `sorted` return another stream as the return type. This allows the operations to be connected to form a query.
What's important is that inermediate operations don't perform any processing until a terminal operations can usually be merged and processed into a single pass by terminal operation.

### Terminal operations

Terminal operations produce a result from a stream pipleline.
A result is any non-stream value such as `List`, an `Integer`, or even `void`.
For example, in the following pipeline, `forEach` is terminal operation that returns
`void` and applies a lambda to each object. Passing `System.out.println` to `forEach` asks it to print every object in the stream.

`stream().forEach(System.out::println);`

## Working with streams

### Filtering

#### Filtering with a predicate

The `Stream` interface supports a `filter` method. This operations takes as argument a _predicate_ (a function returning `boolean`) and returns a stream including all elements that match the predicate.

```java
import static java.util.stream.Collectors.toList;
List<Dish> vegetarianDishes =
    menu.stream()
        .filter(Dish::isVegetarian) // Use a method reference to check if dish is vegetarian friendly.
        .collect(toList());
```

#### Filtering unique elements

Streams also support a method called `distinct` that returns a stream with unique
elements (according to the implementation of the `hashcode` and `equals` methods of
the objects produced by the stream).

```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
numbers.stream()
       .filter(i -> i % 2 == 0)
       .distinct()
       .forEach(System.out::println);
```

#### Truncating a stream

Streams support the `limit(n)` methods, which returns another stream that's no
longer thant a given size. The requested size is passed as argument to `limit`.
If the stream is ordered, the first elements are returned up to a maximum of `n`.

#### Skipping elements

Streams support the `skip(n)` method to return a stream that discards the first `n`
elements. If the stream has fewer than `n` elements, an empty stream is returned.
Note that `limit(n)` and `skip(n)` are complementary.

### Mapping

A common data processing idion is to select information from certain objects.
For example, in SQL you can select a particular column from a table. The Stream API
provides similar facilities through the `map` and `flatMap` methods.

#### Applying a function to each element of a stream

Stream support the `map` method, which takes a function as argument. The function is
Applied to each element, mapping it into a new element (the word _mapping_ is used
because it has a meaning similar to _transforming_ but with the nuance of "creating
a new version of" rather than "modifying").
For example, in the following code you pass a method reference `Dish::getName` to the `map` method to _extract_ the names of the dishess in the stream:

```java
List<String> dishNames = menu.stream()
                             .map(Dish::getName)
                             .collect(toList());
```

#### Flattening streams

How could you return a list of all the _unique characters_ for a list of words?
For example, given the list of words ["Hello", "World"] -> ["H", "e", "l", "o", "W", "r", "d"].

```java
words.stream()
     .map(word -> word.spit(""))
     .distinct()
     .collect(toList());
```

Problem: the `map` method returns a `String[]` (an array of String) for each word.

#### Attempt Using Map and Arrays.stream

First, you need a stream of characters instead of a stream of arrays. There's a method called `Arrays.stream()` that takes an array and produces a stream:

```java
String[] arrayOfWord = {"Goodbye", "World"};
Stream<String> streamOfWords = Arrays.stream(arrayOfWords);
```

Use it in the previous pipeline to see what happens:

```java
words.stream()
     .map(word -> word.split(""))
     .map(Arrays::stream)
     .distinct()
     .collect(toList());
```

Indeed, first convert each word into an array of its individual letters and then
make each array into a separate stream.

#### Using `flatMap`

```java
List<String> uniqueCharacters =
    words.stream()
         .map(word -> word.split("")) // Converts each word into a array of its individual letters
         .flatMap(Arrays::stream) // Flatten each generated stream into single stream
         .distinct()
         .collect(toList());
```

Using the `flatMap` method has the effect of mapping each array not with a stream
but _with the contents of that stream._ All the separate streams that were generated
when using `map(Arrays::stream)` get amalgamated -- flatten into a single stream.

### Finding and matching

Another common data processing idiom is finding whether some elements in a set of
data match a given property.
The Stream API provides such facilities through the `allMatch`, `anyMatch`, `noneMatch`, `findFirst`, and `findAny`

- `anyMatch(Predicate<T>)`: Checking to see if a predicate matches at least one element
- `allMatch(Predicate<T>)`: Checking to see if a predicate matches all elements

  ```java
  boolean isHealthy = menu.stream()
                          .allMatch(dish -> dish.getCalories() < 1000);
  ```

- `noneMatch(Predicate<T>)`: The opposite of `allMatch` is `noneMatch`. It ensures that no elements in the stream match the given predicate.

  ```java
  boolean isHealth = menu.stream()
                         .noneMatch(d -> d.getCalories() >= 1000);
  ```
  
- `findAny()`: Returns an arbitrary element of the current stream.

  ```java
  menu.stream()
      .filter(Dish::isVegetarian)
      .findAny() // Returns an Optional<Dish>
      .ifPresent(dish -> System.out.println(dish.getName())); // If a value is contained, it's printed; otherwise nothing happens.
  ```

- `findFirst()`: Returns a first element in an _encounter order._

  ```java
  List<Integer> someNumbers = Arrays.asList(1, 2, 3, 4, 5);
  Optional<Integer> firstSquareDivisibleByThree =
    someNumbers.stream()
               .map(n -> n * n)
               .filter(n -> n % 3 == 0)
               .findFirst(); // 9
  ```

### Reducing

#### Summing the elements

You can sum all elements of a stream as follows:

```java
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
```

`reduce` takes two arguments:

- An initial value, here 0.
- A `BinaryOperator<T>` to combine two elements and produce a new value; here
you use the lambda (a, b) -> a + b.

#### Maximum and minimum

```java
Optional<Integer> max = numbers.stream().reduce(Integer::max);
Optional<Integer> min = numbers.stream().reduce(Integer::min);
```

### Intermediate and terminal operations

| Operation   | Type          | Return type       | Type / functional interface used | Function descriptor |
|:------------|:--------------|:------------------|:---------------------------------|:--------------------|
| `filter`    | Intermediate  | `Stream<T>`       | `Predicate<T>`                   | `T -> boolean`      |
| `distinct`  | Intermediate  | `Stream<T>`       |                                  |                     |
| `takeWhile` | Intermediate  | `Stream<T>`       | `Predicate<T>`                   | `T -> boolean`      |
| `dropWhile` | Intermediate  | `Stream<T>`       | `Predicate<T>`                   | `T -> boolean`      |
| `skip`      | Intermediate  | `Stream<T>`       | `long`                           |                     |
| `limit`     | Intermediate  | `Stream<T>`       | `long`                           |                     |
| `map`       | Intermediate  | `Stream<R>`       | `Function<T, R>`                 | `T -> R`            |
| `flatMap`   | Intermediate  | `Stream<R>`       | `Function<T, Stream<R>>`         | `T -> Stream<R>`    |
| `sorted`    | Intermediate  | `Stream<T>`       | `Comparator<T>`                  | `(T, T) -> int`     |
| `anyMatch`  | Terminal      | `boolean`         | `Predicate<T>`                   | `T -> boolean`      |
| `noneMatch` | Terminal      | `boolean`         | `Predicate<T>`                   | `T -> boolean`      |
| `allMatch`  | Terminal      | `boolean`         | `Predicate<T>`                   | `T -> boolean`      |
| `findAny`   | Terminal      | `Optional<T>`     |                                  |                     |
| `findFirst` | Terminal      | `Optional<T>`     |                                  |                     |
| `forEach`   | Terminal      | `void`            | `Consumer<T>`                    | `T -> void`         |
| `collect`   | Terminal      | `R`               | `Collector<T, A, R>`             |                     |
| `reduce`    | Terminal      | `Optional<T>`     | `BinaryOperator<T>`              | `(T, T) -> T`       |
| `count`     | Terminal      | `long`            |                                  |                     |

## Collecting data with streams

The `collect` is a reduction operation, like `reduce`, that takes as an argument
various recipes for accumulating the elements of a stream into a summary result.
These recipes are defined by a new `Collector` interface, so it's important to
distinguish `Collection`, `Collector`, and `collect`.

Here are some example queries of what you'll be able to do using `collect` and
collectors:

- Group a list of transactions by currency to obtain the sum of the values of all
transactions with that currency (returning a `Map<Currency, Integer>`)
- Partition a list of transactions into two groups: expensive and not expensive
(returnting a `Map<Boolean, List<Transaction>>`)
- Create multilevel groupings, such as grouping transactions by cities and then
further categorizing by whether they're expensive of not (returning a `Map<String, Map<Boolean, List<Transaction>>>`)

Grouping transactions by currency in imperative style

```java
Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap<>();
for (Transaction transaction : transactions) {
  Currency currency = transaction.getCurrency(); // Extracts the Transaction’s currency
  List<Transaction> transactionsForCurrency = transactionsByCurrencies.get(currency);
  if (transactionsForCurrency == null) {
    transactionsForCurrency = new ArrayList<>();
    transactionsByCurrencies.put(currency, transactionsForCurrency);
  }
  transactionsForCurrency.add(transaction);
}
```

Using a more general `Collector` paramter to the `collect` method on stream rather
than the `toList` special case used in the previous chapter:

```java
Map<Currency, List<Transaction>> transactionsByCurrencies =
    transactions.stream().collect(groupingBy(Transaction::getCurrency));
```

### Grouping

A common database operation is to group items in a set, based on one or more properties.
You can easily perform this task using a collector returned by the Collectors.groupBy factory method, as follows:

```java
Map<Dish.Type, List<Dish>> dishesByType = menu.stream().collect(groupingBy(Dish::getType));
```

Here, you pass to the `groupingBy` method a `Function` (expressed in the form of a
method reference) extracting the corresponding `Dish.Type` for each `Dish` in the stream. We call this `Function` a _classification_ function specifically because it's used to classify the elements of the stream into different groups.

### Partitioning

Partitioning is a special case of grouping: having a predicate called a _partitioning function_ as a classification fuction. The fact that the partitioning
fuction returns a boolean means the resulting grouping `Map` will have `Boolean` as
a key type, and therefore, there can be at most two different groups -- one for `true` and one for `false`.

```java
Map<Boolean, List<Dish>> partitionedMenu =
             menu.stream().collect(partitioningBy(Dish::isVegetarian));
```
