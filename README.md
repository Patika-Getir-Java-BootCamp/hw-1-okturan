## 1 – Why do we need to use OOP? Some major OOP languages?

<details>
<summary>Answer 1 ↓</summary>

Object-oriented programming organizes code into self-contained units that model real-world entities.
OOP begins by abstracting an object from a goal. By focusing on a single component, it can be
designed, tested, and refined independently. A well-designed object can be reused or extended later
without impacting other parts of the code.

**Example:**  
Imagine developing a smart home system. You might start by creating a `Light` class that manages
properties like brightness, color, and power status. Later, you can extend this class to build
specialized smart lights with added features like remote control or scheduling—all without affecting
the core functionality of the `Light` class.

Major OOP languages:  
Ada, ActionScript, C++, Common Lisp, C#, Dart, Eiffel, Fortran 2003, Haxe, Java, JavaScript, Kotlin,
Logo, MATLAB, Objective-C, Object Pascal, Perl, PHP, Python, R, Raku, Ruby, Scala, SIMSCRIPT,
Simula, Smalltalk, Swift, Vala and Visual Basic.NET [[1]](#1).
</details>

## 2 – What is the difference between an interface and an abstract class?

<details>
<summary>Answer 2 ↓</summary>

| Feature                 | Interface                                                   | Abstract Class                                   |
|-------------------------|-------------------------------------------------------------|--------------------------------------------------|
| Method Implementation   | Only abstract methods (default methods allowed from Java 8) | Can have both abstract and concrete methods      |
| Method Access Modifiers | All methods are implicitly `public abstract`                | Can use any access modifier                      |
| Constants/Fields        | Only `public static final` constants allowed                | Can have instance (non-final, non-static) fields |
| Inheritance             | Supports multiple inheritance                               | Supports single inheritance only                 |
| Constructors            | Not allowed                                                 | Allowed                                          |
| Instance Variables      | Not allowed                                                 | Allowed                                          |


</details>

## 3 – Why do we need equals() and hashCode() in Java? When should we override them?

<details>
<summary>Answer 3 ↓</summary>

Both methods serve to enable object comparison and hash-based collection operations:
Both are defined in the root `Object` class.

### `equals()`

The original intent of the equals() method is to determine logical equality between two objects.

- **Default implementation (Object class):**
  Checks if two object references refer to the same memory location (== reference equality).
- **Typical usage (Overridden implementation):**
  Checks if two objects have the same value or state (logical equality).

### `hashcode()`

The original intent of hashCode() is to provide a numeric hash representation of the object's
internal state, allowing the object to be stored and efficiently retrieved in hash-based
collections (e.g., HashMap, HashSet, Hashtable).

- **Default implementation (Object class):**
  Returns an integer typically related to the object's memory address.
- **Typical usage (Overridden implementation):**
  Calculates a consistent hash value based on fields that contribute to logical equality.

**When to override them:**

- Override both methods when you need to compare objects based on their content rather than their
  memory reference.
- Always override both methods together to maintain the contract: if two objects are equal according
  to equals(), they must have the same hashCode().
- Override when your objects will be used in hash-based collections like HashMap, HashSet, or
  Hashtable.

</details>

## 4 – What is the diamond problem in Java, and how can it be solved?

<details>
<summary>Answer 4 ↓</summary>

The **diamond problem** occurs when a class inherits methods from two classes, which both inherit
from a common superclass. This creates ambiguity about which inherited method to use:

```
      A
     / \
    B   C
     \ /
      D
```

- Class `A` defines a method.
- Classes `B` and `C` override it differently.
- Class `D` inherits from both `B` and `C`.  
  **Ambiguity:** Which method does `D` inherit?

---

**How Java Handles the Diamond Problem:**

1. **Single Inheritance**  
   Java allows only one superclass per class. This eliminates traditional diamond problems:
   ```java
   class D extends B {} // Only ONE superclass allowed
   ```

2. **Interfaces** (Before Java 8)  
   Java allows multiple interfaces but traditionally interfaces didn't have implementations, so no
   conflict occurs.

3. **Default Methods (Java 8+)**  
   Interfaces can now have default method implementations, potentially reintroducing the diamond
   problem. Java solves this by:
    - If two interfaces have the same default method, the class **must explicitly override** the
      method.
    - Use explicit syntax to choose a specific interface's default method:
      ```java
      InterfaceName.super.methodName();
      ```

---

**Example:**

```java
interface InterfaceA {
    default void print() {
        System.out.println("Interface A");
    }
}

interface InterfaceB extends InterfaceA {
    default void print() {
        System.out.println("Interface B");
    }
}

interface InterfaceC extends InterfaceA {
    default void print() {
        System.out.println("Interface C");
    }
}

// Diamond problem: Class implements two interfaces with same default method
class MyClass implements InterfaceB, InterfaceC {
    @Override
    public void print() {
        // Explicitly resolve ambiguity
        InterfaceB.super.print(); // or InterfaceC.super.print()
    }
}
```

**Output:**

```
Interface B
```

---
</details>

## 5 – Why do we need the Garbage Collector in Java, and how does it work?

<details>
<summary>Answer 5 ↓</summary>

**Why we need the Garbage Collector:**

- **Memory Management Automation**: Eliminates the need for manual memory allocation and
  deallocation.
- **Prevention of Memory Leaks**: Automatically reclaims memory that is no longer referenced,
  preventing memory leaks.
- **Reduction of Bugs**: Eliminates common memory-related bugs like dangling pointers, double frees,
  and memory fragmentation.
- **Developer Productivity**: Allows developers to focus on application logic rather than memory
  management details.

**How the Garbage Collector works:**

1. **Mark**: The GC starts from "root" objects (like static variables, local variables in the
   current execution thread) and marks all objects that are reachable from these roots as "live."

2. **Sweep/Clean**: Any objects that weren't marked in the first phase are considered unreachable
   and can be removed. The memory occupied by these objects is then reclaimed.

3. **Compact (optional)**: Some GC algorithms also compact the heap by moving all surviving objects
   together, eliminating memory fragmentation.

**Java's Garbage Collection Implementations:**

- **Serial GC**: Simple, single-threaded collector.
- **Parallel GC**: Uses multiple threads for the young generation collection.
- **Concurrent Mark Sweep (CMS)**: Minimizes pauses by doing most of the work concurrently with the
  application.
- **G1 (Garbage First)**: Divides the heap into regions and prioritizes collection in regions with
  the most garbage.
- **ZGC and Shenandoah**: Low-latency collectors designed for very large heaps with minimal pause
  times.

</details>

## 6 – What is the purpose of the static keyword in Java?

<details>
<summary>Answer 6 ↓</summary>

The static keyword in Java is used to indicate that a particular member belongs to the type itself,
rather than to instances of that type. It has several key purposes:

**1. Static Variables (Class Variables):**

- Shared across all instances of a class.
- Exist even when no instances of the class exist.
- Used for constants, counters, or any data that should be common to all instances.
- Example: `private static int instanceCount = 0;`

**2. Static Methods (Class Methods):**

- Can be called without creating an instance of the class.
- Cannot access instance variables or methods directly.
- Used for utility functions that don't depend on instance state.
- Example: `public static double calculateArea(double radius) { return Math.PI * radius * radius; }`

**3. Static Blocks:**

- Execute when the class is first loaded into memory.
- Used for static initialization that requires more than a simple assignment.
- Example:
  ```java
  static {
      System.out.println("Class is being loaded");
      propertyMap = new HashMap<>();
  }
  ```

**4. Static Inner Classes:**

- Nested classes that don't have access to the instance variables of the outer class.
- Can be instantiated without an instance of the outer class.
- Example: `static class Helper { ... }`

**5. Static Imports:**

- Allow static members of other classes to be used without class qualification.
- Example: `import static java.lang.Math.PI;` allows using `PI` directly instead of `Math.PI`.

</details>

## 7 – What does immutability mean? Where, how, and why should we use it?

<details>
<summary>Answer 7 ↓</summary>

Immutability means that once an object is created, its state cannot be changed. All the information
in the object is set when the object is created and cannot be modified afterwards. If you need to
change something, you create a new object with the modified values instead.

**Where** should we use immutability:

1. **Value Objects**: Objects that represent values rather than entities (e.g., Money, Date, Time)
2. **Data Transfer Objects (DTOs)**: Objects used to transfer data between systems
3. **Configuration Objects**: Objects that hold configuration settings
4. **Cache Keys**: Objects used as keys in cache systems
5. **Multi-threaded Environments**: Objects shared between threads

**How** to create immutable classes in Java:

1. Make the class final
2. Make all fields private and final
3. Don't provide setter methods
4. If the class contains mutable objects:
    - Don't provide methods that modify the mutable objects
    - Don't share references to the mutable objects
    - Make defensive copies in constructors and accessor methods

**Why** use immutability:

1. **Thread Safety**: Immutable objects are inherently thread-safe as their state cannot change.
2. **Simplicity**: Without state changes, behavior is more predictable and easier to reason about.
3. **Security**: Prevents accidental or malicious state modification.
4. **Hash-based Collections**: Safe to use as keys in HashMaps or elements in HashSets.
5. **Caching**: Can be safely cached without worrying about state changes.
6. **Failure Atomicity**: No need to worry about objects being in an inconsistent state if an
   exception occurs.

**Examples of immutable classes in Java:**

- String
- All primitive wrapper classes (Integer, Long, Double, etc.)
- BigInteger and BigDecimal
- LocalDate, LocalTime, LocalDateTime

</details>

## 8 – What are composition and aggregation? What are the differences?

<details>
<summary>Answer 8 ↓</summary>

Both composition and aggregation are forms of association between classes that represent "
whole-part" or "has-a" relationships, but they differ in the strength of the relationship and the
lifecycle dependency.

**Composition:**

- **Definition**: A strong "whole-part" relationship where the part cannot exist without the whole.
- **Lifecycle**: The lifecycle of the part is dependent on the lifecycle of the whole. When the
  whole is destroyed, its parts are also destroyed.
- **Ownership**: The whole has exclusive ownership of its parts.
- **UML Representation**: Solid diamond on the whole side of the relationship.

**Aggregation:**

- **Definition**: A weak "whole-part" relationship where the part can exist independently of the
  whole.
- **Lifecycle**: The part can exist independently of the whole. When the whole is destroyed, its
  parts continue to exist.
- **Ownership**: The whole does not have exclusive ownership of its parts, which may be shared.
- **UML Representation**: Hollow diamond on the whole side of the relationship.

**Differences illustrated with examples:**

**Composition Example:**

```java
// House and Room - Composition
public class House {
    private final Room bedroom;
    private final Room kitchen;

    public House() {
        this.bedroom = new Room("Bedroom");
        this.kitchen = new Room("Kitchen");
    }

    // When House is garbage collected, the Room objects
    // will also be garbage collected as they're only
    // referenced by this House
}

public class Room {
    private final String name;

    public Room(String name) {
        this.name = name;
    }
}
```

**Aggregation Example:**

```java
// University and Professor - Aggregation
public class University {
    private final List<Professor> faculty;

    public University() {
        this.faculty = new ArrayList<>();
    }

    public void addProfessor(Professor professor) {
        faculty.add(professor);
    }

    // If the University object is garbage collected,
    // the Professor objects can still exist if referenced elsewhere
}

public class Professor {
    private final String name;

    public Professor(String name) {
        this.name = name;
    }

    // Professor can exist without a University
}
```

</details>

## 9 – What do cohesion and coupling mean, and how do they differ?

<details>
<summary>Answer 9 ↓</summary>

**Cohesion** and **coupling** are fundamental software design principles that help evaluate the
quality of a software system's structure.

**Cohesion** is he degree to which elements within a module belong together. It focuses on the
internal structure of a module/class. High cohesion would mean a module/class has a single,
well-defined responsibility.

- **Benefits of High Cohesion**:
    - Easier to understand and maintain
    - More reusable
    - Less impacted by changes in other parts of the system
    - Easier to test

**Coupling** is the degree of interdependence between modules/classes. It focuses on relationships
between different modules/classes. Loose coupling would mean modules have minimal dependencies on
each other.

- **Benefits of Loose Coupling**:
    - Changes in one module have minimal impact on others
    - Modules can be developed, tested, and understood in isolation
    - Enhanced reusability and maintainability
    - Easier to refactor

| Aspect     | Cohesion                                    | Coupling                                        |
|------------|---------------------------------------------|-------------------------------------------------|
| Focus      | Relationships within a module/class         | Relationships between different modules/classes |
| Definition | How focused a module is on a single purpose | How dependent modules are on each other         |

The ideal software design aims for **high cohesion** and **loose coupling**.
</details>

## 10 – What are heap and stack memory? What are the differences?

<details>
<summary>Answer 10 ↓</summary>

Stack Memory:

- Stores local variables and method calls.
- Allocated automatically when methods run; cleared after completion (Last-In, First-Out).
- Smaller size, but faster.
- Each thread has its own stack.
- Short-lived data (exists only during method execution).

Heap Memory:

- Stores objects created with `new`, classes, and static variables.
- Managed by Java's Garbage Collector; cleared when objects have no references.
- Larger size, slower access, shared across threads.
- Longer-lived data (exists until garbage collected).

Key Differences:

| Feature           | Stack                                | Heap                               |
|-------------------|--------------------------------------|------------------------------------|
| Memory management | Automatic, LIFO                      | Garbage collected                  |
| Size              | Smaller, fixed                       | Larger, dynamic                    |
| Scope             | Thread-specific                      | Shared among threads               |
| Lifetime          | Short-lived                          | Long-lived                         |
| Content Examples  | Method calls, primitives, references | Objects, classes, static variables |
| Speed             | Faster                               | Slower                             |
| Errors            | StackOverflowError                   | OutOfMemoryError                   |

Example:

```java
public void exampleMethod() {
    int count = 10;                       // Stack
    boolean flag = true;                  // Stack
    String name = "Java";     // Reference on stack, object on heap
    int[] numbers = new int[5];           // Reference on stack, array object on heap
    anotherMethod(count);                 // New method call adds frame to stack
}
```

</details>

## 11 - Exception means? Type of Exceptions?

<details>
<summary>Answer 11 ↓</summary>

An exception is an unexpected event during program execution that interrupts normal flow. When an
exception happens, Java creates an object representing the error, which can be handled by the code
or passed up to other methods.

**1. Based on Handling Requirements:**

- **Checked Exceptions**
    - Must be caught or declared (`try-catch` or `throws`)
    - Typically recoverable situations.
    - Examples: `IOException`, `SQLException`, `ClassNotFoundException`

- **Unchecked Exceptions**
    - Don’t require explicit catching or declaration.
    - Usually represent programming mistakes or system errors.
    - Includes:
        - **RuntimeExceptions**: e.g., `NullPointerException`, `ArrayIndexOutOfBoundsException`
        - **Errors**: Serious issues, typically not handled by code, e.g., `OutOfMemoryError`

**2. Based on Exception Hierarchy:**

- **Throwable** (root class)
    - **Error**: Serious system problems (usually not handled)
        - Examples: `OutOfMemoryError`, `StackOverflowError`

    - **Exception**: Conditions that programs may handle
        - **RuntimeException** (unchecked)
            - Examples: `NullPointerException`, `ArithmeticException`, `IllegalArgumentException`

        - **Other Exceptions** (checked)
            - Examples: `IOException`, `SQLException`, `InterruptedException`

**3. Custom Exceptions:**
Developers can define their own exceptions:

```java
// Checked Exception Example
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

// Unchecked Exception Example
public class DataProcessingException extends RuntimeException {
    public DataProcessingException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

**Exception Handling Mechanisms:**

- `try-catch`: Handle exceptions
- `try-catch-finally`: Guarantees execution of code in `finally`
- `try-with-resources`: Auto-close resources
- `throws`: Declare possible exceptions from a method
- `throw`: Manually trigger an exception

</details>

## 12 – How can we summarize 'clean code' as briefly as possible?

<details>
<summary>Answer 12 ↓</summary>

"Clean code is code that has been taken care of."
</details>

## 13 – What is method hiding in Java?

<details>
<summary>Answer 13 ↓</summary>

Method Hiding applies **only to static methods**. Is decided at **compile-time**, not runtime. Does
**not support polymorphism**.

**Example:**

```java
class Parent {
    public static void display() {
        System.out.println("Static method in Parent");
    }

    public void show() {
        System.out.println("Instance method in Parent");
    }
}

class Child extends Parent {
    // Hides Parent's static method
    public static void display() {
        System.out.println("Static method in Child");
    }

    // Overrides Parent's instance method
    @Override
    public void show() {
        System.out.println("Instance method in Child");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();

        Parent.display();  // Output: "Static method in Parent" (method hiding, based on reference type)
        p.show();     // Output: "Instance method in Child" (method overriding, based on object type)

        Child.display();  // Output: "Static method in Child"
    }
}
```

**Differences between Method Hiding and Overriding:**

| Feature           | Method Hiding  | Method Overriding     |
|-------------------|----------------|-----------------------|
| Applies to        | Static methods | Instance methods      |
| Binding           | Compile-time   | Runtime               |
| Decision based on | Reference type | Actual object type    |
| Polymorphism      | No             | Yes                   |
| `@Override`       | Not allowed    | Allowed (recommended) |

</details>

## 14 – What is the difference between abstraction and polymorphism in Java?

<details>
<summary>Answer 14 ↓</summary>

**Abstraction** means hiding internal details and only showing the essential functionality of an
object. Focuses on **what** the object does. Utilizes abstract classes, interfaces, encapsulation.

**Polymorphism** means that the same operation can behave differently with different objects.
Focuses on **how** an object behaves differently based on its actual type. Utilizes method
overriding, interfaces, method overloading.

---

**Abstraction Example:**

```java
// Abstract class representing a vehicle
abstract class Vehicle {
    abstract void startEngine(); // Abstract method (no implementation)

    void stopEngine() { // Regular method
        System.out.println("Engine stopped");
    }
}

// Concrete implementation
class Car extends Vehicle {
    void startEngine() {
        System.out.println("Car engine started");
    }
}
```

**Polymorphism Example:**

```java
class Motorcycle extends Vehicle {
    void startEngine() {
        System.out.println("Motorcycle engine started");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car();
        Vehicle motorcycle = new Motorcycle();

        car.startEngine();        // Output: "Car engine started"
        motorcycle.startEngine(); // Output: "Motorcycle engine started"
    }
}
```

---

**Differences:**

| Aspect            | Abstraction                  | Polymorphism                     |
|-------------------|------------------------------|----------------------------------|
| Purpose           | Hide complexity              | Objects behave differently       |
| Question answered | "What does it do?"           | "How does it behave?"            |
| Tools in Java     | Abstract classes, interfaces | Method overriding, interfaces    |
| Level             | Class design                 | Runtime behavior                 |
| Relationship      | Provides common interfaces   | Uses interfaces to vary behavior |

</details>

## References

<a id="1">[1]</a> [Object-oriented programming - Wikipedia](https://en.wikipedia.org/wiki/Object-oriented_programming#:~:text=Significant%20object%2Doriented%20languages%20include,Vala%20and%20Visual%20Basic.NET)