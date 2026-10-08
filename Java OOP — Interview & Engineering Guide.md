# Java OOP — Interview & Engineering Guide

## 1. Why OOP Matters

Object-Oriented Programming (OOP) is a programming paradigm where software is modeled as a set of **objects that encapsulate state and behavior**.

In Java, OOP is not simply about creating classes. Good object-oriented design is about:

- Managing complexity
- Encapsulating implementation details
- Defining clear responsibilities
- Controlling dependencies
- Making systems extensible
- Reducing coupling
- Improving testability and maintainability

For a senior/lead engineer, the important question is not:

> "Do you know the four pillars of OOP?"

It is:

> "Can you use object-oriented principles to design a system that remains maintainable as requirements evolve?"

---

# 2. The Core Concepts

The four commonly cited pillars of OOP are:

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

However, in real-world Java development, several additional concepts are equally important:

- Composition
- Interfaces
- Immutability
- Dependency inversion
- Cohesion and coupling
- Object identity
- Equality
- Delegation

---

# 3. Classes and Objects

## 3.1 Class

A class is a blueprint defining:

- State
- Behavior
- Constraints

Example:

```java
public class BankAccount {

    private BigDecimal balance;

    public BankAccount(BigDecimal initialBalance) {
        if (initialBalance.signum() < 0) {
            throw new IllegalArgumentException("Balance cannot be negative");
        }

        this.balance = initialBalance;
    }

    public void deposit(BigDecimal amount) {
        if (amount.signum() <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }

        balance = balance.add(amount);
    }

    public BigDecimal getBalance() {
        return balance;
    }
}
```

The class defines what a `BankAccount` is allowed to do.

---

## 3.2 Object

An object is a runtime instance of a class.

```java
BankAccount account = new BankAccount(BigDecimal.valueOf(1000));
```

The object has:

- Identity
- State
- Behavior

Two objects can have identical state but still be different objects.

```java
BankAccount a = new BankAccount(BigDecimal.valueOf(1000));
BankAccount b = new BankAccount(BigDecimal.valueOf(1000));
```

`a` and `b` are different object instances.

---

# 4. Encapsulation

## 4.1 Definition

Encapsulation means **bundling data and the operations that control that data together while restricting inappropriate access to the internal state**.

The important idea is not simply:

> "Make fields private."

The deeper idea is:

> "An object should control its own invariants."

Bad design:

```java
public class BankAccount {
    public BigDecimal balance;
}
```

Any code can now do:

```java
account.balance = BigDecimal.valueOf(-100000);
```

The object cannot protect its own invariants.

Better:

```java
public class BankAccount {

    private BigDecimal balance;

    public void withdraw(BigDecimal amount) {
        if (amount.compareTo(balance) > 0) {
            throw new InsufficientFundsException();
        }

        balance = balance.subtract(amount);
    }
}
```

Now the object controls how its state changes.

---

## 4.2 Encapsulation Is About Invariants

An invariant is a condition that must always remain true.

For a bank account:

```text
balance >= 0
```

For an order:

```text
A shipped order cannot be cancelled.
```

For a trading position:

```text
quantity >= 0
```

A good object design makes it difficult or impossible for external code to violate these rules.

---

## 4.3 Getters and Setters Do Not Automatically Provide Good Encapsulation

This:

```java
private BigDecimal balance;

public BigDecimal getBalance() {
    return balance;
}

public void setBalance(BigDecimal balance) {
    this.balance = balance;
}
```

is technically private, but it may still be poorly encapsulated.

Why?

Because the class has effectively exposed unrestricted state mutation.

Better:

```java
public void deposit(BigDecimal amount) {
    // validation
}

public void withdraw(BigDecimal amount) {
    // validation
}
```

The object exposes **meaningful operations**, not raw state manipulation.

### Interview Insight

If asked:

> "What is encapsulation?"

A strong answer is:

> Encapsulation is not merely making fields private. It is about controlling access to an object's state and ensuring that the object itself protects its invariants and exposes meaningful behavior instead of arbitrary state mutation.

---

# 5. Abstraction

Abstraction means exposing the **essential behavior** of an object while hiding unnecessary implementation details.

For example:

```java
public interface PaymentProcessor {

    PaymentResult process(Payment payment);
}
```

The caller does not need to know whether payment processing uses:

- REST
- gRPC
- a database
- a third-party provider
- a message broker

It only depends on the abstraction.

```java
public class CheckoutService {

    private final PaymentProcessor paymentProcessor;

    public CheckoutService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
}
```

This reduces coupling.

---

# 6. Encapsulation vs Abstraction

These concepts are related but different.

### Encapsulation

Focuses on:

> How do we protect and control internal state?

### Abstraction

Focuses on:

> What should the outside world need to know?

Example:

```java
interface PaymentProcessor {
    PaymentResult process(Payment payment);
}
```

The interface is an abstraction.

The implementation:

```java
class StripePaymentProcessor implements PaymentProcessor {
    // implementation details
}
```

encapsulates how the payment is actually processed.

### Interview Answer

> Encapsulation is primarily about controlling access to state and behavior and protecting invariants, while abstraction is about exposing a simplified contract and hiding unnecessary implementation details.

---

# 7. Inheritance

Inheritance allows one class to derive behavior and structure from another.

```java
public class Animal {
    public void move() {
        System.out.println("Moving");
    }
}

public class Dog extends Animal {
}
```

Now:

```java
Dog dog = new Dog();
dog.move();
```

`Dog` inherits `move()`.

---

# 8. Is-A Relationship

Inheritance should represent a valid **is-a relationship**.

```text
Dog is an Animal
Car is a Vehicle
SavingsAccount is an Account
```

But:

```text
Car is an Engine
```

is obviously incorrect.

A car **has an engine**, so composition is more appropriate.

---

# 9. Method Overriding

A subclass can provide a specialized implementation of an inherited method.

```java
class Animal {

    public void makeSound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {

    @Override
    public void makeSound() {
        System.out.println("Bark");
    }
}
```

The method signature remains compatible with the parent method.

---

# 10. Method Overloading

Overloading means multiple methods have the same name but different parameter lists.

```java
public void send(String message) {
}

public void send(String message, String recipient) {
}

public void send(String message, String recipient, Priority priority) {
}
```

Overloading is resolved at **compile time**.

This is different from overriding, which participates in runtime polymorphism.

---

# 11. Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Same method name | Yes | Yes |
| Parameters | Must differ | Must be compatible |
| Relationship | Usually same class | Parent/child |
| Resolution | Compile time | Runtime |
| Polymorphism | Compile-time | Runtime |

---

# 12. Polymorphism

Polymorphism means:

> One interface/type can represent multiple concrete implementations.

Example:

```java
interface NotificationSender {

    void send(String message);
}
```

Implementations:

```java
class EmailSender implements NotificationSender {

    @Override
    public void send(String message) {
        // email
    }
}
```

```java
class SmsSender implements NotificationSender {

    @Override
    public void send(String message) {
        // SMS
    }
}
```

Now:

```java
NotificationSender sender = new EmailSender();
sender.send("Hello");
```

The reference type is:

```java
NotificationSender
```

but the actual object is:

```java
EmailSender
```

At runtime Java invokes the implementation corresponding to the actual object.

---

# 13. Runtime Polymorphism

Consider:

```java
NotificationSender sender;

if (condition) {
    sender = new EmailSender();
} else {
    sender = new SmsSender();
}

sender.send("Hello");
```

The caller does not need:

```java
if (sender instanceof EmailSender) {
    ...
}
```

This is one of the major benefits of polymorphism.

Instead of:

```java
if (type == EMAIL) {
    ...
} else if (type == SMS) {
    ...
} else if (type == PUSH) {
    ...
}
```

we can use:

```java
sender.send(message);
```

and let the concrete implementation decide how to perform the operation.

---

# 14. Static Type vs Runtime Type

This is an important Java interview topic.

```java
Animal animal = new Dog();
```

There are two relevant types:

### Compile-time / declared type

```java
Animal
```

### Runtime / actual type

```java
Dog
```

The compiler determines what members are accessible through the reference.

The JVM determines which overridden implementation executes at runtime.

---

# 15. Composition

Composition means constructing an object from other objects.

Example:

```java
class OrderService {

    private final PaymentProcessor paymentProcessor;
    private final InventoryService inventoryService;

    public OrderService(
            PaymentProcessor paymentProcessor,
            InventoryService inventoryService) {

        this.paymentProcessor = paymentProcessor;
        this.inventoryService = inventoryService;
    }
}
```

