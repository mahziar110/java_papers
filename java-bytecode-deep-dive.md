# Java Bytecode Execution Deep Dive
## From Source Code to Native Machine Code inside the JVM

---

# 1. Introduction

A common question in Java is:

> Is Java interpreted or compiled?

The technically correct answer is:

**Modern Java uses a hybrid execution model that combines interpretation and Just-In-Time (JIT) compilation.**

This document explores the entire execution pipeline inside the JVM.

---

# 2. High-Level Execution Flow

```text
Java Source Code (.java)
          │
          ▼
      javac Compiler
          │
          ▼
      Bytecode (.class)
          │
          ▼
      Class Loader
          │
          ▼
      Bytecode Verifier
          │
          ▼
      Interpreter
          │
          ▼
      JIT Compiler
          │
          ▼
 Native Machine Code
          │
          ▼
        CPU
```

---

# 3. Java Compilation Phase

Java source code:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello JVM");
    }
}
```

Compiled by:

```bash
javac Main.java
```

Output:

```text
Main.class
```

The `.class` file contains JVM bytecode rather than machine instructions.

---

# 4. What is Bytecode?

Bytecode is an intermediate representation between source code and machine code.

Characteristics:

- Platform independent
- Compact
- Verifiable
- Designed for JVM execution

Example:

```java
int c = a + b;
```

Bytecode:

```text
iload_1
iload_2
iadd
istore_3
```

---

# 5. JVM Architecture Overview

```text
JVM
├── Class Loader Subsystem
├── Runtime Data Areas
├── Execution Engine
│   ├── Interpreter
│   ├── JIT Compiler
│   └── Garbage Collector
└── Native Interface
```

---

# 6. Class Loader Subsystem

Responsibilities:

1. Loading
2. Linking
3. Initialization

### Loading

Loads class bytecode into memory.

### Linking

Consists of:

- Verification
- Preparation
- Resolution

### Initialization

Executes static initializers.

Example:

```java
static {
    System.out.println("Initialized");
}
```

---

# 7. Bytecode Verification

JVM verifies:

- Stack safety
- Type safety
- Illegal memory access
- Instruction validity

Benefits:

- Security
- Stability
- Portability

---

# 8. Runtime Data Areas

## Method Area / Metaspace

Stores:

- Class metadata
- Method metadata
- Constant pools

## Heap

Stores objects.

```java
Person p = new Person();
```

Object lives in Heap.

## Java Stack

Stores:

- Local variables
- Stack frames
- Method calls

## PC Register

Stores current instruction location.

## Native Method Stack

Used by JNI.

---

# 9. Interpreter

The interpreter reads bytecode instructions one at a time.

Pseudo-code:

```text
while(running){

  instruction = fetch()

  decode()

  execute()
}
```

Advantages:

- Fast startup
- No compilation delay

Disadvantages:

- Repeated decoding
- Slower execution

---

# 10. The Performance Problem

Consider:

```java
for(int i=0;i<10000000;i++){
    calculate();
}
```

Without optimization:

```text
Interpret
Interpret
Interpret
Interpret
Interpret
...
```

The same bytecode is decoded millions of times.

---

# 11. JIT Compiler

JIT = Just-In-Time Compiler

Goal:

Convert frequently executed bytecode into machine code.

Process:

```text
Bytecode
   │
   ▼
Interpreter
   │
   ▼
JIT Compilation
   │
   ▼
Native Code
```

---

# 12. Hot Methods

JVM profiles execution.

Methods executed frequently become:

```text
Hot Methods
```

Example:

```java
calculate()
```

after thousands of invocations.

---

# 13. Code Cache

Compiled native code is stored in:

```text
Code Cache
```

Benefits:

- Reuse compiled code
- Avoid recompilation

---

# 14. Tiered Compilation

Modern HotSpot JVM uses multiple execution levels.

```text
Level 0 → Interpreter
Level 1 → C1
Level 2 → Profiled C1
Level 3 → Optimized C1
Level 4 → C2
```

Goal:

- Fast startup
- High peak performance

---

# 15. C1 Compiler

Client Compiler

Characteristics:

- Fast compilation
- Lower optimization cost
- Good startup behavior

---

# 16. C2 Compiler

Server Compiler

Characteristics:

- Aggressive optimizations
- Longer compilation time
- Higher runtime performance

Examples:

- Loop optimizations
- Inlining
- Escape analysis

---

# 17. Method Inlining

Example:

```java
int add(int a,int b){
   return a+b;
}
```

Call:

```java
result = add(x,y);
```

May become:

```java
result = x + y;
```

Benefits:

- Eliminates call overhead
- Enables further optimizations

---

# 18. Escape Analysis

JVM determines whether an object escapes its scope.

Example:

```java
Point p = new Point();
```

If object never escapes:

JVM may avoid heap allocation entirely.

Benefits:

- Reduced GC pressure
- Faster execution

---

# 19. Loop Optimizations

Possible optimizations:

- Loop unrolling
- Loop invariant code motion
- Strength reduction

Example:

```java
for(int i=0;i<1000;i++)
```

Can be transformed into a more CPU-friendly version.

---

# 20. Speculative Optimization

JVM uses runtime assumptions.

Example:

```java
interface Animal
```

Only one implementation observed:

```java
Dog
```

JVM optimizes assuming Dog is always used.

---

# 21. Deoptimization

If assumptions become invalid:

```text
Optimized Code
       ↓
Deoptimization
       ↓
Interpreter
```

This allows aggressive optimization without losing correctness.

---

# 22. Garbage Collection Interaction

JIT and GC cooperate.

Optimized code:

- Reduces allocations
- Reduces object lifetime
- Improves GC efficiency

---

# 23. AOT Compilation

Ahead-Of-Time Compilation

Instead of compiling during execution:

```text
Bytecode
    ↓
Native Binary
```

before deployment.

---

# 24. GraalVM Native Image

Example:

```bash
native-image MyApp
```

Produces:

```text
myapp.exe
```

or

```text
myapp
```

without requiring a traditional JVM startup.

---

# 25. JIT vs AOT

| Feature | JIT | AOT |
|----------|----------|----------|
| Compile Time | Runtime | Build Time |
| Startup Speed | Medium | Fast |
| Runtime Optimization | Excellent | Limited |
| Memory Usage | Higher | Lower |
| Peak Performance | Usually Higher | Sometimes Lower |

---

# 26. Why Java Is Not Purely Interpreted

Java source is compiled:

```text
Java → Bytecode
```

Therefore it is not purely interpreted.

---

# 27. Why Java Is Not Purely Compiled

Bytecode is not native machine code.

Execution still requires:

- Interpretation
- JIT compilation

Therefore it is not purely compiled.

---

# 28. Final Answer

Modern JVMs use a hybrid model:

```text
Source Code
      ↓
javac
      ↓
Bytecode
      ↓
Interpreter
      ↓
Profiling
      ↓
JIT Compiler
      ↓
Native Machine Code
```

This design combines:

- Portability
- Security
- Fast startup
- High performance

and is one of the primary reasons Java remains competitive for large-scale systems.
