# Java Collections — Senior / Tech Lead Interview Guide

## 1. Java Collections Framework Overview

The **Java Collections Framework (JCF)** is a unified architecture for storing, manipulating, and processing groups of objects.

It mainly consists of:

- Interfaces
- Implementations
- Algorithms

The main interfaces are:

```text
Collection
├── List
│   ├── ArrayList
│   └── LinkedList
│
├── Set
│   ├── HashSet
│   ├── LinkedHashSet
│   └── SortedSet
│       └── NavigableSet
│           └── TreeSet
│
└── Queue
    ├── PriorityQueue
    └── Deque
        ├── ArrayDeque
        └── LinkedList
```

`Map` is part of the Collections Framework, but technically it does **not** extend `Collection`.

```text
Map
├── HashMap
├── LinkedHashMap
├── SortedMap
│   └── NavigableMap
│       └── TreeMap
└── ConcurrentMap
    └── ConcurrentHashMap
```

---

# 2. Collection vs Collections

A common interview question.

### Collection

`Collection` is an **interface** representing a group of objects.

Examples:

```java
Collection<String> values = new ArrayList<>();
```

### Collections

`Collections` is a **utility class** containing static helper methods.

Examples:

```java
Collections.sort(list);
Collections.reverse(list);
Collections.shuffle(list);
Collections.unmodifiableList(list);
```

So:

```text
Collection  -> Interface
Collections -> Utility class
```

---

# 3. List

A `List`:

- preserves insertion order
- allows duplicate elements
- provides indexed access
- usually allows `null`

Important implementations:

```text
ArrayList
LinkedList
```

---

# 4. ArrayList

`ArrayList` is probably the most commonly used `List` implementation.

Internally, it is backed by a **resizable array**.

Example:

```java
List<String> names = new ArrayList<>();

names.add("Ali");
names.add("Sara");
names.add("Reza");
```

Conceptually:

```text
ArrayList
      |
      v
Object[]

+------+-------+-------+------+
| Ali  | Sara  | Reza  | null |
+------+-------+-------+------+
```

---

## 4.1 ArrayList Time Complexity

Typical operations:

| Operation | Complexity |
|---|---:|
| `get(index)` | O(1) |
| `set(index)` | O(1) |
| append `add()` | O(1) amortized |
| insert in middle | O(n) |
| remove from middle | O(n) |
| contains | O(n) |
| indexOf | O(n) |

Random access is fast because Java can calculate the array position directly.

```java
list.get(500);
```

is approximately:

```text
baseAddress + index
```

---

# 5. How ArrayList Grows Internally

An `ArrayList` has a backing array with a certain **capacity**.

Suppose:

```text
size = 10
capacity = 10
```

When another element is added, the array cannot grow directly because Java arrays have fixed size.

Therefore, `ArrayList`:

1. allocates a larger array
2. copies the existing elements
3. inserts the new element
4. discards the old array

Conceptually:

```text
Old array

[A][B][C][D]

        |
        | resize
        v

New larger array

[A][B][C][D][ ][ ]
```

Modern Java implementations generally grow the backing array by roughly **50%** when additional capacity is required.

However, interview-wise, the more important concept is:

> ArrayList uses a dynamically resized array, and resizing requires allocation and element copying.

---

# 6. Why is ArrayList.add() O(1) if resizing is O(n)?

Because the complexity is **amortized O(1)**.

Most insertions:

```text
O(1)
```

Occasionally, resizing happens:

```text
O(n)
```

But resizing does not happen on every insertion.

Over many insertions, the average cost per insertion becomes approximately constant.

This concept is called:

```text
Amortized Complexity
```

---

# 7. ArrayList Capacity Optimization

If you know approximately how many elements will be inserted:

```java
List<User> users = new ArrayList<>(100_000);
```

This avoids repeated array reallocations.

You can also use:

```java
arrayList.ensureCapacity(100_000);
```

This can matter in performance-sensitive systems.

---

# 8. ArrayList Removal Cost

Consider:

```text
[A][B][C][D][E]
```

Removing `B`:

```java
list.remove(1);
```

requires shifting later elements:

```text
[A][C][D][E]
```

Therefore:

```text
remove(index) = O(n)
```

---

# 9. ArrayList Thread Safety

`ArrayList` is **not thread-safe**.

Multiple threads modifying the same list concurrently may cause race conditions.

A synchronized wrapper can be created:

```java
List<String> list =
        Collections.synchronizedList(new ArrayList<>());
```

But in highly concurrent environments, other data structures may be more appropriate.

For example:

```java
CopyOnWriteArrayList
```

---

# 10. CopyOnWriteArrayList

`CopyOnWriteArrayList` is a thread-safe List implementation.

Its strategy is:

> Every modifying operation creates a new copy of the underlying array.

For example:

```java
list.add(value);
```

causes the underlying array to be copied.

This means:

### Reads

Very efficient.

### Writes

Expensive.

Therefore, it is suitable when:

```text
Reads >> Writes
```

Examples:

- listeners
- configuration snapshots
- observer lists
- rarely modified shared data

It is usually a poor choice for write-heavy workloads.

---

# 11. LinkedList

`LinkedList` is implemented as a **doubly linked list**.

Each node contains:

```text
previous
value
next
```

Example:

```text
null <- [A] <-> [B] <-> [C] -> null
```

Conceptually:

```java
class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

---

# 12. LinkedList Complexity

| Operation | Complexity |
|---|---:|
| add first | O(1) |
| add last | O(1) |
| remove first | O(1) |
| remove last | O(1) |
| get(index) | O(n) |
| contains | O(n) |
| insert when node is already known | O(1) |

---

# 13. Why is LinkedList.get(index) O(n)?

Because there is no direct indexed access.

For:

```java
list.get(500);
```

Java must traverse nodes until reaching index 500.

Conceptually:

```text
node0 -> node1 -> node2 -> ... -> node500
```

Therefore:

```text
O(n)
```

---

# 14. ArrayList vs LinkedList

A classic interview question.

| Feature | ArrayList | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Doubly linked list |
| Random access | O(1) | O(n) |
| Append | O(1) amortized | O(1) |
| Insert middle | O(n) | potentially O(1) after locating node |
| Remove middle | O(n) | potentially O(1) after locating node |
| Memory overhead | lower | higher |
| CPU cache locality | good | poor |

A common oversimplification is:

> Use LinkedList when you have many inserts and deletes.

This is not always true.

Even for inserts/deletes, you usually first need to **find the node**, which is O(n).

Also, modern CPUs strongly benefit from contiguous memory.

`ArrayList` has much better **cache locality**.

Therefore, in real applications:

> `ArrayList` is usually the default List unless there is a specific reason to choose LinkedList.

That is a stronger senior-level answer than simply comparing Big-O.

---

# 15. Why LinkedList May Be Slower Despite O(1) Removal

LinkedList nodes are allocated separately in memory.

Conceptually:

```text
Node A -> Node B -> Node C
```

but physically they may be scattered throughout the heap.

This causes:

- pointer chasing
- CPU cache misses
- additional memory overhead
- more GC pressure

ArrayList stores references contiguously:

```text
[A][B][C][D]
```

which gives much better CPU cache locality.

This is an important senior-level performance consideration.

---

# 16. Deque

`Deque` means:

```text
Double Ended Queue
```

Elements can be added or removed from both ends.

Important implementation:

```java
ArrayDeque
```

Example:

```java
Deque<String> queue = new ArrayDeque<>();

queue.addFirst("A");
queue.addLast("B");

queue.removeFirst();
queue.removeLast();
```

---

# 17. Stack — Do Not Prefer java.util.Stack

Legacy Java has:

```java
Stack<E>
```

but modern Java code should usually use:

```java
Deque<E>
```

For example:

```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);

int value = stack.pop();
```

Why?

`Stack` is a legacy class extending `Vector`, with old synchronization behavior.

`ArrayDeque` is generally preferred.

---

# 18. Queue

A queue normally follows:

```text
FIFO
First In, First Out
```

Example:

```java
Queue<String> queue = new ArrayDeque<>();

queue.offer("A");
queue.offer("B");

queue.poll();
```

Result:

```text
A
```

---

# 19. add vs offer

Queue APIs provide both:

```java
add()
offer()
```

The difference becomes relevant for capacity-restricted queues.

`add()` may throw an exception if insertion fails.

`offer()` returns:

```text
false
```

when insertion cannot happen.

Similarly:

```text
remove() -> throws exception if empty
poll()   -> returns null if empty

element() -> throws exception if empty
peek()    -> returns null if empty
```

---

# 20. PriorityQueue

`PriorityQueue` does **not** guarantee FIFO ordering.

Elements are retrieved according to priority.

Internally it is usually implemented using a:

```text
Binary Heap
```

Example:

```java
PriorityQueue<Integer> queue = new PriorityQueue<>();

queue.offer(30);
queue.offer(10);
queue.offer(20);

System.out.println(queue.poll());
```

Output:

```text
10
```

Typical complexity:

| Operation | Complexity |
|---|---:|
| peek | O(1) |
| insert | O(log n) |
| remove highest/lowest priority | O(log n) |

---

# 21. Set

A Set represents a collection of **unique elements**.

Common implementations:

```text
HashSet
LinkedHashSet
TreeSet
```

---

# 22. HashSet

`HashSet`:

- stores unique elements
- does not guarantee iteration order
- typically provides O(1) lookup
- internally uses a `HashMap`

This is extremely important:

> HashSet is essentially implemented using HashMap keys.

Conceptually:

```java
HashSet<E>
```

internally behaves approximately like:

```java
HashMap<E, Object>
```

The Set element becomes a Map key.

A dummy value is used internally.

---

# 23. HashSet Example

```java
Set<String> values = new HashSet<>();

values.add("A");
values.add("B");
values.add("A");
```

Result:

```text
A
B
```

The second `"A"` is not added.

---

# 24. HashSet Complexity

Average:

| Operation | Complexity |
|---|---:|
| add | O(1) |
| contains | O(1) |
| remove | O(1) |

Worst-case behavior can be worse because of hash collisions.

Modern Java versions can transform heavily collided buckets into tree structures under certain conditions, improving worst-case lookup behavior compared with a pure linked-list bucket.

---

# 25. HashMap

`HashMap` stores data as:

```text
key -> value
```

Example:

```java
Map<String, User> users = new HashMap<>();

users.put("1001", user1);
users.put("1002", user2);
```

Lookup:

```java
User user = users.get("1001");
```

Average complexity:

```text
O(1)
```

---

# 26. HashMap Internal Structure

This is one of the most important Java interview topics.

A HashMap internally uses an array of buckets.

Conceptually:

```text
table[]