`OrderService` has dependencies rather than inheriting from them.

This is a **has-a** relationship.

---

# 16. Composition vs Inheritance

A classic interview question:

> "Why is composition often preferred over inheritance?"

Inheritance creates a strong coupling between parent and child.

For example:

```java
class OrderService extends PaymentService
```

means `OrderService` inherits the structure and behavior of `PaymentService`.

But perhaps an order service simply needs to **use** payment functionality.

Composition is more flexible:

```java
class OrderService {

    private final PaymentService paymentService;
}
```

Now the dependency can be changed or replaced without changing the inheritance hierarchy.

### General Rule

Prefer:

> Composition for code reuse and collaboration.

Use inheritance when:

> There is a genuine subtype relationship and the child can correctly substitute for the parent.

---

# 17. The Fragile Base Class Problem

Inheritance can create unexpected coupling.

Suppose:

```java
class BaseService {
    protected void process() {
        // logic
    }
}
```

Many subclasses depend on assumptions about `process()`.

Later, changing `BaseService` can unintentionally break subclasses.

This is one reason deep inheritance hierarchies are generally discouraged.

A shallow hierarchy is usually easier to understand and maintain.

---

# 18. Interfaces

An interface defines a contract.

```java
public interface UserRepository {

    Optional<User> findById(Long id);

    User save(User user);
}
```

Implementation:

```java
public class JpaUserRepository implements UserRepository {
    ...
}
```

The consumer depends on the interface:

```java
public class UserService {

    private final UserRepository repository;
}
```

This reduces coupling to a concrete implementation.

---

# 19. Interface vs Abstract Class

Both can be used for abstraction, but they serve somewhat different purposes.

## Interface

Best for defining capabilities or contracts.

```java
interface PaymentProcessor {
    PaymentResult process(Payment payment);
}
```

A class can implement multiple interfaces.

```java
class PaymentService
        implements PaymentProcessor, Auditable {
}
```

## Abstract Class

Useful when related classes share:

- State
- Implementation
- Common behavior

Example:

```java
abstract class AbstractPaymentProcessor {

    protected final Logger logger;

    protected AbstractPaymentProcessor(Logger logger) {
        this.logger = logger;
    }

    protected void audit() {
        ...
    }

    public abstract PaymentResult process(Payment payment);
}
```

---

# 20. Modern Java Interfaces

Interfaces can contain:

### Abstract methods

```java
void process();
```

### Default methods

```java
default void validate() {
}
```

### Static methods

```java
static PaymentProcessor create() {
    ...
}
```

They still cannot normally maintain per-instance state like a regular class.

---

# 21. Multiple Inheritance

Java does not support multiple inheritance of classes.

This is illegal:

```java
class C extends A, B {
}
```

One reason is ambiguity.

If both `A` and `B` provide:

```java
void process()
```

which implementation should `C` inherit?

Java allows multiple interface inheritance/implementation:

```java
class C implements A, B {
}
```

If conflicting default methods exist, the implementing class must resolve the conflict.

---

# 22. The Diamond Problem

Conceptually:

```text
       A
      / \
     B   C
      \ /
       D
```

If both `B` and `C` inherit behavior from `A`, and `D` inherits from both, ambiguity can arise.

Java avoids this for classes by disallowing multiple class inheritance.

---

# 23. Access Modifiers

Java provides:

| Modifier | Accessible from |
|---|---|
| `private` | Same class |
| package-private | Same package |
| `protected` | Same package + subclasses |
| `public` | Everywhere |

### Recommended Principle

Start with the **smallest visibility possible**.

Prefer:

```java
private
```

over:

```java
public
```

unless public access is part of the intended API.

---

# 24. `protected` and Encapsulation

`protected` is often overused.

If subclasses need access to implementation details, you may be increasing coupling.

Instead of:

```java
protected Database database;
```

consider exposing controlled behavior.

```java
protected void executeQuery(...) {
}
```

Or use composition where appropriate.

---

# 25. `final`

`final` has several meanings.

## Final variable

```java
final int x = 10;
```

The variable cannot be reassigned.

## Final reference

```java
final User user = ...
```

The reference cannot point to another object.

But the object itself may still be mutable.

```java
user.setName("Ali");
```

can still work.

## Final method

```java
public final void process() {
}
```

Subclasses cannot override it.

## Final class

```java
public final class Money {
}
```

