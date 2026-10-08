# Java Memory Model – Complete Chapter Pack

## 1. Introduction

Java memory management is one of the most important topics in understanding how Java applications run internally.

The JVM divides memory into several runtime areas to efficiently manage execution, objects, and optimization.

This chapter covers:

- Stack Memory
- Heap Memory
- Method Area (Metaspace)
- JVM Architecture
- Garbage Collection
- String Pool
- Memory leaks
- Interview questions

---

# 2. JVM Architecture Overview

The Java Virtual Machine (JVM) consists of:

- Class Loader
- Runtime Data Areas
- Execution Engine
- Garbage Collector

### Runtime Memory Areas:

```text
+----------------------+
| Method Area          |
+----------------------+
| Heap                 |
+----------------------+
| Stack                |
+----------------------+
| PC Register          |
+----------------------+
| Native Method Stack  |
+----------------------+
```

---

# 3. Stack Memory (Deep Dive)

## Purpose
Stores method calls and local variables.

## Characteristics

- Thread-specific
- LIFO structure
- Fast access
- Automatically managed

## Example

```java
public class Main {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        add(a, b);
    }

    static void add(int x, int y) {
        int sum = x + y;
    }
}
```

## Stack Execution Flow

```text
main() frame
-> add() frame
-> add() removed
-> main() removed
```

---

# 4. Heap Memory (Deep Dive)

## Purpose
Stores objects and arrays.

## Characteristics

- Shared among threads
- Managed by Garbage Collector
- Slower than stack
- Dynamic allocation

## Example

```java
Student s = new Student();
```

## Heap Representation

```text
STACK              HEAP
-----              ----
s  ----------->    Student Object
                   name = null
                   age = 0
```

---

# 5. Method Area / Metaspace

## Stores:

- Class metadata
- Static variables
- Method code
- Constant pool

### Example

```java
class Student {
    static String school = "ABC School";
}
```

`school` is stored in Method Area.

---

# 6. PC Register

Each thread has a Program Counter:

- Tracks current executing instruction
- Helps JVM resume execution

---

# 7. Native Method Stack

Used for native (C/C++) methods via JNI.

---

# 8. Garbage Collection (GC)

## What is GC?

Automatic memory management that removes unreachable objects.

## Example:

```java
Student s = new Student();
s = null;
```

Now object is eligible for GC.

---

## GC Process

1. Mark
2. Sweep
3. Compact (optional)

---

## Types of GC

- Serial GC
- Parallel GC
- G1 GC (modern default)
- ZGC (low latency)

---

# 9. String Pool (Important Concept)

String literals are stored in a special memory area.

```java
String a = "Java";
String b = "Java";
```

Both refer to same object.

```text
String Pool
-----------
"Java"
```

---

## New String vs Literal

```java
String a = "Java";
String b = new String("Java");
```

- Literal → String Pool
- new → Heap

---

# 10. Stack vs Heap (Complete Comparison)

| Feature | Stack | Heap |
|--------|------|------|
| Storage | Methods, locals | Objects |
| Speed | Fast | Slower |
| Size | Small | Large |
| Lifetime | Short | Long |
| Thread | Private | Shared |

---

# 11. Memory Lifecycle

## Object creation:

```java
Student s = new Student();
```

Steps:

1. Reference created in stack
2. Object created in heap
3. Reference points to heap object

---

## Object destruction:

```java
s = null;
```

Object becomes eligible for GC.

---

# 12. Memory Leaks in Java

Even with GC, leaks can happen:

### Causes:

- Unused references
- Static collections
- Listener not removed

### Example:

```java
static List<Object> list = new ArrayList<>();
```

---

# 13. Stack Overflow vs Heap Overflow

## StackOverflowError

Caused by infinite recursion:

```java
void test() {
    test();
}
```

---

## OutOfMemoryError

Caused by too many objects:

```text
Heap is full
```

---

# 14. Primitive vs Reference Memory

## Primitive

```java
int x = 10;
```

Stored in stack.

## Reference

```java
Student s = new Student();
```

Reference in stack, object in heap.

---

# 15. JVM Memory Flow Diagram

```text
        JVM
         |
-------------------------
| Method Area           |
| Heap                  |
| Stack (Thread 1)     |
| Stack (Thread 2)     |
| PC Register          |
-------------------------
```

---

# 16. Performance Insights

## Stack is faster because:

- No GC
- Direct memory allocation

## Heap is slower because:

- GC overhead
- Dynamic allocation

---

# 17. Best Practices

- Avoid deep recursion
- Avoid unnecessary object creation
- Use StringBuilder for heavy string operations
- Clear references when not needed
- Avoid memory leaks in collections

---

# 18. Common Interview Questions

### 1. Difference between stack and heap?

Stack stores methods/local variables; heap stores objects.

---

### 2. What is stored in heap?

Objects and arrays.

---

### 3. What is stored in stack?

Method calls and local variables.

---

### 4. What is String Pool?

Special memory for string literals.

---

### 5. What causes StackOverflowError?

Infinite recursion.

---

### 6. What causes OutOfMemoryError?

Excess object creation.

---

# 19. Summary

Java memory is divided into structured runtime areas that help manage execution efficiently.

- Stack → execution flow
- Heap → objects
- Method Area → class data
- GC → automatic cleanup

Understanding this model is essential for writing efficient, scalable Java applications.
