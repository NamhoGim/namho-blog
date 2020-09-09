---
title: Java Collections Framework
tags:
  - Java
  - collections
categories:
  - - Java
    - collections
date: 2020-09-09 21:46:14
---


## Java Collections Framework

- `java.utils`에 속한 일련의 클래스로, 자료구조를 담당
- 잘 짜여진 `interface`를 기반으로 다양한 자료구조를 구현
- Generic class로 되어 있어, 다양한 객체를 요소로 담을 수 있다.

> some are excerpted from [Java collections overview](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html)

## Advantages of a collections framework

- Reduces programming effort
- Increase performance
- Provide interoperability between unrelated APIs
- Reduces the effort required to learn APIs
- Reduces the effort required to design and implement APIs
- Fosters software reuse

## Collection Interfaces

- [Collection](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html)
  - A group of objects. No assumptions are made about the order of the collection (if any) or whether it can contain duplicate elements.
- [Set](https://docs.oracle.com/javase/8/docs/api/java/util/Set.html)
  - The familiar set abstraction. No duplicate elements permitted. May or may not be ordered. Extends the Collection interface.
- [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)
  - Ordered collection, also known as a sequence. Duplicates are generally permitted. Allows positional access. Extends the Collection interface.
- [Queue](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html)
  - A collection designed for holding elements before processing. Besides basic Collection operations, queues provide additional insertion, extraction, and inspection operations.
- [Deque](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)
  - A double ended queue, supporting element insertion and removal at both ends. Extends the Queue interface.
- [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html)
  - A mapping from keys to values. Each key can map to one value.

## Method of Collection Interfaces

|Method|Description|
|:-----|:----|
|`boolean add(E e)`| Ensures that collection contains the specified element. |
|`boolean addAll(Collection<? exteionds E> c)`| Adds all of the elements in the specified collection to this collection. |
|`void clear()`| Removes all of the elements from this collection. |
|`boolean contains(Object o)`| Returns `true` if this collection contains the specified element. |
|`boolean containsAll(Collection<?> c)` | Returns `true` if this collection contains all of the elements in the specified collection. |
|`boolean equals(Object o)` | Compares the specified object with this collection for equality. |
|`boolean isEmpty()` | Returns `true` if this collection contains no elements. |
|`Iterator<E> iterator()` | Returns `true` if this collection contains on elements. |
|`boolean remove(Object o)` | Remove a single instance of the specified elemtns from this collection, if it is present. |
|`boolean removeAll(Collection<?> c)` | Removes all of this collection's elements that are also contained in the specified collection. |
|`boolean retainAll(Collection<?> c)` | Retains only the elements in this collection that are contained in the specified collection. |
|`int size()` | Returns the number of elements in this collection. |
|`T[] toArray(T[] a)` | Returns an array containing all of the elements in this collection; the runtime type of the returned array is that of the specified array. |
