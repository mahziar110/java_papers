# Java Threads

## Introduction

A thread is the smallest unit of execution within a Java process.
Multiple threads allow a program to perform several tasks concurrently,
improving responsiveness and throughput.

## Process vs Thread

-   **Process:** An independent program with its own memory.
-   **Thread:** A lightweight execution path inside a process. Threads
    share the same heap but have separate stacks.

## Creating Threads

### Extending `Thread`

``` java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Hello from thread");
    }
}

new MyThread().start();
```

### Implementing `Runnable`

``` java
class Task implements Runnable {
    @Override
    public void run() {
        System.out.println("Running task");
    }
}

new Thread(new Task()).start();
```

### Using Lambdas

``` java
new Thread(() -> System.out.println("Lambda thread")).start();
```

## Thread Lifecycle

-   NEW
-   RUNNABLE
-   BLOCKED
-   WAITING
-   TIMED_WAITING
-   TERMINATED

## Important Methods

  Method          Description
  --------------- ----------------------------------------
  `start()`       Starts a new thread

  `run()`         Thread logic

  `join()`        Waits for another thread

  `sleep(ms)`     Pauses current thread

  `interrupt()`   Requests interruption

  `isAlive()`     Checks whether thread is still running

## Synchronization

``` java
class Counter {
    private int value;

    public synchronized void increment() {
        value++;
    }
}
```

### Lock Object

``` java
synchronized(lock) {
    // critical section
}
```

## Volatile

``` java
private volatile boolean running = true;
```

`volatile` guarantees visibility between threads but does not make
compound operations atomic.

## Atomic Classes

``` java
AtomicInteger counter = new AtomicInteger();
counter.incrementAndGet();
```

## Executor Framework

``` java
ExecutorService executor = Executors.newFixedThreadPool(4);

executor.submit(() -> {
    System.out.println("Task");
});

executor.shutdown();
```

## Virtual Threads (Java 21+)

``` java
Thread.startVirtualThread(() -> {
    System.out.println("Virtual thread");
});
```

Or:

``` java
Thread.ofVirtual().start(() -> {
    System.out.println("Running");
});
```

Virtual threads are lightweight and allow applications to efficiently
create thousands or even millions of concurrent tasks.

## Best Practices

-   Prefer `ExecutorService` or virtual threads over manually creating
    many platform threads.
-   Avoid shared mutable state.
-   Use synchronization only where necessary.
-   Prefer immutable objects.
-   Always shut down executors.
-   Handle interruptions correctly.

## Common Problems

-   Race conditions
-   Deadlocks
-   Starvation
-   Livelocks

## Summary

Java provides both platform threads and virtual threads for concurrent
programming. Modern Java applications should generally prefer virtual
threads for I/O-bound workloads and the Executor Framework for managing
task execution.