index 0 -> null
index 1 -> Node
index 2 -> Node -> Node
index 3 -> null
index 4 -> Node
```

Each entry contains approximately:

```text
hash
key
value
next
```

---

# 27. How HashMap.put() Works

Suppose:

```java
map.put(key, value);
```

Conceptually:

### Step 1 — Calculate hash

```java
key.hashCode()
```

HashMap applies additional bit manipulation to spread bits more effectively.

### Step 2 — Calculate bucket index

The hash determines which bucket should contain the entry.

Conceptually:

```text
bucketIndex = hash -> table index
```

### Step 3 — Check existing entries

If bucket is empty:

```text
insert
```

Otherwise HashMap compares keys.

It checks roughly:

```java
hash
```

and then:

```java
equals()
```

### Step 4

If the key already exists:

```text
replace the value
```

Otherwise:

```text
insert a new entry
```

---

# 28. hashCode() vs equals()

This is extremely important.

HashMap uses both:

```java
hashCode()
equals()
```

`hashCode()` helps find the bucket.

`equals()` determines whether two keys are logically equal.

---

# 29. equals/hashCode Contract

Important rule:

If:

```java
a.equals(b) == true
```

then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

But the opposite is not required.

Two objects can have the same hash code while not being equal.

That situation is called:

```text
hash collision
```

---

# 30. What Happens If equals() is Overridden But hashCode() Is Not?

Suppose:

```java
class User {

    private int id;

    @Override
    public boolean equals(Object obj) {
        ...
    }
}
```

but `hashCode()` is not overridden.

Two logically equal objects may produce different hash codes.

HashMap may put them into different buckets.

Therefore:

```java
map.get(key)
```

may unexpectedly fail.

Likewise, a `HashSet` might contain logically duplicate objects.

This violates the `equals/hashCode` contract.

---

# 31. Mutable HashMap Keys — Dangerous

Consider:

```java
class User {

    String username;

    @Override
    public int hashCode() {
        return username.hashCode();
    }
}
```

Then:

```java
User user = new User("ali");

map.put(user, "data");

user.username = "reza";
```

Now the object's hash code has changed.

HashMap stored it based on the original hash.

Later:

```java
map.get(user);
```

may fail.

Therefore:

> HashMap keys should ideally be immutable, especially fields involved in equals/hashCode.

Common good keys:

```text
String
UUID
Integer
immutable value objects
```

This is an excellent senior-level point.

---

# 32. Hash Collision

Suppose two keys generate the same bucket index:

```text
key1 -> bucket 5
key2 -> bucket 5
```

This is called a collision.

Older implementations primarily used linked structures inside buckets.

Modern Java HashMap can convert a heavily collided bucket into a balanced tree under certain conditions.

Conceptually:

```text
bucket

A -> B -> C -> D
```

can eventually become something like:

```text
       C
      / \
     B   D
    /
   A
```

This improves lookup for heavily collided buckets.

---

# 33. Why Treeification Exists

If a bucket contained many elements as a linked list:

```text
lookup = O(n)
```

Using a balanced tree can improve it toward:

```text
O(log n)
```

This reduces the impact of heavy hash collisions.

---

# 34. HashMap Capacity and Load Factor

HashMap has two important concepts:

```text
capacity
load factor
```

The default load factor is typically:

```text
0.75
```

Meaning:

When approximately 75% of the capacity threshold is reached, the HashMap resizes.

Conceptually:

```text
capacity = 16
loadFactor = 0.75

threshold = 12
```

After crossing the threshold:

```text
resize
```

occurs.

---

# 35. Why Load Factor 0.75?

It represents a trade-off between:

```text
memory usage
```

and:

```text
collision probability
```

Lower load factor:

```text
more memory
fewer collisions
```

Higher load factor:

```text
less memory
more collisions
```

`0.75` provides a practical balance.

---

# 36. HashMap Resizing

When the map reaches its resize threshold:

```text
table size increases
```

and entries are redistributed into the larger bucket array.

Resizing is expensive.

Therefore, if the approximate size is known, initializing sufficient capacity may improve performance.

Example:

```java
Map<Integer, User> users = new HashMap<>(expectedCapacity);
```

---

# 37. HashMap and null

`HashMap` allows:

```text
one null key
multiple null values
```

Example:

```java
map.put(null, "A");
map.put("key", null);
```

---

# 38. HashMap Is Not Thread-Safe

This is extremely important.

```java
HashMap
```

should not be concurrently mutated by multiple threads without external synchronization.

Race conditions may occur.

For concurrency, one common alternative is:

```java
ConcurrentHashMap
```

---

# 39. ConcurrentHashMap

`ConcurrentHashMap` is designed for high-performance concurrent access.

Example:

```java
ConcurrentHashMap<String, User> users =
        new ConcurrentHashMap<>();
