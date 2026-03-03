# Java Developer Roadmap - Interview Notes

## 📦 Access Modifiers

| Modifier | Scope |
|----------|-------|
| **public** | Accessible everywhere |
| **private** | Only within the same class |
| **protected** | Same package + subclasses |
| **default** | Same package only |

---

## 🔄 Polymorphism

### Compile-Time (Method Overloading)
- Same method name, different parameters
```java
void add(int a, int b) { }
void add(int a, int b, int c) { }
```

### Runtime (Method Overriding)
- Subclass redefines parent method
```java
class Animal { void sound() { } }
class Dog extends Animal { void sound() { System.out.println("Bark"); } }
```

---

## 🔒 Final Keyword

- **final variable** → constant, cannot be reassigned
- **final method** → cannot be overridden
- **final class** → cannot be extended

---

## 📦 Wrapper Classes

Convert primitives to objects for use in collections:
- `int` → `Integer`
- `char` → `Character`
- `double` → `Double`
- `boolean` → `Boolean`

**Autoboxing:** `Integer i = 5;` (automatic conversion)  
**Unboxing:** `int x = i;` (automatic conversion back)

---

## 🎨 Abstract Classes

- Cannot be instantiated
- Can have both abstract and concrete methods
- Use when you want to share code among related classes

```java
abstract class Vehicle {
    abstract void drive();
    
    void honk() {
        System.out.println("Beep!");
    }
}

class Car extends Vehicle {
    void drive() {
        System.out.println("Car is driving");
    }
}
```

> **Note:** If a subclass doesn't implement all abstract methods, it must also be abstract.

---

## 🔗 Interfaces

- Contract that defines **what** a class must do (not **how**)
- All methods are `public abstract` by default (Java 8+ allows default/static methods)
- Variables are `public static final`
- A class can implement multiple interfaces

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() { System.out.println("Flying"); }
    public void swim() { System.out.println("Swimming"); }
}
```

### Types of Interfaces

| Type | Description | Example |
|------|-------------|---------|
| **Normal** | 2+ abstract methods | `Comparable` |
| **Functional (SAM)** | Exactly 1 abstract method | `Runnable`, `Callable` |
| **Marker** | No methods (tagging) | `Serializable`, `Cloneable` |

---

## ⚡ Functional Interfaces & Lambda

### Functional Interface
```java
@FunctionalInterface
interface Operation {
    int apply(int a, int b);
}
```

### Lambda Expression
```java
Operation add = (a, b) -> a + b;
Operation multiply = (a, b) -> a * b;

System.out.println(add.apply(3, 4));  // 7
```

**Common Functional Interfaces:**
- `Predicate<T>` → `boolean test(T t)`
- `Function<T,R>` → `R apply(T t)`
- `Consumer<T>` → `void accept(T t)`
- `Supplier<T>` → `T get()`

---

## 🏗️ Inner Classes

### Static Nested Class (Builder Pattern)
```java
class Product {
    private String name;
    private int price;
    
    private Product(Builder builder) {
        this.name = builder.name;
        this.price = builder.price;
    }
    
    static class Builder {
        private String name;
        private int price;
        
        Builder setName(String name) { this.name = name; return this; }
        Builder setPrice(int price) { this.price = price; return this; }
        
        Product build() { return new Product(this); }
    }
}

// Usage
Product p = new Product.Builder()
    .setName("Laptop")
    .setPrice(1000)
    .build();
```

### Other Inner Class Types
- **Member Inner Class** → instance-level, can access outer class members
- **Local Inner Class** → defined inside a method
- **Anonymous Inner Class** → one-time use, no name

---

## 🎲 Enums

Named constants for fixed sets of values:
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

Day today = Day.MONDAY;
```

**With methods:**
```java
enum Status {
    SUCCESS(200), ERROR(500);
    
    private int code;
    Status(int code) { this.code = code; }
    public int getCode() { return code; }
}
```

---

## ⚠️ Exception Handling

### Exception Hierarchy
```
Throwable
├── Error (OutOfMemoryError, StackOverflowError)
└── Exception
    ├── RuntimeException (unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── IllegalArgumentException
    └── Checked Exceptions (IOException, SQLException)
```