The class cannot be extended.

---

# 26. Immutability

An immutable object cannot change its state after construction.

Example:

```java
public final class Money {

    private final BigDecimal amount;
    private final Currency currency;

    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }

    public BigDecimal getAmount() {
        return amount;
    }

    public Currency getCurrency() {
        return currency;
    }
}
```

Once constructed, the object cannot be modified.

---

# 27. Why Immutability Matters

Immutable objects provide several benefits:

- Easier reasoning
- Thread safety
- Safer sharing
- Easier testing
- Stable hash codes
- Fewer accidental side effects

They are especially valuable in concurrent and distributed systems.

---

# 28. How to Design an Immutable Class

Common guidelines:

1. Make the class `final` unless controlled inheritance is intentional.
2. Make fields `private final`.
3. Initialize all state through the constructor.
4. Do not provide setters.
5. Do not expose mutable internal objects directly.
6. Make defensive copies where necessary.

Example:

```java
public final class User {

    private final String name;
    private final List<String> roles;

    public User(String name, List<String> roles) {
        this.name = name;
        this.roles = List.copyOf(roles);
    }

    public String getName() {
        return name;
    }

    public List<String> getRoles() {
        return roles;
    }
}
```

`List.copyOf()` prevents external mutation through the original list.

---

# 29. Equality: `==` vs `equals()`

Another classic Java interview question.

For objects:

```java
a == b
```

checks whether both references point to the **same object**.

```java
a.equals(b)
```

checks logical equality according to the class's implementation.

Example:

```java
String a = new String("hello");
String b = new String("hello");
```

Then:

```java
a == b
```

is generally `false`.

But:

```java
a.equals(b)
```

is `true`.

---

# 30. The `equals()` Contract

A correct `equals()` implementation should satisfy:

### Reflexive

```text
x.equals(x) == true
```

### Symmetric

```text
x.equals(y) == y.equals(x)
```

### Transitive

If:

```text
x == y
y == z
```

then:

```text
x == z
```

### Consistent

Repeated calls should produce the same result if relevant state has not changed.

### Non-null

```text
x.equals(null) == false
```

---

# 31. `hashCode()` Contract

If:

```java
a.equals(b)
```

is true, then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

The reverse is not required.

Two unequal objects can have the same hash code.

This matters for:

```java
HashMap
HashSet
ConcurrentHashMap
```

---

# 32. A Classic Bug

Bad:

```java
class User {

    private Long id;

    @Override
    public boolean equals(Object o) {
        ...
    }
}
```

but no `hashCode()`.

Now:

```java
Set<User> users = new HashSet<>();
```

may behave unexpectedly.

If you override `equals()`, you generally need to override `hashCode()` consistently.

---

# 33. Records

Modern Java provides `record` for concise immutable data carriers.

```java
public record UserDto(
        Long id,
        String name
) {}
```

Records automatically provide:

- Accessors
- `equals()`
- `hashCode()`
- `toString()`

Records are excellent for:

- DTOs
- Value objects
- Immutable data carriers

But they are not automatically appropriate for every domain object.

---

# 34. Entity vs Value Object

This distinction becomes important in domain-driven design.

## Entity

Has identity.

Example:

```text
User #123
```

Even if its name changes, it remains the same entity.

## Value Object

Defined by its value.

Example:

```text
Money(100, USD)
```

Two `Money(100, USD)` objects can be considered equal even though they are different instances.

Immutable value objects are particularly useful for domain modeling.

---

# 35. Association, Aggregation, Composition

These terms describe relationships between objects.

### Association

Objects know or interact with each other.

```text
Doctor ---- Patient
```

### Aggregation

A weaker whole-part relationship.

```text
Team ---- Player
```

A player can exist independently of the team.

### Composition

A stronger ownership relationship.

```text
Order ---- OrderLine
```

An `OrderLine` conceptually belongs to its order.

In modern Java development, these distinctions are mostly useful for modeling rather than enforcing special language constructs.

---

# 36. Coupling

Coupling describes how strongly components depend on each other.

High coupling:

```java
class OrderService {

    private final StripeClient stripeClient;
}
```

If the service directly depends on a specific provider everywhere, changing providers becomes harder.

Lower coupling:

```java
class OrderService {

    private final PaymentProcessor paymentProcessor;
}
```

Now:

```text
OrderService
     |
PaymentProcessor
   /       \
Stripe     Adyen
```

The implementation can change without changing the consumer.

---

# 37. Cohesion

Cohesion describes how closely related the responsibilities of a module/class are.

High cohesion:

```java
class PasswordHasher {
    hash(...)
    verify(...)
}
```

Low cohesion:

```java
class UserService {
    createUser()
    sendEmail()
    generatePdf()
    calculateTax()
    uploadImage()
    createBackup()
}
```

A class should ideally have a focused responsibility.

---

# 38. SOLID

SOLID is a collection of object-oriented design principles.

- S — Single Responsibility Principle
- O — Open/Closed Principle
- L — Liskov Substitution Principle
- I — Interface Segregation Principle
- D — Dependency Inversion Principle

These principles are more useful when understood as **ways of managing change and dependencies**, rather than rigid rules.

---

# 39. Single Responsibility Principle

SRP is often incorrectly explained as:

> "A class should have only one method."

That is wrong.

A better interpretation:

> A class should have one coherent responsibility or one primary reason to change.

Bad:

```java
class UserService {

    createUser();
    sendWelcomeEmail();
    generateInvoice();
    exportCsv();
}
```

Better separation:

```text
UserService
EmailService
InvoiceService
CsvExporter
```

---

# 40. Open/Closed Principle

A system should be:

> Open for extension, closed for modification.

Suppose:

```java
interface DiscountStrategy {
    BigDecimal calculate(Order order);
}
```

Implementations:

```java
class RegularDiscount implements DiscountStrategy {}
class VipDiscount implements DiscountStrategy {}
class SeasonalDiscount implements DiscountStrategy {}
```

Adding a new discount should ideally require adding a new implementation rather than rewriting a giant conditional.

---

# 41. Liskov Substitution Principle

If:

```java
Dog extends Animal
```

then code expecting an `Animal` should be able to work correctly with a `Dog`.

Formally:

> Subtypes must be substitutable for their base types without breaking the correctness of the program.

Classic problematic example:

```java
class Bird {
    void fly() {}
}

class Penguin extends Bird {
    @Override
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```

The abstraction is wrong.

Not every bird can fly.

Better:

```java
interface Bird {}

interface FlyingBird extends Bird {
    void fly();
}
```

---

# 42. Interface Segregation Principle

Clients should not be forced to depend on methods they do not use.

Bad:

```java
interface Worker {
    void work();
    void eat();
    void sleep();
}
```

A machine may need `work()` but not `eat()`.

Better:

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}
```

Small, focused interfaces are often easier to use and mock.

---

# 43. Dependency Inversion Principle

High-level modules should not depend directly on low-level implementation details.

Instead:

```text
High-level policy
       |
   abstraction
       |
Low-level detail
```

Example:

```java
class OrderService {

    private final PaymentProcessor paymentProcessor;
}
```

instead of:

```java
class OrderService {

    private final StripePaymentClient stripeClient;
}
```

The abstraction allows the business logic to remain independent from infrastructure details.

---

# 44. Dependency Injection

Dependency Injection is a technique for implementing loose coupling.

Instead of:

```java
class OrderService {

    private PaymentProcessor processor =
        new StripePaymentProcessor();
}
```

use:

```java
class OrderService {

    private final PaymentProcessor processor;

    public OrderService(PaymentProcessor processor) {
        this.processor = processor;
    }
}
```

Now the dependency is provided from outside.

Spring uses dependency injection extensively.

---

# 45. Why Constructor Injection Is Usually Preferred

Example:

```java
@Service
public class OrderService {

    private final PaymentProcessor paymentProcessor;

    public OrderService(PaymentProcessor paymentProcessor) {
        this.paymentProcessor = paymentProcessor;
    }
}
```

Benefits:

- Dependencies are explicit.
- Fields can be `final`.
- Object cannot easily exist in an invalid partially initialized state.
- Easier unit testing.
- Better immutability.

---

# 46. Dependency Inversion vs Dependency Injection

They are not the same thing.

### Dependency Inversion

A design principle.

### Dependency Injection

A technique for providing dependencies.

You can use dependency injection to implement dependency inversion.

---

# 47. Tell, Don't Ask

A useful OO principle is:

> Tell an object what to do instead of asking for its state and manipulating it externally.

Bad:

```java
if (order.getStatus() == Status.PENDING) {
    order.setStatus(Status.PAID);
}
```

Better:

```java
order.markAsPaid();
```

The second design keeps business rules inside the object.

---

# 48. Law of Demeter

A common formulation:

> An object should have limited knowledge of the structure of other objects.

Bad:

```java
order.getCustomer()
     .getAddress()
     .getCity()
     .getCountry()