```

It supports safe concurrent reads and updates from multiple threads.

---

# 40. ConcurrentHashMap vs Hashtable

An old solution for thread-safe maps was:

```java
Hashtable
```

Hashtable synchronizes many operations using broad locking.

This limits concurrency.

Conceptually:

```text
Thread 1 ----\
Thread 2 ----- LOCK WHOLE MAP
Thread 3 ----/
```

Modern code generally prefers:

```java
ConcurrentHashMap
```

because it allows substantially better concurrent access.

---

# 41. ConcurrentHashMap Internal Concurrency

Older Java versions used the concept of:

```text
Segments
```

But modern implementations no longer use the old segment-based architecture.

Modern ConcurrentHashMap uses techniques such as:

- CAS
- volatile visibility
- fine-grained synchronization on individual bins when needed

This is an important interview trap.

If someone says:

> ConcurrentHashMap always works using 16 segments

that is an outdated Java 7-era explanation.

---

# 42. ConcurrentHashMap Reads

Reads are designed to be highly concurrent.

A normal:

```java
map.get(key);
```

usually does not require locking the entire map.

This makes ConcurrentHashMap appropriate for heavily concurrent systems.

---

# 43. ConcurrentHashMap Does Not Allow null

Unlike `HashMap`, `ConcurrentHashMap` does not allow:

```text
null keys
null values
```

Example:

```java
map.put(null, value);
```

will fail.

Why?

In concurrent code, `null` would make the meaning of:

```java
map.get(key) == null
```

ambiguous.

Does it mean:

```text
key does not exist
```

or:

```text
key exists and value is null
```

Removing that ambiguity makes concurrent APIs safer.

---

# 44. Atomic Operations in ConcurrentHashMap

An important senior-level topic.

Do not write:

```java
if (!map.containsKey(key)) {
    map.put(key, value);
}
```

This is a classic:

```text
check-then-act race condition
```

Two threads can both pass `containsKey()`.

Instead use atomic APIs:

```java
map.putIfAbsent(key, value);
```

or:

```java
map.computeIfAbsent(key, k -> createValue());
```

---

# 45. computeIfAbsent

Very common example:

```java
Map<String, List<String>> map =
        new ConcurrentHashMap<>();

map.computeIfAbsent(
    "users",
    key -> new ArrayList<>()
);
```

This is commonly used for lazy initialization.

However, with concurrent code, also consider whether the **value itself** is thread-safe.

This is subtle.

For example:

```java
ConcurrentHashMap<String, ArrayList<String>>
```

does not automatically make each `ArrayList` thread-safe.

The map is concurrent.

The stored objects are not magically concurrent.

This is an excellent Tech Lead-level observation.

---

# 46. ConcurrentHashMap Counter Pattern

Instead of:

```java
Integer count = map.get(key);

map.put(key, count + 1);
```

use an atomic operation:

```java
map.merge(key, 1, Integer::sum);
```

or:

```java
map.compute(key, (k, v) ->
        v == null ? 1 : v + 1
);
```

For high-contention counters, `LongAdder` is often useful:

```java
ConcurrentHashMap<String, LongAdder> counters =
        new ConcurrentHashMap<>();

counters
    .computeIfAbsent(key, k -> new LongAdder())
    .increment();
```

---

# 47. HashMap vs ConcurrentHashMap

| Feature | HashMap | ConcurrentHashMap |
|---|---|---|
| Thread-safe | No | Yes |
| Null key | Yes | No |
| Null value | Yes | No |
| Concurrent reads | unsafe with mutation | supported |
| Atomic operations | limited | `putIfAbsent`, `compute`, `merge`, etc. |
| Typical use | single-threaded / externally synchronized | concurrent systems |

---

# 48. LinkedHashMap

`LinkedHashMap` combines hashing with a linked ordering structure.

It can preserve:

```text
insertion order
```

Example:

```java
Map<Integer, String> map =
        new LinkedHashMap<>();
```

Insertion:

```text
3
1
2
```

Iteration:

```text
3
1
2
```

Unlike regular HashMap, which does not guarantee that iteration order.

---

# 49. LinkedHashMap and LRU Cache

One interesting feature:

LinkedHashMap can maintain:

```text
access order
```

instead of insertion order.

This makes it possible to implement a simple:

```text
LRU Cache
```

LRU means:

```text
Least Recently Used
```

Example concept:

```java
new LinkedHashMap<>(16, 0.75f, true);
```

The third argument:

```text
true
```

means access-order mode.

A subclass can override:

```java
removeEldestEntry()
```

to evict old entries.

In production, dedicated caches such as Caffeine are usually preferable, but knowing this demonstrates understanding of the JDK structures.

---

# 50. TreeMap

`TreeMap` stores keys in sorted order.

Internally, it is based on a balanced search tree, specifically a:

```text
Red-Black Tree
```

Operations:

| Operation | Complexity |
|---|---:|
| get | O(log n) |
| put | O(log n) |
| remove | O(log n) |

Unlike HashMap:

```text
HashMap   -> average O(1), unordered
TreeMap   -> O(log n), sorted
```

---

# 51. When Would You Use TreeMap?

Use TreeMap when you need ordered operations such as:

```java
map.firstKey();
map.lastKey();

map.floorKey(key);
map.ceilingKey(key);

map.lowerKey(key);
map.higherKey(key);
```

Or range queries:

```java
map.subMap(from, to);
```

Example use cases:

- time ranges
- pricing thresholds
- ranked boundaries
- nearest lower/upper key lookup

---

# 52. HashMap vs TreeMap

| Feature | HashMap | TreeMap |
|---|---|---|
| Ordering | No guarantee | Sorted |
| get | average O(1) | O(log n) |
| put | average O(1) | O(log n) |
| implementation | Hash table | Red-Black Tree |
| range queries | No | Yes |

---

# 53. TreeSet

`TreeSet`:

- stores unique elements
- maintains sorted order
- internally uses a TreeMap

Conceptually:

```text
TreeSet ≈ TreeMap where values are dummy objects
```

Operations are generally:

```text
O(log n)
```

---

# 54. HashSet vs LinkedHashSet vs TreeSet

| Feature | HashSet | LinkedHashSet | TreeSet |
|---|---|---|---|
| Unique values | Yes | Yes | Yes |
| Order | None guaranteed | Insertion | Sorted |
| lookup | O(1) avg | O(1) avg | O(log n) |
| internal structure | HashMap | LinkedHashMap | TreeMap |

---

# 55. Comparable vs Comparator

A common interview topic.

## Comparable

The class itself defines its natural ordering.

```java
class User implements Comparable<User> {

