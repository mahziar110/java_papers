# Java Arrays

## Introduction

An **array** in Java is a data structure used to store multiple values of the same type in a single variable.

```java
int[] scores = {90, 85, 78};
```

---

## Why Use Arrays?

- Store multiple values efficiently
- Access data using indexes
- Iterate through collections of data
- Reduce repetitive code

---

## Array Declaration

```java
int[] numbers;
```

Preferred over:

```java
int numbers[];
```

---

## Array Creation

```java
int[] numbers = new int[5];
```

Creates an array with 5 elements.

Indexes:

```text
0 1 2 3 4
```

---

## Array Initialization

```java
int[] numbers = {10, 20, 30, 40, 50};
```

Or:

```java
int[] numbers = new int[]{10, 20, 30, 40, 50};
```

---

## Accessing Elements

```java
int[] numbers = {10, 20, 30};

System.out.println(numbers[0]);
```

Output:

```text
10
```

---

## Modifying Elements

```java
numbers[1] = 99;
```

---

## Array Length

```java
System.out.println(numbers.length);
```

---

## Iterating Through Arrays

### Traditional for Loop

```java
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```

### Enhanced for-each Loop

```java
for (int num : numbers) {
    System.out.println(num);
}
```

---

## Default Values

| Type | Default Value |
|--------|--------------|
| int | 0 |
| long | 0L |
| double | 0.0 |
| float | 0.0f |
| boolean | false |
| char | '\u0000' |
| String | null |

---

## ArrayIndexOutOfBoundsException

```java
int[] numbers = {10, 20, 30};

System.out.println(numbers[5]);
```

This throws:

```text
ArrayIndexOutOfBoundsException
```

---

## Multidimensional Arrays

### 2D Array

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

Accessing an element:

```java
System.out.println(matrix[1][2]);
```

Output:

```text
6
```

---

## Looping Through a 2D Array

```java
for (int row = 0; row < matrix.length; row++) {
    for (int col = 0; col < matrix[row].length; col++) {
        System.out.print(matrix[row][col] + " ");
    }
    System.out.println();
}
```

---

## Arrays Utility Class

```java
import java.util.Arrays;
```

### Print Array

```java
System.out.println(Arrays.toString(numbers));
```

### Sort Array

```java
Arrays.sort(numbers);
```

### Compare Arrays

```java
Arrays.equals(a, b);
```

### Fill Array

```java
Arrays.fill(numbers, 10);
```

---

## Copying Arrays

### clone()

```java
int[] copy = original.clone();
```

### Arrays.copyOf()

```java
int[] copy = Arrays.copyOf(original, original.length);
```

---

## Passing Arrays to Methods

```java
public static void printArray(int[] arr) {
    for (int num : arr) {
        System.out.println(num);
    }
}
```

---

## Returning Arrays from Methods

```java
public static int[] createArray() {
    return new int[]{1, 2, 3};
}
```

---

## Arrays vs ArrayList

| Feature | Array | ArrayList |
|---------|--------|-----------|
| Fixed Size | Yes | No |
| Stores Primitives | Yes | No |
| Faster | Yes | Usually |
| Dynamic Resizing | No | Yes |

---

## Common Interview Questions

### Are arrays objects?

Yes. Arrays are objects in Java.

### Can arrays store multiple data types?

No. Each array stores only one data type.

### Can array size change?

No. Array size is fixed after creation.

---

## Example Program

```java
import java.util.Arrays;

public class ArrayDemo {

    public static void main(String[] args) {

        int[] scores = {90, 85, 78, 95};

        System.out.println("Length: " + scores.length);

        for (int score : scores) {
            System.out.println(score);
        }

        Arrays.sort(scores);

        System.out.println(Arrays.toString(scores));
    }
}
```

---

## Summary

Arrays are a fundamental Java data structure that stores multiple values of the same type. Understanding declaration, creation, indexing, iteration, multidimensional arrays, and the Arrays utility class is essential for Java programming.
