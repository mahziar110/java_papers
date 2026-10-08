# Java Primitive Data Types

## Primitive Type Conversion (Casting)

Java allows values of one primitive type to be converted into another primitive type. This process is called **casting**.

### Types of Conversion

1. **Widening Conversion (Implicit / Automatic)**
2. **Narrowing Conversion (Explicit / Manual)**

---

## 1. Widening Conversion (Automatic)

A smaller type is automatically converted into a larger compatible type.

### Conversion Order

```text
byte → short → int → long → float → double
          ↘
           char → int → long → float → double
```

### Example

```java
int num = 100;
long longNum = num;      // automatic
double d = longNum;      // automatic
```

No data is lost because the destination type can hold all values of the source type.

---

## 2. Narrowing Conversion (Casting)

A larger type is converted into a smaller type.

Java requires an explicit cast because information may be lost.

### Example

```java
double pi = 3.14159;
int value = (int) pi;
```

Output:

```text
3
```

The decimal part is discarded.

---

## Conversion Examples

### int → double

```java
int a = 10;
double b = a;
```

Result:

```text
10.0
```

---

### double → int

```java
double x = 10.99;
int y = (int) x;
```

Result:

```text
10
```

---

### long → int

```java
long large = 5000L;
int small = (int) large;
```

---

### char → int

```java
char letter = 'A';
int code = letter;

System.out.println(code);
```

Output:

```text
65
```

---

### int → char

```java
int code = 66;
char letter = (char) code;

System.out.println(letter);
```

Output:

```text
B
```

---

## Data Loss During Casting

Be careful when converting from larger to smaller types.

```java
int number = 130;
byte b = (byte) number;

System.out.println(b);
```

Output:

```text
-126
```

Why?

`byte` can only store values between -128 and 127. Values outside that range overflow and wrap around.

---

## Floating Point to Integer

```java
float price = 19.99f;
int wholePrice = (int) price;
```

Output:

```text
19
```

Fractional digits are truncated.

---

## Primitive Conversion Table

| From | To | Automatic? |
|--------|--------|------------|
| byte | short | Yes |
| byte | int | Yes |
| short | int | Yes |
| int | long | Yes |
| long | float | Yes |
| float | double | Yes |
| double | float | No |
| long | int | No |
| int | short | No |
| int | byte | No |
| int | char | No |
| char | int | Yes |

---

## Type Promotion in Expressions

Java automatically promotes smaller types during arithmetic operations.

```java
byte a = 10;
byte b = 20;

int result = a + b;
```

Even though both operands are bytes, the result is an int.

---

## Best Practices

- Prefer automatic widening conversions whenever possible.
- Use explicit casts carefully.
- Check for possible overflow when narrowing.
- Use `double` rather than `float` for most decimal calculations.
- Be cautious when converting floating-point values to integers.

---

## Quick Reference

```text
Automatic (Widening)
byte → short → int → long → float → double
char → int → long → float → double

Manual (Narrowing)
double → float → long → int → short → byte
```