    private int age;

    @Override
    public int compareTo(User other) {
        return Integer.compare(age, other.age);
    }
}
```

Used like:

```java
Collections.sort(users);
```

---

## Comparator

Ordering is defined externally.

```java
Comparator<User> byAge =
        Comparator.comparingInt(User::getAge);
```

Then:

```java
users.sort(byAge);
```

You can have many different comparators:

```java
Comparator<User> byName;
Comparator<User> byAge;
Comparator<User> byCreatedAt;
```

---

# 56. Comparable vs Comparator

```text
Comparable
    -> natural ordering
    -> implemented by the class
    -> compareTo()

Comparator
    -> external/custom ordering
    -> separate object
    -> compare()
```

Senior-level consideration:

Avoid defining `Comparable` unless the type truly has a meaningful, stable **natural ordering**.

---

# 57. TreeMap / TreeSet and Comparator Equality

This is subtle and important.

Tree-based collections determine uniqueness based on comparison.

If:

```java
comparator.compare(a, b) == 0
```

TreeSet may treat `a` and `b` as equivalent even if:

```java
a.equals(b) == false
```

Therefore poorly designed comparators can produce surprising behavior.

---

# 58. Iterators

Collections can be traversed using:

```java
Iterator<E>
```

Example:

```java
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {
    String value = iterator.next();
}
```

Enhanced for-loop internally uses an iterator for most collections:

```java
for (String value : list) {
}
```

---

# 59. ConcurrentModificationException

Consider:

```java
for (String value : list) {
    if (value.equals("A")) {
        list.remove(value);
    }
}
```

This may throw:

```text
ConcurrentModificationException
```

Why?

Because many standard collection iterators are:

```text
fail-fast
```

They detect unexpected structural modification.

---

# 60. Correct Removal During Iteration

Use:

```java
Iterator<String> iterator = list.iterator();

while (iterator.hasNext()) {

    String value = iterator.next();

    if (value.equals("A")) {
        iterator.remove();
    }
}
```

Or often more simply:

```java
list.removeIf(value -> value.equals("A"));
```

---

# 61. What Does Fail-Fast Actually Mean?

Fail-fast does **not** mean thread-safe.

It means the iterator tries to detect structural modifications and throws:

```text
ConcurrentModificationException
```

rather than continuing with potentially inconsistent iteration.

It is primarily a bug-detection mechanism.

It should not be relied upon as a synchronization mechanism.

---

# 62. Weakly Consistent Iterators

Concurrent collections such as:

```java
ConcurrentHashMap
```

typically use **weakly consistent iterators**.

They:

- do not generally throw `ConcurrentModificationException`
- may reflect some concurrent modifications
- do not necessarily provide a frozen snapshot

This design allows iteration without globally locking the collection.

---

# 63. Unmodifiable Collections

Example:

```java
List<String> list =
        Collections.unmodifiableList(original);
```

This returns an unmodifiable **view**.

Important:

If `original` changes:

```java
original.add("A");
```

the unmodifiable view may also reflect that change.

Therefore:

```text
unmodifiable != immutable
```

---

# 64. List.of / Set.of / Map.of

Modern Java provides factory methods:

```java
List.of("A", "B");
Set.of("A", "B");
Map.of("A", 1, "B", 2);
```

These create unmodifiable collections.

Example:

```java
List<String> values = List.of("A", "B");

values.add("C");
```

throws:

```text
UnsupportedOperationException
```

They also generally reject `null`.

---

# 65. Defensive Copying

Suppose:

```java
class UserService {

    private final List<User> users;

    UserService(List<User> users) {
        this.users = users;
    }
}
```

The caller can still mutate the original list.

Better:

```java
this.users = List.copyOf(users);
```

This prevents the caller from modifying your internal collection through the original reference.

This concept is:

```text
defensive copying
```

Very relevant for immutable design and API boundaries.

---

# 66. Arrays.asList Trap

Consider:

```java
List<String> list =
        Arrays.asList("A", "B", "C");
```

This returns a fixed-size list backed by the array.

You can do:

```java
list.set(0, "X");
```

but:

```java
list.add("D");
```

throws:

```text
UnsupportedOperationException
```

This is a classic Java interview question.

---

# 67. List.of vs Arrays.asList

```text
Arrays.asList
    fixed size
    elements may be replaced
    backed by array
    allows null

List.of
    unmodifiable
    no add/remove/set
    rejects null
```

---

# 68. remove() Trap With List<Integer>

Consider:

```java
List<Integer> numbers = new ArrayList<>();

numbers.add(10);
numbers.add(20);
numbers.add(30);