### Try-Catch-Finally
```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
} catch (Exception e) {
    System.out.println("General exception");
} finally {
    System.out.println("Always executes");
}
```

### Throw vs Throws
```java
// throws - declares exception
void readFile() throws IOException {
    // method code
}

// throw - manually throws exception
void validate(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be 18+");
    }
}
```

### Try-with-Resources (Java 7+)
```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line = br.readLine();
} // auto-closes resource
```

---

## 🔄 Collections Framework

### Core Interfaces
```
Collection
├── List (ordered, duplicates allowed)
│   ├── ArrayList (fast random access)
│   ├── LinkedList (fast insertion/deletion)
│   └── Vector (synchronized)
├── Set (no duplicates)
│   ├── HashSet (O(1) operations)
│   ├── LinkedHashSet (maintains insertion order)
│   └── TreeSet (sorted)
└── Queue
    ├── PriorityQueue (heap-based)
    └── Deque (ArrayDeque, LinkedList)

Map (key-value pairs)
├── HashMap (O(1) operations)
├── LinkedHashMap (maintains insertion order)
├── TreeMap (sorted by key)
└── Hashtable (synchronized, legacy)
```

### Sorting Collections

**Natural Sorting:**
```java
List<Integer> list = Arrays.asList(5, 2, 8, 1);
Collections.sort(list);  // [1, 2, 5, 8]
```

**Custom Sorting with Comparator:**
```java
// Descending order
Collections.sort(list, (a, b) -> b - a);

// Complex objects
Collections.sort(students, (s1, s2) -> s1.getName().compareTo(s2.getName()));
```

**Comparable Interface:**
```java
class Student implements Comparable<Student> {
    private int age;
    
    @Override
    public int compareTo(Student other) {
        return this.age - other.age;  // ascending by age
    }
}
```

---

## 🧵 Multithreading & Concurrency

### Creating Threads
```java
// Method 1: Extend Thread
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running");
    }
}
new MyThread().start();

// Method 2: Implement Runnable (preferred)
Runnable task = () -> System.out.println("Task running");
new Thread(task).start();

// Method 3: ExecutorService
ExecutorService executor = Executors.newFixedThreadPool(5);
executor.submit(() -> System.out.println("Task"));
executor.shutdown();
```

### Synchronization
```java
synchronized void increment() {
    count++;
}

// Or synchronized block
synchronized(this) {
    count++;
}
```

### Thread States
`NEW → RUNNABLE → RUNNING → BLOCKED/WAITING → TERMINATED`

### Common Concurrency Classes
- **CountDownLatch** → wait for N threads to complete
- **CyclicBarrier** → all threads wait at barrier point
- **Semaphore** → limit concurrent access
- **ReentrantLock** → explicit lock (more flexible than synchronized)
- **AtomicInteger/AtomicLong** → lock-free thread-safe operations

### volatile Keyword
- Ensures variable visibility across threads
- No caching, reads from main memory

```java
private volatile boolean flag = true;
```

---

## 🆕 Java 8+ Features

### Stream API
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Filter, map, collect
List<Integer> evenSquares = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * n)
    .collect(Collectors.toList());

// Reduce
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
```

### Optional
```java
Optional<String> optional = Optional.ofNullable(getName());
String name = optional.orElse("Default");

optional.ifPresent(n -> System.out.println(n));
```

### Default Methods in Interfaces
```java
interface MyInterface {
    void abstractMethod();
    
    default void defaultMethod() {
        System.out.println("Default implementation");
    }
}
```

---

## 🔑 Important Concepts

### equals() vs ==
- `==` → compares references (memory address)
- `equals()` → compares content (override in your classes)

### String, StringBuilder, StringBuffer
- `String` → immutable
- `StringBuilder` → mutable, not thread-safe (faster)
- `StringBuffer` → mutable, thread-safe (slower)

### Static Keyword
- **Static variable** → shared across all instances
- **Static method** → can be called without object
- **Static block** → runs once when class is loaded

### this vs super
- `this` → refers to current object
- `super` → refers to parent class

### Garbage Collection
- Automatic memory management
- `System.gc()` suggests GC (not guaranteed)
- Objects eligible when no references exist
- **Types:** Serial, Parallel, CMS, G1GC, ZGC

### Serialization
```java
class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    private String name;
    private transient int age;  // won't be serialized
}
```

### JVM Architecture
```
JVM
├── Class Loader (loads .class files)
├── Memory Areas
│   ├── Heap (objects, shared)
│   ├── Stack (method calls, local variables per thread)
│   ├── Method Area (class metadata, static variables)
│   └── PC Register (current instruction address)
└── Execution Engine
    ├── Interpreter
    ├── JIT Compiler (converts bytecode to machine code)
    └── Garbage Collector
