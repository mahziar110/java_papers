# Stack and Heap in Java

## Introduction

In Java memory is mainly divided into:

1. Stack Memory
2. Heap Memory

Understanding this is essential for OOP and performance.

---

## Stack Memory

Used for:

- Method calls
- Local variables
- Reference variables

### Characteristics

- Fast
- LIFO (Last In First Out)
- Thread-specific
- Automatically managed

---

## Stack Example

```java
public class Main {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        int sum = a + b;
    }
}
```

### Stack View

```text
main()
-----
a = 10
b = 20
sum = 30
```

When method ends → stack frame removed.

---

## Heap Memory

Used for:

- Objects
- Arrays
- Instance variables

### Characteristics

- Shared across threads
- Slower than stack
- Managed by Garbage Collector
- Created using `new` keyword

---

## Heap Example

```java
class Student {
    String name;
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        s.name = "Alice";
    }
}
```

### Memory Layout

```text
STACK                     HEAP
-----                     ----
s  ------------->        Student Object
                          name = "Alice"
```

---

## Stack vs Heap

| Feature | Stack | Heap |
|----------|------|------|
| Stores | Methods, local vars | Objects |
| Speed | Fast | Slower |
| Size | Small | Large |
| Lifetime | Short | Long |
| Thread | Private | Shared |
| Management | Automatic | Garbage Collector |

---

## Object Creation Flow

```java
Student s = new Student();
```

Steps:

1. Reference `s` created in stack
2. Object created in heap
3. Stack points to heap object

---

## Garbage Collection

```java
s = null;
```

Object becomes eligible for GC.

---

## Stack Overflow

Caused by deep or infinite recursion:

```java
public static void f() {
    f();
}
```

Error:

```text
StackOverflowError
```

---

## Heap Overflow

Caused by too many objects:

```text
OutOfMemoryError
```

---

## Primitive vs Reference Storage

```java
int x = 10;       // Stack
Student s = new Student(); // Heap + Stack reference
```

---

## Multiple References

```java
Student s1 = new Student();
Student s2 = s1;
```

Both point to same heap object.

---

## Visual Summary

```text
STACK                 HEAP
-----                 ----
s1 -----------|     Student Object
s2 -----------|-->  name = "Alice"
```

---

## Method Stack Behavior

```java
A()
 -> B()
```

Each method gets its own stack frame.

---

## Why Stack is Fast

- Simple structure
- No GC overhead
- LIFO allocation

---

## Why Heap is Needed

- Dynamic objects
- Shared memory
- Flexible lifetime

---

## Interview Questions

### What is stack memory used for?
Methods and local variables.

### What is heap used for?
Objects.

### Which is faster?
Stack.

### Who manages heap?
Garbage Collector.

---

## Best Practices

- Avoid deep recursion
- Avoid unnecessary object creation
- Be careful with memory leaks
- Understand references clearly

---

## Summary

Stack handles execution flow and local data. Heap handles objects. Together they form Java’s memory model.