numbers.remove(1);
```

What gets removed?

```text
20
```

because:

```java
remove(int index)
```

is called.

If you want to remove value `1`:

```java
numbers.remove(Integer.valueOf(1));
```

This overload confusion is a common interview trick.

---

# 69. Map.get() and null Ambiguity

For HashMap:

```java
map.get(key)
```

returning `null` can mean:

```text
key doesn't exist
```

or:

```text
key exists and value is null
```

If needed:

```java
map.containsKey(key)
```

can distinguish the cases.

This ambiguity is one reason ConcurrentHashMap forbids null values.

---

# 70. Map API Methods You Should Know

For senior-level Java, know these:

```java
putIfAbsent()
compute()
computeIfAbsent()
computeIfPresent()
merge()
replace()
replaceAll()
getOrDefault()
```

Example:

```java
int count = map.getOrDefault(word, 0);
```

Instead of:

```java
Integer count = map.get(word);

if (count == null) {
    count = 0;
}
```

---

# 71. merge()

Very useful for counters:

```java
Map<String, Integer> counts = new HashMap<>();

counts.merge("java", 1, Integer::sum);
```

Equivalent conceptually to:

```text
if key absent:
    value = 1
else:
    value = oldValue + 1
```

---

# 72. computeIfAbsent()

Useful for grouping:

```java
Map<String, List<User>> groups = new HashMap<>();

groups
    .computeIfAbsent(
        department,
        key -> new ArrayList<>()
    )
    .add(user);
```

This eliminates manual existence checks.

---

# 73. Choosing the Right Collection

This is one of the most important Tech Lead skills.

Do not choose a data structure based on habit.

Ask:

```text
Do I need duplicates?

Do I need ordering?

Do I need sorting?

Do I need fast lookup?

Do I need indexed access?

Do I need range queries?

Do I need concurrency?

What is the read/write ratio?

How large is the data?

What are the memory constraints?
```

---

# 74. Practical Selection Guide

If you need:

### Indexed ordered elements

```text
ArrayList
```

Usually the default List.

---

### FIFO Queue

```text
ArrayDeque
```

---

### Stack

```text
ArrayDeque
```

---

### Unique elements with fast lookup

```text
HashSet
```

---

### Unique elements preserving insertion order

```text
LinkedHashSet
```

---

### Unique sorted elements

```text
TreeSet
```

---

### Key-value storage with fast lookup

```text
HashMap
```

---

### Key-value storage preserving insertion order

```text
LinkedHashMap
```

---

### Sorted keys / range queries

```text
TreeMap
```

---

### Concurrent key-value storage

```text
ConcurrentHashMap
```

---

### Mostly-read concurrent list

```text
CopyOnWriteArrayList
```

---

### Priority-based processing

```text
PriorityQueue
```

---

# 75. Big-O Summary

| Structure | Get/Search | Insert | Remove |
|---|---:|---:|---:|
| ArrayList index | O(1) | end O(1)* | O(n) |
| LinkedList index | O(n) | ends O(1) | ends O(1) |
| HashMap | O(1) avg | O(1) avg | O(1) avg |
| HashSet | O(1) avg | O(1) avg | O(1) avg |
| TreeMap | O(log n) | O(log n) | O(log n) |
| TreeSet | O(log n) | O(log n) | O(log n) |
| PriorityQueue | peek O(1) | O(log n) | poll O(log n) |

`*` ArrayList append is amortized O(1).

---

# 76. Common Senior Interview Question

## Why would you choose ArrayList over LinkedList?

Strong answer:

> ArrayList is generally my default List implementation because it provides O(1) random access, low per-element memory overhead, and excellent cache locality. Although LinkedList theoretically provides O(1) insertion or removal once a node is known, locating that node is usually O(n), and its pointer-heavy memory layout produces more cache misses and allocation overhead. I would choose LinkedList only when its deque characteristics or specific insertion/removal pattern actually benefits the workload.

---

# 77. Common Senior Interview Question

## How does HashMap work internally?

Strong answer:

> HashMap stores entries in an array of buckets. When a key is inserted, its hashCode is processed to calculate a hash, and the hash determines a bucket index. If multiple keys map to the same bucket, HashMap handles the collision by storing multiple entries in that bucket. Java compares the hash and then uses equals to identify the correct key. In modern Java, heavily collided buckets may be treeified, improving lookup from linear toward logarithmic behavior. HashMap resizes when its load threshold is exceeded.

---

# 78. Common Senior Interview Question

## Why must equals and hashCode be consistent?

Strong answer:

> Hash-based collections first use hashCode to locate a bucket and then equals to determine logical equality. If two logically equal objects produce different hash codes, they may be placed in different buckets, causing lookups to fail or HashSet to contain logical duplicates. Therefore, whenever equals is overridden, hashCode must also be implemented consistently.

---

# 79. Common Senior Interview Question

## Why shouldn't a mutable object be used as a HashMap key?

Strong answer:

> If a field participating in equals or hashCode changes after insertion, the object's hash may no longer correspond to the bucket where it was originally stored. The entry is still physically present, but normal lookup may no longer find it. Therefore, keys should ideally be immutable.

---

# 80. Common Senior Interview Question

## HashMap vs ConcurrentHashMap?

Strong answer:

> HashMap provides no thread-safety guarantee for concurrent mutation. ConcurrentHashMap is specifically designed for concurrent access and uses a combination of lock-free techniques and fine-grained synchronization rather than locking the whole map. It also provides atomic compound operations such as putIfAbsent, compute and merge. Unlike HashMap, it does not permit null keys or values.

---

# 81. Common Senior Interview Question

## Is ConcurrentHashMap enough to make an operation thread-safe?

No.

Example:

```java
if (map.containsKey(key)) {
    map.put(key, newValue);
}
```

Each individual call is thread-safe, but the combined operation is not atomic.

Another thread can modify the map between the two operations.

Use an atomic API such as:

```java
compute()
computeIfAbsent()
putIfAbsent()
merge()
```

This distinction between:

```text
thread-safe individual operation
```

and:

```text
atomic business operation
```

is very important at senior level.

---

# 82. Common Senior Interview Question

## Is this thread-safe?

```java
ConcurrentHashMap<String, ArrayList<User>> map;
```

Not necessarily.

The map itself supports concurrent operations, but:

```java
ArrayList<User>
```

is not thread-safe.

For example:

```java
map.get("team").add(user);
```

may still involve concurrent modifications of the same ArrayList.

You must reason about the concurrency of the entire object graph, not only the outer collection.

---

# 83. Common Senior Interview Question

## Why not synchronize everything?

Because synchronization has costs:

- lock contention
- reduced parallelism
- context switching
- increased latency

The goal is not:

```text
make everything synchronized
```

The goal is:

```text
define clear ownership
minimize shared mutable state
use appropriate concurrency primitives
make compound operations atomic
```

This is a much more mature concurrency design mindset.

---

# 84. Collection Design and Domain Modeling

At Tech Lead level, collection choice is not just a coding decision.

For example:

```java
List<Order>
```

and:

```java
Map<OrderId, Order>
```

express different access patterns.

If most operations are:

```text
find order by ID
```

then continuously scanning:

```java
List<Order>
```

is poor modeling.

A map may express the actual domain access pattern better.

Data structures should reflect:

```text
business access patterns
performance requirements
consistency requirements
concurrency requirements
```

---

# 85. Memory Complexity Matters Too

Big-O CPU complexity is not the only factor.

Consider:

```text
ArrayList
```

vs:

```text
LinkedList
```

LinkedList requires an extra node object per element containing references such as:

```text
prev
next
item
```

That means:

- more memory
- more allocations
- more GC work
- worse locality

Senior engineers should consider:

```text
time complexity
memory complexity
allocation rate
CPU cache behavior
concurrency
```

rather than only quoting Big-O.

---

# 86. Streams Do Not Replace Collections

Streams and Collections solve different problems.

Collection:

```text
stores data
```

Stream:

```text
describes data processing
```

Example:

```java
List<User> users = ...
```

stores the users.

Then:

```java
users.stream()
    .filter(User::isActive)
    .map(User::getName)
    .toList();