```

**Key Points:**
- **JDK** = JRE + Development Tools (javac, javadoc)
- **JRE** = JVM + Libraries
- **JVM** = Executes bytecode

---

## 🔷 Generics

### Basic Generic Class
```java
class Box<T> {
    private T item;
    
    public void setItem(T item) { this.item = item; }
    public T getItem() { return item; }
}

Box<Integer> intBox = new Box<>();
intBox.setItem(10);
```

### Generic Methods
```java
public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

---

## 🔷 Generics

### Basic Generic Class
```java
class Box<T> {
    private T item;
    
    public void setItem(T item) { this.item = item; }
    public T getItem() { return item; }
}

Box<Integer> intBox = new Box<>();
intBox.setItem(10);
```

### Generic Methods
```java
public <T> void printArray(T[] array) {
    for (T element : array) {
        System.out.println(element);
    }
}
```

### Bounded Type Parameters
```java
// Upper bound (T must be Number or its subclass)
class Calculator<T extends Number> {
    public double sum(T num1, T num2) {
        return num1.doubleValue() + num2.doubleValue();
    }
}
```

### Wildcards
- `<?>` → Unknown type (any type)
- `<? extends T>` → Upper bound (T or subclass)
- `<? super T>` → Lower bound (T or superclass)

```java
public void process(List<? extends Number> list) {
    // can read but not write
}
```

---

## 🎨 Common Design Patterns

### Creational
- **Singleton** → Only one instance exists
```java
class Singleton {
    private static Singleton instance;
    private Singleton() {}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) instance = new Singleton();
        return instance;
    }
}
```

- **Factory** → Object creation without specifying exact class
- **Builder** → Construct complex objects step by step (see Inner Classes)

### Structural
- **Adapter** → Bridge between incompatible interfaces
- **Decorator** → Add behavior to objects dynamically
- **Proxy** → Placeholder for another object

### Behavioral
- **Observer** → One-to-many dependency (publish-subscribe)
- **Strategy** → Define family of algorithms, make them interchangeable
- **Template Method** → Define skeleton in base class

---

## 🏛️ OOP Principles & SOLID

### 4 Pillars of OOP
1. **Encapsulation** → Bundling data + methods, hiding internals
2. **Abstraction** → Hiding complexity, showing only essentials
3. **Inheritance** → Code reuse through parent-child relationship
4. **Polymorphism** → Many forms (overloading + overriding)

### SOLID Principles

| Principle | Meaning | Example |
|-----------|---------|---------|
| **S** - Single Responsibility | Class should have one reason to change | User class shouldn't handle DB operations |
| **O** - Open/Closed | Open for extension, closed for modification | Use interfaces/abstract classes |
| **L** - Liskov Substitution | Subclass must be substitutable for parent | Square shouldn't extend Rectangle if behavior breaks |
| **I** - Interface Segregation | Don't force classes to implement unused methods | Split fat interfaces into smaller ones |
| **D** - Dependency Inversion | Depend on abstractions, not concretions | Use interfaces instead of concrete classes |

---

## ✅ Best Practices

1. **Use interfaces over concrete classes** for flexibility
2. **Favor composition over inheritance**
3. **Make classes immutable when possible** (final fields, no setters)
4. **Override hashCode() when overriding equals()**
5. **Use generics** for type safety
6. **Close resources** with try-with-resources
7. **Use appropriate collections** based on use case
8. **Prefer StringBuilder** for string concatenation in loops
9. **Use @Override annotation** to catch errors at compile time
10. **Follow naming conventions** (camelCase, PascalCase for classes)





