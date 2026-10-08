# Java Objects

## Introduction

An **object** in Java is an instance of a class.

- Class = Blueprint
- Object = Actual instance created from the blueprint

Example:

```java
Car myCar = new Car();
```

---

## What Is an Object?

An object contains:

1. State (Fields / Attributes)
2. Behavior (Methods)

```java
class Car {
    String brand;
    int year;

    void start() {
        System.out.println("Car started");
    }
}
```

---

## Creating Objects

### Define a Class

```java
class Student {
    String name;
    int age;
}
```

### Create an Object

```java
Student student1 = new Student();
```

- `Student` = Class
- `student1` = Reference Variable
- `new Student()` = Object Creation

---

## Memory Layout

```java
Student student = new Student();
```

Conceptually:

```text
Stack                     Heap
-----                     -----
student  ------------->   Student Object
                           name = null
                           age = 0
```

---

## Accessing Fields

```java
student.name = "Alice";
student.age = 20;
```

```java
System.out.println(student.name);
System.out.println(student.age);
```

---

## Object Methods

```java
class Dog {

    void bark() {
        System.out.println("Woof!");
    }
}
```

```java
Dog dog = new Dog();
dog.bark();
```

Output:

```text
Woof!
```

---

## Multiple Objects

```java
Student s1 = new Student();
Student s2 = new Student();
```

Each object has its own data.

```java
s1.name = "Alice";
s2.name = "Bob";
```

---

## Constructors

A constructor initializes objects.

```java
class Student {

    Student() {
        System.out.println("Object created");
    }
}
```

```java
Student s = new Student();
```

---

## Parameterized Constructor

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
Student s = new Student("Alice", 20);
```

---

## The this Keyword

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

`this` refers to the current object.

---

## Object References

```java
Student s1 = new Student();
Student s2 = s1;
```

Both references point to the same object.

```java
System.out.println(s1 == s2);
```

Output:

```text
true
```

---

## Comparing Objects

### Using ==

Compares references.

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1 == s2);
```

Output:

```text
false
```

### Using equals()

Compares contents if implemented.

```java
String a = "Java";
String b = "Java";

System.out.println(a.equals(b));
```

Output:

```text
true
```

---

## Passing Objects to Methods

```java
public static void changeName(Student s) {
    s.name = "Alice";
}
```

```java
Student student = new Student();
changeName(student);
```

The object's state is modified.

---

## Returning Objects

```java
public static Student createStudent() {

    Student s = new Student();
    s.name = "Alice";

    return s;
}
```

---

## Anonymous Objects

```java
new Student();
```

Or:

```java
new Student().display();
```

Used when an object is needed only once.

---

## The Object Class

All Java classes inherit from `Object`.

Common methods:

```java
toString()
equals()
hashCode()
getClass()
```

---

## Overriding toString()

```java
class Student {

    String name;

    @Override
    public String toString() {
        return name;
    }
}
```

---

## Garbage Collection

```java
Student s = new Student();
s = null;
```

The object becomes eligible for garbage collection.

---

## Complete Example

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    void display() {
        System.out.println(name + " " + age);
    }
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student("Alice", 20);
        Student s2 = new Student("Bob", 22);

        s1.display();
        s2.display();
    }
}
```

Output:

```text
Alice 20
Bob 22
```

---

## Best Practices

- Use constructors for initialization.
- Prefer private fields.
- Use methods for controlled access.
- Override `toString()` for debugging.
- Avoid unnecessary object creation.
- Understand reference behavior.

---

## Common Interview Questions

### What is an object?

An instance of a class.

### What is the difference between a class and an object?

A class is a blueprint; an object is a real instance created from it.

### Where are objects stored?

Heap memory.

### What does the new keyword do?

Allocates memory and creates an object.

### What does this refer to?

The current object.

### Can multiple references point to the same object?

Yes.

### What happens when no references point to an object?

It becomes eligible for garbage collection.

---

## Summary

Objects are the core of Java's object-oriented programming model. They combine state (fields) and behavior (methods), and are created from classes. Understanding object creation, references, constructors, memory layout, and object interaction is essential for becoming proficient in Java.