```

processes the users.

A Stream is not itself a general-purpose data structure.

---

# 87. Collectors.groupingBy

Useful example:

```java
Map<String, List<User>> usersByDepartment =
    users.stream()
        .collect(
            Collectors.groupingBy(User::getDepartment)
        );
```

Result:

```text
engineering -> [user1, user2]
finance     -> [user3]
```

This naturally creates a Map-based grouping.

---

# 88. Parallel Streams and Collections

Be careful with:

```java
parallelStream()
```

Shared mutable collections inside parallel operations can cause race conditions.

Bad:

```java
List<String> results = new ArrayList<>();

users.parallelStream()
     .forEach(user ->
         results.add(user.getName())
     );
```

Instead prefer stream collectors:

```java
List<String> results =
    users.parallelStream()
         .map(User::getName)
         .toList();
```

Avoid shared mutable state when possible.

---

# 89. Useful Concurrent Collections

For senior interviews, know at least the purpose of:

```text
ConcurrentHashMap
CopyOnWriteArrayList
BlockingQueue
ConcurrentLinkedQueue
ConcurrentSkipListMap
```

You do not necessarily need to memorize every internal detail.

---

# 90. BlockingQueue

Important for producer-consumer architectures.

Example:

```java
BlockingQueue<Job> queue =
        new ArrayBlockingQueue<>(1000);
```

Producer:

```java
queue.put(job);
```

Consumer:

```java
Job job = queue.take();
```

If the queue is full:

```text
put() waits
```

If the queue is empty:

```text
take() waits
```

This naturally provides:

```text
backpressure
```

between producer and consumer.

---

# 91. BlockingQueue Implementations

Important ones:

```text
ArrayBlockingQueue
LinkedBlockingQueue
PriorityBlockingQueue
DelayQueue
SynchronousQueue
```

For general bounded producer-consumer workloads:

```text
ArrayBlockingQueue
```

is often a reasonable choice.

Bounded queues are especially important in backend systems because unbounded queues can cause memory exhaustion under overload.

---

# 92. Tech Lead Perspective: Backpressure

Suppose:

```text
Producer = 100,000 msg/sec
Consumer = 10,000 msg/sec
```

An unbounded queue causes backlog to continuously increase.

Eventually:

```text
memory exhaustion
latency explosion
system instability
```

A bounded queue forces the system to define an overload policy.

For example:

```text
block
reject
drop
rate-limit
persist elsewhere
```

This is where collection choice turns into system design.

---

# 93. Collections.synchronizedMap vs ConcurrentHashMap

You can do:

```java
Map<K,V> map =
    Collections.synchronizedMap(new HashMap<>());