```

This creates chains of knowledge and coupling.

Sometimes this is perfectly acceptable for simple data access, but long chains can indicate poor boundaries.

---

# 49. Anemic Domain Model

An anemic domain model stores data in objects while putting all behavior in services.

Example:

```java
class Account {
    private BigDecimal balance;

    // getters/setters only
}
```

Then:

```java
class AccountService {

    void withdraw(Account account, BigDecimal amount) {
        // all business rules
    }
}
```

This can be appropriate for CRUD-oriented applications.

But for complex business domains, richer domain objects can better encapsulate invariants:

```java
account.withdraw(amount);
```

The right choice depends on domain complexity and architecture.

---

# 50. Mutable vs Immutable Objects

Mutable:

```java
user.setName("John");
```

Immutable:

```java
User updated = user.withName("John");
```

Immutability is generally easier to reason about, especially in concurrent systems.

However, mutability can be practical and efficient when carefully controlled.

The goal is not:

> "Everything must be immutable."

The goal is:

> "Mutation should be intentional and controlled."

---

# 51. Object-Oriented Design in Backend Systems

In a backend system, OOP is usually applied across several layers.

Example:

```text
Controller
    |
Application Service
    |
Domain Model
    |
Repository Interface
    |
Infrastructure Implementation
    |
Database
```

A good design keeps responsibilities separated.

For example:

```java
@RestController
class PaymentController {
}
```

should not contain:

```text
business rules
database queries
third-party payment logic
```

Instead:

```text
Controller
    ↓
PaymentService
    ↓
PaymentProcessor
    ↓
PaymentRepository
```

---

# 52. OOP and Clean Architecture

A useful architectural principle is:

> Business rules should not depend directly on infrastructure details.

For example:

```text
              ┌─────────────────┐
              │   Controller    │
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ Application     │
              │    Service      │
              └────────┬────────┘
                       ↓
              ┌─────────────────┐
              │ Domain / Ports  │
              └────────┬────────┘
                       ↓
        ┌──────────────┴──────────────┐
        ↓                             ↓
 Database Adapter              Payment Adapter
```

The domain/application layer depends on abstractions rather than concrete infrastructure.

This is essentially dependency inversion applied at architectural scale.

---

# 53. Polymorphism vs Conditional Logic

Suppose:

```java
if (paymentType == CARD) {
    ...
} else if (paymentType == BANK_TRANSFER) {
    ...
} else if (paymentType == CRYPTO) {
    ...
}
```

If new payment methods are frequently added, this may become difficult to maintain.

Polymorphism can move the variation behind an abstraction:

```java
interface PaymentMethod {
    PaymentResult pay(Payment payment);
}
```

Then:

```java
CardPayment
BankTransferPayment
CryptoPayment
```

can implement it independently.

But polymorphism should not be used mechanically.

If the variation is small, stable, and simple, a conditional may actually be clearer.

### Senior-level insight

> Good design is not about eliminating `if` statements. It is about identifying where variation is likely to occur and isolating that variation behind an appropriate boundary.

---

# 54. OOP and Design Patterns

Design patterns are reusable solutions to common design problems.

Important patterns for Java backend interviews include:

### Strategy

Encapsulates interchangeable algorithms.

```java
interface PricingStrategy {
    BigDecimal calculate(Order order);
}
```

### Factory

Encapsulates object creation.

```java
PaymentProcessor processor =
    PaymentProcessorFactory.create(type);
```

### Builder

Useful for constructing complex objects.

```java
User user = User.builder()
        .name("John")
        .email("john@example.com")
        .build();
```

### Adapter

Makes incompatible interfaces work together.

```text
Your system
    ↓
Adapter
    ↓
Third-party API
```

### Decorator

Adds behavior without modifying the original object.

```text
PaymentProcessor
      ↓
LoggingDecorator
      ↓
RetryDecorator
      ↓
