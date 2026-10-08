# Java Reference Data Types

## Introduction

Java data types are divided into:

1. Primitive Data Types
2. Reference Data Types

A **reference data type** stores a reference (memory address) to an object rather than the actual value.

```java
String name = "Alice";
```

The variable `name` refers to a String object in memory.

---

## Primitive vs Reference Types

| Feature | Primitive | Reference |
|----------|------------|-----------|
| Stores | Actual value | Reference to object |
| Null Allowed | No | Yes |
| Methods | No | Yes |
| Memory | Stack | Stack + Heap |
| Examples | int, double | String, Array, Class |

---

## Common Reference Types

### String

```java
String language = "Java";
```

### Arrays

```java
int[] numbers = {1, 2, 3};
```

### Classes

```java
class Student {
    String name;
}
```

```java
Student s = new Student();
```

### Interfaces

```java
interface Animal {
    void sound();
}
```

### Enums

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY
}
```

---

## Memory Model

```java
String text = "Hello";
```

```text
Stack                  Heap
-----                  ----
text  ------------->   "Hello"
```

The variable contains a reference to the object in the heap.

---

## Creating Objects

```java
Student student = new Student();
```

Steps:

1. Memory allocated in heap
2. Constructor executes
3. Reference returned
4. Variable stores reference

---

## Null References

```java
String name = null;
```

A null reference points to no object.

---

## NullPointerException

```java
String name = null;

System.out.println(name.length());
```

Produces:

```text
NullPointerException
```

---

## Comparing References

### Using ==

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Because the references are different.

---

### Using equals()

```java
System.out.println(a.equals(b));
```

Output:

```text
true
```

Because the contents are equal.

---

## Multiple References

```java
StringBuilder sb1 = new StringBuilder("Java");
StringBuilder sb2 = sb1;
```

Both variables reference the same object.

```java
sb2.append(" Programming");

System.out.println(sb1);
```

Output:

```text
Java Programming
```

---

## Passing Reference Types to Methods

```java
public static void changeName(Student s) {
    s.name = "Alice";
}
```

```java
Student student = new Student();
changeName(student);
```

The object's state changes because both references point to the same object.

---

## Object Class

All Java classes inherit from Object.

```java
Object obj = new String("Java");
```

Common methods:

```java
toString()
equals()
hashCode()
getClass()
```

---

## Wrapper Classes

Wrapper classes are object versions of primitive types.

| Primitive | Wrapper |
|-----------|----------|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

Example:

```java
Integer number = 100;
```

---

## Autoboxing

```java
Integer num = 100;
```

Primitive `int` automatically becomes an `Integer`.

### Unboxing

```java
int value = num;
```

---

## Garbage Collection

```java
Student s = new Student();
s = null;
```

The object may become eligible for garbage collection.

---

## Common Reference Types

| Type | Description |
|--------|-------------|
| String | Text data |
| Array | Collection of values |
| Class | Custom objects |
| Interface | Behavioral contract |
| Enum | Fixed constants |
| Collection | Lists, Sets, Maps |
| Wrapper Class | Object form of primitive |

---

## Example Program

```java
class Student {
    String name;
}

public class ReferenceDemo {

    public static void main(String[] args) {

        Student student = new Student();
        student.name = "Alice";

        Student another = student;

        System.out.println(student == another);
        System.out.println(student.name);
    }
}
```

Output:

```text
true
Alice
```

---

## Best Practices

- Initialize references before use.
- Check for null when appropriate.
- Use equals() for content comparison.
- Understand shared references.
- Avoid unnecessary object creation.

---

## Interview Questions

### What is a reference type?

A type that stores a reference to an object.

### Can reference variables be null?

Yes.

### What does == compare?

References (memory locations).

### What does equals() compare?

Object contents.

### Are arrays reference types?

Yes.

### Is String a reference type?

Yes.

---

## Summary

Reference data types store references to objects located in heap memory. Examples include Strings, arrays, classes, interfaces, enums, collections, and wrapper classes. Understanding references, object creation, null values, equality comparisons, and garbage collection is essential for Java development.