```

But this generally provides coarse-grained synchronization.

ConcurrentHashMap is specifically optimized for concurrent access.

Therefore, for most highly concurrent scenarios:

```text
ConcurrentHashMap
```

is preferable.

---

# 94. IdentityHashMap

Less commonly used, but useful to recognize.

Unlike HashMap, it compares keys using:

```java
==
```

instead of:

```java
equals()
```

It is useful for specialized cases such as object graph processing.

It should generally not be used as a normal business-data Map.

---

# 95. WeakHashMap

`WeakHashMap` stores keys using weak references.

If a key is no longer strongly referenced elsewhere, GC may remove the entry.

Possible use cases:

```text
metadata associated with externally owned objects
specialized caches
```

But it should not automatically be chosen as a general cache.

Modern production caching usually deserves a dedicated caching library and explicit eviction policy.

---

# 96. EnumMap

When keys are an enum:

```java
enum Status {
    CREATED,
    PROCESSING,
    COMPLETED
}
```

Instead of:

```java
HashMap<Status, Handler>
```

consider:

```java
EnumMap<Status, Handler>
```

`EnumMap` is compact and efficient for enum keys.

Example:

```java
Map<Status, Handler> handlers =
        new EnumMap<>(Status.class);
```

---

# 97. EnumSet

Likewise, for a Set of enum values:

```java
EnumSet<Permission> permissions =
        EnumSet.of(
            Permission.READ,
            Permission.WRITE
        );
```

This is usually more efficient than:

```java
HashSet<Permission>
```

EnumSet internally can use bit-vector-like representation.

---

# 98. Senior-Level Rule

Do not memorize:

```text
ArrayList good
LinkedList bad
HashMap fast
TreeMap slow
```

Instead think in terms of requirements:

```text
access pattern
ordering
uniqueness
range queries
memory
mutation frequency
concurrency
contention
latency
data volume
```

The best data structure is context-dependent.

---

# 99. Rapid Interview Cheat Sheet

If the interviewer says:

### ArrayList?

Say:

> Resizable-array-based List, O(1) random access, amortized O(1) append, O(n) middle insert/removal due to shifting, not thread-safe, good cache locality, usually my default List.

### LinkedList?

Say:

> Doubly linked list, O(1) operations at the ends, O(n) indexed access, higher memory overhead and poor locality. I rarely use it as a general List; ArrayDeque is often better for queue/deque use cases.

### HashMap?

Say:

> Hash-table-based key-value store. Average O(1) access. Uses hashCode to locate a bucket and equals to identify keys. Handles collisions and can treeify heavily collided buckets. Not thread-safe.

### HashSet?

Say:

> Unique-element collection internally backed by a HashMap, where elements are stored as keys.

### TreeMap?

Say:

> Sorted Map based on a Red-Black tree with O(log n) operations and support for range and nearest-key queries.

### ConcurrentHashMap?

Say:

> Thread-safe high-concurrency Map using fine-grained synchronization and CAS-style techniques. Reads are highly concurrent, nulls are prohibited, and it provides atomic compound APIs such as compute, merge and putIfAbsent.

### CopyOnWriteArrayList?

Say:

> Thread-safe List optimized for read-heavy workloads. Reads are cheap, but every mutation copies the backing array.

### ArrayDeque?

Say:

> Array-backed double-ended queue and generally the preferred Java implementation for stacks and normal deque operations.

---

# 100. Questions You Should Be Ready to Answer

Before the interview, make sure you can answer these without hesitation:

1. How does HashMap work internally?
2. What happens during a hash collision?
3. What is the relationship between equals and hashCode?
4. Why is using a mutable HashMap key dangerous?
5. What is the load factor?
6. When does HashMap resize?
7. HashMap vs TreeMap?
8. HashMap vs ConcurrentHashMap?
9. Why does ConcurrentHashMap reject null?
10. Is ConcurrentHashMap completely lock-free?
11. Why is `containsKey()` followed by `put()` unsafe in concurrent code?
12. `computeIfAbsent()` vs `putIfAbsent()`?
13. ArrayList vs LinkedList?
14. Why is ArrayList often faster than LinkedList even for many practical workloads?
15. What does amortized O(1) mean?
16. HashSet internals?
17. HashSet vs TreeSet?
18. LinkedHashMap use cases?
19. How would you implement a simple LRU cache?
20. Comparable vs Comparator?
21. What is fail-fast iteration?
22. Is ConcurrentModificationException a thread-safety mechanism?
23. `Arrays.asList()` vs `List.of()`?
24. Why use defensive copying?
25. When should you use CopyOnWriteArrayList?
26. When would you use BlockingQueue?
27. Why can an unbounded queue be dangerous?
28. Why isn't `ConcurrentHashMap<K, ArrayList<V>>` automatically fully thread-safe?
29. Why should you usually prefer ArrayDeque over Stack?
30. How do you select the correct collection for a system?

---

# 101. Final Mental Model

A Senior Java developer should not think:

```text
Which Collection class do I know?
```

They should think:

```text
What access pattern does the problem require?

Do I need ordering?
Do I need uniqueness?
Do I need sorting?
Do I need direct lookup?
Do I need range lookup?
Do I need concurrent mutation?
What is the read/write ratio?
How much contention exists?
How large will this structure become?
What are the allocation and memory costs?
What happens under overload?
```

Then choose the appropriate structure.

That shift—from memorizing APIs to reasoning about **trade-offs**—is what separates a basic Java Collections answer from a Senior / Tech Lead answer.