RealProcessor
```

### Observer

Allows objects to react to events.

In backend systems this idea appears in:

- Event listeners
- Domain events
- Message brokers

---

# 55. Inheritance vs Interface vs Composition

A useful decision framework:

### Use inheritance when:

- There is a genuine subtype relationship.
- The subclass satisfies LSP.
- Shared behavior is truly intrinsic to the hierarchy.
- The hierarchy is stable.

### Use interfaces when:

- You need a contract.
- Multiple implementations are expected.
- You want to decouple consumers from implementations.

### Use composition when:

- You need to reuse functionality.
- Dependencies may change.
- You want flexibility.
- There is a has-a relationship.

---

# 56. Common OOP Interview Questions

## Basic

### Q: What are the four pillars of OOP?

Strong answer:

> Encapsulation, abstraction, inheritance, and polymorphism. Encapsulation protects state and invariants, abstraction exposes a simplified contract, inheritance models subtype relationships, and polymorphism allows the same abstraction to work with different concrete implementations.

---

### Q: Encapsulation vs abstraction?

> Encapsulation focuses on controlling and protecting an object's internal state and behavior. Abstraction focuses on exposing only the relevant contract while hiding unnecessary implementation details.

---

### Q: Overloading vs overriding?

> Overloading means defining multiple methods with the same name but different parameter lists and is resolved at compile time. Overriding occurs when a subclass provides a compatible implementation of an inherited method and is resolved dynamically at runtime.

---

### Q: Why prefer composition over inheritance?

> Composition generally creates weaker coupling and gives us more flexibility to change behavior independently. Inheritance is appropriate when there is a genuine subtype relationship and substitutability is preserved.

---

### Q: Interface vs abstract class?

> An interface primarily defines a contract or capability and allows a class to implement multiple interfaces. An abstract class is more appropriate when closely related classes share state or implementation in addition to an abstraction.

---

### Q: What is polymorphism?

> Polymorphism allows code to work against an abstraction while the actual behavior is provided by different concrete implementations. This lets us replace implementations without changing the consuming code.

---

### Q: What is SOLID?

> SOLID is a set of design principles that help manage coupling, responsibilities, dependencies, and change in object-oriented systems.

Then explain each principle with a short example.

---

# 57. Advanced Interview Questions

## Q: Can you explain why inheritance increases coupling?

Because a subclass depends on:

- Parent behavior
- Parent implementation details
- Parent lifecycle assumptions
- Parent protected members
- Parent contracts

Changes in the base class can therefore affect many subclasses.

---

## Q: Is inheritance always bad?

No.

Inheritance is appropriate when:

```text
B is genuinely an A
```

and the subtype can correctly substitute for the parent.

The problem is not inheritance itself.

The problem is using inheritance primarily for code reuse when composition would better represent the relationship.

---

## Q: Does encapsulation mean all fields must be private?

Not necessarily as an absolute language rule.

But private state is usually a good default because it allows the class to control how state is accessed and modified.

The deeper goal is protecting invariants and controlling coupling.

---

## Q: Is OOP always the best paradigm?

No.

Different problems can benefit from different paradigms.

Java itself supports:

- Object-oriented programming
- Functional programming
- Imperative programming

For example, Java Streams encourage functional-style transformations.

The right paradigm depends on the problem.

---

# 58. Senior-Level Perspective: OOP Is About Managing Change

This is probably the most important concept for a Lead Backend interview.

A junior explanation of OOP is:

> "OOP has four pillars."

A stronger explanation is:

> "OOP provides mechanisms for modeling behavior and controlling dependencies. Good OO design helps isolate change, protect invariants, and keep responsibilities cohesive."

An even stronger engineering perspective:

> "The value of abstraction and polymorphism is not abstraction for its own sake. They are useful when they isolate areas of volatility and allow the rest of the system to remain stable."

---

# 59. Example: Designing a Payment System

Suppose the requirements are:

```text
Support:
- Credit Card
- Bank Transfer
- Wallet
```

A naive design:

```java
class PaymentService {

    void pay(Payment payment) {

        switch (payment.getType()) {

            case CARD:
                // card logic
                break;

            case BANK_TRANSFER:
                // bank logic
                break;

            case WALLET:
                // wallet logic
                break;
        }
    }
}
```

This works.

But every new payment type requires modifying `PaymentService`.

Alternative:

```java
interface PaymentProcessor {

