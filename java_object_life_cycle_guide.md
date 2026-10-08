# Java Object Life Cycle

## Introduction

The Java Object Life Cycle describes the stages an object passes through from creation until its memory is reclaimed by the Garbage Collector.

---

## Life Cycle Overview

```text
Class Loading
      ↓
Object Creation
      ↓
Object Initialization
      ↓
Object Usage
      ↓
Object Becomes Unreachable
      ↓
Garbage Collection
      ↓
Memory Reclamation
```

---

## 1. Class Loading

Before creating an object, the JVM loads the class into memory.

```java
Student s = new Student();
```

The class definition is loaded into the JVM if it is not already available.

---

## 2. Object Creation

Objects are created using the `new` keyword.

```java
Student s = new Student();
```

During creation:

- Memory is allocated in the heap.
- Instance variables receive default values.
- Constructor is prepared to execute.

---

## 3. Object Initialization

The constructor initializes the object.

```java
class Student {
    String name;

    Student() {
        name = "Alice";
    }
}
```

After initialization the object is ready for use.

---

## 4. Object Usage

The object performs work through fields and methods.

```java
s.name = "Bob";
System.out.println(s.name);
```

As long as the object is reachable, it remains alive.

---

## 5. Reachability

Objects remain alive while reachable from GC Roots:

- Local variables
- Static variables
- Active threads
- JNI references

Example:

```java
Student s = new Student();
```

The object is reachable through `s`.

---

## 6. Object Becomes Unreachable

An object becomes eligible for garbage collection when no references point to it.

Example:

```java
Student s = new Student();
s = null;
```

Another example:

```java
Student s1 = new Student();
Student s2 = new Student();

s1 = s2;
```

The first object becomes unreachable.

---

## 7. Garbage Collection

The Garbage Collector identifies unreachable objects and removes them.

Main phases:

### Mark
Find reachable objects.

### Sweep
Remove unreachable objects.

### Compact
Reduce memory fragmentation.

---

## Requesting Garbage Collection

```java
System.gc();
```

This only suggests garbage collection; the JVM decides when to run it.

---

## 8. Memory Reclamation

After GC removes an object:

- Memory is freed.
- Heap space becomes available.
- Future objects can reuse the space.

---

## Memory Example

```java
Student s = new Student();
```

```text
STACK                    HEAP
-----                    -----
s ------------->         Student Object
```

After:

```java
s = null;
```

```text
STACK                    HEAP
-----                    -----
s = null                 Student Object
                         Eligible for GC
```

---

## Object States Summary

| State | Description |
|---------|-------------|
| Loaded | Class loaded |
| Created | Memory allocated |
| Initialized | Constructor executed |
| Active | Object being used |
| Unreachable | No references remain |
| Collected | Removed by GC |
| Reclaimed | Memory reused |

---

## Complete Example

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}

public class Main {

    public static void main(String[] args) {

        Student s = new Student("Alice");

        System.out.println(s.name);

        s = null;
    }
}
```

---

## Common Interview Questions

### What is the Object Life Cycle?

The sequence of stages an object goes through from creation until garbage collection.

### Where are objects stored?

Heap memory.

### What makes an object eligible for GC?

No reachable references remain.

### Can we manually destroy an object?

No. Java uses automatic garbage collection.

---

## Best Practices

- Avoid unnecessary object creation.
- Remove unused references.
- Understand reachability.
- Prevent memory leaks.
- Let GC manage memory efficiently.

---

## Summary

The Java Object Life Cycle consists of:

1. Class Loading
2. Object Creation
3. Object Initialization
4. Object Usage
5. Becoming Unreachable
6. Garbage Collection
7. Memory Reclamation

Understanding these stages helps developers write efficient and memory-safe Java applications.
