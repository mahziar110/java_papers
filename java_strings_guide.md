# Java Strings

## Introduction

A **String** in Java is a sequence of characters. Strings are widely used for storing and manipulating text such as names, messages, emails, and file paths.

In Java, strings are objects of the `String` class found in the `java.lang` package.

```java
String message = "Hello World";
```

---

## Why Strings Are Special

Although `String` is a class, Java provides special syntax to make it easy to use.

```java
String name = "Alice";
```

instead of:

```java
String name = new String("Alice");
```

---

## String Immutability

Strings in Java are **immutable**.

Once a String object is created, its value cannot be changed.

```java
String str = "Hello";
str.concat(" World");

System.out.println(str);
```

Output:

```text
Hello
```

To keep the new value:

```java
str = str.concat(" World");
```

---

## Creating Strings

### Using String Literal

```java
String city = "Berlin";
```

### Using new Keyword

```java
String city = new String("Berlin");
```

---

## String Pool

Java maintains a special memory area called the **String Pool**.

```java
String s1 = "Java";
String s2 = "Java";
```

Both variables reference the same object in memory.

```java
System.out.println(s1 == s2);
```

Output:

```text
true
```

---

## Common String Methods

### length()

Returns the number of characters.

```java
String text = "Java";

System.out.println(text.length());
```

Output:

```text
4
```

---

### charAt()

Returns the character at a specified index.

```java
String text = "Java";

System.out.println(text.charAt(1));
```

Output:

```text
a
```

---

### substring()

Extracts part of a string.

```java
String text = "Programming";

System.out.println(text.substring(0, 4));
```

Output:

```text
Prog
```

---

### toUpperCase()

```java
String text = "java";

System.out.println(text.toUpperCase());
```

Output:

```text
JAVA
```

---

### toLowerCase()

```java
String text = "JAVA";

System.out.println(text.toLowerCase());
```

Output:

```text
java
```

---

### trim()

Removes leading and trailing spaces.

```java
String text = " Java ";

System.out.println(text.trim());
```

Output:

```text
Java
```

---

### replace()

```java
String text = "Java";

System.out.println(text.replace('a', 'o'));
```

Output:

```text
Jovo
```

---

### contains()

Checks whether a substring exists.

```java
String text = "Java Programming";

System.out.println(text.contains("Java"));
```

Output:

```text
true
```

---

### startsWith()

```java
String text = "Java Programming";

System.out.println(text.startsWith("Java"));
```

---

### endsWith()

```java
String text = "Java Programming";

System.out.println(text.endsWith("ing"));
```

---

## Comparing Strings

### Using equals()

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1.equals(s2));
```

Output:

```text
true
```

### Using ==

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

### Rule

- `==` compares references
- `equals()` compares contents

---

## String Concatenation

### Using + Operator

```java
String first = "Hello";
String second = "World";

String result = first + " " + second;
```

Output:

```text
Hello World
```

---

### Using concat()

```java
String result = first.concat(second);
```

---

## Escape Characters

| Escape Sequence | Meaning |
|----------------|----------|
| \n | New Line |
| \t | Tab |
| \\ | Backslash |
| \" | Double Quote |
| \' | Single Quote |

Example:

```java
System.out.println("Hello\nWorld");
```

---

## StringBuilder

When performing many string modifications, use `StringBuilder`.

```java
StringBuilder sb = new StringBuilder();

sb.append("Java");
sb.append(" ");
sb.append("Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

### Why StringBuilder?

Strings are immutable.

Repeated concatenation creates many temporary objects.

`StringBuilder` is faster and more memory efficient.

---

## StringBuilder vs String

| Feature | String | StringBuilder |
|----------|---------|--------------|
| Mutable | No | Yes |
| Thread Safe | Yes | No |
| Performance for modifications | Slower | Faster |
| Memory Usage | Higher | Lower |

---

## Converting Between Types

### String to int

```java
String number = "123";

int value = Integer.parseInt(number);
```

---

### int to String

```java
int value = 123;

String text = String.valueOf(value);
```

---

### String to double

```java
double d = Double.parseDouble("3.14");
```

---

## Useful Methods Summary

| Method | Purpose |
|----------|---------|
| length() | Number of characters |
| charAt() | Character at index |
| substring() | Extract text |
| equals() | Compare contents |
| contains() | Check substring |
| replace() | Replace characters |
| trim() | Remove spaces |
| toUpperCase() | Convert to uppercase |
| toLowerCase() | Convert to lowercase |
| split() | Split string |
| startsWith() | Check prefix |
| endsWith() | Check suffix |

---

## Example Program

```java
public class StringDemo {
    public static void main(String[] args) {

        String name = "Java";

        System.out.println(name.length());
        System.out.println(name.toUpperCase());
        System.out.println(name.charAt(0));

        String greeting = "Hello " + name;

        System.out.println(greeting);

        String number = "100";
        int value = Integer.parseInt(number);

        System.out.println(value + 50);
    }
}
```

---

## Best Practices

- Use String literals whenever possible.
- Use `equals()` instead of `==` for content comparison.
- Use `StringBuilder` for frequent modifications.
- Avoid unnecessary String object creation.
- Remember that Strings are immutable.

---

## Summary

Strings are one of the most important data types in Java. They are immutable objects used to store and manipulate text. Understanding String creation, comparison, common methods, String Pool behavior, and StringBuilder is essential for writing efficient Java applications.