    PaymentResult process(Payment payment);
}
```

Implementations:

```text
CardPaymentProcessor
BankTransferProcessor
WalletPaymentProcessor
```

Now the core service depends on:

```java
PaymentProcessor
```

rather than concrete implementations.

This gives:

- Lower coupling
- Better extensibility
- Easier testing
- Clearer responsibilities

But if there are only two stable payment types and no expected variation, introducing multiple classes may be unnecessary complexity.

This is an important senior-level principle:

> Design for expected change, not hypothetical change.

---

# 60. Common Mistakes in OOP Interviews

Avoid these answers:

### Mistake 1

> "Encapsulation means private fields."

Incomplete.

### Mistake 2

> "Inheritance is for code reuse."

Too shallow.

The primary conceptual purpose is subtype modeling.

### Mistake 3

> "SOLID means every class should have one method."

Incorrect.

### Mistake 4

> "Composition is always better than inheritance."

Too absolute.

### Mistake 5

> "Interfaces are always better than abstract classes."

Also too absolute.

### Mistake 6

Creating interfaces for everything:

```java
UserServiceInterface
UserServiceImpl
```

without an actual need for multiple implementations or a meaningful abstraction.

This can create unnecessary complexity.

---

# 61. Practical Rules to Remember

When designing Java classes:

### 1. Keep responsibilities focused

High cohesion.

### 2. Minimize unnecessary dependencies

Low coupling.

### 3. Hide implementation details

Use encapsulation and abstraction.

### 4. Prefer composition for reuse

Unless inheritance expresses a real subtype relationship.

### 5. Depend on abstractions where useful

Especially at architectural boundaries.

### 6. Protect invariants

Don't expose unrestricted mutable state.

### 7. Prefer immutable value objects when appropriate

Especially for domain concepts.

### 8. Use polymorphism where behavior genuinely varies

Don't create abstractions just to look sophisticated.

### 9. Keep interfaces focused

Follow ISP.

### 10. Design around change

Ask:

> "What is likely to change?"

and isolate that part of the system.

---

# 62. Interview Cheat Sheet

| Concept | One-line explanation |
|---|---|
| Encapsulation | Protect state and control how it changes |
| Abstraction | Expose the essential contract and hide unnecessary details |
| Inheritance | Model a genuine subtype relationship |
| Polymorphism | Use one abstraction with multiple implementations |
| Composition | Build behavior by collaborating with other objects |
| Interface | Define a contract/capability |
| Abstract class | Share abstraction, state, and implementation |
| Cohesion | How closely related a component's responsibilities are |
| Coupling | How strongly components depend on each other |
| SRP | One coherent responsibility/reason to change |
| OCP | Extend behavior without repeatedly modifying stable code |
| LSP | Subtypes must remain substitutable for their base types |
| ISP | Don't force clients to depend on methods they don't need |
| DIP | High-level policy should depend on abstractions, not details |
| Immutability | Object state cannot change after construction |
| Entity | Object identified primarily by identity |
| Value Object | Object identified by its value |

---

# 63. The Mental Model to Take Into the Interview

When you see an OOP design problem, don't immediately think:

```text
Class
↓
Inheritance
↓
Interface
↓
Pattern
```

Instead ask:

```text
What are the responsibilities?
        ↓
What are the invariants?
        ↓
What changes frequently?
        ↓
What should be hidden?
        ↓
Where should the dependencies point?
        ↓
Is this relationship "is-a" or "has-a"?
        ↓
Do I need inheritance, composition, or an interface?
        ↓
How easy will this design be to test and evolve?
```

That mindset is much closer to what a **Lead Backend Engineer** is expected to demonstrate.

---

# 64. Final Interview-Level Summary

If an interviewer asks:

> "What does good OOP mean to you?"

A strong answer would be:

> Good OOP, in my view, is less about using classes or following the four pillars mechanically and more about managing complexity and change. I want objects to have clear responsibilities, protect their own invariants, and expose meaningful contracts. I generally prefer composition and interfaces when they reduce coupling, and I use inheritance when there is a real subtype relationship and Liskov substitution holds. I also try to keep dependencies pointing toward stable abstractions rather than implementation details. Ultimately, I evaluate an OO design by how cohesive, testable, understandable, and adaptable it is as the system evolves.

That is the level of answer that moves the discussion from **"I know Java"** toward **"I know how to engineer software with Java."**