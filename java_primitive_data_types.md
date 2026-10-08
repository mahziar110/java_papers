# Java Primitive Data Types

## Overview

Java provides **8 primitive data types**. Primitive types are the most basic data types built into the Java language. They store simple values directly and are not objects.

| Data Type | Size | Range / Values | Default Value |
|-----------|------|----------------|---------------|
| byte | 8-bit | -128 to 127 | 0 |
| short | 16-bit | -32,768 to 32,767 | 0 |
| int | 32-bit | -2,147,483,648 to 2,147,483,647 | 0 |
| long | 64-bit | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 | 0L |
| float | 32-bit | Approx. ±3.4E38 | 0.0f |
| double | 64-bit | Approx. ±1.7E308 | 0.0d |
| char | 16-bit | Unicode characters | '\u0000' |
| boolean | JVM dependent | true or false | false |

---

## 1. byte

The `byte` type is used to save memory when storing large arrays of integers.

```java
byte age = 25;
```

### Key Points
- Size: 1 byte (8 bits)
- Range: -128 to 127
- Useful for memory-efficient applications

---

## 2. short

The `short` type is larger than `byte` but smaller than `int`.

```java
short year = 2025;
```

### Key Points
- Size: 2 bytes (16 bits)
- Range: -32,768 to 32,767

---

## 3. int

The most commonly used integer type in Java.

```java
int population = 1000000;
```

### Key Points
- Size: 4 bytes (32 bits)
- Preferred choice for integer values

---

## 4. long

Used when integer values exceed the range of `int`.

```java
long distance = 9876543210L;
```

### Key Points
- Size: 8 bytes (64 bits)
- Must use `L` suffix for long literals beyond int range

---

## 5. float

Used for single-precision floating-point numbers.

```java
float price = 19.99f;
```

### Key Points
- Size: 4 bytes
- Approximately 6–7 decimal digits of precision
- Requires `f` or `F` suffix

---

## 6. double

The default floating-point type in Java.

```java
double pi = 3.141592653589793;
```

### Key Points
- Size: 8 bytes
- Approximately 15–16 decimal digits of precision
- Recommended for most decimal calculations

---

## 7. char

Stores a single Unicode character.

```java
char grade = 'A';
char symbol = '$';
```

### Key Points
- Size: 2 bytes (16 bits)
- Stores Unicode characters

---

## 8. boolean

Represents logical values.

```java
boolean isJavaFun = true;
boolean isComplete = false;
```

### Key Points
- Values: `true` or `false`
- Commonly used in conditions and loops

---

## Example Program

```java
public class PrimitiveTypesDemo {
    public static void main(String[] args) {
        byte b = 10;
        short s = 1000;
        int i = 50000;
        long l = 10000000000L;

        float f = 5.75f;
        double d = 19.99;

        char c = 'J';
        boolean flag = true;

        System.out.println(b);
        System.out.println(s);
        System.out.println(i);
        System.out.println(l);
        System.out.println(f);
        System.out.println(d);
        System.out.println(c);
        System.out.println(flag);
    }
}
```

---

## Primitive Types vs Wrapper Classes

| Primitive Type | Wrapper Class |
|---------------|---------------|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

Wrapper classes allow primitive values to be used as objects when required by collections and frameworks.

---

## Summary

Java has 8 primitive data types:

- byte
- short
- int
- long
- float
- double
- char
- boolean

These types form the foundation of data storage and manipulation in Java programs. Choosing the appropriate primitive type can improve memory usage, performance, and code clarity.
